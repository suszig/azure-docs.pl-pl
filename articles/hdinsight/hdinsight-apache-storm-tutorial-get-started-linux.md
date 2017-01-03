---
title: "Samouczek platformy Apache Storm: wprowadzenie do platformy Storm opartej na systemie Linux w usłudze HDInsight | Microsoft Docs"
description: "Wprowadzenie do analizy danych big data na platformie Apache Storm i przykłady z projektu Storm Starter w usłudze HDInsight opartej na systemie Linux. Dowiedz się, jak używać systemu Storm do przetwarzania danych w czasie rzeczywistym."
keywords: apache storm,apache storm tutorial,big data analytics,storm starter
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: d710dcac-35d1-4c27-a8d6-acaf8146b485
ms.service: hdinsight
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/18/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: ec3384dd68a28117cc00de20e4c08ad0ccd67bad
ms.openlocfilehash: a7516a98cbe25f9ed458131083a18899d3d50004


---
# <a name="apache-storm-tutorial-get-started-with-the-storm-starter-samples-for-big-data-analytics-on-hdinsight"></a>Samouczek systemu Apache Storm: wprowadzenie do przykładów z projektu Storm Starter dotyczących analizy danych big data w usłudze HDInsight

Apache Storm to skalowalny, odporny na błędy, rozproszony system obliczeniowy działający w czasie rzeczywistym do przetwarzania strumieni danych. Dzięki platformie Storm w usłudze Azure HDInsight można utworzyć oparty na chmurze klaster Storm do wykonywania analizy danych big data w czasie rzeczywistym.

