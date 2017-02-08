---
title: "Co to jest usługa Hadoop? Wprowadzenie do usługi Azure HDInsight | Microsoft Docs"
description: "Wprowadzenie do ekosystemu i składników platformy Hadoop w usłudze HDInsight. Usługa HDInsight obejmuje platformy Hadoop i Spark, usługę HBase oraz inne służące do przetwarzania i analizy danych big data."
keywords: analiza danych big data,wprowadzenie do platformy hadoop,co to jest platforma hadoop,hadoop w chmurze,stos technologii hadoop,ekosystem hadoop
services: hdinsight
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: e56a396a-1b39-43e0-b543-f2dee5b8dd3a
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/14/2016
ms.author: cgronlun
translationtype: Human Translation
ms.sourcegitcommit: 10d684bd6c9408f2fbd48a0a8804b011f098d71c
ms.openlocfilehash: 8f38b9eaf8bcb498a7983756cbf57f3e372d7fe0


---
# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Wprowadzenie do ekosystemu platformy Hadoop w usłudze Azure HDInsight
 Ten artykuł zawiera wprowadzenie do platformy Hadoop w usłudze Azure HDInsight, jej ekosystemu i danych big data. Poznaj składniki platformy Hadoop, typową terminologię i scenariusze analizy danych big data.

## <a name="what-is-hadoop-on-hdinsight"></a>Co to jest platforma Hadoop w usłudze HDInsight?
Hadoop to ekosystem oprogramowania typu „open source”, który stanowi strukturę rozproszonego przetwarzania, przechowywania i analizowania zestawów danych big data w klastrach komputerów. Usługa Azure HDInsight udostępnia w chmurze składniki platformy Hadoop z dystrybucji **Hortonworks Data Platform (HDP)**, wdraża klastry zarządzane o wysokiej niezawodności i dostępności, a także zapewnia zabezpieczenia i nadzór klasy korporacyjnej przy użyciu usługi Active Directory.  

