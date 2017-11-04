---
title: "BI Spark przy użyciu narzędzi do wizualizacji danych w usłudze Azure HDInsight | Dokumentacja firmy Microsoft"
description: "Użycie narzędzi do wizualizacji danych analytics przy użyciu analizy Biznesowej programu Apache Spark w klastrach HDInsight"
keywords: "Platforma spark jest Apache spark analizy biznesowej, spark analizy biznesowej, spark wizualizację danych, analiza biznesowa"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: d1d5405a635b9f990f53b2bf32c8270a71a0f344
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark BI z usługą Azure HDInsight przy użyciu narzędzi do wizualizacji danych

Dowiedz się, jak używać narzędzi wizualizacji danych, takich jak Power BI i Tableau do analizowania zestawu danych pierwotnych próbki przy użyciu Apache Spark w usłudze BI w klastrach usługi HDInsight.

> [!NOTE]
> Łączność z narzędzi do analizy Biznesowej opisane w tym artykule nie jest obsługiwana na 2.1 Spark on Azure HDInsight 3,6 Preview. Tylko Spark w wersji 1.6 i 2.0 (HDInsight 3.4, 3.5 odpowiednio) są obsługiwane.
>

W tym samouczku jest również dostępny jako notesu Jupyter w klastrze Spark w usłudze HDInsight. Środowisko notesu umożliwia uruchamianie fragmenty kodu języka Python z notesu samej siebie. Aby wykonać samouczek z poziomu Notes, tworzenie klastra Spark, uruchom notesu Jupyter (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), a następnie uruchom notesu **narzędzi do analizy Biznesowej użycia z platformy Apache Spark w HDInsight.ipynb** w obszarze **Python** folderu.

## <a name="prerequisites"></a>Wymagania wstępne

* Klaster Apache Spark w usłudze HDInsight. Aby uzyskać instrukcje, zobacz [klastrów utworzyć Apache Spark w usłudze Azure HDInsight](apache-spark-jupyter-spark-sql.md).


## <a name="hivetable"></a>Przygotowanie danych do wizualizacji danych Spark

