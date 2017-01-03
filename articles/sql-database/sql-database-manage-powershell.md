---
title: "Zarządzanie usługą Azure SQL Database za pomocą programu PowerShell | Microsoft Docs"
description: "Zarządzanie usługą Azure SQL Database za pomocą programu PowerShell."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: 3f21ad5e-ba99-4010-b244-5e5815074d31
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/15/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: adad6b8e27e0996559d5e6dacb8dd60fbf52a631
ms.openlocfilehash: 0c1ce1c29e447d9db4ef0df7873ef89cb835abee


---
# <a name="managing-azure-sql-database-using-powershell"></a>Zarządzanie usługą Azure SQL Database przy użyciu programu PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-manage-portal.md)
> * [Język Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
> * [Program PowerShell](sql-database-manage-powershell.md)
> 
> 

W tym temacie przedstawiono polecenia cmdlet programu PowerShell, które służą do wykonywania wielu zadań bazy danych Azure SQL. Pełną listę znajdziesz w temacie [Polecenia cmdlet usługi Azure SQL Database](https://msdn.microsoft.com/library/mt574084\(v=azure.300\).aspx).

> [!TIP]
> Aby uzyskać samouczek omawiający tworzenie serwera i opartej na serwerze zapory, wyświetlanie właściwości serwera, łączenie się z bazą danych master i jej odpytywanie, tworzenie przykładowej i pustej bazy danych, tworzenie zapytań o właściwości bazy danych, a także łączenie się z przykładową bazą danych i jej odpytywanie, zobacz [Samouczek z wprowadzeniem](sql-database-get-started-powershell.md).
>

## <a name="how-do-i-create-a-resource-group"></a>Jak utworzyć grupę zasobów?
Aby utworzyć grupę zasobów dla bazy danych SQL i powiązanych zasobów platformy Azure, użyj polecenia cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt759837\(v=azure.300\).aspx).

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Aby uzyskać więcej informacji, zobacz temat [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md) (Używanie programu Azure PowerShell z usługą Azure Resource Manager).
Kompletny samouczek znajdziesz w artykule [Wprowadzenie do serwerów, baz danych i reguł zapory usługi Azure SQL Database przy użyciu programu Azure PowerShell](sql-database-get-started-powershell.md).

## <a name="how-do-i-create-a-sql-database-server"></a>Jak utworzyć serwer bazy danych SQL?
Aby utworzyć serwer bazy danych SQL, użyj polecenia cmdlet [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715\(v=azure.300\).aspx). Jako parametr *server1* podaj nazwę serwera. Nazwy serwerów muszą być unikatowe dla wszystkich serwerów baz danych Azure SQL. Jeśli nazwa serwera jest już zajęta, zostanie wyświetlony komunikat o błędzie. Wykonanie tego polecenia może potrwać kilka minut. Grupa zasobów musi już istnieć w subskrypcji.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
$creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword


$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Aby uzyskać więcej informacji o serwerach, zobacz temat [Funkcje usługi Azure SQL Database](sql-database-features.md). Kompletny samouczek znajdziesz w artykule [Wprowadzenie do serwerów, baz danych i reguł zapory usługi Azure SQL Database przy użyciu programu Azure PowerShell](sql-database-get-started-powershell.md).

## <a name="how-do-i-create-a-sql-database-server-firewall-rule"></a>Jak utworzyć regułę zapory serwera bazy danych SQL?
Aby utworzyć regułę zapory w celu uzyskania dostępu do serwera, użyj polecenia cmdlet [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx). Uruchom następujące polecenie, zastępując początkowy i końcowy adres IP prawidłowymi wartościami dla danego klienta. Grupa zasobów i serwer muszą już istnieć w subskrypcji.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Aby zezwolić innym usługom Azure na uzyskiwanie dostępu do serwera, utwórz regułę zapory i dla parametrów `-StartIpAddress` oraz `-EndIpAddress` ustaw wartość **0.0.0.0**. Ta specjalna reguła zapory zezwala na dostęp do serwera dla całego ruchu platformy Azure.

Aby uzyskać więcej informacji, zobacz artykuł [Azure SQL Database Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx) (Zapora usługi Azure SQL Database) Kompletny samouczek znajdziesz w artykule [Wprowadzenie do serwerów, baz danych i reguł zapory usługi Azure SQL Database przy użyciu programu Azure PowerShell](sql-database-get-started-powershell.md).

## <a name="how-do-i-create-a-sql-database"></a>Jak utworzyć bazę danych SQL?
Aby utworzyć bazę danych SQL, użyj polecenia cmdlet [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339\(v=azure.300\).aspx). Grupa zasobów i serwer muszą już istnieć w subskrypcji. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Aby uzyskać więcej informacji, zobacz artykuł [Co to jest SQL Database](sql-database-technical-overview.md). Kompletny samouczek znajdziesz w artykule [Wprowadzenie do serwerów, baz danych i reguł zapory usługi Azure SQL Database przy użyciu programu Azure PowerShell](sql-database-get-started-powershell.md).

## <a name="how-do-i-change-the-performance-level-of-a-sql-database"></a>Jak zmienić poziom wydajności bazy danych SQL?
Aby zmienić poziom wydajności, przeskaluj bazę danych w górę lub w dół za pomocą polecenia cmdlet [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433\(v=azure.300\).aspx). Grupa zasobów, serwer i baza danych muszą już istnieć w subskrypcji. Ustaw wartość parametru `-RequestedServiceObjectiveName` na pojedynczą spację (tak jak w następującym fragmencie kodu), aby przejść na warstwę Podstawowa. Aby wybrać inną warstwę, ustaw wartość *S0*, *S1*, *P1*, *P6* itd., podobnie jak w poprzednim przykładzie.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Więcej informacji znajdziesz w artykule [Opcje i wydajność usługi SQL Database: poznaj, co jest dostępne w poszczególnych warstwach usług](sql-database-service-tiers.md). Aby uzyskać przykładowy skrypt, zobacz sekcję [Sample PowerShell script to change the service tier and performance level of your SQL database](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database) (Przykładowy skrypt programu PowerShell służący do zmiany warstwy usług i poziomu wydajności bazy danych SQL).

## <a name="how-do-i-copy-a-sql-database-to-the-same-server"></a>Jak skopiować bazę danych SQL na ten sam serwer?
Aby skopiować bazę danych SQL na ten sam serwer, użyj polecenia cmdlet [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/azure/mt603644\(v=azure.300\).aspx). Ustaw parametry `-CopyServerName` i `-CopyResourceGroupName` na takie same wartości jak w przypadku źródłowego serwera bazy danych i grupy zasobów.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Więcej informacji znajdziesz w artykule [Kopiowanie bazy danych usługi Azure SQL Database](sql-database-copy.md). Aby uzyskać przykładowy skrypt, zobacz [skrypt programu PowerShell służący do kopiowania bazy danych SQL](sql-database-copy-powershell.md#example-powershell-script).

## <a name="how-do-i-delete-a-sql-database"></a>Jak usunąć bazę danych SQL?
Aby usunąć bazę danych SQL, użyj polecenia cmdlet [Remove-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619368\(v=azure.300\).aspx). Grupa zasobów, serwer i baza danych muszą już istnieć w subskrypcji.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="how-do-i-delete-a-sql-database-server"></a>Jak usunąć serwer bazy danych SQL?
Aby usunąć serwer bazy danych SQL, użyj polecenia cmdlet [Remove-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603488\(v=azure.300\).aspx).

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="how-do-i-create-and-manage-elastic-pools-using-powershell"></a>Jak utworzyć pule elastyczne i nimi zarządzać za pomocą programu PowerShell?
Aby uzyskać szczegółowe informacje o tworzeniu pul elastycznych za pomocą programu PowerShell, zobacz temat [Tworzenie nowej puli elastycznej przy użyciu programu PowerShell](sql-database-elastic-pool-create-powershell.md).

Aby uzyskać szczegółowe informacje o zarządzaniu pulami elastycznymi za pomocą programu PowerShell, zobacz temat [Monitor and manage an elastic pool with PowerShell](sql-database-elastic-pool-manage-powershell.md) (Monitorowanie puli elastycznej i zarządzanie nią przy użyciu programu PowerShell).

## <a name="related-information"></a>Informacje pokrewne
* [Polecenia cmdlet usługi Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx)
* [Dokumentacja poleceń cmdlet platformy Azure](https://msdn.microsoft.com/library/azure/dn708514\(v=azure.300\).aspx)




<!--HONumber=Dec16_HO5-->


