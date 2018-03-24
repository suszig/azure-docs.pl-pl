---
title: Rejestrowania alertów w monitorze Azure - alerty | Dokumentacja firmy Microsoft
description: Wyzwalanie wiadomości e-mail, powiadomienia, należy wywołać adresów URL witryny sieci Web (elementy webhook) lub automatyzacji, po spełnieniu warunków złożonego zapytania, które określisz Azure alertów.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2018
ms.author: vinagara
ms.openlocfilehash: 5928bbcec08d6ba4ac0b0d03b66fa4bfc8f5e3d7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="log-alerts-in-azure-monitor---alerts"></a>Alerty dziennika w monitorze Azure - alertów 
Ten artykuł zawiera szczegółowe informacje, jak alertu reguł w pracach zapytania analityka w alertach Azure i opisano różnice między różnych typów reguł alertów dziennika. Aby uzyskać więcej informacji o alertach Metryka przy użyciu dzienników, zapoznaj się [niemal alerty metryki czasu rzeczywistego](monitoring-near-real-time-metric-alerts.md)

Obecnie alerty Azure obsługuje rejestrowania alertów dla zapytań z [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) i [usługi Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events).

> [!WARNING]

> Obecnie alert dziennika w alertach Azure nie obsługuje zapytań między roboczym lub wielu aplikacji. I alerty dziennika dla usługi Application Insights jest w wersji zapoznawczej — funkcje i środowisko użytkownika może ulec zmianie.

Ponadto użytkownicy mogą doskonała ich zapytania w platformie analizy wyboru na platformie Azure, a następnie *zaimportować je do użycia w alerty po zapisaniu zapytania*. Kroki do wykonania:
- Dla usługi Application Insights: Portal analityka przejdź do weryfikacji zapytań i ich wyników. Następnie zapisz o unikatowej nazwie w *udostępnione zapytania*.
- W celu wykonania analizy dziennika: Przejdź do wyszukiwania dziennika, kwerendy i wyniki sprawdzenia. Następnie użyj zapisanie o unikatowej nazwie w każdej kategorii.

Następnie po [tworzenia alertu dziennika w alertach ](monitor-alerts-unified-usage.md), zobacz zapisanego zapytania, które są wyświetlane jako typ sygnału **dziennika (zapisane zapytanie)**; jak pokazano w poniższym przykładzie: ![zapisanego zapytania, które są importowane do alertów](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog-new.png)

> [!NOTE]
> Przy użyciu **dziennika (zapisane zapytanie)** wynikiem importowania do alertów. Dlatego wszelkie zmiany dokonywane po w module analiz nie będzie odbicia w zapisanych reguł alertów i na odwrót.

## <a name="log-alert-rules"></a>Reguły alertów dziennika

Alerty są tworzone przez Azure alerty do automatycznego uruchamiania kwerend dziennika w regularnych odstępach czasu.  Jeśli wyniki zapytania dziennika spełniających kryteria określonego, tworzony jest rekord alertu. Reguła następnie automatycznie uruchomić co najmniej jednej akcji do aktywnego powiadomienia o alercie lub wywołanie innego procesu, takie jak wysyłanie danych do aplikacji zewnętrznych za pomocą [json na podstawie elementu webhook](monitor-alerts-unified-log-webhook.md)za pomocą [grupy akcji](monitoring-action-groups.md). Różnych typów reguł alertów używać różnych logikę do wykonywania tej analizy.

Reguły alertów są określone przez następujące informacje:

- **Zaloguj się zapytania**.  Zapytanie, do którego jest uruchamiany za każdym razem, gdy generowane reguły alertów.  Rekordów zwróconych przez to zapytanie są używane do określenia, czy alert jest tworzony.
- **Przedział czasu**.  Określa przedział czasu dla zapytania.  Zapytanie zwraca tylko rekordy utworzone w tym zakresie czasu bieżącego.  Przedział czasu może mieć dowolną wartość od 5 minut do 1440 minut lub 24 godziny. Na przykład jeśli przedział czasu jest ustawiona na 60 minut, a godzina 13:15 będzie uruchomieniu kwerendy, jest zwracana tylko rekordy między 12:15:00 a 13:15:00.
- **Częstotliwość**.  Określa, jak często mają być uruchamiane zapytania. Może być dowolną wartość z zakresu od 5 minut do 24 godzin. Powinna być równa lub mniejsza niż przedział czasu.  Jeśli wartość jest większa niż przedział czasu, istnieje ryzyko rekordów jest pominięte.<br>Rozważmy na przykład okno czasu 30 minut i częstotliwość 60 minut.  Jeśli zapytanie jest uruchomione 1:00, zwraca rekordów między 12:30 i 1:00 PM.  Przy następnym uruchom zapytanie to 2:00 po zwróci rekordów między 1:30 i 2:00.  Nigdy nie będzie można obliczyć wszystkie rekordy między 1:00 i 1:30.
- **Próg**.  Wyniki wyszukiwania dziennika są oszacowywane, aby określić, czy można utworzyć alertu.  Próg jest różne dla różnych typów reguł alertów.

