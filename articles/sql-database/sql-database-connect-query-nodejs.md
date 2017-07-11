---
title: "Nawiązywanie połączeń z bazą danych Azure SQL Database przy użyciu środowiska Node.js | Microsoft Docs"
description: "Poniżej przedstawiony jest przykładowy kod Node.js, którego można użyć do nawiązywania połączeń z usługą Azure SQL Database i wykonywania względem niej zapytań."
services: sql-database
documentationcenter: 
author: LuisBosquez
manager: jhubbard
editor: 
ms.assetid: 53f70e37-5eb4-400d-972e-dd7ea0caacd4
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: lbosq
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: a7b1a504c6ae7850e5f986895adcf4d92fd5adc1
ms.contentlocale: pl-pl
ms.lasthandoff: 06/15/2017


---
<a id="azure-sql-database-use-nodejs-to-connect-and-query-data" class="xliff"></a>

# Azure SQL Database: używanie środowiska Node.js do nawiązywania połączeń i wysyłania zapytań dotyczących danych

W tym przewodniku Szybki start pokazano, jak używać środowiska [Node.js](https://nodejs.org/en/) w celu nawiązywania połączeń z usługą Azure SQL Database, a następnie, korzystając z instrukcji Transact-SQL, wysyłać zapytania o dane, a także wstawiać, aktualizować i usuwać dane w bazie danych z poziomu platform Windows, Ubuntu Linux i Mac.

<a id="prerequisites" class="xliff"></a>

## Wymagania wstępne

Ten przewodnik Szybki start używa jako punktu początkowego zasobów utworzonych w jednym z poniższych przewodników Szybki start:

- [Tworzenie bazy danych — portal](sql-database-get-started-portal.md)
- [Tworzenie bazy danych — interfejs wiersza polecenia](sql-database-get-started-cli.md)
- [Tworzenie bazy danych — PowerShell](sql-database-get-started-powershell.md)

<a id="install-nodejs" class="xliff"></a>

## Instalowanie środowiska Node.js 

W krokach w tej sekcji założono, że wiesz już, jak programować za pomocą środowiska Node.js, ale dopiero zaczynasz pracę z usługą Azure SQL Database. Jeśli programowanie w środowisku Node.js jest dla Ciebie czymś nowym, przejdź do tematu [Build an app using SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/) (Tworzenie aplikacji za pomocą programu SQL Server) i wybierz pozycję **Node.js**, a następnie wybierz swój system operacyjny.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Wprowadź następujące polecenia, aby zainstalować rozwiązanie **brew**, czyli łatwy w użyciu menedżer pakietów dla systemu Mac OS X i środowiska **Node.js**.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install node
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Wprowadź następujące polecenia, aby zainstalować środowisko **Node.js** i program **npm**, czyli menedżer pakietów dla środowiska Node.js.

```bash
sudo apt-get install -y nodejs npm
```

<a id="windows" class="xliff"></a>

### **Windows**
Odwiedź [stronę pobierania środowiska Node.js](https://nodejs.org/en/download/) i wybierz odpowiednią opcję Instalatora Windows.


<a id="install-the-tedious-sql-server-driver-for-nodejs" class="xliff"></a>

## Zainstaluj sterownik Tedious programu SQL Server dla środowiska Node.js
Zalecany sterownik dla środowiska Node.js to **[Tedious](https://github.com/tediousjs/tedious)**. Sterownik Tedious został utworzony w ramach inicjatywy typu open source, w której bierze udział firma Microsoft, mającej zapewnić działanie aplikacji Node.js na dowolnej platformie. Na potrzeby tego samouczka wymagany jest pusty katalog, w którym zostanie umieszczony kod oraz instalowane zależności `npm`.

Aby zainstalować sterownik **Tedious**, uruchom następujące polecenie z poziomu katalogu:

```cmd
npm install tedious
```

<a id="get-connection-information" class="xliff"></a>

## Pobieranie informacji o połączeniu

Uzyskaj parametry połączenia potrzebne do nawiązania połączenia z bazą danych Azure SQL. W następnych procedurach będą potrzebne w pełni kwalifikowana nazwa serwera, nazwa bazy danych i informacje logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Wybierz opcję **Bazy danych SQL** z menu po lewej stronie, a następnie kliknij bazę danych na stronie **Bazy danych SQL**. 
3. Na stronie **Przegląd** bazy danych zweryfikuj w pełni kwalifikowaną nazwę serwera, jak pokazano na poniższej ilustracji. Możesz umieścić kursor na nazwie serwera w celu wywołania opcji **Kliknij, aby skopiować**. 

   ![nazwa-serwera](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Jeśli nie pamiętasz informacji logowania dla serwera Azure SQL Database, przejdź do strony serwera SQL Database, aby wyświetlić nazwę administratora oraz, w razie konieczności, zresetować hasło.
    
<a id="select-data" class="xliff"></a>

## Wybieranie danych

Użyj następującego kodu, aby wykonać zapytanie względem bazy danych Azure SQL Database o 20 najpopularniejszych produktów według kategorii. Najpierw zaimportuj klasy Connect i Request sterownika z biblioteki sterownika Tedious. Następnie utwórz obiekt konfiguracji i zastąp zmienne **username**, **password**, **server** i **database** wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy danych AdventureWorksLT. Utwórz obiekt `Connection` przy użyciu określonego obiektu `config`. Następnie zdefiniuj wywołanie zwrotne dla zdarzenia `connect` obiektu `connection`, aby wykonać funkcję `queryDatabase()`.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}
var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        queryDatabase()
    }
});

