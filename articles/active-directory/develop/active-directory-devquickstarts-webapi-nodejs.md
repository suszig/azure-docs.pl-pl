---
title: "Azure AD środowiska Node.js wprowadzenie | Dokumentacja firmy Microsoft"
description: "Jak utworzyć sieci web Node.js REST API, która integruje się z usługą Azure AD do uwierzytelniania."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 4f58177f540c14172d7ece8b4bc8c8a2b9787f8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-web-apis-for-nodejs"></a>Rozpoczynanie pracy z interfejsów API sieci web dla środowiska Node.js
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

*Passport* to uwierzytelniające oprogramowanie pośredniczące dla środowiska Node.js. Elastyczne i moduły, usługa Passport można dyskretnie usunięty każdym Express opartą na lub Restify aplikacji sieci web. Kompleksowy zestaw strategii obsługuje uwierzytelnianie za pomocą nazwy użytkownika i hasła, Facebook i Twitter. Opracowaliśmy strategię dla Microsoft Azure Active Directory (Azure AD). Firma Microsoft zainstalowaniu tego modułu, a następnie dodaj Microsoft Azure Active Directory `passport-azure-ad` wtyczki.

W tym celu należy:

1. Zarejestrować aplikację w usłudze Azure AD.
2. Konfigurowanie aplikacji do użycia w usłudze Passport `passport-azure-ad` wtyczki.
3. Skonfigurować aplikację klienta do wywołania interfejsu API sieci web listy zadań do wykonania.

