---
title: "CI/CD z Wpięć na maszyny wirtualne platformy Azure z usługi Team Services | Dokumentacja firmy Microsoft"
description: "Konfigurowanie ciągłej integracji (CI), jak i ciągłe wdrażanie (CD) aplikacji Node.js przy użyciu Wpięć na maszynach wirtualnych platformy Azure z zarządzania zleceniami w programie Visual Studio Team Services lub Microsoft Team Foundation Server"
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
ms.openlocfilehash: bfda0475b58556db1236c8b051c59393384720f7
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="deploy-your-app-to-linux-vms-by-using-jenkins-and-team-services"></a>Wdrażanie aplikacji do maszyn wirtualnych systemu Linux przy użyciu Wpięć i usługi Team Services

Ciągłej integracji (CI), jak i ciągłe wdrażanie (CD) tworzą potoku, za pomocą którego można kompilacji, wersji i wdrażanie kodu. Visual Studio Team Services zapewnia pełną, kompletny zestaw narzędzi automatyzacji CI/CD wdrażania na platformie Azure. Wpięć to popularnych innych firm CI/CD serwerowych narzędzie, które udostępnia również CI/CD automatyzacji. Można usługi Team Services i Wpięć razem dostosować, jak dostarczać Twojej aplikacji w chmurze lub usługi.

W tym samouczku Wpięć służy do tworzenia aplikacji sieci web Node.js. Następnie użyć usługi Team Services lub Team Foundation Server do wdrażania na [grupę wdrożenia](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) zawierający maszynach wirtualnych systemu Linux (VM).

Wykonasz następujące zadania:

> [!div class="checklist"]
> * Pobierz przykładową aplikację.
> * Skonfiguruj Wpięć dodatków plug-in.
> * Konfigurowanie projektu dowolne Wpięć dla środowiska Node.js.
> * Skonfiguruj Wpięć integracji usługi Team Services.
> * Tworzenie punktu końcowego usługi Wpięć.
> * Utwórz grupę wdrożenia dla maszyn wirtualnych platformy Azure.
> * Tworzenie definicji wersji Team Services.
> * Wykonanie ręczne i wyzwalane elementu konfiguracji wdrożenia.

## <a name="before-you-begin"></a>Przed rozpoczęciem

