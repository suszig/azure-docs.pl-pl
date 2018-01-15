---
title: "Korzystanie z platformy Apache Spark strukturę przesyłania strumieniowego z usługą Event Hubs w usłudze Azure HDInsight | Dokumentacja firmy Microsoft"
description: "Tworzenie Apache Spark przesyłania strumieniowego przykładowy sposób wysyłania strumienia danych do Centrum zdarzeń platformy Azure i będzie otrzymywać tych zdarzeń w klastrze Spark w usłudze HDInsight przy użyciu języka scala aplikacji."
keywords: "Apache spark przesyłania strumieniowego przesyłania strumieniowego spark, przykładowe spark, apache spark przesyłania strumieniowego, np. przykładowych azure Centrum zdarzeń, spark próbki"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: jgao
ms.openlocfilehash: e0486d2c5f78da1d1e4a12703f120eccef43c305
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2018
---
# <a name="apache-spark-structured-streaming-on-hdinsight-to-process-events-from-event-hubs"></a>Apache Spark strukturalnych przesyłania strumieniowego w usłudze HDInsight do przetwarzania zdarzeń z usługi Event Hubs

W tym artykule dowiesz się przetworzyć telemetrii w czasie rzeczywistym za pomocą strukturalnych przesyłania strumieniowego Spark. W tym celu należy wykonać następujące ogólne kroki:

1. Skompiluj i uruchom na lokalnej stacji roboczej do przykładowej aplikacji producent zdarzeń, która generuje zdarzenia do wysłania do usługi Event Hubs.
2. Użyj [powłoki Spark](apache-spark-shell.md) do definiowania i uruchom prostą aplikację strukturalnych przesyłania strumieniowego Spark.

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcja platformy Azure. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Klaster Apache Spark w usłudze HDInsight. Aby uzyskać instrukcje, zobacz [klastrów utworzyć Apache Spark w usłudze Azure HDInsight](apache-spark-jupyter-spark-sql.md).

