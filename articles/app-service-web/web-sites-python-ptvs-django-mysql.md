<properties 
    pageTitle="Obsługa platformy Django i bazy danych MySQL na platformie Azure przy użyciu narzędzi Python Tools 2.2 for Visual Studio" 
    description="Dowiedz się, jak używać narzędzi Python Tools for Visual Studio do utworzenia aplikacji sieci Web Django przechowującej dane w wystąpieniu bazy danych MySQL, a następnie, jak ją wdrożyć w funkcji Web Apps w usłudze Azure App Service." 
    services="app-service\web" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python"
    ms.topic="get-started-article" 
    ms.date="06/01/2016"
    ms.author="huvalo"/>

# Obsługa platformy Django i bazy danych MySQL na platformie Azure przy użyciu narzędzi Python Tools 2.2 for Visual Studio 

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

W ramach tego samouczka użyjesz narzędzi [Python Tools for Visual Studio] (PTVS) w celu utworzenia prostej aplikacji sieci Web z ankietą, wykorzystując przykładowy szablon PTVS. Dowiesz się, jak używać usługi MySQL hostowanej na platformie Azure, jak skonfigurować aplikację sieci Web pod kątem MySQL i jak opublikować tę aplikację w funkcji [Web Apps w usłudze Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

> [AZURE.NOTE] Informacje zawarte w tym samouczku są również dostępne w poniższym klipie wideo:
> 
> [PTVS 2.1: Aplikacja Django z obsługą MySQL]([wideo])

Więcej artykułów o programowaniu funkcji Web Apps w usłudze Azure App Service przy użyciu narzędzi PTVS, środowisk sieci Web Bottle, Flask i Django oraz usług baz danych MongoDB, MySQL, SQL Database i Azure Table Storage możesz znaleźć w [Centrum deweloperów języka Python]. Chociaż ten artykuł dotyczy usługi App Service, opisane kroki są podobne do programowania [usług Azure Cloud Services].

## Wymagania wstępne

 - Program Visual Studio w wersji 2013 lub 2015
 - [32-bitowe środowisko Python w wersji 2.7]
 - [Narzędzia Python Tools 2.2 for Visual Studio]
 - [Zestaw przykładów VSIX dla narzędzi Python Tools 2.2 for Visual Studio]
 - [Azure SDK Tools for VS 2013] lub [Azure SDK Tools for VS 2015]
 - Django w wersji 1.6 lub starszej

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of the the previous include. -->

> [AZURE.NOTE] Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Try App Service](http://go.microsoft.com/fwlink/?LinkId=523751) (Wypróbuj usługę App Service), w którym wyjaśniono, jak od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service. Karta kredytowa nie jest wymagana i nie musisz się do niczego zobowiązywać.

## Tworzenie projektu

W tej sekcji utworzysz projekt programu Visual Studio przy użyciu przykładowego szablonu. Utworzysz środowisko wirtualne i zainstalujesz wymagane pakiety. Utworzysz także lokalną bazę danych przy użyciu systemu SQLite. Następnie uruchomisz aplikację lokalnie.

1. W programie Visual Studio wybierz pozycje **Plik**, **Nowy projekt**.

1. Szablony projektu z zestawu przykładów VSIX dla narzędzi PTVS są dostępne w menu **Python**, **Przykłady**. Wybierz pozycję **Polls Django Web Project** (Projekt sieci Web Django z ankietą) i kliknij przycisk OK, aby utworzyć projekt.

    ![Okno dialogowe Nowy projekt](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)

1. Zostanie wyświetlony monit o zainstalowanie pakietów zewnętrznych. Wybierz pozycję **Zainstaluj w środowisku wirtualnym**.

    ![Okno dialogowe Pakiety zewnętrzne](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)

1. Wybierz jako podstawowy interpreter **Python 2.7**.

    ![Okno dialogowe Dodawanie środowiska wirtualnego](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)

1. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy węzeł projektu i wybierz polecenie **Python**, a następnie polecenie **Baza danych synchronizacji Django**.

    ![Polecenie Baza danych synchronizacji Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSyncDB.png)

1. Spowoduje to otwarcie konsoli zarządzania Django i utworzenie bazy danych SQLite w folderze projektu. Postępuj zgodnie z monitami, aby utworzyć użytkownika.

    ![Okno konsoli zarządzania Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoConsole.png)

1. Sprawdź, czy aplikacja działa, naciskając `F5`.

1. Kliknij pozycję **Zaloguj** na górnym pasku nawigacyjnym.

    ![Pasek nawigacyjny Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)

1. Wprowadź poświadczenia użytkownika, który został utworzony podczas synchronizowania bazy danych.

    ![Formularz logowania](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)

1. Kliknij pozycję **Utwórz przykładową ankietę**.

    ![Tworzenie przykładowej ankiety](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)

1. Kliknij ankietę i zagłosuj.

    ![Głosowanie w przykładowej ankiecie](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## Tworzenie bazy danych MySQL

Utworzysz bazę danych ClearDB MySQL hostowaną na platformie Azure.

Ewentualnie możesz utworzyć własną maszynę wirtualną działającą na platformie Azure, a następnie samodzielnie zainstalować środowisko MySQL i administrować nim.

Wykonując poniższe kroki, możesz utworzyć bazę danych z użyciem planu Free.

1. Zaloguj się do [Azure Portal].

1. W górnej części okienka nawigacji kliknij kolejno pozycje: **NOWE**, **Dane i przechowywanie**, **Baza danych MySQL**. 

1. Wpisz „**mysql**” w polu wyszukiwania, a następnie kliknij kolejno pozycje: **Baza danych MySQL** i **Utwórz**.

    <!-- ![Choose Add-on Dialog](./media/web-sites-python-ptvs-django-mysql/PollsDjangoClearDBAddon1.png) -->

1. Skonfiguruj nową bazę danych MySQL, tworząc nową grupę zasobów i wybierając dla niej odpowiednią lokalizację.

    <!-- ![Personalize Add-on Dialog](./media/web-sites-python-ptvs-django-mysql/PollsDjangoClearDBAddon2.png) -->

1. Po utworzeniu bazy danych MySQL kliknij przycisk **Właściwości** w bloku bazy danych.

1. Użyj przycisku kopiowania, aby umieścić wartość elementu **PARAMETRY POŁĄCZENIA** w Schowku.

## Konfigurowanie projektu

W tej sekcji skonfigurujesz swoją aplikację sieci Web tak, aby używała utworzonej przed chwilą bazy danych MySQL. Zainstalujesz również dodatkowe pakiety Python niezbędne do korzystania z baz danych MySQL w Django. Następnie uruchomisz aplikację sieci Web lokalnie.

1. W programie Visual Studio otwórz plik **settings.py** z folderu *NazwaProjektu*. Wklej tymczasowo parametry połączenia w edytorze. Parametry połączenia mają następujący format:

        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>

    Zmień domyślny **APARAT** bazy danych na MySQL i ustaw wartości elementów **NAZWA**, **UŻYTKOWNIK**, **HASŁO** i **HOST** zgodnie z elementem **PARAMETRY POŁĄCZENIA**.

        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.mysql',
                'NAME': '<Database>',
                'USER': '<User Id>',
                'PASSWORD': '<Password>',
                'HOST': '<Data Source>',
                'PORT': '',
            }
        }