* Wymagany jest dostęp do serwera Wpięć. Jeśli jeszcze nie utworzono serwera Wpięć, zobacz [Tworzenie wzorca Wpięć na maszynie wirtualnej platformy Azure](https://docs.microsoft.com/azure/jenkins/install-jenkins-solution-template). 

* Zaloguj się do konta usługi Team Services (**https://{youraccount}.visualstudio.com**). 
  Możesz uzyskać [bezpłatnego konta usługi Team Services](https://go.microsoft.com/fwlink/?LinkId=307137&clcid=0x409&wt.mc_id=o~msft~vscom~home-vsts-hero~27308&campaign=o~msft~vscom~home-vsts-hero~27308).

  > [!NOTE]
  > Aby uzyskać więcej informacji, zobacz [nawiązywanie połączenia z Team Services](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services).

*  Należy maszyny wirtualnej systemu Linux dla celu wdrożenia.  Aby uzyskać więcej informacji, zobacz [tworzenie i zarządzanie nimi maszyn wirtualnych systemu Linux z wiersza polecenia platformy Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm).

*  Otwórz port wejściowy 80 dla maszyny wirtualnej. Aby uzyskać więcej informacji, zobacz [Utwórz grupy zabezpieczeń sieci przy użyciu portalu Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal).

## <a name="get-the-sample-app"></a>Pobierz przykładową aplikację

Należy aplikację do wdrożenia, przechowywane w repozytorium Git.
W tym samouczku, firma Microsoft zaleca się używanie [tej przykładowej aplikacji dostępne w serwisie GitHub](https://github.com/azooinmyluggage/fabrikam-node). Ten samouczek zawiera przykładowy skrypt, który jest używany do instalowania programu Node.js i aplikacji. Do pracy z własnych repozytorium, należy skonfigurować przykładowe podobne.

Utwórz rozwidlenia tej aplikacji, a następnie zanotuj lokalizację (adres URL) do użycia w kolejnych krokach tego samouczka. Aby uzyskać więcej informacji, zobacz [rozwidlania repozytorium](https://help.github.com/articles/fork-a-repo/).    

> [!NOTE]
> Aplikacja została skompilowana przy użyciu [narzędzia Yeoman](http://yeoman.io/learning/index.html). Używa Express, bower i grunt. I ma niektórych pakietów npm jako zależności.
> Przykład zawiera także skrypt, który konfiguruje Nginx, a następnie wdroży aplikację. Jest ona wykonywana na maszynach wirtualnych. W szczególności skrypt:
> 1. Instaluje węzła, Nginx i PM2.
> 2. Konfiguruje Nginx i PM2.
> 3. Uruchamia aplikację węzła.

## <a name="configure-jenkins-plug-ins"></a>Skonfiguruj Wpięć dodatki plug-in

Najpierw należy skonfigurować dwa wtyczek Wpięć: **NodeJS** i **VS Team Services ciągłe wdrażanie**.

1. Otwórz konto Wpięć i wybierz **Zarządzanie Wpięć**.
2. Na **Zarządzanie Wpięć** wybierz pozycję **Zarządzanie wtyczkami**.
3. Filtruj listę, aby zlokalizować **NodeJS** wtyczki i wybierz pozycję **zainstalować bez ponownego uruchomienia** opcji.
    ![Dodawanie wtyczki NodeJS do Wpięć](media/tutorial-build-deploy-jenkins/jenkins-nodejs-plugin.png)
4. Filtruj listę, aby znaleźć **VS Team Services ciągłe wdrażanie** wtyczki i wybierz pozycję **zainstalować bez ponownego uruchomienia** opcji.
5. Wróć do pulpitu nawigacyjnego Wpięć i wybierz **Zarządzanie Wpięć**.
6. Wybierz **konfiguracji narzędzie globalne**. Znajdź **NodeJS** i wybierz **instalacje NodeJS**.
7. Wybierz **są instalowane automatycznie** opcji, a następnie wprowadź **nazwa** wartość.
8. Wybierz pozycję **Zapisz**.

## <a name="configure-a-jenkins-freestyle-project-for-nodejs"></a>Konfigurowanie projektu dowolne Wpięć dla środowiska Node.js

1. Wybierz **nowy element**. Wprowadź nazwę elementu.
2. Wybierz **stylu projektu**. Kliknij przycisk **OK**.
3. Na **zarządzania kodem źródłowym** wybierz opcję **Git** , a następnie wprowadź szczegóły repozytorium i gałąź zawierające kod aplikacji.    
    ![Dodaj repozytorium do kompilacji](media/tutorial-build-deploy-jenkins/jenkins-git.png)
4. Na **kompilacji wyzwalaczy** wybierz opcję **SCM sondowania** , a następnie wprowadź harmonogram `H/03 * * * *` sondowanie repozytorium Git dla zmian co trzy minuty. 
5. Na **Build Environment** wybierz opcję **Podaj węzła &amp; npm bin / ŚCIEŻKĘ folderu** i wybierz **instalacji NodeJS** wartość. Pozostaw **pliku npmrc** ustawioną **Użyj domyślnej systemu**.
6. Na **kompilacji** wybierz opcję **wykonywania powłoki** , a następnie wprowadź polecenie `npm install` aby upewnić się, że wszystkie zależności są aktualizowane.


## <a name="configure-jenkins-for-team-services-integration"></a>Skonfiguruj Wpięć integracji usługi Team Services

> [!NOTE]
> Upewnij się, że osobisty token dostępu (PAWEŁ) można używać dla następujących kroków zawiera *wersji* (Odczyt, zapis, wykonanie i zarządzanie) uprawnień w Team Services.
 
1.  Utwórz jest element PAT w ramach konta usługi Team Services, jeśli nie został wcześniej. Wpięć wymaga tych informacji, aby uzyskać dostęp do konta usługi Team Services. Pamiętaj przechowywać informacji o tokenie dla kolejnych kroków w tej sekcji.
  
    Aby dowiedzieć się, jak wygenerować token, przeczytaj [jak utworzyć osobisty token dostępu TFS i programu VSTS?](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate).
2. W **akcje postkompilacyjnego** wybierz opcję **dodać akcję postkompilacyjnego**. Wybierz **archiwum artefaktów**.
3. Aby uzyskać **pliki do archiwum**, wprowadź `**/*` uwzględnienie wszystkich plików.
4. Aby utworzyć inną akcję, wybierz **dodać akcję postkompilacyjnego**.
5. Wybierz **wyzwalanie wydania w TFS/Team Services**. Wprowadź identyfikator URI dla konta usługi Team Services, takich jak **https://{your-account-name}.visualstudio.com**.
6. Wprowadź **projektu zespołowego** nazwy.
7. Wybierz nazwę dla definicji wersji. (Tworzenia tej wersji definicji w dalszej części Team Services.)
8. Wybierz poświadczenia niezbędne do połączenia ze środowiskiem usługi Team Services lub program Team Foundation Server:
   - Pozostaw **Username** puste, jeśli używasz usługi Team Services. 
   - Wprowadź nazwę użytkownika i hasło, korzystając z lokalnej wersji serwera Team Foundation Server.    
   ![Konfigurowanie Wpięć działania mające miejsce po kompilacji](media/tutorial-build-deploy-jenkins/trigger-release-from-jenkins.png)
5. Zapisz projekt Wpięć.


## <a name="create-a-jenkins-service-endpoint"></a>Tworzenie punktu końcowego usługi Wpięć

Punkt końcowy usługi umożliwia usługom zespołu nawiązać połączenia z Wpięć.

1. Otwórz **usług** strony w Team Services, otwórz **nowy punkt końcowy usługi** listy, a następnie wybierz **Wpięć**.
   ![Dodawanie punktu końcowego Wpięć](media/tutorial-build-deploy-jenkins/add-jenkins-endpoint.png)
2. Wprowadź nazwę dla połączenia.
3. Wprowadź adres URL serwera Wpięć, a następnie wybierz **zaakceptować niezaufane certyfikaty protokołu SSL** opcji. Przykładowy adres URL jest **http://{YourJenkinsURL}.westcentralus.cloudapp.azure.com**.
4. Wprowadź nazwę użytkownika i hasło do konta Wpięć.
5. Wybierz **sprawdzić połączenie** do sprawdzenia, czy informacje są poprawne.
6. Wybierz **OK** można utworzyć punktu końcowego usługi.

## <a name="create-a-deployment-group-for-azure-virtual-machines"></a>Utwórz grupę wdrożenia maszyn wirtualnych platformy Azure

Należy [grupę wdrożenia](https://www.visualstudio.com/docs/build/concepts/definitions/release/deployment-groups/) można zarejestrować agenta usługi Team Services, dlatego definicji wersji można wdrożyć do maszyny wirtualnej. Wdrożenie grupy ułatwiają określenie logiczne grupy komputerów docelowych dla wdrożenia i zainstalować agenta wymagane na każdej maszynie.

   > [!NOTE]
   > W poniższej procedurze należy zainstalować wymagania wstępne i *nie Uruchom skrypt z uprawnieniami sudo.*

1. Otwórz **wersje** karcie **kompilacji &amp; wersji** koncentratora, otwórz **grupy wdrożenia**i wybierz **+ nowy**.
2. Wprowadź nazwę grupy wdrożenia i opcjonalny opis. Następnie wybierz przycisk **Utwórz**.
3. Wybierz system operacyjny dla maszyny wirtualnej docelowej wdrożenia. Na przykład wybierz **Ubuntu 16.04 +**.
4. Wybierz **do uwierzytelniania należy używać osobisty token dostępu w skrypcie**.
5. Wybierz **wymagania wstępne systemu** łącza. Zainstaluj wymagania wstępne dotyczące systemu operacyjnego.
6. Wybierz **Skopiuj skrypt do Schowka** do Skopiuj skrypt.
7. Zaloguj się do wdrożenia maszyny wirtualnej docelowy i uruchom skrypt. Nie można uruchomić skrypt z uprawnieniami sudo.
8. Po zakończeniu instalacji zostanie wyświetlony monit o wdrożenia grupy tagów. Zaakceptuj ustawienia domyślne.
9. W usłudze Team Services, sprawdź nowo zarejestrowanych maszyny wirtualnej w taki sposób, w **celów** w obszarze **grupy wdrożenia**.

## <a name="create-a-team-services-release-definition"></a>Tworzenie definicji wersji Team Services

Definicja wersji określa proces, który używa usługi Team Services do wdrażania aplikacji. W tym przykładzie można wykonać skryptu powłoki.

Aby utworzyć definicję wersji w usłudze Team Services:

1. Otwórz **wersje** karcie **kompilacji &amp; wersji** koncentratora, a następnie wybierz **Tworzenie wersji definicji**. 
2. Wybierz **pusty** szablonu, wybierając pozycję początkową **pusta procesu**.
3. W **artefakty** zaznacz **+ Dodaj artefaktu** i wybierz polecenie **Wpięć** dla **typ źródła**. Wybierz połączenie Wpięć usługi punktu końcowego. Następnie wybierz zadanie źródła Wpięć i wybierz **Dodaj**.
4. Wybierz przycisk wielokropka obok **1 środowiska**. Wybierz **Dodaj wdrożenie w grupie fazie**.
5. Wybierz grupy wdrożenia.
5. Wybierz  **+**  można dodać zadania do **faza wdrożenia grupy**.
6. Wybierz **skrypt powłoki** zadań i wybierz **Dodaj**. **Skrypt powłoki** zadań zapewnia konfiguracji skrypt uruchamiany na każdym serwerze, aby można było zainstalować Node.js i uruchomić aplikację.
8. Aby uzyskać **ścieżka skryptu**, wprowadź **$(System.DefaultWorkingDirectory)/Fabrikam-Node/deployscript.sh**.
9. Wybierz **zaawansowane**, a następnie Włącz **określić katalog roboczy**.
10. Aby uzyskać **katalog roboczy**, wprowadź **$(System.DefaultWorkingDirectory) / Fabrikam węzła**.
11. Edytuj nazwę definicji wersji, do nazwy określonej w **działania mające miejsce po kompilacji** kartę kompilacji w Wpięć. Wpięć wymaga tej nazwy, aby można było do wyzwolenia nową wersję, po zaktualizowaniu artefakty źródła.
12. Wybierz **zapisać** i wybierz **OK** można zapisać definicji wersji.

## <a name="execute-manual-and-ci-triggered-deployments"></a>Wdrożenia ręczne i wyzwalane CI wykonać

1. Wybierz **+ wersji** i wybierz **Tworzenie wersji**.
2. W wyróżnionych listy rozwijanej wybierz kompilacji, która została ukończona, a następnie wybierz **kolejki**.
3. Wybierz łącze wersji komunikat podręczny. Na przykład: "wersji **wersji 1** został utworzony."
4. Otwórz **dzienniki** kartę, aby obejrzeć dane wyjściowe konsoli wersji.
5. W przeglądarce otwórz adres URL jednego z serwerów, które zostały dodane do tej grupy wdrożenia. Na przykład wprowadź **http://{your-server-ip-address}**.
6. Przejdź do repozytorium Git źródła i zmodyfikowania zawartości **h1** pozycji app/views/index.jade pliku z tekstem zmienione.
7. Zatwierdź zmiany.
8. Po kilku minutach, pojawi się nowe wydanie utworzone na **wersje** strona Team Services lub program Team Foundation Server. Otwórz wydania do wdrażania odbywa się w temacie. Gratulacje!

## <a name="next-steps"></a>Następne kroki

W tym samouczku można automatyczne wdrażanie aplikacji na platformie Azure przy użyciu Wpięć dla kompilacji i usługi Team Services w wersji. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie aplikacji w Wpięć.
> * Skonfiguruj Wpięć integracji usługi Team Services.
> * Utwórz grupę wdrożenia dla maszyn wirtualnych platformy Azure.
> * Tworzenie definicji wersji, która umożliwia skonfigurowanie maszyn wirtualnych, a następnie wdroży aplikację.

Aby dowiedzieć się więcej o sposobie wdrażania światło (Linux, Apache MySQL i PHP) stosu, przejdź do następnego samouczka.

> [!div class="nextstepaction"]
> [Wdrażanie stosu LAMP](tutorial-lamp-stack.md)
