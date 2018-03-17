---
title: "Stanowe pojedynczych wystąpień w funkcjach trwałe - Azure"
description: "Jak zaimplementować stanowe singleton w rozszerzeniu trwałe funkcji dla usługi Azure Functions."
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
ms.openlocfilehash: 46cdd8523117e1100e7ce2a29ade9eb2dc0afe75
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="stateful-singletons-in-durable-functions---counter-sample"></a>Stanowe pojedynczych wystąpień w funkcjach trwałe - próbka licznika

> [!NOTE]
> Firma Microsoft ponowne zapisywanie w tym przykładzie. Wstawki kodu zostały usunięte, a nowy artykuł nowe próbki zostanie ono zamienione.

Stanowe pojedynczych są długotrwałe (potencjalnie eternal) orchestrator funkcje, które można przechowywać stanu i można wywołać i żądanych przez inne funkcje. Stanowe pojedynczych są podobne do [modelu aktora](https://en.wikipedia.org/wiki/Actor_model) w przetwarzania rozproszonego.

Podczas nie implementacji prawidłowego "aktora" funkcji programu orchestrator ma wiele cech środowiska wykonawczego. Na przykład są stanowe, niezawodnych, jednowątkowych, przezroczysty lokalizacji i globalnie adresowanego. Są to właściwości, które należy implementacje rzeczywistych aktora szczególnie przydatne, ale bez konieczności oddzielnego framework.

W tym artykule przedstawiono sposób uruchamiania *licznika* próbki. W przykładzie pokazano pojedynczego obiektu, który obsługuje *przyrostu* i *dekrementacji* operacji i odpowiednio aktualizuje jego stan wewnętrzny.

## <a name="prerequisites"></a>Wymagania wstępne

* Postępuj zgodnie z instrukcjami [zainstalować funkcje trwałe](durable-functions-install.md) do skonfigurowania próbki.
* W tym artykule przyjęto już przeszły [Hello sekwencji](durable-functions-sequence.md) wskazówki przykładowe.

## <a name="scenario-overview"></a>Omówienie scenariusza

Scenariusz licznika zaskakująco trudno jest zrealizować przy użyciu funkcji bezstanowych regularne. Jednym z głównych wyzwań masz zarządza **współbieżności**. Operacji, takich jak *przyrostu* i *dekrementacji* muszą być atomic lub else może być wyścigu, powodujących operacji zastępowały siebie nawzajem.

Host dane liczników za pomocą jednej maszyny Wirtualnej jest jedną z opcji, ale jest kosztowne i zarządzanie nimi **niezawodności** może być wyzwaniem, ponieważ jednej maszyny Wirtualnej może być okresowo ponowny rozruch. Można też użyć rozproszonej platformy przy synchronizacji narzędzi, takich jak dzierżawy blob pomagające w zarządzaniu współbieżności, ale powstaje dużą **złożoności**.

Trwałe funkcji powoduje, że taki scenariusz prosta do wdrożenia, ponieważ wystąpień aranżacji są skoligowanych z jednej maszyny Wirtualnej, a następnie wykonanie funkcji programu orchestrator jest zawsze jednowątkowego. Nie tylko który, ale są one długotrwałe, stanowe i może reagowania na zdarzenia zewnętrzne. Poniższy przykładowy kod pokazuje, jak do zaimplementowania licznik jako długotrwałe funkcji programu orchestrator.

## <a name="the-sample-function"></a>Funkcja próbki

W tym artykule przedstawiono **E3_Counter** funkcji w przykładowej aplikacji.



## <a name="the-counter-orchestration"></a>Licznik aranżacji

W poniższych sekcjach opisano kod, który jest używany do tworzenia aplikacji programu Visual Studio Code i portalu Azure.

### <a name="c-script"></a>Skryptu C#

Plik function.json:

<!-- [!code-json[Main](~/samples-durable-functions/samples/csx/E3_Counter/function.json)] -->

Plik run.csx:

<!-- [!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_Counter/run.csx)] -->

### <a name="precompiled-c"></a>Prekompilowane C# 

W poniższych sekcjach opisano kod, który jest używany do tworzenia aplikacji programu Visual Studio.

Oto kod, który implementuje funkcję programu orchestrator:

<!-- [!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Counter.cs)] -->

### <a name="explanation-of-the-code"></a>Wyjaśnienie kodu

Ta funkcja orchestrator zasadniczo wykonuje następujące czynności:

1. Nasłuchuje zewnętrznych zdarzenia o nazwie *operacji* przy użyciu [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_).
2. Zwiększa lub zmniejsza `counterState` zmiennej lokalnej w zależności od żądanej operacji.
3. Uruchamia ponownie orchestrator przy użyciu [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) metody, ustawiając wartość najnowszej `counterState` jako nowe dane wejściowe.
4. Wciąż trwa nieograniczony czas lub do momentu *zakończenia* odebrać wiadomości.

