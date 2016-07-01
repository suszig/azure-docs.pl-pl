<properties
    pageTitle="Co to jest platforma Hadoop w chmurze? Wprowadzenie do usługi HDInsight | Microsoft Azure"
    description="Co to jest platforma Hadoop w chmurze i jak jest zarządzana w usłudze HDInsight? Wprowadzenie do składników i analizy danych big data w ramach platformy Hadoop."
    keywords="big data analysis,introduction to hadoop,what is hadoop,hadoop in the cloud"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="paulettm"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="03/29/2016"
   ms.author="cgronlun"/>


# Co to jest platforma Hadoop w chmurze? Wprowadzenie do składników platformy Hadoop w usłudze HDInsight w celu analizy danych big data

Wprowadzenie do platformy Hadoop, jej ekosystemu i danych big data w usłudze Azure HDInsight: co to jest platforma Hadoop w usłudze HDInsight i jakie są składniki platformy Hadoop, wspólna terminologia i scenariusze analizy danych big data? Zapoznaj się z samouczkami, dokumentacją i zasobami dotyczącymi korzystania z platformy Hadoop w chmurze w usłudze HDInsight.

## Co to jest platforma Hadoop w chmurze w usłudze HDInsight?

Azure HDInsight wdraża zarządzane klastry platformy Apache Hadoop w chmurze oraz inicjuje ich obsługę, zapewniając niezawodne i wysoko dostępne środowisko oprogramowania przeznaczone do przetwarzania, analizy i raportowania danych big data. Usługa HDInsight korzysta z dystrybucji w usłudze Hadoop w ramach platformy **Hortonworks Data Platform (HDP)**. Platforma Hadoop często oznacza cały ekosystem składników Hadoop, m.in. Apache HBase, Apache Spark i Apache Storm, a także inne technologie w obszarze parasola platformy Hadoop. Aby zapoznać się ze szczegółami, zobacz temat [Przegląd ekosystemu Hadoop w usłudze HDInsight](#overview) poniżej.


## Co to są dane big data?
Dane big data to dane gromadzone coraz szybciej, w coraz większych ilościach, coraz bardziej różnorodnych nieustrukturyzowanych formatach i w zmiennych kontekstach semantycznych.

Dane big data opisują dowolną dużą porcję informacji cyfrowych. Mogą obejmować zarówno teksty z serwisu Twitter, jak i dane pochodzące z czujnika sprzętu przemysłowego albo informacje dotyczące przeglądania Internetu przez klienta i zakupów z katalogu internetowego. Dane big data mogą być historyczne (dane przechowywane) lub w czasie rzeczywistym (to znaczy przesyłane strumieniowo bezpośrednio ze źródła).

Aby dane big data zapewniły wiarygodną analizę lub wgląd, należy nie tylko zgromadzić odpowiednie dane i zadać odpowiednie pytania. Dane muszą być również dostępne, czyszczone, poddane analizie, a następnie w przystępny sposób przedstawione. Właśnie te czynności ułatwia funkcja analizy danych big data w ramach platformy Hadoop w usłudze HDInsight.


## <a name="overview"></a>Omówienie ekosystemu platformy Hadoop w usłudze HDInsight

Usługa HDInsight jest wdrożeniem chmury na platformie Microsoft Azure należącej do dynamicznie rozwijającego się stosu technologii Apache Hadoop, stanowiącego docelowe rozwiązanie w przypadku analizy danych big data. Obejmuje ona wdrożenia Apache Spark, HBase, Storm, Pig, Hive, Sqoop, Oozie, Ambari itd. Usługa HDInsight integruje się również z narzędziami analizy biznesowej (BI), takimi jak Power BI, Excel, SQL Server Analysis Services oraz SQL Server Reporting Services.

### Klastry w systemie Linux

Usługa Azure HDInsight wdraża klastry Hadoop w chmurze w systemie **Linux** oraz inicjuje ich obsługę. Szczegółowe informacje znajdują się w tabeli poniżej.

Kategoria | Platforma Hadoop w systemie Linux
---------| -------------------
**System operacyjny klastra** | Ubuntu 12.04 Long Term Support (LTS)
**Typ klastra** | Hadoop, Spark, HBase, Storm
**Wdrożenie** | Portal Azure, Azure CLI, Azure PowerShell
**Interfejs użytkownika klastra** | Ambari
**Dostęp zdalny** | Secure Shell (SSH), REST API, ODBC, JDBC



### Hadoop, HBase, Spark, Storm i klastry niestandardowe

Usługa HDInsight zapewnia konfiguracje klastrów dla technologii Apache Hadoop, Spark, HBase lub Storm. Można też [dostosować klastry za pomocą akcji skryptów](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoop** (obciążenie „Zapytania”): zapewnia niezawodny magazyn danych przy użyciu [systemu plików HDFS](#HDFS) i prosty model programowania [MapReduce](#mapreduce) do równoległego przetwarzania i analizy danych.

* **<a target="_blank" href="http://spark.apache.org/">Apache Spark</a>**: platforma przetwarzania równoległego obsługująca przetwarzanie w pamięci w celu zwiększania wydajności aplikacji do analizy danych big data. Platforma Spark jest odpowiednia do języka SQL, strumieniowego przesyłania danych oraz uczenia maszynowego. Zobacz temat [Przegląd: platforma Apache Spark w usłudze HDInsight w systemie Linux](hdinsight-apache-spark-overview.md).

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>** (obciążenie „NoSQL”): baza danych NoSQL oparta na platformie Hadoop, która zapewnia dostęp losowy i wysoki poziom spójności w przypadku dużych ilości nieustrukturyzowanych i częściowo ustrukturyzowanych danych — potencjalnie miliardów wierszy pomnożonych przez miliony kolumn. Zobacz temat [Omówienie baz danych HBase w usłudze HDInsight](hdinsight-hbase-overview.md).

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache Storm</a>** (obciążenie „Strumień”): rozproszony system obliczeniowy działający w czasie rzeczywistym do szybkiego przetwarzania dużych strumieni danych. Storm jest oferowany jako zarządzany klaster w usłudze HDInsight. Zobacz temat [Analyze real-time sensor data using Storm and Hadoop](hdinsight-storm-sensor-data-analysis.md) (Analizowanie danych czujnika w czasie rzeczywistym przy użyciu platform Storm i Hadoop).

#### Przykładowe skrypty dostosowania

Akcje skryptu to skrypty, które są uruchamiane podczas inicjowania obsługi klastra i mogą służyć do instalowania dodatkowych składników w klastrze. W przypadku klastrów z systemem Linux są to skrypty Bash.

Zespół usługi HDInsight zapewnia następujące przykładowe skrypty:

* [Hue](hdinsight-hadoop-hue-linux.md): zestaw aplikacji sieci web umożliwiający interakcję z klastrem. Tylko klastry z systemem Linux.

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): przetwarzanie wykresów do modelowania relacji między rzeczami lub osobami.

* [R](hdinsight-hadoop-r-scripts-linux.md): język i środowisko typu open source do statystycznego przetwarzania danych używane w uczeniu maszynowym.

* [Solr](hdinsight-hadoop-solr-install-linux.md): platforma wyszukiwania w skali przedsiębiorstwa, która umożliwia wyszukiwanie pełnotekstowe danych.

Informacje na temat tworzenia własnych akcji skryptu można znaleźć w temacie [Script Action development with HDInsight](hdinsight-hadoop-script-actions-linux.md) (Tworzenie akcji skryptu za pomocą usługi HDInsight).

## HDInsight Standard i HDInsight Premium

Usługa HDInsight zapewnia oferty danych big data w chmurze w dwóch różnych kategoriach, Standard i Premium. HDInsight Standard zapewnia klaster w skali przedsiębiorstwa, który może być używany przez organizacje do uruchamiania obciążeń typu big data. HDInsight Premium również zapewnia tę funkcję oraz zaawansowane możliwości analityczne i zabezpieczenia dla klastra usługi HDInsight. Więcej informacji można znaleźć w temacie [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

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

> [AZURE.NOTE] Aby uzyskać informacje na temat określonych składników i informacje o wersji, zobacz temat [What's new in the Hadoop cluster versions provided by HDInsight?][component-versioning] (Nowości w wersjach klastra platformy Hadoop w usłudze HDInsight) .

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
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> to system koordynacji przepływu pracy, który zarządza zadaniami na platformie Hadoop. Jest zintegrowany ze stosem platformy Hadoop i obsługuje zadania platformy Hadoop dla MapReduce, Pig, Hive i Sqoop. Może również służyć do planowania zadań specyficznych dla systemu, np. programów Java lub skryptów powłoki. Zobacz temat [Use a time-based Oozie Coordinator with Hadoop](hdinsight-use-oozie-coordinator-time.md) (Korzystanie z Oozie Coordinator działającego w tle przy użyciu platformy Hadoop).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> to warstwa relacyjnej bazy danych za pośrednictwem HBase. Phoenix obejmuje sterownik JDBC, który umożliwia użytkownikom bezpośrednie wysyłanie zapytań do tabel SQL oraz zarządzenie nimi. Phoenix tłumaczy zapytania i inne instrukcje do macierzystych wywołań interfejsu API NoSQL — zamiast używać MapReduce — umożliwiając szybsze zastosowania w ramach magazynów NoSQL. Zobacz temat [Use Apache Phoenix and SQuirreL with HBase clusters](hdinsight-hbase-phoenix-squirrel.md) (Używanie Apache Phoenix i SQuirreL z klastrami HBase).


### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> to platforma wysokiego poziomu umożliwiająca wykonywanie złożonych przekształceń MapReduce na bardzo dużych zestawach danych przy użyciu prostego języka skryptowego o nazwie Pig Latin. Pig tłumaczy skrypty Pig Latin po to, by działały w ramach platformy Hadoop. Aby rozszerzyć język Pig Latin, można utworzyć funkcje zdefiniowane przez użytkownika (UDF). Zobacz temat [Use Pig with Hadoop to analyze an Apache log file](hdinsight-use-pig.md) (Używanie Pig na platformie Hadoop do analizowania pliku dziennika Apache).

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

* Najnowocześniejsze składniki platformy Hadoop. Aby uzyskać szczegółowe informacje, zobacz temat [What's new in the Hadoop cluster versions provided by HDInsight?][component-versioning] (Nowości w wersjach klastra platformy Hadoop w usłudze HDInsight).

* Wysoka dostępność i niezawodność klastrów.  Do klastrów platformy Hadoop został dodany drugi węzeł główny wdrożony w ramach usługi HDInsight, co zwiększa dostępność usługi. Standardowe wdrożenia klastrów platformy Hadoop zwykle mają jeden węzeł główny. W usłudze HDInsight ten pojedynczy punkt awarii został usunięty dzięki dodaniu drugiego węzła głównego. Przełącznik nowej konfiguracji klastra HA nie powoduje zmiany ceny klastra, o ile klienci nie utworzą klastrów z bardzo dużym węzłem głównym zamiast domyślnego węzła o dużym rozmiarze.

    Aby uzyskać szczegółowe informacje, zobacz temat [Availability and reliability of Hadoop clusters in HDInsight](hdinsight-high-availability-linux.md) (Dostępność i niezawodność klastrów platformy Hadoop w usłudze HDInsight).

* Wydajny i oszczędny magazyn danych z magazynem Azure Blob, opcja zgodna z platformą Hadoop. Aby uzyskać szczegółowe informacje, zobacz temat [Use Azure Blob storage with Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md) (Korzystanie z magazynu Azure Blob przy użyciu platformy Hadoop w ramach usługi HDInsight).

* Integracja z innymi usługami Azure, m.in. [Web Apps](../documentation/services/app-service/web/) i [SQL Database](../documentation/services/sql-database/).

* Dodatkowe rozmiary maszyn wirtualnych. Klastry HDInsight są dostępne w różnych typach maszyn wirtualnych o różnych rozmiarach. Klastry HDInsight mogą teraz wykorzystywać rozmiary od A2 do A7, przeznaczone do celów ogólnych, węzły D-Series, które zawierają dyski półprzewodnikowe (SSD) oraz procesory szybsze o 60 procent, a także rozmiary A8 i A9, obsługiwane przez InfiniBand w celu szybkiej transmisji w sieci. Klienci korzystający z bazy danych Apache HBase w usłudze Azure HDInsight mogą zyskać na większych konfiguracjach pamięci D-Series, które zwiększają wydajność. Klienci korzystający z platformy Apache Storm w usłudze Azure HDInsight mogą również zyskać dzięki dodatkowej pamięci w przypadku ładowania większych zestawów danych referencyjnych oraz szybszych procesorów CPU umożliwiających uzyskanie większej produktywności.

* Skalowanie klastra. Skalowanie klastra umożliwia zmianę liczby węzłów klastra działającego w usłudze HDInsight bez konieczności jego usuwania lub ponownego tworzenia.

* Obsługa usługi Virtual Network. Klastrów HDInsight można używać w ramach usługi Azure Virtual Network do obsługi izolacji zasobów chmury lub scenariuszy hybrydowych łączących zasoby chmury z tymi znajdującymi się w centrum danych.

* Niski koszt rejestracji. Rozpocznij [bezpłatny okres próbny](/pricing/free-trial/) lub zapoznaj się ze [szczegółowym cennikiem usługi HDInsight](/pricing/details/hdinsight/).


Aby dowiedzieć się więcej o zaletach korzystania z platformy Hadoop w usłudze HDInsight, zobacz [stronę opisującą funkcje platformy Azure w usłudze HDInsight][marketing-page].



## <a id="resources"></a>Zasoby zawierające więcej informacji na temat analizy danych big data, platformy Hadoop oraz usługi HDInsight

Warto skorzystać z wprowadzenia do platformy Hadoop w chmurze oraz analizy danych big data oraz zasobów podanych poniżej.


### Dokumentacja dotycząca platformy Hadoop dla usługi HDInsight

* [Dokumentacja dotycząca usługi HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/): strona zawierająca dokumentację usługi Azure HDInsight wraz z łączami do artykułów, filmów wideo i innych zasobów.

* [Get started with HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md) (Wprowadzenie do usługi HDInsight w systemie Linux): samouczek szybki start dotyczący inicjowania obsługi klastrów platformy Hadoop w usłudze HDInsight w systemie Linux i uruchamiania przykładowych zapytań Hive.

* [Get started with Linux-based Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) (Wprowadzenie do platformy Strom działającej w oparciu o system Linux w usłudze HDInsight): samouczek szybki start dotyczący inicjowania obsługi platformy Storm w klastrze HDInsight i uruchamiania przykładowych topologii platformy Storm.

* [Provision HDInsight on Linux](hdinsight-hadoop-provision-linux-clusters.md) (Inicjowanie obsługi usługi HDInsight w systemie Linux): informacje o sposobie inicjowania obsługi klastra platformy Hadoop w usłudze HDInsight w systemie Linux za pośrednictwem portalu Azure, Azure CLI lub Azure PowerShell.

* [Working with HDInsight on Linux](hdinsight-hadoop-linux-information.md) (Praca w usłudze HDInsight w systemie Linux): zwięzłe porady dotyczące pracy z klastrami platformy Hadoop w systemie Linux udostępnionymi na platformie Azure.

* [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md) (Zarządzanie klastrami usługi HDInsight przy użyciu Ambari): informacje na temat monitorowania platformy Hadoop działającej w systemie Linux w klastrze usługi HDInsight oraz zarządzania nią przy użyciu Ambari Web lub interfejsu Ambari API REST.


### Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: więcej informacji dotyczących biblioteki oprogramowania platformy Apache Hadoop, struktury umożliwiającej przetwarzanie rozproszone dużych zestawów danych w klastrach komputerów.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">System plików HDFS</a>: więcej informacji na temat architektury i projektu rozproszonego systemu plików Hadoop, podstawowego systemu magazynu używanego przez aplikacje platformy Hadoop.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce Tutorial</a> (Samouczek MapReduce): więcej informacji na temat struktury programistycznej używanej do tworzenia aplikacji Hadoop, które szybko przetwarzają równolegle duże ilości danych w dużych klastrach węzłów obliczeniowych.

### SQL Database na platformie Azure

* [SQL Database na platformie Azure](/documentation/services/sql-database/): dokumentacja, samouczki i filmy wideo dotyczące usługi SQL Database.

* [SQL Database w portalu Azure](../sql-database/sql-database-manage-portal.md): nieskomplikowane i łatwe w użyciu narzędzie do zarządzania usługą SQL Database w chmurze.

* [Adventure Works dla SQL Database](http://msftdbprodsamples.codeplex.com/releases/view/37304): strona pobierania przykładowej bazy danych usługi SQL Database.

### Analiza biznesowa Microsoft (dla usługi HDInsight w systemie Windows)

Znane narzędzia do analizy biznesowej (BI), np. Excel, PowerPivot, SQL Server Analysis Services i SQL Server Reporting Services, pobierają, analizują i raportują dane zintegrowane z usługą HDInsight przy użyciu dodatku Power Query lub sterownika ODBC usługi Microsoft Hive.

Te narzędzia analizy biznesowej mogą ułatwić analizę danych big data:

* [Podłączenie programu Excel do platformy Hadoop za pomocą dodatku Power Query](hdinsight-connect-excel-power-query.md): informacje na temat łączenia programu Excel z kontem usługi Azure Storage, na którym są przechowywane dane skojarzone z klastrem usługi HDInsight przy użyciu programu Microsoft Power Query dla programu Excel.

* [Podłączanie programu Excel do platformy Hadoop za pomocą sterownika ODBC usługi Microsoft Hive](hdinsight-connect-excel-hive-ODBC-driver.md): informacje na temat importowania danych z usługi HDInsight za pomocą sterownika ODBC usługi Microsoft Hive.

* [Platforma Microsoft Cloud](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): informacje na temat Power BI dla pakietu Office 365, pobierania wersji próbnej programu SQL Server oraz konfiguracji programów SharePoint Server 2013 i SQL Server BI.

* <a target="_blank" href="http://msdn.microsoft.com/library/hh231701.aspx">Więcej informacji na temat usług SQL Server Analysis Services</a>.

* <a target="_blank" href="http://msdn.microsoft.com/library/ms159106.aspx">Więcej informacji na temat usług SQL Server Reporting Services</a>.


### Wersje próbne rozwiązań platformy Hadoop do analizy danych big data (dla usługi HDInsight w systemie Windows)

Użyj analizy danych big data do analizy danych organizacji i uzyskaj informacje na temat swojej firmy. Oto kilka przykładów:

* [Analizowanie danych czujnika HVAC](hdinsight-hive-analyze-sensor-data.md): informacje na temat analizowania danych czujnika przy użyciu usługi Hive z HDInsight (na platformie Hadoop), a następnie wizualizacji danych w programie Microsoft Excel. W tym przykładzie usługa Hive zostanie użyta do przetwarzania danych historycznych wytworzonych w systemach HVAC w celu identyfikacji systemów, które nie mogą w sposób niezawodny utrzymać stałej temperatury.

* [Użycie usługi Hive z usługą HDInsight do analizy dzienników witryny sieci Web](hdinsight-hive-analyze-website-log.md): informacje na temat używania usługi HiveQL w usłudze HDInsight do analizy dzienników witryny sieci Web w celu uzyskania danych dotyczących częstotliwości odwiedzin z zewnętrznych witryn sieci Web w ciągu jednego dnia oraz podsumowania błędów witryny sieci Web, z którymi mają do czynienia użytkownicy.

* [Analizowanie danych czujnika w czasie rzeczywistym za pomocą platformy Storm oraz bazy danych HBase w usłudze HDInsight (na platformie Hadoop)](hdinsight-storm-sensor-data-analysis.md): informacje na temat tworzenia rozwiązania, w którym klaster platformy Storm w usłudze HDInsight zostaje użyty do przetwarzania danych czujnika z usługi Azure Event Hubs, a następnie wyświetlania przetworzonych danych czujnika niemal w czasie rzeczywistym na pulpicie nawigacyjnym w sieci Web.


[marketing-page]: ../services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/



<!--HONumber=Jun16_HO2-->


