---
title: "Eksplorowanie danych klastra usługi Hadoop i tworzenia modeli w usłudze Azure Machine Learning | Dokumentacja firmy Microsoft"
description: "Przy użyciu procesu nauki danych zespołu scenariusz end-to-end wykorzystujących klastra usługi Hadoop w usłudze HDInsight do tworzenia i wdrażania modelu za pomocą publicznie dostępnych zestawu danych."
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e9e76c91-d0f6-483d-bae7-2d3157b86aa0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 1be39ab258235740c7e0875a5c0c29ee4a665a71
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# <a name="the-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>Proces nauki danych zespołu w działaniu: klastrów użycia usługi Azure HDInsight Hadoop
W tym przewodniku używamy [zespołu danych nauki procesu (TDSP)](overview.md) w scenariuszu end-to-end przy użyciu [klastra usługi Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) do przechowywania, Eksploruj i odtwarzania danych z publicznie dostępnych funkcji [rund taksówki NYC](http://www.andresmh.com/nyctaxitrips/) zestawu danych i w dół przykładowe dane. Modele danych są tworzone przy użyciu usługi Azure Machine Learning do obsługi binarnej i wieloklasowej klasyfikacji i regresji predykcyjnej zadania.

Aby uzyskać wskazówki, pokazujący sposób obsługi większych zestawu danych (1 terabajt) dla podobny scenariusz using klastrów HDInsight Hadoop do przetwarzania danych, zobacz [zespołu danych nauki proces — za pomocą usługi Azure HDInsight klastrów platformy Hadoop w zestawie danych 1 TB](hive-criteo-walkthrough.md).

Istnieje również możliwość Notes IPython służy do wykonywania zadań przedstawione wskazówki przy użyciu zestawu danych 1 TB. Aby skorzystać z tej metody użytkownicy powinni skontaktować [wskazówki Criteo za pomocą połączenia ODBC programu Hive](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) tematu.

## <a name="dataset"></a>Opis elementu Dataset rund taksówki NYC
Dane podróży taksówki NYC to około 20GB skompresowanego wartości rozdzielanych przecinkami (CSV) plików (~ 48GB nieskompresowane), składającej się z więcej niż 173 milionów poszczególnych podróży i opłaty płatnej dla każdej podróży. Każdy rekord podróży obejmuje odbiór i przyjmowania lokalizacji i czasu, hack anonimowe (sterownik) numer licencji i numer Medalionu (taksówki jego unikatowy identyfikator). Dane obejmuje wszystkie rund w roku 2013 i jest dostępne w następujących dwóch zestawów danych dla każdego miesiąca:

1. Pliki CSV "trip_data" zawierają szczegóły podróży, takie jak liczba pasażerów, pobranie i punkty dropoff czas trwania podróży i długość podróży. Poniżej przedstawiono kilka przykładowych rekordów:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. Pliki CSV "trip_fare" zawierają szczegółowe informacje o klasie za każdym razem, takie jak typ płatności, kwota taryfy, przeciążenia i podatków, porady i przejazd i sumy płatnej. Poniżej przedstawiono kilka przykładowych rekordów:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Unikatowy klucz do przyłączenia podróży\_danych i podróży\_taryfy składa się z pola: Medalionu, hack\_licencji i pobrania\_daty/godziny.

Aby uzyskać wszystkie szczegóły dotyczące konkretnego podróży, jest wystarczające, aby dołączyć za pomocą trzech kluczy: "Medalionu", "hack\_licencji" i "podnoszenia\_datetime".

Gdy są przechowywane ich do tabele programu Hive wkrótce opisano niektóre szczegółowe dane.

## <a name="mltasks"></a>Przykłady prognozowania zadań
Gdy zbliża się dane, do określenia rodzaj prognoz, które mają być oparty na jego analizy pomaga wyjaśnienia zadania, które należy uwzględnić w procesie.
Poniżej przedstawiono trzy przykłady problemów prognozowania, które można rozwiązać w ramach tego przewodnika, w której skład opiera się na *Porada\_kwota*:

1. **Klasyfikacji binarnej**: prognozowania, czy etykietki został płatnej podróży, tj. *Porada\_kwota* większą niż $0 jest przykład dodatnią, podczas gdy *Porada\_kwota* $ 0 jest ujemny przykład.
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0
2. **Wieloklasowej klasyfikacji**: przewidywanie zakres kwoty Porada uregulowaniu płatności podróży. Możemy podzielić *Porada\_kwota* do pięciu bins lub klasy:
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0 and tip_amount <= $5
        Class 2: tip_amount > $5 and tip_amount <= $10
        Class 3: tip_amount > $10 and tip_amount <= $20
        Class 4: tip_amount > $20
3. **Zadanie regresji**: przewidywanie ilość Porada płatnej w podróży.  

## <a name="setup"></a>Konfigurowanie klastra usługi HDInsight Hadoop zaawansowana analityka
> [!NOTE]
> Jest to zazwyczaj **Admin** zadań.
> 
> 

Można skonfigurować środowiska platformy Azure zaawansowana analityka używającego klastra usługi HDInsight w trzy kroki:

1. [Utwórz konto magazynu](../../storage/common/storage-create-storage-account.md): to konto magazynu jest używany do przechowywania danych w magazynie obiektów Blob Azure. Dane używane w klastrach HDInsight również znajdują się tutaj.
2. [Dostosowywanie klastrów usługi Azure HDInsight Hadoop dla procesu zaawansowane analizy i technologii](customize-hadoop-cluster.md). Spowoduje to utworzenie Azure HDInsight Hadoop, klaster z 64-bitowych Anaconda Python 2.7 zainstalowane we wszystkich węzłach. Istnieją dwie ważne czynności umożliwiające pamiętać podczas dostosowywania z klastrem usługi HDInsight.
   
   * Pamiętaj, aby połączyć utworzony w kroku 1 z klastrem usługi HDInsight, podczas jej tworzenia konta magazynu. To konto magazynu jest używana do dostępu do danych jest przetwarzane w ramach klastra.
   * Po utworzeniu klastra, należy włączyć dostęp zdalny do węzła głównego klastra. Przejdź do **konfiguracji** i kliknij polecenie **Włącz zdalne**. Ten krok Określa poświadczenia użytkownika służące do logowania zdalnego.
3. [Utwórz obszar roboczy usługi Azure Machine Learning](../studio/create-workspace.md): to Azure Machine Learning obszaru roboczego jest używany do tworzenia modeli uczenia maszyny. To zadanie zostanie rozwiązana po zakończeniu Eksploracja danych początkowych i w dół próbkowania przy użyciu klastra usługi HDInsight.

## <a name="getdata"></a>Pobierz dane ze źródła publiczny
> [!NOTE]
> Jest to zazwyczaj **Admin** zadań.
> 
> 

Aby uzyskać [rund taksówki NYC](http://www.andresmh.com/nyctaxitrips/) zestawu danych z lokalizacji publicznej, możesz użyć dowolnej z metod opisanych w [przenoszenie danych do i z magazynu obiektów Blob Azure](move-azure-blob.md) do skopiowania danych na tym komputerze.

Opisano jak transfer plików zawierających dane za pomocą narzędzia AzCopy. Aby pobrać i zainstalować narzędzia AzCopy postępuj zgodnie z instrukcjami w [wprowadzenie do korzystania z wiersza polecenia Azcopy](../../storage/common/storage-use-azcopy.md).

1. Z poziomu okna wiersza polecenia, należy wydać następujące polecenia narzędzia AzCopy, zastępując *< path_to_data_folder >* z docelowej:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. Po zakończeniu kopiowania, łącznie 24 plików zip znajdują się w folderze danych wybrany. Rozpakuj pobrane pliki do katalogu na komputerze lokalnym. Zwróć uwagę na folder, w którym znajdują się pliki nieskompresowane. Ten folder będzie ona nazywana jako *< ścieżka\_do\_unzipped_data\_pliki\>*  jest poniżej.

## <a name="upload"></a>Przekazywanie danych do kontenera domyślnego klastra usługi Azure HDInsight Hadoop
> [!NOTE]
> Jest to zazwyczaj **Admin** zadań.
> 
> 

W poniższych poleceniach narzędzia AzCopy, Zamień następujące parametry rzeczywistej wartości, które można określić podczas tworzenia klastra usługi Hadoop i rozpakować pliki danych.

* ***&#60; path_to_data_folder >*** katalogu (wraz ze ścieżką) na komputerze, zawierających pliki rozpakowane danych  
* ***&#60; nazwa konta magazynu z klastra usługi Hadoop >*** konta magazynu skojarzone z klastrem usługi HDInsight
* ***&#60; domyślny kontener klastra usługi Hadoop >*** domyślny kontener używane przez klaster. Należy pamiętać, że nazwa kontenera domyślnego zwykle taką samą nazwę jak samego klastra. Na przykład jeśli klaster jest nazywany "abc123.azurehdinsight.net", domyślny kontener jest abc123.
* ***&#60; klucz konta magazynu >*** klucza dla konta magazynu używane przez klaster

Z wiersza polecenia lub okno programu Windows PowerShell na tym komputerze uruchom następujące dwa polecenia AzCopy.

To polecenie wysyła dane podróży ***nyctaxitripraw*** katalogu w domyślnym kontenerze klastra usługi Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

To polecenie wysyła dane taryfy ***nyctaxifareraw*** katalogu w domyślnym kontenerze klastra usługi Hadoop.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Dane powinno się teraz w magazynie obiektów Blob Azure i są gotowe do użycia w klastrze usługi HDInsight.

## <a name="#download-hql-files"></a>Zaloguj się do węzła głównego klastra usługi Hadoop i i przygotować do analizy danych poznawcze
> [!NOTE]
> Jest to zazwyczaj **Admin** zadań.
> 
> 

Aby uzyskać dostęp do węzła głównego klastra do analizy danych poznawcze i w dół próbkowania danych, wykonaj procedurę opisaną w [dostęp węzła Head klastra usługi Hadoop](customize-hadoop-cluster.md).

W tym przewodniku używamy głównie zapytań w [Hive](https://hive.apache.org/), język kwerendy przypominającego SQL, do wykonywania eksploracji wstępne dane. Zapytania Hive są przechowywane w plikach .hql. Firma Microsoft następnie wyłącza przykładowe tych danych do użycia w usłudze Azure Machine Learning przeznaczone do budowania modeli.

Aby przygotować klaster do analizy danych poznawcze, możemy pobrać pliki .hql zawierające odpowiednie skrypty Hive z [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) do katalogu lokalnego (C:\temp) w węźle głównym. Aby to zrobić, otwórz **wiersza polecenia** z wewnątrz węzła głównego klastra i wydać następujące dwa polecenia:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Te dwa polecenia pobierze wszystkie pliki .hql potrzebne w tym przewodniku do katalogu lokalnego ***C:\temp &#92;*** w węzła głównego.

## <a name="#hive-db-tables"></a>Utwórz gałąź bazy danych i tabele partycjonowane według miesięcy
> [!NOTE]
> Jest to zazwyczaj **Admin** zadań.
> 
> 

Firma Microsoft są teraz gotowe do tworzenia tabel gałęzi dla naszego zestawu danych taksówki NYC.
Węzła głównego klastra usługi Hadoop, otwórz ***wiersza polecenia platformy Hadoop*** na pulpicie węzła głównego i wprowadź katalog Hive przez wprowadzenie polecenia

    cd %hive_home%\bin

> [!NOTE]
> **Uruchom wszystkie polecenia gałęzi w tym przewodniku z powyższych bin gałęzi / directory wiersza. To zajmie się wszelkie problemy, ścieżka automatycznie. Używane pojęcia "Hive katalogu wiersza", "bin gałęzi / directory wiersza" i "wiersza polecenia usługi Hadoop" zamiennie w tym przewodniku.**
> 
> 

W wierszu katalogu Hive wprowadź następujące polecenie w Hadoop wiersza polecenia węzła głównego do przesyłania zapytań Hive, aby utworzyć gałąź bazy danych i tabele:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Oto zawartość ***C:\temp\sample\_hive\_utworzyć\_db\_i\_tables.hql*** pliku, który tworzy gałąź bazy danych ***nyctaxidb*** i tabele ***podróży*** i ***taryfy***.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Ten skrypt Hive tworzy dwie tabele:

* Tabela "podróży" zawiera podróży szczegóły każdego jazdy (Szczegóły sterownika, czasu odbioru, odległość podróży i razy)
* Tabela "opłata" zawiera szczegóły taryfy (kwota taryfy, kwota poradę, przejazd i opłaty).

Jeśli potrzebna jest uzyskania dodatkowej pomocy z tych procedur, i chcesz zbadać tych alternatywnych, zobacz sekcję [Hive przesyłania kwerend bezpośrednio z wiersza polecenia usługi Hadoop ](move-hive-tables.md#submit).

## <a name="#load-data"></a>Ładowanie danych do tabel Hive przez partycje
> [!NOTE]
> Jest to zazwyczaj **Admin** zadań.
> 
> 

Zestaw danych taksówki NYC ma fizycznych partycjonowania według miesięcy, której używamy umożliwiające szybsze przetwarzanie i zapytania. Poniższe polecenia programu PowerShell (wystawiony na podstawie przy użyciu katalogu Hive **wiersza polecenia platformy Hadoop**) ładowanie danych do tabel gałąź "podróży" i "opłata" podzielona na partycje według miesięcy.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

*Próbki\_hive\_załadować\_danych\_przez\_partitions.hql* plik zawiera następujące **ZAŁADOWAĆ** poleceń.

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Należy pamiętać, że liczba zapytań programu Hive tutaj używanych w ramach procesu eksploracji są związane z jednej partycji lub tylko kilka partycji. Jednak te zapytania może zostać uruchomione przez cały dane.

### <a name="#show-db"></a>Pokaż baz danych w klastrze usługi HDInsight Hadoop
Aby wyświetlić baz danych, utworzonych w klastrze usługi HDInsight Hadoop w oknie wiersza polecenia platformy Hadoop, uruchom następujące polecenie w wierszu polecenia Hadoop:

    hive -e "show databases;"

### <a name="#show-tables"></a>Pokaż tabele gałęzi w bazie danych nyctaxidb
Aby wyświetlić tabele w bazie danych nyctaxidb, uruchom następujące polecenie w wierszu polecenia Hadoop:

    hive -e "show tables in nyctaxidb;"

Możemy potwierdzić, że tabele są dzielone wydając polecenie poniżej:

    hive -e "show partitions nyctaxidb.trip;"

Poniżej przedstawiono oczekiwane dane wyjściowe:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Podobnie Upewniamy się, że jest podzielona na partycje tabeli taryfy wydając polecenie poniżej:

    hive -e "show partitions nyctaxidb.fare;"

Poniżej przedstawiono oczekiwane dane wyjściowe:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Eksploracja danych i funkcji inżynieryjne w gałęzi
> [!NOTE]
> Jest to zazwyczaj **naukowca danych** zadań.
> 
> 

Eksploracja danych i funkcji inżynierii zadania dla danych załadowanych do tabel Hive można osiągnąć za pomocą zapytań Hive. Poniżej przedstawiono przykłady takich zadań, że firma Microsoft przeprowadzi użytkownika przez proces w tej sekcji:

* Wyświetlanie rekordów 10 pierwszych w obu tabel.
* Eksplorowanie danych dystrybucji kilka pól w różnym czasie systemu windows.
* Zbadaj jakości danych pól długości i szerokości geograficznej.
* Generowanie na podstawie etykiet klasyfikacji binarnej i wieloklasowej **Porada\_kwota**.
* Generowanie funkcji przez Obliczanie odległości bezpośredniego podróży.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Eksploracja: Wyświetl rekordy 10 pierwszych w podróży tabeli
> [!NOTE]
> Jest to zazwyczaj **naukowca danych** zadań.
> 
> 

Aby zobaczyć, jak wygląda danych, omówione 10 rekordy z każdej tabeli. Uruchomić następujące dwa zapytania niezależnie od wiersza katalogu gałęzi w konsoli Hadoop wiersza polecenia, aby sprawdzić rekordy.

Aby uzyskać rekordów 10 pierwszych w tabeli "podróży" z pierwszego miesiąca:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

Aby uzyskać top 10 rekordów w tabeli "opłata" od pierwszego miesiąca:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Często jest to przydatne można zapisać do pliku w celu wyświetlenia wygodny rekordy. Niewielkie zmiany w zapytaniu powyżej rozwiązanie to:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Eksploracja: Wyświetl liczby rekordów w każdej partycji 12
> [!NOTE]
> Jest to zazwyczaj **naukowca danych** zadań.
> 
> 

Odsetek jest jak Liczba podróży może być różna w roku kalendarzowym. Grupowanie według miesięcy pozwala zobaczyć, jak wygląda rozkład ten rund.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Daje dane wyjściowe:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

W tym miejscu pierwszej kolumny jest miesiąc, a drugą jest wartość liczbę rund w danym miesiącu.

Firma Microsoft liczba całkowita liczba rekordów w naszym zestawie danych podróży wydając polecenie w wierszu polecenia programu Hive katalogu.

    hive -e "select count(*) from nyctaxidb.trip;"

Daje to:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Za pomocą poleceń podobne do tych wyświetlane dla zestawu danych w podróży, firma Microsoft może wystawiać zapytań programu Hive z poziomu wiersza katalogu gałęzi dla zestawu danych taryfy do sprawdzania poprawności liczby rekordów.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Daje dane wyjściowe:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Należy pamiętać, że dokładnie samą liczbę rund miesięcznie jest zwracany w obu zestawach danych. Zapewnia to pierwsze sprawdzenie, czy dane zostały załadowane poprawnie.

Całkowita liczba rekordów w zestawie danych taryfy zliczania może odbywać się przy użyciu poniższe polecenie w wierszu katalogu gałęzi:

    hive -e "select count(*) from nyctaxidb.fare;"

Daje to:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Całkowita liczba rekordów w obu tabel jest również. Zapewnia to drugi weryfikacji, że dane zostały załadowane poprawnie.

### <a name="exploration-trip-distribution-by-medallion"></a>Eksploracja: Podróży dystrybucji przez Medalionu
> [!NOTE]
> Jest to zazwyczaj **naukowca danych** zadań.
> 
> 

W tym przykładzie identyfikuje Medalionu (taksówki numery) z więcej niż 100 rund w danym okresie. Zapytanie korzysta z tabeli partycjonowanej dostępu, ponieważ należy przygotować przez zmienną partycji **miesiąca**. Wyniki zapytania są zapisywane w pliku lokalnego queryoutput.tsv w `C:\temp` w węźle głównym.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Oto zawartość *próbki\_hive\_podróży\_liczba\_przez\_medallion.hql* pliku inspekcji.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Medalionu w zestawie danych taksówki NYC identyfikuje unikatowy pliku cab. Firma Microsoft można określić, które pliki cab są "zajęte" pytając, które z nich wprowadzone więcej niż określoną liczbę rund w określonym przedziale czasu. Poniższy przykład identyfikuje pliki cab, wprowadzone w więcej niż stu rund w pierwsze trzy miesiące, a następnie zapisuje wyniki zapytania do pliku lokalnego C:\temp\queryoutput.tsv.

Oto zawartość *próbki\_hive\_podróży\_liczba\_przez\_medallion.hql* pliku inspekcji.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

W wierszu katalogu Hive należy wydać polecenie poniżej:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Eksploracja: Podróży dystrybucji Medalionu i hack_license
> [!NOTE]
> Jest to zazwyczaj **naukowca danych** zadań.
> 
> 

Podczas eksplorowania zestawu danych, chcemy często Sprawdź liczbę wystąpień co grupy wartości. W tej sekcji zawierają przykładem, w tym dla plików cab i sterowników.

*Próbki\_hive\_podróży\_liczba\_przez\_Medalionu\_license.hql* pliku grup taryfy zestawu danych na "Medalionu" i "hack_license" i zwraca liczby w każdej kombinacji. Poniżej przedstawiono jego zawartość.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Ta kwerenda zwraca kombinacji określony sterownik, uporządkowanych malejąco liczby rund i cab.

W wierszu katalogu Hive Uruchom polecenie:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Wyniki zapytania są zapisywane do pliku lokalnego C:\temp\queryoutput.tsv.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Eksploracja: Oceny jakości danych przez wyszukiwanie rekordów nieprawidłowa długość i szerokość geograficzna
> [!NOTE]
> Jest to zazwyczaj **naukowca danych** zadań.
> 
> 

Typowe celem analizy danych poznawcze jest chwastów limit rekordów nieprawidłowe lub uszkodzone. Przykład w tej sekcji określa, czy albo wartość współrzędnych geograficznych pola zawiera wartości do tej pory poza obszarem NYC. Ponieważ istnieje prawdopodobieństwo, że takie rekordów mają wartości błędne współrzędne geograficzne, chcemy Usuń je z żadnych danych, która ma być używana do modelowania.

Oto zawartość *próbki\_hive\_jakości\_assessment.hql* pliku inspekcji.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


W wierszu katalogu Hive Uruchom polecenie:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

*-S* wydruku ekranu stan zadań Hive mapy/Zmniejsz pomija argument zawarte w tym poleceniu. Jest to przydatne, ponieważ sprawia, że ekran wydruku wynik zapytania Hive bardziej czytelne.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Eksploracja: Binarny klasy dystrybucje podróży porad
> [!NOTE]
> Jest to zazwyczaj **naukowca danych** zadań.
> 
> 

W przypadku problemu klasyfikacji binarnej opisane w temacie [przykłady zadań prognozowania](hive-walkthrough.md#mltasks) sekcji warto wiedzieć, czy etykietki został podany lub nie. Tej dystrybucji porady jest plikiem binarnym:

* Porada podane (klasy 1, porada\_ilość > $0)  
* Brak porady (klasa 0, porada\_kwota = $0).

*Próbki\_hive\_Przechylony\_frequencies.hql* pliku pokazano poniżej robi to.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

W wierszu katalogu Hive Uruchom polecenie:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Eksploracja: Klasa dystrybucje w ustawieniu wieloklasowej
> [!NOTE]
> Jest to zazwyczaj **naukowca danych** zadań.
> 
> 

W przypadku problemu wieloklasowej klasyfikacji opisane w [przykłady zadań prognozowania](hive-walkthrough.md#mltasks) sekcji ten zestaw danych również jest przydatna do fizycznych klasyfikacji, gdy chcemy przewidzieć ilość wskazówki podane. Możemy użyć bins, aby zdefiniować zakresów Porada w zapytaniu. Uzyskanie dystrybucje klasy dla różnych Porada zakresów, używamy *próbki\_hive\_Porada\_zakres\_frequencies.hql* pliku. Poniżej przedstawiono jego zawartość.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Uruchom następujące polecenie z wiersza polecenia platformy Hadoop konsoli:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Eksploracji: Bezpośrednie odległość między dwiema lokalizacjami współrzędne geograficzne obliczeniowe
> [!NOTE]
> Jest to zazwyczaj **naukowca danych** zadań.
> 
> 

O miary bezpośredniego odległości pozwala sprawdzić rozbieżność między nim a odległość rzeczywiste podróży. Ta funkcja możemy Motywuj przez wskazujące, że osób może być mniej prawdopodobne się jeśli one dowiedzieć się, że sterownik celowo miało je przez dłużej trasy.

Aby zobaczyć porównanie odległość rzeczywiste podróży i [odległość Haversine](http://en.wikipedia.org/wiki/Haversine_formula) między dwoma punktami współrzędne geograficzne (odległość "koła"), używamy funkcje trygonometryczne dostępne w gałęzi, w związku z tym:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

W powyższym zapytaniu R jest radius ziemi w milach i pi jest konwertowana na wartość w radianach. Należy pamiętać, że punkty współrzędne geograficzne są "filtrowane" można usunąć wartości, które są daleko od obszaru NYC.

W takim przypadku możemy zapisać naszych wyników katalog o nazwie "queryoutputdir". Sekwencji poleceń pokazano poniżej najpierw tworzy ten katalog wyjściowy, a następnie uruchamia polecenie gałęzi.

W wierszu katalogu Hive Uruchom polecenie:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Wyniki zapytania są zapisywane do 9 obiektów blob Azure ***queryoutputdir/000000\_0*** do ***queryoutputdir/000008\_0*** w ramach kontenera domyślnego klastra usługi Hadoop.

Aby wyświetlić rozmiar poszczególnych obiektów blob, możemy uruchom następujące polecenie w wierszu katalogu gałęzi:

    hdfs dfs -ls wasb:///queryoutputdir

Aby wyświetlić zawartość określonego pliku, powiedz 000000\_0, używamy jego Hadoop `copyToLocal` poleceń, w związku z tym.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> `copyToLocal`może być bardzo wolno dla dużych plików, a nie jest zalecane używanie z nimi.  
> 
> 

Zaletą tych danych znajdują się w obiekcie blob Azure jest, że firma Microsoft może Eksplorowanie danych w ramach przy użyciu usługi Azure Machine Learning [i zaimportuj dane] [ import-data] modułu.

## <a name="#downsample"></a>Przykładowe modele danych i kompilacji w usłudze Azure Machine Learning w dół
> [!NOTE]
> Jest to zazwyczaj **naukowca danych** zadań.
> 
> 

Po fazie analizy danych poznawcze mamy teraz przystąpić do przykładowych danych do tworzenia modeli w usłudze Azure Machine Learning w dół. W tej sekcji zostanie przedstawiony sposób Użyj zapytania programu Hive w dół przykładowych danych, który następnie jest dostępny z [i zaimportuj dane] [ import-data] modułu w usłudze Azure Machine Learning.

### <a name="down-sampling-the-data"></a>Próbkowanie danych w dół
Istnieją dwa kroki tej procedury. Najpierw dołączyć możemy **nyctaxidb.trip** i **nyctaxidb.fare** tabel w trzech kluczy, które znajdują się we wszystkich rekordach: "Medalionu", "hack\_licencji", i "podnoszenia\_datetime". Następnie możemy wygenerować etykiety klasyfikacji binarnej **Przechylony** i etykiety klasyfikacji wielu klasy **Porada\_klasy**.

Aby można było użyć w dół pobrane dane bezpośrednio z [i zaimportuj dane] [ import-data] modułu w usłudze Azure Machine Learning, należy go przechowywać wyniki zapytania powyżej do wewnętrznej tabeli Hive. W poniżej możemy utworzyć wewnętrznej tabeli Hive i wypełnić zawartością z połączonych i w dół próbki danych.

Zapytanie stosuje standardowe funkcje Hive bezpośrednio do wygenerowania godzinę dnia, tygodnia w roku, dzień tygodnia (1 zawiera poniedziałek i 7 zawiera niedziela) z "podnoszenia\_datetime" pola i bezpośredniego odległość między lokalizacjami odbiór i dropoff. Użytkownicy mogą odwoływać się do [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) pełną listę funkcji.

Zapytanie następnie wyłącza próbek danych tak, aby wyniki zapytania można umieścić w usłudze Azure Machine Learning Studio. Tylko około 1% oryginalnego zestawu danych jest importowany do programu Studio.

Poniżej przedstawiono zawartość *próbki\_hive\_przygotowanie\_dla\_aml\_full.hql* pliku, który przygotowuje danych do tworzenia w usłudze Azure Machine Learning model.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Do uruchomienia tego zapytania z poziomu wiersza katalogu gałęzi:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Mamy teraz wewnętrznej tabeli "nyctaxidb.nyctaxi_downsampled_dataset", który można uzyskać dostęp za pomocą [i zaimportuj dane] [ import-data] modułu na podstawie usługi Azure Machine Learning. Ponadto firma Microsoft może używać tego elementu dataset do budowania modeli uczenia maszynowego.  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a>Używaj modułu importowanie danych w usłudze Azure Machine Learning dostępu dół próbki danych do
Jako wymagania wstępne dotyczące wystawiania Hive zapytania w programie [i zaimportuj dane] [ import-data] moduł usługi Azure Machine Learning, potrzebujemy dostęp do Azure Machine Learning obszaru roboczego i dostęp do poświadczeń klastra i jego skojarzone konto magazynu.

Niektóre szczegóły na [i zaimportuj dane] [ import-data] modułu i parametry wejściowe:

**Identyfikator URI serwera HCatalog**: Jeśli nazwa klastra jest abc123, to po prostu: https://abc123.azurehdinsight.net

**Nazwa konta użytkownika Hadoop**: nazwa użytkownika wybrany dla klastra (**nie** nazwa użytkownika dostępu zdalnego)

**Hasło konta ser Hadoop**: hasło wybrane dla klastra (**nie** hasła dostępu zdalnego)

**Lokalizacja danych wyjściowych**: to jest wybierany jako platformy Azure.

**Nazwa konta magazynu Azure**: Nazwa domyślnego konta magazynu skojarzone z klastrem.

**Nazwa kontenera Azure**: to jest domyślna nazwa kontenera dla klastra i zwykle jest taka sama jak nazwa klastra. W przypadku klastra o nazwie "abc123" jest po prostu abc123.

> [!IMPORTANT]
> **Wszystkie tabele Życzymy zapytań przy użyciu [i zaimportuj dane] [ import-data] modułu w usłudze Azure Machine Learning musi być wewnętrznej tabeli.** Porada na określenie, czy tabela T w bazie danych D.db wewnętrznej tabeli ma następującą składnię.
> 
> 

W wierszu katalogu Hive należy wydać polecenie:

    hdfs dfs -ls wasb:///D.db/T

Jeśli tabela jest wewnętrznej tabeli i zostanie wprowadzony, jego zawartość musi wskazywać tutaj. Inny sposób, aby określić, czy tabela jest wewnętrznej tabeli jest za pomocą Eksploratora magazynu Azure. Umożliwia ona przejdź do kontenera domyślną nazwę klastra, a następnie Filtruj według nazwy tabeli. Jeśli w tabeli i jego zawartość występuje, to potwierdzenie jest wewnętrznej tabeli.

W tym miejscu jest migawką zapytanie Hive i [i zaimportuj dane] [ import-data] modułu:

![Zapytanie hive dla modułu importowanie danych](./media/hive-walkthrough/1eTYf52.png)

Od naszych dół próbki danych znajduje się w domyślnym kontenerze, wynikowe zapytanie Hive z usługi Azure Machine Learning jest bardzo prosty i jest tylko "Wybierz * z nyctaxidb.nyctaxi\_próbkowane w dół\_danych".

Zestaw danych może teraz używać jako punktu wyjścia do tworzenia modeli uczenia maszynowego.

### <a name="mlmodel"></a>Tworzenie modeli w usłudze Azure Machine Learning
Możemy teraz przystąpić do konstruowania modelu i wdrażania modelu w [usługi Azure Machine Learning](https://studio.azureml.net). Dane są gotowe do nas do użycia w zakresie rozwiązywania problemów prognozowania określonych powyżej:

**1. Klasyfikacji binarnej**: przewidywanie, czy etykietki został płatnej w podróży.

**Uczeń używane:** Regresja logistyczna Two-class

a. W przypadku tego problemu naszą etykietę docelowego (lub klasy) jest "Przechylony". Nasze oryginalnego zestawu danych próbkowania w dół ma kilka kolumn, które są przecieki docelowych do tego eksperymentu klasyfikacji. W szczególności: Porada\_klasy, porada\_kwota i całkowitej\_ilość ujawniania informacji o etykiecie docelowej, która nie jest dostępny na czas testowania. Firma Microsoft Usuń te kolumny z brany pod uwagę przy użyciu [Select Columns in Dataset] [ select-columns] modułu.

Migawki poniżej przedstawia naszych eksperymentu do prognozowania, czy poradę został płatnej dla danego podróży.

![Migawki eksperymentu](./media/hive-walkthrough/QGxRz5A.png)

b. W tym eksperymencie naszych dystrybucje etykiety docelowego zostały około 1:1.

Migawki poniżej pokazuje rozkład porady etykiety klasy dla problemu klasyfikacji binarnej.

![Dystrybucji Porada etykiet — klasa](./media/hive-walkthrough/9mM4jlD.png)

W związku z tym możemy uzyskać AUC 0.987, jak pokazano na poniższej ilustracji.

![Wartość AUC](./media/hive-walkthrough/8JDT0F8.png)

**2. Wieloklasowej klasyfikacji**: przewidywanie zakres kwoty Porada płatnej podczas podróży, za pomocą wcześniej zdefiniowanych klas.

**Uczeń używane:** Wieloklasowej Regresja logistyczna

a. W przypadku tego problemu jest naszą etykietę docelowego (lub klasy) "Porada\_klasy" może to zająć jednej z pięciu wartości (0,1,2,3,4). Tak jak w przypadku klasyfikacji binarnej mamy kilka kolumn, które są przecieki docelowych do tego eksperymentu. W szczególności: Przechylony, porada\_łączna kwota\_ilość ujawniania informacji o etykiecie docelowej, która nie jest dostępny na czas testowania. Firma Microsoft Usuń te kolumny za pomocą [Select Columns in Dataset] [ select-columns] modułu.

Migawki poniżej przedstawia naszych eksperymentu do prognozowania, w których bin Porada jest mogącego wchodzić (klasy 0: Porada = $0, 1 klasy: Porada > $0 i Porada < = $5, 2 klasy: Porada > $5 i porady < = $10 klasy 3: Porada > $10 i Porada < = $20 Klasa 4: Porada > $20)

![Migawki eksperymentu](./media/hive-walkthrough/5ztv0n0.png)

Teraz Pokaż się wygląd naszego dystrybucji klasy rzeczywiste testu. Widzimy, że klasa 0 i 1 klasy są powszechnie znane, innych klas są rzadko.

![Testowanie dystrybucji — klasa](./media/hive-walkthrough/Vy1FUKa.png)

b. W tym eksperymencie używamy macierzy pomyłek spojrzeć na naszych dokładności przewidywania. Jest to pokazane poniżej.

![Macierz pomyłek](./media/hive-walkthrough/cxFmErM.png)

Należy pamiętać, że nasze dokładności klasy w klasach powszechnie znane są bardzo dobre, modelu, które nie są dobrym zadanie "learning" w rzadkich klasy.

**3. Zadanie regresji**: przewidywanie ilość Porada płatnej w podróży.

**Uczeń używane:** Boosted drzewa decyzyjnego

a. W przypadku tego problemu jest naszą etykietę docelowego (lub klasy) "Porada\_kwota". Nasze przecieki docelowego w tym przypadku są: Przechylony, porada\_klasy całkowita\_kwota; te zmienne ujawnienia informacji o ilości poradę, która jest zazwyczaj niedostępna na czas testowania. Firma Microsoft Usuń te kolumny za pomocą [Select Columns in Dataset] [ select-columns] modułu.

Belows migawki pokazuje naszych eksperymentu do prognozowania ilość danego poradę.

![Migawki eksperymentu](./media/hive-walkthrough/11TZWgV.png)

b. Regresja problemów firma Microsoft pomiarów dokładności przewidywania naszych analizując kwadratów błąd w prognozy, współczynnik determinacji i podobne. Zostanie przedstawiony je poniżej.

![Statystyki prognozowania](./media/hive-walkthrough/Jat9mrz.png)

Widzimy o współczynnik determinacji jest 0.709, co oznacza około 71% wariancję tłumaczy naszych współczynników modelu.

> [!IMPORTANT]
> Aby dowiedzieć się więcej na temat usługi Azure Machine Learning oraz sposób dostępu i przy jego użyciu, zapoznaj się [co to jest Machine Learning](../studio/what-is-machine-learning.md). Jest bardzo przydatny zasobów do odtwarzania z licznych eksperymenty uczenia maszynowego w usłudze Azure Machine Learning [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/). Galerii obejmuje gamy eksperymenty i zapewnia kompleksowy wprowadzenia zakres możliwości usługi Azure Machine Learning.
> 
> 

## <a name="license-information"></a>Informacje o licencji
Ten przewodnik próbki i jego towarzyszący skrypty są udostępniane przez firmę Microsoft w ramach licencji MIT. Sprawdź, czy w pliku LICENSE.txt w katalogu przykładowy kod w serwisie GitHub więcej szczegółów.

## <a name="references"></a>Dokumentacja
• [Andrés Monroy taksówki NYC rund stronę pobierania](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC taksówki podróży danych przez Krzysztof Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Taksówki NYC i Limousine Komisji badań i statystyki](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[2]: ./media/hive-walkthrough/output-hive-results-3.png
[11]: ./media/hive-walkthrough/hive-reader-properties.png
[12]: ./media/hive-walkthrough/binary-classification-training.png
[13]: ./media/hive-walkthrough/create-scoring-experiment.png
[14]: ./media/hive-walkthrough/binary-classification-scoring.png
[15]: ./media/hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
