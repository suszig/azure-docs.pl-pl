---
title: "Nawiązywanie połączenia z bazą danych Azure SQL Database przy użyciu środowiska PHP | Microsoft Docs"
description: "Przykładowy kod PHP, którego można użyć do nawiązywania połączeń z usługą Azure SQL Database i wykonywania względem niej zapytań."
services: sql-database
documentationcenter: 
author: meet-bhagdev
manager: jhubbard
editor: 
ms.assetid: 4e71db4a-a22f-4f1c-83e5-4a34a036ecf3
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: meetb
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 5b90c9b49448c6bc225f9d6ba7227782b81fc5ed
ms.contentlocale: pl-pl
ms.lasthandoff: 06/15/2017


---
<a id="azure-sql-database-use-php-to-connect-and-query-data" class="xliff"></a>

# Azure SQL Database: używanie kodu PHP do nawiązywania połączeń i wykonywania zapytań względem danych

W tym przewodniku Szybki start pokazano, jak używać kodu [PHP](http://php.net/manual/en/intro-whatis.php) w celu nawiązywania połączeń z usługą Azure SQL Database, a następnie, korzystając z instrukcji Transact-SQL, wysyłać zapytania o dane, a także wstawiać, aktualizować i usuwać dane w bazie danych z poziomu platform Mac OS, Ubuntu Linux i Windows.

<a id="prerequisites" class="xliff"></a>

## Wymagania wstępne

Ten przewodnik Szybki start używa jako punktu początkowego zasobów utworzonych w jednym z poniższych przewodników Szybki start:

- [Tworzenie bazy danych — portal](sql-database-get-started-portal.md)
- [Tworzenie bazy danych — interfejs wiersza polecenia](sql-database-get-started-cli.md)
- [Tworzenie bazy danych — PowerShell](sql-database-get-started-powershell.md)

<a id="install-php-and-database-communications-software" class="xliff"></a>

## Instalowanie środowiska PHP i oprogramowania do komunikacji z bazą danych

W krokach w tej sekcji założono, że wiesz już, jak programować w środowisku PHP, ale dopiero zaczynasz pracę z usługą Azure SQL Database. Jeśli programowanie w środowisku PHP jest dla Ciebie czymś nowym, przejdź do tematu [Build an app using SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/) (Tworzenie aplikacji za pomocą programu SQL Server) i wybierz pozycję **PHP**, a następnie wybierz swój system operacyjny.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Otwórz terminal i wprowadź następujące polecenia, aby zainstalować rozwiązanie **brew**, sterownik **Microsoft ODBC dla komputerów Mac** i sterowniki **PHP firmy Microsoft dla programu SQL Server**. 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install msodbcsql 
#for silent install ACCEPT_EULA=y brew install msodbcsql 
pecl install sqlsrv-4.1.7preview
pecl install pdo_sqlsrv-4.1.7preview
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Wprowadź następujące polecenia, aby zainstalować sterownik **Microsoft ODBC dla systemu Linux** i sterowniki **PHP firmy Microsoft dla programu SQL Server**.

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql.list
exit
sudo apt-get update
sudo apt-get install msodbcsql mssql-tools unixodbc-dev gcc g++ php-dev
sudo pecl install sqlsrv pdo_sqlsrv
sudo echo "extension= pdo_sqlsrv.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`
sudo echo "extension= sqlsrv.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`
```

<a id="windows" class="xliff"></a>

### **Windows**
- Instalowanie środowiska PHP 7.1.1 (x64) [za pomocą instalatora WebPlatform](https://www.microsoft.com/web/downloads/platform.aspx?lang=) 
- Zainstaluj sterownik [Microsoft ODBC Driver 13.1](https://www.microsoft.com/download/details.aspx?id=53339). 
- Pobierz biblioteki DLL Non-Thread Safe dla sterownika [PHP firmy Microsoft dla programu SQL Server](https://pecl.php.net/package/sqlsrv/4.1.6.1/windows) i umieść pliki binarne w folderze PHP\v7.x\ext.
- Następnie edytuj plik php.ini (C:\Program Files\PHP\v7.1\php.ini), dodając odwołanie do biblioteki DLL. Na przykład:
      
      extension=php_sqlsrv.dll
      extension=php_pdo_sqlsrv.dll

W tym momencie biblioteki DLL powinny być zarejestrowany w środowisku PHP.

<a id="get-connection-information" class="xliff"></a>

## Pobieranie informacji o połączeniu

Uzyskaj parametry połączenia potrzebne do nawiązania połączenia z bazą danych Azure SQL. W następnych procedurach będą potrzebne w pełni kwalifikowana nazwa serwera, nazwa bazy danych i informacje logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Wybierz opcję **Bazy danych SQL** z menu po lewej stronie, a następnie kliknij bazę danych na stronie **Bazy danych SQL**. 
3. Na stronie **Przegląd** bazy danych zweryfikuj w pełni kwalifikowaną nazwę serwera, jak pokazano na poniższej ilustracji. Możesz umieścić kursor na nazwie serwera w celu wywołania opcji **Kliknij, aby skopiować**.  

   ![nazwa-serwera](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Jeśli nie pamiętasz informacji logowania do serwera, przejdź do strony serwera usługi SQL Database, aby wyświetlić nazwę administratora serwera oraz, w razie konieczności, zresetować hasło.     
    
<a id="select-data" class="xliff"></a>

## Wybieranie danych
Użyj następującego kodu, aby wykonać zapytanie o 20 najpopularniejszych produktów według kategorii, używając funkcji [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) wraz z instrukcją [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) języka Transact-SQL. Funkcja sqlsrv_query służy do pobierania zestawu wyników z zapytania względem bazy danych SQL. Ta funkcja akceptuje zapytanie i zwraca zestaw wyników, względem którego można przeprowadzić iterację przy użyciu funkcji [sqlsrv_fetch_array()](http://php.net/manual/en/function.sqlsrv-fetch-array.php). Zastąp parametry server, database, username i password wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy danych AdventureWorksLT. 

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


<a id="insert-data" class="xliff"></a>

## Wstawianie danych
Użyj następującego kodu, aby wstawić nowy produkt do tabeli SalesLT.Product przy użyciu instrukcji funkcji [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) i instrukcji [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) języka Transact-SQL. Zastąp parametry server, database, username i password wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy danych AdventureWorksLT. 

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
$tsql= "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";
$params = array("BrandNewProduct", "200989", "Blue", 75, 80, "7/1/2016");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) inserted" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
?>
```

<a id="update-data" class="xliff"></a>

## Aktualizowanie danych
Użyj następującego kodu, aby zaktualizować dane w bazie danych Azure SQL Database przy użyciu instrukcji funkcji [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) i instrukcji [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) języka Transact-SQL. Zastąp parametry server, database, username i password wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy danych AdventureWorksLT.

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
$tsql= "UPDATE SalesLT.Product SET ListPrice =? WHERE Name = ?";
$params = array(50,"BrandNewProduct");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) updated" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
?>
```

<a id="delete-data" class="xliff"></a>

## Usuwanie danych
Użyj następującego kodu, aby usunąć wcześniej dodany nowy produkt przy użyciu funkcji [sqlsrv_query()](https://docs.microsoft.com/sql/connect/php/sqlsrv-query) i instrukcji [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) języka Transact-SQL. Zastąp parametry server, database, username i password wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy danych AdventureWorksLT.

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
$tsql= "DELETE FROM SalesLT.Product WHERE Name = ?";
$params = array("BrandNewProduct");
$getResults= sqlsrv_query($conn, $tsql, $params);
if ($getResults == FALSE)
    echo print_r(sqlsrv_errors(), true);
else{
    $rowsAffected = sqlsrv_rows_affected($getResults);
    echo ($rowsAffected. " row(s) deleted" . PHP_EOL);
    sqlsrv_free_stmt($getResults);
}
```

<a id="next-steps" class="xliff"></a>

## Następne kroki
- [Projektowanie pierwszej bazy danych SQL na platformie Azure](sql-database-design-first-database.md)
- Sterowniki [PHP firmy Microsoft dla programu SQL Server](https://github.com/Microsoft/msphpsql/)
- [Zgłaszanie problemów/zadawanie pytań](https://github.com/Microsoft/msphpsql/issues)


