---
title: "Środowisko Azure Functions kolejki magazynu powiązania | Dokumentacja firmy Microsoft"
description: "Zrozumienie, jak używać usługi Azure Storage wyzwalaczy i powiązań w funkcji platformy Azure."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "funkcje usługi Azure, funkcje, przetwarzania zdarzeń, dynamiczne obliczeń niekorzystającą architektury"
ms.assetid: 4e6a837d-e64f-45a0-87b7-aa02688a75f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: glenga
ms.openlocfilehash: b68ce106ceb25d19ee0bbde287891d553a448560
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/20/2017
---
# <a name="azure-functions-queue-storage-bindings"></a>Powiązania funkcji magazynu kolejek Azure
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule opisano sposób konfigurowania i powiązania magazynu kolejek Azure kodu usługi Azure Functions. Usługi Azure Functions obsługuje uruchomić i dane wyjściowe powiązań dla kolejek platformy Azure. Na temat funkcji, które są dostępne w wszystkie powiązania [usługi Azure Functions wyzwalaczy i powiązań pojęcia](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="queue-storage-trigger"></a>Kolejki magazynu wyzwalacza
Wyzwalacz magazynu kolejek Azure umożliwia monitorowanie magazynu kolejek, nowe wiadomości i szybkiego reagowania na. 

Zdefiniuj wyzwalacza kolejki przy użyciu **integracji** kartę w portalu funkcji. Portal tworzy następującą definicję w **powiązania** sekcji *function.json*:

```json
{
    "type": "queueTrigger",
    "direction": "in",
    "name": "<The name used to identify the trigger data in your code>",
    "queueName": "<Name of queue to poll>",
    "connection":"<Name of app setting - see below>"
}
```

* `connection` Właściwości musi zawierać nazwę ustawienia aplikacji, która zawiera parametry połączenia magazynu. W portalu Azure, standardowego edytora w **integracji** kartę konfiguruje to ustawienie aplikacji, można wybrać konto magazynu.

Można podać dodatkowe ustawienia w [pliku host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) dopasować kolejki magazynu wyzwalaczy. Na przykład można zmienić interwał w host.json sondowania kolejki.

<a name="triggerusage"></a>

## <a name="using-a-queue-trigger"></a>Użycie wyzwalacza kolejki
W przypadku funkcji Node.js uzyskiwać dostęp za pomocą danych kolejki `context.bindings.<name>`.


W przypadku funkcji .NET dostępu ładunku kolejki przy użyciu parametru metody, takich jak `CloudQueueMessage paramName`. W tym miejscu `paramName` jest wartością określoną w [konfiguracji wyzwalacza](#trigger). Komunikat z kolejki może być zdeserializowany do żadnej z następujących typów:

* Obiekt POCO. Użyj, jeśli ładunek kolejki jest obiekt JSON. Środowisko uruchomieniowe Functions deserializuje ładunku w obiekcie POCO. 
* `string`
* `byte[]`
* [`CloudQueueMessage`]

<a name="meta"></a>

### <a name="queue-trigger-metadata"></a>Metadane wyzwalacza kolejki
Wyzwalacz kolejki zawiera kilka właściwości metadanych. Te właściwości mogą służyć jako część wyrażenia powiązania w pozostałych powiązaniach lub parametrów w kodzie. Wartości mają tej samej semantyki jako [ `CloudQueueMessage` ].

* **QueueTrigger** -ładunku kolejki (jeśli prawidłowy ciąg)
* **DequeueCount** — typ `int`. Ile razy ten komunikat został usuniętej.
* **ExpirationTime** — typ `DateTimeOffset?`. Czas wygaśnięcia wiadomości.
* **Identyfikator** — typ `string`. Identyfikator kolejki wiadomości.
* **InsertionTime** — typ `DateTimeOffset?`. Godzina dodania wiadomości do kolejki.
* **NextVisibleTime** — typ `DateTimeOffset?`. Czas, który następnie będzie widoczny komunikat.
* **Elementu PopReceipt** — typ `string`. Pop odbieranie komunikatu.

Jak używać metadanych kolejki w [próbki wyzwalacza](#triggersample).

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Przykładowe wyzwalacza
Załóżmy, że masz następujące function.json, który definiuje wyzwalacz kolejki:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionString"
        }
    ]
}
```

Przykładu specyficzny dla języka, które są pobierane i dzienniki kolejka metadanych.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Przykładowe wyzwalacza w języku C# #
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

<!--
<a name="triggerfsharp"></a>
### Trigger sample in F# ## 
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Przykładowe wyzwalacza w środowisku Node.js

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id=', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

### <a name="handling-poison-queue-messages"></a>Obsługa wiadomości w kolejce skażone
Gdy funkcja wyzwalacza kolejki nie powiodło się, usługi Azure Functions ponowi próbę funkcji maksymalnie pięć razy dla danej kolejki wiadomości, czyli pierwszej próby. Jeśli wszystkie próby pięć zakończą się niepowodzeniem, środowisko uruchomieniowe functions dodaje komunikat do kolejki magazynu o nazwie  *&lt;originalqueuename >-skażone*. Można wpisać funkcji do przetwarzania komunikatów z kolejki skażone przez rejestrowania ich lub wysyłania powiadomienia tej uwagi ręczne jest wymagane. 

Do obsługi wiadomości, ręcznie, sprawdź `dequeueCount` wiadomości w kolejce (zobacz [kolejki wyzwalacza metadanych](#meta)).

<a name="output"></a>

## <a name="queue-storage-output-binding"></a>Magazyn kolejek powiązania wyjściowego
Magazyn kolejek Azure powiązania danych wyjściowych umożliwia pisanie wiadomości do kolejki. 

Zdefiniuj kolejki danych wyjściowych powiązanie using **integracji** kartę w portalu funkcji. Portal tworzy następującą definicję w **powiązania** sekcji *function.json*:

```json
{
   "type": "queue",
   "direction": "out",
   "name": "<The name used to identify the trigger data in your code>",
   "queueName": "<Name of queue to write to>",
   "connection":"<Name of app setting - see below>"
}
```

* `connection` Właściwości musi zawierać nazwę ustawienia aplikacji, która zawiera parametry połączenia magazynu. W portalu Azure, standardowego edytora w **integracji** kartę konfiguruje to ustawienie aplikacji, można wybrać konto magazynu.

<a name="outputusage"></a>

## <a name="using-a-queue-output-binding"></a>Przy użyciu kolejki powiązania wyjściowego
W przypadku funkcji Node.js dostęp przy użyciu kolejki danych wyjściowych `context.bindings.<name>`.

W przypadku funkcji .NET można dane wyjściowe do żadnego z następujących typów. Po parametrze typu `T`, `T` musi być jednego z typów obsługiwanych wyjścia, takich jak `string` lub POCO.

* `out T`(zserializowanym w formacie JSON)
* `out string`
* `out byte[]`
* `out` [`CloudQueueMessage`] 
* `ICollector<T>`
* `IAsyncCollector<T>`
* [`CloudQueue`](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

Umożliwia także typ zwracany metody w powiązaniu danych wyjściowych.

<a name="outputsample"></a>

## <a name="queue-output-sample"></a>Przykładowe dane wyjściowe kolejki
Następujące *function.json* definiuje wyzwalacza HTTP z kolejki powiązania wyjściowego:

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
      "connection": "MyStorageConnectionString",
    }
  ]
}
``` 

Zobacz przykładowe specyficzny dla języka, który wyprowadza komunikatu w kolejce Przychodzące ładunek HTTP.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="queue-output-sample-in-c"></a>Przykładowe dane wyjściowe kolejek w języku C# #

```cs
// C# example of HTTP trigger binding to a custom POCO, with a queue output binding
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

Aby wysłać wiele wiadomości, należy użyć `ICollector`:

```cs
public static void Run(CustomQueueMessage input, ICollector<CustomQueueMessage> myQueueItem, TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

<a name="outnodejs"></a>

### <a name="queue-output-sample-in-nodejs"></a>Przykładowe dane wyjściowe kolejki w środowisku Node.js

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Lub, w celu wysłania wiele komunikatów

```javascript
module.exports = function(context) {
    // Define a message array for the myQueueItem output binding. 
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="next-steps"></a>Następne kroki

Na przykład funkcja, która używa kolejki magazynu wyzwalaczy i powiązań zobacz [tworzy wyzwalane przez magazynu kolejek Azure funkcję](functions-create-storage-queue-triggered-function.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

<!-- LINKS -->

["CloudQueueMessage"]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
