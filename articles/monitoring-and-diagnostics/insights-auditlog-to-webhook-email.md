---
title: "Wywołanie elementu webhook alerty dziennika aktywności platformy Azure | Dokumentacja firmy Microsoft"
description: "Zdarzenia dziennika aktywności trasy do innych usług dla akcji niestandardowej. Na przykład wysyłanie SMS dziennika błędów lub powiadomić zespołu za pomocą rozmowy/wiadomości usługi."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 64d333d1-7f37-4a00-9d16-dda6e69a113b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: johnkem
ms.openlocfilehash: 08467aed4e1601b32598fc42515d9c38b601a9d4
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a>Wywołanie elementu webhook alerty dziennika aktywności platformy Azure
Elementów Webhook pozwalają kierować Azure powiadomień o alertach do innych systemów przetwarzania końcowego lub niestandardowych działań. Elementu webhook na alert służy do kierowania go do usługi, które wysyłanie SMS, dziennika błędów, powiadamiania zespołu za pomocą usług wiadomości rozmów/lub czy dowolna liczba innych działań. W tym artykule opisano sposób ustawiania elementu webhook ma być wywoływana po uruchamiany alertu dziennika aktywności platformy Azure. Pokazuje też, jak wygląda ładunek dla HTTP POST do elementu webhook. Informacje dotyczące instalacji i schematu dla alertu metryki Azure [wyświetlona następująca strona](insights-webhooks-alerts.md). Dziennik aktywności alertu można również skonfigurować do wysyłania wiadomości e-mail, gdy aktywowany.

> [!NOTE]
> Ta funkcja jest obecnie w wersji zapoznawczej i zostanie usunięta w pewnym momencie w przyszłości.
>
>

