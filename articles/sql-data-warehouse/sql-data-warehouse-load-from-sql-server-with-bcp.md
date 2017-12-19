---
title: "Ładowanie danych z programu SQL Server do usługi Azure SQL Data Warehouse (bcp) | Dokumentacja firmy Microsoft"
description: "Dane niewielkich rozmiarów można kopiować, używając programu bcp do eksportowania danych z programu SQL Server do plików prostych i importowania danych bezpośrednio do usługi Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: f87d8d7c-8276-43c5-90e7-d4ccc0e3a224
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: dae7b5f7456f4ec0daf60d55f9c38b780896ff83
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Ładowanie danych z programu SQL Server do usługi Azure SQL Data Warehouse (pliki proste)
> [!div class="op_single_selector"]
> * [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

W przypadku małych zestawów danych można użyć narzędzia wiersza polecenia bcp do eksportowania danych z programu SQL Server, a następnie ładowania ich bezpośrednio do usługi Azure SQL Data Warehouse.

W tym samouczku przy użyciu programu bcp zostaną wykonane następujące czynności:

* Eksportowanie tabeli z programu SQL Server przy użyciu polecenia bcp out (lub tworzenie prostego pliku przykładowego)
* Importowanie tabeli z pliku prostego do usługi SQL Data Warehouse.
* Tworzenie statystyk dotyczących ładowanych danych.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="before-you-begin"></a>Przed rozpoczęciem
### <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków opisanych w tym samouczku potrzebne są:

* Baza danych usługi SQL Data Warehouse
* Zainstalowane narzędzie wiersza polecenia bcp
* Zainstalowane narzędzie wiersza polecenia sqlcmd

Narzędzia bcp i sqlcmd można pobrać z [Centrum pobierania Microsoft][Microsoft Download Center].

### <a name="data-in-ascii-or-utf-16-format"></a>Dane w formacie ASCII lub UTF-16
Jeśli próbujesz wykonać kroki tego samouczka z użyciem własnych danych, musisz zastosować do danych kodowanie ASCII lub UTF-16, ponieważ narzędzie bcp nie obsługuje formatu UTF-8. 

Aparat PolyBase obsługuje format UTF-8, ale nie obsługuje jeszcze formatu UTF-16. Uwaga: jeśli chcesz połączyć program bcp z aparatem PolyBase, konieczna będzie konwersja danych do formatu UTF-8 po ich wyeksportowaniu z programu SQL Server. 

## <a name="1-create-a-destination-table"></a>1. Tworzenie tabeli docelowej
W usłudze SQL Data Warehouse zdefiniuj tabelę, która będzie tabelą docelową ładowania. Kolumny w tabeli muszą odpowiadać danym w poszczególnych wierszach pliku danych.

Aby utworzyć tabelę, otwórz wiersz polecenia i użyj programu sqlcmd.exe, aby uruchomić następujące polecenie:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```


## <a name="2-create-a-source-data-file"></a>2. Tworzenie źródłowego pliku danych
Otwórz program Notatnik i skopiuj następujące wiersze danych do nowego pliku tekstowego, a następnie zapisz ten plik w lokalnym katalogu tymczasowym: C:\Temp\DimDate2.txt. Te dane są w formacie ASCII.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(Opcjonalnie) Aby wyeksportować własne dane z bazy danych programu SQL Server, otwórz wiersz polecenia i uruchom następujące polecenie. Zastąp parametry TableName, ServerName, DatabaseName, Username i Password odpowiednimi informacjami.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-the-data"></a>3. Ładowanie danych
Aby załadować dane, otwórz wiersz polecenia i uruchom następujące polecenie, zastępując najpierw wartości parametrów nazwą serwera, nazwą bazy danych, nazwą użytkownika i hasłem.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Użyj tego polecenia, aby sprawdzić, czy dane zostały załadowane poprawnie:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Wyniki powinny wyglądać następująco:

| DateId | CalendarQuarter | FiscalQuarter |
| --- | --- | --- |
| 20150101 |1 |3 |
| 20150201 |1 |3 |
| 20150301 |1 |3 |
| 20150401 |2 |4 |
| 20150501 |2 |4 |
| 20150601 |2 |4 |
| 20150701 |3 |1 |
| 20150801 |3 |1 |
| 20150801 |3 |1 |
| 20151001 |4 |2 |
| 20151101 |4 |2 |
| 20151201 |4 |2 |

## <a name="4-create-statistics"></a>4. Tworzenie statystyk
Usługa SQL Data Warehouse nie obsługuje jeszcze automatycznego tworzenia ani aktualizowania statystyk. Aby uzyskać najlepszą wydajność zapytań, należy utworzyć statystyki dla wszystkich kolumn wszystkich tabel po pierwszym załadowaniu danych, a następnie po każdej istotnej zmianie. Aby uzyskać szczegółowy opis statystyk, zobacz [statystyki][Statistics]. 

Uruchom następujące polecenie, aby utworzyć statystyki dotyczące nowo załadowanej tabeli.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. Eksportowanie danych z usługi SQL Data Warehouse
Dla zabawy możesz wyeksportować z powrotem załadowane przed chwilą dane z usługi SQL Data Warehouse.  Polecenie eksportu jest dokładnie takie samo, jak użyte podczas eksportowania z programu SQL Server.

Istnieje jednak różnica w wynikach. Ponieważ dane są przechowywane w rozproszonych lokalizacjach w usłudze SQL Data Warehouse, podczas eksportowania danych każdy węzeł obliczeniowy zapisuje swoje dane do pliku wyjściowego. Kolejność danych w pliku wyjściowym może być inna niż kolejność danych w pliku wejściowym.

### <a name="export-a-table-and-compare-exported-results"></a>Eksportowanie tabeli i porównywanie wyeksportowanych wyników
Aby zobaczyć wyeksportowane dane, otwórz wiersz polecenia i uruchom to polecenie z własnymi parametrami. ServerName jest nazwą serwera logicznego SQL platformy Azure.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Możesz sprawdzić, czy dane zostały poprawnie wyeksportowane, otwierając nowy plik. Dane w pliku powinny zawierać poniższy tekst, ale prawdopodobnie będą posortowane w innej kolejności:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="export-the-results-of-a-query"></a>Eksportowanie wyników zapytania
Możesz użyć funkcji **queryout** programu bcp do wyeksportowania wyników zapytania, zamiast eksportować całą tabelę. 

## <a name="next-steps"></a>Następne kroki
Omówienie ładowania można znaleźć w artykule [Ładowanie danych do usługi SQL Data Warehouse][Load data into SQL Data Warehouse].
Więcej porad dla deweloperów znajduje się w artykule [Omówienie programowania w usłudze SQL Data Warehouse][SQL Data Warehouse development overview].
Zobacz [omówienie tabeli] [ Table Overview] lub [składni polecenia CREATE TABLE] [ CREATE TABLE syntax] uzyskać więcej informacji dotyczących tworzenia tabel w magazynie danych SQL.

<!--Image references-->

<!--Article references-->

[Load data into SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Table Overview]: ./sql-data-warehouse-tables-overview.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
