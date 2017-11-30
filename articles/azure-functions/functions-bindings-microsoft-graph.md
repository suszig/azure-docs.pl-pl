---
title: "Powiązania funkcji Microsoft Graph platformy Azure | Dokumentacja firmy Microsoft"
description: "Zrozumienie, jak używać programu Microsoft Graph wyzwalaczy i powiązań w usługi Azure Functions."
services: functions
author: mattchenderson
manager: cfowler
editor: 
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 09/19/2017
ms.author: mahender
ms.openlocfilehash: dd7bcd57260b9763eabb9b4c915d9ff46e79e931
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="azure-functions-microsoft-graph-bindings"></a>Powiązania funkcji Microsoft Graph platformy Azure

W tym artykule opisano sposób konfigurowania i pracować z programu Microsoft Graph wyzwalaczy i powiązań w funkcji platformy Azure.
Z tych opcji, możesz użyć funkcji Azure do pracy z danymi, szczegółowych informacji i zdarzeń z [Microsoft Graph](https://graph.microsoft.io).

Rozszerzenia Microsoft Graph zapewnia następujące powiązania:
- [Powiązania danych wejściowych token uwierzytelniania](#token-input) pozwala korzystać z interfejsu API programu Graph żadnych firmy Microsoft.
- [Powiązania danych wejściowych tabelę programu Excel](#excel-input) umożliwia odczytywanie danych z programu Excel.
- [Powiązania wyjściowego tabelę programu Excel](#excel-output) umożliwia modyfikowanie danych programu Excel.
- [Powiązania danych wejściowych plików usługi OneDrive](#onedrive-input) umożliwia odczytanie plików z poziomu usługi OneDrive.
- [Powiązania wyjściowego pliku OneDrive](#onedrive-output) umożliwia zapisywanie do plików w usłudze OneDrive.
- [Powiązania wyjściowego wiadomości programu Outlook](#outlook-output) pozwala na wysyłanie wiadomości e-mail za pomocą programu Outlook.
- Kolekcja [Microsoft Graph elementu webhook wyzwalaczy i powiązań](#webhooks) służy do reagowania na zdarzenia z programu Microsoft Graph.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!Note] 
> Program Microsoft Graph powiązania są obecnie w wersji zapoznawczej.

## <a name="setting-up-the-extensions"></a>Konfigurowanie rozszerzeń

Program Microsoft Graph powiązania są dostępne za pośrednictwem _powiązanie rozszerzenia_. Rozszerzenia powiązania są opcjonalne składniki środowiska uruchomieniowego usługi Azure Functions. W tej sekcji opisano, jak skonfigurować program Microsoft Graph i rozszerzenia tokenu uwierzytelniania.

### <a name="enabling-functions-20-preview"></a>Włączanie funkcji 2.0 w wersji zapoznawczej

Rozszerzenia powiązania tylko są dostępne tylko dla 2.0 funkcji Azure w wersji zapoznawczej. 

[!INCLUDE [functions-set-runtime-version](../../includes/functions-set-runtime-version.md)]

Aby dowiedzieć się więcej, zobacz [jak korzystać z wersji środowiska uruchomieniowego usługi Azure Functions](functions-versions.md).

### <a name="installing-the-extension"></a>Instalowanie rozszerzenia

Aby zainstalować rozszerzenie z portalu Azure, należy przejść do szablonu lub powiązanie, która odwołuje się. Utwórz nową funkcję, a w ekranu wyboru szablonu, wybierz scenariusz "Microsoft Graph". Wybierz jeden z szablonów z tego scenariusza. Można też, przejdź do karty "Integracji" istniejącej funkcji i wybierz jedno z powiązań opisanych w tym temacie.

W obu przypadkach zostanie wyświetlone ostrzeżenie, który określa rozszerzenia do zainstalowania. Kliknij przycisk **zainstalować** uzyskać rozszerzenia.

> [!Note] 
> Każde rozszerzenie tylko musi być zainstalowany po każdej funkcji aplikacji. Plan przez proces instalacji w portalu może zająć do 10 minut.

Jeśli używasz programu Visual Studio rozszerzeń można uzyskać przez zainstalowanie tych pakietów NuGet:
- [Microsoft.Azure.WebJobs.Extensions.AuthTokens](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.AuthTokens/)
- [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/)

### <a name="configuring-app-service-authentication--authorization"></a>Konfigurowanie aplikacji usługi uwierzytelniania / autoryzacji

Powiązania opisane w tym temacie wymagają tożsamości do użycia. Dzięki temu program Microsoft Graph do wymuszenia uprawnień i inspekcji interakcji. Tożsamość może być dostęp do aplikacji lub aplikacji użytkownika. Aby skonfigurować tożsamość, musisz skonfigurować [aplikacji usługi uwierzytelniania / autoryzacji](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) usłudze Azure Active Directory. Należy również żądania żadnych uprawnień zasobów, które wymagają funkcji.

> [!Note] 
> Rozszerzenia Microsoft Graph obsługuje tylko uwierzytelniania w usłudze AAD. Użytkownicy muszą przeprowadzić logowania za pomocą konta służbowego.

Jeśli przy użyciu portalu Azure, poniżej wiersza, aby zainstalować to rozszerzenie zostanie wyświetlone ostrzeżenie, którym można skonfigurować uwierzytelniania usługi aplikacji / autoryzacji i żądanie wymaga uprawnień powiązania lub szablonu. Kliknij przycisk **skonfigurować AAD** lub **teraz dodać uprawnienia** odpowiednio.






<a name="token-input"></a>
## <a name="auth-token-input-binding"></a>Token powiązania wejściowego uwierzytelniania

To powiązanie pobiera token AAD zasobu i udostępnia go do kodu jako ciąg. Zasób może to być dowolna, dla którego aplikacji ma uprawnienia. 

### <a name="configuring-an-auth-token-input-binding"></a>Konfigurowanie uwierzytelniania tokenu powiązania wejściowego

Powiązanie sam nie wymaga żadnych uprawnień usługi AAD, ale w zależności od tego, jak jest używany, konieczne może być zażądania dodatkowych uprawnień. Sprawdź wymagania zasób, do którego chcesz uzyskać dostęp z tokenem.

Wiązanie obsługuje następujące właściwości:

|Właściwość|Opis|
|--------|--------|
|**Nazwa**|Wymagana — nazwa zmiennej używane w kodzie funkcja tokenu uwierzytelniania. Zobacz [przy użyciu tokenu uwierzytelniania wejściowych powiązania z kodu](#token-input-code).|
|**Typ**|Wymagana — musi być ustawiona `token`.|
|**Kierunek**|Wymagana — musi być ustawiona `in`.|
|**tożsamości**|Wymagana — tożsamości, która będzie służyć do wykonania akcji. Może to być jedna z następujących wartości:<ul><li><code>userFromRequest</code>— Jedyne prawidłowe z [wyzwalacza HTTP]. Używa tożsamości użytkownika wywołującego.</li><li><code>userFromId</code>-Używa tożsamości wcześniej zalogowanego użytkownika o określonym identyfikatorze. Zobacz <code>userId</code> właściwości.</li><li><code>userFromToken</code>-Używa tożsamości reprezentowany przez określony token. Zobacz <code>userToken</code> właściwości.</li><li><code>clientCredentials</code>-Ma zostać użyta tożsamość aplikacji funkcji.</li></ul>|
|**Nazwa użytkownika** |Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromId`. Użytkownik identyfikator podmiotu zabezpieczeń skojarzone z wcześniej zalogowanego użytkownika.|
|**userToken**|Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromToken`. Token nieprawidłowy dla funkcji aplikacji. |
|**zasobów**|Wymagana — adres URL zasobu usługi AAD, dla którego token jest wymagany.|

<a name="token-input-code"></a>
### <a name="using-an-auth-token-input-binding-from-code"></a>Przy użyciu uwierzytelniania tokenu powiązania wejściowego z kodu

Token jest zawsze widoczne dla kodu jako ciąg.

#### <a name="sample-getting-user-profile-information"></a>Przykład: Pobieranie informacji o profilu użytkownika

Załóżmy, że masz następujące function.json, definiujący wyzwalacz protokołu HTTP z tokenu powiązania wejściowego:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "token",
      "direction": "in",
      "name": "graphToken",
      "resource": "https://graph.microsoft.com",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

W poniższym przykładzie C# używa tokenu do wywoływania HTTP Microsoft Graph i zwraca wynik:

```csharp
using System.Net; 
using System.Net.Http; 
using System.Net.Http.Headers; 

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, string graphToken, TraceWriter log)
{
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", graphToken);
    return await client.GetAsync("https://graph.microsoft.com/v1.0/me/");
}
```

W poniższym przykładzie JS używa tokenu do wywoływania HTTP Microsoft Graph i zwraca wynik. W `function.json` powyżej, zmień `$return` do `res` pierwszy.

```js
const rp = require('request-promise');

module.exports = function (context, req) {
    let token = "Bearer " + context.bindings.graphToken;

    let options = {
        uri: 'https://graph.microsoft.com/v1.0/me/',
        headers: {
            'Authorization': token
        }
    };
    
    rp(options)
        .then(function(profile) {
            context.res = {
                body: profile
            };
            context.done();
        })
        .catch(function(err) {
            context.res = {
                status: 500,
                body: err
            };
            context.done();
        });
};
```





<a name="excel-input"></a>
## <a name="excel-table-input-binding"></a>Powiązania wejściowego tabeli programu Excel

To powiązanie odczytuje zawartość tabeli programu Excel zapisanych w usłudze OneDrive.

### <a name="configuring-an-excel-table-input-binding"></a>Konfigurowanie powiązania wejściowego tabeli programu Excel

To powiązanie wymaga następujących uprawnień usługi AAD:
|Zasób|Uprawnienie|
|--------|--------|
|Microsoft Graph|Odczytuj pliki użytkownika|

Wiązanie obsługuje następujące właściwości:

|Właściwość|Opis|
|--------|--------|
|**Nazwa**|Wymagana — nazwa zmiennej używane w kodzie funkcji tabeli programu Excel. Zobacz [przy użyciu tabeli programu Excel wejściowych powiązania z kodu](#excel-input-code).|
|**Typ**|Wymagana — musi być ustawiona `excel`.|
|**Kierunek**|Wymagana — musi być ustawiona `in`.|
|**tożsamości**|Wymagana — tożsamości, która będzie służyć do wykonania akcji. Może to być jedna z następujących wartości:<ul><li><code>userFromRequest</code>— Jedyne prawidłowe z [wyzwalacza HTTP]. Używa tożsamości użytkownika wywołującego.</li><li><code>userFromId</code>-Używa tożsamości wcześniej zalogowanego użytkownika o określonym identyfikatorze. Zobacz <code>userId</code> właściwości.</li><li><code>userFromToken</code>-Używa tożsamości reprezentowany przez określony token. Zobacz <code>userToken</code> właściwości.</li><li><code>clientCredentials</code>-Ma zostać użyta tożsamość aplikacji funkcji.</li></ul>|
|**Nazwa użytkownika** |Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromId`. Użytkownik identyfikator podmiotu zabezpieczeń skojarzone z wcześniej zalogowanego użytkownika.|
|**userToken**|Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromToken`. Token nieprawidłowy dla funkcji aplikacji. |
|**Ścieżka**|Wymagana — ścieżka w usłudze OneDrive do skoroszytu programu Excel.|
|**worksheetName**|Arkusz, w którym znajduje się tabela.|
|**tableName**|Nazwa tabeli. Jeśli nie zostanie określony, będzie używany zawartość arkusza.|

<a name="excel-input-code"></a>
### <a name="using-an-excel-table-input-binding-from-code"></a>Za pomocą powiązania wejściowego tabeli programu Excel z kodu

Powiązanie udostępnia następujące funkcje platformy .NET:
- [String []]
- Microsoft.Graph.WorkbookTable
- Typów obiektów niestandardowych (przy użyciu wiązania modelu strukturalnych)

#### <a name="sample-reading-an-excel-table"></a>Przykład: Odczytywanie tabeli programu Excel

Załóżmy, że masz następujące function.json, definiujący wyzwalacz protokołu HTTP z powiązaniem wejściowy programu Excel:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "excel",
      "direction": "in",
      "name": "excelTableData",
      "path": "{query.workbook}",
      "identity": "UserFromRequest",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

W poniższym przykładzie C# dodaje odczytuje zawartość z określonej tabeli i zwraca je do użytkownika:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives; 

public static IActionResult Run(HttpRequest req, string[][] excelTableData, TraceWriter log)
{
    return new OkObjectResult(excelTableData);
}
```

W poniższym przykładzie JS dodaje odczytuje zawartość z określonej tabeli i zwraca je do użytkownika. W `function.json` powyżej, zmień `$return` do `res` pierwszy.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.excelTableData
    };
    context.done();
};
```

<a name="excel-output"></a>
## <a name="excel-table-output-binding"></a>Powiązania wyjściowego tabelę programu Excel

To powiązanie modyfikuje zawartość tabeli programu Excel zapisanych w usłudze OneDrive.

### <a name="configuring-an-excel-table-output-binding"></a>Konfigurowanie tabeli programu Excel powiązania wyjściowego

To powiązanie wymaga następujących uprawnień usługi AAD:
|Zasób|Uprawnienie|
|--------|--------|
|Microsoft Graph|Miej pełny dostęp do plików użytkownika|

Wiązanie obsługuje następujące właściwości:

|Właściwość|Opis|
|--------|--------|
|**Nazwa**|Wymagana — nazwa zmiennej używane w kodzie funkcja tokenu uwierzytelniania. Zobacz [przy użyciu tabeli programu Excel powiązania z kodu wyjściowego](#excel-output-code).|
|**Typ**|Wymagana — musi być ustawiona `excel`.|
|**Kierunek**|Wymagana — musi być ustawiona `out`.|
|**tożsamości**|Wymagana — tożsamości, która będzie służyć do wykonania akcji. Może to być jedna z następujących wartości:<ul><li><code>userFromRequest</code>— Jedyne prawidłowe z [wyzwalacza HTTP]. Używa tożsamości użytkownika wywołującego.</li><li><code>userFromId</code>-Używa tożsamości wcześniej zalogowanego użytkownika o określonym identyfikatorze. Zobacz <code>userId</code> właściwości.</li><li><code>userFromToken</code>-Używa tożsamości reprezentowany przez określony token. Zobacz <code>userToken</code> właściwości.</li><li><code>clientCredentials</code>-Ma zostać użyta tożsamość aplikacji funkcji.</li></ul>|
|**Nazwa użytkownika** |Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromId`. Użytkownik identyfikator podmiotu zabezpieczeń skojarzone z wcześniej zalogowanego użytkownika.|
|**userToken**|Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromToken`. Token nieprawidłowy dla funkcji aplikacji. |
|**Ścieżka**|Wymagana — ścieżka w usłudze OneDrive do skoroszytu programu Excel.|
|**worksheetName**|Arkusz, w którym znajduje się tabela.|
|**tableName**|Nazwa tabeli. Jeśli nie zostanie określony, będzie używany zawartość arkusza.|
|**Typ aktualizacji**|Wymagana — typ zmiany należy do tabeli. Może to być jedna z następujących wartości:<ul><li><code>update</code>-Zastępuje zawartość tabeli w usłudze OneDrive.</li><li><code>append</code>-Dodaje ładunku na koniec tabeli w usłudze OneDrive, tworząc nowe wiersze.</li></ul>|

<a name="excel-output-code"></a>
### <a name="using-an-excel-table-output-binding-from-code"></a>Za pomocą tabeli programu Excel powiązania z kodu wyjściowego

Powiązanie udostępnia następujące funkcje platformy .NET:
- [String []]
- Newtonsoft.Json.Linq.JObject
- Microsoft.Graph.WorkbookTable
- Typów obiektów niestandardowych (przy użyciu wiązania modelu strukturalnych)

#### <a name="sample-adding-rows-to-an-excel-table"></a>Przykład: Dodawanie wierszy do tabeli programu Excel

Załóżmy, że masz następujące powiązania wyjściowego function.json, który definiuje wyzwalacza HTTP przy użyciu programu Excel:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "newExcelRow",
      "type": "excel",
      "direction": "out",
      "identity": "userFromRequest",
      "updateType": "append",
      "path": "{query.workbook}",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```


W poniższym przykładzie C# dodaje nowy wiersz do tabeli (przyjęto, że jedna kolumna) na podstawie danych wprowadzonych z ciągu zapytania:

```csharp
using System.Net;
using System.Text;

public static async Task Run(HttpRequest req, IAsyncCollector<object> newExcelRow, TraceWriter log)
{
    string input = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await newExcelRow.AddAsync(new {
        Text = input
        // Add other properties for additional columns here
    });
    return;
}
```

W poniższym przykładzie JS dodaje nowy wiersz do tabeli (przyjęto, że jedna kolumna) na podstawie danych wprowadzonych z ciągu zapytania. W `function.json` powyżej, zmień `$return` do `res` pierwszy.

```js
module.exports = function (context, req) {
    context.bindings.newExcelRow = {
        text: req.query.text
        // Add other properties for additional columns here
    }
    context.done();
};
```




<a name="onedrive-input"></a>
## <a name="onedrive-file-input-binding"></a>Powiązania wejściowego pliku OneDrive

To powiązanie odczytuje zawartość pliku zapisanych w usłudze OneDrive.

### <a name="configuring-a-onedrive-file-input-binding"></a>Konfigurowanie usługi OneDrive powiązania wejściowego pliku

To powiązanie wymaga następujących uprawnień usługi AAD:
|Zasób|Uprawnienie|
|--------|--------|
|Microsoft Graph|Odczytuj pliki użytkownika|

Wiązanie obsługuje następujące właściwości:

|Właściwość|Opis|
|--------|--------|
|**Nazwa**|Wymagana — nazwa zmiennej używana w funkcji kodu dla pliku. Zobacz [przy użyciu pliku OneDrive wejściowych powiązania z kodu](#onedrive-input-code).|
|**Typ**|Wymagana — musi być ustawiona `onedrive`.|
|**Kierunek**|Wymagana — musi być ustawiona `in`.|
|**tożsamości**|Wymagana — tożsamości, która będzie służyć do wykonania akcji. Może to być jedna z następujących wartości:<ul><li><code>userFromRequest</code>— Jedyne prawidłowe z [wyzwalacza HTTP]. Używa tożsamości użytkownika wywołującego.</li><li><code>userFromId</code>-Używa tożsamości wcześniej zalogowanego użytkownika o określonym identyfikatorze. Zobacz <code>userId</code> właściwości.</li><li><code>userFromToken</code>-Używa tożsamości reprezentowany przez określony token. Zobacz <code>userToken</code> właściwości.</li><li><code>clientCredentials</code>-Ma zostać użyta tożsamość aplikacji funkcji.</li></ul>|
|**Nazwa użytkownika** |Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromId`. Użytkownik identyfikator podmiotu zabezpieczeń skojarzone z wcześniej zalogowanego użytkownika.|
|**userToken**|Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromToken`. Token nieprawidłowy dla funkcji aplikacji. |
|**Ścieżka**|Wymagana — ścieżka w usłudze OneDrive do pliku.|

<a name="onedrive-input-code"></a>
### <a name="using-a-onedrive-file-input-binding-from-code"></a>Za pomocą usługi OneDrive pliku wejściowego powiązania z kodu

Powiązanie udostępnia następujące funkcje platformy .NET:
- Byte]
- Strumień
- Ciąg
- Microsoft.Graph.DriveItem

#### <a name="sample-reading-a-file-from-onedrive"></a>Przykład: Odczytywanie pliku z usługi OneDrive

Załóżmy, że masz następujące function.json, definiujący wyzwalacz protokołu HTTP z usługi OneDrive powiązania wejściowego:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "in",
      "path": "{query.filename}",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

W poniższym przykładzie C# odczytuje plik określony w ciągu kwerendy i rejestruje jego długość:

```csharp
using System.Net;

public static void Run(HttpRequestMessage req, Stream myOneDriveFile, TraceWriter log)
{
    log.Info(myOneDriveFile.Length.ToString());
}
```

W poniższym przykładzie JS odczytuje plik określony w ciągu kwerendy i zwraca jego długość. W `function.json` powyżej, zmień `$return` do `res` pierwszy.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.myOneDriveFile.length
    };
    context.done();
};
```


<a name="onedrive-output"></a>
## <a name="onedrive-file-output-binding"></a>Powiązania wyjściowego pliku OneDrive

To powiązanie modyfikuje zawartość pliku zapisanych w usłudze OneDrive.

### <a name="configuring-a-onedrive-file-output-binding"></a>Konfigurowanie usługi OneDrive powiązania wyjściowego pliku

To powiązanie wymaga następujących uprawnień usługi AAD:
|Zasób|Uprawnienie|
|--------|--------|
|Microsoft Graph|Miej pełny dostęp do plików użytkownika|

Wiązanie obsługuje następujące właściwości:

|Właściwość|Opis|
|--------|--------|
|**Nazwa**|Wymagana — nazwa zmiennej używana w funkcji kodu dla pliku. Zobacz [przy użyciu pliku OneDrive powiązania z kodu wyjściowego](#onedrive-output-code).|
|**Typ**|Wymagana — musi być ustawiona `onedrive`.|
|**Kierunek**|Wymagana — musi być ustawiona `out`.|
|**tożsamości**|Wymagana — tożsamości, która będzie służyć do wykonania akcji. Może to być jedna z następujących wartości:<ul><li><code>userFromRequest</code>— Jedyne prawidłowe z [wyzwalacza HTTP]. Używa tożsamości użytkownika wywołującego.</li><li><code>userFromId</code>-Używa tożsamości wcześniej zalogowanego użytkownika o określonym identyfikatorze. Zobacz <code>userId</code> właściwości.</li><li><code>userFromToken</code>-Używa tożsamości reprezentowany przez określony token. Zobacz <code>userToken</code> właściwości.</li><li><code>clientCredentials</code>-Ma zostać użyta tożsamość aplikacji funkcji.</li></ul>|
|**Nazwa użytkownika** |Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromId`. Użytkownik identyfikator podmiotu zabezpieczeń skojarzone z wcześniej zalogowanego użytkownika.|
|**userToken**|Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromToken`. Token nieprawidłowy dla funkcji aplikacji. |
|**Ścieżka**|Wymagana — ścieżka w usłudze OneDrive do pliku.|

<a name="onedrive-output-code"></a>
### <a name="using-a-onedrive-file-output-binding-from-code"></a>Przy użyciu usługi OneDrive pliku wyjściowego powiązania z kodu

Powiązanie udostępnia następujące funkcje platformy .NET:
- Byte]
- Strumień
- Ciąg
- Microsoft.Graph.DriveItem

#### <a name="sample-writing-to-a-file-in-onedrive"></a>Przykład: Zapis do pliku w usłudze OneDrive

Załóżmy, że masz następujące powiązania wyjściowego function.json, który definiuje wyzwalacza HTTP z usługi OneDrive:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "out",
      "path": "FunctionsTest.txt",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

W poniższym przykładzie C# pobiera tekst z ciągu zapytania i zapisuje go w pliku tekstowym (FunctionsTest.txt określone w powyższej konfiguracji) w katalogu głównym wywołującego OneDrive:

```csharp
using System.Net;
using System.Text;

public static async Task Run(HttpRequest req, TraceWriter log, Stream myOneDriveFile)
{
    string data = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await myOneDriveFile.WriteAsync(Encoding.UTF8.GetBytes(data), 0, data.Length);
    return;
}
```
W poniższym przykładzie JS pobiera tekst z ciągu zapytania i zapisuje go w pliku tekstowym (FunctionsTest.txt zgodnie z definicją w pliku config powyżej) w katalogu głównym usługi OneDrive obiektu wywołującego. W `function.json` powyżej, zmień `$return` do `res` pierwszy.

```js
module.exports = function (context, req) {
    context.bindings.myOneDriveFile = req.query.text;
    context.done();
};
```



<a name="outlook-output"></a>
## <a name="outlook-message-output-binding"></a>Powiązania wyjściowego wiadomości programu Outlook

Wysyła wiadomość e-mail, za pomocą programu Outlook.

### <a name="configuring-an-outlook-message-output-binding"></a>Konfigurowanie wiadomości programu Outlook powiązania wyjściowego

To powiązanie wymaga następujących uprawnień usługi AAD:
|Zasób|Uprawnienie|
|--------|--------|
|Microsoft Graph|Wysyłania wiadomości e-mail jako użytkownik|

Wiązanie obsługuje następujące właściwości:

|Właściwość|Opis|
|--------|--------|
|**Nazwa**|Wymagana — nazwa zmiennej używane w kodzie funkcji wiadomości e-mail. Zobacz [wiadomości programu Outlook powiązania z kodu wyjściowego](#outlook-output-code).|
|**Typ**|Wymagana — musi być ustawiona `outlook`.|
|**Kierunek**|Wymagana — musi być ustawiona `out`.|
|**tożsamości**|Wymagana — tożsamości, która będzie służyć do wykonania akcji. Może to być jedna z następujących wartości:<ul><li><code>userFromRequest</code>— Jedyne prawidłowe z [wyzwalacza HTTP]. Używa tożsamości użytkownika wywołującego.</li><li><code>userFromId</code>-Używa tożsamości wcześniej zalogowanego użytkownika o określonym identyfikatorze. Zobacz <code>userId</code> właściwości.</li><li><code>userFromToken</code>-Używa tożsamości reprezentowany przez określony token. Zobacz <code>userToken</code> właściwości.</li><li><code>clientCredentials</code>-Ma zostać użyta tożsamość aplikacji funkcji.</li></ul>|
|**Nazwa użytkownika** |Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromId`. Użytkownik identyfikator podmiotu zabezpieczeń skojarzone z wcześniej zalogowanego użytkownika.|
|**userToken**|Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromToken`. Token nieprawidłowy dla funkcji aplikacji. |

<a name="outlook-output-code"></a>
### <a name="using-an-outlook-message-output-binding-from-code"></a>Wiadomości programu Outlook powiązania z kodu wyjściowego

Powiązanie udostępnia następujące funkcje platformy .NET:
- Microsoft.Graph.Message
- Newtonsoft.Json.Linq.JObject
- Ciąg
- Typów obiektów niestandardowych (przy użyciu wiązania modelu strukturalnych)

#### <a name="sample-sending-an-email-through-outlook"></a>Przykład: Wysyłanie wiadomości e-mail za pomocą programu Outlook

Załóżmy, że masz następujące powiązania wyjściowego function.json, który definiuje wyzwalacza HTTP z wiadomości programu Outlook:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "message",
      "type": "outlook",
      "direction": "out",
      "identity": "userFromRequest"
    }
  ],
  "disabled": false
}
```

W poniższym przykładzie C# wysyła wiadomości e-mail z wywołującego do adresatów określonych w ciągu zapytania:

```csharp
using System.Net;

public static void Run(HttpRequest req, out Message message, TraceWriter log)
{ 
    string emailAddress = req.Query["to"];
    message = new Message(){
        subject = "Greetings",
        body = "Sent from Azure Functions",
        recipient = new Recipient() {
            address = emailAddress
        }
    };
}

public class Message {
    public String subject {get; set;}
    public String body {get; set;}
    public Recipient recipient {get; set;}
}

public class Recipient {
    public String address {get; set;}
    public String name {get; set;}
}
```

W poniższym przykładzie JS wysyła wiadomości e-mail z wywołującego do adresatów określonych w ciągu zapytania:

```js
module.exports = function (context, req) {
    context.bindings.message = {
        subject: "Greetings",
        body: "Sent from Azure Functions with JavaScript",
        recipient: {
            address: req.query.to 
        } 
    };
    context.done();
};
```





<a name="webhooks"></a>
## <a name="microsoft-graph-webhook-bindings"></a>Program Microsoft Graph powiązania elementu webhook

Elementów Webhook umożliwiają reagowania na zdarzenia w programie Microsoft Graph. Aby zapewnić obsługę elementów webhook, funkcje są potrzebne do tworzenia, Odśwież i reagowania na _subskrypcje elementu webhook_. Rozwiązanie elementu webhook pełną wymaga kombinację następujących powiązania:
- A [wyzwalacza elementu webhook Microsoft Graph](#webhook-trigger) służy do reagowania na przychodzące elementu webhook.
- A [powiązania danych wejściowych subskrypcji elementu webhook Microsoft Graph](#webhook-input) pozwala na liście istniejące subskrypcje i opcjonalnie odświeżać je.
- A [powiązania wyjściowego subskrypcji elementu webhook Microsoft Graph](#webhook-output) służy do tworzenia lub usuwania elementu webhook subskrypcji.

Powiązania się nie wymagają żadnych uprawnień usługi AAD, ale musisz zażądać uprawnień odpowiedni typ zasobu, który ma zostać reagowania na. Dla listy, które są potrzebne uprawnienia dla każdego typu zasobu, zobacz [uprawnienia subskrypcji](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/api/subscription_post_subscriptions#permissions).

Aby uzyskać więcej informacji na temat elementów webhook, zobacz [Praca z elementów webhook w programie Microsoft Graph].





<a name="webhook-trigger"></a>
### <a name="microsoft-graph-webhook-trigger"></a>Program Microsoft Graph wyzwalacza elementu webhook

Wyzwalacz umożliwia funkcja reagowanie na element webhook przychodzące z programu Microsoft Graph. Każde wystąpienie tego wyzwalacza mogą reagować na jeden typ zasobu Microsoft Graph.

#### <a name="configuring-a-microsoft-graph-webhook-trigger"></a>Konfigurowanie programu Microsoft Graph wyzwalacza elementu webhook

Wiązanie obsługuje następujące właściwości:

|Właściwość|Opis|
|--------|--------|
|**Nazwa**|Wymagana — nazwa zmiennej używane w kodzie funkcji wiadomości e-mail. Zobacz [wiadomości programu Outlook powiązania z kodu wyjściowego](#outlook-output-code).|
|**Typ**|Wymagana — musi być ustawiona `graphWebhook`.|
|**Kierunek**|Wymagana — musi być ustawiona `trigger`.|
|**Typ zasobu**|Wymagana — zasobów wykresu, dla których ta funkcja powinno odpowiedzieć na elementów webhook. Może to być jedna z następujących wartości:<ul><li><code>#Microsoft.Graph.Message</code>-zmiany wprowadzone w wiadomości programu Outlook.</li><li><code>#Microsoft.Graph.DriveItem</code>-zmiany wprowadzone do usługi OneDrive elementów głównych.</li><li><code>#Microsoft.Graph.Contact - changes made to personal contacts in Outlook.</code></li><li><code>#Microsoft.Graph.Event - changes made to Outlook calendar items.</code></li></ul>|

> [!Note]
> Aplikacji funkcja może mieć tylko jedną funkcję, który jest zarejestrowany dla danego `resourceType` wartość.

#### <a name="using-a-microsoft-graph-webhook-trigger-from-code"></a>Za pomocą programu Microsoft Graph wyzwalacza elementu webhook z kodu

Powiązanie udostępnia następujące funkcje platformy .NET:
- Typy Microsoft Graph SDK odpowiednie do zasobu typu, np. Microsoft.Graph.Message, Microsoft.Graph.DriveItem
- Typów obiektów niestandardowych (przy użyciu wiązania modelu strukturalnych)

Przykłady kodu za pomocą tego powiązania, zobacz [przykłady elementu webhook Microsoft Graph](#webhook-samples).



<a name="webhook-input"></a>
### <a name="microsoft-graph-webhook-subscription-input-binding"></a>Program Microsoft Graph powiązania wejściowego subskrypcji elementu webhook

To powiązanie umożliwia pobieranie listy zarządzane przez tę aplikację funkcji subskrypcji. Powiązanie odczytuje z funkcji aplikacji magazynu i nie odzwierciedla inne subskrypcje utworzone na podstawie poza aplikacją.

#### <a name="configuring-a-microsoft-graph-webhook-subscription-input-binding"></a>Konfigurowanie programu Microsoft Graph powiązania wejściowego subskrypcji elementu webhook

Wiązanie obsługuje następujące właściwości:

|Właściwość|Opis|
|--------|--------|
|**Nazwa**|Wymagana — nazwa zmiennej używane w kodzie funkcji wiadomości e-mail. Zobacz [wiadomości programu Outlook powiązania z kodu wyjściowego](#outlook-output-code).|
|**Typ**|Wymagana — musi być ustawiona `graphWebhookSubscription`.|
|**Kierunek**|Wymagana — musi być ustawiona `in`.|
|**Filtr**| Jeśli ustawioną `userFromRequest`, a następnie powiązanie pobierze tylko subskrypcje należących do użytkownika wywołującego (prawidłowa tylko w przypadku [wyzwalacza HTTP]).| 

#### <a name="using-a-microsoft-graph-webhook-subscription-input-binding-from-code"></a>Za pomocą programu Microsoft Graph elementu webhook subskrypcji wejściowych powiązania z kodu

Powiązanie udostępnia następujące funkcje platformy .NET:
- ciąg]
- Tablice typu niestandardowego obiektu
- [Newtonsoft.Json.Linq.JObject]
- [Microsoft.Graph.Subscription]

Przykłady kodu za pomocą tego powiązania, zobacz [przykłady elementu webhook Microsoft Graph](#webhook-samples).


<a name="webhook-output"></a>
### <a name="microsoft-graph-webhook-subscription-output-binding"></a>Program Microsoft Graph webhook subskrypcji powiązania wyjściowego

To powiązanie umożliwia tworzenie, usuwanie i odświeżanie subskrypcji elementu webhook w programie Microsoft Graph.

#### <a name="configuring-a-microsoft-graph-webhook-subscription-output-binding"></a>Konfigurowanie elementu webhook Microsoft Graph subskrypcji powiązania wyjściowego

Wiązanie obsługuje następujące właściwości:

|Właściwość|Opis|
|--------|--------|
|**Nazwa**|Wymagana — nazwa zmiennej używane w kodzie funkcji wiadomości e-mail. Zobacz [wiadomości programu Outlook powiązania z kodu wyjściowego](#outlook-output-code).|
|**Typ**|Wymagana — musi być ustawiona `graphWebhookSubscription`.|
|**Kierunek**|Wymagana — musi być ustawiona `out`.|
|**tożsamości**|Wymagana — tożsamości, która będzie służyć do wykonania akcji. Może to być jedna z następujących wartości:<ul><li><code>userFromRequest</code>— Jedyne prawidłowe z [wyzwalacza HTTP]. Używa tożsamości użytkownika wywołującego.</li><li><code>userFromId</code>-Używa tożsamości wcześniej zalogowanego użytkownika o określonym identyfikatorze. Zobacz <code>userId</code> właściwości.</li><li><code>userFromToken</code>-Używa tożsamości reprezentowany przez określony token. Zobacz <code>userToken</code> właściwości.</li><li><code>clientCredentials</code>-Ma zostać użyta tożsamość aplikacji funkcji.</li></ul>|
|**Nazwa użytkownika** |Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromId`. Użytkownik identyfikator podmiotu zabezpieczeń skojarzone z wcześniej zalogowanego użytkownika.|
|**userToken**|Wymagane w przypadku i tylko wtedy, gdy _tożsamości_ ma ustawioną wartość `userFromToken`. Token nieprawidłowy dla funkcji aplikacji. |
|**Akcja**|Niewymagana — określa akcji powiązanie powinny być obsługiwane. Może to być jedna z następujących wartości:<ul><li><code>create</code>-Rejestruje nową subskrypcję.</li><li><code>delete</code>— Usuwa z określonej subskrypcji.</li><li><code>refresh</code>-Odświeża określonej subskrypcji, aby zapobiec wygaśnięciu.</li></ul>|
|**subscriptionResource**|Wymagane w przypadku i tylko wtedy, gdy _akcji_ ma ustawioną wartość `create`. Określa zasób Microsoft Graph, które będą monitorowane zmian. Zobacz [Praca z elementów webhook w programie Microsoft Graph]. |
|**changeType**|Wymagane w przypadku i tylko wtedy, gdy _akcji_ ma ustawioną wartość `create`. Wskazuje typ zmiany w subskrybowanego zasób, który zostanie podniesiony powiadomienie. Obsługiwane wartości to: `created`, `updated`, `deleted`. Można łączyć wiele wartości, używając listy rozdzielanej przecinkami.|

#### <a name="using-a-microsoft-graph-webhook-subscription-output-binding-from-code"></a>Przy użyciu elementu webhook Microsoft Graph subskrypcji output powiązania z kodu

Powiązanie udostępnia następujące funkcje platformy .NET:
- Ciąg
- Microsoft.Graph.Subscription

Przykłady kodu za pomocą tego powiązania, zobacz [przykłady elementu webhook Microsoft Graph](#webhook-samples).
 
<a name="webhook-samples"></a>
### <a name="microsoft-graph-webhook-samples"></a>Przykłady elementu webhook Microsoft Graph

#### <a name="sample-creating-a-subscription"></a>Przykład: Tworzenie subskrypcji

Załóżmy, że masz następujące function.json, definiujący wyzwalacz protokołu HTTP z wyjściowego subskrypcji, powiązanie, korzystając z akcji Utwórz:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphwebhook",
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "listen": "me/mailFolders('Inbox')/messages",
      "changeTypes": [
        "created"
      ],
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

W poniższym przykładzie C# rejestruje elementu webhook, który powiadomi to funkcja aplikacji podczas wywoływania użytkownik odbiera wiadomości programu Outlook:

```csharp
using System;
using System.Net;

public static HttpResponseMessage run(HttpRequestMessage req, out string clientState, TraceWriter log)
{
  log.Info("C# HTTP trigger function processed a request.");
    clientState = Guid.NewGuid().ToString();
    return new HttpResponseMessage(HttpStatusCode.OK);
}
```

W poniższym przykładzie JS rejestruje elementu webhook, który powiadomi to funkcja aplikacji podczas wywoływania użytkownik odbiera wiadomości programu Outlook:

```js
const uuidv4 = require('uuid/v4');

module.exports = function (context, req) {
    context.bindings.clientState = uuidv4();
    context.done();
};
```

#### <a name="sample-handling-notifications"></a>Przykład: Obsługiwanie powiadomień

Załóżmy, że masz następujące function.json, definiujący wyzwalacza elementu webhook wykresu do obsługi wiadomości programu Outlook:

```json
{
  "bindings": [
    {
      "name": "msg",
      "type": "GraphWebhookTrigger",
      "direction": "in",
      "resourceType": "#Microsoft.Graph.Message"
    }
  ],
  "disabled": false
}
```

W poniższym przykładzie C# reaguje na przychodzące wiadomości e-mail i rejestruje treść tych wysyłane przez odbiorcę i zawierający "Usługi Azure Functions" w temacie:

```csharp
#r "Microsoft.Graph"
using Microsoft.Graph;
using System.Net;

public static async Task Run(Message msg, TraceWriter log)  
{
    log.Info("Microsoft Graph webhook trigger function processed a request.");

    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if (msg.Subject.Contains("Azure Functions") && msg.From.Equals(msg.Sender)) {
        log.Info($"Processed email: {msg.BodyPreview}");
    }
}
```

W poniższym przykładzie JS reaguje na przychodzące wiadomości e-mail i rejestruje treść tych wysyłane przez odbiorcę i zawierający "Usługi Azure Functions" w temacie:

```js
module.exports = function (context) {
    context.log("Microsoft Graph webhook trigger function processed a request.");
    const msg = context.bindings.msg
    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if((msg.subject.indexOf("Azure Functions") > -1) && (msg.from === msg.sender) ) {
      context.log(`Processed email: ${msg.bodyPreview}`);
    }
    context.done();
};
```

#### <a name="sample-deleting-subscriptions"></a>Przykład: Usunięcie subskrypcji

Załóżmy, że masz następujące function.json, który definiuje wyzwalacza HTTP z powiązania wejściowego subskrypcję i wyjścia subskrypcji, powiązanie, korzystając z akcji usuwania:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in",
      "filter": "userFromRequest"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToDelete",
      "direction": "out",
      "action": "delete",
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "res",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

W poniższym przykładzie C# pobiera wszystkie subskrypcje dla wywołania użytkownika, a następnie usuwa je:

```csharp
using System.Net;

public static async Task Run(HttpRequest req, string[] existingSubscriptions, IAsyncCollector<string> subscriptionsToDelete, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    foreach (var subscription in existingSubscriptions)
    {
        log.Info($"Deleting subscription {subscription}");
        await subscriptionsToDelete.AddAsync(subscription);
    }
}
```

W poniższym przykładzie JS pobiera wszystkie subskrypcje dla wywołania użytkownika, a następnie usuwa je:

```js
module.exports = function (context, req) {
    const existing = context.bindings.existingSubscriptions;
    var toDelete = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Deleting subscription ${existing[i]}`);
        todelete.push(existing[i]);
    }
    context.bindings.subscriptionsToDelete = toDelete;
    context.done();
};
```

#### <a name="sample-refreshing-subscriptions"></a>Przykład: Odświeżanie subskrypcji

Istnieją dwa podejścia do odświeżania subskrypcji:
- Użyj tożsamości aplikacji na wypadek wszystkie subskrypcje. Będzie to wymagało zgody administratora usługi Azure Active Directory. Może być używany przez wszystkie języki obsługiwane przez usługi Azure Functions.
- Użyj tożsamość skojarzoną z każdej subskrypcji przez ręcznie powiązanie każdy identyfikator użytkownika. Będzie to wymagało kodu niestandardowego w celu wykonania powiązania. Może być używany tylko przez funkcje środowiska .NET.

Poniżej przedstawiono obie opcje.

**Przy użyciu tożsamości aplikacji**

Załóżmy, że masz następujące function.json, definiujący wyzwalacza bazującego na czasomierzu o wyjściowej subskrypcji powiązania wejściowego subskrypcji powiązanie, przy użyciu tożsamości aplikacji:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToRefresh",
      "direction": "out",
      "action": "refresh",
      "identity": "clientCredentials"
    }
  ],
  "disabled": false
}
```

W poniższym przykładzie C# odświeża subskrypcji z czasomierzem, przy użyciu tożsamości aplikacji:

```csharp
using System;

public static void Run(TimerInfo myTimer, string[] existingSubscriptions, ICollector<string> subscriptionsToRefresh, TraceWriter log)
{
    // This template uses application permissions and requires consent from an Azure Active Directory admin.
    // See https://go.microsoft.com/fwlink/?linkid=858780
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
      log.Info($"Refreshing subscription {subscription}");
      subscriptionsToRefresh.Add(subscription);
    }
}
```

W poniższym przykładzie JS odświeża subskrypcji z czasomierzem, przy użyciu tożsamości aplikacji:

```js
// This template uses application permissions and requires consent from an Azure Active Directory admin.
// See https://go.microsoft.com/fwlink/?linkid=858780

module.exports = function (context) {
    const existing = context.bindings.existingSubscriptions;
    var toRefresh = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Deleting subscription ${existing[i]}`);
        todelete.push(existing[i]);
    }
    context.bindings.subscriptionsToRefresh = toRefresh;
    context.done();
};
```

**Przy użyciu tożsamości użytkownika dynamiczne**

Dla opcji alternatywnych Załóżmy, że masz następujące function.json, który definiuje wyzwalacz czasomierza odkładanie powiązania do subskrypcji wejściowych powiązania kod funkcji:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

W poniższym przykładzie C# odświeża subskrypcji z czasomierzem, tworząc powiązania danych wyjściowych w kodzie za pomocą tożsamości każdego użytkownika:
```csharp
using System;

public static async Task Run(TimerInfo myTimer, UserSubscription[] existingSubscriptions, IBinder binder, TraceWriter log)
{
  log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
        // binding in code to allow dynamic identity
        using (var subscriptionsToRefresh = await binder.BindAsync<IAsyncCollector<string>>(
            new GraphWebhookSubscriptionAttribute() {
                Action = "refresh",
                Identity = "userFromId",
                UserId = subscription.UserId
            }
        ))
        {
            log.Info($"Refreshing subscription {subscription}");
            await subscriptionsToRefresh.AddAsync(subscription);
        }

    }
}

public class UserSubscription {
    public string UserId {get; set;}
    public string Id {get; set;}
}
```



[wyzwalacza HTTP]: functions-bindings-http-webhook.md
[Praca z elementów webhook w programie Microsoft Graph]: https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/webhooks

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Dowiedz się więcej o usługę Azure functions wyzwalaczy i powiązań](functions-triggers-bindings.md)
