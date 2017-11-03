---
title: "Nowości do prognozowania vehicle kondycji i wspierającym zwyczaje - Azure | Dokumentacja firmy Microsoft"
description: "Korzystanie z możliwości Cortana Intelligence, aby uzyskać wgląd w czasie rzeczywistym oraz predykcyjnej na vehicle kondycji i wspierającym zwyczaje."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 4050fdc2056df395bbcc37e3783f61eebd90f80a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Podręcznik dotyczący rozwiązań analizy telemetrii pojazdów: szczegółowe informacje na temat rozwiązania
To **menu** łącza do sekcji tego podręcznika dotyczącego: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Ta sekcja rozwija na każdym z etapów przedstawiono architekturę rozwiązania wraz z instrukcjami i wskaźniki do dostosowania. 

## <a name="data-sources"></a>Źródła danych
Rozwiązanie używa dwóch różnych źródeł danych:

* **Symulowane sygnały vehicle i danych diagnostycznych** i 
* **vehicle katalogu**

Symulator telematyki vehicle wchodzi w skład tego rozwiązania. Emituje informacje diagnostyczne, a sygnały odpowiadającego stanu mechanizm i kierowania wzorca w danym punkcie w czasie. Kliknij przycisk [symulatora telematyki Vehicle](http://go.microsoft.com/fwlink/?LinkId=717075) do pobrania **Vehicle telematyki symulatora rozwiązania Visual Studio** dostosowań, w zależności od wymagań. Katalog vehicle zawiera odwołanie do zestawu danych VIN do mapowania modelu.

![Symulator telematyki vehicle](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)

*Rysunek 1 — Vehicle telematyki symulatora*

Jest to formacie JSON zestawu danych, który zawiera następujące schematu.

| Kolumna | Opis | Wartości |
| --- | --- | --- |
| VIN |Losowo generowany numer identyfikacyjny |To są uzyskiwane ze wzorca listę 10 000 numery identyfikacyjne losowo generowany pojazdów. |
| Poza temperatury |Gdzie sterujące mechanizm temperatury na zewnątrz |Losowo generowany numer od 0 do 100 |
| Aparat temperatury |Aparat temperatury mechanizm |Losowo generowany numer od 0-500 |
| Szybkość |Szybkość aparatu, jaką mechanizm sterujące |Losowo generowany numer od 0 do 100 |
| Paliwa |Poziom paliwa mechanizm |Losowo generowany numer od 0 do 100 (procent poziomu paliwa wskazuje) |
| EngineOil |Poziom wydobycie ropy naftowej aparat mechanizm |Losowo generowany numer od 0 do 100 (procent poziomu wydobycie ropy naftowej aparat oznacza) |
| Opona wykorzystania |Wykorzystanie opona pojazdów. |Losowo generowany numer 50 0 (wskazuje procent poziomu wykorzystania opona) |
| Licznika |Odczytywanie licznika mechanizm |Losowo generowany numer od 0 200000 |
| Accelerator_pedal_position |Pozycja szkła mechanizm klawiszy skrótów |Losowo generowany numer od 0 do 100 (procent poziomu akceleratora wskazuje) |
| Parking_brake_status |Wskazuje, czy mechanizm jest stoi lub nie |Wartość PRAWDA lub FAŁSZ |
| Headlamp_status |Wskazuje, gdzie reflektor znajduje się na lub nie |Wartość PRAWDA lub FAŁSZ |
| Brake_pedal_status |Wskazuje, czy pedału hamulca zostanie naciśnięty, czy nie |Wartość PRAWDA lub FAŁSZ |
| Transmission_gear_position |Pozycja koło zębate transmisji mechanizm |Stan: najpierw drugie, trzecie czwarty, piąte, szóstego, siódme, kolejnego, ósmego |
| Ignition_status |Wskazuje, czy mechanizm jest uruchomiona lub zatrzymana |Wartość PRAWDA lub FAŁSZ |
| Windshield_wiper_status |Wskazuje, czy Wycieraczka szyby jest wyłączone lub nie |Wartość PRAWDA lub FAŁSZ |
| ABS |Wskazuje, czy ABS prowadzi lub nie |Wartość PRAWDA lub FAŁSZ |
| Znacznik czasu |Sygnatura czasowa po utworzeniu punktu danych. |Date |
| Miasto |Lokalizacja mechanizm |4 miejscowości, w tym rozwiązaniu: Bellevue, Redmond, Sammamish, Seattle |

Zestaw danych odwołania modelu vehicle zawiera VIN do mapowania modelu. 

| VIN | Model |
| --- | --- |
| FHL3O1SA4IEHB4WU1 |Limuzyna |
| 8J0U8XCPRGW4Z3NQE |Połączenie hybrydowe |
| WORG68Z2PLTNZDBI7 |Rodziny zamknięte |
| JTHMYHQTEPP4WBMRN |Limuzyna |
| W9FTHG27LZN1YWO0Y |Połączenie hybrydowe |
| MHTP9N792PHK08WJM |Rodziny zamknięte |
| EI4QXI2AXVQQING4I |Limuzyna |
| 5KKR2VB4WHQH97PF8 |Połączenie hybrydowe |
| W9NSZ423XZHAONYXB |Rodziny zamknięte |
| 26WJSGHX4MA5ROHNL |Możliwe do przekonwertowania |
| GHLUB6ONKMOSI7E77 |Kombi |
| 9C2RHVRVLMEJDBXLP |Samochód kompaktowy |
| BRNHVMZOUJ6EOCP32 |Mała SUV |
| VCYVW0WUZNBTM594J |Samochodu sportowych |
| HNVCE6YFZSA5M82NY |Średnia SUV |
| 4R30FOR7NUOBL05GJ |Kombi |
| WYNIIY42VKV6OQS1J |Duże SUV |
| 8Y5QKG27QET1RBK7I |Duże SUV |
| DF6OX2WSRA6511BVG |Coupe |
| Z2EOZWZBXAEW3E60T |Limuzyna |
| M4TV6IEALD5QDS3IR |Połączenie hybrydowe |
| VHRA1Y2TGTA84F00H |Rodziny zamknięte |
| R0JAUHT1L1R3BIKI0 |Limuzyna |
| 9230C202Z60XX84AU |Połączenie hybrydowe |
| T8DNDN5UDCWL7M72H |Rodziny zamknięte |
| 4WPYRUZII5YV7YA42 |Limuzyna |
| D1ZVY26UV2BFGHZNO |Połączenie hybrydowe |
| XUF99EW9OIQOMV7Q7 |Rodziny zamknięte |
| 8OMCL3LGI7XNCC21U |Możliwe do przekonwertowania |
| ……. | |

### <a name="references"></a>Dokumentacja
[Rozwiązanie programu Visual Studio symulatora telematyki vehicle](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Centrum zdarzeń platformy Azure](https://azure.microsoft.com/services/event-hubs/)

[Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)

## <a name="ingestion"></a>Wprowadzanie
Kombinacje Azure Event Hubs, Stream Analytics i fabryki danych można wykorzystać do pozyskiwania sygnały vehicle, zdarzeń diagnostycznych i w czasie rzeczywistym i partii analytics. Wszystkie te składniki są tworzone i skonfigurowany jako część wdrożenia rozwiązania. 

### <a name="real-time-analysis"></a>Analiza w czasie rzeczywistym
Zdarzenia generowane przez symulator telematyki Vehicle są publikowane w Centrum zdarzeń przy użyciu zestawu SDK Centrum zdarzeń. Zadanie Stream Analytics wysyła strumień te zdarzenia z Centrum zdarzeń i przetwarza dane w czasie rzeczywistym do analizy kondycji pojazdów. 

![Pulpit nawigacyjny Centrum zdarzeń](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

*Rysunek 4 - pulpit nawigacyjny Centrum zdarzeń*

![Dane przetwarzania zadania usługi analiza strumienia](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Rysunek 5. zadania usługi analiza strumienia przetwarzania danych*

Zadanie Stream Analytics.

* wysyła strumień danych z Centrum zdarzeń 
* wykonuje sprzężenie z mapowania vehicle VIN na odpowiedni model danych źródłowych 
* są utrwalane w magazynie obiektów blob platformy Azure dla analizach wsadowych sformatowanego. 

Następujące zapytanie Stream Analytics jest używany do utrwalenia danych do magazynu obiektów blob platformy Azure. 

![Zapytanie dotyczące zadań analizy strumienia dla wprowadzanie danych](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Rysunek 6 - analiza strumienia zadania zapytania na wprowadzanie danych*

### <a name="batch-analysis"></a>Analiza partii
Możemy również generowania woluminów sygnały symulowane vehicle i diagnostycznych zestawu danych dla bardziej zaawansowane funkcje analizy partii. Jest to wymagane, aby zapewnić objętość danych reprezentatywnych dla przetwarzania wsadowego. W tym celu potoku o nazwie "PrepareSampleDataPipeline" w przepływie pracy z fabryką danych Azure jest używany do generowania wartość jednego roku sygnały symulowane vehicle i zestawu danych diagnostycznych. Kliknij przycisk [działania niestandardowego fabryki danych](http://go.microsoft.com/fwlink/?LinkId=717077) do pobrania działania DotNet niestandardowego fabryki danych rozwiązania Visual Studio dla dostosowania, w zależności od wymagań. 

![Przygotowanie przykładowych danych do przepływu pracy przetwarzania wsadowego](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Rysunek 7 — przygotowanie przykładowych danych do przepływu pracy przetwarzania wsadowego*

Potok składa się z niestandardowych .net ADF działania, Pokaż tutaj:

![Działanie PrepareSampleDataPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Rysunek 8 - PrepareSampleDataPipeline*

Po potoku wykonuje pomyślnie i zestawu danych "RawCarEventsTable" jest oznaczona jako "Gotowe", jednego roku warto symulowane vehicle sygnałów oraz diagnostyki danych są tworzone. Zostanie wyświetlony następujący folder i plik utworzony na koncie magazynu w kontenerze "connectedcar":

![Dane wyjściowe PrepareSampleDataPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Rysunek 9 - PrepareSampleDataPipeline danych wyjściowych*

### <a name="references"></a>Dokumentacja
[Azure SDK Centrum zdarzeń pozyskiwania strumieni](../../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Możliwości przenoszenia danych fabryki danych platformy Azure](../../data-factory/v1/data-factory-data-movement-activities.md)
[działania DotNet fabryki danych Azure](../../data-factory/v1/data-factory-use-custom-activities.md)

[Rozwiązanie programu visual studio działania DotNet fabryki danych Azure przygotowania przykładowe dane](http://go.microsoft.com/fwlink/?LinkId=717077) 

## <a name="partition-the-dataset"></a>Partycja zestawu danych
Sygnały raw vehicle częściową strukturą i zestawu danych diagnostycznych są dzielone w kroku przygotowania danych do formatu rok i miesiąc. Zamienia kwerend większą wydajność i skalowalność magazynu długoterminowego jako pierwsze konto się zapełni, włączając błędów w tryb failover z jednego obiektu blob konta do następnego tej partycji. 

>[!NOTE] 
>Ten krok w rozwiązaniu ma zastosowanie tylko do przetwarzania wsadowego.

Wejście i wyjście danych zarządzania danymi:

* **Dane wyjściowe** (etykietą *PartitionedCarEventsTable*) ma być przechowywane przez dłuższy czas jako podstawowych / "rawest" formularza danych odbiorcy "Data Lake". 
* **Dane wejściowe** do tego potoku będzie zazwyczaj odrzucone jako danych wyjściowych ma pełnej rozdzielczości w danych wejściowych — wystarczy znajduje (podzielona na partycje) lepiej dla dalszego wykorzystania.

![Przepływ pracy zdarzenia samochodu partycji](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)

*Rysunek 10 — przepływ pracy zdarzenia samochodu partycji*

Dane pierwotne jest podzielona na partycje przy użyciu działania Hive HDInsight w "PartitionCarEventsPipeline". Przykładowe dane generowane w kroku 1 rok partycjonowanego rok i miesiąc. Partycje są używane do generowania sygnałów vehicle i danych diagnostycznych dla każdego miesiąca roku (całkowita liczba partycji 12). 

![Działanie PartitionCarEventsPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)

*Rysunek 11 — PartitionCarEventsPipeline*

***Skryptu PartitionConnectedCarEvents Hive***

Poniższy skrypt Hive o nazwie "partitioncarevents.hql" służy do partycjonowania i znajduje się w folderze "\demo\src\connectedcar\scripts" pobrany zip. 
    
    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string,
                YearNo                             int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

Po pomyślnym wykonaniu potoku zapoznaj się następujące partycje, które są generowane na koncie magazynu w kontenerze "connectedcar".

![Dane wyjściowe podzielonym na partycje](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

*Rysunek 12 — partycjonowanej danych wyjściowych*

Dane są zoptymalizowane pod, jest bardziej łatwe w zarządzaniu i gotowa do dalszego przetwarzania, aby uzyskać wgląd w partii sformatowanego. 

## <a name="data-analysis"></a>Analiza danych
W tej sekcji zobacz temat jak połączyć usługi Azure Stream Analytics, usługi Azure Machine Learning fabryki danych Azure i usłudze Azure HDInsight dla zaawansowanej, zaawansowane analizy na vehicle kondycji i wspierającym zwyczaje. Istnieją trzy podpunkty tutaj:

1. **Machine Learning**: ta podsekcja zawiera informacje na temat eksperymentu wykrywania anomalii, która użyliśmy w tym rozwiązaniu do prognozowania pojazdów wymagających obsługi konserwacji i pojazdów wymagających odwołania z powodu problemów bezpieczeństwa.
2. **W czasie rzeczywistym analizy**: ta podsekcja zawiera informacje dotyczące analiz w czasie rzeczywistym za pomocą język zapytań usługi analiza strumienia i operacyjnych eksperymentu uczenia maszynowego w czasie rzeczywistym za pomocą aplikacji niestandardowej.
3. **Partii analizy**: ta podsekcja zawiera informacje dotyczące, przekształcanie i przetwarzania wsadowego danych przy użyciu usługi Azure HDInsight i usługi Azure Machine Learning operationalized przez fabryki danych Azure.

### <a name="machine-learning"></a>Usługa Machine Learning
Naszym celem jest do prognozowania pojazdów, które wymagają konserwacji lub wycofania oparte na niektórych statystyki kondycji. Firma Microsoft wprowadzać następujące założenia

* Jeśli jeden z następujących trzech warunków są spełnione, wymagają pojazdów **obsługi konserwacji**:
  
  * Brakuje opona wykorzystania
  * Brakuje wydobycie ropy naftowej aparatu
  * Aparat temperatury jest wysoka
* Jeśli jeden z następujących warunków jest spełniony, może być pojazdów **problem bezpieczeństwa** i wymagają **odwołania**:
  
  * Aparat temperatury jest wysoka, ale brakuje poza temperatury
  * Aparat temperatury jest niska, ale poza temperatury jest wysoka

Na podstawie poprzedniego wymagań, został utworzony dwóch osobnych modeli do wykrywania anomalii: jeden vehicle obsługi wykrywania i jeden do wykrywania odwołania pojazdów. W obu tych modeli wbudowanych algorytm analizy składnik główny (PCA) służy do wykrywania anomalii. 

**Model wykrywania konserwacji**

Jeśli jeden z trzech wskaźniki opona wykorzystania, aparat wydobycie ropy naftowej lub temperatury aparat — spełniał stanu odpowiednich, model wykrywania konserwacji raporty anomalii. W związku z tym tylko należy wziąć pod uwagę następujące trzy zmienne podczas konstruowania modelu. W naszym eksperymentu w usłudze Azure Machine Learning, najpierw używamy **Select Columns in Dataset** modułu, aby wyodrębnić te trzy zmienne. Następnie używamy modułu wykrywania anomalii oparte na PCA do tworzenia modelu wykrywania anomalii. 

Podmiot zabezpieczeń składnika analiza (PCA) to technikę ustalonych w uczeniu maszynowym, który można zastosować funkcji wyboru cech, klasyfikacji i wykrywania anomalii. PCA Konwertuje zestaw przypadku zawierającego prawdopodobnie skorelowane zmienne, do wartości o nazwie główne składniki. Klucza pomysł na podstawie PCA modelowania jest danych projektu na małe wymiarowej miejsca, aby funkcje i anomalie, można łatwiej zidentyfikować.

Dla każdego nowego danych wejściowych do modelu wykrywania wykrywanie anomalii najpierw oblicza swojej projekcji na eigenvectors, a następnie oblicza błąd znormalizowane odbudowy. Ten błąd znormalizowane jest wynik anomalii. Im wyższa błąd, więcej nietypowych wystąpienie jest. 

Problem wykrywania konserwacji każdy rekord jest uznawana za ustalony punktu 3-wymiarową przestrzeni opona wykorzystania, aparat wydobycie ropy naftowej i temperatury aparat współrzędnych. Aby przechwytywać anomalie te, firma Microsoft project oryginalnych danych w przestrzeni trójwymiarowej 3 na 2-wymiarową miejsca, przy użyciu PCA. W związku z tym firma Microsoft ustaw dla parametru liczby składników do użycia w PCA 2. Ten parametr odgrywa ważną rolę w stosowaniu wykrywania anomalii oparte na PCA. Po zakończeniu operacji przewidywania danych przy użyciu PCA możemy łatwiej ustalić tych nieprawidłowości.

**Model wykrywania anomalii odwołania** w modelu wykrywania anomalii odwołania używamy Select Columns in Dataset i anomalii oparte na PCA modułów wykrywania w podobny sposób. W szczególności firma Microsoft najpierw wyodrębnić, trzy - poza temperatury i szybkość temperatury aparat — zmienne przy użyciu **Select Columns in Dataset** modułu. Możemy także zmiennej szybkości ponieważ temperatury aparat zwykle są korelowane szybkości. Następnie używamy modułu wykrywania anomalii oparte na PCA do projektu danych z 3-wymiarową miejsca na miejsce 2-wymiarowej. Są spełnione kryteria odwołania i mechanizm wymaga odwołania gdy aparat temperatury i poza temperatury wysokiej negatywnie skorelowane. Za pomocą algorytmu wykrywania anomalii oparte na PCA, możemy przechwytywania po wykonaniu PCA nieprawidłowości. 

Podczas uczenia modelu albo, musimy użyj normalnych danych, które nie wymaga jako dane wejściowe do uczenia modelu wykrywania anomalii oparte na PCA konserwacji lub wycofania. W eksperymencie oceniania używamy wykrywania anomalii uczonego modelu do wykrywania, czy mechanizm wymaga konserwacji lub wycofania. 

### <a name="real-time-analysis"></a>Analiza w czasie rzeczywistym
Następujące zapytanie SQL Stream Analytics jest używany do pobierania średnią wszystkie parametry ważny, takie jak prędkość, poziom paliwa temperatury aparatu, odczytu licznika, opona wykorzystania, aparatu wydobycie ropy naftowej i inne. Średnie są używane do wykrywania anomalii, alerty, wystawiania i określają warunki ogólnej kondycji pojazdów obsługiwane w określonym regionie i następnie skorelowany demograficznych. 

![Stream Analytics zapytania do przetworzenia w czasie rzeczywistym](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

*Rysunek 13 — zapytań usługi Stream Analytics do przetworzenia w czasie rzeczywistym*

Wszystkie wartości średnie są obliczane przez TumblingWindow 3 sekundy. Użyto TubmlingWindow w takim przypadku ponieważ wymagamy przedziały czasu nienakładający, jak i ciągłe. 

Aby dowiedzieć się więcej na temat wszystkie funkcje "Okien" Azure Stream Analytics, kliknij przycisk [okien (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**Prognozowanie w czasie rzeczywistym**

Aplikacja jest zawarte w ramach rozwiązania, który umożliwia operacjonalizację modelu uczenia maszynowego w czasie rzeczywistym. Ta aplikacja o nazwie "RealTimeDashboardApp" jest utworzony i skonfigurowany jako część wdrożenia rozwiązania. Aplikacja wykonuje następujące działania:

1. Nasłuchuje Centrum zdarzeń wystąpienia których Stream Analytics jest publikowanie zdarzenia we wzorcu stale. ![Stream Analytics zapytania do publikowania danych](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *rysunek 14 — zapytań usługi Stream Analytics do publikowania danych wyjściowych Centrum zdarzeń wystąpienia* 
2. Dla każdego zdarzenia, które otrzymuje tej aplikacji: 
   
   * Przetwarza dane przy użyciu punktu końcowego Machine Learning żądań i odpowiedzi oceniania (RR). Punkt końcowy rekordy zasobów są automatycznie publikowane jako część wdrożenia.
   * Dane wyjściowe rekordy zasobów jest opublikowana w zestawie danych usługi Power BI za pomocą wypychania interfejsów API.

Ten wzorzec jest również dotyczy scenariuszy, w których chcesz zintegrować aplikacji biznesowych (LoB) wraz z przepływem analiz w czasie rzeczywistym dla scenariuszy, takich jak alerty, powiadomienia i wiadomości.

Kliknij przycisk [pobierania RealtimeDashboardApp](http://go.microsoft.com/fwlink/?LinkId=717078) Aby pobrać rozwiązanie RealtimeDashboardApp Visual Studio dostosowań. 

**Do wykonania aplikacji pulpitu nawigacyjnego w czasie rzeczywistym**
1. Wyodrębnij i Zapisz lokalnie ![folderu RealtimeDashboardApp](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *rysunek 16 – RealtimeDashboardApp folderu*  
2. Uruchom aplikację RealtimeDashboardApp.exe
3. Podaj prawidłowe poświadczenia usługi Power BI, zaloguj się i kliknij przycisk Akceptuj ![W czasie rzeczywistym pulpit nawigacyjny aplikacji logowania do usługi Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) ![Zakończ w czasie rzeczywistym pulpit nawigacyjny aplikacji logowania do usługi Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Rysunek 17 — RealtimeDashboardApp: Logowania do usługi Power BI*

>[!NOTE] 
>Jeśli chcesz opróżnić zestawu danych usługi Power BI, wykonaj RealtimeDashboardApp z parametrem "flushdata": 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a>Analiza partii
Celem jest pokazanie, jak silniki Contoso korzysta z możliwości obliczeń platformy Azure o danych big data, aby uzyskać szczegółowe, bogate informacje na wzorzec, zachowanie użycia i kondycji pojazdów. Dzięki temu możliwe:

* Poprawy obsługi klienta i zapewnić ich tańsze zapewniając wgląd na zwyczaje i wydajne zachowania pobudzenie paliwa
* Zapoznaj się z wyprzedzeniem klientów i ich pobudzenie patters regulują decyzje biznesowe i podaj najciekawszych klasy produktów i usług

W tym rozwiązaniu możemy przeznaczonych następujące metryki:

1. **Agresywna zwiększają zachowanie**: identyfikuje trend modeli, lokalizacje pobudzenie warunki i czas roku, aby uzyskać wgląd w bliskiej schematy. Silniki contoso można używać tych insights kampanii marketingowych, nowe funkcje spersonalizowane i ubezpieczenia opartej na użyciu.
2. **Paliwa wydajne działanie pobudzenie**: identyfikuje trend modeli, lokalizacje pobudzenie warunki i czas roku, aby uzyskać szczegółowe informacje na efektywne schematy paliwa. Silniki contoso można używać tych insights kampanii, zwiększają nowe funkcje i aktywne podlegające sterowniki kosztów przyjazną zwyczaje pobudzenie wejścia w życie i środowiska. 
3. **Odwołaj modele**: identyfikuje modele wymagających odwołań przez operacyjnych eksperymentu uczenia maszynowego wykrywania anomalii

Oto do szczegółów każdego z tych metryk

**Agresywne pobudzenie wzorca**

Sygnały vehicle podzielonym na partycje i danych diagnostycznych są przetwarzane w potoku o nazwie "AggresiveDrivingPatternPipeline" przy użyciu Hive w celu określenia modele, lokalizacji vehicle, warunki jazdy i inne parametry który wykazuje zwiększają aktywnego wzorzec.

![Agresywne pobudzenie przepływu pracy wzorzec](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*rysunek 18 — agresywne pobudzenie przepływu pracy wzorca*


***Agresywne kierowania zapytań Hive wzorca***

Skryptu Hive o nazwie "aggresivedriving.hql" używany do analizowania aktywnego wzorca warunku jazdy znajduje się w folderze "\demo\src\connectedcar\scripts" zip pobranego. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                   vin                         string, 
                model                        string,
                timestamp                    string,
                city                        string,
                speed                          string,
                transmission_gear_position    string,
                brake_pedal_status            string,
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'


Kombinacja transmisji koło zębate pozycji w vehicle, stan szkła hamulca i szybkość używa do wykrywania reckless/agresywne działanie pobudzenie hamowania wzorzec dużą prędkością. 

Po pomyślnym wykonaniu potoku zapoznaj się następujące partycje, które są generowane na koncie magazynu w kontenerze "connectedcar".

![Dane wyjściowe AggressiveDrivingPatternPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Rysunek 19 — AggressiveDrivingPatternPipeline danych wyjściowych*

**Efektywne wzorzec pobudzenie paliwa**

Sygnały vehicle podzielonym na partycje i danych diagnostycznych są przetwarzane w potoku o nazwie "FuelEfficientDrivingPatternPipeline". Gałąź służy do określenia modele, lokalizacji vehicle, warunki jazdy i inne właściwości, które wykazują paliwa wydajne pobudzenie wzorzec.

![Wzorzec pobudzenie obniżające zużycie paliwa](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Rysunek 20 – obniżające zużycie paliwa pobudzenie przepływu pracy wzorca*

***Paliwa wydajne pobudzenie wzorzec zapytań Hive***

Skryptu Hive o nazwie "fuelefficientdriving.hql" używany do analizowania aktywnego wzorca warunku jazdy znajduje się w folderze "\demo\src\connectedcar\scripts" zip pobranego. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                   vin                         string, 
                model                        string,
                   city                        string,
                speed                          string,
                transmission_gear_position    string,                
                brake_pedal_status            string,            
                accelerator_pedal_position    string,                             
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


Używa kombinacji pozycji koło zębate transmisji vehicle w, stan szkła hamulca, szybkości, akceleratora pedał wykryć paliwa wydajne pobudzenie działanie przyspieszenie hamowania i szybkości wzorce. 

Po pomyślnym wykonaniu potoku zapoznaj się następujące partycje, które są generowane na koncie magazynu w kontenerze "connectedcar".

![Dane wyjściowe FuelEfficientDrivingPatternPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Rysunek 21 — FuelEfficientDrivingPatternPipeline danych wyjściowych*

**Operacje przewidywania dla odwołania**

Eksperymentu uczenia maszynowego jest udostępniane i opublikowane jako usługi sieci web jako część wdrożenia rozwiązania. Punkt końcowy oceniania partii jest wykorzystywana w tym przepływie pracy, zarejestrowany jako usługa połączone fabryki danych i operationalized przy użyciu danych fabryki działanie wsadowego oceniania.

![Punkt końcowy Machine Learning](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

*Rysunek 22 — punktu końcowego uczenia maszynowego zarejestrowany jako połączonej usługi z fabryki danych*

Zarejestrowanej połączonej usługi jest używany w DetectAnomalyPipeline punkty danych za pomocą modelu wykrywania anomalii. 

![Learning działanie wsadowego oceniania w fabryce danych komputera](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png) 

*Rysunek 23 — działanie usługi Azure Machine Learning wsadowego oceniania z fabryki danych* 

Istnieje kilka czynności wykonywanych w tym potoku w celu przygotowania danych, dzięki czemu mogą być operationalized z wsadowego oceniania usługi sieci web. 

![DetectAnomalyPipeline do prognozowania pojazdów wymagających odwołań](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png) 

*Rysunek 24 — DetectAnomalyPipeline do prognozowania pojazdów wymagających odwołań* 

***Zapytanie Hive wykrywania anomalii***

Po zakończeniu oceny działanie HDInsight służy do przetwarzania i agregowanie danych, które są skategoryzowane jako anomalii przez model z wynikiem prawdopodobieństwo: 0,60 lub nowszej.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                            string,
                model                        string,
                gendate                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                fuel                        string,
                engineoil                    string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position    string,
                parking_brake_status        string,
                headlamp_status                string,
                brake_pedal_status            string,
                transmission_gear_position    string,
                ignition_status                string,
                windshield_wiper_status        string,
                abs                          string,
                maintenanceLabel            string,
                maintenanceProbability        string,
                RecallLabel                    string,
                RecallProbability            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                            string,
                model                        string,
                city                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                Year                        string,
                Month                        string                

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Po pomyślnym wykonaniu potoku zapoznaj się następujące partycje, które są generowane na koncie magazynu w kontenerze "connectedcar".

![Dane wyjściowe DetectAnomalyPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Rysunek 25 — DetectAnomalyPipeline danych wyjściowych*

## <a name="publish"></a>Publikowanie

### <a name="real-time-analysis"></a>Analiza w czasie rzeczywistym
Jeden z zapytania w zadaniu Stream Analytics publikuje zdarzenia w danych wyjściowych Centrum zdarzeń wystąpienia. 

![Publikuje dane wyjściowe zadania usługi analiza strumienia wystąpienia Centrum zdarzeń](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Rysunek 26 — publikuje dane wyjściowe zadania usługi analiza strumienia wystąpienia Centrum zdarzeń*

![Stream Analytics zapytania do publikowania danych wyjściowych Centrum zdarzeń wystąpienia](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Rysunek 27 — zapytań usługi Stream Analytics do publikowania danych wyjściowych Centrum zdarzeń wystąpienia*

Ten strumień zdarzeń jest używany przez RealTimeDashboardApp zawarty w rozwiązaniu. Ta aplikacja korzysta z usługą sieci web Machine Learning żądań i odpowiedzi w czasie rzeczywistym oceniania i publikuje dane wynikowe z zestawem danych usługi Power BI zużycia. 

### <a name="batch-analysis"></a>Analiza partii
Wyniki partii i przetwarzanie w czasie rzeczywistym są publikowane w tabelach bazy danych SQL Azure zużycia. Azure SQL Server, bazy danych i tabele są tworzone automatycznie w ramach skrypt instalacyjny. 

![Kopiuj wyniki do przepływu pracy składnicy danych przetwarzania wsadowego](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Rysunek 28 — kopiowania wyniki do przepływu pracy składnicy danych przetwarzania wsadowego*

![Zadania usługi analiza strumienia publikuje do składnicy danych](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Rysunek 29 — analiza strumienia zadania publikuje do składnicy danych*

![Ustawienie składnicy danych w zadaniu Stream Analytics](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Rysunek 30 – składnicy danych ustawienie w zadaniu Stream Analytics*

## <a name="consume"></a>Używanie
Usługi Power BI pozwala to rozwiązanie sformatowanego pulpitu nawigacyjnego i analizy predykcyjnej wizualizacji danych w czasie rzeczywistym. 

Kliknij tutaj, aby uzyskać szczegółowe instrukcje dotyczące konfigurowania raportów usługi Power BI i pulpitu nawigacyjnego. Końcowy pulpitu nawigacyjnego wygląda następująco:

![Pulpit nawigacyjny programu Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

*Rysunek 31 - pulpitu nawigacyjnego programu Power BI*

## <a name="summary"></a>Podsumowanie
Ten dokument zawiera szczegółowe przechodzenia z rozwiązania analizy Telemetrii pojazdów. Ten wzorzec architektury lambda, dla których są wyświetlane w czasie rzeczywistym i partii analizy w usłudze prognoz i akcji. Ten wzorzec dotyczy szeroką gamę przypadków użycia, które wymagają aktywnej ścieżki (w czasie rzeczywistym) i analiza zimnych ścieżki (partii). 

