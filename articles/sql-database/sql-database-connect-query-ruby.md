---
title: "Połączenie z usługą Azure SQL Database przy użyciu języka Ruby | Microsoft Docs"
description: "W artykule przedstawiono przykładowy kod języka Ruby, którego można użyć do łączenia z usługą Azure SQL Database oraz wysyłania do niej zapytań."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 94fec528-58ba-4352-ba0d-25ae4b273e90
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: andrela
ms.translationtype: Human Translation
ms.sourcegitcommit: 6adaf7026d455210db4d7ce6e7111d13c2b75374
ms.openlocfilehash: c5d09cf03c87c8da1d8588be62fea3f0cc3eec4f
ms.contentlocale: pl-pl
ms.lasthandoff: 06/22/2017

---

# <a name="azure-sql-database-use-ruby-to-connect-and-query-data"></a>Azure SQL Database: używanie języka Ruby do łączenia i wysyłania zapytań o dane

W tym przewodniku Szybki start pokazano, jak używać języka [Ruby](https://www.ruby-lang.org) w celu nawiązywania połączeń z usługą Azure SQL Database, a następnie, korzystając z instrukcji Transact-SQL, wysyłać zapytania o dane, a także wstawiać, aktualizować i usuwać dane w bazie danych z poziomu platform Mac OS i Ubuntu Linux.

## <a name="prerequisites"></a>Wymagania wstępne

Ten przewodnik Szybki start używa jako punktu początkowego zasobów utworzonych w jednym z poniższych przewodników Szybki start:

- [Tworzenie bazy danych — portal](sql-database-get-started-portal.md)
- [Tworzenie bazy danych — interfejs wiersza polecenia](sql-database-get-started-cli.md)
- [Tworzenie bazy danych — PowerShell](sql-database-get-started-powershell.md)

## <a name="install-ruby-and-database-communication-libraries"></a>Instalowanie bibliotek komunikacji języka Ruby i bazy danych

W krokach w tej sekcji założono, że wiesz już, jak programować za pomocą języka Ruby, ale dopiero zaczynasz pracę z usługą Azure SQL Database. Jeśli programowanie za pomocą języka Ruby jest dla Ciebie czymś nowym, przejdź do artykułu [Build an app using SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/) (Tworzenie aplikacji za pomocą programu SQL Server) i wybierz pozycję **Ruby**, a następnie wybierz swój system operacyjny.

### <a name="mac-os"></a>**Mac OS**
Otwórz terminal i przejdź do katalogu, w którym planujesz utworzyć skrypt języka Ruby. Wprowadź następujące polecenia, aby zainstalować rozwiązania **brew**, **FreeTDS** i **TinyTDS**.

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install FreeTDS
gem install tiny_tds
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Otwórz terminal i przejdź do katalogu, w którym planujesz utworzyć skrypt języka Ruby. Wprowadź następujące polecenia, aby zainstalować rozwiązania **FreeTDS** i **TinyTDS**.

```bash
wget ftp://ftp.freetds.org/pub/freetds/stable/freetds-1.00.27.tar.gz
tar -xzf freetds-1.00.27.tar.gz
cd freetds-1.00.27
./configure --prefix=/usr/local --with-tdsver=7.3
make
make install
gem install tiny_tds
```

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu

Uzyskaj parametry połączenia potrzebne do nawiązania połączenia z bazą danych Azure SQL. W następnych procedurach będą potrzebne w pełni kwalifikowana nazwa serwera, nazwa bazy danych i informacje logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Wybierz opcję **Bazy danych SQL** z menu po lewej stronie, a następnie kliknij bazę danych na stronie **Bazy danych SQL**. 
3. Na stronie **Przegląd** bazy danych zweryfikuj w pełni kwalifikowaną nazwę serwera, jak pokazano na poniższej ilustracji. Możesz umieścić kursor na nazwie serwera w celu wywołania opcji **Kliknij, aby skopiować**. 

   ![nazwa-serwera](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Jeśli nie pamiętasz informacji logowania do serwera, przejdź do strony serwera usługi SQL Database, aby wyświetlić nazwę administratora serwera oraz, w razie konieczności, zresetować hasło.
    

## <a name="select-data"></a>Wybieranie danych
Użyj następującego kodu, aby wykonać zapytanie o 20 najpopularniejszych produktów według kategorii, używając funkcji [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) wraz z instrukcją [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) języka Transact-SQL. Funkcja TinyTDS::Client akceptuje zapytanie i zwraca zestaw wyników. Zestaw wyników jest iterowany przy użyciu polecenia [result.each do |row|](https://github.com/rails-sqlserver/tiny_tds). Zastąp parametry server, database, username i password wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy AdventureWorksLT.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

puts "Reading data from table"
tsql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
        FROM [SalesLT].[ProductCategory] pc
        JOIN [SalesLT].[Product] p
        ON pc.productcategoryid = p.productcategoryid"
result = client.execute(tsql)
result.each do |row|
    puts row
end
```

## <a name="insert-data"></a>Wstawianie danych
Użyj poniższego kodu, aby wstawić nowy produkt do tabeli SalesLT.Product przy użyciu funkcji [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) i instrukcji [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) języka Transact-SQL. Zastąp parametry server, database, username i password wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy AdventureWorksLT.

W tym przykładzie pokazano, jak bezpiecznie wykonać instrukcję INSERT, przekazać parametry, które chronią aplikację przed luką w zabezpieczeniach umożliwiającą [wstrzyknięcie kodu SQL](https://technet.microsoft.com/library/ms161953(v=sql.105).aspx), i pobrać wygenerowaną automatycznie wartość [Klucz podstawowy](https://docs.microsoft.com/sql/relational-databases/tables/primary-and-foreign-key-constraints).    
  
Aby użyć rozwiązania TinyTDS na platformie Azure, dobrze jest wykonać kilka instrukcji `SET` w celu zmienienia sposobu obsługi określonych informacji przez bieżącą sesję. Zalecane instrukcje `SET` znajdują się w przykładowym kodzie. Na przykład instrukcja `SET ANSI_NULL_DFLT_ON` umożliwi utworzenie nowych kolumn w celu dopuszczenia wartości null, nawet jeśli stan dopuszczania wartości null dla kolumny nie został jawnie określony.  
  
Aby zachować zgodność z formatem [datetime](https://docs.microsoft.com/sql/t-sql/data-types/datetime-transact-sql) platformy Microsoft SQL Server, użyj funkcji [strftime](http://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime), aby rzutować na odpowiedni format datetime.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

# settings for Azure
result = client.execute("SET ANSI_NULLS ON")
result = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")
result = client.execute("SET ANSI_NULL_DFLT_ON ON")
result = client.execute("SET IMPLICIT_TRANSACTIONS OFF")
result = client.execute("SET ANSI_PADDING ON")
result = client.execute("SET QUOTED_IDENTIFIER ON")
result = client.execute("SET ANSI_WARNINGS ON")
result = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")

def insert(name, productnumber, color, standardcost, listprice, sellstartdate)
    tsql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) 
        VALUES (N'#{name}', N'#{productnumber}',N'#{color}',N'#{standardcost}',N'#{listprice}',N'#{sellstartdate}')"
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
insert('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')
```

## <a name="update-data"></a>Aktualizowanie danych
Użyj następującego kodu, aby zaktualizować wcześniej dodany nowy produkt przy użyciu funkcji [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) i instrukcji [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) języka Transact-SQL. Zastąp parametry server, database, username i password wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy AdventureWorksLT.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true
    
def update(name, listPrice, client)
    tsql = "UPDATE SalesLT.Product SET ListPrice = N'#{listPrice}' WHERE Name =N'#{name}'";
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
update('BrandNewProduct', 500, client)
```

## <a name="delete-data"></a>Usuwanie danych
Użyj następującego kodu, aby usunąć wcześniej dodany nowy produkt przy użyciu funkcji [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) i instrukcji [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) języka Transact-SQL. Zastąp parametry server, database, username i password wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy AdventureWorksLT.

```ruby
require 'tiny_tds'
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
client = TinyTds::Client.new username: username, password: password, 
    host: server, port: 1433, database: database, azure: true

# settings for Azure
result = client.execute("SET ANSI_NULLS ON")
result = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")
result = client.execute("SET ANSI_NULL_DFLT_ON ON")
result = client.execute("SET IMPLICIT_TRANSACTIONS OFF")
result = client.execute("SET ANSI_PADDING ON")
result = client.execute("SET QUOTED_IDENTIFIER ON")
result = client.execute("SET ANSI_WARNINGS ON")
result = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")

def delete(name, client)
    tsql = "DELETE FROM SalesLT.Product WHERE Name = N'#{name}'"
    result = client.execute(tsql)
    result.each
    puts "#{result.affected_rows} row(s) affected"
end
delete('BrandNewProduct', client)
```

## <a name="next-steps"></a>Następne kroki
- [Projektowanie pierwszej bazy danych SQL na platformie Azure](sql-database-design-first-database.md)
- [Repozytorium GitHub dla rozwiązania TinyTDS](https://github.com/rails-sqlserver/tiny_tds)
- [Zgłaszanie problemów/zadawanie pytań](https://github.com/rails-sqlserver/tiny_tds/issues)
- [Sterowniki języka Ruby dla platformy SQL Server](https://docs.microsoft.com/sql/connect/ruby/ruby-driver-for-sql-server/)


