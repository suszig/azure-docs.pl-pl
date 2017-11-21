---
title: "Wdrażanie aplikacji .NET w kontenerze sieci szkieletowej usług Azure | Dokumentacja firmy Microsoft"
description: "Przedstawienie sposobu pakietu aplikacji .NET w programie Visual Studio w kontenerze Docker. Ta nowa aplikacja \"kontener\" następnie jest wdrażany klastra sieci szkieletowej usług."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/19/2017
ms.author: mikhegn
ms.openlocfilehash: 31c1cee5ddc4c8893da729af884ae7b7b8a58093
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2017
---
# <a name="deploy-a-net-application-in-a-windows-container-to-azure-service-fabric"></a>Wdrażanie aplikacji .NET w kontenerze systemu Windows Azure sieci szkieletowej usług

Ten samouczek przedstawia sposób wdrażania istniejącej aplikacji ASP.NET w kontenerze systemu Windows Azure.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie projektu platformy Docker w programie Visual Studio
> * Containerize istniejącej aplikacji
> * Instalator ciągłej integracji z programu Visual Studio i VSTS

## <a name="prerequisites"></a>Wymagania wstępne

1. Zainstaluj [CE Docker dla systemu Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description) , co umożliwia uruchamianie kontenerów w systemie Windows 10.
2. Zapoznaj się z [Szybki Start systemu Windows 10 kontenery][link-container-quickstart].
3. Pobierz [Fabrikam Fiber CallCenter] [ link-fabrikam-github] przykładowej aplikacji.
4. Zainstaluj [programu Azure PowerShell][link-azure-powershell-install]
5. Zainstaluj [rozszerzenie ciągłego dostarczania narzędzi dla programu Visual Studio 2017 r.][link-visualstudio-cd-extension]
6. Utwórz [subskrypcji platformy Azure] [ link-azure-subscription] i [konto usługi Visual Studio Team Services][link-vsts-account]. 
7. [Tworzenie klastra na platformie Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md)

## <a name="create-a-cluster-on-azure"></a>Tworzenie klastra na platformie Azure
Sieć szkieletowa usług aplikacje uruchamiane w klastrze, zestaw połączonych z siecią maszyn wirtualnych lub fizycznych. [Konfiguracja klastra sieci szkieletowej usług działających na platformie Azure](service-fabric-tutorial-create-vnet-and-windows-cluster.md) przed utworzeniem i wdrożeniem aplikacji. Podczas tworzenia klastra, wybierz jednostki SKU, obsługujący uruchomionych kontenerów (np. Windows Server Datacenter 2016 z kontenerami).

## <a name="containerize-the-application"></a>Containerize aplikacji

Teraz, gdy masz klastra sieci szkieletowej usług działających na platformie Azure możesz przystąpić do tworzenia i wdrażania konteneryzowanych aplikacji. Uruchomienia aplikacji w kontenerze, należy dodać **Obsługa Docker** do projektu programu Visual Studio. Po dodaniu **Obsługa Docker** do aplikacji, to dwa wiele problemów. Najpierw _plik Dockerfile_ zostanie dodany do projektu. Ten nowy plik opisano kontenera obraz ma zostać utworzony. Następnie po drugie, nowy _rozwiązania docker compose_ projekt zostanie dodany do rozwiązania. Nowy projekt zawiera kilka rozwiązania docker compose plików. Pliki rozwiązania docker compose może służyć do opisywania działanie kontenera.

Więcej informacji na temat pracy z [programu Visual Studio Tools kontenera][link-visualstudio-container-tools].

>[!NOTE]
>Jeśli używasz obrazów kontenera systemu Windows na komputerze po raz pierwszy, Docker CE musi rozwiń obrazy podstawowe dla kontenerów. Obrazy używane w tym samouczku są 14 GB. Przejdź dalej i uruchom następujące polecenie terminali ściągania obrazy podstawowe:
>```cmd
>docker pull microsoft/mssql-server-windows-developer
>docker pull microsoft/aspnet:4.6.2
>```

