---
title: "CI/CD z Wpięć na maszyny wirtualne platformy Azure z programu VSTS | Dokumentacja firmy Microsoft"
description: "Konfigurowanie ciągłej integracji (CI), jak i ciągłe wdrażanie (CD) aplikacji Node.js przy użyciu Wpięć na maszynach wirtualnych platformy Azure z zarządzania zleceniami w programie Visual Studio Team Services (VSTS) lub programu Microsoft Team Foundation Server (TFS)"
author: ahomer
manager: douge
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/19/2017
ms.author: ahomer
ms.custom: mvc
ms.openlocfilehash: d5c15d6ab36a8454d1c69bac498be89b990c7e33
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="deploy-your-app-to-linux-vms-using-jenkins-and-vsts"></a>Wdrażanie aplikacji w usłudze przy użyciu Wpięć i VSTS maszyn wirtualnych systemu Linux

Ciągłej integracji (CI), jak i ciągłe wdrażanie (CD) jest potoku, za pomocą którego można kompilacji, wersji i wdrażanie kodu. Visual Studio Team Services (VSTS) zapewnia pełną, kompletny zestaw narzędzi automatyzacji CI/CD wdrażania na platformie Azure. Wpięć to popularnych 3rd firm CI/CD serwerowych narzędzie, które udostępnia również automatyzacji CI/CD. Można jednocześnie ze sobą dostosować, jak dostarczać Twojej aplikacji w chmurze lub usługi.

W tym samouczku umożliwia Wpięć kompilacji **aplikacji sieci web Node.js**i VSTS lub Team Foundation Server (TFS), aby wdrożyć ją [grupę wdrożenia](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) zawierających maszyny wirtualne systemu Linux.

Wykonasz następujące zadania:

> [!div class="checklist"]
> * Pobierz przykładową aplikację
> * Skonfiguruj Wpięć wtyczek
> * Konfigurowanie projektu dowolne Wpięć dla środowiska Node.js
> * Konfigurowanie integracji programu VSTS Wpięć
> * Tworzenie punktu końcowego usługi Wpięć
> * Utwórz grupę wdrożenia dla maszyn wirtualnych platformy Azure
> * Tworzenie definicji wersji programu VSTS
> * Wykonaj ręcznie i wdrożeń PW wyzwolone

## <a name="before-you-begin"></a>Przed rozpoczęciem

