<properties 
    pageTitle="Wdrażanie pierwszej aplikacji sieci Web na platformie Azure w ciągu 5 minut" 
    description="Dowiedz się, jak łatwo można uruchamiać aplikacje sieci Web w usłudze App Service, wdrażając przykładową aplikację w zaledwie kilku krokach. Rozpocznij rzeczywiste tworzenie aplikacji w ciągu 5 minut i od razu zobacz wyniki." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin" 
    manager="wpickett" 
    editor="" 
/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article"
    ms.date="05/12/2016" 
    ms.author="cephalin"
/>
    
# Wdrażanie pierwszej aplikacji sieci Web na platformie Azure w ciągu 5 minut

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Ten samouczek ułatwia wdrażanie pierwszej aplikacji sieci Web w [usłudze Azure App Service](../app-service/app-service-value-prop-what-is.md). Usługa App Service umożliwia tworzenie aplikacji sieci Web, [zapleczy aplikacji mobilnych](/documentation/learning-paths/appservice-mobileapps/) oraz [aplikacji interfejsu API](../app-service-api/app-service-api-apps-why-best-platform.md).

Po wykonaniu zaledwie kilku czynności: 

- wdrożysz przykładową aplikację sieci Web (z możliwością wyboru technologii ASP.NET, PHP, Node.js, Java lub Python);
- wyświetlisz działającą aplikację w ciągu kilku sekund;
- zaktualizujesz aplikację sieci Web w taki sam sposób, w jaki [wypycha się zatwierdzenia Git](https://git-scm.com/docs/git-push).

Zapoznasz się również z [Portalem Azure](https://portal.azure.com) i poznasz dostępne w nim funkcje. 

## Wymagania wstępne

- [Zainstaluj oprogramowanie Git](http://www.git-scm.com/downloads). 
- [Zainstaluj interfejs wiersza polecenia platformy Azure](../xplat-cli-install.md). 
- Utwórz konto platformy Microsoft Azure. Jeśli nie masz konta, możesz [utworzyć konto bezpłatnej wersji próbnej](/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

>[AZURE.NOTE] Zobacz działającą aplikację sieci Web. Od razu [wypróbuj usługę App Service](http://go.microsoft.com/fwlink/?LinkId=523751) i utwórz początkową aplikację o krótkim okresie istnienia — bez karty kredytowej i bez zobowiązań.

## Wdrażanie aplikacji sieci Web

Wdróżmy aplikację sieci Web w usłudze Azure App Service. 

1. Otwórz nowy wiersz polecenia systemu Windows, okno programu PowerShell, powłokę systemu Linux lub terminal systemu OS X. Uruchom polecenia `git --version` i `azure --version`, aby upewnić się, że oprogramowanie Git i interfejs wiersza polecenia platformy Azure są zainstalowane na komputerze. 

    ![Testowanie instalacji narzędzi interfejsu wiersza polecenia dla pierwszej aplikacji sieci Web na platformie Azure](./media/app-service-web-get-started/1-test-tools.png)

    Jeśli narzędzia nie zostały zainstalowane, zobacz [Wymagania wstępne](#Prerequisites), aby uzyskać linki do pobierania.

1. `CD` — za pomocą tej komendy przejdź do katalogu roboczego i sklonuj przykładową aplikację w następujący sposób:

        git clone <github_sample_url>

    ![Klonowanie kodu przykładowej aplikacji dla pierwszej aplikacji sieci Web na platformie Azure](./media/app-service-web-get-started/2-clone-sample.png)

    Dla parametru *&lt;github_sample_url>* użyj jednego z następujących adresów URL, zależnie od preferowanego środowiska: 

    - HTML+CSS+JS: [https://github.com/Azure-Samples/app-service-web-html-get-started.git](https://github.com/Azure-Samples/app-service-web-html-get-started.git)
    - ASP.NET: [https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git](https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git)
    - PHP (CodeIgniter): [https://github.com/Azure-Samples/app-service-web-php-get-started.git](https://github.com/Azure-Samples/app-service-web-php-get-started.git)
    - Node.js (Express): [https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git](https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git) 
    - Java: [https://github.com/Azure-Samples/app-service-web-java-get-started.git](https://github.com/Azure-Samples/app-service-web-java-get-started.git)
    - Python (Django): [https://github.com/Azure-Samples/app-service-web-python-get-started.git](https://github.com/Azure-Samples/app-service-web-python-get-started.git)

2. `CD` — za pomocą tej komendy przejdź do repozytorium przykładowej aplikacji. Na przykład: 

        cd app-service-web-html-get-started

3. Zaloguj się do platformy Azure w następujący sposób:

        azure login
    
    Postępuj zgodnie z instrukcjami w komunikacie pomocy, aby kontynuować proces logowania.
    
    ![Logowanie do platformy Azure w celu utworzenia pierwszej aplikacji sieci Web](./media/app-service-web-get-started/3-azure-login.png)

4. Utwórz zasób aplikacji App Service na platformie Azure, używając unikatowej nazwy aplikacji, za pomocą poniższego polecenia. Po wyświetleniu monitu podaj numer odpowiedniego regionu.

        azure site create --git <app_name>
    
    ![Tworzenie zasobu platformy Azure dla pierwszej aplikacji sieci Web na platformie Azure](./media/app-service-web-get-started/4-create-site.png)
    
    >[AZURE.NOTE] Jeśli nigdy nie skonfigurowano poświadczeń wdrożenia dla subskrypcji platformy Azure, zostanie wyświetlony monit z prośbą o ich utworzenie. Te poświadczenia, a nie poświadczenia konta Azure, są używane przez usługę App Service tylko do wdrażania przy użyciu usługi Git i logowania za pomocą protokołu FTP. 
    
    Aplikacja na platformie Azure została utworzona. Ponadto dla bieżącego katalogu zainicjowano obsługę narzędzia Git i podłączono go do nowej aplikacji usługi App Service jako zdalny katalog Git.
    Możesz przejść do adresu URL aplikacji (http://&lt;nazwa_aplikacji>.azurewebsites.net), aby wyświetlić atrakcyjną domyślną stronę HTML, ale teraz umieśćmy tam Twój kod.

4. Teraz wdróż przykładowy kod do nowej aplikacji usługi App Service, tak jak wypycha się dowolny kod przy użyciu narzędzia Git:

        git push azure master 

    ![Wypychanie kodu do pierwszej aplikacji sieci Web na platformie Azure](./media/app-service-web-get-started/5-push-code.png)    
    
    Jeśli użyto jednego ze środowisk języka, wyświetlone dane wyjściowe będą różnić się od przedstawionych powyżej. Dzieje się tak, ponieważ polecenie `git push` nie tylko umieszcza kod na platformie Azure, ale wyzwala również zadania wdrażania w aparacie wdrażania. Jeśli w katalogu głównym (repozytorium) projektu znajduje się plik package.json (Node.js) lub requirements.txt (Python) albo w projekcie ASP.NET znajduje się plik packages.config, wymagane pakiety zostaną przywrócone przez skrypty wdrażania. Możesz również [włączyć rozszerzenie Composer](web-sites-php-mysql-deploy-use-git.md#composer), aby automatycznie przetwarzać pliki composer.json w aplikacji w języku PHP.

Gratulacje, wdrożono aplikację w usłudze Azure App Service. 

## Wyświetlanie działającej aplikacji

Aby wyświetlić aplikację działającą na platformie Azure, uruchom to polecenie w dowolnym katalogu repozytorium:

    azure site browse

## Aktualizowanie aplikacji

Teraz możesz wypychać zmiany z katalogu głównego projektu (repozytorium) przy użyciu narzędzia Git, aby w dowolnej chwili aktualizować działającą witrynę. Procedura jest taka sama, jak podczas wdrażania aplikacji na platformie Azure po raz pierwszy. Na przykład za każdym razem, gdy chcesz wypchnąć nową zmianę przetestowaną lokalnie, uruchom następujące polecenia w katalogu głównym (repozytorium) projektu:
    
    git add .
    git commit -m "<your_message>"
    git push azure master

## Wyświetlanie aplikacji w Portalu Azure

Teraz przejdź do Portalu Azure, aby wyświetlić utworzoną aplikację:

1. Zaloguj się do [Portalu Azure](https://portal.azure.com) przy użyciu poświadczeń konta Microsoft zawierającego subskrypcję platformy Azure.

2. Na pasku po lewej stronie kliknij pozycję **Usługi App Services**.

3. Kliknij utworzoną aplikację, aby otworzyć jej stronę w portalu (nazywaną [blokiem](../azure-portal-overview.md)). Domyślnie, dla Twojej wygody, otwierany jest również blok **Ustawienia**.

    ![Widok pierwszej aplikacji sieci Web w Portalu Azure](./media/app-service-web-get-started/portal-view.png) 

W bloku aplikacji usługi App Service w portalu jest dostępny bogaty zestaw ustawień i narzędzi do konfigurowania, monitorowania i zabezpieczania aplikacji oraz rozwiązywania problemów z aplikacją. Poświęć chwilę, aby zapoznać się z tym interfejsem, wykonując kilka prostych zadań (numer zadania odpowiada numerowi na zrzucie ekranu):

1. Zatrzymaj aplikację.
2. Ponownie uruchom aplikację.
3. Kliknij link **Grupa zasobów**, aby wyświetlić zasoby wdrożone w grupie zasobów.
4. Kliknij pozycję **Ustawienia** > **Właściwości**, aby wyświetlić pozostałe informacje dotyczące aplikacji.
5. Kliknij pozycję **Narzędzia**, aby uzyskać dostęp do przydatnych narzędzi do monitorowania i rozwiązywania problemów.  

## Następne kroki

- Przenieś swoją aplikację Azure na wyższy poziom. Zabezpiecz ją przy użyciu uwierzytelniania. Skaluj ją zależnie od potrzeb. Skonfiguruj alerty dotyczące wydajności. Wszystkie te czynności możesz wykonać za pomocą kilku kliknięć. Zobacz [Dodawanie funkcji do pierwszej aplikacji sieci Web](app-service-web-get-started-2.md).
- Oprócz narzędzia Git i interfejsu wiersza polecenia platformy Azure istnieją inne sposoby wdrażania aplikacji sieci Web na platformie Azure. Zobacz [Deploy your app to Azure App Service](../app-service-web/web-sites-deploy.md) (Wdrażanie aplikacji w usłudze Azure App Service).
Znajdź odpowiednie kroki tworzenia i wdrażania aplikacji dla używanego środowiska języka, wybierając środowisko u góry tego artykułu.



<!--HONumber=Jun16_HO2-->


