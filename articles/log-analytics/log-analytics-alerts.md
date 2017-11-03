---
title: "Opis alertów w Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Alerty w analizy dzienników zidentyfikować ważne informacje zawarte w repozytorium OMS i aktywne powiadamia użytkownika o problemy lub akcji, aby je poprawić.  W tym artykule opisano różne rodzaje reguły alertów i jak są zdefiniowane."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/13/2017
ms.author: bwren
ms.openlocfilehash: ee11f64484a66fad06b6536a18f9b3e239fa40d5
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2017
---
# <a name="understanding-alerts-in-log-analytics"></a>Opis alertów w analizy dzienników

Alerty w analizy dzienników zidentyfikować ważne informacje zawarte w repozytorium analizy dzienników.  Ten artykuł zawiera szczegółowe informacje, jak alertu reguł w pracach analizy dzienników i opisano różnice między różnych typów reguł alertów.

W procesie tworzenia reguły alertów zobacz następujące artykuły:

- Tworzyć reguły alertów za pomocą [portalu Azure](log-analytics-alerts-creating.md)
- Tworzyć reguły alertów za pomocą [szablonu usługi Resource Manager](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md)
- Tworzyć reguły alertów za pomocą [interfejsu API REST](log-analytics-api-alerts.md)


## <a name="alert-rules"></a>Reguły alertów

Alerty są tworzone przez reguły alertów, które automatycznie uruchamiać dziennik wyszukiwania w regularnych odstępach czasu.  Jeśli wyniki wyszukiwania dziennika spełniających kryteria określonego tworzony jest rekord alertu.  Reguła następnie automatycznie uruchomić co najmniej jednej akcji do aktywnego powiadomienia o alercie lub wywołać inny proces.  Różnych typów reguł alertów używać różnych logikę do wykonywania tej analizy.

![Alerty usługi Log Analytics](media/log-analytics-alerts/overview.png)

Reguły alertów są określone przez następujące informacje:

- **Dziennik wyszukiwania**.  Zapytanie, do którego jest uruchamiany za każdym razem, gdy generowane reguły alertów.  Rekordów zwróconych przez to zapytanie jest używany do określenia, czy alert jest tworzony.
- **Przedział czasu**.  Określa przedział czasu dla zapytania.  Zapytanie zwraca tylko te rekordy, które zostały utworzone w ramach tego zakresu bieżącego czasu.  Może to być dowolna wartość od 5 minut do 24 godzin. Na przykład jeśli przedział czasu jest ustawiona na 60 minut, a godzina 13:15 będzie uruchomieniu kwerendy, jest zwracana tylko rekordy między 12:15:00 a 13:15:00.
- **Częstotliwość**.  Określa, jak często mają być uruchamiane zapytania. Może być dowolną wartość z zakresu od 5 minut do 24 godzin. Powinna być równa lub mniejsza niż przedział czasu.  Jeśli wartość jest większa niż przedział czasu, istnieje ryzyko rekordów jest pominięte.<br>Rozważmy na przykład okno czasu 30 minut i częstotliwość 60 minut.  Jeśli zapytanie jest uruchomione 1:00, zwraca rekordów między 12:30 i 1:00 PM.  Przy następnym uruchom zapytanie to 2:00 po zwróci rekordów między 1:30 i 2:00.  Nigdy nie będzie można obliczyć wszystkie rekordy między 1:00 i 1:30.
- **Próg**.  Wyniki wyszukiwania dziennika są oszacowywane, aby określić, czy można utworzyć alertu.  Próg jest różne dla różnych typów reguł alertów.

Każdej reguły alertu w analizy dzienników jest jednym z dwóch typów.  Każdy z tych typów jest szczegółowo opisane w kolejnych sekcjach.

