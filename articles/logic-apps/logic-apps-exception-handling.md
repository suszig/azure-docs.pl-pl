---
title: "Obsługa wyjątków — usługi Azure Logic Apps & błędu | Dokumentacja firmy Microsoft"
description: "Wzorce dla błędów i obsługa wyjątków w aplikacjach logiki platformy Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 9af2f71b3d288cc6f4e271d0915545d43a1249bc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="handle-errors-and-exceptions-in-azure-logic-apps"></a>Obsługa błędów i wyjątków w aplikacjach logiki platformy Azure

Aplikacje logiki platformy Azure udostępnia zaawansowane narzędzia i wzorce, aby upewnić się, że Twoje integracji są niezawodne i podatne na błędy. Wszelkie Architektura integracji stanowi wyzwanie, by odpowiednio obsłużyć przestoje lub problemów z systemów zależnych. Logic Apps sprawia, że obsługa błędów najwyższej jakości środowisko, umożliwiając narzędzi, potrzebnych do działania na wyjątków i błędów w swoich przepływach pracy.

## <a name="retry-policies"></a>Spróbuj ponownie zasad

Zasady ponawiania jest najbardziej podstawowy typ wyjątku i obsługa błędów. Jeśli upłynął limit czasu żądania początkowego, lub nie powiodło się (każde żądanie, która powoduje 429 lub odpowiedź 5xx), ta zasada definiuje, czy akcja powinna ponowić. Domyślnie wszystkie akcje próbę 4 razy dodatkowe za pośrednictwem 20 sekund. Dlatego w przypadku pierwszego żądania odbiera `500 Internal Server Error` odpowiedzi, aparatu przepływu pracy wstrzymuje 20 sekund i próbuje ponownie żądanie. Jeśli po wszystkich próbach odpowiedzi jest nadal wyjątku lub błędu, przepływ pracy będzie kontynuowane i oznacza stanu akcji jako `Failed`.

