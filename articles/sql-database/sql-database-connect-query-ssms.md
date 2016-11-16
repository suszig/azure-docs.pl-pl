---
title: "Łączenie z bazą danych SQL Database za pomocą programu SQL Server Management Studio | Microsoft Docs"
description: "Dowiedz się, jak łączyć się z bazą danych SQL Database na platformie Azure przy użyciu programu SQL Server Management Studio (SSMS). Następnie uruchom przykładowe zapytanie za pomocą języka Transact-SQL (T-SQL)."
metacanonical: 
keywords: "łączenie z bazą danych SQL, sql server management studio"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 7cd2a114-c13c-4ace-9088-97bd9d68de12
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: sstein;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0eb25eb76c6c6c2446ac0b2b07c65975c3719db0


---
# <a name="connect-to-sql-database-with-sql-server-management-studio-and-execute-a-sample-tsql-query"></a>Nawiązywanie połączenia z bazą danych SQL Database za pomocą programu SQL Server Management Studio i wykonywanie przykładowego zapytania T-SQL
> [!div class="op_single_selector"]
> * [Program Visual Studio](sql-database-connect-query.md)
> * [SSMS](sql-database-connect-query-ssms.md)
> * [Excel](sql-database-connect-excel.md)
> 
> 

W tym artykule opisano, jak nawiązać połączenie z bazą danych SQL na platformie Azure przy użyciu programu SQL Server Management Studio (SSMS). Po pomyślnym nawiązaniu połączenia uruchomimy proste zapytanie Transact-SQL (T-SQL), aby sprawdzić połączenie z bazą danych.

[!INCLUDE [SSMS Install](../../includes/sql-server-management-studio-install.md)]

[!INCLUDE [SSMS Connect](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

## <a name="run-sample-queries"></a>Uruchamianie przykładowych zapytań
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

## <a name="next-steps"></a>Następne kroki
Instrukcje T-SQL umożliwiają tworzenie baz danych i zarządzanie nimi w systemie Azure podobnie jak w programie SQL Server. Jeśli wiesz, jak używać języka T-SQL w programie SQL Server, zobacz temat [Azure SQL Database Transact-SQL information](sql-database-transact-sql-information.md) (Baza danych Azure SQL Database i język Transact-SQL — informacje) zawierający podsumowanie różnic.

Jeśli dopiero zaczynasz pracę z językiem T-SQL, zobacz tematy [Tutorial: Writing Transact-SQL Statements](https://msdn.microsoft.com/library/ms365303.aspx) (Samouczek: Pisanie instrukcji w języku Transact-SQL) i [Transact-SQL Reference (Database Engine)](https://msdn.microsoft.com/library/bb510741.aspx) (Informacje o języku Transact-SQL — aparat bazy danych).

Aby rozpocząć tworzenie użytkowników i administratorów bazy danych, zobacz temat [Get Started with Azure SQL Database security](sql-database-get-started-security.md) (Wprowadzenie do zabezpieczeń bazy danych Azure SQL Database).

Aby uzyskać więcej informacji o programie SSMS, zobacz artykuł [Use SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (Korzystanie z programu SQL Server Management Studio).




<!--HONumber=Nov16_HO2-->