function queryDatabase(){
    console.log('Reading rows from the Table...');

    // Read all rows from table
    request = new Request(
        "SELECT TOP 1 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) returned');
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

<a id="insert-data-into-the-database" class="xliff"></a>

## Wstawianie danych do bazy danych
Użyj następującego kodu, aby wstawić nowy produkt do tabeli SalesLT.Product przy użyciu instrukcji funkcji `insertIntoDatabase()` i instrukcji [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) języka Transact-SQL. Zastąp zmienne **username**, **password**, **server** i **database** wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy danych AdventureWorksLT. 

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        insertIntoDatabase()
    }
});

function insertIntoDatabase(){
    console.log("Inserting a brand new product into database...");
    request = new Request(
        "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) inserted');
        }
    );
    connection.execSql(request);
}
```

<a id="update-data-in-the-database" class="xliff"></a>

## Aktualizowanie danych w bazie danych
Użyj następującego kodu, aby usunąć wcześniej dodany nowy produkt przy użyciu funkcji `updateInDatabase()` i instrukcji [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) języka Transact-SQL. Zastąp zmienne **username**, **password**, **server** i **database** wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy danych AdventureWorksLT. W tym przykładzie użyto nazwy produktu wstawionego w poprzednim przykładzie.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        updateInDatabase()
    }
});

function updateInDatabase(){
    console.log("Updating the price of the brand new product in database...");
    request = new Request(
        "UPDATE SalesLT.Product SET ListPrice = 50 WHERE Name = 'BrandNewProduct'",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) updated');
        }
    );
    connection.execSql(request);
}
```

<a id="delete-data-from-the-database" class="xliff"></a>

## Usuwanie danych z bazy danych
Użyj następującego kodu, aby usunąć dane z bazy danych. Zastąp zmienne **username**, **password**, **server** i **database** wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy danych AdventureWorksLT. Tym razem użyj **instrukcji DELETE** w funkcji `deleteFromDatabase()`. W tym przykładzie również użyto nazwy produktu wstawionego w poprzednim przykładzie.

```js
var Connection = require('tedious').Connection;
var Request = require('tedious').Request;


// Create connection to database
var config = {
  userName: 'your_username', // update me
  password: 'your_password', // update me
  server: 'your_server.database.windows.net', // update me
  options: {
      database: 'your_database' //update me
  }
}

var connection = new Connection(config);

// Attempt to connect and execute queries if connection goes through
connection.on('connect', function(err) {
    if (err) {
        console.log(err)
    }
    else{
        deleteFromDatabase()
    }
});

function deleteFromDatabase(){
    console.log("Deleting the brand new product in database...");
    request = new Request(
        "DELETE FROM SalesLT.Product WHERE Name = 'BrandNewProduct'",
        function(err, rowCount, rows) {
            console.log(rowCount + ' row(s) returned');
        }
    );
    connection.execSql(request);
}
```


<a id="next-steps" class="xliff"></a>

## Następne kroki
- [Projektowanie pierwszej bazy danych SQL na platformie Azure](sql-database-design-first-database.md)
- [Sterownik firmy Microsoft środowiska Node.js dla programu SQL Server](https://docs.microsoft.com/sql/connect/node-js/node-js-driver-for-sql-server/)
- [Nawiązywać połączenia i wykonywać zapytania za pomocą programu SSMS](sql-database-connect-query-ssms.md).
- [Nawiązywanie połączeń i tworzenie zapytań przy użyciu narzędzia Visual Studio Code](sql-database-connect-query-vscode.md).



