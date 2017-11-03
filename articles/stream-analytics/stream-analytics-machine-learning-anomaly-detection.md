---
title: "Wykrywanie anomalii w przewodniku użytkowania platformy Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft"
description: "Za pomocą usługi stream analytics i wykrywania anomalii uczenia maszynowego."
services: stream-analytics
documentationcenter: 
author: dubansal
manager: jhubbard
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: dubansal
ms.openlocfilehash: 43a2a9784668fad2aa5b1441cfd37751c0c240b6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="using-the-anomalydetection-operator"></a>Przy użyciu operatora ANOMALYDETECTION

> [!IMPORTANT]
> Ta funkcja jest dostępna w wersji zapoznawczej. Nie zaleca się użycie w środowisku produkcyjnym.

**ANOMALYDETECTION** operator może służyć do wykrywania anomalii w strumieni zdarzeń.
Na przykład powolne spadek wolnej pamięci przez dłuższy czas może wskazywać przeciek pamięci lub liczbę żądań usług sieci web, które są trwałe w zakresie może znacznie zwiększyć lub zmniejszyć.

Sprawdza następujące typy anomalii przez określony czas trwania:

- Zmiany poziomu dwukierunkowych
- Trend dodatnią powolne
- Powolne trend ujemna

Interwał czasu, w jakim Wstecz w historii z bieżącego zdarzenia musi być postrzegana jest ograniczony przy użyciu **LIMIT DURATION** klauzuli. **ANOMALYDETECTION** Opcjonalnie można ograniczyć do tylko zdarzenia, które odpowiadają niektórych właściwości lub warunku, za pomocą **podczas** klauzuli.

Opcjonalnie może przetwarzać grup zdarzeń klucz określony w oparciu o oddzielnie **PARTITION BY** klauzuli. Szkolenie i prognozowania występuje niezależnie w każdej partycji.

## <a name="syntax"></a>Składnia

`ANOMALYDETECTION(\<scalar_expression\>) OVER ([PARTITION BY \<partition key\>] LIMIT DURATION(\<unit\>, \<length\>) [WHEN boolean_expression])` 


## <a name="example-usage"></a>Przykład użycia

`SELECT id, val, ANOMALYDETECTION(val) OVER(PARTITION BY id LIMIT DURATION(hour, 1) WHEN id \> 100) FROM input`|


## <a name="arguments"></a>Argumenty

- **scalar_expression**

  Wyrażenie skalarne, w którym będzie można wykonać wykrywania anomalii. To wyrażenie typu float lub bigint, która zwraca pojedynczą wartość (skalarną). Wyrażenia z symbolami wieloznacznymi  **\***  jest niedozwolone. **scalar_expression** nie może zawierać inne funkcje analityczne lub funkcji zewnętrznych.

- **Za pośrednictwem (limit_duration_clause [partition_by_clause] [when_clause])**

- **partition_by_clause** 

  `PARTITION BY \<partition key\>` Klauzuli dzieli uczenie i szkolenia w osobnych partycji. Innymi słowy, oddzielne modelu będzie służyć za wartości `\<partition key\>` i tylko zdarzenia z daną wartością mają być używane do nauki i szkolenia w tym modelu. Na przykład:

  `SELECT sensorId, reading, ANOMALYDETECTION(reading) OVER(PARTITION BY sensorId LIMIT DURATION(hour, 1)) FROM input`

  zostanie szkolenie i wynik odczytu względem innych odczyty tylko tej samej czujnika.

- **Klauzula limit_duration** czas trwania (\<jednostki\>, \<długość\>)

  Określa, ile historii z bieżącego zdarzenia jest rozpatrywany w **ANOMALYDETECTION** obliczeń. Zobacz DATEDIFF szczegółowy opis obsługiwanych jednostek i ich skrótów.

- **when_clause** 

  Określa warunek typu Boolean dla zdarzenia wziąć pod uwagę **ANOMALYDETECTION** obliczeń.

## <a name="return-types"></a>Zwracane typy

