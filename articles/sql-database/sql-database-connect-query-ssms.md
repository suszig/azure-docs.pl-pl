<properties
    pageTitle="Łączenie z bazą danych SQL Database za pomocą programu SQL Server Management Studio | Microsoft Azure"
    description="Dowiedz się, jak łączyć się z bazą danych SQL Database na platformie Azure przy użyciu programu SQL Server Management Studio (SSMS). Następnie uruchom przykładowe zapytanie za pomocą języka Transact-SQL (T-SQL)."
    metaCanonical=""
    keywords="connect to sql database,sql server management studio"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="" />

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/09/2016"
    ms.author="sstein;carlrab" />

# Nawiązywanie połączenia z bazą danych SQL Database za pomocą programu SQL Server Management Studio i wykonywanie przykładowego zapytania T-SQL

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

W tym artykule opisano sposób nawiązania połączenia z bazą danych SQL Azure za pomocą najnowszej wersji programu SQL Server Management Studio (SSMS) i wykonania prostego zapytania z użyciem instrukcji języka Transact-SQL (T-SQL).

[AZURE.INCLUDE [Sign in](../../includes/azure-getting-started-portal-login.md)]

[AZURE.INCLUDE [SSMS Install](../../includes/sql-server-management-studio-install.md)]

[AZURE.INCLUDE [SSMS Connect](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

Aby uzyskać informacje na temat reguł zapory, zobacz temat [How to: Configure Firewall Settings (Azure SQL Database)](sql-database-configure-firewall-settings.md) (Poradnik: Konfigurowanie ustawień zapory (baza danych Azure SQL Database)).

## Uruchamianie przykładowych zapytań

Po nawiązaniu połączenia z serwerem logicznym można połączyć się z bazą danych i uruchomić przykładowe zapytanie. 

1. W **Eksploratorze obiektów** przejdź do bazy danych na serwerze, do którego masz uprawnienia, takiej jak przykładowa baza danych **AdventureWorks**.
2. Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz pozycję **Nowe zapytanie**.

    ![Nowe zapytanie. Łączenie z serwerem usługi SQL Database: SQL Server Management Studio](./media/sql-database-connect-query-ssms/4-run-query.png)

3. W oknie zapytania skopiuj i wklej następujący kod.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Kliknij przycisk **Wykonaj**.  Poniższy zrzut ekranu przedstawia pomyślnie wykonane zapytanie.

    ![Powodzenie. Łączenie z serwerem usługi SQL Database: SQL Server Management Studio](./media/sql-database-connect-query-ssms/5-success.png)

## Następne kroki

Instrukcje T-SQL umożliwiają tworzenie baz danych i zarządzanie nimi w systemie Azure podobnie jak w programie SQL Server. Jeśli wiesz, jak używać języka T-SQL w programie SQL Server, zobacz temat [Azure SQL Database Transact-SQL information](sql-database-transact-sql-information.md) (Baza danych Azure SQL Database i język Transact-SQL — informacje) zawierający podsumowanie różnic.

Jeśli dopiero zaczynasz pracę z językiem T-SQL, zobacz tematy [Tutorial: Writing Transact-SQL Statements](https://msdn.microsoft.com/library/ms365303.aspx) (Samouczek: Pisanie instrukcji w języku Transact-SQL) i [Transact-SQL Reference (Database Engine)](https://msdn.microsoft.com/library/bb510741.aspx) (Informacje o języku Transact-SQL — aparat bazy danych).

Aby rozpocząć tworzenie użytkowników i administratorów bazy danych, zobacz temat [Get Started with Azure SQL Database security](sql-database-get-started-security.md) (Wprowadzenie do zabezpieczeń bazy danych Azure SQL Database).



<!--HONumber=Jun16_HO2-->