Możesz skonfigurować dziennik aktywności alertów za pomocą [polecenia cmdlet programu PowerShell usługi Azure](insights-powershell-samples.md#create-metric-alerts), [interfejsu wiersza polecenia i Platform](insights-cli-samples.md#work-with-alerts), lub [interfejsu API REST Monitor Azure](https://msdn.microsoft.com/library/azure/dn933805.aspx). Obecnie nie można ustawić jedną przy użyciu portalu Azure.

## <a name="authenticating-the-webhook"></a>Uwierzytelnianie elementu webhook
Element webhook uwierzytelniania z użyciem jednej z tych metod:

1. **Autoryzacji opartej na tokenach** — identyfikator URI jest na przykład zapisane przy użyciu Identyfikatora tokenu elementu webhook`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2. **Podstawowe autoryzacji** — identyfikator URI jest zapisane przy użyciu nazwy użytkownika i hasła, na przykład elementu webhook`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Schemat ładunku
Operację POST zawiera następujące ładunek JSON i schematu dla wszystkich alertów na podstawie dziennik aktywności. Ten schemat jest podobny do tego używanego przez alerty na podstawie metryki.

```json
{
    "WebhookName": "Alert1515526229589",
    "RequestBody": {
        "schemaId": "Microsoft.Insights/activityLogs",
        "data": {
            "status": "Activated",
            "context": {
                "activityLog": {
                    "authorization": {
                        "action": "Microsoft.Compute/virtualMachines/deallocate/action",
                        "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1"
                    },
                    "channels": "Operation",
                    "claims": {
                        "aud": "https://management.core.windows.net/",
                        "iss": "https://sts.windows.net/00000000-0000-0000-0000-000000000000/",
                        "iat": "1234567890",
                        "nbf": "1234567890",
                        "exp": "1234567890",
                        "aio": "Y2NgYBD8ZLlhu27JU6WZsXemMIvVAAA=",
                        "appid": "00000000-0000-0000-0000-000000000000",
                        "appidacr": "2",
                        "e_exp": "262800",
                        "http://schemas.microsoft.com/identity/claims/identityprovider": "https://sts.windows.net/00000000-0000-0000-0000-000000000000/",
                        "http://schemas.microsoft.com/identity/claims/objectidentifier": "00000000-0000-0000-0000-000000000000",
                        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "00000000-0000-0000-0000-000000000000",
                        "http://schemas.microsoft.com/identity/claims/tenantid": "00000000-0000-0000-0000-000000000000",
                        "uti": "XnCk46TrDkOQXwo49Y8fAA",
                        "ver": "1.0"
                    },
                    "caller": "00000000-0000-0000-0000-000000000000",
                    "correlationId": "00000000-0000-0000-0000-000000000000",
                    "description": "",
                    "eventSource": "Administrative",
                    "eventTimestamp": "2018-01-09T20:11:25.8410967+00:00",
                    "eventDataId": "00000000-0000-0000-0000-000000000000",
                    "level": "Informational",
                    "operationName": "Microsoft.Compute/virtualMachines/deallocate/action",
                    "operationId": "00000000-0000-0000-0000-000000000000",
                    "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
                    "resourceGroupName": "ContosoVM",
                    "resourceProviderName": "Microsoft.Compute",
                    "status": "Succeeded",
                    "subStatus": "",
                    "subscriptionId": "00000000-0000-0000-0000-000000000000",
                    "submissionTimestamp": "2018-01-09T20:11:40.2986126+00:00",
                    "resourceType": "Microsoft.Compute/virtualMachines"
                }
            },
            "properties": {}
        }
    },
    "RequestHeader": {
        "Expect": "100-continue",
        "Host": "s1events.azure-automation.net",
        "User-Agent": "IcMBroadcaster/1.0",
        "X-CorrelationContext": "RkkKACgAAAACAAAAEADlBbM7x86VTrHdQ2JlmlxoAQAQALwazYvJ/INPskb8S5QzgDk=",
        "x-ms-request-id": "00000000-0000-0000-0000-000000000000"
    }
}
```

| Nazwa elementu | Opis |
| --- | --- |
| status |Używany w przypadku alertów metryki. Zawsze ustawiony na "aktywowany" alertów dziennik aktywności. |
| Kontekst |Kontekst zdarzenia. |
| Dziennik aktywności | Właściwości dziennika zdarzeń.|
| Autoryzacji |Właściwości RBAC zdarzenia. Obejmują one zazwyczaj "Akcja" i "rola" 'scope'. |
| action | Akcja przechwycone przez alert. |
| Zakres | Zakres alertu (tj. zasób).|
| kanały | Operacja |
| Oświadczenia | Kolekcja informacji znajduje się na odnosi się do oświadczenia. |
| element wywołujący |Identyfikator GUID lub nazwa użytkownika, który wykonał operację, oświadczenia UPN lub nazwy SPN oświadczenia na podstawie dostępności. Może mieć wartości null dla niektórych wywołań systemowych. |
| correlationId |Zazwyczaj identyfikator GUID w postaci ciągu. Zdarzenia z correlationId należą do tego samego działania większych i zazwyczaj udziału correlationId. |
| description |Opis alertu jako zestaw podczas tworzenia alertu. |
| eventSource |Nazwa usługi Azure lub infrastruktury, który wygenerował zdarzenie. |
| eventTimestamp |Godzina wystąpienia zdarzenia. |
| eventDataId |Unikatowy identyfikator zdarzenia. |
| poziom |Jedną z następujących wartości: "Krytyczne", "Błąd", "Ostrzeżenie", "Informacyjny" i "Pełne." |
| operationName |Nazwa operacji. |
| operationId |Zazwyczaj identyfikator GUID współdzielenia zdarzenia odpowiadające jednej operacji. |
| resourceId |Identyfikator zasobu ryzyko zasobu. |
| resourceGroupName |Nazwa grupy zasobów dla wpływ na zasób |
| resourceProviderName |Dostawca zasobów ryzyko zasobów. |
| status |Ciąg. Stan operacji. Typowe wartości to: "Uruchomiona", "W toku", "Powiodło się", "Nie powiodło się", "Active", "Rozwiązany". |
| subStatus |Zwykle zawiera kod stanu HTTP odpowiedniego wywołania REST. Może również zawierać inne parametry opisujące podstanu. Obejmują wartości typowych podstanu: OK (kod stanu HTTP: 200), utworzony (kod stanu HTTP: 201), akceptowane (kod stanu HTTP: 202), nie zawartości (kod stanu HTTP: 204), nieprawidłowe żądanie (kod stanu HTTP: 400), nie znaleziono (kod stanu HTTP: 404), konflikt (kod stanu HTTP: 409), wewnętrzny błąd serwera (kod stanu HTTP: 500), Usługa niedostępna (kod stanu HTTP: 503), limit czasu bramy (kod stanu HTTP: 504) |
| subscriptionId |Identyfikator subskrypcji platformy Azure. |
| submissionTimestamp |Czas, w którym to zdarzenie zostało wygenerowane przez usługę Azure, który przetwarzał żądanie. |
| resourceType | Typ zasobu, który wygenerował zdarzenie.|
| properties |Zestaw `<Key, Value>` par (tj. `Dictionary<String, String>`) zawierającą szczegółowe informacje o zdarzeniu. |

## <a name="next-steps"></a>Kolejne kroki
* [Dowiedz się więcej o dziennika aktywności](monitoring-overview-activity-logs.md)
* [Wykonywanie skryptów automatyzacji Azure (elementów Runbook) Azure alertów](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Wyślij wiadomość SMS za pośrednictwem usługi Twilio z poziomu alertu Azure przy użyciu aplikacji logiki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). W tym przykładzie jest metryki alertów, ale może zostać zmodyfikowany do pracy z alert dziennik aktywności.
* [Wyślij wiadomość Slack z Azure alert przy użyciu aplikacji logiki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). W tym przykładzie jest metryki alertów, ale może zostać zmodyfikowany do pracy z alert dziennik aktywności.
* [Wyślij wiadomość do kolejki Azure z poziomu alertu Azure przy użyciu aplikacji logiki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). W tym przykładzie jest metryki alertów, ale może zostać zmodyfikowany do pracy z alert dziennik aktywności.
