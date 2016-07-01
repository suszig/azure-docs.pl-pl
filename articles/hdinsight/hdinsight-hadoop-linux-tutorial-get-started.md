<properties
    pageTitle="Samouczek systemu Linux: wprowadzenie do usług Hadoop i Hive | Microsoft Azure"
    description="Skorzystaj z tego samouczka systemu Linux, aby rozpocząć korzystanie z usługi Hadoop w usłudze HDInsight. Dowiedz się, jak udostępniać klastry z systemem Linux i wykonywać zapytania na danych przy użyciu Hive."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="03/16/2016"
    ms.author="jgao"/>

# Samouczek Hadoop: rozpoczynanie pracy z opartą na systemie Linux usługą Hadoop w usłudze HDInsight

> [AZURE.SELECTOR]
- [Oparte na systemie Linux](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Oparte na systemie Windows](hdinsight-hadoop-tutorial-get-started-windows.md)

Informacje o sposobie tworzenia opartych na systemie Linux klastrów [Hadoop](http://hadoop.apache.org/) w usłudze HDInsight i uruchamiania zadań Hive w usłudze HDInsight. [Apache Hive](https://hive.apache.org/) jest najbardziej popularnym składnikiem w ekosystemie usługi Hadoop. Obecnie usługa HDInsight obejmuje 4 różne typy klastrów: [Hadoop](hdinsight-hadoop-introduction), [Spark](hdinsight-apache-spark-overview.md), [HBase](hdinsight-hbase-overview.md) i [Storm](hdinsight-storm-overview.md).  Każdy typ klastra obsługuje inny zestaw składników. Wszystkie 4 typy klastrów obsługują składnik Hive. Aby uzyskać listę obsługiwanych składników w usłudze HDInsight, zobacz artykuł [Nowości w wersjach klastra Hadoop dostarczanych z usługą HDInsight](hdinsight-component-versioning.md)  

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

### Wymagania wstępne

Przed rozpoczęciem tego samouczka potrzebna będzie:

- **Subskrypcja platformy Azure**: aby utworzyć bezpłatne konto próbne na jeden miesiąc, przejdź do [azure.microsoft.com/free](https://azure.microsoft.com/free).

## Tworzenie klastra

Większość zadań usługi Hadoop to zadania wsadowe. Tworzysz klaster, uruchamiasz pewne zadania, a następnie usuwasz klaster. W tej sekcji utworzysz oparty na systemie Linux klaster Hadoop w usłudze HDInsight przy użyciu [szablonu Azure ARM](../resource-group-template-deploy.md). Szablon ARM jest w pełni konfigurowalny. Dzięki niemu tworzenie zasobów Azure, takich jak usługa HDInsight, jest proste. Znajomość szablonów Azure ARM nie jest wymagana do korzystania z tego samouczka. Inne metody tworzenia klastrów i opis właściwości używanych w tym samouczku znajdziesz w artykule [Tworzenie klastrów usługi HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Szablon ARM używany w tym samouczku znajduje się w publicznym kontenerze obiektu blob [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight.json). 

1. Kliknij poniższy obraz, aby zalogować się do platformy Azure i otworzyć szablon ARM w Portalu Azure. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. W bloku **Parametry** wprowadź następujące informacje:

    ![HDInsight Linux wprowadzenie do używania szablonu ARM w portalu](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png).

    - **ClusterName**: wprowadź nazwę tworzonego klastra Hadoop.
    - **Nazwa logowania i hasło klastra**: domyślna nazwa logowania to **admin**.
    - **Nazwa użytkownika i hasło SSH**: domyślna nazwa użytkownika to **sshuser**.  Tę nazwę można zmienić. 
    
    Inne parametry są opcjonalne podczas wykonywania tego samouczka. Można je pozostawić bez zmian. 
    
    Każdy klaster zależy od konta Magazynu obiektów Blob platformy Azure. Zwykle jest ono określane jako domyślne konto magazynu. Klaster usługi HDInsight i jego domyślne konto magazynu muszą znajdować się wspólnie w tym samym regionie Azure. Usunięcie klastrów nie powoduje usunięcia konta magazynu. W tym szablonie domyślna nazwa konta magazynu to nazwa klastra z dołączonym członem „store”. 
    
3. Kliknij przycisk **OK**, aby zapisać parametry.
4. W bloku **Wdrożenie niestandardowe** wprowadź **nazwę nowej grupy zasobów**, aby utworzyć nową grupę zasobów.  Grupa zasobów jest kontenerem, który grupuje klaster, konto magazynu zależnego oraz inne zasoby. Lokalizacja grupy zasobów może różnić się od lokalizacji klastra.
5. Kliknij opcję **Postanowienia prawne**, a następnie kliknij przycisk **Utwórz**.
6. Sprawdź, czy zaznaczone jest pole wyboru **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij przycisk **Utwórz**. Zobaczysz nowy kafelek zatytułowany **Wdrażanie szablonu wdrożenia**. Utworzenie klastra trwa około 20 minut. 
7.  Po utworzeniu klastra napis na kafelku zmieni się na podaną nazwę grupy zasobów. Następnie portal automatycznie otwiera dwa bloki dotyczące klastra oraz ustawienia klastra. 

    ![HDInsight Linux wprowadzenie do ustawień klastra](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-cluster-settings.png).

    Na liście są dwa zasoby, czyli klaster i domyślne konto magazynu.

##Uruchamianie zapytań Hive

[Apache Hive](hdinsight-use-hive.md) jest najbardziej popularnym składnikiem używanym w usłudze HDInsight Istnieje wiele sposobów uruchamiania zadań Hive w usłudze HDInsight. W tym samouczku użyjesz widoku Hive narzędzia Ambari, korzystając z portalu, aby uruchamiać zadania Hive. Aby poznać inne metody przesyłania zadań Hive, zobacz temat [Używanie Hive w usłudze HDInsight](hdinsight-use-hive.md).

1. Aby otworzyć narzędzie Ambari, przejdź do **https://&lt;ClusterName>. azurehdinsight.net**, gdzie &lt;ClusterName> to klaster utworzony w poprzedniej sekcji.
2. Wprowadź nazwę użytkownika Hadoop i hasło określone w poprzedniej części. Domyślna nazwa użytkownika to **admin**.
3. Otwórz widok **Hive View** pokazany na poniższym zrzucie ekranu:

    ![Wybieranie widoków ambari](./media/hdinsight-hadoop-linux-tutorial-get-started/selecthiveview.png).
4. W części strony __Query Editor__ (Edytor zapytań) wklej poniższe instrukcje HiveQL do arkusza:

        SHOW TABLES;

    >[AZURE.NOTE] Średnik jest wymagany przez Hive.       
        
5. Kliknij przycisk __Execute__ (Wykonaj). Sekcja __Query Process Results__ (Wyniki przetwarzania zapytania) powinna pojawić się poniżej edytora zapytań, prezentując informacje o zadaniu. 

    Po zakończeniu przetwarzania zapytania sekcja __Query Process Results__ (Wyniki przetwarzania zapytania) będzie prezentować wyniki operacji. Powinna być widoczna jedna tabela o nazwie **hivesampletable**. Ta przykładowa tabela składnika Hive jest dostarczana z wszystkimi klastrami usługi HDInsight.

    ![Widoki Hive usługi HDInsight](./media/hdinsight-hadoop-linux-tutorial-get-started/hiveview.png).

6. Powtórz kroki 4 i 5, aby uruchomić następujące zapytanie:

        SELECT * FROM hivesampletable;

    > [AZURE.TIP] Zwróć uwagę na rozwijane menu __Save results__ (Zapisz wyniki) w lewym górnym rogu sekcji __Query Process Results__ (Wyniki przetwarzania zapytania). Służy ono do pobierania wyników lub zapisywania ich w magazynie usługi HDInsight w formacie pliku CSV.

7. Kliknij przycisk **History** (Historia) w celu uzyskania listy zadań.

Po zakończeniu zadania Hive można [wyeksportować wyniki do bazy danych Azure SQL lub bazy danych programu SQL Server](hdinsight-use-sqoop-mac-linux.md), można także [wizualizować wyniki za pomocą programu Excel](hdinsight-connect-excel-power-query.md). Aby uzyskać więcej informacji o korzystaniu z Hive w usłudze HDInsight, zobacz [Używanie Hive i HiveQL z usługą Hadoop w usłudze HDInsight do analizy przykładowego pliku Apache log4j](hdinsight-use-hive.md).

##Czyszczenie na koniec samouczka

Po ukończeniu korzystania z samouczka warto usunąć klaster. Dzięki usłudze HDInsight dane są przechowywane w usłudze Azure Storage, więc można bezpiecznie usunąć klaster, gdy nie jest używany. Opłaty za klaster usługi HDInsight są naliczane nawet wtedy, gdy nie jest używany. Ponieważ opłaty za klaster są wielokrotnie większe niż opłaty za magazyn, ze względów ekonomicznych warto usuwać klastry, gdy nie są używane. 

>[AZURE.NOTE] Przy użyciu [fabryki danych Azure](hdinsight-hadoop-create-linux-clusters-adf.md) można tworzyć klastry usługi HDInsight na żądanie i skonfigurować ustawienie TimeToLive, aby usuwać te klastry automatycznie. 

**Usuwanie klastra i/lub domyślnego konta magazynu**

1. Zaloguj się w [Portalu Azure](https://portal.azure.com).
2. Na pulpicie nawigacyjnym portalu kliknij kafelek z nazwą grupy zasobów użytą podczas tworzenia klastra.
3. Kliknij przycisk **Usuń** w bloku zasobów, aby usunąć grupę zasobów, zawierającą klaster i domyślne konto magazynu; lub kliknij nazwę klastra na kafelku **Zasoby**, a następnie kliknij przycisk **Usuń** w bloku klastra. Uwaga: usunięcie grupy zasobów powoduje usunięcie konta magazynu. Jeśli chcesz zachować konta magazynu, wybierz opcję usunięcia tylko klastra.

## Następne kroki

Po przeczytaniu tego samouczka umiesz utworzyć oparty na systemie Linux klaster usługi HDInsight przy użyciu szablonu ARM oraz wykonywać podstawowe zapytania Hive.

Aby dowiedzieć się więcej na temat analizowania danych za pomocą usługi HDInsight, zobacz następujące tematy:

- Aby dowiedzieć się więcej o korzystaniu z programu Hive z usługą HDInsight, w tym poznać sposoby wykonywania zapytań Hive z programu Visual Studio, zobacz artykuł [Korzystanie z programu Hive z usługą HDInsight][hdinsight-use-hive].

- Aby dowiedzieć się więcej na temat Pig, języka używanego do przekształcania danych, zobacz [Korzystanie z języka Pig z usługą HDInsight][hdinsight-use-pig].

- Aby dowiedzieć się więcej o MapReduce, sposobie pisania programów, które przetwarzają dane w usłudze Hadoop, zobacz [Używanie MapReduce z usługą HDInsight][hdinsight-use-mapreduce].

- Aby dowiedzieć się więcej o używaniu narzędzi HDInsight Tools for Visual Studio do analizowania danych w usłudze HDInsight, zobacz [Wprowadzenie do używania narzędzi Visual Studio Hadoop dla usługi HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).

Jeśli chcesz już zacząć korzystać z własnych danych i dowiedzieć się więcej o sposobie przechowywania danych w usłudze HDInsight lub sposobie pobierania danych do usługi HDInsight, zobacz następujące tematy:

- Informacje o używaniu Magazynu obiektów Blob platformy Azure w usłudze HDInsight, zobacz [Używanie Magazynu obiektów Blob platformy Azure z usługą HDInsight](hdinsight-hadoop-use-blob-storage.md).

- Aby uzyskać informacje na temat przekazywania danych do usługi HDInsight, zobacz [Przekazywanie danych do usługi HDInsight][hdinsight-upload-data].

Jeśli chcesz dowiedzieć się więcej o tworzeniu klastra usługi HDInsight i zarządzaniu nim, zobacz następujące tematy:

- Aby uzyskać więcej informacji na temat zarządzania opartym na systemie Linux klastrem usługi HDInsight, zobacz [Zarządzanie klastrami usługi HDInsight za pomocą narzędzia Ambari](hdinsight-hadoop-manage-ambari.md).

- Aby dowiedzieć się więcej na temat opcji, które można wybrać podczas tworzenia klastra usługi HDInsight, zobacz [Tworzenie klastra usługi HDInsight w systemie Linux przy użyciu niestandardowych opcji](hdinsight-hadoop-provision-linux-clusters.md).

- Jeśli znasz system Linux i usługę Hadoop, ale chcesz poznać szczegóły dotyczące usługi Hadoop w usłudze HDInsight, zobacz [Praca z usługą HDInsight w systemie Linux](hdinsight-hadoop-linux-information.md). Artykuł zawiera następujące informacje:

    * Adresy URL dla usług uruchamianych w klastrze, takich jak Ambari i WebHCat
    * Lokalizacja plików usługi Hadoop oraz przykłady lokalnego systemu plików
    * Korzystanie z usługi Azure Storage (WASB) zamiast systemu plików HDFS jako domyślnego magazynu danych


[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[powershell-download]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[powershell-install-configure]: powershell-install-configure.md
[powershell-open]: powershell-install-configure.md#Install

[img-hdi-dashboard]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.png
[img-hdi-dashboard-query-select]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.png
[img-hdi-dashboard-query-select-result]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.png
[img-hdi-dashboard-query-select-result-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.output.png
[img-hdi-dashboard-query-browse-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.browse.output.png
[image-hdi-clusterstatus]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.ClusterStatus.png
[image-hdi-gettingstarted-powerquery-importdata]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData.png
[image-hdi-gettingstarted-powerquery-importdata2]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData2.png



<!--HONumber=Jun16_HO2-->


