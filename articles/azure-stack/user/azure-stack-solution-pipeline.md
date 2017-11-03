---
title: "Wdrażanie aplikacji na platformie Azure oraz Azure stosu | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wdrażać aplikacje na platformie Azure oraz Azure stosu z potokiem CI/CD hybrydowego."
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: helaw
ms.custom: mvc
ms.openlocfilehash: 83bb401d5d65cd2c34015a1a14673363aeee81d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-apps-to-azure-and-azure-stack"></a>Wdrażanie aplikacji na platformie Azure oraz Azure stosu
*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Hybrydowego [ciągłej integracji](https://www.visualstudio.com/learn/what-is-continuous-integration/)/[ciągłego dostarczania](https://www.visualstudio.com/learn/what-is-continuous-delivery/)potoku (CI/CD) pozwala na tworzenie, testowanie i wdrażanie aplikacji na wiele chmur.  W tym samouczku tworzenia środowiska próbki, aby dowiedzieć się, jak potoku CI/CD hybrydowego można:
 
> [!div class="checklist"]
> * Rozpocznij nową kompilację oparte na kodzie zatwierdzeń do repozytorium programu Visual Studio Team Services (VSTS).
> * Automatyczne wdrażanie nowo zbudowany kodu na platformie Azure dla testów akceptacyjnych przez użytkowników.
> * Po kodzie testowania automatycznie wdraża aplikację Azure stosu. 


## <a name="prerequisites"></a>Wymagania wstępne
Kilka składników są wymagane do utworzenia potoku CI/CD hybrydowego i może zająć trochę czasu, aby przygotować.  Jeśli masz już niektóre z tych składników, upewnij się, że spełniają one wymagania przed rozpoczęciem.

W tym temacie założono również, czy masz pewna znajomość Azure oraz Azure stosu. Jeśli chcesz dowiedzieć się więcej, aby kontynuować, należy koniecznie zaczynać się w tych tematach:

- [Wprowadzenie do platformy Azure](https://docs.microsoft.com/azure/fundamentals-introduction-to-azure)
- [Kluczowe założenia Azure stosu](../azure-stack-key-features.md)

### <a name="azure"></a>Azure
 - Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
 - Utwórz [aplikacji sieci Web](../../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md)i konfigurowania jej dla [publikacji FTP](../../app-service/app-service-deploy-ftp.md).  Zanotuj URL nowej aplikacji sieci Web, które jest używane później.


### <a name="azure-stack"></a>Azure Stack
 - [Wdrażanie usługi Azure stosu](../azure-stack-run-powershell-script.md).  Instalacja zwykle ma kilka godzin, aby zakończyć, więc odpowiednio zaplanować.
 - Wdrażanie [usługi aplikacji](../azure-stack-app-service-deploy.md) PaaS usług Azure stosu.
 - Tworzenie aplikacji sieci Web i konfigurowania jej dla [publikacji FTP](../azure-stack-app-service-enable-ftp.md).  Zanotuj URL nowej aplikacji sieci Web, które jest używane później.  

### <a name="developer-tools"></a>Narzędzia dla deweloperów
 - Utwórz [obszaru roboczego programu VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).  Proces tworzenia konta tworzy projekt o nazwie "MyFirstProject."  
 - [Zainstaluj program Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) i [logowania do programu VSTS](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#connect-and-share-code-from-visual-studio)
 - Połącz się z projektem i [sklonować lokalnie](https://www.visualstudio.com/docs/git/gitquickstart).
 - Utwórz [puli agenta](https://www.visualstudio.com/docs/build/concepts/agents/pools-queues#creating-agent-pools-and-queues) w VSTS.
 - Instalowanie programu Visual Studio i wdrażanie [agenta kompilacji programu VSTS](https://www.visualstudio.com/docs/build/actions/agents/v2-windows) do maszyny wirtualnej na stosie Azure. 
 

## <a name="create-app--push-to-vsts"></a>Tworzenie aplikacji & wypychania do programu VSTS

### <a name="create-application"></a>Tworzenie aplikacji
W tej sekcji możesz utworzyć prostą aplikację ASP.NET i wypchnąć go do usługi VSTS.  Czynności te reprezentują developer normalnych przepływu pracy i może być dostosowane do narzędzia dla deweloperów i języków. 

1.  Otwórz program Visual Studio.
2.  Z obszaru Team Explorer i **rozwiązania...**  obszaru, kliknij przycisk **nowy**.
3.  Wybierz **Visual C#** > **Web** > **aplikacji sieci Web platformy ASP.NET (.NET Framework)**.
4.  Podaj nazwę aplikacji i kliknij przycisk **OK**.
5.  Na następnym ekranie, zachować ustawienia domyślne (formularze sieci Web), a następnie kliknij przycisk **OK**.

### <a name="commit-and-push-changes-to-vsts"></a>Zatwierdź i Wypchnij zmiany do programu VSTS
1.  W programie Visual Studio przy użyciu programu Team Explorer, wybierz z listy rozwijanej, a następnie kliknij przycisk **zmiany**.
2.  Podaj komunikat zatwierdzenia i wybierz **zatwierdzić wszystkie**. Może pojawić się prośba do zapisywania pliku rozwiązania, kliknij przycisk Tak, aby zapisać wszystkie.
3.  Po zatwierdzone, Visual Studio oferuje zsynchronizować zmiany do projektu. Wybierz **synchronizacji**.

    ![Obraz przedstawiający ekranu zatwierdzania po zakończeniu przekazywania](./media/azure-stack-solution-pipeline/image1.png)

4.  Na karcie Synchronizacja w obszarze *wychodzące*, zobacz Twoje nowe zatwierdzenia.  Wybierz **Push** zsynchronizować zmiany do usługi VSTS.

    ![Obraz przedstawiający synchronizacji kroki](./media/azure-stack-solution-pipeline/image2.png)

### <a name="review-code-in-vsts"></a>Przegląd kodu w VSTS
Po zostały zatwierdzone zmiany oraz do programu VSTS, sprawdź swój kod z portalu usługi VSTS.  Wybierz **kod**, a następnie **pliki** z menu rozwijanego.  Widać rozwiązania, który został utworzony.

## <a name="create-build-definition"></a>Tworzenie definicji kompilacji
Proces kompilacji definiuje sposób wbudowane i pakiecie w celu wdrażania na każdym zatwierdzeniu zmian w kodzie aplikacji. W naszym przykładzie używamy dołączone szablonu do konfigurowania procesu kompilacji dla aplikacji ASP.NET, chociaż ta konfiguracja może być dostosowywane w zależności od aplikacji.

1.  Zaloguj się do swojego obszaru roboczego programu VSTS z przeglądarki sieci web.
2.  Transparent, zaznacz **kompilacji i wydania** , a następnie **kompilacje**.
3.  Kliknij przycisk **+ nową definicję**.
4.  Wybierz z listy szablonów **ASP.NET (wersja zapoznawcza)** i wybierz **Zastosuj**.
5.  Modyfikowanie *argumenty programu MSBuild* w *Kompiluj rozwiązanie* kroku:

    `/p:DeployOnBuild=True /p:WebPublishMethod=FileSystem /p:DeployDefaultTarget=WebPublish /p:publishUrl="$(build.artifactstagingdirectory)\\"`

6.  Wybierz **opcje** , a następnie wybierz kolejkę agenta na agenta kompilacji wdrożone na maszynie wirtualnej na stosie Azure. 
7.  Wybierz **wyzwalaczy** , a następnie Włącz **ciągłej integracji**.
7.  Kliknij przycisk **Zapisz & kolejka** , a następnie wybierz **zapisać** z listy rozwijanej. 

## <a name="create-release-definition"></a>Utwórz definicję zlecenia
Proces wersji definiuje sposób wdrożenia kompilacji z poprzedniego kroku do środowiska.  W tym samouczku firma Microsoft publikuje aplikacji ASP.NET przy użyciu FTP do aplikacji sieci Web platformy Azure. Aby skonfigurować zlecenia do platformy Azure, wykonaj następujące kroki:

1.  Transparent VSTS zaznacz **kompilacji i wydania** , a następnie **wersje**.
2.  Kliknij zielony **+ nową definicję**.
3.  Wybierz **pusty** i kliknij przycisk **dalej**.
4.  Pole wyboru dla *ciągłe wdrażanie*, a następnie kliknij przycisk **Utwórz**.

Utworzeniu definicji wersji pusta i z kompilacji, dodamy procedura środowiska platformy Azure:

1.  Kliknij zielony  **+**  można dodać zadania.
2.  Wybierz **wszystkie**, a następnie na liście, Dodaj **przekazać FTP** i wybierz **Zamknij**.
3.  Wybierz **FTP przekazać** zadań można tylko dodać i skonfigurować następujące parametry:
    
    | Parametr | Wartość |
    | ----- | ----- |
    |Metoda uwierzytelniania| Wprowadź poświadczenia|
    |Adres URL serwera | Adres URL sieci Web aplikacji FTP pobrany z portalu Azure |
    |Nazwa użytkownika | Nazwa użytkownika skonfigurowane podczas tworzenia poświadczeń FTP dla aplikacji sieci Web |
    |Hasło | Hasła utworzonego podczas ustanawiania FTP poświadczenia dla aplikacji sieci Web|
    |Katalog źródłowy | $(System.DefaultWorkingDirectory)\**\ |
    |Katalog zdalny | /Site/wwwroot / |
    |Zachowaj ścieżki do plików | Włączony (zaznaczone)|

4.  Kliknij przycisk **Zapisz**

Teraz można skonfigurować definicji wersji do używania puli agenta zawierającego agenta wdrożone, wykonując następujące czynności:
1.  Wybierz definicji wersji, a następnie kliknij przycisk **Edytuj**.
2.  Wybierz **Uruchom agenta** w środkowej kolumnie.  W prawej kolumnie Wybierz kolejki agenta zawierającego agenta kompilacji systemem Azure stosu.  
    ![Obraz przedstawiający konfigurację definicji wersji, można użyć określonej kolejki](./media/azure-stack-solution-pipeline/image3.png)


## <a name="deploy-your-app-to-azure"></a>Wdrażanie aplikacji na platformie Azure
Ten krok korzysta z nowo zbudowany potok CI/CD można wdrożyć aplikacji ASP.NET w aplikacji sieci Web na platformie Azure. 

1.  Na transparencie VSTS, zaznacz **kompilacji i wydania**, a następnie wybierz **kompilacje**.
2.  Kliknij przycisk **...**  w definicji kompilacji wcześniej utworzony i wybierz **nowej kompilacji w kolejce**.
3.  Zaakceptuj ustawienia domyślne, a następnie kliknij przycisk **Ok**.  Kompilacja rozpoczyna się i wyświetla postęp.
4.  Po zakończeniu kompilacji, wybierając można śledzić stan **kompilacji i wydania** i wybierając **wersje**.
5.  Po zakończeniu kompilacji, odwiedź witrynę sieci Web przy użyciu adresu URL, podczas tworzenia aplikacji sieci Web.    


## <a name="add-azure-stack-to-pipeline"></a>Dodaj stosu Azure do potoku
Obecnie planowaną CI/CD zostały przetestowane przez wdrażanie na platformie Azure, jest dodać stosu Azure do potoku.  W poniższych krokach tworzenia nowego środowiska i dodać zadanie FTP przekazać do wdrożenia aplikacji Azure stosu.  Możesz również dodać osoba zatwierdzająca wersji, która służy jako sposób, aby symulować "off" Podpisywanie na zlecenia kodu do stosu usługi Azure.  

1.  W definicji wersji, wybierz **+ Dodaj środowiska** i **Tworzenie nowego środowiska**.
2.  Wybierz **pusty**, kliknij przycisk **dalej**.
3.  Wybierz **określonych użytkowników** i określ konto.  Wybierz pozycję **Utwórz**.
4.  Zmień nazwę środowiska istniejącej nazwy i wpisując *stosu Azure*.
5.  Teraz, wybór środowiska Azure stosu, następnie wybierz **dodawać zadania**.
6.  Wybierz **FTP przekazać** zadań i wybierz **Dodaj**, a następnie wybierz pozycję **Zamknij**.


### <a name="configure-ftp-task"></a>Konfigurowanie zadań FTP
Teraz, po utworzeniu zlecenia, należy skonfigurować kroki wymagane do publikowania aplikacji sieci Web na stosie Azure.  Podobnie jak zadań Przekaż FTP jest skonfigurowane dla platformy Azure, należy skonfigurować zadania Azure stosu:

1.  Wybierz **FTP przekazać** zadań można tylko dodać i skonfigurować następujące parametry:
    
    | Parametr | Wartość |
    | -----     | ----- |
    |Metoda uwierzytelniania| Wprowadź poświadczenia|
    |Adres URL serwera | Adres URL FTP aplikacji sieci Web pobrany z portalu Azure stosu |
    |Nazwa użytkownika | Nazwa użytkownika skonfigurowane podczas tworzenia poświadczeń FTP dla aplikacji sieci Web |
    |Hasło | Hasła utworzonego podczas ustanawiania FTP poświadczenia dla aplikacji sieci Web|
    |Katalog źródłowy | $(System.DefaultWorkingDirectory)\**\ |
    |Katalog zdalny | /Site/wwwroot /|
    |Zachowaj ścieżki do plików | Włączony (zaznaczone)|

2.  Kliknij przycisk **Zapisz**

Na koniec skonfiguruj definicji wersji do używania puli agenta zawierającego agenta wdrożone, wykonując następujące czynności:
1.  Wybierz definicji wersji, a następnie kliknij przycisk **edycji**
2.  Wybierz **Uruchom agenta** w środkowej kolumnie. W prawej kolumnie Wybierz kolejki agenta zawierającego agenta kompilacji systemem Azure stosu.  
    ![Obraz przedstawiający konfigurację definicji wersji, można użyć określonej kolejki](./media/azure-stack-solution-pipeline/image3.png)

## <a name="deploy-new-code"></a>Wdrażanie nowego kodu
Teraz możesz przetestować potoku CI/CD hybrydowych, z ostatnim krokiem publikowania stos Azure.  W tej sekcji zmodyfikuj stopki witryny i rozpocząć wdrażanie za pośrednictwem potoku.  Po zakończeniu wyświetlona zostanie wdrożony na platformie Azure do przeglądu zmiany, a następnie po zaakceptowaniu wersji, zostaną opublikowane na stosie Azure.

1. W programie Visual Studio Otwórz *site.master* plików i zmień ten wiersz:
    
    `
        <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application</p>
    `

    wprowadź następujące zmiany:

    `
        <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application delivered by VSTS, Azure, and Azure Stack</p>
    `
3.  Zatwierdź zmiany i synchronizacji do programu VSTS.  
4.  W obszarze roboczym VSTS sprawdzić stan kompilacji, wybierając **kompilacji i wydania** > **kompilacji**
5.  Zostanie wyświetlone kompilacji w toku.  Kliknij dwukrotnie stan, i obserwować postęp kompilacji.  Po "Zakończono kompilacji" zostanie wyświetlony w konsoli, przejdź do sprawdzenia wydania z **kompilacji i wydania** > **wersji**.  Kliknij dwukrotnie wersji.
6.  Otrzymasz powiadomienie, że zlecenia wymaga przeglądu. Sprawdź adres URL aplikacji sieci Web i sprawdź, czy występują nowych zmian.  Zatwierdzanie wersji w VSTS.
    ![Obraz przedstawiający konfigurację definicji wersji, można użyć określonej kolejki](./media/azure-stack-solution-pipeline/image4.png)
    
7.  Sprawdź publikowanie stos Azure wszystkie odwiedzając witrynę sieci Web przy użyciu adresu URL, podczas tworzenia aplikacji sieci Web.
    ![Obraz przedstawiający ASP. NEt aplikacji za pomocą stopki zmienione](./media/azure-stack-solution-pipeline/image5.png)


Można teraz używać potoku sieci nowego elementu konfiguracji/CD hybrydowego jako bloku konstrukcyjnego dla innych wzorcach chmury hybrydowej.

## <a name="next-steps"></a>Następne kroki
W tym samouczku przedstawiono sposób tworzenia hybrydowego potoku CI/CD, który:

> [!div class="checklist"]
> * Inicjuje nową kompilację na podstawie kodu zatwierdza do repozytorium programu Visual Studio Team Services (VSTS).
> * Automatycznie wdraża kodzie nowo zbudowany na platformie Azure dla testów akceptacyjnych przez użytkowników.
> * Po kodzie testowania jest wdrażana automatycznie na stosie Azure. 

Teraz, gdy masz potoku CI/CD hybrydowego nadal nauczyć opracowywać aplikacje dla stosu Azure.

> [!div class="nextstepaction"]
> [Opracowywanie zawartości dla usługi Azure Stack](azure-stack-developer.md)