To jest przykład *eternal aranżacji* &mdash; tzn potencjalnie nigdy nie został zakończony. Odpowiada na komunikaty wysyłane do niego przez [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) metody, który może być wywoływany przez funkcję z systemem innym niż orchestrator.

Jeden unikatowy charakterystyczne dla tej funkcji programu orchestrator jest, że go skutecznie nie miała: `ContinueAsNew` metoda powoduje zresetowanie historii po każdym zdarzeniu przetworzone. Jest to preferowany sposób wdrożenia programu orchestrator z dowolnego okresu istnienia. Przy użyciu `while` pętli może spowodować, że funkcja orchestrator historii powiększać bez ograniczeń, co zapewnia niepotrzebnie wysokie użycie pamięci.

> [!NOTE]
> `ContinueAsNew` Metoda ma inne przypadki użycia, oprócz eternal orchestrations. Aby uzyskać więcej informacji, zobacz [Eternal Orchestrations](durable-functions-eternal-orchestrations.md).

## <a name="run-the-sample"></a>Uruchamianie aplikacji przykładowej

Orchestration można uruchomić poprzez wysłanie następujące żądania HTTP POST. Aby umożliwić `counterState` do rozpoczynają się od zera (wartość domyślna dla `int`), Brak zawartości w tym żądaniu.

```
POST http://{host}/orchestrators/E3_Counter
Content-Length: 0
```

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{
  "id":"bcf6fb5067b046fbb021b52ba7deae5a",
  "statusQueryGetUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}",
  "sendEventPostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}",
  "terminatePostUri":"http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}"}
```

**E3_Counter** uruchamia wystąpienie, a następnie natychmiast oczekiwanie zdarzenia do wysłania do niej przy użyciu `RaiseEventAsync` lub przy użyciu **sendEventUrl** elementu webhook HTTP POST, do którego odwołuje się 202 odpowiedzi. Nieprawidłowa `eventName` wartości to *incr*, *decr —*, i *zakończenia*.

```
POST http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
Content-Type: application/json
Content-Length: 6

"incr"
```

Analizując dzienniki funkcji w portalu Azure Functions można wyświetlić wyniki operacji "incr".

```
2017-06-29T18:54:53.998 Function started (Id=34e34a61-38b3-4eac-b6e2-98b85e32eec8)
2017-06-29T18:54:53.998 Current counter state is 0. Waiting for next operation.
2017-06-29T18:58:01.458 Function started (Id=b45d6c2f-39f3-42a2-b904-7761b2614232)
2017-06-29T18:58:01.458 Current counter state is 0. Waiting for next operation.
2017-06-29T18:58:01.458 Received 'incr' operation.
2017-06-29T18:58:01.458 Function completed (Success, Id=b45d6c2f-39f3-42a2-b904-7761b2614232, Duration=8ms)
2017-06-29T18:58:11.518 Function started (Id=e1f38cb2-546a-404d-ab22-1ac8f81a93d9)
2017-06-29T18:58:11.518 Current counter state is 1. Waiting for next operation.
```

Podobnie, jeśli możesz sprawdzić stan programu orchestrator, zobaczysz `input` pola została ustawiona jako wartość zaktualizowane (1).

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey} HTTP/1.1
```

```
HTTP/1.1 202 Accepted
Content-Length: 129
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"runtimeStatus":"Running","input":1,"output":null,"createdTime":"2017-06-29T18:58:01Z","lastUpdatedTime":"2017-06-29T18:58:11Z"}
```

Można kontynuować wysyłanie nowych operacji w tym wystąpieniu i obserwować, jego stan jest odpowiednio aktualizowany. Jeśli chcesz skasować wystąpienie, możesz to zrobić, wysyłając *zakończenia* operacji.

> [!WARNING]
> W czasie zapisywania, istnieją znane wyścigu podczas wywoływania metody `ContinueAsNew` podczas współbieżnego przetwarzania komunikaty, takie jak zewnętrzny zdarzenia lub żądania zakończenia. Aby uzyskać najnowsze informacje dotyczące tych wyścigu, zobacz ten [problem GitHub](https://github.com/Azure/azure-functions-durable-extension/issues/67).

## <a name="next-steps"></a>Kolejne kroki

W tym przykładzie wykazała sposób obsługi [zdarzenia zewnętrzne](durable-functions-external-events.md) i wdrożenie [eternal orchestrations](durable-functions-eternal-orchestrations.md) w [stanowe pojedynczych](durable-functions-singletons.md). Następny przykład przedstawia sposób użycia zdarzenia zewnętrzne i [trwałe czasomierze](durable-functions-timers.md) do obsługi człowieka.

> [!div class="nextstepaction"]
> [Uruchamianie przykładowych człowieka](durable-functions-phone-verification.md)
