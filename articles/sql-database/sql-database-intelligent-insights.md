---
title: "Monitorowanie użycia bazy danych za pomocą inteligentnego szczegółowych informacji — usługi Azure SQL Database | Dokumentacja firmy Microsoft"
description: "Azure Insights inteligentnego bazy danych SQL używa wbudowane narzędzie analizy do stale monitorowanie użycia bazy danych za pośrednictwem sztucznego analizy i wykrywania destrukcyjne zdarzenia, które powodują pogorszenie wydajności."
services: sql-database
documentationcenter: 
author: danimir
manager: drasumic
editor: carlrab
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 09/25/2017
ms.author: v-daljep
ms.openlocfilehash: 86011610885ff913bfd70aa46389e4e39989d0a3
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="intelligent-insights"></a>Wyniki analizy

Usługa Azure Insights inteligentnego bazy danych SQL informuje o tym, co się dzieje z wydajność bazy danych.

Inteligentnego Insights używa wbudowane narzędzie analizy do stale monitorowanie użycia bazy danych za pośrednictwem sztucznego analizy i wykrywania destrukcyjne zdarzenia, które powodują pogorszenie wydajności. Gdy wykryte, szczegółowej analizy odbywa się generujący dziennika diagnostyki inteligentnego oceny problemu. Ocena składa się z analiza głównych przyczyn problemu z wydajnością bazy danych i, jeśli to możliwe, zalecenia dotyczące poprawy wydajności. 

## <a name="what-can-intelligent-insights-do-for-you"></a>Inteligentnego Insights czynności dla Ciebie?

Inteligentnego Insights jest wyjątkowa możliwość Azure wbudowanych analizy, która zapewnia następujące wartości:

- Aktywne monitorowanie
- Szczegółowe informacje o wydajności dopasowane
- Wczesne wykrywanie spadek wydajności bazy danych
- Analiza problemów wykrytych Przyczyna
- Zalecenia dotyczące poprawy wydajności
- Skalowanie w poziomie funkcja na setki tysięcy baz danych
- Pozytywny wpływ na zasoby DevOps i całkowity koszt posiadania

## <a name="how-does-intelligent-insights-work"></a>Jak działa inteligentnego Insights?

Inteligentnego Insights analizuje wydajności bazy danych SQL na podstawie porównania ilości obciążenie bazy danych z ostatniej godziny z ostatnich siedmiu dni linii bazowej obciążenie. Obciążenie bazy danych składa się z zapytania uznany za najbardziej znaczących wydajność bazy danych, takich jak najbardziej powtarzające się i największy zapytania. Ponieważ każda baza danych jest unikatowa na podstawie jego struktury danych, użycia i aplikacji, każdej linii bazowej obciążenia, który jest generowany jest specyficzne i unikatowym dla poszczególnych wystąpień. Insights inteligentnego, niezależnie od podstawy obciążenia również monitoruje bezwzględną progi operacyjne i wykrywa problemy z czasy oczekiwania nadmiernego, wyjątki krytyczne i problemy z parameterizations zapytania, które mogłyby wpłynąć na wydajność.

Po wykrycia problemu spadku wydajności z wielu metryki obserwowanych za pomocą analizy sztucznego odbywa się analizy. Dzienników diagnostycznych jest generowana za pomocą inteligentnego wglądu w tym, co dzieje się z bazą danych. Inteligentnego Insights ułatwia to śledzenie problem z wydajnością bazy danych od jego pierwszego wygląd do rozpoznawania. Każdy wykryty problem jest śledzony przez cały cykl jej życia z początkowej emisji wykrywania i weryfikacji zwiększenie wydajności do jego zakończenia. Aktualizacje są zawarte w dzienniku diagnostyki co 15 minut. 

![Przepływ pracy analizy wydajności bazy danych](./media/sql-database-intelligent-insights/intelligent-insights-concept.png)

