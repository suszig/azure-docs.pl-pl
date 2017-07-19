---
title: "Nawiązywanie połączenia z usługą Azure SQL Database przy użyciu języka Python | Microsoft Docs"
description: "Przykładowy kod języka Python, którego można użyć do nawiązywania połączenia z usługą Azure SQL Database i do wykonywania w niej zapytań."
services: sql-database
documentationcenter: 
author: meet-bhagdev
manager: jhubbard
editor: 
ms.assetid: 452ad236-7a15-4f19-8ea7-df528052a3ad
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 05/24/2017
ms.author: meetb
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 99195b43a1577f978562864bac5fa12cdeb95d63
ms.contentlocale: pl-pl
ms.lasthandoff: 06/17/2017

---
# <a name="azure-sql-database-use-python-to-connect-and-query-data"></a>Azure SQL Database: nawiązywanie połączenia i wysyłanie zapytań dotyczących danych przy użyciu języka Python

 W tym przewodniku Szybki start pokazano, jak nawiązać połączenie z usługą Azure SQL Database przy użyciu języka [Python](https://python.org), a następnie, korzystając z instrukcji Transact-SQL, wysyłać zapytania o dane, a także wstawiać, aktualizować i usuwać dane z bazy danych z platform Mac OS, Ubuntu Linux i Windows.

## <a name="prerequisites"></a>Wymagania wstępne

Ten przewodnik Szybki start używa jako punktu początkowego zasobów utworzonych w jednym z poniższych przewodników Szybki start:

- [Tworzenie bazy danych — portal](sql-database-get-started-portal.md)
- [Tworzenie bazy danych — interfejs wiersza polecenia](sql-database-get-started-cli.md)
- [Tworzenie bazy danych — PowerShell](sql-database-get-started-powershell.md)

## <a name="install-the-python-and-database-communication-libraries"></a>Instalowanie bibliotek komunikacji języka Python i bazy danych

W krokach w tej sekcji założono, że wiesz już, jak programować przy użyciu języka Python, ale dopiero zaczynasz pracę z usługą Azure SQL Database. Jeśli programowanie przy użyciu języka Python jest dla Ciebie czymś nowym, przejdź do artykułu [Build an app using SQL Server](https://www.microsoft.com/sql-server/developer-get-started/) (Tworzenie aplikacji za pomocą programu SQL Server) i wybierz pozycję **Python**, a następnie wybierz swój system operacyjny.

### <a name="mac-os"></a>**Mac OS**
Otwórz terminal i przejdź do katalogu, w którym planujesz utworzyć skrypt języka Python. Wprowadź następujące polecenia, aby zainstalować **brew**, **sterownik Microsoft ODBC dla komputerów Mac** i **moduł pyodbc**. Moduł pyodbc używa sterownika Microsoft ODBC w systemie Linux do nawiązania połączenia z bazami danych SQL.

``` bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew tap microsoft/msodbcsql https://github.com/Microsoft/homebrew-msodbcsql-preview
brew update
brew install msodbcsql 
#for silent install ACCEPT_EULA=y brew install msodbcsql
sudo pip install pyodbc==3.1.1
```

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Otwórz terminal i przejdź do katalogu, w którym planujesz utworzyć skrypt języka Python. Wprowadź następujące polecenia, aby zainstalować **sterownik Microsoft ODBC dla systemu Linux** i **moduł pyodbc**. Moduł pyodbc używa sterownika Microsoft ODBC w systemie Linux do nawiązania połączenia z bazami danych SQL.

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql.list
exit
sudo apt-get update
sudo apt-get install msodbcsql mssql-tools unixodbc-dev
sudo pip install pyodbc==3.1.1
```

### <a name="windows"></a>**Windows**
Zainstaluj [Sterownik Microsoft ODBC 13.1](https://www.microsoft.com/download/details.aspx?id=53339) (uaktualnij sterownik, jeśli zostanie wyświetlony monit). Moduł pyodbc używa sterownika Microsoft ODBC w systemie Linux do nawiązania połączenia z bazami danych SQL. 

Następnie zainstaluj **moduł pyodbc** przy użyciu parametrów inicjacji programu.

```cmd
pip install pyodbc==3.1.1
```

Instrukcje, w których przedstawiono, jak umożliwić korzystanie z parametrów inicjowania programu, można znaleźć [tutaj](http://stackoverflow.com/questions/4750806/how-to-install-pip-on-windows)

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu

Uzyskaj parametry połączenia potrzebne do nawiązania połączenia z bazą danych Azure SQL. W następnych procedurach będą potrzebne w pełni kwalifikowana nazwa serwera, nazwa bazy danych i informacje logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Wybierz opcję **Bazy danych SQL** z menu po lewej stronie, a następnie kliknij bazę danych na stronie **Bazy danych SQL**. 
3. Na stronie **Przegląd** bazy danych zweryfikuj w pełni kwalifikowaną nazwę serwera, jak pokazano na poniższej ilustracji. Możesz umieścić kursor na nazwie serwera w celu wywołania opcji **Kliknij, aby skopiować**. 

   ![nazwa-serwera](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Jeśli nie pamiętasz informacji logowania serwera, przejdź do strony serwera usługi SQL Database, aby wyświetlić nazwę administratora serwera oraz, w razie konieczności, zresetować hasło.     
   
## <a name="select-data"></a>Wybieranie danych

Użyj następującego kodu, aby wykonać zapytanie o 20 najpopularniejszych produktów według kategorii, używając funkcji [pyodbc.connect](https://github.com/mkleehammer/pyodbc/wiki) wraz z instrukcją [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) języka Transact-SQL. Funkcja [cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) służy do pobierania zestawu wyników z zapytania względem usługi SQL Database. Ta funkcja akceptuje zapytanie i zwraca zestaw wyników, względem którego można przeprowadzić iterację przy użyciu funkcji **cursor.fetchone()**. Zastąp parametry server, database, username i password wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy AdventureWorksLT.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
cursor.execute("SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName FROM [SalesLT].[ProductCategory] pc JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid")
row = cursor.fetchone()
while row:
    print str(row[0]) + " " + str(row[1])
    row = cursor.fetchone()
```

## <a name="insert-data"></a>Wstawianie danych
Użyj następującego kodu, aby wstawić nowy produkt do tabeli SalesLT.Product przy użyciu instrukcji funkcji [cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) i instrukcji [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) języka Transact-SQL. Zastąp parametry server, database, username i password wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy AdventureWorksLT.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
with cursor.execute("INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES ('BrandNewProduct', '200989', 'Blue', 75, 80, '7/1/2016')"): 
    print ('Successfuly Inserted!')
cnxn.commit()
```

## <a name="update-data"></a>Aktualizowanie danych
Użyj następującego kodu, aby zaktualizować wcześniej dodany nowy produkt przy użyciu funkcji [cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) i instrukcji [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) języka Transact-SQL. Zastąp parametry server, database, username i password wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy AdventureWorksLT.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
tsql = "UPDATE SalesLT.Product SET ListPrice = ? WHERE Name = ?"
with cursor.execute(tsql,50,'BrandNewProduct'):
    print ('Successfuly Updated!')
cnxn.commit()

```

## <a name="delete-data"></a>Usuwanie danych
Użyj następującego kodu, aby usunąć wcześniej dodany nowy produkt przy użyciu funkcji [cursor.execute](https://github.com/mkleehammer/pyodbc/wiki/Cursor) i instrukcji [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) języka Transact-SQL. Zastąp parametry server, database, username i password wartościami określonymi podczas tworzenia bazy danych za pomocą przykładowych danych bazy AdventureWorksLT.

```Python
import pyodbc
server = 'your_server.database.windows.net'
database = 'your_database'
username = 'your_username'
password = 'your_password'
driver= '{ODBC Driver 13 for SQL Server}'
cnxn = pyodbc.connect('DRIVER='+driver+';PORT=1433;SERVER='+server+';PORT=1443;DATABASE='+database+';UID='+username+';PWD='+ password)
cursor = cnxn.cursor()
tsql = "DELETE FROM SalesLT.Product WHERE Name = ?"
with cursor.execute(tsql,'BrandNewProduct'):
    print ('Successfuly Deleted!')
cnxn.commit()
```

## <a name="next-steps"></a>Następne kroki

- [Projektowanie pierwszej bazy danych SQL na platformie Azure](sql-database-design-first-database.md)
- [Sterowniki Microsoft Python dla platformy SQL Server](https://docs.microsoft.com/sql/connect/python/python-driver-for-sql-server/)
- [Centrum deweloperów języka Python](https://azure.microsoft.com/develop/python/?v=17.23h)


