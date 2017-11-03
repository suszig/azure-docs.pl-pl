---
title: "Przepływ pracy wyzwalacze i akcje - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o rodzaju wyzwalacze i akcje, które służą do tworzenia i automatyzacji przepływów pracy i procesów przy użyciu usługi Azure Logic Apps"
services: logic-apps
author: MandiOhlinger
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 86a53bb3-01ba-4e83-89b7-c9a7074cb159
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/17/2016
ms.author: LADocs; mandia
ms.openlocfilehash: 7e0266cdc477715a5d2f9067c6dcea73da9ba763
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="triggers-and-actions-for-logic-app-workflows"></a>Wyzwalacze i akcje dla przepływów pracy aplikacji logiki

Wszystkie aplikacje logiki rozpoczynać wyzwalacz następuje akcje. W tym temacie opisano rodzaje wyzwalacze i akcje, które służy do tworzenia integracji systemu i automatyzacja przepływów pracy firmowych lub procesy, tworzenie aplikacji logiki. 
  
## <a name="triggers-overview"></a>Omówienie wyzwalaczy 

Wszystkie aplikacje logiki rozpoczynać wyzwalacz, który określa wywołania, które można uruchomić aplikację logiki, uruchom. Istnieją dwa sposoby, które można uruchomić inicjować uruchomienia przepływu pracy:  

* Wyzwalacz sondowania  
* Wyzwalacz wypychania, który wywołuje [interfejsu API REST usługi przepływu pracy](https://docs.microsoft.com/rest/api/logic/workflows)  
  
Wszystkich wyzwalaczy zawierać tych elementów najwyższego poziomu:  
  
```json
"trigger-name": {
    "type": "trigger-type",
    "inputs": { call-settings },
    "recurrence": {  
        "frequency": "Second|Minute|Hour|Day|Week|Month",
        "interval": recurrence-interval-based-on-frequency
    },
    "conditions": [ array-of-required-conditions ],
    "splitOn": "property-used-for-creating-separate-workflows",
    "operationOptions": "operation-options-for-trigger"
}
```

### <a name="trigger-types-and-inputs"></a>Typy wyzwalaczy i dane wejściowe  

Każdy typ wyzwalacza ma innego interfejsu i inne *dane wejściowe* definiuje zachowanie. 

| Typ wyzwalacza | Opis | 
| ------------ | ----------- | 
| **Cyklu** | Generowane na podstawie zdefiniowanego harmonogramu. Można ustawić przyszłą datę i godzinę dla wyzwalania tego wyzwalacza. Na podstawie częstotliwości, można również określić godziny i dni na potrzeby uruchamiania przepływu pracy. | 
| **Żądanie**  | Umożliwia aplikacji logiki do punktu końcowego, który można wywołać, znanej także jako wyzwalacz "manual". | 
| **HTTP** | Sprawdza, lub *sond*, punkt końcowy HTTP sieci web. Punkt końcowy HTTP musi być zgodny z określonym kontraktem wyzwalająca za pomocą "202" wzorca asynchronicznego lub tablicę. | 
| **ApiConnection** | Sonduje jak wyzwalacz protokołu HTTP, ale używa [zarządzany przez firmę Microsoft interfejsy API](../connectors/apis-list.md). | 
| **HTTPWebhook** | Umożliwia aplikacji logiki do punktu końcowego można wywołać, takich jak wyzwalacza żądania, ale określony adres URL odwołuje się do rejestrowania i wyrejestrowania. |
| **ApiConnectionWebhook** | Jak działa **HTTPWebhook** wyzwalacz, ale używa API zarządzany przez firmę Microsoft. | 
||| 

Informacje o innych szczegółów, zobacz [język definicji przepływu pracy](../logic-apps/logic-apps-workflow-definition-language.md). 
  
## <a name="recurrence-trigger"></a>Wyzwalacz cyklu  

Wyzwalacz uruchamia na podstawie cyklu i harmonogram, które określisz i zapewnia prosty sposób regularnie uruchamiania przepływu pracy. Oto przykład wyzwalacza cyklu podstawowych, w którym jest przeprowadzana codziennie:

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Day",
        "interval": 1
    }
}
```
Można również zaplanować datę i godzinę dla wyzwalania wyzwalacza. Na przykład aby uruchomić raport co tydzień w każdy poniedziałek, można zaplanować aplikację logiki, aby uruchomić na komputerze określonym poniedziałek, tak jak ten przykład: 

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "Week",
        "interval": "1",
        "startTime" : "2017-09-18T00:00:00Z"
    }
}
```

Poniżej przedstawiono definicję dla tego wyzwalacza: 

```json
"myRecurrenceTrigger": {
    "type": "Recurrence",
    "recurrence": {
        "frequency": "second|minute|hour|day|week|month",
        "interval": recurrence-interval-based-on-frequency,
        "schedule": {
            // Applies only when frequency is Day or Week. Separate values with commas.
            "hours": [ one-or-more-hour-marks ], 
            // Applies only when frequency is Day or Week. Separate values with commas.
            "minutes": [ one-or-more-minute-marks ], 
            // Applies only when frequency is Week. Separate values with commas.
            "weekDays": [ "Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday" ] 
        },
        "startTime": "start-date-time-with-format-YYYY-MM-DDThh:mm:ss",
        "timeZone": "specify-time-zone"
    }
}
```

