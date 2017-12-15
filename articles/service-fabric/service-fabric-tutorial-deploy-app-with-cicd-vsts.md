---
title: "Wdróż aplikację Azure Service Fabric przy ciągłej integracji (Team Services) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować ciągłej integracji i wdrażania aplikacji sieci szkieletowej usług za pomocą programu Visual Studio Team Services.  Wdrażanie aplikacji do klastra usługi sieć szkieletowa na platformie Azure."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/13/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 2fb7ab906208a58c0b5cd3af8b53188fbab94029
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/15/2017
---
# <a name="deploy-an-application-with-cicd-to-a-service-fabric-cluster"></a>Wdrażanie aplikacji przy użyciu elementu konfiguracji/CD do klastra sieci szkieletowej usług
W tym samouczku jest częścią trzy serii i zawiera opis sposobu konfigurowania ciągłej integracji i wdrażania aplikacji sieci szkieletowej usług Azure przy użyciu programu Visual Studio Team Services.  Istniejącej aplikacji usługi sieć szkieletowa jest potrzebna, aplikacji utworzony w [tworzenia aplikacji .NET](service-fabric-tutorial-create-dotnet-app.md) służy jako przykład.

W trzech części serii, możesz dowiedzieć się, jak:

> [!div class="checklist"]
> * Dodaj do kontroli źródła do projektu
> * Utwórz definicję kompilacji w Team Services
> * Tworzenie definicji wersji w usłudze Team Services
> * Automatyczne wdrażanie i uaktualnianie aplikacji