* Przestrzeń nazw usługi Azure Event Hubs. Zobacz [tworzenie przestrzeni nazw usługi Azure Event Hubs](apache-spark-eventhub-streaming.md#create-an-azure-event-hub) Aby uzyskać więcej informacji.

* Oracle Java Development kit. Możesz zainstalować ją z [tutaj](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

* Apache Maven. Możesz pobrać go z [tutaj](https://maven.apache.org/download.cgi). Instrukcje dotyczące instalacji Maven są dostępne [tutaj](https://maven.apache.org/install.html).

## <a name="build-configure-and-run-the-event-producer"></a>Tworzenie, konfigurowanie i uruchamianie producent zdarzeń
To zadanie służy do klonowania przykładowej aplikacji, która tworzy przypadkowych zdarzeń i wysyła je do skonfigurowanych Centrum zdarzeń. Ta przykładowa aplikacja jest dostępna w witrynie GitHub pod [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).

1. Upewnij się, że zostały zainstalowane narzędzia git. Możesz pobrać z [pobiera GIT](http://www.git-scm.com/downloads). 
2. Otwórz wiersz polecenia lub powłoki terminalu i uruchom następujące polecenie w katalogu wybranym sklonować projektu.
        
        git clone https://github.com/hdinsight/eventhubs-sample-event-producer.git eventhubs-client-sample

3. Spowoduje to utworzenie nowy folder o nazwie eventhubs klienta próby. W ramach shell lub w wierszu polecenia przejdź do tego folderu.
4. Uruchom Maven do tworzenia aplikacji przy użyciu następującego polecenia:

          mvn package

5. W ramach shell lub w wierszu polecenia przejdź do katalogu docelowego jest tworzony, który zawiera plik ``com-microsoft-azure-eventhubs-client-example-0.2.0.jar``.
6. Następnie należy do zbudowania wiersza polecenia do wykonywania producent zdarzeń do Centrum zdarzeń. To zrobić, zastępując wartości w poleceniu w następujący sposób:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.azure.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "<namespace>" --eventhubs-name "hub1" --policy-name "RootManageSharedAccessKey" --policy-key "<policyKey>" --message-length 32 --thread-count 1 --message-count -1

7. Na przykład pełne polecenie będzie wyglądać podobnie do poniższej:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.azure.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "sparkstreaming" --eventhubs-name "hub1" --policy-name "RootManageSharedAccessKey" --policy-key "2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU=" --message-length 32 --thread-count 1 --message-count -1

8. Polecenia uruchamiania i jeśli konfiguracji jest poprawny za chwilę zostanie wyświetlony związanych ze zdarzeniami, który wysyła go do Centrum zdarzeń, podobne do następujących danych wyjściowych:

        Map('policyKey -> 2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU, 'eventhubsName -> hub1, 'policyName -> RootManageSharedAccessKey, 'eventhubsNamespace -> sparkstreaming, 'messageCount -> -1, 'messageLength -> 32, 'threadCount -> 1)
        Events per thread: -1 (-1 for unlimited)
        10 > Sun Jun 18 11:32:58 PDT 2017 > 1024 > 1024 > Sending event: ZZ93958saG5BUKbvUI9wHVmpuA2TrebS
        10 > Sun Jun 18 11:33:46 PDT 2017 > 2048 > 2048 > Sending event: RQorGRbTPp6U2wYzRSnZUlWEltRvTZ7R
        10 > Sun Jun 18 11:34:33 PDT 2017 > 3072 > 3072 > Sending event: 36Eoy2r8ptqibdlfCYSGgXe6ct4AyOX3
        10 > Sun Jun 18 11:35:19 PDT 2017 > 4096 > 4096 > Sending event: bPZma9V0CqOn6Hj9bhrrJT0bX2rbPSn3
        10 > Sun Jun 18 11:36:06 PDT 2017 > 5120 > 5120 > Sending event: H2TVD77HNTVyGsVcj76g0daVnYxN4Sqs

9. Pozostaw producent zdarzenia uruchomione podczas przejść do czynności.

## <a name="run-spark-shell-on-your-hdinsight-cluster"></a>Uruchom powłokę Spark w klastrze usługi HDInsight
W tym zadaniu zostanie SSH do węzła głównego klastra usługi HDInsight, uruchom powłokę Spark i uruchomić aplikację przesyłania strumieniowego Spark, która pobiera i przetwarza zdarzenia z centrów zdarzeń. 

W tym punkcie zapewne zechcesz z klastrem usługi HDInsight. Jeśli nie musisz czekać do momentu zakończenia aprowizacji. Gdy jest gotowy, kontynuować następujące czynności:

1. Połącz z klastrem usługi HDInsight przy użyciu protokołu SSH. Aby uzyskać instrukcje, zobacz [nawiązywanie połączenia z usługi HDInsight przy użyciu protokołu SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

5. Tworzenia aplikacja wymaga pakietu centra zdarzeń przesyłania strumieniowego Spark. Uruchomienie powłoki Spark, tak aby automatycznie pobiera tę zależność od [Maven centralnej](https://search.maven.org), upewnij się, podaj pakiety przełącznik z współrzędne Maven w następujący sposób:

        spark-shell --packages "com.microsoft.azure:spark-streaming-eventhubs_2.11:2.1.5"

6. Po zakończeniu powłoki Spark ładowania, powinny pojawić się:

        Welcome to
            ____              __
            / __/__  ___ _____/ /__
            _\ \/ _ \/ _ `/ __/  '_/
        /___/ .__/\_,_/_/ /_/\_\   version 2.1.1.2.6.2.3-1
            /_/
                
        Using Scala version 2.11.8 (OpenJDK 64-Bit Server VM, Java 1.8.0_151)
        Type in expressions to have them evaluated.
        Type :help for more information.

        scala> 

7. Skopiuj poniższy fragment kodu w edytorze tekstu, a następnie zmodyfikować go, dzięki czemu ma on klucza zasad i Namespace ustawiony odpowiednio do Centrum zdarzeń.

        val eventhubParameters = Map[String, String] (
            "eventhubs.policyname" -> "RootManageSharedAccessKey",
            "eventhubs.policykey" -> "<policyKey>",
            "eventhubs.namespace" -> "<namespace>",
            "eventhubs.name" -> "hub1",
            "eventhubs.partition.count" -> "2",
            "eventhubs.consumergroup" -> "$Default",
            "eventhubs.progressTrackingDir" -> "/eventhubs/progress",
            "eventhubs.sql.containsProperties" -> "true"
            )
            
8. Jeśli przyjrzymy się punkt końcowy zgodnego EventHub w następującym formacie, część które odczytuje `iothub-xxxxxxxxxx` oznacza nazwę Namespace EventHub zgodny i może służyć do `eventhubs.namespace`. Pole `SharedAccessKeyName` może służyć do `eventhubs.policyname`, i `SharedAccessKey` dla `eventhubs.policykey`: 

        Endpoint=sb://iothub-xxxxxxxxxx.servicebus.windows.net/;SharedAccessKeyName=xxxxx;SharedAccessKey=xxxxxxxxxx 

9. Wkleić fragment zmodyfikowane scala oczekiwania > wierszu polecenia i naciśnij klawisz return. Powinny zostać wyświetlone dane wyjściowe podobne do poniższych:

        scala> val eventhubParameters = Map[String, String] (
            |       "eventhubs.policyname" -> "RootManageSharedAccessKey",
            |       "eventhubs.policykey" -> "2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU",
            |       "eventhubs.namespace" -> "sparkstreaming",
            |       "eventhubs.name" -> "hub1",
            |       "eventhubs.partition.count" -> "2",
            |       "eventhubs.consumergroup" -> "$Default",
            |       "eventhubs.progressTrackingDir" -> "/eventhubs/progress",
            |       "eventhubs.sql.containsProperties" -> "true"
            |     )
        eventhubParameters: scala.collection.immutable.Map[String,String] = Map(eventhubs.sql.containsProperties -> true, eventhubs.name -> hub1, eventhubs.consumergroup -> $Default, eventhubs.partition.count -> 2, eventhubs.progressTrackingDir -> /eventhubs/progress, eventhubs.policykey -> 2P1Q17Wd1rdLP1OZQYn6dD2S13Bb3nF3h2XZD9hvyyU, eventhubs.namespace -> hdiz-docs-eventhubs, eventhubs.policyname -> RootManageSharedAccessKey)

10. Następnie zacząć tworzyć Spark strukturalnych przesyłania strumieniowego zapytania można określania źródła. Wklej następujący kod do powłoki Spark, a następnie naciśnij klawisz return.

        val inputStream = spark.readStream.
        format("eventhubs").
        options(eventhubParameters).
        load()

11. Powinny zostać wyświetlone dane wyjściowe podobne do poniższych:

        inputStream: org.apache.spark.sql.DataFrame = [body: binary, offset: bigint ... 5 more fields]

12. Następnie możesz tworzyć zapytania tak, aby go zapisuje dane wyjściowe do konsoli. W tym wklejając następujący kod do powłoki Spark i naciskając klawisz return.

        val streamingQuery1 = inputStream.writeStream.
        outputMode("append").
        format("console").start().awaitTermination()

13. Niektóre partie zaczynać się dane wyjściowe podobne do następujących powinny być widoczne

        -------------------------------------------
        Batch: 0
        -------------------------------------------
        [Stage 0:>                                                          (0 + 2) / 2]

14. Następuje to wyniki danych wyjściowych przetwarzania każdego microbatch zdarzenia. 

        -------------------------------------------
        Batch: 0
        -------------------------------------------
        17/06/18 18:57:39 WARN TaskSetManager: Stage 1 contains a task of very large size (419 KB). The maximum recommended task size is 100 KB.
        +--------------------+------+---------+------------+---------+------------+----------+
        |                body|offset|seqNumber|enqueuedTime|publisher|partitionKey|properties|
        +--------------------+------+---------+------------+---------+------------+----------+
        |[7B 22 74 65 6D 7...|     0|        0|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   112|        1|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   224|        2|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   336|        3|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   448|        4|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   560|        5|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   672|        6|  1497734887|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   784|        7|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|   896|        8|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1008|        9|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1120|       10|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1232|       11|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1344|       12|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1456|       13|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1568|       14|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1680|       15|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1792|       16|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  1904|       17|  1497734888|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  2016|       18|  1497734889|     null|        null|     Map()|
        |[7B 22 74 65 6D 7...|  2128|       19|  1497734889|     null|        null|     Map()|
        +--------------------+------+---------+------------+---------+------------+----------+
        only showing top 20 rows

15. Nadejścia nowych zdarzeń od producenta zdarzeń są przetwarzane przez to zapytanie strukturalnych przesyłania strumieniowego.
16. Pamiętaj usunąć z klastrem usługi HDInsight, po zakończeniu uruchomione w tym przykładzie.



## <a name="see-also"></a>Zobacz także

* [Omówienie przesyłania strumieniowego Spark](apache-spark-streaming-overview.md)