| Nazwa elementu | Wymagane | Typ | Opis | 
| ------------ | -------- | ---- | ----------- | 
| frequency | Tak | Ciąg | Jednostka czasu częstotliwość wyzwalacza. Użyj tylko jedną z następujących wartości: "drugi", "min", "Godzina", "day", "tygodnia" lub "miesiąc" | 
| interval | Tak | Liczba całkowita | Dodatnia liczba całkowita, w tym artykule opisano, jak często uruchamia przepływ pracy na podstawie częstotliwości. <p>Poniżej przedstawiono minimalne i maksymalne odstępach czasu: <p>-Miesięczny: 1-16 miesięcy </br>-Dniowego: 1-500 dni </br>-Godzinnym: 1-12 000 godzin </br>-Minutowy: 1-72,000 minut </br>-Drugi: 1-9,999,999 sekund<p>Na przykład jeśli interwał to 6 i częstotliwości jest "miesiąc", cykl jest co 6 miesięcy. | 
| Strefa czasowa | Nie | Ciąg | Dotyczy tylko po określeniu godziny rozpoczęcia, ponieważ wyzwalacz nie akceptuje [przesunięcie UTC](https://en.wikipedia.org/wiki/UTC_offset). Określ strefę czasową, który chcesz zastosować. | 
| startTime | Nie | Ciąg | Określ datę i godzinę w następującym formacie: <p>RRRR-MM-Ddtgg, jeśli określona strefa czasowa <p>— lub — <p>RRRR-MM-Ddtgg, jeśli nie określisz strefy czasowej <p>Tak na przykład, jeśli chcesz 18 września 2017 godzinie 2:00, określ "2017-09-18T14:00:00" i określ strefę czasową, takie jak "Czas pacyficzny". Alternatywnie można wskazać "2017-09-18T14:00:00Z" bez strefę czasową. <p>**Uwaga:** ta godzina rozpoczęcia musi występować po [ISO 8601 daty czasu specyfikacji](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) w [format daty i godziny UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), ale bez [przesunięcie UTC](https://en.wikipedia.org/wiki/UTC_offset). Jeśli nie określisz strefy czasowej, należy dodać litery "Z" na końcu bez spacji. To "Z" odwołuje się do jego odpowiednik [milową czas](https://en.wikipedia.org/wiki/Nautical_time). <p>Proste harmonogramów, godzina rozpoczęcia jest pierwsze wystąpienie, natomiast w przypadku złożonych harmonogramy wyzwalacz nie wyzwalać żadnych wcześniej niż czas rozpoczęcia. Aby uzyskać więcej informacji na temat daty rozpoczęcia i godziny, zobacz [tworzenie i harmonogram regularnie uruchomionych zadań](../connectors/connectors-native-recurrence.md). | 
| weekDays | Nie | Ciąg lub tablicę ciągów | Jeśli określisz "Tydzień" dla `frequency`, można określić co najmniej jeden dzień, oddzielając je średnikami, jeśli chcesz uruchomić przepływ pracy: "Poniedziałek", "Wtorek", "Środa", "Czwartek", "Piątek", "Sobota" i "Niedziela" | 
| hours | Nie | Liczba całkowita lub tablicy liczba całkowita | Jeśli określisz "Day" lub "Tydzień" dla `frequency`, można określić co najmniej jeden liczby całkowite z przedziału od 0 do 23 rozdzielonych przecinkami, w formacie godziny, dnia, gdy chcesz uruchomić przepływ pracy. <p>Na przykład jeśli określisz "10", "12" i "14" możesz uzyskać 10 AM, 12 PM i 14: 00 jako znaki godzinę. | 
| minutes | Nie | Liczba całkowita lub tablicy liczba całkowita | Jeśli określisz "Dzień" lub "Tydzień" dla `frequency`, można określić co najmniej jeden liczby całkowite z przedziału od 0 do 59, oddzielonych przecinkami, jako minuty, godziny, jeśli chcesz uruchomić przepływ pracy. <p>Na przykład "30" można określić jako minuta znaku i w poprzednim przykładzie, godziny, dnia, możesz uzyskać godziny 10:30, 12:30:00 i 14:30:00. | 
|||||| 

Na przykład tego wyzwalacza cyklu określa, że aplikację logiki jest uruchamiane co tydzień w każdy poniedziałek w godziny 10:30, 12:30:00 i 14:30:00 i Pacyfik (czas standardowy), nie wcześniej niż 9 września 2017, 2:00 PM uruchamianie:

``` json
{
    "triggers": {
        "myRecurrenceTrigger": {
            "type": "Recurrence",
            "recurrence": {
                "frequency": "Week",
                "interval": 1,
                "schedule": {
                    "hours": [
                        10,
                        12,
                        14
                    ],
                    "minutes": [
                        30
                    ],
                    "weekDays": [
                        "Monday"
                    ]
                },
               "startTime": "2017-09-07T14:00:00",
               "timeZone": "Pacific Standard Time"
            }
        }
    }
}
```

Aby uzyskać więcej informacji o cyklu i przykłady czasu rozpoczęcia dla tego wyzwalacza, zobacz [tworzenie i harmonogram regularnie uruchomionych zadań](../connectors/connectors-native-recurrence.md).

## <a name="request-trigger"></a>Wyzwalacz żądania

Wyzwalacz służy jako punkt końcowy, który służy do wywoływania aplikację logiki, za pomocą żądania HTTP. Wyzwalacz żądania wygląda następująco:  
  
```json
"myRequestTrigger": {
    "type": "Request",
    "kind": "Http",
    "inputs": {
        "schema": {
            "type": "Object",
            "properties": {
                "myInputProperty1": { "type" : "string" },
                "myInputProperty2": { "type" : "number" }
            },
            "required": [ "myInputProperty1" ]
        }
    }
} 
```

Opcjonalna właściwość o nazwie ma wyzwalacz *schematu*:
  
| Nazwa elementu | Wymagane | Typ | Opis |
| ------------ | -------- | ---- | ----------- |
| Schemat | Nie | Obiekt | Schematu JSON, która weryfikuje żądanie przychodzące. Przydatne w przypadku myśl kolejnych kroków znać właściwości, które ma dotyczyć odwołanie. | 
||||| 

Aby wywołać ten punkt końcowy, należy wywołać *listCallbackUrl* interfejsu API. Zobacz [interfejsu API REST usługi przepływu pracy](https://docs.microsoft.com/rest/api/logic/workflows).

## <a name="http-trigger"></a>Wyzwalacz HTTP  

HTTP wyzwalacze sondowania określony punkt końcowy i sprawdź odpowiedzi, aby określić, czy należy uruchomić przepływ pracy. W tym miejscu `inputs` obiektu przyjmuje tych parametrów wymaganych do konstruowania wywołanie HTTP:  

| Nazwa elementu | Wymagane | Typ | Opis | 
| ------------ | -------- | ---- | ----------- | 
| — Metoda | Tak | Ciąg | Użyto jednego z tych metod HTTP: "GET", "POST", "PUT", "DELETE", "Poprawka" ani "HEAD" | 
| Identyfikator URI | Tak| Ciąg | Końcowy HTTP lub HTTPs, który sprawdza zgodność wyzwalacza. Maksymalny rozmiar ciągu: 2 KB | 
| — zapytania | Nie | Obiekt | Reprezentuje wszystkie parametry zapytania, które chcesz uwzględnić w adresie URL. <p>Na przykład `"queries": { "api-version": "2015-02-01" }` dodaje `?api-version=2015-02-01` do adresu URL. | 
| Nagłówki | Nie | Obiekt | Reprezentuje każdy nagłówek, który jest wysyłany w żądaniu. <p>Na przykład, aby ustawić język i typ żądania: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| Treści | Nie | Obiekt | Reprezentuje ładunek, które są wysyłane do punktu końcowego. | 
| retryPolicy | Nie | Obiekt | Ten obiekt jest używany dla Dostosowywanie zachowania ponownych prób dla 4xx lub 5xx błędów. | 
| Uwierzytelnianie | Nie | Obiekt | Reprezentuje metodę, która powinna być używana do uwierzytelniania żądania. Aby uzyskać więcej informacji, zobacz [uwierzytelniania połączeń wychodzących harmonogramu](../scheduler/scheduler-outbound-authentication.md). <p>Poza harmonogramem, jest jedna właściwość więcej obsługiwanych: `authority`. Domyślnie ta wartość jest `https://login.windows.net` gdy nie jest określony, ale możesz użyć innej wartości, takich jak`https://login.windows\-ppe.net`. | 
||||| 

A *zasady ponawiania* dotyczy sporadycznych błędów jest określony jako kodów stanu HTTP 408 429 i 5xx, oprócz wszelkie wyjątki łączności. Można zdefiniować te zasady z `retryPolicy` obiektów, jak pokazano poniżej:
  
```json
"retryPolicy": {
    "type": "retry-policy-type",
    "interval": retry-interval,
    "count": number-of-retry-attempts
}
```
 
Aby pracować z aplikacji logiki, wyzwalacza HTTP wymaga API protokołu HTTP był zgodny z określonym wzorcem. Wyzwalacz rozpoznaje tych właściwości:  
  
| Odpowiedź | Wymagane | Opis | 
| -------- | -------- | ----------- |  
| Kod stanu | Tak | Kod stanu 200 ("OK") powoduje, że działa. Każdy kod stanu nie powoduje uruchomienia. | 
| Spróbuj ponownie po nagłówka | Nie | Liczba sekund do aplikacji logiki sonduje ponownie punkt końcowy. | 
| Nagłówek lokalizacji | Nie | Adres URL do wywołania w następnym interwału sondowania. Jeśli nie zostanie określony, używany jest oryginalny adres URL. | 
|||| 

Poniżej przedstawiono niektóre przykładowe zachowania dla różnych typów żądań:
  
| Kod odpowiedzi | Ponów próbę po | Zachowanie | 
| ------------- | ----------- | -------- | 
| 200 | {Brak} | Uruchamianie przepływu pracy, a następnie wyszukaj ponownie większej ilości danych po zdefiniowanych cyklu. | 
| 200 | 10 sekund | Uruchamianie przepływu pracy, a następnie wyszukaj ponownie większej ilości danych po 10 sekundach. |  
| 202 | 60 sekund | Nie wyzwolić przepływu pracy. Przy następnej próbie odbywa się na minutę, mogą ulec zdefiniowanych cyklu. Zdefiniowanych cyklu jest mniej niż minutę, pierwszeństwo ma nagłówek ponownych prób po. W przeciwnym razie jest używana zdefiniowanych cyklu. | 
| 400 | {Brak} | Niewłaściwe żądanie, nie uruchamiaj przepływ pracy. Jeśli nie `retryPolicy` jest zdefiniowany, zostanie użyta domyślna zasada. Po osiągnięciu liczby ponownych prób wyzwalacz sprawdza, czy ponownie danych po zdefiniowanych cyklu. | 
| 500 | {Brak}| Błąd serwera, nie uruchamiaj przepływ pracy. Jeśli nie `retryPolicy` jest zdefiniowany, zostanie użyta domyślna zasada. Po osiągnięciu liczby ponownych prób wyzwalacz sprawdza, czy ponownie danych po zdefiniowanych cyklu. | 
|||| 

Poniżej przedstawiono dane wyjściowe wyzwalacza HTTP: 
  
| Nazwa elementu | Typ | Opis |
| ------------ | ---- | ----------- |
| Nagłówki | Obiekt | Nagłówki odpowiedzi HTTP | 
| Treści | Obiekt | Treść odpowiedzi HTTP | 
|||| 

## <a name="api-connection-trigger"></a>Połączenie z interfejsem API wyzwalacza  

Wyzwalacz połączenia interfejsu API jest podobny do wyzwalacza HTTP w jego podstawowych funkcji. Jednak parametry do identyfikacji akcji są różne. Oto przykład:  
  
```json
"myDailyReportTrigger": {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://myarticles.example.com/"
            }
        },
        "connection": {
            "name": "@parameters('$connections')['myconnection'].name"
        }
    },  
    "method": "POST",
    "body": {
        "category": "awesomest"
    }
}
```

| Nazwa elementu | Wymagane | Typ | Opis | 
| ------------ | -------- | ---- | ----------- | 
| Host | Tak | Obiekt | Hostowanej bramy i identyfikator dla aplikacji interfejsu API | 
| — Metoda | Tak | Ciąg | Użyto jednego z tych metod HTTP: "GET", "POST", "PUT", "DELETE", "Poprawka" ani "HEAD" | 
| — zapytania | Nie | Obiekt | Reprezentuje wszystkie parametry zapytania, które chcesz uwzględnić w adresie URL. <p>Na przykład `"queries": { "api-version": "2015-02-01" }` dodaje `?api-version=2015-02-01` do adresu URL. | 
| Nagłówki | Nie | Obiekt | Reprezentuje każdy nagłówek, który jest wysyłany w żądaniu. <p>Na przykład, aby ustawić język i typ żądania: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| Treści | Nie | Obiekt | Reprezentuje ładunek, które są wysyłane do punktu końcowego. | 
| retryPolicy | Nie | Obiekt | Ten obiekt jest używany dla Dostosowywanie zachowania ponownych prób dla 4xx lub 5xx błędów. | 
| Uwierzytelnianie | Nie | Obiekt | Reprezentuje metodę, która powinna być używana do uwierzytelniania żądania. Aby uzyskać więcej informacji, zobacz [uwierzytelniania połączeń wychodzących harmonogramu](../scheduler/scheduler-outbound-authentication.md). | 
||||| 

Aby uzyskać `host` obiektu, w tym miejscu są właściwości:  
  
| Nazwa elementu | Wymagane | Opis | 
| ------------ | -------- | ----------- | 
| runtimeUrl interfejsu API | Tak | Punkt końcowy dla zarządzanego interfejsu API | 
| Nazwa połączenia |  | Nazwa zarządzanego połączenia interfejsu API, używanych przez przepływ pracy. Musi odwoływać się do parametru o nazwie `$connection`. |
|||| 

A *zasady ponawiania* dotyczy sporadycznych błędów jest określony jako kodów stanu HTTP 408 429 i 5xx, oprócz wszelkie wyjątki łączności. Można zdefiniować te zasady z `retryPolicy` obiektów, jak pokazano poniżej:
  
```json
"retryPolicy": {
    "type": "retry-policy-type",
    "interval": retry-interval,
    "count": number-of-retry-attempts
}
```

Poniżej przedstawiono dane wyjściowe dla wyzwalacza połączenia interfejsu API:
  
| Nazwa elementu | Typ | Opis |
| ------------ | ---- | ----------- |
| Nagłówki | Obiekt | Nagłówki odpowiedzi HTTP | 
| Treści | Obiekt | Treść odpowiedzi HTTP | 
|||| 
  
## <a name="httpwebhook-trigger"></a>Wyzwalacz HTTPWebhook  

Wyzwalacz HTTPWebhook zapewnia punkt końcowy, podobnie jak wyzwalacza żądania, ale wyzwalacza HTTPWebhook wymaga również określony adres URL rejestrowania i wyrejestrowania. Oto przykład co wyzwalacz HTTPWebhook może wyglądać tak:  

```json
"myAppsSpotTrigger": {
    "type": "HttpWebhook",
    "inputs": {
        "subscribe": {
            "method": "POST",
            "uri": "https://pubsubhubbub.appspot.com/subscribe",
            "headers": { },
            "body": {
                "hub.callback": "@{listCallbackUrl()}",
                "hub.mode": "subscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "authentication": { },
            "retryPolicy": { }
        },
        "unsubscribe": {
            "url": "https://pubsubhubbub.appspot.com/subscribe",
            "body": {
                "hub.callback": "@{workflow().endpoint}@{listCallbackUrl()}",
                "hub.mode": "unsubscribe",
                "hub.topic": "https://pubsubhubbub.appspot.com/articleCategories/technology"
            },
            "method": "POST",
            "authentication": { }
        }
    },
    "conditions": [ ]
}
```

Wiele z tych sekcji są opcjonalne i zachowanie wyzwalacza HTTPWebhook zależy od sekcje, które zapewniają lub Pomiń. Poniżej przedstawiono właściwości wyzwalacza HTTPWebhook:
  
| Nazwa elementu | Wymagane | Opis | 
| ------------ | -------- | ----------- |  
| Subskrypcja | Nie | Określa żądania wychodzącego do wywołania po utworzeniu wyzwalacza i wykonuje początkowe rejestracyjny. | 
| Anulowanie subskrypcji | Nie | Określa żądania wychodzącego do wywołania po wyzwalacz jest usuwany. | 
|||| 

Można określić limity akcji elementu webhook w taki sam sposób jak [limity asynchroniczne HTTP](#asynchronous-limits). Poniżej przedstawiono więcej informacji na temat `subscribe` i `unsubscribe` akcje:

* `subscribe`nazywa się tak, aby uruchomić wyzwalacz nasłuchuje zdarzeń. To wywołanie wychodzące rozpoczyna się od takich samych parametrach co standardowe akcji HTTP. To wywołanie odbywa się podczas przepływu pracy zmiany w dowolny sposób, na przykład jeśli poświadczenia zostały wycofane, lub zmień parametry wejściowe tego wyzwalacza. 
  
  Do obsługi tego wywołania `@listCallbackUrl()` funkcja zwraca unikatowy adres URL dla tego wyzwalacza określonych w przepływie pracy. Ten adres URL reprezentuje unikatowy identyfikator dla punktów końcowych, które używają interfejsu API REST usługi.
  
* `unsubscribe`jest automatycznie wywoływane, gdy operacja renderuje tego wyzwalacza nieprawidłowe, w tym te operacje:

  * Usuwanie lub wyłączanie wyzwalacza. 
  * Usuwanie lub wyłączanie przepływ pracy. 
  * Usuwanie lub wyłączanie subskrypcji. 
  
  Parametry dla tej funkcji są takie same jak wyzwalacza HTTP.

Poniżej przedstawiono dane wyjściowe z HTTPWebhook wyzwolenia i treść żądania przychodzące są:
  
| Nazwa elementu | Typ | Opis |
| ------------ | ---- | ----------- |
| Nagłówki | Obiekt | Nagłówki odpowiedzi HTTP | 
| Treści | Obiekt | Treść odpowiedzi HTTP | 
|||| 

## <a name="conditions"></a>Warunki  

Dla dowolnego wyzwalacza można użyć co najmniej jednego warunku można określić, czy przepływ pracy, należy uruchomić lub nie. Na przykład:  

```json
"myDailyReportTrigger": {
    "type": "Recurrence",
    "conditions": [ 
        {
            "expression": "@parameters('sendReports')"
        } 
    ],
    "recurrence": {
        "frequency": "Day",
        "interval": 1
    }
}
```

W takim przypadku tylko wyzwalacze raportu podczas przepływu pracy `sendReports` parametr ma wartość true. Na koniec warunki można odwoływać się do kod stanu wyzwalacza. Na przykład tylko wtedy, gdy witryny sieci Web zwraca kod stanu 500, na przykład można uruchomić przepływu pracy:
  
``` json
"conditions": [ 
    {  
      "expression": "@equals(triggers().code, 'InternalServerError')"  
    }  
]  
```  
  
> [!NOTE]  
> Gdy dowolne wyrażenie odwołuje się do tego wyzwalacza kodu stanu w dowolny sposób, zachowanie domyślne, czyli wyzwalacza tylko na 200 przycisk OK, zostanie zastąpiony. Na przykład, jeśli chcesz wyzwolić na kod stanu 201 i kodem stanu 200, użytkownik musi zawierać: `@or(equals(triggers().code, 200),equals(triggers().code,201))` jako warunek.
  
## <a name="start-multiple-runs-for-a-request"></a>Uruchom wielu uruchomień dla żądania

Aby rozpocząć poza wielu uruchomień dla pojedynczego żądania `splitOn` jest przydatne, na przykład, gdy chcesz przeprowadzać sondowanie punktu końcowego, który może mieć wiele nowych elementów między interwałami sondowania.
  
Z `splitOn`, określ właściwość ładunku odpowiedzi, który zawiera tablicę elementów, z których każdy ma być używany do uruchamiania wykonywania wyzwalacza. Załóżmy na przykład, że masz interfejs API, który zwraca odpowiedź:  
  
```json
{
    "status": "Succeeded",
    "rows": [
        {  
            "id" : 938109380,
            "name" : "myFirstRow"
        },
        {
            "id" : 938109381,
            "name" : "mySecondRow"
        }
    ]
}
```
  
Musi tylko aplikację logiki `rows` zawartości, dlatego można konstruować wyzwalacz tak jak ten przykład:  

```json
"mySplitterTrigger": {
    "type": "Http",
    "recurrence": {
        "frequency": "minute",
        "interval": 1
    },
    "intputs": {
        "uri": "https://mydomain.com/myAPI",
        "method": "GET"
    },
    "splitOn": "@triggerBody()?.rows"
}
```
> [!NOTE]  
> Jeśli używasz `SplitOn` polecenia i nie można pobrać właściwości, które są poza tablicy, tak na przykład nie można pobrać `status` zwrócił w odpowiedzi z interfejsu API.
> Ponadto, w tym przykładzie używamy `?` operatora, firma Microsoft można uniknąć awarii `rows` właściwość nie istnieje. 

Tak w definicji przepływu pracy `@triggerBody().name` zwraca `myFirstRow` dla przy pierwszym uruchomieniu i `mySecondRow` dla drugiego przebiegu. Wygląd danych wyjściowych wyzwalacz tak jak ten przykład:  

```json
{
    "body": {
        "id": 938109380,
        "name": "mySecondRow"
    }
}
```

```json
{
    "body": {
        "id": 938109381,
        "name": "mySecondRow"
    }
}
```
  
## <a name="single-run-instance"></a>Pojedyncze wystąpienie wykonywania

Wyzwalacze cyklu można skonfigurować tak, aby ich uruchomienie tylko, gdy wszystkie aktywnej przebiegi została ukończona. W przypadku wystąpienia przepływu pracy jest uruchomiona zaplanowanego cyklu wyzwalacza pomija i czeka, aż do następnego interwał cyklu zaplanowane, aby ponownie sprawdzić.
Aby skonfigurować to ustawienie, ustaw `operationOptions` właściwości `singleInstance`:

```json
"triggers": {
    "myHTTPTrigger": {
        "type": "Http",
        "inputs": { ... },
        "recurrence": { ... },
        "operationOptions": "singleInstance"
    }
}
```

## <a name="actions-overview"></a>Przegląd działań

Istnieje wiele typów działań, każde z nich unikatowe zachowanie. Każdy typ działania ma różnych komponentów, które określają zachowanie akcji. Akcje kolekcji może zawierać wiele działań między sobą. 

### <a name="standard-actions"></a>Działania standardowe  

| Typ akcji | Opis | 
| ----------- | ----------- | 
| **HTTP** | Wywołuje punkt końcowy HTTP sieci web. | 
| **ApiConnection**  | Działa jak akcja HTTP, ale używa [zarządzany przez firmę Microsoft interfejsy API](https://docs.microsoft.com/azure/connectors/apis-list). | 
| **ApiConnectionWebhook** | Działa jak HTTPWebhook, ale używają API zarządzany przez firmę Microsoft. | 
| **Odpowiedź** | Określa odpowiedź dla połączenia przychodzącego. | 
| **Funkcja** | Reprezentuje funkcję platformy Azure. | 
| **Oczekiwania** | Czeka stałą lub określonego czasu. | 
| **Przepływ pracy** | Reprezentuje zagnieżdżony przepływ pracy. | 
| **Redaguj** | Tworzy obiekt arbitary akcji danych wejściowych. | 
| **Zapytanie** | Filtruje tablicy na podstawie warunku. | 
| **Wybierz** | Projektów każdy element tablicy na nową wartość. Na przykład można przekonwertować tablicy liczb znajdujących się na tablicę obiektów. | 
| **Tabela** | Konwertuje tablicę elementów do tabeli CSV lub HTML. | 
| **Zakończenie** | Zatrzymuje wykonywanie przepływu pracy. | 
||| 

### <a name="collection-actions"></a>Akcje kolekcji

| Typ akcji | Opis | 
| ----------- | ----------- | 
| **Warunek** | Oblicza wyrażenie i na podstawie wyniku, uruchamia odpowiednie gałęzi. | 
| **Zakres** | Służy do logicznego grupowania innych działań. | 
| **Instrukcja ForEach** | Ta akcja pętli iteruje tablicy i wykonuje akcje wewnętrzne na każdy element tablicy. | 
| **Do czasu** | Ta akcja pętli wykonuje akcje wewnętrzny dopóki warunek wyników do wartości true. | 
||| 

## <a name="http-action"></a>Akcja HTTP  

Akcje HTTP wywołać określony punkt końcowy i sprawdź odpowiedzi, aby określić, czy należy uruchomić przepływ pracy. Na przykład:
  
```json
"myLatestNewsAction": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest"
    }
}
```

W tym miejscu `inputs` obiektu przyjmuje tych parametrów wymaganych do konstruowania wywołanie HTTP: 

| Nazwa elementu | Wymagane | Typ | Opis | 
| ------------ | -------- | ---- | ----------- | 
| — Metoda | Tak | Ciąg | Użyto jednego z tych metod HTTP: "GET", "POST", "PUT", "DELETE", "Poprawka" ani "HEAD" | 
| Identyfikator URI | Tak| Ciąg | Końcowy HTTP lub HTTPs, który sprawdza zgodność wyzwalacza. Maksymalny rozmiar ciągu: 2 KB | 
| — zapytania | Nie | Obiekt | Reprezentuje wszystkie parametry zapytania, które chcesz uwzględnić w adresie URL. <p>Na przykład `"queries": { "api-version": "2015-02-01" }` dodaje `?api-version=2015-02-01` do adresu URL. | 
| Nagłówki | Nie | Obiekt | Reprezentuje każdy nagłówek, który jest wysyłany w żądaniu. <p>Na przykład, aby ustawić język i typ żądania: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| Treści | Nie | Obiekt | Reprezentuje ładunek, które są wysyłane do punktu końcowego. | 
| retryPolicy | Nie | Obiekt | Ten obiekt jest używany dla Dostosowywanie zachowania ponownych prób dla 4xx lub 5xx błędów. | 
| operationsOptions | Nie | Ciąg | Definiuje zestaw specjalnego zachowania do zastąpienia. | 
| Uwierzytelnianie | Nie | Obiekt | Reprezentuje metodę, która powinna być używana do uwierzytelniania żądania. Aby uzyskać więcej informacji, zobacz [uwierzytelniania połączeń wychodzących harmonogramu](../scheduler/scheduler-outbound-authentication.md). <p>Poza harmonogramem, jest jedna właściwość więcej obsługiwanych: `authority`. Domyślnie ta wartość jest `https://login.windows.net` gdy nie jest określony, ale możesz użyć innej wartości, takich jak`https://login.windows\-ppe.net`. | 
||||| 

Akcje HTTP oraz akcje APIConnection obsługuje *ponów zasady*. Zasady ponawiania dotyczy sporadycznych błędów jest określony jako kodów stanu HTTP 408 429 i 5xx, oprócz wszelkie wyjątki łączności. Można zdefiniować te zasady z `retryPolicy` obiektów, jak pokazano poniżej:
  
```json
"retryPolicy": {
    "type": "retry-policy-type",
    "interval": retry-interval,
    "count": number-of-retry-attempts
}
```
W tym przykładzie akcji HTTP ponowi próbę pobieranie najnowsze dwa razy, jeśli występują sporadyczne błędy dla wszystkich trzech wykonaniami i 30-sekundowe opóźnienie między kolejnymi próbami:
  
```json
"myLatestNewsAction": {
    "type": "Http",
    "inputs": {
        "method": "GET",
        "uri": "https://mynews.example.com/latest",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT30S",
            "count": 2
        }
    }
}
```

Interwał ponawiania prób jest określona w [formacie ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). Ten interwał ma wartości domyślnej i co najmniej 20 sekund, podczas gdy wartość maksymalna to jedna godzina. Domyślne i maksymalnej liczby ponownych prób odpowiada czterem godzinom. Jeśli nie określisz definicję zasad ponawiania `fixed` strategii jest używany z domyślnej wartości interwału i liczba ponownych prób. Aby wyłączyć zasady ponawiania, ustaw jej typ `None`.

### <a name="asynchronous-patterns"></a>Asynchroniczne wzorce

Domyślnie wszystkie działania oparte na protokole HTTP obsługuje wzorzec standardowych operacji asynchronicznej. Tak w serwerze zdalnym wskazuje, czy żądanie jest zaakceptowano do przetwarzania za pomocą odpowiedzi "202 ZAAKCEPTOWANE", aparat Logic Apps śledzi sondowanie adres URL określony w odpowiedzi nagłówek lokalizacji do momentu osiągnięcia terminali stan odpowiedzi z systemem innym niż 202.
  
Aby wyłączyć asynchroniczne zachowanie opisany powyżej, ustaw `operationOptions` do `DisableAsyncPattern` w danych wejściowych działania. W takim przypadku wynik akcji jest oparty na początkowej 202 odpowiedzi z serwera. Na przykład:
  
```json
"invokeLongRunningOperationAction": {
    "type": "Http",
    "inputs": {
        "method": "POST",
        "uri": "https://host.example.com/resources"
    },
    "operationOptions": "DisableAsyncPattern"
}
```
<a name="asynchronous-limits"></a>

#### <a name="asynchronous-limits"></a>Limity asynchroniczne

Można ograniczyć czas trwania asynchroniczny wzorzec do określonego interwału. Jeśli interwał czasu musi upłynąć bez osiągnięcia terminali stanu, stanu akcji jest oznaczony jako `Cancelled` z `ActionTimedOut` kodu. Limit czasu został określony w formacie ISO 8601. Można określić ograniczenia, jak pokazano poniżej:

``` json
"action-name": {
    "type": "Workflow|Webhook|Http|ApiConnectionWebhook|ApiConnection",
    "inputs": { },
    "limit": {
        "timeout": "PT10S"
    }
}
```
  
## <a name="apiconnection-action"></a>Akcja APIConnection

Akcja APIConnection odwołuje się do łącznika zarządzany przez firmę Microsoft. Ta akcja wymaga odwołania do prawidłowego połączenia i informacji na temat interfejsu API i parametry.
Oto przykład APIConnection akcji:

```json
"Send_Email": {
    "type": "ApiConnection",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "method": "POST",
        "body": {
            "Subject": "New tweet from @{triggerBody()['TweetedBy']}",
            "Body": "@{triggerBody()['TweetText']}",
            "To": "me@example.com"
        },
        "path": "/Mail"
    },
    "runAfter": {}
}
```

| Nazwa elementu | Wymagane | Typ | Opis | 
| ------------ | -------- | ---- | ----------- | 
| Host | Tak | Obiekt | Reprezentuje informacje łącznika, takich jak `runtimeUrl` i odwołania do obiektu połączenia. | 
| — Metoda | Tak | Ciąg | Użyto jednego z tych metod HTTP: "GET", "POST", "PUT", "DELETE", "Poprawka" ani "HEAD" | 
| Ścieżka | Tak | Ciąg | Ścieżka dla operacji interfejsu API | 
| — zapytania | Nie | Obiekt | Reprezentuje wszystkie parametry zapytania, które chcesz uwzględnić w adresie URL. <p>Na przykład `"queries": { "api-version": "2015-02-01" }` dodaje `?api-version=2015-02-01` do adresu URL. | 
| Nagłówki | Nie | Obiekt | Reprezentuje każdy nagłówek, który jest wysyłany w żądaniu. <p>Na przykład, aby ustawić język i typ żądania: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| Treści | Nie | Obiekt | Reprezentuje ładunek, które są wysyłane do punktu końcowego. | 
| retryPolicy | Nie | Obiekt | Ten obiekt jest używany dla Dostosowywanie zachowania ponownych prób dla 4xx lub 5xx błędów. | 
| operationsOptions | Nie | Ciąg | Definiuje zestaw specjalnego zachowania do zastąpienia. | 
| Uwierzytelnianie | Nie | Obiekt | Reprezentuje metodę, która powinna być używana do uwierzytelniania żądania. Aby uzyskać więcej informacji, zobacz [uwierzytelniania połączeń wychodzących harmonogramu](../scheduler/scheduler-outbound-authentication.md). |
||||| 

Zasady ponawiania dotyczy sporadycznych błędów jest określony jako kodów stanu HTTP 408 429 i 5xx, oprócz wszelkie wyjątki łączności. Można zdefiniować te zasady z `retryPolicy` obiektów, jak pokazano poniżej:
  
```json
"retryPolicy": {
    "type": "retry-policy-type",
    "interval": retry-interval,
    "count": number-of-retry-attempts
}
```

## <a name="apiconnection-webhook-action"></a>APIConnection akcji elementu webhook

Akcja APIConnectionWebhook odwołuje się do łącznika zarządzany przez firmę Microsoft. Ta akcja wymaga odwołania do prawidłowego połączenia i informacji na temat interfejsu API i parametry. Można określić limity akcji elementu webhook w taki sam sposób jak [limity asynchroniczne HTTP](#asynchronous-limits).

```json
"Send_approval_email": {
    "type": "ApiConnectionWebhook",
    "inputs": {
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-df.azure-apim.net/apim/office365"
            },
            "connection": {
                "name": "@parameters('$connections')['office365']['connectionId']"
            }
        },
        "body": {
            "Message": {
                "Subject": "Approval Request",
                "Options": "Approve, Reject",
                "Importance": "Normal",
                "To": "me@email.com"
            }
        },
        "path": "/approvalmail",
        "authentication": "@parameters('$authentication')"
    },
    "runAfter": {}
}
```

| Nazwa elementu | Wymagane | Typ | Opis | 
| ------------ | -------- | ---- | ----------- | 
| Host | Tak | Obiekt | Reprezentuje informacje łącznika, takich jak `runtimeUrl` i odwołania do obiektu połączenia. | 
| Ścieżka | Tak | Ciąg | Ścieżka dla operacji interfejsu API | 
| — zapytania | Nie | Obiekt | Reprezentuje wszystkie parametry zapytania, które chcesz uwzględnić w adresie URL. <p>Na przykład `"queries": { "api-version": "2015-02-01" }` dodaje `?api-version=2015-02-01` do adresu URL. | 
| Nagłówki | Nie | Obiekt | Reprezentuje każdy nagłówek, który jest wysyłany w żądaniu. <p>Na przykład, aby ustawić język i typ żądania: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| Treści | Nie | Obiekt | Reprezentuje ładunek, które są wysyłane do punktu końcowego. | 
| retryPolicy | Nie | Obiekt | Ten obiekt jest używany dla Dostosowywanie zachowania ponownych prób dla 4xx lub 5xx błędów. | 
| operationsOptions | Nie | Ciąg | Definiuje zestaw specjalnego zachowania do zastąpienia. | 
| Uwierzytelnianie | Nie | Obiekt | Reprezentuje metodę, która powinna być używana do uwierzytelniania żądania. Aby uzyskać więcej informacji, zobacz [uwierzytelniania połączeń wychodzących harmonogramu](../scheduler/scheduler-outbound-authentication.md). |
||||| 

## <a name="response-action"></a>Akcja odpowiedzi  

Ta akcja zawiera ładunek całej odpowiedzi z żądania HTTP i `statusCode`, `body`, i `headers`:
  
```json
"myResponseAction": {
    "type": "response",
    "inputs": {
        "statusCode": 200,
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        },
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        }
    },
    "runAfter": {}
}
```

Akcja odpowiedź ma specjalne ograniczenia, które nie dotyczą innych działań, w szczególności:  
  
* Akcje reakcji nie może mieć w równoległych gałęziach w ramach definicji aplikacji logiki, ponieważ przychodzące żądanie wymaga deterministyczne odpowiedzi.
  
* Jeśli przepływ pracy osiągnie akcji odpowiedzi po żądania przychodzącego, jeśli już odebrał odpowiedź, akcji odpowiedzi uważa się nie powiodło się lub powoduje konflikt. W związku z tym jest oznaczony jako Uruchom aplikację logiki `Failed`.
  
* Nie można użyć przepływu pracy z akcjami odpowiedzi `splitOn` polecenie w definicji wyzwalacza, ponieważ wywołanie tworzy wiele przebiegów. W związku z tym sprawdzanie dla tego przypadku, gdy operacji przepływu pracy jest WŁĄCZONY i zwrócenie odpowiedzi "złe żądanie".

## <a name="function-action"></a>Akcja — funkcja   

Ta akcja umożliwia reprezentują i wywołanie [funkcji Azure](../azure-functions/functions-overview.md), na przykład:

```json
"my-Azure-Function-name": {
   "type": "Function",
    "inputs": {
        "function": {
            "id": "/subscriptions/{Azure-subscription-ID}/resourceGroups/{Azure-resource-group}/providers/Microsoft.Web/sites/{your-Azure-function-app-name}/functions/{your-Azure-function-name}"
        },
        "queries": {
            "extrafield": "specialValue"
        },  
        "headers": {
            "x-ms-date": "@utcnow()"
        },
        "method": "POST",
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "runAfter": {}
}
```
| Nazwa elementu | Wymagane | Typ | Opis | 
| ------------ | -------- | ---- | ----------- |  
| id — funkcja | Tak | Ciąg | Identyfikator zasobu dla funkcji platformy Azure, która ma zostać wywołana. | 
| — Metoda | Nie | Ciąg | Metoda HTTP używana do wywołania tej funkcji. Jeśli nie zostanie określony, "POST" jest domyślną metodą. | 
| — zapytania | Nie | Obiekt | Reprezentuje wszystkie parametry zapytania, które chcesz uwzględnić w adresie URL. <p>Na przykład `"queries": { "api-version": "2015-02-01" }` dodaje `?api-version=2015-02-01` do adresu URL. | 
| Nagłówki | Nie | Obiekt | Reprezentuje każdy nagłówek, który jest wysyłany w żądaniu. <p>Na przykład, aby ustawić język i typ żądania: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| Treści | Nie | Obiekt | Reprezentuje ładunek, które są wysyłane do punktu końcowego. | 
|||||

Podczas zapisywania aplikacji logiki Azure Logic Apps sprawdza funkcji, do którego istnieje odwołanie:

* Musi mieć dostęp do funkcji.
* Można użyć tylko standardowe wyzwalacze HTTP lub ogólny wyzwalaczy elementu webhook JSON.
* Funkcja nie powinny mieć żadnej trasy zdefiniowane.
* Tylko "funkcji" i "anonymous" autoryzacji na poziomie jest dozwolone.

Adres URL wyzwalacza jest pobierana, w pamięci podręcznej i używany w czasie wykonywania. Dlatego jeśli żadnej operacji unieważnia pamięci podręcznej adres URL, akcja zakończy się niepowodzeniem w czasie wykonywania. Aby obejść ten problem, należy zapisać aplikację logiki, co powoduje, że aplikację logiki, aby pobrać i ponownie pamięci podręcznej adres URL wyzwalacza.

## <a name="wait-action"></a>Poczekaj akcji  

Ta akcja wstrzymuje wykonywanie przepływu pracy dla określonego interwału. W tym przykładzie powoduje uruchomienie przepływu pracy poczekaj 15 minut:
  
```json
"waitForFifteenMinutesAction": {
    "type": "Wait",
    "inputs": {
        "interval": {
            "unit": "minute",
            "count": 15
        }
    }
}
```
  
Można również czekać, aż do określonego momentu w czasie, można użyć w tym przykładzie:
  
```json
"waitUntilOctoberAction": {
    "type": "Wait",
    "inputs": {
        "until": {
            "timestamp": "2017-10-01T00:00:00Z"
        }
    }
}
```
  
> [!NOTE]  
> Czas oczekiwania albo za pomocą można określić `until` obiektu lub `interval` obiektu, ale nie oba.
  
| Nazwa elementu | Wymagane | Typ | Opis | 
| ------------ | -------- | ---- | ----------- | 
| do czasu | Nie | Obiekt | Czas trwania oczekiwania na podstawie punktu w czasie | 
| do sygnatury czasowej | Tak | Ciąg | Do punktu w czasie w [format daty i godziny UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) wygaśnięcia czas oczekiwania | 
| interval | Nie | Obiekt | Czas oczekiwania na podstawie jednostkę interwału i liczby | 
| Interwał | Tak | Ciąg | Jednostka czasu. Użyj tylko jedną z następujących wartości: "drugi", "min", "Godzina", "day", "tygodnia" lub "miesiąc" | 
| Liczba interwale | Tak | Liczba całkowita | Dodatnią liczbę całkowitą reprezentującą liczbę jednostek interwału czasu trwania oczekiwania | 
||||| 

## <a name="workflow-action"></a>Działania przepływu pracy   

Ta akcja reprezentuje innego przepływu pracy. Logic Apps wykonuje kontrolę dostępu na przepływu pracy lub w szczególności wyzwalacza, co oznacza, że trzeba mieć dostęp do przepływu pracy.

Ta akcja dane wyjściowe są oparte na definiowanie w `response` akcji podrzędnych przepływu pracy. Jeśli jeszcze nie zdefiniowano `response` akcji, a następnie dane wyjściowe są puste.

```json
"myNestedWorkflowAction": {
    "type": "Workflow",
    "inputs": {
        "host": {
            "id": "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName": "mytrigger001"
        },
        "queries": {
            "extrafield": "specialValue"
        },  
        "headers": {
            "x-ms-date": "@utcnow()",
            "Content-type": "application/json"
        },
        "body": {
            "contentFieldOne": "value100",
            "anotherField": 10.001
        }
    },
    "runAfter": {}
}
```

| Nazwa elementu | Wymagane | Typ | Opis | 
| ------------ | -------- | ---- | ----------- |  
| Identyfikator hosta | Tak | Ciąg| Identyfikator zasobu dla przepływu pracy, który ma zostać wywołana | 
| Nazwa_wyzwalacza hosta | Tak | Ciąg | Nazwa wyzwalacza, który chcesz wywołać | 
| — zapytania | Nie | Obiekt | Reprezentuje wszystkie parametry zapytania, które chcesz uwzględnić w adresie URL. <p>Na przykład `"queries": { "api-version": "2015-02-01" }` dodaje `?api-version=2015-02-01` do adresu URL. | 
| Nagłówki | Nie | Obiekt | Reprezentuje każdy nagłówek, który jest wysyłany w żądaniu. <p>Na przykład, aby ustawić język i typ żądania: <p>`"headers": { "Accept-Language": "en-us", "Content-Type": "application/json" }` | 
| Treści | Nie | Obiekt | Reprezentuje ładunek, które są wysyłane do punktu końcowego. | 
|||||   

## <a name="compose-action"></a>Redagowanie akcji

Ta akcja umożliwia utworzenia dowolnego obiektu, a dane wyjściowe są wyniku obliczenia akcji danych wejściowych. 

> [!NOTE]
> Można użyć `Compose` akcji dla tworzenia żadnych danych wyjściowych, w tym obiektów, tablic i innego typu obsługiwane przez logikę aplikacji, takich jak XML i pliku binarnego.

Na przykład można użyć akcji tworzenia scalania danych wyjściowych z wielu działań:

```json
"composeUserRecordAction": {
    "type": "Compose",
    "inputs": {
        "firstName": "@actions('getUser').firstName",
        "alias": "@actions('getUser').alias",
        "thumbnailLink": "@actions('lookupThumbnail').url"
    }
}
```

## <a name="select-action"></a>Wybierz akcję

Ta akcja umożliwia projektów każdy element tablicy na nową wartość.
Na przykład aby przekonwertować tablicy liczb znajdujących się na tablicę obiektów, można:

```json
"selectNumbersAction": {
    "type": "Select",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "select": { "number": "@item()" }
    }
}
```

| Nazwa | Wymagane | Typ | Opis | 
| ---- | -------- | ---- | ----------- | 
| Z | Tak | Tablica | Tablica źródłowa |
| Wybierz | Tak | Dowolne | Projekcja zastosowane do każdego elementu w tablicy źródłowej |
||||| 

Dane wyjściowe z `select` akcji jest tablicą, który ma tego samego Kardynalność jako tablica wejściowa. Każdy element jest przekształcana zgodnie z definicją w `select` właściwości. Jeśli dane wejściowe jest pusta tablica, dane wyjściowe jest również być pustą tablicą.

## <a name="query-action"></a>Akcja kwerendy

Ta akcja umożliwia filtrowanie na podstawie warunku tablicy. W tym przykładzie wybiera większa niż dwie liczb:

```json
"filterNumbersAction": {
    "type": "Query",
    "inputs": {
        "from": [ 1, 3, 0, 5, 4, 2 ],
        "where": "@greater(item(), 2)"
    }
}
```

Dane wyjściowe z `query` akcji jest tablicę, która ma elementy z tablicy wejściowej, które spełniają warunek.

> [!NOTE]
> Jeśli spełniać żadnych wartości `where` warunek, wynik ma być pustą tablicą.

| Nazwa | Wymagane | Typ | Opis | 
| ---- | -------- | ---- | ----------- | 
| Z | Tak | Tablica | Tablica źródłowa |
| gdzie | Tak | Ciąg | Warunek, który jest stosowany do każdego elementu z tablicy źródłowej |
||||| 

## <a name="table-action"></a>Akcja tabeli

Ta akcja umożliwia konwertowanie tablicę elementów do **CSV** lub **HTML** tabeli. Na przykład, załóżmy, że masz `@triggerBody()` z tej tablicy:

```json
[ 
    {
      "id": 0,
      "name": "apples"
    },
    {
      "id": 1, 
      "name": "oranges"
    }
]
```

I definiowania akcji tabeli tak jak ten przykład:

```json
"convertToTableAction": {
    "type": "Table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html"
    }
}
```

Wynik, w tym przykładzie wygląda tej tabeli HTML: 

<table><thead><tr><th>id</th><th>name</th></tr></thead><tbody><tr><td>0</td><td>jabłek</td></tr><tr><td>1</td><td>Pomarańcze</td></tr></tbody></table>

Aby dostosować tej tabeli, można określić kolumn jawnie, na przykład:

```json
"ConvertToTableAction": {
    "type": "Table",
    "inputs": {
        "from": "@triggerBody()",
        "format": "html",
        "columns": [ 
            {
                "header": "Produce ID",
                "value": "@item().id"
            },
            {
              "header": "Description",
              "value": "@concat('fresh ', item().name)"
            }
        ]
    }
}
```

Wynik, w tym przykładzie wygląda tej tabeli HTML: 

<table><thead><tr><th>Tworzy identyfikator</th><th>Opis</th></tr></thead><tbody><tr><td>0</td><td>Nowa jabłek</td></tr><tr><td>1</td><td>świeżych pomarańczy</td></tr></tbody></table>

| Nazwa | Wymagane | Typ | Opis | 
| ---- | -------- | ---- | ----------- | 
| Z | Tak | Tablica | Tablica źródłowa. Jeśli `from` wartość właściwości jest pusta tablica, dane wyjściowe jest pusta tabela. | 
| Format | Tak | Ciąg | Format tabeli, który ma, albo **CSV** lub **HTML** | 
| kolumny | Nie | Tablica | Kolumny tabeli, które mają. Umożliwia zastąpienie domyślnego kształtu tabeli. | 
| Nagłówek kolumny | Nie | Ciąg | Nagłówek kolumny | 
| wartość w kolumnie | Tak | Ciąg | Wartość kolumny | 
||||| 

## <a name="terminate-action"></a>Zakończenie akcji

Ta akcja zatrzymuje uruchomienie przepływu pracy, anuluje wszystkie akcje locie i pomija wszystkie pozostałe akcje. Akcja zakończenia nie wpłynie na wszystkie akcje zakończono.

Na przykład aby zatrzymać wykonywania, której "Nie" stan, służy w tym przykładzie:

```json
"handleUnexpectedResponseAction": {
    "type": "Terminate",
    "inputs": {
        "runStatus": "Failed",
        "runError": {
            "code": "UnexpectedResponse",
            "message": "Received an unexpected response"
        }
    }
}
```

| Nazwa | Wymagane | Typ | Opis | 
| ---- | -------- | ---- | ----------- | 
| runStatus | Tak | Ciąg | Element docelowy, uruchom jego stan, który może być `Failed` lub`Cancelled` |
| runError | Nie | Obiekt | Szczegóły błędu. Obsługiwane tylko wtedy, gdy `runStatus` ma ustawioną wartość `Failed`. |
| Kod runError | Nie | Ciąg | Kod błędu przy uruchomieniu |
| komunikat runError | Nie | Ciąg | Komunikat o błędzie przy uruchomieniu |
||||| 

## <a name="collection-actions-overview"></a>Przegląd działań kolekcji

Niektóre akcje mogą obejmować akcje między sobą. Odwołanie do działań w kolekcji można odwoływać się bezpośrednio poza kolekcji. Na przykład w przypadku definiowania `Http` w `scope`, następnie `@body('http')` jest nadal ważny w dowolnym miejscu w przepływie pracy. Dostępne akcje w kolekcji `runAfter` tylko z innymi działaniami w tej samej kolekcji.

## <a name="condition-if-action"></a>Warunek: Jeśli akcji

Ta akcja umożliwia ocena warunku i wykonywanie gałęzi według tego, czy wyrażenie daje w wyniku `true`. 
  
```json
"myCondition": {
    "type": "If",
    "actions": {
        "if_true": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {}
        }
    },
    "else": {
        "actions": {
            "if_false": {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://myurl"
                },
                "runAfter": {}
            }
        }
    },
    "expression": "@equals(triggerBody(), json(true))",
    "runAfter": {}
}
``` 

| Nazwa | Wymagane | Typ | Opis | 
| ---- | -------- | ---- | ----------- | 
| Akcje | Tak | Obiekt | Wewnętrzny akcje do wykonania, kiedy `expression` daje w wyniku`true` | 
| wyrażenie | Tak | Ciąg | Wyrażenie do oceny |
| else | Nie | Obiekt | Wewnętrzny akcje do wykonania, kiedy `expression` daje w wyniku`false` |
||||| 

Jeśli warunek pomyślnie, warunek zostanie oznaczona jako `Succeeded`. Akcje w jednym `actions` lub `else` obliczać obiektów: 

* `Succeeded`podczas uruchamiania i powiodło się
* `Failed`podczas uruchamiania i zakończyć się niepowodzeniem
* `Skipped`gdy odpowiednich gałęzi nie działa

Poniżej przedstawiono przykłady warunków użycia wyrażeń w akcji:
  
| Wartość JSON | wynik | 
| ---------- | -------| 
| `"expression": "@parameters('hasSpecialAction')"` | Wartość, która daje w wyniku wartość true spowoduje, że ten stan do przekazania. Obsługuje tylko wyrażeń logicznych. Aby przekonwertować inne typy Boolean, korzystania z tych funkcji: `empty` i`equals` | 
| `"expression": "@greater(actions('act1').output.value, parameters('threshold'))"` | Obsługuje funkcje porównania. Na przykład akcja działa tylko wtedy, gdy dane wyjściowe `act1` przekracza wartość progową. | 
| `"expression": "@or(greater(actions('act1').output.value, parameters('threshold')), less(actions('act1').output.value, 100))"` | Obsługuje funkcje logiki tworzenia zagnieżdżonych wyrażeń logicznych. Na przykład akcji działa, kiedy dane wyjściowe `act1` jest powyżej wartości progowej lub poniżej 100. | 
| `"expression": "@equals(length(actions('act1').outputs.errors), 0))"` | Aby sprawdzić, czy tablica zawiera wszystkie elementy, możesz użyć funkcji tablicy. Na przykład akcja jest uruchamiany podczas `errors` tablica jest pusta. | 
| `"expression": "parameters('hasSpecialAction')"` | Błąd, nie jest prawidłowym warunku, ponieważ @ jest wymagany dla warunków. |  
|||

## <a name="scope-action"></a>Akcja zakresu

Ta akcja umożliwia w logiczne grupy działań w przepływie pracy.

```json
"myScope": {
    "type": "Scope",
    "actions": {
        "call_bing": {
            "type": "Http",
             "inputs": {
                "url": "http://www.bing.com"
            }
        }
    }
}
```

| Nazwa | Wymagane | Typ | Opis | 
| ---- | -------- | ---- | ----------- |  
| Akcje | Tak | Obiekt | Wewnętrzny akcje do wykonania w zakresie |
||||| 

## <a name="foreach-action"></a>Akcja ForEach

Ta akcja pętli iteruje tablicy i wykonuje akcje wewnętrzne na każdy element tablicy. Domyślnie `foreach` pętli uruchamia się równolegle i uruchomić 20 wykonaniami równolegle w tym samym czasie. Aby ustawić zasady wykonywania, użyj `operationOptions` parametru.

```json
"forEach_EmailAction": {
    "type": "Foreach",
    "foreach": "@body('email_filter')",
    "actions": {
        "Send_email": {
            "type": "ApiConnection",
            "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                },
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
            }
        }
    },
    "runAfter": {
        "email_filter": [ "Succeeded" ]
    }
}
```

| Nazwa | Wymagane | Typ | Opis | 
| ---- | -------- | ---- | ----------- | 
| Akcje | Tak | Obiekt | Wewnętrzny akcje do wykonania w pętli | 
| Instrukcja foreach | Tak | Ciąg | Tablica do iteracji | 
| operationOptions | Nie | Ciąg | Określa opcje operacji dostosowywania zachowanie. Aktualnie obsługuje tylko `Sequential` sekwencyjnie uruchamiania iteracji domyślne zachowanie w przypadku równoległego. |
||||| 

## <a name="until-action"></a>Do działania

Ta akcja pętli wewnętrzny akcje uruchamiane do warunku wyników na wartość true.

```json
 "runUntilSucceededAction": {
    "type": "Until",
    "actions": {
        "Http": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {}
        }
    },
    "expression": "@equals(outputs('Http')['statusCode', 200)",
    "limit": {
        "count": 1000,
        "timeout": "PT1H"
    },
    "runAfter": {}
}
```

| Nazwa | Wymagane | Typ | Opis | 
| ---- | -------- | ---- | ----------- | 
| Akcje | Tak | Obiekt | Wewnętrzny akcje do wykonania w pętli | 
| wyrażenie | Tak | Ciąg | Wyrażenie do oceny po każdej iteracji | 
| Limit | Tak | Obiekt | Limity dla pętli. Należy zdefiniować co najmniej jeden limit. | 
| Liczba | Nie | Liczba całkowita | Limit liczby iteracji do wykonania | 
| timeout | Nie | Ciąg | Limit czasu w [formacie ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) Określa, jak długo uruchamiać pętli |
||||| 

## <a name="next-steps"></a>Następne kroki

* [Język definicji przepływu pracy](../logic-apps/logic-apps-workflow-definition-language.md)
* [Interfejs API REST przepływu pracy](https://docs.microsoft.com/rest/api/logic/workflows)
