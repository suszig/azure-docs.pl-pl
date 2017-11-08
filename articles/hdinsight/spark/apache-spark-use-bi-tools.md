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
ms.date: 10/24/2017
ms.author: nitinme
ms.openlocfilehash: 3886923639be8a7bd8167f10db503d7ebf8c1657
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark BI z usługą Azure HDInsight przy użyciu narzędzi do wizualizacji danych

Dowiedz się, jak używać usługi Power BI i Tableau do wizualizacji danych w klastrze Apache Spark w usłudze Azure HDInsight.

## <a name="prerequisites"></a>Wymagania wstępne

* Klaster Apache Spark w usłudze HDInsight. Aby uzyskać instrukcje, zobacz [klastrów utworzyć Apache Spark w usłudze Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* Przykładowe dane w klastrze. Aby uzyskać instrukcje, zobacz [uruchamianie interakcyjnych zapytań w klastrze Spark w usłudze HDInsight](apache-spark-load-data-run-query.md).
* Usługa Power BI: [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) i [subskrypcji wersji próbnej usługi Power BI](https://app.powerbi.com/signupredirect?pbi_source=web) (opcjonalnie).
* TABLEAU: [pulpitu Tableau](http://www.tableau.com/products/desktop) i [sterownika Microsoft Spark ODBC](http://go.microsoft.com/fwlink/?LinkId=616229).


## <a name="hivetable"></a>Przeglądanie przykładowych danych

Notesu Jupyter, który został utworzony w [poprzedniego samouczek](apache-spark-load-data-run-query.md) zawiera kod w celu utworzenia `hvac` tabeli. Ta tabela jest oparta na plik CSV dostępne na wszystkich klastrach HDInsight Spark w **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. Teraz Sprawdź dane w klastrze Spark przed utworzeniem wizualizacji.

1. Sprawdź, czy istnieją oczekiwanego tabel. W pustej komórce w notesie, skopiuj poniższy fragment kodu i naciśnij klawisz **SHIFT + ENTER**.

        %%sql
        SHOW TABLES

    Zostaną wyświetlone dane wyjściowe podobne do przedstawionego poniżej:

    ![Pokaż tabele w łączniku Spark](./media/apache-spark-use-bi-tools/show-tables.png)

    Jeśli został zamknięty przed rozpoczęciem tego samouczka notesu `hvactemptable` jest czyszczona, więc nie znajduje się w danych wyjściowych.
    Tylko hive tabel, które są przechowywane w potrzeby magazynu metadanych (wskazywanym przez **False** w obszarze **isTemporary** kolumny) można uzyskać z narzędzi do analizy Biznesowej. W tym samouczku połączyć się z **hvac** utworzyliśmy tabeli.

2. Sprawdź, czy tabela zawiera oczekiwane dane. W pustej komórce w notesie, skopiuj poniższy fragment kodu i naciśnij klawisz **SHIFT + ENTER**.

        %%sql
        SELECT * FROM hvac LIMIT 10

    Zostaną wyświetlone dane wyjściowe podobne do przedstawionego poniżej:

    ![Pokaż wiersze z tabeli hvac w łączniku Spark](./media/apache-spark-use-bi-tools/select-limit.png)

3. Zamknij aby zwolnić zasoby. W tym celu w menu **File** (Plik) w notesie kliknij polecenie **Close and Halt** (Zamknij i zatrzymaj).

## <a name="powerbi"></a>Użyj usługi Power BI dla Spark wizualizacji danych

Skoro upewnieniu się, że istnieje oczekiwane dane, można użyć usługi Power BI do utworzenia wizualizacji, raporty i pulpity nawigacyjne z danych. W tym artykule firma Microsoft będzie wyświetlać prosty przykład z danymi statycznymi, ale prosimy o kontakt w komentarzach Jeśli chcesz zobaczyć bardziej złożonych przykłady przesyłania strumieniowego.

### <a name="create-a-report-in-power-bi-desktop"></a>Tworzenie raportu w programie Power BI Desktop
Pierwszym krokiem w pracy z platformy Spark jest Połącz się z klastrem w programie Power BI Desktop, ładowanie danych z klastra i tworzenia podstawowych wizualizacji na podstawie tych danych.

> [!NOTE]
> Łącznik przedstawiona w tym artykule jest obecnie w przeglądzie, ale firma Microsoft zachęca do go użyć i podaj opinię, masz za pośrednictwem [Power BI społeczności](https://community.powerbi.com/) lokacji lub [Power BI pomysły](https://ideas.powerbi.com/forums/265200-power-bi-ideas).

1. W programie Power BI Desktop na **Home** , kliknij pozycję **Pobierz dane**, następnie **więcej**.

2. Wyszukaj `Spark`, wybierz pozycję **Azure HDInsight Spark**, następnie kliknij przycisk **Connect**.

    ![Pobieranie danych do usługi Power BI z Apache Spark w usłudze BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "pobieranie danych do usługi Power BI z Apache Spark w usłudze BI")

3. Podaj adres URL klastra (w postaci `mysparkcluster.azurehdinsight.net`), wybierz pozycję **DirectQuery**, następnie kliknij przycisk **OK**.

    ![Nawiązania połączenia platformy Apache Spark w usłudze BI](./media/apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "nawiązać Apache Spark analizy Biznesowej")

    > [!NOTE]
    > Można albo tryb łączności z platformy Spark. Jeśli używasz zapytania bezpośredniego, zmiany są uwzględniane w raportach bez odświeżania cały zestaw danych. W przypadku importowania danych należy odświeżyć zestawu danych, aby zobaczyć zmiany. Aby uzyskać więcej informacji o tym, jak i kiedy korzystać z zapytania bezpośredniego, zobacz [przy użyciu zapytania bezpośredniego w usłudze Power BI](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/). 

4. Wprowadź informacje o koncie logowania HDInsight dla **nazwy użytkownika** i **hasło** (jest domyślnym kontem `admin`), następnie kliknij przycisk **Connect**.

    ![Platforma Spark jest klastra nazwę użytkownika i hasło](./media/apache-spark-use-bi-tools/user-password.png "Spark klastra nazwę użytkownika i hasło")

5. Wybierz `hvac` tabeli i poczekaj, aby wyświetlić podgląd danych. Następnie kliknij przycisk **obciążenia**.

    ![Platforma Spark jest klastra nazwę użytkownika i hasło](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Spark klastra nazwę użytkownika i hasło")

    Power BI Desktop ma teraz wszystkie informacje niezbędne do połączenia się Spark klastra i ładowanie danych z `hvac` tabeli. Tabeli i jej kolumn są wyświetlane w **pola** okienka.

    ![Lista tabel na pulpicie nawigacyjnym Apache Spark w usłudze BI](./media/apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "listy tabel na pulpicie nawigacyjnym Apache Spark w usłudze BI")

7. Tworzenie wizualizacji, aby pokazać różnica między temperatury docelowych i rzeczywistego temperatury dla każdego tworzenia: 

    1. W **WIZUALIZACJE** okienku wybierz **obszaru wykresu**. Przeciągnij **BuildingID** do **osi**i przeciągnij **ActualTemp** i **TargetTemp** pól do **wartość**.

        ![Tworzenie wizualizacji danych przy użyciu Apache Spark w usłudze BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "wizualizacje danych utworzyć Spark przy użyciu Apache Spark w usłudze BI")

    2. Domyślnie wizualizację zawiera sumę dla **ActualTemp** i **TargetTemp**. Dla obu tych pól, z listy rozwijanej wybierz **średni** uzyskanie średniej rzeczywistego i temperatury docelowy dla każdego tworzenia.

        ![Tworzenie wizualizacji danych przy użyciu Apache Spark w usłudze BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "wizualizacje danych utworzyć Spark przy użyciu Apache Spark w usłudze BI")

    3. Twoje wizualizacji danych powinna być podobny do przedstawionego na zrzucie ekranu. Przesuń kursor nad wizualizacji uzyskanie etykietki narzędzi z właściwymi danymi.

        ![Tworzenie wizualizacji danych przy użyciu Apache Spark w usłudze BI Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "wizualizacje danych utworzyć Spark przy użyciu Apache Spark w usłudze BI")

11. Kliknij przycisk **pliku** następnie **zapisać**, a następnie wprowadź nazwę `spark.pbix` pliku. 

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Publikowanie raportu z usługą Power BI (opcjonalnie)
Masz teraz raport funkcjonalnej w programie Power BI Desktop, można zatrzymać istnieje, a wiele osób chce korzystać z usługi Power BI, co ułatwia udostępnianie raporty i pulpity nawigacyjne w Twojej organizacji. 

W tej sekcji należy opublikować zestawu danych i raportu, który jest zawarty w pliku Power BI Desktop, który został utworzony. Można następnie numeru pin wizualizacji z raportu do pulpitu nawigacyjnego. Pulpity nawigacyjne są zwykle używane do skupić się na podzbiór danych w raporcie; masz tylko jedną wizualizacji w raporcie, ale nadal jest użyteczne kroków.

1. W programie Power BI Desktop na **Home** , kliknij pozycję **publikowania**.

    ![Publikowanie z Power BI Desktop](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "publikowanie z Power BI Desktop")

2. Wybierz obszar roboczy, aby opublikować swojego zestawu danych i raport do, a następnie kliknij przycisk **wybierz**. Na poniższej ilustracji, domyślnie **Mój obszar roboczy** jest zaznaczone.

    ![Wybierz obszar roboczy, aby opublikować zestawu danych i raportów do](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "wybierz obszar roboczy do publikowania zestawu danych i raportów do") 

3. Po Power BI Desktop zwraca komunikat z potwierdzeniem, kliknij przycisk **Otwórz "spark.pbix" w usłudze Power BI**.

    ![Publikowanie sukces, kliknij, aby wprowadzić poświadczenia](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "opublikować sukces, kliknij, aby wprowadzić poświadczenia") 

4. W usłudze Power BI, kliknij przycisk **wprowadź poświadczenia**.

    ![Wprowadź poświadczenia w usłudze Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "wprowadzić poświadczenia w usłudze Power BI")

5. Kliknij przycisk **Edycja poświadczeń**.

    ![Edytowanie poświadczeń w usłudze Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "Edycja poświadczeń w usłudze Power BI")

6. Wprowadź dane konta logowania HDInsight (zazwyczaj domyślne `admin` konto używane w programie Power BI Desktop), następnie kliknij przycisk **Zaloguj**.

    ![Zaloguj się do klastra Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "Zaloguj się do klastra Spark")

7. W okienku po lewej stronie, przejdź do **obszarów roboczych** > **Mój obszar roboczy** > **raporty**, następnie kliknij przycisk **spark**.

    ![Raport na liście raportów w lewym okienku](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "raportu na liście raportów w okienku po lewej stronie")

    Powinno również zostać wyświetlone **spark** kategorii **zestawów danych** w okienku po lewej stronie.

8. Element wizualny, który utworzono w programie Power BI Desktop jest teraz dostępna w usłudze. Aby przypiąć ten element wizualny na pulpicie nawigacyjnym, umieść kursor nad element wizualny, a następnie kliknij ikonę numeru pin.

    ![Raport w usłudze Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "raportu w usłudze Power BI")

9. Wybierz pozycję "Nowy pulpit nawigacyjny", wprowadź nazwę `SparkDemo`, następnie kliknij przycisk **numeru Pin**.

    ![Przypnij do pulpitu nawigacyjnego nowy](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "Przypnij do pulpitu nawigacyjnego nowy")

10. W raporcie, kliknij przycisk **przejdź do pulpitu nawigacyjnego**. 

    ![Przejdź do pulpitu nawigacyjnego](./media/apache-spark-use-bi-tools/apache-spark-bi-open-dashboard.png "przejdź do pulpitu nawigacyjnego")

Wizualny jest przypięta do pulpitu nawigacyjnego — możesz dodawać innych elementach wizualnych do raportu i przypiąć je do tej samej pulpitu nawigacyjnego. Aby uzyskać więcej informacji o raporty i pulpity nawigacyjne, zobacz [raporty w usłudze Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-reports/)i [pulpitów nawigacyjnych w usłudze Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

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