Metryki używane do mierzenia i wykrywania problemów z wydajnością bazy danych są oparte na czas trwania kwerendy, limit czasu żądania czasy oczekiwania nadmiernego i błędami żądania. Aby uzyskać więcej informacji o metrykach, zobacz [metryki wykrywania](sql-database-intelligent-insights.md#detection-metrics) sekcji tego dokumentu.

## <a name="degradations-detected"></a>Wykryto degradations

Identyfikuje bazy danych SQL degradations wydajności są rejestrowane w dzienniku diagnostyki inteligentnego wpisów, które składają się z następującymi właściwościami:

| Właściwość             | Szczegóły              |
| :------------------- | ------------------- |
| Informacje o bazie danych | Metadane dotyczące bazy danych, na którym wykryto szczegółowe informacje, takie jak identyfikator URI zasobu. |
| W określonym zakresie | Godzina rozpoczęcia i zakończenia okres wiedzę wykryte. |
| Wpływ na metryk | Metryki, który spowodował wgląd do wygenerowania: <ul><li>Zapytania [s] zwiększyć czas trwania.</li><li>Nadmierne oczekiwania [s].</li><li>Upłynął limit czasu żądania [wartość procentowa].</li><li>Raportuje żądania [wartość procentowa].</li></ul>|
| Wartość wpływu | Mierzona wartość metryki. |
| Wpływ na zapytania i kody błędów | Zapytanie skrótu lub kodu błędu. Mogą one używane do skorelowania łatwe do zapytań, których dotyczy. Podano metryk, które składają się z zapytania zwiększyć czas trwania, czas oczekiwania, liczby limitu czasu lub kody błędów. |
| Wykryć | Wykrywanie określonych w bazie danych w czasie zdarzenia. Brak 15 wzorce wykrywania. Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z wydajnością bazy danych z informacjami dotyczącymi inteligentnego](sql-database-intelligent-insights-troubleshoot-performance.md). |
| Analiza głównych przyczyn | Przyczyna analizy problem wskazany w formacie czytelnym dla człowieka. Niektóre szczegółowe informacje może zawierać zalecenie poprawy wydajności, gdy jest to możliwe. |
|||

## <a name="issues-state-lifecycle-active-verifying-and-complete"></a>Problemy dotyczące stanu cyklu: "Active", "Sprawdzanie" i "Zakończ"

Problemy z wydajnością, które są rejestrowane w dzienniku diagnostyki są oznaczone jeden z trzech stanów życia problem: "Active", "Sprawdzanie" i "Zakończ". Po wydajności zostanie wykryty problem, i jak długo ma uznała jako obecny przez wbudowane narzędzie analizy bazy danych SQL, problem jest oznaczony jako "Aktywny". Gdy problem jest uważany za skorygowane, sprawdza i stan problemu zostanie zmieniona na "Sprawdzanie". Po bazy danych SQL wbudowane narzędzie analizy uwzględnia rozwiązać problem, stan problemu z flagą "Zakończ".

## <a name="use-intelligent-insights"></a>Użyj inteligentnego Insights

Analiza dzienników Azure, Azure Event Hubs i usługi Azure Storage, możesz wysłać dziennika diagnostyki inteligentnego szczegółowych informacji. Aby uzyskać więcej informacji, zobacz [metryki bazy danych SQL Azure i rejestrowanie danych diagnostycznych](sql-database-metrics-diag-logging.md). Po wysłaniu dziennika do jednego z następujących elementów docelowych dziennika można niestandardowe alerty i monitorowania Programowanie przy użyciu Microsoft lub narzędzi innych firm. 

Aby uzyskać więcej informacji dotyczących rozwiązywania problemów przy użyciu usługi inteligentnego Insights wydajności bazy danych SQL, zobacz [problemy z wydajnością rozwiązywanie problemów z bazą danych SQL Azure z informacjami dotyczącymi inteligentnego](sql-database-intelligent-insights-troubleshoot-performance.md).

## <a name="built-in-intelligent-insights-analytics-with-log-analytics"></a>Wbudowane analizy inteligentnego wgląd w usłudze analizy dzienników 

Rozwiązania analizy dzienników umożliwia raportowanie i możliwości alertów na górze diagnostyki inteligentnego Insights rejestrowanie danych. W poniższym przykładzie przedstawiono raportu inteligentnego wgląd w module analiz SQL Azure:

![Raport Insights inteligentnego](./media/sql-database-intelligent-insights/intelligent-insights-azure-sql-analytics.png)

Po skonfigurowaniu dziennika diagnostyki inteligentnego wgląd do transmisji danych do analizy SQL można [monitorować bazy danych SQL za pomocą analizy SQL](../log-analytics/log-analytics-azure-sql.md).

## <a name="custom-integrations-of-intelligent-insights-log"></a>Niestandardowej integracji inteligentnego Insights dziennika