> [!NOTE]
> Kroki opisane w tym artykule umożliwiają utworzenie klastra usługi HDInsight na bazie systemu Linux. Aby uzyskać instrukcje tworzenia klastra Storm opartego na systemie Windows w usłudze HDInsight, zobacz temat [Samouczek platformy Apache Storm: wprowadzenie do przykładu z projektu Storm Starter z użyciem analizy danych w usłudze HDInsight](hdinsight-apache-storm-tutorial-get-started.md)

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Do pomyślnego ukończenia samouczka platformy Apache Storm potrzebne są:

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Znajomość protokołów SSH i SCP**. Aby uzyskać więcej informacji o korzystaniu protokołów SSH i SCP z usługą HDInsight, zobacz następujące artykuły:
  
    * **Klient systemu Linux, Unix lub OS X**: Zobacz [Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Klient systemu Windows**: zobacz temat [Używanie protokołu SSH (PuTTY) z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu

[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-a-storm-cluster"></a>Tworzenie klastra Storm

W tej sekcji utworzysz klaster usługi HDInsight w wersji 3.5 (Storm 1.0.1) przy użyciu szablonu usługi Azure Resource Manager. Informacje o wersji usługi HDInsight i umowach SLA można znaleźć w temacie [Przechowywanie wersji składnika usługi HDInsight](hdinsight-component-versioning.md). Aby zapoznać się z innymi metodami tworzenia klastra, zobacz temat [Tworzenie klastrów usługi HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknij poniższy obraz, aby otworzyć szablon w usłudze Azure Portal.         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-storm-cluster-in-hdinsight-35.json" target="_blank"><img src="./media/hdinsight-apache-storm-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    Szablon znajduje się w publicznym kontenerze obiektów blob *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-storm-cluster-in-hdinsight.json*. 

2. W bloku __Wdrożenie niestandardowe__ wprowadź wartości następujących opcji:
   
    * __Grupa zasobów__: grupa zasobów, w której jest tworzony klaster.

    * **Nazwa klastra**: nazwa klastra Hadoop.

    * __Nazwa logowania klastra__ i __hasło__: domyślna nazwa logowania to admin.
    
    * __Nazwa użytkownika SSH__ i __hasło__: nazwa użytkownika i hasło służące do połączenia z klastrem przy użyciu protokołu SSH.

    * __Lokalizacja__: lokalizacja geograficzna klastra.
     
     Zapisz te wartości.  Będą potrzebne później podczas korzystania z samouczka.
     
     > [!NOTE]
     > Protokół SSH umożliwia zdalny dostęp do klastra usługi HDInsight przy użyciu wiersza polecenia. Nazwa użytkownika i hasło używane w tym miejscu służą do nawiązywania połączenia z klastrem za pośrednictwem protokołu SSH. Ponadto nazwa użytkownika SSH musi być unikatowa, ponieważ służy do tworzenia konta użytkownika na wszystkich węzłach klastra usługi HDInsight. Następujące nazwy kont są zarezerwowane do użytku przez usługi w klastrze i nie można ich użyć jako nazwy użytkownika SSH:
     > 
     > root, hdiuser, storm, hbase, ubuntu, zookeeper, hdfs, yarn, mapred, hbase, hive, oozie, falcon, sqoop, admin, tez, hcat, hdinsight-zookeeper.
     > 
     > Aby uzyskać więcej informacji o korzystaniu z protokołu SSH z usługą HDInsight, zobacz jeden z następujących artykułów:
     > 
     > * [Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)
     > * [Używanie protokołu SSH (PuTTY) z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Wybierz pozycje __Wyrażam zgodę na powyższe warunki i postanowienia__**OK**, a następnie wybierz pozycję __Przypnij do pulpitu nawigacyjnego__.

6. Kliknij pozycję **Kup**. Zostanie wyświetlony nowy kafelek zatytułowany Submitting deployment for Template deployment (Przesyłanie wdrożenia dla wdrożenia szablonu). Utworzenie klastra trwa około 20 minut.

## <a name="run-a-storm-starter-sample-on-hdinsight"></a>Uruchamianie przykładu z projektu Storm Starter w usłudze HDInsight

Przykłady z projektu [storm-starter](https://github.com/apache/storm/tree/master/examples/storm-starter) są dołączone do klastra usługi HDInsight. Wykonanie poniższych kroków spowoduje uruchomienie przykładu WordCount.

1. Nawiąż połączenie z klastrem usługi HDInsight przy użyciu protokołu SSH:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Jeśli do zabezpieczania konta użytkownika SSH użyto hasła, zostanie wyświetlony monit o jego wprowadzenie. Jeśli używasz klucza publicznego, może być konieczne użycie parametru `-i` w celu określenia zgodnego klucza prywatnego. Na przykład `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
   
    Aby uzyskać więcej informacji o korzystaniu z protokołu SSH z usługą HDInsight opartą na systemie Linux, zobacz jeden z następujących artykułów:
   
    * [Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Używanie protokołu SSH (PuTTY) z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Użyj następującego polecenia, aby uruchomić przykładową topologię:
   
        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar storm jar org.apache.storm.starter.WordCountTopology wordcount
   
    > [!NOTE]
    > We wcześniejszych wersjach usługi HDInsight nazwa klasy topologii to `storm.starter.WordCountTopology` zamiast `org.apache.storm.starter.WordCountTopology`.
   
    To polecenie uruchamia w klastrze przykładową topologię WordCount o przyjaznej nazwie „wordcount”. Przykład obejmuje losowe wygenerowanie zdań i zliczenie wystąpień poszczególnych wyrazów w zdaniach.
   
    > [!NOTE]
    > Podczas przesyłania własnych topologii do klastra przed użyciem polecenia `storm` należy skopiować plik JAR zawierający klaster. Można to zrobić za pomocą polecenia `scp` z poziomu klienta, na którym znajduje się plik. Na przykład: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    > 
    > Przykład WordCount i inne przykłady z projektu Storm Starter znajdują się już w klastrze w lokalizacji `/usr/hdp/current/storm-client/contrib/storm-starter/`.

Jeśli chcesz zobaczyć źródło przykładów z projektu Storm Starter, kod możesz znaleźć na stronie [https://github.com/apache/storm/tree/1.0.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.0.x-branch/examples/storm-starter). Ten link dotyczy platformy Storm 1.0.x, która jest dostarczana z usługą HDInsight 3.5. W przypadku innych wersji platformy Storm należy za pomocą przycisku __Branch__ (Gałąź) w górnej części strony wybrać inną wersję platformy Storm.

## <a name="monitor-the-topology"></a>Monitorowanie topologii

Interfejs użytkownika platformy Storm udostępnia interfejs sieci Web do pracy z uruchomionymi topologiami i jest zawarty w klastrze usługi HDInsight.

Wykonaj następujące kroki, aby monitorować topologię za pomocą interfejsu użytkownika platformy Storm:

1. Otwórz przeglądarkę sieci Web z adresem https://CLUSTERNAME.azurehdinsight.net/stormui, gdzie **CLUSTERNAME** jest nazwą klastra. Spowoduje to otwarcie interfejsu użytkownika platformy Storm.
    
    > [!NOTE]
    > Jeśli zostanie wyświetlony monit o podanie nazwy użytkownika i hasła, wprowadź nazwę administratora klastra (admin) i hasło użyte podczas tworzenia klastra.

2. W obszarze **Topology summary** (Podsumowanie topologii) wybierz pozycję **wordcount** w kolumnie **Name** (Nazwa). Spowoduje to wyświetlenie dodatkowych informacji na temat topologii.
    
    ![Pulpit nawigacyjny platformy Storm z informacjami o topologii przykładu z projektu Storm Starter o nazwie WordCount.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)
    
    Ta strona przedstawia następujące informacje:
    
    * **Topology stats** (Statystyka topologii) — podstawowe informacje na temat wydajności topologii podzielone na okna czasowe.
     
        > [!NOTE]
        > Wybór określonego okna czasowego zmienia przedział czasu dla informacji wyświetlanych w innych sekcjach strony.

    * **Spouts** (Elementy spout) — podstawowe informacje o elementach spout, łącznie z ostatnim błędem zwróconym przez poszczególne elementy spout.
    
    * **Bolts** (Elementy bolt) — podstawowe informacje o elementach bolt.
    
    * **Topology configuration** (Konfiguracja topologii) — szczegółowe informacje o konfiguracji topologii.
     
    Ta strona zawiera również akcje, które można wykonać w odniesieniu do topologii:
   
    * **Activate** (Aktywuj) — wznowienie przetwarzania dezaktywowanej topologii.
    
    * **Deactivate** (Dezaktywuj) — wstrzymanie uruchomionej topologii.
    
    * **Rebalance** (Ponowne równoważenie) — dopasowanie równoległości topologii. Po zmianie liczby węzłów w klastrze należy przeprowadzić ponowne równoważenie uruchomionych topologii. Pozwala to dostosować równoległość topologii w celu kompensacji zwiększonej/zmniejszonej liczby węzłów w klastrze. Aby uzyskać więcej informacji, zobacz temat [Pojęcie równoległości w topologii Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).
    
    * **Kill** (Kasuj) — zakończenie działania topologii Storm po określonym czasie.

3. Na tej stronie wybierz pozycję z sekcji **Spouts** (Elementy spout) lub **Bolts** (Elementy bolt). Spowoduje to wyświetlenie informacji na temat wybranego składnika.
   
    ![Pulpit nawigacyjny Storm z informacjami o wybranych składnikach.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)
   
    Ta strona zawiera następujące informacje:
   
    * **Spout/Bolt stats** (Statystyki elementów spout/bolt) — podstawowe informacje na temat wydajności składników podzielone na okna czasowe.
     
        > [!NOTE]
        > Wybór określonego okna czasowego zmienia przedział czasu dla informacji wyświetlanych w innych sekcjach strony.
     
    * **Input stats** (Statystyki danych wejściowych) tylko dla elementów bolt — informacje na temat składników, które tworzą dane używane przez elementy bolt.
    
    * **Output stats** (Statystyki danych wyjściowych) — informacje na temat danych emitowanych przez dany element bolt.
    
    * **Executors** (Wykonawcy) — informacje dotyczące wystąpień danego składnika.
    
    * **Errors** (Błędy) — błędy generowane przez dany składnik.

4. Wyświetlając szczegółowe informacje o elemencie spout lub bolt, wybierz pozycję w kolumnie **Port** w sekcji **Executors** (Wykonawcy), aby wyświetlić szczegóły dotyczące określonego wystąpienia składnika.
   
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]
   
    Na podstawie tych danych można stwierdzić, że wyraz **seven** (siedem) wystąpił 1493957 razy. Tyle razy został napotkany od momentu uruchomienia tej topologii.

## <a name="stop-the-topology"></a>Zatrzymywanie topologii

Wróć do strony **Topology summary** (Podsumowanie topologii) dla topologii WordCount, a następnie naciśnij przycisk **Kill** (Kasuj) w obszarze **Topology actions** (Akcje topologii). Po wyświetleniu monitu wprowadź 10 jako liczbę sekund oczekiwania przed zatrzymaniem topologii. Po upłynięciu limitu czasu topologia nie będzie już wyświetlana w obszarze **interfejsu użytkownika platformy Storm** pulpitu nawigacyjnego.

## <a name="delete-the-cluster"></a>Usuwanie klastra

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="a-idnextanext-steps"></a><a id="next"></a>Następne kroki

W tym samouczku platformy Apache Storm użyto przykładu z projektu Storm Starter, aby przedstawić sposób tworzenia klastra platformy Storm w usłudze HDInsight oraz używania pulpitu nawigacyjnego Storm do wdrażania i monitorowania topologii Storm oraz zarządzania nimi. W dalszej kolejności zapoznaj się ze sposobem [opracowywania topologii opartych na języku Java za pomocą programu Maven](hdinsight-storm-develop-java-topology.md).

Jeśli umiesz już opracowywać topologie oparte na języku Java i chcesz wdrożyć istniejącą topologię w usłudze HDInsight, zobacz temat [Wdrażanie topologii Apache Storm w usłudze HDInsight i zarządzanie nimi](hdinsight-storm-deploy-monitor-topology-linux.md).

Jeśli jesteś deweloperem platformy .NET, możesz tworzyć topologie C# lub hybrydowe topologie C#/Java za pomocą programu Visual Studio. Aby uzyskać więcej informacji na ten temat, zobacz [Develop C# topologies for Apache Storm on HDInsight using Hadoop tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) (Tworzenie topologii C# dla Apache Storm w usłudze HDInsight przy użyciu narzędzi Hadoop dla programu Visual Studio).

Aby uzyskać przykładowe topologie, które mogą być używane z narzędziem Storm w usłudze HDInsight, zobacz następujące przykłady:

* [Przykładowe topologie dla systemu Storm w usłudze HDInsight](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[azureportal]: https://manage.windowsazure.com/
[hdinsight-provision]: hdinsight-provision-clusters.md
[preview-portal]: https://portal.azure.com/



<!--HONumber=Dec16_HO5-->


