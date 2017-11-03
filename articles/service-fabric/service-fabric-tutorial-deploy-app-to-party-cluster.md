---
title: "Wdrażanie aplikacji sieci szkieletowej usług Azure do klastra strony | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrożyć aplikację w klastrze strony."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.custom: mvc
ms.openlocfilehash: 5766ef2097b0da295d42e7c5909efc524049f418
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-an-application-to-a-party-cluster-in-azure"></a>Wdrażanie aplikacji do klastra strony na platformie Azure
W tym samouczku jest częścią dwóch serii i pokazuje, jak wdrożyć aplikację sieci szkieletowej usług Azure do klastra strony na platformie Azure.

W części dwóch serii samouczka, dowiesz się, jak:
> [!div class="checklist"]
> * Wdrażanie aplikacji do zdalnego klastra przy użyciu programu Visual Studio
> * Usuń aplikację z klastra przy użyciu Eksploratora usługi sieć szkieletowa

W tym samouczku dowiesz się, jak:
> [!div class="checklist"]
> * [Tworzenie aplikacji sieci szkieletowej usług .NET](service-fabric-tutorial-create-dotnet-app.md)
> * Wdrażanie aplikacji do zdalnego klastra
> * [Konfigurowanie elementu konfiguracji/CD za pomocą programu Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Konfigurowanie monitorowania i diagnostyki dla aplikacji](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka:
- Jeśli nie masz subskrypcji platformy Azure, Utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Zainstaluj program Visual Studio 2017](https://www.visualstudio.com/) i zainstaluj **Azure programowanie** i **ASP.NET i sieć web development** obciążeń.
- [Zainstaluj zestaw SDK sieci szkieletowej usług](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>Pobierz aplikację przykładową głosowania
Jeśli nie zbudować głosowania przykładowej aplikacji [część jednego z tego samouczka serii](service-fabric-tutorial-create-dotnet-app.md), można go pobrać. W oknie poleceń uruchom następujące polecenie sklonować repozytorium przykładowej aplikacji na komputerze lokalnym.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-a-party-cluster"></a>Konfigurowanie klastra strony
Klastry testowe to bezpłatne, ograniczone czasowo klastry usługi Service Fabric hostowane na platformie Azure i uruchamiane przez zespół usługi Service Fabric, w których każdy może wdrażać aplikacje i poznawać platformę. Bezpłatnie!

Aby uzyskać dostęp do klastra strony, przejdź do tej witryny: http://aka.ms/tryservicefabric i postępuj zgodnie z instrukcjami, aby uzyskać dostęp do klastra. Należy na koncie usługi Facebook lub GitHub mogą uzyskać dostępu do klastra strony.

Własnego klastra można użyć zamiast klastra strony, jeśli chcesz.  Frontonem ASP.NET core dla sieci web używa zwrotnego serwera proxy do komunikacji z zaplecza usługi stanowej.  Klastry firm i lokalnego klastra projektowego mają zwrotny serwer proxy domyślnie włączone.  Jeśli wdrożono przykładową aplikację głosowania do własnego klastra, należy najpierw [włączyć zwrotnego serwera proxy w klastrze](service-fabric-reverseproxy.md#setup-and-configuration).

> [!NOTE]
> Klastry firm nie są zabezpieczone, dlatego może być widoczny dla innych aplikacji i danych, które należy umieścić w nich. Nie zostanie wdrożona niczego nie ma innych osób. Należy przeczytać przez naszych warunków użytkowania szczegółowe informacje.

## <a name="configure-the-listening-port"></a>Skonfiguruj port nasłuchujący
Po utworzeniu usługi frontonu VotingWeb Visual Studio losowo wybiera port dla usługi do nasłuchiwania.  Usługa VotingWeb działa jako frontonu dla tej aplikacji i akceptuje ruch zewnętrzny, więc warto powiązać ustalonego tej usługi i także znać port. W Eksploratorze rozwiązań Otwórz *VotingWeb/PackageRoot/ServiceManifest.xml*.  Znajdź **punktu końcowego** zasobu w **zasobów** sekcji i zmień **portu** wartość 80.

```xml
<Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="80" />
    </Endpoints>
  </Resources>
```

Również zaktualizować wartość właściwości adresu URL aplikacji w projekcie głosowania, więc w przeglądarce sieci web zostanie otwarty do właściwego portu podczas debugowania za pomocą "F5".  W Eksploratorze rozwiązań wybierz **głosowania** projektu i zaktualizuj **adres URL aplikacji** właściwości.

![Adres URL aplikacji](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-url.png)

## <a name="deploy-the-app-to-the-azure"></a>Wdrażanie aplikacji na platformie Azure
Teraz, że aplikacja jest gotowa, wdrożysz ją do bezpośredniego klastra strony z programu Visual Studio.

1. Kliknij prawym przyciskiem myszy **głosowania** w Eksploratorze rozwiązań i wybierz polecenie **publikowania**.

    ![Okno dialogowe Publikowanie](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

2. Typ w punkcie końcowym połączenia klastra strony w **punktu końcowego połączenia** a następnie kliknij przycisk **publikowania**.

    Po zakończeniu publikowania, należy wysłać żądania do aplikacji za pośrednictwem przeglądarki.

3. Otwórz preferowane przeglądarkę i wprowadź adres klastra (punktu końcowego połączenia bez informacji o porcie — na przykład win1kw5649s.westus.cloudapp.azure.com).

    Takiego samego wyniku powinna zostać wyświetlona, ponieważ był wyświetlany podczas uruchamiania aplikacji lokalnie.

    ![Interfejs API odpowiedzi z klastra](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="remove-the-application-from-a-cluster-using-service-fabric-explorer"></a>Usuń aplikację z klastra przy użyciu Eksploratora usługi sieć szkieletowa
Eksploratora usługi sieć szkieletowa jest graficzny interfejs użytkownika do eksplorowania aplikacji i zarządzanie nimi w klastrze usługi sieć szkieletowa usług.

Aby usunąć aplikację z klastra strony:

1. Przejdź do Eksploratora usługi sieć szkieletowa, za pomocą linku podanego przez stronę tworzenia konta firm klastra. Na przykład http://win1kw5649s.westus.cloudapp.azure.com:19080/Explorer/index.html.

2. W narzędziu Service Fabric Explorer, przejdź do **fabric://Voting** węzeł w widoku drzewa po lewej stronie.

3. Kliknij przycisk **akcji** prawym przyciskiem **Essentials** okienko i wybierz polecenie **Usuń aplikację**. Potwierdź usunięcie wystąpienia aplikacji, która usuwa wystąpienie aplikacji uruchamianych w klastrze.

![Usuwanie aplikacji w narzędziu Service Fabric Explorer](./media/service-fabric-tutorial-deploy-app-to-party-cluster/delete-application.png)

## <a name="remove-the-application-type-from-a-cluster-using-service-fabric-explorer"></a>Usuń typ aplikacji z klastra przy użyciu Eksploratora usługi sieć szkieletowa
Aplikacje są wdrażane jako typy aplikacji w klastrze usługi sieć szkieletowa pozwala mieć wiele wystąpień i wersji aplikacji działających w klastrze. Po usunięto działającego wystąpienia aplikacji, możemy usunąć typu, czyszczenie wdrożenia.

Aby uzyskać więcej informacji na temat modelu aplikacji w sieci szkieletowej usług, zobacz [modelu aplikacji w sieci szkieletowej usług](service-fabric-application-model.md).

1. Przejdź do **VotingType** węzła w elemencie treeview.

2. Kliknij przycisk **akcji** prawym przyciskiem **Essentials** okienko i wybierz polecenie **Cofnij Aprowizację typu**. Upewnij się, wstrzymania obsługi administracyjnej typu aplikacji.

![Cofnij aprowizację typu aplikacji w narzędziu Service Fabric Explorer](./media/service-fabric-tutorial-deploy-app-to-party-cluster/unprovision-type.png)

To jest zakończenie samouczka.

## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wdrażanie aplikacji do zdalnego klastra przy użyciu programu Visual Studio
> * Usuń aplikację z klastra przy użyciu Eksploratora usługi sieć szkieletowa

Przejdź do następnego samouczek:
> [!div class="nextstepaction"]
> [Konfigurowanie ciągłej integracji przy użyciu programu Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)