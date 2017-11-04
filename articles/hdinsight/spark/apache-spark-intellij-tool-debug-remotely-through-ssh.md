---
title: "Azure Toolkit for IntelliJ: aplikacji Spark debugować zdalnie za pośrednictwem SSH | Dokumentacja firmy Microsoft"
description: "Wskazówki krok po kroku dotyczące sposobu używania narzędzi HDInsight Tools w zestawie narzędzi Azure for IntelliJ debugowanie aplikacji zdalnie w usłudze HDInsight clusters za pośrednictwem SSH"
keywords: zdalne debugowanie intellij, zdalnego debugowania intellij, ssh, intellij, hdinsight, debugowania intellij, debugowania
services: hdinsight
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 08/24/2017
ms.author: jejiang
ms.openlocfilehash: cebbe2a0e28d49c93d0ebf12cc04b3d201dcec97
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>Debugowanie aplikacji Spark w klastrze usługi HDInsight narzędzi Azure for IntelliJ za pośrednictwem SSH

Ten artykuł zawiera wskazówki krok po kroku dotyczące sposobu używania narzędzi HDInsight Tools w zestawie narzędzi Azure for IntelliJ debugowanie aplikacji zdalnie w klastrze usługi HDInsight. Aby debugować projektu, można również wyświetlić [aplikacji Spark w usłudze HDInsight debugowania narzędzi Azure for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) wideo.

**Wymagania wstępne**

