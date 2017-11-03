---
title: "Zrozumienie schematu elementu webhook używane w alertach dziennika aktywności | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat schematu JSON, zamieszczony na adres URL elementu webhook, aktywuje alert dziennika aktywności."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: 75c71bcd16573d4f4dd3377c623aa9b414aa3906
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="webhooks-for-azure-activity-log-alerts"></a>Elementów Webhook alertów dziennik aktywności platformy Azure
W ramach definicji grupę można skonfigurować elementu webhook punktów końcowych, aby otrzymywać powiadomienia o alertach dziennika aktywności. Z elementów webhook można kierować te powiadomienia do innych systemów akcje przetwarzania końcowego lub niestandardowych. W tym artykule opisano, jak wygląda ładunek dla HTTP POST do elementu webhook.

Aby uzyskać więcej informacji dotyczących alertów dziennika aktywności, zobacz temat jak [tworzyć działanie Azure alerty dziennika](monitoring-activity-log-alerts.md).

Informacji dotyczących grup akcji, zobacz temat jak [tworzenie grup akcji](monitoring-action-groups.md).

## <a name="authenticate-the-webhook"></a>Uwierzytelnianie elementu webhook
Elementu webhook można używać do uwierzytelniania opartego na tokenie autoryzacji. Identyfikator URI jest na przykład zapisane przy użyciu Identyfikatora tokenu elementu webhook `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.

## <a name="payload-schema"></a>Schemat ładunku
Ładunek JSON zawarte w operacji POST różni się na podstawie w polu data.context.activityLog.eventSource ładunku.

###<a name="common"></a>Wspólne
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "channels": "Operation",
                "correlationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "eventSource": "Administrative",
                "eventTimestamp": "2017-03-29T15:43:08.0019532+00:00",
                "eventDataId": "8195a56a-85de-4663-943e-1a2bf401ad94",
                "level": "Informational",
                "operationName": "Microsoft.Insights/actionGroups/write",
                "operationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "status": "Started",
                "subStatus": "",
                "subscriptionId": "52c65f65-0518-4d37-9719-7dbbfc68c57a",
                "submissionTimestamp": "2017-03-29T15:43:20.3863637+00:00",
                ...
            }
        },
        "properties": {}
    }
}
```
###<a name="administrative"></a>Administracyjne
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "authorization": {
                    "action": "Microsoft.Insights/actionGroups/write",
                    "scope": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions"
                },
                "claims": "{...}",
                "caller": "me@contoso.com",
                "description": "",
                "httpRequest": "{...}",
                "resourceId": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions",
                "resourceGroupName": "CONTOSO-TEST",
                "resourceProviderName": "Microsoft.Insights",
                "resourceType": "Microsoft.Insights/actionGroups"
            }
        },
        "properties": {}
    }
}

