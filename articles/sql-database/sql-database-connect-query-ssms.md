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
ms.custom: quick start manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/15/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: c173f1b6937739f662eb41aa1886e66cb06ed729
ms.lasthandoff: 04/12/2017


---
# <a name="azure-sql-database-use-sql-server-management-studio-to-connect-and-query-data"></a>Azure SQL Database: używanie programu SQL Server Management Studio do nawiązywania połączenia i wykonywania zapytań dotyczących danych

[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) to narzędzie do zarządzania umożliwiające tworzenie zasobów SQL Server i zarządzanie nimi z poziomu interfejsu użytkownika lub w skryptach. W tym przewodniku Szybki start pokazano, jak używać narzędzia SSMS w celu nawiązywania połączenia z usługą Azure SQL Database, a następnie, korzystając z instrukcji Transact-SQL, wysyłać zapytania o dane, a także wstawiać, aktualizować i usuwać dane z bazy danych. 

Ten przewodnik Szybki start używa jako punktu początkowego zasobów utworzonych w jednym z poniższych przewodników Szybki start:

- [Tworzenie bazy danych — portal](sql-database-get-started-portal.md)
- [Tworzenie bazy danych — interfejs wiersza polecenia](sql-database-get-started-cli.md)

Przed rozpoczęciem upewnij się, że zainstalowano najnowszą wersję programu [SSMS](https://msdn.microsoft.com/library/mt238290.aspx). 

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu

Pobierz w pełni kwalifikowaną nazwę serwera dla serwera Azure SQL Database w witrynie Azure Portal. W pełni kwalifikowanej nazwy serwera używa się do nawiązywania połączenia z serwerem przy użyciu programu SQL Server Management Studio.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Wybierz opcję **Bazy danych SQL** z menu po lewej stronie, a następnie kliknij bazę danych na stronie **Bazy danych SQL**. 
3. W okienku **Essentials** na stronie bazy danych w witrynie Azure Portal zlokalizuj i skopiuj **nazwę serwera**.

   ![informacje o połączeniu](./media/sql-database-connect-query-ssms/connection-information.png) 


## <a name="connect-to-the-server-and-your-new-database"></a>Łączenie z serwerem i nową bazą danych

Użyj programu SQL Server Management Studio, aby nawiązać połączenie z serwerem Azure SQL Database. 

> [!IMPORTANT]
> Serwer logiczny usługi Azure SQL Database nasłuchuje na porcie 1433. Jeśli próbujesz nawiązać połączenie z serwerem logicznym usługi Azure SQL Database za pośrednictwem zapory firmowej, to aby połączenie się powiodło, ten port musi być otwarty w zaporze firmowej.
>

1. Otwórz program SQL Server Management Studio.

2. W oknie dialogowym **Połącz z serwerem** wprowadź następujące informacje:
   - **Typ serwera**: określ aparat bazy danych
   - **Nazwa serwera**: wprowadź w pełni kwalifikowaną nazwę serwera, na przykład **mynewserver20170313.database.windows.net**
   - **Uwierzytelnianie**: określ uwierzytelnianie programu SQL Server
   - **Logowanie**: wprowadź nazwę konta administratora serwera
   - **Hasło**: wprowadź hasło konta administratora serwera

   ![łączenie z serwerem](./media/sql-database-connect-query-ssms/connect.png)  

3. Kliknij pozycję **Opcje**. W sekcji **Nawiązywanie połączenia z bazą danych** wprowadź ciąg **mySampleDatabase**, aby nawiązać połączenie z utworzoną uprzednio bazą danych.

   ![nawiązywanie połączenia z bazą danych na serwerze](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Kliknij przycisk **Połącz**. W programie SSMS zostanie otwarte okno Eksplorator obiektów. 

   ![nawiązane połączenie z serwerem](./media/sql-database-connect-query-ssms/connected.png)  

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

1. W oknie zapytania zastąp poprzednie zapytanie następującym zapytaniem:

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

2. Na pasku narzędzi kliknij opcję **Wykonaj**, aby wstawić nowy wiersz w tabeli Product.

    <img src="./media/sql-database-connect-query-ssms/insert.png" alt="insert" style="width: 780px;" />

## <a name="update-data"></a>Aktualizowanie danych

Użyj instrukcji Transact-SQL [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx), aby zaktualizować dane w bazie danych Azure SQL.

1. W oknie zapytania zastąp poprzednie zapytanie następującym zapytaniem:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Na pasku narzędzi kliknij opcję **Wykonaj**, aby zaktualizować określony wiersz w tabeli Product.

    <img src="./media/sql-database-connect-query-ssms/update.png" alt="update" style="width: 780px;" />

## <a name="delete-data"></a>Usuwanie danych

Użyj instrukcji Transact-SQL [DELETE](https://msdn.microsoft.com/library/ms189835.aspx), aby usunąć dane w bazie danych Azure SQL.

1. W oknie zapytania zastąp poprzednie zapytanie następującym zapytaniem:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Na pasku narzędzi kliknij opcję **Wykonaj**, aby usunąć określony wiersz w tabeli Product.

    <img src="./media/sql-database-connect-query-ssms/delete.png" alt="delete" style="width: 780px;" />

## <a name="next-steps"></a>Następne kroki

- Aby uzyskać więcej informacji o programie SSMS, zobacz [Korzystanie z programu SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).
- Aby nawiązywać połączenia i wykonywać zapytania za pomocą programu Visual Studio Code, zobacz [Connect and query with Visual Studio Code](sql-database-connect-query-vscode.md) (Nawiązywanie połączeń i wykonywanie zapytań za pomocą programu Visual Studio Code).
- Aby nawiązywać połączenia i wykonywać zapytania za pomocą platformy .NET, zobacz [Connect and query with .NET](sql-database-connect-query-dotnet.md) (Nawiązywanie połączeń i wykonywanie zapytań za pomocą platformy .NET).
- Aby nawiązywać połączenia i wykonywać zapytania za pomocą języka PHP, zobacz [Connect and query with PHP](sql-database-connect-query-php.md) (Nawiązywanie połączeń i wykonywanie zapytań za pomocą języka PHP).
- Aby nawiązywać połączenia i wykonywać zapytania za pomocą oprogramowania Node.js, zobacz [Connect and query with Node.js](sql-database-connect-query-nodejs.md) (Nawiązywanie połączeń i wykonywanie zapytań za pomocą oprogramowania Node.js).
- Aby nawiązywać połączenia i wykonywać zapytania za pomocą języka Java, zobacz [Connect and query with Java](sql-database-connect-query-java.md) (Nawiązywanie połączeń i wykonywanie zapytań za pomocą języka Java).
- Aby nawiązywać połączenia i wykonywać zapytania za pomocą języka Python, zobacz [Connect and query with Python](sql-database-connect-query-python.md) (Nawiązywanie połączeń i wykonywanie zapytań za pomocą języka Python).
- Aby nawiązywać połączenia i wykonywać zapytania za pomocą języka Ruby, zobacz [Connect and query with Ruby](sql-database-connect-query-ruby.md) (Nawiązywanie połączeń i wykonywanie zapytań za pomocą języka Ruby).

