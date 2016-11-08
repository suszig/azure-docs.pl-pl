---
title: Ładowanie danych z programu SQL Server do usługi Azure SQL Data Warehouse (PolyBase) | Microsoft Docs
description: Eksportowanie danych z programu SQL Server do plików prostych przy użyciu programu bcpw, importowanie danych do usługi Azure Blob Storage przy użyciu programu AZCopy oraz pozyskiwanie danych do usługi Azure SQL Data Warehouse przy użyciu programu PolyBase.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: ''

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/30/2016
ms.author: cakarst;barbkess;sonyama

---
# Ładowanie danych z programu SQL Server do usługi Azure SQL Data Warehouse (AZCopy)
Użyj narzędzia bcp i narzędzia wiersza polecenia AZCopy do ładowania danych z programu SQL Server do usługi Azure Blob Storage. Następnie zastosuj technologię PolyBase lub usługę Azure Data Factory do ładowania danych do usługi Azure SQL Data Warehouse. 

## Wymagania wstępne
Do wykonania kroków opisanych w tym samouczku potrzebne są:

* Baza danych usługi SQL Data Warehouse
* Zainstalowane narzędzie wiersza polecenia bcp
* Zainstalowane narzędzie wiersza polecenia SQLCMD

> [!NOTE]
> Narzędzia bcp i sqlcmd można pobrać z [Centrum pobierania Microsoft][Centrum pobierania Microsoft].
> 
> 

## Importowanie danych do usługi SQL Data Warehouse
W tym samouczku utworzysz tabelę w usłudze Azure SQL Data Warehouse i zaimportujesz dane do tej tabeli.

### Krok 1: tworzenie tabeli w usłudze Azure SQL Data Warehouse
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
> Zobacz artykuł [Omówienie tabel][Omówienie tabel] lub [składni polecenia CREATE TABLE][składni polecenia CREATE TABLE], aby uzyskać więcej informacji dotyczących tworzenia tabel w usłudze SQL Data Warehouse oraz dostępnych opcji klauzuli WITH.
> 
> 

### Krok 2: tworzenie źródłowego pliku danych
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

### Krok 3: nawiązywanie połączenia i importowanie danych
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

### Krok 4: tworzenie statystyk na podstawie nowo załadowanych danych
Usługa Azure SQL Data Warehouse nie obsługuje jeszcze automatycznego tworzenia ani aktualizowania statystyk. W celu uzyskania najlepszej wydajności zapytań należy utworzyć statystyki dla wszystkich kolumn wszystkich tabel po pierwszym załadowaniu danych, a następnie po każdej istotnej zmianie. Szczegółowy opis statystyk znajduje się w temacie [Statystyki][Statystyki] w grupie artykułów dla programistów. Poniżej przedstawiono prosty przykład tworzenia statystyk dotyczących tabeli załadowanej w tym przykładzie.

W wierszu polecenia sqlcmd wykonaj następującą instrukcję CREATE STATISTICS:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## Eksportowanie danych z usługi SQL Data Warehouse
W tym samouczku utworzysz plik danych z tabeli w usłudze SQL Data Warehouse. Wyeksportujemy dane utworzone powyżej do nowego pliku danych o nazwie DimDate2_export.txt.

### Krok 1: eksportowanie danych
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

## Następne kroki
Ogólny opis ładowania można znaleźć w artykule [Ładowanie danych do usługi SQL Data Warehouse][Ładowanie danych do usługi SQL Data Warehouse].
Więcej porad programistycznych znajdziesz w artykule [Omówienie programowania w usłudze SQL Data Warehouse][Omówienie programowania w usłudze SQL Data Warehouse].

<!--Image references-->

<!--Article references-->

[Ładowanie danych do usługi SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[Omówienie programowania w usłudze SQL Data Warehouse]: ./sql-data-warehouse-overview-develop.md
[Omówienie tabel]: ./sql-data-warehouse-tables-overview.md
[Statystyki]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[składni polecenia CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Centrum pobierania Microsoft]: https://www.microsoft.com/download/details.aspx?id=36433



<!--HONumber=Sep16_HO3-->


