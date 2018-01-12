---
title: "Co to jest strumieniowego Spark w usłudze Azure HDInsight | Dokumentacja firmy Microsoft"
description: "Jak używać aplikacji przesyłania strumieniowego Spark w klastrach HDInsight Spark."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: maxluk
ms.openlocfilehash: 6a57576704d1814d08514dc436070cab07f12b39
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="overview-of-spark-streaming"></a>Omówienie przesyłania strumieniowego Spark

Przesyłanie strumieniowe Spark umożliwia przetwarzanie strumienia danych Spark w usłudze HDInsight klastrów z gwarancją, że wszystkie zdarzenia wejściowe jest przetwarzany tylko raz, nawet jeśli wystąpi awaria węzła. Strumień Spark jest długotrwałe zadanie, które otrzymuje danych wejściowych z różnych źródeł, takich jak Azure Event Hubs, Centrum IoT Azure, Kafka, Flume, Twitter, ZeroMQ, gniazd TCP lub monitorowania systemu plików HDFS systemy plików. W przeciwieństwie do procesu wyłącznie zdarzeniami strumienia Spark partii danych wejściowych do uruchomienia systemu windows, takich jak wycinek 2 sekundy, a następnie przekształca poszczególnych partii danych przy użyciu mapy, Zmniejsz join i wyodrębnianie działań. Następnie zapisuje przekształcone dane systemy plików baz danych, pulpity nawigacyjne i konsoli.

![Strumień przetwarzania za pomocą usługi HDInsight i przesyłania strumieniowego Spark](./media/apache-spark-streaming-overview/hdinsight-spark-streaming.png)

Przesyłanie strumieniowe Spark aplikacji należy poczekać kilka części sekundy zbierać poszczególnych *partii micro* zdarzeń przed wysłaniem tej partii potrzeby przetwarzania. Z kolei aplikacji sterowane zdarzeniami przetwarza każdego zdarzenia natychmiast. Opóźnienia przesyłania strumieniowego Spark jest zazwyczaj w ciągu kilku sekund. Zalety podejście micro partii są bardziej wydajne przetwarzania danych i obliczeń agregujących łatwiejsze.

## <a name="introducing-the-dstream"></a>Wprowadzenie do DStream

Przesyłanie strumieniowe Spark reprezentuje ciągłego strumienia przychodzącego danych przy użyciu *zdyskretyzowany strumienia* o nazwie DStream. DStream mogą być tworzone z źródeł danych wejściowych, takich jak usługi Event Hubs lub Kafka lub przez stosowanie przekształceń do innego DStream.

DStream zapewnia warstwę abstrakcji nałożoną dane pierwotne zdarzenia. 

Rozpoczynać pojedyncze zdarzenie, co oznacza temperatury odczytu z połączonych termostat. To zdarzenie dociera do lokalizacji aplikacji przesyłania strumieniowego Spark, zdarzenia są przechowywane w sposób niezawodny, oznacza to, że jest replikowana na wielu węzłach. To tolerancji gwarantuje, że awarii dowolnego pojedynczego węzła nie spowoduje utratę wydarzenia. Spark core wykorzystuje dystrybuująca dane w wielu węzłach w klastrze, struktura danych, w którym każdy węzeł zazwyczaj zajmuje własnych danych w pamięci najlepszą wydajność. Ta struktura danych jest wywoływana *odporność rozproszonej zestawu danych* (RDD).

Każdy RDD reprezentuje zdarzenia zebrane przez przedział czasu zdefiniowany przez użytkownika o nazwie Interwał partii. Zgodnie z każdej partii ciągu określonego czasu, jest generowany nowy RDD zawierający wszystkie dane z tego interwału. Ciągłego zestawu RDDs są zbierane w DStream. Na przykład jeśli interwał partii jest długa jednej sekundy, DStream Twojego emituje partii co drugi zawierający RDD jedną, z danymi pozyskanych w ciągu sekundy tego. Podczas przetwarzania DStream, zdarzenie temperatury występuje w jednym z tych partii. Do przesyłania strumieniowego Spark aplikacji przetwarza partie, które zawierają zdarzenia i ostatecznie działania w przypadku danych przechowywanych w każdej RDD.

