---
title: "Ładowanie przykładowych danych do usługi SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Ładowanie przykładowych danych do usługi SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e338ecf8-cfee-419b-b7b6-98108d381c62
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 1e0df958a2f18fe1e988168918e5cfd293f84e64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="load-sample-data-into-sql-data-warehouse"></a>Ładowanie przykładowych danych do usługi SQL Data Warehouse
Wykonaj poniższe kroki proste do ładowania i w bazie danych firmy Adventure Works próbki. Skrypty te należy najpierw użyć narzędzia sqlcmd można uruchomić programu SQL, co spowoduje utworzenie tabel i widoków. Po utworzeniu tabel skrypty użyje bcp można załadować danych.  Jeśli nie masz jeszcze sqlcmd i zainstalować narzędzia bcp, skorzystaj z poniższych linków, aby [zainstalować narzędzia bcp] [ install bcp] i [zainstalować narzędzia sqlcmd][install sqlcmd].

## <a name="load-sample-data"></a>Ładuj dane przykładowe
1. Pobierz [Adventure Works przykładowe skrypty dla usługi SQL Data Warehouse] [ Adventure Works Sample Scripts for SQL Data Warehouse] pliku zip.
2. Wyodrębnij pliki z pobranego zip do katalogu na komputerze lokalnym.
3. Edytuj aw_create.bat wyodrębnionego pliku i ustaw następujące zmienne znajdujących się u góry pliku.  Pamiętaj pozostawić nie spacji między "=" i parametr.  Poniżej przedstawiono przykłady może wyglądać dokonanych zmian.
   
    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```
4. W wierszu polecenia systemu Windows uruchom aw_create.bat edytowany.  Upewnij się, że jesteś w katalogu, w którym zapisano aw_create.bat Twojego zmodyfikowaną wersję.
   Ten skrypt zostanie...
   
   * Upuść Adventure Works tabel ani widoków, które już istnieją w bazie danych
   * Utwórz Adventure Works tabele i widoki
   * Ładowanie każdej tabeli Adventure Works przy użyciu narzędzia bcp
   * Sprawdzanie poprawności liczby wierszy dla każdej tabeli Adventure Works
   * Zbieranie statystyk w każdej kolumnie dla każdej tabeli Adventure Works

## <a name="query-sample-data"></a>Dane przykładowe zapytania
Po przykładowych danych zostało załadowane do magazynu danych SQL, można szybko uruchomić kilka zapytań.  Aby uruchomić kwerendę, połączyć się z nowo utworzone bazy danych firmy Adventure Works w magazynu danych SQL Azure za pomocą programu Visual Studio i narzędzi SSDT, zgodnie z opisem w [zapytania z programem Visual Studio] [ query with Visual Studio] dokumentu.

Przykład prostego instrukcji select, aby uzyskać wszystkie informacje o pracowników:

```sql
SELECT * FROM DimEmployee;
```

Przykład bardziej złożonego zapytania przy użyciu konstrukcji, takich jak GROUP BY aby przyjrzeć się suma wszystkich sprzedaży każdego dnia:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Przykład SELECT z klauzulą WHERE filtrowanie zleceń przed określonej daty:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Magazyn danych SQL obsługuje prawie wszystkie konstrukcje T-SQL, które obsługuje program SQL Server.  Różnice są udokumentowane w naszym [Migrowanie kodu] [ migrate code] dokumentacji.

## <a name="next-steps"></a>Następne kroki
Skoro już wcześniej masz szansę wypróbować kilka zapytań z przykładowymi danymi, sprawdź informacje dotyczące sposobu [opracowanie][develop], [załadować][load], lub [ Migrowanie] [ migrate] SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[query with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[migrate code]: sql-data-warehouse-migrate-code.md
[install bcp]: sql-data-warehouse-load-with-bcp.md
[install sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works Sample Scripts for SQL Data Warehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
