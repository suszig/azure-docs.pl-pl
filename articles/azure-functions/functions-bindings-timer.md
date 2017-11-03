---
title: Azure wyzwalacza czasomierza funkcje | Dokumentacja firmy Microsoft
description: "Zrozumienie, jak używać czasomierza Wyzwalacze w funkcji Azure."
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: "funkcje usługi Azure, funkcje, przetwarzania zdarzeń, dynamiczne obliczeń niekorzystającą architektury"
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: glenga
ms.custom: 
ms.openlocfilehash: 12beb090a95a31c7e83ae03a920016bdfbf474e3
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="azure-functions-timer-trigger"></a>Azure wyzwalacza czasomierza funkcji

[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule opisano sposób konfigurowania i wyzwalaczy czasomierza kodu w usługi Azure Functions. Środowisko Azure Functions ma powiązanie wyzwalacza czasomierza umożliwia uruchamianie funkcji kodu na podstawie zdefiniowanego harmonogramu. 

Wyzwalacz czasomierza obsługuje wiele wystąpień skalowalnego w poziomie. Pojedyncze wystąpienie funkcji określonego czasomierza jest uruchamiane we wszystkich wystąpieniach.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a id="trigger"></a>

## <a name="timer-trigger"></a>Wyzwalacz czasomierza
Wyzwalacz czasomierza funkcji używa następujący obiekt JSON w `bindings` tablicy function.json:

```json
{
    "schedule": "<CRON expression - see below>",
    "name": "<Name of trigger parameter in function signature>",
    "type": "timerTrigger",
    "direction": "in"
}
```

Wartość `schedule` jest [wyrażenie CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) zawierających te sześć pola: 

    {second} {minute} {hour} {day} {month} {day-of-week}
&nbsp;
>[!NOTE]   
>Wiele wyrażeń cron możesz znaleźć online Pomiń `{second}` pola. Po skopiowaniu jednego z nich, należy dostosować nadmiarowe `{second}` pola. Aby uzyskać szczegółowe przykłady, zobacz [zaplanować przykłady](#examples) poniżej.

Domyślna strefa czasowa używane w wyrażeniach CRON jest uniwersalny czas koordynowany (UTC). Aby wymusić wyrażenie CRON oparte na innej strefie czasowej, Utwórz nowe ustawienie aplikacji dla aplikacji funkcja o nazwie `WEBSITE_TIME_ZONE`. Ustaw wartość na nazwę odpowiednie strefy czasowej, jak pokazano w [indeksu strefy czasowej Microsoft](https://technet.microsoft.com/library/cc749073(v=ws.10).aspx). 

Na przykład *wschodni czas standardowy* jest UTC-05:00. Aby Twoje czasomierza wyzwolenia fire na 10:00 CET codziennie, użyj następujących kont dla strefy czasowej UTC wyrażenie CRON:

```json
"schedule": "0 0 15 * * *",
``` 

Alternatywnie możesz dodać nowe ustawienie aplikacji dla aplikacji funkcja o nazwie `WEBSITE_TIME_ZONE` i ustaw wartość **wschodni czas standardowy**.  Poniższe wyrażenie CRON można następnie używać do 10:00 AM EST: 

```json
"schedule": "0 0 10 * * *",
``` 


<a name="examples"></a>

## <a name="schedule-examples"></a>Przykłady harmonogramu
Poniżej przedstawiono niektóre przykłady można użyć dla wyrażeń CRON `schedule` właściwości. 

Aby wyzwolić co pięć minut:

```json
"schedule": "0 */5 * * * *"
```

Aby wyzwolić raz, w górnej części co godzinę:

```json
"schedule": "0 0 * * * *",
```

Aby wyzwolić co dwie godziny:

```json
"schedule": "0 0 */2 * * *",
```

Aby wyzwolić co godzinę z 9 AM do 17: 00:

```json
"schedule": "0 0 9-17 * * *",
```

Aby wyzwolić na 9:30 AM codziennie:

```json
"schedule": "0 30 9 * * *",
```

Aby wyzwolić na 9:30 AM każdy dzień tygodnia:

```json
"schedule": "0 30 9 * * 1-5",
```

<a name="usage"></a>

## <a name="trigger-usage"></a>Użycie wyzwalacza
Po wywołaniu funkcji wyzwalacza czasomierza [obiekt czasomierza](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) została przekazana do funkcji. Następujący kod JSON jest przykład reprezentację obiekt czasomierza. 

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00.012699+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

<a name="sample"></a>

## <a name="trigger-sample"></a>Przykładowe wyzwalacza
Załóżmy, że masz następujące wyzwalacza czasomierza `bindings` tablicy function.json:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Zobacz próbka specyficzny dla języka, która odczytuje obiekt czasomierza, aby zobaczyć, czy działa późne.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Przykładowe wyzwalacza w języku C# #
```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}" );  
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>Przykładowe wyzwalacza w języku F # #
```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Przykładowe wyzwalacza w środowisku Node.js
```JavaScript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);   

    context.done();
};
```

## <a name="next-steps"></a>Następne kroki
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

