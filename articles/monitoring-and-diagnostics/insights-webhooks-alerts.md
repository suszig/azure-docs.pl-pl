---
title: "Konfigurowanie elementów webhook Azure alerty metryki | Dokumentacja firmy Microsoft"
description: "Przekieruj Azure alerty do innych systemów innych niż Azure."
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
ms.openlocfilehash: 1a885166e5c71f13da222bfc22b0fc579096c52f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Konfigurowanie elementu webhook na alert metryki Azure
Elementów Webhook pozwalają kierować Azure powiadomień o alertach do innych systemów przetwarzania końcowego lub niestandardowych działań. Elementu webhook na alert służy do kierowania go do usługi, które wysyłanie SMS, dziennika błędów, powiadamiania zespołu za pomocą usług wiadomości rozmów/lub czy dowolna liczba innych działań. W tym artykule opisano, jak ustawić elementu webhook w Azure alertu metryki i wygląda ładunek dla HTTP POST do elementu webhook. Informacje dotyczące instalacji i schematu dla alertu dziennika aktywności platformy Azure (alert zdarzeń) [wyświetlona następująca strona](insights-auditlog-to-webhook-email.md).

Format alertu zawartość w formacie JSON Azure alerty HTTP POST schematu określonych poniżej, aby webhook identyfikator URI, który należy podać podczas tworzenia alertu. Ten identyfikator URI musi być prawidłowy punkt końcowy HTTP lub HTTPS. Azure wpisów jednego wpisu na żądanie, gdy alert jest aktywny.

## <a name="configuring-webhooks-via-the-portal"></a>Konfigurowanie elementów webhook za pośrednictwem portalu
Można dodać lub zaktualizować identyfikatora URI elementu webhook na ekranie alerty Create/Update w [portal](https://portal.azure.com/).

![Dodaj regułę alertu](./media/insights-webhooks-alerts/Alertwebhook.png)

Istnieje również możliwość skonfigurowania alertu do wysłania do identyfikatora URI elementu webhook przy użyciu [poleceń cmdlet programu PowerShell Azure](insights-powershell-samples.md#create-metric-alerts), [interfejsu wiersza polecenia i Platform](insights-cli-samples.md#work-with-alerts), lub [interfejsu API REST Monitor Azure](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-the-webhook"></a>Uwierzytelnianie elementu webhook
Element webhook uwierzytelniania z użyciem autoryzacji opartej na tokenach. Zapisać elementu webhook identyfikatora URI jest identyfikatorem tokenu, np. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Schemat ładunku
Operację POST zawiera następujące ładunek JSON i schematu dla wszystkich alertów na podstawie metryki.

```JSON
{
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
```


| Pole | Obowiązkowy | Ustalony zbiór wartości | Uwagi |
|:--- |:--- |:--- |:--- |
| status |Tak |"Aktywna", "Rozwiązane" |Stan alertu na podstawie warunków została ustawiona. |
| Kontekst |Tak | |Kontekst alertu. |
| sygnatura czasowa |Tak | |Czas, w którym alert został uruchomiony. |
| id |Tak | |Każda reguła alertów ma unikatowy identyfikator. |
| name |Tak | |Nazwa alertu. |
| Opis elementu |Tak | |Opis alertu. |
| conditionType |Tak |"Metryki", "Event" |Obsługiwane są dwa typy alertów. Jeden na podstawie warunku metryki i inne, na podstawie zdarzenia w dzienniku aktywności. Użyj tej wartości, aby sprawdzić, czy alert jest oparty na Metryka lub zdarzeń. |
| Warunek |Tak | |Na conditionType na podstawie określonych pól do sprawdzenia. |
| metricName |Metryki alertów | |Nazwa metryki, który definiuje reguły monitoruje. |
| metricUnit |Metryki alertów |"B", "BytesPerSecond", "Count", "CountPerSecond", "%", "S" |Jednostka może metryki. [Dozwolone wartości są wyświetlane tutaj](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Metryki alertów | |Rzeczywista wartość metryki, który spowodował alert. |
| Próg |Metryki alertów | |Wartość progowa, w którym włączono alert. |
| Rozmiar_okna |Metryki alertów | |Okres czasu, który służy do monitorowania alertów działania oparte na wartość progową. Musi należeć do zakresu od 5 minut do 1 dnia. Format czasu trwania ISO 8601. |
| timeAggregation |Metryki alertów |"Średnia", "Last", "Maksimum", "Minimalna", "None", "całkowita" |Jak można łączyć dane, które są zbierane wraz z upływem czasu. Wartość domyślna to średnia. [Dozwolone wartości są wyświetlane tutaj](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| Operator |Metryki alertów | |Operator użyty do porównania bieżące dane metryk do ustalonego progu. |
| subscriptionId |Tak | |Identyfikator subskrypcji platformy Azure. |
| Grupy zasobów o nazwie |Tak | |Nazwa grupy zasobów dla zasobu ryzyko. |
| resourceName |Tak | |Nazwa zasobu ryzyko zasobu. |
| Typ zasobu |Tak | |Typ zasobu ryzyko zasobu. |
| resourceId |Tak | |Identyfikator zasobu ryzyko zasobu. |
| resourceRegion |Tak | |Region lub lokalizacji zasobów objęte wpływem. |
| portalLink |Tak | |Bezpośredniego łącza do strony podsumowania portalu zasobów. |
| properties |N |Optional (Opcjonalność) |Zestaw `<Key, Value>` par (tj. `Dictionary<String, String>`) zawierającą szczegółowe informacje o zdarzeniu. Pole właściwości jest opcjonalne. W przypadku niestandardowego interfejsu użytkownika lub logiki aplikacji na podstawie użytkownicy mogą wprowadzać klucza/wartości, które mogą zostać przekazane za pośrednictwem ładunku. Inny sposób, aby przekazywać właściwości niestandardowych elementu webhook jest za pośrednictwem uri elementu webhook (jako parametry kwerendy) |

> [!NOTE]
> W polu właściwości można ustawić tylko za pomocą [interfejsu API REST Monitor Azure](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o alertach Azure i elementów webhook wideo [integracji Azure alerty PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
* [Wykonywanie skryptów automatyzacji Azure (elementów Runbook) Azure alertów](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Wyślij wiadomość SMS za pośrednictwem usługi Twilio z poziomu alertu Azure przy użyciu aplikacji logiki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
* [Wyślij wiadomość Slack z Azure alert przy użyciu aplikacji logiki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
* [Wyślij wiadomość do kolejki Azure z poziomu alertu Azure przy użyciu aplikacji logiki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