![Przykład DStream ze zdarzeniami temperatury ](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-example.png)

## <a name="structure-of-a-spark-streaming-application"></a>Struktura aplikacji Spark przesyłania strumieniowego
Przesyłanie strumieniowe Spark aplikacja to długotrwałe odbiera dane ze źródeł pozyskiwania, stosuje przekształcenia do przetwarzania danych, a następnie wypycha dane do jednego lub więcej miejsc docelowych. Struktura aplikacji Spark przesyłania strumieniowego ma dwie główne części. Najpierw należy zdefiniować w przypadku, gdy dane pochodzą z, jakie operacje na danych, jak i gdzie powinien pojawiać się wyniki. Po drugie Uruchom aplikację, oczekiwanie na sygnał zatrzymania.

Na przykład w tym miejscu jest prostą aplikację, która odbiera wiersza tekstu za gniazda TCP i oblicza liczbę razy, zostanie wyświetlony każdego wyrazu.

### <a name="define-the-application"></a>Definiowanie aplikacji

Definicja aplikacji logiki obejmuje cztery kroki:

1. Utwórz StreamingContext.
2. Utwórz DStream StreamingContext.
3. Przekształcane DStream.
4. Zapisuje wyniki.

Ta definicja jest statyczny, a nie dane są przetwarzane do czasu uruchomienia aplikacji.

#### <a name="create-a-streamingcontext"></a>Utwórz StreamingContext
Utwórz StreamingContext SparkContext, wskazujące do klastra. Podczas tworzenia StreamingContext, należy określić rozmiar partii w sekundach, na przykład:

    val ssc = new StreamingContext(spark, Seconds(1))

#### <a name="create-a-dstream"></a>Utwórz DStream
Za pomocą utworzonego wystąpienie StreamingContext Utwórz wejściowych DStream dla źródła danych wejściowych. W takim przypadku możemy oglądania wyglądu nowe pliki w domyślny magazyn dołączony do klastra usługi HDInsight.

    val lines = ssc.textFileStream("/uploads/2017/01/")

#### <a name="apply-transformations"></a>Zastosuj przekształcenia
Zaimplementowaniem przetwarzania przez stosowanie przekształceń na DStream. Naszej aplikacji otrzymuje jednego wiersza tekstu w czasie z pliku, dzieli każdego wiersza w wyrazy, a następnie używa wzorca map-reduce można określić liczbę razy, zostanie wyświetlony każdego wyrazu.

    val words = lines.flatMap(_.split(" "))
    val pairs = words.map(word => (word, 1))
    val wordCounts = pairs.reduceByKey(_ + _)

#### <a name="output-results"></a>Wynik
Przekładanie wyniki transformacji do docelowe systemy, stosując operacji wyjścia. W takim przypadku zostanie przedstawiony wynik każdego uruchomienia obliczenia w danych wyjściowych konsoli.

    wordCounts.print()

### <a name="run-the-application"></a>Uruchamianie aplikacji
Uruchom przesyłania strumieniowego aplikacji i uruchomić do momentu otrzymania sygnału zakończenia.

    ssc.start()            
    ssc.awaitTermination()

Aby uzyskać więcej informacji na temat interfejsu API strumienia Spark, wraz z danych wyjściowych operacji źródła zdarzeń, transformacji i obsługuje zobacz [przewodnik programowania w języku przesyłania strumieniowego Spark](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html).

