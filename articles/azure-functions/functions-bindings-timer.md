---
title: "Wyzwalacz czasomierza dla usługi Azure Functions"
description: "Zrozumienie, jak używać czasomierza Wyzwalacze w funkcji Azure."
services: functions
documentationcenter: na
author: tdykstra
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
ms.author: tdykstra
ms.custom: 
ms.openlocfilehash: bd1a2643d9faf65d664c786169c38f01767fb7e5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="timer-trigger-for-azure-functions"></a>Wyzwalacz czasomierza dla usługi Azure Functions 

W tym artykule opisano sposób pracy z wyzwalaczy czasomierza w usługi Azure Functions. Wyzwalacz czasomierza umożliwia uruchamianie funkcji zgodnie z harmonogramem. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages"></a>Pakiety

Wyzwalacz czasomierza znajduje się w [Microsoft.Azure.WebJobs.Extensions](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) pakietu NuGet. Kod źródłowy dla pakietu jest w [azure-zadań webjob sdk rozszerzenia](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/) repozytorium GitHub.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="example"></a>Przykład

Zapoznaj się z przykładem specyficzny dla języka:

* [C#](#trigger---c-example)
* [Skryptu C# (csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="c-example"></a>Przykład C#

W poniższym przykładzie przedstawiono [C# funkcja](functions-dotnet-class-library.md) co pięć minut:

```cs
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

### <a name="c-script-example"></a>Przykładowy skrypt w języku C#

W poniższym przykładzie przedstawiono wyzwalacza bazującego na czasomierzu powiązanie w *function.json* pliku i [funkcji skryptu C#](functions-reference-csharp.md) używającą powiązania. Funkcja zapisuje dziennik wskazującą, czy to wywołanie funkcji jest z powodu wystąpienia brakujących harmonogramu.

W tym miejscu jest powiązanie danych *function.json* pliku:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Oto kod skryptu C#:

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

### <a name="f-example"></a>Przykład F #

W poniższym przykładzie przedstawiono wyzwalacza bazującego na czasomierzu powiązanie w *function.json* pliku i [funkcji skryptu języka F #](functions-reference-fsharp.md) używającą powiązania. Funkcja zapisuje dziennik wskazującą, czy to wywołanie funkcji jest z powodu wystąpienia brakujących harmonogramu.

W tym miejscu jest powiązanie danych *function.json* pliku:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Oto kod skryptu języka F #:

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

### <a name="javascript-example"></a>Przykład JavaScript

W poniższym przykładzie przedstawiono wyzwalacza bazującego na czasomierzu powiązanie w *function.json* pliku i [funkcji JavaScript](functions-reference-node.md) używającą powiązania. Funkcja zapisuje dziennik wskazującą, czy to wywołanie funkcji jest z powodu wystąpienia brakujących harmonogramu.

W tym miejscu jest powiązanie danych *function.json* pliku:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Oto kod skryptu JavaScript:

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

## <a name="attributes"></a>Atrybuty

W [bibliotek klas C#](functions-dotnet-class-library.md), użyj [TimerTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs).

Konstruktor atrybutu ma wyrażenie CRON, jak pokazano w poniższym przykładzie:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
   ...
}
 ```

Można określić `TimeSpan` zamiast wyrażenia CRON, gdy funkcja aplikacja działa w plan usługi aplikacji (nie plan zużycie).

Pełny przykład, zobacz [przykład C#](#c-example).

## <a name="configuration"></a>Konfigurowanie

W poniższej tabeli opisano powiązania właściwości konfiguracyjne, które można ustawić w *function.json* pliku i `TimerTrigger` atrybutu.

|Właściwość Function.JSON | Właściwość atrybutu |Opis|
|---------|---------|----------------------|
|**Typ** | Nie dotyczy | Musi być równa "timerTrigger". Ta właściwość ma wartość automatycznie, podczas tworzenia wyzwalacza w portalu Azure.|
|**Kierunek** | Nie dotyczy | Należy wybrać opcję "w". Ta właściwość ma wartość automatycznie, podczas tworzenia wyzwalacza w portalu Azure. |
|**Nazwa** | Nie dotyczy | Nazwa zmiennej, która reprezentuje obiekt czasomierza w kodzie funkcji. | 
|**schedule**|**ScheduleExpression**|W planie zużycie można zdefiniować harmonogramy za pomocą usługi CRON wyrażenia. Jeśli używasz planu usługi App Service można również użyć `TimeSpan` ciągu. W poniższych sekcjach opisano CRON wyrażenia. Można umieścić wyrażenia harmonogramu w ustawieniu aplikacji i ustawić tę właściwość na wartość otoczona  **%**  znaków, jak w poniższym przykładzie: "% NameOfAppSettingWithCRONExpression %". |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

### <a name="cron-format"></a>Format usługi CRON 

A [wyrażenie CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) dla usługi Azure Functions czasomierza wyzwalacza obejmuje sześć pola: 

```
{second} {minute} {hour} {day} {month} {day-of-week}
```

>[!NOTE]   
>Wiele wyrażeń CRON możesz znaleźć online Pomiń `{second}` pola. Po skopiowaniu jednego z nich, Dodaj brakujące `{second}` pola.

### <a name="cron-time-zones"></a>Strefy czasowe usługi CRON

Domyślna strefa czasowa używane w wyrażeniach CRON jest uniwersalny czas koordynowany (UTC). Aby wymusić wyrażenie CRON oparte na innej strefie czasowej, Utwórz nowe ustawienie aplikacji dla aplikacji funkcja o nazwie `WEBSITE_TIME_ZONE`. Ustaw wartość na nazwę odpowiednie strefy czasowej, jak pokazano w [indeksu strefy czasowej Microsoft](https://technet.microsoft.com/library/cc749073(v=ws.10).aspx). 

Na przykład *wschodni czas standardowy* jest UTC-05:00. Aby Twoje czasomierza wyzwolenia fire na 10:00 CET codziennie, użyj następujących kont dla strefy czasowej UTC wyrażenie CRON:

```json
"schedule": "0 0 15 * * *",
``` 

Alternatywnie możesz dodać nowe ustawienie aplikacji dla aplikacji funkcja o nazwie `WEBSITE_TIME_ZONE` i ustaw wartość **wschodni czas standardowy**.  Poniższe wyrażenie CRON można następnie używać do 10:00 AM EST: 

```json
"schedule": "0 0 10 * * *",
``` 
### <a name="cron-examples"></a>Przykłady usługi CRON

Oto kilka przykładów CRON wyrażeń, które można użyć wyzwalacza czasomierza w funkcji platformy Azure. 

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

## <a name="usage"></a>Sposób użycia

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

## <a name="scale-out"></a>Skalowanie w poziomie

Wyzwalacz czasomierza obsługuje wiele wystąpień skalowalnego w poziomie. Pojedyncze wystąpienie funkcji określonego czasomierza jest uruchamiane we wszystkich wystąpieniach.

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Przejdź do szybkiego startu, który korzysta z wyzwalacza bazującego na czasomierzu](functions-create-scheduled-function.md)

> [!div class="nextstepaction"]
> [Dowiedz się więcej o usługę Azure functions wyzwalaczy i powiązań](functions-triggers-bindings.md)
