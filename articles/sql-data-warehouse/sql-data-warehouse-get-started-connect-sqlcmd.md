<properties
   pageTitle="Tworzenie zapytań względem usługi Azure SQL Data Warehouse (sqlcmd) | Microsoft Azure"
   description="Tworzenie zapytań względem usługi Azure SQL Data Warehouse przy użyciu narzędzia wiersza polecenia sqlcmd."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/06/2016"
   ms.author="barbkess;sonyama"/>


# Tworzenie zapytań względem usługi Azure SQL Data Warehouse (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Z tego przewodnika dowiesz się, jak utworzyć zapytanie względem usługi Azure SQL Data Warehouse przy użyciu narzędzia wiersza polecenia [sqlcmd][].  

## 1. Połączenie

Aby rozpocząć pracę z narzędziem [sqlcmd][], otwórz wiersz polecenia i wpisz **sqlcmd**, a następnie podaj parametry połączenia z bazą danych usługi SQL Data Warehouse. Wymagane parametry połączenia to:

+ **Serwer (-S):** serwer w postaci `<`nazwa_serwera`>`.database.windows.net
+ **Baza danych (-d):** nazwa bazy danych.
+ **Włącz cytowane identyfikatory (-I):** cytowane identyfikatory muszą być włączone w celu połączenia z wystąpieniem usługi SQL Data Warehouse.

Aby użyć uwierzytelniania programu SQL Server, należy dodać parametry nazwy użytkownika/hasła:

+ **Użytkownik (-U):** użytkownik serwera w formie `<`Użytkownik`>`
+ **Hasło (-P):** hasło skojarzone z użytkownikiem.

Na przykład parametry połączenia mogą wyglądać następująco:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Aby użyć zintegrowanego uwierzytelniania usługi Azure Active Directory, należy dodać parametry usługi Azure Active Directory:

+ **Uwierzytelnianie usługi Azure Active Directory (-G):** używaj usługi Azure Active Directory do uwierzytelniania

Na przykład parametry połączenia mogą wyglądać następująco:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [AZURE.NOTE] Musisz [włączyć uwierzytelnianie usługi Azure Active Directory](sql-data-warehouse-authentication.md), aby uwierzytelniać przy użyciu usługi Active Directory.

## 2. Zapytanie

Po nawiązaniu połączenia można wydawać dowolne obsługiwane instrukcje języka Transact-SQL dotyczące wystąpienia.  W tym przykładzie zapytania są przesyłane w trybie interaktywnym.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

W następnych przykładach przedstawiono, jak można uruchamiać zapytania w trybie wsadowym przy użyciu opcji -Q lub przesyłania potokiem instrukcji języka SQL do narzędzia sqlcmd.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## Następne kroki

Aby uzyskać więcej informacji na temat opcji dostępnych w narzędziu sqlcmd, zobacz [dokumentację narzędzia sqlcmd][sqlcmd].

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure Portal]: https://portal.azure.com

<!--Other Web references-->



<!--HONumber=Sep16_HO3-->


