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
ms.date: 10/11/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: bccec1e4078c38e1cc9205a36d3a5df579df35b6
ms.openlocfilehash: d5ff397e947a7edc8310da59ff9fe8896829e35d


---
# <a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>Wprowadzenie do systemu Apache Storm w usłudze HDInsight: analiza w czasie rzeczywistym dla usługi Hadoop
System Apache Storm w usłudze HDInsight służy do tworzenia rozproszonych rozwiązań analitycznych działających w czasie rzeczywistym w środowisku platformy Azure przy użyciu oprogramowania [Apache Hadoop](http://hadoop.apache.org).

## <a name="what-is-apache-storm"></a>Co to jest Apache Storm?
Apache Storm to rozproszony, odporny na uszkodzenia system obliczeniowy typu open source, który umożliwia przetwarzanie danych w czasie rzeczywistym za pomocą usługi Hadoop. Rozwiązanie Storm oferuje również gwarantowane przetwarzanie danych z możliwością powtarzania danych, które nie zostały pomyślnie przetworzone po raz pierwszy.

## <a name="why-use-storm-on-hdinsight"></a>Dlaczego warto używać systemu Storm w usłudze HDInsight?
Apache Storm w usłudze HDInsight to klaster zarządzany zintegrowany ze środowiskiem Azure. Ma on następujące główne zalety:

* Działa jako usługa zarządzana przy dostępności 99,9% czasu według SLA
* Umożliwia używanie preferowanego języka: zapewnia obsługę składników systemu Storm napisanych w języku **Java**, **C#** i **Python**
  
  * Obsługuje łączenie języków programowania: odczytywanie danych przy użyciu języka Java, a następnie przetwarzanie za pomocą C#
    
    > [!NOTE]
    > Aby użyć topologii C# z klastrem opartym na systemie Linux, należy zaktualizować pakiet NuGet Microsoft.SCP.Net.SDK używany w projekcie do wersji 0.10.0.6 lub nowszej. Wersja pakietu musi być również zgodna z wersją główną systemu Storm zainstalowanego w usłudze HDInsight. Na przykład system Storm występujący w usłudze HDInsight w wersjach 3.3 i 3.4 jest w wersji 0.10.x, a usługa HDInsight 3.5 używa systemu Storm 1.0.x.
    > 
    > Topologie C# w klastrach opartych na systemie Linux muszą korzystać z platformy .NET 4.5 i używać platformy Mono, aby mogły działać w klastrze usługi HDInsight. Większości funkcji będzie działać, jednak należy zapoznać się z dokumentem dotyczącym [zgodności platformy Mono](http://www.mono-project.com/docs/about-mono/compatibility/), aby przewidzieć potencjalne niezgodności.
    > 
  * Używanie interfejsu Java **Trident** do tworzenia topologii Storm obsługujących „dokładnie jednokrotne” przetwarzanie komunikatów, „transakcyjną” trwałość magazynu danych i zestaw typowych operacji analizy strumienia
* Zawiera wbudowane funkcje skalowania w górę i w dół: umożliwia skalowanie klastra usługi HDInsight bez wpływu na uruchomione topologie Storm
* Integracja z innymi usługami Azure, w tym Centrum zdarzeń, Azure Virtual Network, SQL Database, Blob Storage i DocumentDB
  
  * Łączenie możliwości wielu klastrów HDInsight za pomocą sieci Azure Virtual Network: tworzenie potoków analitycznych, korzystających z klastrów HDInsight, HBase lub Hadoop

Listę firm używających systemu Apache Storm w rozwiązaniach analitycznych działających w czasie rzeczywistym podano w części [Companies Using Apache Storm](https://storm.apache.org/documentation/Powered-By.html) (Firmy używające systemu Apache Storm).

Aby rozpocząć korzystanie z systemu Storm, zobacz [Rozpoczynanie pracy z systemem Storm w usłudze HDInsight][gettingstarted].

### <a name="ease-of-provisioning"></a>Łatwość aprowizacji
Nowy system Storm w usłudze HDInsight można udostępnić w ciągu kilku minut. Wystarczy podać nazwę klastra, jego wielkość, konto administratora i konto magazynu. Platforma Azure utworzy klaster z przykładowymi topologiami i przeglądarkowy pulpit nawigacyjny.

> [!NOTE]
> Klastry Storm można również udostępniać za pomocą [interfejsu wiersza polecenia platformy Azure](../xplat-cli-install.md) lub [Azure PowerShell](/powershell/azureps-cmdlets-docs).
> 
> 

W ciągu 15 minut od przesłania żądania nowy klaster Storm jest gotowy do pracy i przyjęcia pierwszego potoku analitycznego w czasie rzeczywistym.

### <a name="ease-of-use"></a>Łatwość obsługi
**W przypadku klastrów Storm w usłudze HDInsight opartych na systemie Linux** z klastrem można się połączyć przy użyciu protokołu SSH, a za pomocą polecenia `storm` można uruchamiać topologie i zarządzać nimi. Ponadto można używać narzędzia Ambari do monitorowania usługi Storm, a interfejsu użytkownika Storm do monitorowania uruchomionych topologii i zarządzania nimi.

Aby uzyskać więcej informacji na temat pracy z klastrami Storm opartymi na systemie Linux, zobacz [Get started with Apache Storm on Linux-based HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) (Wprowadzenie do Apache Storm w usłudze HDInsight opartej na systemie Linux).

**W przypadku klastrów Storm w usłudze HDInsight opartych na systemie Windows** narzędzia HDInsight Tools for Visual Studio umożliwiają tworzenie topologii C# i hybrydowych topologii C#/Java i przesyłanie ich do klastra Storm w usłudze HDInsight.  

![Tworzenie projektu Storm](./media/hdinsight-storm-overview/createproject.png)

Narzędzia HDInsight Tools for Visual Studio również udostępniają interfejs, który umożliwia monitorowanie i zarządzanie topologiami Storm w klastrze.

![Zarządzanie projektami Storm](./media/hdinsight-storm-overview/stormview.png)

Przykład użycia narzędzi HDInsight do tworzenia aplikacji Storm przedstawiono w części [Develop C# Storm topologies with the HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) (Tworzenie topologii Storm C# za pomocą narzędzi HDInsight Tools for Visual Studio).

Aby dowiedzieć się więcej o narzędziach HDInsight Tools for Visual Studio, zobacz [Get started using the HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) (Rozpoczęcie pracy z narzędziami HDInsight Tools for Visual Studio).

Każdy klaster Storm w usłudze HDInsight udostępnia również przeglądarkowy pulpit nawigacyjny Storm Dashboard, który służy do przesyłania i monitorowania topologii Storm działających w klastrze oraz zarządzania nimi.

![Pulpit nawigacyjny Storm](./media/hdinsight-storm-overview/dashboard.png)

Więcej informacji dotyczących korzystania z pulpitu nawigacyjnego Storm Dashboard można znaleźć w temacie [Deploy and manage Apache Storm topologies on HDInsight](hdinsight-storm-deploy-monitor-topology.md) (Wdrażanie topologii Apache Storm i zarządzanie nimi w usłudze HDInsight).

System Storm w usłudze HDInsight zapewnia również łatwą integrację z usługą Azure Event Hubs za pośrednictwem **elementów Spout Centrum zdarzeń**. Najnowsza wersja tego składnika jest dostępna na stronie [https://github.com/hdinsight/hdinsight-storm-examples/tree/master/lib/eventhubs](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/lib/eventhubs). Więcej informacji o korzystaniu z tego składnika można znaleźć w następujących dokumentach.

* [Develop a C# topology that uses Azure Event Hubs (Tworzenie topologii C# wykorzystującej usługę Azure Event Hubs)](hdinsight-storm-develop-csharp-event-hub-topology.md)
* [Develop a Java topology that uses Azure Event Hubs (Tworzenie topologii Java wykorzystującej usługę Azure Event Hubs)](hdinsight-storm-develop-java-event-hub-topology.md)

### <a name="reliability"></a>Niezawodność
Apache Storm gwarantuje, że każdy przychodzący komunikat zostanie w pełni przetworzony, nawet wtedy, gdy analiza danych jest rozłożona na setki węzłów.

**Węzeł Nimbus** zapewnia funkcje podobne do Hadoop JobTracker i przypisuje zadania do innych węzłów w klastrze za pośrednictwem **dozorcy**. Węzły dozorcy zapewniają koordynację klastra i ułatwiają komunikację między węzłem Nimbus i procesem **nadzorczym** procesów roboczych. Jeśli jeden węzeł przetwarzania przestanie działać, zostanie zawiadomiony węzeł Nimbus, który przypisze zadanie i związane z nim dane do innego węzła.

W domyślnej konfiguracji Apache Storm występuje tylko jeden węzeł Nimbus. W systemie Storm w usłudze HDInsight działają dwa węzły Nimbus. W przypadku awarii węzła podstawowego klaster usługi HDInsight przechodzi do węzła pomocniczego, a węzeł podstawowy jest przywracany.

![Schemat węzłów Nimbus, dozorcy i nadzorcy](./media/hdinsight-storm-overview/nimbus.png)

### <a name="scale"></a>Skalowanie
Chociaż można określić liczbę węzłów w klastrze podczas tworzenia, przydatna może okazać się możliwość zwiększania i zmniejszania klastra w celu dopasowania go do obciążeń. Wszystkie klastry usługi HDInsight umożliwiają zmianę liczby węzłów w klastrze nawet w trakcie przetwarzania danych.

> [!NOTE]
> Aby móc skorzystać z nowych węzłów dodanych do skalowania, konieczne będzie ponowne zrównoważenie topologii uruchomionych przed zwiększeniem rozmiaru klastra.
> 
> 

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
  > W wielu przypadkach dane są odczytywane z kolejki, takiej jak Kafka, kolejek usługi Azure Service Bus lub Event Hubs. Kolejka zapewnia trwałość danych w przypadku wystąpienia awarii.
  > 
  > 
* **Bolt**: zużywa **strumienie**, przetwarza **krotki** i może emitować **strumienie**. Elementy bolt są również odpowiedzialne za zapisywanie danych w pamięci zewnętrznej, takiej jak kolejka, HDInsight, HBase, obiekt blob lub inny magazyn danych.
* **Apache Thrift**: platforma oprogramowania do tworzenia skalowalnych usług z wykorzystaniem różnych języków oprogramowania. Umożliwia tworzenie usług, które wykorzystują współdziałanie języków C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk itp.
  
  * **Nimbus** jest usługą Thrift, a **topologia** jest definicją Thrift, zatem możliwe jest opracowywanie topologii za pomocą różnych języków oprogramowania.

Aby uzyskać więcej informacji na temat składników systemu Storm, zobacz [Storm tutorial][apachetutorial] (Samouczek Storm) w serwisie apache.org.

## <a name="what-programming-languages-can-i-use"></a>Jakich języków programowania można użyć?
Klaster Storm w usłudze HDInsight obsługuje języki C#, Java i Python.

### <a name="c35"></a>C&#35;
Narzędzia HDInsight Tools for Visual Studio umożliwiają programistom .NET projektowanie i implementację topologii w języku C#. Można również tworzyć hybrydowe topologie, wykorzystujące składniki Java i C#.

Aby uzyskać więcej informacji na ten temat, zobacz [Develop C# topologies for Apache Storm on HDInsight using Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) (Tworzenie topologii C# dla Apache Storm w usłudze HDInsight przy użyciu programu Visual Studio).

### <a name="java"></a>Java
Większość przykładów w języku Java to zwykły język Java lub Trident. Trident to abstrakcja wysokiego poziomu, która ułatwia wykonywanie czynności takich jak sprzężenia, agregacje, grupowanie i filtrowanie. Jednak Trident działa na partiach krotek, natomiast rozwiązanie w pierwotnym języku Java przetwarza strumień po jednej krotce na raz.

Aby uzyskać więcej informacji na temat języka Trident, zobacz [Trident tutorial](https://storm.apache.org/documentation/Trident-tutorial.html) (Samouczek Trident) w witrynie apache.org.

Aby poznać przykłady topologii Java i Trident, zobacz [listę przykładowych topologii Storm](hdinsight-storm-example-topology.md) lub przykłady z projektu storm-starter w klastrze usługi HDInsight.

Przykłady z projektu storm-starter znajdują się w katalogu ** /usr/hdp/current/storm-client/contrib/storm-starter** w klastrach z systemem Linux i w katalogu **%storm_home%\contrib\storm-starter** w klastrach z systemem Windows.

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
Partie można tworzyć na kilka sposobów. W przypadku topologii Storm w języku Java można użyć prostego licznika, aby utworzyć partię złożoną z X krotek przed ich wyemitowaniem, lub użyć wewnętrznego mechanizmu zegarowego nazywanego „krotką znacznikową” w celu emitowania partii co X sekund.

Przykład zastosowania krotek znacznikowych przedstawiono w temacie [Analyzing sensor data with Storm and HBase on HDInsight](hdinsight-storm-sensor-data-analysis.md) (Analizowanie danych czujników za pomocą Storm i bazy danych HBase w usłudze HDInsight).

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



<!--HONumber=Jan17_HO1-->