- **[Liczba wyników](#number-of-results-alert-rules)**. Pojedynczy alert utworzony, jeśli liczba rekordów zwróconych przez wyszukiwanie dziennika przekracza podanej liczby.
- **[Metryki pomiaru](#metric-measurement-alert-rules)**.  Alert utworzony dla każdego obiektu w wynikach wyszukiwania dziennika z wartościami, które wykraczają poza określoną wartość progową.

Dostępne są następujące różnice między typami reguły alertów.

- **Liczba wyników** reguły alertu zawsze spowoduje utworzenie jednego alertu chwilę **metryki pomiaru** alertu zasada tworzy alert dla każdego obiektu, który przekracza wartość progową.
- **Liczba wyników** reguły alertów tworzą alert po przekroczeniu progu jeden raz. **Metryki pomiaru** reguły alertów można utworzyć alertu, po przekroczeniu progu określona liczba razy w określonym interwale.

## <a name="number-of-results-alert-rules"></a>Liczba wyników reguły alertów
**Liczba wyników** reguły alertów Utwórz pojedynczy alert, kiedy to liczba rekordów zwróconych przez zapytanie wyszukiwania przekraczają określony próg.

### <a name="threshold"></a>Próg
Próg **liczba wyników** reguły alertu jest po prostu większa lub mniejsza od określonej wartości.  Jeśli liczba rekordów zwróconych przez wyszukiwanie dziennika zgodna te kryteria, tworzona jest alert.

### <a name="scenarios"></a>Scenariusze

#### <a name="events"></a>Zdarzenia
Ten typ alertu jest idealne rozwiązanie w przypadku pracy z zdarzenia, takie jak dzienniki zdarzeń systemu Windows, Syslog, i niestandardowe dzienniki.  Możesz utworzyć alert podczas tworzenia pobiera zdarzeń określony błąd, lub wielu zdarzeń błędu są tworzone w ramach okna określony czas.

Aby alert po wystąpieniu pojedyncze zdarzenie, ustaw liczbę wyników na wartość większą niż 0 i częstotliwość i przedział czasu na 5 minut.  Uruchamiające kwerendę co 5 minut i sprawdź, czy wystąpienie jednego zdarzenia, który został utworzony od czasu ostatniego uruchomienia zapytania.  Częstotliwość dłużej może być opóźniona czas między zdarzenia są zbierane i tworzony alert.

Niektóre aplikacje mogą rejestrować okazjonalne błąd, który nie należy koniecznie zgłosi alert.  Na przykład aplikacja może ponów próbę wykonania procesu, który utworzył zdarzenie błędu i powiodło się podczas następnego.  W takim przypadku nie możesz utworzyć alert, jeśli wiele zdarzeń są tworzone w ramach okna określonym czasie.  

W niektórych przypadkach warto utworzyć alert w przypadku braku zdarzeń.  Na przykład procesu mogą rejestrować regularnego zdarzenia w celu wskazania, że działa prawidłowo.  Jeżeli nie rejestrować jedno z tych zdarzeń w oknie określonym czasie, powinien zostać utworzony alert.  W takim przypadku należy ustawić wartość progową **mniej niż 1**.

#### <a name="performance-alerts"></a>Alerty wydajności
[Dane dotyczące wydajności](log-analytics-data-sources-performance-counters.md) jest przechowywana jako rekordów w repozytorium OMS podobne do zdarzenia.  Jeśli chcesz alert, gdy licznik wydajności przekracza próg określonego progu powinien uwzględnione w zapytaniu.

Na przykład, jeśli chcesz alertów po uruchomieniu procesor ponad 90%, można skorzystać podobnie do następującej kwerendy z progiem dla reguły alertu **większa niż 0**.

    Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" and CounterValue>90

    

Jeśli chcesz alert, gdy procesor średnio ponad 90% okna określony czas, należy użyć zapytania za pomocą [miar polecenia](log-analytics-search-reference.md#commands) podobnie do następującego z progiem dla reguły alertu **większa niż 0**.

    Perf | where ObjectName=="Processor" and CounterName=="% Processor Time" | summarize avg(CounterValue) by Computer | where CounterValue>90

    
>[!NOTE]
> Jeśli obszar roboczy nie została jeszcze uaktualniona do [języka zapytań nowe analizy dzienników](log-analytics-log-search-upgrade.md), następnie zmieniłby powyżej zapytania do następującego:`Type=Perf ObjectName=Processor CounterName="% Processor Time" CounterValue>90`
> `Type=Perf ObjectName=Processor CounterName="% Processor Time" | measure avg(CounterValue) by Computer | where AggregatedValue>90`


## <a name="metric-measurement-alert-rules"></a>Metryki pomiaru reguły alertów

>[!NOTE]
> Reguły alertów metryki pomiaru są obecnie w wersji zapoznawczej.

**Metryki pomiaru** reguły alertów tworzyć alert dla każdego obiektu w zapytaniu z wartością, która przekracza określoną wartość progową.  Mają one następujące różnice różne **liczba wyników** reguły alertów.

#### <a name="log-search"></a>Wyszukiwanie w dzienniku
Za pomocą dowolnego zapytania dla **liczba wyników** reguły alertu, istnieją wymagania zapytania dla metryki pomiaru reguły alertu.  Musi on zawierać [miar polecenia](log-analytics-search-reference.md#commands) do grupowania wyników w określonym polu. To polecenie musi zawierać następujące elementy.

- **Funkcję agregacji**.  Określa obliczeń, który jest wykonywane i potencjalnie liczbową pole do zagregowania.  Na przykład **count()** zwróci liczbę rekordów w zapytaniu, **avg(CounterValue)** zwróci średnią pole równowartości dla interwału.
- **Pole grupy**.  Zostaje utworzony rekord z zagregowane wartości dla poszczególnych wystąpień tego pola, a alert jest generowany dla każdego.  Na przykład, jeśli chcesz generować alert dla każdego komputera, możesz użyć **przez komputer**.   
- **Interwał**.  Określa przedział czasu, w którym są agregowane.  Na przykład, jeśli określono **5minutes**, może zostać utworzony rekord dla każdego wystąpienia pola grupy przez przedział czasu określony dla alertu agregowana co 5 minut.

#### <a name="threshold"></a>Próg
Próg metryki pomiaru reguły alertów jest definiowany przez wartości zagregowanej i liczbę naruszeń.  Dowolnego punktu danych w dzienniku wyszukiwania przekracza tę wartość, jest uznawane za naruszenia.  Jeśli liczba naruszeń w dla dowolnego obiektu w wynikach przekracza określoną wartość, alert zostanie utworzony dla tego obiektu.

#### <a name="example"></a>Przykład
Rozważmy scenariusz, w którym chce alert dowolnego komputera przekroczeniu użycie procesora przez 90% trzy razy ponad 30 minut.  Należy utworzyć regułę alertu z poniższymi szczegółami.  

**Zapytanie:** wydajności | gdzie ObjectName == "Procesor" i CounterName == "% czasu procesora" | Podsumuj AggregatedValue = avg(CounterValue) przez bin (TimeGenerated, 5 m), komputer<br>
**Przedział czasu:** 30 minut<br>
**Alert częstotliwości:** 5 minut<br>
**Wartość agregacji:** dużą niż 90<br>
**Na podstawie wyzwalania alertu:** naruszeń łączna liczba jest większa niż 5<br>

Zapytanie spowodowałoby średnią wartość dla każdego komputera co 5 minut.  To zapytanie może działać co 5 minut przez dane zbierane przez poprzednie 30 minut.  Poniżej przedstawiono przykładowe dane na trzech komputerach.

![Przykładowe wyniki zapytania](media/log-analytics-alerts/metrics-measurement-sample-graph.png)

W tym przykładzie oddzielne alerty zostałyby utworzone dla srv02 i srv03, ponieważ ich naruszenia progu 90% 3 razy za pośrednictwem przedział czasu.  Jeśli **na podstawie wyzwalania alertu:** zostały zmienione na **kolejno** , a następnie alert będzie można utworzyć tylko dla srv03, ponieważ jego naruszenia progu dla kolejnych próbek 3.

## <a name="alert-records"></a>Rejestruje alertu
Rekordy alertu przez reguły alertów w analizy dzienników mają **typu** z **Alert** i **SourceSystem** z **OMS**.  Mają one właściwości w poniższej tabeli.

| Właściwość | Opis |
|:--- |:--- |
| Typ |*Alert* |
| SourceSystem |*OMS* |
| *Obiekt*  | [Alerty metryki pomiaru](#metric-measurement-alert-rules) ma właściwość dla pola grupy.  Na przykład jeśli wyszukiwanie dziennika grupy na komputerze, alertów rekord z ma pole komputera o nazwie komputera jako wartość.
| AlertName |Nazwa alertu. |
| AlertSeverity |Poziom ważności alertu. |
| LinkToSearchResults |Link do wyszukiwania dziennika analizy dzienników, która zwraca odpowiednie rekordy z kwerendy utworzony alert. |
| Zapytanie |Tekst zapytania, które zostało uruchomione. |
| QueryExecutionEndTime |Koniec przedziału czasu dla zapytania. |
| QueryExecutionStartTime |Początek przedziału czasu dla zapytania. |
| ThresholdOperator | Operator, który został użyty przez regułę alertu. |
| ThresholdValue | Wartość, która była używana przez regułę alertu. |
| TimeGenerated |Data i godzina utworzenia alertu. |

Istnieją inne rodzaje rekordy alertu przez [rozwiązania do zarządzania Alert](log-analytics-solution-alert-management.md) oraz [eksportuje usługi Power BI](log-analytics-powerbi.md).  Te wszystkie mają **typu** z **alertu** , ale można rozróżnić za ich **SourceSystem**.


## <a name="next-steps"></a>Następne kroki
* Zainstaluj [rozwiązania zarządzania alertami](log-analytics-solution-alert-management.md) do analizowania alertach tworzonych w analizy dzienników oraz alertów zebranych z programu System Center Operations Manager.
* Przeczytaj więcej na temat [dziennika wyszukiwania](log-analytics-log-searches.md) który generowania alertów.
* Zakończenie wskazówki dla [Konfigurowanie elementu webhook](log-analytics-alerts-webhooks.md) z reguły alertu.  
* Dowiedz się, jak napisać [elementy runbook automatyzacji Azure](https://azure.microsoft.com/documentation/services/automation) skorygować problemy zidentyfikowane przez alertów.
