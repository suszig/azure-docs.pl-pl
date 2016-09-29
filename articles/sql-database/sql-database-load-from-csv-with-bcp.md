<properties
   pageTitle="Ładowanie danych z pliku CSV do usługi Azure SQL Database (bcp) | Microsoft Azure"
   description="Dane niewielkich rozmiarów można importować do usługi Azure SQL Database za pomocą narzędzia bcp."
   services="sql-database"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/13/2016"
   ms.author="carlrab"/>



# Ładowanie danych z pliku CSV do usługi Azure SQL Data Warehouse (pliki proste)

Dane z pliku CSV można importować do usługi Azure SQL Database za pomocą narzędzia wiersza polecenia bcp.

## Przed rozpoczęciem

### Wymagania wstępne

Do wykonania kroków opisanych w tym samouczku potrzebne są:

- Serwer logiczny i baza danych usługi Azure SQL Database
- Zainstalowane narzędzie wiersza polecenia bcp
- Zainstalowane narzędzie wiersza polecenia sqlcmd

Narzędzia bcp i sqlcmd można pobrać z [Centrum pobierania Microsoft][].

### Dane w formacie ASCII lub UTF-16

Jeśli próbujesz wykonać kroki tego samouczka z użyciem własnych danych, musisz zastosować do danych kodowanie ASCII lub UTF-16, ponieważ narzędzie bcp nie obsługuje formatu UTF-8. 

## 1. Tworzenie tabeli docelowej

Zdefiniuj tabelę w usłudze SQL Database jako tabelę docelową. Kolumny w tabeli muszą odpowiadać danym w poszczególnych wierszach pliku danych.

Aby utworzyć tabelę, otwórz wiersz polecenia i użyj programu sqlcmd.exe, aby uruchomić następujące polecenie:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
"
```


## 2. Tworzenie źródłowego pliku danych

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

## 3. Ładowanie danych
Aby załadować dane, otwórz wiersz polecenia i uruchom następujące polecenie, zastępując najpierw wartości parametrów nazwą serwera, nazwą bazy danych, nazwą użytkownika i hasłem.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Użyj tego polecenia, aby sprawdzić, czy dane zostały załadowane poprawnie:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Wyniki powinny wyglądać następująco:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2


## Następne kroki

Aby przeprowadzić migrację bazy danych programu SQL Server, zobacz [SQL Server database migration](sql-database-cloud-migrate.md) (Migracja bazy danych programu SQL Server).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[składni polecenia CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Centrum pobierania Microsoft]: https://www.microsoft.com/download/details.aspx?id=36433



<!--HONumber=Sep16_HO3-->