W tej sekcji użyjesz [Jupyter](https://jupyter.org) notesu z klastra Spark w usłudze HDInsight do uruchomienia zadań, które przetwarzają przykładowe nieprzetworzone dane i zapisz go jako tabelę. Dane przykładowe są plik CSV (hvac.csv) dostępne na wszystkich klastrach domyślnie. Gdy dane są zapisywane jako tabelę, w następnej sekcji używamy narzędzi do analizy Biznesowej Aby podłączyć się do tabeli i wykonać wizualizacje danych.

> [!NOTE]
> Jeśli kroki są wykonywane w tym artykule po ukończeniu instrukcjami [uruchamianie interakcyjnych zapytań w klastrze Spark w usłudze HDInsight](apache-spark-load-data-run-query.md), możesz przejść do kroku 8 poniżej.
>

1. W witrynie [Azure Portal](https://portal.azure.com/) na tablicy startowej kliknij kafelek klastra Spark (jeśli został przypięty do tablicy startowej). Możesz także przejść do klastra, wybierając polecenia **Przeglądaj wszystko** > **Klastry usługi HDInsight**.   

2. W bloku klastra Spark kliknij pozycję **Pulpit nawigacyjny klastra**, a następnie opcję **Jupyter Notebook**. Jeśli zostanie wyświetlony monit, wprowadź poświadczenia administratora klastra.

   > [!NOTE]
   > Można również przejść do aplikacji Jupyter Notebook dla klastra, otwierając następujący adres URL w przeglądarce. Zastąp ciąg **CLUSTERNAME** nazwą klastra:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Utwórz notes. Kliknij opcję **New** (Nowy), a następnie kliknij pozycję **PySpark**.

    ![Tworzenie notesu Jupyter dla Apache Spark w usłudze BI](./media/apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "tworzenie notesu Jupyter do analizy Biznesowej programu Apache Spark")

4. Zostanie utworzony i otwarty nowy notes o nazwie Untitled.pynb. Kliknij nazwę notesu u góry, a następnie wprowadź przyjazną nazwę.

    ![Podaj nazwę notesu dla Apache Spark w usłudze BI](./media/apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "podanie nazwy notesu Apache Spark w usłudze BI")

5. Ponieważ notes został utworzony z użyciem jądra PySpark, nie ma konieczności jawnego tworzenia kontekstów. Konteksty Spark i Hive są automatycznie tworzone po uruchomieniu pierwszej komórki kodu. Możesz zacząć od importowania typów wymaganych w tym scenariuszu. Aby to zrobić, umieść kursor w komórce i naciśnij klawisze **SHIFT + ENTER**.

        from pyspark.sql import *

6. Załaduj przykładowe dane do tabeli tymczasowej. Podczas tworzenia klastra Spark w usłudze HDInsight przykładowy plik danych **hvac.csv** jest kopiowany do skojarzonego konta magazynu pod adresem **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    W pustej komórce Wklej następujący fragment kodu i naciśnij klawisz **SHIFT + ENTER**. Ta Wstawka kodu rejestruje dane w tabeli o nazwie **hvac**.

        # Create an RDD from sample data
        hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

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

7. Sprawdź, czy tabela została pomyślnie utworzona. Można użyć `%%sql` magic do uruchomienia Hive wysyła zapytanie bezpośrednio. Aby uzyskać więcej informacji na temat polecenia magicznego `%%sql` oraz innych poleceń magicznych dostępnych za pośrednictwem jądra PySpark, zobacz [Jądra dostępne dla notesu Jupyter w klastrze Spark w usłudze HDInsight](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SHOW TABLES

    Pojawić się dane wyjściowe, jak pokazano poniżej:

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true        |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    Tylko tabele, które mają wartość false w obszarze **isTemporary** kolumny są tabele programu hive są przechowywane w potrzeby magazynu metadanych, które są dostępne z narzędzi do analizy Biznesowej. W tym samouczku połączyć się z **hvac** utworzyliśmy tabeli.

8. Sprawdź, czy tabela zawiera dane przeznaczone. W pustej komórce w notesie, skopiuj poniższy fragment kodu i naciśnij klawisz **SHIFT + ENTER**.

        %%sql
        SELECT * FROM hvac LIMIT 10

9. Zamknij aby zwolnić zasoby. W tym celu w menu **File** (Plik) w notesie kliknij polecenie **Close and Halt** (Zamknij i zatrzymaj).

## <a name="powerbi"></a>Użyj usługi Power BI dla Spark wizualizacji danych

> [!NOTE]
> Ta sekcja ma zastosowanie tylko do wersji 1.6 Spark w usłudze HDInsight w wersji 3.4 i 2.0 Spark w usłudze HDInsight 3.5.
>
>

Po zapisaniu dane jako tabelę, można użyć usługi Power BI, aby podłączyć się do danych i wizualizacji, aby utworzyć raporty, pulpity nawigacyjne,... itd.

1. Upewnij się, że masz dostęp do usługi Power BI. Możesz uzyskać bezpłatne sprawdzenie subskrypcji usługi Power BI z [http://www.powerbi.com/](http://www.powerbi.com/).

2. Zaloguj się do [Power BI](http://www.powerbi.com/).

3. W dolnej części okienka po lewej stronie, kliknij przycisk **Pobierz dane**.

4. Na **Pobierz dane** w obszarze **importu lub Połącz z danymi**, dla **baz danych**, kliknij przycisk **uzyskać**.

    ![Pobieranie danych do usługi Power BI dla Apache Spark w usłudze BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "pobieranie danych do usługi Power BI dla Apache Spark w usłudze BI")

5. Na następnym ekranie kliknij **Spark w usłudze Azure HDInsight** , a następnie kliknij przycisk **Connect**. Po wyświetleniu monitu wprowadź adres URL klastra (`mysparkcluster.azurehdinsight.net`) oraz poświadczenia, aby połączyć się z klastrem.

    ![Nawiązania połączenia platformy Apache Spark w usłudze BI](./media/apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "nawiązać Apache Spark analizy Biznesowej")

    Po nawiązaniu połączenia usługi Power BI uruchamia importowania danych z klastra Spark w usłudze HDInsight.

6. Usługa Power BI importuje dane i dodaje **Spark** zestawu danych w obszarze **zestawów danych** nagłówka. Kliknij zestaw danych, aby otworzyć nowy arkusz do wizualizacji danych. Można także zapisać arkusza jako raport. Aby zapisać arkusza z **pliku** menu, kliknij przycisk **Zapisz**.

    ![Apache Spark w usłudze BI kafelka pulpitu nawigacyjnego usługi Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark w usłudze BI kafelka pulpitu nawigacyjnego usługi Power BI")
7. Zwróć uwagę, że **pola** listy na liście prawym **hvac** tabeli utworzony wcześniej. Rozwiń tabeli, aby wyświetlić pola w tabeli, zgodnie z definicją w notesie wcześniej.

      ![Lista tabel na pulpicie nawigacyjnym Apache Spark w usłudze BI](./media/apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "listy tabel na pulpicie nawigacyjnym Apache Spark w usłudze BI")

8. Tworzenie wizualizacji, aby pokazać różnica między temperatury docelowych i rzeczywistego temperatury dla każdego tworzenia. W celu wizualizacji danych, wybierz **wykres warstwowy** (pokazywaną w czerwonym prostokątem). Aby zdefiniować oś, przeciągnij i upuść **BuildingID** pole w obszarze **osi**, i **ActualTemp**/**TargetTemp** pól w obszarze **wartość**.

    ![Tworzenie wizualizacji danych przy użyciu Apache Spark w usłudze BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "wizualizacje danych utworzyć Spark przy użyciu Apache Spark w usłudze BI")

9. Domyślnie wizualizację zawiera sumę dla **ActualTemp** i **TargetTemp**. Zarówno w przypadku pól z listy rozwijanej wybierz **średni** można uzyskać zarówno budynków średniej rzeczywistego i temperatury docelowej.

    ![Tworzenie wizualizacji danych przy użyciu Apache Spark w usłudze BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "wizualizacje danych utworzyć Spark przy użyciu Apache Spark w usłudze BI")

10. Twoje wizualizacji danych powinna być podobny do przedstawionego na zrzucie ekranu. Przesuń kursor nad wizualizacji uzyskanie etykietki narzędzi z właściwymi danymi.

    ![Tworzenie wizualizacji danych przy użyciu Apache Spark w usłudze BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "wizualizacje danych utworzyć Spark przy użyciu Apache Spark w usłudze BI")

11. Kliknij przycisk **zapisać** z górnego menu i podaj nazwę raportu. Możesz również przypiąć element wizualny. Przypnij wizualizacje, są przechowywane na pulpicie nawigacyjnym, więc można śledzić najpóźniejszą wartość jeden rzut oka.

   Możesz dodać dowolną liczbę wizualizacji do tego samego zestawu danych a przypiąć je do pulpitu nawigacyjnego dla migawki danych. Ponadto klastry Spark w usłudze HDInsight są podłączone do usługi Power BI bezpośrednio połączenia. Daje to pewność, że usługi Power BI zawsze ma najbardziej aktualne dane z klastra, dzięki czemu nie trzeba zaplanować odświeżania dla zestawu danych.

## <a name="tableau"></a>Za pomocą pulpitu Tableau dla Spark wizualizacji danych

> [!NOTE]
> Ta sekcja dotyczy tylko klastry Spark 1.5.2 utworzone w usłudze Azure HDInsight.
>
>

1. Zainstaluj [pulpitu Tableau](http://www.tableau.com/products/desktop) na komputerze, na którym są uruchomione w tym samouczku Apache Spark w usłudze BI.

2. Upewnij się, że ten komputer ma również zainstalowanego sterownika Microsoft Spark ODBC. Można zainstalować sterownika z [tutaj](http://go.microsoft.com/fwlink/?LinkId=616229).

1. Uruchom Tableau pulpitu. W okienku po lewej stronie, z listy serwera do nawiązania połączenia, kliknij przycisk **Spark SQL**. Jeśli Spark SQL nie znajduje się domyślnie w okienku po lewej stronie, możesz go znaleźć, kliknij przycisk **więcej serwerów**.
2. W oknie dialogowym połączenia Spark SQL Podaj wartości, jak pokazano na zrzucie ekranu, a następnie kliknij przycisk **OK**.

    ![Połącz z klastrem usługi Apache Spark BI](./media/apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Połącz z klastrem usługi Apache Spark BI")

    Listy rozwijane uwierzytelniania **usługi Microsoft Azure HDInsight** opcjonalnie tylko w przypadku zainstalowania [sterownika Microsoft Spark ODBC](http://go.microsoft.com/fwlink/?LinkId=616229) na komputerze.
3. Na następnym ekranie z **schematu** listy rozwijanej kliknij **znaleźć** ikonę, a następnie kliknij przycisk **domyślne**.

    ![Znaleźć schematu dla Apache Spark w usłudze BI](./media/apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Znajdź schematu dla Apache Spark w usłudze BI")
4. Dla **tabeli** kliknij **znaleźć** ikonę ponownie, aby wyświetlić listę wszystkich tabel Hive dostępne w klastrze. Powinny pojawić się **hvac** tabeli zostały utworzone wcześniej za pomocą notesu.

    ![Znajdź tabeli dla Apache Spark w usłudze BI](./media/apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "tabeli Znajdź Apache Spark w usłudze BI")
5. Przeciągnij i upuść tabeli polu u góry po prawej stronie. TABLEAU importuje dane i wyświetla schematu jako wyróżnione przez czerwonym prostokątem.

    ![Dodawanie tabel do Tableau dla Apache Spark w usłudze BI](./media/apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "dodanie tabel Tableau Apache Spark w usłudze BI")
6. Kliknij przycisk **Sheet1 —** kartę w lewym dolnym rogu. Należy wizualizacji przedstawiający średni docelowy i rzeczywistego temperatur w budynkach wszystkie dla każdego dnia. Przeciągania **data** i **tworzenia identyfikator** do **kolumn** i **rzeczywiste Temp**/**Target Temp** Aby **wierszy**. W obszarze **znaczniki**, wybierz pozycję **obszaru** do Użyj obszaru mapy wizualizacji danych platformy Spark.

     ![Dodawanie pól do wizualizacji danych Spark](./media/apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Dodawanie pól do wizualizacji danych Spark")
7. Domyślnie są wyświetlane pola temperatury jako agregacji. Jeśli chcesz wyświetlić średnia temperatura zamiast tego, możesz to zrobić z listy rozwijanej, jak pokazano poniżej.

    ![Zająć średnia temperatura wizualizacji danych Spark](./media/apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "zająć średnia temperatura Spark wizualizacji danych")

8. Można również super skonfigurować jedną mapę temperatury za pośrednictwem innych uzyskanie lepszego działania różnica między temperatury rzeczywiste i docelowe. Przesuń wskaźnik myszy do rogu niższe obszaru mapy dopóki Zobacz kształtu uchwyt wyróżnione kolorem czerwonym kółku. Przeciągnij mapy do innych mapy u góry, a następnie zwolnij przycisk myszy, gdy pojawi się kształtu wyróżnione kolorem czerwonym prostokątem.

    ![Scal mapy do wizualizacji danych Spark](./media/apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "scalania mapy dla Spark wizualizacji danych")

     Należy zmienić sieci wizualizacji danych, jak pokazano na zrzucie ekranu:

    ![Dane wyjściowe TABLEAU do wizualizacji danych Spark](./media/apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau danych wyjściowych Spark wizualizacji danych")
9. Kliknij przycisk **zapisać** można zapisać w arkuszu. Można tworzyć pulpity nawigacyjne i dodawać do niego co najmniej jeden arkusz.

## <a name="next-steps"></a>Następne kroki

Do tej pory przedstawiono sposób tworzenia klastra, Utwórz Spark ramek danych wykonać zapytania o dane i następnie dostęp do danych z narzędzi do analizy Biznesowej. Teraz można przeglądać instrukcje dotyczące zarządzania zasobami klastra i debugowanie zadań uruchomionych w klastrze Spark w usłudze HDInsight.

* [Zarządzanie zasobami klastra Apache Spark w usłudze Azure HDInsight](apache-spark-resource-manager.md)
* [Śledzenie i debugowanie zadań uruchamianych w klastrze Apache Spark w usłudze HDInsight](apache-spark-job-debugging.md)

