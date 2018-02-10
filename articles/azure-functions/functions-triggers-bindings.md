---
title: "Wyzwalaczy i powiązań w usługi Azure Functions"
description: "Dowiedz się, jak używać wyzwalaczy i powiązań w usługi Azure Functions nawiązać połączenia z wykonanie kodu zdarzenia w sieci i usług w chmurze."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "usługa Azure Functions, funkcje, przetwarzanie zdarzeń, elementy webhook, obliczanie dynamiczne, architektura bez serwera"
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: 90a192f58f0e4b285f7aece8a3555c08df051f38
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure funkcje wyzwalaczy i powiązań pojęcia

Ten artykuł zawiera omówienie wyzwalaczy i powiązań w funkcji platformy Azure. W tym miejscu opisano funkcje, które są wspólne dla wszystkich powiązań i wszystkich obsługiwanych językach.

## <a name="overview"></a>Przegląd

A *wyzwalacza* definiuje sposób wywoływania funkcji. Funkcja musi mieć dokładnie jeden wyzwalacz. Wyzwalacze mieć skojarzone dane, co jest zazwyczaj ładunku, który wywołał funkcję.

Wejście i wyjście *powiązania* Podaj deklaratywne, aby nawiązać połączenie danych z poziomu kodu. Powiązania są opcjonalne i mieć wielu danych wejściowych i wyjściowych powiązania funkcji. 

