---
title: Zainstaluj Presto w klastrach Azure HDInsight Linux | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak zainstalować Presto i Airpal w klastrach opartych na systemie Linux usługi HDInsight Hadoop za pomocą akcji skryptu."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: af6bf1d69761520d20d0a0c5d872377793f6650b
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a>Zainstalować i używać Presto w klastrach HDInsight Hadoop

W tym temacie możesz dowiedzieć się, jak instalować Presto w klastrach HDInsight Hadoop za pomocą akcji skryptu. Możesz również informacje o instalowaniu Airpal w istniejącym klastrze Presto usługi HDInsight.

> [!IMPORTANT]
> Kroki w tym dokumencie wymagają **klastra usługi HDInsight Hadoop 3.5** używającą systemu Linux. Linux jest jedynym systemem operacyjnym używanym w połączeniu z usługą HDInsight w wersji 3.4 lub nowszą. Aby uzyskać więcej informacji, zobacz [wersji usługi HDInsight](hdinsight-component-versioning.md).

## <a name="what-is-presto"></a>Co to jest Presto?
[Presto](https://prestodb.io/overview.html) jest szybkie rozproszonej aparatu zapytań SQL dla danych big data. Presto nadaje się do interaktywnego kwerend do poziomu petabajtów danych. Aby uzyskać więcej informacji na składniki Presto i ich współpracę, zobacz [Presto pojęcia](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).

> [!WARNING]
> Składniki dostarczony z klastrem usługi HDInsight są w pełni obsługiwane, a Microsoft Support pomoże w celu odizolowania i rozwiązać problemy związane z tych składników.
> 
> Niestandardowe składniki, takie jak Presto, otrzymywanie pomocy uzasadnione ekonomicznie ułatwiające aby dalej rozwiązywać ten problem. Może to spowodować w rozwiązaniu problemu lub monitem o Uwzględnij dostępnych kanałów dla technologiach typu open source wykryto głębokie doświadczenia z tej technologii. Na przykład istnieje wiele witryn społeczności, które mogą być używane, takie jak: [forum MSDN dla usługi HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Projekty Apache mieć witryny projektu na [http://apache.org](http://apache.org), na przykład: [Hadoop](http://hadoop.apache.org/).
> 
> 


## <a name="install-presto-using-script-action"></a>Zainstaluj Presto za pomocą akcji skryptu

Ta sekcja zawiera instrukcje dotyczące sposobu używania przykładowy skrypt, podczas tworzenia nowego klastra za pomocą portalu Azure. 

1. Uruchom inicjowania obsługi klastra, wykonując kroki opisane w [klastrów usługi HDInsight opartych na systemie Linux należy](hdinsight-hadoop-create-linux-clusters-portal.md). Upewnij się, że tworzenia klastra przy użyciu **niestandardowy** przepływu tworzenia klastra. Należy się upewnić, że klastra, którą utworzysz spełnia następujące wymagania.

    a. Musi to być klastra usługi Hadoop z usługą HDInsight w wersji 3.5.

    b. Należy użyć magazynu Azure do przechowywania danych. Używanie Presto w klastrze, który używa usługi Azure Data Lake Store jako opcji magazynu nie jest jeszcze obsługiwane. 

    ![Tworzenie klastra usługi HDInsight przy użyciu niestandardowych opcji](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. Na **Zaawansowane ustawienia** bloku, wybierz opcję **akcji skryptu**i podaj poniższe informacje:
   
   * **Nazwa**: Wprowadź przyjazną nazwę dla akcji skryptu.
   * **Identyfikator URI skryptu powłoki systemowej**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`
   * **HEAD**: Zaznaczenie tego pola wyboru
   * **Proces ROBOCZY**: Zaznaczenie tego pola wyboru
   * **DOZORCY**: wyczyść to pole wyboru
   * **Parametry**: pozostaw to pole puste


3. W dolnej części **akcji skryptu** bloku, kliknij przycisk **wybierz** przycisk, aby zapisać konfigurację. Na koniec kliknij **wybierz** przycisk w dolnej części **Zaawansowane ustawienia** bloku, aby zapisać informacje o konfiguracji.

4. Kontynuuj, inicjowania obsługi klastra, zgodnie z opisem w [klastrów usługi HDInsight opartych na systemie Linux należy](hdinsight-hadoop-create-linux-clusters-portal.md).

    > [!NOTE]
    > Program Azure PowerShell, interfejsu wiersza polecenia Azure, zestawu .NET SDK usługi HDInsight lub szablonów usługi Azure Resource Manager można również zastosować akcji skryptu. Akcje skryptu można również dotyczą już działające klastry. Aby uzyskać więcej informacji, zobacz [Dostosowywanie klastrów usługi HDInsight z akcji skryptu](hdinsight-hadoop-customize-cluster-linux.md).
    > 
    > 

## <a name="use-presto-with-hdinsight"></a>Korzystanie z usługą HDInsight Presto

Wykonaj poniższe kroki, aby użyć Presto w klastrze HDInsight po zainstalowaniu go za pomocą kroków opisanych powyżej.

1. Nawiąż połączenie z klastrem usługi HDInsight przy użyciu protokołu SSH:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Aby uzyskać więcej informacji, zobacz [Używanie protokołu SSH w usłudze HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).
     

2. Uruchom powłokę Presto przy użyciu następującego polecenia.
   
        presto --schema default

3. Uruchom zapytania w tabeli próbki **hivesampletable**, który jest dostępny na wszystkich klastrach HDInsight domyślnie.
   
        select count (*) from hivesampletable;
   
    Domyślnie [Hive](https://prestodb.io/docs/current/connector/hive.html) i [TPCH](https://prestodb.io/docs/current/connector/tpch.html) łączników dla Presto są już skonfigurowane. Gałąź łącznik jest skonfigurowany do używania domyślnie zainstalowaną instalacji Hive, dlatego wszystkie tabele z gałęzi będzie automatycznie wyświetlane w Presto.

    Aby uzyskać szczegółowy opis używania Presto, zobacz [Presto dokumentacji](https://prestodb.io/docs/current/index.html).

## <a name="use-airpal-with-presto"></a>Airpal za pomocą Presto

[Airpal](https://github.com/airbnb/airpal#airpal) jest interfejsem open source opartych na sieci web zapytania dla Presto. Aby uzyskać więcej informacji o Airpal, zobacz [dokumentacji Airpal](https://github.com/airbnb/airpal#airpal).

W tej sekcji opisano kroki, aby **zainstalować Airpal na edgenode** klastra usługi HDInsight Hadoop Presto już zainstalowane. Dzięki temu, że Airpal zapytania interfejs jest dostępny za pośrednictwem Internetu.

1. Przy użyciu protokołu SSH, podłącz do headnode Presto zainstalowanym klastra usługi HDInsight:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Aby uzyskać więcej informacji, zobacz [Używanie protokołu SSH w usłudze HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Po nawiązaniu połączenia, uruchom następujące polecenie.

        sudo slider registry  --name presto1 --getexp presto 
   
    Powinny pojawić się dane wyjściowe podobne do następujących:

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. Z danych wyjściowych, zwróć uwagę na wartość dla **wartość** właściwości. Będzie on potrzebny podczas instalowania Airpal na edgenode klastra. W powyższych danych wyjściowych, wartość, która będzie potrzebny jest **10.0.0.12:9090**.

4. Szablon  **[tutaj](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)**  do tworzenia edgenode klastra usługi HDInsight i podaj wartości, jak pokazano na poniższym zrzucie ekranu.

    ![HDInsight instalacji Airpal w klastrze Presto](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. Kliknij pozycję **Kup**.

6. Po zmiany zostaną zastosowane do konfiguracji klastra, uzyskać dostęp do interfejsu sieci web Airpal, wykonując następujące kroki.

    a. W bloku klastra, kliknij **aplikacji**.

    ![Uruchamianie usługi HDInsight Airpal w klastrze Presto](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    b. Z **zainstalowane aplikacje** bloku, kliknij przycisk **Portal** przed airpal.

    ![Uruchamianie usługi HDInsight Airpal w klastrze Presto](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    c. Po wyświetleniu monitu wprowadź poświadczenia administratora, które można określić podczas tworzenia klastra usługi HDInsight Hadoop.

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a>Dostosowanie Presto instalacji w klastrze usługi HDInsight

Po zainstalowaniu Presto na klaster usługi HDInsight Hadoop, można dostosować instalacji, aby wprowadzić zmiany, takie jak zaktualizować ustawienia pamięci, zmienianie łączników, itp. W tym celu wykonaj poniższe kroki.

1. Przy użyciu protokołu SSH, podłącz do headnode Presto zainstalowanym klastra usługi HDInsight:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    Aby uzyskać więcej informacji, zobacz [Używanie protokołu SSH w usłudze HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Wprowadź zmiany w konfiguracji w pliku `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`. Aby uzyskać więcej informacji na Presto konfiguracji, zobacz [Presto konfiguracji dla klastrów YARN](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).

3. Zatrzymaj i Zakończ bieżący uruchomione wystąpienie Presto.

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. Uruchom nowe wystąpienie klasy Presto do dostosowywania.

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. Poczekaj, aż gotowość i zanotuj adres presto koordynatora nowego wystąpienia.


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a>Generowanie wyników testów dla klastrów usługi HDInsight, które są uruchamiane Presto

TPC DS to branżowy standard do pomiaru wydajności wiele systemów wsparcia decyzji, w tym systemy danych big data. Umożliwia Presto w klastrach HDInsight generowania danych i ocenić, jak są porównywane z własnych danych testowych HDInsight. Aby uzyskać więcej informacji, zobacz [tutaj](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).



## <a name="see-also"></a>Zobacz też
* [Zainstalować i używać Hue w klastrach HDInsight](hdinsight-hadoop-hue-linux.md). HUE jest interfejs użytkownika, który ułatwia tworzenie, uruchamianie i Zapisz zadań Pig i Hive, a także przeglądania magazynu domyślnego dla Twojej usługi HDInsight klastra w sieci web.

* [Zainstaluj Giraph w klastrach HDInsight](hdinsight-hadoop-giraph-install-linux.md). Dostosowywanie klastra należy zainstalować Giraph w klastrach HDInsight Hadoop. Giraph umożliwia wykonywanie wykresu przetwarzanie przy użyciu platformy Hadoop i mogą być używane z usługi Azure HDInsight.

* [Zainstaluj Solr w klastrach HDInsight](hdinsight-hadoop-solr-install-linux.md). Dostosowywanie klastra należy zainstalować Solr w klastrach HDInsight Hadoop. Solr umożliwia wykonywanie operacji wyszukiwania zaawansowanego w przechowywanych danych.

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
