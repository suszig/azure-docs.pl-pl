---
title: Co to jest platforma Hadoop w chmurze? Wprowadzenie do usługi HDInsight | Microsoft Docs
description: Co to jest platforma Hadoop w chmurze i jak jest zarządzana w usłudze Microsoft Azure HDInsight? Wprowadzenie do składników i analizy danych big data w ramach platformy Hadoop.
keywords: big data analysis,introduction to hadoop,what is hadoop,hadoop in the cloud,hadoop technology stack,hadoop ecosystem
services: hdinsight
documentationcenter: ''
author: cjgronlund
manager: jhubbard
editor: cgronlun

ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/21/2016
ms.author: cgronlun

---
# Co to jest platforma Hadoop w chmurze? Wprowadzenie do ekosystemu platformy Hadoop w usłudze HDInsight
Wprowadzenie do platformy Hadoop, jej ekosystemu i danych big data w usłudze Azure HDInsight: co to jest platforma Hadoop w usłudze HDInsight i jakie są składniki platformy Hadoop, wspólna terminologia i scenariusze analizy danych big data? Zapoznaj się z samouczkami, dokumentacją i zasobami dotyczącymi korzystania z platformy Hadoop w chmurze w usłudze HDInsight.

## Co to jest platforma Hadoop w usłudze HDInsight?
Usługa Azure HDInsight korzysta z dystrybucji w usłudze Hadoop w ramach platformy **Hortonworks Data Platform (HDP)**. Usługa HDInsight wdraża zarządzane klastry platformy Apache Hadoop w chmurze oraz aprowizuje je, zapewniając niezawodne i wysoko dostępne środowisko oprogramowania przeznaczone do przetwarzania, analizy i raportowania danych big data.  