Funkcja zwraca rekord zawierający wszystkie trzy wyniki jako dane wyjściowe. Właściwości skojarzone z różnymi typami detektory anomalii są nazywane:

- BiLevelChangeScore
- SlowPosTrendScore
- SlowNegTrendScore

Aby wyodrębnić poszczególne wartości poza rekordu, użyj **funkcji GetRecordPropertyValue** funkcji. Na przykład:

`SELECT id, val FROM input WHERE (GetRecordPropertyValue(ANOMALYDETECTION(val) OVER(LIMIT DURATION(hour, 1)), 'BiLevelChangeScore')) \> 3.25` 


Anomalii określonego typu została wykryta, gdy jeden z tych wyników anomalii przekracza próg. Próg może być żadnych zmiennoprzecinkową numer punktu \>= 0. Próg wynosi zależności między czułość i zaufania. Na przykład niższego progu wprowadzić bardziej podatna na zmiany wykrywania i generowanie więcej alertów wyższa wartość progową można wprowadzić wykrywania mniej poufne i większa pewność, ale zamaskować niektórych anomalii. Wartość progowa dokładnie do użycia zależy od scenariusza. Nie ma żadnego limitu górnego, ale zalecane zakres jest 3,25 5.

## <a name="algorithm"></a>Algorytm

**ANOMALYDETECTION** używa przedłużanie semantyki okna, co oznacza, że wykonuje obliczenia na zdarzenie, które wprowadza funkcji i wynikiem jest generowany dla tego zdarzenia. Obliczenia jest oparta na Martingales zamienności, które działają przez sprawdzenie, czy rozkład wartości zdarzenia został zmieniony. Jeśli tak, Wykryto potencjalną anomalii. Zwrócony wynik jest wskaźnik poziomu zaufania z tym anomalii. Wewnętrzny optymalizacji **ANOMALYDETECTION** oblicza wynik anomalii zdarzenia na podstawie *d* do *2d* które zdarzenia, gdzie *d*jest wykrywania określonego rozmiaru okna.

**ANOMALYDETECTION** oczekuje serii godzina jest jednolita. Strumień zdarzeń można ujednolicone przez agregowanie za pośrednictwem wirowania lub Skaczące okno. W scenariuszach, w którym odstęp między zdarzeniami zawsze jest mniejszy niż okno agregacji okno wirowania jest wystarczające, aby wprowadzić uniform szeregów czasowych. Gdy przerwa może być większy, luk może zostać wypełniony powtarzając ostatnią wartość przy użyciu okna przeskoku. Oba te scenariusze są obsługiwane przez następującym przykładzie. Obecnie `FillInMissingValuesStep` krok nie może być pominięty. Nie ma tego kroku wystąpi błąd kompilacji.

## <a name="performance-guidance"></a>Wytyczne dotyczące wydajności

- Użyj 6 SU dla zadania. 
- Wysyłanie zdarzeń co najmniej 1 sekundę od siebie.
- Niepartycjonowany zapytania za pomocą **ANOMALYDETECTION** funkcja może wygenerować wyniki z opóźnieniem obliczeń około 25 MS średnia.
- Opóźnienie doświadczają partycjonowanej zapytania nieco zależy od liczby partycji, jak liczba obliczenia jest większa. Jednak opóźnienie nastąpi taka sama jak w przypadku niepartycjonowany porównywalne łączna liczba zdarzeń wszystkich partycji.
- Podczas odczytywania danych z systemem innym niż czasu rzeczywistego, szybko jest pozyskanych dużej ilości danych. Przetwarzanie tych danych jest obecnie znacznie niższej. Czas oczekiwania w takich scenariuszach znaleziono zwiększające liniowo do liczby punktów danych w oknie zamiast interwał rozmiar lub zdarzenia okna per se. Aby skrócić czas oczekiwania w przypadku się w czasie rzeczywistym, należy wziąć pod uwagę przy użyciu mniejszego rozmiaru okna. Alternatywnie należy rozważyć uruchomienie zadania od bieżącego czasu. Kilka przykładów opóźnienia w zapytaniu niepartycjonowany: 
    - 60 punktów danych w oknie wykrywania może spowodować opóźnienia 10 sekund o przepustowości 3 MB/s. 
    - 600 punktów danych opóźnienie może nawiązać połączenie o 80 sekund o przepustowości 0,4 MB/s.