Kod używany w tym samouczku jest przechowywany [ w serwisie GitHub](https://github.com/Azure-Samples/active-directory-node-webapi).

> [!NOTE]
> W tym artykule nie opisano sposobu wdrażania logowania, rejestracji lub profil zarządzania za pomocą usługi Azure AD B2C. Uwzględniono w szczególności wywoływania interfejsów API sieci web po użytkownik jest już uwierzytelniony.  Zalecamy rozpocząć od [sposobu integracji z usługą Azure Active Directory dokumentem](active-directory-how-to-integrate.md) się podstawowe informacje na temat usługi Azure Active Directory.
>
>

Zostały Opublikowano kod źródłowy w tym przykładzie uruchomionej w usłudze GitHub MIT licencji, tak się klonowania (lub nawet lepiej rozwidlenia) i wyrazić swoją opinię oraz żądań ściągnięcia.

## <a name="about-nodejs-modules"></a>O modułach Node.js
Używamy modułów programu Node.js w tym przewodniku. Moduły są obciążana pakiety języka JavaScript, które zapewniają określonych funkcji aplikacji. Zazwyczaj należy zainstalować moduły przy użyciu środowiska Node.js NPM narzędzia wiersza polecenia w katalogu instalacyjnym programu NPM. Jednak niektóre moduły, takich jak moduł HTTP znajdują się w pakiecie Node.js core.

Zainstalowane moduły są zapisywane w **node_modules** katalogu w głównym katalogu instalacji środowiska Node.js. Każdy moduł w **node_modules** directory zachowuje własną **node_modules** katalog, który zawiera wszystkie moduły, których ona zależy. Ponadto każdego wymaganego modułu jest **node_modules** katalogu. Ta struktura katalogów cykliczne reprezentuje łańcuch zależności.

Ta struktura łańcuch zależności powoduje większe zużycie aplikacji. Jednak gwarantuje również to, że wszystkie zależności są spełnione i że wersji modułów, która jest używana do rozwoju jest również używany w środowisku produkcyjnym. To zapewnia bardziej przewidywalne zachowanie aplikacji produkcyjnych i zapobiega problemom z wersjami, które mogą mieć wpływ na użytkowników.

## <a name="step-1-register-an-azure-ad-tenant"></a>Krok 1: Zarejestruj dzierżawę usługi Azure AD
Aby korzystać z tej próbki, należy dzierżawy usługi Azure Active Directory. Jeśli nie masz pewności jakie dzierżawcy jest lub sposobie pobierania, zobacz [jak uzyskać dzierżawę usługi Azure AD](active-directory-howto-tenant.md).

## <a name="step-2-create-an-application"></a>Krok 2: Tworzenie aplikacji
Następnie utwórz aplikację w katalogu zawiera informacje wymagane do bezpiecznego komunikowania się z aplikacji Azure AD.  Zarówno aplikacja klienta, jak i interfejsu API sieci web są reprezentowane przez jeden **identyfikator aplikacji** w tym przypadku, ponieważ stanowią jedną aplikację logiczną.  Aby utworzyć aplikację, postępuj zgodnie z [tymi instrukcjami](active-directory-how-applications-are-added.md). Jeśli tworzysz aplikację z biznesowych, [te dodatkowe instrukcje mogą okazać się przydatne](../active-directory-applications-guiding-developers-for-lob-applications.md).

Aby utworzyć aplikację:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. W górnym menu wybierz konto. Następnie w obszarze **katalogu** wybierz dzierżawy usługi Active Directory, które chcesz zarejestrować aplikację.

3. W menu po lewej stronie wybierz **więcej usług**, a następnie wybierz **usługi Azure Active Directory**.

4. Wybierz **rejestracji aplikacji**, a następnie wybierz **Dodaj**.

5. Postępuj zgodnie z monitami, aby utworzyć **aplikacji sieci Web i/lub WebAPI**.

      * **Nazwa** aplikacji opisuje aplikacji dla użytkowników końcowych.

      * **Adres URL logowania** jest podstawowy adres URL aplikacji.  Jest to domyślny adres URL w przykładowym kodzie `https://localhost:8080`.

6. Po zarejestrowaniu, usługi Azure AD przypisuje aplikacji Unikatowy identyfikator aplikacji. Potrzebujesz tej wartości w kolejnych sekcjach, dlatego skopiuj go ze strony aplikacji.

7. Z **ustawienia** -> **właściwości** strony dla aplikacji, zaktualizuj identyfikator URI aplikacji. **Identyfikator URI aplikacji** to unikatowy identyfikator aplikacji. Konwencji jest użycie `https://<tenant-domain>/<app-name>`, na przykład: `https://contoso.onmicrosoft.com/my-first-aad-app`.

8. Utwórz **klucza** aplikacji z **ustawienia** strony, a następnie skopiuj go innym. Będzie on potrzebny później.

## <a name="step-3-download-nodejs-for-your-platform"></a>Krok 3: Pobieranie środowiska Node.js dla danej platformy
Aby pomyślnie korzystać z tej próbki, należy dysponować działającą instalacją środowiska Node.js.

Zainstaluj środowisko Node.js z [http://nodejs.org](http://nodejs.org).

## <a name="step-4-install-mongodb-on-your-platform"></a>Krok 4: Instalacja bazy danych MongoDB na platformie
Aby pomyślnie korzystać z tej próbki, należy dysponować działającą instalacją bazy danych mongodb. Używasz bazy danych MongoDB ustawienia interfejsu API REST trwałego w wystąpieniach serwera.

Instalowanie bazy danych MongoDB z [http://mongodb.org](http://www.mongodb.org).

> [!NOTE]
> W tym przewodniku założono użycie domyślnej instalacji i serwera punktów końcowych dla bazy danych MongoDB, które w momencie pisania tego dokumentu jest mongodb://localhost.
>
>

## <a name="step-5-install-the-restify-modules-in-your-web-api"></a>Krok 5: Instalowanie modułów Restify w interfejsie API sieci web
Restify jest używany do tworzenia interfejsie API REST. Restify jest minimalną i elastyczną Node.js aplikacji platforma, która jest pochodną Express. Oferuje rozbudowany zestaw funkcji do tworzenia interfejsów API REST na bazie usługi Connect.

### <a name="install-restify"></a>Instalacja modułu Restify
1. W wierszu polecenia przejdź do **azuread** katalogu. Jeśli **azuread** katalog nie istnieje, utwórz go.

        `cd azuread - or- mkdir azuread; cd azuread`

2. Wpisz następujące polecenie:

    `npm install restify`

    To polecenie powoduje zainstalowanie modułu Restify.

#### <a name="did-you-get-an-error"></a>Czy został wyświetlony komunikat o błędzie?
Korzystając z programu NPM w niektórych systemach operacyjnych, zostanie zgłoszony błąd informujący o **błąd: EPERM, chmod "/ usr/lokalnej/bin /..."** i sugestii, spróbuj konto uruchamiania jako administrator. W takim przypadku należy użyć polecenia sudo, aby uruchomić NPM na wyższym poziomie uprawnień.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>Czy został wyświetlony błąd dotyczący narzędzia DTRACE?
Podczas instalowania modułu Restify mogą pojawić następujący błąd:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```
Moduł Restify zapewnia zaawansowany mechanizm śledzenia wywołań REST przy użyciu narzędzia DTrace. Jednak wiele systemów operacyjnych bez narzędzia DTrace. Można te błędy bezpiecznie zignorować.

Dane wyjściowe tego polecenia powinny wyglądać podobnie do następujących danych wyjściowych:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)


## <a name="step-6-install-passportjs-in-your-web-api"></a>Krok 6: Zainstaluj Passport.js w interfejsie API sieci web
[Passport](http://passportjs.org/) to uwierzytelniające oprogramowanie pośredniczące dla środowiska Node.js. Elastyczne i moduły, usługa Passport można dyskretnie usunięty każdym Express opartą na lub Restify aplikacji sieci web. Kompleksowy zestaw strategii obsługuje uwierzytelnianie za pomocą nazwy użytkownika i hasła, Facebook i Twitter.

Opracowaliśmy strategię dla usługi Azure Active Directory. Firma Microsoft zainstalowaniu tego modułu, a następnie dodaj wtyczki usługi Azure Active Directory.

1. W wierszu polecenia przejdź do **azuread** katalogu.

2. Aby zainstalować passport.js, wprowadź następujące polecenie:

    `npm install passport`

    Dane wyjściowe polecenia powinien wyglądać podobnie do następującego:

``
        passport@0.1.17 node_modules\passport
        ├── pause@0.0.1
        └── pkginfo@0.2.3
``

## <a name="step-7-add-passport-azure-ad-to-your-web-api"></a>Krok 7: Dodawanie usługi Passport-Azure-AD do interfejsu API sieci web
Następnie dodamy strategię OAuth przy użyciu `passport-azure-ad`, zestawu strategii łączących usługę Azure Active Directory do usługi Passport. Używamy tej strategii do tokenów elementów nośnych w próbce tego interfejsu API REST.

> [!NOTE]
> Chociaż protokół OAuth2 zapewnia framework, w którym mogą być wystawiane dowolnego znanego typu tokenu, tylko niektóre typy tokenów są często używane. Tokeny elementów nośnych są najczęściej używane tokeny zabezpieczające punkty końcowe. Są one najczęściej wystawiany typ tokenu protokołu OAuth2. W wielu wdrożeniach zakłada, że tokeny elementów nośnych są jedynym typem tokeny wystawione.
>
>

W wierszu polecenia przejdź do **azuread** katalogu.

Wpisz następujące polecenie, aby zainstalować Passport.js `passport-azure-ad module`:

`npm install passport-azure-ad`

Dane wyjściowe polecenia powinny wyglądać podobnie do następujących danych wyjściowych:


    passport-azure-ad@1.0.0 node_modules/passport-azure-ad
    ├── xtend@4.0.0
    ├── xmldom@0.1.19
    ├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
    ├── underscore@1.8.3
    ├── async@1.3.0
    ├── jsonwebtoken@5.0.2
    ├── xml-crypto@0.5.27 (xpath.js@1.0.6)
    ├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
    ├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
    ├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    └── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)



## <a name="step-8-add-mongodb-modules-to-your-web-api"></a>Krok 8: Dodawanie modułów MongoDB do interfejsu API sieci web
Korzystamy z bazy danych MongoDB jako naszego magazynu danych. Z tego powodu należy zainstalować powszechnie używaną wtyczkę Mongoose o nazwie do zarządzania zabezpieczeniami. Musimy również zainstalować sterownik bazy danych dla bazy danych MongoDB (nazywane również bazy danych MongoDB).

 `npm install mongoose`

## <a name="step-9-install-additional-modules"></a>Krok 9: Instalowanie dodatkowych modułów
Następnie możemy Zainstaluj pozostałe wymagane moduły.

1. W wierszu polecenia przejdź do **azuread** folderu, jeśli nie masz już istnieje.

    `cd azuread`

2. Wprowadź następujące polecenia, aby zainstalować te moduły w Twojej **node_modules** katalogu:

    * `npm install assert-plus`
    * `npm install bunyan`
    * `npm update`

## <a name="step-10-create-a-serverjs-with-your-dependencies"></a>Krok 10: Tworzenie server.js własnymi zależnościami
Pliku server.js zapewnia większość funkcjonalności serwera interfejsu API sieci web. Możemy dodać większość naszego kodu do tego pliku. Do celów produkcyjnych zaleca się, że refaktoryzować funkcje na mniejsze pliki, np. odrębne trasy i kontrolery. Z tego pokazu używamy server.js dla tej funkcji.

1. W wierszu polecenia przejdź do **azuread** folderu, jeśli nie masz już istnieje.

    `cd azuread`

2. Utwórz `server.js` plik w edytorze Ulubione, a następnie dodaj następujące informacje:

    ```Javascript
        'use strict';

        /**
         * Module dependencies.
         */

        var fs = require('fs');
        var path = require('path');
        var util = require('util');
        var assert = require('assert-plus');
        var bunyan = require('bunyan');
        var getopt = require('posix-getopt');
        var mongoose = require('mongoose/');
        var restify = require('restify');
        var passport = require('passport');
      var BearerStrategy = require('passport-azure-ad').BearerStrategy;
    ```

3. Zapisz plik. Wrócimy do niego później.

## <a name="step-11-create-a-config-file-to-store-your-azure-ad-settings"></a>Krok 11: Utwórz plik konfiguracji do przechowywania ustawień usługi Azure AD
Ten plik kodu przekazuje parametry konfiguracji z portalu usługi Azure Active Directory do Passport.js. Te wartości konfiguracji są tworzone po dodaniu interfejsu API sieci web w portalu w pierwszej części tego przewodnika. Wyjaśnijmy, co należy umieścić w wartościach tych parametrów, po skopiowaniu kodu.

1. W wierszu polecenia przejdź do **azuread** folderu, jeśli nie masz już istnieje.

    `cd azuread`

2. Utwórz `config.js` plik w edytorze Ulubione, a następnie dodaj następujące informacje:

    ```Javascript
         exports.creds = {
             mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
             clientID: 'your client ID',
             audience: 'your application URL',
            // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
          // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
             identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
             validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
             passReqToCallback: false,
             loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

         };
    ```
3. Zapisz plik.

## <a name="step-12-add-configuration-values-to-your-serverjs-file"></a>Krok 12: Wartości konfiguracji należy dodać do pliku server.js
Należy odczytać wartości z pliku .config, który został utworzony w naszej aplikacji. Aby to zrobić, możemy dodać pliku .config jako wymagany zasób w naszej aplikacji. Następnie ustaw zmienne globalne, aby dopasować zmiennych w dokumencie config.js.

1. W wierszu polecenia przejdź do **azuread** folderu, jeśli nie masz już istnieje.

    `cd azuread`

2. Otwórz z `server.js` plik w edytorze Ulubione, a następnie dodaj następujące informacje:

    ```Javascript
    var config = require('./config');
    ```
3. Następnie dodaj nową sekcję dotyczącą `server.js` następującym kodem:

    ```Javascript
    var options = {
        // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
        identityMetadata: config.creds.identityMetadata,
        clientID: config.creds.clientID,
        validateIssuer: config.creds.validateIssuer,
        audience: config.creds.audience,
        passReqToCallback: config.creds.passReqToCallback,
        loggingLevel: config.creds.loggingLevel

    };

    // Array to hold logged in users and the current logged in user (owner).
    var users = [];
    var owner = null;

    // Our logger.
    var log = bunyan.createLogger({
        name: 'Azure Active Directory Bearer Sample',
             streams: [
            {
                stream: process.stderr,
                level: "error",
                name: "error"
            },
            {
                stream: process.stdout,
                level: "warn",
                name: "console"
            }, ]
    });

      // If the logging level is specified, switch to it.
      if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    ```

4. Zapisz plik.

## <a name="step-13-add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Krok 13: Dodaj informacje o modelu bazy danych MongoDB i schematu przy użyciu wtyczki Mongoose
Teraz tego przygotowania jest będzie można uruchomić płatności, ponieważ połączono tych trzech plików w ramach usługi interfejsu API REST.

W ramach tego przewodnika używamy bazy danych MongoDB do przechowywania nasze zadania zgodnie z opisem w kroku 4.

W `config.js` pliku, że utworzyliśmy krok 11, dzwoniliśmy do naszej bazie danych `tasklist`, ponieważ został który testujemy na końcu naszych **mogoose_auth_local** adres URL połączenia. Nie trzeba tworzyć wcześniej tej bazy danych w module MongoDB. Zamiast tego bazy danych MongoDB tworzy to firmie Microsoft przy pierwszym uruchomieniu aplikacji serwera (przy założeniu, że baza danych już nie istnieje).

Teraz, gdy firma Microsoft zostały informację serwera, które firma Microsoft ma być używana baza danych MongoDB, należy napisać dodatkowy kod w celu utworzenia modelu i schematu dla naszego serwera zadań.

### <a name="discussion-of-the-model"></a>Omówienie modelu
Nasz model schematu jest proste. Rozwiń go zgodnie z potrzebami.

Nazwa: Nazwa osoby, która jest przypisana do zadania. A **ciąg**.

ZADANIE: Samo zadanie. A **ciąg**.

Data: Data przypada zadania. A **DATETIME**.

UKOŃCZONO: Jeśli zadanie zostało zakończone lub nie. A **LOGICZNA**.

### <a name="creating-the-schema-in-the-code"></a>Tworzenie schematu w kodzie
1. W wierszu polecenia przejdź do **azuread** folderu, jeśli nie masz już istnieje.

    `cd azuread`

2. Otwórz z `server.js` plik w edytorze Ulubione, a następnie dodaj poniższe informacje poniżej pozycji konfiguracji:

    ```Javascript
    // Connect to MongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');

    // Here we create a schema to store our tasks and users. It's a fairly simple schema for now.
    var TaskSchema = new Schema({
        owner: String,
        task: String,
        completed: Boolean,
        date: Date
    });

    // Use the schema to register a model.
    mongoose.model('Task', TaskSchema);
    var Task = mongoose.model('Task');
    ```
Jak stwierdzić, z kodu, utworzymy naszych schematu najpierw. Następnie utwórz obiekt modelu używanego do przechowywania danych w całym kodzie podczas definiujemy naszych **tras**.

## <a name="step-14-add-our-routes-for-our-task-rest-api-server"></a>Krok 14: Dodawanie naszych tras do serwera zadań interfejsu API REST
Teraz, gdy mamy model bazy danych do pracy z Dodajmy tras, któremu zamierzamy korzystanie z naszego serwera interfejsu API REST.

### <a name="about-routes-in-restify"></a>Informacje o trasach w module Restify
Trasy działają w module Restify tak samo jak w stosie Express. Trasy są definiowane za pomocą identyfikatora URI, który ma być wywoływany przez aplikacje klienta. Zazwyczaj należy zdefiniować trasy w oddzielnym pliku. W celach naszych testujemy naszych trasy w pliku server.js. Firma Microsoft zaleca współczynnika te trasy do ich własnych plików do użytku produkcyjnego.

Typowy wzorzec trasy modułu Restify jest następujący:

```Javascript
function createObject(req, res, next) {

// Do work on object.

 _object.name = req.params.object; // passed value is in req.params under object

 ///...

return next(); // Keep the server going.
}

....

server.post('/service/:add/:object', createObject); // Calls createObject on routes that match this.

```


Jest to wzorzec na poziomie najbardziej podstawowym. Moduły restify (i Express) podaj znacznie bardziej złożone funkcje, takie jak Definiowanie typów aplikacji i zapewnianie złożonego routingu przez różne punkty końcowe. Dla naszych celów możemy utrzymują te trasy proste.

### <a name="add-default-routes-to-our-server"></a>Dodawanie tras domyślnych do serwera
Teraz możemy dodać podstawowe trasy CRUD tworzenie, pobieranie, aktualizacji i usunąć.

1. W wierszu polecenia przejdź do **azuread** folderu, jeśli nie masz już istnieje:

    `cd azuread`

2. Otwórz `server.js` plik w edytorze Ulubione, a następnie dodaj poniższe poprzednie wpisy w bazie danych, które należy podjąć następujące informacje:

```Javascript

/**
 *
 * APIs for our REST Task server.
 */

// Create a task.

function createTask(req, res, next) {

    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it, and save it to Mongodb.
    var _task = new Task();

    if (!req.params.task) {
        req.log.warn('createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.task = req.params.task;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}


// Delete a task by name.

function removeTask(req, res, next) {

    Task.remove({
        task: req.params.task,
        owner: owner
    }, function(err) {
        if (err) {
            req.log.warn(err,
                'removeTask: unable to delete %s',
                req.params.task);
            next(err);
        } else {
            log.info('Deleted task:', req.params.task);
            res.send(204);
            next();
        }
    });
}

// Delete all tasks.

function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
}


// Get a specific task based on name.

function getTask(req, res, next) {

    log.info('getTask was called for: ', owner);
    Task.find({
        owner: owner
    }, function(err, data) {
        if (err) {
            req.log.warn(err, 'get: unable to read %s', owner);
            next(err);
            return;
        }

        res.json(data);
    });

    return next();
}

/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Restify currently has a bug which doesn't allow you to set default headers.
    // These headers comply with CORS and allow us to mongodbServer our response to any origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Did you initialize the database as stated in the README?");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}

```

### <a name="add-error-handling-in-our-apis"></a>Dodawanie obsługi błędów w naszych interfejsów API
```

///--- Errors for communicating something interesting back to the client.

function MissingTaskError() {
    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'MissingTask',
        message: '"task" is a required parameter',
        constructorOpt: MissingTaskError
    });

    this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);


function TaskExistsError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'TaskExists',
        message: owner + ' already exists',
        constructorOpt: TaskExistsError
    });

    this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);


