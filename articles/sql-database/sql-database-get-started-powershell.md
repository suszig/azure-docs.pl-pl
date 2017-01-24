---
title: "Konfiguracja nowej usługi SQL Database za pomocą programu PowerShell | Microsoft Docs"
description: "Dowiedz się teraz, jak utworzyć bazę danych SQL za pomocą programu PowerShell. Typowe zadania dotyczące konfigurowania bazy danych można wykonywać za pomocą poleceń cmdlet programu PowerShell."
keywords: tworzenie nowej bazy danych sql, konfiguracja bazy danych
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: 7d99869b-cec5-4583-8c1c-4c663f4afd4d
ms.service: sql-database
ms.custom: single databases
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 12/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 145cdc5b686692b44d2c3593a128689a56812610
ms.openlocfilehash: b557c6d3591b1d56144651e876fa4bb1b0646935


---

# <a name="get-started-with-azure-sql-database-servers-databases-and-firewall-rules-by-using-azure-powershell"></a>Wprowadzenie do serwerów, baz danych i reguł zapory usługi Azure SQL Database przy użyciu programu Azure PowerShell

Z tego samouczka ułatwiającego rozpoczęcie pracy nauczysz się używać programu PowerShell do wykonywania następujących czynności:

* Tworzenie nowej grupy zasobów platformy Azure
* Tworzenie serwera logicznego usługi Azure SQL
* Wyświetlanie właściwości serwera usługi Azure SQL
* Tworzenie reguły zapory na poziomie serwera
* Tworzenie przykładowej bazy danych AdventureWorksLT jako autonomicznej bazy danych
* Wyświetlanie właściwości przykładowej bazy danych AdventureWorksLT
* Tworzenie pustej autonomicznej bazy danych

W tym samouczku zostaną również wykonane następujące czynności:

* Nawiązywanie połączenia z serwerem logicznym i jego bazą danych master
* Wyświetlanie właściwości bazy danych master
* Nawiązywanie połączenia z przykładową bazą danych
* Wyświetlanie właściwości użytkownika bazy danych

Po ukończeniu tego samouczka uzyskasz przykładową bazę danych i pustą bazę danych, uruchomione w grupie zasobów platformy Azure i dołączone do serwera logicznego. Ponadto będziesz mieć regułę zapory na poziomie serwera skonfigurowaną tak, aby umożliwić podmiotowi zabezpieczeń na poziomie serwera logowanie się do serwera z określonego adresu IP (lub zakresu adresów IP). 

**Szacowany czas**: ten samouczek zajmie Ci około 30 minut (przy założeniu, że spełniasz już wymagania wstępne).


> [!TIP]
> Te same czynności w samouczku ułatwiającym rozpoczęcie pracy można wykonać przy użyciu witryny [Azure Portal](sql-database-get-started.md).
>


## <a name="prerequisites"></a>Wymagania wstępne

