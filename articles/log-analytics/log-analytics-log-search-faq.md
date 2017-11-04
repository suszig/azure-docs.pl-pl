---
title: "Nowe wyszukiwanie dziennika analizy dziennika często zadawane pytania | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera często zadawane pytania dotyczące uaktualniania analizy dzienników dla nowego języka zapytań."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: bwren
ms.openlocfilehash: bf48cbc52a1ed96ed1bb49b1879d5cd7aece945c
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="log-analytics-new-log-search-faq-and-known-issues"></a>Często zadawane pytania dotyczące wyszukiwania i znane problemy dziennika nowe analizy dzienników

Ten artykuł zawiera często zadawane pytania i znane problemy dotyczące uaktualniania [analizy dzienników dla nowego języka zapytań](log-analytics-log-search-upgrade.md).  W tym artykule całego powinien przeczytania przed podjęciem decyzji o uaktualnienie obszaru roboczego.


## <a name="alerts"></a>Alerty

### <a name="question-i-have-a-lot-of-alert-rules-do-i-need-to-create-them-again-in-the-new-language-after-i-upgrade"></a>Pytanie: masz wiele reguł alertów. Należy utworzyć je ponownie w nowym języku po uaktualnieniu?  
Nie, regułami alertów są konwertowane na nowy język wyszukiwania podczas uaktualniania.  

### <a name="question-i-have-alert-rules-with-webhook-and-runbook-actions-will-these-continue-to-work-when-i-upgrade"></a>Pytanie: masz reguły alertów z Akcje elementu webhook i elementu runbook. Będą one nadal działać po uaktualnieniu?

Nie, istnieją pewne zmiany w akcji elementu webhook i runbook, które mogą wymagać można wprowadzić zmian w sposobie przetwarzania ładunku. Wprowadziliśmy tych zmian do standaryzacji różnych formatów wyjściowych i zmniejszyć rozmiar ładunku. Szczegóły dotyczące tych formatów są w [Dodawanie akcji do reguły alertów w analizy dzienników](log-analytics-alerts-actions.md).


## <a name="computer-groups"></a>Grupy komputerów

### <a name="question-im-getting-errors-when-trying-to-use-computer-groups--has-their-syntax-changed"></a>Pytanie: błędów występujących podczas próby użycia grupy komputerów.  Zmieniono ich składni?
Tak, przy użyciu komputera składnia grupuje zmiany po uaktualnieniu obszaru roboczego.  Zobacz [grup komputerów w analizy dzienników dziennika wyszukiwania](log-analytics-computer-groups.md) szczegółowe informacje.

### <a name="known-issue-groups-imported-from-active-directory"></a>Znany problem: grup importowany z usługi Active Directory
Nie można obecnie utworzyć kwerendę, która używa grupy komputerów importowany z usługi Active Directory.  Jako rozwiązanie alternatywne dopóki ten problem zostanie rozwiązany, utworzyć grupę komputerów za pomocą zaimportowanych grupy usługi Active Directory, a następnie użyć tej nowej grupy w zapytaniu.

Przykładowe zapytanie, aby utworzyć nową grupę komputera, która zawiera grupę usługi Active Directory zaimportowanych wygląda następująco:

    ComputerGroup | where GroupSource == "ActiveDirectory" and Group == "AD Group Name" and TimeGenerated >= ago(24h) | distinct Computer


## <a name="dashboards"></a>Pulpity nawigacyjne

### <a name="question-can-i-still-use-dashboards-in-an-upgraded-workspace"></a>Pytanie: Czy nadal mogę korzystać pulpitów nawigacyjnych w uaktualnionym obszaru roboczego?
Po uaktualnieniu, firma Microsoft rozpoczyna proces depracating **Mój pulpit nawigacyjny**.  Można nadal korzystać z żadnych kafelków dodane do pulpitu nawigacyjnego, przed obszaru roboczego został uaktualniony, ale nie można edytować tych kafelków ani dodawać nowe.  Możesz nadal tworzyć i edytować widoków z [Widok projektanta](log-analytics-view-designer.md), który zawiera bardziej zaawansowane funkcje funkcji ustawiona, a także tworzenia pulpitów nawigacyjnych w portalu Azure.