function TaskNotFoundError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 404,
        restCode: 'TaskNotFound',
        message: owner + ' was not found',
        constructorOpt: TaskNotFoundError
    });

    this.name = 'TaskNotFoundError';
}

util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="step-15-create-your-server"></a>Krok 15: Tworzenie serwera
Zdefiniowanych naszej bazie danych i naszych tras na miejscu. Ostatnim etapem jest dodanie wystąpienia serwera zarządzanego naszych wywołania.

Restify (i Express) można zrobić dużo dostosowania do serwera interfejsu API REST, ale ponownie zamierzamy naszych służącej do najbardziej podstawowa konfiguracja.

```Javascript
/**
 * Our server.
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads.
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());

// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in).
server.use(restify.requestLogger());

// Allow five requests per second by IP, and burst to 10.
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want.
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allow for JSON mapping to REST.
```

## <a name="step-16-add-the-routes-to-the-server-without-authentication-for-now"></a>Krok 16: Dodawanie tras do serwera (bez uwierzytelniania obecnie)
```Javascript
/// Now the real handlers. Here we just CRUD.
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'.
/* In the pasport.authenticate() method. We set 'session: false' because REST is stateless and
/* we don't need to maintain session state. You can experiment with removing API protection
/* by removing the passport.authenticate() method as follows:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler.
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="step-17-run-the-server-before-adding-oauth-support"></a>Kroku 17: Uruchom serwer (przed dodaniem obsługi protokołu OAuth)
Przetestowanie serwera przed dodamy uwierzytelniania.

Najprostszym sposobem przetestować serwer jest przy użyciu programu curl w wierszu polecenia. Zanim przejdziemy, potrzebujemy narzędzia, która umożliwia firmie Microsoft w celu analizy danych wyjściowych w formacie JSON.

1. Zainstaluj narzędzie JSON następujące (następujące przykłady narzędzie to):

    `$npm install -g jsontool`

    Narzędzie JSON zostanie zainstalowane globalnie. Teraz, gdy firma Microsoft już osiągnąć, który, teraz odtworzyć na serwerze:

2. Najpierw upewnij się, że wystąpienie bazy danych mongoDB jest uruchomione:

    `$sudo mongod`

3. Następnie przejdź do katalogu i uruchom curling:

    `$ cd azuread` `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 171
    Date: Tue, 14 Jul 2015 05:43:38 GMT
    [
    "GET /",
    "POST /tasks/:owner/:task",
    "POST /tasks (for JSON body)",
    "GET /tasks",
    "PUT /tasks/:owner",
    "GET /tasks/:owner",
    "DELETE /tasks/:owner/:task"
    ]
    ```

4. Następnie można dodać zadania w ten sposób:

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    Odpowiedź powinna wyglądać w następujący sposób:

        ```Shell
        HTTP/1.1 201 Created
        Connection: close
        Access-Control-Allow-Origin: *
        Access-Control-Allow-Headers: X-Requested-With
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 5
        Date: Tue, 04 Feb 2014 01:02:26 GMT
        Hello
        ```
    I można utworzyć listę zadań dla Brandon w ten sposób:

        `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Jeśli wszystko to działa, jesteśmy gotowi dodać protokół OAuth do serwera interfejsu API REST.

