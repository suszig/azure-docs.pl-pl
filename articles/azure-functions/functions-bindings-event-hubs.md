---
title: "Azure powiązania funkcji Event Hubs | Dokumentacja firmy Microsoft"
description: "Zrozumienie, jak używać usługi Azure Event Hubs powiązania w usługi Azure Functions."
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: "funkcje usługi Azure, funkcje, przetwarzania zdarzeń, dynamiczne obliczeń niekorzystającą architektury"
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/20/2017
ms.author: wesmc
ms.openlocfilehash: 85eb6985ef3579b1b2313db3ce5f91c3471da72f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-event-hubs-bindings"></a>Azure powiązania centra zdarzeń funkcji
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule opisano sposób konfigurowania i użyj [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) powiązania dla usługi Azure Functions.
Usługi Azure Functions obsługuje uruchomić i dane wyjściowe powiązania usługi Event hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Jeśli jesteś nowym użytkownikiem usługi Azure Event Hubs, zobacz [Przegląd usługi Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

<a name="trigger"></a>

## <a name="event-hub-trigger"></a>Wyzwalacz Centrum zdarzeń
Wyzwalacz usługi Event Hubs umożliwia odpowiadanie na zdarzenia wysłanego do strumienia zdarzeń w Centrum zdarzeń. Musi mieć dostęp do odczytu do Centrum zdarzeń, aby skonfigurować wyzwalacz.

Wyzwalacz funkcji usługi Event Hubs używa następujący obiekt JSON w `bindings` tablicy function.json:

```json
{
    "type": "eventHubTrigger",
    "name": "<Name of trigger parameter in function signature>",
    "direction": "in",
    "path": "<Name of the event hub>",
    "consumerGroup": "Consumer group to use - see below",
    "connection": "<Name of app setting with connection string - see below>"
}
```

`consumerGroup`jest to opcjonalna właściwość służy do określania [grupy odbiorców](../event-hubs/event-hubs-features.md#event-consumers) umożliwia subskrybowanie zdarzeń w Centrum. Pominięcie `$Default` służy grupy odbiorców.  
`connection`musi być nazwą ustawienia aplikacji, które zawiera parametry połączenia do Centrum zdarzeń w przestrzeni nazw.
Skopiować te parametry połączenia, klikając **informacje o połączeniu** przycisk dla *przestrzeni nazw*, nie Centrum zdarzeń samej siebie. Ten ciąg połączenia musi mieć co najmniej uprawnienia do odczytu wyzwalacz.

[Dodatkowe ustawienia](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) można podać w pliku host.json dalszego dostosowania wyzwalaczy usługi Event Hubs.  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Użycie wyzwalacza
Po wyzwoleniu funkcja wyzwalacza usługi Event Hubs komunikat, który je uruchamia jest przekazywany do funkcji jako ciąg.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Przykładowe wyzwalacza
Załóżmy, że masz następujące usługi Event Hubs wyzwalacz w `bindings` tablicy function.json:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Patrz zaloguje się treść wyzwalacz Centrum zdarzeń próbki specyficzny dla języka.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Przykładowe wyzwalacza w języku C# #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Można również odbierać zdarzeń jako [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) obiektu, który umożliwia dostęp do metadanych zdarzeń.

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

Aby odbierać zdarzenia w partii, Zmień podpis metody `string[]` lub `EventData[]`.

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Przykładowe wyzwalacza w języku F # #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Przykładowe wyzwalacza w środowisku Node.js

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hubs-output-binding"></a>Centra zdarzeń powiązania wyjściowego
Za pomocą raportu usługi Event Hubs powiązanie się zapisać zdarzeń do strumienia zdarzeń w Centrum zdarzeń. Musi mieć uprawnienie wysyłania do Centrum zdarzeń można zapisać zdarzenia.

Powiązanie danych wyjściowych używa następujący obiekt JSON w `bindings` tablicy function.json:

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

`connection`musi być nazwą ustawienia aplikacji, które zawiera parametry połączenia do Centrum zdarzeń w przestrzeni nazw.
Skopiować te parametry połączenia, klikając **informacje o połączeniu** przycisk dla *przestrzeni nazw*, nie Centrum zdarzeń samej siebie. Ten ciąg połączenia musi mieć uprawnienia wysyłania do wysłania tej wiadomości do strumienia zdarzeń.

## <a name="output-usage"></a>Użycie danych wyjściowych
W tej sekcji przedstawiono sposób użycia danych wyjściowych usługi Event Hubs powiązania w kodzie funkcji.

Możesz można komunikaty wyjściowe do Centrum zdarzeń skonfigurowany z następującymi typami parametrów:

* `out string`
* `ICollector<string>`(aby wielu komunikaty wyjściowe)
* `IAsyncCollector<string>`(wersja async `ICollector<T>`)

<a name="outputsample"></a>

## <a name="output-sample"></a>Przykładowe dane wyjściowe
Załóżmy, że masz następujące usługi Event Hubs output powiązanie w `bindings` tablicy function.json:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Zobacz przykład specyficzny dla języka, która zapisuje zdarzenia do strumienia nawet.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Przykładowe dane wyjściowe w języku C# #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Lub, aby utworzyć wiele komunikatów:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Przykładowe dane wyjściowe w języku F # #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a>Przykładowe dane wyjściowe dla środowiska Node.js

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Lub, w celu wysłania wiele komunikatów

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a>Następne kroki
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