## <a name="log-searches"></a>Dziennik wyszukiwania

### <a name="question-i-have-saved-searches-outside-of-my-upgraded-workspace-can-i-convert-them-to-the-new-search-language-automatically"></a>Pytanie: zostały już zapisane wyszukiwanie poza obszar Mój obszar roboczy uaktualniony. Można przekonwertować je na nowy język wyszukiwania automatycznie?
Narzędzia konwertera języka na stronie dziennik wyszukiwania umożliwia konwertowanie każdej z nich.  Nie istnieje metoda aby dokonać automatycznej konwersji wielu wyszukiwania bez uaktualniania obszaru roboczego.

### <a name="question-why-are-my-query-results-not-sorted"></a>Pytanie: Dlaczego moja wyników zapytania nie są posortowane?
Wyniki są sortowane nie domyślnie w nowym języku zapytania.  Użyj [operator sortowania](https://go.microsoft.com/fwlink/?linkid=856079) Aby posortować wyników według co najmniej jednej właściwości.

### <a name="question-where-did-the-metrics-view-go-after-i-upgraded"></a>Pytanie: Gdzie widoku metryki stało po uaktualnieniu?
Widok metryki nadaje graficzną reprezentację danych wydajności z wyszukiwania dziennika.  Ten widok nie jest już dostępne po uaktualnieniu.  Można użyć [renderowania operator](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) do formatowania danych wyjściowych z zapytania w timechart.

### <a name="question-where-did-minify-go-after-i-upgraded"></a>Pytanie: Gdzie zminimalizowania Przejdź po uaktualnieniu?
Zminimalizowania jest funkcją, która zapewnia widok podsumowania wyników wyszukiwania.  Po uaktualnieniu, Minify nie jest dostępna w portalu wyszukiwania dziennika.  Można uzyskać podobne funkcje z nowego wyszukiwania języka przy użyciu [zmniejszyć](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/reduce-operator) lub [autocluster_v2](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/evaluate-operator/autocluster). 

    Event
    | where TimeGenerated > ago(10h)
    | reduce by RenderedDescription

    Event
    | where TimeGenerated > ago(10h)
    | evaluate autocluster_v2()


### <a name="known-issue-search-results-in-a-list-may-include-properties-with-no-data"></a>Znany problem: wyniki wyszukiwania na liście mogą zawierać właściwości bez danych
Dziennik wyniki wyszukiwania na liście mogą wyświetlać właściwości nie zawiera żadnych danych.  Przed uaktualnieniem te właściwości nie będą dołączone.  Ten problem zostanie rozwiązany, aby nie są wyświetlane właściwości puste.

### <a name="known-issue-selecting-a-value-in-a-chart-doesnt-display-detailed-results"></a>Znany problem: Wybieranie wartość na wykresie nie są wyświetlane szczegółowe wyniki
Przed uaktualnieniem gdy wybrana wartość na wykresie go zwróci szczegółową listę rekordy pasujące do wybranej wartości.  Po uaktualnieniu jest zwracana tylko pojedynczy wiersz podsumowania.  Obecnie trwa bada ten problem.

## <a name="log-search-api"></a>Interfejs API wyszukiwania w dzienniku

### <a name="question-does-the-log-search-api-get-updated-after-i-upgrade"></a>Pytanie: Interfejs API wyszukiwania dziennika zostanie zaktualizowana po uaktualnieniu?
Starszego [interfejs API dziennika wyszukiwania](log-analytics-log-search-api.md) przestanie działać po uaktualnieniu obszaru roboczego.  Zobacz [interfejsu API REST usługi Analiza dzienników Azure](https://dev.loganalytics.io/) szczegółowe informacje o nowym interfejsem API.


## <a name="portals"></a>Portale

### <a name="question-should-i-use-the-new-advanced-analytics-portal-or-keep-using-the-log-search-portal"></a>Pytanie: Należy I użyć nowego portalu analityka zaawansowane lub nadal korzystać z portalu wyszukiwania dziennika?
Widać porównanie dwa portale w [portali tworzenia i edytowania dziennika zapytań w programie Azure Log Analytics](log-analytics-log-search-portals.md).  Każdy ma wiele zalet, można wybrać najlepszy do wymagań.  Jest często stosowanym rozwiązaniem pisać zapytania w portalu analityka zaawansowane i wklej je do innych miejsc, takich jak Widok projektanta.  Należy się zapoznać [zagadnień do uwzględnienia](log-analytics-log-search-portals.md#advanced-analytics-portal) po operacją.


### <a name="question--after-upgrade-i-get-an-error-trying-to-run-queries-and-am-also-seeing-errors-in-my-views"></a>Pytanie: Po uaktualnieniu I wystąpi błąd w trakcie wykonywania kwerend i również zwróciła błędy w moich widoków.

Przeglądarka wymaga dostępu do następujących adresów do wykonywania kwerend analizy dzienników po uaktualnieniu.  Jeśli przeglądarka uzyskuje dostęp do portalu Azure za pośrednictwem zapory, należy włączyć dostęp do tych adresów.

| Identyfikator URI | Adres IP | Porty |
|:---|:---|:---|
| Portal.loganalytics.IO | Dynamiczny | 80,443 |
| API.loganalytics.IO    | Dynamiczny | 80,443 |
| docs.loganalytics.IO   | Dynamiczny | 80,443 |



## <a name="power-bi"></a>Power BI

### <a name="question-does-anything-change-with-powerbi-integration"></a>Pytanie: Czy niczego zmienia się z integracją usługi Power BI?
Tak.  Po uaktualnieniu obszaru roboczego procesu eksportowania analizy dzienników danych do usługi Power BI nie będzie działać.  Istniejących harmonogramów, które zostały utworzone przed rozpoczęciem uaktualniania zostanie wyłączony.  Po uaktualnieniu, analiza dzienników Azure używa tej samej platformy jako usługi Application Insights i używać tego samego procesu eksportowania analizy dzienników zapytań do usługi Power BI jako [procesu eksportowania zapytań usługi Application Insights do usługi Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).

### <a name="known-issue-power-bi-request-size-limit"></a>Znany problem: limit rozmiaru żądania usługi Power BI
Obecnie jest ograniczenie do 8 MB dla zapytania analizy dzienników, które mogą być eksportowane do usługi Power BI.  Wkrótce będzie można zwiększyć ten limit.


## <a name="powershell-cmdlets"></a>Polecenia cmdlet programu PowerShell

### <a name="question-does-the-log-search-powershell-cmdlet-get-updated-after-i-upgrade"></a>Pytanie: Polecenie cmdlet programu PowerShell wyszukiwania dziennika zostanie zaktualizowana po uaktualnieniu?
[Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/Get-AzureRmOperationalInsightsSearchResults) zostaną wycofane po zakończeniu uaktualniania wszystkich obszarów roboczych.  Użyj [polecenia cmdlet Invoke-LogAnalyticsQuery](https://dev.loganalytics.io/documentation/Tools/PowerShell-Cmdlets) na wyszukiwanie dziennika w uaktualnionym obszarów roboczych.




## <a name="resource-manager-templates"></a>Szablony usługi Resource Manager

### <a name="question-can-i-create-an-upgraded-workspace-with-a-resource-manager-template"></a>Pytanie: Można utworzyć obszar roboczy uaktualniony przy użyciu szablonu usługi Resource Manager?
Tak.  Należy użyć wersji interfejsu API 2017-03-15-preview i obejmują **funkcje** sekcji w szablonie, jak w poniższym przykładzie.

    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "apiVersion": "2017-03-15-preview",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceRegion')]",
            "properties": {
                "sku": {
                    "name": "Free"
                },
                "features": {
                    "legacy": 0,
                    "searchVersion": 1
                }
            }
        }
    ],



## <a name="solutions"></a>Rozwiązania

### <a name="question-will-my-solutions-continue-to-work"></a>Pytanie: Moje rozwiązań będzie działać?
Wszystkie rozwiązania będą nadal działać w obszarze roboczym uaktualniony, mimo że ich wydajności zwiększy wtedy są konwertowane na nowy język kwerendy.  Istnieją znane problemy z istniejącego rozwiązania, które zostały opisane w tej sekcji.

### <a name="known-issue-capacity-and-performance-solution"></a>Znany problem: pojemność i wydajność rozwiązania
Niektóre elementy w [pojemność i wydajność](log-analytics-capacity.md) widok może być pusta.  Rozwiązano ten problem będzie dostępna wkrótce.

### <a name="known-issue-application-insights-connector"></a>Znany problem: łącznika usługi Application Insights
Perspektywy [rozwiązanie Application Insights łącznik](log-analytics-app-insights-connector.md) nie są obecnie obsługiwane w obszarze roboczym uaktualniony.  Rozwiązania tego problemu jest obecnie w obszarze analizy.

### <a name="known-issue-backup-solution"></a>Znany problem: kopii zapasowej rozwiązania
Rozwiązanie tworzenia kopii zapasowej nie będzie zbierał dane w uaktualnionym obszaru roboczego. Wkrótce będą ogłaszane nowe rozwiązanie tworzenia kopii zapasowej współpracujące z obszaru roboczego uaktualniony.

## <a name="upgrade-process"></a>Proces uaktualniania

### <a name="question-i-have-several-workspaces-can-i-upgrade-all-workspaces-at-the-same-time"></a>Pytanie: masz kilka obszarów roboczych. W tym samym czasie można uaktualnić wszystkie obszary robocze?  
Nie.  Uaktualnienie dotyczy jednego obszaru roboczego zawsze. Obecnie nie istnieje sposób uaktualnienia jednocześnie wiele obszarów roboczych. Należy pamiętać, również wpływ innym użytkownikom uaktualniony obszaru roboczego.  

### <a name="question-will-existing-log-data-collected-in-my-workspace-be-modified-if-i-upgrade"></a>Pytanie: Zostaną istniejące dane dziennika zebrane w obszarze roboczym zmodyfikowane I uaktualnienia?  
Nie. Dostępne do wyszukiwania obszaru roboczego danych dziennika nie ma wpływu na uaktualnienie. Zapisane wyszukiwania, alertów i widoków zostaną przekonwertowane na nowy język wyszukiwania automatycznie.  

### <a name="question-what-happens-if-i-dont-upgrade-my-workspace"></a>Pytanie: Co się stanie, jeśli nie można uaktualnić obszar Mój obszar roboczy?  
Wyszukiwanie starszych dziennika zostaną wycofane w najbliższych miesiącach. Obszary robocze, które nie zostały uaktualnione do tego czasu zostanie automatycznie uaktualniona.

### <a name="question-can-i-revert-back-after-i-upgrade"></a>Pytanie: Strony I przywrócić po uaktualnieniu?
Przed ogólnie można przywrócić obszaru roboczego po uaktualnieniu.  Teraz, gdy nowy język osiągnęła ogólnej dostępności, ta funkcja została usunięta gdy Rozpoczniemy wycofywania starszej wersji platformy.



## <a name="views"></a>Widoki

### <a name="question-how-do-i-create-a-new-view-with-view-designer"></a>Pytanie: Jak utworzyć nowy widok z projektanta widoków?
Przed uaktualnieniem można utworzyć nowy widok z projektanta widoków z kafelka na głównym pulpicie nawigacyjnym.  Po uaktualnieniu obszaru roboczego tego kafelka zostaną usunięte.  Można utworzyć nowy widok z projektanta widoków w portalu OMS, klikając przycisk w lewym menu + zielony.

### <a name="known-issue-see-all-option-for-line-charts-in-views-doesnt-result-in-a-line-chart"></a>Znany problem: Zobacz wszystkich opcji w przypadku wykresów liniowych w widokach nie powoduje wykres liniowy
Po kliknięciu *zobaczyć wszystkie* opcji w dolnej części wykresu wiersza w widoku jest wyświetlana z tabelą.  Przed uaktualnieniem będą przedstawiane z wykres liniowy.  Ten problem jest analizowana potencjalnych modyfikacji.


## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [uaktualnianie obszaru roboczego do nowego języka zapytań usługi Analiza dzienników](log-analytics-log-search-upgrade.md).