### <a name="add-docker-support"></a>Dodawanie obsługi Docker

Otwórz [FabrikamFiber.CallCenter.sln] [ link-fabrikam-github] pliku w programie Visual Studio.

Kliknij prawym przyciskiem myszy **FabrikamFiber.Web** projektu > **Dodaj** > **Obsługa Docker**.

### <a name="add-support-for-sql"></a>Dodawanie obsługi SQL

Ta aplikacja używa SQL jako dostawcy danych, więc programu SQL server jest wymagany do uruchomienia aplikacji. Odwołuje się obraz kontenera programu SQL Server w naszym pliku docker compose.override.yml.

W programie Visual Studio Otwórz **Eksploratora rozwiązań**, Znajdź **rozwiązania docker compose**i Otwórz plik **docker compose.override.yml**.

Przejdź do `services:` węzła, Dodaj węzeł o nazwie `db:` definiuje wpis programu SQL Server dla kontenera.

```yml
  db:
    image: microsoft/mssql-server-windows-developer
    environment:
      sa_password: "Password1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433"
    healthcheck:
      test: [ "CMD", "sqlcmd", "-U", "sa", "-P", "Password1", "-Q", "select 1" ]
      interval: 1s
      retries: 20
```

>[!NOTE]
>Można użyć dowolnego wolisz dla debugowania lokalnego programu SQL Server, tak długo, jak jest dostępny z hosta. Jednak **localdb** nie obsługuje `container -> host` komunikacji.

>[!WARNING]
>Programem SQL Server w kontenerze nie obsługuje trwałych danych. Po zatrzymaniu kontenera danych jest usunięte. Nie używaj tej konfiguracji w środowisku produkcyjnym.

Przejdź do `fabrikamfiber.web:` węzeł i Dodaj węzeł podrzędny o nazwie `depends_on:`. Gwarantuje to, że `db` usługi (kontenera programu SQL Server), który rozpoczyna się przed naszej aplikacji sieci web (fabrikamfiber.web).

```yml
  fabrikamfiber.web:
    depends_on:
      - db
```

### <a name="update-the-web-config"></a>Aktualizacja konfiguracji sieci web

W **FabrikamFiber.Web** projektu, zaktualizuj parametry połączenia w **web.config** pliku, aby wskazać serwer SQL w kontenerze.

```xml
<add name="FabrikamFiber-Express" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />

<add name="FabrikamFiber-DataWarehouse" connectionString="Data Source=db,1433;Database=FabrikamFiber;User Id=sa;Password=Password1;MultipleActiveResultSets=True" providerName="System.Data.SqlClient" />
```

>[!NOTE]
>Jeśli chcesz użyć innego programu SQL Server podczas kompilowania zlecenia tworzenie aplikacji sieci web, Dodaj inny ciąg połączenia do pliku web.release.config.

### <a name="test-your-container"></a>Twoje kontener testu

Naciśnij klawisz **F5** by przeprowadzić debugowanie aplikacji w Twojej kontenera.

Krawędź otwiera stronę zdefiniowanych uruchamiania aplikacji przy użyciu adresu IP kontenera w sieci wewnętrznej translatora adresów Sieciowych (zazwyczaj 172.x.x.x). Aby dowiedzieć się więcej na temat debugowania aplikacji w kontenerach przy użyciu programu Visual Studio 2017, zobacz [w tym artykule][link-debug-container].

![przykład firma fabrikam w kontenerze][image-web-preview]

Kontener jest teraz gotowy do być zbudowany i opakowane w aplikacji sieci szkieletowej usług. Po utworzeniu kontenera obraz oparty na tym komputerze, możesz wypchnąć go do dowolnego rejestru kontenera i umieszczenie jej do dowolnego hosta do uruchomienia.

## <a name="get-the-application-ready-for-the-cloud"></a>Przygotowanie aplikacji dla chmury

Do przygotowania aplikacji do uruchamiania w sieci szkieletowej usług platformy Azure, należy wykonać dwie czynności:

1. Udostępnianie portów, gdy chcemy mieć dostęp do aplikacji sieci web w klastrze usługi sieć szkieletowa usług.
2. Podaj produkcji gotowy bazy danych SQL dla naszej aplikacji.

### <a name="expose-the-port-for-the-app"></a>Udostępnianie portów dla aplikacji
Klaster sieci szkieletowej usług, firma Microsoft skonfigurowano ma port *80* otwarty domyślnie w usłudze równoważenia obciążenia Azure, która równoważy ruch przychodzący do klastra. Uwidaczniamy naszych kontenera na tym porcie za pośrednictwem naszego plik docker-compose.yml.

W programie Visual Studio Otwórz **Eksploratora rozwiązań**, Znajdź **rozwiązania docker compose**i Otwórz plik **docker-compose.yml**.

Modyfikowanie `fabrikamfiber.web:` węzła, Dodaj węzeł podrzędny o nazwie `ports:`.

Dodaj wpis ciąg `- "80:80"`. Jest to, jak powinien wyglądać plik docker-compose.yml:

```yml
  version: '3'

  services:
    fabrikamfiber.web:
      image: fabrikamfiber.web
      build:
        context: .\FabrikamFiber.Web
        dockerfile: Dockerfile
      ports:
        - "80:80"
```

### <a name="use-a-production-sql-database"></a>Użyj produkcyjną bazę danych SQL
Podczas uruchamiania w środowisku produkcyjnym, potrzebujemy nasze dane utrwalone w naszej bazie danych. Obecnie nie istnieje sposób zagwarantowanie trwałych danych w kontenerze, dlatego nie można zapisać danych produkcyjnych w programie SQL Server w kontenerze.

Zaleca się korzystanie z bazy danych SQL Azure. Aby skonfigurować i uruchomić zarządzanego serwera SQL na platformie Azure, odwiedź stronę [Quickstarts bazy danych SQL Azure] [ link-azure-sql] artykułu.

>[!NOTE]
>Pamiętaj, aby zmienić parametry połączenia z programem SQL server w **web.release.config** w pliku **FabrikamFiber.Web** projektu.
>
>Ta aplikacja powiedzie się, jeśli baza danych SQL jest osiągalny. Można teraz i wdrażanie aplikacji bez serwera SQL.

## <a name="deploy-with-visual-studio-team-services"></a>Rozmieszczanie za pomocą programu Visual Studio Team Services

Aby skonfigurować wdrożenie przy użyciu programu Visual Studio Team Services, musisz zainstalować [rozszerzenie ciągłego dostarczania narzędzi dla programu Visual Studio 2017][link-visualstudio-cd-extension]. To rozszerzenie ułatwia wdrażanie na platformie Azure przez skonfigurowanie Visual Studio Team Services i uzyskiwanie wdrożone do klastra usługi sieć szkieletowa usług dla swojej aplikacji.

Aby rozpocząć, kod musi być hostowany w kontroli źródła. Pozostałej części tej sekcji założono **git** jest używany.

### <a name="set-up-a-vsts-repo"></a>Konfigurowanie repozytorium VSTS
W prawym dolnym rogu Visual Studio kliknij **Dodaj do kontroli źródła** > **Git** (lub użyć jednego z tych opcji).

![Naciśnij przycisk kontroli źródła][image-source-control]

W _Team Explorer_ okienka, naciśnij klawisz **Publikuj repozytorium Git**.

Wybierz nazwę repozytorium VSTS i naciśnij klawisz **repozytorium**.

![Publikowanie programu VSTS repozytorium][image-publish-repo]

Teraz, gdy kod jest zsynchronizowany z repozytorium źródłowe VSTS, można skonfigurować ciągłej integracji i ciągłego dostarczania.

### <a name="setup-continuous-delivery"></a>Instalator ciągłego dostarczania

W _Eksploratora rozwiązań_, kliknij prawym przyciskiem myszy **rozwiązania** > **skonfigurować ciągłego dostarczania**.