Masz serwera interfejsu API REST z bazy danych MongoDB.

## <a name="step-18-add-authentication-to-our-rest-api-server"></a>Krok 18: Dodawanie uwierzytelniania do serwera interfejsu API REST
Teraz, gdy będziemy mieć uruchomiony interfejs API REST, Zacznijmy przydatne z usługą Azure AD.

W wierszu polecenia przejdź do **azuread** folderu, jeśli nie masz już istnieje.

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Korzystanie ze strategii OIDCBearerStrategy dołączonej do modułu passport-azure-ad
Do tej pory nawiązaliśmy typowy serwer REST TODO bez żadnej autoryzacji. Jest to, gdzie Rozpoczniemy, który zestawienie.

1. Najpierw musimy wskazać chcemy korzystać z usługi Passport. To prawo należy umieścić po konfiguracji serwera:

    ```Javascript
            // Let's start using Passport.js.

            server.use(passport.initialize()); // Starts passport.
            server.use(passport.session()); // Provides session support.
    ```
    > [!TIP]
    > Podczas pisania interfejsów API, zaleca się, że możesz zawsze połączyć dane z unikatowym z tokenu, który użytkownik nie może się podszyć. Gdy ten serwer przechowuje elementy przechowuje je na podstawie Identyfikatora obiektu użytkownika w tokenie (wywoływanym za pośrednictwem token.oid), który testujemy w polu "właściciela". Daje to pewność, że tylko ten użytkownik ma dostęp do ich TODOs. Nie ma ekspozycji w interfejsie API "właściciela", dlatego użytkownikowi zewnętrznemu zażądanie TODOs innych nawet po dokonaniu uwierzytelnienia.                    