Aby uzyskać więcej informacji na niestandardowe alerty oraz ich monitorowania Programowanie przy użyciu Microsoft lub innych narzędzi, zobacz [użycie dziennika diagnostycznego wydajności bazy danych usługi inteligentnego Insights](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="set-up-intelligent-insights-with-event-hubs"></a>Konfigurowanie inteligentnego szczegółowych informacji z usługi Event Hubs

- Aby skonfigurować inteligentnego wgląd do strumienia zdarzeń dziennika do usługi Event Hubs, zobacz [dzienników diagnostycznych strumienia Azure do usługi Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md).
- Aby użyć niestandardowe monitorowanie i alerty usługi Event Hubs, zobacz [co należy zrobić metryki i informacji diagnostycznych loguje usługi Event Hubs](sql-database-metrics-diag-logging.md#what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs). 

## <a name="set-up-intelligent-insights-with-storage"></a>Konfigurowanie inteligentnego szczegółowych informacji z magazynu

- Aby skonfigurować inteligentnego wgląd do przechowania magazynu, zobacz [strumienia do usługi Azure Storage](sql-database-metrics-diag-logging.md#stream-into-storage).

## <a name="detection-metrics"></a>Metryki wykrywania

Metryki używane dla modeli wykrywania Generowanie inteligentnego szczegółowe informacje są oparte na monitorowania:

- czas trwania kwerendy
- Limit czasu żądania
- Czas oczekiwania nadmiernego
- Błąd limit żądań

Czas trwania i limit czasu żądania zapytań są używane jako podstawowy modeli w procesie wykrywania problemów z wydajnością obciążenia bazy danych. Ponieważ bezpośrednio miar co się dzieje z obciążeniem, są używane. W celu wykrycia wszystkich możliwych przypadków wydajność przetwarzania obciążenia degradacji nadmiernego czas oczekiwania i raportuje żądania są używane jako dodatkowe modele do wskazują na problemy, które mają wpływ na wydajność obciążenia.

System automatycznie uważa się zmiany obciążenia i zmiany liczby żądań zapytań dynamicznie wprowadzone w bazie danych, aby określić progów wydajności normalnych, jak i poza zwykłych bazy danych.

Wszystkie metryki weźmie się w różnych relacjach za pośrednictwem modelu danych naukowo pochodnej, który kategoryzuje każdego wykrył problem z wydajnością. Informacje udostępniane za pośrednictwem inteligentnego wglądu obejmują:

* Szczegóły wykrył problem z wydajnością. 
* Analiza głównych przyczyn wykrytego problemu. 
* Zalecenia dotyczące poprawić wydajność monitorowanych bazy danych SQL, jeśli jest to możliwe.

## <a name="query-duration"></a>czas trwania kwerendy

Model degradacji czas trwania zapytania analizuje poszczególnych kwerend i wykrywa wzrostu czasu potrzebnego do kompilowania i wykonywanie zapytań w porównaniu do linii bazowej wydajności.

Jeśli baza danych SQL wbudowane narzędzie analizy wykryje znaczne zwiększenie w kompilacji zapytania lub czasu wykonywania zapytania, która wpływa na wydajność obciążenia, te zapytania są oznaczone jako czas trwania kwerendy problemów spadek wydajności. 

Dziennika diagnostyki inteligentnego Insights generuje skrót zapytania ze spadkiem wydajności zapytania. Zapytanie skrótu wskazuje, czy spadek wydajności był powiązany z wzrost zapytania kompilacji lub czasu wykonywania, którego zwiększyć czas trwania zapytania.

## <a name="timeout-requests"></a>Limit czasu żądania

Limit czasu żądania degradacji modelu analizuje poszczególnych kwerend i wykrywa zwiększenie limitów czasu na poziomie wykonywania zapytań i ogólną przekroczeń limitu czasu żądania w porównaniu do okresu linii bazowej wydajności na poziomie bazy danych.

Upłynął limit czasu niektórych kwerend może nawet przed osiągną etap wykonywania. Środkami przerwane pracowników, a żądania bazy danych SQL wbudowane narzędzie analizy mierzy i analizuje wszystkie zapytania, które osiągnęły bazy danych, czy zapewnia do etapu wykonywania lub nie. 

Po liczba przekroczeń limitu czasu na wykonanie kwerend lub liczbę procesów roboczych żądanie zostało przerwane przekracza próg kontrolowany przez system, dzienników diagnostycznych jest wypełniana inteligentnego szczegółowych informacji.

Wygenerowany szczegółowych danych zawierają liczby żądań przekroczyła limit czasu i liczby zapytań upłynął limit czasu. Wskazanie spadek wydajności jest powiązana z zwiększenie limitu czasu na etapie wykonywania, lub ogólnego poziomu bazy danych jest dostępne. Gdy zwiększenie limitów czasu uważa za istotne dla wydajność bazy danych, te zapytania są oznaczane jako problemów spadek wydajności limitu czasu. 

## <a name="excessive-wait-times"></a>Czasy oczekiwania nadmiernego

Model czas oczekiwania nadmiernego monitoruje zapytań poszczególne bazy danych. Ta funkcja wykrywa Statystyka oczekiwania nadzwyczaj dużego zapytania, które przekroczeniu progów bezwzględną kontrolowany przez system. Następujące metryki czasu oczekiwania nadmiernego zapytania są przestrzegane przy użyciu nowych funkcji programu SQL Server, zapytania magazynu oczekiwania statystyka (sys.query_store_wait_stats):

- Osiągnięcia limitów zasobów
- Osiągnięcia limitów zasobów w puli elastycznej
- Nadmiernej liczby wątków roboczych lub sesji
- Blokowanie nadmiernego bazy danych
- Wykorzystanie pamięci
- Statystyka innych oczekiwania

Osiągnięcia limitów zasobów lub limitów zasobów w puli elastycznej Określa, czy zużycie zasobów dostępnych w ramach subskrypcji lub w puli elastycznej przekroczeniu progów bezwzględną. Te statystyki wskazywać spadku wydajności obciążeń. Nadmierną liczbę wątków roboczych lub sesji określa warunek, w którym liczbę wątków roboczych lub sesje zainicjowane przekroczeniu progów bezwzględną. Te statystyki wskazywać spadku wydajności obciążeń.

Blokowanie nadmiernego bazy danych określa warunek, w którym liczba blokady w bazie danych przekroczyło progi bezwzględną. Ten stan wskazuje spadek wydajności obciążeń. Pamięci wykorzystania jest warunek, w którym daje to liczba wątków żąda pamięci przekroczyła próg bezwzględną. Ten stan wskazuje spadek wydajności obciążeń.

Inne wykrywania Statystyka oczekiwania wskazuje warunek, w którym różne metryki zmierzone za pośrednictwem Statystyka oczekiwania magazynu zapytań przekroczyła próg bezwzględną. Te statystyki wskazywać spadku wydajności obciążeń.

Po wykryciu czasy oczekiwania nadmiernego, w zależności od dostępnych danych, Diagnostyka inteligentnego Insights dziennika skrótów wyniki zapytań obejmujące i dotknięte problemem ze spadkiem wydajności, szczegóły metryki, które powodują zapytania oczekiwania podczas wykonywania, a czas oczekiwania zmierzona.

## <a name="errored-requests"></a>Błędnych żądań

Model degradacji błędnych żądań poszczególne monitory wysyła kwerendy i wykrywa wzrost liczby zapytań czy błędne się w porównaniu do okresu linii bazowej. Ten model monitoruje również wyjątki krytyczne przekroczeniu progów bezwzględną zarządzane przez wbudowane narzędzie analizy bazy danych SQL. System automatycznie uwzględnia liczbę żądań zapytania do bazy danych i kont zmian obciążenia w okresie monitorowane.

Gdy zmierzona wzrost błędnych żądań względem ogólnej liczby żądań jest uznane za istotne dla wydajność przetwarzania obciążenia, uwzględnione kwerendy są oznaczane jako problemów spadek wydajności błędnych żądań.

Dziennik Insights inteligentnego generuje liczba błędnych żądań. Wskazuje, czy spadek wydajności był powiązany wzrost błędnych żądań lub przekroczenia progu monitorowanych wyjątek krytyczny i czasem degradacji wydajności. 

Jeśli monitorowana wyjątki krytyczne cross bezwzględną progów zarządzane przez system, inteligentnego wglądu jest generowany ze szczegółami wyjątek krytyczny.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [bazy danych SQL Rozwiązywanie problemów z wydajnością z informacjami dotyczącymi inteligentnego](sql-database-intelligent-insights-troubleshoot-performance.md).
* Użyj [dziennika diagnostyki wydajności bazy danych SQL inteligentnego Insights](sql-database-intelligent-insights-use-diagnostics-log.md).
* Dowiedz się, jak [monitorować bazy danych SQL za pomocą analizy SQL](../log-analytics/log-analytics-azure-sql.md).
* Dowiedz się, jak [zbierania i wykorzystywania danych dziennika z zasobów platformy Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).


