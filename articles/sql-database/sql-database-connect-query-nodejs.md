---
title: "Korzystanie z narzędzia Node.js do wykonywania zapytań w bazie danych Azure SQL | Microsoft Docs"
description: "W tym temacie przedstawiono sposób użycia narzędzia Node.js do utworzenia programu, który nawiązuje połączenie z bazą danych SQL Azure i wykonuje zapytania za pomocą instrukcji języka Transact-SQL."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 53f70e37-5eb4-400d-972e-dd7ea0caacd4
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: d2765d4606bf6f926def15788ab23efd2a000ad4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-nodejs-to-query-an-azure-sql-database"></a>Korzystanie z narzędzia Node.js do wykonywania zapytań w bazie danych Azure SQL

W tym przewodniku Szybki start pokazano, jak używać narzędzia [Node.js](https://nodejs.org/en/) w celu utworzenia programu służącego do nawiązywania połączenia z bazą danych Azure SQL, a następnie, korzystając z instrukcji Transact-SQL, wysyłać zapytania o dane.

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć ten samouczek Szybki start, upewnij się, że dysponujesz następującymi elementami:

- Baza danych Azure SQL. Ten przewodnik Szybki start używa zasobów utworzonych w jednym z poniższych przewodników Szybki start: 

   - [Tworzenie bazy danych — portal](sql-database-get-started-portal.md)
   - [Tworzenie bazy danych — interfejs wiersza polecenia](sql-database-get-started-cli.md)
   - [Tworzenie bazy danych — PowerShell](sql-database-get-started-powershell.md)

- [Reguła zapory poziomu serwera](sql-database-get-started-portal.md#create-a-server-level-firewall-rule) dla publicznego adresu IP komputera, który będzie używany w tym samouczku Szybki start.
- W systemie operacyjnym zainstalowano narzędzie Node.js i związane z nim oprogramowanie.
    - **System MacOS**: zainstaluj oprogramowanie Homebrew i Node.js, a następnie zainstaluj sterownik ODBC i program SQLCMD. Zobacz [kroki 1.2 i 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/mac/).
    - **System Ubuntu**: zainstaluj oprogramowanie Node.js, a następnie zainstaluj sterownik ODBC i program SQLCMD. Zobacz [kroki 1.2 i 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/ubuntu/).
    - **System Windows**: zainstaluj oprogramowanie Chocolatey i Node.js, a następnie zainstaluj sterownik ODBC i program SQLCMD. Zobacz [kroki 1.2 i 1.3](https://www.microsoft.com/sql-server/developer-get-started/node/windows/).

## <a name="sql-server-connection-information"></a>Informacje o połączeniu z serwerem SQL

Uzyskaj parametry połączenia potrzebne do nawiązania połączenia z bazą danych Azure SQL. W następnych procedurach będą potrzebne w pełni kwalifikowana nazwa serwera, nazwa bazy danych i informacje logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Wybierz opcję **Bazy danych SQL** z menu po lewej stronie, a następnie kliknij bazę danych na stronie **Bazy danych SQL**. 
3. Na stronie **Przegląd** bazy danych zweryfikuj w pełni kwalifikowaną nazwę serwera, jak pokazano na poniższej ilustracji. Możesz umieścić kursor na nazwie serwera w celu wywołania opcji **Kliknij, aby skopiować**. 

   ![nazwa-serwera](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Jeśli nie pamiętasz informacji logowania dla serwera Azure SQL Database, przejdź do strony serwera SQL Database, aby wyświetlić nazwę administratora oraz, w razie konieczności, zresetować hasło.

> [!IMPORTANT]
> W przypadku publicznego adresu IP komputera, na którym jest wykonywany ten samouczek, niezbędne jest posiadanie reguły zapory. Jeśli pracujesz na innym komputerze lub masz inny publiczny adres IP, utwórz [regułę zapory poziomu serwera przy użyciu portalu Azure](sql-database-get-started-portal.md#create-a-server-level-firewall-rule). 

## <a name="create-a-nodejs-project"></a>Tworzenie projektu z użyciem narzędzia Node.js

Otwórz wiersz polecenia i utwórz folder o nazwie *sqltest*. Przejdź do utworzonego folderu i uruchom następujące polecenie:

    
    npm init -y
    npm install tedious
    npm install async
    

## <a name="insert-code-to-query-sql-database"></a>Wstawianie kodu zapytania bazy danych SQL

1. W środowisku projektowym lub ulubionym edytorze tekstów utwórz nowy plik o nazwie **sqltest.js**.

2. Zastąp jego zawartość następującym kodem i dodaj odpowiednie wartości dla serwera, bazy danych, użytkownika i hasła.

   ```js
   var Connection = require('tedious').Connection;
   var Request = require('tedious').Request;

   // Create connection to database
   var config = 
      {
        userName: 'someuser', // update me
        password: 'somepassword', // update me
        server: 'edmacasqlserver.database.windows.net', // update me
        options: 
           {
              database: 'somedb' //update me
              , encrypt: true
           }
      }
   var connection = new Connection(config);

   // Attempt to connect and execute queries if connection goes through
   connection.on('connect', function(err) 
      {
        if (err) 
          {
             console.log(err)
          }
       else
          {
              queryDatabase()
          }
      }
    );

   function queryDatabase()
      { console.log('Reading rows from the Table...');

          // Read all rows from table
        request = new Request(
             "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
                function(err, rowCount, rows) 
                   {
                       console.log(rowCount + ' row(s) returned');
                       process.exit();
                   }
               );
    
        request.on('row', function(columns) {
           columns.forEach(function(column) {
               console.log("%s\t%s", column.metadata.colName, column.value);
            });
                });
        connection.execSql(request);
      }
```

## <a name="run-the-code"></a>Uruchamianie kodu

1. W wierszu polecenia uruchom następujące polecenia:

   ```js
   node sqltest.js
   ```

2. Sprawdź, czy zostało zwróconych 20 pierwszych wierszy, a następnie zamknij okno aplikacji.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [sterowniku firmy Microsoft środowiska Node.js dla programu SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)
- Dowiedz się, jak [uzyskać połączenie i wykonywać zapytania bazy danych Azure SQL przy użyciu platformy .NET Core](sql-database-connect-query-dotnet-core.md) w systemach operacyjnych Windows/Linux/macOS.  
- Dowiedz się więcej o [rozpoczynaniu pracy z platformą .NET Core w systemie Windows/Linux/macOS przy użyciu wiersza polecenia](/dotnet/core/tutorials/using-with-xplat-cli).
- Dowiedz się, jak [zaprojektować pierwszą bazę danych Azure SQL przy użyciu narzędzia SSMS](sql-database-design-first-database.md) lub [zaprojektować pierwszą bazę danych Azure SQL przy użyciu platformy .NET](sql-database-design-first-database-csharp.md).
- Dowiedz się, jak [nawiązywać połączenia i wykonywać zapytania za pomocą programu SSMS](sql-database-connect-query-ssms.md).
- Dowiedz się, jak [nawiązywać połączenia i tworzyć zapytania przy użyciu narzędzia Visual Studio Code](sql-database-connect-query-vscode.md).


