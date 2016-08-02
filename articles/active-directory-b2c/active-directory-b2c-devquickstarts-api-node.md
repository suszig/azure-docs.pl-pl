<properties
    pageTitle="Wersja zapoznawcza usługi B2C: zabezpieczanie interfejsu API sieci Web przy użyciu środowiska Node.js | Microsoft Azure"
    description="Jak tworzyć interfejs API sieci Web w środowisku Node.js, który akceptuje tokeny od dzierżawcy usługi B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="msmbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="brandwe"/>

# Wersja zapoznawcza usługi B2C: zabezpieczanie interfejsu API sieci Web przy użyciu środowiska Node.js

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]


> [AZURE.NOTE] W tym artykule nie opisano sposobu wdrażania logowania, rejestracji i zarządzania profilami za pomocą usługi Azure AD B2C. Dotyczy on w szczególności wywoływania interfejsów API sieci Web po uwierzytelnieniu użytkownika. Należy rozpocząć od [ samouczka ułatwiającego rozpoczęcie pracy z aplikacjami sieci Web platformy .NET](active-directory-b2c-devquickstarts-web-dotnet.md), aby zapoznać się z podstawami usługi Azure Active Directory B2C, jeśli nie zostało to jeszcze wykonane.


> [AZURE.NOTE]  Ta próbka została zapisana w taki sposób, aby podłączanie do niej odbywało się przy użyciu [przykładowej aplikacji B2C systemu iOS](active-directory-b2c-devquickstarts-ios.md). Należy najpierw zapoznać się z tym przewodnikiem, a następnie kontynuować pracę z próbką.

**Passport** to uwierzytelniające oprogramowanie pośredniczące dla środowiska Node.js. Jest to niezwykle elastyczne i modułowe oprogramowanie, które można dyskretnie zainstalować w dowolnej aplikacji sieci Web opartej na module Express lub Restify. Kompleksowy zestaw strategii obsługuje uwierzytelnianie przy użyciu m.in. nazwy użytkownika i hasła lub kont w serwisach Facebook i Twitter. Opracowaliśmy strategię dla usługi Azure Active Directory (Azure AD). Polega ona na zainstalowaniu tego modułu, a następnie dodaniu wtyczki `passport-azure-ad` usługi Azure AD.

W tym celu należy:

1. Zarejestrować aplikację w usłudze Azure AD.
2. Skonfigurować aplikację do korzystania z wtyczki `azure-ad-passport` oprogramowania Passport.
3. Skonfigurować aplikację klienta do wywołania interfejsu API sieci Web listy zadań do wykonania.

Kod używany w tym samouczku [jest przechowywany w serwisie GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs). Aby z niego skorzystać, można [pobrać szkielet aplikacji w formie pliku .zip](https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs/archive/skeleton.zip) lub sklonować szkielet:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs.git```

Ukończona aplikacja znajduje się na końcu tego samouczka.

> [AZURE.WARNING]   W przypadku wersji zapoznawczej usługi B2C należy używać tego samego **identyfikatora klienta**/**identyfikatora aplikacji** i zasad zarówno w przypadku serwera zadań interfejsu API sieci Web, jak i klienta, który nawiązuje z nim połączenie. Dotyczy to również samouczków dotyczących systemów iOS i Android. Jeśli uprzednio utworzono aplikację w jednym z tych skróconych podręczników, można użyć tych wartości, zamiast tworzyć nowe.


## Tworzenie katalogu usługi Azure AD B2C

Przed rozpoczęciem korzystania z usługi Azure AD B2C należy utworzyć katalog lub dzierżawę.  Katalog jest kontenerem dla wszystkich użytkowników, aplikacji, grup i innych elementów.  Jeśli jeszcze go nie masz, [utwórz katalog usługi B2C](active-directory-b2c-get-started.md), zanim przejdziesz dalej.

## Tworzenie aplikacji

Następnie należy utworzyć aplikację w katalogu B2C, która dostarcza do usługi Azure AD pewne informacje, które są potrzebne w celu bezpiecznego komunikowania się z aplikacją. W takim przypadku zarówno aplikacja klienta, jak i interfejs API sieci Web będą reprezentowane przez jeden **Identyfikator aplikacji**, ponieważ stanowią jedną aplikację logiczną. Aby utworzyć aplikację, postępuj zgodnie z [tymi instrukcjami](active-directory-b2c-app-registration.md). Należy pamiętać o wykonaniu następujących czynności:

- Dołącz do aplikacji **aplikację sieci Web/interfejs API sieci Web**.
- Wprowadź `http://localhost/TodoListService` w polu **Adres URL odpowiedzi**. Jest to domyślny adres URL dla tej próbki kodu.
- Utwórz **klucz tajny aplikacji** i skopiuj go. Będzie potrzebny później. Będziesz potrzebować go za chwilę. Pamiętaj, że aby go użyć, jego wartość musi być [ujęta w kodzie XML w znaki ucieczki](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape).
- Skopiuj **identyfikator aplikacji** przypisany do aplikacji. On również będzie później potrzebny.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Tworzenie zasad

