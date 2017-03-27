---
title: "Rozpoczynanie pracy z klastrem platformy Apache Spark w usłudze Azure HDInsight | Microsoft Docs"
description: "Instrukcje krok po kroku dotyczące sposobu szybkiego tworzenia klastra Apache Spark w usłudze HDInsight oraz używania programu Spark SQL w notesach Jupyter do wykonywania interakcyjnych zapytań."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/13/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: f43a08a7096476fea79884163f89441e7e0c34cf
ms.lasthandoff: 03/17/2017


---
# <a name="get-started-create-apache-spark-cluster-in-azure-hdinsight-and-run-interactive-queries-using-spark-sql"></a>Wprowadzenie: tworzenie klastra Apache Spark w usłudze Azure HDInsight i uruchamianie interakcyjnych zapytań przy użyciu programu Spark SQL

Dowiedz się, jak utworzyć klaster [Apache Spark](hdinsight-apache-spark-overview.md) w usłudze HDInsight, a następnie użyć notesu [Jupyter](https://jupyter.org) do uruchamiania interakcyjnych zapytań Spark SQL w klastrze Spark.

   ![Rozpoczęcie pracy z platformą Apache Spark w usłudze HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.getstartedflow.png "Samouczek Rozpoczęcie pracy z platformą Apache Spark w usłudze HDInsight. Przedstawione kroki: tworzenie konta magazynu; tworzenie klastra; uruchamianie instrukcji rozwiązania Spark SQL")

## <a name="prerequisites"></a>Wymagania wstępne
* **Subskrypcja platformy Azure**. Przed rozpoczęciem tego samouczka musisz dysponować subskrypcją platformy Azure. Zobacz [Utwórz bezpłatne konto platformy Azure już dzisiaj](https://azure.microsoft.com/free).

## <a name="create-a-spark-cluster"></a>Tworzenie klastra Spark
W tej sekcji tworzysz klaster Spark w usłudze HDInsight przy użyciu [szablonu usługi Azure Resource Manager](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Aby zapoznać się z innymi metodami tworzenia klastra, zobacz temat [Tworzenie klastrów usługi HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknij poniższy obraz, aby otworzyć szablon w usłudze Azure Portal.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Wprowadź następujące wartości:

    ![Tworzenie klastra Spark w usłudze HDInsight przy użyciu szablonu usługi Azure Resource Manager](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Tworzenie klastra Spark w usłudze HDInsight przy użyciu szablonu usługi Azure Resource Manager")

    * **Subskrypcja**: Wybierz swoją subskrypcję platformy Azure dla tego klastra.
    * **Grupa zasobów**: utwórz grupę zasobów lub wybierz istniejącą. Grupa zasobów służy do zarządzania zasobami platformy Azure na potrzeby projektów.
    * **Lokalizacja**: Wybierz lokalizację dla grupy zasobów.  Ta lokalizacja jest używana również na potrzeby domyślnego magazynu klastra i klastra usługi HDInsight.
    * **ClusterName**: wprowadź nazwę tworzonego klastra Hadoop.
    * **Wersja Spark**: wybierz wersję Spark, którą chcesz zainstalować w klastrze.
    * **Nazwa logowania i hasło klastra**: domyślna nazwa logowania to admin.
    * **Nazwa użytkownika i hasło SSH**.

   Zapisz te wartości.  Będą potrzebne później podczas korzystania z samouczka.

3. Wybierz pozycję **Wyrażam zgodę na powyższe warunki i postanowienia** i pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij przycisk **Kup**. Zostanie wyświetlony nowy kafelek zatytułowany Submitting deployment for Template deployment (Przesyłanie wdrożenia dla wdrożenia szablonu). Utworzenie klastra trwa około 20 minut.

> [!NOTE]
> W tym artykule opisano tworzenie klastra Spark korzystającego z [obiektów blob usługi Azure Storage jako magazynu klastra](hdinsight-hadoop-use-blob-storage.md). Można również utworzyć klaster Spark korzystający z usługi [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) jako magazynu dodatkowego oprócz obiektów blob usługi Azure Storage używanymi jako magazyn domyślny. Aby uzyskać instrukcje, zobacz [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) (Tworzenie klastra HDInsight z usługą Data Lake Store).
>
>

## <a name="run-a-spark-sql-query"></a>Uruchamianie zapytań Spark SQL

W tej sekcji użyjesz notesu Jupyter do uruchamiania zapytań Spark SQL w klastrze Spark. Klastry HDInsight Spark zapewniają trzy jądra, których można użyć z notesem Jupyter. Są to:

* **PySpark** (dla aplikacji napisanych w języku Python)
* **PySpark3** (dla aplikacji napisanych w języku Python3)
* **Spark** (dla aplikacji napisanych w języku Scala)

W tym artykule jest używane jądro **PySpark**. Aby uzyskać informacje na temat tych jąder, zobacz [Use Jupyter notebooks kernels with Apache Spark clusters in HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md) (Używanie jąder notesu Jupyter z klastrami Apache Spark w usłudze HDInsight). Do najważniejszych korzyści zapewnianych przez jądro PySpark należą:

* Konteksty dla usług Spark i Hive są ustawiane automatycznie.
* W komórkach można używać poleceń magicznych, takich jak `%%sql`, aby bezpośrednio uruchamiać zapytania SQL lub Hive, bez żadnych poprzedzających fragmentów kodu.
* Automatyczna wizualizacja wyników zapytań SQL lub Hive.

### <a name="create-jupyter-notebook-with-pyspark-kernel"></a>Tworzenie notesu Jupyter z użyciem jądra PySpark

1. Otwórz [portal Azure](https://portal.azure.com/).

2. Jeśli wybrana została opcja przypięcia klastra do pulpitu nawigacyjnego, kliknij kafelek klastra na pulpicie nawigacyjnym, aby uruchomić blok klastra.

    Jeśli klaster nie został przypięty do pulpitu nawigacyjnego, w okienku po lewej kliknij pozycję **Klastry HDInsight**, a następnie kliknij utworzony klaster.

3. W obszarze **Szybkie łącza** kliknij pozycję **Pulpity nawigacyjne klastra**, a następnie pozycję **Notes Jupyter**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klastra.

   ![Pulpity nawigacyjne klastra usługi HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-azure-portal-cluster-dashboards.png "Pulpity nawigacyjne klastra usługi HDInsight")

   > [!NOTE]
   > Można również przejść do aplikacji Jupyter Notebook dla klastra, otwierając następujący adres URL w przeglądarce. Zastąp ciąg **CLUSTERNAME** nazwą klastra:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Utwórz notes. Kliknij opcję **New** (Nowy), a następnie kliknij pozycję **PySpark**.

   ![Tworzenie notesu Jupyter](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.createnotebook.png "Tworzenie notesu Jupyter")

   Zostanie utworzony i otwarty nowy notes o nazwie Untitled (Untitled.pynb).

4. Jeśli chcesz, kliknij nazwę notesu u góry, a następnie wprowadź przyjazną nazwę.

    ![Wprowadzanie nazwy notesu](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.note.jupyter.notebook.name.png "Wprowadzanie nazwy notesu")

5. Wklej następujący kod do pustej komórki, a następnie naciśnij klawisze **SHIFT + ENTER**, aby go wykonać. Kod importuje typy wymagane w tym scenariuszu:

        from pyspark.sql.types import *

    Ponieważ notes został utworzony z użyciem jądra PySpark, nie ma konieczności jawnego tworzenia kontekstów. Konteksty Spark i Hive są automatycznie tworzone po uruchomieniu pierwszej komórki kodu.

    ![Stan zadania notesu Jupyter](./media/hdinsight-apache-spark-jupyter-spark-sql/hdispark.jupyter.job.status.png "Stan zadania notesu Jupyter")

    Przy każdym uruchomieniu zadania w oprogramowaniu Jupyter w tytule okna przeglądarki sieci Web wyświetlany jest stan **(Busy)** (Zajęty) wraz z tytułem notesu. Widoczne jest także pełne kółko obok tekstu **PySpark** w prawym górnym rogu. Po zakończeniu zadania zmienia się ono w pusty okrąg.

6. Zarejestruj zestaw danych jako tabelę tymczasową (**hvac**), uruchamiając następujący kod.

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

    Klastry Spark w usłudze HDInsight mają plik danych przykładowych **hvac.csv** w katalogu **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

7. W celu wykonania zapytania względem danych uruchom następujący kod.

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   Ponieważ używane jest jądro PySpark, można teraz bezpośrednio uruchomić zapytanie SQL w tabeli tymczasowej **hvac** utworzonej za pomocą polecenia magicznego `%%sql`. Aby uzyskać więcej informacji na temat polecenia magicznego `%%sql` oraz innych poleceń magicznych dostępnych za pośrednictwem jądra PySpark, zobacz [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   Domyślnie jest wyświetlana następująca tabela danych wyjściowych.

     ![Wyniki zapytania w tabeli danych wyjściowych](./media/hdinsight-apache-spark-jupyter-spark-sql/tabular.output.png "Wyniki zapytania w tabeli danych wyjściowych")

    Wyniki można również przeglądać w postaci innych wizualizacji. Na przykład wykres warstwowy tych samych danych wyjściowych będzie wyglądać w następujący sposób.

    ![Wykres warstwowy wyników zapytania](./media/hdinsight-apache-spark-jupyter-spark-sql/area.output.png "Wykres warstwowy wyników zapytania")

9. Po zakończeniu działania aplikacji zamknij notes w celu zwolnienia zasobów klastra. W tym celu w menu **File** (Plik) w notesie kliknij polecenie **Close and Halt** (Zamknij i zatrzymaj).

## <a name="troubleshoot"></a>Rozwiązywanie problemów

Poniżej przedstawiono typowe problemy, które można napotkać podczas pracy z klastrami HDInsight.

### <a name="access-control-requirements"></a>Wymagania dotyczące kontroli dostępu
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="delete-the-cluster"></a>Usuwanie klastra
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="see-also"></a>Zobacz też
* [Przegląd: platforma Apache Spark w usłudze Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenariusze
* [Platforma Spark i analiza biznesowa: interakcyjna analiza danych na platformie Spark w usłudze HDInsight z użyciem narzędzi do analizy biznesowej](hdinsight-apache-spark-use-bi-tools.md)
* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do analizy temperatury w budynku z użyciem danych HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Platforma Spark i usługa Machine Learning: korzystanie z platformy Spark w usłudze HDInsight do przewidywania wyników kontroli żywności](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Przesyłanie strumieniowe Spark: korzystanie z platformy Spark w usłudze HDInsight do tworzenia aplikacji do przesyłania strumieniowego w czasie rzeczywistym](hdinsight-apache-spark-eventhub-streaming.md)
* [Analiza dzienników witryny sieci Web na platformie Spark w usłudze HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Analiza danych telemetrycznych usługi Application Insight przy użyciu platformy Spark w usłudze HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Tworzenie i uruchamianie aplikacji
* [Tworzenie autonomicznych aplikacji przy użyciu języka Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Zdalne uruchamianie zadań w klastrze Spark przy użyciu programu Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Narzędzia i rozszerzenia
* [Tworzenie i przesyłanie aplikacji Spark Scala przy użyciu dodatku HDInsight Tools Plugin for IntelliJ IDEA](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Zdalne debugowanie aplikacji Spark przy użyciu dodatku HDInsight Tools Plugin for IntelliJ IDEA](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Korzystanie z notesów Zeppelin w klastrze Spark w usłudze HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Korzystanie z zewnętrznych pakietów z notesami Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalacja oprogramowania Jupyter na komputerze i nawiązywanie połączenia z klastrem Spark w usłudze HDInsight](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Zarządzanie zasobami
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

