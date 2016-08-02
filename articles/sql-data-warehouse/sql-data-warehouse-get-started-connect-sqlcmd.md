<properties
   pageTitle="Wprowadzenie: nawiązywanie połączenia z usługą Azure SQL Data Warehouse | Microsoft Azure"
   description="Wprowadzenie do nawiązywania połączenia z usługą SQL Data Warehouse i uruchamiania niektórych zapytań."
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
   ms.date="05/16/2016"
   ms.author="mausher;barbkess;sonyama"/>

# Nawiązywanie połączenia i wykonywanie zapytań przy użyciu narzędzia SQLCMD

> [AZURE.SELECTOR]
- [Visual Studio](sql-data-warehouse-get-started-connect.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md)
- [AAD](sql-data-warehouse-get-started-connect-aad-authentication.md)


W tym przewodniku przedstawiono sposób nawiązywania połączenia z bazą danych usługi Azure SQL Data Warehouse oraz wykonywania w niej zapytań w ciągu zaledwie kilku minut przy użyciu narzędzia sqlcmd.exe. Korzystając z tego przewodnika, wykonasz następujące czynności:

+ Instalowanie wstępnie wymaganego oprogramowania
+ Nawiązywanie połączenia z bazą danych, która zawiera przykładową bazę danych AdventureWorksDW
+ Wykonywanie zapytań w przykładowej bazie danych  

## Wymagania wstępne

+ Aby pobrać program [sqlcmd.exe][], zobacz narzędzia [Microsoft Command Line Utilities 11 for SQL Server][].

## Pobieranie w pełni kwalifikowanej nazwy serwera Azure SQL

Do połączenia z bazą danych potrzebna jest pełna nazwa serwera (***nazwa_serwera**.database.windows.net*) zawierającego tę bazę danych.

1. Przejdź do [Portalu Azure][].
2. Przejdź do bazy danych, z którą chcesz nawiązać połączenie.
3. Ustal pełną nazwę serwera (użyjemy jej w poniższych krokach):

![][1]


## Nawiązywanie połączenia z usługą SQL Data Warehouse przy użyciu narzędzia sqlcmd

Aby połączyć się z określonym wystąpieniem usługi SQL Data Warehouse przy użyciu narzędzia sqlcmd, otwórz wiersz polecenia i wprowadź polecenie **sqlcmd**, a następnie parametry połączenia z bazą danych usługi SQL Data Warehouse. Wymaganymi parametrami połączenia są:

+ **Serwer (-S):** serwer w postaci `<`nazwa_serwera`>`.database.windows.net
+ **Baza danych (-d):** nazwa bazy danych.
+ **Użytkownik (-U):** użytkownik serwera w formie `<`Użytkownik`>`
+ **Hasło (-P):** hasło skojarzone z użytkownikiem.
+ **Włącz cytowane identyfikatory (-I):** cytowane identyfikatory muszą być włączone w celu połączenia z wystąpieniem usługi SQL Data Warehouse.

Podsumowując, aby nawiązać połączenie z wystąpieniem usługi SQL Data Warehouse, należy wpisać następujące polecenie:

```sql
C:\>sqlcmd -S <Server Name>.database.windows.net -d <Database> -U <User> -P <Password> -I
```

## Uruchamianie przykładowych zapytań

Po nawiązaniu połączenia można wydawać dowolne obsługiwane instrukcje języka Transact-SQL dotyczące wystąpienia.

```sql
C:\>sqlcmd -S <Server Name>.database.windows.net -d <Database> -U <User> -P <Password> -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Dodatkowe informacje na temat narzędzia sqlcmd znajdują się w [dokumentacji narzędzia sqlcmd][sqlcmd.exe].


## Następne kroki

Teraz, kiedy umiesz nawiązywać połączenia i wykonywać zapytania, spróbuj [nawiązać połączenie z usługą Power BI][].

Aby skonfigurować środowisko do uwierzytelniania systemu Windows, zobacz artykuł [Nawiązywanie połączeń z usługami SQL Database lub SQL Data Warehouse przy użyciu uwierzytelniania usługi Azure Active Directory][].

<!--Articles-->
[Nawiązywanie połączeń z usługami SQL Database lub SQL Data Warehouse przy użyciu uwierzytelniania usługi Azure Active Directory]: ../sql-data-warehouse/sql-data-warehouse-get-started-connect-aad-authentication.md
[nawiązać połączenie z usługą Power BI]: ./sql-data-warehouse-integrate-power-bi.md
[Visual Studio]: ./sql-data-warehouse-get-started-connect.md
[SQLCMD]: ./sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other-->
[sqlcmd.exe]: https://msdn.microsoft.com/en-us/library/ms162773.aspx
[Microsoft Command Line Utilities 11 for SQL Server]: http://go.microsoft.com/fwlink/?LinkId=321501
[Portalu Azure]: https://portal.azure.com

<!--Image references-->
[1]: ./media/sql-data-warehouse-get-started-connect/get-server-name.png



<!--HONumber=Jun16_HO2-->


