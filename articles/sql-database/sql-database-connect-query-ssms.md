<properties
    pageTitle="Łączenie z bazą danych SQL Database za pomocą programu SQL Server Management Studio | Microsoft Azure"
    description="Dowiedz się, jak łączyć się z bazą danych SQL Database na platformie Azure przy użyciu programu SQL Server Management Studio (SSMS). Następnie uruchom przykładowe zapytanie za pomocą języka Transact-SQL (T-SQL)."
    metaCanonical=""
    keywords="łączenie z bazą danych SQL, sql server management studio"
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
    ms.date="08/17/2016"
    ms.author="sstein;carlrab" />

# Nawiązywanie połączenia z bazą danych SQL Database za pomocą programu SQL Server Management Studio i wykonywanie przykładowego zapytania T-SQL

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

W tym artykule opisano, jak nawiązać połączenie z bazą danych SQL na platformie Azure przy użyciu programu SQL Server Management Studio (SSMS). Po pomyślnym nawiązaniu połączenia uruchomimy proste zapytanie Transact-SQL (T-SQL), aby sprawdzić połączenie z bazą danych.

[AZURE.INCLUDE [SSMS Install](../../includes/sql-server-management-studio-install.md)]

[AZURE.INCLUDE [SSMS Connect](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


## Uruchamianie przykładowych zapytań

Po nawiązaniu połączenia z serwerem można połączyć się z bazą danych i uruchomić przykładowe zapytanie. Jeśli dopiero zaczynasz pisanie zapytań, zobacz [Writing Transact-SQL Statements](https://msdn.microsoft.com/library/ms365303.aspx) (Pisanie instrukcji w języku Transact-SQL).

1. W **Eksploratorze obiektów** przejdź do bazy danych na serwerze, takiej jak przykładowa baza danych **AdventureWorks**.
2. Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz pozycję **Nowe zapytanie**:

    ![Nowe zapytanie. Łączenie z serwerem usługi SQL Database: SQL Server Management Studio](./media/sql-database-connect-query-ssms/4-run-query.png)

3. W oknie zapytania skopiuj i wklej następujący tekst:

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Kliknij przycisk **Wykonaj**:

    ![Powodzenie. Łączenie z serwerem usługi SQL Database: SQL Server Management Studio](./media/sql-database-connect-query-ssms/5-success.png)

## Następne kroki

Instrukcje T-SQL umożliwiają tworzenie baz danych i zarządzanie nimi w systemie Azure podobnie jak w programie SQL Server. Jeśli wiesz, jak używać języka T-SQL w programie SQL Server, zobacz temat [Azure SQL Database Transact-SQL information](sql-database-transact-sql-information.md) (Baza danych Azure SQL Database i język Transact-SQL — informacje) zawierający podsumowanie różnic.

Jeśli dopiero zaczynasz pracę z językiem T-SQL, zobacz tematy [Tutorial: Writing Transact-SQL Statements](https://msdn.microsoft.com/library/ms365303.aspx) (Samouczek: Pisanie instrukcji w języku Transact-SQL) i [Transact-SQL Reference (Database Engine)](https://msdn.microsoft.com/library/bb510741.aspx) (Informacje o języku Transact-SQL — aparat bazy danych).

Aby rozpocząć tworzenie użytkowników i administratorów bazy danych, zobacz temat [Get Started with Azure SQL Database security](sql-database-get-started-security.md) (Wprowadzenie do zabezpieczeń bazy danych Azure SQL Database).

Aby uzyskać więcej informacji o programie SSMS, zobacz artykuł [Use SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (Korzystanie z programu SQL Server Management Studio).



<!--HONumber=sep16_HO1-->


