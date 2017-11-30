---
title: "Uruchamianie interakcyjnych zapytań w klastrze Spark w usłudze HDInsight Azure | Dokumentacja firmy Microsoft"
description: "Przewodnik Szybki start platformy HDInsight Spark opisujący sposób tworzenia klastra platformy Apache Spark w usłudze HDInsight."
keywords: spark quickstart,interactive spark,interactive query,hdinsight spark,azure spark
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: jgao
ms.openlocfilehash: db8f0056fa3813e95c2c5bea583d7b66ac64260f
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="run-interactive-queries-on-an-hdinsight-spark-cluster"></a>Uruchamianie interakcyjnych zapytań w klastrze Spark w usłudze HDInsight

W tym artykule należy użyć notesu Jupyter do uruchamiania interakcyjnych zapytań Spark SQL w odniesieniu do klastra Spark. Notesu Jupyter jest oparty na przeglądarce aplikacji, rozszerzający interaktywna opartych na konsoli sieci Web. Aby uzyskać więcej informacji, zobacz [notesu Jupyter](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).

W tym samouczku, użyj **PySpark** jądra notesu Jupyter do uruchamiania interaktywnego zapytań Spark SQL. Notesów Jupyter w klastrach HDInsight obsługuje również dwa inne jądra - **PySpark3** i **Spark**. Aby uzyskać więcej informacji na temat jądra i korzyści wynikające ze stosowania **PySpark**, zobacz [klastrów jądra notesu Jupyter do użycia z platformy Apache Spark w usłudze HDInsight](apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Wymagania wstępne

* **Klaster usługi Azure HDInsight Spark**. Aby uzyskać instrukcje, zobacz [utworzyć klaster Apache Spark w usłudze Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-to-run-interactive-queries"></a>Tworzenie notesu Jupyter do wykonywania interakcyjnych zapytań

Aby uruchamiać kwerendy, używamy przykładowych danych, które jest domyślnie dostępna w magazynie skojarzony z klastrem. Jednak należy najpierw załadować dane do Spark jako dataframe. Po utworzeniu dataframe, można wykonać zapytania na nim za pomocą notesu Jupyter. W tym artykule należy przyjrzeć się jak:

* Rejestrowanie zestawu danych przykładowych jako Spark dataframe.
* Uruchamianie kwerend dotyczących dataframe.

Zacznijmy od początku.

1. Otwórz [portal Azure](https://portal.azure.com/). Jeśli wybrana została opcja przypięcia klastra do pulpitu nawigacyjnego, kliknij kafelek klastra na pulpicie nawigacyjnym, aby uruchomić blok klastra.

    Jeśli klaster nie został przypięty do pulpitu nawigacyjnego, w okienku po lewej kliknij pozycję **Klastry HDInsight**, a następnie kliknij utworzony klaster.

3. W obszarze **Szybkie łącza** kliknij pozycję **Pulpity nawigacyjne klastra**, a następnie pozycję **Notes Jupyter**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klastra.

   ![Otwieranie notesu programu Jupyter w celu uruchomienia interakcyjnego zapytania Spark SQL](./media/apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "Otwieranie notesu programu Jupyter w celu uruchomienia interakcyjnego zapytania Spark SQL")

   > [!NOTE]
   > Dostęp do notesu programu Jupyter dla klastra można również uzyskać, otwierając następujący adres URL w przeglądarce. Zastąp ciąg **CLUSTERNAME** nazwą klastra:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Utwórz notes. Kliknij opcję **New** (Nowy), a następnie kliknij pozycję **PySpark**.

   ![Tworzenie notesu programu Jupyter w celu uruchomienia interakcyjnego zapytania Spark SQL](./media/apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Tworzenie notesu programu Jupyter w celu uruchomienia interakcyjnego zapytania Spark SQL")

   Zostanie utworzony i otwarty nowy notes o nazwie Untitled (Untitled.pynb).

4. Jeśli chcesz, kliknij nazwę notesu u góry, a następnie wprowadź przyjazną nazwę.

    ![Podawanie nazwy dla notesu programu Jupyter, z poziomu którego będzie uruchamiane interakcyjne zapytanie Spark](./media/apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "Podawanie nazwy dla notesu programu Jupyter, z poziomu którego będzie uruchamiane interakcyjne zapytanie Spark")

5. Wklej następujący kod do pustej komórki, a następnie naciśnij klawisze **SHIFT + ENTER**, aby go uruchomić. Kod importuje typy wymagane w tym scenariuszu:

        from pyspark.sql import *
        from pyspark.sql.types import *

    Ponieważ notes został utworzony z użyciem jądra PySpark, nie ma konieczności jawnego tworzenia kontekstów. Konteksty Spark i Hive są automatycznie tworzone po uruchomieniu pierwszej komórki kodu.

    ![Stan interakcyjnego zapytania Spark SQL](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Stan interakcyjnego zapytania Spark SQL")

    Przy każdym uruchomieniu interakcyjnego zapytania w programie Jupyter w tytule okna przeglądarki internatowej wyświetlany jest stan **(Busy)** (Zajęty) wraz z tytułem notesu. Widoczne jest także pełne kółko obok tekstu **PySpark** w prawym górnym rogu. Po zakończeniu zadania zmienia się ono w pusty okrąg.

6. Przed załadowaniem danych do klastra Spark NAS Szukaj migawkę go. Przykładowe dane używane w tym samouczku jest dostępna jako plik CSV we wszystkich klastrach HDInsight Spark w **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. Przechwytywanie danych zmiany temperatury w budynku. Poniżej przedstawiono kilka pierwszych wierszy danych.

    ![Migawki danych do interaktywnego zapytań Spark SQL](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "migawki danych do interaktywnego zapytań Spark SQL")

6. Tworzenie dataframe i tabeli tymczasowej (**hvac**), uruchamiając poniższy kod. W tym samouczku firma Microsoft nie twórz wszystkie kolumny w pliku CSV. 

        # Create an RDD from sample data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse the data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))
        
        # Infer the schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. Po utworzeniu tabeli, uruchom interakcyjne zapytania na danych, należy użyć poniższego kodu.

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   Ponieważ używane jest jądro PySpark, można teraz bezpośrednio uruchomić interakcyjne zapytanie SQL w tabeli tymczasowej **hvac** utworzonej za pomocą polecenia magicznego `%%sql`. Aby uzyskać więcej informacji na temat polecenia magicznego `%%sql` oraz innych poleceń magicznych dostępnych za pośrednictwem jądra PySpark, zobacz [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   Domyślnie jest wyświetlana następująca tabela danych wyjściowych.

     ![Tabela wyjściowa wyników interakcyjnego zapytania Spark](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Tabela wyjściowa wyników interakcyjnego zapytania Spark")

9. Wyniki można również przeglądać w postaci innych wizualizacji. Aby wyświetlić wykres warstwowy tych samych danych wyjściowych, wybierz **obszaru** następnie ustaw inne wartości, jak pokazano.

    ![Wykres warstwowy wyników interakcyjnego zapytania Spark](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Wykres warstwowy wyników interakcyjnego zapytania Spark")

10. Z **pliku** menu w notesie kliknij **zapisywanie i punktu kontrolnego**. 

11. Jeśli zaczynasz [następny samouczek](apache-spark-use-bi-tools.md) teraz, pozostaw otwarte notesu. Jeśli nie, należy wyłączyć aby zwolnić zasoby klastra: z **pliku** menu w notesie kliknij **zamknąć i zatrzymuje**.

## <a name="next-step"></a>Następny krok

W tym artykule przedstawiono sposób wykonywania interakcyjnych zapytań w łączniku Spark przy użyciu notesu Jupyter. Przejdź do następnego artykuł, aby zobaczyć, jak dane, które są zarejestrowane w Spark można ściągnąć do narzędzia analytics analizy Biznesowej, takich jak Power BI i Tableau. 

> [!div class="nextstepaction"]
>[Platforma Spark BI z usługą Azure HDInsight przy użyciu narzędzi do wizualizacji danych](apache-spark-use-bi-tools.md)