Wybierz subskrypcję platformy Azure.

Ustaw **hosta typu** do **usługi klastra sieci szkieletowej**.

Ustaw **hosta docelowego** do klastra sieci szkieletowej usług został utworzony w poprzedniej sekcji.

Wybierz **rejestru kontenera** kontener tak, aby opublikować.

>[!TIP]
>Użyj **Edytuj** przycisk, aby utworzyć rejestru kontenera.

Naciśnij przycisk **OK**.

![Instalator usługi sieci szkieletowej ciągłej integracji][image-setup-ci]
   
   Po zakończeniu konfiguracji sieci szkieletowej usług jest wdrażany z kontenera. Zawsze, gdy użytkownik wypychania aktualizacje repozytorium nowej kompilacji i wersji jest wykonywana.
   
   >[!NOTE]
   >Tworzenie kontenera podjęcia obrazy około 15 minut.
   >Pierwszym wdrożeniu klastra sieci szkieletowej usług powoduje, że podstawowa obrazy kontenera systemu Windows Server Core do pobrania. Pobieranie ma dodatkowe 5-10 minut.

Przejdź do aplikacji firmy Fabrikam biurem przy użyciu adresu url klastra: na przykład *http://mycluster.westeurope.cloudapp.azure.com*

Teraz, gdy masz konteneryzowanych i wdrożyć rozwiązanie biurem firmy Fabrikam, możesz otworzyć [portalu Azure] [ link-azure-portal] i aplikacja była uruchomiona w sieci szkieletowej usług. Aby wypróbować aplikacji, otwórz przeglądarkę sieci web i przejdź do adresu URL klastra usługi sieć szkieletowa usług.

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie projektu platformy Docker w programie Visual Studio
> * Containerize istniejącej aplikacji
> * Instalator ciągłej integracji z programu Visual Studio i VSTS

W następnej części samouczka, Dowiedz się, jak skonfigurować [monitorowania z kontenera](service-fabric-tutorial-monitoring-wincontainers.md).

<!--   NOTE SURE WHAT WE SHOULD DO YET HERE

Advance to the next tutorial to learn how to bind a custom SSL certificate to it.

> [!div class="nextstepaction"]
> [Bind an existing custom SSL certificate to Azure Web Apps](app-service-web-tutorial-custom-ssl.md)

## Next steps

- [Container Tooling in Visual Studio][link-visualstudio-container-tools]
- [Get started with containers in Service Fabric][link-servicefabric-containers]
- [Creating Service Fabric applications][link-servicefabric-createapp]
-->

[link-debug-container]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-fabrikam-github]: https://aka.ms/fabrikamcontainer
[link-container-quickstart]: /virtualization/windowscontainers/quick-start/quick-start-windows-10
[link-visualstudio-container-tools]: /dotnet/articles/core/docker/visual-studio-tools-for-docker
[link-azure-powershell-install]: /powershell/azure/install-azurerm-ps
[link-servicefabric-create-secure-clusters]: service-fabric-cluster-creation-via-arm.md
[link-visualstudio-cd-extension]: https://aka.ms/cd4vs
[link-servicefabric-containers]: service-fabric-get-started-containers.md
[link-servicefabric-createapp]: service-fabric-create-your-first-application-in-visual-studio.md
[link-azure-portal]: https://portal.azure.com
[link-sf-clustertemplate]: https://aka.ms/securepreviewonelineclustertemplate
[link-azure-pricing-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
[link-azure-subscription]: https://azure.microsoft.com/en-us/free/
[link-vsts-account]: https://www.visualstudio.com/team-services/pricing/
[link-azure-sql]: /azure/sql-database/

[image-web-preview]: media/service-fabric-host-app-in-a-container/fabrikam-web-sample.png
[image-source-control]: media/service-fabric-host-app-in-a-container/add-to-source-control.png
[image-publish-repo]: media/service-fabric-host-app-in-a-container/publish-repo.png
[image-setup-ci]: media/service-fabric-host-app-in-a-container/configure-continuous-integration.png
