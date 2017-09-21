---
title: "Tworzenie klastra Apache Spark w usłudze Azure HDInsight | Microsoft Docs"
description: "Przewodnik Szybki start platformy HDInsight Spark opisujący sposób tworzenia klastra platformy Apache Spark w usłudze HDInsight."
keywords: spark quickstart,interactive spark,interactive query,hdinsight spark,azure spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/07/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 78051b9df15c62d4caf56d800c9a5f4421ea2254
ms.contentlocale: pl-pl
ms.lasthandoff: 09/13/2017

---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a>Tworzenie klastra platformy Apache Spark w usłudze Azure HDInsight

W tym artykule omówiono sposób tworzenia klastra platformy Apache Spark w usłudze Azure HDInsight, a następnie uruchamiania zapytania Spark SQL na tabeli programu Hive. Aby uzyskać informacje na temat klastra Spark w usłudze HDInsight, zobacz [Przegląd: platforma Apache Spark w usłudze Azure HDInsight](hdinsight-apache-spark-overview.md).

   ![Diagram szybkiego startu zawierający opis czynności prowadzących do utworzenia klastra Apache Spark w usłudze Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Szybki start z projektem Spark przy użyciu Apache Spark w usłudze HDInsight. Przedstawione kroki: tworzenie klastra; uruchamianie interakcyjnych zapytań Spark")

## <a name="prerequisites"></a>Wymagania wstępne

* **Subskrypcja platformy Azure**. Przed rozpoczęciem tego samouczka musisz dysponować subskrypcją platformy Azure. Zobacz [Utwórz bezpłatne konto platformy Azure już dzisiaj](https://azure.microsoft.com/free).

## <a name="create-hdinsight-spark-cluster"></a>Tworzenie klastra HDInsight Spark

W tej sekcji tworzysz klaster HDInsight Spark przy użyciu [szablonu usługi Azure Resource Manager](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Aby zapoznać się z innymi metodami tworzenia klastra, zobacz temat [Tworzenie klastrów usługi HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknij poniższy obraz, aby otworzyć szablon w usłudze Azure Portal.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Wprowadź następujące wartości:

    ![Tworzenie klastra HDInsight Spark przy użyciu szablonu usługi Azure Resource Manager](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Tworzenie klastra Spark w usłudze HDInsight przy użyciu szablonu usługi Azure Resource Manager")

    * **Subskrypcja**: Wybierz swoją subskrypcję platformy Azure dla tego klastra.
    * **Grupa zasobów**: utwórz grupę zasobów lub wybierz istniejącą. Grupa zasobów służy do zarządzania zasobami platformy Azure na potrzeby projektów.
    * **Lokalizacja**: Wybierz lokalizację dla grupy zasobów. Szablon używa tej lokalizacji do tworzenia klastra oraz na potrzeby domyślnego magazynu klastra.
    * **Nazwa klastra**: wprowadź nazwę tworzonego klastra usługi HDInsight.
    * **Wersja platformy Spark**: wybierz pozycję **2.0** jako wersję platformy Spark, którą chcesz zainstalować w klastrze.
    * **Nazwa logowania i hasło klastra**: domyślna nazwa logowania to admin.
    * **Nazwa użytkownika i hasło SSH**.

   Zapisz te wartości.  Będą potrzebne później podczas korzystania z samouczka.

3. Wybierz pozycję **Wyrażam zgodę na powyższe warunki i postanowienia** i pozycję **Przypnij do pulpitu nawigacyjnego**, a następnie kliknij przycisk **Kup**. Zostanie wyświetlony nowy kafelek zatytułowany Submitting deployment for Template deployment (Przesyłanie wdrożenia dla wdrożenia szablonu). Utworzenie klastra trwa około 20 minut.

Problemy podczas tworzenia klastrów usługi HDInsight mogą świadczyć o braku odpowiednich uprawnień. Aby uzyskać więcej informacji, zobacz [Wymagania dotyczące kontroli dostępu](hdinsight-administer-use-portal-linux.md#create-clusters).

> [!NOTE]
> W tym artykule opisano tworzenie klastra Spark korzystającego z [obiektów blob usługi Azure Storage jako magazynu klastra](hdinsight-hadoop-use-blob-storage.md). Można również utworzyć klaster platformy Spark korzystający z usługi [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) jako magazynu domyślnego. Aby uzyskać instrukcje, zobacz [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) (Tworzenie klastra HDInsight z usługą Data Lake Store).
>
>

## <a name="run-spark-sql-statements-on-a-hive-table"></a>Uruchamianie instrukcji rozwiązania Spark SQL na tabeli programu Hive

SQL (Structured Query Language) to najczęściej używany język służący do definiowania danych i wykonywania zapytań na tych danych. Twórcy platformy Spark skorzystali z tego dobrze znanego rozwiązania, umożliwiając wykonywanie zapytań szerokiemu gronu analityków, którzy chcą pracować z danymi przechowywanymi w rozproszonym systemie plików Hadoop (HDFS). Możliwości te zapewnia rozwiązanie Spark SQL. Stanowi ono rozszerzenie platformy Apache Spark służące do przetwarzania danych strukturalnych za pomocą dobrze znanej składni języka SQL.

Rozwiązanie Spark SQL obsługuje języki wykonywania zapytań SQL i HiveQL. Jego możliwości obejmują obsługę powiązań w językach Python, Scala i Java. Pozwala ono wykonywać zapytania na danych przechowywanych w wielu lokalizacjach, takich jak zewnętrzne bazy danych, pliki danych strukturalnych (na przykład JSON) i tabele programu Hive.

### <a name="running-spark-sql-on-an-hdinsight-cluster"></a>Uruchamianie instrukcji rozwiązania Spark SQL w klastrze usługi HDInsight

Gdy używasz notesu programu Jupyter skonfigurowanego na potrzeby klastra platformy HDInsight Spark, możesz pobrać ustawienie wstępne `sqlContext` służące do uruchamiania zapytań programu Hive przy użyciu modułu Spark SQL. W tej sekcji omówiono sposób tworzenia notesu programu Jupyter i uruchamiania podstawowego zapytania Spark SQL na istniejącej tabeli programu Hive (**hivesampletable**), dostępnej we wszystkich klastrach usługi HDInsight.

1. Otwórz [portal Azure](https://portal.azure.com/).

2. Jeśli wybrana została opcja przypięcia klastra do pulpitu nawigacyjnego, kliknij kafelek klastra na pulpicie nawigacyjnym, aby uruchomić blok klastra.

    Jeśli klaster nie został przypięty do pulpitu nawigacyjnego, w okienku po lewej kliknij pozycję **Klastry HDInsight**, a następnie kliknij utworzony klaster.

3. W obszarze **Szybkie łącza** kliknij pozycję **Pulpity nawigacyjne klastra**, a następnie pozycję **Notes Jupyter**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klastra.

   ![Otwieranie notesu programu Jupyter w celu uruchomienia interakcyjnego zapytania Spark SQL](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Otwieranie notesu programu Jupyter w celu uruchomienia interakcyjnego zapytania Spark SQL")

   > [!NOTE]
   > Dostęp do notesu programu Jupyter dla klastra można również uzyskać, otwierając następujący adres URL w przeglądarce. Zastąp ciąg **CLUSTERNAME** nazwą klastra:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Utwórz notes. Kliknij opcję **New** (Nowy), a następnie kliknij pozycję **PySpark**.

   ![Tworzenie notesu programu Jupyter w celu uruchomienia interakcyjnego zapytania Spark SQL](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Tworzenie notesu programu Jupyter w celu uruchomienia interakcyjnego zapytania Spark SQL")

   Zostanie utworzony i otwarty nowy notes o nazwie Untitled (Untitled.pynb).

4. Jeśli chcesz, kliknij nazwę notesu u góry, a następnie wprowadź przyjazną nazwę.

    ![Podawanie nazwy dla notesu programu Jupyter, z poziomu którego będzie uruchamiane interakcyjne zapytanie Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Podawanie nazwy dla notesu programu Jupyter, z poziomu którego będzie uruchamiane interakcyjne zapytanie Spark")

5.  Wklej następujący kod do pustej komórki, a następnie naciśnij klawisze **SHIFT + ENTER**, aby go uruchomić. W poniższym kodzie wyrażenie `%%sql` (zwane magicznym wyrażeniem SQL) informuje notes programu Jupyter o konieczności użycia ustawienia wstępnego `sqlContext` do uruchomienia zapytania programu Hive. Zapytanie pobiera pierwszych 10 wierszy z tabeli programu Hive (**hivesampletable**), która jest dostępna domyślnie na wszystkich klastrach usługi HDInsight.

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    ![Zapytanie programu Hive na platformie HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Zapytanie programu Hive na platformie HDInsight Spark")

    Aby uzyskać więcej informacji na temat magicznego wyrażenia `%%sql` i wstępnie ustawionych kontekstów, zobacz [Jupyter kernels available for an HDInsight cluster](hdinsight-apache-spark-jupyter-notebook-kernels.md) (Jądra programu Jupyter dostępne dla klastra usługi HDInsight).

    > [!NOTE]
    > Podczas każdego uruchomienia zapytania w programie Jupyter w tytule okna przeglądarki internetowej wyświetlany jest stan **(Busy)** (Zajęty) wraz z tytułem notesu. Widoczne jest także pełne kółko obok tekstu **PySpark** w prawym górnym rogu. Po zakończeniu zadania zmienia się ono w pusty okrąg.
    >
    >
    
6. Ekran powinien zostać odświeżony w celu wyświetlenia wyników zapytania.

    ![Wyniki zapytania programu Hive na platformie HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Wyniki zapytania programu Hive na platformie HDInsight Spark")

7. Po zakończeniu działania aplikacji zamknij notes w celu zwolnienia zasobów klastra. W tym celu w menu **File** (Plik) w notesie kliknij polecenie **Close and Halt** (Zamknij i zatrzymaj).

8. Jeśli planujesz wykonać poniższe kroki w późniejszym czasie, pamiętaj, aby usunąć klaster usługi HDInsight utworzony zgodnie z tym artykułem. 

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a>Następny krok 

W tym artykule omówiono sposób tworzenia klastra platformy HDInsight Spark i uruchamiania podstawowego zapytania modułu Spark SQL. Przejdź do następnego artykułu, aby dowiedzieć się, jak uruchamiać interakcyjne zapytania dotyczące przykładowych danych za pomocą klastra HDInsight Spark.

> [!div class="nextstepaction"]
>[Run interactive queries on an HDInsight Spark cluster (Uruchamianie interakcyjnych zapytań w klastrze HDInsight Spark)](hdinsight-apache-spark-load-data-run-query.md)