* **Narzędzia HDInsight Tools w Azure Toolkit for IntelliJ**. To narzędzie jest częścią zestawu narzędzi Azure for IntelliJ. Aby uzyskać więcej informacji, zobacz [zainstalować zestaw narzędzi platformy Azure dla IntelliJ](https://docs.microsoft.com/azure/azure-toolkit-for-intellij-installation).
* **Azure Toolkit for IntelliJ**. Ten zestaw narzędzi umożliwia tworzenie aplikacji Spark dla klastra usługi HDInsight. Aby uzyskać więcej informacji, postępuj zgodnie z instrukcjami [użyj narzędzi Azure dla IntelliJ tworzenie aplikacji dla klastra usługi HDInsight Spark](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).
* **Usługa HDInsight SSH z nazwy użytkownika i hasła zarządzania**. Aby uzyskać więcej informacji, zobacz [nawiązywanie połączenia z usługą HDInsight (Hadoop) przy użyciu protokołu SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) i [używanie protokołu SSH tunneling Ambari dostępu do sieci web UI, JobHistory, NameNode, Oozie i innych sieci web UI](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel). 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a>Tworzenie aplikacji Spark Scala i skonfiguruj ją do zdalnego debugowania

1. Uruchom IntelliJ IDEA, a następnie utworzyć projekt. W **nowy projekt** okno dialogowe, wykonaj następujące czynności:

   a. Wybierz **HDInsight**. 

   b. Wybierz język Java lub Scala szablon na podstawie preferencji użytkownika. Wybierz jedną z następujących opcji:

      - **Platforma Spark w usłudze HDInsight (Scala)**

      - **Platforma Spark w usłudze HDInsight (Java)**

      - **Platforma Spark dla klastra usługi HDInsight wykonywania próbki (Scala)**

      W tym przykładzie użyto **Spark dla próbki Uruchom klastra usługi HDInsight (Scala)** szablonu.

   c. W **narzędzia kompilacji** listy, wybierz jedną z następujących czynności, w zależności od potrzeb:

      - **Maven**, obsługę języka Scala Kreator tworzenia projektu

      -  **SBT**, do zarządzania zależności i budowania projektu języka Scala 

      ![Tworzenie projektu debugowania](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-create-projectfor-debug-remotely.png)

   d. Wybierz **dalej**.     
 
3. W następnej **nowy projekt** okna, wykonaj następujące czynności:

   ![Wybierz Spark zestawu SDK](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-new-project.png)

   a. Wprowadź nazwę projektu i lokalizacja projektu.

   b. W **SDK projektu** listy rozwijanej wybierz **Java 1.8** dla **Spark 2.x** klastra lub wybierz **Java 1.7** dla **Spark 1.x** klastra.

   c. W **wersji Spark** listy rozwijanej, Kreator tworzenia projektu Scala integruje poprawnej wersji dla platformy Spark zestawy SDK i Scala. Jeśli wersja klastra spark jest starsza niż 2.0, wybierz **Spark 1.x**. W przeciwnym razie wybierz **Spark 2.x.** W tym przykładzie użyto **Spark pkt 2.0.2 (Scala 2.11.8)**.

   d. Wybierz pozycję **Finish** (Zakończ).

4. Wybierz **src** > **głównego** > **scala** otworzyć kodu w projekcie. W tym przykładzie użyto **SparkCore_wasbloTest** skryptu.

5. Aby uzyskać dostęp do **Edytuj konfiguracje** menu, wybierz ikonę w prawym górnym rogu. Z tego menu można utworzyć lub edytować konfiguracji do zdalnego debugowania.

   ![Edycja konfiguracji](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-edit-configurations.png) 

6. W **konfiguracji uruchomienia/debugowania** oknie dialogowym wybierz znak plus (**+**). Następnie wybierz **Prześlij zadanie Spark** opcji.

   ![Dodawanie nowej konfiguracji](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-add-new-Configuration.png)
7. Wprowadź informacje dotyczące **nazwa**, **klastra Spark**, i **Nazwa klasy głównym**. Następnie wybierz **konfiguracji zaawansowanej**. 

   ![Uruchom konfiguracje debugowania](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-run-debug-configurations.png)

8. W **Spark przesyłanie Advanced Configuration** okno dialogowe, wybierz opcję **Spark włączenia debugowania zdalnego**. Wprowadź nazwę użytkownika SSH, a następnie wprowadź hasło lub użyć pliku klucza prywatnego. Aby zapisać konfigurację, zaznacz **OK**.

   ![Włączanie debugowania zdalnego Spark](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-enable-spark-remote-debug.png)

9. Konfiguracja jest teraz zapisywane o nazwie podane. Aby wyświetlić szczegóły konfiguracji, wybierz nazwę konfiguracji. Aby wprowadzić zmiany, wybierz **Edytuj konfiguracje**. 

10. Po ukończeniu ustawienia konfiguracji, można uruchomić projektu na zdalny klaster lub przeprowadzić debugowania zdalnego.

## <a name="learn-how-to-perform-remote-debugging"></a>Dowiedz się, jak przeprowadzić debugowanie zdalne
### <a name="scenario-1-perform-remote-run"></a>Scenariusz 1: Wykonania zdalnego wykonywania

W tej sekcji możemy opisano, jak debugować sterowniki i modułów.

    import org.apache.spark.{SparkConf, SparkContext}

    object LogQuery {
      val exampleApacheLogs = List(
        """10.10.10.10 - "FRED" [18/Jan/2013:17:56:07 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 315 "http://referall.com/" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.350 "-" - "" 265 923 934 ""
          | 62.24.11.25 images.com 1358492167 - Whatup""".stripMargin.lines.mkString,
        """10.10.10.10 - "FRED" [18/Jan/2013:18:02:37 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 306 "http:/referall.com" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.352 "-" - "" 256 977 988 ""
          | 0 73.23.2.15 images.com 1358492557 - Whatup""".stripMargin.lines.mkString
      )
      def main(args: Array[String]) {
        val sparkconf = new SparkConf().setAppName("Log Query")
        val sc = new SparkContext(sparkconf)
        val dataSet = sc.parallelize(exampleApacheLogs)
        // scalastyle:off
        val apacheLogRegex =
          """^([\d.]+) (\S+) (\S+) \[([\w\d:/]+\s[+\-]\d{4})\] "(.+?)" (\d{3}) ([\d\-]+) "([^"]+)" "([^"]+)".*""".r
        // scalastyle:on
        /** Tracks the total query count and number of aggregate bytes for a particular group. */
        class Stats(val count: Int, val numBytes: Int) extends Serializable {
          def merge(other: Stats): Stats = new Stats(count + other.count, numBytes + other.numBytes)
          override def toString: String = "bytes=%s\tn=%s".format(numBytes, count)
        }
        def extractKey(line: String): (String, String, String) = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              if (user != "\"-\"") (ip, user, query)
              else (null, null, null)
            case _ => (null, null, null)
          }
        }
        def extractStats(line: String): Stats = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              new Stats(1, bytes.toInt)
            case _ => new Stats(1, 0)
          }
        }
        
        dataSet.map(line => (extractKey(line), extractStats(line)))
          .reduceByKey((a, b) => a.merge(b))
          .collect().foreach{
          case (user, query) => println("%s\t%s".format(user, query))}

        sc.stop()
      }
    }


1. Ustawianie punktów podziału, a następnie wybierz **debugowania** ikony.

   ![Wybierz ikonę debugowania](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-icon.png)

2. Podczas wykonywania programu osiągnie punkt podziału, zobacz **sterownika** kartę i dwa **Moduł wykonujący** tabulatorów w **debugera** okienka. Wybierz **Program Wznów** ikonę, aby kontynuować wykonywanie kodu, który następnie osiągnie następnego punktu przerwania i skupiono się w odpowiedniej **Moduł wykonujący** kartę. Możesz przejrzeć dzienniki wykonywania w odpowiedniej **konsoli** kartę.

   ![Karta debugowanie](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debugger-tab.png)

### <a name="scenario-2-perform-remote-debugging-and-bug-fixing"></a>Scenariusz 2: Wykonaj debugowanie zdalne i naprawienie usterki
W tej sekcji możemy opisano, jak dynamicznie Aktualizuj wartości zmiennej przy użyciu możliwości debugowania IntelliJ poprawkę proste. W poniższym przykładzie kodu jest zgłaszany wyjątek, ponieważ plik docelowy już istnieje.
  
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext

        object SparkCore_WasbIOTest {
          def main(arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkCore_WasbIOTest")
            val sc = new SparkContext(conf)
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            // Find the rows that have only one digit in the sixth column.
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)

            try {
              var target = "wasb:///HVACout2_testdebug1";
              rdd1.saveAsTextFile(target);
            } catch {
              case ex: Exception => {
                throw ex;
              }
            }
          }
        }


