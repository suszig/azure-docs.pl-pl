---
title: "Praca z wyzwalaczy i powiązań w usługi Azure Functions | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać wyzwalaczy i powiązań w usługi Azure Functions nawiązać połączenia z wykonanie kodu zdarzenia w sieci i usług w chmurze."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "usługa Azure Functions, funkcje, przetwarzanie zdarzeń, elementy webhook, obliczanie dynamiczne, architektura bez serwera"
ms.assetid: cbc7460a-4d8a-423f-a63e-1cd33fef7252
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: glenga
ms.openlocfilehash: 74933d9c3535ab71f47c792e20bfbc35e589ec08
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure funkcje wyzwalaczy i powiązań pojęcia
Środowisko Azure Functions umożliwia pisanie kodu w odpowiedzi na zdarzenia w Azure i innych usług za pośrednictwem *wyzwalaczy* i *powiązania*. Ten artykuł zawiera omówienie wyzwalaczy i powiązań dla wszystkich obsługiwanych języków programowania. Funkcje, które są wspólne dla wszystkich powiązań są opisane poniżej.

## <a name="overview"></a>Omówienie

Wyzwalaczy i powiązań są deklaratywne Definiowanie sposób wywoływania funkcji i co działa z danych. A *wyzwalacza* definiuje sposób wywoływania funkcji. Funkcja musi mieć dokładnie jeden wyzwalacz. Wyzwalacze mieć skojarzone dane, co jest zazwyczaj ładunku, który wywołał funkcję. 

Wejście i wyjście *powiązania* Podaj deklaratywne, aby nawiązać połączenie danych z poziomu kodu. Podobnie jak wyzwalaczy, należy określić parametry połączenia i inne właściwości konfiguracji funkcji. Powiązania są opcjonalne i mieć wielu danych wejściowych i wyjściowych powiązania funkcji. 

Przy użyciu wyzwalaczy i powiązań, napisać kod, który jest więcej ogólny i nie umieszczaj szczegóły usługi, z którego nastąpi interakcja. Dane pochodzące z usługi po prostu stają się wartości wejściowe dla kodu funkcji. Do wysyłania danych do innej usługi (np. utworzenie nowego wiersza w magazynie tabel platformy Azure), użyj wartości zwracanej metody. Lub, jeśli zajdzie potrzeba output wiele wartości, użyj obiektu pomocnika. Mieć wyzwalaczy i powiązań **nazwa** właściwość, która jest identyfikatorem w kodzie uzyskiwania dostępu za pomocą powiązania.

Można skonfigurować wyzwalaczy i powiązań w **integracji** kartę w portalu Azure Functions. W obszarze obejmuje, interfejs użytkownika modyfikuje plik o nazwie *function.json* pliku w katalogu funkcji. Ten plik można edytować, zmieniając **Zaawansowany edytor**.

W poniższej tabeli przedstawiono wyzwalaczy i powiązań, które są obsługiwane w środowisku Azure Functions. 

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

### <a name="example-queue-trigger-and-table-output-binding"></a>Przykład: wyzwalacz kolejki i tabeli powiązania wyjściowego

Załóżmy, że chcesz zapisać nowy wiersz do magazynu tabel Azure przy każdym wyświetleniu nowego komunikatu w magazynie kolejek Azure. W tym scenariuszu można implementować przy użyciu kolejek Azure wyzwalacza i Azure Table Storage powiązania wyjściowego. 

Wyzwalacz magazynu kolejek Azure wymaga następujących informacji w **integracji** karty:

* Nazwa ustawienia aplikacji, która zawiera parametry połączenia konta magazynu Azure dla magazynu kolejek Azure
* Nazwa kolejki
* Identyfikator w kodzie do odczytu treści wiadomości kolejki, takich jak `order`.

Można zapisać do magazynu tabel Azure, użyj powiązania danych wyjściowych z następującymi szczegółami:

* Nazwa ustawienia aplikacji, który zawiera parametry połączenia konta magazynu Azure do magazynu tabel Azure
* Nazwa tabeli
* Identyfikator kod w celu utworzenia elementów wyjściowych lub wartości zwracanej z funkcji.

Powiązania Użyj parametrów połączenia z wartościami przechowywanymi w ustawieniach aplikacji w celu wymuszenia najlepszych rozwiązań, które *function.json* nie zawiera kluczy tajnych usługi, a zamiast tego po prostu zawierają nazwy ustawień aplikacji.

Następnie należy użyć identyfikatorów, które są dostarczane do integracji z usługą Azure Storage w kodzie.

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, TraceWriter log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

```javascript
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

Oto *function.json* odpowiadający poprzedni kod. Należy pamiętać, że tej samej konfiguracji mogą być używane, niezależnie od języka implementację funkcji.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```
Aby wyświetlić i edytować zawartość *function.json* w portalu Azure kliknij **Zaawansowany edytor** opcja **integracji** kartę funkcji.