Każdej reguły alertu w analizy dzienników jest jednym z dwóch typów.  Każdy z tych typów jest szczegółowo opisane w kolejnych sekcjach.

- **[Liczba wyników](#number-of-results-alert-rules)**. Pojedynczy alert utworzony, jeśli liczba rekordów zwróconych przez wyszukiwanie dziennika przekracza podanej liczby.
- **[Metryki pomiaru](#metric-measurement-alert-rules)**.  Alert utworzony dla każdego obiektu w wynikach wyszukiwania dziennika z wartościami, które wykraczają poza określoną wartość progową.

Dostępne są następujące różnice między typami reguły alertów.

- ** Liczba wyników reguły alertów zawsze tworzy pojedynczy alert chwilę **metryki pomiaru** alertu zasada tworzy alert dla każdego obiektu, który przekracza wartość progową.
- **Liczba wyników** reguły alertów tworzą alert po przekroczeniu progu jeden raz. **Metryki pomiaru** reguły alertów można utworzyć alertu, po przekroczeniu progu określona liczba razy w określonym interwale.

## <a name="number-of-results-alert-rules"></a>Liczba wyników reguły alertów
**Liczba wyników** reguły alertów Utwórz pojedynczy alert, kiedy to liczba rekordów zwróconych przez zapytanie wyszukiwania przekraczają określony próg. Ten typ alertu jest idealne rozwiązanie w przypadku pracy z zdarzenia, takie jak dzienniki zdarzeń systemu Windows, Syslog odpowiedzi aplikacji sieci Web i niestandardowe dzienniki.  Możesz utworzyć alert podczas tworzenia pobiera zdarzeń określony błąd, lub wielu zdarzeń błędu są tworzone w ramach okna określony czas.

**Próg**: próg ** liczba wyników reguły alertu jest większa lub mniejsza od określonej wartości.  Jeśli liczba rekordów zwróconych przez wyszukiwanie dziennika zgodna te kryteria, tworzona jest alert.

Aby alertów na pojedyncze zdarzenie, ustaw liczbę wyników na wartość większą niż 0 i sprawdź, czy wystąpienie jednego zdarzenia, który został utworzony od czasu ostatniego uruchomienia zapytania. Niektóre aplikacje mogą rejestrować okazjonalne błąd, który nie należy koniecznie zgłosi alert.  Na przykład aplikacja może ponów próbę wykonania procesu, który utworzył zdarzenie błędu i powiodło się podczas następnego.  W takim przypadku nie możesz utworzyć alert, jeśli wiele zdarzeń są tworzone w ramach okna określonym czasie.  

W niektórych przypadkach warto utworzyć alert w przypadku braku zdarzeń.  Na przykład procesu mogą rejestrować regularnego zdarzenia w celu wskazania, że działa prawidłowo.  Jeżeli nie rejestrować jedno z tych zdarzeń w oknie określonym czasie, powinien zostać utworzony alert.  W takim przypadku należy ustawić wartość progową **mniej niż 1**.

### <a name="example"></a>Przykład
Rozważmy scenariusz, w którym chcesz wiedzieć, kiedy aplikacji opartych na sieci web zapewnia odpowiedzi dla użytkowników z kodem 500 (czyli) wewnętrzny błąd serwera. Należy utworzyć regułę alertu z następującymi szczegółami:  
**Zapytanie:** żądań | gdzie resultCode == "500"<br>
**Przedział czasu:** 30 minut<br>
**Alert częstotliwości:** pięć minut<br>
**Wartość progowa:** dużą niż 0<br>

Następnie alert może uruchomić kwerendę co 5 minut z 30 minut dane — do wyszukania wszystkich rekordów gdzie kod wyniku: 500. Jeśli zostanie znaleziony nawet jednego takiego zapisu, generowane alert i wyzwala Akcja skonfigurowana.

## <a name="metric-measurement-alert-rules"></a>Metryki pomiaru reguły alertów

**Metryki pomiaru** reguły alertów tworzyć alert dla każdego obiektu w zapytaniu z wartością, która przekracza określoną wartość progową.  Mają one następujące różnice różne **liczba wyników** reguły alertów.

**Funkcję agregacji**: Określa obliczeń, który jest wykonywane i potencjalnie liczbową pole do zagregowania.  Na przykład **count()** zwraca liczbę rekordów w zapytaniu, **avg(CounterValue)** zwraca średnią pole równowartości dla interwału.

> [!NOTE]

> Funkcji agregującej w zapytaniu muszą być o nazwie wywołuje: AggregatedValue i podaj wartość liczbową. 


**Pole grupy**: zostaje utworzony rekord z zagregowane wartości dla poszczególnych wystąpień tego pola, a alert jest generowany dla każdego.  Na przykład, jeśli chcesz generować alert dla każdego komputera, możesz użyć **przez komputer**   

> [!NOTE]

> Dla metryki pomiaru reguły alertów, które są oparte na usługi Application Insights można określić pole do grupowania danych. Aby to zrobić, użyj **agregacji na** opcja w definicji reguły.   

**Interwał**: Określa przedział czasu, w którym są agregowane.  Na przykład, jeśli określono **pięć minut**, może zostać utworzony rekord dla każdego wystąpienia pola grupy przez przedział czasu określony dla alertu agregowana co 5 minut.
> [!NOTE]
> Funkcja Kosza musi być używany w zapytaniu. Jak bin() może spowodować przedziały czasu nierówne — Alert będzie zamiast tego funkcja bin_at z właściwym czasie w czasie wykonywania, zapewnić wyniki ze stałym punktem

**Próg**: próg metryki pomiaru reguły alertów jest definiowana za pomocą wartości zagregowanej i liczbę naruszeń.  Dowolnego punktu danych w dzienniku wyszukiwania przekracza tę wartość, jest uznawane za naruszenia.  Jeśli liczba naruszeń w dla dowolnego obiektu w wynikach przekracza określoną wartość, alert zostanie utworzony dla tego obiektu.

#### <a name="example"></a>Przykład
Rozważmy scenariusz, w którym chce alert dowolnego komputera przekroczeniu użycie procesora przez 90% trzy razy ponad 30 minut.  Należy utworzyć regułę alertu z następującymi szczegółami:  

**Zapytanie:** wydajności | gdzie ObjectName == "Procesor" i CounterName == "% czasu procesora" | Podsumuj AggregatedValue = avg(CounterValue) przez bin (TimeGenerated, 5 m), komputer<br>
**Przedział czasu:** 30 minut<br>
**Alert częstotliwości:** pięć minut<br>
**Wartość agregacji:** dużą niż 90<br>
**Na podstawie wyzwalania alertu:** naruszeń łączna liczba jest większa niż 5<br>

Zapytanie spowodowałoby średnią wartość dla każdego komputera co 5 minut.  To zapytanie może działać co 5 minut przez dane zbierane przez poprzednie 30 minut.  Poniżej przedstawiono przykładowe dane na trzech komputerach.

![Przykładowe wyniki zapytania](../log-analytics/media/log-analytics-alerts/metrics-measurement-sample-graph.png)

W tym przykładzie oddzielne alerty zostałyby utworzone dla srv02 i srv03, ponieważ ich naruszenia progu 90% 3 razy za pośrednictwem przedział czasu.  Jeśli **na podstawie wyzwalania alertu:** zostały zmienione na **kolejno** , a następnie alert będzie można utworzyć tylko w przypadku srv03, ponieważ jego naruszenia progu dla trzech kolejnych próbek.


## <a name="next-steps"></a>Kolejne kroki
* Zrozumienie [Akcje elementu Webhook dla dziennika alertów](monitor-alerts-unified-log-webhook.md)
* [Omówienie usługi Azure alertów ](monitoring-overview-unified-alerts.md)
* Dowiedz się więcej o [przy użyciu usługi Azure alertów ](monitor-alerts-unified-usage.md)
* Dowiedz się więcej o [usługi Application Insights](../application-insights/app-insights-analytics.md)
* Dowiedz się więcej o [analizy dzienników](../log-analytics/log-analytics-overview.md).    