Platforma Hadoop często oznacza cały ekosystem składników Hadoop, m.in. Apache HBase, Apache Spark i Apache Storm, a także inne technologie w obszarze parasola platformy Hadoop. Aby zapoznać się ze szczegółami, zobacz [Omówienie ekosystemu platformy Hadoop w usłudze HDInsight](#overview) poniżej.

## Co to są dane big data?
Dane big data to dowolna duża porcja informacji cyfrowych. Mogą obejmować takie dane, jak teksty z usługi Twitter, dane pochodzące z czujnika sprzętu przemysłowego albo informacje dotyczące przeglądania Internetu przez klienta i zakupów w witrynie sieci Web. Dane big data mogą być historyczne (dane przechowywane) lub w czasie rzeczywistym (to znaczy przesyłane strumieniowo bezpośrednio ze źródła). Dane big data są gromadzone coraz szybciej, w coraz większych ilościach i coraz bardziej różnorodnych formatach.

Aby dane big data zapewniły wiarygodną analizę lub wgląd, należy nie tylko zgromadzić odpowiednie dane i zadać odpowiednie pytania. Dane muszą być również dostępne, czyszczone, poddane analizie, a następnie w przystępny sposób przedstawione. Właśnie te czynności ułatwia funkcja analizy danych big data w ramach platformy Hadoop w usłudze HDInsight.

## <a name="overview"></a>Omówienie ekosystemu platformy Hadoop w usłudze HDInsight
Usługa HDInsight jest wdrożeniem chmury na platformie Microsoft Azure należącej do dynamicznie rozwijającego się stosu technologii Apache Hadoop, stanowiącego docelowe rozwiązanie w przypadku analizy danych big data. Obejmuje ona wdrożenia Apache Spark, HBase, Storm, Pig, Hive, Sqoop, Oozie, Ambari itd. Usługa HDInsight integruje się również z narzędziami analizy biznesowej (BI), takimi jak Power BI, Excel, SQL Server Analysis Services oraz SQL Server Reporting Services.

### Hadoop, HBase, Spark, Storm i klastry niestandardowe
Usługa HDInsight zapewnia konfiguracje klastrów dla technologii Apache Hadoop, Spark, HBase lub Storm. Można też [dostosować klastry za pomocą akcji skryptów](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoop** (obciążenie „Zapytanie”): zapewnia niezawodny magazyn danych przy użyciu [systemu plików HDFS](#hdfs) i prosty model programowania [MapReduce](#mapreduce) do równoległego przetwarzania i analizy danych.
* **<a target="_blank" href="http://spark.apache.org/">Apache Spark</a>**: platforma przetwarzania równoległego obsługująca przetwarzanie w pamięci w celu zwiększania wydajności aplikacji do analizy danych big data. Platforma Spark jest odpowiednia do języka SQL, strumieniowego przesyłania danych oraz uczenia maszynowego. Zobacz temat [Przegląd: platforma Apache Spark w usłudze HDInsight w systemie Linux](hdinsight-apache-spark-overview.md).
* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>** (obciążenie „NoSQL”): baza danych NoSQL oparta na platformie Hadoop, która zapewnia dostęp losowy i wysoki poziom spójności w przypadku dużych ilości nieustrukturyzowanych i częściowo ustrukturyzowanych danych — potencjalnie miliardów wierszy pomnożonych przez miliony kolumn. Zobacz temat [Omówienie baz danych HBase w usłudze HDInsight](hdinsight-hbase-overview.md).
* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache Storm</a>** (obciążenie „Strumień”): rozproszony system obliczeniowy działający w czasie rzeczywistym do szybkiego przetwarzania dużych strumieni danych. Storm jest oferowany jako zarządzany klaster w usłudze HDInsight. Zobacz temat [Analyze real-time sensor data using Storm and Hadoop](hdinsight-storm-sensor-data-analysis.md) (Analizowanie danych czujnika w czasie rzeczywistym przy użyciu platform Storm i Hadoop).

### Przykładowe skrypty dostosowania
Akcje skryptu to skrypty, które są uruchamiane podczas inicjowania obsługi klastra i mogą służyć do instalowania dodatkowych składników w klastrze. W przypadku klastrów z systemem Linux są to skrypty Bash.

Zespół usługi HDInsight zapewnia następujące przykładowe skrypty:

* [Hue](hdinsight-hadoop-hue-linux.md): zestaw aplikacji sieci web umożliwiający interakcję z klastrem. Tylko klastry z systemem Linux.
* [Giraph](hdinsight-hadoop-giraph-install-linux.md): przetwarzanie wykresów do modelowania relacji między rzeczami lub osobami.
* [R](hdinsight-hadoop-r-scripts-linux.md): język i środowisko typu open source do statystycznego przetwarzania danych używane w uczeniu maszynowym.
* [Solr](hdinsight-hadoop-solr-install-linux.md): platforma wyszukiwania w skali przedsiębiorstwa, która umożliwia wyszukiwanie pełnotekstowe danych.

Informacje na temat tworzenia własnych akcji skryptu można znaleźć w temacie [Script Action development with HDInsight](hdinsight-hadoop-script-actions-linux.md) (Tworzenie akcji skryptu za pomocą usługi HDInsight).

## Jakie są składniki i narzędzia platformy Hadoop?
W klastrach HDInsight znajdują się następujące składniki i narzędzia.

* **[Ambari](#ambari)**: inicjowanie obsługi klastrów, zarządzanie nimi, ich monitorowanie oraz narzędzia.
* **[Avro](#avro)** (biblioteka Microsoft .NET dla Avro): serializacja danych w środowisku Microsoft .NET.
* **[Hive i HCatalog](#hive)**: przeszukiwanie przypominające Structured Query Language (SQL) oraz warstwa zarządzania tabeli i magazynu.
* **[Mahout](#mahout)**: uczenie maszynowe.
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

### <a name="ambari"></a>Ambari
Apache Ambari służy do inicjowania obsługi, monitorowania klastrów platformy Apache Hadoop oraz zarządzania nimi. Obejmuje intuicyjny zestaw narzędzi operatora oraz niezawodny zestaw interfejsów API, które neutralizują złożoność platformy Hadoop, upraszczając działanie klastrów. Klastry HDInsight oparte na systemie Linux zapewniają zarówno interfejs użytkownika w sieci Web Ambari, jak i interfejs API REST Ambari, a klastry oparte na systemie Windows zapewniają podzestaw interfejsu API REST. Widoki Ambari w klastrach HDInsight zapewniają funkcje interfejsu użytkownika wtyczek.

Zobacz tematy [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md) (Zarządzanie klastrami HDInsight przy użyciu Ambari), [Monitor Hadoop clusters in HDInsight using the Ambari API](hdinsight-monitor-use-ambari-api.md) (Monitorowanie klastrów platformy Hadoop w HDInsight przy użyciu Ambari API) oraz <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API reference</a> (Dokumentacja interfejsu API Apache Ambari).

### <a name="avro"></a>Avro (biblioteka programu Microsoft .NET dla Avro)
Biblioteka Microsoft .NET dla Avro wdraża format wymiany kompaktowych danych binarnych Apache Avro do serializacji dla środowiska Microsoft .NET. Używa ona formatu <a target="_blank" href="http://www.json.org/">JavaScript Object Notation (JSON)</a> do definiowania schematu z niesprecyzowanym językiem, który zapewnia współdziałanie języków, co oznacza, że dane serializowane w jednym języku mogą być odczytywane w innym. Szczegółowe informacje dotyczące formatu można znaleźć w temacie <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro Specification</a> (Specyfikacja systemu Apache Avro).
Format plików Avro obsługuje rozproszony model programowania MapReduce. Pliki są „podzielne”, co oznacza, że można wyszukiwać dowolne miejsce w pliku i rozpocząć odczytywanie od określonego bloku. Aby dowiedzieć się, w jaki sposób, zobacz temat [Serialize data with the Microsoft .NET Library for Avro](hdinsight-dotnet-avro-serialization.md) (Serializacja danych za pomocą biblioteki Microsoft .NET dla Avro).

### <a name="hdfs"></a>SYSTEM PLIKÓW HDFS
Rozproszony system plików Hadoop (HDFS) jest rozproszonym systemem plików, który wraz z MapReduce i YARN stanowi podstawę ekosystemu platformy Hadoop. HDFS jest standardowym systemem plików klastrów platformy Hadoop w usłudze HDInsight.

### <a name="hive"></a>Hive i HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> to oprogramowanie magazynu danych oparte na platformie Hadoop, które pozwala wysyłać zapytania do dużych zestawów danych oraz zarządzać nimi w magazynie rozproszonym przy użyciu języka przypominającego SQL o nazwie HiveQL. Hive, tak samo jak Pig, jest abstrakcją nałożoną na MapReduce. Uruchomione oprogramowanie Hive przekłada zapytania na serię zadań MapReduce. Oprogramowanie Hive jest koncepcyjnie bliższe relacyjnemu systemowi zarządzania bazami danych niż program Pig i dlatego jest odpowiednie do używania z bardziej ustrukturyzowanymi danymi. W przypadku danych bez struktury lepszym rozwiązaniem jest użycie programu Pig. Zobacz temat [Use Hive with Hadoop in HDInsight](hdinsight-use-hive.md) (Używanie Hive w ramach platformy Hadoop w usłudze HDInsight).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> to warstwa zarządzania tabeli i magazynu dla platformy Hadoop, która zapewnia użytkownikom relacyjny widok danych. W HCatalog można odczytać i zapisać pliki w dowolnym formacie, dla którego można napisać Hive SerDe (serializator-deserializator).

### <a name="mahout"></a>Mahout
<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> to skalowalna biblioteka algorytmów uczenia maszynowego, która działa na platformie Hadoop. Zgodnie z zasadami statystyki aplikacje do uczenia maszynowego uczą systemy uczenia z danych oraz korzystania z wcześniejszych wyników, aby określić zachowanie w przyszłości. Zobacz temat [Generate movie recommendations using Mahout on Hadoop](hdinsight-mahout.md) (Generowanie zaleceń filmu przy użyciu Mahout na platformie Hadoop).

### <a name="mapreduce"></a>MapReduce
Funkcja MapReduce to starsza platforma oprogramowania dla platformy Hadoop do zapisywania aplikacji w celu równoległego przetwarzania wsadowego zestawów danych big data. Zadanie MapReduce dzieli duże zestawy danych i organizuje dane w pary klucz-wartość do przetworzenia.

[YARN](#yarn), zwany też MapReduce 2.0, to menedżer zasobów nowej generacji oraz platforma aplikacji na platformie Hadoop. Zadania MapReduce działają w ramach platformy YARN.

Aby uzyskać więcej informacji o MapReduce, zobacz temat <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> w witrynie Wiki dotyczącej platformy Hadoop.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> to system koordynacji przepływu pracy, który zarządza zadaniami na platformie Hadoop. Jest zintegrowany ze stosem platformy Hadoop i obsługuje zadania platformy Hadoop dla MapReduce, Pig, Hive i Sqoop. Może również służyć do planowania zadań specyficznych dla systemu, np. programów Java lub skryptów powłoki. Zobacz [Use Oozie with Hadoop](hdinsight-use-oozie.md) (Stosowanie systemu Oozie z platformą Hadoop).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> to warstwa relacyjnej bazy danych za pośrednictwem HBase. Phoenix obejmuje sterownik JDBC, który umożliwia użytkownikom bezpośrednie wysyłanie zapytań do tabel SQL oraz zarządzenie nimi. Phoenix tłumaczy zapytania i inne instrukcje do macierzystych wywołań interfejsu API NoSQL — zamiast używać MapReduce — umożliwiając szybsze zastosowania w ramach magazynów NoSQL. Zobacz temat [Use Apache Phoenix and SQuirreL with HBase clusters](hdinsight-hbase-phoenix-squirrel.md) (Używanie Apache Phoenix i SQuirreL z klastrami HBase).

### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> to platforma wysokiego poziomu umożliwiająca wykonywanie złożonych przekształceń MapReduce na bardzo dużych zestawach danych przy użyciu prostego języka skryptowego o nazwie Pig Latin. Pig tłumaczy skrypty Pig Latin po to, by działały w ramach platformy Hadoop. Aby rozszerzyć język Pig Latin, można utworzyć funkcje zdefiniowane przez użytkownika (UDF). Zobacz [Use Pig with Hadoop](hdinsight-use-pig.md) (Stosowanie języka Pig z platformą Hadoop).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> to narzędzie, które przenosi dane zbiorcze pomiędzy platformą Hadoop a relacyjnymi bazami danych, takimi jak SQL lub inne strukturalne magazyny danych, w jak najbardziej wydajny sposób. Zobacz temat [Use Sqoop with Hadoop](hdinsight-use-sqoop.md) (Korzystanie ze Sqoop przy użyciu platformy Hadoop).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> to struktura aplikacji działająca w oparciu o Hadoop YARN, wykonująca złożone, acykliczne wykresy będące wynikiem ogólnego przetwarzania danych. Jest bardziej elastycznym i wydajnym następcą platformy MapReduce, zwiększającym skuteczność wielkoskalowych procesów przetwarzających duże ilości danych, takich jak Hive. Zobacz część [„Use Apache Tez for improved performance” („Używanie Apache Tez w celu zwiększenia wydajności”) w temacie Use Hive and HiveQL (Korzystanie z Hive i HiveQL) ](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>YARN
Struktura Apache YARN to platforma MapReduce nowej generacji (MapReduce 2.0 lub MRv2), która obsługuje scenariusze przetwarzania danych poza przetwarzaniem wsadowym MapReduce z większą skalowalnością i obejmuje przetwarzanie w czasie rzeczywistym. YARN umożliwia zarządzanie zasobami oraz rozproszoną strukturę aplikacji. Zadania MapReduce działają w ramach platformy YARN.

Aby dowiedzieć się więcej o YARN, zobacz temat <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>.

### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordynuje procesy w dużych systemach rozproszonych za pomocą udostępnionej hierarchicznej przestrzeni nazw rejestrów danych (znode). Rejestry znode zawierają niewielkie ilości metadanych potrzebnych do zapewnienia koordynacji procesów: stan, lokalizację, konfigurację itd.

## Języki programowania w usłudze HDInsight
Klastry HDInsight — klastry platform Hadoop, HBase, Storm i Spark — obsługują pewną liczbę języków programowania, ale niektóre nie są instalowane domyślnie. W przypadku bibliotek, modułów lub pakietów niezainstalowanych domyślnie do instalacji składnika należy użyć akcji skryptu. Zobacz temat [Script action development with HDInsight](hdinsight-hadoop-script-actions-linux.md) (Tworzenie akcji skryptów w usłudze HDInsight).

### Domyślna obsługa języka programowania
Domyślnie klastry usługi HDInsight obsługują języki:

* Java
* Python

Dodatkowe języki można zainstalować za pomocą akcji skryptu: [Script action development with HDInsight](hdinsight-hadoop-script-actions-linux.md) (Tworzenie akcji skryptów w usłudze HDInsight).

### Języki maszyny wirtualnej Java (JVM)
Wiele języków innych niż Java można uruchomić za pomocą maszyny wirtualnej Java (JVM), jednak do uruchomienia niektórych z nich mogą być potrzebne dodatkowe składniki zainstalowane w klastrze.

Te języki działające w oparciu o JVM są obsługiwane w klastrach usługi HDInsight:

* Clojure
* Jython (Python dla platformy Java)
* Scala

### Języki specyficzne dla platformy Hadoop
Klastry HDInsight zapewniają obsługę następujących języków specyficznych dla ekosystemu Hadoop:

* Pig Latin do zadań Pig
* HiveQL do zadań Hive oraz SparkSQL

## <a name="advantage"></a>Zalety platformy Hadoop w chmurze
W ramach ekosystemu chmury Azure platforma Hadoop w usłudze HDInsight zapewnia pewne korzyści, m.in.:

* automatyczne inicjowanie klastrów platformy Hadoop. Znacznie łatwiej jest utworzyć klastry usługi HDInsight niż ręcznie konfigurować klastry platformy Hadoop. Aby uzyskać szczegółowe informacje, zobacz temat [Provision Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) (Inicjowanie obsługi klastrów platformy Hadoop w usłudze HDInsight).
* Najnowocześniejsze składniki platformy Hadoop. Aby uzyskać szczegółowe informacje, zobacz [Hadoop components, versioning, and service offerings in HDInsight][component-versioning] (Składniki, wersje i oferty usługowe platformy Hadoop w usłudze HDInsight).
* Wysoka dostępność i niezawodność klastrów. Aby uzyskać szczegółowe informacje, zobacz temat [Availability and reliability of Hadoop clusters in HDInsight](hdinsight-high-availability-linux.md) (Dostępność i niezawodność klastrów platformy Hadoop w usłudze HDInsight).
* Wydajny i oszczędny magazyn danych z magazynem Azure Blob, opcja zgodna z platformą Hadoop. Aby uzyskać szczegółowe informacje, zobacz temat [Use Azure Blob storage with Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md) (Korzystanie z magazynu Azure Blob przy użyciu platformy Hadoop w ramach usługi HDInsight).
* Integracja z innymi usługami Azure, m.in. [Web Apps](https://azure.microsoft.com/documentation/services/app-service/web/) i [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).
* Dodatkowe rozmiary i typy maszyn wirtualnych do uruchamiania klastrów usługi HDInsight. Aby uzyskać szczegółowe informacje, zobacz [Hadoop components, versioning, and service offerings in HDInsight][component-versioning] (Składniki, wersje i oferty usługowe platformy Hadoop w usłudze HDInsight).
* Skalowanie klastra. Skalowanie klastra umożliwia zmianę liczby węzłów klastra działającego w usłudze HDInsight bez konieczności jego usuwania lub ponownego tworzenia.
* Obsługa usługi Virtual Network. Klastrów HDInsight można używać w ramach usługi Azure Virtual Network do obsługi izolacji zasobów chmury lub scenariuszy hybrydowych łączących zasoby chmury z tymi znajdującymi się w centrum danych.
* Niski koszt rejestracji. Rozpocznij [bezpłatny okres próbny](https://azure.microsoft.com/free/) lub zapoznaj się ze [szczegółowymi cenami usługi HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

Aby dowiedzieć się więcej o zaletach korzystania z platformy Hadoop w usłudze HDInsight, zobacz [stronę opisującą funkcje platformy Azure w usłudze HDInsight][marketing-page].

## HDInsight Standard i HDInsight Premium
Usługa HDInsight zapewnia oferty danych big data w chmurze w dwóch różnych kategoriach, Standard i Premium. HDInsight Standard zapewnia klaster w skali przedsiębiorstwa, który może być używany przez organizacje do uruchamiania obciążeń typu big data. HDInsight Premium również zapewnia tę funkcję oraz zaawansowane możliwości analityczne i zabezpieczenia dla klastra usługi HDInsight. Więcej informacji można znaleźć w temacie [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

## <a id="resources"></a>Zasoby zawierające więcej informacji na temat analizy danych big data, platformy Hadoop oraz usługi HDInsight
Warto skorzystać z wprowadzenia do platformy Hadoop w chmurze oraz analizy danych big data oraz zasobów podanych poniżej.

### Dokumentacja dotycząca platformy Hadoop dla usługi HDInsight
* [HDInsight — dokumentacja](https://azure.microsoft.com/documentation/services/hdinsight/): strona zawierająca dokumentację platformy Hadoop w usłudze Azure HDInsight wraz z linkami do artykułów, filmów wideo i innych zasobów.
* [Wprowadzenie do platformy Hadoop w usłudze HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): samouczek Szybki start dotyczący aprowizowania klastrów platformy Hadoop w usłudze HDInsight i uruchamiania przykładowych zapytań Hive.
* [Wprowadzenie do platformy Spark w usłudze HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): samouczek Szybki start dotyczący tworzenia klastra Spark i uruchamiania interaktywnych zapytań SQL Spark.
* [Use R Server on HDInsight](hdinsight-hadoop-r-server-get-started.md) (Stosowanie platformy R Server w usłudze HDInsight): rozpoczynanie korzystania z platformy R Server w usłudze HDInsight Premium.
* [Provision HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md) (Aprowizowanie klastrów HDInsight): informacje o sposobie inicjowania obsługi klastra platformy Hadoop w usłudze HDInsight za pośrednictwem witryny Azure Portal, interfejsu wiersza polecenia platformy Azure lub programu Azure PowerShell.

### Apache Hadoop
* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: więcej informacji dotyczących biblioteki oprogramowania platformy Apache Hadoop, struktury umożliwiającej przetwarzanie rozproszone dużych zestawów danych w klastrach komputerów.
* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">System plików HDFS</a>: więcej informacji na temat architektury i projektu rozproszonego systemu plików Hadoop, podstawowego systemu magazynu używanego przez aplikacje platformy Hadoop.
* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce Tutorial</a> (Samouczek MapReduce): więcej informacji na temat struktury programistycznej używanej do tworzenia aplikacji Hadoop, które szybko przetwarzają równolegle duże ilości danych w dużych klastrach węzłów obliczeniowych.

### Analiza biznesowa Microsoft
Znane narzędzia do analizy biznesowej (BI), np. Excel, PowerPivot, SQL Server Analysis Services i SQL Server Reporting Services, pobierają, analizują i raportują dane zintegrowane z usługą HDInsight przy użyciu dodatku Power Query lub sterownika ODBC usługi Microsoft Hive.

Te narzędzia analizy biznesowej mogą ułatwić analizę danych big data:

* [Podłączenie programu Excel do platformy Hadoop za pomocą dodatku Power Query](hdinsight-connect-excel-power-query.md): informacje na temat łączenia programu Excel z kontem usługi Azure Storage, na którym są przechowywane dane skojarzone z klastrem usługi HDInsight przy użyciu programu Microsoft Power Query dla programu Excel. Wymagana stacja robocza z systemem Windows. Współpracuje z klastrami systemu Windows i Linux.
* [Podłączanie programu Excel do platformy Hadoop za pomocą sterownika ODBC usługi Microsoft Hive](hdinsight-connect-excel-hive-odbc-driver.md): informacje na temat importowania danych z usługi HDInsight za pomocą sterownika ODBC usługi Microsoft Hive. Wymagana stacja robocza z systemem Windows. Współpracuje z klastrami systemu Windows i Linux.
* [Platforma Microsoft Cloud](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): informacje na temat Power BI dla pakietu Office 365, pobierania wersji próbnej programu SQL Server oraz konfiguracji programów SharePoint Server 2013 i SQL Server BI.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx).

[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/



<!--HONumber=Oct16_HO1-->


