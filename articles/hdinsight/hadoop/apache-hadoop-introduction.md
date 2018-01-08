---
title: "Co to jest usługa HDInsight oraz stos technologii Hadoop i Spark? — platforma Azure | Microsoft Docs"
description: "Wprowadzenie do usługi HDInsight oraz stosu i składników technologii Hadoop i Spark, w tym systemów Kafka, Hive, Storm i bazy danych HBase do analizy danych big data."
keywords: "azure hadoop, hadoop azure, wprowadzenie do hadoop, wprowadzenie do usługi hadoop, stos technologii hadoop, wstęp do hadoop, wprowadzenie do platformy hadoop, co to jest klaster hadoop, co to są klastry hadoop, do czego służy usługa hadoop"
services: hdinsight
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: e56a396a-1b39-43e0-b543-f2dee5b8dd3a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/13/2017
ms.author: cgronlun
ms.openlocfilehash: 49ec2f305c1aef50fcff977d60ff3ce0079ff5af
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/04/2018
---
# <a name="introduction-to-azure-hdinsight-the-hadoop-and-spark-technology-stack"></a>Wprowadzenie do usługi Azure HDInsight oraz stosu technologii Hadoop i Spark
Ten artykuł zawiera podstawowe informacje na temat usługi Azure HDInsight, w pełni zarządzanej usługi analitycznej typu „open source” o szerokim zakresie, przeznaczonej dla przedsiębiorstw. Można używać z nią struktur typu „open source” takich jak Hadoop, Spark, Hive, LLAP, Kafka, Storm, R i nie tylko. 

[!INCLUDE [hdinsight-price-change](../../../includes/hdinsight-enhancements.md)]

