---
title: "Azure kolejki magazynu powiązania dla usługi Azure Functions"
description: "Zrozumienie, jak używać magazynu kolejek Azure wyzwalacza i powiązania w usługi Azure Functions wyjściowego."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "funkcje usługi Azure, funkcje, przetwarzania zdarzeń, dynamiczne obliczeń niekorzystającą architektury"
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/23/2017
ms.author: glenga
ms.openlocfilehash: ce28b6eea9843ce423b57e539a844b4dacb552aa
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="azure-queue-storage-bindings-for-azure-functions"></a>Azure kolejki magazynu powiązania dla usługi Azure Functions

W tym artykule opisano sposób pracy z usługi Azure Functions powiązania magazynu kolejek Azure. Usługi Azure Functions obsługuje uruchomić i dane wyjściowe powiązania dla kolejki.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="trigger"></a>Wyzwalacz

Użyj wyzwalacza kolejki, aby uruchomić funkcję, po odebraniu nowego elementu w kolejce. Komunikat z kolejki jest podana jako dane wejściowe do funkcji.

## <a name="trigger---example"></a>Wyzwalacz — przykład

Zapoznaj się z przykładem specyficzny dla języka:

* [C#](#trigger---c-example)
* [Skryptu C# (csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Wyzwalacz — przykład C#

W poniższym przykładzie przedstawiono [C# funkcja](functions-dotnet-class-library.md) który sonduje `myqueue-items` kolejki i zapisuje dziennik za każdym razem, jest przetwarzany element kolejki.

```csharp
public static class QueueFunctions
{
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

### <a name="trigger---c-script-example"></a>Wyzwalacz — przykładowy skrypt w języku C#

W poniższym przykładzie przedstawiono wyzwalacza obiektu blob powiązanie w *function.json* pliku i [skryptu C# (csx)](functions-reference-csharp.md) kodu korzystającego z powiązania. Funkcja sond `myqueue-items` kolejki i zapisuje dziennik za każdym razem, jest przetwarzany element kolejki.

Oto *function.json* pliku:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

[Konfiguracji](#trigger---configuration) sekcji opisano te właściwości.

Oto kod skryptu C#:

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

[Użycia](#trigger---usage) sekcji opisano `myQueueItem`, które nosi nazwę przez `name` właściwości w function.json.  [Komunikatu sekcji metadanych](#trigger---message-metadata) opisano wszystkie zmienne, wyświetlane.

### <a name="trigger---javascript-example"></a>Wyzwalacz — przykład JavaScript

W poniższym przykładzie przedstawiono wyzwalacza obiektu blob powiązanie w *function.json* pliku i [funkcji JavaScript](functions-reference-node.md) używającą powiązania. Funkcja sond `myqueue-items` kolejki i zapisuje dziennik za każdym razem, jest przetwarzany element kolejki.

Oto *function.json* pliku:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

[Konfiguracji](#trigger---configuration) sekcji opisano te właściwości.

Oto kod JavaScript:

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id =', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

[Użycia](#trigger---usage) sekcji opisano `myQueueItem`, które nosi nazwę przez `name` właściwości w function.json.  [Komunikatu sekcji metadanych](#trigger---message-metadata) opisano wszystkie zmienne, wyświetlane.

## <a name="trigger---attributes"></a>Wyzwalacz — atrybuty
 
W [bibliotek klas C#](functions-dotnet-class-library.md), umożliwia skonfigurowanie wyzwalacz kolejki następujące atrybuty:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueTriggerAttribute.cs), zdefiniowany w pakiecie NuGet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs)

  Konstruktor atrybutu ma nazwę kolejki, aby monitorować, jak pokazano w poniższym przykładzie:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      TraceWriter log)
  {
      ...
  }
  ```

  Można ustawić `Connection` właściwości w celu określenia konta magazynu do użycia, jak pokazano w poniższym przykładzie:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      TraceWriter log)
  {
      ....
  }
  ```
 
  Pełny przykład, zobacz [wyzwalacza — przykład C#](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs), zdefiniowany w pakiecie NuGet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs)

  Udostępnia innym sposobem określania konta magazynu do użycia. Konstruktor przyjmuje nazwę ustawienia aplikacji, która zawiera parametry połączenia magazynu. Ten atrybut można stosować na poziomie klasy parametrów, metody lub. W poniższym przykładzie przedstawiono poziom klasy i metody:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("QueueTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

Konta magazynu do użycia jest określany w następującej kolejności:

* `QueueTrigger` Atrybutu `Connection` właściwości.
* `StorageAccount` Atrybut zastosowany do tego samego parametru jako `QueueTrigger` atrybutu.
* `StorageAccount` Atrybut zastosowany do funkcji.
* `StorageAccount` Atrybut zastosowany do klasy.
* Ustawienie aplikacji "AzureWebJobsStorage".

## <a name="trigger---configuration"></a>Wyzwalacz — Konfiguracja

W poniższej tabeli opisano powiązania właściwości konfiguracyjne, które można ustawić w *function.json* pliku i `QueueTrigger` atrybutu.

|Właściwość Function.JSON | Właściwość atrybutu |Opis|
|---------|---------|----------------------|
|**Typ** | Nie dotyczy| należy wybrać opcję `queueTrigger`. Ta właściwość ma wartość automatycznie, podczas tworzenia wyzwalacza w portalu Azure.|
|**Kierunek**| Nie dotyczy | W *function.json* tylko plików. należy wybrać opcję `in`. Ta właściwość ma wartość automatycznie, podczas tworzenia wyzwalacza w portalu Azure. |
|**Nazwa** | Nie dotyczy |Nazwa zmiennej, która reprezentuje kolejkę w kodzie funkcji.  | 
|**queueName** | **QueueName**| Nazwa kolejki do sondowania. | 
|**połączenia** | **Połączenia** |Nazwa ustawienia aplikacji, która zawiera parametry połączenia magazynu do użycia dla tego powiązania. Jeśli nazwa ustawienia aplikacji rozpoczyna się od "AzureWebJobs", można określić tylko w pozostałej części nazwy w tym miejscu. Na przykład jeśli ustawisz `connection` do "MyStorage" środowisko uruchomieniowe Functions szuka ustawienie aplikacji o nazwie "AzureWebJobsMyStorage." Jeśli opuścisz `connection` pusta, środowisko uruchomieniowe Functions używa domyślnego ciągu połączenia magazynu w ustawieniu aplikacji o nazwie `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Wyzwalacz — użycie
 
W języku C# i skryptu C#, dostęp do danych obiektów blob za pomocą parametru metody, takie jak `Stream paramName`. W języku C# skryptu `paramName` jest wartością określoną w `name` właściwość *function.json*. Można powiązać żadnego z następujących typów:

* Obiektów POCO — środowisko uruchomieniowe Functions następuje deserializacja obiektu ładunek JSON na obiekt POCO. 
* `string`
* `byte[]`
* [CloudQueueMessage]

W języku JavaScript, użyj `context.bindings.<name>` do ładunku elementu kolejki. Jeśli ładunek JSON, jest deserializacji do obiektu.

## <a name="trigger---message-metadata"></a>Wyzwalacz - metadanych wiadomości

Wyzwalacz kolejki zawiera kilka właściwości metadanych. Te właściwości mogą służyć jako część wyrażenia powiązania w pozostałych powiązaniach lub parametrów w kodzie. Wartości mają tej samej semantyki jako [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage).

|Właściwość|Typ|Opis|
|--------|----|-----------|
|`QueueTrigger`|`string`|Ładunek kolejki (jeśli prawidłowy ciąg). Jeśli kolejka komunikatów ładunek w postaci ciągu, `QueueTrigger` ma taką samą wartość jak zmienna o nazwie `name` właściwości w *function.json*.|
|`DequeueCount`|`int`|Ile razy ten komunikat został usuniętej.|
|`ExpirationTime`|`DateTimeOffset?`|Czas wygaśnięcia wiadomości.|
|`Id`|`string`|Identyfikator kolejki wiadomości.|
|`InsertionTime`|`DateTimeOffset?`|Godzina dodania wiadomości do kolejki.|
|`NextVisibleTime`|`DateTimeOffset?`|Czas, który następnie będzie widoczny komunikat.|
|`PopReceipt`|`string`|Pop odbieranie komunikatu.|

## <a name="trigger---poison-messages"></a>Wyzwalacz - skażone wiadomości

Gdy funkcja wyzwalacza kolejki nie powiodło się, usługi Azure Functions ponowi próbę funkcji maksymalnie pięć razy dla danej kolejki wiadomości, czyli pierwszej próby. Jeśli nie wszystkie próby pięć, środowisko uruchomieniowe functions dodaje komunikat do kolejki o nazwie  *&lt;originalqueuename >-skażone*. Można wpisać funkcji do przetwarzania komunikatów z kolejki skażone przez rejestrowania ich lub wysyłania powiadomienia tej uwagi ręczne jest wymagane.

Do obsługi wiadomości, ręcznie, sprawdź [dequeueCount](#trigger---message-metadata) kolejki wiadomości.

## <a name="trigger---hostjson-properties"></a>Wyzwalacz - host.json właściwości

[Host.json](functions-host-json.md#queues) plik zawiera ustawienia, które kontrolują zachowanie wyzwalacza kolejki.

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="output"></a>Dane wyjściowe

Za pomocą raportu magazynu kolejek Azure powiązanie do zapisywania wiadomości do kolejki.

## <a name="output---example"></a>OUTPUT — przykład

Zapoznaj się z przykładem specyficzny dla języka:

* [C#](#output---c-example)
* [Skryptu C# (csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Dane wyjściowe — przykład C#

W poniższym przykładzie przedstawiono [C# funkcja](functions-dotnet-class-library.md) tworzącą komunikatu w kolejce dla każdego żądania HTTP odebrane.

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }
}
```

### <a name="output---c-script-example"></a>Dane wyjściowe — przykładowy skrypt w języku C#

W poniższym przykładzie przedstawiono wyzwalacza obiektu blob powiązanie w *function.json* pliku i [skryptu C# (csx)](functions-reference-csharp.md) kodu korzystającego z powiązania. Funkcja tworzy element kolejki z ładunku POCO dla każdego żądania HTTP odebrane.

Oto *function.json* pliku:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting",
    }
  ]
}
``` 

[Konfiguracji](#output---configuration) sekcji opisano te właściwości.

Oto C# kodu skryptu, który tworzy pojedynczy komunikat z kolejki:

```cs
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, TraceWriter log)
{
    return input;
}
```

Wiele wiadomości można wysyłać jednocześnie za pomocą `ICollector` lub `IAsyncCollector` parametru. W tym kodu C# skrypt wysyłanej przez wiele komunikatów, jeden z danymi żądania HTTP i jeden z wpisaną na stałe wartości:

```cs
public static void Run(
    CustomQueueMessage input, 
    ICollector<CustomQueueMessage> myQueueItem, 
    TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

### <a name="output---javascript-example"></a>Dane wyjściowe — przykład JavaScript

W poniższym przykładzie przedstawiono wyzwalacza obiektu blob powiązanie w *function.json* pliku i [funkcji JavaScript](functions-reference-node.md) używającą powiązania. Funkcja tworzy element kolejki dla każdego żądania HTTP odebrane.

Oto *function.json* pliku:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting",
    }
  ]
}
``` 

[Konfiguracji](#output---configuration) sekcji opisano te właściwości.

Oto kod JavaScript:

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Wiele wiadomości można wysyłać na raz, definiując tablicą komunikat dla `myQueueItem` powiązania wyjściowego. Następujący kod JavaScript wysyła dwa komunikaty kolejki z wpisaną na stałe wartości dla każdego żądania HTTP odebrane.

```javascript
module.exports = function(context) {
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="output---attributes"></a>Dane wyjściowe — atrybuty
 
W [bibliotek klas C#](functions-dotnet-class-library.md), użyj [QueueAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), która jest zdefiniowana w pakiecie NuGet [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs).

Ten atrybut ma zastosowanie do `out` parametrów lub wartości zwracanej przez funkcję. Konstruktor atrybutu ma nazwę kolejki, jak pokazano w poniższym przykładzie:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

Można ustawić `Connection` właściwości w celu określenia konta magazynu do użycia, jak pokazano w poniższym przykładzie:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items", Connection = "StorageConnectionAppSetting")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

Pełny przykład, zobacz [dane wyjściowe — przykład C#](#output---c-example).

Można użyć `StorageAccount` atrybutu, aby określić konto magazynu na poziomie klasy, metody lub parametru. Aby uzyskać więcej informacji, zobacz [wyzwalacza — atrybuty](#trigger---attribute).

## <a name="output---configuration"></a>OUTPUT — Konfiguracja

W poniższej tabeli opisano powiązania właściwości konfiguracyjne, które można ustawić w *function.json* pliku i `Queue` atrybutu.

|Właściwość Function.JSON | Właściwość atrybutu |Opis|
|---------|---------|----------------------|
|**Typ** | Nie dotyczy | należy wybrać opcję `queue`. Ta właściwość ma wartość automatycznie, podczas tworzenia wyzwalacza w portalu Azure.|
|**Kierunek** | Nie dotyczy | należy wybrać opcję `out`. Ta właściwość ma wartość automatycznie, podczas tworzenia wyzwalacza w portalu Azure. |
|**Nazwa** | Nie dotyczy | Nazwa zmiennej, która reprezentuje kolejkę w kodzie funkcji. Ustaw `$return` odwoływać się do wartości zwracane funkcji.| 
|**queueName** |**QueueName** | Nazwa kolejki. | 
|**połączenia** | **Połączenia** |Nazwa ustawienia aplikacji, która zawiera parametry połączenia magazynu do użycia dla tego powiązania. Jeśli nazwa ustawienia aplikacji rozpoczyna się od "AzureWebJobs", można określić tylko w pozostałej części nazwy w tym miejscu. Na przykład jeśli ustawisz `connection` do "MyStorage" środowisko uruchomieniowe Functions szuka ustawienie aplikacji o nazwie "AzureWebJobsMyStorage." Jeśli opuścisz `connection` pusta, środowisko uruchomieniowe Functions używa domyślnego ciągu połączenia magazynu w ustawieniu aplikacji o nazwie `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Dane wyjściowe — użycie
 
W języku C# i skryptu C#, należy zapisać pojedynczy komunikat z kolejki przy użyciu parametru metody takie jak `out T paramName`. W języku C# skryptu `paramName` jest wartością określoną w `name` właściwość *function.json*. Można użyć zwracany typ metody zamiast `out` parametru i `T` może być dowolny z następujących typów:

* POCO do serializacji w formacie JSON
* `string`
* `byte[]`
* [CloudQueueMessage] 

W języku C# i skryptu C# zapisu wielu komunikatów z kolejki przy użyciu jednej z następujących typów: 

* `ICollector<T>` lub `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

W funkcji JavaScript, za pomocą `context.bindings.<name>` dostępu do danych wyjściowych kolejki wiadomości. Ciąg lub obiekt możliwy do serializacji JSON można użyć do ładunku elementu kolejki.


## <a name="exceptions-and-return-codes"></a>Wyjątki i kody powrotne

| Powiązanie |  Informacje ogólne |
|---|---|
| Kolejka | [Kody błędów kolejki](https://docs.microsoft.com/rest/api/storageservices/fileservices/table-service-error-codes) |
| Obiekt blob, tabeli, kolejki | [Kody błędów magazynu](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Obiekt blob, tabeli, kolejki |  [Rozwiązywanie problemów](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Przejdź do szybkiego startu, używającej wyzwalacz kolejki magazynu](functions-create-storage-queue-triggered-function.md)

> [!div class="nextstepaction"]
> [Przejdź do samouczka, który używa magazynu kolejek powiązania wyjściowego](functions-integrate-storage-queue-output-binding.md)

> [!div class="nextstepaction"]
> [Dowiedz się więcej o usługę Azure functions wyzwalaczy i powiązań](functions-triggers-bindings.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
