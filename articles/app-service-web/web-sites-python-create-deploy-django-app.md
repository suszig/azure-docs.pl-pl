<properties
    pageTitle="Tworzenie aplikacji sieci Web przy użyciu platformy Django na platformie Azure"
    description="Samouczek przedstawiający uruchamianie aplikacji sieci Web w języku Python w aplikacjach Web Apps w usłudze Azure App Service."
    services="app-service\web"
    documentationCenter="python"
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article" 
    ms.date="02/19/2016"
    ms.author="huvalo"/>


# Tworzenie aplikacji sieci Web przy użyciu platformy Django na platformie Azure

Ten samouczek zawiera wprowadzenie do uruchamiania skryptów w języku Python w [aplikacjach Web Apps w usłudze Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Usługa Web Apps zapewnia ograniczony bezpłatny hosting i szybkie wdrażanie, a także możliwość korzystania z języka Python. Wraz z rozwojem aplikacji można przejść do płatnego hostingu i przeprowadzić integrację z innymi usługami Azure.

Aplikacja zostanie utworzona przy użyciu platformy sieci Web Django (zobacz alternatywne wersje tego samouczka dla platform [Flask](web-sites-python-create-deploy-flask-app.md) i [Bottle](web-sites-python-create-deploy-bottle-app.md)). Utworzysz aplikację sieci Web z portalu Azure Marketplace, skonfigurujesz wdrożenie systemu Git i sklonujesz repozytorium lokalnie. Następnie uruchomisz aplikację lokalnie, wprowadzisz zmiany, które zatwierdzisz i wypchniesz na platformę Azure. Samouczek pokazuje, jak można to zrobić w systemie Windows lub Mac/Linux.

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Wypróbuj usługę App Service](http://go.microsoft.com/fwlink/?LinkId=523751), w którym wyjaśniono, jak od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service. Bez kart kredytowych i bez zobowiązań.


## Wymagania wstępne

- System Windows, Mac lub Linux
- Środowisko Python w wersji 2.7 lub 3.4
- Narzędzia setuptools pip, virtualenv (tylko środowisko Python 2.7)
- Usługa Git
- [Python Tools for Visual Studio][] (PTVS) — uwaga: ten składnik jest opcjonalny.

**Uwaga**: publikowanie TFS nie jest obecnie obsługiwane dla projektów języka Python.

### System Windows

Jeśli środowisko Python 2.7 lub 3.4 (32-bitowe) nie zostało jeszcze zainstalowane, zalecane jest zainstalowanie [Zestaw Azure SDK dla języka Python w wersji 2.7] lub [zestawu Azure SDK dla języka Python 3.4] przy użyciu Instalatora platformy sieci Web. Spowoduje to zainstalowanie 32-bitowej wersji środowiska Python, narzędzi setuptools, pip i virtualenv itp. (32-bitowe środowisko Python jest instalowane na maszynach hostów Azure). Możesz również pobrać środowisko Python z witryny [python.org].

W przypadku systemu Git zalecane jest narzędzie [Git dla systemu Windows] lub [Github dla systemu Windows]. Jeśli używasz programu Visual Studio, możesz korzystać ze zintegrowanej obsługi systemu Git.

Zalecane jest również zainstalowanie narzędzi [Python Tools 2.2 for Visual Studio]. Jest to opcjonalne, ale jeśli masz program [Visual Studio], taki jak bezpłatny program Visual Studio Community 2013 lub Visual Studio Express 2013 for Web, możesz korzystać z doskonałego środowiska IDE języka Python.

### System Mac/Linux

Środowisko Python i system Git powinny być już zainstalowane, ale upewnij się, że masz środowisko Python 2.7 lub 3.4.


## Tworzenie aplikacji sieci Web w portalu

Pierwszym krokiem procedury tworzenia aplikacji jest utworzenie aplikacji sieci Web za pośrednictwem [Portalu Azure](https://portal.azure.com).

1. Zaloguj się do Portalu Azure i kliknij przycisk **NOWY** w lewym dolnym rogu.
3. W polu wyszukiwania wpisz „python”.
4. W wynikach wyszukiwania wybierz pozycję **Django**, a następnie kliknij pozycję **Utwórz**.
5. Skonfiguruj nową aplikację Django, taką jak tworzenie nowego planu usługi App Service i nowej grupy zasobów dla tego planu. Następnie kliknij pozycję **Utwórz**.
6. Skonfiguruj publikowanie w systemie Git dla nowo utworzonej aplikacji sieci Web zgodnie z poniższymi instrukcjami w artykule [Continuous deployment using GIT in Azure App Service](web-sites-publish-source-control.md) (Ciągłe wdrażanie przy użyciu systemu GIT w usłudze Azure App Service).

## Omówienie aplikacji

### Zawartość repozytorium Git

Poniżej przedstawiono pliki w początkowym repozytorium Git, które sklonujemy w następnej sekcji.

    \app\__init__.py
    \app\forms.py
    \app\models.py
    \app\tests.py
    \app\views.py
    \app\static\content\
    \app\static\fonts\
    \app\static\scripts\
    \app\templates\about.html
    \app\templates\contact.html
    \app\templates\index.html
    \app\templates\layout.html
    \app\templates\login.html
    \app\templates\loginpartial.html
    \DjangoWebProject\__init__.py
    \DjangoWebProject\settings.py
    \DjangoWebProject\urls.py
    \DjangoWebProject\wsgi.py

Główne źródła dla aplikacji. Składa się z 3 stron (indeks, informacje, kontakt) z układem głównym. Zawartość statyczna i skrypty obejmują bootstrap, jquery, modernizr i respond.

    \manage.py

Obsługa zarządzania lokalnego i serwera projektowego. Umożliwia to uruchamianie aplikacji lokalnie, synchronizowanie bazy danych itp.

    \db.sqlite3

Domyślna baza danych. Zawiera tabele niezbędne do uruchomienia aplikacji, ale nie zawiera żadnych użytkowników (należy zsynchronizować bazę danych w celu utworzenia użytkownika).

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

Pliki projektu do użycia z narzędziami [Python Tools for Visual Studio].

    \ptvs_virtualenv_proxy.py

Serwer proxy usług IIS dla środowisk wirtualnych i obsługa zdalnego debugowania narzędzi PTVS.

    \requirements.txt

Pakiety zewnętrzne wymagane przez tę aplikację. Skrypt wdrożenia będzie instalować przy użyciu narzędzia pip pakiety wymienione w tym pliku.

    \web.2.7.config
    \web.3.4.config

Pliki konfiguracji programu IIS. Skrypt wdrożenia użyje odpowiedniego pliku web.x.y.config i skopiuje go jako plik web.config.

### Pliki opcjonalne — dostosowywanie wdrożenia

[AZURE.INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### Pliki opcjonalne — środowisko uruchomieniowe języka Python

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### Dodatkowe pliki na serwerze

Niektóre pliki istnieją na serwerze, ale nie są dodawane do repozytorium git. Są one tworzone przez skrypt wdrożenia.

    \web.config

Plik konfiguracji programu IIS. Tworzony na podstawie pliku web.x.y.config przy każdym wdrożeniu.

    \env\

Środowisko wirtualne języka Python. Tworzone podczas wdrażania, jeśli nie istnieje jeszcze zgodne środowisko wirtualne dla aplikacji sieci Web. Pakiety wymienione w pliku requirements.txt są instalowane przy użyciu narzędzia pip, ale narzędzie pip pomija instalację, jeśli pakiety zostały już zainstalowane.

W 3 następnych sekcjach opisano procedury wdrażania aplikacji sieci Web w 3 różnych środowiskach:

- System Windows — przy użyciu narzędzi Python Tools for Visual Studio
- System Windows — przy użyciu wiersza polecenia
- System Mac/Linux — przy użyciu wiersza polecenia


## Wdrażanie aplikacji sieci Web — system Windows — narzędzia Python Tools for Visual Studio

### Klonowanie repozytorium

Najpierw sklonuj repozytorium przy użyciu adresu URL podanego w Portalu Azure. Aby uzyskać więcej informacji, zobacz [Continuous deployment using GIT in Azure App Service](web-sites-publish-source-control.md) (Ciagłe wdrażanie przy użyciu systemu GIT w usłudze Azure App Service).

Otwórz plik rozwiązania (SLN), który znajduje się w folderze głównym repozytorium.

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### Tworzenie środowiska wirtualnego

Teraz utwórz środowisko wirtualne dla wdrożenia lokalnego. Kliknij prawym przyciskiem myszy pozycję **Środowiska Python** i wybierz pozycję **Dodaj środowisko wirtualne**.

- Upewnij się, że nazwa środowiska to `env`.

- Wybierz interpreter podstawowy. Upewnij się, że jest używana ta sama wersja środowiska Python, którą wybrano dla aplikacji sieci Web (w pliku runtime.txt lub bloku **Ustawienia aplikacji** dla aplikacji sieci Web w Portalu Azure).

- Upewnij się, że opcja pobierania i instalowania pakietów została zaznaczona.

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

Kliknij pozycję **Utwórz**. Spowoduje to utworzenie środowiska wirtualnego i zainstalowanie składników zależnych wymienionych w pliku requirements.txt.

### Tworzenie administratora

Baza danych dołączona do aplikacji nie zawiera definicji administratora. Aby można było używać funkcji logowania w aplikacji lub interfejsu administracyjnego Django (jeśli zostanie włączony przez użytkownika), musisz utworzyć administratora.

Uruchom poniższe polecenie z poziomu wiersza polecenia w folderze projektu:

    env\scripts\python manage.py createsuperuser

Postępuj zgodnie z monitami, aby ustawić nazwę użytkownika, hasło itp.

### Uruchamianie przy użyciu serwera projektowego

Naciśnij klawisz F5, aby rozpocząć debugowanie. Spowoduje to automatyczne otwarcie przeglądarki sieci Web i wyświetlenie strony uruchomionej lokalnie.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

Można ustawiać punkty przerwania w źródłach, używać okien wyrażeń kontrolnych itp. Zapoznaj się z [Dokumentacja narzędzi Python Tools for Visual Studio], aby uzyskać więcej informacji na temat różnych funkcji.

### Wprowadzanie zmian

Teraz możesz eksperymentować, wprowadzając zmiany w źródłach i/lub szablonach aplikacji.

Po przetestowaniu wprowadzonych zmian zatwierdź je w repozytorium Git:

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### Instalowanie dodatkowych pakietów

Aplikacja może mieć zależności poza środowiskiem Python i platformą Django.

Możesz zainstalować dodatkowe pakiety przy użyciu narzędzia pip. Aby zainstalować pakiet, kliknij prawym przyciskiem myszy środowisko wirtualne i wybierz pozycję **Zainstaluj pakiet języka Python**.

Aby na przykład zainstalować zestaw Azure SDK dla języka Python zapewniający dostęp do magazynu Azure, magistrali usług i innych usług Azure, wprowadź `azure`:

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

Kliknij prawym przyciskiem myszy środowisko wirtualne i wybierz pozycję **Generuj plik requirements.txt**, aby zaktualizować plik requirements.txt.

Następnie zatwierdź zmiany w pliku requirements.txt w repozytorium Git.

### Wdrażanie na platformie Azure

Aby wyzwolić wdrożenie, kliknij pozycję **Synchronizuj** lub **Wypchnij**. Synchronizacja obejmuje zarówno wypychanie, jak i ściąganie.

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

Pierwsze wdrożenie trwa dłużej, ponieważ obejmuje tworzenie środowiska wirtualnego, instalowanie pakietów itp.

Program Visual Studio nie wyświetla postępu wdrożenia. Jeśli chcesz przejrzeć dane wyjściowe, zobacz sekcję [Rozwiązywanie problemów — wdrożenie](#troubleshooting-deployment).

Przejdź do adresu URL platformy Azure, aby przejrzeć wprowadzone zmiany.


## Wdrażanie aplikacji sieci Web — system Windows — wiersz polecenia

### Klonowanie repozytorium

Najpierw sklonuj repozytorium przy użyciu adresu URL podanego w Portalu Azure i dodaj repozytorium Azure jako repozytorium zdalne. Aby uzyskać więcej informacji, zobacz [Continuous deployment using GIT in Azure App Service](web-sites-publish-source-control.md) (Ciagłe wdrażanie przy użyciu systemu GIT w usłudze Azure App Service).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### Tworzenie środowiska wirtualnego

Zostanie utworzone nowe środowisko wirtualne dla celów wdrożenia (nie należy dodawać go do repozytorium). Nie można zmieniać lokalizacji wirtualnych środowisk języka Python, dlatego każdy deweloper pracujący nad aplikacją tworzy własne środowisko lokalnie.

Upewnij się, że jest używana ta sama wersja środowiska Python, którą wybrano dla aplikacji sieci Web (w pliku runtime.txt lub bloku Ustawienia aplikacji dla aplikacji sieci Web w Portalu Azure).

Dla środowiska Python w wersji 2.7:

    c:\python27\python.exe -m virtualenv env

Dla środowiska Python w wersji 3.4:

    c:\python34\python.exe -m venv env

Zainstaluj pakiety zewnętrzne wymagane przez aplikację. Korzystając z pliku requirements.txt w folderze głównym repozytorium, możesz instalować pakiety w środowisku wirtualnym:

    env\scripts\pip install -r requirements.txt

### Tworzenie administratora

Baza danych dołączona do aplikacji nie zawiera definicji administratora. Aby można było używać funkcji logowania w aplikacji lub interfejsu administracyjnego Django (jeśli zostanie włączony przez użytkownika), musisz utworzyć administratora.

Uruchom poniższe polecenie z poziomu wiersza polecenia w folderze projektu:

    env\scripts\python manage.py createsuperuser

Postępuj zgodnie z monitami, aby ustawić nazwę użytkownika, hasło itp.

### Uruchamianie przy użyciu serwera projektowego

Możesz uruchomić aplikację na serwerze projektowym przy użyciu następującego polecenia:

    env\scripts\python manage.py runserver

Na konsoli zostanie wyświetlony adres URL i port nasłuchiwany przez serwer:

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

Następnie otwórz w przeglądarce sieci Web stronę wskazywaną przez ten adres URL.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### Wprowadzanie zmian

Teraz możesz eksperymentować, wprowadzając zmiany w źródłach i/lub szablonach aplikacji.

Po przetestowaniu wprowadzonych zmian zatwierdź je w repozytorium Git:

    git add <modified-file>
    git commit -m "<commit-comment>"

### Instalowanie dodatkowych pakietów

Aplikacja może mieć zależności poza środowiskiem Python i platformą Django.

Możesz zainstalować dodatkowe pakiety przy użyciu narzędzia pip. Aby na przykład zainstalować zestaw Azure SDK dla języka Python zapewniający dostęp do magazynu Azure, magistrali usług i innych usług Azure, wpisz:

    env\scripts\pip install azure

Pamiętaj o zaktualizowaniu pliku requirements.txt:

    env\scripts\pip freeze > requirements.txt

Zatwierdź zmiany:

    git add requirements.txt
    git commit -m "Added azure package"

### Wdrażanie na platformie Azure

Aby wyzwolić wdrożenie, wypchnij zmiany na platformę Azure:

    git push azure master

Zostaną wyświetlone dane wyjściowe skryptu wdrożenia, łącznie z tworzeniem środowiska wirtualnego, instalowaniem pakietów i tworzeniem pliku web.config.

Przejdź do adresu URL platformy Azure, aby przejrzeć wprowadzone zmiany.


## Wdrażanie aplikacji sieci Web — system Mac/Linux — wiersz polecenia

### Klonowanie repozytorium

Najpierw sklonuj repozytorium przy użyciu adresu URL podanego w Portalu Azure i dodaj repozytorium Azure jako repozytorium zdalne. Aby uzyskać więcej informacji, zobacz [Continuous deployment using GIT in Azure App Service](web-sites-publish-source-control.md) (Ciagłe wdrażanie przy użyciu systemu GIT w usłudze Azure App Service).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### Tworzenie środowiska wirtualnego

Zostanie utworzone nowe środowisko wirtualne dla celów wdrożenia (nie należy dodawać go do repozytorium). Nie można zmieniać lokalizacji wirtualnych środowisk języka Python, dlatego każdy deweloper pracujący nad aplikacją tworzy własne środowisko lokalnie.

Upewnij się, że jest używana ta sama wersja środowiska Python, którą wybrano dla aplikacji sieci Web (w pliku runtime.txt lub bloku Ustawienia aplikacji dla aplikacji sieci Web w Portalu Azure).

Dla środowiska Python w wersji 2.7:

    python -m virtualenv env

Dla środowiska Python w wersji 3.4:

    python -m venv env

lub

    pyvenv env

Zainstaluj pakiety zewnętrzne wymagane przez aplikację. Korzystając z pliku requirements.txt w folderze głównym repozytorium, możesz instalować pakiety w środowisku wirtualnym:

    env/bin/pip install -r requirements.txt

### Tworzenie administratora

Baza danych dołączona do aplikacji nie zawiera definicji administratora. Aby można było używać funkcji logowania w aplikacji lub interfejsu administracyjnego Django (jeśli zostanie włączony przez użytkownika), musisz utworzyć administratora.

Uruchom poniższe polecenie z poziomu wiersza polecenia w folderze projektu:

    env/bin/python manage.py createsuperuser

Postępuj zgodnie z monitami, aby ustawić nazwę użytkownika, hasło itp.

### Uruchamianie przy użyciu serwera projektowego

Możesz uruchomić aplikację na serwerze projektowym przy użyciu następującego polecenia:

    env/bin/python manage.py runserver

Na konsoli zostanie wyświetlony adres URL i port nasłuchiwany przez serwer:

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

Następnie otwórz w przeglądarce sieci Web stronę wskazywaną przez ten adres URL.

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### Wprowadzanie zmian

Teraz możesz eksperymentować, wprowadzając zmiany w źródłach i/lub szablonach aplikacji.

Po przetestowaniu wprowadzonych zmian zatwierdź je w repozytorium Git:

    git add <modified-file>
    git commit -m "<commit-comment>"

### Instalowanie dodatkowych pakietów

Aplikacja może mieć zależności poza środowiskiem Python i platformą Django.

Możesz zainstalować dodatkowe pakiety przy użyciu narzędzia pip. Aby na przykład zainstalować zestaw Azure SDK dla języka Python zapewniający dostęp do magazynu Azure, magistrali usług i innych usług Azure, wpisz:

    env/bin/pip install azure

Pamiętaj o zaktualizowaniu pliku requirements.txt:

    env/bin/pip freeze > requirements.txt

Zatwierdź zmiany:

    git add requirements.txt
    git commit -m "Added azure package"

### Wdrażanie na platformie Azure

Aby wyzwolić wdrożenie, wypchnij zmiany na platformę Azure:

    git push azure master

Zostaną wyświetlone dane wyjściowe skryptu wdrożenia, łącznie z tworzeniem środowiska wirtualnego, instalowaniem pakietów i tworzeniem pliku web.config.

Przejdź do adresu URL platformy Azure, aby przejrzeć wprowadzone zmiany.


## Rozwiązywanie problemów — instalowanie pakietów

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## Rozwiązywanie problemów — środowisko wirtualne

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]


## Rozwiązywanie problemów — pliki statyczne

Platforma Django korzysta z koncepcji zbierania plików statycznych. Polega to na skopiowaniu wszystkich plików statycznych z oryginalnej lokalizacji do pojedynczego folderu. W przypadku tej aplikacji są one kopiowane do folderu `/static`.

Ta operacja jest wykonywana, ponieważ pliki statyczne mogą pochodzić z różnych „aplikacji” Django. Na przykład pliki statyczne z interfejsów administracyjnych Django znajdują się w podfolderze biblioteki Django w środowisku wirtualnym. Pliki statyczne zdefiniowane przez tę aplikację znajdują się w folderze `/app/static`. W przypadku użycia dodatkowych „aplikacji” Django pliki statyczne będą znajdować się w kilku miejscach.

Aplikacja uruchomiona w trybie debugowania udostępnia pliki statyczne z oryginalnej lokalizacji.

Aplikacja uruchomiona w trybie przygotowania do wydania **nie** udostępnia plików statycznych. Serwer sieci Web jest zobowiązany do udostępniania plików. W przypadku tej aplikacji usługa IIS udostępnia pliki statyczne z folderu `/static`.

Pliki statyczne są zbierane automatycznie w ramach skryptu wdrożenia, a uprzednio zebrane pliki są czyszczone. Oznacza to, że pliki są zbierane przy każdym wdrożeniu, co powoduje nieznaczne spowolnienie wdrażania, ale gwarantuje, że nieaktualne pliki będą niedostępne i nie wystąpi potencjalny problem z zabezpieczeniami.

Jeśli chcesz pominąć zbieranie plików statycznych dla aplikacji Django:

    \.skipDjango

Musisz zebrać pliki ręcznie na komputerze lokalnym:

    env\scripts\python manage.py collectstatic

Następnie usuń folder `\static` z pliku `.gitignore` i dodaj go do repozytorium Git.


## Rozwiązywanie problemów — ustawienia

Różne ustawienia dla aplikacji można zmienić w pliku `DjangoWebProject/settings.py`.

Dla wygody deweloperów tryb debugowania jest włączony. Jedną z zalet tego rozwiązania jest możliwość wyświetlania obrazów i innej zawartości statycznej podczas uruchamiania lokalnego bez konieczności zbierania plików statycznych.

Aby wyłączyć tryb debugowania:

    DEBUG = False

Po wyłączeniu debugowania konieczne jest zaktualizowanie wartości `ALLOWED_HOSTS` w celu uwzględnienia nazwy hosta Azure. Na przykład:

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

lub zezwolenie na dowolne hosty:

    ALLOWED_HOSTS = (
        '*',
    )

W praktyce może być konieczne wykonanie bardziej złożonych czynności w celu uwzględnienia przełączania trybów debugowania i przygotowania do wydania oraz pobrania nazwy hosta.

Można ustawić zmienne środowiskowe za pośrednictwem strony **KONFIGURACJA** Portalu Azure w sekcji **Ustawienia aplikacji**.  Może to być przydatne do ustawiania wartości, które nie powinny pojawiać się w źródłach (parametry połączenia, hasła itp.) lub powinny zostać ustawione inaczej dla platformy Azure i komputera lokalnego. W pliku `settings.py` możesz wykonać zapytanie dotyczące zmiennych środowiskowych przy użyciu pliku `os.getenv`.


## Korzystanie z bazy danych

Bazy danych dołączona do aplikacji jest bazą danych SQLite. Jest to wygodna i przydatna domyślna baza danych, której można użyć dla wdrożenia, ponieważ prawie nie wymaga konfiguracji. Baza danych jest przechowywana w pliku db.sqlite3 w folderze projektu.

Platforma Azure oferuje usługi bazy danych, które są łatwe w użyciu z poziomu aplikacji Django. Samouczki dotyczące użycia [SQL Database] i [MySQL] z poziomu aplikacji Django przedstawiają kroki niezbędne do utworzenia usługi bazy danych, zmiany ustawień bazy danych w pliku `DjangoWebProject/settings.py` i identyfikacji bibliotek wymaganych do instalacji.

Oczywiście jeśli wolisz zarządzać własnymi serwerami bazy danych, możesz to zrobić przy użyciu maszyn wirtualnych systemu Windows lub Linux działających na platformie Azure.


## Interfejs administracyjny Django

Po rozpoczęciu tworzenia modeli może być konieczne wypełnienie bazy danych określonymi danymi. Prostą metodą interakcyjnego dodawania i edytowania zawartości jest użycie interfejsu administracyjnego Django.

Kod dla interfejsu administracyjnego został umieszczony w komentarzach w źródłach aplikacji, ale jest jednoznacznie oznaczony, dlatego możesz go łatwo uaktywnić (wyszukaj „admin”).

Po uaktywnieniu tego kodu zsynchronizuj bazę danych, uruchom aplikację i przejdź do folderu `/admin`.


## Następne kroki

Aby dowiedzieć się więcej na temat struktury Django i narzędzi Python Tools for Visual Studio:

- [Dokumentacja platformy Django]
- [Dokumentacja narzędzi Python Tools for Visual Studio]

Aby uzyskać informacje na temat baz danych SQL Database i MySQL:

- [Obsługa platformy Django i bazy danych MySQL na platformie Azure przy użyciu narzędzi Python Tools for Visual Studio]
- [Obsługa platformy Django i bazy danych SQL Database na platformie Azure przy użyciu narzędzi Python Tools for Visual Studio]

Aby uzyskać więcej informacji, zobacz [Centrum deweloperów Python](/develop/python/).


## Co zostało zmienione
* Przewodnik dotyczący przejścia od usługi Witryny sieci Web do usługi App Service można znaleźć w temacie [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714) (Usługa Azure App Service i jej wpływ na istniejące usługi platformy Azure).


<!--Link references-->
[Obsługa platformy Django i bazy danych MySQL na platformie Azure przy użyciu narzędzi Python Tools for Visual Studio]: web-sites-python-ptvs-django-mysql.md
[Obsługa platformy Django i bazy danych SQL Database na platformie Azure przy użyciu narzędzi Python Tools for Visual Studio]: web-sites-python-ptvs-django-sql.md
[SQL Database]: web-sites-python-ptvs-django-sql.md
[Baza danych MySQL]: web-sites-python-ptvs-django-mysql.md

<!--External Link references-->
[Zestaw Azure SDK dla języka Python w wersji 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Zestaw Azure SDK dla języka Python w wersji 3.4:]: http://go.microsoft.com/fwlink/?linkid=516990
[python.org]: http://www.python.org/
[Git dla systemu Windows]: http://msysgit.github.io/
[Usługa GitHub dla systemu Windows]: https://windows.github.com/
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Dokumentacja narzędzi Python Tools for Visual Studio]: http://aka.ms/ptvsdocs
[Dokumentacja platformy Django]: https://www.djangoproject.com/



<!--HONumber=Jun16_HO2-->