#### <a name="to-perform-remote-debugging-and-bug-fixing"></a>Do wykonania zdalnego debugowania i naprawienie usterki
1. Skonfiguruj dwa punkty podziału, a następnie wybierz **debugowania** ikonę, aby rozpocząć proces zdalnego debugowania.

2. Kod zatrzymuje się w pierwszym punkcie podziału, a parametr, a informacje o zmiennych są wyświetlane w **zmienne** okienka. 

3. Wybierz **Program Wznów** ikonę, aby kontynuować. Kod zatrzymuje się na drugi punkt. Wyjątek jest zgodnie z oczekiwaniami.

  ![Zgłoś błąd](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-throw-error.png) 

4. Wybierz **Program Wznów** ikonę ponownie. **HDInsight Spark przesyłanie** okno wyświetla błąd "zadanie uruchamianie nie powiodło się".

  ![Przesyłanie błędów](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-error-submission.png) 

5. Aby dynamicznie aktualizować wartość zmiennej przy użyciu IntelliJ debugowania możliwości, wybierz **debugowania** ponownie. **Zmienne** okienko zostanie wyświetlony ponownie. 

6. Kliknij prawym przyciskiem myszy docelowy **debugowania** , a następnie wybierz **ustaw wartość**. Następnie wprowadź nową wartość dla zmiennej. Następnie wybierz **Enter** można zapisać wartości. 

  ![Ustaw wartość](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-set-value.png) 

7. Wybierz **Program Wznów** ikonę, aby kontynuować do uruchomienia programu. Tym razem nie jest wyjątek. Widać, że projekt zostanie pomyślnie uruchomiony bez żadnych wyjątków.

  ![Debugowanie bez wyjątku](./media/apache-spark-intellij-tool-debug-remotely-through-ssh/hdinsight-debug-without-exception.png)

## <a name="seealso"></a>Następne kroki
* [Przegląd: platforma Apache Spark w usłudze Azure HDInsight](apache-spark-overview.md)

### <a name="demo"></a>Demonstracja
* Tworzenie projektu Scala (klip wideo): [tworzenie Spark Scala aplikacji](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Debugowanie zdalne (klip wideo): [użyj narzędzi Azure dla IntelliJ debugowanie aplikacji Spark zdalnie w klastrze usługi HDInsight](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Scenariusze
* [Platforma Spark w usłudze BI: wykonaj interakcyjna analiza danych przy użyciu platformy Spark w usłudze HDInsight z narzędzi do analizy Biznesowej](apache-spark-use-bi-tools.md)
* [Platforma Spark przy użyciu Machine Learning: Korzystanie z platformy Spark w usłudze HDInsight do analizy temperatury w budynku z użyciem danych HVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do przewidywania wyników kontroli żywności](apache-spark-machine-learning-mllib-ipython.md)
* [Przesyłanie strumieniowe Spark: Korzystanie z platformy Spark w usłudze HDInsight do tworzenia aplikacji przesyłania strumieniowego w czasie rzeczywistym](apache-spark-eventhub-streaming.md)
* [Analiza dzienników witryny sieci Web na platformie Spark w usłudze HDInsight](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Tworzenie i uruchamianie aplikacji
* [Tworzenie autonomicznych aplikacji przy użyciu języka Scala](../hdinsight-apache-spark-create-standalone-application.md)
* [Zdalne uruchamianie zadań w klastrze Spark przy użyciu programu Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Narzędzia i rozszerzenia
* [Zestaw narzędzi platformy Azure dla IntelliJ umożliwia tworzenie aplikacji Spark dla klastra usługi HDInsight](apache-spark-intellij-tool-plugin.md)
* [Debugowanie aplikacji Spark zdalnie za pośrednictwem sieci VPN przy użyciu zestawu narzędzi Azure for IntelliJ](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Użyj narzędzia HDInsight Tools for IntelliJ z Hortonworks piaskownicy](../hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Użyj narzędzia HDInsight Tools w zestawie narzędzi Azure dla programu Eclipse tworzenie aplikacji Spark](../hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Korzystanie z notesów Zeppelin w klastrze Spark w usłudze HDInsight](apache-spark-zeppelin-notebook.md)
* [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Korzystanie z zewnętrznych pakietów z notesami Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalacja oprogramowania Jupyter na komputerze i nawiązywanie połączenia z klastrem Spark w usłudze HDInsight](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Zarządzanie zasobami
* [Zarządzanie zasobami klastra Apache Spark w usłudze Azure HDInsight](apache-spark-resource-manager.md)
* [Śledzenie i debugowanie zadań uruchamianych w klastrze Apache Spark w usłudze HDInsight](apache-spark-job-debugging.md)