2. Następny Użyjmy strategii elementu nośnego dołączonej `passport-azure-ad`. Sprawdź kod teraz i wyjaśniamy pozostałe wkrótce. Umieścić po wkleić powyżej:

```Javascript
    /**
    /*
    /* Calling the OIDCBearerStrategy and managing users.
    /*
    /* Passport pattern provides the need to manage users and info tokens
    /* with a FindorCreate() method that must be provided by the implementor.
    /* Here we just auto-register any user and implement a FindById().
    /* You'll want to do something smarter.
    **/

    var findById = function(id, fn) {
        for (var i = 0, len = users.length; i < len; i++) {
            var user = users[i];
            if (user.sub === id) {
                log.info('Found user: ', user);
                return fn(null, user);
            }
        }
        return fn(null, null);
    };


    var bearerStrategy = new BearerStrategy(options,
        function(token, done) {
            log.info('verifying the user');
            log.info(token, 'was the token retreived');
            findById(token.sub, function(err, user) {
                if (err) {
                    return done(err);
                }
                if (!user) {
                    // "Auto-registration"
                    log.info('User was added automatically as they were new. Their sub is: ', token.sub);
                    users.push(token);
                    owner = token.sub;
                    return done(null, token);
                }
                owner = token.sub;
                return done(null, user, token);
            });
        }
    );

    passport.use(bearerStrategy);
```