Oryginalnym projektem typu „open source” dotyczącym przetwarzania danych big data była platforma Apache Hadoop. Później nastąpił rozwój pokrewnego oprogramowania i narzędzi uważanych za część stosu technologii Hadoop, takich jak Apache Hive, Apache HBase, Apache Spark, Apache Kafka i wielu innych. Aby zapoznać się ze szczegółami, zobacz [Omówienie ekosystemu platformy Hadoop w usłudze HDInsight](#overview).

## <a name="what-is-big-data"></a>Co to są dane big data?
Dane big data to dowolna duża porcja informacji cyfrowych. Mogą obejmować takie dane, jak teksty z usługi Twitter, dane pochodzące z czujnika sprzętu przemysłowego albo informacje dotyczące przeglądania Internetu przez klienta i zakupów w witrynie sieci Web. Dane big data mogą być historyczne (dane przechowywane) lub w czasie rzeczywistym (to znaczy przesyłane strumieniowo bezpośrednio ze źródła). Dane big data są gromadzone coraz szybciej, w coraz większych ilościach i coraz bardziej różnorodnych formatach.

Aby dane big data mogły zapewnić odpowiednią analizę lub szczegółowe informacje, musisz zgromadzić odpowiednią ilość danych i zadać właściwe pytania. Musisz również upewnić się, że dane są dostępne, oczyszczone, przeanalizowane i przedstawione w wygodny sposób. Właśnie te czynności ułatwia funkcja analizy danych big data w ramach platformy Hadoop w usłudze HDInsight.

## <a name="a-nameoverviewaoverview-of-the-hadoop-ecosystem-in-hdinsight"></a><a name="overview"></a>Omówienie ekosystemu platformy Hadoop w usłudze HDInsight
Usługa HDInsight jest dystrybucją chmury na platformie Microsoft Azure należącej do dynamicznie rozwijającego się stosu technologii Apache Hadoop służącego do analizy danych big data. Obejmuje ona wdrożenia technologii Apache Spark, HBase, Kafka, Storm, Pig, Hive, Interactive Hive, Sqoop, Oozie, Ambari itd. Usługa HDInsight integruje się również z narzędziami analizy biznesowej (BI), takimi jak Power BI, Excel, SQL Server Analysis Services oraz SQL Server Reporting Services.

### <a name="hadoop-hbase-spark-kafka-interactive-hive-storm-customized-and-other-clusters"></a>Klastry Hadoop, HBase, Spark, Kafka, Interactive Hive, Storm, klastry niestandardowe i inne
W usłudze HDInsight dostępne są następujące typy klastrów:

* **[Apache Hadoop](https://wiki.apache.org/hadoop)**: zapewnia niezawodny magazyn danych przy użyciu [systemu plików HDFS](#hdfs) i prosty model programowania [MapReduce](#mapreduce) do równoległego przetwarzania i analizy danych.
* **[Apache Spark](http://spark.apache.org/)**: platforma przetwarzania równoległego obsługująca przetwarzanie w pamięci w celu zwiększania wydajności aplikacji do analizy danych big data. Platforma Spark jest odpowiednia do języka SQL, strumieniowego przesyłania danych oraz uczenia maszynowego. Zobacz temat [Przegląd: platforma Apache Spark w usłudze HDInsight w systemie Linux](hdinsight-apache-spark-overview.md).
* **[Apache HBase](http://hbase.apache.org/)**: baza danych NoSQL oparta na platformie Hadoop, która zapewnia dostęp losowy i wysoki poziom spójności w przypadku dużych ilości danych z częściową strukturą lub bez struktury — potencjalnie miliardów wierszy pomnożonych przez miliony kolumn. Zobacz temat [Omówienie baz danych HBase w usłudze HDInsight](hdinsight-hbase-overview.md).
* **[Microsoft R Server](https://msdn.microsoft.com/en-us/microsoft-r/rserver)**: serwer klasy korporacyjnej przeznaczony do hostowania równoległych, rozproszonych procesów w języku R i do zarządzania nimi. Umożliwia on analitykom danych, statystykom i programistom języka R dostęp na żądanie do skalowalnych, rozproszonych metod analizy w usłudze HDInsight. Zobacz temat [Overview of R Server on HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-r-server-overview) (Omówienie serwera R Server w usłudze HDInsight).
* **[Apache Storm](https://storm.incubator.apache.org/)**: rozproszony system obliczeniowy działający w czasie rzeczywistym do szybkiego przetwarzania dużych strumieni danych. Storm jest oferowany jako zarządzany klaster w usłudze HDInsight. Zobacz temat [Analyze real-time sensor data using Storm and Hadoop](hdinsight-storm-sensor-data-analysis.md) (Analizowanie danych czujnika w czasie rzeczywistym przy użyciu platform Storm i Hadoop).
* **[Apache Interactive Hive (Live Long and Process) w wersji zapoznawczej](https://cwiki.apache.org/confluence/display/Hive/LLAP)**: buforowanie w pamięci umożliwiające interaktywne i szybsze zapytania programu Hive. Zobacz temat [Use Interactive Hive in HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-use-interactive-hive) (Używanie oprogramowania Interactive Hive w usłudze HDInsight).
* **[Apache Kafka w wersji zapoznawczej](https://kafka.apache.org/)**: platforma typu „open source” służąca do opracowywania potoków danych przesyłanych strumieniowo i aplikacji do obsługi tych danych. Platforma Kafka obejmuje również funkcję kolejki komunikatów, która umożliwia publikowanie i subskrybowanie strumieni danych. Zobacz temat [Introduction to Apache Kafka on HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-kafka-introduction) (Wprowadzenie do platformy Apache Kafka w usłudze HDInsight).
* **[Klastry przyłączone do domeny w wersji zapoznawczej](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-domain-joined-introduction)**: klaster przyłączony do domeny usługi Active Directory umożliwiający kontrolę dostępu i zapewnienie nadzoru nad danymi.
* **[Klastry niestandardowe z akcjami skryptu](hdinsight-hadoop-customize-cluster-linux.md)**: klastry ze skryptami, które są uruchamiane podczas aprowizacji i instalują dodatkowe składniki.

### <a name="example-customization-scripts"></a>Przykładowe skrypty dostosowania
Akcje skryptu to skrypty Bash w systemie Linux, które są uruchamiane podczas aprowizacji klastra i mogą służyć do instalowania dodatkowych składników w klastrze.

Zespół usługi HDInsight zapewnia następujące przykładowe skrypty:

* [Hue](hdinsight-hadoop-hue-linux.md): zestaw aplikacji sieci web umożliwiający interakcję z klastrem. Tylko klastry z systemem Linux.
* [Giraph](hdinsight-hadoop-giraph-install-linux.md): przetwarzanie wykresów do modelowania relacji między rzeczami lub osobami.
* [R](hdinsight-hadoop-r-scripts-linux.md): język i środowisko typu open source do statystycznego przetwarzania danych używane w uczeniu maszynowym.
* [Solr](hdinsight-hadoop-solr-install-linux.md): platforma wyszukiwania w skali przedsiębiorstwa, która umożliwia wyszukiwanie pełnotekstowe danych.

Informacje na temat tworzenia własnych akcji skryptu można znaleźć w temacie [Script Action development with HDInsight](hdinsight-hadoop-script-actions-linux.md) (Tworzenie akcji skryptu za pomocą usługi HDInsight).

## <a name="what-are-the-hadoop-components-and-utilities"></a>Jakie są składniki i narzędzia platformy Hadoop?
W klastrach HDInsight znajdują się następujące składniki i narzędzia.

* **[Ambari](#ambari)**: inicjowanie obsługi klastrów, zarządzanie nimi, ich monitorowanie oraz narzędzia.
* **[Avro](#avro)** (biblioteka Microsoft .NET dla Avro): serializacja danych w środowisku Microsoft .NET.
* **[Hive i HCatalog](#hive)**: przeszukiwanie przypominające Structured Query Language (SQL) oraz warstwa zarządzania tabeli i magazynu.
* **[Mahout](#mahout)**: na potrzeby skalowalnych aplikacji uczenia maszynowego.
* **[MapReduce](#mapreduce)**: Starsza struktura do przetwarzania rozproszonego oraz zarządzania zasobami w ramach platformy Hadoop. Zobacz [YARN](#yarn), platforma zasobów nowej generacji.
* **[Oozie](#oozie)**: zarządzanie przepływem pracy.
* **[Phoenix](#phoenix)**: warstwa relacyjnej bazy danych za pośrednictwem HBase.
* **[Pig](#pig)**: łatwiejsze wykonywanie skryptów do transformacji MapReduce.
* **[Sqoop](#sqoop)**: importowanie i eksportowanie danych.
* **[Tez](#tez)**: umożliwia sprawne działanie wielkoskalowych procesów przetwarzających duże ilości danych.
* **[YARN](#yarn)**: część podstawowej biblioteki platformy Hadoop oraz nowa generacja platformy oprogramowania MapReduce.
* **[ZooKeeper](#zookeeper)**: koordynacja procesów w systemach rozproszonych.

> [!NOTE]
> Aby uzyskać informacje na temat określonych składników i wersji, zobacz [Hadoop components, versioning, and service offerings in HDInsight][component-versioning] (Składniki, wersje i oferty usługowe platformy Hadoop w usłudze HDInsight)
>
>

### <a name="a-nameambariaambari"></a><a name="ambari"></a>Ambari
Apache Ambari służy do inicjowania obsługi, monitorowania klastrów platformy Apache Hadoop oraz zarządzania nimi. Obejmuje intuicyjny zestaw narzędzi operatora oraz niezawodny zestaw interfejsów API, które neutralizują złożoność platformy Hadoop, upraszczając działanie klastrów. Klastry HDInsight w systemie Linux zapewniają zarówno interfejs użytkownika w sieci Web Ambari, jak i interfejs API REST Ambari, natomiast klastry w systemie Windows zapewniają podzestaw interfejsu API REST. Widoki Ambari w klastrach HDInsight zapewniają funkcje interfejsu użytkownika wtyczek.

Zobacz tematy [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md) (Zarządzanie klastrami HDInsight przy użyciu Ambari), [Monitor Hadoop clusters in HDInsight using the Ambari API](hdinsight-monitor-use-ambari-api.md) (Monitorowanie klastrów platformy Hadoop w HDInsight przy użyciu Ambari API) oraz <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API reference</a> (Dokumentacja interfejsu API Apache Ambari).

### <a name="a-nameavroaavro-microsoft-net-library-for-avro"></a><a name="avro"></a>Avro (biblioteka programu Microsoft .NET dla Avro)
Biblioteka Microsoft .NET dla Avro wdraża format wymiany kompaktowych danych binarnych Apache Avro do serializacji dla środowiska Microsoft .NET. Używa ona formatu <a target="_blank" href="http://www.json.org/">JavaScript Object Notation (JSON)</a> do definiowania schematu z niesprecyzowanym językiem, który zapewnia współdziałanie języków, co oznacza, że dane serializowane w jednym języku mogą być odczytywane w innym. Szczegółowe informacje dotyczące formatu można znaleźć w temacie <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro Specification</a> (Specyfikacja systemu Apache Avro).
Format plików Avro obsługuje rozproszony model programowania MapReduce. Pliki są „podzielne”, co oznacza, że można wyszukiwać dowolne miejsce w pliku i rozpocząć odczytywanie od określonego bloku. Aby dowiedzieć się, w jaki sposób, zobacz temat [Serialize data with the Microsoft .NET Library for Avro](hdinsight-dotnet-avro-serialization.md) (Serializacja danych za pomocą biblioteki Microsoft .NET dla Avro).

### <a name="a-namehdfsahdfs"></a><a name="hdfs"></a>HDFS
Rozproszony system plików Hadoop (HDFS) jest rozproszonym systemem plików, który wraz z MapReduce i YARN stanowi podstawę ekosystemu platformy Hadoop. HDFS jest standardowym systemem plików klastrów platformy Hadoop w usłudze HDInsight.

### <a name="a-namehiveahive--hcatalog"></a><a name="hive"></a>Hive i HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> to oprogramowanie magazynu danych oparte na platformie Hadoop, które pozwala wysyłać zapytania do dużych zestawów danych oraz zarządzać nimi w magazynie rozproszonym przy użyciu języka przypominającego SQL o nazwie HiveQL. Hive, tak samo jak Pig, jest abstrakcją nałożoną na MapReduce. Uruchomione oprogramowanie Hive przekłada zapytania na serię zadań MapReduce. Oprogramowanie Hive jest koncepcyjnie bliższe relacyjnemu systemowi zarządzania bazami danych niż program Pig i dlatego jest odpowiednie do używania z bardziej ustrukturyzowanymi danymi. W przypadku danych bez struktury lepszym rozwiązaniem jest użycie programu Pig. Zobacz temat [Use Hive with Hadoop in HDInsight](hdinsight-use-hive.md) (Używanie Hive w ramach platformy Hadoop w usłudze HDInsight).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> to warstwa zarządzania tabelami i magazynem dla platformy Hadoop, która zapewnia relacyjny widok danych. W HCatalog można odczytać i zapisać pliki w dowolnym formacie, dla którego można napisać Hive SerDe (serializator-deserializator).

### <a name="a-namemahoutamahout"></a><a name="mahout"></a>Mahout
<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> to skalowalna biblioteka algorytmów uczenia maszynowego, która działa na platformie Hadoop. Zgodnie z zasadami statystyki aplikacje do uczenia maszynowego uczą systemy uczenia z danych oraz korzystania z wcześniejszych wyników, aby określić zachowanie w przyszłości. Zobacz temat [Generate movie recommendations using Mahout on Hadoop](hdinsight-mahout.md) (Generowanie zaleceń filmu przy użyciu Mahout na platformie Hadoop).

### <a name="a-namemapreduceamapreduce"></a><a name="mapreduce"></a>MapReduce
Funkcja MapReduce to starsza platforma oprogramowania dla platformy Hadoop do zapisywania aplikacji w celu równoległego przetwarzania wsadowego zestawów danych big data. Zadanie MapReduce dzieli duże zestawy danych i organizuje dane w pary klucz-wartość do przetworzenia.

[YARN](#yarn), zwany też MapReduce 2.0, to menedżer zasobów nowej generacji oraz platforma aplikacji na platformie Hadoop. Zadania MapReduce działają w ramach platformy YARN.

Aby uzyskać więcej informacji o MapReduce, zobacz temat <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> w witrynie Wiki dotyczącej platformy Hadoop.

### <a name="a-nameoozieaoozie"></a><a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> to system koordynacji przepływu pracy, który zarządza zadaniami na platformie Hadoop. Jest zintegrowany ze stosem platformy Hadoop i obsługuje zadania platformy Hadoop dla MapReduce, Pig, Hive i Sqoop. Może również służyć do planowania zadań specyficznych dla systemu, np. programów Java lub skryptów powłoki. Zobacz [Use Oozie with Hadoop](hdinsight-use-oozie.md) (Stosowanie systemu Oozie z platformą Hadoop).

### <a name="a-namephoenixaphoenix"></a><a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> to warstwa relacyjnej bazy danych za pośrednictwem HBase. Warstwa Phoenix obejmuje sterownik JDBC, który umożliwia bezpośrednie wysyłanie zapytań do tabel SQL oraz zarządzenie nimi. Phoenix tłumaczy zapytania i inne instrukcje do macierzystych wywołań interfejsu API NoSQL — zamiast używać MapReduce — umożliwiając szybsze zastosowania w ramach magazynów NoSQL. Zobacz temat [Use Apache Phoenix and SQuirreL with HBase clusters](hdinsight-hbase-phoenix-squirrel.md) (Używanie Apache Phoenix i SQuirreL z klastrami HBase).

### <a name="a-namepigapig"></a><a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> to platforma wysokiego poziomu umożliwiająca wykonywanie złożonych przekształceń MapReduce na bardzo dużych zestawach danych przy użyciu prostego języka skryptowego o nazwie Pig Latin. Pig tłumaczy skrypty Pig Latin po to, by działały w ramach platformy Hadoop. Aby rozszerzyć język Pig Latin, można utworzyć funkcje zdefiniowane przez użytkownika (UDF). Zobacz [Use Pig with Hadoop](hdinsight-use-pig.md) (Stosowanie języka Pig z platformą Hadoop).

### <a name="a-namesqoopasqoop"></a><a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> to narzędzie, które przenosi dane zbiorcze pomiędzy platformą Hadoop a relacyjnymi bazami danych, takimi jak SQL lub inne strukturalne magazyny danych, w jak najbardziej wydajny sposób. Zobacz temat [Use Sqoop with Hadoop](hdinsight-use-sqoop.md) (Korzystanie ze Sqoop przy użyciu platformy Hadoop).

### <a name="a-nametezatez"></a><a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> to struktura aplikacji działająca w oparciu o Hadoop YARN, wykonująca złożone, acykliczne wykresy będące wynikiem ogólnego przetwarzania danych. Jest bardziej elastycznym i wydajnym następcą platformy MapReduce, zwiększającym skuteczność wielkoskalowych procesów przetwarzających duże ilości danych, takich jak Hive. Zobacz część [„Use Apache Tez for improved performance” („Używanie Apache Tez w celu zwiększenia wydajności”) w temacie Use Hive and HiveQL (Korzystanie z Hive i HiveQL) ](hdinsight-use-hive.md#usetez).

### <a name="a-nameyarnayarn"></a><a name="yarn"></a>YARN
Struktura Apache YARN to platforma MapReduce nowej generacji (MapReduce 2.0 lub MRv2), która obsługuje scenariusze przetwarzania danych poza przetwarzaniem wsadowym MapReduce z większą skalowalnością i obejmuje przetwarzanie w czasie rzeczywistym. YARN umożliwia zarządzanie zasobami oraz rozproszoną strukturę aplikacji. Zadania MapReduce działają w ramach platformy YARN.

Aby dowiedzieć się więcej o YARN, zobacz temat <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>.

### <a name="a-namezookeeperazookeeper"></a><a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordynuje procesy w dużych systemach rozproszonych za pomocą udostępnionej hierarchicznej przestrzeni nazw rejestrów danych (znode). Rejestry znode zawierają niewielkie ilości metadanych potrzebnych do zapewnienia koordynacji procesów: stan, lokalizację, konfigurację itd.

## <a name="programming-languages-on-hdinsight"></a>Języki programowania w usłudze HDInsight
Klastry HDInsight — klastry platform Hadoop, HBase, Storm i Spark — obsługują pewną liczbę języków programowania, ale niektóre nie są instalowane domyślnie. W przypadku bibliotek, modułów lub pakietów niezainstalowanych domyślnie do instalacji składnika należy użyć akcji skryptu. Zobacz temat [Script action development with HDInsight](hdinsight-hadoop-script-actions-linux.md) (Tworzenie akcji skryptów w usłudze HDInsight).

### <a name="default-programming-language-support"></a>Domyślna obsługa języka programowania
Domyślnie klastry usługi HDInsight obsługują języki:

* Java
* Python

Dodatkowe języki można zainstalować za pomocą akcji skryptu: [Script action development with HDInsight](hdinsight-hadoop-script-actions-linux.md) (Tworzenie akcji skryptów w usłudze HDInsight).

### <a name="java-virtual-machine-jvm-languages"></a>Języki maszyny wirtualnej Java (JVM)
Wiele języków innych niż Java można uruchomić za pomocą maszyny wirtualnej Java (JVM), jednak do uruchomienia niektórych z nich mogą być potrzebne dodatkowe składniki zainstalowane w klastrze.

Te języki działające w oparciu o JVM są obsługiwane w klastrach usługi HDInsight:

* Clojure
* Jython (Python dla platformy Java)
* Scala

### <a name="hadoop-specific-languages"></a>Języki specyficzne dla platformy Hadoop
Klastry HDInsight obsługują następujące języki specyficzne dla ekosystemu Hadoop:

* Pig Latin do zadań Pig
* HiveQL do zadań Hive oraz SparkSQL

## <a name="a-nameadvantageaadvantages-of-hadoop-in-the-cloud"></a><a name="advantage"></a>Zalety platformy Hadoop w chmurze
W ramach ekosystemu chmury Azure platforma Hadoop w usłudze HDInsight zapewnia pewne korzyści, m.in.:

* automatyczne inicjowanie klastrów platformy Hadoop. Znacznie łatwiej jest utworzyć klastry usługi HDInsight niż ręcznie konfigurować klastry platformy Hadoop. Aby uzyskać szczegółowe informacje, zobacz temat [Provision Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) (Inicjowanie obsługi klastrów platformy Hadoop w usłudze HDInsight).
* Najnowocześniejsze składniki platformy Hadoop. Aby uzyskać szczegółowe informacje, zobacz [Hadoop components, versioning, and service offerings in HDInsight][component-versioning] (Składniki, wersje i oferty usługowe platformy Hadoop w usłudze HDInsight).
* Wysoka dostępność i niezawodność klastrów. Aby uzyskać szczegółowe informacje, zobacz temat [Availability and reliability of Hadoop clusters in HDInsight](hdinsight-high-availability-linux.md) (Dostępność i niezawodność klastrów platformy Hadoop w usłudze HDInsight).
* Wydajny i oszczędny magazyn danych z magazynem Azure Blob Storage lub Azure Data Lake Store, obie opcje magazynowania zgodne z platformą Hadoop. Aby uzyskać szczegółowe informacje, zobacz temat [Używanie usługi Azure Blob Storage z usługą Hadoop w usłudze HDInsight](hdinsight-hadoop-use-blob-storage.md) lub [Use Data Lake Store with an HDInsight cluster](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal) (Używanie usługi Data Lake Store z klastrem usługi HDInsight).
* Integracja z innymi usługami Azure, m.in. [Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/) i [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).
* Dodatkowe rozmiary i typy maszyn wirtualnych do uruchamiania klastrów usługi HDInsight. Aby uzyskać szczegółowe informacje, zobacz [Hadoop components, versioning, and service offerings in HDInsight][component-versioning] (Składniki, wersje i oferty usługowe platformy Hadoop w usłudze HDInsight).
* Skalowanie klastra. Skalowanie klastra umożliwia zmianę liczby węzłów klastra działającego w usłudze HDInsight bez konieczności jego usuwania lub ponownego tworzenia.
* Obsługa usługi Virtual Network. Klastrów HDInsight można używać w ramach usługi Azure Virtual Network do obsługi izolacji zasobów chmury lub scenariuszy hybrydowych łączących zasoby chmury z tymi znajdującymi się w centrum danych.
* Niski koszt rejestracji. Rozpocznij [bezpłatny okres próbny](https://azure.microsoft.com/free/) lub zapoznaj się ze [szczegółowymi cenami usługi HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

Aby dowiedzieć się więcej o zaletach korzystania z platformy Hadoop w usłudze HDInsight, zobacz [stronę opisującą funkcje platformy Azure w usłudze HDInsight][marketing-page].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard i HDInsight Premium
Usługa HDInsight zapewnia oferty danych big data w chmurze w dwóch różnych kategoriach, Standard i Premium. HDInsight Standard zapewnia klaster w skali przedsiębiorstwa, który może być używany przez organizacje do uruchamiania obciążeń typu big data. HDInsight Premium również zapewnia tę funkcję oraz zaawansowane możliwości analityczne i zabezpieczenia dla klastra usługi HDInsight. Więcej informacji można znaleźć w temacie [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

## <a name="a-idresourcesaresources-for-learning-more-about-big-data-analysis-hadoop-and-hdinsight"></a><a id="resources"></a>Zasoby zawierające więcej informacji na temat analizy danych big data, platformy Hadoop oraz usługi HDInsight
Warto skorzystać z wprowadzenia do platformy Hadoop w chmurze oraz analizy danych big data oraz zasobów podanych poniżej.

### <a name="hadoop-documentation-for-hdinsight"></a>Dokumentacja dotycząca platformy Hadoop dla usługi HDInsight
* [HDInsight — dokumentacja](https://azure.microsoft.com/documentation/services/hdinsight/): strona zawierająca dokumentację platformy Hadoop w usłudze Azure HDInsight wraz z linkami do artykułów, filmów wideo i innych zasobów.
* [Wprowadzenie do platformy Hadoop w usłudze HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): samouczek Szybki start dotyczący aprowizowania klastrów platformy Hadoop w usłudze HDInsight i uruchamiania przykładowych zapytań Hive.
* [Wprowadzenie do platformy Spark w usłudze HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): samouczek Szybki start dotyczący tworzenia klastra Spark i uruchamiania interaktywnych zapytań SQL Spark.
* [Use R Server on HDInsight](hdinsight-hadoop-r-server-get-started.md) (Stosowanie platformy R Server w usłudze HDInsight): rozpoczynanie korzystania z platformy R Server w usłudze HDInsight Premium.
* [Provision HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md) (Aprowizowanie klastrów HDInsight): informacje o sposobie inicjowania obsługi klastra platformy Hadoop w usłudze HDInsight za pośrednictwem portalu Azure, interfejsu wiersza polecenia platformy Azure lub programu Azure PowerShell.

### <a name="apache-hadoop"></a>Apache Hadoop
* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: więcej informacji dotyczących biblioteki oprogramowania platformy Apache Hadoop, struktury umożliwiającej przetwarzanie rozproszone dużych zestawów danych w klastrach komputerów.
* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">System plików HDFS</a>: więcej informacji na temat architektury i projektu rozproszonego systemu plików Hadoop, podstawowego systemu magazynu używanego przez aplikacje platformy Hadoop.
* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce Tutorial</a> (Samouczek MapReduce): więcej informacji na temat struktury programistycznej używanej do tworzenia aplikacji Hadoop, które szybko przetwarzają równolegle duże ilości danych w dużych klastrach węzłów obliczeniowych.

### <a name="microsoft-business-intelligence"></a>Analiza biznesowa Microsoft
Znane narzędzia do analizy biznesowej (BI), np. Excel, PowerPivot, SQL Server Analysis Services i SQL Server Reporting Services, pobierają, analizują i raportują dane zintegrowane z usługą HDInsight przy użyciu dodatku Power Query lub sterownika ODBC usługi Microsoft Hive.

Te narzędzia analizy biznesowej mogą ułatwić analizę danych big data:

* [Podłączenie programu Excel do platformy Hadoop za pomocą dodatku Power Query](hdinsight-connect-excel-power-query.md): informacje na temat łączenia programu Excel z kontem usługi Azure Storage, na którym są przechowywane dane skojarzone z klastrem usługi HDInsight przy użyciu programu Microsoft Power Query dla programu Excel. Wymagana stacja robocza z systemem Windows. Działa z klastrami w systemie Linux lub Windows.
* [Podłączanie programu Excel do platformy Hadoop za pomocą sterownika ODBC usługi Microsoft Hive](hdinsight-connect-excel-hive-odbc-driver.md): informacje na temat importowania danych z usługi HDInsight za pomocą sterownika ODBC usługi Microsoft Hive. Wymagana stacja robocza z systemem Windows. Działa z klastrami w systemie Linux lub Windows.
* [Platforma Microsoft Cloud](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): informacje na temat Power BI dla pakietu Office 365, pobierania wersji próbnej programu SQL Server oraz konfiguracji programów SharePoint Server 2013 i SQL Server BI.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).

[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/



<!--HONumber=Dec16_HO2-->


