---
title: "Co to jest usługa HDInsight oraz stos i klastry technologii Hadoop? — platforma Azure | Microsoft Docs"
description: "Wprowadzenie do usługi HDInsight oraz stosu i składników technologii Hadoop, w tym systemów Spark, Kafka, Hive i bazy danych HBase do analizy danych big data."
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
ms.date: 07/20/2017
ms.author: cgronlun
ms.openlocfilehash: 00a8f14603bf05f013fadda1f1b80fb1de8f9a7c
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="introduction-to-azure-hdinsight-the-hadoop-technology-stack-and-hadoop-clusters"></a>Wprowadzenie do usługi Azure HDInsight, stosu technologii Hadoop oraz klastrów Hadoop
 Ten artykuł zawiera wprowadzenie do usługi Azure HDInsight oraz dystrybucji w chmurze stosu technologii Hadoop. Wyjaśnia on również, czym jest klaster Hadoop i kiedy należy go używać. 

## <a name="what-is-hdinsight-and-the-hadoop-technology-stack"></a>Co to jest usługa HDInsight oraz stos technologii Hadoop? 
Usługa Azure HDInsight jest dystrybucją w chmurze składników usługi Hadoop z platformy [Hortonworks Data Platform (HDP)](https://hortonworks.com/products/data-center/hdp/). [Apache Hadoop](http://hadoop.apache.org/) to oryginalna struktura typu „open source” do przetwarzania rozproszonego i analizy zestawów danych big data w klastrach komputerów. 


Stos technologii Hadoop obejmuje powiązane oprogramowanie i narzędzia, w tym Apache Hive, HBase, Spark, Kafka i wiele innych. Aby wyświetlić dostępne składniki stosu technologii Hadoop w usłudze HDInsight, zobacz [Components and versions available with HDInsight][component-versioning] (Składniki i wersje dostępne w usłudze HDInsight). Aby dowiedzieć się więcej o platformie Hadoop w usłudze HDInsight, zobacz [stronę z opisem funkcji platformy Azure w usłudze HDInsight](https://azure.microsoft.com/services/hdinsight/).

## <a name="what-is-a-hadoop-cluster-and-when-do-you-use-it"></a>Co to jest klaster Hadoop i kiedy należy go używać?
*Hadoop* jest również typem klastra, który zapewnia:

* YARN do planowania zadań i zarządzania zasobami
* MapReduce do przetwarzania równoległego
* Rozproszony system plików Hadoop (HDFS)
  
Klastry Hadoop są najczęściej używane do przetwarzania wsadowego przechowywanych danych. Inne rodzaje klastrów w usłudze HDInsight oferują dodatkowe możliwości: platforma Spark jest coraz bardziej popularna dzięki możliwościom szybszego przetwarzania w pamięci. Aby uzyskać szczegółowe informacje, zobacz sekcję [Typy klastrów w usłudze HDInsight](#overview).

## <a name="what-is-big-data"></a>Co to są dane big data?
Dane big data opisują dowolną dużą porcję danych cyfrowych, takich jak:

* Dane z czujnika sprzętu przemysłowego
* Działania klienta zebrane z witryny sieci Web
* Kanał aktualności w serwisie Twitter

Dane big data są gromadzone szybciej, w większych ilościach i bardziej różnorodnych formatach. Dzielą się one na historyczne (przechowywane) lub dostępne w czasie rzeczywistym (przesyłane strumieniowo ze źródła). 

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

Klastry można również skonfigurować za pomocą następujących metod:
* **[Klastry przyłączone do domeny w wersji zapoznawczej](../domain-joined/apache-domain-joined-introduction.md)**: klaster przyłączony do domeny usługi Active Directory umożliwiający kontrolę dostępu i zapewnienie nadzoru nad danymi.
* **[Klastry niestandardowe z akcjami skryptu](../hdinsight-hadoop-customize-cluster-linux.md)**: klastry ze skryptami, które są uruchamiane podczas aprowizacji i instalują dodatkowe składniki.

### <a name="example-cluster-customization-scripts"></a>Przykładowe skrypty dostosowania klastra
Akcje skryptu to skrypty Bash w systemie Linux, które są uruchamiane podczas aprowizacji klastra i mogą służyć do instalowania dodatkowych składników w klastrze. 

Zespół usługi HDInsight zapewnia następujące przykładowe skrypty:

* **[Hue](../hdinsight-hadoop-hue-linux.md)**: zestaw aplikacji sieci Web umożliwiający interakcję z klastrem. Tylko klastry z systemem Linux.
* **[Giraph](../hdinsight-hadoop-giraph-install-linux.md)**: przetwarzanie wykresów do celów modelowania relacji między rzeczami lub osobami.
* **[Solr](../hdinsight-hadoop-solr-install-linux.md)**: platforma wyszukiwania w skali przedsiębiorstwa, która umożliwia pełnotekstowe wyszukiwanie danych.

Informacje na temat tworzenia własnych akcji skryptu można znaleźć w temacie [Script Action development with HDInsight](../hdinsight-hadoop-script-actions-linux.md) (Tworzenie akcji skryptu za pomocą usługi HDInsight).

## <a name="components-and-utilities-on-hdinsight-clusters"></a>Składniki i narzędzia w klastrach HDInsight
Klastry HDInsight zawierają następujące składniki i narzędzia:

* **[Ambari](#ambari)**: inicjowanie obsługi klastrów, zarządzanie nimi, ich monitorowanie oraz narzędzia.
* **[Avro](#avro)** (biblioteka Microsoft .NET dla Avro): serializacja danych w środowisku Microsoft .NET. 
* **[Hive i HCatalog](#hive)**: przeszukiwanie w sposób przypominający kwerendę SQL oraz warstwa zarządzania tabelą i magazynem.
* **[Mahout](#mahout)**: na potrzeby skalowalnych aplikacji uczenia maszynowego.
* **[MapReduce](#mapreduce)**: Starsza struktura do przetwarzania rozproszonego oraz zarządzania zasobami w ramach platformy Hadoop. Zobacz sekcję [YARN](#yarn).
* **[Oozie](#oozie)**: zarządzanie przepływem pracy.
* **[Phoenix](#phoenix)**: warstwa relacyjnej bazy danych za pośrednictwem HBase.
* **[Pig](#pig)**: łatwiejsze wykonywanie skryptów do transformacji MapReduce.
* **[Sqoop](#sqoop)**: importowanie i eksportowanie danych.
* **[Tez](#tez)**: umożliwia sprawne działanie wielkoskalowych procesów przetwarzających duże ilości danych.
* **[YARN](#yarn)**: zarządzanie zasobami w ramach podstawowej biblioteki usługi Hadoop.
* **[ZooKeeper](#zookeeper)**: koordynacja procesów w systemach rozproszonych.

> [!NOTE]
> Aby uzyskać informacje na temat określonych składników i wersji, zobacz temat [Składniki i wersje platformy Hadoop w usłudze HDInsight][component-versioning]
>
>

### <a name="ambari"></a>Ambari
Apache Ambari służy do aprowizacji i monitorowania klastrów Apache Hadoop oraz zarządzania nimi. Obejmuje intuicyjny zestaw narzędzi operatora oraz niezawodny zestaw interfejsów API, które neutralizują złożoność platformy Hadoop, upraszczając działanie klastrów. Klastry HDInsight w systemie Linux zapewniają zarówno interfejs użytkownika sieci Web Ambari, jak i interfejs API REST Ambari. Widoki Ambari w klastrach HDInsight zapewniają funkcje interfejsu użytkownika wtyczek.
Zobacz tematy [Zarządzanie klastrami HDInsight przy użyciu Ambari](../hdinsight-hadoop-manage-ambari.md) i <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API reference</a> (Dokumentacja interfejsu API Apache Ambari).

### <a name="avro"></a>Avro (biblioteka programu Microsoft .NET dla Avro)
Biblioteka Microsoft .NET dla Avro wdraża format wymiany kompaktowych danych binarnych Apache Avro do serializacji dla środowiska Microsoft .NET. Definiuje schemat niezależny od języka, aby dane serializowane w jednym języku mogły być odczytywane w innym. Szczegółowe informacje dotyczące formatu można znaleźć w temacie <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro Specification</a> (Specyfikacja systemu Apache Avro). Format plików Avro obsługuje rozproszony model programowania MapReduce: pliki są „podzielne”, co oznacza, że można wyszukiwać dowolne miejsce w pliku i rozpocząć odczytywanie od określonego bloku. Aby dowiedzieć się, w jaki sposób, zobacz temat [Serialize data with the Microsoft .NET Library for Avro](apache-hadoop-dotnet-avro-serialization.md) (Serializacja danych za pomocą biblioteki Microsoft .NET dla Avro). Obsługa klastrów opartych na systemie Linux zostanie wprowadzona w przyszłości.

### <a name="hdfs"></a>HDFS
Rozproszony system plików Hadoop (HDFS) jest systemem plików, który wraz z modelem MapReduce i strukturą YARN stanowi podstawę technologii Hadoop. Jest to standardowy system plików klastrów Hadoop w usłudze HDInsight. Zobacz temat [Korzystanie z magazynu zgodnego z systemem plików HDFS w połączeniu z platformą Hadoop w usłudze HDInsight](../hdinsight-hadoop-use-blob-storage.md).

### <a name="hive"></a>Hive i HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> to oprogramowanie magazynu danych oparte na platformie Hadoop, które pozwala wysyłać zapytania do dużych zestawów danych oraz zarządzać nimi w magazynie rozproszonym przy użyciu języka przypominającego SQL o nazwie HiveQL. Program Hive, tak samo jak Pig, jest warstwą abstrakcji nałożoną na model MapReduce, który przekłada zapytania na serię zadań MapReduce. Oprogramowanie Hive jest bliższe relacyjnemu systemowi zarządzania bazami danych niż program Pig i jest używane w przypadku bardziej ustrukturyzowanych danych. W przypadku danych bez struktury lepszym rozwiązaniem jest użycie programu Pig. Zobacz temat [Use Hive with Hadoop in HDInsight](hdinsight-use-hive.md) (Używanie Hive w ramach platformy Hadoop w usłudze HDInsight).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> to warstwa zarządzania tabelami i magazynem dla platformy Hadoop, która zapewnia relacyjny widok danych. W usłudze HCatalog można odczytywać i zapisywać pliki w dowolnym formacie obsługiwanym przez bibliotekę Hive SerDe (serializator-deserializator).

### <a name="mahout"></a>Mahout
<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> to biblioteka algorytmów uczenia maszynowego, która działa na platformie Hadoop. Zgodnie z zasadami statystyki aplikacje do uczenia maszynowego uczą systemy uczenia z danych oraz korzystania z wcześniejszych wyników, aby określić zachowanie w przyszłości. Zobacz temat [Generate movie recommendations using Mahout on Hadoop](../hdinsight-mahout.md) (Generowanie zaleceń filmu przy użyciu Mahout na platformie Hadoop).

### <a name="mapreduce"></a>MapReduce
Funkcja MapReduce to starsza platforma oprogramowania dla platformy Hadoop do zapisywania aplikacji w celu równoległego przetwarzania wsadowego zestawów danych big data. Zadanie MapReduce dzieli duże zestawy danych i organizuje dane w pary klucz-wartość do przetworzenia. Zadania MapReduce są uruchamiane w strukturze [YARN](#yarn). Zobacz <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> w witrynie Wiki dotyczącej platformy Hadoop.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> to system koordynacji przepływu pracy, który zarządza zadaniami na platformie Hadoop. Jest zintegrowany ze stosem platformy Hadoop i obsługuje zadania platformy Hadoop dla MapReduce, Pig, Hive i Sqoop. Może również służyć do planowania zadań specyficznych dla systemu, np. programów Java lub skryptów powłoki. Zobacz [Use Oozie with Hadoop](../hdinsight-use-oozie-linux-mac.md) (Stosowanie systemu Oozie z platformą Hadoop).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> to warstwa relacyjnej bazy danych za pośrednictwem HBase. Warstwa Phoenix obejmuje sterownik JDBC, który umożliwia bezpośrednie wysyłanie zapytań do tabel SQL oraz zarządzenie nimi. Phoenix tłumaczy zapytania i inne instrukcje do macierzystych wywołań interfejsu API NoSQL — zamiast używać MapReduce — umożliwiając szybsze zastosowania w ramach magazynów NoSQL. Zobacz temat [Use Apache Phoenix and SQuirreL with HBase clusters](../hdinsight-hbase-phoenix-squirrel.md) (Używanie Apache Phoenix i SQuirreL z klastrami HBase).

### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> to platforma wysokiego poziomu umożliwiająca wykonywanie złożonych przekształceń MapReduce na dużych zestawach danych przy użyciu prostego języka skryptów o nazwie Pig Latin. Pig tłumaczy skrypty Pig Latin po to, by działały w ramach platformy Hadoop. Aby rozszerzyć język Pig Latin, można utworzyć funkcje zdefiniowane przez użytkownika (UDF). Zobacz [Use Pig with Hadoop](hdinsight-use-pig.md) (Stosowanie języka Pig z platformą Hadoop).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> to narzędzie, które przenosi dane zbiorcze pomiędzy platformą Hadoop a relacyjnymi bazami danych, takimi jak SQL lub inne strukturalne magazyny danych, w sposób możliwie najbardziej wydajny. Zobacz temat [Use Sqoop with Hadoop](hdinsight-use-sqoop.md) (Korzystanie ze Sqoop przy użyciu platformy Hadoop).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> to struktura aplikacji działająca w oparciu o Hadoop YARN, wykonująca złożone, acykliczne wykresy będące wynikiem ogólnego przetwarzania danych. Jest bardziej elastycznym i wydajnym następcą platformy MapReduce, zwiększającym skuteczność wielkoskalowych procesów przetwarzających duże ilości danych, takich jak Hive. Zobacz część [„Use Apache Tez for improved performance” („Używanie Apache Tez w celu zwiększenia wydajności”) w temacie Use Hive and HiveQL (Korzystanie z Hive i HiveQL) ](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>YARN
Struktura Apache YARN to platforma MapReduce nowej generacji (MapReduce 2.0 lub MRv2), która obsługuje scenariusze przetwarzania danych poza przetwarzaniem wsadowym MapReduce z większą skalowalnością i obejmuje przetwarzanie w czasie rzeczywistym. YARN umożliwia zarządzanie zasobami oraz rozproszoną strukturę aplikacji. Zadania MapReduce działają w ramach platformy YARN. Zobacz temat <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>.

### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordynuje procesy w dużych systemach rozproszonych za pomocą udostępnionego hierarchicznego obszaru nazw rejestrów danych (znode). Rejestry znode zawierają niewielkie ilości metadanych potrzebnych do zapewnienia koordynacji procesów: stan, lokalizację, konfigurację itd. Zobacz przykład [ZooKeeper z klastrem HBase i Apache Phoenix](../hbase/apache-hbase-phoenix-squirrel-linux.md). 

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

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard i HDInsight Premium
Usługa HDInsight zapewnia oferty danych big data w chmurze w dwóch różnych kategoriach, Standard i Premium. HDInsight Standard zapewnia klaster w skali przedsiębiorstwa, który może być używany przez organizacje do uruchamiania obciążeń typu big data. HDInsight Premium zapewnia możliwości kategorii Standard poszerzone o zaawansowane funkcje analityczne i zabezpieczenia dla klastra usługi HDInsight. Więcej informacji można znaleźć w temacie [Azure HDInsight Premium](../hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

## <a name="microsoft-business-intelligence-and-hdinsight"></a>Analiza biznesowa Microsoft i usługa HDInsight
Znane narzędzia do analizy biznesowej (BI) pobierają, analizują i raportują dane zintegrowane z usługą HDInsight przy użyciu dodatku Power Query lub sterownika Microsoft Hive ODBC:

* [Podłączenie programu Excel do platformy Hadoop za pomocą dodatku Power Query](apache-hadoop-connect-excel-power-query.md): dowiedz się, jak połączyć program Excel z kontem usługi Azure Storage, na którym są przechowywane dane z klastra usługi HDInsight, przy użyciu dodatku Microsoft Power Query dla programu Excel. Wymagana stacja robocza z systemem Windows. 
* [Podłączanie programu Excel do platformy Hadoop za pomocą sterownika ODBC usługi Microsoft Hive](apache-hadoop-connect-excel-hive-odbc-driver.md): informacje na temat importowania danych z usługi HDInsight za pomocą sterownika ODBC usługi Microsoft Hive. Wymagana stacja robocza z systemem Windows. 
* [Platforma Microsoft Cloud](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): informacje na temat Power BI dla pakietu Office 365, pobierania wersji próbnej programu SQL Server oraz konfiguracji programów SharePoint Server 2013 i SQL Server BI.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx)


## <a name="next-steps"></a>Następne kroki

* [Wprowadzenie do platformy Hadoop w usłudze HDInsight](apache-hadoop-linux-tutorial-get-started.md): samouczek Szybki start dotyczący aprowizowania klastrów platformy Hadoop w usłudze HDInsight i uruchamiania przykładowych zapytań Hive.
* [Wprowadzenie do platformy Spark w usłudze HDInsight](../spark/apache-spark-jupyter-spark-sql.md): samouczek Szybki start dotyczący tworzenia klastra Spark i uruchamiania interaktywnych zapytań SQL Spark.
* [Use R Server on HDInsight](../r-server/r-server-get-started.md) (Stosowanie platformy R Server w usłudze HDInsight): rozpoczynanie korzystania z platformy R Server w usłudze HDInsight Premium.
* [Provision HDInsight clusters](../hdinsight-hadoop-provision-linux-clusters.md) (Aprowizowanie klastrów HDInsight): informacje o sposobie inicjowania obsługi klastra platformy Hadoop w usłudze HDInsight za pośrednictwem portalu Azure, interfejsu wiersza polecenia platformy Azure lub programu Azure PowerShell.


[component-versioning]: ../hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/