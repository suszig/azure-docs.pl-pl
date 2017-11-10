---
title: "Użyj przesyłania strumieniowego z usługą Event Hubs w usłudze Azure HDInsight Spark Apache | Dokumentacja firmy Microsoft"
description: "Tworzenie Apache Spark przesyłania strumieniowego przykładowy sposób wysyłania strumienia danych do Centrum zdarzeń platformy Azure i będzie otrzymywać tych zdarzeń w klastrze Spark w usłudze HDInsight przy użyciu języka scala aplikacji."
keywords: "Apache spark przesyłania strumieniowego przesyłania strumieniowego spark, przykładowe spark, apache spark przesyłania strumieniowego, np. przykładowych azure Centrum zdarzeń, spark próbki"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 68894e75-3ffa-47bd-8982-96cdad38b7d0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: d0678388fea79797c3cb4cd84deeab827981ebff
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# <a name="apache-spark-streaming-process-data-from-azure-event-hubs-with-spark-cluster-on-hdinsight"></a>Platforma Apache Spark przesyłania strumieniowego: klastra przetwarzania danych z usługi Azure Event Hubs z platformy Spark w usłudze HDInsight

W tym artykule możesz tworzyć Apache Spark przesyłania strumieniowego próbka, która obejmuje następujące kroki:

1. Aplikacja autonomiczna umożliwia pozyskiwania komunikaty do usługi Azure Event Hub.

2. Z dwa różne podejścia można pobierać wiadomości z Centrum zdarzeń w czasie rzeczywistym za pomocą aplikacji uruchomionej w klastrze Spark w usłudze Azure HDInsight.

