---
title: "Wizualizacja danych usługi SQL Data Warehouse przy użyciu usługi Power BI | Microsoft Azure"
description: "Wizualizacja danych usługi SQL Data Warehouse przy użyciu usługi Power BI"
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: d7fb89d1-da1d-4788-a111-68d0e3fda799
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: a41393730143b14e91318a61858d989fff3786c1
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
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

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków opisanych w tym samouczku potrzebne są:

* Baza danych usługi SQL Data Warehouse ze wstępnie załadowaną bazą danych AdventureWorksDW. Aby dowiedzieć się, jak załadować dane przykładowe, zobacz [Tworzenie magazynu danych SQL Data Warehouse][Create a SQL Data Warehouse]. Jeśli masz już magazyn danych, ale bez przykładowych danych, możesz [ręcznie załadować przykładowe dane][load sample data manually].

## <a name="1-connect-to-your-database"></a>1. Nawiązywanie połączenia z bazą danych
Aby otworzyć usługę Power BI nawiązać połączenie się z bazą danych AdventureWorksDW:

1. Zaloguj się do witryny [Azure Portal][Azure portal].
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
Po przećwiczeniu podstawowych zadań z użyciem przykładowych danych zapoznaj się ze sposobami wykonywania takich czynności, jak [opracowywanie][develop], [ładowanie][load] lub [migrowanie][migrate] danych. Możesz też zapoznać się z [witryną sieci Web usługi Power BI][Power BI website].

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
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI website]: http://www.powerbi.com/
