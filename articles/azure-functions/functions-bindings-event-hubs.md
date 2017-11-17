---
title: "Azure powiązania centra zdarzeń funkcji"
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
ms.date: 11/08/2017
ms.author: wesmc
ms.openlocfilehash: c2660a3ca8ee7569d49a6998d0dfd5a98a97d294
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="azure-functions-event-hubs-bindings"></a>Azure powiązania centra zdarzeń funkcji

W tym artykule opisano sposób pracy z [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) powiązania dla usługi Azure Functions. Usługi Azure Functions obsługuje uruchomić i dane wyjściowe powiązania usługi Event hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="event-hubs-trigger"></a>Wyzwalacz centra zdarzeń

Wyzwalacz usługi Event Hubs umożliwia odpowiadanie na zdarzenia wysłanego do strumienia zdarzeń w Centrum zdarzeń. Musi mieć dostęp do odczytu do Centrum zdarzeń, aby skonfigurować wyzwalacz.

Po wyzwoleniu funkcja wyzwalacza usługi Event Hubs komunikat, który je uruchamia jest przekazywany do funkcji jako ciąg.

## <a name="trigger---example"></a>Wyzwalacz — przykład

Zapoznaj się z przykładem specyficzny dla języka:

* [Prekompilowane C#](#trigger---c-example)
* [Skryptu C#](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Wyzwalacz — przykład C#

W poniższym przykładzie przedstawiono [wstępnie skompilowana C#](functions-dotnet-class-library.md) kod, który rejestruje treść wyzwalacz Centrum zdarzeń.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Uzyskanie dostępu do metadanych zdarzeń powiązać [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) obiektu (wymaga `using` instrukcji dla `Microsoft.ServiceBus.Messaging`).

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```
Aby odbierać zdarzenia w partii, `string` lub `EventData` tablicy:

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---c-script-example"></a>Wyzwalacz — przykładowy skrypt w języku C#

W poniższym przykładzie przedstawiono wyzwalacz Centrum zdarzeń powiązanie w *function.json* pliku i [funkcji skryptu C#](functions-reference-csharp.md) używającą powiązania. Funkcja rejestruje treść wyzwalacz Centrum zdarzeń.

W tym miejscu jest powiązanie danych *function.json* pliku:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```
Oto kod skryptu C#:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Uzyskanie dostępu do metadanych zdarzeń powiązać [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) obiektu (wymaga użycia instrukcji dla `Microsoft.ServiceBus.Messaging`).

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

Aby odbierać zdarzenia w partii, `string` lub `EventData` tablicy:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---f-example"></a>Wyzwalacz — przykład F #

W poniższym przykładzie przedstawiono wyzwalacz Centrum zdarzeń powiązanie w *function.json* pliku i [F # funkcja](functions-reference-fsharp.md) używającą powiązania. Funkcja rejestruje treść wyzwalacz Centrum zdarzeń.

W tym miejscu jest powiązanie danych *function.json* pliku:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Oto kod F #:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### <a name="trigger---javascript-example"></a>Wyzwalacz — przykład JavaScript

W poniższym przykładzie przedstawiono wyzwalacz Centrum zdarzeń powiązanie w *function.json* pliku i [funkcji JavaScript](functions-reference-node.md) używającą powiązania. Funkcja rejestruje treść wyzwalacz Centrum zdarzeń.

W tym miejscu jest powiązanie danych *function.json* pliku:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Oto kod JavaScript:

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

## <a name="trigger---attributes-for-precompiled-c"></a>Wyzwalacz — atrybuty dla prekompilowanego C#

Dla [wstępnie skompilowana C#](functions-dotnet-class-library.md) funkcje, używają [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs) atrybut, który jest zdefiniowany w pakiecie NuGet [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

Konstruktor atrybutu ma nazwę Centrum zdarzeń, nazwę grupy odbiorców i nazwa ustawienia aplikacji, która zawiera parametry połączenia. Aby uzyskać więcej informacji o tych ustawieniach, zobacz [wyzwolenia sekcji konfiguracji](#trigger---configuration). Oto `EventHubTriggerAttribute` przykład atrybutu:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
```

## <a name="trigger---configuration"></a>Wyzwalacz — Konfiguracja

W poniższej tabeli opisano powiązania właściwości konfiguracyjne, które można ustawić w *function.json* pliku i `EventHubTrigger` atrybutu.

|Właściwość Function.JSON | Właściwość atrybutu |Opis|
|---------|---------|----------------------|
|**Typ** | Brak | należy wybrać opcję `eventHubTrigger`. Ta właściwość ma wartość automatycznie, podczas tworzenia wyzwalacza w portalu Azure.|
|**Kierunek** | Brak | należy wybrać opcję `in`. Ta właściwość ma wartość automatycznie, podczas tworzenia wyzwalacza w portalu Azure. |
|**Nazwa** | Brak | Nazwa zmiennej, która reprezentuje element zdarzeń w kodzie funkcji. | 
|**Ścieżka** |**EventHubName** | Nazwa Centrum zdarzeń. | 
|**grupy konsumentów** |**Grupy konsumentów** | Opcjonalna właściwość, która ustawia [grupy odbiorców](../event-hubs/event-hubs-features.md#event-consumers) umożliwia subskrybowanie zdarzeń w Centrum. Pominięcie `$Default` służy grupy odbiorców. | 
|**połączenia** |**Połączenia** | Nazwa ustawienia aplikacji, które zawiera parametry połączenia do Centrum zdarzeń w przestrzeni nazw. Skopiować te parametry połączenia, klikając **informacje o połączeniu** przycisk dla *przestrzeni nazw*, nie Centrum zdarzeń samej siebie. Ten ciąg połączenia musi mieć co najmniej uprawnienia do odczytu wyzwalacz.<br/>Gdy tworzony jest lokalnie, ustawienia aplikacji przejdź do wartości [pliku local.settings.json](functions-run-local.md#local-settings-file).|

## <a name="trigger---hostjson-properties"></a>Wyzwalacz - host.json właściwości

[Host.json](functions-host-json.md#eventhub) plik zawiera ustawienia, które kontrolują zachowanie wyzwalacza centrów zdarzeń.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="event-hubs-output-binding"></a>Centra zdarzeń powiązania wyjściowego

Za pomocą raportu usługi Event Hubs powiązanie się zapisać zdarzeń do strumienia zdarzeń. Musi mieć uprawnienie wysyłania do Centrum zdarzeń można zapisać zdarzenia.

## <a name="output---example"></a>OUTPUT — przykład

Zapoznaj się z przykładem specyficzny dla języka:

* [Prekompilowane C#](#output---c-example)
* [Skryptu C#](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Dane wyjściowe — przykład C#

W poniższym przykładzie przedstawiono [wstępnie skompilowana funkcja C#](functions-dotnet-class-library.md) który zapisuje komunikat w Centrum zdarzeń za pomocą zwracana wartość metody jako dane wyjściowe:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

### <a name="output---c-script-example"></a>Dane wyjściowe — przykładowy skrypt w języku C#

W poniższym przykładzie przedstawiono wyzwalacz Centrum zdarzeń powiązanie w *function.json* pliku i [funkcji skryptu C#](functions-reference-csharp.md) używającą powiązania. Funkcja zapisuje komunikat w Centrum zdarzeń.

W tym miejscu jest powiązanie danych *function.json* pliku:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Oto C# kodu skryptu, który tworzy jeden komunikat:

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

W tym C# kodu skryptu, który tworzy wiele komunikatów:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Dane wyjściowe — przykład F #

W poniższym przykładzie przedstawiono wyzwalacz Centrum zdarzeń powiązanie w *function.json* pliku i [F # funkcja](functions-reference-fsharp.md) używającą powiązania. Funkcja zapisuje komunikat w Centrum zdarzeń.

W tym miejscu jest powiązanie danych *function.json* pliku:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Oto kod F #:

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

### <a name="output---javascript-example"></a>Dane wyjściowe — przykład JavaScript

W poniższym przykładzie przedstawiono wyzwalacz Centrum zdarzeń powiązanie w *function.json* pliku i [funkcji JavaScript](functions-reference-node.md) używającą powiązania. Funkcja zapisuje komunikat w Centrum zdarzeń.

W tym miejscu jest powiązanie danych *function.json* pliku:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Kod JavaScript w tym wysyła pojedynczy komunikat:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Kod JavaScript w tym wysyła wiele komunikatów:

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

## <a name="output---attributes-for-precompiled-c"></a>Dane wyjściowe — atrybuty dla prekompilowanego C#

Dla [wstępnie skompilowana C#](functions-dotnet-class-library.md) funkcje, używają [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs) atrybut, który jest zdefiniowany w pakiecie NuGet [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

Konstruktor atrybutu ma nazwę Centrum zdarzeń i nazwa ustawienia aplikacji, która zawiera parametry połączenia. Aby uzyskać więcej informacji o tych ustawieniach, zobacz [wyjście - konfiguracji](#output---configuration). Oto `EventHub` przykład atrybutu:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
```

## <a name="output---configuration"></a>OUTPUT — Konfiguracja

W poniższej tabeli opisano powiązania właściwości konfiguracyjne, które można ustawić w *function.json* pliku i `EventHub` atrybutu.

|Właściwość Function.JSON | Właściwość atrybutu |Opis|
|---------|---------|----------------------|
|**Typ** | Brak | Musi być równa "eventHub". |
|**Kierunek** | Brak | Należy wybrać opcję "out". Ten parametr jest ustawiany automatycznie, podczas tworzenia powiązania w portalu Azure. |
|**Nazwa** | Brak | Nazwa zmiennej używana w kodzie funkcja, która reprezentuje zdarzenia. | 
|**Ścieżka** |**EventHubName** | Nazwa Centrum zdarzeń. | 
|**połączenia** |**Połączenia** | Nazwa ustawienia aplikacji, które zawiera parametry połączenia do Centrum zdarzeń w przestrzeni nazw. Skopiować te parametry połączenia, klikając **informacje o połączeniu** przycisk dla *przestrzeni nazw*, nie Centrum zdarzeń samej siebie. Ten ciąg połączenia musi mieć uprawnienia wysyłania do wysłania tej wiadomości do strumienia zdarzeń.<br/>Gdy tworzony jest lokalnie, ustawienia aplikacji przejdź do wartości [pliku local.settings.json](functions-run-local.md#local-settings-file).|

## <a name="output---usage"></a>Dane wyjściowe — użycie

W języku C# i skryptu C#, wysyłanie wiadomości przy użyciu parametru metody, takie jak `out string paramName`. W języku C# skryptu `paramName` jest wartością określoną w `name` właściwość *function.json*. Aby zapisać wiele wiadomości, można użyć `ICollector<string>` lub `IAsyncCollector<string>` zamiast `out string`.

W języku JavaScript, dostęp do danych wyjściowych zdarzeń przy użyciu `context.bindings.<name>`. `<name>`wartość jest określona w `name` właściwość *function.json*.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Dowiedz się więcej o usługę Azure functions wyzwalaczy i powiązań](functions-triggers-bindings.md)