Następujące przykładowej aplikacji jest autonomiczna, więc można uruchomić go w programie [notesu Jupyter](apache-spark-jupyter-notebook-kernels.md). W tym przykładzie tworzymy źródłem danych testowych w klasie DummySource, która wyświetla wartość licznika i bieżący czas w milisekundach co pięć sekund. Utworzymy nowy obiekt StreamingContext ma przedział partii 30 sekund. Za każdym razem, gdy partii jest tworzony, jego sprawdza RDD utworzone, konwertuje ją na Spark DataFrame i tworzy tabeli tymczasowej w DataFrame.

    class DummySource extends org.apache.spark.streaming.receiver.Receiver[(Int, Long)](org.apache.spark.storage.StorageLevel.MEMORY_AND_DISK_2) {

        /** Start the thread that simulates receiving data */
        def onStart() {
            new Thread("Dummy Source") { override def run() { receive() } }.start()
        }

        def onStop() {  }

        /** Periodically generate a random number from 0 to 9, and the timestamp */
        private def receive() {
            var counter = 0  
            while(!isStopped()) {
            store(Iterator((counter, System.currentTimeMillis)))
            counter += 1
            Thread.sleep(5000)
            }
        }
    }

    // A batch is created every 30 seconds
    val ssc = new org.apache.spark.streaming.StreamingContext(spark.sparkContext, org.apache.spark.streaming.Seconds(30))

    // Set the active SQLContext so that we can access it statically within the foreachRDD
    org.apache.spark.sql.SQLContext.setActive(spark.sqlContext)

    // Create the stream
    val stream = ssc.receiverStream(new DummySource())

    // Process RDDs in the batch
    stream.foreachRDD { rdd =>

        // Access the SQLContext and create a table called demo_numbers we can query
        val _sqlContext = org.apache.spark.sql.SQLContext.getOrCreate(rdd.sparkContext)
        _sqlContext.createDataFrame(rdd).toDF("value", "time")
            .registerTempTable("demo_numbers")
    } 

    // Start the stream processing
    ssc.start()

Firma Microsoft może następnie zapytania DataFrame okresowo, aby wyświetlić bieżący zestaw wartości istnieje w partii. W takim przypadku stosujemy następującej kwerendy SQL:

    %%sql
    SELECT * FROM demo_numbers

Dane wyjściowe wygląda następująco:

| wartość | time |
| --- | --- |
|10 | 1497314465256 |
|11 | 1497314470272 |
|12 | 1497314475289 |
|13 | 1497314480310 |
|14 | 1497314485327 |
|15 | 1497314490346 |

Oczekuje się zobaczyć sześciu wartości, ponieważ DummySource tworzy wartość 5 sekund i firma Microsoft emitowania partii co 30 sekund.

## <a name="sliding-windows"></a>Przedłużanie systemu windows
Jeśli chcesz wykonać obliczenia agregacji na Twojej DStream w niektórych przedziale czasu, na przykład w celu pobrania średnia temperatura przez ostatnich 2 sekundy, można użyć przesuwanego okna operacje dołączone do przesyłania strumieniowego Spark. Metodzie przesuwanego okna jest zdefiniowany jako mających czas trwania (długość okna) i interwał, w którym zawartość okna są oceniane (interwał slajdów).

Te przedłużanie systemu windows mogą nakładać się na, na przykład można zdefiniować okno o długości 2 sekundy slajdów co 1 s. Oznacza to, za każdym razem, gdy obliczanie agregacji, w oknie zostaną wyświetlone dane z ostatnich 1 sekundę poprzedniego okna, a także wszystkie nowe dane w następnym 1 sekundę.

![Przykład okna ze zdarzeniami temperatury](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-01.png)

![Przykład okna ze zdarzeniami temperatury po przedłużanie](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-window-02.png)

W poniższym przykładzie modyfikacjom kod, który używa DummySource w celu zbierania partii do okna z duration co minutę i slajdów co minutę.

    // A batch is created every 30 seconds
    val ssc = new org.apache.spark.streaming.StreamingContext(spark.sparkContext, org.apache.spark.streaming.Seconds(30))

    // Set the active SQLContext so that we can access it statically within the foreachRDD
    org.apache.spark.sql.SQLContext.setActive(spark.sqlContext)

    // Create the stream
    val stream = ssc.receiverStream(new DummySource())

    // Process batches in 1 minute windows
    stream.window(org.apache.spark.streaming.Minutes(1)).foreachRDD { rdd =>

        // Access the SQLContext and create a table called demo_numbers we can query
        val _sqlContext = org.apache.spark.sql.SQLContext.getOrCreate(rdd.sparkContext)
        _sqlContext.createDataFrame(rdd).toDF("value", "time")
        .registerTempTable("demo_numbers")
    } 

    // Start the stream processing
    ssc.start()

