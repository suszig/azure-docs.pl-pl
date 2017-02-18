---
title: "Obsługa platformy Django i bazy danych MySQL na platformie Azure przy użyciu narzędzi Python Tools 2.2 for Visual Studio"
description: "Dowiedz się, jak używać narzędzi Python Tools for Visual Studio do utworzenia aplikacji sieci Web Django przechowującej dane w wystąpieniu bazy danych MySQL, a następnie, jak ją wdrożyć w funkcji Web Apps w usłudze Azure App Service."
services: app-service\web
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
ms.assetid: c60a50b5-8b5e-4818-a442-16362273dabb
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: get-started-article
ms.date: 07/07/2016
ms.author: huvalo
translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 1c29cfc0a5b6361a7f526c37d5421ee4be3fe2c1


---
# <a name="django-and-mysql-on-azure-with-python-tools-22-for-visual-studio"></a>Obsługa platformy Django i bazy danych MySQL na platformie Azure przy użyciu narzędzi Python Tools 2.2 for Visual Studio
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

W ramach tego samouczka użyjesz narzędzi [Python Tools for Visual Studio](https://www.visualstudio.com/vs/python) w celu utworzenia prostej aplikacji sieci Web z ankietą, wykorzystując jeden z przykładowych szablonów PTVS. Dowiesz się, jak używać usługi MySQL hostowanej na platformie Azure, jak skonfigurować aplikację sieci Web pod kątem MySQL i jak opublikować tę aplikację w funkcji [Web Apps w usłudze Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

> [!NOTE]
> Informacje zawarte w tym samouczku są również dostępne w poniższym klipie wideo:
> 
> [PTVS 2.1: Aplikacja Django z obsługą MySQL][video]
> 
> 

Więcej artykułów o programowaniu aplikacji Web Apps w usłudze Azure App Service przy użyciu narzędzi PTVS, środowisk sieci Web Bottle, Flask i Django oraz usług baz danych Azure Table Storage, MySQL i SQL Database możesz znaleźć w [Centrum deweloperów języka Python]. Chociaż ten artykuł dotyczy usługi App Service, opisane kroki są podobne do programowania [usług Azure Cloud Services].

## <a name="prerequisites"></a>Wymagania wstępne
* Visual Studio 2015
* [32-bitowe środowisko Python w wersji 2.7] lub [32-bitowe środowisko Python w wersji 3.4]
* [Python Tools 2.2 for Visual Studio]
* [Zestaw przykładów VSIX dla narzędzi Python Tools 2.2 for Visual Studio]
* [Azure SDK Tools for VS 2015]
* Django 1.9 lub nowsze

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of the the previous include. -->

> [!NOTE]
> Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Wypróbuj usługę App Service](https://azure.microsoft.com/try/app-service/), w którym wyjaśniono, jak od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service. Karta kredytowa nie jest wymagana i nie musisz się do niczego zobowiązywać.
> 
> 

## <a name="create-the-project"></a>Tworzenie projektu
W tej sekcji utworzysz projekt programu Visual Studio przy użyciu przykładowego szablonu. Utworzysz środowisko wirtualne i zainstalujesz wymagane pakiety. Utworzysz także lokalną bazę danych przy użyciu systemu SQLite. Następnie uruchomisz aplikację lokalnie.

1. W programie Visual Studio wybierz pozycje **Plik**, **Nowy projekt**.
2. Szablony projektu z [Zestaw przykładów VSIX dla narzędzi Python Tools 2.2 for Visual Studio] są dostępne w menu **Python**, **Przykłady**. Wybierz pozycję **Polls Django Web Project** (Projekt sieci Web Django z ankietą) i kliknij przycisk OK, aby utworzyć projekt.
   
    ![Okno dialogowe Nowy projekt](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)
3. Zostanie wyświetlony monit o zainstalowanie pakietów zewnętrznych. Wybierz pozycję **Zainstaluj w środowisku wirtualnym**.
   
    ![Okno dialogowe Pakiety zewnętrzne](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)
4. Wybierz jako podstawowy interpreter **Python 2.7** lub **Python 3.4**.
   
    ![Okno dialogowe Dodawanie środowiska wirtualnego](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)
5. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy węzeł projektu i wybierz pozycję **Python**, a następnie wybierz pozycję **Migracja platformy Django**.  Następnie wybierz pozycję **Django — tworzenie administratora**.
6. Spowoduje to otwarcie konsoli zarządzania Django i utworzenie bazy danych SQLite w folderze projektu. Postępuj zgodnie z monitami, aby utworzyć użytkownika.
7. Sprawdź, czy aplikacja działa, naciskając `F5`.
8. Kliknij pozycję **Zaloguj** na górnym pasku nawigacyjnym.
   
    ![Pasek nawigacyjny Django](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)
9. Wprowadź poświadczenia użytkownika, który został utworzony podczas synchronizowania bazy danych.
   
    ![Formularz logowania](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)
10. Kliknij pozycję **Utwórz przykładową ankietę**.
    
     ![Tworzenie przykładowej ankiety](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)
11. Kliknij ankietę i zagłosuj.
    
     ![Głosowanie w przykładowej ankiecie](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-mysql-database"></a>Tworzenie bazy danych MySQL
Utworzysz bazę danych ClearDB MySQL hostowaną na platformie Azure.

Ewentualnie możesz utworzyć własną maszynę wirtualną działającą na platformie Azure, a następnie samodzielnie zainstalować środowisko MySQL i administrować nim.

Wykonując poniższe kroki, możesz utworzyć bazę danych z użyciem planu Free.

1. Zaloguj się do [Azure Portal].
2. W górnej części okienka nawigacji kliknij kolejno pozycje: **NOWE**, **Dane i przechowywanie**, **Baza danych MySQL**.
3. Skonfiguruj nową bazę danych MySQL, tworząc nową grupę zasobów i wybierając dla niej odpowiednią lokalizację.
4. Po utworzeniu bazy danych MySQL kliknij przycisk **Właściwości** w bloku bazy danych.
5. Użyj przycisku kopiowania, aby umieścić wartość elementu **PARAMETRY POŁĄCZENIA** w Schowku.

## <a name="configure-the-project"></a>Konfigurowanie projektu
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
2. W Eksploratorze rozwiązań w obszarze **Środowiska Python** kliknij prawym przyciskiem myszy środowisko wirtualne i wybierz polecenie **Zainstaluj pakiet języka Python**.
3. Zainstaluj pakiet `mysqlclient`, korzystając z polecenia **pip**.
   
    ![Okno dialogowe Instalowanie pakietu](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)
4. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy węzeł projektu i wybierz pozycję **Python**, a następnie wybierz pozycję **Migracja platformy Django**.  Następnie wybierz pozycję **Django — tworzenie administratora**.
   
    Spowoduje to utworzenie tabel w bazie danych MySQL utworzonej w poprzedniej sekcji. Postępuj zgodnie z monitami, aby utworzyć użytkownika. Nie musi on być taki sam, jak użytkownik w bazie danych SQLite utworzonej w pierwszej sekcji tego artykułu.
5. Uruchom aplikację klawiszem `F5`. Ankieta utworzona za pomocą funkcji **Tworzenie przykładowej ankiety** oraz dane przesłane podczas głosowania zostaną zserializowane w bazie danych MySQL.

## <a name="publish-the-web-app-to-azure-app-service"></a>Publikowanie aplikacji sieci Web w usłudze Azure App Service
Zestaw .NET SDK platformy Azure pozwala łatwo wdrożyć aplikację sieci Web w usłudze Azure App Service.

1. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy węzeł projektu i wybierz polecenie **Opublikuj**.
   
    ![Okno dialogowe Publikowanie w sieci Web](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)
2. Kliknij pozycję **Usługa aplikacji Microsoft Azure**.
3. Kliknij pozycję **Nowa**, aby utworzyć nową aplikację sieci Web.
4. Wypełnij poniższe pola i kliknij przycisk **Utwórz**:
   
   * **Nazwa aplikacji sieci Web**
   * **Plan usługi App Service**
   * **Grupa zasobów**
   * **Region**
   * W polu **Serwer bazy danych** pozostaw wartość **Brak bazy danych**
5. Zaakceptuj wszystkie inne ustawienia domyślne i kliknij przycisk **Opublikuj**.
6. Opublikowana aplikacja sieci Web zostanie automatycznie otwarta w przeglądarce internetowej. Aplikacja sieci Web powinna działać zgodnie z oczekiwaniami i z wykorzystaniem bazy danych **MySQL** hostowanej na platformie Azure.
   
    ![Przeglądarka internetowa](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)
   
    Gratulacje! Twoja aplikacja sieci Web oparta na bazie danych MySQL została pomyślnie opublikowana na platformie Azure.

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat narzędzi Python Tools for Visual Studio, Django i MySQL, kliknij poniższe łącza.

* [Dokumentacja narzędzi Python Tools for Visual Studio]
  * [Projekty sieci Web]
  * [Projekty usługi w chmurze]
  * [Debugowanie zdalne na platformie Microsoft Azure]
* [Dokumentacja platformy Django]
* [MySQL]

Więcej informacji możesz znaleźć w [Centrum deweloperów języka Python](/develop/python/).

<!--Link references-->

[Centrum deweloperów języka Python]: /develop/python/
[usług Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->

[Azure Portal]: https://portal.azure.com
[Python Tools for Visual Studio]: https://www.visualstudio.com/vs/python/
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Zestaw przykładów VSIX dla narzędzi Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[32-bitowe środowisko Python w wersji 2.7]: http://go.microsoft.com/fwlink/?LinkId=517190
[32-bitowe środowisko Python w wersji 3.4]: http://go.microsoft.com/fwlink/?LinkId=517191
[Dokumentacja narzędzi Python Tools for Visual Studio]: http://aka.ms/ptvsdocs
[Debugowanie zdalne na platformie Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Projekty sieci Web]: http://go.microsoft.com/fwlink/?LinkId=624027
[Projekty usługi w chmurze]: http://go.microsoft.com/fwlink/?LinkId=624028
[Dokumentacja platformy Django]: https://www.djangoproject.com/
[MySQL]: http://www.mysql.com/
[video]: http://youtu.be/oKCApIrS0Lo



<!--HONumber=Jan17_HO3-->


