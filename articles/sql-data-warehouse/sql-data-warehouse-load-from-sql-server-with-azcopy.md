---
title: "Ładowanie danych z programu SQL Server do usługi Azure SQL Data Warehouse (PolyBase) | Microsoft Docs"
description: "Eksportowanie danych z programu SQL Server do plików prostych przy użyciu programu bcpw, importowanie danych do usługi Azure Blob Storage przy użyciu programu AZCopy oraz pozyskiwanie danych do usługi Azure SQL Data Warehouse przy użyciu programu PolyBase."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4d42786a-fb28-43c9-9c3b-72d19c0ecc11
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 21b4cc704e271ac220fd606305f8f97c9b2593bb
ms.lasthandoff: 03/21/2017



---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a>Ładowanie danych z programu SQL Server do usługi Azure SQL Data Warehouse (AZCopy)
Użyj narzędzia bcp i narzędzia wiersza polecenia AZCopy do ładowania danych z programu SQL Server do usługi Azure Blob Storage. Następnie zastosuj technologię PolyBase lub usługę Azure Data Factory do ładowania danych do usługi Azure SQL Data Warehouse. 

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków opisanych w tym samouczku potrzebne są:

* Baza danych usługi SQL Data Warehouse
* Zainstalowane narzędzie wiersza polecenia bcp
* Zainstalowane narzędzie wiersza polecenia SQLCMD

> [!NOTE]
> Narzędzia bcp i sqlcmd można pobrać z [Centrum pobierania Microsoft][Microsoft Download Center].
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a>Importowanie danych do usługi SQL Data Warehouse
W tym samouczku utworzysz tabelę w usłudze Azure SQL Data Warehouse i zaimportujesz dane do tej tabeli.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Krok 1: tworzenie tabeli w usłudze Azure SQL Data Warehouse
W wierszu polecenia użyj polecenia sqlcmd, aby uruchomić następujące zapytanie w celu utworzenia tabeli w wystąpieniu:

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

> [!NOTE]
> Zobacz artykuł [Omówienie tabel][Table Overview] lub [Składnia polecenia CREATE TABLE][CREATE TABLE syntax], aby uzyskać więcej informacji dotyczących tworzenia tabel w usłudze SQL Data Warehouse oraz dostępnych opcji klauzuli WITH.
> 
> 

### <a name="step-2-create-a-source-data-file"></a>Krok 2: tworzenie źródłowego pliku danych
Otwórz program Notatnik i skopiuj następujące wiersze danych do nowego pliku tekstowego, a następnie zapisz ten plik w lokalnym katalogu tymczasowym: C:\Temp\DimDate2.txt.

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

> [!NOTE]
> Należy pamiętać, że program bcp.exe nie obsługuje kodowania pliku w formacie UTF-8. Podczas korzystania z programu bcp.exe należy używać plików kodowanych w formacie ASCII lub UTF-16.
> 
> 

### <a name="step-3-connect-and-import-the-data"></a>Krok 3: nawiązywanie połączenia i importowanie danych
Przy użyciu narzędzia bcp można nawiązać połączenie i zaimportować dane, stosując następujące polecenie z odpowiednio zastąpionymi wartościami:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Poprawność załadowania danych można sprawdzić za pomocą następującego zapytania uruchomionego przy użyciu narzędzia sqlcmd:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Powinny zostać zwrócone następujące wyniki:

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

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Krok 4: tworzenie statystyk na podstawie nowo załadowanych danych
Usługa Azure SQL Data Warehouse nie obsługuje jeszcze automatycznego tworzenia ani aktualizowania statystyk. W celu uzyskania najlepszej wydajności zapytań należy utworzyć statystyki dla wszystkich kolumn wszystkich tabel po pierwszym załadowaniu danych, a następnie po każdej istotnej zmianie. Szczegółowy opis statystyk znajduje się w temacie [Statystyki][Statistics] w grupie artykułów dla deweloperów. Poniżej przedstawiono prosty przykład tworzenia statystyk dotyczących tabeli załadowanej w tym przykładzie.

W wierszu polecenia sqlcmd wykonaj następującą instrukcję CREATE STATISTICS:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Eksportowanie danych z usługi SQL Data Warehouse
W tym samouczku utworzysz plik danych z tabeli w usłudze SQL Data Warehouse. Wyeksportujemy dane utworzone powyżej do nowego pliku danych o nazwie DimDate2_export.txt.

### <a name="step-1-export-the-data"></a>Krok 1: eksportowanie danych
Przy użyciu narzędzia bcp można nawiązać połączenie i wyeksportować dane za pomocą następującego polecenia z odpowiednio zastąpionymi wartościami:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Możesz sprawdzić, czy dane zostały poprawnie wyeksportowane, otwierając nowy plik. Dane w pliku powinny zawierać poniższy tekst:

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

> [!NOTE]
> Ze względu na specyfikę systemów rozproszonych kolejność danych w bazach danych usługi SQL Data Warehouse może się różnić. Innym rozwiązaniem jest użycie funkcji **queryout** programu bcp, aby napisać zapytanie wyodrębniające, zamiast eksportowania całej tabeli.
> 
> 

## <a name="next-steps"></a>Następne kroki
Omówienie ładowania można znaleźć w artykule [Ładowanie danych do usługi SQL Data Warehouse][Load data into SQL Data Warehouse].
Więcej porad dla deweloperów znajduje się w artykule [Omówienie programowania w usłudze SQL Data Warehouse][SQL Data Warehouse development overview].

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

