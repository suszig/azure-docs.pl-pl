---
title: "SSMS: nawiązywanie połączenia i wykonywanie zapytań dotyczących danych w bazie danych Azure SQL Database | Microsoft Docs"
description: "Dowiedz się, jak łączyć się z bazą danych SQL Database na platformie Azure przy użyciu programu SQL Server Management Studio (SSMS). Następnie uruchom instrukcje Transact-SQL (T-SQL), aby wykonać zapytanie i edytować dane."
metacanonical: 
keywords: "łączenie z bazą danych SQL, sql server management studio"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 7cd2a114-c13c-4ace-9088-97bd9d68de12
ms.service: sql-database
ms.custom: development
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/15/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 9f149c3959f1b249a15f2c2714d12c7c9be94bbb
ms.lasthandoff: 03/21/2017


---
# <a name="azure-sql-database-use-sql-server-management-studio-to-connect-and-query-data"></a>Azure SQL Database: używanie programu SQL Server Management Studio do nawiązywania połączenia i wykonywania zapytań dotyczących danych

Użyj programu [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS), aby utworzyć zasoby SQL Server i zarządzać nimi z poziomu interfejsu użytkownika lub w skryptach. Ten przewodnik zawiera szczegółowe informacje o używaniu programu SSMS do nawiązywania połączenia z bazą danych Azure SQL, a następnie wykonywania zapytania, wstawiania, aktualizowania i usuwania instrukcji.

Ten przewodnik Szybki start używa jako punktu początkowego zasobów utworzonych w jednym z poniższych przewodników Szybki start:

- [Tworzenie bazy danych — portal](sql-database-get-started-portal.md)
- [Tworzenie bazy danych — interfejs wiersza polecenia](sql-database-get-started-cli.md)
- [Tworzenie bazy danych — PowerShell](sql-database-get-started-powershell.md) 

Przed rozpoczęciem upewnij się, że zainstalowano najnowszą wersję programu [SSMS](https://msdn.microsoft.com/library/mt238290.aspx). 

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu

Pobierz w pełni kwalifikowaną nazwę serwera dla serwera Azure SQL Database w witrynie Azure Portal. W pełni kwalifikowanej nazwy serwera używa się do nawiązywania połączenia z serwerem przy użyciu programu SQL Server Management Studio.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Wybierz opcję **Bazy danych SQL** z menu po lewej stronie, a następnie kliknij bazę danych na stronie **Bazy danych SQL**. 
3. W okienku **Essentials** na stronie bazy danych w witrynie Azure Portal zlokalizuj i skopiuj **nazwę serwera**.

    <img src="./media/sql-database-connect-query-ssms/connection-information.png" alt="connection information" style="width: 780px;" />

## <a name="connect-to-the-server"></a>Nawiązywanie połączenia z serwerem

Użyj programu SQL Server Management Studio, aby nawiązać połączenie z serwerem Azure SQL Database.

1. Wpisz **SSMS** w polu wyszukiwania w systemie Windows, a następnie kliknij opcję **Uruchom**, aby otworzyć program SSMS.

2. W oknie dialogowym **Połącz z serwerem** wprowadź następujące informacje:
   - **Typ serwera**: określ aparat bazy danych
   - **Nazwa serwera**: wprowadź w pełni kwalifikowaną nazwę serwera, na przykład **mynewserver20170313.database.windows.net**
   - **Uwierzytelnianie**: określ uwierzytelnianie programu SQL Server
   - **Logowanie**: wprowadź nazwę konta administratora serwera
   - **Hasło**: wprowadź hasło konta administratora serwera
 
    <img src="./media/sql-database-connect-query-ssms/connect.png" alt="connect to server" style="width: 780px;" />

3. Kliknij przycisk **Połącz**. W programie SSMS zostanie otwarte okno Eksplorator obiektów. 

    <img src="./media/sql-database-connect-query-ssms/connected.png" alt="connected to server" style="width: 780px;" />

4. W Eksploratorze obiektów rozwiń pozycję **Bazy danych**, a następnie rozwiń pozycję **mySampleDatabase**, aby wyświetlić obiekty w przykładowej bazie danych.

## <a name="query-data"></a>Zapytania o dane

Użyj instrukcji Transact-SQL [SELECT](https://msdn.microsoft.com/library/ms189499.aspx), aby wykonać zapytanie dotyczące danych w bazie danych Azure SQL.

1. W Eksploratorze obiektów kliknij prawym przyciskiem myszy pozycję **mySampleDatabase** i kliknij opcję **Nowe zapytanie**. Zostanie otwarte puste okno zapytania, które jest połączone z Twoją bazą danych.
2. W oknie zapytania wprowadź następujące zapytanie:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. Na pasku narzędzi kliknij opcję **Wykonaj**, aby pobrać dane z tabel Product i ProductCategory.

    <img src="./media/sql-database-connect-query-ssms/query.png" alt="query" style="width: 780px;" />

## <a name="insert-data"></a>Wstawianie danych

Użyj instrukcji Transact-SQL [INSERT](https://msdn.microsoft.com/library/ms174335.aspx), aby wstawić dane do bazy danych Azure SQL.

1. Na pasku narzędzi kliknij pozycję **Nowe zapytanie**. Zostanie otwarte puste okno zapytania połączone z Twoją bazą danych.
2. W oknie zapytania wprowadź następujące zapytanie:

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

3. Na pasku narzędzi kliknij opcję **Wykonaj**, aby wstawić nowy wiersz w tabeli Product.

    <img src="./media/sql-database-connect-query-ssms/insert.png" alt="insert" style="width: 780px;" />

## <a name="update-data"></a>Aktualizowanie danych

Użyj instrukcji Transact-SQL [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx), aby zaktualizować dane w bazie danych Azure SQL.

1. Na pasku narzędzi kliknij pozycję **Nowe zapytanie**. Zostanie otwarte puste okno zapytania połączone z Twoją bazą danych.
2. W oknie zapytania wprowadź następujące zapytanie:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

3. Na pasku narzędzi kliknij opcję **Wykonaj**, aby zaktualizować określony wiersz w tabeli Product.

    <img src="./media/sql-database-connect-query-ssms/update.png" alt="update" style="width: 780px;" />

## <a name="delete-data"></a>Usuwanie danych

Użyj instrukcji Transact-SQL [DELETE](https://msdn.microsoft.com/library/ms189835.aspx), aby usunąć dane w bazie danych Azure SQL.

1. Na pasku narzędzi kliknij pozycję **Nowe zapytanie**. Zostanie otwarte puste okno zapytania połączone z Twoją bazą danych.
2. W oknie zapytania wprowadź następujące zapytanie:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

3. Na pasku narzędzi kliknij opcję **Wykonaj**, aby usunąć określony wiersz w tabeli Product.

    <img src="./media/sql-database-connect-query-ssms/delete.png" alt="delete" style="width: 780px;" />

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej informacji o programie SSMS, zobacz [Korzystanie z programu SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).
- Aby uzyskać informacje dotyczące wysyłania zapytań i edytowania danych przy użyciu programu Visual Studio Code, zobacz [Visual Studio Code](https://code.visualstudio.com/docs)