* Wymagany jest dostęp do serwera Wpięć. Jeśli jeszcze nie utworzono serwera Wpięć, zobacz [Tworzenie wzorca Wpięć na maszynie wirtualnej platformy Azure](https://docs.microsoft.com/en-us/azure/jenkins/install-jenkins-solution-template). 

* Zaloguj się do konta usługi VSTS (`https://{youraccount}.visualstudio.com`). 
  Możesz uzyskać [bezpłatnego konta usługi VSTS](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > Aby uzyskać więcej informacji, zobacz [nawiązywanie VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

*  Należy maszyny wirtualnej systemu Linux dla celu wdrożenia.  Aby uzyskać więcej informacji, zobacz [tworzenie i zarządzanie maszyn wirtualnych systemu Linux z wiersza polecenia platformy Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/tutorial-manage-vm).

*  Otwórz port wejściowy 80 dla maszyny wirtualnej.  Aby uzyskać więcej informacji, zobacz [Utwórz grupy zabezpieczeń sieci przy użyciu portalu Azure](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

## <a name="get-the-sample-app"></a>Pobierz przykładową aplikację

Należy aplikację do wdrożenia przechowywane w repozytorium Git.
W tym samouczku, zalecane jest użycie [tej przykładowej aplikacji dostępne w serwisie GitHub](https://github.com/azooinmyluggage/fabrikam-node).  Ten samouczek zawiera przykładowy skrypt używane do instalowania programu Node.js i aplikacji.  Do pracy z własnych repozytorium, należy skonfigurować przykładowe podobne.

1. Utwórz rozwidlenia tej aplikacji, a następnie zanotuj lokalizację (adres URL) do użycia w kolejnych krokach tego samouczka.  Aby uzyskać więcej informacji, zobacz [rozwidlania repozytorium](https://help.github.com/articles/fork-a-repo/)    

> [!NOTE]
> Aplikacja została skompilowana przy użyciu [narzędzia Yeoman](http://yeoman.io/learning/index.html); używa **Express**, **bower**, i **grunt**; i zawiera niektóre **npm** pakietów jako zależności.
> Przykład zawiera także skrypt, który konfiguruje Nginx, a następnie wdroży aplikację. Jest ona wykonywana na maszynach wirtualnych. W szczególności skrypt instaluje węzła, Nginx i PM2; Konfiguruje Nginx i PM2; następnie uruchamia aplikację węzła.

## <a name="configure-jenkins-plugins"></a>Skonfiguruj Wpięć wtyczek

Najpierw należy skonfigurować dwa wtyczek Wpięć dla **NodeJS** i **VS Team Services ciągłe wdrażanie**.

1. Otwórz konta Wpięć i wybierz polecenie **Zarządzanie Wpięć**.
2. W **Zarządzanie Wpięć** wybierz pozycję **Zarządzanie wtyczkami**.
3. Filtruj listę, aby zlokalizować **NodeJS** wtyczki i wybierz polecenie **zainstalować bez ponownego uruchomienia** opcji.
    ![Dodawanie wtyczki NodeJS do Wpięć](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)
4. Filtruj listę, aby znaleźć **VS Team Services ciągłe wdrażanie** wtyczki i wybierz polecenie **zainstalować bez ponownego uruchomienia** opcji.
5. Przejdź z powrotem do pulpitu nawigacyjnego Wpięć i wybierz polecenie **Zarządzanie Wpięć**.
6. Wybierz **konfiguracji narzędzie globalne**.  Znajdź **NodeJS** i kliknij przycisk **instalacje NodeJS**.
7. Włącz **są instalowane automatycznie** opcji, a następnie wprowadź **nazwa** wartość.
8. Kliknij pozycję **Zapisz**.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Konfigurowanie projektu dowolne Wpięć dla środowiska Node.js

1. Kliknij przycisk **nowy element**.  Wprowadź **nazwa elementu**.
2. Wybierz **stylu projektu**.  Kliknij przycisk **OK**.
3. W **zarządzania kodem źródłowym** wybierz opcję **Git** , a następnie wprowadź szczegóły repozytorium i gałąź zawierającą kodu aplikacji.    
    ![Dodaj repozytorium do kompilacji](media/tutorial-build-deploy-jenkins/jenkins-git.png)
4. W **kompilacji wyzwalaczy** wybierz opcję **SCM sondowania** , a następnie wprowadź harmonogram `H/03 * * * *` sondowanie repozytorium Git dla zmian co trzy minuty. 
5. W **Build Environment** wybierz opcję **Podaj węzła &amp; npm bin / ŚCIEŻKĘ folderu** i wybierz **instalacji NodeJS** wartość. Pozostaw **pliku npmrc** ustawioną wartość "Użyj domyślnej system."
6. W **kompilacji** , wybierz pozycję **wykonywania powłoki** , a następnie wprowadź polecenie `npm install` zapewnienie wszystkie zależności są aktualizowane.


## <a name="configure-jenkins-for-vsts-integration"></a>Konfigurowanie integracji programu VSTS Wpięć

  > [!NOTE]
  Upewnij się, zawiera osobisty token dostępu (PAWEŁ) można używać dla następujących kroków **uprawnienia zlecenia (do odczytu, zapisu, wykonywania i zarządzania) w VSTS**.
 
1.  Jeśli nie masz już konto należy utworzyć jest element PAT w ramach konta usługi VSTS. Wpięć wymaga tych informacji, aby uzyskać dostęp do konta usługi VSTS.  Upewnij się, możesz **przechowywania** informacji o tokenie dla kolejnych kroków w tej sekcji.
  Odczyt [jak utworzyć osobisty token dostępu TFS i programu VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) więcej informacji na temat go wygenerować.
2. W **akcje postkompilacyjnego** , kliknij pozycję **dodać akcję postkompilacyjnego**. Wybierz **archiwum artefaktów**.
3. Aby uzyskać **pliki do archiwum**, wprowadź `**/*` uwzględnienie wszystkich plików.
4. Aby utworzyć inną akcję, kliknij przycisk **dodać akcję postkompilacyjnego**.
5. Wybierz **wyzwalanie wydania w TFS/Team Services**, takich jak wprowadź identyfikator uri dla Twojego konta usługi VSTS: `https://{your-account-name}.visualstudio.com`.
6. Wprowadź **projektu zespołowego** nazwy.
7. Wybierz nazwę dla **wersji definicji** (tworzenia tej wersji definicji w dalszej części programu VSTS).
8. Wybierz poświadczenia do połączenia ze środowiskiem programu VSTS lub TFS.  Pozostaw **Username** puste, jeśli używasz programu VSTS.
   Wprowadź **nazwy użytkownika i hasła** Jeśli używasz wersji lokalnego programu TFS.    
    ![Konfigurowanie Wpięć działania mające miejsce po kompilacji](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)
5. **Zapisz** wpięć projektu.

## <a name="create-a-jenkins-service-endpoint"></a>Tworzenie punktu końcowego usługi Wpięć

Punkt końcowy usługi umożliwia VSTS nawiązać połączenia z Wpięć.

1. Otwórz **usług** strony w VSTS, otwórz **nowy punkt końcowy usługi** listy, a następnie wybierz pozycję **Wpięć**.
    ![Dodawanie punktu końcowego Wpięć](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)
2. Wprowadź nazwę dla połączenia.
3. Wprowadź adres URL serwera Wpięć i znaczników **zaakceptować niezaufane certyfikaty protokołu SSL** opcji.  Przykładem jest adres URL:`http://{YourJenkinsURL}.westcentralus.cloudapp.azure.com`
4. Wprowadź **nazwy użytkownika i hasła** Wpięć konta.
5. Wybierz **sprawdzić połączenie** do sprawdzenia, czy informacje są poprawne.
6. Wybierz **OK** można utworzyć punktu końcowego usługi.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Utwórz grupę wdrożenia maszyn wirtualnych platformy Azure

Należy [grupę wdrożenia](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) można zarejestrować agenta programu VSTS, więc definicji wersji można wdrożyć maszyny wirtualnej.  Wdrożenie grupy ułatwiają zdefiniuj logiczne grupy komputerów docelowych dla wdrożenia i zainstalować agenta wymagane na każdym komputerze.

   > [!NOTE]
   > W poniższych krokach, upewnij się, zainstaluj wymagania wstępne i **nie wykonać skryptu za pomocą uprawnień sudo.**

1. Otwórz **wersje** karcie **kompilacji &amp; wersji** koncentratora, otwórz **grupy wdrożenia**i wybierz polecenie **+ nowy**.
2. Wprowadź nazwę grupy wdrożenia i opcjonalny opis.
   Następnie wybierz pozycję **Utwórz**.
3. Wybierz system operacyjny dla maszyny wirtualnej docelowej wdrożenia.  Na przykład wybrać **Ubuntu 16.04 +**.
4. Znaczników **do uwierzytelniania należy używać osobisty token dostępu w skrypcie**.
5. Wybierz **wymagania wstępne systemu** łącza.  Zainstaluj wymagania wstępne dotyczące systemu operacyjnego.
6. Wybierz **Skopiuj skrypt do Schowka** do Skopiuj skrypt.
7. **Zaloguj się za** do wdrożenia maszyny wirtualnej docelowych i **wykonania** skryptu.  **Nie** Uruchom skrypt z uprawnieniami sudo.
8. Po zakończeniu instalacji zostanie wyświetlony monit o wdrożenia grupy tagów.  Zaakceptuj ustawienia domyślne.
9. VSTS, wyszukaj nowo zarejestrowanych maszyny wirtualnej **celów** w obszarze **grupy wdrożenia**.

## <a name="create-a-vsts-release-definition"></a>Tworzenie definicji wersji programu VSTS

Określa definicji wersji, proces programu VSTS wykonuje wdrażania aplikacji.  W tym przykładzie można wykonać skryptu powłoki.

Aby utworzyć definicję wersji w VSTS:

1. Otwórz **wersje** na **kompilacji &amp; wersji** koncentratora i wybierz polecenie **Tworzenie wersji definicji**. 
2. Wybierz **pusty** szablonu, wybierając rozpoczyna się od **pusta procesu**.
3. W **artefakty** kliknij na **+ Dodaj artefaktu** i wybierz polecenie **Wpięć** dla **typ źródła**. Wybierz połączenie Wpięć usługi punktu końcowego. Następnie wybierz zadanie źródła Wpięć i wybierz **Dodaj**.
4.  Kliknij przycisk wielokropka obok **1 środowiska**.  Kliknij przycisk **Dodaj wdrożenie w grupie fazie**.
5.  Wybierz użytkownika **grupę wdrożenia**.
5. Kliknij przycisk  **+**  można dodać zadania do **faza wdrożenia grupy**.
6. Wybierz **skrypt powłoki** zadań, a następnie kliknij przycisk **Dodaj**.    
    **Skrypt powłoki** zadanie służy do zapewnienia konfiguracji skrypt uruchamiany na każdym serwerze, aby zainstalować Node.js i uruchomić aplikację. Skonfiguruj w następujący sposób:
8. **Ścieżka skryptu**:     
    `$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh`
9.  Kliknij przycisk **zaawansowane**, a następnie Włącz **określić katalog roboczy**.
10.  **Katalog roboczy**:`$(System.DefaultWorkingDirectory)/Fabrikam-Node`
11. Edytuj nazwę definicji wersji, do nazwy określonej w **działania mające miejsce po kompilacji** kartę kompilacji w Wpięć. Wpięć wymaga tej nazwy, aby można było do wyzwolenia nową wersję, po zaktualizowaniu artefakty źródła.
12. Kliknij przycisk **zapisać**i kliknij przycisk **OK** można zapisać definicji wersji.

## <a name="execute-manual-and-ci-triggered-deployments"></a>Wykonaj ręcznie i wdrożeń PW wyzwolone

1. Wybierz **+ wersji** i wybierz **Tworzenie wersji**.
2. Zaznacz na liście rozwijanej wyróżnione kompilacji został ukończony i wybierz polecenie **kolejki**.
3. Wybierz łącze wersji komunikatu podręcznego. Na przykład: "wersji **wersji 1** został utworzony."
4. Otwórz **dzienniki** kartę, aby obejrzeć dane wyjściowe konsoli wersji.
5. W przeglądarce otwórz adres URL jednego z serwerów, które można dodać do grupy wdrożenia. Na przykład wprowadź`http://{your-server-ip-address}`
6. Przejdź do repozytorium Git źródła i zmodyfikowania zawartości **h1** pozycji app/views/index.jade pliku z tekstem zmienione.
7. **Zatwierdź** zmiany.
8. Po kilku minutach, pojawi się nowe wydanie utworzone w **wersje** strony VSTS lub TFS. Otwórz wydania do wdrażania odbywa się w temacie. Gratulacje!

## <a name="next-steps"></a>Następne kroki

Ten samouczek służy do automatycznego wdrożenia aplikacji na platformie Azure przy użyciu Wpięć kompilacji i VSTS dla wersji. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie aplikacji w Wpięć
> * Konfigurowanie integracji programu VSTS Wpięć
> * Utwórz grupę wdrożenia dla maszyn wirtualnych platformy Azure
> * Tworzenie definicji wersji, która konfiguruje maszyn wirtualnych i wdraża aplikację

Przejście do następnego samouczkiem, aby dowiedzieć się więcej o sposobie wdrażania światło stosu (Linux, Apache MySQL i PHP).

> [!div class="nextstepaction"]
> [Wdrażanie stosu LAMP](tutorial-lamp-stack.md)