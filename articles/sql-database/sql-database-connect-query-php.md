---
title: "Korzystanie z języka PHP do wykonywania zapytań w bazie danych Azure SQL | Microsoft Docs"
description: "W tym temacie przedstawiono sposób użycia języka PHP do utworzenia programu, który nawiązuje połączenie z bazą danych SQL Azure i wykonuje zapytania za pomocą instrukcji języka Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a 22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: quickstart
ms.date: 08/08/2017
ms.author: carlrab
ms.openlocfilehash: 1cf95cf2c9413f99a4013ff961c8a18c31cc51f3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-php-to-query-an-azure-sql-database"></a>Korzystanie z języka PHP do wykonywania zapytań w bazie danych Azure SQL

W tym przewodniku Szybki start pokazano, jak używać języka [PHP](http://php.net/manual/en/intro-whatis.php) w celu utworzenia programu służącego do nawiązywania połączenia z bazą danych Azure SQL, a następnie, korzystając z instrukcji Transact-SQL, wysyłać zapytania o dane.

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten samouczek Szybki start, upewnij się, że dysponujesz następującymi elementami:

- Baza danych Azure SQL. Ten przewodnik Szybki start używa zasobów utworzonych w jednym z poniższych przewodników Szybki start: 

   - [Tworzenie bazy danych — portal](sql-database-get-started-portal.md)
   - [Tworzenie bazy danych — interfejs wiersza polecenia](sql-database-get-started-cli.md)
   - [Tworzenie bazy danych — PowerShell](sql-database-get-started-powershell.md)

- [Reguła zapory poziomu serwera](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) dla publicznego adresu IP komputera, który będzie używany w tym samouczku Szybki start.

- W systemie operacyjnym zainstalowano język PHP i związane z nim oprogramowanie.

    - **System MacOS**: zainstaluj oprogramowania Homebrew i PHP, zainstaluj sterownik ODBC i pakiet SQLCMD, a następnie zainstaluj sterownik języka PHP dla programu SQL Server. Zobacz [kroki 1.2, 1.3 i 2.1](https://www.microsoft.com/en-us/sql-server/developer-get-started/php/mac/).
    - **System Ubuntu**: zainstaluj język PHP i inne wymagane pakiety, a następnie zainstaluj sterownik języka PHP dla programu SQL Server. Zobacz [kroki 1.2 i 2.1](https://www.microsoft.com/sql-server/developer-get-started/php/ubuntu/).
    - **System Windows**: zainstaluj najnowszą wersję programu PHP dla usług IIS Express, najnowszą wersja sterowników firmy Microsoft dla programu SQL Server w usługach IIS Express, Chocolatey, sterownik ODBC i pakiet SQLCMD. Zobacz [kroki 1.2 i 1.3](https://www.microsoft.com/sql-server/developer-get-started/php/windows/).    

## <a name="sql-server-connection-information"></a>Informacje o połączeniu z serwerem SQL

Uzyskaj parametry połączenia potrzebne do nawiązania połączenia z bazą danych Azure SQL. W następnych procedurach będą potrzebne w pełni kwalifikowana nazwa serwera, nazwa bazy danych i informacje logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Wybierz opcję **Bazy danych SQL** z menu po lewej stronie, a następnie kliknij bazę danych na stronie **Bazy danych SQL**. 
3. Na stronie **Przegląd** bazy danych zweryfikuj w pełni kwalifikowaną nazwę serwera, jak pokazano na poniższej ilustracji. Możesz umieścić kursor na nazwie serwera w celu wywołania opcji **Kliknij, aby skopiować**.  

   ![nazwa-serwera](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Jeśli nie pamiętasz informacji logowania do serwera, przejdź do strony serwera usługi SQL Database, aby wyświetlić nazwę administratora serwera oraz, w razie konieczności, zresetować hasło.     
    
## <a name="insert-code-to-query-sql-database"></a>Wstawianie kodu zapytania bazy danych SQL

1. W swoim ulubionym edytorze tekstów utwórz nowy plik o nazwie **sqltest.php**.  

2. Zastąp jego zawartość następującym kodem i dodaj odpowiednie wartości dla serwera, bazy danych, użytkownika i hasła.

   ```PHP
   <?php
   $serverName = "your_server.database.windows.net";
   $connectionOptions = array(
       "Database" => "your_database",
       "Uid" => "your_username",
       "PWD" => "your_password"
   );
   //Establishes the connection
   $conn = sqlsrv_connect($serverName, $connectionOptions);
   $tsql= "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
           FROM [SalesLT].[ProductCategory] pc
           JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid";
   $getResults= sqlsrv_query($conn, $tsql);
   echo ("Reading data from table" . PHP_EOL);
   if ($getResults == FALSE)
       echo (sqlsrv_errors());
   while ($row = sqlsrv_fetch_array($getResults, SQLSRV_FETCH_ASSOC)) {
    echo ($row['CategoryName'] . " " . $row['ProductName'] . PHP_EOL);
   }
   sqlsrv_free_stmt($getResults);
   ?>
   ```

## <a name="run-the-code"></a>Uruchamianie kodu

1. W wierszu polecenia uruchom następujące polecenia:

   ```php
   php sqltest.php
   ```

2. Sprawdź, czy zostało zwróconych 20 pierwszych wierszy, a następnie zamknij okno aplikacji.

## <a name="next-steps"></a>Następne kroki
- [Projektowanie pierwszej bazy danych SQL na platformie Azure](sql-database-design-first-database.md)
- Sterowniki [PHP firmy Microsoft dla programu SQL Server](https://github.com/Microsoft/msphpsql/)
- [Zgłaszanie problemów/zadawanie pytań](https://github.com/Microsoft/msphpsql/issues)
