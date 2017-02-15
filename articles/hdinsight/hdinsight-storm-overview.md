---
title: "Wprowadzenie do systemu Apache Storm w usłudze HDInsight | Microsoft Docs"
description: "Poznaj podstawy systemu Apache Storm i dowiedz się, jak używać systemu Storm w usłudze HDInsight, aby tworzyć rozwiązania do analizowania danych w czasie rzeczywistym w chmurze."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 72d54080-1e48-4a5e-aa50-cce4ffc85077
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/11/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 0d5b68d26d708a28edee13ff3d9a57588ce83e12
ms.openlocfilehash: 426c7f5860a4be45b4c8e7ff4dd4b4406715ea34


---
# <a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>Wprowadzenie do systemu Apache Storm w usłudze HDInsight: analiza w czasie rzeczywistym dla usługi Hadoop

System Apache Storm w usłudze HDInsight służy do tworzenia rozproszonych rozwiązań analitycznych działających w czasie rzeczywistym w środowisku platformy Azure przy użyciu oprogramowania [Apache Hadoop](http://hadoop.apache.org).

## <a name="what-is-apache-storm"></a>Co to jest Apache Storm?

Apache Storm to rozproszony, odporny na uszkodzenia system obliczeniowy typu open source, który umożliwia przetwarzanie danych w czasie rzeczywistym za pomocą usługi Hadoop. Rozwiązanie Storm oferuje również gwarantowane przetwarzanie danych z możliwością powtarzania danych, które nie zostały pomyślnie przetworzone po raz pierwszy.

## <a name="why-use-storm-on-hdinsight"></a>Dlaczego warto używać systemu Storm w usłudze HDInsight?

Apache Storm w usłudze HDInsight to klaster zarządzany zintegrowany ze środowiskiem Azure. STORM i inne składniki usługi Hadoop w usłudze HDInsight opierają się na platformie danych Hortonworks (HDP), a systemem operacyjnym klastra jest Ubuntu (dystrybucja systemu Linux). Dzięki temu można korzystać z platformy w dużym stopniu zgodnej z popularnymi narzędziami i usługami w ekosystemie usługi Hadoop.

> [!IMPORTANT]
> Linux jest jedynym systemem operacyjnym używanym w połączeniu z usługą HDInsight w wersji 3.4 lub nowszą. Aby uzyskać więcej informacji, zobacz [HDInsight Deprecation on Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date) (Zakończenie obsługi usługi HDInsight w systemie Windows).

Usługa Apache Storm w usłudze HDInsight oferuje następujące kluczowe korzyści:

* Działa jako usługa zarządzana przy dostępności 99,9% czasu według umowy SLA.

* Łatwe dostosowywanie dzięki uruchamianiu skryptów w klastrze podczas tworzenia lub po jego zakończeniu. Aby uzyskać więcej informacji, zobacz [Dostosowywanie klastrów usługi HDInsight za pomocą akcji skryptu](hdinsight-hadoop-customize-cluster-linux.md).

* Umożliwia używanie preferowanego języka: zapewnia obsługę składników systemu Storm napisanych w języku **Java**, **C#** i **Python**.
  
  * Integracja programu Visual Studio z usługą HDInsight na potrzeby tworzenia i monitorowania topologii języka C# oraz zarządzania nimi. Aby uzyskać więcej informacji, zobacz [Develop C# Storm topologies with the HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) (Tworzenie topologii języka C# przy użyciu narzędzi HDInsight Tools dla programu Visual Studio).

  * Obsługuje interfejs języka Java **Trident**. Ten interfejs umożliwia tworzenie topologii Storm obsługujących „dokładnie jednokrotne” przetwarzanie komunikatów, „transakcyjną” trwałość magazynu danych i zestaw typowych operacji analizy strumienia.

* Łatwe skalowanie klastra w górę i w dół: dodawanie lub usuwanie węzłów procesu roboczego bez wpływu na działające topologie Storm.

* Integracja z innymi usługami Azure, w tym Event Hub, Azure Virtual Network, SQL Database, Blob Storage i DocumentDB.
  
  * Bezpieczne łączenie możliwości wielu klastrów HDInsight za pomocą sieci Azure Virtual Network: tworzenie potoków analitycznych, korzystających z klastrów HDInsight, HBase lub Hadoop.

Listę firm używających systemu Apache Storm w rozwiązaniach analitycznych działających w czasie rzeczywistym podano w części [Companies Using Apache Storm](https://storm.apache.org/documentation/Powered-By.html) (Firmy używające systemu Apache Storm).

Aby rozpocząć korzystanie z systemu Storm, zobacz [Rozpoczynanie pracy z systemem Storm w usłudze HDInsight][gettingstarted].

### <a name="ease-of-provisioning"></a>Łatwość aprowizacji

Nowy system Storm w usłudze HDInsight można udostępnić w ciągu kilku minut. Wystarczy podać nazwę klastra, jego wielkość, konto administratora i konto magazynu. Platforma Azure utworzy klaster z przykładowymi topologiami i przeglądarkowy pulpit nawigacyjny.

> [!NOTE]
> Klastry Storm można również udostępniać za pomocą [interfejsu wiersza polecenia platformy Azure](../xplat-cli-install.md) lub [Azure PowerShell](/powershell/azureps-cmdlets-docs).

W ciągu 15 minut od przesłania żądania nowy klaster Storm jest gotowy do pracy i przyjęcia pierwszego potoku analitycznego w czasie rzeczywistym.

### <a name="ease-of-use"></a>Łatwość obsługi

* __Łączność z protokołem Secure Shell__: dostęp do węzłów głównych klastra usługi HDInsight w Internecie przy użyciu protokołu SSH. Umożliwia to uruchamianie poleceń bezpośrednio w klastrze.

  Aby uzyskać więcej informacji, zobacz [Używanie protokołu SSH w usłudze HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* __Łączność w sieci Web__: klastry HDInsight oferują interfejs użytkownika sieci Web Ambari. Dzięki temu można łatwo monitorować i konfigurować usługi oraz zarządzać nimi w klastrze. Usługa Storm w usłudze HDInsight udostępnia także interfejs użytkownika Storm, który umożliwia monitorowanie działających topologii usługi Storm i zarządzanie nimi w przeglądarce.

  Aby uzyskać więcej informacji, zobacz [Manage HDInsight using the Ambari Web UI](hdinsight-hadoop-manage-ambari.md) (Zarządzanie usługą HDInsight przy użyciu interfejsu użytkownika sieci Web Ambari) i [Monitor and manage using the Storm UI](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-using-the-storm-ui) (Monitorowanie i zarządzanie przy użyciu interfejsu użytkownika Storm).

* __Środowisko Azure PowerShell i interfejs wiersza polecenia__: środowisko Azure PowerShell i interfejs wiersza polecenia platformy Azure udostępniają narzędzia wiersza polecenia, których można używać w systemie klienta do pracy z usługą HDInsight i innymi usługami platformy Azure.

* __Integracja z programem Visual Studio__: narzędzia Data Lake Tools for Visual Studio (wcześniej znane jako narzędzia HDInsight Tools for Visual Studio) obejmują szablony projektów umożliwiające tworzenie topologii Storm języka C# oraz obsługę narzędzi dla usługi Storm w usłudze HDInsight. Możesz tworzyć, wdrażać i monitorować topologie języka C# oraz zarządzać nimi z poziomu programu Visual Studio.

  Aby uzyskać więcej informacji, zobacz [Develop C# Storm topologies with the HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) (Tworzenie topologii języka C# przy użyciu narzędzi HDInsight Tools dla programu Visual Studio).

* __Integracja z innymi usługami platformy Azure__

  * W przypadku programowania w języku __Java__ firma Microsoft korzysta z istniejących składników usługi w celu przeprowadzenia integracji z innymi usługami Azure tam, gdzie jest to możliwe. W niektórych przypadkach może być wymagany składnika lub rozwiązanie powiązane z określoną usługą.

    * __Azure Data Lake Store__: topologie oparte na języku Java umożliwiają uzyskiwanie dostępu do usługi Data Lake Store przy użyciu elementu bolt systemu plików Storm-HDFS przy użyciu schematu identyfikatora URI `adl://`. Przykład użycia elementu bolt systemu plików Storm-HDFS można znaleźć w temacie [Use Azure Data Lake Store with Apache Storm on HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-write-data-lake-store) (Korzystanie z usługi Azure Data Lake Store razem z usługą Apache Storm w usłudze HDInsight).

    * __Azure Blob Storage__ (jeśli usługa jest używana jako magazyn dla usługi HDInsight): topologie oparte na języku Java umożliwiają uzyskiwanie dostępu do magazynu usługi Azure Blob Storage skojarzonego z klastrem przy użyciu elementu bolt systemu plików Storm-HDFS ze schematem identyfikatora URI `wasb://`.

    * __Azure Event Hubs__: dostęp można uzyskiwać za pomocą składników EventHubSpout i EventHubBolt dostarczanych przez firmę Microsoft. Składniki te są napisane w języku Java i udostępniane jako autonomiczny plik JAR.

    Aby uzyskać więcej informacji na temat projektowania rozwiązań Java, zobacz [Develop a Java-based topology for Storm on HDInsight](hdinsight-storm-develop-java-topology.md) (Opracowywanie topologii opartej na języku Java dla usługi Storm w usłudze HDInsight).

  * W przypadku programowania w języku __C#__ na potrzeby usługi platformy Azure można przeważnie użyć zestawu .NET SDK. W niektórych przypadkach zestaw SDK może polegać na platformach, które nie są dostępne w systemie Linux (system operacyjny hosta HDInsight 3.4 lub nowsze wersje). W takim przypadku można używać składników Java z rozwiązania w języku C#.

    * Przykłady pracy z usługami __SQL DB__, __DocumentDB__, __EventHub__, i __HBase__ są dołączone jako szablony do narzędzi Azure Data Lake Tools for Visual Studio. Aby uzyskać więcej informacji, zobacz [Develop a C# topology for Storm on HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md) (Opracowywanie technologii języka C# dla usługi Storm w usłudze HDInsight).

    * __Azure Event Hubs__: przykład korzystania ze składników Java z poziomu rozwiązań języka C# można znaleźć w temacie [Process events from Azure Event Hubs with Storm on HDInsight (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md) (Przetwarzanie zdarzeń usługi Azure Event Hubs za pomocą usługi Storm w usłudze HDInsight (C#)).

    Aby uzyskać więcej informacji na temat projektowania rozwiązań w języku C#, zobacz [Develop a C# topology for Storm on HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md) (Opracowywanie topologii języka C# dla usługi Storm w usłudze HDInsight).

### <a name="reliability"></a>Niezawodność

Apache Storm gwarantuje, że każdy przychodzący komunikat zostanie w pełni przetworzony, nawet wtedy, gdy analiza danych jest rozłożona na setki węzłów.

**Węzeł Nimbus** zapewnia funkcje podobne do Hadoop JobTracker i przypisuje zadania do innych węzłów w klastrze za pośrednictwem **dozorcy**. Węzły dozorcy zapewniają koordynację klastra i ułatwiają komunikację między węzłem Nimbus i procesem **nadzorczym** procesów roboczych. Jeśli jeden węzeł przetwarzania przestanie działać, zostanie zawiadomiony węzeł Nimbus, który przypisze zadanie i związane z nim dane do innego węzła.

W domyślnej konfiguracji Apache Storm występuje tylko jeden węzeł Nimbus. W systemie Storm w usłudze HDInsight działają dwa węzły Nimbus. W przypadku awarii węzła podstawowego klaster usługi HDInsight przechodzi do węzła pomocniczego, a węzeł podstawowy jest przywracany.

![Schemat węzłów Nimbus, dozorcy i nadzorcy](./media/hdinsight-storm-overview/nimbus.png)

### <a name="scale"></a>Skalowanie

Chociaż można określić liczbę węzłów w klastrze podczas tworzenia, przydatna może okazać się możliwość zwiększania i zmniejszania klastra w celu dopasowania go do obciążeń. Wszystkie klastry usługi HDInsight umożliwiają zmianę liczby węzłów w klastrze nawet w trakcie przetwarzania danych.

> [!NOTE]
> Aby móc skorzystać z nowych węzłów dodanych do skalowania, konieczne będzie ponowne zrównoważenie topologii uruchomionych przed zwiększeniem rozmiaru klastra.

### <a name="support"></a>Pomoc techniczna

System Storm w usłudze HDInsight jest dostarczany z pełną całodobową pomocą techniczną 24/7 na poziomie korporacyjnym. System Storm w usłudze HDInsight gwarantuje również dostępność na poziomie 99,9% zgodnie z umową SLA. Oznacza to, że gwarantujemy, iż klaster będzie utrzymywać łączność zewnętrzną przez nie mniej niż 99,9% czasu.

## <a name="common-use-cases-for-real-time-analytics"></a>Typowe przypadki użycia analityki w czasie rzeczywistym

Poniżej przedstawiono kilka typowych scenariuszy, w których można skorzystać z systemu Apache Storm w usłudze HDInsight. Informacje o praktycznych scenariuszach podano na stronie [How companies are using Storm](https://storm.apache.org/documentation/Powered-By.html) (Zastosowania systemu Storm w firmach).

* Internet rzeczy (IoT)
* Wykrywanie oszustw
* Analityka społecznościowa
* Ekstrakcja, transformacja, ładowanie (ETL)
* Monitorowanie sieci
* Wyszukiwanie
* Marketing na urządzeniach przenośnych

## <a name="how-is-data-in-hdinsight-storm-processed"></a>Jak przebiega przetwarzanie danych w HDInsight Storm?

Apache Storm uruchamia **topologie** zamiast zadań MapReduce, które być może już znasz z usług HDInsight lub Hadoop. Klaster Storm w usłudze HDInsight zawiera dwa typy węzłów: węzły główne (z systemem **Nimbus**) i węzły procesów roboczych (z systemem **nadzorczym**).

* **Nimbus**: podobnie do funkcji JobTracker w Hadoop jest odpowiedzialny za przesyłanie kodu w klastrze, przypisywanie zadań do maszyn wirtualnych i monitorowanie wystąpień błędów. Usługa HDInsight dostarcza dwóch węzłów Nimbus, więc nie ma żadnego pojedynczego punktu awarii systemu Storm w usłudze HDInsight
* **Nadzorca**: nadzorca każdego procesu roboczego odpowiada za uruchamianie i zatrzymywanie **procesów roboczych** w węźle.
* **Proces roboczy**: uruchamia podzbiór **topologii**. Uruchomiona topologia jest dystrybuowana do wielu procesów roboczych w całym klastrze.
* **Topologia**: definiuje graf obliczeń, który przetwarza **strumienie** danych. W przeciwieństwie do zadań MapReduce topologie działają aż do momentu wyłączenia.
* **Strumień**: niepowiązany zbiór **krotek**. Strumienie są produkowane przez elementy **spout** i **bolt**, a zużywane są przez elementy **bolt**.
* **Krotka**: nazwana lista wartości o typach określanych dynamicznie.
* **Spout**: zużywa dane ze źródła danych i emituje jeden lub kilka **strumieni**.
  
  > [!NOTE]
  > W wielu przypadkach dane są odczytywane z kolejki, takiej jak Kafka, lub kolejek usługi Azure Event Hubs. Kolejka zapewnia trwałość danych w przypadku wystąpienia awarii.

* **Bolt**: zużywa **strumienie**, przetwarza **krotki** i może emitować **strumienie**. Elementy bolt są również odpowiedzialne za zapisywanie danych w pamięci zewnętrznej, takiej jak kolejka, HDInsight, HBase, obiekt blob lub inny magazyn danych.
* **Apache Thrift**: platforma oprogramowania do tworzenia skalowalnych usług z wykorzystaniem różnych języków oprogramowania. Umożliwia tworzenie usług, które wykorzystują współdziałanie języków C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk itp.
  
  * **Nimbus** jest usługą Thrift, a **topologia** jest definicją Thrift, zatem możliwe jest opracowywanie topologii za pomocą różnych języków oprogramowania.

Aby uzyskać więcej informacji na temat składników systemu Storm, zobacz [Storm tutorial][apachetutorial] (Samouczek Storm) w serwisie apache.org.

## <a name="what-programming-languages-can-i-use"></a>Jakich języków programowania można użyć?

Klaster Storm w usłudze HDInsight obsługuje języki C#, Java i Python.

### <a name="c35"></a>C&#35;

Narzędzia Data Lake Tools for Visual Studio umożliwiają programistom .NET projektowanie i implementację topologii w języku C#. Można również tworzyć hybrydowe topologie, wykorzystujące składniki Java i C#.

Aby uzyskać więcej informacji na ten temat, zobacz [Develop C# topologies for Apache Storm on HDInsight using Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) (Tworzenie topologii C# dla Apache Storm w usłudze HDInsight przy użyciu programu Visual Studio).

### <a name="java"></a>Java

Większość przykładów w języku Java to zwykły język Java lub Trident. Trident to abstrakcja wysokiego poziomu, która ułatwia wykonywanie czynności takich jak sprzężenia, agregacje, grupowanie i filtrowanie. Jednak Trident działa na partiach krotek, natomiast rozwiązanie w pierwotnym języku Java przetwarza strumień po jednej krotce na raz.

Aby uzyskać więcej informacji na temat języka Trident, zobacz [Trident tutorial](https://storm.apache.org/documentation/Trident-tutorial.html) (Samouczek Trident) w witrynie apache.org.

Aby poznać przykłady topologii Java i Trident, zobacz [listę przykładowych topologii Storm](hdinsight-storm-example-topology.md) lub przykłady z projektu storm-starter w klastrze usługi HDInsight.

Przykłady elementu storm-starter można znaleźć w katalogu ** /usr/hdp/current/storm-client/contrib/storm-starter** w klastrze usługi HDInsight.

## <a name="what-are-some-common-development-patterns"></a>Jakie są typowe wzorce programowania?

### <a name="guaranteed-message-processing"></a>Gwarantowane przetwarzanie komunikatów

Storm zapewnia różne poziomy gwarantowanego przetwarzania komunikatów. Na przykład podstawowa aplikacja Storm może zagwarantować przetwarzanie co najmniej jednokrotne, a Trident może zagwarantować przetwarzanie dokładnie jednokrotne.

Aby uzyskać więcej informacji, zobacz [Guarantees on data processing](https://storm.apache.org/about/guarantees-data-processing.html) (Gwarancje przetwarzania danych) w serwisie apache.org.

### <a name="ibasicbolt"></a>IBasicBolt

Wzorzec obejmujący odczytywanie krotki wejściowej, emitującej zero lub więcej krotek, a następnie użycie metody ack na krotce wejściowej natychmiast po zakończeniu przetwarzania jest dość typowy. System Storm udostępnia interfejs [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html) w celu automatyzacji tego wzorca.

### <a name="joins"></a>Sprzężenia

Sprzęganie dwóch strumieni danych przebiega różnie w różnych aplikacjach. Na przykład można łączyć poszczególne krotki z wielu strumieni w jeden nowy strumień lub łączyć tylko partie krotek w określonym oknie. Łączenie w obu przypadkach można wykonać za pomocą metody [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), która określa sposób kierowania krotek do elementów bolt.

W poniższym przykładzie w języku Java metoda fieldsGrouping służy do kierowana krotek pochodzących ze składników „1”, „2” i „3” do elementu bolt o nazwie **MyJoiner**.

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batching"></a>Tworzenie partii

Partie można tworzyć na kilka sposobów. W przypadku topologii języka C# lub Java można użyć prostego licznika, aby utworzyć partię złożoną z X krotek przed ich wyemitowaniem, lub użyć wewnętrznego mechanizmu zegarowego nazywanego „krotką znacznikową” w celu emitowania partii co X sekund.

Aby zapoznać się z przykładem korzystania z krotki znacznikowej z poziomu składnika języka C#, zobacz [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

Jeśli używasz języka Trident, opiera się ono na przetwarzaniu partii krotek.

### <a name="caching"></a>Buforowanie

Buforowanie w pamięci jest często używane jako mechanizm przyspieszania przetwarzania, ponieważ utrzymuje często używane zasoby w pamięci. Ponieważ topologia jest rozmieszczona na wielu węzłach i w wielu procesach w każdym węźle, należy rozważyć użycie metody [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), aby mieć pewność, że krotki zawierające pola używane do wyszukiwania w pamięci podręcznej zawsze są kierowane do tego samego procesu. Pozwala to uniknąć duplikowania wpisów pamięci podręcznej między procesami.

### <a name="streaming-top-n"></a>Przesyłanie strumieniowe pierwszych N

Gdy topologia zależy od obliczenia wartości „pierwszych N”, na przykład pierwszych 5 trendów w serwisie Twitter, należy obliczyć wartość pierwszych N równolegle, a następnie scalić dane wyjściowe z tych obliczeń do wartości globalnej. Można to zrobić za pomocą metody [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), aby kierować według pola do równoległych elementów bolt (co dzieli dane według wartości pola), a następnie kierować do elementu bolt, który globalnie określa wartość pierwszych N.

Przykład [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java) przedstawia praktyczne wykonanie.

## <a name="what-type-of-logging-does-storm-use"></a>Jakiego rodzaju rejestrowania używa system Storm?

System Storm używa mechanizmu Apache Log4j do rejestrowania informacji. Domyślnie rejestrowana jest duża ilość danych i sortowanie informacji może być trudne. W topologii systemu Storm można uwzględnić plik konfiguracji rejestrowania, aby sterować zachowaniem rejestrowania.

Przykładową topologię pokazującą metodę konfigurowania logowania można znaleźć w przykładzie [aplikacji WordCount opartej na języku Java](hdinsight-storm-develop-java-topology.md) dla systemu Storm w usłudze HDInsight.

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat rozwiązań analitycznych w czasie rzeczywistym z wykorzystaniem systemu Apache Storm w usłudze HDInsight:

* [Wprowadzenie do korzystania z systemu Storm w usłudze HDInsight][gettingstarted]
* [Przykładowe topologie dla systemu Storm w usłudze HDInsight](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md



<!--HONumber=Feb17_HO2-->


