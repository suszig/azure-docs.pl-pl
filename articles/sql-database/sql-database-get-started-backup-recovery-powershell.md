---
title: "Wprowadzenie do tworzenia kopii zapasowych i przywracania baz danych Azure SQL w celu ochrony i odzyskiwania danych przy użyciu programu Azure PowerShell | Dokumentacja firmy Microsoft"
description: "W tym samouczku przedstawiono sposób przywracania automatycznych kopii zapasowych do punktu w czasie, przechowywania automatycznych kopii zapasowych w magazynie usługi Azure Recovery Services oraz przywracania ich z magazynu usługi Azure Recovery Services za pomocą programu PowerShell"
keywords: "samouczek usługi sql database"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: hero-article
ms.date: 12/19/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 68a4ed7aad946dda644a0f085c48fd33f453e018
ms.openlocfilehash: 15d5cb803332133c8015a8ba23ca5751b8abc29a


---


# <a name="get-started-with-backup-and-restore-for-data-protection-and-recovery-using-powershell"></a>Wprowadzenie do tworzenia kopii zapasowych i przywracania w celu ochrony i odzyskiwania danych za pomocą programu PowerShell

Korzystając z tego samouczka ułatwiającego rozpoczęcie pracy, nauczysz się wykonywać następujące czynności za pomocą programu Azure PowerShell:

- Wyświetlanie istniejących kopii zapasowych bazy danych
- Przywracanie bazy danych do określonego punktu w czasie
- Konfigurowanie długoterminowego przechowywania pliku kopii zapasowej bazy danych w magazynie usługi Azure Recovery Services
- Przywracanie bazy danych z magazynu usługi Azure Recovery Services

**Szacowany czas**: ukończenie tego samouczka zajmuje około 30 minut (przy założeniu, że spełniasz już wymagania wstępne).


## <a name="prerequisites"></a>Wymagania wstępne

