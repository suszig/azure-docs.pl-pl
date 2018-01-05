---
title: "Jak pracować z serwera wewnętrznej bazy danych Node.js SDK dla aplikacji mobilnych | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak pracować z serwera wewnętrznej bazy danych Node.js SDK dla usługi Azure App Service Mobile Apps."
services: app-service\mobile
documentationcenter: 
author: elamalani
manager: elamalani
editor: 
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 336da28bea7de313bced97e447fc6b7b1fb1390d
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/04/2018
---
# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Jak używać zestawu SDK usługi Azure Mobile Apps Node.js
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Ten artykuł zawiera szczegółowe informacje i przykłady przedstawiający sposób pracy z zaplecza Node.js w usłudze Azure App Service Mobile Apps.

## <a name="Introduction"></a>Wprowadzenie
Usługa Azure App Service Mobile Apps oferuje możliwość dodawania danych zoptymalizowanych pod kątem mobile dostępu do interfejsu API sieci Web do aplikacji sieci web.  Zestaw SDK aplikacji mobilnych usługi aplikacji Azure jest dostępna dla aplikacji sieci web ASP.NET i Node.js.  Zestaw SDK udostępnia następujące operacje:

* Operacje tabeli (Odczyt, Insert, Update, Delete) dla dostępu do danych
* Operacje niestandardowego interfejsu API

Obie operacje zapewnienia uwierzytelniania przez wszystkich dostawców tożsamości dozwolone w usłudze Azure App Service, w tym dostawców tożsamości społecznościowych, takich jak Facebook, Twitter, Google i Microsoft, jak również usługi Azure Active Directory dla tożsamości organizacji.

Przykłady można znaleźć do każdego przypadku użycia w [katalogu przykładów w witrynie GitHub].

## <a name="supported-platforms"></a>Obsługiwane platformy
Zestaw SDK usługi Azure Mobile Apps węzła obsługuje bieżącej wersji LTS węzła i nowszych.  Począwszy od zapisu, najnowsza wersja LTS jest v4.5.0 węzła.  Inne wersje węzeł może działać, ale nie są obsługiwane.

Zestaw SDK usługi Azure Mobile Apps węzła obsługuje dwa sterowniki bazy danych — sterownik mssql węzła obsługuje SQL Azure i lokalnego wystąpienia programu SQL Server.  Sterownik sqlite3 obsługuje tylko jedno wystąpienie bazy danych SQLite.

### <a name="howto-cmdline-basicapp"></a>Porady: tworzenie zaplecza Node.js podstawowe przy użyciu wiersza polecenia
Jako aplikacja ExpressJS rozpoczyna się co wewnętrznej bazy danych usługi Azure App Service Mobile aplikacji Node.js.  ExpressJS jest dostępna dla środowiska Node.js struktury usługi najpopularniejszych sieci web.  Można utworzyć basic [Express] aplikacji w następujący sposób:

1. Polecenie lub oknie programu PowerShell należy utworzyć katalog w projekcie.

        mkdir basicapp
2. Uruchom npm inicjowania zainicjować struktury pakietu.

        cd basicapp
        npm init

    Polecenie init npm zapyta zestaw pytań można zainicjować projektu.  Zobacz przykład danych wyjściowych:

    ![Dane wyjściowe init npm][0]
3. Zainstaluj biblioteki express i aplikacje w przypadku mobilne platformy azure z repozytorium npm.

        npm install --save express azure-mobile-apps
4. Tworzenie pliku app.js do zaimplementowania podstawowego serwera przenośnych.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Ta aplikacja tworzy zoptymalizowanych pod kątem mobile WebAPI z jednym punktem końcowym (`/tables/TodoItem`) który nieuwierzytelnionym udostępnia odpowiedni magazyn danych SQL przy użyciu dynamicznych schematu.  Jest ona odpowiednia dla następującego Szybki Start biblioteki klienta:

* [Szybki Start kliencką dla systemu android]
* [Apache Cordova klienta — Szybki Start]
* [iOS — Szybki Start klienta]
* [Szybki Start klienta Sklepu Windows]
* [Szybki Start klienta platformy Xamarin.iOS]
* [Szybki Start klienta platformy Xamarin.Android]
* [Szybki Start klienta platformy Xamarin.Forms]

Możesz znaleźć kod dla tej aplikacji w warstwie podstawowa w [basicapp przykładem w witrynie GitHub].

### <a name="howto-vs2015-basicapp"></a>Porady: tworzenie zaplecza węzła z programem Visual Studio 2015
Visual Studio 2015 wymaga rozszerzenia do tworzenia aplikacji Node.js w środowisku IDE.  Aby rozpocząć, zainstaluj [Node.js Tools 1.1 dla programu Visual Studio].  Po zainstalowaniu narzędzia Node.js dla programu Visual Studio tworzy aplikację 4.x Express:

1. Otwórz **nowy projekt** okna dialogowego (z **pliku** > **nowy** > **projektu...** ).
2. Rozwiń węzeł **szablony** > **JavaScript** > **Node.js**.
3. Wybierz **aplikacji podstawowe Azure Node.js Express 4**.
4. Wprowadź nazwę projektu.  Kliknij przycisk *OK*.

    ![Nowy projekt programu Visual Studio 2015][1]
5. Kliknij prawym przyciskiem myszy **npm** a następnie wybierz węzeł **zainstalować nowych pakietów npm...** .
6. Należy odświeżyć katalog npm na tworzenie pierwszej aplikacji Node.js.  Kliknij przycisk **Odśwież** w razie potrzeby.
7. Wprowadź *apps w usłudze azure-mobile* w polu wyszukiwania.  Kliknij przycisk **usługi azure mobile apps 2.0.0** pakietu, a następnie kliknij przycisk **zainstaluj pakiet**.

    ![Zainstaluj nowych pakietów npm][2]
