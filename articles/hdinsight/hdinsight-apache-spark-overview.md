<properties 
    pageTitle="Przegląd platformy Apache Spark w usłudze HDInsight | Microsoft Azure" 
    description="Wprowadzenie do platformy Apache Spark w usłudze HDInsight i scenariusze użycia platformy Spark w usłudze HDInsight w aplikacjach użytkownika." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="paulettm" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="06/06/2016" 
    ms.author="nitinme"/>

# Przegląd: platforma Apache Spark w usłudze HDInsight w systemie Linux
 
<a href="http://spark.apache.org/" target="_blank">Apache Spark</a> to platforma przetwarzania równoległego typu open source, która obsługuje przetwarzanie w pamięci w celu zwiększania wydajności aplikacji do analizy danych big data. Aparat przetwarzania Spark został opracowany po kątem szybkości, łatwości użycia i zaawansowanych możliwości analitycznych. Dzięki oferowanym funkcjom przetwarzania w pamięci platforma Spark stanowi dobry wybór w przypadku algorytmów iteracyjnych używanych w uczeniu maszynowym i obliczeniach na grafach. Platforma Spark jest również zgodna z magazynem obiektów blob Azure (WASB), co pozwala na łatwe przetwarzanie istniejących danych przechowywanych na platformie Azure za pośrednictwem platformy Spark.

Tworząc klaster Spark w usłudze HDInsight, tworzysz zasoby obliczeniowe systemu Azure z zainstalowaną i skonfigurowaną platformą Spark. Utworzenie klastra Spark w usłudze HDInsight trwa tylko około 10 minut. Dane, które mają być przetwarzane, są przechowywane w magazynie obiektów blob Azure. Zobacz temat [Korzystanie z usługi Azure Blob Storage z usługą HDInsight][hdinsight-storage].

![Platforma Apache Spark w usłudze Azure HDInsight](./media/hdinsight-apache-spark-overview/hdispark.architecture.png  "Apache Spark on Azure HDInsight")


**Chcesz rozpocząć korzystanie z platformy Apache Spark w usłudze Azure HDInsight?** Zobacz temat [Szybki start: tworzenie klastra Spark w usłudze HDInsight w systemie Linux i uruchamianie przykładowych aplikacji przy użyciu oprogramowania Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).

>[AZURE.NOTE] Lista znanych problemów i ograniczeń dotyczących bieżącej wersji znajduje się w artykule [Znane problemy platformy Apache Spark w usłudze Azure HDInsight (Linux)](hdinsight-apache-spark-jupyter-spark-sql.md).


## Dlaczego warto używać platformy Spark w usłudze Azure HDInsight? 

Usługa Azure HDInsight oferuje w pełni zarządzaną usługę platformy Spark. Korzyści z używania platformy Spark w usłudze HDInsight:

| Cecha                             | Opis       |
|-------------------------------------|-------------------|
| Łatwość tworzenia klastrów            | Nowy klaster Spark w usłudze HDInsight można utworzyć w kilka minut przy użyciu Portalu zarządzania Azure, programu Azure PowerShell lub zestawu .NET SDK usługi HDInsight. Zobacz temat [Wprowadzenie do klastra Spark w usłudze HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Łatwość obsługi                     | Platforma Spark w klastrach HDInsight obejmuje wstępnie skonfigurowane notesy Jupyter. Można ich używać do interakcyjnego przetwarzania danych i wizualizacji. Adresy URL to https://CLUSTERNAME.azurehdinsight.net/jupyter. Zastąp ciąg __CLUSTERNAME__ nazwą klastra Spark w usłudze HDInsight:|
| Interfejsy API REST                       | Platforma Spark w usłudze HDInsight obejmuje [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), czyli serwer zadań Spark oparty na interfejsach API REST do zdalnego przesyłania i monitorowania uruchomionych zadań. |
| Obsługa usługi Azure Data Lake Store | Platformę Spark w usłudze HDInsight można skonfigurować do korzystania z usługi Azure Data Lake Store jako dodatkowego magazynu. Aby uzyskać więcej informacji o usłudze Data Lake — magazyn, zobacz temat [Przegląd usługi Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
| Integracja z usługami Azure | Platforma Spark w usłudze HDInsight zawiera łącznik usługi Azure Event Hubs. Klienci mogą tworzyć aplikacje do przesyłania strumieniowego przy użyciu usługi Event Hubs w uzupełnieniu do oprogramowania [Kafka](http://kafka.apache.org/), które jest już dostępne w ramach platformy Spark. |
| Obsługa platformy R Server  | Możesz skonfigurować platformę R Server w klastrze Spark w usłudze HDInsight, aby uruchamiać rozproszone obliczenia R z szybkością zapewnianą przez klaster Spark. Aby uzyskać więcej informacji, zobacz temat [Rozpoczęcie pracy z platformą R Server w usłudze HDInsight](hdinsight-hadoop-r-server-get-started.md).   |
| Integracja z rozwiązaniem IntelliJ IDEA  | Dodatek HDInsight for IntelliJ umożliwia tworzenie i przesyłanie aplikacji do klastra Spark w usłudze HDInsight. Aby uzyskać więcej informacji, zobacz temat [Tworzenie aplikacji Spark dla klastra Spark w usłudze HDInsight w systemie Linux przy użyciu dodatku HDInsight Tools Plugin for IntelliJ IDEA](hdinsight-apache-spark-intellij-tool-plugin.md). |
| Zapytania jednoczesne              | Platforma Spark w usłudze HDInsight obsługuje zapytania jednoczesne. Dzięki temu wielu zapytań od jednego użytkownika lub wiele zapytań od różnych użytkowników i aplikacji może współdzielić te same zasoby klastra. |
| Buforowanie na dyskach SSD                 | Istnieje możliwość buforowania danych w pamięci lub na dyskach SSD podłączonych do węzłów klastra. Buforowanie w pamięci zapewnia najlepszą wydajność zapytań, ale może być kosztowne. Buforowanie na dyskach SSD stanowi doskonałe rozwiązanie umożliwiające poprawę wydajności zapytań bez konieczności tworzenia klastra o rozmiarze obejmującym cały zestaw danych w pamięci.|
| Integracja z narzędziami do analizy biznesowej       | Platforma Spark dla usługi HDInsight zawiera łączniki dla narzędzi do analizy biznesowej danych, takich jak [Power BI](http://www.powerbi.com/) i [Tableau](http://www.tableau.com/products/desktop).|
| Wstępnie załadowane biblioteki Anaconda        | Klastry Spark w usłudze HDInsight są dostarczane z wstępnie zainstalowanymi bibliotekami Anaconda. Platforma [Anaconda](http://docs.continuum.io/anaconda/) dostarcza prawie 200 bibliotek do uczenia maszynowego, analizy danych, wizualizacji itp.|
| Skalowalność                     | Chociaż można określić liczbę węzłów w klastrze podczas tworzenia, przydatna może okazać się możliwość zwiększania i zmniejszania klastra w celu dopasowania go do obciążeń. Wszystkie klastry usługi HDInsight umożliwiają zmianę liczby węzłów w klastrze. Ponadto klastry Spark można porzucić bez utraty danych, ponieważ wszystkie dane są przechowywane w usłudze Azure Blob Storage. |
| Całodobowa pomoc techniczna                    | Platforma Spark w usłudze HDInsight jest oferowana razem z całodobową (24/7) pomocą techniczną dla przedsiębiorstw oraz umową SLA gwarantującą 99,9% czasu działania.|



## Jakie są przypadki użycia platformy Spark w usłudze HDInsight?

Platforma Apache Spark w usłudze HDInsight umożliwia realizację następujących podstawowych scenariuszy.

### Interakcyjna analiza danych i analiza biznesowa

[Zobacz samouczek](hdinsight-apache-spark-use-bi-tools.md)

Platforma Apache Spark w usłudze HDInsight przechowuje dane w obiektach blob Azure. Eksperci biznesowi i osoby podejmujące kluczowe decyzje mogą analizować i tworzyć raporty na podstawie danych oraz korzystać z usługi Microsoft Power BI w celu sporządzania interakcyjnych raportów na podstawie analizowanych danych. Analitycy mogą rozpocząć pracę od danych o częściowej strukturze lub bez struktury w magazynie Azure, zdefiniować schemat danych za pomocą notesów, a następnie skompilować modele danych przy użyciu usługi Microsoft Power BI. Platforma Spark w usłudze HDInsight obsługuje również wiele narzędzi do analizy biznesowej innych firm, takich jak Tableau, Qlikview i SAP Lumira, dzięki czemu jest idealną platformą dla analityków danych, ekspertów biznesowych i osób podejmujących kluczowe decyzje.

### Iteracyjna usługa Machine Learning

[Zobacz samouczek: przewidywanie temperatur w budynkach z użyciem danych HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Zobacz samouczek: przewidywanie wyników inspekcji żywności](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Platforma Apache Spark jest dostarczana z biblioteką [MLlib](http://spark.apache.org/mllib/) do uczenia maszynowego opartą na platformie Spark. Oprócz tego platforma Spark w usłudze HDInsight obejmuje platformę Anaconda — dystrybucję oprogramowania Python z szeregiem różnych pakietów do uczenia maszynowego. Połączenie z wbudowaną obsługą notesów Jupyter zapewnia najwyższej jakości środowisko do tworzenia aplikacji do uczenia maszynowego.  

### Przesyłanie strumieniowe i analiza danych w czasie rzeczywistym

[Zobacz samouczek](hdinsight-apache-spark-eventhub-streaming.md)

Analiza danych w czasie rzeczywistym znajduje zastosowanie w różnych scenariuszach: od skracania czasu uzyskiwania wglądu w dane przez przetwarzanie spływających danych po opracowywanie rozwiązań do przesyłania strumieniowego. Platforma Spark w usłudze HDInsight zapewnia szeroką obsługę tworzenia rozwiązań do analizy w czasie rzeczywistym. Platforma Spark jest już wyposażona w łączniki do przyjmowania danych z wielu źródeł, takich jak Kafka, Flume, Twitter, ZeroMQ lub gniazda TCP, a ponadto platforma Spark w usłudze HDInsight oferuje wysokiej klasy obsługę pobierania danych z usługi Azure Event Hubs. Event Hubs to najczęściej używana usługa kolejkowania w systemie Azure. Dostępność wbudowanej obsługi usługi Event Hubs sprawia, że platforma Spark w usłudze HDInsight idealnie nadaje się do tworzenia potoku analizy w czasie rzeczywistym.

##<a name="next-steps"></a>Jakie składniki wchodzą w skład klastra Spark?

Platforma Spark w usłudze HDInsight obejmuje następujące składniki, które są domyślnie dostępne w klastrach.

- [Spark Core](https://spark.apache.org/docs/1.5.1/). Obejmuje takie składniki, jak Spark Core, Spark SQL, interfejsy API przesyłania strumieniowego Spark, GraphX oraz MLlib.
- [Anaconda](http://docs.continuum.io/anaconda/)
- [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Jupyter Notebook](https://jupyter.org)

Platforma Spark w usłudze HDInsight jest ponadto dostarczana ze [sterownikiem ODBC](http://go.microsoft.com/fwlink/?LinkId=616229) zapewniającym łączność z klastrami Spark w usłudze HDInsight z poziomu narzędzi do analizy biznesowej, takich jak Microsoft Power BI i Tableau.

## Od czego zacząć?

Rozpocznij od utworzenia klastra Spark w usłudze HDInsight w systemie Linux. Zobacz temat [Szybki start: tworzenie klastra Spark w usłudze HDInsight w systemie Linux i uruchamianie przykładowych aplikacji przy użyciu oprogramowania Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md). 

## Następne kroki

### Scenariusze

* [Platforma Spark i analiza biznesowa: interakcyjna analiza danych na platformie Spark w usłudze HDInsight z użyciem narzędzi do analizy biznesowej](hdinsight-apache-spark-use-bi-tools.md)

* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do analizy temperatury w budynku z użyciem danych HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do przewidywania wyników kontroli żywności](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Przesyłanie strumieniowe Spark: korzystanie z platformy Spark w usłudze HDInsight do tworzenia aplikacji do przesyłania strumieniowego w czasie rzeczywistym](hdinsight-apache-spark-eventhub-streaming.md)

* [Analiza dzienników witryny sieci Web na platformie Spark w usłudze HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### Tworzenie i uruchamianie aplikacji

* [Tworzenie autonomicznych aplikacji przy użyciu języka Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Zdalne uruchamianie zadań w klastrze Spark przy użyciu programu Livy](hdinsight-apache-spark-livy-rest-interface.md)

### Narzędzia i rozszerzenia

* [Tworzenie i przesyłanie aplikacji Spark Scala przy użyciu dodatku HDInsight Tools Plugin for IntelliJ IDEA](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Zdalne debugowanie aplikacji Spark przy użyciu dodatku HDInsight Tools Plugin for IntelliJ IDEA](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Korzystanie z notesów Zeppelin w klastrze Spark w usłudze HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Korzystanie z zewnętrznych pakietów z notesami Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Instalacja oprogramowania Jupyter na komputerze i nawiązywanie połączenia z klastrem Spark w usłudze HDInsight](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### Zarządzanie zasobami

* [Zarządzanie zasobami klastra Apache Spark w usłudze Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Śledzenie i debugowanie zadań uruchamianych w klastrze Apache Spark w usłudze HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md



<!--HONumber=Jun16_HO2-->