* Musisz mieć konto platformy Azure. Możesz [utworzyć konto bezpłatnej wersji próbnej Azure](/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Musisz zalogować się przy użyciu konta, które jest elementem członkowskim roli współautora lub właściciela subskrypcji. Aby uzyskać więcej informacji o kontroli dostępu na podstawie ról (RBAC, role-based access control), zobacz [Getting started with access management in the Azure portal](../active-directory/role-based-access-control-what-is.md) (Wprowadzenie do zarządzania dostępem w witrynie Azure Portal).

* Wymagany jest plik bacpac przykładowej bazy danych AdventureWorksLT w magazynie usługi Azure Blob Storage.

### <a name="download-the-adventureworkslt-sample-database-bacpac-file-and-save-it-in-azure-blob-storage"></a>Pobieranie pliku bacpac przykładowej bazy danych AdventureWorksLT i zapisywanie go w magazynie usługi Azure Blob Storage

W tym samouczku opisano proces tworzenia nowej bazy danych AdventureWorksLT przez zaimportowanie pliku bacpac z magazynu usługi Azure Storage. Pierwszym krokiem jest uzyskanie kopii pliku AdventureWorksLT.bacpac i przekazanie go do magazynu Blob Storage.
Wykonanie poniższych kroków umożliwia przygotowanie przykładowej bazy danych do zaimportowania:

1. [Pobierz plik AdventureWorksLT.bacpac](https://sqldbbacpacs.blob.core.windows.net/bacpacs/AdventureWorksLT.bacpac) i zapisz go z rozszerzeniem pliku bacpac.
2. [Utwórz nowe konto magazynu](../storage/storage-create-storage-account.md#create-a-storage-account) — konto magazynu można utworzyć z domyślnymi ustawieniami.
3. Utwórz nowy **kontener**, przechodząc do konta magazynu, wybierz pozycję **Obiekty blob**, a następnie kliknij pozycję **+Kontener**.
4. Przekaż plik bacpac do kontenera obiektów blob w ramach konta magazynu. Możesz użyć przycisku **Przekaż** u góry strony kontenera lub [użyć narzędzia AzCopy](../storage/storage-use-azcopy.md#blob-upload). 
5. Po zapisaniu pliku AdventureWorksLT.bacpac wymagany jest adres URL i klucz konta magazynu na potrzeby importowania fragmentu kodu w dalszej części tego samouczka. 
   * Wybierz plik bacpac i skopiuj adres URL. Powinien on być podobny do następującego: https://{nazwa-konta-magazynu}.blob.core.windows.net/{nazwa-kontenera}/AdventureWorksLT.bacpac. Na stronie konta magazynu kliknij pozycję **Klucze dostępu** i skopiuj wartość **klucz1**.


[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]


## <a name="create-a-new-logical-sql-server-using-azure-powershell"></a>Tworzenie nowego serwera logicznego SQL za pomocą programu Azure PowerShell

Wymagana jest grupa zasobów, w której będzie znajdować się serwer, więc pierwszym krokiem jest utworzenie nowej grupy zasobów i serwera ([New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.3.0/new-azurermresourcegroup), [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqlserver)) lub uzyskanie odwołania do istniejących ([Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.3.0/get-azurermresourcegroup), [Get-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqlserver)).
Następujące fragmenty kodu spowodują utworzenie grupy zasobów i serwera Azure SQL oraz grupy zasobów, jeśli jeszcze nie istnieją:

Aby uzyskać listę prawidłowych lokalizacji platformy Azure i formatu ciągu, zobacz poniższą sekcję [Fragmenty kodu pomocnika](#helper-snippets).
```
# Create new, or get existing resource group
############################################

$resourceGroupName = "{resource-group-name}"
$resourceGroupLocation = "{resource-group-location}"

$myResourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ea SilentlyContinue

if(!$myResourceGroup)
{
   Write-Output "Creating resource group: $resourceGroupName"
   $myResourceGroup = New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else
{
   Write-Output "Resource group $resourceGroupName already exists:"
}
$myResourceGroup



# Create a new, or get existing server
######################################

$serverName = "{server-name}"
$serverVersion = "12.0"
$serverLocation = $resourceGroupLocation
$serverResourceGroupName = $resourceGroupName

$serverAdmin = "{server-admin}"
$serverAdminPassword = "{server-admin-password}"

$securePassword = ConvertTo-SecureString –String $serverAdminPassword –AsPlainText -Force
$serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue
if(!$myServer)
{
   Write-Output "Creating SQL server: $serverName"
   $myServer = New-AzureRmSqlServer -ResourceGroupName $serverResourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
}
else
{
   Write-Output "SQL server $serverName already exists:"
}
$myServer

```


## <a name="view-the-logical-sql-server-properties-using-azure-powershell"></a>Wyświetlanie właściwości serwera logicznego SQL Server za pomocą programu Azure PowerShell

```
#$serverResourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName 

Write-Host "Server name: " $myServer.ServerName
Write-Host "Fully qualified server name: $serverName.database.windows.net"
Write-Host "Server location: " $myServer.Location
Write-Host "Server version: " $myServer.ServerVersion
Write-Host "Server administrator login: " $myServer.SqlAdministratorLogin
```


## <a name="create-a-server-level-firewall-rule-using-azure-powershell"></a>Tworzenie reguły zapory na poziomie serwera za pomocą programu Azure PowerShell

Aby ustawić regułę zapory, należy znać swój publiczny adres IP. Adres IP można uzyskać przy użyciu wybranej przeglądarki (zadaj pytanie „jaki jest mój adres IP”). Aby uzyskać więcej szczegółów, zobacz [reguły zapory](sql-database-firewall-configure.md).

Następujące reguły używają poleceń cmdlet [Get-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqlserverfirewallrule) i [New-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqlserverfirewallrule), aby pobrać odwołanie lub utworzyć nową regułę. Dla tego fragmentu kodu, jeśli reguła już istnieje, pobierane jest tylko odwołanie do niej, a początkowe i końcowe adresy IP nie są aktualizowane. Zawsze można zmienić klauzulę **else**, aby użyć polecenia [Set-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/set-azurermsqlserverfirewallrule) w celu utworzenia funkcji lub jej zaktualizowania.

> [!NOTE] 
> Zaporę usługi SQL Database możesz otworzyć na serwerze dla pojedynczego adresu IP lub dla całego zakresu adresów. Otwarcie zapory umożliwia administratorom SQL i użytkownikom logowanie się do dowolnej bazy danych na serwerze, dla którego mają prawidłowe poświadczenia.
>

```
#$serverName = "{server-name}"
#$serverResourceGroupName = "{resource-group-name}"
$serverFirewallRuleName = "{server-firewall-rule-name}"
$serverFirewallStartIp = "{server-firewall-rule-startIp}"
$serverFirewallEndIp = "{server-firewall-rule-endIp}"

$myFirewallRule = Get-AzureRmSqlServerFirewallRule -FirewallRuleName $serverFirewallRuleName -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue

if(!$myFirewallRule)
{
   Write-host "Creating server firewall rule: $serverFirewallRuleName"
   $myFirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $serverResourceGroupName -ServerName $serverName -FirewallRuleName $serverFirewallRuleName -StartIpAddress $serverFirewallStartIp -EndIpAddress $serverFirewallEndIp
}
else
{
   Write-host "Server firewall rule $serverFirewallRuleName already exists:"
}
$myFirewallRule
```


## <a name="connect-to-sql-server-using-azure-powershell"></a>Nawiązywanie połączenia z serwerem SQL Server za pomocą programu Azure PowerShell

Uruchommy proste zapytanie do głównej bazy danych w celu sprawdzenia, czy możliwe jest nawiązanie połączenia z serwerem. Następujący fragment kodu używa programu [.NET Framework Provider for SQL Server (System.Data.SqlClient)](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx) w celu nawiązania połączenia z główną bazą danych serwera i utworzenia dotyczących jej zapytań. Tworzy on parametry połączenia na podstawie zmiennych użytych w poprzednich fragmentach kodu. Zastąp symbole zastępcze nazwą i hasłem administratora serwera SQL Server, których użyto do utworzenia serwera w poprzednich krokach.


```
#$serverName = "{server-name}"
#$serverAdmin = "{server-admin}"
#$serverAdminPassword = "{server-admin-password}"
$databaseName = "master"

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=$serverAdmin;Password=$serverAdminPassword" + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"



$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()


$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()
```


## <a name="create-new-adventureworkslt-sample-database-using-azure-powershell"></a>Tworzenie nowej przykładowej bazy danych AdventureWorksLT za pomocą programu Azure PowerShell

Poniższy fragment kodu służy do zaimportowania pliku bacpac przykładowej bazy danych AdventureWorksLT przy użyciu polecenia cmdlet [New-AzureRmSqlDatabaseImport](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqldatabaseimport). Plik bacpac znajduje się w magazynie Azure Blob Storage. Po uruchomieniu polecenia cmdlet służącego do importowania można monitorować postęp operacji importu za pomocą polecenia cmdlet [Get-AzureRmSqlDatabaseImportExportStatus](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqldatabaseimportexportstatus).
Element $StorageUri to właściwość adresu URL pliku bacpac wcześniej przekazana do portalu, która powinna mieć format podobny do następującego: https://{storage-account}.blob.core.windows.net/{container}/AdventureWorksLT.bacpac

```
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"

$databaseName = "AdventureWorksLT"
$databaseEdition = "Basic"
$databaseServiceLevel = "Basic"

$storageKeyType = "StorageAccessKey"
$storageUri = "{storage-uri}" # URL of bacpac file you uploaded to your storage account
$storageKey = "{storage-key}" # key1 in the Access keys setting of your storage account

$importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $resourceGroupName –ServerName $serverName –DatabaseName $databaseName –StorageKeytype $storageKeyType –StorageKey $storageKey -StorageUri $storageUri –AdministratorLogin $serverAdmin –AdministratorLoginPassword $securePassword –Edition $databaseEdition –ServiceObjectiveName $databaseServiceLevel -DatabaseMaxSizeBytes 5000000


Do {
     $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
     Write-host "Importing database..." $importStatus.StatusMessage
     Start-Sleep -Seconds 30
     $importStatus.Status
   }
   until ($importStatus.Status -eq "Succeeded")
$importStatus
```



## <a name="view-database-properties-using-azure-powershell"></a>Wyświetlanie właściwości bazy danych za pomocą programu Azure PowerShell

Po utworzeniu bazy danych wyświetl niektóre jej właściwości.

```
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
#$databaseName = "{database-name}"

$myDatabase = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName


Write-Host "Database name: " $myDatabase.DatabaseName
Write-Host "Server name: " $myDatabase.ServerName
Write-Host "Creation date: " $myDatabase.CreationDate
Write-Host "Database edition: " $myDatabase.Edition
Write-Host "Database performance level: " $myDatabase.CurrentServiceObjectiveName
Write-Host "Database status: " $myDatabase.Status
```

## <a name="connect-and-query-the-sample-database-using-azure-powershell"></a>Nawiązywanie połączenia z przykładową bazy danych i tworzenie dotyczących jej zapytań za pomocą programu Azure PowerShell

Uruchommy proste zapytanie do bazy danych AdventureWorksLT w celu sprawdzenia, czy możliwe jest nawiązanie połączenia. Następujący fragment kodu używa programu [.NET Framework Provider for SQL Server (System.Data.SqlClient)](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx) w celu nawiązania połączenia z bazą danych i utworzenia do niej zapytań. Tworzy on parametry połączenia na podstawie zmiennych użytych w poprzednich fragmentach kodu. Zastąp symbol zastępczy hasłem administratora serwera SQL Server.

```
#$serverName = {server-name}
#$serverAdmin = "{server-admin}"
#$serverAdminPassword = "{server-admin-password}"
#$databaseName = {database-name}

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=$serverAdmin;Password=$serverAdminPassword" + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"


$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()


$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()
```

## <a name="create-a-new-blank-database-using-azure-powershell"></a>Tworzenie nowej pustej bazy danych za pomocą programu Azure PowerShell

```
#$resourceGroupName = {resource-group-name}
#$serverName = {server-name}

$blankDatabaseName = "blankdb"
$blankDatabaseEdition = "Basic"
$blankDatabaseServiceLevel = "Basic"


$myBlankDatabase = New-AzureRmSqlDatabase -DatabaseName $blankDatabaseName -ServerName $serverName -ResourceGroupName $resourceGroupName -Edition $blankDatabaseEdition -RequestedServiceObjectiveName $blankDatabaseServiceLevel
$myBlankDatabase
```


## <a name="complete-azure-powershell-script-to-create-a-server-firewall-rule-and-database"></a>Kończenie skryptu programu Azure PowerShell w celu utworzenia serwera, reguły zapory i bazy danych



```
# Sign in to Azure and set the subscription to work with
########################################################

$SubscriptionId = "{subscription-id}"

Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId

# User variables
################

$myResourceGroupName = "{resource-group-name}"
$myResourceGroupLocation = "{resource-group-location}"

$myServerName = "{server-name}"
$myServerVersion = "12.0"
$myServerLocation = $myResourceGroupLocation
$myServerResourceGroupName = $myResourceGroupName
$myServerAdmin = "{server-admin}"
$myServerAdminPassword = "{server-admin-password}" 

$myServerFirewallRuleName = "{server-firewall-rule-name}"
$myServerFirewallStartIp = "{start-ip}"
$myServerFirewallEndIp = "{end-ip}"

$myDatabaseName = "AdventureWorksLT"
$myDatabaseEdition = "Basic"
$myDatabaseServiceLevel = "Basic"

$myStorageKeyType = "StorageAccessKey"
# Get these values from your Azure storage account:
$myStorageUri = "{http://your-storage-account.blob.core.windows.net/your-container/AdventureWorksLT.bacpac}"
$myStorageKey = "{your-storage-key}"


# Create new, or get existing resource group
############################################

$resourceGroupName = $myResourceGroupName
$resourceGroupLocation = $myResourceGroupLocation

$myResourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ea SilentlyContinue

if(!$myResourceGroup)
{
   Write-host "Creating resource group: $resourceGroupName"
   $myResourceGroup = New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else
{
   Write-host "Resource group $resourceGroupName already exists:"
}
$myResourceGroup


# Create a new, or get existing server
######################################

$serverName = $myServerName
$serverVersion = $myServerVersion
$serverLocation = $myServerLocation
$serverResourceGroupName = $myServerResourceGroupName

$serverAdmin = $myServerAdmin
$serverAdminPassword = $myServerAdminPassword

$securePassword = ConvertTo-SecureString –String $serverAdminPassword –AsPlainText -Force
$serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue
if(!$myServer)
{
   Write-host "Creating SQL server: $serverName"
   $myServer = New-AzureRmSqlServer -ResourceGroupName $serverResourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
}
else
{
   Write-host "SQL server $serverName already exists:"
}
$myServer


# View server properties
##########################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName 

Write-Host "Server name: " $myServer.ServerName
Write-Host "Fully qualified server name: $serverName.database.windows.net"
Write-Host "Server location: " $myServer.Location
Write-Host "Server version: " $myServer.ServerVersion
Write-Host "Server administrator login: " $myServer.SqlAdministratorLogin


# Create or update server firewall rule
#######################################

$serverFirewallRuleName = $myServerFirewallRuleName
$serverFirewallStartIp = $myServerFirewallStartIp
$serverFirewallEndIp = $myServerFirewallEndIp

$myFirewallRule = Get-AzureRmSqlServerFirewallRule -FirewallRuleName $serverFirewallRuleName -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue

if(!$myFirewallRule)
{
   Write-host "Creating server firewall rule: $serverFirewallRuleName"
   $myFirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $serverResourceGroupName -ServerName $serverName -FirewallRuleName $serverFirewallRuleName -StartIpAddress $serverFirewallStartIp -EndIpAddress $serverFirewallEndIp
}
else
{
   Write-host "Server firewall rule $serverFirewallRuleName already exists:"
}
$myFirewallRule


# Connect to the server and master database
###########################################
$databaseName = "master"

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=" + $myServer.SqlAdministratorLogin + ";Password=" + $myServerAdminPassword + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"


$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()

$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()


# Create the AdventureWorksLT database from a bacpac
####################################################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName

$databaseName = $myDatabaseName
$databaseEdition = $myDatabaseEdition
$databaseServiceLevel = $myDatabaseServiceLevel

$storageKeyType = $myStorageKeyType
$storageUri = $myStorageUri
$storageKey = $myStorageKey

$importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $resourceGroupName –ServerName $serverName –DatabaseName $databaseName –StorageKeytype $storageKeyType –StorageKey $storageKey -StorageUri $storageUri –AdministratorLogin $serverAdmin –AdministratorLoginPassword $securePassword –Edition $databaseEdition –ServiceObjectiveName $databaseServiceLevel -DatabaseMaxSizeBytes 5000000

Do {
     $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
     Write-host "Importing database..." $importStatus.StatusMessage
     Start-Sleep -Seconds 30
     $importStatus.Status
   }
   until ($importStatus.Status -eq "Succeeded")
$importStatus


# View database properties
##########################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$databaseName = $myDatabaseName

$myDatabase = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName

Write-Host "Database name: " $myDatabase.DatabaseName
Write-Host "Server name: " $myDatabase.ServerName
Write-Host "Creation date: " $myDatabase.CreationDate
Write-Host "Database edition: " $myDatabase.Edition
Write-Host "Database performance level: " $myDatabase.CurrentServiceObjectiveName
Write-Host "Database status: " $myDatabase.Status


# Query the database
####################

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=" + $myServer.SqlAdministratorLogin + ";Password=" + $myServerAdminPassword + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"

$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()

$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()


# Create a blank database
#########################

$blankDatabaseName = "blankdb"
$blankDatabaseEdition = "Basic"
$blankDatabaseServiceLevel = "Basic"


$myBlankDatabase = New-AzureRmSqlDatabase -DatabaseName $blankDatabaseName -ServerName $serverName -ResourceGroupName $resourceGroupName -Edition $blankDatabaseEdition -RequestedServiceObjectiveName $blankDatabaseServiceLevel
$myBlankDatabase
```



> [!TIP]
> W trakcie nauki możesz zaoszczędzić pieniądze, usuwając nieużywane bazy danych. Bazy danych w wersji Podstawowa możesz przywrócić w ciągu 7 dni. Nie usuwaj jednak serwera. Jeśli to zrobisz, nie będzie można odzyskać ani serwera, ani jego usuniętych baz danych.

## <a name="helper-snippets"></a>Fragmenty kodu pomocnika

```
# Get a list of Azure regions where you can create SQL resources

$sqlRegions = (Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" })
foreach ($region in $sqlRegions)
{
   $region.Location
}

# Clean up
# Delete a resource group (and all contained resources)
Remove-AzureRmResourceGroup -Name {resource-group-name}
```

> [!TIP]
> W trakcie nauki możesz zaoszczędzić pieniądze, usuwając nieużywane bazy danych. Bazy danych w wersji Podstawowa możesz przywrócić w ciągu siedmiu dni. Nie usuwaj jednak serwera. Jeśli to zrobisz, nie będzie można odzyskać ani serwera, ani jego usuniętych baz danych.
>

## <a name="next-steps"></a>Następne kroki
Po ukończeniu tego pierwszego samouczka z wprowadzeniem i utworzeniu bazy danych z pewnymi przykładowymi danymi możesz zapoznać się z kilkoma dodatkowymi samouczkami, które poszerzą zdobytą wiedzę. 

* Jeśli chcesz zacząć poznawać zabezpieczenia usługi Azure SQL Database, zobacz artykuł [Wprowadzenie do zabezpieczeń](sql-database-get-started-security.md).
* Jeśli posługujesz się programem Excel, naucz się [nawiązywać połączenie z bazą danych SQL w programie Excel](sql-database-connect-excel.md).
* Jeśli chcesz zacząć programować, wybierz język programowania w obszarze [Biblioteki połączeń dla usługi SQL Database i programu SQL Server](sql-database-libraries.md).
* Jeśli chcesz przenieść lokalne bazy danych programu SQL Server na platformę Azure, zobacz artykuł [Migrating a database to Azure SQL Database](sql-database-cloud-migrate.md) (Migracja bazy danych do usługi Azure SQL Database).
* Jeśli chcesz załadować dane z pliku CSV do nowej tabeli przy użyciu narzędzia wiersza polecenia BCP, zobacz artykuł [Loading data into SQL Database from a CSV file using BCP](sql-database-load-from-csv-with-bcp.md) (Ładowanie danych do usługi SQL Database z pliku CSV przy użyciu narzędzia BCP).
* Jeśli chcesz zacząć tworzyć tabele i inne obiekty, zobacz temat „Aby utworzyć tabelę” w artykule [Tworzenie tabeli](https://msdn.microsoft.com/library/ms365315.aspx).

## <a name="additional-resources"></a>Dodatkowe zasoby
[Co to jest SQL Database?](sql-database-technical-overview.md)


<!--HONumber=Dec16_HO2-->


