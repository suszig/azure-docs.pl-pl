---
title: "Akcje elementu Webhook dla dziennika alertów w alertach Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób dziennika regułę alertu za pomocą dziennika insights analityka lub aplikacji, przeprowadzi wypychanie danych jako elementu webhook HTTP i szczegóły różne dostosowania możliwe."
author: msvijayn
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 49905638-f9f2-427b-8489-a0bcc7d8b9fe
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2018
ms.author: vinagara
ms.openlocfilehash: 4af1bb61888810011ce64fde7931cabfefe76ab6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="webhook-actions-for-log-alert-rules"></a>Akcje elementu Webhook dla dziennika reguły alertów
Gdy [alert jest tworzony na platformie Azure (wersja zapoznawcza)](monitor-alerts-unified-usage.md), masz możliwość [konfigurowania za pomocą grup akcji](monitoring-action-groups.md) do wykonywania co najmniej jednej akcji.  W tym artykule opisano webhook różne akcje, które są dostępne i szczegółowe informacje na temat konfigurowania niestandardowego elementu webhook opartych na formacie JSON.


## <a name="webhook-actions"></a>Akcje elementu Webhook

Akcje elementu Webhook umożliwiają wywołanie procesu zewnętrznego przez pojedyncze żądanie HTTP POST.  Usługa wywoływana powinien obsługuje elementów webhook i określić, jak używać żadnych ładunku odbiera.  Można także wywołać interfejsu API REST, które w szczególności nie obsługuje elementów webhook, jak długo żądanie znajduje się w formacie, który obsługuje usługę interfejsu API.  Przykłady użycia elementu webhook w odpowiedzi na alert wysyłania wiadomości [Slack](http://slack.com) lub Tworzenie zdarzenia w [PagerDuty](http://pagerduty.com/).  Pełny Przewodnik tworzenia reguły alertu z elementu webhook do wywołania zapas czasu jest dostępna na [elementów Webhook w alertach analizy dzienników](../log-analytics/log-analytics-alerts-webhooks.md).

Akcje elementu Webhook wymagają właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--- |:--- |
| Adres URL elementu Webhook |Adres URL elementu webhook. |
| Niestandardowy ładunek JSON |Niestandardowy ładunek do wysłania z elementu webhook.  Jeśli wybrano opcję podczas tworzenia alertu. Szczegóły dostępne pod adresem [Zarządzanie alertami korzystanie z alertów Azure (wersja zapoznawcza)](monitor-alerts-unified-usage.md) |

> [!NOTE]
> Testowanie przycisk elementu Webhook obok *Uwzględnij niestandardowy ładunek JSON dla elementu webhook* opcja dla dziennika alertu, wyzwoli fikcyjny wywołanie test adresu URL elementu webhook. Nie zawiera danych rzeczywistych lub przedstawicielem używany w przypadku alertów dziennika schematu JSON. Podczas gdy można przetestować żadnych jeden element webhook o przedstawiciel niestandardowych JSON, wszystkich elementów webhook skonfigurowany w grupie akcji będą wysyłane z niestandardowy ładunek JSON.

Elementów Webhook obejmują adres URL i zapisany w formacie JSON, które to dane wysyłane do zewnętrznych usługi ładunku.  Domyślnie ładunek zawiera wartości w tabeli poniżej.  Można zastąpić to ładunku niestandardowego własny.  W takim przypadku służy zmiennych w tabeli dla każdego z parametrów do uwzględnienia w niestandardowy ładunek ich wartości.


| Parametr | Zmienna | Opis |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Nazwa reguły alertów. |
| AlertThresholdOperator |#thresholdoperator |Operator próg dla reguł alertów.  *Większa niż* lub *mniej niż*. |
| AlertThresholdValue |#thresholdvalue |Wartość progowa reguły alertów. |
| LinkToSearchResults |#linktosearchresults |Link do wyszukiwania dziennika analizy dzienników, która zwraca odpowiednie rekordy z kwerendy utworzony alert. |
| ResultCount |#searchresultcount |Liczba rekordów w wynikach wyszukiwania. |
| SearchIntervalEndtimeUtc |#searchintervalendtimeutc |Godzina zakończenia dla zapytania w formacie UTC. |
| SearchIntervalInSeconds |#searchinterval |Przedział czasu dla reguły alertów. |
| SearchIntervalStartTimeUtc |#searchintervalstarttimeutc |Godzina rozpoczęcia dla zapytania w formacie UTC. |
| SearchQuery |#searchquery |Dziennik wyszukiwania używane przez reguły alertów. |
| Wynikówwyszukiwania |"IncludeSearchResults":true|Rekordów zwróconych przez zapytanie jako tabelę JSON, ograniczona do pierwszych 1000 rekordów/wierszy; Jeśli "IncludeSearchResults": true został dodany w niestandardowych definicji elementu webhook JSON jako właściwość najwyższego poziomu |
| WorkspaceID |#workspaceid |Identyfikator obszaru roboczego analizy dzienników. |
| Ważność |#severity |Ważność, ustaw dla alertu wypalane dziennika. |

Można na przykład określić następujące niestandardowy ładunek, który zawiera jeden parametr o nazwie *tekstu*.  Usługa, która wywołuje ten element webhook czy oczekiwano tego parametru.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Ten przykładowy ładunek może rozpoznać przypominać następujące przy wysyłaniu do elementu webhook.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Aby dołączyć wyniki wyszukiwania niestandardowy ładunek, upewnij się, że **IncudeSearchResults** jest ustawiony jako właściwość najwyższego poziomu w ładunku json.


Można przeprowadzić za pomocą pełny przykład tworzenie reguły alertu z elementu webhook można uruchomić usługi zewnętrzne w [utworzenie alertu elementu webhook w analizy dzienników do wysyłania wiadomości zapas czasu](../log-analytics/log-analytics-alerts-webhooks.md).

## <a name="sample-payload"></a>Przykładowy ładunek
W tej sekcji przedstawiono przykładowy ładunek dla elementu webhook dla dziennika alertów, włączając podczas ładunku jest standardowe i niestandardowe jej.

> [!NOTE]
> Aby zapewnić zgodność z poprzednimi wersjami, jest taka sama jak ładunek standardowego elementu webhook dla alertów za pomocą usługi Analiza dzienników Azure [OMS alertów zarządzania](../log-analytics/log-analytics-solution-alert-management.md). Nawet w przypadku alertów dziennika za pomocą [usługi Application Insights](../application-insights/app-insights-analytics.md), ładunku standardowego elementu webhook jest oparta na schemat grupy akcji

### <a name="standard-webhook-for-log-alerts"></a>Standardowa elementu Webhook dla dziennika alertów
Oba te przykłady, że uważają fikcyjny ładunku z tylko dwa kolumnami i wierszami.

#### <a name="log-alert-for-azure-log-analytics"></a>Alert dziennika dla usługi Analiza dzienników Azure
Poniżej przedstawiono przykładowe ładunku dla akcji elementu webhook standardowe bez niestandardowych Json, gdy jest używany dla dziennika alerty na podstawie analizy dziennika.

    {
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"},
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Low"
    }



#### <a name="log-alert-for-azure-application-insights"></a>Alert dziennika dla usługi Azure Application Insights
Poniżej przedstawiono przykładowe ładunku dla akcji elementu webhook standardowe bez niestandardowych Json, gdy jest używany dla dziennika alerty na podstawie szczegółowych informacji aplikacji.


    {
    "schemaId":"LogAlert","data":
    {
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"},
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Low"
    }
    }


#### <a name="log-alert-with-custom-json-payload"></a>Alert dziennika z niestandardowy ładunek JSON
Na przykład aby utworzyć niestandardowy ładunek, zawierający tylko nazwę alertu i wyniki wyszukiwania, można poniżej.

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }

Poniżej przedstawiono przykładowe ładunku dla akcji niestandardowej elementu webhook dla wszystkich alertów dziennika.


    {
    "AlertRuleName":"AcmeRule","IncludeSearchResults":true,
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"},
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }




## <a name="next-steps"></a>Kolejne kroki
- Tworzenie i zarządzanie nimi [grupy akcji na platformie Azure](monitoring-action-groups.md)
- Dowiedz się więcej o [alerty dziennika w alertach Azure (wersja zapoznawcza)](monitor-alerts-unified-log.md)