[Apache Hadoop](http://hadoop.apache.org/) to oryginalna struktura typu „open source” do przetwarzania rozproszonego i analizy zestawów danych big data w klastrach. Stos technologii Hadoop obejmuje powiązane oprogramowanie i narzędzia, w tym Apache Hive, HBase, Spark, Kafka i wiele innych. Aby wyświetlić dostępne składniki stosu technologii Hadoop w usłudze HDInsight, zobacz [Components and versions available with HDInsight][component-versioning] (Składniki i wersje dostępne w usłudze HDInsight). Aby dowiedzieć się więcej o platformie Hadoop w usłudze HDInsight, zobacz [stronę z opisem funkcji platformy Azure w usłudze HDInsight](https://azure.microsoft.com/services/hdinsight/).

[Apache Spark](http://spark.apache.org) to platforma przetwarzania równoległego typu open source, która obsługuje przetwarzanie w pamięci w celu zwiększania wydajności aplikacji do analizy danych big data. Aby dowiedzieć się więcej na temat platformy Spark w usłudze HDInsight, zobacz [Wprowadzenie do platformy Spark w usłudze HDInsight](../spark/apache-spark-overview.md). 

<a href="https://ms.portal.azure.com/#create/Microsoft.HDInsightCluster" target="_blank"><img src="./media/apache-hadoop-introduction/deploy-to-azure.png" alt="Deploy an Azure HDInsight cluster"></a>

## <a name="what-is-hdinsight-and-the-hadoop-technology-stack"></a>Co to jest usługa HDInsight oraz stos technologii Hadoop? 
Usługa Azure HDInsight jest dystrybucją w chmurze składników usługi Hadoop z platformy [Hortonworks Data Platform (HDP)](https://hortonworks.com/products/data-center/hdp/).  Usługa Azure HDInsight ułatwia i przyspiesza przetwarzanie ogromnych ilości danych przy jednoczesnej minimalizacji kosztów. Można używać w niej najpopularniejszych struktur typu „open source”, takich jak Hadoop, Spark, Hive, LLAP, Kafka, Storm, R i nie tylko, aby realizować rozmaite scenariusze związane z wyodrębnianiem, transformacją i ładowaniem danych, magazynowaniem danych, uczeniem maszynowym oraz Internetem rzeczy (IoT).

## <a name="what-is-big-data"></a>Co to są dane big data?

Dane big data są gromadzone szybciej, w większych ilościach i bardziej różnorodnych formatach. Dzielą się one na historyczne (przechowywane) lub dostępne w czasie rzeczywistym (przesyłane strumieniowo ze źródła). Zobacz [Scenariusze użycia usługi HDInsight](#scenarios-for-using-hdinsight), aby zapoznać się z najpopularniejszymi przypadkami użycia danych big data.

## <a name="why-should-i-use-hdinsight"></a>Po co korzystać z usługi HDInsight?

W tej sekcji wymieniono możliwości usługi Azure HDInsight.


|Możliwości  |Opis  |
|---------|---------|
|Natywna usługa w chmurze     |     Usługa Azure HDInsight umożliwia tworzenie na platformie Azure klastrów zoptymalizowanych dla technologii [Hadoop](apache-hadoop-linux-tutorial-get-started.md),  [Spark](../spark/apache-spark-jupyter-spark-sql.md),  [zapytania interakcyjnego (LLAP)](../interactive-query/apache-interactive-query-get-started.md),  [Kafka](../kafka/apache-kafka-get-started.md),  [Storm](../storm/apache-storm-tutorial-get-started-linux.md),  [HBase](../hbase/apache-hbase-tutorial-get-started-linux.md) i  [R Server](../r-server/r-server-get-started.md). Usługa HDInsight zapewnia również kompleksową umowę dotyczącą poziomu usług dla wszystkich obciążeń produkcyjnych.  |
|Niskie koszty i skalowalność     | Usługa HDInsight umożliwia [skalowanie](../hdinsight-administer-use-portal-linux.md)  obciążeń w górę lub w dół. Możesz obniżyć koszty,  [tworząc klastry na żądanie](../hdinsight-hadoop-create-linux-clusters-adf.md)  i płacąc wyłącznie za rzeczywiste użycie. Możesz także tworzyć potoki danych w celu operacjonalizacji zadań. Oddzielenie obliczeń i magazynu zapewnia wyższą wydajność i elastyczność. |
|Bezpieczeństwo i zgodność    | Usługa HDInsight umożliwia ochronę zasobów danych przedsiębiorstwa przy użyciu usługi [Azure Virtual Network](../hdinsight-extend-hadoop-virtual-network.md), [szyfrowania](../hdinsight-hadoop-create-linux-clusters-with-secure-transfer-storage.md) oraz integracji z usługą [Azure Active Directory](../domain-joined/apache-domain-joined-introduction.md). Usługa HDInsight spełnia również najpopularniejsze branżowe i rządowe [normy zgodności](https://azure.microsoft.com/overview/trusted-cloud).        |
|Monitorowanie    | Usługa Azure HDInsight jest zintegrowana z usługą [Azure Log Analytics](../hdinsight-hadoop-oms-log-analytics-tutorial.md), zapewniając jeden interfejs do monitorowania wszystkich klastrów.        |
|Globalna dostępność | Usługa HDInsight jest dostępna w większej liczbie  [regionów](https://azure.microsoft.com/regions/services/)  niż jakiekolwiek inne rozwiązanie do analizy danych big data. Usługa Azure HDInsight jest również dostępna w ramach chmur Azure Government, Azure (Chiny) i Azure (Niemcy), dzięki czemu odpowiada na potrzeby Twojego przedsiębiorstwa w najważniejszych obszarach suwerenności. |  
|Produktywność     |  Usługa Azure HDInsight umożliwia korzystanie z zaawansowanych narzędzi zapewniających produktywność w usłudze Hadoop i na platformie Spark w ramach preferowanego środowiska deweloperskiego, na przykład [Visual Studio](apache-hadoop-visual-studio-tools-get-started.md), [Eclipse](../spark/apache-spark-eclipse-tool-plugin.md) lub [IntelliJ](../spark/apache-spark-intellij-tool-plugin.md) w języku Scala, Python, R, Java i .NET. Analitycy danych mogą także współpracować przy użyciu popularnych notesów, takich jak [Jupyter](../spark/apache-spark-jupyter-notebook-kernels.md) i [Zeppelin](../spark/apache-spark-zeppelin-notebook.md).    |
|Rozszerzalność     |  Klastry usługi HDInsight można rozszerzać, instalując składniki (Hue, Presto itp.) przy użyciu [akcji skryptów](../hdinsight-hadoop-customize-cluster-linux.md), [dodając węzły brzegowe](../hdinsight-apps-use-edge-node.md) lub [integrując je z innymi certyfikowanymi aplikacjami do przetwarzania danych big data](../hdinsight-apps-install-applications.md). Usługa HDInsight umożliwia bezproblemową integrację z najpopularniejszymi rozwiązaniami danych big data oraz wdrażanie za pomocą [jednego kliknięcia](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/).|

## <a name="scenarios-for-using-hdinsight"></a>Scenariusze użycia usługi HDInsight

Z usługi Azure HDInsight można korzystać w różnorodnych przypadkach użycia związanych z przetwarzaniem danych big data. Dane big data są gromadzone szybciej, w większych ilościach i bardziej różnorodnych formatach. Dzielą się one na dane historyczne (już zebrane i przechowywane) oraz dostępne w czasie rzeczywistym (przesyłane strumieniowo bezpośrednio ze źródła). Scenariusze użycia związane z przetwarzaniem takich danych można podzielić na następujące kategorie: 

### <a name="batch-processing-etl"></a>Przetwarzanie wsadowe (ETL)

Wyodrębnianie, transformacja, ładowanie (ETL, extraction, transformation, and loading) to proces wyodrębniania danych ze strukturą lub bez struktury z heterogenicznych źródeł danych, przekształcania tych danych do formatu strukturalnego i ładowania ich do magazynu danych. Przekształcone dane mogą być używane do analizy lub magazynowania danych.

### <a name="internet-of-things-iot"></a>Internet rzeczy (IoT)

Przy użyciu usługi HDInsight można przetwarzać dane przesyłane strumieniowo, odbierane w czasie rzeczywistym z różnych urządzeń. Aby uzyskać więcej informacji, [przeczytaj ten wpis w blogu](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/).

![Architektura usługi HDInsight: Internet rzeczy](./media/apache-hadoop-introduction/hdinsight-architecture-iot.png) 

### <a name="data-science"></a>Nauka o danych

Przy użyciu usługi HDInsight można tworzyć aplikacje wyodrębniające z danych kluczowe informacje. Dodatkowo można zastosować usługę Azure Machine Learning, aby przewidywać przyszłe tendencje biznesowe. Aby uzyskać więcej informacji, [przeczytaj tę historię klienta](https://customers.microsoft.com/story/pros).

![Architektura usługi HDInsight: Nauka o danych](./media/apache-hadoop-introduction/hdinsight-architecture-data-science.png)

### <a name="data-warehousing"></a>Magazynowanie danych

Przy użyciu usługi HDInsight można wykonywać interakcyjne zapytania w skali petabajtów względem danych ze strukturą lub bez struktury w dowolnym formacie. Można także tworzyć modele łączące je z narzędziami analizy biznesowej. Aby uzyskać więcej informacji, [przeczytaj tę historię klienta](https://customers.microsoft.com/story/milliman). 

![Architektura usługi HDInsight: Magazynowanie danych](./media/apache-hadoop-introduction/hdinsight-architecture-data-warehouse.png)

### <a name="hybrid"></a>Połączenie hybrydowe

Za pomocą usługi HDInsight możesz rozszerzyć istniejącą lokalną infrastrukturę przetwarzania danych big data na platformę Azure, aby korzystać z zaawansowanych funkcji analizy w chmurze.

![Architektura usługi HDInsight: Połączenie hybrydowe](./media/apache-hadoop-introduction/hdinsight-architecture-hybrid.png)

## <a name="overview"></a>Typy klastrów w usłudze HDInsight
Usługa HDInsight zawiera określone typy klastrów i oferuje możliwości dostosowywania klastra, takie jak dodawanie składników, narzędzi i języków.

### <a name="spark-kafka-interactive-query-hbase-customized-and-other-cluster-types"></a>Klastry typu Spark, Kafka, zapytań interakcyjnych, HBase, niestandardowe i inne
W usłudze HDInsight dostępne są następujące typy klastrów:

* **[Apache Hadoop](https://wiki.apache.org/hadoop)**: korzysta z systemu [HDFS](#hdfs), zarządzania zasobami [YARN](#yarn) i prostego modelu programowania [MapReduce](#mapreduce) do celów równoległego przetwarzania i analizowania danych partii.
* **[Apache Spark](http://spark.apache.org/)**: platforma przetwarzania równoległego obsługująca przetwarzanie w pamięci w celu zwiększania wydajności aplikacji do analizy danych big data. Platforma Spark jest odpowiednia do języka SQL, strumieniowego przesyłania danych oraz uczenia maszynowego. Zobacz temat [Przegląd: platforma Apache Spark w usłudze HDInsight](../spark/apache-spark-overview.md).
* **[Apache HBase](http://hbase.apache.org/)**: baza danych NoSQL oparta na platformie Hadoop, która zapewnia dostęp losowy i wysoki poziom spójności w przypadku dużych ilości danych z częściową strukturą lub bez struktury — potencjalnie miliardów wierszy pomnożonych przez miliony kolumn. Zobacz temat [Co to jest usługa HBase w usłudze HDInsight?](../hbase/apache-hbase-overview.md)
* **[Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver)**: serwer przeznaczony do hostowania równoległych, rozproszonych procesów języka R oraz zarządzania nimi. Umożliwia on analitykom danych, statystykom i programistom języka R dostęp na żądanie do skalowalnych, rozproszonych metod analizy w usłudze HDInsight. Zobacz temat [Overview of R Server on HDInsight](../r-server/r-server-overview.md) (Omówienie serwera R Server w usłudze HDInsight).
* **[Apache Storm](https://storm.incubator.apache.org/)**: rozproszony system obliczeniowy działający w czasie rzeczywistym do szybkiego przetwarzania dużych strumieni danych. Storm jest oferowany jako zarządzany klaster w usłudze HDInsight. Zobacz temat [Analyze real-time sensor data using Storm and Hadoop](../storm/apache-storm-sensor-data-analysis.md) (Analizowanie danych czujnika w czasie rzeczywistym przy użyciu platform Storm i Hadoop).
* **[Zapytanie interakcyjne Apache (Live Long and Process) w wersji zapoznawczej](https://cwiki.apache.org/confluence/display/Hive/LLAP)**: buforowanie w pamięci umożliwiające interaktywne i szybsze zapytania programu Hive. Zobacz temat [Use Interactive Query in HDInsight](../interactive-query/apache-interactive-query-get-started.md) (Używanie zapytań interakcyjnych w usłudze HDInsight).
* **[Apache Kafka](https://kafka.apache.org/)**: platforma typu „open source” służąca do tworzenia potoków danych przesyłanych strumieniowo i aplikacji do obsługi tych danych. Platforma Kafka obejmuje również funkcję kolejki komunikatów, która umożliwia publikowanie i subskrybowanie strumieni danych. Zobacz temat [Introduction to Apache Kafka on HDInsight](../kafka/apache-kafka-introduction.md) (Wprowadzenie do platformy Apache Kafka w usłudze HDInsight).

## <a name="open-source-components-in-hdinsight"></a>Składniki typu „open source” w usłudze HDInsight

Usługa Azure HDInsight umożliwia tworzenie klastrów przy użyciu struktur typu „open source”, takich jak Hadoop, Spark, Hive, LLAP, Kafka, Storm, HBase i R. Klastry te zawierają domyślnie inne składniki typu „open source”, takie jak [Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md), [Avro](http://avro.apache.org/docs/current/spec.html), [Hive](http://hive.apache.org), [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog/), [Mahout](https://mahout.apache.org/), [MapReduce](http://wiki.apache.org/hadoop/MapReduce), [YARN](http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), [Phoenix](http://phoenix.apache.org/), [Pig](http://pig.apache.org/), [Sqoop](http://sqoop.apache.org/), [Tez](http://tez.apache.org/), [Oozie](http://oozie.apache.org/) czy [ZooKeeper](http://zookeeper.apache.org/).  


## <a name="programming-languages-on-hdinsight"></a>Języki programowania w usłudze HDInsight
Klastry HDInsight — Spark, HBase, Kafka, Hadoop i inne — obsługują wiele języków programowania, ale niektóre z nich nie są instalowane domyślnie. W przypadku bibliotek, modułów lub pakietów niezainstalowanych domyślnie [do instalacji składnika należy użyć akcji skryptu](../hdinsight-hadoop-script-actions-linux.md). 

### <a name="default-programming-language-support"></a>Domyślna obsługa języka programowania
Domyślnie klastry usługi HDInsight obsługują języki:

* Java
* Python

Dodatkowe języki można zainstalować przy użyciu [akcji skryptu](../hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Języki maszyny wirtualnej Java (JVM)
Wiele języków innych niż Java można uruchomiać za pomocą maszyny wirtualnej Java (JVM), jednak do uruchomienia niektórych z nich mogą być potrzebne dodatkowe składniki zainstalowane w klastrze.

Te języki działające w oparciu o JVM są obsługiwane w klastrach usługi HDInsight:

* Clojure
* Jython (Python dla platformy Java)
* Scala

### <a name="hadoop-specific-languages"></a>Języki specyficzne dla platformy Hadoop
Klastry HDInsight obsługują następujące języki specyficzne dla stosu technologii Hadoop:

* Pig Latin do zadań Pig
* HiveQL do zadań Hive oraz SparkSQL

## <a name="business-intelligence-on-hdinsight"></a>Analiza biznesowa w usłudze HDInsight
Znane narzędzia do analizy biznesowej (BI) pobierają, analizują i raportują dane zintegrowane z usługą HDInsight przy użyciu dodatku Power Query lub sterownika Microsoft Hive ODBC:

* [Apache Spark BI using data visualization tools with Azure HDInsight (Analiza biznesowa przy użyciu platformy Apache Spark i narzędzi do wizualizacji danych w usłudze Azure HDInsight)](../spark/apache-spark-use-bi-tools.md)
* [Visualize Hive data with Microsoft Power BI in Azure HDInsight (Wizualizowanie danych programu Hive przy użyciu usługi Microsoft Power BI w usłudze Azure HDInsight)](apache-hadoop-connect-hive-power-bi.md) 
* [Visualize Interactive Query Hive data with Power BI in Azure HDInsight (Wizualizowanie danych programu Hive zapytania bezpośredniego przy użyciu usługi Power BI w usłudze Azure HDInsight)](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)
* [Podłączenie programu Excel do platformy Hadoop za pomocą dodatku Power Query](apache-hadoop-connect-excel-power-query.md): dowiedz się, jak połączyć program Excel z kontem usługi Azure Storage, na którym są przechowywane dane z klastra usługi HDInsight, przy użyciu dodatku Microsoft Power Query dla programu Excel. Wymagana stacja robocza z systemem Windows. 
* [Podłączanie programu Excel do platformy Hadoop za pomocą sterownika ODBC usługi Microsoft Hive](apache-hadoop-connect-excel-hive-odbc-driver.md): informacje na temat importowania danych z usługi HDInsight za pomocą sterownika ODBC usługi Microsoft Hive. Wymagana stacja robocza z systemem Windows. 
* [Platforma Microsoft Cloud](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): informacje na temat Power BI dla pakietu Office 365, pobierania wersji próbnej programu SQL Server oraz konfiguracji programów SharePoint Server 2013 i SQL Server BI.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx)


## <a name="next-steps"></a>Następne kroki

* [Wprowadzenie do usługi Hadoop w usłudze HDInsight](apache-hadoop-linux-tutorial-get-started.md)
* [Wprowadzenie do platformy Spark w usłudze HDInsight](../spark/apache-spark-jupyter-spark-sql.md)
* [Wprowadzenie do platformy Kafka w usłudze HDInsight](../kafka/apache-kafka-get-started.md)
* [Wprowadzenie do platformy Apache Storm w usłudze HDInsight ](../storm/apache-storm-tutorial-get-started-linux.md)
* [Wprowadzenie do usługi HBase w usłudze HDInsight](../hbase/apache-hbase-tutorial-get-started-linux.md)
* [Get started with Interactive Query (LLAP) on HDInsight (Wprowadzenie do zapytania interaktywnego [LLAP] w usłudze HDInsight)](../interactive-query/apache-interactive-query-get-started.md)
* [Wprowadzenie do oprogramowania R Server w usłudze HDInsight](../r-server/r-server-get-started.md)
* [Manage HDInsight clusters (Zarządzanie klastrami usługi HDInsight)](../hdinsight-administer-use-portal-linux.md)
* [Secure your HDInsight clusters (Zabezpieczanie klastrów usługi HDInsight)](../domain-joined/apache-domain-joined-introduction.md)
* [Monitor HDInsight clusters (Monitorowanie klastrów usługi HDInsight)](../hdinsight-hadoop-oms-log-analytics-tutorial.md)


[component-versioning]: ../hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/