## <a name="example"></a>Przykład

Następujące zapytanie może służyć do wyjściowego alert w przypadku wykrycia anomalii.
Jeśli strumień wejściowy nie jest jednolite, krok agregacji może pomóc przekształcania serii jednolita postać czasu. W przykładzie użyto **AVG** , ale określony typ agregacji jest zależny od scenariusza użytkownika. Ponadto po szeregów czasowych ma luki większe niż okno agregacji, nie będą żadne zdarzenia w szeregi czasowe do wykrywania anomalii wyzwalacza (zgodnie z harmonogramem przesuwanego okna semantyki). W związku z tym założeniu jednolitość przestaną działać po nadejściu następne zdarzenie. W takich sytuacjach należy sposób wypełniania luk w szeregu czasowym. Jednym z podejść możliwe ma mieć ostatnie zdarzenie co okno przeskoku, jak pokazano poniżej.

Nie należy pomijać wspomnianego przed `FillInMissingValuesStep` krok teraz. Pominięcie tego kroku wystąpi błąd kompilacji.

    WITH AggregationStep AS 
    (
         SELECT
               System.Timestamp as tumblingWindowEnd,

               AVG(value) as avgValue

         FROM input
         GROUP BY TumblingWindow(second, 5)
    ),

    FillInMissingValuesStep AS
    (
          SELECT
                System.Timestamp AS hoppingWindowEnd,

                TopOne() OVER (ORDER BY tumblingWindowEnd DESC) AS lastEvent

         FROM AggregationStep
         GROUP BY HOPPINGWINDOW(second, 300, 5)

    ),

    AnomalyDetectionStep AS
    (

          SELECT
                hoppingWindowEnd,
                lastEvent.tumblingWindowEnd as lastTumblingWindowEnd,
                lastEvent.avgValue as lastEventAvgValue,
                system.timestamp as anomalyDetectionStepTimestamp,

                ANOMALYDETECTION(lastEvent.avgValue) OVER (LIMIT DURATION(hour, 1)) as
                scores

          FROM FillInMissingValuesStep
    )

    SELECT
          alert = 1,
          hoppingWindowEnd,
          lastTumblingWindowEnd,
          lastEventAvgValue,
          anomalyDetectionStepTimestamp,
          scores

    INTO output

    FROM AnomalyDetectionStep

    WHERE

        CAST(GetRecordPropertyValue(scores, 'BiLevelChangeScore') as float) \>= 3.25

        OR CAST(GetRecordPropertyValue(scores, 'SlowPosTrendScore') as float) \>=
        3.25

       OR CAST(GetRecordPropertyValue(scores, 'SlowNegTrendScore') as float) \>=
       3.25

## <a name="references"></a>Dokumentacja

* [Wykrywanie anomalii — przy użyciu maszyny Learning w celu wykrycia nieprawidłowości w czasie danych w serii.](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/)
* [Wykrywanie anomalii interfejsu API uczenia maszynowego](https://docs.microsoft.com/en-gb/azure/machine-learning/machine-learning-apps-anomaly-detection-api)
* [Wykrywanie anomalii serii czasu](https://msdn.microsoft.com/library/azure/mt775197.aspx)

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej
Aby uzyskać dodatkową pomoc, spróbuj naszych [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Następne kroki

* [Wprowadzenie do usługi Azure Stream Analytics](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics (Rozpoczynanie pracy z usługą Azure Stream Analytics)](stream-analytics-real-time-fraud-detection.md)
* [Scale Azure Stream Analytics jobs (Skalowanie zadań usługi Azure Stream Analytics)](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Query Language Reference (Dokumentacja dotycząca języka zapytań usługi Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API Reference (Dokumentacja interfejsu API REST zarządzania usługą Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