Usługa Passport używa podobnego wzorca do wszystkich swoich strategii (Twitter, Facebook itd.) zgodne ze wszyscy twórcy kodu strategii. Spojrzenie na strategii, zobacz możemy przebiegu funkcję, która ma token i gotowe jako parametry. Strategia zostanie zwrócona do nas po jego działa. Po tak, możemy przechować dane użytkownika i Zapisz token, więc nie należy zażądać go ponownie.

> [!IMPORTANT]
> Poprzedni kod obejmuje wszystkich użytkowników, która do uwierzytelniania serwera. Jest to nazywane rejestracji automatycznej. W przypadku serwerów produkcyjnych, firma Microsoft zaleca, aby nie zezwolić każda osoba, która bez konieczności ich przejściu przez proces rejestracji, który podjęciu decyzji o. Jest to zwykle wzorzec, które są widoczne w aplikacjach komercyjnych, dzięki której można zarejestrować w usłudze Facebook, ale następnie wyświetli monit o podanie dodatkowych informacji. Jeśli to nie programu wiersza polecenia, być może wyodrębniono wiadomość e-mail z obiektu tokena, który został zwrócony, a następnie monit użytkownika o podanie dodatkowych informacji. Ze względu na to, że jest to serwer testowy, informacje zostają po prostu dodane do bazy danych w pamięci.
>
>

