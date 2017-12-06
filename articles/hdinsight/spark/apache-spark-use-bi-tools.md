---
title: "BI Spark przy użyciu narzędzi do wizualizacji danych w usłudze Azure HDInsight | Dokumentacja firmy Microsoft"
description: "Użycie narzędzi do wizualizacji danych analytics przy użyciu analizy Biznesowej programu Apache Spark w klastrach HDInsight"
keywords: "Platforma spark jest Apache spark analizy biznesowej, spark analizy biznesowej, spark wizualizację danych, analiza biznesowa"
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jgao
ms.openlocfilehash: 18f495864befafd26e7adafb5c01612222d2cfdf
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark BI z usługą Azure HDInsight przy użyciu narzędzi do wizualizacji danych

Dowiedz się, jak używać [Microsoft Power BI](http://powerbi.microsoft.com) i [Tableau](http://www.tableau.com) do wizualizacji danych w klastrze Apache Spark w usłudze Azure HDInsight.

## <a name="prerequisites"></a>Wymagania wstępne

* **Pełne [uruchamianie interakcyjnych zapytań w klastrze Spark w usłudze HDInsight](./apache-spark-load-data-run-query.md)**.
* **Power BI**: [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) i [subskrypcji wersji próbnej usługi Power BI](https://app.powerbi.com/signupredirect?pbi_source=web) (opcjonalnie).
* **TABLEAU**: [pulpitu Tableau](http://www.tableau.com/products/desktop) i [sterownika Microsoft Spark ODBC](http://go.microsoft.com/fwlink/?LinkId=616229).


## <a name="hivetable"></a>Sprawdź dane

Notesu Jupyter, który został utworzony w [poprzedniego samouczek](apache-spark-load-data-run-query.md) zawiera kod w celu utworzenia `hvac` tabeli. Ta tabela jest oparta na plik CSV dostępne na wszystkich klastrach HDInsight Spark w **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. Użyj poniższej procedury, aby sprawdzić dane.

1. Z notesu Jupyter Wklej następujący kod, a następnie naciśnij klawisz **SHIFT + ENTER**. Kod sprawdza, czy istnienie tabel.

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    Dane wyjściowe wyglądają następująco:

    ![Pokaż tabele w łączniku Spark](./media/apache-spark-use-bi-tools/show-tables.png)

    Jeśli został zamknięty przed rozpoczęciem tego samouczka notesu `hvactemptable` jest czyszczona, więc nie znajduje się w danych wyjściowych.
    Tylko Hive tabel, które są przechowywane w potrzeby magazynu metadanych (wskazywanym przez **False** w obszarze **isTemporary** kolumny) można uzyskać z narzędzi do analizy Biznesowej. W tym samouczku, możesz nawiązać **hvac** tabelę, która zostanie utworzona.

2. W pustej komórce Wklej następujący kod, a następnie naciśnij klawisz **SHIFT + ENTER**. Kod sprawdza dane w tabeli.

    ```PySpark
    %%sql
    SELECT * FROM hvac LIMIT 10
    ```

    Dane wyjściowe wyglądają następująco:

    ![Pokaż wiersze z tabeli hvac w łączniku Spark](./media/apache-spark-use-bi-tools/select-limit.png)

3. Z **pliku** menu w notesie kliknij **zamknąć i zatrzymuje**. Zamknij aby zwolnić zasoby. 















## <a name="powerbi"></a>Przy użyciu usługi Power BI

W tej sekcji umożliwiają usługi Power BI tworzenie wizualizacji, raporty i pulpity nawigacyjne na podstawie danych klastra Spark. 

### <a name="create-a-report-in-power-bi-desktop"></a>Tworzenie raportu w programie Power BI Desktop
Pierwszym krokiem w pracy z platformy Spark jest Połącz się z klastrem w programie Power BI Desktop, ładowanie danych z klastra i tworzenia podstawowych wizualizacji na podstawie tych danych.

> [!NOTE]
> Łącznik przedstawiona w tym artykule jest obecnie w przeglądzie. Podaj opinię, masz za pośrednictwem [Power BI społeczności](https://community.powerbi.com/) lokacji lub [Power BI pomysły](https://ideas.powerbi.com/forums/265200-power-bi-ideas).

1. Otwórz [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).
1. Z **Home** , kliknij pozycję **Pobierz dane**, następnie **więcej**.

    ![Pobieranie danych do programu Power BI Desktop z Apache Spark w usłudze HDInsight](./media/apache-spark-use-bi-tools/hdinsight-spark-power-bi-desktop-get-data.png "pobieranie danych do usługi Power BI z Apache Spark w usłudze BI")


2. Wprowadź `Spark` w polu wyszukiwania wybierz **Azure HDInsight Spark (Beta)**, a następnie kliknij przycisk **Connect**.

    ![Pobieranie danych do usługi Power BI z Apache Spark w usłudze BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "pobieranie danych do usługi Power BI z Apache Spark w usłudze BI")

3. Podaj adres URL klastra (w postaci `mysparkcluster.azurehdinsight.net`), wybierz pozycję **DirectQuery**, a następnie kliknij przycisk **OK**.

    Umożliwia trybie połączenie danych z platformy Spark. Jeśli używasz zapytania bezpośredniego, zmiany są uwzględniane w raportach bez odświeżania cały zestaw danych. W przypadku importowania danych należy odświeżyć zestawu danych, aby zobaczyć zmiany. Aby uzyskać więcej informacji o tym, jak i kiedy korzystać z zapytania bezpośredniego, zobacz [przy użyciu zapytania bezpośredniego w usłudze Power BI](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/). 

4. Wprowadź dane konta logowania usługi HDInsight, a następnie kliknij przycisk **Connect**. Domyślna nazwa konta jest *admin*.

5. Wybierz `hvac` tabeli, aby wyświetlić podgląd danych, a następnie kliknij **obciążenia**.

    ![Platforma Spark jest klastra nazwę użytkownika i hasło](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Spark klastra nazwę użytkownika i hasło")

    Power BI Desktop zawiera informacje niezbędne do połączenia się Spark klastra i ładowanie danych z `hvac` tabeli. Tabeli i jej kolumn są wyświetlane w **pola** okienka.  Zobacz poniższy zrzut ekranu.

6. Wizualizuj różnica między temperatury docelowych i rzeczywistego temperatury dla każdego tworzenia: 

    1. W **WIZUALIZACJE** okienku wybierz **obszaru wykresu**. 
    2. Przeciągnij **BuildingID** do **osi**i przeciągnij **ActualTemp** i **TargetTemp** pól do **wartość**.

        ![Tworzenie wizualizacji danych przy użyciu Apache Spark w usłudze BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "wizualizacje danych utworzyć Spark przy użyciu Apache Spark w usłudze BI")

        Diagram wygląda następująco:

        ![Tworzenie wizualizacji danych przy użyciu Apache Spark w usłudze BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "wizualizacje danych utworzyć Spark przy użyciu Apache Spark w usłudze BI")

        Domyślnie wizualizację zawiera sumę dla **ActualTemp** i **TargetTemp**. Kliknij strzałkę w dół obok pozycji **ActualTemp** i **TragetTemp** w okienku wizualizacje widać **suma** jest zaznaczone.

    3. Kliknij strzałkę w dół obok pozycji **ActualTemp** i **TragetTemp** wybierz w okienku wizualizacje **średni** uzyskanie średniej rzeczywistego i temperatury docelowy dla każdego Tworzenie.

        ![Tworzenie wizualizacji danych przy użyciu Apache Spark w usłudze BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "wizualizacje danych utworzyć Spark przy użyciu Apache Spark w usłudze BI")

        Twoje wizualizacji danych jest podobny do przedstawionego na zrzucie ekranu. Przesuń kursor nad wizualizacji uzyskanie etykietki narzędzi z właściwymi danymi.

        ![Tworzenie wizualizacji danych przy użyciu Apache Spark w usłudze BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph-sum.png "wizualizacje danych utworzyć Spark przy użyciu Apache Spark w usłudze BI")

7. Kliknij przycisk **pliku** następnie **zapisać**, a następnie wprowadź nazwę `BuildingTemperature.pbix` pliku. 

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Publikowanie raportu z usługą Power BI (opcjonalnie)

Usługi Power BI pozwala na udostępnianie raporty i pulpity nawigacyjne w Twojej organizacji. W tej sekcji należy najpierw opublikować zestawu danych i raportu. Następnie można przypiąć raport do pulpitu nawigacyjnego. Pulpity nawigacyjne są zwykle używane do skupić się na podzbiór danych w raporcie; masz tylko jedną wizualizacji w raporcie, ale nadal jest użyteczne kroków.

1. Otwórz program Power BI Desktop.
2. Z **Home** , kliknij pozycję **publikowania**.

    ![Publikowanie z Power BI Desktop](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "publikowanie z Power BI Desktop")

2. Wybierz obszar roboczy, aby opublikować swojego zestawu danych i raport do, a następnie kliknij przycisk **wybierz**. Na poniższej ilustracji, domyślnie **Mój obszar roboczy** jest zaznaczone.

    ![Wybierz obszar roboczy, aby opublikować zestawu danych i raportów do](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "wybierz obszar roboczy do publikowania zestawu danych i raportów do") 

3. Po publikowanie zakończyło się pomyślnie, kliknij przycisk **Otwórz "BuildingTemperature.pbix" w usłudze Power BI**.

    ![Publikowanie sukces, kliknij, aby wprowadzić poświadczenia](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "opublikować sukces, kliknij, aby wprowadzić poświadczenia") 

4. W usłudze Power BI, kliknij przycisk **wprowadź poświadczenia**.

    ![Wprowadź poświadczenia w usłudze Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "wprowadzić poświadczenia w usłudze Power BI")

5. Kliknij przycisk **Edycja poświadczeń**.

    ![Edytowanie poświadczeń w usłudze Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "Edycja poświadczeń w usłudze Power BI")

6. Wprowadź dane konta logowania usługi HDInsight, a następnie kliknij przycisk **Zaloguj**. Domyślna nazwa konta jest *admin*.

    ![Zaloguj się do klastra Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "Zaloguj się do klastra Spark")

7. W okienku po lewej stronie, przejdź do **obszarów roboczych** > **Mój obszar roboczy** > **raporty**, następnie kliknij przycisk **BuildingTemperature**.

    ![Raport na liście raportów w lewym okienku](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "raportu na liście raportów w okienku po lewej stronie")

    Powinno również zostać wyświetlone **BuildingTemperature** kategorii **zestawów danych** w okienku po lewej stronie.

    Element wizualny, który utworzono w programie Power BI Desktop jest teraz dostępna w usłudze Power BI. 

8. Umieść kursor nad wizualizacji, a następnie kliknij ikonę przypinania w prawym górnym rogu.

    ![Raport w usłudze Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "raportu w usłudze Power BI")

9. Wybierz pozycję "Nowy pulpit nawigacyjny", wprowadź nazwę `Building temperature`, następnie kliknij przycisk **numeru Pin**.

    ![Przypnij do pulpitu nawigacyjnego nowy](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "Przypnij do pulpitu nawigacyjnego nowy")

10. W raporcie, kliknij przycisk **przejdź do pulpitu nawigacyjnego**. 

Wizualny jest przypięta do pulpitu nawigacyjnego — możesz dodawać innych elementach wizualnych do raportu i przypiąć je do tej samej pulpitu nawigacyjnego. Aby uzyskać więcej informacji o raporty i pulpity nawigacyjne, zobacz [raporty w usłudze Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-reports/)i [pulpitów nawigacyjnych w usłudze Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="tableau"></a>Za pomocą pulpitu Tableau 

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
7. Domyślnie są wyświetlane pola temperatury jako agregacji. Jeśli chcesz wyświetlić średnia temperatura zamiast tego, możesz to zrobić z listy rozwijanej, jak pokazano na poniższym zrzucie ekranu.

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

