---
title: "Rozpoczynanie pracy z platformą Apache Storm w usłudze Azure HDInsight | Microsoft Docs"
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
ms.date: 03/17/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 5d74f2c130eeddb1022acf9673c6a2006af2db58
ms.lasthandoff: 03/25/2017

---
#<a name="get-started-with-the-storm-starter-samples-for-big-data-analytics-on-linux-based-hdinsight"></a>Wprowadzenie do przykładów z projektu Storm Starter dotyczących analizy danych big data w usłudze HDInsight opartej na systemie Linux

Apache Storm to skalowalny, odporny na błędy, rozproszony system obliczeniowy działający w czasie rzeczywistym do przetwarzania strumieni danych. Dzięki platformie Storm w usłudze Azure HDInsight można utworzyć oparty na chmurze klaster Storm do wykonywania analizy danych big data w czasie rzeczywistym.

> [!IMPORTANT]
> Linux jest jedynym systemem operacyjnym używanym w połączeniu z usługą HDInsight w wersji 3.4 lub nowszą. Aby uzyskać więcej informacji, zobacz [HDInsight Deprecation on Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date) (Zakończenie obsługi usługi HDInsight w systemie Windows).

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Znajomość protokołów SSH i SCP**. Aby uzyskać informacje, zobacz [Używanie protokołu SSH w usłudze HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu

[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-a-storm-cluster"></a>Tworzenie klastra Storm

Aby utworzyć klaster platformy Storm w usłudze HDInsight, wykonaj następujące czynności:

1. W witrynie [Azure Portal](https://portal.azure.com) wybierz pozycję **+ NOWY** i pozycję **Zbieranie danych i analiza**, a następnie pozycję **HDInsight**.
   
    ![Tworzenie klastra usługi HDInsight](./media/hdinsight-apache-storm-tutorial-get-started-linux/create-hdinsight.png)

2. W bloku **Podstawowe** wprowadź następujące informacje:

    * **Nazwa klastra**: nazwa klastra usługi HDInsight.
    * **Subskrypcja**: wybierz subskrypcję, której chcesz użyć.
    * **Nazwa użytkownika logowania klastra** i **Hasło logowania klastra**: dane logowania podczas uzyskiwania dostępu do klastra przy użyciu protokołu HTTPS. Te poświadczenia umożliwiają dostęp do takich usług jak interfejs użytkownika sieci Web Ambari lub interfejs API REST.
    * **Nazwa użytkownika SSH (Secure Shell)**: nazwa logowania używana podczas uzyskiwania dostępu do klastra za pośrednictwem protokołu SSH. Domyślnie hasło jest takie samo jak hasło logowania klastra.
    * **Grupa zasobów**: grupa zasobów, w której ma zostać utworzony klaster.
    * **Lokalizacja**: region platformy Azure, w którym ma zostać utworzony klaster.
   
    ![Wybieranie subskrypcji](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-basic-configuration.png)

3. Wybierz pozycję **Typ klastra**, a następnie ustaw następujące wartości w bloku **Konfiguracja klastra**:
   
    * **Typ klastra**: Storm

    * **System operacyjny**: Linux

    * **Wersja**: Storm 1.0.1 (HDI 3.5)

    * **Warstwa klastra**: Standardowa
     
    Zapisz ustawienia przy użyciu przycisku **Wybierz**.
     
    ![Wybieranie typu klastra](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-cluster-type.png)

4. Po wybraniu typu klastra ustaw typ klastra przy użyciu przycisku __Wybierz__. Następnie zakończ konfigurację podstawową za pomocą przycisku __Dalej__.

5. W bloku **Magazyn** wybierz lub utwórz konto magazynu. Na potrzeby procedury w tym dokumencie pozostaw wartości domyślne w pozostałych polach w tym bloku. Zapisz konfigurację magazynu za pomocą przycisku __Dalej__.

    ![Konfigurowanie ustawień konta magazynu dla usługi HDInsight](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-storage-account.png)

6. W bloku **Podsumowanie** przejrzyj konfigurację klastra. Zmień niepoprawne ustawienia przy użyciu linków __Edytuj__. Utwórz klaster przy użyciu przycisku __Utwórz__.
   
    ![Podsumowanie konfiguracji klastra](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > Tworzenie klastra może potrwać do 20 minut.

## <a name="run-a-storm-starter-sample-on-hdinsight"></a>Uruchamianie przykładu z projektu Storm Starter w usłudze HDInsight

1. Nawiąż połączenie z klastrem usługi HDInsight przy użyciu protokołu SSH:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Jeśli do zabezpieczenia konta użytkownika SSH użyto hasła, zostanie wyświetlony monit o jego wprowadzenie. Jeśli używasz klucza publicznego, może być konieczne użycie parametru `-i` w celu wskazania zgodnego klucza prywatnego. Na przykład `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
   
    Aby uzyskać informacje, zobacz [Używanie protokołu SSH w usłudze HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Użyj następującego polecenia, aby uruchomić przykładową topologię:
   
        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount
   
    > [!NOTE]
    > We wcześniejszych wersjach usługi HDInsight nazwa klasy topologii to `storm.starter.WordCountTopology` zamiast `org.apache.storm.starter.WordCountTopology`.
   
    To polecenie uruchamia w klastrze przykładową topologię WordCount o przyjaznej nazwie „wordcount”. Przykład obejmuje losowe wygenerowanie zdań i zliczenie wystąpień poszczególnych wyrazów w zdaniach.
   
    > [!NOTE]
    > Podczas przesyłania własnych topologii do klastra przed użyciem polecenia `storm` należy skopiować plik JAR zawierający klaster. Skopiuj plik za pomocą polecenia `scp`. Na przykład: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    > 
    > Przykład WordCount i inne przykłady z projektu Storm Starter znajdują się już w klastrze w lokalizacji `/usr/hdp/current/storm-client/contrib/storm-starter/`.

Jeśli chcesz zobaczyć źródło przykładów z projektu Storm Starter, kod możesz znaleźć na stronie [https://github.com/apache/storm/tree/1.0.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.0.x-branch/examples/storm-starter). Ten link dotyczy platformy Storm 1.0.x, która jest dostarczana z usługą HDInsight 3.5. W przypadku innych wersji platformy Storm należy za pomocą przycisku __Branch__ (Gałąź) w górnej części strony wybrać inną wersję platformy Storm.

## <a name="monitor-the-topology"></a>Monitorowanie topologii

Interfejs użytkownika platformy Storm udostępnia interfejs sieci Web do pracy z uruchomionymi topologiami i jest zawarty w klastrze usługi HDInsight.

Wykonaj następujące kroki, aby monitorować topologię za pomocą interfejsu użytkownika platformy Storm:

1. Aby wyświetlić interfejs użytkownika platformy Storm, otwórz stronę o adresie https://CLUSTERNAME.azurehdinsight.net/stormui w przeglądarce sieci Web. Zastąp ciąg **CLUSTERNAME** nazwą klastra.
    
    > [!NOTE]
    > Jeśli zostanie wyświetlony monit o podanie nazwy użytkownika i hasła, wprowadź nazwę administratora klastra (admin) i hasło użyte podczas tworzenia klastra.

2. W obszarze **Topology summary** (Podsumowanie topologii) wybierz pozycję **wordcount** w kolumnie **Name** (Nazwa). Zostaną wyświetlone informacje o topologii.
    
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
    
    * **Rebalance** (Ponowne równoważenie) — dopasowanie równoległości topologii. Po zmianie liczby węzłów w klastrze należy przeprowadzić ponowne równoważenie uruchomionych topologii. Pozwoli to dostosować równoległość topologii w celu kompensacji zwiększonej/zmniejszonej liczby węzłów w klastrze. Aby uzyskać więcej informacji, zobacz temat [Pojęcie równoległości w topologii Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).
    
    * **Kill** (Kasuj) — zakończenie działania topologii Storm po określonym czasie.

3. Na tej stronie wybierz pozycję z sekcji **Spouts** (Elementy spout) lub **Bolts** (Elementy bolt). Zostaną wyświetlone informacje o wybranym składniku.
   
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
   
    W tym przykładzie wyraz **seven** wystąpił 1493957 razy. Tyle razy został on napotkany od momentu uruchomienia tej topologii.

## <a name="stop-the-topology"></a>Zatrzymywanie topologii

Wróć do strony **Topology summary** (Podsumowanie topologii) dla topologii WordCount, a następnie naciśnij przycisk **Kill** (Kasuj) w obszarze **Topology actions** (Akcje topologii). Po wyświetleniu monitu wprowadź 10 jako liczbę sekund oczekiwania przed zatrzymaniem topologii. Po upłynięciu limitu czasu topologia nie będzie już wyświetlana w obszarze **interfejsu użytkownika platformy Storm** pulpitu nawigacyjnego.

## <a name="delete-the-cluster"></a>Usuwanie klastra

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a id="next"></a>Następne kroki

W tym samouczku dotyczącym platformy Apache Storm przedstawiono podstawy pracy z platformą Storm w usłudze HDInsight. W dalszej kolejności zapoznaj się ze sposobem [opracowywania topologii opartych na języku Java za pomocą programu Maven](hdinsight-storm-develop-java-topology.md).

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