Po pierwszym minutę to umożliwia uzyskanie wpisy 12 - sześć wpisów z każdej partii dwóch zbierane w oknie.

| wartość | time |
| --- | --- |
| 1 | 1497316294139 |
| 2 | 1497316299158
| 3 | 1497316304178
| 4 | 1497316309204
| 5 | 1497316314224
| 6 | 1497316319243
| 7 | 1497316324260
| 8 | 1497316329278
| 9 | 1497316334293
| 10 | 1497316339314
| 11 | 1497316344339
| 12 | 1497316349361

Metodzie przesuwanego okna funkcji dostępnych w interfejsie API przesyłania strumieniowego Spark obejmują okna, countByWindow reduceByWindow i countByValueAndWindow. Aby uzyskać więcej informacji na temat tych funkcji, zobacz [przekształcenia DStreams](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html#transformations-on-dstreams).

## <a name="checkpointing"></a>Tworzenie punktów kontrolnych
Aby zapewnić odporność i odporność na uszkodzenia, przesyłanie strumieniowe Spark polega na tworzenie punktów kontrolnych, aby mieć pewność, że można kontynuować nieprzerwaną, nawet w wypadku awarii węzła przetwarzania strumienia. W usłudze HDInsight Spark tworzy punkty kontrolne do magazynu trwałego (magazynu Azure lub usługi Data Lake Store). Te punkty kontrolne przechowują metadane dotyczące przesyłania strumieniowego aplikacji, takie jak konfiguracji, zdefiniowany przez aplikację i wszystkie instancje, które zostały umieszczone w kolejce, ale jeszcze nie przetworzonych operacji. W niektórych przypadkach punkty kontrolne zostaną także zapisywania danych w RDDs skrócić czas potrzebny do odbudowywania stanu danych z co znajduje się w RDDs zarządza Spark.

## <a name="deploying-spark-streaming-applications"></a>Wdrażanie aplikacji Spark przesyłania strumieniowego
Zwykle skompilować przesyłania strumieniowego Spark aplikację lokalnie, a następnie wdrożyć go na Spark w usłudze HDInsight, skopiuj plik JAR zawierający aplikację do magazynu domyślnego powiązany z klastrem usługi HDInsight. Następnie możesz uruchomić aplikację przy użyciu interfejsów API REST LIVY, które są dostępne w klastrze. Jest to operacja POST gdy treść wpisu zawiera dokument JSON, który określa ścieżkę do Twojej JAR nazwę klasy, którego metoda główna definiuje i uruchamia przesyłania strumieniowego aplikacji oraz opcjonalnie wymagania dotyczące zasobów (takich jak liczba ex zadania ecutors, pamięci i rdzeni), a wszystkie ustawienia konfiguracji kodu aplikacji wymaga.

![Przykład okna ze zdarzeniami temperatury po przedłużanie](./media/apache-spark-streaming-overview/hdinsight-spark-streaming-livy.png)

Można również sprawdzić stan wszystkich aplikacji z żądanie GET względem punktu końcowego programu LIVY. Na koniec można zakończyć działającej aplikacji, wysyłając żądanie usunięcia, względem punktu końcowego na żywo. Aby uzyskać więcej informacji na temat interfejsu API programu LIVY, zobacz [zdalnego zadań z programu LIVY](apache-spark-livy-rest-interface.md)

## <a name="next-steps"></a>Kolejne kroki

* [Utworzyć klaster Apache Spark w usłudze HDInsight](../hdinsight-hadoop-create-linux-clusters-portal.md)
* [Przewodnik programowania w języku przesyłania strumieniowego Spark](https://people.apache.org/~pwendell/spark-releases/latest/streaming-programming-guide.html)
* [Uruchamianie zadań Spark zdalnie za pomocą programu LIVY](apache-spark-livy-rest-interface.md)