3. Tworzenie przesyłania strumieniowego potoków analitycznych do utrwalenia danych do innego magazynu systemów lub uzyskiwanie szczegółowych informacji z danych na bieżąco.

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcja platformy Azure. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Klaster Apache Spark w usłudze HDInsight. Aby uzyskać instrukcje, zobacz [klastrów utworzyć Apache Spark w usłudze Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="spark-streaming-concepts"></a>Przesyłanie strumieniowe Spark pojęcia

Aby uzyskać szczegółowy opis przesyłania strumieniowego Spark, zobacz [Apache Spark przesyłania strumieniowego omówienie](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). Klastra Spark w usłudze Azure HDInsight oferuje te same funkcje przesyłania strumieniowego.  

## <a name="what-does-this-solution-do"></a>Do czego służy to rozwiązanie?

W tym artykule Aby utworzyć przesyłania strumieniowego przykład Spark, wykonaj następujące czynności:

1. Tworzenie usługi Azure Event Hub, który będzie otrzymywał strumienia zdarzeń.

2. Uruchom aplikacja autonomiczna lokalnej, która generuje zdarzenia i wypychanie go do Centrum zdarzeń platformy Azure. Przykładowej aplikacji, w tym jest opublikowana w [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).

3. Zdalne uruchamianie przesyłania strumieniowego aplikacji w klastrze Spark, które odczytuje wydarzeń transmisji strumieniowej z Azure Centrum zdarzeń i przeprowadzania różnych przetwarzania danych/analizy.

## <a name="create-an-azure-event-hub"></a>Tworzenie Centrum zdarzeń platformy Azure

1. Zaloguj się do [Azure Portal](https://ms.portal.azure.com)i kliknij przycisk **nowy** w lewym górnym rogu ekranu.

2. Kliknij pozycję **Internet rzeczy**, a następnie kliknij pozycję **Event Hubs**.

    ![Tworzenie Centrum zdarzeń do przesyłania strumieniowego przykład Spark](./media/apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "tworzenia Centrum zdarzeń do przesyłania strumieniowego przykład Spark")

3. W bloku **Tworzenie przestrzeni nazw** wprowadź nazwę przestrzeni nazw. Wybierz warstwę cenową (Basic lub Standard). Ponadto wybierz subskrypcję platformy Azure, grupę zasobów i lokalizację, w której chcesz utworzyć zasób. Kliknij pozycję **Utwórz**, aby utworzyć przestrzeń nazw.

      ![Podaj nazwę Centrum zdarzeń przesyłania strumieniowego przykład Spark](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "Podaj nazwę Centrum zdarzeń przesyłania strumieniowego przykład Spark")

    > [!NOTE]
    > Należy wybrać takie same **lokalizacji** jako klastra Apache Spark w usłudze HDInsight w celu zmniejszenia opóźnienia i koszty.
    >
    >

4. Na liście przestrzeni nazw usługi Event Hubs kliknij nowo utworzoną przestrzeń nazw.      


5. W bloku przestrzeni nazw, kliknij **usługi Event Hubs**, a następnie kliknij przycisk **+ Centrum zdarzeń** do utworzenia nowego Centrum zdarzeń.
   
    ![Tworzenie Centrum zdarzeń do przesyłania strumieniowego przykład Spark](./media/apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "tworzenia Centrum zdarzeń do przesyłania strumieniowego przykład Spark")

6. Wpisz nazwę Centrum zdarzeń, ustaw liczbę partycji do 10 i przechowywania wiadomości do 1. Firma Microsoft nie są archiwizacji z wiadomości w tym rozwiązaniu, pozostaw rest jako domyślne, a następnie kliknij przycisk **Utwórz**.
   
    ![Podaj szczegóły zdarzenia koncentratora do przesyłania strumieniowego przykład Spark](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "podanie szczegółów Centrum zdarzeń przesyłania strumieniowego przykład Spark")

7. Nowo utworzone Centrum zdarzeń jest wyświetlane w bloku Centrum zdarzeń.
    
     ![Przeglądanie Centrum zdarzeń pod kątem przesyłania strumieniowego przykład Spark](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "Widok Centrum zdarzeń do przesyłania strumieniowego przykład Spark")

8. W bloku przestrzeni nazw (nie w bloku centrum zdarzeń) kliknij pozycję **Zasady dostępu współdzielonego**, a następnie kliknij pozycję **RootManageSharedAccessKey**.
    
     ![Ustawianie zasad Centrum zdarzeń dla przesyłania strumieniowego przykład Spark](./media/apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "ustawić Centrum zdarzeń zasady dotyczące przesyłania strumieniowego przykład Spark")

9. Kliknij przycisk Kopiuj, aby skopiować **RootManageSharedAccessKey** podstawowego klucza i parametry połączenia do Schowka. Zapisywanie ich w dalszej części tego samouczka.
    
     ![Wyświetlanie kluczy zasad Centrum zdarzeń do przesyłania strumieniowego przykład Spark](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "zasad Centrum zdarzeń widoku kluczy, na przykład przesyłania strumieniowego Spark")

## <a name="send-messages-to-azure-event-hub-using-a-sample-scala-application"></a>Wysyłanie komunikatów do Centrum zdarzeń platformy Azure przy użyciu języka Scala przykładowej aplikacji

W tej sekcji służy lokalnego Scala aplikacja autonomiczna generuje strumienia zdarzeń i wysyła go do Centrum zdarzeń platformy Azure, który został utworzony wcześniej. Ta aplikacja jest dostępna w witrynie GitHub pod [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer). Kroki opisane w tym miejscu założono ma repozytorium GitHub już rozwidlone.

1. Upewnij się, że masz następujące zainstalowane na komputerze, na którym należy uruchomić tę aplikację.

    * Oracle Java Development kit. Możesz zainstalować ją z [tutaj](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
    * Apache Maven. Możesz pobrać go z [tutaj](https://maven.apache.org/download.cgi). Instrukcje dotyczące instalacji Maven są dostępne [tutaj](https://maven.apache.org/install.html).

2. Otwórz wiersz polecenia i przejdź do lokalizacji sklonować repozytorium GitHub dla przykładowej aplikacji Scala i uruchom następujące polecenie w celu skompilowania aplikacji.

        mvn package

3. Jar danych wyjściowych dla aplikacji, **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, utworzony na podstawie **/target** katalogu. Kompletne rozwiązanie testowania za pomocą tego JAR w dalszej części tego artykułu.

## <a name="create-application-to-receive-messages-from-event-hub-into-a-spark-cluster"></a>Tworzenie aplikacji na odbieranie komunikatów z Centrum zdarzeń do klastra Spark 

Mamy dwa podejścia do połączenia przesyłania strumieniowego Spark i usługi Azure Event Hubs, połączenie oparte na odbiorcy i oparte na DStream bezpośrednie połączenie. Na podstawie DStream bezpośrednio na stycznia 2017 wprowadzono w 2.0.3 wersji. Jest domniemana na zastąpienie oryginalnego połączenia oparte na odbiorcy, ponieważ jest on więcej wydajności i wydajne zasobów. Znaleziono więcej szczegółów w [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs). Bezpośrednie DStream obsługuje tylko Spark 2.0 +.

### <a name="build-applications-with-the-dependency-to-spark-eventhubs-connector"></a>Tworzenie aplikacji z zależnością łącznikiem spark eventhubs

Firma Microsoft opublikuje również przemieszczania wersji Spark EventHubs w usłudze GitHub. Aby użyć wersji przemieszczania Spark EventHubs, pierwszym krokiem jest wskazać GitHub jako źródło repozytorium, dodając następujący wpis do pom.xml:

```xml
<repository>
      <id>spark-eventhubs</id>
      <url>https://raw.github.com/hdinsight/spark-eventhubs/maven-repo/</url>
      <snapshots>
        <enabled>true</enabled>
        <updatePolicy>always</updatePolicy>
      </snapshots>
</repository>
```

Następnie można dodać następującej zależności do projektu do podjęcia wersji wstępnej.

Zależność narzędzia maven

```xml
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11 -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-streaming-eventhubs_2.11</artifactId>
    <version>2.0.4</version>
</dependency>
```

Zależności SBT

```
// https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11
libraryDependencies += "com.microsoft.azure" % "spark-streaming-eventhubs_2.11" % "2.0.4"
```

### <a name="direct-dstream-connection"></a>Bezpośrednie połączenie DStream

Można pobrać plik jar wbudowanych zawierający przykłady użycia bezpośredniego DStream w [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).

Plik jar zawiera trzy przykłady, których kodu źródłowego są dostępne pod adresem [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).

Biorąc [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) na przykład:

```scala
private def createStreamingContext(
  sparkCheckpointDir: String,
  batchDuration: Int,
  namespace: String,
  eventHunName: String,
  eventhubParams: Map[String, String],
  progressDir: String) = {
val ssc = new StreamingContext(new SparkContext(), Seconds(batchDuration))
ssc.checkpoint(sparkCheckpointDir)
val inputDirectStream = EventHubsUtils.createDirectStreams(
  ssc,
  namespace,
  progressDir,
  Map(eventHunName -> eventhubParams))

inputDirectStream.map(receivedRecord => (new String(receivedRecord.getBody), 1)).
  reduceByKeyAndWindow((v1, v2) => v1 + v2, (v1, v2) => v1 - v2, Seconds(batchDuration * 3),
    Seconds(batchDuration)).print()

ssc
}

def main(args: Array[String]): Unit = {

if (args.length != 8) {
  println("Usage: program progressDir PolicyName PolicyKey EventHubNamespace EventHubName" +
    " BatchDuration(seconds) Spark_Checkpoint_Directory maxRate")
  sys.exit(1)
}

val progressDir = args(0)
val policyName = args(1)
val policykey = args(2)
val namespace = args(3)
val name = args(4)
val batchDuration = args(5).toInt
val sparkCheckpointDir = args(6)
val maxRate = args(7)

val eventhubParameters = Map[String, String] (
  "eventhubs.policyname" -> policyName,
  "eventhubs.policykey" -> policykey,
  "eventhubs.namespace" -> namespace,
  "eventhubs.name" -> name,
  "eventhubs.partition.count" -> "32",
  "eventhubs.consumergroup" -> "$Default",
  "eventhubs.maxRate" -> s"$maxRate"
)

val ssc = StreamingContext.getOrCreate(sparkCheckpointDir,
  () => createStreamingContext(sparkCheckpointDir, batchDuration, namespace, name,
    eventhubParameters, progressDir))

ssc.start()
ssc.awaitTermination()
}
```

W powyższym przykładzie `eventhubParameters` są specyficzne dla pojedynczego wystąpienia EventHubs parametrów i trzeba przekazać go do `createDirectStreams` interfejs API, który tworzy bezpośrednie DStream obiektu mapowania do przestrzeni nazw usługi Event Hubs. Za pośrednictwem obiektu DStream bezpośredniego należy wywołać wszystkich API DStream dostarczonych przez strukturę interfejsu API platformy Spark przesyłania strumieniowego. W tym przykładzie firma Microsoft obliczyć częstotliwości każdego wyrazu w ciągu ostatnich interwałów 3 micro partii.

### <a name="receiver-based-connection"></a>Odbiornik połączenia

Spark, przesyłanie strumieniowe przykładowej aplikacji napisanych w języku Scala, który odbiera zdarzenia i trasy do różnych miejsc docelowych, znajduje się w temacie [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples). Wykonaj poniższe kroki, aby zaktualizować aplikacji dla danej konfiguracji Centrum zdarzeń i utworzyć jar danych wyjściowych.

1. Uruchom IntelliJ IDEA i wybierz z ekranu startowego **wyewidencjonowania z kontroli wersji** , a następnie kliknij przycisk **Git**.
   
    ![Apache Spark przesyłania strumieniowego przykład — Pobierz źródła z repozytorium Git](./media/apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark przesyłania strumieniowego przykład — Pobierz źródła z repozytorium Git")

2. W **klonowania repozytorium** okna dialogowego wprowadź adres URL repozytorium Git clone z, określ katalog w klonowania do, a następnie kliknij przycisk **klonowania**.
   
    ![Apache Spark przesyłania strumieniowego przykład — klonować z Git](./media/apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark przesyłania strumieniowego przykład — klonować z Git")
3. Postępuj zgodnie z monitami, dopóki projektu jest całkowicie sklonować. Naciśnij klawisz **Alt + 1** otworzyć **widoku projektu**. Powinien on przypominać następujące czynności.
   
    ![Apache Spark przesyłania strumieniowego przykład — widok projektu](./media/apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark przesyłania strumieniowego przykład — widok projektu")
4. Upewnij się, że kod aplikacji jest skompilowana przy użyciu Java8. Aby to zapewnić, kliknij przycisk **pliku**, kliknij przycisk **struktury projektu**i na **projektu** , upewnij się, że projekt ma ustawioną wartość poziomu języka **8 - wyrażeń lambda, adnotacje typu itp**.
   
    ![Apache Spark przesyłania strumieniowego przykład — zestaw kompilatora](./media/apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark przesyłania strumieniowego przykład — zestaw kompilatora")
5. Otwórz **pom.xml** i upewnij się, że wersja Spark jest poprawna. W obszarze `<properties>` węzła, Wyszukaj następujący fragment kodu i sprawdź wersję platformy Spark.

        <scala.version>2.11.8</scala.version>
        <scala.compat.version>2.11.8</scala.compat.version>
        <scala.binary.version>2.11</scala.binary.version>
        <spark.version>2.0.0</spark.version>

6. Aplikacja wymaga jar zależności, nazywany **jar sterownik JDBC**. Jest to wymagane w celu zapisywania komunikatów odebranych z Centrum zdarzeń do bazy danych Azure SQL. Możesz pobrać ten jar (4.1 lub nowszym) z [tutaj](https://msdn.microsoft.com/sqlserver/aa937724.aspx). Dodaj odwołanie do tego jar w bibliotece projektu. Wykonaj następujące czynności:
     
     1. Z którym masz aplikacji, Otwórz okno IntelliJ IDEA, kliknij przycisk **pliku**, kliknij przycisk **struktury projektu**, a następnie kliknij przycisk **biblioteki**. 
     2. Kliknij ikonę add (![ikonę Dodaj](./media/apache-spark-eventhub-streaming/add-icon.png)), kliknij przycisk **Java**, a następnie przejdź do lokalizacji, do którego pobrano jar sterownik JDBC. Postępuj zgodnie z monitami, aby dodać plik jar do biblioteki projektu.

         ![Dodaj brakujące zależności](./media/apache-spark-eventhub-streaming/add-missing-dependency-jars.png "Dodaj brakujące słoików zależności")
     3. Kliknij przycisk **Zastosuj**.

7. Utwórz plik jar danych wyjściowych. Wykonaj poniższe kroki.

   1. W **struktury projektu** okno dialogowe, kliknij przycisk **artefakty** , a następnie kliknij znak plus. W wyświetlonym oknie dialogowym, kliknij polecenie **JAR**, a następnie kliknij przycisk **z modułów z zależnościami**.      
       
       ![Przykład strumieniowych Apache Spark — tworzenie JAR](./media/apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "przykład strumieniowych Apache Spark — tworzenie JAR")
   2. W **utworzyć JAR z modułów** okna dialogowego, kliknij przycisk wielokropka (![wielokropka](./media/apache-spark-eventhub-streaming/ellipsis.png)) przed **klasy głównym**.
   3. W **Wybierz klasy głównym** okno dialogowe, wybierz jedno z dostępnych klas a następnie kliknij przycisk **OK**.
      
       ![Apache Spark przesyłania strumieniowego przykład — wybierz klasę dla jar](./media/apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark przesyłania strumieniowego przykład — wybierz klasę dla jar")
   4. W **utworzyć JAR z modułów** okna dialogowego upewnij się, że opcja **Wyodrębnij do docelowego JAR** jest zaznaczone, a następnie kliknij przycisk **OK**. Spowoduje to utworzenie jednego JAR z wszystkie zależności.
      
       ![Przykład strumieniowych Apache Spark — tworzenie jar z modułów](./media/apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "przykład strumieniowych Apache Spark — tworzenie jar z modułów")
   5. **Układu dane wyjściowe** karta zawiera listę wszystkich słoików, które są częścią projektu Maven. Można wybrać i usunąć te, na którym aplikacja Scala nie ma żadnych zależności bezpośrednich. Dla aplikacji jest tworzone w tym miejscu, należy usunąć wszystkie oprócz ostatnią (**danych wyjściowych kompilacji spark-przesyłania strumieniowego danych trwałości — przykłady**). Wybierz słoików usunąć, a następnie kliknij przycisk **usunąć** ikona (![ikona usuwania](./media/apache-spark-eventhub-streaming/delete-icon.png)).
      
       ![Apache Spark przesyłania strumieniowego przykład — usuń wyodrębnione słoików](./media/apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark przesyłania strumieniowego przykład — usuń wyodrębnione słoików")
      
       Upewnij się, że **kompilacji w upewnij** pole jest zaznaczone, co zapewnia, że jar jest tworzony za każdym razem, gdy projekt jest utworzony lub zaktualizowany. Kliknij przycisk **Zastosuj**.
   6. W **układu dane wyjściowe** karty u dołu **dostępne elementy** okno, masz jar SQL JDBC, które wcześniej dodano do biblioteki projektu. Należy dodać do **układu dane wyjściowe** kartę. Kliknij prawym przyciskiem myszy plik jar, a następnie kliknij przycisk **Wyodrębnij do głównego dane wyjściowe**.
      
       ![Apache Spark przesyłania strumieniowego przykład — jar zależności wyodrębniania](./media/apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark przesyłania strumieniowego przykład — jar zależności wyodrębniania")  
      
       **Układu dane wyjściowe** kartę powinna wyglądać następująco.
      
       ![Apache Spark przesyłania strumieniowego przykład — karta ostateczne dane wyjściowe](./media/apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark przesyłania strumieniowego przykład — karta ostateczne dane wyjściowe")        
      
       W **struktury projektu** okno dialogowe, kliknij przycisk **Zastosuj** , a następnie kliknij przycisk **OK**.    
   7. Na pasku menu kliknij **kompilacji**, a następnie kliknij przycisk **projektu należy**. Możesz również kliknąć **artefaktów kompilacji** utworzyć jar. Dane wyjściowe jar utworzony na podstawie **\classes\artifacts**.
      
       ![Przykład strumieniowych Apache Spark - output JAR](./media/apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "przykład strumieniowych Apache Spark - output JAR")

## <a name="run-the-application-remotely-on-a-spark-cluster-using-livy"></a>Zdalne uruchamianie aplikacji w klastrze Spark przy użyciu programu Livy

W tym artykule Livy używane do uruchamiania Apache Spark przesyłania strumieniowego aplikacji zdalnie w klastrze Spark. Aby uzyskać szczegółowe omówienie dotyczące sposobu używania programu Livy z klastrem Spark w usłudze HDInsight, zobacz [klastra zadania przesyłania zdalnie Apache Spark w usłudze Azure HDInsight](apache-spark-livy-rest-interface.md). Przed rozpoczęciem systemem przesyłania strumieniowego aplikacji Spark, istnieje kilka rzeczy, które należy wykonać:

1. Uruchom aplikację autonomiczny lokalnych w celu wygenerowania zdarzenia i wysyłane do Centrum zdarzeń. Aby to zrobić, użyj następującego polecenia:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "mysbnamespace" --eventhubs-name "myeventhub" --policy-name "mysendpolicy" --policy-key "<policy key>" --message-length 32 --thread-count 32 --message-count -1

2. Skopiuj jar przesyłania strumieniowego (**spark-przesyłania strumieniowego data trwałości examples.jar**) do magazynu obiektów Blob platformy Azure skojarzone z klastrem. Dzięki temu jar są dostępne dla programu Livy. Można użyć [ **AzCopy**](../../storage/common/storage-use-azcopy.md), narzędzie wiersza polecenia, aby to zrobić. Istnieje wiele innych klientów, których można użyć w celu przekazania danych. Można znaleźć więcej informacji na temat je pod adresem [przekazywanie danych dotyczących zadań Hadoop w usłudze HDInsight](../hdinsight-upload-data.md).
3. Zainstaluj CURL na komputerze, na którym uruchomiony jest program tych aplikacji. Używamy CURL do wywołania Livy punkty końcowe można zdalnie uruchamiać zadania.

### <a name="run-the-spark-streaming-application-to-receive-the-events-into-an-azure-storage-blob-as-text"></a>Uruchom Spark przesyłania strumieniowego aplikacji na odbieranie zdarzeń do obiektu Blob magazynu Azure jako tekst

Otwórz wiersz polecenia, przejdź do katalogu, w którym zostało zainstalowane narzędzie CURL i uruchom następujące polecenie (zastąp nazwę użytkownika/hasło i klaster nazwa):

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputBlob.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Parametry w pliku **inputBlob.txt** są zdefiniowane w następujący sposób:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsEventCount", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Daj nam zrozumieć, jakie są parametrów w pliku wejściowego:

* **plik** to ścieżka do pliku jar aplikacji na konto magazynu Azure skojarzony z klastrem.
* **className** jest nazwą klasy w jar.
* **argumenty** znajduje się lista argumentów wymaganych przez klasę
* **numExecutors** jest liczba rdzeni używane przez Spark, aby uruchomić aplikację przesyłania strumieniowego. Zawsze należy co najmniej dwa razy liczba partycji Centrum zdarzeń.
* **executorMemory**, **executorCores**, **driverMemory** parametry używane do przydzielenia zasobów wymaganych do przesyłania strumieniowego aplikacji.

> [!NOTE]
> Nie trzeba tworzyć foldery danych wyjściowych (EventCheckpoint, EventCount/EventCount10), które są używane jako parametry. Przesyłania strumieniowego aplikacji utworzy je.
>
>

Po uruchomieniu polecenia, powinny pojawić się dane wyjściowe podobne do następujących:

    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /18
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Tue, 01 Dec 2015 05:39:10 GMT
    < Content-Length: 37
    <
    {"id":1,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

Zanotuj identyfikator wsadu ostatni wiersz danych wyjściowych (w tym przykładzie jest '1'). Aby sprawdzić, czy aplikacja zostanie uruchomiona pomyślnie, można sprawdzić konta magazynu Azure skojarzony z klastrem i powinna zostać wyświetlona **/EventCount/EventCount10** folder utworzony istnieje. Ten folder powinien zawierać obiektów blob, które znajdują się liczba zdarzeń przetworzonych w ciągu okresu czasu określony w parametrze **partii interwał w sekundach**.

Przesyłania strumieniowego aplikacji Spark będzie kontynuował działanie aż do jego zakończenia. Aby to zrobić, użyj następującego polecenia:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/1"

### <a name="run-the-applications-to-receive-the-events-into-an-azure-storage-blob-as-json"></a>Uruchamianie aplikacji na odbieranie zdarzeń do obiektu Blob magazynu Azure w formacie JSON
Otwórz wiersz polecenia, przejdź do katalogu, w którym zostało zainstalowane narzędzie CURL i uruchom następujące polecenie (zastąp nazwę użytkownika/hasło i klaster nazwa):

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputJSON.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Parametry w pliku **inputJSON.txt** są zdefiniowane w następujący sposób:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureBlobAsJSON", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-store-folder", "/EventStore10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Parametry są podobne do określonych dla danych wyjściowych tekstu w poprzednim kroku. Nie trzeba ponownie utworzyć foldery danych wyjściowych (EventCheckpoint, EventCount/EventCount10), które są używane jako parametry. Przesyłania strumieniowego aplikacji utworzy je.

 Po uruchomieniu polecenia, można sprawdzić konta magazynu Azure skojarzony z klastrem i powinna zostać wyświetlona **/EventStore10** folder utworzony istnieje. Otwórz każdy plik i jest poprzedzony prefiksem **części -** i powinna zostać wyświetlona zdarzenia przetwarzane w formacie JSON.

### <a name="run-the-applications-to-receive-the-events-into-a-hive-table"></a>Uruchamianie aplikacji na odbieranie zdarzeń w tabeli programu Hive
Aby uruchomić przesyłania strumieniowego aplikacji, która strumieni zdarzeń w tabeli programu Hive Spark należy niektóre dodatkowe składniki. Są to:

* datanucleus-api-jdo-3.2.6.jar
* datanucleus-rdbms-3.2.9.jar
* datanucleus-core-3.2.10.jar
* gałąź site.xml

**JAR** pliki są dostępne w klastrze Spark w usłudze HDInsight w `/usr/hdp/current/spark-client/lib`. **Hive-site.xml** znajduje się w temacie `/usr/hdp/current/spark-client/conf`.

Można użyć [WinScp](http://winscp.net/eng/download.php) kopiować te pliki z klastra na komputerze lokalnym. Można następnie użyć narzędzia kopiować pliki do konta magazynu skojarzone z klastrem. Aby uzyskać więcej informacji na temat przekazywania plików do konta magazynu, zobacz [przekazywanie danych dotyczących zadań Hadoop w usłudze HDInsight](../hdinsight-upload-data.md).

Po skopiowaniu nad plikami z kontem magazynu platformy Azure, otwórz wiersz polecenia, przejdź do katalogu, w którym zostało zainstalowane narzędzie CURL i uruchom następujące polecenie (zastąp nazwę użytkownika/hasło i klaster nazwa):

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputHive.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Parametry w pliku **inputHive.txt** są zdefiniowane w następujący sposób:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToHiveTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-hive-table", "EventHiveTable10" ], "jars":["wasb:///example/jars/datanucleus-api-jdo-3.2.6.jar", "wasb:///example/jars/datanucleus-rdbms-3.2.9.jar", "wasb:///example/jars/datanucleus-core-3.2.10.jar"], "files":["wasb:///example/jars/hive-site.xml"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Parametry są podobne do określonego dla wyjścia tekstu, w poprzednich krokach. Ponownie, nie trzeba tworzyć foldery danych wyjściowych (EventCheckpoint, EventCount/EventCount10) lub dane wyjściowe tabeli Hive (EventHiveTable10), które są używane jako parametry. Przesyłania strumieniowego aplikacji utworzy je. Należy pamiętać, że **słoików** i **pliki** opcja zawiera ścieżki do plików JAR i hive-site.xml kopiowana za pośrednictwem konta magazynu.

Aby sprawdzić, czy tabeli hive została pomyślnie utworzona, użyj [widoku Hive narzędzia Ambari](../hadoop/apache-hadoop-use-hive-ambari-view.md). Można uruchomić zapytania SELECT, aby wyświetlić zawartość tej tabeli.

    SELECT * FROM eventhivetable10 LIMIT 10;

Powinny pojawić się dane wyjściowe podobne do następujących:

    ZN90apUSQODDTx7n6Toh6jDbuPngqT4c
    sor2M7xsFwmaRW8W8NDwMneFNMrOVkW1
    o2HcsU735ejSi2bGEcbUSB4btCFmI1lW
    TLuibq4rbj0T9st9eEzIWJwNGtMWYoYS
    HKCpPlWFWAJILwR69MAq863nCWYzDEw6
    Mvx0GQOPYvPR7ezBEpIHYKTKiEhYammQ
    85dRppSBSbZgThLr1s0GMgKqynDUqudr
    5LAWkNqorLj3ZN9a2mfWr9rZqeXKN4pF
    ulf9wSFNjD7BZXCyunozecov9QpEIYmJ
    vWzM3nvOja8DhYcwn0n5eTfOItZ966pa
    Time taken: 4.434 seconds, Fetched: 10 row(s)


### <a name="run-the-applications-to-receive-the-events-into-an-azure-sql-database-table"></a>Uruchamianie aplikacji na odbieranie zdarzeń w tabeli bazy danych Azure SQL
Przed uruchomieniem tego kroku, upewnij się, że masz utworzony bazę danych Azure SQL. Aby uzyskać instrukcje, zobacz [Utwórz bazę danych SQL w minutach](../../sql-database/sql-database-get-started-portal.md). Do wykonania w tej sekcji, potrzebne wartości dla nazwy bazy danych, nazwę serwera bazy danych i poświadczeń administratora bazy danych jako parametry. Nie trzeba jednak utworzyć tabeli bazy danych. Przesyłania strumieniowego aplikacji Spark utworzy który.

Otwórz wiersz polecenia, przejdź do katalogu, w którym zostało zainstalowane narzędzie CURL i uruchom następujące polecenie:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputSQL.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Parametry w pliku **inputSQL.txt** są zdefiniowane w następujący sposób:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureSQLTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--sql-server-fqdn", "<database-server-name>.database.windows.net", "--sql-database-name", "mysparkdatabase", "--database-username", "sparkdbadmin", "--database-password", "<put-password-here>", "--event-sql-table", "EventContent" ], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Aby sprawdzić, czy aplikacja zostanie uruchomiona pomyślnie, można połączyć z bazą danych Azure SQL przy użyciu programu SQL Server Management Studio. Aby uzyskać instrukcje, jak to zrobić, zobacz [Połącz z bazą danych SQL za pomocą programu SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md). Po nawiązaniu połączenia z bazą danych, można przejść do **EventContent** tabeli, która została utworzona przez przesyłania strumieniowego aplikacji. Można uruchomić szybkie zapytanie w celu pobrania danych z tabeli. Uruchom następującą kwerendę:

    SELECT * FROM EventCount

Powinny zostać wyświetlone dane wyjściowe podobne do następujących:

    00046b0f-2552-4980-9c3f-8bba5647c8ee
    000b7530-12f9-4081-8e19-90acd26f9c0c
    000bc521-9c1b-4a42-ab08-dc1893b83f3b
    00123a2a-e00d-496a-9104-108920955718
    0017c68f-7a4e-452d-97ad-5cb1fe5ba81b
    001KsmqL2gfu5ZcuQuTqTxQvVyGCqPp9
    001vIZgOStka4DXtud0e3tX7XbfMnZrN
    00220586-3e1a-4d2d-a89b-05c5892e541a
    0029e309-9e54-4e1b-84be-cd04e6fce5ec
    003333cf-874f-4045-9da3-9f98c2b4ea49
    0043c07e-8d73-420a-9af7-1fcb94575356
    004a11a9-0c2c-4bc0-a7d5-2e0ebd947ab9


## <a name="seealso"></a>Zobacz też
* [Przegląd: platforma Apache Spark w usłudze Azure HDInsight](apache-spark-overview.md)
* [Projekt połączenie oparte na odbiorcy i bezpośredniego DStream](https://www.slideshare.net/NanZhu/seattle-sparkmeetup032317)

### <a name="scenarios"></a>Scenariusze
* [Platforma Spark i analiza biznesowa: interakcyjna analiza danych na platformie Spark w usłudze HDInsight z użyciem narzędzi do analizy biznesowej](apache-spark-use-bi-tools.md)
* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do analizy temperatury w budynku z użyciem danych HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do przewidywania wyników kontroli żywności](apache-spark-machine-learning-mllib-ipython.md)
* [Analiza dzienników witryny sieci Web na platformie Spark w usłudze HDInsight](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Tworzenie i uruchamianie aplikacji
* [Tworzenie autonomicznych aplikacji przy użyciu języka Scala](../hdinsight-apache-spark-create-standalone-application.md)
* [Zdalne uruchamianie zadań w klastrze Spark przy użyciu programu Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Narzędzia i rozszerzenia
* [Tworzenie i przesyłanie aplikacji Spark Scala przy użyciu dodatku HDInsight Tools Plugin for IntelliJ IDEA](apache-spark-intellij-tool-plugin.md)
* [Zdalne debugowanie aplikacji Spark przy użyciu dodatku HDInsight Tools Plugin for IntelliJ IDEA](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Korzystanie z notesów Zeppelin w klastrze Spark w usłudze HDInsight](apache-spark-zeppelin-notebook.md)
* [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Korzystanie z zewnętrznych pakietów z notesami Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalacja oprogramowania Jupyter na komputerze i nawiązywanie połączenia z klastrem Spark w usłudze HDInsight](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Zarządzanie zasobami
* [Zarządzanie zasobami klastra Apache Spark w usłudze Azure HDInsight](apache-spark-resource-manager.md)
* [Śledzenie i debugowanie zadań uruchamianych w klastrze Apache Spark w usłudze HDInsight](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]: ../storage-create-storage-account/