Aby uzyskać więcej przykładów kodu i szczegółowe informacje o integracji z usługą Azure Storage, zobacz [usługi Azure Functions wyzwalaczy i powiązań usługi Azure Storage](functions-bindings-storage.md).

### <a name="binding-direction"></a>Kierunek powiązania

Wszystkich wyzwalaczy i powiązań ma `direction` właściwości:

- Wyzwalacze kierunek jest zawsze`in`
- Użyj powiązań wejściowych i wyjściowych `in` i`out`
- Niektóre powiązania obsługuje specjalne kierunku `inout`. Jeśli używasz `inout`, tylko **Zaawansowany edytor** jest dostępna w **integracji** kartę.

## <a name="using-the-function-return-type-to-return-a-single-output"></a>Zwraca jeden z za pomocą zwracanego typu funkcji

Poprzedni przykład przedstawia sposób użycia funkcji zwracana wartość zapewnienie dane wyjściowe do powiązania, które jest realizowane za pośrednictwem parametru specjalną nazwą `$return`. (To jest tylko obsługiwana w językach, które mają wartość zwracaną, takich jak C#, JavaScript i F #.) Jeśli funkcja ma wiele powiązań danych wyjściowych, użyj `$return` tylko jednego powiązania danych wyjściowych. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Przykłady poniżej Pokaż jak przywrócić typy są używane z powiązaniami danych wyjściowych w języku C#, JavaScript i F #.

```cs
// C# example: use method return value for output binding
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
// C# example: async method, using return value for output binding
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

```javascript
// JavaScript: return a value in the second parameter to context.done
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

```fsharp
// F# example: use return value for output binding
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="binding-datatype-property"></a>Właściwość dataType powiązania

W środowisku .NET należy użyć typów do definiowania typu danych dla danych wejściowych. Na przykład użyć `string` powiązać tekstu wyzwalacza kolejki, tablica bajtów do odczytu jako binarny i niestandardowego typu do deserializacji do obiektu POCO.

Dla języków, które są dynamicznie wpisane takich jak JavaScript, użyj `dataType` właściwości w definicji powiązania. Na przykład można odczytać treści żądania HTTP w formacie binarnym, użyj typu `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Inne opcje `dataType` są `stream` i `string`.

## <a name="resolving-app-settings"></a>Rozpoznawanie ustawień aplikacji
Najlepszym rozwiązaniem kluczy tajnych i parametry połączenia mają być zarządzane przy użyciu ustawienia aplikacji, a nie plików konfiguracyjnych. To ogranicza dostęp do tych kluczy tajnych i pozwala bezpiecznie przechowywać *function.json* w repozytorium kontroli źródła publicznego.

Ustawienia aplikacji są przydatne także w przypadku, gdy chcesz zmienić konfigurację na podstawie środowiska. Na przykład w środowisku testowym można monitorować różne kontenera magazynu kolejek i obiektów blob.

Ustawienia aplikacji zostaną rozwiązane w każdym przypadku, gdy wartość jest ujęta w znaki procentu, takich jak `%MyAppSetting%`. Należy pamiętać, że `connection` wyzwalaczy i powiązań jest szczególnych przypadkach i automatycznie rozpoznaje wartości jako ustawienia aplikacji. 

Poniższy przykład jest magazyn kolejek Azure wyzwalacz, który używa ustawienia aplikacji `%input-queue-name%` do definiowania wyzwalane w kolejce.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "%input-queue-name%",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

## <a name="trigger-metadata-properties"></a>Właściwości metadanych wyzwalacza

Oprócz ładunku danych dostarczonych przez wyzwalacz (na przykład kolejki komunikatów, który wywołał funkcję) wiele wyzwalaczy, podaj wartości dodatkowe metadane. Te wartości może służyć jako parametry wejściowe w języku C# i F # lub właściwości na `context.bindings` obiektu w języku JavaScript. 

Na przykład wyzwalacz kolejki magazynu Azure obsługuje następujące właściwości:

* QueueTrigger - wyzwalania zawartość komunikatu, jeśli prawidłowy ciąg
* DequeueCount
* expirationTime
* Identyfikator
* InsertionTime
* NextVisibleTime
* Elementu PopReceipt

Szczegółowe informacje o właściwości metadanych dla każdego wyzwalacza są opisane w odpowiedni temat odwołania. Dokumentacja jest również dostępna w **integracji** kartę portalu w **dokumentacji** sekcji poniżej obszar konfiguracji powiązania.  

Na przykład, ponieważ wyzwalacze obiektu blob mają pewne opóźnienia, umożliwia wyzwalacz kolejki uruchomienia funkcji (zobacz [wyzwalacza magazynu obiektów Blob](functions-bindings-storage-blob.md#storage-blob-trigger)). Komunikat z kolejki może zawierać filename obiektu blob do wyzwolenia na. Przy użyciu `queueTrigger` właściwości metadanych to zachowanie można określić w konfiguracji, a nie w kodzie.

```json
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "direction": "in",
      "connection": "MyStorageConnection"
    }
  ]
```

Właściwości metadanych od wyzwalacza można również w *powiązanie wyrażenie* dla innego powiązania, zgodnie z opisem w poniższej sekcji.

## <a name="binding-expressions-and-patterns"></a>Wyrażenia wiązania i wzorce

Jedną z najbardziej zaawansowanych funkcji, wyzwalaczy i powiązań jest *wyrażenia powiązania*. W ramach wiązania, można zdefiniować wzorzec wyrażenia, które mogą być następnie używane w pozostałych powiązaniach lub kodu. Można także metadanych wyzwalacza w wyrażenia, powiązania jako Pokaż w próbce w poprzedniej sekcji.

Załóżmy na przykład, aby zmienić rozmiar obrazów w kontenera magazynu obiektów blob w określonym, podobnie jak **zmiany rozmiaru obrazu** szablonu w **nową funkcję** strony. Przejdź do **nową funkcję** -> języka **C#** -> Scenariusz **przykłady** -> **ImageResizer CSharp**. 

Oto *function.json* definicji:

```json
{
  "bindings": [
    {
      "name": "image",
      "type": "blobTrigger",
      "path": "sample-images/{filename}",
      "direction": "in",
      "connection": "MyStorageConnection"
    },
    {
      "name": "imageSmall",
      "type": "blob",
      "path": "sample-images-sm/{filename}",
      "direction": "out",
      "connection": "MyStorageConnection"
    }
  ],
}
```

Zwróć uwagę, że `filename` parametr jest używany zarówno definicję wyzwalacza obiektu blob, jak również obiektu blob powiązania wyjściowego. Ten parametr może również w kodzie funkcji.

```csharp
// C# example of binding to {filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->


### <a name="random-guids"></a>Losowe identyfikatory GUID
Środowisko Azure Functions zapewnia składni wygody generowania identyfikatorów GUID w powiązania, za pośrednictwem `{rand-guid}` powiązanie wyrażenia. W poniższym przykładzie użyto to nazwy obiektu blob unikatowy: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a>Bieżący czas

Można użyć wyrażenia powiązania `DateTime`, który jest rozpoznawany jako `DateTime.UtcNow`.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-to-custom-input-properties-in-a-binding-expression"></a>Powiązania niestandardowe właściwości wejściowych w wyrażeniu powiązania

Wyrażenia powiązania można także odwoływać właściwości, które są zdefiniowane w ładunku wyzwalacza samej siebie. Na przykład można dynamicznie powiązania do pliku magazynu obiektów blob z nazwą w elementu webhook.

Na przykład następująca *function.json* używa właściwość o nazwie `BlobName` z ładunku wyzwalacz:

```json
{
  "bindings": [
    {
      "name": "info",
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson"
    },
    {
      "name": "blobContents",
      "type": "blob",
      "direction": "in",
      "path": "strings/{BlobName}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

W tym celu w języku C# i F #, należy zdefiniować POCO, definiujący pola, które będą deserializowane w ładunku wyzwalacza.

```csharp
using System.Net;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

W języku JavaScript deserializacji JSON jest wykonywana automatycznie i można użyć właściwości bezpośrednio.

```javascript
module.exports = function (context, info) {
    if ('BlobName' in info) {
        context.res = {
            body: { 'data': context.bindings.blobContents }
        }
    }
    else {
        context.res = {
            status: 404
        };
    }
    context.done();
}
```

## <a name="configuring-binding-data-at-runtime"></a>Konfigurowanie powiązania danych w czasie wykonywania

W języku C# i innych języków .NET, można użyć wzorca wiązania konieczne, w przeciwieństwie do deklaratywne powiązania w *function.json*. Powiązanie konieczne jest przydatne, gdy Parametry wiążące muszą ma zostać obliczony w czasie środowiska uruchomieniowego zamiast projektu. Aby dowiedzieć się więcej, zobacz [powiązania w czasie wykonywania za pośrednictwem powiązania imperatywnych](functions-reference-csharp.md#imperative-bindings) w dokumentacja dla deweloperów języka C#.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na określone powiązanie zobacz następujące artykuły:

- [HTTP i elementy webhook](functions-bindings-http-webhook.md)
- [Czasomierz](functions-bindings-timer.md)
- [Queue Storage](functions-bindings-storage-queue.md)
- [Blob Storage](functions-bindings-storage-blob.md)
- [Table Storage](functions-bindings-storage-table.md)
- [Centrum zdarzeń](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [Azure Cosmos DB](functions-bindings-documentdb.md)
- [Microsoft Graph](functions-bindings-microsoft-graph.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Notification Hubs](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Plik zewnętrzny](functions-bindings-external-file.md)
