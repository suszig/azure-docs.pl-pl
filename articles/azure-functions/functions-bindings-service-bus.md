---
title: "Azure funkcji usługi Service Bus wyzwalaczy i powiązań | Dokumentacja firmy Microsoft"
description: "Zrozumienie, jak używać usługi Azure Service Bus wyzwalaczy i powiązań w funkcji platformy Azure."
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: "funkcje usługi Azure, funkcje, przetwarzania zdarzeń, dynamiczne obliczeń niekorzystającą architektury"
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/01/2017
ms.author: glenga
ms.openlocfilehash: 71149aaacc940a62e085cf1ce103a0214d05bd1c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-service-bus-bindings"></a>Azure powiązania funkcji usługi Service Bus
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule opisano sposób konfigurowania i pracować z usługi Azure Service Bus powiązania usługi Azure Functions. 

Usługi Azure Functions obsługuje uruchomić i dane wyjściowe powiązania dla tematów i kolejek usługi Service Bus.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a>Wyzwalacz usługi Service Bus
Wyzwalacz usługi Service Bus umożliwia odpowiada na komunikaty z kolejki usługi Service Bus lub temat. 

Wyzwalacze kolejki i tematu usługi Service Bus są definiowane przez następujące obiekty JSON w `bindings` tablicy function.json:

* *kolejka* wyzwalacz:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

* *temat* wyzwalacz:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

Pamiętaj o następujących kwestiach:

* Dla `connection`, [utworzyć ustawienie aplikacji w aplikacji funkcji](functions-how-to-use-azure-function-app-settings.md) zawierający parametry połączenia do przestrzeni nazw magistrali usług, następnie określ nazwę ustawienia aplikacji w `connection` właściwości w wyzwalacz. Możesz uzyskać, wykonując kroki opisane w parametrach połączenia [uzyskać poświadczenia zarządzania](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  Parametry połączenia muszą mieć dla przestrzeni nazw usługi Service Bus, nie są ograniczone do określonych kolejka lub temat.
  Jeśli opuścisz `connection` pusta, wyzwalacz przyjęto założenie, że domyślnego ciągu połączenia magistrali usług jest określona w aplikacji, ustawienie o nazwie `AzureWebJobsServiceBus`.
* Aby uzyskać `accessRights`, dostępne wartości to `manage` i `listen`. Wartość domyślna to `manage`, co oznacza, że `connection` ma **Zarządzaj** uprawnienia. Jeśli używasz parametry połączenia, które nie ma **Zarządzaj** zestawu uprawnień, `accessRights` do `listen`. W przeciwnym razie środowiska uruchomieniowego może się nie powieść próba wykonania tej operacji, które wymagają funkcji zarządzania prawami.

## <a name="trigger-behavior"></a>Zachowanie wyzwalacza
* **Wątkowość jednym** — domyślnie procesy środowiska uruchomieniowego funkcji wielu wiadomości jednocześnie. Aby skierować środowiska uruchomieniowego do jednoczesnego przetwarzania tylko pojedynczej kolejki lub tematu wiadomości, ustaw `serviceBus.maxConcurrentCalls` 1 w *host.json*. 
  Aby uzyskać informacje o *host.json*, zobacz [struktury folderów](functions-reference.md#folder-structure) i [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).
* **Obsługa komunikatów zanieczyszczonych** -usługi Service Bus jest jego własnej obsługi uszkodzonych komunikatów, który nie może być kontrolowana ani skonfigurowana w konfiguracji usługi Azure Functions lub kod. 
* **Zachowanie PeekLock** — środowisko uruchomieniowe Functions odbiera wiadomości w [ `PeekLock` tryb](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) i wywołania `Complete` na komunikat, jeśli funkcja zakończy działanie pomyślnie, lub wywołania `Abandon` Jeśli funkcja kończy się niepowodzeniem. 
  Jeśli funkcja uruchomione dłużej niż `PeekLock` limit czasu blokady automatycznie zostanie odnowiona.

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Użycie wyzwalacza
W tej sekcji przedstawiono sposób użycia wyzwalacz usługi Service Bus w kodzie funkcji. 

W języku C# i F # w dowolnej z następujących typów wejściowych można przeprowadzić deserializacji komunikatu wyzwalacza magistrali usług:

* `string`-przydatne w przypadku wiadomości ciąg
* `byte[]`-przydatne dla danych binarnych
* Wszelkie [obiektu](https://msdn.microsoft.com/library/system.object.aspx) — jest to przydatne w przypadku danych serializacji JSON.
  Jeśli niestandardowy typ danych wejściowych, takich jak zadeklarować `CustomType`, usługi Azure Functions próbuje deserializowanie danych JSON w sieci określonego typu.
* `BrokeredMessage`— Umożliwia zdeserializowany wiadomości z [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) metody.

W środowisku Node.js komunikat wyzwalacza usługi Service Bus jest przekazywany do funkcji jako ciąg lub obiekt JSON.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Przykładowe wyzwalacza
Załóżmy, że masz następujące function.json:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

Zobacz przykład specyficzny dla języka, który przetwarza komunikat z kolejki usługi Service Bus.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Przykładowe wyzwalacza w języku C# #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Przykładowe wyzwalacza w języku F # #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Przykładowe wyzwalacza w środowisku Node.js

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a>Powiązania wyjściowego usługi Service Bus
Wyjście kolejek i tematów usługi Service Bus dla funkcji, użyj następujących obiektów JSON w `bindings` tablicy function.json:

* *kolejka* danych wyjściowych:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```
* *temat* danych wyjściowych:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```

Pamiętaj o następujących kwestiach:

* Dla `connection`, [utworzyć ustawienie aplikacji w aplikacji funkcji](functions-how-to-use-azure-function-app-settings.md) zawierający parametry połączenia do przestrzeni nazw magistrali usług, następnie określ nazwę ustawienia aplikacji w `connection` właściwości wiązania danych wyjściowych. Możesz uzyskać, wykonując kroki opisane w parametrach połączenia [uzyskać poświadczenia zarządzania](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  Parametry połączenia muszą mieć dla przestrzeni nazw usługi Service Bus, nie są ograniczone do określonych kolejka lub temat.
  Jeśli opuścisz `connection` pusta, powiązania wyjściowego przyjęto założenie, że domyślnego ciągu połączenia magistrali usług jest określona w aplikacji, ustawienie o nazwie `AzureWebJobsServiceBus`.
* Aby uzyskać `accessRights`, dostępne wartości to `manage` i `listen`. Wartość domyślna to `manage`, co oznacza, że `connection` ma **Zarządzaj** uprawnienia. Jeśli używasz parametry połączenia, które nie ma **Zarządzaj** zestawu uprawnień, `accessRights` do `listen`. W przeciwnym razie środowiska uruchomieniowego może się nie powieść próba wykonania tej operacji, które wymagają funkcji zarządzania prawami.

<a name="outputusage"></a>

## <a name="output-usage"></a>Użycie danych wyjściowych
W języku C# i F # usługi Azure Functions można utworzyć komunikat z kolejki usługi Service Bus za pomocą dowolnego z następujących typów:

* Wszelkie [obiektu](https://msdn.microsoft.com/library/system.object.aspx) — definicja parametru wygląda `out T paramName` (C#).
  Funkcje deserializuje obiekt do wiadomości w formacie JSON. Jeśli wartość wyjściowa ma wartość null, gdy funkcja kończy, funkcje tworzy komunikat z obiektem null.
* `string`-Definicja parametru wygląda `out string paraName` (C#). Jeśli wartość parametru jest różna od null, gdy funkcja kończy, funkcje tworzy komunikat.
* `byte[]`-Definicja parametru wygląda `out byte[] paraName` (C#). Jeśli wartość parametru jest różna od null, gdy funkcja kończy, funkcje tworzy komunikat.
* `BrokeredMessage`Definicja parametru wygląda `out BrokeredMessage paraName` (C#). Jeśli wartość parametru jest różna od null, gdy funkcja kończy, funkcje tworzy komunikat.

W przypadku tworzenia wielu wiadomości w funkcji języka C#, można użyć `ICollector<T>` lub `IAsyncCollector<T>`. Komunikat jest tworzony podczas wywoływania `Add` metody.

W środowisku Node.js, można przypisać ciąg, tablica bajtów lub obiektu Javascript (deserializacji do postaci JSON) `context.binding.<paramName>`.

<a name="outputsample"></a>

## <a name="output-sample"></a>Przykładowe dane wyjściowe
Załóżmy, że masz następujące function.json, definiujący wyjściowego kolejki usługi Service Bus:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Zobacz przykładowe specyficzny dla języka, który wysyła komunikat do kolejki magistrali usług.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>Przykładowe dane wyjściowe w języku C# #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

Lub, aby utworzyć wiele komunikatów:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>Przykładowe dane wyjściowe w języku F # #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Przykładowe dane wyjściowe w środowisku Node.js

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

Lub, aby utworzyć wiele komunikatów:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = [];
    context.bindings.outputSbQueueMsg.push("1 " + message);
    context.bindings.outputSbQueueMsg.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a>Następne kroki
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

