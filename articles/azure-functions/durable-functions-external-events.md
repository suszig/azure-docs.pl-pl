---
title: "Obsługa zdarzeń zewnętrznych w funkcjach trwałe - Azure"
description: "Informacje o sposobie obsługi zdarzenia zewnętrzne w rozszerzeniu trwałe funkcji dla usługi Azure Functions."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 5ffbe6a7d74f0be2193d711d304f19e62ab08741
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="handling-external-events-in-durable-functions-azure-functions"></a>Obsługa zdarzeń zewnętrzne w funkcji trwałe (funkcje platformy Azure)

Funkcje programu orchestrator mają możliwość poczekać i nasłuchiwania zdarzeń zewnętrznych. Ta funkcja [trwałe funkcje](durable-functions-overview.md) często jest przydatne w przypadku obsługi człowieka lub innych zewnętrznych wyzwalaczy.

## <a name="wait-for-events"></a>Poczekaj na zdarzenia

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) metoda pozwala funkcji orchestrator asynchronicznie poczekać i nasłuchiwania zdarzenie zewnętrzne. Deklaruje nasłuchiwania funkcja orchestrator *nazwa* zdarzenia i *kształtu danych* oczekuje do odbierania.

```csharp
[FunctionName("BudgetApproval")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool approved = await context.WaitForExternalEvent<bool>("Approval");
    if (approved)
    {
        // approval granted - do the approved action
    }
    else
    {
        // approval denied - send a notification
    }
}
```

Powyższy przykład nasłuchuje określonych pojedyncze zdarzenie i podejmuje działanie po odebraniu.

Nasłuchiwanie wielu zdarzeń jednocześnie, podobnie jak w poniższym przykładzie czeka na jedno z trzech powiadomienia o zdarzeniach możliwe.

```csharp
[FunctionName("Select")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    var event1 = context.WaitForExternalEvent<float>("Event1");
    var event2 = context.WaitForExternalEvent<bool>("Event2");
    var event3 = context.WaitForExternalEvent<int>("Event3");

    var winner = await Task.WhenAny(event1, event2, event3);
    if (winner == event1)
    {
        // ...
    }
    else if (winner == event2)
    {
        // ...
    }
    else if (winner == event3)
    {
        // ...
    }
}
```

Poprzedni przykład nasłuchuje *żadnych* wiele zdarzeń. Istnieje również możliwość oczekiwania *wszystkie* zdarzenia.

```csharp
[FunctionName("NewBuildingPermit")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string applicationId = context.GetInput<string>();

    var gate1 = context.WaitForExternalEvent("CityPlanningApproval");
    var gate2 = context.WaitForExternalEvent("FireDeptApproval");
    var gate3 = context.WaitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    await Task.WhenAll(gate1, gate2, gate3);

    await context.CallActivityAsync("IssueBuildingPermit", applicationId);
}
```

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) oczekiwania przez czas nieokreślony niektórych danych wejściowych.  Funkcja aplikacji może być bezpiecznie zwolniony podczas oczekiwania. Jeśli dla tego wystąpienia aranżacji odebraniu zdarzenia jest automatycznie wznowione i natychmiast przetwarza zdarzenia.

> [!NOTE]
> Jeśli aplikacja funkcja korzysta z Planowanie użycia, nie są naliczane opłaty podczas funkcja orchestrator oczekuje na zadanie `WaitForExternalEvent`, niezależnie od tego, jak długo czekać.

Jeśli ładunek zdarzenia nie można przekonwertować na typ oczekiwany `T`, jest zgłaszany wyjątek.

## <a name="send-events"></a>Wysyłanie zdarzeń

[RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) metody [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasy wysyła zdarzenia `WaitForExternalEvent` oczekiwania.  `RaiseEventAsync` Ma metodę *eventName* i *eventData* jako parametry. Dane zdarzenia musi być możliwy do serializacji JSON.

Poniżej znajduje się przykład wyzwalane kolejki funkcję, która wysyła zdarzenia "Zatwierdzenia" do wystąpienia funkcji programu orchestrator. Identyfikator wystąpienia aranżacji pochodzi z treści komunikatu w kolejce.

```csharp
[FunctionName("ApprovalQueueProcessor")]
public static async Task Run(
    [QueueTrigger("approval-queue")] string instanceId,
    [OrchestrationClient] DurableOrchestrationClient client)
{
    await client.RaiseEventAsync(instanceId, "Approval", true);
}
```

Wewnętrznie `RaiseEventAsync` enqueues komunikat, który pobiera pobrania przez funkcję orchestrator oczekiwania.

> [!WARNING]
> Jeśli żadne wystąpienie aranżacji z określonym *identyfikator wystąpienia* lub jeśli wystąpienie nie oczekuje na określonym *Nazwa zdarzenia*, komunikaty o zdarzeniach zostaną odrzucone. Aby uzyskać więcej informacji dotyczących tego zachowania, zobacz [problem GitHub](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Dowiedz się, jak skonfigurować eternal orchestrations](durable-functions-eternal-orchestrations.md)

> [!div class="nextstepaction"]
> [Uruchom program oczekuje na zdarzenia zewnętrzne](durable-functions-phone-verification.md)

> [!div class="nextstepaction"]
> [Uruchom program oczekuje na człowieka](durable-functions-phone-verification.md)