Można skonfigurować zasady ponawiania w **dane wejściowe** dla określonej akcji. Na przykład można skonfigurować zasady ponawiania próby maksymalnie 4 razy na 1 godzinę. Aby uzyskać szczegółowe informacje o właściwościach wejściowego, zobacz [działania przepływu pracy i wyzwalaczy][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Jeśli potrzebujesz Twoja Akcja HTTP, aby ponowić próbę 4 godziny i odczekaj 10 minut między kolejnymi próbami należy użyć następującej definicji:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Aby uzyskać więcej informacji o obsługiwanych składni, zobacz [części zasady ponawiania akcji przepływu pracy i wyzwalaczy][retryPolicyMSDN].

## <a name="catch-failures-with-the-runafter-property"></a>Błędy z właściwością RunAfter catch

Każde działanie aplikacji logiki deklaruje akcje, które muszą zostać zakończone przed uruchomieniem akcji, takich jak kolejności kroków w przepływie pracy. W definicji działania ta kolejność nosi nazwę `runAfter` właściwości. Ta właściwość jest obiekt, który opisuje, które akcje i Stany Akcja wykonania akcji. Domyślnie wszystkie akcje dodane za pomocą projektanta aplikacji logiki są ustawione na `runAfter` poprzedniego kroku Jeśli poprzedni krok `Succeeded`. Można jednak dostosować tę wartość, aby zostać wywołane akcje podczas poprzedniej akcji ma `Failed`, `Skipped`, lub zestaw możliwych tych wartości. Jeśli chcesz dodać element do wyznaczonych tematu usługi Service Bus po określonym działaniu `Insert_Row` nie powiedzie się, można użyć następującego `runAfter` konfiguracji:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Powiadomienie `runAfter` wyzwalana, gdy właściwość ma wartość `Insert_Row` akcji jest `Failed`. Aby uruchomić akcję, gdy stan działania to `Succeeded`, `Failed`, lub `Skipped`, należy użyć następującej składni:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Akcje, które są uruchomione i pomyślnie zakończone po poprzednim akcja nie powiodła się, są oznaczone jako `Succeeded`. To zachowanie oznacza, że jeśli możesz pomyślnie catch wszystkich błędów w przepływie pracy, uruchom sam jest oznaczony jako `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Zakresy i wyniki oceny akcje

Podobnie jak uruchamianie po poszczególnych działań można również pogrupować Akcje wewnątrz [zakres](../logic-apps/logic-apps-loops-and-scopes.md), działających jako logiczne grupowanie akcje. Zakresy są przydatne, zarówno do organizowania akcji aplikacji logiki, jak i do wykonywania oceny Łączny stan zakresu. Sam zakres przechodzi w stan po ukończeniu wszystkich działań w zakresie. Stan zakresu jest określana z takich samych kryteriów jako Uruchom. Po ostatnim akcji w gałęzi wykonywania `Failed` lub `Aborted`, stan jest `Failed`.

Aby określonych akcji dla wszystkich błędów, które wystąpiły w zakresie, można użyć `runAfter` z zakresem, który jest oznaczony jako `Failed`. Jeśli *żadnych* akcje w zakresie zakończyć się niepowodzeniem, po zakresu zakończy się niepowodzeniem, umożliwia tworzenie jednej akcji, aby wykryć błędów.

### <a name="getting-the-context-of-failures-with-results"></a>Uzyskanie kontekstu błędów z wynikami

Mimo że przechwytywanie błędów z zakresu jest przydatne, będą również chcieli kontekście pomagające zrozumieć dokładnie akcje, które nie powiodło się, a jakiekolwiek błędy i kodów stanu, które zostały zwrócone. `@result()` Funkcji przepływu pracy zawierają kontekst o wyniku wszystkie akcje w zakresie.

`@result()`przyjmuje jeden parametr, nazwa zakresu i zwraca tablicę wszystkich akcji wyników w tym zakresie. Te obiekty działania obejmują takie same atrybuty jak `@actions()` generuje obiekt, w tym czas rozpoczęcia działania, czas zakończenia działania stanu akcji, dane wejściowe działań, identyfikatorów korelacji działania i akcji. Aby wysłać kontekstu wszystkie akcje, które nie powiodło się w zakresie, można łatwo skojarzyć `@result()` działać z `runAfter`.

Do wykonania akcji *dla każdego* akcji w zakresie który `Failed`, filtrować tablicy wyników z akcjami, których nie powiodła się, można skojarzyć `@result()` z  **[tablicy filtrów](../connectors/connectors-native-query.md)**  akcji i  **[ForEach](../logic-apps/logic-apps-loops-and-scopes.md)**  pętli. Możesz pobrać tablicy filtrowane wyniki i wykonania czynności dla każdego błędu przy użyciu funkcji **ForEach** pętli. Oto przykład, a następnie szczegółowy opis, który wysyła żądanie HTTP POST z treści odpowiedzi żadnych akcji, których nie powiodła się w zakresie `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Poniżej przedstawiono szczegółowy przewodnik opisujący, co się stanie:

1. Aby uzyskać wynik wszystkich akcji w ramach `My_Scope`, **tablicy filtrów** filtry akcji `@result('My_Scope')`.

2. Warunek **tablicy filtrów** dowolnego `@result()` elementu, który znajduje się w stanie równa `Failed`. Ten warunek filtruje tablicy o wszystkich wyników akcji z `My_Scope` do tablicy o tylko nie powiodło się wyniki akcji.

3. Wykonaj **dla każdego** akcji **filtrowane tablicy** danych wyjściowych. Ten krok wykonuje akcję *dla każdego* nie powiodło się wynik akcji, która wcześniej została przefiltrowana.

    Jeśli nie jednej akcji w zakresie, akcje w `foreach` uruchomić tylko raz. 
    Wiele zakończonych niepowodzeniem akcje mogą spowodować jedną akcję na błąd.

4. Wyślij HTTP POST `foreach` elementu treści odpowiedzi lub `@item()['outputs']['body']`. `@result()` Kształt element jest taka sama jak `@actions()` kształtu i może być analizowana taki sam sposób.

5. Obejmują dwa Nagłówki niestandardowe o nazwie nieudanych akcji `@item()['name']` i nieudane Uruchom klienta, identyfikator śledzenia `@item()['clientTrackingId']`.

Odwołania, Oto przykład jedną `@result()` elementu przedstawiający `name`, `body`, i `clientTrackingId` właściwości, które są parsowane w poprzednim przykładzie. Poza `foreach`, `@result()` zwraca tablicę tych obiektów.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/folder-name/resource-name does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Do wykonywania różnych wzorców w obsłudze wyjątków, używając wyrażenia przedstawione wcześniej. Może również wykonać pojedynczego wyjątków, Obsługa akcji poza zakres, który akceptuje całą macierz filtrowane awarii, a Usuń `foreach`. Możesz również uwzględnić inne przydatne właściwości z `@result()` odpowiedzi przedstawione wcześniej.

## <a name="azure-diagnostics-and-telemetry"></a>Diagnostyka Azure i telemetrii

Te wzorce poprzedniej to doskonały sposób na Obsługa błędów i wyjątków w ramach Uruchom, ale można również zidentyfikować i odpowiadać na błędy, niezależnie od samego przebiegu. 
[Diagnostyka Azure](../logic-apps/logic-apps-monitor-your-logic-apps.md) zapewnia prosty sposób wysyłania wszystkich zdarzeń przepływu pracy (w tym wszystkie stany akcji i uruchom) na konto magazynu Azure lub usługi Azure Event Hub. Aby ocenić stany wykonywania, Monitoruj dzienniki i metryki lub publikować je do dowolnego narzędzia monitorowania preferowany. Jedną z opcji potencjalnych jest do strumienia wszystkie zdarzenia do Centrum zdarzeń Azure do [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). W Stream Analytics można zapisywać zapytania na żywo poza wszelkie nieprawidłowości, średnie lub błędy z dzienników diagnostycznych. Analiza strumienia może łatwo dane wyjściowe do innych źródeł danych, takich jak kolejki, tematy, SQL, bazy danych rozwiązania Cosmos Azure i usługi Power BI.

## <a name="next-steps"></a>Następne kroki

* [Zobacz, jak klient tworzy błąd obsługi z usługi Azure Logic Apps](../logic-apps/logic-apps-scenario-error-and-exception-handling.md)
* [Znajdź więcej Logic Apps przykłady i scenariusze](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Dowiedz się, jak utworzyć zautomatyzowanych wdrożeń dla aplikacji logiki](../logic-apps/logic-apps-create-deploy-template.md)
* [Tworzenie i wdrażanie aplikacji logiki w programie Visual Studio](logic-apps-deploy-from-vs.md)

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
