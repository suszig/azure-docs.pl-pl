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
ms.openlocfilehash: 341ab32ad0ec691285fbf1537ee298ab30156a5d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
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

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

| Nazwa elementu | Opis |
| --- | --- |
| status |Używany w przypadku alertów metryki. Zawsze ustawiony na "aktywowany" alertów dziennik aktywności. |
| Kontekst |Kontekst zdarzenia. |
| resourceProviderName |Dostawca zasobów ryzyko zasobów. |
| conditionType |Zawsze "Event". |
| name |Nazwa reguły alertów. |
| id |Identyfikator zasobu alertu. |
| description |Opis alertu jako zestaw podczas tworzenia alertu. |
| subscriptionId |Identyfikator subskrypcji platformy Azure. |
| sygnatura czasowa |Czas, w którym to zdarzenie zostało wygenerowane przez usługę Azure, który przetwarzał żądanie. |
| resourceId |Identyfikator zasobu ryzyko zasobu. |
| Grupy zasobów o nazwie |Nazwa grupy zasobów dla wpływ na zasób |
| properties |Zestaw `<Key, Value>` par (tj. `Dictionary<String, String>`) zawierającą szczegółowe informacje o zdarzeniu. |
| Zdarzenia |Element zawierający metadane dotyczące zdarzenia. |
| Autoryzacji |Właściwości RBAC zdarzenia. Obejmują one zazwyczaj "Akcja" i "rola" 'scope'. |
| category |Kategoria zdarzenia. Obsługiwane wartości to: administracyjne, alertów zabezpieczeń, ServiceHealth, zalecenie. |
| obiekt wywołujący |Adres e-mail użytkownika, który wykonał operację, oświadczenia UPN lub nazwy SPN oświadczenia na podstawie dostępności. Może mieć wartości null dla niektórych wywołań systemowych. |
| correlationId |Zazwyczaj identyfikator GUID w postaci ciągu. Zdarzenia z correlationId należą do tego samego działania większych i zazwyczaj udziału correlationId. |
| eventDescription |Opis zdarzenia tekst statyczny. |
| eventDataId |Unikatowy identyfikator zdarzenia. |
| Źródła zdarzeń |Nazwa usługi Azure lub infrastruktury, który wygenerował zdarzenie. |
| httpRequest |Obejmuje zazwyczaj "clientRequestId", "clientIpAddress" i "method" (np. umieścić metoda HTTP). |
| poziom |Jedną z następujących wartości: "Krytyczne", "Błąd", "Ostrzeżenie", "Informacyjny" i "Pełne." |
| operationId |Zazwyczaj identyfikator GUID współdzielenia zdarzenia odpowiadające jednej operacji. |
| operationName |Nazwa operacji. |
| properties |Właściwości zdarzenia. |
| status |Ciąg. Stan operacji. Typowe wartości to: "Uruchomiona", "W toku", "Powiodło się", "Nie powiodło się", "Active", "Rozwiązany". |
| Podstan |Zwykle zawiera kod stanu HTTP odpowiedniego wywołania REST. Może również zawierać inne parametry opisujące podstanu. Obejmują wartości typowych podstanu: OK (kod stanu HTTP: 200), utworzony (kod stanu HTTP: 201), akceptowane (kod stanu HTTP: 202), nie zawartości (kod stanu HTTP: 204), nieprawidłowe żądanie (kod stanu HTTP: 400), nie znaleziono (kod stanu HTTP: 404), konflikt (kod stanu HTTP: 409), wewnętrzny błąd serwera (kod stanu HTTP: 500), Usługa niedostępna (kod stanu HTTP: 503), limit czasu bramy (kod stanu HTTP: 504) |

## <a name="next-steps"></a>Następne kroki
* [Dowiedz się więcej o dziennika aktywności](monitoring-overview-activity-logs.md)
* [Wykonywanie skryptów automatyzacji Azure (elementów Runbook) Azure alertów](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Wyślij wiadomość SMS za pośrednictwem usługi Twilio z poziomu alertu Azure przy użyciu aplikacji logiki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). W tym przykładzie jest metryki alertów, ale może zostać zmodyfikowany do pracy z alert dziennik aktywności.
* [Wyślij wiadomość Slack z Azure alert przy użyciu aplikacji logiki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). W tym przykładzie jest metryki alertów, ale może zostać zmodyfikowany do pracy z alert dziennik aktywności.
* [Wyślij wiadomość do kolejki Azure z poziomu alertu Azure przy użyciu aplikacji logiki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). W tym przykładzie jest metryki alertów, ale może zostać zmodyfikowany do pracy z alert dziennik aktywności.
