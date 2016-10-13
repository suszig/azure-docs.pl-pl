<properties
    pageTitle="Tworzenie klastra Spark w usłudze HDInsight w systemie Linux i korzystanie z programu Spark SQL w aplikacji Jupyter do analizy interakcyjnej | Microsoft Azure"
    description="Instrukcje krok po kroku dotyczące sposobu szybkiego tworzenia klastra Apache Spark w usłudze HDInsight oraz używania programu Spark SQL w notesach Jupyter do wykonywania interakcyjnych zapytań."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/25/2016"
    ms.author="nitinme"/>



# Wprowadzenie: tworzenie klastra Apache Spark w usłudze HDInsight w systemie Linux i uruchamianie interakcyjnych zapytań przy użyciu programu Spark SQL

Dowiedz się, jak utworzyć klaster Apache Spark w usłudze HDInsight, a następnie użyć notesu [Jupyter](https://jupyter.org) do uruchamiania interakcyjnych zapytań Spark SQL w klastrze Spark.

   ![Rozpoczęcie pracy z platformą Apache Spark w usłudze HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png  "Get started using Apache Spark in HDInsight tutorial. Steps illustrated: create a storage account; create a cluster; run Spark SQL statements")

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## Wymagania wstępne

- **Subskrypcja platformy Azure**. Przed rozpoczęciem tego samouczka musisz dysponować subskrypcją platformy Azure. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Klient Secure Shell (SSH)**: systemy Linux, Unix i OS X dostarczają klienta SSH za pośrednictwem polecenia `ssh`. W przypadku systemów Windows zalecamy użycie programu [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
    
- **Klucze Secure Shell (SSH) (opcjonalnie)**: można zabezpieczyć konto SSH użyte do nawiązania połączenia z klastrem przy użyciu hasła lub klucza publicznego. Zastosowanie hasła pozwala szybko rozpocząć pracę. Warto użyć tej opcji, jeśli chcesz szybko utworzyć klaster i wykonywać zadania testowe. Użycie klucza jest bardziej bezpieczne, jednak wymaga dodatkowej konfiguracji. Ta metoda jest przydatna podczas tworzenia klastra produkcyjnego. W tym artykule używamy metody hasła. Instrukcje dotyczące sposobu tworzenia i używania kluczy SSH z usługą HDInsight można znaleźć w następujących artykułach:

    -  Komputer z systemem Linux — [Używanie SSH z opartą na systemie Linux usługą HDInsight (Hadoop) z systemów Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
    
    -  Komputer z systemem Windows — [Używanie SSH z opartą na systemie Linux usługą HDInsight (Hadoop) z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md).

>[AZURE.NOTE] W tym artykule użyto szablonu usługi Azure Resource Manager do utworzenia klastra Spark korzystającego z [obiektów blob usługi Azure Storage jako magazynu klastra](hdinsight-hadoop-use-blob-storage.md). Można również utworzyć klaster Spark korzystający z usługi [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) jako magazynu dodatkowego oprócz obiektów blob usługi Azure Storage używanymi jako magazyn domyślny. Aby uzyskać instrukcje, zobacz [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) (Tworzenie klastra HDInsight z usługą Data Lake Store).

### Wymagania dotyczące kontroli dostępu

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## Tworzenie klastra Spark

W tej sekcji utworzysz klaster usługi HDInsight w wersji 3.4 (Spark 1.6.1) przy użyciu szablonu usługi Azure Resource Manager. Informacje o wersji usługi HDInsight i umowach SLA można znaleźć w temacie [Przechowywanie wersji składnika usługi HDInsight](hdinsight-component-versioning.md). Aby zapoznać się z innymi metodami tworzenia klastra, zobacz temat [Tworzenie klastrów usługi HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknij poniższy obraz, aby otworzyć szablon w witrynie Azure Portal.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-spark-cluster-in-hdinsight.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Szablon znajduje się w publicznym kontenerze obiektów blob *https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-spark-cluster-in-hdinsight.json*. 
   
2. W bloku Parametry wprowadź następujące informacje:

    - **ClusterName**: wprowadź nazwę tworzonego klastra Hadoop.
    - **Nazwa logowania i hasło klastra**: domyślna nazwa logowania to admin.
    - **Nazwa użytkownika i hasło SSH**.
    
    Zapisz te wartości.  Będą potrzebne później podczas korzystania z samouczka.

    > [AZURE.NOTE] Protokół SSH umożliwia zdalny dostęp do klastra usługi HDInsight przy użyciu wiersza polecenia. Nazwa użytkownika i hasło używane w tym miejscu służą do nawiązywania połączenia z klastrem za pośrednictwem protokołu SSH. Ponadto nazwa użytkownika SSH musi być unikatowa, ponieważ służy do tworzenia konta użytkownika na wszystkich węzłach klastra usługi HDInsight. Następujące nazwy kont są zarezerwowane do użytku przez usługi w klastrze i nie można ich użyć jako nazwy użytkownika SSH:
    >
    > root, hdiuser, storm, hbase, ubuntu, zookeeper, hdfs, yarn, mapred, hbase, hive, oozie, falcon, sqoop, admin, tez, hcat, hdinsight-zookeeper.

    > Aby uzyskać więcej informacji o korzystaniu z protokołu SSH z usługą HDInsight, zobacz jeden z następujących artykułów:

    > * [Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemów Linux, Unix lub OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    > * [Używanie protokołu SSH z opartą na systemie Linux platformą Hadoop w usłudze HDInsight z systemu Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    
3. Kliknij przycisk **OK**, aby zapisać parametry.

4. W bloku **Wdrożenie niestandardowe** kliknij pole listy rozwijanej **Grupa zasobów**, a następnie kliknij przycisk **Nowa**, aby utworzyć nową grupę zasobów. Grupa zasobów jest kontenerem, który grupuje klaster, zależne konto magazynu oraz inne powiązane zasoby.

5. Kliknij opcję **Postanowienia prawne**, a następnie kliknij przycisk **Utwórz**.

6. Kliknij przycisk **Utwórz**. Zostanie wyświetlony nowy kafelek zatytułowany Submitting deployment for Template deployment (Przesyłanie wdrożenia dla wdrożenia szablonu). Utworzenie klastra i bazy danych SQL trwa około 20 minut.



## Uruchamianie zapytań Spark SQL za pomocą notesu Jupyter

W tej sekcji użyjesz notesu Jupyter do uruchamiania zapytań Spark SQL w klastrze Spark. Klastry HDInsight Spark zapewniają dwa jądra, których można użyć z notesem Jupyter. Są to:

* **PySpark** (dla aplikacji napisanych w języku Python)
* **Spark** (dla aplikacji napisanych w języku Scala)

W tym artykule będzie używane jądro PySpark. W artykule [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels) opisano szczegółowo zalety jądra PySpark. Do najważniejszych korzyści zapewnianych przez jądro PySpark należą:

* Brak konieczności ustawiania kontekstów dla platformy Spark i programu Hive. Są one ustawiane automatycznie.
* Możliwość używania w komórkach poleceń magicznych, takich jak `%%sql`, aby bezpośrednio uruchamiać zapytania SQL lub Hive, bez żadnych poprzedzających fragmentów kodu.
* Automatyczna wizualizacja wyników zapytań SQL lub Hive.

### Tworzenie notesu Jupyter z użyciem jądra PySpark 

1. W [Portalu Azure](https://portal.azure.com/) na tablicy startowej kliknij kafelek klastra Spark (jeśli został przypięty do tablicy startowej). Możesz także przejść do klastra, wybierając polecenia **Przeglądaj wszystko** > **Klastry usługi HDInsight**.   

2. W bloku klastra Spark kliknij pozycję **Szybkie linki**, a następnie w bloku **Pulpit nawigacyjny klastra** kliknij pozycję **Jupyter Notebook**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klastra.

    > [AZURE.NOTE] Można również przejść do aplikacji Jupyter Notebook dla klastra, otwierając następujący adres URL w przeglądarce. Zastąp ciąg __CLUSTERNAME__ nazwą klastra:
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Utwórz nowy notes. Kliknij opcję **New** (Nowy), a następnie kliknij pozycję **PySpark**.

    ![Tworzenie nowego notesu Jupyter](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Create a new Jupyter notebook")

3. Zostanie utworzony i otwarty nowy notes o nazwie Untitled.pynb. Kliknij nazwę notesu u góry, a następnie wprowadź przyjazną nazwę.

    ![Wprowadzanie nazwy notesu](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Provide a name for the notebook")

4. Ponieważ notes został utworzony z użyciem jądra PySpark, nie ma konieczności jawnego tworzenia kontekstów. Konteksty Spark i Hive zostaną automatycznie utworzone po uruchomieniu pierwszej komórki kodu. Możesz zacząć od importowania typów wymaganych w tym scenariuszu. W tym celu wklej poniższy fragment kodu w komórce i naciśnij klawisze **SHIFT + ENTER**.

        from pyspark.sql.types import *
        
    Przy każdym uruchomieniu zadania w oprogramowaniu Jupyter w tytule okna przeglądarki sieci Web będzie wyświetlony stan **(Busy)** (Zajęty) wraz z tytułem notesu. Widoczne będzie także pełne kółko obok tekstu **PySpark** w prawym górnym rogu. Po zakończeniu zadania zmieni się ono w pusty okrąg.

     ![Stan zadania notesu Jupyter](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Status of a Jupyter notebook job")

4. Załaduj przykładowe dane do tabeli tymczasowej. Podczas tworzenia klastra Spark w usłudze HDInsight przykładowy plik danych **hvac.csv** jest kopiowany do skojarzonego konta magazynu pod adresem **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    W pustej komórce wklej następujący przykładowy kod i naciśnij klawisze **SHIFT + ENTER**. Ten przykład kodu rejestruje dane w tabeli tymczasowej o nazwie **hvac**.

        # Load the data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])
        
        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))
        
        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)
        
        # Register the data fram as a table to run queries against
        hvacdf.registerTempTable("hvac")

5. Ponieważ używane jest jądro PySpark, można teraz bezpośrednio uruchomić zapytanie SQL w tabeli tymczasowej **hvac** utworzonej przed chwilą za pomocą polecenia magicznego `%%sql`. Aby uzyskać więcej informacji na temat polecenia magicznego `%%sql` oraz innych poleceń magicznych dostępnych za pośrednictwem jądra PySpark, zobacz [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).
        
        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

5. Po pomyślnym ukończeniu zadania domyślnie wyświetlane są następujące tabelaryczne dane wyjściowe.

    ![Wyniki zapytania w tabeli danych wyjściowych](./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Table output of query result")

    Wyniki można również przeglądać w postaci innych wizualizacji. Na przykład wykres warstwowy tych samych danych wyjściowych będzie wyglądać w następujący sposób.

    ![Wykres warstwowy wyników zapytania](./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Area graph of query result")


6. Po zakończeniu działania aplikacji należy ją zamknąć, aby zwolnić zasoby. W tym celu w menu **File** (Plik) w notesie kliknij polecenie **Close and Halt** (Zamknij i zatrzymaj). Spowoduje to zakończenie pracy i zamknięcie notesu.

##Usuwanie klastra

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## Zobacz też


* [Przegląd: platforma Apache Spark w usłudze Azure HDInsight](hdinsight-apache-spark-overview.md)

### Scenariusze

* [Platforma Spark i analiza biznesowa: interakcyjna analiza danych na platformie Spark w usłudze HDInsight z użyciem narzędzi do analizy biznesowej](hdinsight-apache-spark-use-bi-tools.md)

* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do analizy temperatury w budynku z użyciem danych HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do przewidywania wyników kontroli żywności](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Przesyłanie strumieniowe Spark: korzystanie z platformy Spark w usłudze HDInsight do tworzenia aplikacji do przesyłania strumieniowego w czasie rzeczywistym](hdinsight-apache-spark-eventhub-streaming.md)

* [Analiza dzienników witryny sieci Web na platformie Spark w usłudze HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

* [Analiza danych telemetrycznych usługi Application Insight przy użyciu platformy Spark w usłudze HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

### Tworzenie i uruchamianie aplikacji

* [Tworzenie autonomicznych aplikacji przy użyciu języka Scala](hdinsight-apache-spark-create-standalone-application.md)

* [Zdalne uruchamianie zadań w klastrze Spark przy użyciu programu Livy](hdinsight-apache-spark-livy-rest-interface.md)

### Narzędzia i rozszerzenia

* [Tworzenie i przesyłanie aplikacji Spark Scala przy użyciu dodatku HDInsight Tools Plugin for IntelliJ IDEA](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Zdalne debugowanie aplikacji Spark przy użyciu dodatku HDInsight Tools Plugin for IntelliJ IDEA](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Korzystanie z notesów Zeppelin w klastrze Spark w usłudze HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Korzystanie z zewnętrznych pakietów z notesami Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Instalacja oprogramowania Jupyter na komputerze i nawiązywanie połączenia z klastrem Spark w usłudze HDInsight](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### Zarządzanie zasobami

* [Zarządzanie zasobami klastra Apache Spark w usłudze Azure HDInsight](hdinsight-apache-spark-resource-manager.md)

* [Śledzenie i debugowanie zadań uruchamianych w klastrze Apache Spark w usłudze HDInsight](hdinsight-apache-spark-job-debugging.md)


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md



<!--HONumber=Sep16_HO5-->


