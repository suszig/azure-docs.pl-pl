---
title: "Interfejsy API protokołu HTTP w funkcjach trwałe - Azure"
description: "Dowiedz się, jak wdrożyć interfejsy API protokołu HTTP w rozszerzeniu trwałe funkcji dla usługi Azure Functions."
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
ms.openlocfilehash: bb5361022e4c9693812753ae33df5aeb037b5aaa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="http-apis-in-durable-functions-azure-functions"></a>Interfejsy API protokołu HTTP w funkcji trwałe (funkcje platformy Azure)

Rozszerzenie zadania trwałe udostępnia zestaw interfejsów API protokołu HTTP, który może służyć do wykonywania następujących zadań:

* Pobierz stan wystąpienia aranżacji.
* Wysyła zdarzenie do wystąpienia aranżacji oczekiwania.
* Przerwanie uruchomione wystąpienie aranżacji.

Wszystkie te interfejsy API protokołu HTTP są webhook operacje, które są obsługiwane bezpośrednio przez rozszerzenie zadania trwałe. Nie są one właściwe dla dowolnej funkcji w funkcji aplikacji.

> [!NOTE]
> Te operacje może również wywoływany bezpośrednio za pomocą interfejsów API zarządzania wystąpienia na [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasy. Aby uzyskać więcej informacji, zobacz [Zarządzanie wystąpieniami](durable-functions-instance-management.md).

## <a name="http-api-url-discovery"></a>Adres URL HTTP interfejsu API odnajdywania

[DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) klasy ujawnia [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_) interfejsu API, który może służyć do generowania odpowiedzi HTTP ładunek zawierający łącza do obsługiwanych operacji. Oto przykład funkcja wyzwalacza HTTP, który demonstruje sposób użycia tego interfejsu API:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/HttpStart/run.csx)]

Ta funkcja przykład tworzy następujące dane JSON w odpowiedzi. Typ danych we wszystkich polach jest `string`.

| Pole             |Opis                           |
|-------------------|--------------------------------------|
| id                |Identyfikator wystąpienia aranżacji. |
| statusQueryGetUri |Adres URL stan wystąpienia aranżacji. |
| sendEventPostUri  |Adres URL "Zgłoś zdarzenia" wystąpienia aranżacji. |
| terminatePostUri  |"Zakończ" adres URL wystąpienia aranżacji. |

Oto przykład odpowiedzi:

```http
HTTP/1.1 202 Accepted
Content-Length: 923
Content-Type: application/json; charset=utf-8
Location: https://{host}/webhookextensions/handler/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX

{
    "id":"34ce9a28a6834d8492ce6a295f1a80e2",
    "statusQueryGetUri":"https://{host}/webhookextensions/handler/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "sendEventPostUri":"https://{host}/webhookextensions/handler/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "terminatePostUri":"https://{host}/webhookextensions/handler/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code=XXX"
}
```
> [!NOTE]
> Format adresów URL elementu webhook mogą się różnić w zależności od posiadanej wersji programu host usługi Azure Functions. Powyższy przykład dotyczy hosta Azure funkcji 2.0.

## <a name="async-operation-tracking"></a>Operacja asynchroniczna śledzenia

Odpowiedź HTTP wspomniano ułatwia wdrażanie asynchronicznej HTTP długotrwałe interfejsów API za pomocą funkcji trwałe. Jest to czasami określane jako *sondowania wzorca odbiorców*. Przepływ klient/serwer działa w następujący sposób:

1. Klient generuje żądanie HTTP do uruchomienia procesu wymagającą dużo czasu, takich jak funkcja programu orchestrator.
2. Wyzwalacz docelowych HTTP zwraca odpowiedź HTTP 202 z `Location` nagłówek o `statusQueryGetUri` wartość.
3. Klient sonduje adres URL w `Location` nagłówka. Nadal odpowiedzi HTTP 202 z `Location` nagłówka.
4. Gdy wystąpienie zakończeniu (lub nie powiedzie się), punktu końcowego w `Location` nagłówka zwraca 200 protokołu HTTP.

Ten protokół umożliwia koordynowanie procesy długotrwałe z klientami zewnętrznymi lub usług, które obsługują sondowania punktu końcowego HTTP i wykonując `Location` nagłówka. Podstawowe elementy już są wbudowane w trwałe funkcje API HTTP.