```
###<a name="servicehealth"></a>ServiceHealth
```json
{
    "schemaId": "unknown",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "properties": {
                    "title": "...",
                    "service": "...",
                    "region": "...",
                    "communication": "...",
                    "incidentType": "Incident",
                    "trackingId": "...",
                    "groupId": "...",
                    "impactStartTime": "3/29/2017 3:43:21 PM",
                    "impactMitigationTime": "3/29/2017 3:43:21 PM",
                    "eventCreationTime": "3/29/2017 3:43:21 PM",
                    "impactedServices": "[{...}]",
                    "defaultLanguageTitle": "...",
                    "defaultLanguageContent": "...",
                    "stage": "Active",
                    "communicationId": "...",
                    "version": "0.1"
                }
            }
        },
        "properties": {}
    }
}
```

Szczegółowe schematu usługi kondycji powiadomień działania dziennika alertów, patrz [usługi powiadomień o kondycji](monitoring-service-notifications.md).

Dla określonego schematu informacji na temat wszystkich innych alertów dziennika aktywności, zobacz [omówienie dziennik aktywności platformy Azure](monitoring-overview-activity-logs.md).

| Nazwa elementu | Opis |
| --- | --- |
| status |Używany w przypadku alertów metryki. Zawsze ustawiony na "aktywowanego" dla alertów dotyczących działań w dzienniku. |
| Kontekst |Kontekst zdarzenia. |
| resourceProviderName |Dostawca zasobów ryzyko zasobów. |
| conditionType |Zawsze "Event". |
| name |Nazwa reguły alertów. |
| id |Identyfikator zasobu alertu. |
| description |Opis alertu ustawić podczas tworzenia alertu. |
| subscriptionId |Identyfikator subskrypcji platformy Azure. |
| sygnatura czasowa |Czas, w którym to zdarzenie zostało wygenerowane przez usługę Azure, który przetwarzał żądanie. |
| resourceId |Identyfikator zasobu ryzyko zasobu. |
| Grupy zasobów o nazwie |Nazwa grupy zasobów dla zasobu ryzyko. |
| properties |Zestaw `<Key, Value>` par (czyli `Dictionary<String, String>`) zawierającą szczegółowe informacje o zdarzeniu. |
| Zdarzenia |Element zawierający metadane dotyczące zdarzenia. |
| Autoryzacji |Właściwości kontroli dostępu opartej na rolach zdarzenia. Te właściwości obejmują zazwyczaj akcji, roli i zakresu. |
| category |Kategoria zdarzenia. Obsługiwane wartości to administracyjne, Alert zabezpieczeń, ServiceHealth i zalecenia. |
| obiekt wywołujący |Adres e-mail użytkownika, który wykonał operację, oświadczenia UPN lub nazwy SPN oświadczenia na podstawie dostępności. Może mieć wartości null dla niektórych wywołań systemowych. |
| correlationId |Zazwyczaj identyfikator GUID w postaci ciągu. Zdarzenia z correlationId należą do tego samego działania większych i zazwyczaj udziału correlationId. |
| eventDescription |Opis zdarzenia tekst statyczny. |
| eventDataId |Unikatowy identyfikator zdarzenia. |
| Źródła zdarzeń |Nazwa usługi Azure lub infrastruktury, który wygenerował zdarzenie. |
| httpRequest |Żądanie obejmuje zazwyczaj clientRequestId, clientIpAddress i metodę HTTP (na przykład umieścić). |
| poziom |Jedną z następujących wartości: krytyczny, błąd, ostrzeżenie, informacyjny i pełne. |
| operationId |Zazwyczaj identyfikator GUID współdzielenia zdarzenia odpowiadające jednej operacji. |
| operationName |Nazwa operacji. |
| properties |Właściwości zdarzenia. |
| status |Ciąg. Stan operacji. Typowe wartości to rozpoczęte, w toku, zakończyło się pomyślnie, nie powiodło się, aktywnych i rozwiązanych. |
| Podstan |Zwykle zawiera kod stanu HTTP odpowiedniego wywołania REST. Może również zawierać innych ciągów, które opisują podstanu. Typowe wartości podstanu obejmują OK (kod stanu HTTP: 200), utworzony (kod stanu HTTP: 201), akceptowane (kod stanu HTTP: 202), nie zawartości (kod stanu HTTP: 204), nieprawidłowe żądanie (kod stanu HTTP: 400), nie znaleziono (kod stanu HTTP: 404), konflikt (kod stanu HTTP: 409), wewnętrzny błąd serwera (kod stanu HTTP: 500), Usługa niedostępna (kod stanu HTTP: 503) i limit czasu bramy (kod stanu HTTP : 504). |

## <a name="next-steps"></a>Następne kroki
* [Dowiedz się więcej o dziennika aktywności](monitoring-overview-activity-logs.md).
* [Wykonywanie skryptów automatyzacji Azure (elementów Runbook) Azure alerty](http://go.microsoft.com/fwlink/?LinkId=627081).
* [Wyślij wiadomość SMS za pośrednictwem usługi Twilio z poziomu alertu Azure przy użyciu aplikacji logiki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). W tym przykładzie jest metryki alertów, ale można go modyfikować do pracy z alert dziennika aktywności.
* [Użyj aplikacji logiki, aby wysłać wiadomość Slack z poziomu alertu Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). W tym przykładzie jest metryki alertów, ale można go modyfikować do pracy z alert dziennika aktywności.
* [Użyj aplikacji logiki, aby wysłać komunikat do kolejki systemu Azure z poziomu alertu Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). W tym przykładzie jest metryki alertów, ale można go modyfikować do pracy z alert dziennika aktywności.