W tym samouczku dowiesz się, jak:
> [!div class="checklist"]
> * [Tworzenie aplikacji sieci szkieletowej usług .NET](service-fabric-tutorial-create-dotnet-app.md)
> * [Wdrażanie aplikacji do zdalnego klastra](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * Konfigurowanie elementu konfiguracji/CD za pomocą programu Visual Studio Team Services
> * [Konfigurowanie monitorowania i diagnostyki dla aplikacji](service-fabric-tutorial-monitoring-aspnet.md)

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka:
- Jeśli nie masz subskrypcji platformy Azure, Utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Zainstaluj program Visual Studio 2017](https://www.visualstudio.com/) i zainstaluj **Azure programowanie** i **ASP.NET i sieć web development** obciążeń.
- [Zainstaluj zestaw SDK sieci szkieletowej usług](service-fabric-get-started.md)
- Tworzenie klastra sieci szkieletowej usług systemu Windows na platformie Azure, na przykład przez [tego samouczka](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
- Utwórz [konta usługi Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).

## <a name="download-the-voting-sample-application"></a>Pobierz aplikację przykładową głosowania
Jeśli nie zbudować głosowania przykładowej aplikacji [część jednego z tego samouczka serii](service-fabric-tutorial-create-dotnet-app.md), można go pobrać. W oknie polecenia uruchom następujące polecenie, aby sklonować przykładowe repozytorium aplikacji na komputer lokalny.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="prepare-a-publish-profile"></a>Przygotowanie profilu publikowania
Znasz [utworzona aplikacja](service-fabric-tutorial-create-dotnet-app.md) i mieć [po wdrożeniu aplikacji na platformie Azure](service-fabric-tutorial-deploy-app-to-party-cluster.md), wszystko jest gotowe do skonfigurowania ciągłej integracji.  Najpierw przygotować profilu publikowania w aplikacji do użycia przez proces wdrażania, która jest wykonywana w ramach usług Team Services.  Profil publikowania, należy skonfigurować pod kątem klastra, które wcześniej zostały utworzone.  Uruchom program Visual Studio i otworzyć istniejący projekt aplikacji sieci szkieletowej usług.  W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy aplikację i wybierz **publikowania...** .

Wybierz profil docelowy w ramach projektu aplikacji w celu używania przepływu pracy ciągłej integracji, na przykład w chmurze.  Określ punktu końcowego połączenia klastra.  Sprawdź **uaktualnienie aplikacji** pole wyboru, aby aplikacja uaktualnia dla każdego wdrożenia w Team Services.  Kliknij przycisk **zapisać** hiperłącze, aby zapisać ustawienia w profilu publikowania, a następnie kliknij przycisk **anulować** aby zamknąć okno dialogowe.  

![Profil wypychania][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-team-services-git-repo"></a>Udostępnianie rozwiązania Visual Studio do nowego repozytorium Git programu Team Services
Udostępnianie plików źródłowych aplikacji do projektu zespołowego Team Services, można wygenerować kompilacji.  

Utwórz nowe lokalne repozytorium Git dla projektu, zaznaczając **Dodaj do kontroli źródła** -> **Git** na pasku stanu w prawym dolnym rogu programu Visual Studio. 

W **Push** wyświetlić w **Team Explorer**, wybierz pozycję **Publikuj repozytorium Git** przycisku w obszarze **wypychania do programu Visual Studio Team Services**.

![Wypychanie repozytorium Git][push-git-repo]

Sprawdź pocztę i wybierz konto w **zespołu usług domenowych** listy rozwijanej. Wprowadź nazwę repozytorium i wybierz **Publikuj repozytorium**.

![Wypychanie repozytorium Git][publish-code]

Opublikowanie repozytorium powoduje utworzenie nowego projektu zespołowego na koncie z taką samą nazwę jak lokalnego repozytorium. Aby utworzyć repozytorium w istniejących projektów zespołowych, kliknij przycisk **zaawansowane** obok **repozytorium** nazwę, a następnie wybierz projekt zespołowy. Kod w sieci web można wyświetlić, wybierając **jest widoczny w sieci web**.

## <a name="configure-continuous-delivery-with-vsts"></a>Konfigurowanie ciągłego dostarczania przy użyciu programu VSTS
Definicja kompilacji Team Services zawiera opis przepływu pracy, który składa się z zestaw kroków kompilacji, które są wykonywane sekwencyjnie. Utwórz definicję kompilacji, który, który spowoduje utworzenie pakietu aplikacji sieci szkieletowej usług i innych artefakty do wdrożenia klastra sieci szkieletowej usług. Dowiedz się więcej o [definicje kompilacji Team Services](https://www.visualstudio.com/docs/build/define/create). 

Definicja wersji Team Services opisano przepływ pracy, który wdraża pakiet aplikacji do klastra. Razem definicję kompilacji i wersji definicji wykonać całego przepływu pracy, zaczynając od plików źródłowych z uruchomioną aplikację w klastrze. Dowiedz się więcej na temat usługi Team Services [wersji definicji](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-build-definition"></a>Tworzenie definicji kompilacji
Otwórz przeglądarkę sieci web i przejdź do nowego projektu zespołowego na: [https://&lt;myaccount&gt;.visualstudio.com/Voting/Voting%20Team/_git/Voting](https://myaccount.visualstudio.com/Voting/Voting%20Team/_git/Voting). 

Wybierz **kompilacji i wydania** karcie następnie **kompilacje**, następnie **+ nową definicję**.  W **wybierz szablon**, wybierz pozycję **aplikacji sieci szkieletowej usług Azure** szablon i kliknij przycisk **Zastosuj**. 

![Wybierz szablon kompilacji][select-build-template] 

W **zadania**, wprowadź "VS2017 hostowanej" jako **kolejki agenta**. 

![Wybierz zadania][save-and-queue]

W obszarze **wyzwalaczy**, Włącz ciągłej integracji, ustawiając **wyzwolenia stan**.  Wybierz **Zapisz i kolejka** ręcznie uruchomić kompilację.  

![Wybierz wyzwalacze][save-and-queue2]

Opisano również wyzwalacza wypychania lub zaewidencjonowania. Aby sprawdzić postęp kompilacji, Przełącz **kompilacje** kartę.  Po upewnieniu się, że Kompilacja została wykonana pomyślnie, należy zdefiniować definicji wersji, która wdraża aplikację do klastra. 

### <a name="create-a-release-definition"></a>Utwórz definicję zlecenia  

Wybierz **kompilacji i wydania** karcie następnie **wersje**, następnie **+ nową definicję**.  W **wybierz szablon**, wybierz pozycję **wdrażania usługi Azure Service Fabric** szablon z listy, a następnie **Zastosuj**.  

![Wybierz szablon zlecenia][select-release-template]

Wybierz **zadania**->**1 środowiska** , a następnie **+ nowy** można dodać nowego połączenia klastra.

![Dodaj połączenie klastra][add-cluster-connection]

W **Dodaj nowe połączenia z usługą sieci szkieletowej** wyświetlić wybierz **na podstawie certyfikatu** lub **usługi Azure Active Directory** uwierzytelniania.  Określ nazwę połączenia "mysftestcluster" i punkt końcowy klastra o "tcp://mysftestcluster.southcentralus.cloudapp.azure.com:19000" (lub punktu końcowego klastra, który jest wdrażany z). 

W przypadku uwierzytelniania opartego na certyfikatach, dodać **odcisk palca certyfikatu serwera** certyfikatu serwera, używane do tworzenia klastra.  W **certyfikatu klienta**, Dodaj kodowanie base-64 plik certyfikatu klienta. Zobacz okno Pomocy o to pole, aby uzyskać informacje dotyczące sposobu uzyskania tego base-64 reprezentacja zakodowanego certyfikatu. Dodaj również **hasło** certyfikatu.  Jeśli nie masz certyfikatu klienta w osobnym, można użyć certyfikatu klastra lub serwera. 

Poświadczenia usługi Azure Active Directory, można dodać **odcisk palca certyfikatu serwera** certyfikatu serwera używany do tworzenia klastra i poświadczenia ma być używana do łączenia się z klastrem w **nazwyużytkownika** i **hasło** pola. 

Kliknij przycisk **Dodaj** można zapisać połączenia klastra.

Następnie dodaj artefaktów kompilacji do potoku, więc definicji wersji można znaleźć dane wyjściowe kompilacji. Wybierz **potoku** i **artefakty**->**+ Dodaj**.  W **źródła (definicja kompilacji)**, wybierz wcześniej utworzony definicji kompilacji.  Kliknij przycisk **Dodaj** zapisać artefaktów kompilacji.

![Dodaj artefaktów][add-artifact]

Wyzwalacz ciągłego wdrażania należy włączyć, dzięki czemu zlecenia zostało automatycznie utworzone po zakończeniu kompilacji. Kliknij ikonę w artefaktu, Włącz wyzwalacza, a następnie kliknij przycisk **zapisać** można zapisać definicji wersji.

![Włącz wyzwalacza][enable-trigger]

Wybierz **+ wersji** -> **Tworzenie wersji** -> **Utwórz** ręczne tworzenie zlecenia.  Sprawdź, czy wdrożenie zakończyło się pomyślnie, a aplikacja jest uruchomiona w klastrze.  Otwórz przeglądarkę sieci web i przejdź do [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Należy pamiętać, wersja aplikacji, w tym przykładzie jest "1.0.0.20170616.3". 

## <a name="commit-and-push-changes-trigger-a-release"></a>Zatwierdź i Wypchnij zmiany, wyzwalanie wydania
Aby sprawdzić, czy działa potoku ciągłej integracji, sprawdzając w pewnych zmian kodu usługi Team Services.    

Podczas pisania kodu zmiany automatycznie są śledzone przez program Visual Studio. Zatwierdź zmiany w lokalnym repozytorium Git, wybierając ikonę (oczekujących zmian![Oczekujące][pending]) na pasku stanu, w prawym dolnym rogu.

Na **zmiany** wyświetlić w programie Team Explorer, Dodaj komunikat opisujący aktualizacji i zatwierdzić zmiany.

![Zatwierdź wszystko][changes]

Wybierz ikonę paska nieopublikowane zmiany stanu (![nieopublikowane zmiany][unpublished-changes]) lub widoku synchronizacji w programie Team Explorer. Wybierz **Push** można zaktualizować kodu w Team Services/TFS.

![Wypchnij zmiany][push]

Wypychanie zmiany do usługi Team Services automatycznie wyzwala kompilacji.  Po pomyślnym zakończeniu definicji kompilacji, zlecenia zostanie utworzona automatycznie i rozpoczyna uaktualnianie aplikacji w klastrze.

Aby sprawdzić postęp kompilacji, Przełącz **kompilacje** karcie **Team Explorer** w programie Visual Studio.  Po upewnieniu się, że Kompilacja została wykonana pomyślnie, należy zdefiniować definicji wersji, która wdraża aplikację do klastra.

Sprawdź, czy wdrożenie zakończyło się pomyślnie, a aplikacja jest uruchomiona w klastrze.  Otwórz przeglądarkę sieci web i przejdź do [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Należy pamiętać, wersja aplikacji, w tym przykładzie jest "1.0.0.20170815.3".

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a>Aktualizowanie aplikacji
Zmiany kodu w aplikacji.  Zapisz i zatwierdź zmiany po poprzednich kroków.

Po rozpoczęciu uaktualnienia aplikacji, możesz obserwować postęp uaktualniania w narzędziu Service Fabric Explorer:

![Service Fabric Explorer][sfx2]

Uaktualnianie aplikacji może potrwać kilka minut. Po ukończeniu uaktualniania aplikacja zostanie uruchomiona następnej wersji.  W tym przykładzie "1.0.0.20170815.4".

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Dodaj do kontroli źródła do projektu
> * Tworzenie definicji kompilacji
> * Utwórz definicję zlecenia
> * Automatyczne wdrażanie i uaktualnianie aplikacji

Przejdź do następnego samouczek:
> [!div class="nextstepaction"]
> [Konfigurowanie monitorowania i diagnostyki dla aplikacji](service-fabric-tutorial-monitoring-aspnet.md) 


<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/PublishCode.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectBuildTemplate.png
[save-and-queue]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue.png
[save-and-queue2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SaveAndQueue2.png
[select-release-template]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SelectReleaseTemplate.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddClusterConnection.png
[add-artifact]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/AddArtifact.png
[enable-trigger]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/EnableTrigger.png
[sfx1]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-app-with-cicd-vsts/NewServiceEndpointDialog.png