* Musisz mieć konto platformy Azure. Możesz [utworzyć konto bezpłatnej wersji próbnej Azure](/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Musisz połączyć się z usługą Azure przy użyciu konta, które jest elementem członkowskim roli współautora lub właściciela subskrypcji. Aby uzyskać więcej informacji o kontroli dostępu na podstawie ról (RBAC, role-based access control), zobacz [Getting started with access management in the Azure portal](../active-directory/role-based-access-control-what-is.md) (Wprowadzenie do zarządzania dostępem w witrynie Azure Portal).

* Wymagana jest najnowsza wersja programu Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz artykuł [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Instalowanie i konfigurowanie programu Azure PowerShell).

* Wykonano kroki samouczka [Rozpoczynanie pracy z serwerami, bazami danych i regułami zapory usługi Azure SQL Database przy użyciu witryny Azure Portal i programu SQL Server Management Studio](sql-database-get-started.md) lub odpowiedniej [wersji dla programu PowerShell](sql-database-get-started-powershell.md). W przeciwnym razie przed kontynuowaniem wykonaj kroki tego samouczka dotyczącego wymagań wstępnych lub uruchom skrypt programu PowerShell wskazany pod koniec samouczka w [wersji dla programu PowerShell](sql-database-get-started-powershell.md).

> [!TIP]
> Te same czynności przedstawione w samouczku ułatwiającym rozpoczęcie pracy można wykonać przy użyciu witryny [Azure Portal](sql-database-get-started-backup-recovery.md).

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="view-the-oldest-restore-point-from-the-service-generated-backups-of-a-database"></a>Wyświetlanie najstarszego punktu przywracania z kopii zapasowych bazy danych wygenerowanych przez usługę

W tej części samouczka zapoznasz się z informacjami o najstarszym punkcie przywracania związanym z [wygenerowanymi przez usługi automatycznymi kopiami zapasowymi](sql-database-automated-backups.md) bazy danych. 

Możesz przywrócić bazę danych do dowolnego punktu w czasie pomiędzy najwcześniejszym punktem przywracania i ostatnią dostępną kopią zapasową (wcześniejszą o 6 minut od czasu bieżącego). 

W poniższym fragmencie kodu polecenie cmdlet [Get AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqldatabase) jest użyte do uzyskania najwcześniejszego punktu przywracania bazy danych, która ma zostać przywrócona. Czas jest zwracany jako czas UTC, ale następujące fragmenty kodu pokazują sposób pracy w czasie lokalnym. Najnowszy dostępny punkt przywracania aktywnej bazy danych pochodzi zwykle sprzed około sześciu minut, więc w celu użycia najnowszego punktu przywracania wystarczy po prostu ustawić czas bieżący minus sześć minut. 

```
# Get available restore points

$resourceGroupName = "{resource-group-name}"
$serverName = "{server-name}"
$databaseName = "{database-name}"

$databaseToRestore = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName

$earliestRestorePoint = $databaseToRestore.EarliestRestoreDate.ToLocalTime()
$latestRestorePoint = (Get-Date).AddMinutes(-6).ToLocalTime()

Write-Host "'$databaseName' on '$serverName' can be restored to any point-in-time between '$earliestRestorePoint' thru '$latestRestorePoint'"
```



## <a name="restore-a-database-to-a-previous-point-in-time"></a>Przywracanie bazy danych do określonego punktu w czasie

W tej części samouczka przywrócisz bazę danych do nowej bazy danych z określonego punktu w czasie. 

>[!NOTE]
>Nie można zmienić serwera, na którym odbywa się przywracanie do określonego punktu w czasie. Aby przywrócić na innym serwerze, należy użyć funkcji [przywracania geograficznego](sql-database-disaster-recovery.md#recover-using-geo-restore). Warto również zauważyć, że można przywrócić bazę danych do [elastycznej puli baz danych](sql-database-elastic-jobs-overview.md) lub do innej warstwy cenowej. 

W poniższym fragmencie kodu użyto polecenia cmdlet [Restore-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/restore-azurermsqldatabase), aby przywrócić bazę $databaseToRestore ustawioną w poprzednim fragmencie kodu. Parametr **-PointInTime** wymaga czasu w formacie UTC, na przykład: *12/09/2016 20:00:00*. Ten fragment kodu konwertuje czas lokalny.

```
# Restore a database to a previous point in time

#$resourceGroupName = {resource-group-name}
#$serverName = {server-name}
$newRestoredDatabaseName = "{new-database-name}"
$localTimeToRestoreTo = "{12/9/2016 12:00:00 PM}" # change to a valid restore point for your database
$restorePointInTime = (Get-Date $localTimeToRestoreTo).ToUniversalTime()
$newDatabaseEdition = "Basic"
$newDatabaseServiceLevel = "Basic"

Write-Host "Restoring database '$databaseName' to its state at:"(Get-Date $restorePointInTime).ToLocalTime()", to a new database named '$newRestoredDatabaseName'."

$restoredDb = Restore-AzureRmSqlDatabase -FromPointInTimeBackup -PointInTime $restorePointInTime -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $newRestoredDatabaseName -Edition $newDatabaseEdition -ServiceObjectiveName $newDatabaseServiceLevel `
 -ResourceId $databaseToRestore.ResourceID

$restoredDb
```

> [!NOTE]
> W tym miejscu możesz połączyć się z przywróconą bazą danych przy użyciu programu [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx), aby wykonać niezbędne zadania, takie jak [wyodrębnienie bitu danych z przywróconej bazy danych w celu skopiowania do istniejącej bazy danych lub usunięcie istniejącej bazę danych i zmiana nazwy przywróconej bazy danych na nazwę istniejącej nazwę bazy danych](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="configure-long-term-retention-of-automated-backups-in-an-azure-recovery-services-vault"></a>Konfigurowanie długoterminowego przechowywania automatycznych kopii zapasowych w magazynie usługi Azure Recovery Services 

W tej części samouczka możesz [skonfigurować magazyn usługi Azure Recovery Services do przechowywania automatycznych kopii zapasowych](sql-database-long-term-retention.md) przez okres dłuższy niż okres przechowywania w warstwie usługi (do 10 lat). 


> [!TIP]
> Aby usunąć kopie zapasowe podlegające długoterminowemu przechowywaniu, zobacz [Delete long-term retention backups](sql-database-long-term-retention-delete.md) (Usuwanie kopii zapasowych podlegających długoterminowemu przechowywaniu).


### <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu usługi Recovery Services

> [!IMPORTANT]
> Magazyn musi znajdować się w tym samym regionie co serwer logiczny Azure SQL i musi używać tej samej grupy zasobów co serwer logiczny.

W poniższym fragmencie użyto poleceń cmdlet [New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices/v2.3.0/new-azurermrecoveryservicesvault) i [Set-AzureRmRecoveryServicesBackupProperties](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices/v2.3.0/set-azurermrecoveryservicesbackupproperties), aby utworzyć nowy magazyn usługi Recovery Services i ustawić poziom nadmiarowości kopii zapasowych w magazynie. 

```
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-to-use-the-recovery-vault-you-just-created-for-its-long-term-retention-backups"></a>Skonfiguruj serwer do korzystania z nowo utworzonego magazynu Recovery Services w celu długoterminowego przechowywania kopii zapasowych

W poniższym fragmencie użyto polecenia cmdlet [Set-AzureRmSqlServerBackupLongTermRetentionVault](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/set-azurermsqlserverbackuplongtermretentionvault), aby skojarzyć utworzony wcześniej magazyn usługi Recovery Services z określonym serwerem Azure SQL.

```
# Set your server to use the vault to for long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a>Tworzenie zasad przechowywania

Zasady przechowywania określają czas przechowywania kopii zapasowej bazy danych. 

W poniższym fragmencie użyto polecenia cmdlet [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject), aby uzyskać domyślne zasady przechowywania, które zostaną wykorzystane jako szablon do utworzenia nowych zasad. Ustawiamy szablon na przechowywanie kopii zapasowej przez 2 lata, a następnie uruchamiamy polecenie [New AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/new-azurermrecoveryservicesbackupprotectionpolicy), aby utworzyć nową zasadę. 

Należy zauważyć, że niektóre polecenia cmdlet wymagają przed uruchomieniem skonfigurowania kontekstu magazynu ([Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices/v2.3.0/set-azurermrecoveryservicesvaultcontext)), dlatego to polecenie cmdlet występuje w kilku powiązanych fragmentach kodu. Ustawiamy kontekst, ponieważ zasady są częścią magazynu. Można utworzyć wiele zasad przechowywania dla każdego magazynu, a następnie zastosować żądane zasady do określonych baz danych. 


```
# Retrieve the default retention policy for the AzureSQLDatabase workload type
$retentionPolicy = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase

# Set the retention value to two years (you can set to any time between 1 week and 10 years)
$retentionPolicy.RetentionDurationType = "Years"
$retentionPolicy.RetentionCount = 2
$retentionPolicyName = "my2YearRetentionPolicy"

# Set the vault context to the vault you are creating the policy for
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Create the new policy
$policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name $retentionPolicyName -WorkloadType AzureSQLDatabase -retentionPolicy $retentionPolicy
$policy
```

### <a name="configure-a-database-to-use-the-previously-defined-retention-policy"></a>Konfigurowanie bazy danych do korzystania z wcześniej zdefiniowanej zasady przechowywania

W poniższym fragmencie użyto polecenia cmdlet [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/set-azurermsqldatabasebackuplongtermretentionpolicy), aby zastosować nową zasadę do określonej bazy danych.

```
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

> [!IMPORTANT]
> Skonfigurowane kopie zapasowe zostaną wyświetlone w magazynie w ciągu następnych siedmiu dni. Pracę z tym samouczkiem można kontynuować po pojawieniu się kopii zapasowych w magazynie.

## <a name="view-backup-info-and-backups-in-long-term-retention"></a>Wyświetlanie informacji o kopiach zapasowych i kopii zapasowych podlegających długoterminowemu przechowywaniu

W tej części samouczka wyświetlisz informacje dotyczące kopii zapasowych bazy danych podlegających [długoterminowemu przechowywaniu kopii zapasowych](sql-database-long-term-retention.md). 

W następujących fragmentach kodu użyto poleceń cmdlet [Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupcontainer), [Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupitem) i [Get AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackuprecoverypoint), aby wykonać zapytania o informacje z magazynu.

```
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$databaseNeedingRestore = $databaseName

# Set the vault context to the vault we want to restore from
#$vault = Get-AzureRmRecoveryServicesVault -ResourceGroupName $resourceGroupName
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# the following commands find the container associated with the server 'myserver' under resource group 'myresourcegroup'
$container = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL -FriendlyName $vault.Name

# Get the long-term retention metadata associated with a specific database
$item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name $databaseNeedingRestore


# Get all available backups for the previously indicated database
# Optionally, set the -StartDate and -EndDate parameters to return backups within a specific time period
$availableBackups = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item
$availableBackups
```


## <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Przywracanie bazy danych z kopii zapasowej podlegającej długoterminowemu przechowywaniu kopii zapasowych

W tej części samouczka przywrócisz bazę danych do nowej bazy danych z kopii zapasowej w magazynie usługi Azure Recovery Services.

Podobnie jak w przypadku fragmentów kodu dotyczących punktu w czasie omówionych wcześniej w tym samouczku, przywracanie z kopii zapasowej polegającej przechowywaniu długoterminowemu wymaga użycia polecenia cmdlet [Restore-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/restore-azurermsqldatabase), jednak tym razem z zastosowaniem parametru **-FromLongTermRetentionBackup** (zamiast użytego wcześniej parametru **-FromPointInTimeBackup**).

```
# Restore the most recent backup: $availableBackups[0]
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$restoredDatabaseName = "{new-database-name}"
$edition = "Basic"
$performanceLevel = "Basic"

$restoredDb = Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $availableBackups[0].Id -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $restoredDatabaseName -Edition $edition -ServiceObjectiveName $performanceLevel
$restoredDb
```


> [!NOTE]
> W tym miejscu możesz połączyć się z przywróconą bazą danych przy użyciu programu SQL Server Management Studio, aby wykonać niezbędne zadania, takie jak [wyodrębnienie bitu danych z przywróconej bazy danych w celu skopiowania do istniejącej bazy danych lub usunięcie istniejącej bazę danych i zmiana nazwy przywróconej bazy danych na istniejącą nazwę bazy danych](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="complete-azure-powershell-script-to-restore-from-a-previous-point-in-time-and-to-configure-and-restore-from-long-term-backup-retention-using-powershell"></a>Wykonywanie skryptu programu Azure PowerShell w celu przywrócenia bazy danych z wcześniejszego punktu w czasie oraz skonfigurowania i przywrócenia bazy danych podlegającej przechowywaniu długoterminowemu za pomocą programu PowerShell

> [!NOTE]
> Jeśli w magazynie nie ma żadnej kopii zapasowej, podczas próby przywrócenia najnowszej kopii zapasowej na końcu tego skryptu wystąpi wyjątek *Nie można indeksować w tablicy o wartości null*.

```
# Sign in to Azure and set the subscription to work with
########################################################

$subscriptionId = "{subscription-id}"

Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId


# User variables
################

$myResourceGroupName = "{resource-group-name}"
$myServerName = "{server-name}"
$myDatabaseToRestoreFromPointInTime = "{database-name}"
$myLocalTimeToRestoreTo = "{12/08/2016 16:00:00}" # change to a valid restore point for your database
$myNewDatabaseRestoredFromPointInTime = "{new-database-name}"

$myRecoveryServiceVaultName = "{vault-name}"
$myRetentionPolicyDurationType = "{duration-period}" # set to Years, Months, or Weeks
$myRetentionPolicyDuration = {2}
$myRetentionPolicyName = "{retention-policy-name}"
$myDatabaseToRestoreFromLTR = "{database-name}"
$myNewDatabaseRestoredFromLTR = "{new-database-name}"


# Get available restore points for a specific database
######################################################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$databaseName = $myDatabaseToRestoreFromPointInTime


$databaseToRestore = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName

$earliestRestorePoint = $databaseToRestore.EarliestRestoreDate.ToLocalTime()
$latestRestorePoint = (Get-Date).AddMinutes(-6).ToLocalTime()

Write-Host "'$databaseName' on '$serverName' can be restored to any point-in-time between '$earliestRestorePoint' thru '$latestRestorePoint'"


# Restore a database to a previous point in time
################################################

$newRestoredDatabaseName = $myNewDatabaseRestoredFromPointInTime
$localTimeToRestoreTo = $myLocalTimeToRestoreTo
$restorePointInTime = (Get-Date $localTimeToRestoreTo).ToUniversalTime()
$newDatabaseEdition = "Basic"
$newDatabaseServiceLevel = "Basic"

Write-Host "Restoring database '$databaseName' to its state at:"(Get-Date $restorePointInTime).ToLocalTime()", to a new database named '$newRestoredDatabaseName'."

$restoredDb = Restore-AzureRmSqlDatabase -FromPointInTimeBackup -PointInTime $restorePointInTime -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $newRestoredDatabaseName -Edition $newDatabaseEdition -ServiceObjectiveName $newDatabaseServiceLevel `
 -ResourceId $databaseToRestore.ResourceID

$restoredDb



# CONFIGURE LONG-TERM RETENTION

# Create a recovery services vault
##################################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = $myRecoveryServiceVaultName

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $resourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
$vault

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id

# Retrieve the default retention policy for the AzureSQLDatabase workload type
##############################################################################

$retentionPolicy = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase

# Set the retention values
$retentionPolicy.RetentionDurationType = $myRetentionPolicyDurationType
$retentionPolicy.RetentionCount = $myRetentionPolicyDuration

$retentionPolicyName = $myRetentionPolicyName

# Set the vault context to the vault you are creating the policy for
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Create the new policy
$policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name $retentionPolicyName -WorkloadType AzureSQLDatabase -retentionPolicy $retentionPolicy
$policy

$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id


$databaseNeedingRestore = $myDatabaseToRestoreFromLTR

# Set the vault context to the vault we want to restore from
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Get the container associated with your vault
$container = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL -FriendlyName $vault.Name

# Get the long-term retention metadata associated with a specific database
$item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name $databaseNeedingRestore


# Get all available backups for the previously indicated database
# Optionally, set the -StartDate and -EndDate parameters to return backups within a specific time period
$availableBackups = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item
$availableBackups
###

# This command restores the most recent backup: $availableBackups[0]
$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$restoredDatabaseName = $myNewDatabaseRestoredFromLTR
$edition = "Basic"
$performanceLevel = "Basic"

$restoredDbFromLtr = Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $availableBackups[0].Id -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $restoredDatabaseName -Edition $edition -ServiceObjectiveName $performanceLevel

$restoredDbFromLtr
```

## <a name="next-steps"></a>Następne kroki

- Aby dowiedzieć się więcej o automatycznych kopiach zapasowych generowanych przez usługi, zobacz artykuł dotyczący [automatycznych kopii zapasowych](sql-database-automated-backups.md)
- Aby dowiedzieć się więcej o długoterminowym przechowywaniu kopii zapasowych, zobacz temat dotyczący [długoterminowego przechowywania kopii zapasowych](sql-database-long-term-retention.md)
- Aby dowiedzieć się więcej o przywracaniu z kopii zapasowych, zobacz temat dotyczący [przywracania z kopii zapasowej](sql-database-recovery-using-backups.md)


<!--HONumber=Dec16_HO4-->