1. W Eksploratorze rozwiązań w obszarze **Środowiska Python** kliknij prawym przyciskiem myszy środowisko wirtualne i wybierz polecenie **Zainstaluj pakiet języka Python**.

1. Zainstaluj pakiet `mysql-python`, korzystając z opcji **easy_install**.

    ![Okno dialogowe Instalowanie pakietu](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)

1. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy węzeł projektu i wybierz polecenie **Python**, a następnie polecenie **Baza danych synchronizacji Django**.

    Spowoduje to utworzenie tabel w bazie danych MySQL utworzonej w poprzedniej sekcji. Postępuj zgodnie z monitami, aby utworzyć użytkownika. Nie musi on być taki sam, jak użytkownik w bazie danych SQLite utworzonej w pierwszej sekcji tego artykułu.

    ![Okno konsoli zarządzania Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoConsole.png)

1. Uruchom aplikację klawiszem `F5`. Ankieta utworzona za pomocą funkcji **Tworzenie przykładowej ankiety** oraz dane przesłane podczas głosowania zostaną zserializowane w bazie danych MySQL.

## Publikowanie aplikacji sieci Web w usłudze Azure App Service

Zestaw .NET SDK platformy Azure pozwala łatwo wdrożyć aplikację sieci Web w usłudze Azure App Service.

1. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy węzeł projektu i wybierz polecenie **Opublikuj**.

    ![Okno dialogowe Publikowanie w sieci Web](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)

1. Kliknij pozycję **Microsoft Azure Web Apps**.

1. Kliknij pozycję **Nowa**, aby utworzyć nową aplikację sieci Web.

1. Wypełnij poniższe pola i kliknij przycisk **Utwórz**:
    - **Nazwa aplikacji sieci Web**
    - **Plan usługi App Service**
    - **Grupa zasobów**
    - **Region**
    - W polu **Serwer bazy danych** pozostaw wartość **Brak bazy danych**

    <!-- ![Create Site on Microsoft Azure Dialog](./media/web-sites-python-ptvs-django-mysql/PollsCommonCreateWebSite.png) -->

1. Zaakceptuj wszystkie inne ustawienia domyślne i kliknij przycisk **Opublikuj**.

1. Opublikowana aplikacja sieci Web zostanie automatycznie otwarta w przeglądarce internetowej. Aplikacja sieci Web powinna działać zgodnie z oczekiwaniami i z wykorzystaniem bazy danych **MySQL** hostowanej na platformie Azure.

    ![Przeglądarka internetowa](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)

    Gratulacje! Twoja aplikacja sieci Web oparta na bazie danych MySQL została pomyślnie opublikowana na platformie Azure.

## Następne kroki

Aby dowiedzieć się więcej na temat narzędzi Python Tools for Visual Studio, Django i MySQL, kliknij poniższe łącza.

- [Dokumentacja narzędzi Python Tools for Visual Studio]
  - [Projekty sieci Web]
  - [Projekty usługi w chmurze]
  - [Debugowanie zdalne na platformie Microsoft Azure]
- [Dokumentacja platformy Django]
- [MySQL]

Więcej informacji możesz znaleźć w [Centrum deweloperów języka Python](/develop/python/).

<!--Link references-->

[Centrum deweloperów języka Python]: /develop/python/
[usług Azure Cloud Services]: ../cloud-services-python-ptvs.md

<!--External Link references-->

[Azure Portal]: https://portal.azure.com
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Narzędzia Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Zestaw przykładów VSIX dla narzędzi Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[32-bitowe środowisko Python w wersji 2.7]: http://go.microsoft.com/fwlink/?LinkId=517190 
[Dokumentacja narzędzi Python Tools for Visual Studio]: http://aka.ms/ptvsdocs
[Debugowanie zdalne na platformie Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Projekty sieci Web]: http://go.microsoft.com/fwlink/?LinkId=624027
[Projekty usługi w chmurze]: http://go.microsoft.com/fwlink/?LinkId=624028
[Dokumentacja platformy Django]: https://www.djangoproject.com/
[MySQL]: http://www.mysql.com/
[wideo]: http://youtu.be/oKCApIrS0Lo



<!--HONumber=Jun16_HO2-->