W usłudze Azure AD B2C każde działanie użytkownika jest definiowane przy użyciu [zasad](active-directory-b2c-reference-policies.md). Ta aplikacja obejmuje trzy operacje dotyczące tożsamości: rejestrację, logowanie i logowanie przy użyciu konta serwisu Facebook. Dla każdego typu należy utworzyć jeden zbiór zasad zgodnie z opisem w [artykule o zasadach](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy).  Podczas tworzenia trzech zbiorów zasad należy koniecznie:

- Wybrać wartość **Nazwa wyświetlana** i inne atrybuty rejestracji w ramach zasad rejestracji.
- Wybrać oświadczenia aplikacji **Nazwa wyświetlana** oraz **Identyfikator obiektu** we wszystkich zasadach.  Można również wybrać inne oświadczenia.
- Skopiować wartość **Nazwa** poszczególnych zasad po ich utworzeniu. Powinny zawierać prefiks `b2c_1_`.  Te nazwy zasad będą potrzebne później.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po utworzeniu trzech zbiorów zasad można rozpocząć tworzenie aplikacji.

Należy pamiętać, że w tym artykule nie opisano sposobu korzystania z nowo utworzonych zasad. Aby dowiedzieć się, jak działają zasady w usłudze Azure AD B2C, należy najpierw zapoznać się z [samouczkiem ułatwiającym rozpoczęcie pracy z aplikacją sieci Web platformy .NET](active-directory-b2c-devquickstarts-web-dotnet.md).

## Pobieranie środowiska Node.js dla platformy

Aby pomyślnie korzystać z tej próbki, należy dysponować działającą instalacją środowiska Node.js.

