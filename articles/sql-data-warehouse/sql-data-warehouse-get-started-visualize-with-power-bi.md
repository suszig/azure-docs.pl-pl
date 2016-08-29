<properties
   pageTitle="Wizualizacja danych usługi SQL Data Warehouse przy użyciu usługi Power BI | Microsoft Azure"
   description="Wizualizacja danych usługi SQL Data Warehouse przy użyciu usługi Power BI"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="" />

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/18/2016"
   ms.author="lodipalm;barbkess;sonyama" />

# Wizualizacja danych przy użyciu usługi Power BI

> [AZURE.SELECTOR]
- [Power BI][]
- [Azure Machine Learning][]

Ten samouczek przedstawia sposób nawiązania połączenia z usługą SQL Data Warehouse przy użyciu usługi Power BI oraz tworzenia podstawowych wizualizacji.

> [AZURE.NOTE] Do wykonania opisanych w nim kroków jest potrzebna baza danych usługi SQL Data Warehouse, do której wstępnie załadowano przykładową bazę danych AdventureWorksDW. Sposób jej tworzenia pokazano w artykule [Tworzenie bazy danych w usłudze SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).
>
> Jeśli masz już bazę danych usługi SQL Data Warehouse, ale bez przykładowych danych, możesz [ręcznie załadować przykładowe dane][].

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## Nawiązywanie połączenia z bazą danych AdventureWorksDW

Aby otworzyć usługę Power BI nawiązać połączenie się z bazą danych AdventureWorksDW:

1. Zaloguj się do [Portalu Azure][].
2. Kliknij pozycję **Bazy danych SQL** i wybierz bazę danych AdventureWorks usługi SQL Data Warehouse.

    ![Znajdowanie bazy danych][1]

3. Kliknij przycisk „Otwórz w usłudze Power BI”.

    ![Przycisk usługi Power BI][2]

4. Powinna zostać wyświetlona strona połączenia z usługą SQL Data Warehouse przedstawiająca adres sieci Web bazy danych. Kliknij przycisk Dalej.

    ![Połączenie z usługą Power BI][3]

6. Wprowadź nazwę użytkownika i hasło serwera Azure SQL. Zostanie nawiązane pełne połączenie z bazą danych usługi SQL Data Warehouse.

    ![Logowanie w usłudze Power BI][4]

1. Po zalogowaniu w usłudze Power BI kliknij zestaw danych AdventureWorksDW w lewym bloku. Spowoduje to otwarcie bazy danych.

    ![Otwieranie bazy danych AdventureWorksDW w usłudze Power BI][5]



## Tworzenie raportu usługi Power BI do analizowania przykładowych danych

Teraz możesz przystąpić do analizowania przykładowych danych bazy AdventureWorksDW przy użyciu usługi Power BI. Do wykonywania analizy służy widok bazy danych AdventureWorksDW o nazwie AggregateSales (Łączna sprzedaż). Ten widok zawiera kilka kluczowych metryk do analizowania sprzedaży firmy.

1. Aby utworzyć mapę kwot sprzedaży na podstawie kodów pocztowych, w okienku pól po prawej stronie kliknij widok AggregateSales (Łączna sprzedaż), aby go rozwinąć. Kliknij kolumny PostalCode (kod pocztowy) and SalesAmount (kwota sprzedaży), aby je zaznaczyć.

    ![Wybór widoku AggregateSales w usłudze Power BI][6]

    Usługa Power BI automatycznie rozpoznaje dane geograficzne i umieszcza je na mapie.

    ![Mapa w usłudze Power BI][7]

2. W tym kroku zostanie utworzony wykres słupkowy przedstawiający kwotę sprzedaży w odniesieniu do dochodu klienta. Aby go utworzyć, przejdź do rozszerzonego widoku AggregateSales (Łączna sprzedaż). Kliknij pole SalesAmount (Kwota sprzedaży). Przeciągnij pole Customer Income (Dochód klienta) w lewo i upuść go w sekcji Axis (Oś).

    ![Wybór osi w usłudze Power BI][8]

    Przenieśliśmy wykres słupkowy na lewą stronę.

    ![Wykres słupkowy w usłudze Power BI][9]

3. W tym kroku utworzymy wykres liniowy, który przedstawia kwoty sprzedaży według dat zamówienia. Aby go utworzyć, przejdź do rozszerzonego widoku AggregateSales (Łączna sprzedaż). Kliknij pola SalesAmount (Kwota sprzedaży) i OrderDate (Data zamówienia). W kolumnie Wizualizacje kliknij ikonę Wykres liniowy. Jest to pierwsza ikona w drugim wierszu w obszarze wizualizacji.

    ![Wybór wykresu liniowego w usłudze Power BI][10]

    Masz teraz raport wyświetlający trzy różne wizualizacje danych.

    ![Wykres liniowy w usłudze Power BI][11]

W dowolnym momencie możesz kliknąć menu **Plik** i wybrać polecenie **Zapisz**, aby zapisać postęp.

## Następne kroki
Po przećwiczeniu podstawowych zadań z użyciem przykładowych danych zapoznaj się ze sposobami wykonywania takich czynności, jak [opracowywanie][], [ładowanie][] lub [migrowanie][] danych. Możesz też przyjrzeć się [witrynie sieci Web usługi Power BI][].

<!--Image references-->
[1]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]:./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrowanie]: ./sql-data-warehouse-overview-migrate.md
[opracowywanie]: ./sql-data-warehouse-overview-develop.md
[ładowanie]: ./sql-data-warehouse-overview-load.md
[ręcznie załadować przykładowe dane]: ./sql-data-warehouse-get-started-manually-load-samples.md
[nawiązywanie połączenia z usługą SQL Data Warehouse]: ./sql-data-warehouse-integrate-power-bi.md
[Tworzenie bazy danych w usłudze SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Azure Machine Learning]: ./sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md

<!--Other-->
[Portalu Azure]: https://portal.azure.com/
[witrynie sieci Web usługi Power BI]: http://www.powerbi.com/



<!----HONumber=Jun16_HO2-->