8. Kliknij przycisk **Zamknij**.
9. Otwórz *app.js* plik, aby dodać obsługę zestaw SDK usługi Azure Mobile Apps.  W wierszu 6 at dolnej części biblioteki wymagają instrukcje, Dodaj następujący kod:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    W wierszu około 27 po innych instrukcji app.use Dodaj następujący kod:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Zapisz plik.
10. Uruchom aplikację lokalnie (interfejs API jest obsługiwana na http://localhost: 3000) lub publikowanie na platformie Azure.

### <a name="create-node-backend-portal"></a>Porady: tworzenie zaplecza Node.js przy użyciu portalu Azure
Można tworzyć w prawej zaplecza aplikacji mobilnej [portalu Azure]. Można wykonać następujące czynności lub utworzyć klienta i serwera razem [tworzenie aplikacji mobilnej](app-service-mobile-ios-get-started.md) samouczka. Samouczek zawiera uproszczonej wersji tych instrukcji i najlepiej stosować do weryfikacji koncepcji projektów.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

W *wprowadzenie* bloku, w obszarze **Utwórz tabelę interfejsu API**, wybierz **Node.js** jako sieci **język wewnętrznej bazy danych**.
Pole wyboru dla "**potwierdzam, że spowoduje to zastąpienie wszystkich lokacji zawartości.**", następnie kliknij przycisk **Utwórz tabelę TodoItem**.

### <a name="download-quickstart"></a>Porady: pobieranie projekt kodu szybkiego startu zaplecza Node.js przy użyciu narzędzia Git
Po utworzeniu zaplecza aplikacji mobilnej Node.js przy użyciu portalu **szybki start** bloku projektu Node.js jest utworzony i wdrożone do tej witryny. Można dodać tabele i interfejsów API i edytować pliki kodu dla zaplecza Node.js w portalu. Aby pobrać projektu wewnętrznej bazy danych, dzięki czemu można dodać lub zmodyfikować tabele i interfejsów API, a następnie ponownie opublikować projekt umożliwia także różne narzędzia wdrażania. Aby uzyskać więcej informacji, zobacz [Azure App Service Deployment Guide]. Poniższa procedura wykorzystuje repozytorium Git do pobierania kodu projektu Szybki Start.

1. Zainstaluj usługę Git, jeśli jeszcze tego nie zrobiono. Kroki wymagane do zainstalowania Git się różnić między systemami operacyjnymi. Zobacz [instalowanie Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) dystrybucje systemu operacyjnego i instalacji.
2. Postępuj zgodnie z instrukcjami [włączyć repozytorium aplikacji usługi App Service](../app-service/app-service-deploy-local-git.md#Step3) umożliwiające repozytorium Git dla witryny sieci wewnętrznej bazy danych, co Uwaga wdrożenia nazwy użytkownika i hasła.
3. W bloku dla zaplecza aplikacji mobilnej, zanotuj **adres URL klonowania Git** ustawienie.
4. Wykonanie `git clone` polecenia przy użyciu narzędzia Git clone adres URL, wprowadzania hasła, gdy jest to wymagane, jak w poniższym przykładzie:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git
5. Przejdź do katalogu lokalnego, co w poprzednim przykładzie jest /todolist i zwróć uwagę, że pliki projektu zostały pobrane. Zlokalizuj `todoitem.json` w pliku `/tables` katalogu.  Ten plik definiuje uprawnienia w tabeli.  Jest także `todoitem.js` pliku w tym samym katalogu, który definiuje tej operacji CRUD skrypty dla tabeli.
6. Po wprowadzeniu zmian w plikach projektu, wykonaj następujące polecenia, aby dodać, zatwierdzić, a następnie przekazać zmiany do witryny:

        $ git commit -m "updated the table script"
        $ git push origin master

    Po dodaniu nowych plików do projektu, należy najpierw wykonać `git add .` polecenia.

Opublikowaniu lokacji za każdym razem, gdy nowy zestaw zatwierdzeń zostanie przypisany do lokacji.

### <a name="howto-publish-to-azure"></a>Porady: publikowanie zaplecza Node.js na platformie Azure
Microsoft Azure udostępnia wiele mechanizmów publikowania z wewnętrzną bazą danych Azure App Service Mobile aplikacji Node.js w usłudze platformy Azure.  Obejmują one przy użyciu narzędzia wdrażania zintegrowane w programie Visual Studio, narzędzia wiersza polecenia i opcje ciągłego wdrażania w oparciu o kontroli źródła.  Aby uzyskać więcej informacji na ten temat, zobacz [Azure App Service Deployment Guide].

Usługa aplikacji Azure ma poradę należy zapoznać się przed przystąpieniem do wdrażania aplikacji Node.js:

* Jak [Określ wersję węzła]
* Jak [Użyj modułów węzła]

### <a name="howto-enable-homepage"></a>Porady: Włączanie strony głównej aplikacji
Wiele aplikacji są kombinacją sieci web i aplikacji mobilnych i ExpressJS framework pozwala na połączenie dwóch zestawów reguł.  Czasami jednak warto tylko zaimplementować interfejs przenośnych.  Należy zapewnić strony docelowej, aby upewnić się, usługi app service jest uruchomiona.  Można podać strony głównej lub włączyć tymczasowe strony głównej.  Aby włączyć tymczasowe strony głównej, należy użyć następującego można utworzyć wystąpienia usługi Azure Mobile Apps:

    var mobile = azureMobileApps({ homePage: true });

Jeśli mają tylko tej opcji, które są dostępne podczas tworzenia lokalnie, możesz dodać to ustawienie, aby Twoje `azureMobile.js` pliku.

## <a name="TableOperations"></a>Operacje tabeli
Aplikacje azure-mobile Node.js Server SDK zawiera mechanizmy do udostępnienia tabel danych przechowywanych w bazie danych SQL Azure jako WebAPI.  Podano pięć operacji.

| Operacja | Opis |
| --- | --- |
| GET /tables/*tablename* |Pobierz wszystkie rekordy w tabeli |
| GET /tables/*tablename*/:id |Pobierz określonego rekordu w tabeli |
| POST /tables/*tablename* |Utwórz rekord w tabeli |
| POPRAWKA /tables/*tablename*/:id |Aktualizacja rekordu w tabeli |
| Usuń /tables/*tablename*/:id |Usuwanie rekordu w tabeli |

Obsługuje ten WebAPI [OData] i rozszerza schemat tabeli w celu obsługi [synchronizacji danych w trybie offline].

### <a name="howto-dynamicschema"></a>Porady: Definiowanie tabel za pomocą dynamicznej schematu
Przed użyciem tabeli musi być zdefiniowany.  Tabele mogą być definiowane ze schematem statyczne (gdzie dewelopera definiuje kolumn w schemacie) lub dynamicznie (gdzie zestawu SDK Określa schemat oparte na przychodzące żądania). Ponadto deweloper może kontrolować określone aspekty WebAPI przez dodanie kodu Javascript do definicji.

Najlepszym rozwiązaniem można zdefiniować każdej tabeli w pliku Javascript w katalogu tabel, następnie użyj metody tables.import() Aby zaimportować tabele.  Rozszerzanie basic-app, będzie można dostosować pliku app.js:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Zdefiniuj tabelę w. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Tabele domyślnie używają schematu dynamicznych.  Aby wyłączyć funkcję dynamicznego schematu globalnie, określ dla ustawienia aplikacji **MS_DynamicSchema** o wartości false w portalu Azure.

Znajduje się pełny przykład w [todo przykładem w witrynie GitHub].

### <a name="howto-staticschema"></a>Porady: Definiowanie tabel za pomocą statycznego schematu
Można jawnie definiować kolumn do ujawnienia za pomocą WebAPI.  Zestaw Node.js SDK apps w usłudze azure-mobile automatycznie dodaje wszystkie dodatkowe kolumny są wymagane dla synchronizacji danych w trybie offline do listy, który podasz.  Na przykład wymagać tabeli z kolumnami przez aplikacje klienckie Szybki Start: tekst (ciąg) i ukończyć (wartość logiczna).  
Tabeli można zdefiniować w tabeli JavaScript pliku definicji (znajduje się w katalogu tabele) w następujący sposób:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Po zdefiniowaniu statycznie tabel, należy także wywołać metodę tables.initialize() utworzyć schemat bazy danych podczas uruchamiania.  Zwraca metodę tables.initialize() [Promise] tak, aby usługi sieci web nie obsługiwać żądań przed bazy danych został zainicjowany.

### <a name="howto-sqlexpress-setup"></a>Porady: użycie programu SQL Express jako programowanie magazynu danych na komputerze lokalnym
Azure Mobile Apps AzureMobile aplikacji węzła SDK udostępnia trzy opcje do obsługi danych bez: zestaw SDK zawiera trzy opcje do obsługi danych poza pola:

* Użyj **pamięci** sterownika do zapewnienia magazynu przykład nietrwałe
* Użyj **mssql** sterownik do zapewnienia magazynu danych programu SQL Express programowanie
* Użyj **mssql** sterowników w celu zapewnienia magazynu danych bazy danych SQL Azure dla trybu produkcyjnego

Zestaw SDK usługi Azure Mobile Apps Node.js używa [pakietu Node.js mssql] i użycia połączenia do bazy danych SQL i programu SQL Express.  Ten pakiet wymaga włączenia połączeń TCP w wystąpieniu programu SQL Express.

> [!TIP]
> Sterownik pamięci nie ma kompletnego zestawu urządzenia do testowania.  Jeśli chcesz przetestować zaplecza lokalnie, firma Microsoft zaleca korzystanie z magazynu danych programu SQL Express i sterownik mssql.
>
>

1. Pobierz i zainstaluj [programu Microsoft SQL Server 2014 Express].  Upewnij się, że instalowanie programu SQL Server 2014 Express Edition narzędzia.  Chyba że jawnie wymagana jest Obsługa 64-bitowy, 32-bitowej wersji zużywa mniej pamięci podczas uruchamiania.
2. Uruchom Menedżera konfiguracji programu SQL Server 2014.

   1. Rozwiń węzeł **konfigurację sieci programu SQL Server** węzła w menu po lewej stronie drzewa.
   2. Kliknij przycisk **protokoły dla programu SQLEXPRESS**.
   3. Kliknij prawym przyciskiem myszy **TCP/IP** i wybierz **włączyć**.  Kliknij przycisk **OK** w wyskakującym oknie dialogowym.
   4. Kliknij prawym przyciskiem myszy **TCP/IP** i wybierz **właściwości**.
   5. Kliknij przycisk **adresów IP** kartę.
   6. Znajdź **IPWszystkie** węzła.  W **TCP Port** wprowadź **1433**.

          ![Configure SQL Express for TCP/IP][3]
   7. Kliknij przycisk **OK**.  Kliknij przycisk **OK** w wyskakującym oknie dialogowym.
   8. Kliknij przycisk **usług SQL Server** w menu po lewej stronie drzewa.
   9. Kliknij prawym przyciskiem myszy **programu SQL Server (SQLEXPRESS)** i wybierz **ponownego uruchomienia**
   10. Zamknij Menedżera konfiguracji programu SQL Server 2014.
3. Uruchom program SQL Server 2014 Management Studio i nawiąż połączenie lokalne wystąpienie programu SQL Express

   1. Kliknij prawym przyciskiem myszy wystąpienie w Eksploratorze obiektów i wybierz **właściwości**
   2. Wybierz **zabezpieczeń** strony.
   3. Upewnij się, **programu SQL Server i uwierzytelniania systemu Windows tryb** jest zaznaczone
   4. Kliknij przycisk **OK**.

          ![Configure SQL Express Authentication][4]
   5. Rozwiń węzeł **zabezpieczeń** > **logowania** w Eksploratorze obiektów
   6. Kliknij prawym przyciskiem myszy **logowania** i wybierz **nowe dane logowania...**
   7. Wprowadź nazwę logowania.  Wybierz pozycję **Uwierzytelnianie programu SQL Server**.  Wprowadź hasło, a następnie wprowadź to samo hasło w **Potwierdź hasło**.  Hasło musi spełniać wymagania co do złożoności systemu Windows.
   8. Kliknij przycisk **OK**.

          ![Add a new user to SQL Express][5]
   9. Kliknij prawym przyciskiem myszy nazwę nowego użytkownika, a następnie wybierz **właściwości**
   10. Wybierz **ról serwera** strony
   11. Zaznacz pole wyboru obok pozycji **dbcreator** roli serwera
   12. Kliknij przycisk **OK**.
   13. Zamknij program SQL Server 2015 Management Studio

Upewnij się, że rekord nazwy użytkownika i hasła, które wybrano.  Może być konieczne do przypisywania dodatkowych ról serwera lub uprawnienia w zależności od wymagań określonej bazy danych.

Odczyty aplikacji Node.js **SQLCONNSTR_MS_TableConnectionString** zmiennej środowiskowej ciągu połączenia dla tej bazy danych.  Można ustawić tę zmienną w danym środowisku.  Na przykład aby ustawić tej zmiennej środowiskowej można Użyj programu PowerShell:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Dostęp do bazy danych przy użyciu połączenia TCP/IP i podaj nazwę użytkownika i hasło dla połączenia.

### <a name="howto-config-localdev"></a>Porady: Konfigurowanie projektu dla rozwoju lokalnych
Aplikacje mobilne platformy Azure odczytuje plik JavaScript o nazwie *azureMobile.js* z lokalnego systemu plików.  Nie należy używać tego pliku można skonfigurować zestaw SDK usługi Azure Mobile Apps w środowisku produkcyjnym — za pomocą ustawień aplikacji w ramach [portalu Azure] zamiast tego.  *AzureMobile.js* plików należy wyeksportować obiekt konfiguracji.  Najczęściej używane ustawienia to:

* Ustawienia bazy danych
* Ustawienia rejestrowania diagnostycznego
* Alternatywne ustawienia mechanizmu CORS

Przykład *azureMobile.js* plik implementacji poprzedniego ustawienia bazy danych:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

Firma Microsoft zaleca dodanie *azureMobile.js* do Twojej *.gitignore* pliku (lub inne Ignoruj pliku kontroli kodu źródłowego) aby zapobiec hasła są przechowywane w chmurze.  Zawsze skonfigurować ustawienia produkcji w ustawieniach aplikacji w ramach [portalu Azure].

### <a name="howto-appsettings"></a>Instrukcje: Konfigurowanie ustawień aplikacji dla aplikacji mobilnej
Większość ustawień w *azureMobile.js* plik ma odpowiednik ustawienia aplikacji [portalu Azure].  Aby skonfigurować aplikację w ustawieniach aplikacji, należy użyć następującej listy:

| Ustawienia aplikacji | *azureMobile.js* ustawienie | Opis | Prawidłowe wartości |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |name |Nazwa aplikacji |ciąg |
| **MS_MobileLoggingLevel** |Logging.level |Poziom minimalna dziennika komunikatów do zarejestrowania |błąd, ostrzeżenie, informacje o verbose, debug, niemądre |
| **MS_DebugMode** |debuguj |Włącz lub wyłącz tryb debugowania |wartość true, false |
| **MS_TableSchema** |Data.Schema |Domyślna nazwa schematu dla tabel SQL |ciąg (domyślne: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Włącz lub wyłącz tryb debugowania |wartość true, false |
| **MS_DisableVersionHeader** |Wersja (Ustaw do niezdefiniowanego) |Wyłącza nagłówka X-ZUMO-Server-Version |wartość true, false |
| **MS_SkipVersionCheck** |skipversioncheck |Wyłącza sprawdzanie wersji klienta interfejsu API |wartość true, false |

Można skonfigurować ustawienie aplikacji:

1. Zaloguj się do witryny [portalu Azure].
2. Wybierz **wszystkie zasoby** lub **usługi aplikacji** następnie kliknij nazwę aplikacji mobilnej.
3. Domyślnie zostanie otwarty blok ustawień. Jeśli nie kliknij przycisk **ustawienia**.
4. Kliknij przycisk **ustawienia aplikacji** w menu Ogólne.
5. Przewiń do sekcji ustawień aplikacji.
6. Jeśli aplikacja, ustawienia już istnieje, kliknij wartość ustawienia aplikacji, aby edytować wartość.
7. Jeśli ustawienia aplikacji nie istnieje, wprowadź ustawienie aplikacji w polu klucza i wartość w polu wartość.
8. Po zakończeniu, kliknij przycisk **zapisać**.

Zmiana większość ustawień aplikacji wymaga ponownego uruchomienia usługi.

### <a name="howto-use-sqlazure"></a>Porady: Użyj bazy danych SQL jako magazyn danych produkcyjnych
<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Przy użyciu bazy danych SQL Azure jako magazynu danych jest identyczne we wszystkich typów aplikacji w usłudze Azure App Service. Jeśli jeszcze tego nie zrobiono, wykonaj następujące kroki, aby utworzyć zaplecze aplikacji mobilnej.

1. Zaloguj się do witryny [portalu Azure].
2. W górnym lewym rogu okna, kliknij przycisk **+ nowy** przycisk > **sieci Web i mobilność** > **aplikacji mobilnej**, następnie podaj nazwę zaplecza aplikacji mobilnej.
3. W **grupy zasobów** wprowadź takiej samej nazwie co aplikacja.
4. Plan usługi aplikacji domyślnie jest zaznaczone.  Jeśli chcesz zmienić swój plan usługi aplikacji, możesz to zrobić, klikając Plan usługi aplikacji > **+ Utwórz nowe**.  Podaj nazwę nowego planu usługi aplikacji i wybierz odpowiednią lokalizację.  Kliknij warstwa cenowa i wybierz odpowiednią warstwę cenową dla usługi. Wybierz **Wyświetl wszystkie** do widoku więcej cennik opcje, takie jak **wolne** i **Shared**.  Po wybraniu warstwy cenowej kliknij **wybierz** przycisku.  W **planu usługi aplikacji** bloku, kliknij przycisk **OK**.
5. Kliknij przycisk **Utwórz**. Inicjowanie obsługi administracyjnej zaplecza aplikacji mobilnej może potrwać kilka minut.  Po zainicjowaniu obsługi zaplecza aplikacji mobilnej otwartego portalu **ustawienia** bloku dla zaplecza aplikacji mobilnej.

Po utworzeniu zaplecza aplikacji mobilnej można połączyć z istniejącej bazy danych SQL do zaplecza aplikacji mobilnej albo utwórz nową bazę danych SQL.  W tej sekcji utworzymy bazy danych SQL.

> [!NOTE]
> Jeśli masz już bazę danych w tej samej lokalizacji co zaplecza aplikacji mobilnej, zamiast tego można wybrać **Użyj istniejącej bazy danych** , a następnie wybrać tę bazę danych. Bazy danych w innej lokalizacji nie jest zalecane z powodu większych opóźnień.
>
>

1. W zaplecze nowej aplikacji mobilnej kliknij **ustawienia** > **aplikacji mobilnej** > **danych** > **+ Dodaj**.
2. W **Dodaj połączenie danych** bloku, kliknij przycisk **baza danych SQL — Skonfiguruj wymagane ustawienia** > **Utwórz nową bazę danych**.  Wprowadź nazwę nowej bazy danych w **nazwa** pola.
3. Kliknij przycisk **serwera**.  W **nowy serwer** bloku, wprowadź unikatową nazwą serwera w **nazwy serwera** pól i zapewnienia odpowiedniej **identyfikator logowania administratora serwera** i **hasło**.  Upewnij się, **Zezwalaj usługom platformy azure na dostęp do serwera** jest zaznaczony.  Kliknij przycisk **OK**.

    ![Utwórz bazę danych Azure SQL][6]
4. Na **nową bazę danych** bloku, kliknij przycisk **OK**.
5. Ponownie **Dodaj połączenie danych** bloku, wybierz opcję **ciąg połączenia**, wprowadź identyfikator logowania i hasło, podane podczas tworzenia bazy danych.  Jeśli używasz istniejącej bazy danych, należy podać poświadczenia logowania dla tej bazy danych.  Po wpisaniu, kliknij przycisk **OK**.
6. Ponownie **Dodaj połączenie danych** bloku ponownie, kliknij przycisk **OK** utworzyć bazę danych.

<!--- END OF ALTERNATE INCLUDE -->

Utworzenie bazy danych może potrwać kilka minut.  Użyj **powiadomienia** obszaru do monitorowania postępu wdrożenia.  Nie postępu aż do bazy danych został wdrożony pomyślnie.  Po pomyślnym wdrożeniu, ciąg połączenia jest tworzony dla wystąpienia bazy danych SQL w zapleczu swojej przenośnych ustawień aplikacji.  Można wyświetlić to ustawienie aplikacji w **ustawienia** > **ustawienia aplikacji** > **parametry połączenia**.

### <a name="howto-tables-auth"></a>Porady: Wymagaj uwierzytelniania dostępu do tabel
Jeśli chcesz użyć uwierzytelniania usługi aplikacji z punktem końcowym tabel, należy skonfigurować uwierzytelnianie usługi aplikacji w [portalu Azure] pierwszy.  Aby uzyskać więcej informacji na temat konfigurowania uwierzytelniania w usłudze Azure App Service Sprawdź w przewodniku konfiguracji dla dostawcy tożsamości, które mają być używane:

* [Jak skonfigurować uwierzytelnianie usługi Azure Active Directory]
* [Jak skonfigurować uwierzytelnianie usługi Facebook]
* [Jak skonfigurować uwierzytelnianie Google]
* [Jak skonfigurować Authentication firmy Microsoft]
* [Jak skonfigurować uwierzytelnianie w usłudze Twitter]

Każda tabela ma właściwość dostępu, który może służyć do kontrolowania dostępu do tabeli.  Poniższy przykład przedstawia statycznie zdefiniowanych tabeli z wymagane uwierzytelnienie.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Właściwość dostępu można wykonać jedną z następujących wartości:

* *anonimowe* wskazuje, czy aplikacja kliencka może odczytać danych bez uwierzytelniania
* *uwierzytelniony* oznacza, że aplikacja kliencka musi wysłany token uwierzytelniania prawidłowe z żądaniem
* *wyłączone* wskazuje, że ta tabela jest obecnie wyłączona

Jeśli zdefiniowano właściwość dostęp jest dozwolony dostęp nieuwierzytelniony.

### <a name="howto-tables-getidentity"></a>Porady: Użyj uwierzytelniania oświadczeń z tabel
Można skonfigurować różne oświadczenia, które są wymagane podczas konfigurowania uwierzytelniania.  Te oświadczenia nie są zwykle dostępne za pośrednictwem `context.user` obiektu.  Jednak mogą zostać pobrane za pomocą `context.user.getIdentity()` metody.  `getIdentity()` Metoda zwraca Promise, który jest rozpoznawany jako obiekt.  Obiekt jest wyznaczaną przez metodę uwierzytelniania (facebook, google, twitter, microsoftaccount lub aad).

Na przykład jeśli skonfigurowano uwierzytelnianie Microsoft Account i żądania oświadczeń adresów e-mail, można dodać adres e-mail do rekordu o następujący kontroler tabeli:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Aby zobaczyć, jakie oświadczenia są dostępne, użyj przeglądarki sieci web, aby wyświetlić `/.auth/me` punktu końcowego witryny.

### <a name="howto-tables-disabled"></a>Porady: wyłączanie dostępu do określonej tabeli operacji
Oprócz wymienionych w tabeli, właściwości dostępu można można użyć do kontrolowania poszczególnych działań.  Istnieją cztery operacje:

* *Przeczytaj* jest operacji RESTful GET, w tabeli
* *Wstaw* jest operacji RESTful POST w tabeli
* *Zaktualizuj* jest poprawka RESTful operacji w tabeli
* *Usuń* jest operacją RESTful usuwania w tabeli

Na przykład możesz podać nieuwierzytelnione tabeli tylko do odczytu:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Porady: dopasowywanie kwerendę, która jest używana z operacjami tabeli
Typowe wymagania dotyczące operacji tabeli jest zapewnienie ograniczony widok danych.  Na przykład może udostępnić tabelę, która zostanie oznaczony przy użyciu Identyfikatora uwierzytelnionego użytkownika w taki sposób, że można tylko do odczytu lub aktualizacji własnych rekordów.  Oferuje następujące definicji tabeli:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Operacje, które zwykle wykonać zapytania ma właściwość zapytania, który można dostosować, w której klauzuli. Właściwość kwerendy jest [QueryJS] obiekt, który służy do konwertowania zapytania OData do zasobu, który może przetwarzać danych wewnętrznej bazy danych.  W przypadku prostego równości (na przykład mieszanego) można użyć mapy. Możesz także dodać punkty SQL:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Porady: Konfigurowanie usuwania nietrwałego dla tabeli
Faktycznie usuwania nietrwałego nie powoduje usunięcia rekordów.  Zamiast tego oznacza je jako usunięte w bazie danych, ustawiając usuniętą kolumnę na wartość true.  Zestaw SDK aplikacji usługi Azure Mobile automatycznie usuwa wszystkie usunięte nietrwale rekordy z wyników, chyba że IncludeDeleted() korzysta z zestawu SDK klienta Mobile.  Aby skonfigurować tabelę dla usuwania nietrwałego, ustaw `softDelete` właściwość w pliku definicji tabeli:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Należy określić mechanizm przeczyszczanie rekordów — od aplikacji klienta, za pomocą zadania WebJob, funkcji platformy Azure lub za pomocą niestandardowego interfejsu API.

### <a name="howto-tables-seeding"></a>Porady: inicjatora bazy danych z danymi
Podczas tworzenia nowej aplikacji, możesz inicjatora tabelę z danymi.  Można to zrobić w pliku JavaScript definicji tabeli w następujący sposób:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Wstępne wypełnianie danych jest wykonywane tylko, gdy tabela została utworzona przez zestaw SDK usługi Azure Mobile Apps.  Jeśli tabela już istnieje w bazie danych, bez danych jest dodane do tabeli.  Jeśli włączono dynamiczne schematu schematu jest wywnioskować na podstawie wprowadzonych danych.

Zaleca się, że można jawnie wywołać `tables.initialize()` metodę w celu utworzenia tabeli, gdy usługa zacznie działać.

### <a name="Swagger"></a>Porady: Włączanie obsługi programu Swagger
Usługa Azure App Service Mobile Apps jest dostarczany z wbudowanych [Swagger] obsługuje.  Aby włączyć obsługę programu Swagger, należy najpierw zainstalować interfejsu użytkownika programu swagger jako zależność:

    npm install --save swagger-ui

Po zakończeniu instalacji można włączyć obsługę struktury Swagger w Konstruktorze Azure Mobile Apps:

    var mobile = azureMobileApps({ swagger: true });

Możesz tylko prawdopodobnie chcesz włączyć obsługi struktury Swagger w wersjach programowanie.  Można to zrobić przy użyciu `NODE_ENV` ustawienie aplikacji:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Punktu końcowego struktury swagger znajduje się pod adresem http://*yoursite*.azurewebsites.net/swagger.  Można uzyskać dostępu do interfejsu użytkownika programu Swagger za pomocą `/swagger/ui` punktu końcowego.  Jeśli wybierzesz opcję Wymagaj uwierzytelniania na całej aplikacji, Swagger powoduje błąd.  Aby uzyskać najlepsze wyniki, wybierz, aby zezwalać nieuwierzytelnione żądania za pośrednictwem w uwierzytelnianiu usługi aplikacji Azure / następnie kontrolować ustawienia autoryzacji uwierzytelnianie przy użyciu `table.access` właściwości.

Możesz także dodać opcję Swagger z `azureMobile.js` plik, jeśli mają tylko obsługę struktury Swagger podczas opracowywania lokalnie.

## <a name="a-namepushpush-notifications"></a><a name="push">Powiadomienia wypychane
Aplikacje mobilne integruje się z usługi Azure Notification Hubs umożliwia wysyłanie powiadomień wypychanych docelowe do milionów urządzeń we wszystkich głównych platform. Przy użyciu usługi Notification Hubs, można wysyłać powiadomienia wypychane w systemach iOS, Android i Windows. Aby dowiedzieć się więcej na temat wszystkich, które można wykonać przy użyciu usługi Notification Hubs, zobacz [omówienie centra powiadomień](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Porady: wysyłanie powiadomień wypychanych
Poniższy kod przedstawia sposób użycia obiektu wypychania do wysyłania powiadomień wypychanych emisji do urządzeń z systemem iOS zarejestrowanego:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Tworząc rejestracji wypychanych szablonu z klienta, można zamiast tego Wyślij wiadomość wypychania szablonu na urządzeniach, na wszystkich obsługiwanych platformach. Poniższy kod przedstawia sposób wysłania powiadomienia szablonu:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


### <a name="push-user"></a>Porady: wysyłanie powiadomień wypychanych do uwierzytelnionego użytkownika przy użyciu tagów
Uwierzytelniony użytkownik rejestrujący dla powiadomień wypychanych, tag identyfikator użytkownika jest automatycznie dodawany do rejestracji. Za pomocą tego tagu, można wysyłać powiadomienia wypychane do wszystkich urządzeń zarejestrowanych przez określonego użytkownika. Poniższy kod pobiera identyfikator SID użytkownika zgłaszającego żądanie i wyśle powiadomienie wypychane szablonu do rejestracji urządzeń dla tego użytkownika:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Podczas rejestrowania dla powiadomień wypychanych z uwierzytelnionego klienta, upewnij się, że przed podjęciem próby wykonania rejestracji zakończeniem uwierzytelniania.

## <a name="CustomAPI"></a>Niestandardowych interfejsów API
### <a name="howto-customapi-basic"></a>Porady: Definiowanie niestandardowego interfejsu API
Oprócz dostępu do danych interfejsu API za pośrednictwem punktu końcowego /tables Azure Mobile Apps zapewniają niestandardowe pokrycia interfejsu API.  Niestandardowe interfejsy API są definiowane w sposób podobny do definicji tabeli i mogą uzyskiwać dostęp do tych samych urządzeń, włącznie z uwierzytelnianiem.

Jeśli chcesz korzystać z API niestandardowych aplikacji usługi uwierzytelniania, należy skonfigurować uwierzytelnianie usługi aplikacji w [portalu Azure] pierwszy.  Aby uzyskać więcej informacji na temat konfigurowania uwierzytelniania w usłudze Azure App Service Sprawdź w przewodniku konfiguracji dla dostawcy tożsamości, które mają być używane:

* [Jak skonfigurować uwierzytelnianie usługi Azure Active Directory]
* [Jak skonfigurować uwierzytelnianie usługi Facebook]
* [Jak skonfigurować uwierzytelnianie Google]
* [Jak skonfigurować Authentication firmy Microsoft]
* [Jak skonfigurować uwierzytelnianie w usłudze Twitter]

Niestandardowych interfejsów API są definiowane w znacznie taki sam sposób jak tabele interfejsu API.

1. Utwórz **interfejsu api** katalogu
2. Utwórz plik JavaScript definicji interfejsu API w **interfejsu api** katalogu.
3. Metoda importu do zaimportowania **interfejsu api** katalogu.

W tym miejscu znajduje się definicja interfejsu api prototypu na podstawie próbki basic-app, do którego użyliśmy wcześniej.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Spójrzmy na przykład interfejsu API, która zwraca datę serwerze przy użyciu *Date.now()* metody.  Oto plik api/date.js:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Każdy parametr jest jednym z zleceń standardowych RESTful - GET, POST, poprawki lub DELETE.  Metoda jest standardem [oprogramowanie pośredniczące ExpressJS] funkcja, która wysyła wymagane dane wyjściowe.

### <a name="howto-customapi-auth"></a>Porady: wymagają uwierzytelniania w celu uzyskania dostępu do niestandardowego interfejsu API
Azure Mobile Apps SDK implementuje uwierzytelniania w taki sam sposób niestandardowych interfejsów API i tabele punktu końcowego.  Aby dodać uwierzytelniania interfejsu API w poprzedniej sekcji, Dodaj **dostępu** właściwości:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Można również określić uwierzytelniania na określonych operacji:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Należy użyć tego samego tokenu, który jest używany dla punktu końcowego, tabelami dla niestandardowych interfejsów API wymaga uwierzytelniania.

### <a name="howto-customapi-auth"></a>Porady: Obsługa wysyłania dużych plików
Azure Mobile Apps SDK używa [oprogramowanie pośredniczące treści analizator](https://github.com/expressjs/body-parser) do akceptowania i dekodowania zawartości w treści w Twoje zgłoszenie.  Można wstępnie skonfigurować analizator treści do przyjęcia przekazywania plików większych:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Plik jest algorytmem przed transmisją base-64.  To spowoduje zwiększenie rozmiaru rzeczywistego przekazywania (i dlatego rozmiar należy uwzględnić).

### <a name="howto-customapi-sql"></a>Porady: wykonanie niestandardowych instrukcje SQL
Zestaw SDK usługi Azure Mobile Apps umożliwia dostęp do całego kontekstu za pośrednictwem obiektu żądania, umożliwiając łatwe wykonywanie sparametryzowanych instrukcji SQL dla dostawcy danych:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Debugowanie, łatwe tabel i łatwe interfejsów API
### <a name="howto-diagnostic-logs"></a>Porady: debugowanie, diagnozowanie i rozwiązywanie problemów z usługi Azure Mobile apps
Usługi Azure App Service zapewnia kilka debugowania i rozwiązywanie problemów z techniki aplikacji Node.js.
Zobacz następujące artykuły, aby rozpocząć pracę z Rozwiązywanie problemów z poziomu zaplecza Node.js Mobile:

* [Monitorowanie usługi aplikacji Azure]
* [Włączanie rejestrowania diagnostycznego w usłudze aplikacji Azure]
* [Rozwiązywanie problemów z usługi aplikacji Azure w programie Visual Studio]

Aplikacje środowiska node.js mają dostęp do szeroką gamę narzędzi diagnostycznych dziennika.  Wewnętrznie, zestaw SDK usługi Azure Mobile Apps Node.js używa [Winston] dla rejestrowania diagnostycznego.  Rejestrowanie jest automatycznie włączone, należy włączyć tryb debugowania lub przez ustawienie **MS_DebugMode** ustawienia aplikacji na wartość true w [portalu Azure]. Dzienniki generowane są wyświetlane w dziennikach diagnostycznych na [portalu Azure].

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Porady: Praca z tabelami łatwy w portalu Azure
Łatwe tabel w portalu umożliwiają tworzenie i Praca z tabelami bezpośrednio w portalu. Możesz przekazać do tabel łatwe zestawu danych w formacie CSV. Należy pamiętać, że nie można użyć nazwy właściwości (w zestawie danych CSV), które są w konflikcie z nazwami właściwości systemu zaplecza Azure Mobile Apps. Nazwy właściwości systemu są:
* CreatedAt
* updatedAt
* usunięte
* wersja

Operacje tabeli za pomocą edytora usługi aplikacji nawet można edytować. Po kliknięciu **łatwe tabel** w ustawieniach wewnętrznej bazy danych lokacji można Dodawanie, modyfikowanie lub usuwanie tabeli. Można również wyświetlić dane w tabeli.

![Praca z tabelami łatwe](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Poniższe polecenia są dostępne na pasku poleceń dla tabeli:

* **Zmienianie uprawnień** — modyfikować uprawnienia do odczytu, wstawianie, aktualizowanie i usuwanie operacji względem tabeli.
  Aby zezwolić na dostęp anonimowy, aby wymagać uwierzytelniania lub wyłącz dostęp do operacji są opcje.
* **Edytowanie skryptu** — plik skryptu dla tabeli jest otwarty w edytorze usługi aplikacji.
* **Zarządzanie schematem** — Dodawanie lub usuwanie kolumn lub zmiana indeksu tabeli.
* **Wyczyść tabeli** -obcina istniejącej tabeli jest usunięcie wszystkich wierszy danych, ale pozostawienie schematu bez zmian.
* **Usuwanie wierszy** -Usuń poszczególne wiersze danych.
* **Wyświetl podgląd dzienników przesyłanych strumieniowo** -łączy do przesyłania strumieniowego usługi rejestrowania dla witryny.

### <a name="work-easy-apis"></a>Porady: pracy z interfejsami API łatwy w portalu Azure
Łatwe interfejsów API w portalu umożliwiają tworzenie i Praca z niestandardowych interfejsów API bezpośrednio w portalu. Można edytować skrypty interfejsu API za pomocą edytora usługi aplikacji.

Po kliknięciu **łatwe interfejsów API** w ustawieniach wewnętrznej bazy danych lokacji można Dodawanie, modyfikowanie lub usuwanie niestandardowych punkt końcowy interfejsu API.

![Praca z łatwe interfejsów API](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

W portalu można zmienić uprawnienia dostępu dla danej akcji HTTP, Edytuj plik skryptu interfejsu API w edytorze usługi aplikacji lub Wyświetl dzienniki przesyłania strumieniowego.

### <a name="online-editor"></a>Porady: edytowanie kodu w edytorze usługi aplikacji
Azure portal umożliwia edytowanie plików skryptów zaplecza Node.js w edytorze usługi aplikacji bez konieczności pobierania projektu na komputerze lokalnym. Aby edytować pliki skryptów w edytorze online:

1. W bloku zaplecza aplikacji mobilnej, kliknij **wszystkie ustawienia** > albo **łatwe tabel** lub **łatwe interfejsów API**, kliknij tabelę lub interfejsu API, a następnie kliknij przycisk **edytowanie skryptu**. Plik skryptu jest otwarty w edytorze usługi aplikacji.

    ![Edytor usługi App Service](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)
2. Wprowadź zmiany w pliku kodu w edytorze online. Zmiany zostaną zapisane automatycznie podczas pisania.

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Szybki Start kliencką dla systemu android]: app-service-mobile-android-get-started.md
[Apache Cordova klienta — Szybki Start]: app-service-mobile-cordova-get-started.md
[iOS — Szybki Start klienta]: app-service-mobile-ios-get-started.md
[Szybki Start klienta platformy Xamarin.iOS]: app-service-mobile-xamarin-ios-get-started.md
[Szybki Start klienta platformy Xamarin.Android]: app-service-mobile-xamarin-android-get-started.md
[Szybki Start klienta platformy Xamarin.Forms]: app-service-mobile-xamarin-forms-get-started.md
[Szybki Start klienta Sklepu Windows]: app-service-mobile-windows-store-dotnet-get-started.md
[synchronizacji danych w trybie offline]: app-service-mobile-offline-data-sync.md
[Jak skonfigurować uwierzytelnianie usługi Azure Active Directory]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Jak skonfigurować uwierzytelnianie usługi Facebook]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[Jak skonfigurować uwierzytelnianie Google]: ../app-service/app-service-mobile-how-to-configure-google-authentication.md
[Jak skonfigurować Authentication firmy Microsoft]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Jak skonfigurować uwierzytelnianie w usłudze Twitter]: ../app-service/app-service-mobile-how-to-configure-twitter-authentication.md
[Azure App Service Deployment Guide]: ../app-service/app-service-deploy-local-git.md
[Monitorowanie usługi aplikacji Azure]: ../app-service/web-sites-monitor.md
[Włączanie rejestrowania diagnostycznego w usłudze aplikacji Azure]: ../app-service/web-sites-enable-diagnostic-log.md
[Rozwiązywanie problemów z usługi aplikacji Azure w programie Visual Studio]: ../app-service/web-sites-dotnet-troubleshoot-visual-studio.md
[Określ wersję węzła]: ../nodejs-specify-node-version-azure-apps.md
[Użyj modułów węzła]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[portalu Azure]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp przykładem w witrynie GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo przykładem w witrynie GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[katalogu przykładów w witrynie GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 dla programu Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[pakietu Node.js mssql]: https://www.npmjs.com/package/mssql
[programu Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[oprogramowanie pośredniczące ExpressJS]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