> [!NOTE]
> Domyślnie wszystkie działania oparte na protokole HTTP pochodzącymi [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) obsługują wzorzec standardowych operacji asynchronicznej. Umożliwia osadzanie funkcję trwałego długotrwałe jako część przepływu pracy aplikacji logiki. Więcej informacji na temat aplikacji logiki obsługę wzorców asynchronicznych HTTP można znaleźć w [Azure Logic Apps przepływu pracy akcji i wyzwalaczy dokumentacji](../logic-apps/logic-apps-workflow-actions-triggers.md#asynchronous-patterns).

## <a name="http-api-reference"></a>Dokumentacja interfejsu API HTTP

Wszystkie interfejsy API protokołu HTTP implementowana przez rozszerzenie podejmij następujące parametry. Typ danych wszystkie parametry jest `string`.

| Parametr  | Typ parametru  | Opis |
|------------|-----------------|-------------|
| Identyfikator wystąpienia | ADRES URL             | Identyfikator wystąpienia aranżacji. |
| taskHub    | Ciąg zapytania    | Nazwa [Centrum zadań](durable-functions-task-hubs.md). Jeśli nie zostanie określony, przyjęto, że nazwa Centrum zadania bieżącej aplikacji funkcji. |
| połączenie | Ciąg zapytania    | **Nazwa** ciągu połączenia dla konta magazynu. Jeśli nie zostanie określona, przyjmowana jest domyślnego ciągu połączenia dla aplikacji funkcja. |
| systemKey  | Ciąg zapytania    | Klucz autoryzacji wymaganych do wywołania interfejsu API. |

`systemKey`jest klucz autoryzacji wygenerowana automatycznie przez hosta usługi Azure Functions. W szczególności nieograniczony dostęp do rozszerzenia zadania trwałe interfejsów API i można zarządzać w taki sam sposób jak [innych kluczy o autoryzacji](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Key-management-API). Najprostszym sposobem, aby odnaleźć `systemKey` wartość jest za pomocą `CreateCheckStatusResponse` wspomniano interfejsu API.

Następne sekcje kilka obejmują określonych interfejsów API HTTP obsługiwane przez rozszerzenie i zawierają przykłady sposobu ich użycia.

### <a name="get-instance-status"></a>Pobierz stan wystąpienia

Pobiera stan wystąpienia określonego aranżacji.

#### <a name="request"></a>Żądanie

Funkcje 1.0 format żądania jest następujący:

```http
GET /admin/extensions/DurableTaskExtension/instances/{instanceId}?taskHub={taskHub}&connection={connection}&code={systemKey}
```

Format 2.0 funkcji ma takie same parametry, ale ma nieco inny prefiks adresu URL:

```http
GET /webhookextensions/handler/DurableTaskExtension/instances/{instanceId}?taskHub={taskHub}&connection={connection}&code={systemKey}
```

#### <a name="response"></a>Odpowiedź

Może być zwracany kilka wartości Kod stanu możliwe.

* **HTTP 200 (OK)**: określone wystąpienie jest w stanie ukończone.
* **HTTP 202 (zaakceptowane)**: określone wystąpienie jest w toku.
* **HTTP 400 (nieprawidłowe żądanie)**: określone wystąpienie nie powiodło się lub zostało przerwane.
* **HTTP 404 (nie znaleziono)**: określone wystąpienie nie istnieje lub nie została uruchomiona uruchomiona.

Ładunek odpowiedzi dla **200 protokołu HTTP** i **HTTP 202** przypadków jest to obiekt JSON z następującymi polami.

| Pole           | Typ danych | Opis |
|-----------------|-----------|-------------|
| runtimeStatus   | Ciąg    | Stanu działania wystąpienia. Wartości to *systemem*, *oczekujące*, *niepowodzenie*, *anulowane*, *zwolniony*, *Ukończone*. |
| Dane wejściowe           | JSON      | Dane JSON, używane do inicjowania wystąpienia. |
| Dane wyjściowe          | JSON      | Dane wyjściowe JSON wystąpienia. To pole jest `null` Jeśli wystąpienie nie jest w stanie ukończone. |
| createdTime     | Ciąg    | Podczas tworzenia tego wystąpienia. Używa ISO 8601 rozszerzony notacji. |
| LastUpdatedTime | Ciąg    | Czas, jaką trwała ostatniego wystąpienia. Używa ISO 8601 rozszerzony notacji. |

Oto przykład ładunku odpowiedzi (sformatowany dla czytelności):

```json
{
  "runtimeStatus": "Completed",
  "input": null,
  "output": [
    "Hello Tokyo!",
    "Hello Seattle!",
    "Hello London!"
  ],
  "createdTime": "2017-10-06T18:30:24Z",
  "lastUpdatedTime": "2017-10-06T18:30:30Z"
}
```

**HTTP 202** odpowiedzi zawiera również **lokalizacji** nagłówka odpowiedzi, który odwołuje się do tego samego adresu URL jako `statusQueryGetUri` pola wymienione wcześniej.

### <a name="raise-event"></a>Wywołaj zdarzenie

Wysyła powiadomienie o zdarzeniu do uruchomionego wystąpienia aranżacji.

#### <a name="request"></a>Żądanie

Funkcje 1.0 format żądania jest następujący:

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection={connection}&code={systemKey}
```

Format 2.0 funkcji ma takie same parametry, ale ma nieco inny prefiks adresu URL:

```http
POST /webhookextensions/handler/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection={connection}&code={systemKey}
```

Parametry dla tego interfejsu API to domyślny zestaw wspomniano powyżej, a także następujące parametry unikatowy żądań.

| Pole       | Typ parametru  | TType danych | Opis |
|-------------|-----------------|-----------|-------------|
| EventName   | ADRES URL             | Ciąg    | Nazwa zdarzenia, które czeka na wystąpienie docelowe aranżacji. |
| {zawartość}   | Żądanie zawartości | JSON      | Ładunek zdarzenia w formacie JSON. |

#### <a name="response"></a>Odpowiedź

Może być zwracany kilka wartości Kod stanu możliwe.

* **HTTP 202 (zaakceptowane)**: zgłoszono zdarzeń zaakceptowano do przetwarzania.
* **HTTP 400 (nieprawidłowe żądanie)**: zawartość żądania nie typu `application/json` lub nie jest poprawne dane JSON.
* **HTTP 404 (nie znaleziono)**: nie można odnaleźć określonego wystąpienia.
* **HTTP 410 (Gone)**: określone wystąpienie ma zakończone lub nie powiodło się i nie może przetwarzać zgłoszono zdarzeń.

Poniżej przedstawiono przykładowe żądanie, która wysyła ciągu JSON `"incr"` do wystąpienia oczekiwania na zdarzenie o nazwie **operacji** (pobierane z [licznika](durable-functions-counter.md) przykładowe):

```
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code=XXX
Content-Type: application/json
Content-Length: 6

"incr"
```

Odpowiedzi dla tego interfejsu API nie zawierają żadnej zawartości.

### <a name="terminate-instance"></a>Przerwanie wystąpienia

Kończy uruchomione wystąpienie aranżacji.

#### <a name="request"></a>Żądanie

Funkcje 1.0 format żądania jest następujący:

```http
DELETE /admin/extensions/DurableTaskExtension/instances/{instanceId}/terminate?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

Format 2.0 funkcji ma takie same parametry, ale ma nieco inny prefiks adresu URL:

```http
DELETE /webhookextensions/handler/DurableTaskExtension/instances/{instanceId}/terminate?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

Parametry dla tego interfejsu API to wymienione wcześniej oraz następujący parametr unikatowy domyślny zestaw żądań.

| Pole       | Typ parametru  | Typ danych | Opis |
|-------------|-----------------|-----------|-------------|
| Przyczyna      | Ciąg zapytania    | Ciąg    | Opcjonalny. Przyczyna przerywanie wystąpienia aranżacji. |

#### <a name="response"></a>Odpowiedź

Może być zwracany kilka wartości Kod stanu możliwe.

* **HTTP 202 (zaakceptowane)**: żądanie przerwania zaakceptowano do przetwarzania.
* **HTTP 404 (nie znaleziono)**: nie można odnaleźć określonego wystąpienia.
* **HTTP 410 (Gone)**: określone wystąpienie ma zakończone lub nie powiodło się.

Poniżej przedstawiono przykładowe żądanie, która kończy uruchomionego wystąpienia i określa przyczynę **buggy**:

```
DELETE /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason=buggy&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

Odpowiedzi dla tego interfejsu API nie zawierają żadnej zawartości.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Informacje o sposobie obsługi błędów](durable-functions-error-handling.md)
