---
title: "Wdrażanie aplikacji usługi Azure Service Fabric w klastrze testowym | Microsoft Docs"
description: "Dowiedz się, jak wdrożyć aplikację w klastrze testowym."
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
ms.openlocfilehash: 91d4398589707e8007c4b93639ddb568e39f51a7
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="deploy-an-application-to-a-party-cluster-in-azure"></a>Wdrażanie aplikacji w klastrze testowym na platformie Azure
Ten samouczek to druga część serii. Przedstawiono w nim sposób wdrażania aplikacji usługi Azure Service Fabric w klastrze testowym na platformie Azure.

W drugiej części serii samouczków zawarto informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * [Kompilowanie aplikacji .NET Service Fabric](service-fabric-tutorial-create-dotnet-app.md)
> * Wdrażanie aplikacji w klastrze zdalnym
> * [Konfigurowanie procesu CI/CD za pomocą usługi Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Konfigurowanie monitorowania i diagnostyki dla aplikacji](service-fabric-tutorial-monitoring-aspnet.md)

Ta seria samouczków zawiera informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Wdrażanie aplikacji w klastrze zdalnym przy użyciu programu Visual Studio
> * Usuwanie aplikacji z klastra przy użyciu narzędzia Service Fabric Explorer

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka:
- Jeśli nie masz subskrypcji platformy Azure, utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Zainstaluj program Visual Studio 2017](https://www.visualstudio.com/), a następnie zainstaluj obciążenia **Programowanie na platformie Azure** i **Tworzenie aplikacji na platformie ASP.NET i tworzenie aplikacji internetowych**.
- [Instalowanie zestawu SDK usługi Service Fabric](service-fabric-get-started.md)

## <a name="download-the-voting-sample-application"></a>Pobieranie przykładowej aplikacji do głosowania
Jeśli nie skompilowano przykładowej aplikacji do głosowania w [pierwszej części tej serii samouczków](service-fabric-tutorial-create-dotnet-app.md), można ją pobrać. W oknie polecenia uruchom następujące polecenie, aby sklonować przykładowe repozytorium aplikacji na komputer lokalny.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-a-party-cluster"></a>Konfigurowanie klastra testowego
Klastry testowe to bezpłatne, ograniczone czasowo klastry usługi Service Fabric hostowane na platformie Azure i uruchamiane przez zespół usługi Service Fabric, w których każdy może wdrażać aplikacje i poznawać platformę. Bezpłatnie!

Aby uzyskać dostęp do klastra testowego, przejdź do tej witryny: http://aka.ms/tryservicefabric i postępuj zgodnie z instrukcjami, aby uzyskać dostęp do klastra. Do uzyskania dostępu do klastra testowego niezbędne jest konto w serwisie Facebook lub GitHub.

Zamiast klastra testowego możesz użyć własnego klastra.  Podstawowy fronton internetowy platformy ASP.NET używa zwrotnego serwera proxy do komunikowania się z zapleczem usługi stanowej.  Klastry testowe i lokalny klaster programowania mają domyślnie włączony zwrotny serwer proxy.  Jeśli przykładową aplikację głosowania wdrożono we własnym klastrze, należy najpierw [włączyć zwrotny serwer proxy w klastrze](service-fabric-reverseproxy.md#setup-and-configuration).

> [!NOTE]
> Klastry testowe nie są zabezpieczone, dlatego umieszczane w nich aplikacje i dowolne dane mogą być widoczne dla innych użytkowników. Dlatego wdrażaj tylko to, co mogą zobaczyć inne osoby. Aby uzyskać szczegółowe informacje, pamiętaj o przeczytaniu naszych Warunków użytkowania.

Zaloguj się i [dołącz do klastra z systemem Windows](http://aka.ms/tryservicefabric). Pobierz certyfikat PFX na komputer, klikając link **PFX**. Certyfikat i wartość **Punkt końcowy połączenia** będą używane w kolejnych krokach.

![Plik PFX i punkt końcowy połączenia](./media/service-fabric-quickstart-containers/party-cluster-cert.png)

Na komputerze z systemem Windows zainstaluj plik PFX w magazynie certyfikatów *CurrentUser\My*.

```powershell
PS C:\mycertificates> Import-PfxCertificate -FilePath .\party-cluster-873689604-client-cert.pfx -CertStoreLocation Cert:
\CurrentUser\My


  PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
```


## <a name="deploy-the-app-to-the-azure"></a>Wdrażanie aplikacji na platformie Azure
Kiedy aplikacja jest gotowa, można wdrożyć ją w klastrze testowym bezpośrednio z programu Visual Studio.

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy pozycję **Voting (Głosowanie)** i wybierz polecenie **Publikuj**. 

    ![Okno dialogowe Publikowanie](./media/service-fabric-quickstart-containers/publish-app.png)

2. Skopiuj **punkt końcowy połączenia** ze strony klastra testowego do pola **Punkt końcowy połączenia**. Na przykład `zwin7fh14scd.westus.cloudapp.azure.com:19000`. Kliknij pozycję **Zaawansowane parametry połączenia** i wprowadź poniższe informacje.  Wartości *FindValue* i *ServerCertThumbprint* muszą być zgodne z odciskiem palca certyfikatu zainstalowanego w poprzednim kroku. Kliknij przycisk **Opublikuj**. 

    Po zakończeniu publikowania powinno być możliwe wysłanie żądania do aplikacji za pośrednictwem przeglądarki.

3. Otwórz preferowaną przeglądarkę i wpisz adres klastra (punkt końcowy połączenia bez informacji o porcie — na przykład win1kw5649s.westus.cloudapp.azure.com).

    Powinien być teraz widoczny ten sam wynik co po lokalnym uruchomieniu aplikacji.

    ![Odpowiedź interfejsu API z klastra](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="remove-the-application-from-a-cluster-using-service-fabric-explorer"></a>Usuwanie aplikacji z klastra przy użyciu narzędzia Service Fabric Explorer
Service Fabric Explorer to graficzny interfejs użytkownika, który służy do eksplorowania aplikacji i zarządzania nimi w klastrze usługi Service Fabric.

Aby usunąć aplikację z klastra testowego:

1. Przejdź do narzędzia Service Fabric Explorer za pomocą linku podanego na stronie tworzenia konta klastra testowego. Na przykład https://win1kw5649s.westus.cloudapp.azure.com:19080/Explorer/index.html.

2. W narzędziu Service Fabric Explorer przejdź do węzła **fabric://Voting** w widoku drzewa po lewej stronie.

3. Kliknij przycisk **Akcja** w prawym okienku **Podstawy** i wybierz pozycję **Usuń aplikację**. Potwierdź usunięcie wystąpienia aplikacji, co spowoduje usunięcie wystąpienia naszej aplikacji działającego w klastrze.

![Usuwanie aplikacji w narzędziu Service Fabric Explorer](./media/service-fabric-tutorial-deploy-app-to-party-cluster/delete-application.png)

## <a name="remove-the-application-type-from-a-cluster-using-service-fabric-explorer"></a>Usuwanie typu aplikacji z klastra przy użyciu narzędzia Service Fabric Explorer
Aplikacje są wdrażane jako typy aplikacji w klastrze usługi Service Fabric, co pozwala na korzystanie z wielu wystąpień i wersji aplikacji działającej w klastrze. Po usunięciu działającego wystąpienia naszej aplikacji możemy również usunąć typ w celu ukończenia czyszczenia wdrożenia.

Aby uzyskać więcej informacji na temat modelu aplikacji w usłudze Service Fabric, zobacz [Model an application in Service Fabric (Modelowanie aplikacji w usłudze Service Fabric)](service-fabric-application-model.md).

1. Przejdź do węzła **VotingType** w widoku drzewa.

2. Kliknij przycisk **Akcja** w prawym okienku **Podstawy** i wybierz pozycję **Cofnij aprowizację typu**. Potwierdź cofnięcie aprowizacji typu aplikacji.

![Cofanie aprowizacji typu aplikacji w narzędziu Service Fabric Explorer](./media/service-fabric-tutorial-deploy-app-to-party-cluster/unprovision-type.png)

Na tym kończy się praca z samouczkiem.

## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wdrażanie aplikacji w klastrze zdalnym przy użyciu programu Visual Studio
> * Usuwanie aplikacji z klastra przy użyciu narzędzia Service Fabric Explorer

Przejdź do następnego samouczka:
> [!div class="nextstepaction"]
> [Konfigurowanie ciągłej integracji za pomocą usługi Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