Zainstaluj środowisko Node.js z witryny [nodejs.org](http://nodejs.org).

## Instalowanie bazy danych MongoDB dla platformy

Aby pomyślnie korzystać z tej próbki, należy także dysponować działającą instalacją bazy danych MongoDB. Baza danych MongoDB zostanie użyta w celu uzyskania trwałego interfejsu API REST w wystąpieniach serwera.

Zainstaluj bazę danych MongoDB z witryny [mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] W tym przewodniku założono, że zostanie użyta domyślna instalacja i domyślne punkty końcowe serwera dla bazy danych MongoDB, które aktualnie mają wartość `mongodb://localhost`.

## Instalowanie modułów Restify w interfejsie API sieci Web

Moduł Restify służy do tworzenia interfejsu API REST. Restify jest minimalną i elastyczną strukturą aplikacji w środowisku Node.js, która pochodzi z modułu Express. Oferuje rozbudowany zestaw funkcji do tworzenia interfejsów API REST na bazie usługi Connect.

### Instalacja modułu Restify

W wierszu polecenia zmień katalog na `azuread`. Jeśli katalog `azuread` nie istnieje, utwórz go.

`cd azuread` lub `mkdir azuread;`

Wprowadź następujące polecenie:

`npm install restify`

To polecenie powoduje zainstalowanie modułu Restify.

#### Czy został wyświetlony komunikat o błędzie?

W niektórych systemach operacyjnych użycie ciągu `npm` może spowodować wystąpienie błędu `Error: EPERM, chmod '/usr/local/bin/..'` oraz pojawienie się żądania uruchomienia konta z uprawnieniami administratora. W takim przypadku należy użyć polecenia `sudo`, aby uruchomić polecenie `npm` na wyższym poziomie uprawnień.

#### Czy został wyświetlony błąd narzędzia DTrace?

Po zainstalowaniu modułu Restify mogą zostać wyświetlone następujące informacje:

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

Moduł Restify zapewnia zaawansowany mechanizm śledzenia wywołań REST przy użyciu narzędzia DTrace. Jednak w wielu systemach operacyjnych narzędzie DTrace jest niedostępne. Można te błędy bezpiecznie zignorować.

Dane wyjściowe polecenia powinny wyglądać podobnie jak poniżej:

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

## Instalacja oprogramowania Passport w interfejsie API sieci Web

[Passport](http://passportjs.org/) to uwierzytelniające oprogramowanie pośredniczące dla środowiska Node.js. Jest to niezwykle elastyczne i modułowe oprogramowanie, które można dyskretnie zainstalować w dowolnej aplikacji sieci Web opartej na module Express lub Restify. Kompleksowy zestaw strategii obsługuje uwierzytelnianie przy użyciu m.in. nazwy użytkownika i hasła lub kont w serwisach Facebook i Twitter. Opracowaliśmy strategię dla usługi Azure AD. Polega ona na zainstalowaniu tego modułu, a następnie dodaniu wtyczki usługi Azure AD.

W wierszu polecenia zmień katalog na `azuread`, jeśli nie jest jeszcze określony.

Wprowadź poniższe polecenie, aby zainstalować oprogramowanie Passport:

`npm install passport`

Dane wyjściowe polecenia powinny wyglądać podobnie jak poniżej:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## Dodawanie biblioteki passport-azuread do interfejsu API interfejsu sieci Web

Dodaj strategię OAuth przy użyciu biblioteki `passport-azuread` — zestawu strategii łączących usługę Azure AD z oprogramowaniem Passport. Użyj tej strategii w odniesieniu do tokenów elementów nośnych w próbce interfejsu API REST.

> [AZURE.NOTE] Chociaż protokół OAuth2 zapewnia platformę umożliwiającą wystawienie dowolnego znanego typu tokenu, tylko niektóre typy tokenów weszły do powszechnego użycia. Tokeny zabezpieczające punkty końcowe są tokenami elementów nośnych. Jest to najczęściej wystawiany typ tokenu protokołu OAuth2. W wielu wdrożeniach zakłada się, że tokeny elementów nośnych są jedynym typem wystawianych tokenów.

W wierszu polecenia zmień katalog na `azuread`, jeśli nie jest jeszcze określony.

Wprowadź następujące polecenie, aby zainstalować moduł `passport-azure-ad` oprogramowania Passport:

`npm install passport-azure-ad`

Dane wyjściowe polecenia powinny wyglądać podobnie jak poniżej:

``
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
``

## Dodawanie modułów MongoDB do interfejsu API sieci Web

Moduł MongoDB będzie używany jako magazyn danych. Z tego powodu należy zainstalować zarówno powszechnie używaną wtyczkę Mongoose do zarządzania modelami i schematami, jak i sterownik bazy danych dla modułu MongoDB, również noszący nazwę MongoDB.

* `npm install mongoose`
* `npm install mongodb`

## Instalowanie dodatkowych modułów

Następnie zainstaluj pozostałe wymagane moduły.

W wierszu polecenia zmień katalog na `azuread`, jeśli nie jest jeszcze określony:

`cd azuread`

Wprowadź następujące polecenia, aby zainstalować moduły w katalogu `node_modules`:

* `npm install crypto`
* `npm install assert-plus`
* `npm install posix-getopt`
* `npm install util`
* `npm install path`
* `npm install connect`
* `npm install xml-crypto`
* `npm install xml2js`
* `npm install xmldom`
* `npm install async`
* `npm install request`
* `npm install underscore`
* `npm install grunt-contrib-jshint@0.1.1`
* `npm install grunt-contrib-nodeunit@0.1.2`
* `npm install grunt-contrib-watch@0.2.0`
* `npm install grunt@0.4.1`
* `npm install xtend@2.0.3`
* `npm install bunyan`
* `npm update`


## Tworzenie pliku server.js z własnymi zależnościami

Plik `server.js` zapewni obsługę większości funkcji serwera interfejsu API sieci Web. Do tego pliku należy dodać większość naszego kodu. Do celów produkcyjnych należy refaktoryzować funkcje na mniejsze pliki, np. odrębne trasy i kontrolery. Na potrzeby tego samouczka do tego celu zostanie użyty plik server.js.

W wierszu polecenia zmień katalog na `azuread`, jeśli nie jest jeszcze określony:

`cd azuread`

Utwórz plik `server.js` w edytorze. Dodaj następujące informacje:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Zapisz plik. Powrócisz do niego później.

## Tworzenie pliku config.js do przechowywania ustawień usługi Azure AD

Ten plik kodu przekazuje parametry konfiguracji z portalu usługi Azure AD do pliku `Passport.js`. Te wartości konfiguracji zostały utworzone po dodaniu interfejsu API sieci Web do portalu w pierwszej części przewodnika. Wyjaśnijmy, co należy umieścić w wartościach tych parametrów po skopiowaniu kodu.

W wierszu polecenia zmień katalog na `azuread`, jeśli nie jest jeszcze określony:

`cd azuread`

Utwórz plik `config.js` w edytorze. Dodaj następujące informacje:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>',
identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration', // For using Microsoft you should never need to change this.
tenantName:'<tenant name>',
policyName:'b2c_1_<sign in policy name>',
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### Wymagane wartości

`IdentityMetadata`: w tym miejscu moduł `passport-azure-ad` będzie szukać danych konfiguracji dostawcy tożsamości. Tutaj również będą wyszukiwane klucze do sprawdzania poprawności tokenów sieci Web JSON. Jeśli używasz usługi Azure AD, prawdopodobnie nie musisz zmieniać tego ustawienia.

`audience`: jednolity identyfikator zasobów (URI) z portalu, który identyfikuje usługę. W przypadku naszej próbki jest to `http://localhost/TodoListService`.

`tenantName`: nazwa dzierżawy (na przykład **contoso.onmicrosoft.com**).

`policyName`: zasady, które mają być używane do weryfikacji tokenów przychodzących na serwerze. Powinny to być te same zasady, które są używane do logowania w aplikacji klienta.

> [AZURE.NOTE] W wersji zapoznawczej usługi B2C należy używać tych samych zasad w konfiguracji klienta i serwera. Jeśli zasady zostały już utworzone w ramach tego przewodnika, nie trzeba tego robić ponownie. Ponieważ zadania w tym przewodniku zostały wykonane, nie ma potrzeby konfigurowania nowych zasad w ramach przewodników klienta w witrynie.

## Dodawanie konfiguracji do pliku server.js

Należy odczytać wartości z nowo utworzonego pliku `config.js` w aplikacji. W tym celu dodaj plik `.config` jako wymagany zasób w aplikacji, a następnie ustaw zmienne globalne na zmienne z dokumentu `config.js`.

W wierszu polecenia zmień katalog na `azuread`, jeśli nie jest jeszcze określony:

`cd azuread`

Otwórz plik `server.js` w edytorze. Dodaj następujące informacje:

```Javascript
var config = require('./config');
```
Dodaj do pliku `server.js` nową sekcję zawierającą następujący kod:

```Javascript
// We pass these options in to the ODICBearerStrategy.
var options = {
// The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience
};
// array to hold logged-in users and the current logged-in user (owner)
var users = [];
var owner = null;
// Our logger
var log = bunyan.createLogger({
name: 'Microsoft Azure Active Directory Sample'
});
```

## Dodawanie informacji o modelu i schemacie modułu MongoDB przy użyciu wtyczki Mongoose

Wcześniejsze przygotowania będą przydatne podczas łączenia tych trzech plików w usłudze interfejsu API REST.

Na potrzeby tego przewodnika do przechowywania zadań użyj bazy danych MongoDB, zgodnie z wcześniejszym opisem.

W utworzonym pliku `config.js` została wywołana **lista zadań** bazy danych. Została ona również umieszczona na końcu adresu URL połączenia `mongoose_auth_local`. Nie trzeba tworzyć wcześniej tej bazy danych w module MongoDB. Zostanie ona utworzona przy pierwszym uruchomieniu aplikacji serwera, jeśli jeszcze nie istnieje.

Po przekazaniu do serwera informacji, która baza danych MongoDB ma być używana, należy napisać dodatkowy kod w celu utworzenia modelu i schematu dla zadań serwera.

### Rozszerzanie modelu

Ten model schematu jest bardzo prosty. Można go rozszerzyć w zależności od potrzeb.

`name`: kto jest przydzielony do zadania. Jest to **ciąg**.

`task`: samo zadanie. Jest to **ciąg**.

`date`: termin ukończenia zadania. Jest to **data/godzina**.

`completed`: czy zadanie zostało ukończone, czy nie. Jest to **wartość logiczna**.

### Tworzenie schematu w kodzie

W wierszu polecenia zmień katalog na `azuread`, jeśli nie jest jeszcze określony:

`cd azuread`

Otwórz plik `server.js` w edytorze. Dodaj następujące informacje poniżej pozycji konfiguracji:

```Javascript
// MongoDB setup
// Set up some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');
```
Spowoduje to połączenie z serwerem MongoDB i ponownie przekazanie obiektu schematu.

### Użycie schematu w celu utworzenia modelu w kodzie

Pod kodem napisanym powyżej dodaj następujący kod:

```Javascript
// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
owner: String,
task: String,
completed: Boolean,
date: Date
});
// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Najpierw tworzony jest schemat, a następnie obiekt modelu używany do przechowywania danych w całym kodzie podczas definiowania **tras**.

## Dodawanie tras do serwera zadań interfejsu API REST

Dysponując modelem bazy danych, możesz dodać trasy, które będą używane na potrzeby serwera interfejsu API REST.

### Informacje o trasach w module Restify

Trasy działają w module Restify dokładnie w taki sam sposób, jak podczas korzystania ze stosu modułu Express. Trasy są definiowane za pomocą identyfikatora URI, który ma być wywoływany przez aplikacje klienta. W większości przypadków należy zdefiniować trasy w oddzielnym pliku. Na potrzeby tego samouczka należy umieścić trasy w pliku `server.js`. W przypadku środowiska produkcyjnego zaleca się ich faktorowanie do oddzielnego pliku.

Oto typowy wzorzec trasy modułu Restify:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep the server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

Jest to wzorzec na poziomie najbardziej podstawowym. Moduły Restify i Express mogą zapewniać o wiele bardziej złożone funkcje, takie jak definiowanie typów aplikacji i wykonywanie złożonego routingu przez różne punkty końcowe. W tym samouczku użyte zostaną proste trasy.

#### Dodawanie tras domyślnych do serwera

Teraz należy dodać podstawowe trasy CRUD **tworzenia**, **pobierania**, **aktualizowania** i **usuwania**.

W wierszu polecenia zmień katalog na `azuread`, jeśli nie jest jeszcze określony:

`cd azuread`

Otwórz plik `server.js` w edytorze. Dodaj poniższe informacje poniżej pozycji bazy danych utworzonych powyżej:

```Javascript
/**
*
* APIs for our REST task server
*/
// Create a task
function createTask(req, res, next) {
// Restify currently has a bug that doesn't allow you to set default headers
// The headers comply with CORS and allow us to mongodbServer our response to any origin
res.header("Access-Control-Allow-Origin", "*");
res.header("Access-Control-Allow-Headers", "X-Requested-With");
// Create a new task model, fill it up and save it to Mongodb
var _task = new Task();
if (!req.params.task) {
req.log.warn({
params: p
}, 'createTodo: missing task');
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
// Delete a task by name
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
// Delete all tasks
function removeAll(req, res, next) {
Task.remove();
res.send(204);
return next();
}
// Get a specific task based on name
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
// Restify currently has a bug that doesn't allow you to set default headers
// The headers comply with CORS and allow us to mongodbServer our response to any origin
res.header("Access-Control-Allow-Origin", "*");
res.header("Access-Control-Allow-Headers", "X-Requested-With");
log.info("listTasks was called for: ", owner);
Task.find({
owner: owner
}).limit(20).sort('date').exec(function(err, data) {
if (err)
return next(err);
if (data.length > 0) {
log.info(data);
}
if (!data.length) {
log.warn(err, "There is no tasks in the database. Add one!");
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

#### Dodawanie obsługi błędów dla tras

Należy dodać obsługę błędów, co umożliwi przekazanie informacji o napotkanych błędach z powrotem do klienta w zrozumiały dla niego sposób.

Dodaj następujący kod poniżej kodu zapisanego powyżej:

```Javascript
///--- Errors for communicating something interesting back to the client
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


## Tworzenie serwera

Baza danych została zdefiniowana, a trasy — określone. Ostatnim zadaniem, które należy wykonać, jest dodanie wystąpienia serwera, które będzie zarządzać wywołaniami.

Moduły Restify i Express oferują możliwość głębokiego dostosowania do serwera interfejsu API REST, ale w tym przypadku zostanie zastosowana najbardziej podstawowa konfiguracja.

```Javascript
/**
* Our server
*/
var server = restify.createServer({
name: "Microsoft Azure Active Directory TODO Server",
version: "2.0.1"
});
// Ensure that we don't drop data on uploads
server.pre(restify.pre.pause());
// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());
// Handle annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());
// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());
// Allow five requests/second by IP, and burst to 10
server.use(restify.throttle({
burst: 10,
rate: 5,
ip: true,
}));
// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
mapParams: true
}));
```
## Dodawanie tras do serwera (bez uwierzytelniania)

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the passport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiment with removing API protection
/* by removing the passport.authenticate() method like this:
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
// Register a default '/' handler
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
## Uruchamianie serwera przed dodaniem obsługi protokołu OAuth

Przed dodaniem uwierzytelniania należy przetestować serwer.

Najprościej jest to zrobić, wpisując polecenie `curl` w wierszu polecenia. Jednak najpierw potrzebne jest proste narzędzie umożliwiające analizę danych wyjściowych w formacie JSON. Najpierw zainstaluj narzędzie JSON.

`$npm install -g jsontool`

Narzędzie JSON zostanie zainstalowane globalnie. Po zainstalowaniu narzędzia JSON przetestuj serwer:

Upewnij się, że wystąpienie bazy danych MongoDB zostało uruchomione.

`$sudo mongodb`

Zmień katalog na `azuread` i użyj polecenia `curl`.

`$ cd azuread`
`$ node server.js`

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

Dodaj zadanie:

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
Zadania dla użytkownika „Brandon” można przedstawić w następujący sposób:

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Jeśli wynik działania jest prawidłowy, można dodać protokół OAuth do serwera API REST.

Dysponujesz teraz serwerem interfejsu API REST z bazą danych MongoDB.

## Dodawanie uwierzytelniania do serwera interfejsu API REST

Działający serwer interfejsu API REST można wykorzystać do celów usługi Azure AD.

W wierszu polecenia zmień katalog na `azuread`, jeśli nie jest jeszcze określony:

`cd azuread`

### Korzystanie ze strategii OIDCBearerStrategy dołączonej do modułu passport-azure-ad

Jak dotąd utworzony został typowy serwer REST listy zadań do wykonania bez żadnej autoryzacji. Teraz można rozpocząć proces autoryzacji.

Najpierw należy wskazać, że ma być używane oprogramowanie Passport. Dodaj następujący kod bezpośrednio pod pozostałą konfiguracją serwera:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts Passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
Podczas pisania interfejsów API należy zawsze połączyć dane z unikatowym elementem tokenu, pod który użytkownik nie może się podszyć. Jeśli serwer przechowuje elementy listy zadań do wykonania, odbywa się to w oparciu o **identyfikator obiektu** użytkownika w tokenie (wywoływanym za pośrednictwem token.oid), który znajduje się w polu „właściciela”. Dzięki temu tylko użytkownik może uzyskać dostęp do swoich zadań do wykonania i nikt inny nie ma dostępu do wprowadzonych zadań do wykonania. W interfejsie API nie ma ekspozycji „właściciela” umożliwiającej użytkownikowi zewnętrznemu zażądanie zadań do wykonania innych osób, nawet po dokonaniu uwierzytelnienia.

Następnie użyj strategii elementu nośnego dołączonej do modułu `passport-azure-ad`. (Na razie przyjrzyjmy się tylko kodowi). Po kodzie przedstawionym powyżej należy umieścić następujące polecenia:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementer.
/* Here we just autoregister any user and implement a FindById().
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
var oidcStrategy = new OIDCBearerStrategy(options,
function(token, done) {
log.info('verifying the user');
log.info(token, 'was the token retrieved');
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
passport.use(oidcStrategy);
```

Oprogramowanie Passport w przypadku wszystkich swoich strategii (w tym serwisów Facebook i Twitter) korzysta ze wzorca, który jest podobny do strategii stosowanych przez wszystkich autorów. Zawiera on polecenie `function()` z parametrami `token` i `done`. Strategia zostanie zwrócona po wykonaniu całego zadania. Następnie należy przechować dane użytkownika i zapisać token, aby nie trzeba było żądać go ponownie.

> [AZURE.IMPORTANT]
Powyższy kod obejmuje wszystkich użytkowników, którzy są uwierzytelniani na serwerze. Ta czynność nazywana jest autorejestracją. W przypadku serwerów produkcyjnych nie chcemy akceptować użytkowników, jeśli nie przejdą oni najpierw przez ustalony proces rejestracji. Zwykle stosowany jest wzorzec występujący w aplikacjach komercyjnych, który zezwala na rejestrację przy użyciu konta w serwisie Facebook, po czym wyświetlony zostaje monit o podanie dodatkowych informacji. Jeśli nie użyto programu wiersza polecenia, być może wyodrębniono wiadomość e-mail z obiektu tokena, który został zwrócony, a następnie użytkownicy zostali poproszeni o podanie dodatkowych informacji. Ze względu na to, że jest to serwer testowy, informacje zostają po prostu dodane do bazy danych w pamięci.

### Ochrona punktów końcowych

Punkty końcowe można chronić po określeniu wywołania `passport.authenticate()` przy użyciu protokołu, którego chcemy użyć.

Można edytować trasę w kodzie serwera, aby wykonać ciekawsze zadania:

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

## Ponowne uruchamianie aplikacji serwera w celu sprawdzenia, czy użytkownik jest odrzucany

Można ponownie użyć polecenia `curl`, aby sprawdzić, czy ochrona punktów końcowych przy użyciu protokołu OAuth2 jest aktualnie włączona. Należy to zrobić przed uruchomieniem dowolnych zestawów SDK klientów w odniesieniu do danego punktu końcowego. Zwrócone nagłówki powinny wystarczyć do ustalenia, czy działanie zostało wykonane prawidłowo.

Upewnij się, że wystąpienie bazy danych MongoDB zostało uruchomione:

    $sudo mongodb

Przejdź do katalogu i użyj polecenia `curl`:

    $ cd azuread
    $ node server.js

Wypróbuj podstawową procedurę testową POST:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

Oczekiwaną odpowiedzią jest błąd 401. Oznacza on, że warstwa oprogramowania Passport podejmuje próbę przekierowania do punktu końcowego autoryzacji.


## Efektem jest usługa interfejsu API REST korzystająca z protokołu OAuth2

W przypadku tego serwera nie można wykonać żadnych dalszych czynności bez użycia klienta zgodnego z protokołem OAuth2. Do tego celu potrzebny jest dodatkowy przewodnik.

Jeśli szukasz jedynie informacji na temat wdrażania interfejsu API REST przy użyciu modułu Restify i protokołu OAuth2, przedstawiony kod jest wystarczający do kontynuowania opracowywania usługi i kompilowania kodu na jego podstawie.

Gotowa próbka (bez wartości konfiguracji) [jest dostępna w pliku .zip](https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs/archive/complete.zip). Można ją także sklonować z serwisu GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebAPI-nodejs.git```


## Następne kroki

Możesz teraz przejść do bardziej zaawansowanych tematów, takich jak:

[Podłączanie do interfejsu API sieci Web przy użyciu systemu iOS z usługą B2C](active-directory-b2c-devquickstarts-ios.md)



<!--HONumber=Jun16_HO2-->