Wyzwalaczy i powiązań pozwalają uniknąć hardcoding szczegółowe informacje o pracy z usługi. Funkcja użytkownik odbiera dane (na przykład zawartość komunikatu w kolejce) w parametrów funkcji. Wysyłanie danych (na przykład można utworzyć komunikatu w kolejce) przy użyciu funkcji, wartość zwracana `out` parametru lub [obiekt moduł zbierający](functions-reference-csharp.md#writing-multiple-output-values).

Podczas opracowywania funkcji przy użyciu portalu Azure, wyzwalaczy i powiązań są konfigurowane w *function.json* pliku. Portal zawiera interfejsu użytkownika dla tej konfiguracji, ale plik można edytować bezpośrednio przez zmianę na **Zaawansowany edytor**.

Podczas opracowywania funkcji za pomocą programu Visual Studio do tworzenia biblioteki klas, skonfiguruj wyzwalaczy i powiązań przez dekoracji parametry z atrybutami i metod.

## <a name="supported-bindings"></a>Obsługiwane powiązania

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Aby uzyskać informacje o tym, które są w wersji zapoznawczej powiązań, lub są zatwierdzone do użycia w środowisku produkcyjnym, zobacz [obsługiwanych języków](supported-languages.md).

## <a name="example-queue-trigger-and-table-output-binding"></a>Przykład: wyzwalacz kolejki i tabeli powiązania wyjściowego

Załóżmy, że chcesz zapisać nowy wiersz do magazynu tabel Azure przy każdym wyświetleniu nowego komunikatu w magazynie kolejek platformy Azure. W tym scenariuszu można implementować przy użyciu kolejek Azure wyzwalacza z magazynu i magazynu tabel Azure powiązania wyjściowego. 

Oto *function.json* pliku dla tego scenariusza. 

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

Pierwszym elementem w `bindings` tablica ma wyzwalacz magazynu kolejki. `type` i `direction` właściwości zidentyfikować wyzwalacza. `name` Właściwość identyfikuje parametru funkcji, które otrzymają zawartość komunikatu w kolejce. Nazwa kolejki, aby monitorować jest `queueName`, i parametry połączenia są identyfikowane przez ustawienie aplikacji `connection`.

Drugi element w `bindings` tablica jest magazyn tabel Azure powiązania wyjściowego. `type` i `direction` właściwości identyfikacji powiązania. `name` Właściwość określa, jak funkcja zapewni nowego wiersza tabeli, w tym przypadku przy użyciu wartości zwracanej funkcji. Nazwa tabeli jest `tableName`, i parametry połączenia są identyfikowane przez ustawienie aplikacji `connection`.

Aby wyświetlić i edytować zawartość *function.json* w portalu Azure kliknij **Zaawansowany edytor** opcja **integracji** kartę funkcji.

> [!NOTE]
> Wartość `connection` jest nazwą ustawienia aplikacji, które zawiera parametry połączenia, a nie parametry połączenia się. Powiązania połączenia ciągi przechowywane w ustawieniach aplikacji w celu wymuszenia najlepszych rozwiązań, które *function.json* nie zawiera kluczy tajnych usługi.

Oto C# kodu skryptu, który współpracuje z tego wyzwalacza i powiązania. Należy zauważyć, że nazwa parametru, który udostępnia zawartość komunikatu w kolejce jest `order`; ta nazwa jest wymagana, ponieważ `name` wartości właściwości w *function.json* jest`order` 

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
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

Tego samego pliku function.json można użyć z funkcją JavaScript:

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

W bibliotece klas, taki sam wyzwalacz i informacje o powiązaniu &mdash; nazwy kolejki i tabeli kont magazynu funkcji parametry wejściowe i wyjściowe &mdash; są dostarczane przez atrybuty:

```csharp
 public static class QueueTriggerTableOutput
 {
     [FunctionName("QueueTriggerTableOutput")]
     [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
     public static Person Run(
         [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order, 
         TraceWriter log)
     {
         return new Person() {
                 PartitionKey = "Orders",
                 RowKey = Guid.NewGuid().ToString(),
                 Name = order["Name"].ToString(),
                 MobileNumber = order["MobileNumber"].ToString() };
     }
 }

 public class Person
 {
     public string PartitionKey { get; set; }
     public string RowKey { get; set; }
     public string Name { get; set; }
     public string MobileNumber { get; set; }
 }
```

## <a name="binding-direction"></a>Kierunek powiązania

Wszystkich wyzwalaczy i powiązań ma `direction` właściwości w *function.json* pliku:

- Wyzwalacze kierunek jest zawsze`in`
- Użyj powiązań wejściowych i wyjściowych `in` i`out`
- Niektóre powiązania obsługuje specjalne kierunku `inout`. Jeśli używasz `inout`, tylko **Zaawansowany edytor** jest dostępna w **integracji** kartę.

Jeśli używasz [atrybutów w bibliotece klas](functions-dotnet-class-library.md) skonfigurować wyzwalaczy i powiązań, kierunek dostarczony w Konstruktorze atrybutu lub wywnioskować typu parametru.

## <a name="using-the-function-return-type-to-return-a-single-output"></a>Zwraca jeden z za pomocą zwracanego typu funkcji

Poprzedni przykład przedstawia sposób użycia funkcji zwracana wartość zapewnienie powiązania, które jest określone w danych wyjściowych *function.json* przy użyciu wartości specjalne `$return` dla `name` właściwości. (To jest tylko obsługiwana w językach, które mają wartość zwracaną, takie jak skryptu C#, JavaScript i F #.) Jeśli funkcja ma wiele powiązań danych wyjściowych, użyj `$return` tylko jednego powiązania danych wyjściowych. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Przykłady poniżej Pokaż jak przywrócić typy są używane z powiązaniami danych wyjściowych w języku C# skrypt, JavaScript i F #.

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

W środowisku .NET należy użyć typu parametru do definiowania typu danych dla danych wejściowych. Na przykład użyć `string` powiązać tekstu wyzwalacza kolejki, tablica bajtów do odczytu jako binarny i niestandardowego typu do deserializacji do obiektu POCO.

Dla języków, które są dynamicznie wpisane takich jak JavaScript, użyj `dataType` właściwości w *function.json* pliku. Na przykład można odczytać treści żądania HTTP w formacie binarnym, należy ustawić `dataType` do `binary`:

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

Można użyć tej samej metody w bibliotekach klas:

```csharp
[FunctionName("QueueTrigger")]
public static void Run(
    [QueueTrigger("%input-queue-name%")]string myQueueItem, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
}
```

## <a name="trigger-metadata-properties"></a>Właściwości metadanych wyzwalacza

Oprócz ładunku danych dostarczonych przez wyzwalacz (na przykład kolejki komunikatów, który wywołał funkcję) wiele wyzwalaczy, podaj wartości dodatkowe metadane. Te wartości może służyć jako parametry wejściowe w języku C# i F # lub właściwości na `context.bindings` obiektu w języku JavaScript. 

Na przykład wyzwalacz magazynu kolejek Azure obsługuje następujące właściwości:

* QueueTrigger - wyzwalania zawartość komunikatu, jeśli prawidłowy ciąg
* DequeueCount
* ExpirationTime
* Identyfikator
* InsertionTime
* NextVisibleTime
* PopReceipt

Te wartości metadanych są dostępne w *function.json* właściwości pliku. Na przykład użyć wyzwalacza kolejki i kolejki wiadomości zawiera nazwę obiektu blob, który chcesz odczytać. W *function.json* plików, można użyć `queueTrigger` metadanych właściwości w obiekcie blob `path` właściwości, jak pokazano w poniższym przykładzie:

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

Szczegółowe informacje o właściwości metadanych dla każdego wyzwalacza są opisane w odpowiedniej artykule. Na przykład zobacz [kolejki wyzwalacza metadanych](functions-bindings-storage-queue.md#trigger---message-metadata). Dokumentacja jest również dostępna w **integracji** kartę portalu w **dokumentacji** sekcji poniżej obszar konfiguracji powiązania.  

## <a name="binding-expressions-and-patterns"></a>Wyrażenia wiązania i wzorce

Jedną z najbardziej zaawansowanych funkcji, wyzwalaczy i powiązań jest *wyrażenia powiązania*. W konfiguracji dla powiązania można zdefiniować wzorzec wyrażenia, które mogą być następnie używane w pozostałych powiązaniach lub kodu. Wyzwalacz metadanych można również w wyrażeniach wiązania, jak pokazano w poprzedniej sekcji.

Załóżmy na przykład, aby zmienić rozmiar obrazów w szczególności kontenera magazynu obiektów blob, podobnie jak **zmiany rozmiaru obrazu** szablonu w **nową funkcję** strony portalu Azure (zobacz **próbek**  scenariusz). 

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

Zwróć uwagę, że `filename` parametr jest używany w definicji wyzwalacza obiektu blob i obiektu blob powiązania wyjściowego. Ten parametr może również w kodzie funkcji.

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

Tej samej możliwość używania wyrażeń powiązania i wzorce ma zastosowanie do atrybutów w bibliotekach klas. Na przykład w tym miejscu to obraz zmiany rozmiaru w bibliotece klas funkcji:

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```

### <a name="create-guids"></a>Tworzenie identyfikatorów GUID

`{rand-guid}` Powiązanie wyrażenia tworzy identyfikator GUID. W poniższym przykładzie użyto identyfikatora GUID, aby utworzyć nazwę unikatową obiektów blob: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a>Bieżący czas

Wyrażenie powiązania `DateTime` jest rozpoznawana jako `DateTime.UtcNow`.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-to-custom-input-properties"></a>Powiązania niestandardowe właściwości wejściowych

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

W języku C# i innych języków .NET, można użyć wzorca wiązania konieczne, w przeciwieństwie do deklaratywne powiązania w *function.json* i atrybutów. Powiązanie konieczne jest przydatne, gdy Parametry wiążące muszą ma zostać obliczony w czasie środowiska uruchomieniowego zamiast projektu. Aby dowiedzieć się więcej, zobacz [powiązania w czasie wykonywania za pośrednictwem powiązania imperatywnych](functions-reference-csharp.md#imperative-bindings) w dokumentacja dla deweloperów języka C#.

## <a name="functionjson-file-schema"></a>Schemat pliku Function.JSON

*Function.json* schemat pliku znajduje się w temacie [http://json.schemastore.org/function](http://json.schemastore.org/function).

## <a name="handling-binding-errors"></a>Obsługa błędów powiązań

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

Łącza do wszystkich odpowiednich błąd tematów dotyczących różnych usług obsługiwanych przez funkcje, zobacz [powiązanie kody błędów](functions-bindings-error-pages.md#binding-error-codes) sekcji [obsługi błędów usługi Azure Functions](functions-bindings-error-pages.md) temat.  

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na określone powiązanie zobacz następujące artykuły:

- [HTTP i elementy webhook](functions-bindings-http-webhook.md)
- [Czasomierz](functions-bindings-timer.md)
- [Queue Storage](functions-bindings-storage-queue.md)
- [Blob Storage](functions-bindings-storage-blob.md)
- [Table Storage](functions-bindings-storage-table.md)
- [Centrum zdarzeń](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [Azure Cosmos DB](functions-bindings-cosmosdb.md)
- [Microsoft Graph](functions-bindings-microsoft-graph.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Notification Hubs](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Plik zewnętrzny](functions-bindings-external-file.md)
