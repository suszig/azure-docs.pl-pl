---
title: "Nawiązywanie połączenia z bazą danych Azure SQL Database przy użyciu platformy Java | Microsoft Docs"
description: "Przykładowy kod platformy Java, którego można użyć do nawiązywania połączenia z usługą Azure SQL Database i do wykonywania w niej zapytań."
services: sql-database
documentationcenter: 
author: ajlam
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 05/23/2017
ms.author: andrela
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 0df7bd56d3aac462a86e31b5512e48371918bbf2
ms.contentlocale: pl-pl
ms.lasthandoff: 07/04/2017


---
<a id="azure-sql-database-use-java-to-connect-and-query-data" class="xliff"></a>

# Baza danych Azure SQL Database: nawiązywanie połączenia i wysyłanie zapytań dotyczących danych przy użyciu platformy Java

W tym przewodniku Szybki start pokazano, jak nawiązać połączenie z usługą Azure SQL Database przy użyciu środowiska [Java](https://docs.microsoft.com/sql/connect/jdbc/microsoft-jdbc-driver-for-sql-server), a następnie, korzystając z instrukcji Transact-SQL, wysyłać zapytania o dane, a także wstawiać, aktualizować i usuwać dane z bazy danych z platform Mac OS, Ubuntu Linux i Windows.

<a id="prerequisites" class="xliff"></a>

## Wymagania wstępne

Ten przewodnik Szybki start używa jako punktu początkowego zasobów utworzonych w jednym z poniższych przewodników Szybki start:

- [Tworzenie bazy danych — portal](sql-database-get-started-portal.md)
- [Tworzenie bazy danych — interfejs wiersza polecenia](sql-database-get-started-cli.md)
- [Tworzenie bazy danych — PowerShell](sql-database-get-started-powershell.md)

<a id="install-java-software" class="xliff"></a>

## Instalowanie oprogramowania Java

W krokach w tej sekcji założono, że wiesz już, jak opracowywać zawartość za pomocą platformy Java, i dopiero zaczynasz pracę z usługą Azure SQL Database. Jeśli opracowywanie na platformie Java jest dla Ciebie czymś nowym, przejdź do tematu [Build an app using SQL Server](https://www.microsoft.com/en-us/sql-server/developer-get-started/) (Tworzenie aplikacji za pomocą programu SQL Server) i wybierz pozycję **Java**, a następnie wybierz swój system operacyjny.

<a id="mac-os" class="xliff"></a>

### **Mac OS**
Otwórz terminal i przejdź do katalogu, w którym planujesz utworzyć projekt języka Java. Zainstaluj rozwiązania **brew** i **Maven** przez wprowadzenie następujących poleceń: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

<a id="linux-ubuntu" class="xliff"></a>

### **Linux (Ubuntu)**
Otwórz terminal i przejdź do katalogu, w którym planujesz utworzyć projekt języka Java. Zainstaluj rozwiązanie **Maven** przez wprowadzenie następujących poleceń:

```bash
sudo apt-get install maven
```

<a id="windows" class="xliff"></a>

### **Windows**
Zainstaluj rozwiązanie [Maven](https://maven.apache.org/download.cgi) za pomocą oficjalnego instalatora. Rozwiązanie Maven ułatwia zarządzanie zależnościami oraz skompilowanie, przetestowanie i uruchomienie projektu języka Java. 

<a id="get-connection-information" class="xliff"></a>

## Pobieranie informacji o połączeniu

Uzyskaj parametry połączenia potrzebne do nawiązania połączenia z bazą danych Azure SQL. W następnych procedurach będą potrzebne w pełni kwalifikowana nazwa serwera, nazwa bazy danych i informacje logowania.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).
2. Wybierz opcję **Bazy danych SQL** z menu po lewej stronie, a następnie kliknij bazę danych na stronie **Bazy danych SQL**. 
3. Na stronie **Przegląd** bazy danych zweryfikuj w pełni kwalifikowaną nazwę serwera, jak pokazano na poniższej ilustracji. Możesz umieścić kursor na nazwie serwera w celu wywołania opcji **Kliknij, aby skopiować**. 

   ![nazwa-serwera](./media/sql-database-connect-query-dotnet/server-name.png) 

4. Jeśli nie pamiętasz informacji logowania do serwera, przejdź do strony serwera usługi SQL Database, aby wyświetlić nazwę administratora serwera oraz, w razie konieczności, zresetować hasło.
5. Kliknij pozycję **Pokaż parametry połączenia bazy danych**.

6. Sprawdź pełne parametry połączenia **JDBC**.

    ![Parametry połączenia sterownika JDBC](./media/sql-database-connect-query-jdbc/jdbc-connection-string.png)   

<a id="create-maven-project" class="xliff"></a>

### **Tworzenie projektu rozwiązania Maven**
Z poziomu terminalu utwórz nowy projekt rozwiązania Maven. 
```bash
mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
```

Dodaj **sterownik JDBC firmy Microsoft dla programu SQL Server** do zależności w pliku ***pom.xml***. 

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.0.jre8</version>
</dependency>
```

<a id="select-data" class="xliff"></a>

## Wybieranie danych

Użyj poniższego kodu, aby wykonać zapytanie o 20 najpopularniejszych produktów według kategorii, używając klasy [connection](https://docs.microsoft.com/sql/connect/jdbc/working-with-a-connection) wraz z instrukcją [SELECT](https://docs.microsoft.com/sql/t-sql/queries/select-transact-sql) języka Transact-SQL. Zamień parametry hostName, dbName, user i password na wartości określone podczas tworzenia bazy danych za pomocą przykładowych danych bazy danych AdventureWorksLT. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.Statement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Query data example:");
                System.out.println("=========================================");

                // Create and execute a SELECT SQL statement.
                String selectSql = "SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName " 
                    + "FROM [SalesLT].[ProductCategory] pc "  
                    + "JOIN [SalesLT].[Product] p ON pc.productcategoryid = p.productcategoryid";
                
                try (Statement statement = connection.createStatement();
                    ResultSet resultSet = statement.executeQuery(selectSql)) {

                        // Print results from select statement
                        System.out.println("Top 20 categories:");
                        while (resultSet.next())
                        {
                            System.out.println(resultSet.getString(1) + " "
                                + resultSet.getString(2));
                        }
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```

<a id="insert-data" class="xliff"></a>

## Wstawianie danych

Użyj poniższego kodu, aby wstawić nowy produkt do tabeli SalesLT.Product przy użyciu klasy [Prepared Statements](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) i instrukcji [INSERT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) języka Transact-SQL. Zamień parametry hostName, dbName, user i password na wartości określone podczas tworzenia bazy danych za pomocą przykładowych danych bazy danych AdventureWorksLT. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Insert data example:");
                System.out.println("=========================================");

                // Prepared statement to insert data
                String insertSql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, " 
                    + " StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

                java.util.Date date = new java.util.Date();
                java.sql.Timestamp sqlTimeStamp = new java.sql.Timestamp(date.getTime());

                try (PreparedStatement prep = connection.prepareStatement(insertSql)) {
                        prep.setString(1, "BrandNewProduct");
                        prep.setInt(2, 200989);
                        prep.setString(3, "Blue");
                        prep.setDouble(4, 75);
                        prep.setDouble(5, 80);
                        prep.setTimestamp(6, sqlTimeStamp);

                        int count = prep.executeUpdate();
                        System.out.println("Inserted: " + count + " row(s)");
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```
<a id="update-data" class="xliff"></a>

## Aktualizowanie danych

Użyj następującego kodu, aby zaktualizować nowy, wcześniej dodany produkt przy użyciu klasy [Prepared Statements](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) i instrukcji [UPDATE](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql) języka Transact-SQL w celu zaktualizowania danych w bazie danych Azure SQL Database. Zamień parametry hostName, dbName, user i password na wartości określone podczas tworzenia bazy danych za pomocą przykładowych danych bazy danych AdventureWorksLT. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Update data example:");
                System.out.println("=========================================");

                // Prepared statement to update data
                String updateSql = "UPDATE SalesLT.Product SET ListPrice = ? WHERE Name = ?";

                try (PreparedStatement prep = connection.prepareStatement(updateSql)) {
                        prep.setString(1, "500");
                        prep.setString(2, "BrandNewProduct");

                        int count = prep.executeUpdate();
                        System.out.println("Updated: " + count + " row(s)")
                }
        }
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```



<a id="delete-data" class="xliff"></a>

## Usuwanie danych

Użyj następującego kodu, aby usunąć nowy, wcześniej dodany produkt przy użyciu klasy [Prepared Statements](https://docs.microsoft.com/sql/connect/jdbc/using-statements-with-sql) i instrukcji [DELETE](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) języka Transact-SQL. Zamień parametry hostName, dbName, user i password na wartości określone podczas tworzenia bazy danych za pomocą przykładowych danych bazy danych AdventureWorksLT. 

```java
package com.sqldbsamples;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.DriverManager;

public class App {

    public static void main(String[] args) {
    
        // Connect to database
        String hostName = "your_server";
        String dbName = "your_database";
        String user = "your_username";
        String password = "your_password";
        String url = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", hostName, dbName, user, password);
        Connection connection = null;

        try {
                connection = DriverManager.getConnection(url);
                String schema = connection.getSchema();
                System.out.println("Successful connection - Schema: " + schema);

                System.out.println("Delete data example:");
                System.out.println("=========================================");

                // Prepared statement to delete data
                String deleteSql = "DELETE SalesLT.Product WHERE Name = ?";

                try (PreparedStatement prep = connection.prepareStatement(deleteSql)) {
                        prep.setString(1, "BrandNewProduct");

                        int count = prep.executeUpdate();
                        System.out.println("Deleted: " + count + " row(s)");
                }
        }       
        catch (Exception e) {
                e.printStackTrace();
        }
    }
}
```

<a id="next-steps" class="xliff"></a>

## Następne kroki
- [Projektowanie pierwszej bazy danych SQL na platformie Azure](sql-database-design-first-database.md)
- [Sterownik JDBC firmy Microsoft dla programu SQL Server](https://github.com/microsoft/mssql-jdbc)
- [Zgłaszanie problemów/zadawanie pytań](https://github.com/microsoft/mssql-jdbc/issues)