### <a name="protect-some-endpoints"></a>Ochrona niektórych punktów końcowych
Ochrona punktów końcowych, określając `passport.authenticate()` wywołania z protokołem, którego chcesz używać.

Aby można było naszego kodu serwera wykonać ciekawsze, umożliwia edytowanie trasy.

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="step-19-run-your-server-application-again-and-ensure-it-rejects-you"></a>Krok 19: Uruchom ponownie aplikację serwera i upewnij się, że użytkownik jest odrzucany
Użyjmy `curl` ponownie, aby zobaczyć, czy obecnie istnieją ochrony OAuth2 względem naszego punktów końcowych. Możemy przeprowadzić ten test przed uruchomieniem żadnego z zestawów SDK klientów przed tym punktem końcowym. Zwrócone nagłówki powinny wystarczyć Poinformuj nas, jeśli chcemy dół prawidłową ścieżkę.

1. Najpierw upewnij się, że wystąpienie bazy danych mongoDB jest uruchomione:

    `$sudo mongod`

2. Następnie przejdź do katalogu i uruchom curling.

      `$ cd azuread` `$ node server.js`

3. Spróbuj podstawową procedurę testową POST.

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

401 jest odpowiedź, którego szukasz tutaj. Odpowiedź wskazuje, że warstwa oprogramowania Passport podejmuje próbę przekierowania do autoryzowanych punktu końcowego, który ma dokładnie.

## <a name="next-steps"></a>Następne kroki
Został usunięty w zakresie, w jakim można z tego serwera bez użycia klienta zgodne OAuth2. Należy przeprowadzić dodatkowe wskazówki.

Teraz znasz implementowania interfejsu API REST przy użyciu modułu Restify i protokołu OAuth2. Masz również wystarczająco kod, aby zachować opracowywania usługi i poznanie kompilacji w tym przykładzie.

Jeśli interesuje Cię w następnych krokach w podróży ADAL, poniżej przedstawiono niektóre, firma Microsoft zaleca, aby kontynuować pracę z obsługiwanych klientów biblioteki ADAL.

Sklonuj dół komputerze dewelopera i skonfigurować zgodnie z opisem w tym przewodnikiem.

[Biblioteka ADAL dla systemu iOS](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[ADAL dla systemu Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
