---
title: "Wizualizacja danych usługi SQL Data Warehouse przy użyciu usługi Power BI | Microsoft Azure"
description: "Wizualizacja danych usługi SQL Data Warehouse przy użyciu usługi Power BI"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: d7fb89d1-da1d-4788-a111-68d0e3fda799
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: f498f4546e8c23f2141d8d30160a360fa0fc2514


---
# <a name="visualize-data-with-power-bi"></a>Wizualizacja danych przy użyciu usługi Power BI
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Program Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Ten samouczek przedstawia sposób nawiązania połączenia z usługą SQL Data Warehouse przy użyciu usługi Power BI oraz tworzenia podstawowych wizualizacji.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków opisanych w tym samouczku potrzebne są:

* Baza danych usługi SQL Data Warehouse ze wstępnie załadowaną bazą danych AdventureWorksDW. Aby dowiedzieć się, jak załadować dane przykładowe, zobacz artykuł [Tworzenie bazy danych w usłudze SQL Data Warehouse][Tworzenie bazy danych w usłudze SQL Data Warehouse]. Jeśli masz już magazyn danych, ale bez przykładowych danych, możesz [ręcznie załadować przykładowe dane][ręczne ładowanie przykładowych danych].

## <a name="1-connect-to-your-database"></a>1. Nawiązywanie połączenia z bazą danych
Aby otworzyć usługę Power BI nawiązać połączenie się z bazą danych AdventureWorksDW:

1. Zaloguj się w witrynie [Azure Portal][Azure Portal].
2. Kliknij pozycję **Bazy danych SQL** i wybierz bazę danych AdventureWorks usługi SQL Data Warehouse.
   
    ![Znajdowanie bazy danych][1]
3. Kliknij przycisk „Otwórz w usłudze Power BI”.
   
    ![Przycisk usługi Power BI][2]
4. Powinna zostać wyświetlona strona połączenia z usługą SQL Data Warehouse przedstawiająca adres sieci Web bazy danych. Kliknij przycisk Dalej.
   
    ![Połączenie z usługą Power BI][3]
5. Wprowadź nazwę użytkownika i hasło serwera Azure SQL. Zostanie nawiązane pełne połączenie z bazą danych usługi SQL Data Warehouse.
   
    ![Logowanie w usłudze Power BI][4]
6. Po zalogowaniu w usłudze Power BI kliknij zestaw danych AdventureWorksDW w lewym bloku. Spowoduje to otwarcie bazy danych.
   
    ![Otwieranie bazy danych AdventureWorksDW w usłudze Power BI][5]

## <a name="2-create-a-report"></a>2. Tworzenie raportu
Teraz możesz przystąpić do analizowania przykładowych danych bazy AdventureWorksDW przy użyciu usługi Power BI. Do wykonywania analizy służy widok bazy danych AdventureWorksDW o nazwie AggregateSales (Łączna sprzedaż). Ten widok zawiera kilka kluczowych metryk do analizowania sprzedaży firmy.

1. Aby utworzyć mapę kwot sprzedaży na podstawie kodów pocztowych, w okienku pól po prawej stronie kliknij widok AggregateSales (Łączna sprzedaż), aby go rozwinąć. Kliknij kolumny PostalCode (Kod pocztowy) and SalesAmount (Kwota sprzedaży), aby je zaznaczyć.
   
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

## <a name="next-steps"></a>Następne kroki
Po przećwiczeniu podstawowych zadań z użyciem przykładowych danych zapoznaj się ze sposobami wykonywania takich czynności, jak [opracowywanie][opracowywanie], [ładowanie][ładowanie] lub [migrowanie][migrowanie] danych. Możesz też zapoznać się z [witryną sieci Web usługi Power BI][witryna sieci Web usługi Power BI].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrowanie]: sql-data-warehouse-overview-migrate.md
[opracowywanie]: sql-data-warehouse-overview-develop.md
[ładowanie]: sql-data-warehouse-overview-load.md
[ręczne ładowanie przykładowych danych]: sql-data-warehouse-load-sample-databases.md
[nawiązywanie połączenia z usługą SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Tworzenie bazy danych w usłudze SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure Portal]: https://portal.azure.com/
[witryna sieci Web usługi Power BI]: http://www.powerbi.com/



<!--HONumber=Nov16_HO2-->


