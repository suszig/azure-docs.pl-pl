---
title: "Wdrożenia rozwiązania bazy danych SQL Azure rozproszona geograficznie | Dokumentacja firmy Microsoft"
description: "Dowiedz się do konfigurowania bazy danych SQL Azure i aplikacji dla trybu failover z bazą danych replikowanych i testowanie trybu failover."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,business continuity
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 05/26/2017
ms.author: carlrab
ms.openlocfilehash: 910be8ff5f9a882c7bb8ae875b8bf5fc74d1fb9a
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="implement-a-geo-distributed-database"></a>Wdrożenie rozproszone geograficznie bazy danych

W tym samouczku skonfiguruj bazy danych Azure SQL i aplikacji dla trybu failover w regionie zdalnego, a następnie testować tryb failover planu. Omawiane kwestie: 

> [!div class="checklist"]
> * Tworzenie bazy danych użytkowników i udzielić im uprawnień
> * Skonfiguruj regułę zapory poziomu bazy danych
> * Utwórz [— replikacja geograficzna trybu failover grupy](sql-database-geo-replication-overview.md)
> * Tworzenie i kompilacja aplikacji Java kwerendy bazy danych Azure SQL
> * Wykonaj wyszczególniania odzyskiwania po awarii

Jeśli nie masz subskrypcji platformy Azure, [utworzyć bezpłatne konto](https://azure.microsoft.com/free/) przed rozpoczęciem.


## <a name="prerequisites"></a>Wymagania wstępne

Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

- Zainstalowana najnowsza wersja [programu Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs). 
- Zainstalowana baza danych Azure SQL. W tym samouczku używana przykładową bazę danych AdventureWorksLT o nazwie **mySampleDatabase** z jednego z tych Szybki Start:

   - [Tworzenie bazy danych — portal](sql-database-get-started-portal.md)
   - [Tworzenie bazy danych — interfejs wiersza polecenia](sql-database-get-started-cli.md)
   - [Tworzenie bazy danych — PowerShell](sql-database-get-started-powershell.md)

- Zidentyfikowano metodę wykonywanie skryptów SQL bazy danych, można użyć jednej z następujących narzędzi zapytania:
   - Edytor zapytań w [portalu Azure](https://portal.azure.com). Aby uzyskać więcej informacji na temat używania edytora zapytań w portalu Azure, zobacz [Connect i zapytania za pomocą edytora zapytań](sql-database-get-started-portal.md#query-the-sql-database).
   - Najnowsza wersja [programu SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), która jest zintegrowane środowisko umożliwiające zarządzanie dowolnej infrastruktury SQL z programu SQL Server z bazą danych SQL systemu Microsoft Windows.
   - Najnowsza wersja [Visual Studio Code](https://code.visualstudio.com/docs), czyli edytorze graficznego kodu dla systemu Linux, macOS, i systemu Windows, który obsługuje rozszerzenia, w tym [rozszerzenia mssql](https://aka.ms/mssql-marketplace) do wykonywania zapytań programu Microsoft SQL Server Baza danych Azure SQL i usługi SQL Data Warehouse. Aby uzyskać więcej informacji o usłudze Azure SQL Database za pomocą tego narzędzia, zobacz [Connect i zapytanie z kodem VS](sql-database-connect-query-vscode.md). 

## <a name="create-database-users-and-grant-permissions"></a>Tworzenie bazy danych użytkowników i udzielanie uprawnień

Połączenia z bazą danych i tworzenie kont użytkowników przy użyciu jednej z następujących narzędzi zapytania:

- Edytor zapytań w portalu Azure
- SQL Server Management Studio
- Visual Studio Code

Te konta użytkowników automatycznie replikowane na serwerze pomocniczym (i być utrzymywane w synchronizacji). Aby użyć programu SQL Server Management Studio lub Visual Studio Code, może być konieczne skonfigurowanie reguły zapory, jeśli łączysz się z adresem IP, dla którego nie została jeszcze skonfigurowana zapora klienta. Aby uzyskać szczegółowe instrukcje, zobacz [utworzyć regułę zapory poziomu serwera](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).

- W oknie zapytania wykonaj następujące zapytanie, aby utworzyć dwa konta użytkownika w bazie danych. Ten skrypt przyznaje **db_owner** uprawnień do **app_admin** konto i przyznaje **wybierz** i **aktualizacji** uprawnień do **app_user** konta. 

   ```sql
   CREATE USER app_admin WITH PASSWORD = 'ChangeYourPassword1';
   --Add SQL user to db_owner role
   ALTER ROLE db_owner ADD MEMBER app_admin; 
   --Create additional SQL user
   CREATE USER app_user WITH PASSWORD = 'ChangeYourPassword1';
   --grant permission to SalesLT schema
   GRANT SELECT, INSERT, DELETE, UPDATE ON SalesLT.Product TO app_user;
   ```

## <a name="create-database-level-firewall"></a>Utwórz zapory poziomu bazy danych

Utwórz [reguły zapory poziomu bazy danych](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) bazy danych SQL. Tę regułę zapory poziomu bazy danych automatycznie replikuje dane na serwerze pomocniczym, utworzone w tym samouczku. Dla uproszczenia (w tym samouczku) Użyj publiczny adres IP komputera, na którym wykonywana kroki opisane w tym samouczku. Aby określić adres IP używany dla reguły zapory poziomu serwera dla bieżącego komputera, zobacz [utworzenie zapory poziomu serwera](sql-database-get-started-portal.md#create-a-server-level-firewall-rule).  

- W oknie zapytania otwarte należy zastąpić poprzednie zapytanie następującej kwerendy, zastępując adresy IP odpowiednie adresy IP dla danego środowiska.  

   ```sql
   -- Create database-level firewall setting for your public IP address
   EXECUTE sp_set_database_firewall_rule @name = N'myGeoReplicationFirewallRule',@start_ip_address = '0.0.0.0', @end_ip_address = '0.0.0.0';
   ```

## <a name="create-an-active-geo-replication-auto-failover-group"></a>Utwórz grupę aktywna replikacja geograficzna automatycznej pracy awaryjnej 

Przy użyciu programu Azure PowerShell utworzyć [aktywna replikacja geograficzna automatycznej pracy awaryjnej grupy](sql-database-geo-replication-overview.md) między istniejącego serwera Azure SQL i nowy pusty serwera Azure SQL w regionie Azure, a następnie dodaj przykładowej bazie danych do trybu failover grupy.

> [!IMPORTANT]
> Te polecenia cmdlet wymagają programu Azure PowerShell 4.0. [!INCLUDE [sample-powershell-install](../../includes/sample-powershell-install-no-ssh.md)]
>

1. Wypełnić zmienne dla skryptów PowerShell przy użyciu wartości dla istniejącego serwera i przykładową bazę danych i podaj globalnie unikatowa wartość nazwy grupy pracy awaryjnej.

   ```powershell
   $adminlogin = "ServerAdmin"
   $password = "ChangeYourAdminPassword1"
   $myresourcegroupname = "<your resource group name>"
   $mylocation = "<your resource group location>"
   $myservername = "<your existing server name>"
   $mydatabasename = "mySampleDatabase"
   $mydrlocation = "<your disaster recovery location>"
   $mydrservername = "<your disaster recovery server name>"
   $myfailovergroupname = "<your unique failover group name>"
   ```

2. W danym regionie trybu failover, należy utworzyć pusty serwer kopii zapasowej.

   ```powershell
   $mydrserver = New-AzureRmSqlServer -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername `
      -Location $mydrlocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $mydrserver   
   ```

3. Utwórz grupę trybu failover między dwoma serwerami.

   ```powershell
   $myfailovergroup = New-AzureRMSqlDatabaseFailoverGroup `
      –ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -PartnerServerName $mydrservername  `
      –FailoverGroupName $myfailovergroupname `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $myfailovergroup   
   ```

4. Dodaj bazę danych do grupy pracy awaryjnej.

   ```powershell
   $myfailovergroup = Get-AzureRmSqlDatabase `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $myservername `
      -DatabaseName $mydatabasename | `
    Add-AzureRmSqlDatabaseToFailoverGroup `
      -ResourceGroupName $myresourcegroupname ` `
      -ServerName $myservername `
      -FailoverGroupName $myfailovergroupname
   $myfailovergroup   
   ```

## <a name="install-java-software"></a>Instalowanie oprogramowania Java

W krokach w tej sekcji założono, że wiesz już, jak opracowywać zawartość za pomocą platformy Java, i dopiero zaczynasz pracę z usługą Azure SQL Database. 

### <a name="mac-os"></a>**Mac OS**
Otwórz terminal i przejdź do katalogu, w którym planujesz utworzyć projekt języka Java. Zainstaluj rozwiązania **brew** i **Maven** przez wprowadzenie następujących poleceń: 

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew update
brew install maven
```

Aby uzyskać szczegółowe instrukcje dotyczące instalowania i konfigurowania środowiska Java i Maven, przejdź [tworzenie aplikacji za pomocą programu SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), wybierz pozycję **Java**, wybierz pozycję **MacOS**i postępuj szczegółowe instrukcje dotyczące konfigurowania Java i Maven w kroku 1.2 i 1.3.

### <a name="linux-ubuntu"></a>**Linux (Ubuntu)**
Otwórz terminal i przejdź do katalogu, w którym planujesz utworzyć projekt języka Java. Zainstaluj rozwiązanie **Maven** przez wprowadzenie następujących poleceń:

```bash
sudo apt-get install maven
```

Aby uzyskać szczegółowe instrukcje dotyczące instalowania i konfigurowania środowiska Java i Maven, przejdź [tworzenie aplikacji za pomocą programu SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), wybierz pozycję **Java**, wybierz pozycję **Ubuntu**i postępuj szczegółowe instrukcje dotyczące konfigurowania Java i Maven w kroku 1.2, 1.3 i 1.4.

### <a name="windows"></a>**Windows**
Zainstaluj rozwiązanie [Maven](https://maven.apache.org/download.cgi) za pomocą oficjalnego instalatora. Użyj Maven, aby ułatwić zarządzanie zależności, tworzenia, testowania i uruchom projekt języka Java. Aby uzyskać szczegółowe instrukcje dotyczące instalowania i konfigurowania środowiska Java i Maven, przejdź [tworzenie aplikacji za pomocą programu SQL Server](https://www.microsoft.com/sql-server/developer-get-started/), wybierz pozycję **Java**wybierz systemu Windows, a następnie postępuj zgodnie z instrukcjami szczegółowe Konfigurowanie języka Java i Maven w kroku 1.2 i 1.3.

## <a name="create-sqldbsample-project"></a>Utwórz projekt SqlDbSample

1. W konsoli poleceń (na przykład Bash) Utwórz projekt Maven. 
   ```bash
   mvn archetype:generate "-DgroupId=com.sqldbsamples" "-DartifactId=SqlDbSample" "-DarchetypeArtifactId=maven-archetype-quickstart" "-Dversion=1.0.0"
   ```
2. Typ **Y** i kliknij przycisk **Enter**.
3. Przejdź do nowo utworzonego projektu.

   ```bash
   cd SqlDbSamples
   ```

4. Za pomocą ulubionego edytora, otwórz plik pom.xml w folderze projektu. 

5. Dodaj sterownik JDBC firmy Microsoft dla programu SQL Server zależności na projekt Maven przez otwarcie w ulubionym edytorze tekstów i kopiowanie i wklejanie następujące wiersze w pliku pom.xml. Nie zastępuj istniejącego wartości wstępnie w pliku. Zależności JDBC należy wkleić w większych () sekcji "zależności".   

   ```xml
   <dependency>
     <groupId>com.microsoft.sqlserver</groupId>
     <artifactId>mssql-jdbc</artifactId>
    <version>6.1.0.jre8</version>
   </dependency>
   ```

6. Określ wersję Java do skompilowania projektu w odniesieniu do, dodając w poniższej sekcji "właściwości" w pliku pom.xml po sekcji "zależności". 

   ```xml
   <properties>
     <maven.compiler.source>1.8</maven.compiler.source>
     <maven.compiler.target>1.8</maven.compiler.target>
   </properties>
   ```
7. Dodaj poniższą sekcję "kompilacji" w pliku pom.xml po sekcji "właściwości" do obsługi plików manifestu w słoików.       

   ```xml
   <build>
     <plugins>
       <plugin>
         <groupId>org.apache.maven.plugins</groupId>
         <artifactId>maven-jar-plugin</artifactId>
         <version>3.0.0</version>
         <configuration>
           <archive>
             <manifest>
               <mainClass>com.sqldbsamples.App</mainClass>
             </manifest>
           </archive>
        </configuration>
       </plugin>
     </plugins>
   </build>
   ```
8. Zapisz i zamknij plik pom.xml.
9. Otwórz plik App.java (C:\apache-maven-3.5.0\SqlDbSample\src\main\java\com\sqldbsamples\App.java) i Zastąp zawartość następującą zawartość. Nazwa grupy pracy awaryjnej należy zastąpić nazwę grupy pracy awaryjnej. Zmiana wartości dla nazwy bazy danych użytkownika lub hasło, należy zmienić również tych wartości.

   ```java
   package com.sqldbsamples;

   import java.sql.Connection;
   import java.sql.Statement;
   import java.sql.PreparedStatement;
   import java.sql.ResultSet;
   import java.sql.Timestamp;
   import java.sql.DriverManager;
   import java.util.Date;
   import java.util.concurrent.TimeUnit;

   public class App {

      private static final String FAILOVER_GROUP_NAME = "myfailovergroupname";
  
      private static final String DB_NAME = "mySampleDatabase";
      private static final String USER = "app_user";
      private static final String PASSWORD = "ChangeYourPassword1";

      private static final String READ_WRITE_URL = String.format("jdbc:sqlserver://%s.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);
      private static final String READ_ONLY_URL = String.format("jdbc:sqlserver://%s.secondary.database.windows.net:1433;database=%s;user=%s;password=%s;encrypt=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;", FAILOVER_GROUP_NAME, DB_NAME, USER, PASSWORD);

      public static void main(String[] args) {
         System.out.println("#######################################");
         System.out.println("## GEO DISTRIBUTED DATABASE TUTORIAL ##");
         System.out.println("#######################################");
         System.out.println(""); 

         int highWaterMark = getHighWaterMarkId();

         try {
            for(int i = 1; i < 1000; i++) {
                //  loop will run for about 1 hour
                System.out.print(i + ": insert on primary " + (insertData((highWaterMark + i))?"successful":"failed"));
                TimeUnit.SECONDS.sleep(1);
                System.out.print(", read from secondary " + (selectData((highWaterMark + i))?"successful":"failed") + "\n");
                TimeUnit.SECONDS.sleep(3);
            }
         } catch(Exception e) {
            e.printStackTrace();
      }
   }

   private static boolean insertData(int id) {
      // Insert data into the product table with a unique product name that we can use to find the product again later
      String sql = "INSERT INTO SalesLT.Product (Name, ProductNumber, Color, StandardCost, ListPrice, SellStartDate) VALUES (?,?,?,?,?,?);";

      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         pstmt.setInt(2, 200989 + id + 10000);
         pstmt.setString(3, "Blue");
         pstmt.setDouble(4, 75.00);
         pstmt.setDouble(5, 89.99);
         pstmt.setTimestamp(6, new Timestamp(new Date().getTime()));
         return (1 == pstmt.executeUpdate());
      } catch (Exception e) {
         return false;
      }
   }

   private static boolean selectData(int id) {
      // Query the data that was previously inserted into the primary database from the geo replicated database
      String sql = "SELECT Name, Color, ListPrice FROM SalesLT.Product WHERE Name = ?";

      try (Connection connection = DriverManager.getConnection(READ_ONLY_URL); 
              PreparedStatement pstmt = connection.prepareStatement(sql)) {
         pstmt.setString(1, "BrandNewProduct" + id);
         try (ResultSet resultSet = pstmt.executeQuery()) {
            return resultSet.next();
         }
      } catch (Exception e) {
         return false;
      }
   }

   private static int getHighWaterMarkId() {
      // Query the high water mark id that is stored in the table to be able to make unique inserts 
      String sql = "SELECT MAX(ProductId) FROM SalesLT.Product";
      int result = 1;
        
      try (Connection connection = DriverManager.getConnection(READ_WRITE_URL); 
              Statement stmt = connection.createStatement();
              ResultSet resultSet = stmt.executeQuery(sql)) {
         if (resultSet.next()) {
             result =  resultSet.getInt(1);
            }
         } catch (Exception e) {
          e.printStackTrace();
         }
         return result;
      }
   }
   ```
6. Zapisz i zamknij plik App.java.

## <a name="compile-and-run-the-sqldbsample-project"></a>Kompilowanie i uruchamianie projektu SqlDbSample

1. W konsoli polecenie należy wykonać następujące polecenie.

   ```bash
   mvn package
   ```
2. Po zakończeniu uruchom następujące polecenie do uruchomienia aplikacji (działa na godzinę, chyba że zostanie zatrzymana ręcznie):

   ```bash
   mvn -q -e exec:java "-Dexec.mainClass=com.sqldbsamples.App"
   
   #######################################
   ## GEO DISTRIBUTED DATABASE TUTORIAL ##
   #######################################

   1. insert on primary successful, read from secondary successful
   2. insert on primary successful, read from secondary successful
   3. insert on primary successful, read from secondary successful
   ```

## <a name="perform-disaster-recovery-drill"></a>Wykonaj wyszczególniania odzyskiwania po awarii

1. Wywołanie ręcznego przełączania trybu failover grupy pracy awaryjnej. 

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $mydrservername `
   -FailoverGroupName $myfailovergroupname
   ```

2. Obserwować wyniki aplikacji podczas pracy awaryjnej. Niektóre wstawia się niepowodzeniem podczas odświeżania pamięci podręcznej DNS.     

3. Dowiedz się od roli działa serwer odzyskiwania po awarii.

   ```powershell
   $mydrserver.ReplicationRole
   ```

4. Powrót po awarii.

   ```powershell
   Switch-AzureRMSqlDatabaseFailoverGroup `
   -ResourceGroupName $myresourcegroupname  `
   -ServerName $myservername `
   -FailoverGroupName $myfailovergroupname
   ```

5. Obserwować wyniki aplikacji podczas powrotu po awarii. Niektóre wstawia się niepowodzeniem podczas odświeżania pamięci podręcznej DNS.     

6. Dowiedz się od roli działa serwer odzyskiwania po awarii.

   ```powershell
   $fileovergroup = Get-AzureRMSqlDatabaseFailoverGroup `
      -FailoverGroupName $myfailovergroupname `
      -ResourceGroupName $myresourcegroupname `
      -ServerName $mydrservername
   $fileovergroup.ReplicationRole
   ```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz [aktywnych grup — replikacja geograficzna i pracy awaryjnej](sql-database-geo-replication-overview.md).
