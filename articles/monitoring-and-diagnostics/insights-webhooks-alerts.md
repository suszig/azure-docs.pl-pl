---
title: "Konfigurowanie elementów webhook Azure alerty metryki | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak przekierować Azure alerty do innych, innych niż Azure systemów."
author: johnkemnetz
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 8b3ae540-1d19-4f3d-a635-376042f8a5bb
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: johnkem
ms.openlocfilehash: 049803e7701c68559103d9b1fa5dfacf820d0548
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2018
---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Konfigurowanie elementu webhook na alert metryki Azure
Elementów webhook służy do kierowania powiadomień o alertach Azure z innymi systemami przetwarzania końcowego lub niestandardowych działań. Elementu webhook na alert służy do kierowania go do usługi, które wysyłanie wiadomości SMS do dziennika błędów do powiadamiania zespołu za pomocą rozmowy lub usług obsługi wiadomości, lub dla różnych innych działań. 

W tym artykule opisano sposób ustawiania elementu webhook na alert metryki platformy Azure. Przedstawia on także wygląda ładunek dla HTTP POST do elementu webhook. Informacje o instalacji i schematu dla działania usługi Azure alert dziennika (alert zdarzeń), zobacz [wywołać elementu webhook dla alertu dziennik aktywności platformy Azure](insights-auditlog-to-webhook-email.md).

Alerty Azure używają HTTP POST do wysyłania alertów zawartość w formacie JSON do elementu webhook identyfikator URI, który należy podać podczas tworzenia alertu. Schemat jest zdefiniowany w dalszej części tego artykułu. Identyfikator URI musi być prawidłowy punkt końcowy HTTP lub HTTPS. Azure wpisów jednego wpisu na żądanie, gdy alert jest aktywny.

## <a name="configure-webhooks-via-the-azure-portal"></a>Konfigurowanie elementów webhook za pośrednictwem portalu Azure
Aby dodać lub zaktualizować identyfikator URI elementu webhook w [portalu Azure](https://portal.azure.com/), przejdź do **alerty Create/Update**.

![Dodawanie okienka reguły alertu](./media/insights-webhooks-alerts/Alertwebhook.png)

Istnieje również możliwość skonfigurowania alertu do wysłania do elementu webhook identyfikator URI przy użyciu [poleceń cmdlet programu Azure PowerShell](insights-powershell-samples.md#create-metric-alerts), [interfejsu wiersza polecenia i platform](insights-cli-samples.md#work-with-alerts), lub [interfejsów API REST Monitor Azure](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticate-the-webhook"></a>Uwierzytelnianie elementu webhook
Elementu webhook można uwierzytelniać za pomocą opartego na tokenie autoryzacji. Identyfikator URI elementu webhook jest zapisywany przy użyciu identyfikatora tokenu. Na przykład: `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Schemat ładunku
Operację POST zawiera następujące ładunek JSON i schematu dla wszystkich alertów na podstawie Metryka:

```JSON
{
    "WebhookName": "Alert1515515157799",
    "RequestBody": {
        "status": "Activated",
        "context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                "metricName": "Requests",
                "metricUnit": "Count",
                "metricValue": "10",
                "threshold": "10",
                "windowSize": "15",
                "timeAggregation": "Average",
                "operator": "GreaterThanOrEqual"
            },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
        },
        "properties": {
            "key1": "value1",
            "key2": "value2"
        }
    }
}
```


| Pole | Obowiązkowy | Ustalony zbiór wartości | Uwagi |
|:--- |:--- |:--- |:--- |
| status |Tak |Aktywna, rozwiązane |Stan alertu na podstawie wybranych warunków ustawieniu. |
| Kontekst |Tak | |Kontekst alertu. |
| sygnatura czasowa |Tak | |Czas, w którym alert został uruchomiony. |
| id |Tak | |Każda reguła alertów ma unikatowy identyfikator. |
| name |Tak | |Nazwa alertu. |
| description |Tak | |Opis alertu. |
| conditionType |Tak |Metryki i zdarzenia |Obsługiwane są dwa typy alertów: metryk i zdarzeń. Metryki alerty są oparte na stanie metryki. Alerty zdarzeń są oparte na zdarzenie w dzienniku aktywności. Użyj tej wartości, aby sprawdzić, czy alert jest oparta na metrykę lub zdarzenia. |
| warunek |Tak | |Na podstawie określonych pól do sprawdzenia **conditionType** wartości. |
| metricName |Metryki alertów | |Nazwa metryki, który definiuje reguły monitoruje. |
| metricUnit |Metryki alertów |Bajty, BytesPerSecond, Count, CountPerSecond, wartość procentowa, w sekundach |Jednostka może metryki. Zobacz [dozwolone wartości](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Metryki alertów | |Rzeczywista wartość metryki, który spowodował alert. |
| Próg |Metryki alertów | |Wartość progowa, w którym włączono alert. |
| windowSize |Metryki alertów | |Czas, który jest używany do monitorowania alertów działania oparte na wartość progową. Wartość musi należeć do zakresu od 5 minut do 1 dnia. Wartość musi być w formacie czasu trwania ISO 8601. |
| timeAggregation |Metryki alertów |Średnia, Last, maksimum, Minimum, None, łączna liczba |Jak można łączyć dane, które są zbierane wraz z upływem czasu. Wartość domyślna to średnia. Zobacz [dozwolone wartości](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| Operator |Metryki alertów | |Operator, który służy do porównywania bieżące dane metryk do ustalonego progu. |
| subscriptionId |Tak | |Identyfikator subskrypcji platformy Azure. |
| resourceGroupName |Tak | |Nazwa grupy zasobów dla zasobu. |
| resourceName |Tak | |Nazwa zasobu zasobu. |
| resourceType |Tak | |Typ zasobu zasobu. |
| resourceId |Tak | |Identyfikator zasobu zasobu. |
| resourceRegion |Tak | |Obszar lub lokalizacja zasobu. |
| portalLink |Tak | |Bezpośrednie łącze na stronie Podsumowanie zasobów portalu. |
| properties |N |Optional (Opcjonalność) |Zestaw pary klucz wartość zawiera informacje o zdarzeniu. Na przykład `Dictionary<String, String>`. Pole właściwości jest opcjonalne. Niestandardowy interfejs użytkownika lub przepływ pracy oparty na aplikację logiki użytkownicy mogą wprowadzać pary klucz wartość, które mogą zostać przekazane za pośrednictwem ładunku. Alternatywny sposób, aby przekazywać właściwości niestandardowych elementu webhook jest za pośrednictwem elementu webhook identyfikatora URI się (jako parametry kwerendy). |

> [!NOTE]
> Można ustawić **właściwości** pole tylko przy użyciu [interfejsów API REST Monitor Azure](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Kolejne kroki
* Dowiedz się więcej o alertach Azure i elementów webhook wideo [integracji Azure alerty PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080).
* Dowiedz się, jak [wykonywanie skryptów automatyzacji Azure (elementów runbook) Azure alerty](http://go.microsoft.com/fwlink/?LinkId=627081).
* Dowiedz się, jak [użycie aplikacji logiki do wysyłania wiadomości SMS za pośrednictwem usługi Twilio z poziomu alertu Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).
* Dowiedz się, jak [użyj aplikacji logiki, aby wysłać wiadomość Slack z poziomu alertu Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).
* Dowiedz się, jak [użyj aplikacji logiki, aby wysłać komunikat do kolejki systemu Azure z poziomu alertu Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).
