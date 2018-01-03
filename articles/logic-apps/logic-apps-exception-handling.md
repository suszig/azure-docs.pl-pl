---
title: "Błąd i obsługi dla usługi Logic Apps w usłudze Azure wyjątków | Dokumentacja firmy Microsoft"
description: "Wzorce dla błędów i obsługa wyjątków w aplikacji logiki."
services: logic-apps
documentationcenter: .net,nodejs,java
author: derek1ee
manager: anneta
editor: 
ms.assetid: e50ab2f2-1fdc-4d2a-be40-995a6cc5a0d4
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: a74c7d18306359c9152f139299de1208b5932fe5
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/20/2017
---
# <a name="handle-errors-and-exceptions-in-logic-apps"></a>Obsługa błędów i wyjątków w aplikacji logiki

Logic Apps w usłudze Azure udostępnia zaawansowane narzędzia i wzorce w celu zapewnienia, że z integracji są niezawodne i odporne na błędy. Wszelkie Architektura integracji stanowi wyzwanie odpowiednio obsługi przestoju lub problemów z systemów zależnych. Logic Apps sprawia, że obsługa błędów najwyższej jakości środowisko. Udostępnia narzędzia potrzebne do działania na wyjątków i błędów w swoich przepływach pracy.

## <a name="retry-policies"></a>Spróbuj ponownie zasad

Zasady ponawiania jest najbardziej podstawowy typ wyjątku i obsługa błędów. Upłynął limit czasu żądania początkowego lub (każde żądanie, która powoduje 429 lub odpowiedź 5xx), zasady ponawiania Określa, czy i jak ponowione akcji. 

Istnieją cztery typy zasad ponawiania: domyślna, brak stałej interwał i interwał wykładniczej. Jeśli zasady ponawiania nie jest określona w definicji przepływu pracy, używana jest domyślne zasady zdefiniowane przez usługę. 

Można skonfigurować zasady ponawiania w *dane wejściowe* dla określonej akcji lub wyzwalacza, jeśli jest powtarzający operację. Podobnie można skonfigurować zasady ponawiania (jeśli dotyczy) w Projektancie aplikacji logiki. Aby skonfigurować zasady ponawiania w Projektancie aplikacji logiki, przejdź do **ustawienia** dla określonej akcji.

Aby uzyskać informacje o ograniczeniach zasady ponawiania, zobacz [limity Logic Apps i konfiguracji](../logic-apps/logic-apps-limits-and-config.md). Aby uzyskać więcej informacji o obsługiwanych składni, zobacz [ponów sekcji zasad w działaniach przepływu pracy i wyzwalaczy][retryPolicyMSDN].

### <a name="default"></a>Domyślne

Jeśli nie można zdefiniować zasady ponawiania (**retryPolicy** nie jest zdefiniowana), jest używane domyślne zasady. Domyślne zasady są zasady wykładniczej interwał, które wysyła do czterech ponownych prób, na wykładniczo rosnących odstępach czasu skalowania 7.5 sekund. Interwał jest ograniczona do zakresu od 5 do 45 sekund. Ta zasada domyślna jest odpowiednikiem zasad w tym przykładzie HTTP definicji przepływu pracy:

```json
"HTTP":
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "exponential",
            "count": 4,
            "interval": "PT7.5S",
            "minimumInterval": "PT5S",
            "maximumInterval": "PT45S"
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

### <a name="none"></a>None

Jeśli **retryPolicy** ustawiono **Brak**, próba nieudanego żądania nie zostanie ponowiona.

| Nazwa elementu | Wymagane | Typ | Opis |
| ------------ | -------- | ---- | ----------- |
| type | Yes | Ciąg | **Brak** |

### <a name="fixed-interval"></a>Stały odstęp

Jeśli **retryPolicy** ustawiono **stałej**, zasady ponowi próbę nieudanych żądań przez określony interwał przed wysłaniem żądania dalej oczekiwania.

| Nazwa elementu | Wymagane | Typ | Opis |
| ------------ | -------- | ---- | ----------- |
| type | Yes | Ciąg | **stałe** |
| liczba | Yes | Liczba całkowita | Liczba ponownych prób. Musi należeć do zakresu od 1 do 90. |
| interval | Yes | Ciąg | Interwał w ponawiania próby [formacie ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Musi należeć do zakresu od PT5S i PT1D. |

### <a name="exponential-interval"></a>Interwał wykładniczy

Jeśli **retryPolicy** ustawiono **wykładniczej**, zasady ponawia próby nieudanych żądań, po losowych odstępach czasu z wykładniczo powiększających się. Każdy ponowienia próby jest gwarantowana do wysłania w losowych odstępach czasu, która jest większa niż **minimumInterval** i mniejsza niż **maximumInterval**. Uniform zmienną losową z zakresu wskazanych w poniższej tabeli jest generowany przez każdego ponów włącznie **liczba**:

**Zakres zmiennej losowej**

| Spróbuj ponownie numer | Minimalny interwał | Maksymalny interwał |
| ------------ |  ------------ |  ------------ |
| 1 | MAX (0, **minimumInterval**) | Min (interwał, **maximumInterval**) |
| 2 | MAX (interwał, **minimumInterval**) | Min (2 * interwał, **maximumInterval**) |
| 3 | MAX (2 * interwał, **minimumInterval**) | Min (4 * interwał, **maximumInterval**) |
| 4 | MAX (4 * interwał, **minimumInterval**) | Min (8 * interwał, **maximumInterval**) |
| Przyciski ... |

Dla zasad typu wykładniczej **liczba** i **interwał** są wymagane. Wartości **minimumInterval** i **maximumInterval** są opcjonalne. Możesz dodać je, aby zastąpić wartości domyślne PT5S i PT1D, odpowiednio.

| Nazwa elementu | Wymagane | Typ | Opis |
| ------------ | -------- | ---- | ----------- |
| type | Yes | Ciąg | **wykładniczy** |
| liczba | Yes | Liczba całkowita | Liczba ponownych prób. Musi należeć do zakresu od 1 do 90.  |
| interval | Yes | Ciąg | Interwał w ponawiania próby [formacie ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Musi należeć do zakresu od PT5S i PT1D. |
| minimumInterval | Nie | Ciąg | Spróbuj ponownie minimalny interwał w [formacie ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Musi należeć do zakresu od PT5S i **interwał**. |
| maximumInterval | Nie | Ciąg | Spróbuj ponownie minimalny interwał w [formacie ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations). Musi należeć do zakresu od **interwał** i PT1D. |

## <a name="catch-failures-with-the-runafter-property"></a>Błędy z właściwością runAfter catch

Każde działanie aplikacji logiki deklaruje akcje, które muszą zostać zakończone przed uruchomieniem akcji. Jest on podobny do kolejności kroków w przepływie pracy. W definicji działania ta kolejność jest znana jako **runAfter** właściwości. 

**RunAfter** właściwość jest obiekt, który opisuje, które akcje i Stany Akcja wykonania akcji. Domyślnie wszystkie akcje, które zostały dodane przy użyciu projektanta aplikacji logiki jest ustawiana po poprzednim kroku, jeśli wynik poprzedniego kroku jest **zakończyło się pomyślnie**. 

Można jednak dostosować **runAfter** wartość uruchomienie akcji, gdy w wyniku poprzedniego działania ****, **pomijane**, lub zestaw możliwych tych wartości. Jeśli chcesz dodać element do wyznaczonych tematu Azure Service Bus po określonym działaniu **Insert_Row** nie powiedzie się, można użyć następującego **runAfter** konfiguracji:

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

Należy pamiętać, że **runAfter** ustawiono wyzwalana, gdy **Insert_Row** jest wynik akcji ****. Aby uruchomić akcję, gdy stan działania to **zakończyło się pomyślnie**, ****, lub **pomijane**, należy użyć następującej składni:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

> [!TIP]
> Akcje, które Uruchom i zakończyła się pomyślnie po poprzednim akcja nie powiodła się są oznaczone jako **zakończyło się pomyślnie**. Oznacza to, że jeśli możesz pomyślnie catch wszystkich błędów w przepływie pracy, uruchom sam jest oznaczona jako **zakończyło się pomyślnie**.

## <a name="scopes-and-results-to-evaluate-actions"></a>Zakresy i wyniki oceny akcje

Można pogrupować Akcje wewnątrz [zakres](../logic-apps/logic-apps-loops-and-scopes.md), podobnie jak uruchomić po poszczególnych działań. Zakres działa logiczne grupowanie akcje. 

Zakresy są przydatne, zarówno do organizowania czynności użytkownika aplikacji logiki, jak i do wykonywania oceny Łączny stan zakresu. Sam zakres przechodzi w stan po ukończeniu wszystkich działań w zakresie. Stan zakresu jest określana z takich samych kryteriów jako Uruchom. Po ostatnim akcji w gałęzi wykonywania **** lub **przerwania**, stan jest **nie powiodło się**.

Aby określonych akcji żadnych błędów, które wystąpiły w zakresie, można użyć **runAfter** z zakresem, który jest oznaczony jako ****. Jeśli *żadnych* akcje w zakresie zakończą się niepowodzeniem, jeśli używasz **runAfter** dla zakresu, można utworzyć jednej akcji, aby wykryć błędów.

### <a name="get-the-context-of-failures-with-results"></a>Pobierz kontekst błędy z wynikami

Mimo że przechwytywanie błędów z zakresu jest przydatne, będą również chcieli kontekście pomagające zrozumieć dokładnie akcje, które nie powiodło się i zrozumieć ewentualne błędy lub kodów stanu, które zostały zwrócone.  **@result()** Funkcji przepływu pracy zawierają kontekst o wyniku wszystkie akcje w zakresie.

 **@result()** Funkcja przyjmuje jeden parametr (nazwa zakresu) i zwraca tablicę wszystkich akcji wyników w tym zakresie. Te obiekty działania obejmują takie same atrybuty jak  **@actions()** generuje obiekt, w tym czas rozpoczęcia działania, czas zakończenia działania stanu akcji, dane wejściowe działań, identyfikatorów korelacji działania i akcji. 

Aby wysłać kontekstu wszystkie akcje, które nie powiodło się w zakresie, można łatwo skojarzyć  **@result()** działać z **runAfter** właściwości.

Do wykonania akcji *dla każdego* akcji w zakresie, którego **nie powiodło się** wyników, aby filtrować tablicy wyników z akcjami, których nie powiodła się, służący  **@result()** z [Filter_array](../connectors/connectors-native-query.md) akcji i [foreach](../logic-apps/logic-apps-loops-and-scopes.md) pętli. Z tablicą filtrowane wyniki akcję można wykonać dla każdego błędu przy użyciu **foreach** pętli. 

Oto przykład, w którym HTTP POST wysyła żądania z treści odpowiedzi z akcji, które nie powiodło się w zakresie My_Scope:

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

Poniżej przedstawiono szczegółowy przewodnik opisujący, co się stanie w poprzednim przykładzie:

1. Aby uzyskać wynik wszystkich akcji w obrębie My_Scope, **Filter_array** filtry akcji  **@result(My_Scope)**.

2. Warunek **Filter_array** dowolnego  **@result()** element, który ma stan równa ****. Ten warunek filtruje tablicy z wszystkich wyników akcji z My_Scope, do tablicy z wynikami jedyną akcją nie powiodło się.

3. Wykonaj **foreach** akcji *tablicy filtrowane* danych wyjściowych. Ten krok wykonuje akcję *dla każdego* nie powiodło się wynik akcji, która wcześniej została przefiltrowana.

    Jeśli w zakresie jednej akcji zakończyło się niepowodzeniem, akcje w **foreach** uruchomić tylko raz. Wiele zakończonych niepowodzeniem akcje mogą spowodować jedną akcję na błąd.

4. Wyślij HTTP POST **foreach** elementu treści odpowiedzi lub  **@item() ['wyniki'] [treści]**.  **@result()** Kształt element jest taka sama jak  **@actions()** kształtu. Taki sam sposób jak mógł zostać przeanalizowany.

5. Obejmują dwa Nagłówki niestandardowe o nazwie nieudanych akcji  **@item() [nazwa]** i nieudane Uruchom klienta, identyfikator śledzenia  **@item() [clientTrackingId]**.

Odwołania, Oto przykład jedną  **@result()** elementu. Dzięki niemu **nazwa**, **treści**, i **clientTrackingId** właściwości, które są parsowane w poprzednim przykładzie. Poza metodą **foreach** akcji,  **@result()** zwraca tablicę tych obiektów.

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

Do obsługi wzorców różnych wyjątków można użyć wyrażenia opisem we wcześniejszej części tego artykułu. Może również wykonać pojedynczego wyjątków, Obsługa akcji poza zakres, który akceptuje całą macierz filtrowane awarii, a Usuń **foreach**. Możesz również uwzględnić inne przydatne właściwości z  **@result()** odpowiedzi, zgodnie z wcześniejszym opisem.

## <a name="azure-diagnostics-and-telemetry"></a>Diagnostyka Azure i telemetrii

Wzorce opisane w tym artykule zapewnić skuteczne sposoby Obsługa błędów i wyjątków wewnątrz Uruchom, ale można również zidentyfikować i odpowiadać na błędy, niezależnie od samego przebiegu. [Diagnostyka Azure](../logic-apps/logic-apps-monitor-your-logic-apps.md) zapewnia prosty sposób wysyłania wszystkich zdarzeń przepływu pracy (w tym wszystkie stany akcji i uruchom), do konta magazynu platformy Azure lub Centrum zdarzeń w usłudze Azure Event Hubs. 

Aby ocenić stany wykonywania, Monitoruj dzienniki i metryki lub publikować dowolnego narzędzia monitorowania, które chcesz. Potencjalne opcja polega na wszystkich zdarzeń za pomocą usługi Event Hubs do strumienia [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). W Stream Analytics można zapisywać zapytania na żywo na podstawie wszelkie nieprawidłowości, średnie lub błędy z dzienników diagnostycznych. Analiza strumienia służy do wysyłania informacji do innych źródeł danych, takich jak do kolejki, tematy, SQL, bazy danych rozwiązania Cosmos Azure lub usługi Power BI.

## <a name="next-steps"></a>Kolejne kroki

* Zobacz, jak klient [kompilacje obsługi z usługą Logic Apps w usłudze Azure błędów](../logic-apps/logic-apps-scenario-error-and-exception-handling.md).
* Znajdź więcej [Logic Apps przykłady i scenariusze](../logic-apps/logic-apps-examples-and-scenarios.md).
* Dowiedz się, jak utworzyć [automatycznego wdrożenia dla aplikacji logiki](../logic-apps/logic-apps-create-deploy-template.md).
* Dowiedz się, jak [tworzenia i wdrażania aplikacji logiki z programem Visual Studio](logic-apps-deploy-from-vs.md).

<!-- References -->
[retryPolicyMSDN]: https://docs.microsoft.com/rest/api/logic/actions-and-triggers#Anchor_9
