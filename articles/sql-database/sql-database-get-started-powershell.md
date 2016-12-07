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
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 08/19/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 87e52fe29f659577d7dc0c9661ebde2c1c475cfc


---
# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>Tworzenie bazy danych SQL i wykonywanie typowych zadań dotyczących konfiguracji bazy danych przy użyciu poleceń cmdlet programu PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-get-started.md)
> * [PowerShell](sql-database-get-started-powershell.md)
> * [C#](sql-database-get-started-csharp.md)
> 
> 

Dowiedz się, jak utworzyć bazę danych SQL za pomocą poleceń cmdlet programu PowerShell. (W celu tworzenia elastycznych baz danych zobacz artykuł [Tworzenie nowej elastycznej puli baz danych przy użyciu programu PowerShell](sql-database-elastic-pool-create-powershell.md)).

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Konfiguracja bazy danych: tworzenie grupy zasobów, serwera i reguły zapory
Skoro możesz już uruchamiać polecenia cmdlet dla wybranej subskrypcji platformy Azure, następnym krokiem jest ustanowienie grupy zasobów zawierającej serwer, na którym zostanie utworzona baza danych. Możesz edytować poniższe polecenie, aby użyć dowolnej wybranej przez siebie prawidłowej lokalizacji. Uruchom polecenie **(Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" }).Location**, aby uzyskać listę prawidłowych lokalizacji.

Uruchom poniższe polecenie, aby utworzyć grupę zasobów:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Tworzenie serwera
Bazy danych SQL są tworzone wewnątrz serwerów usługi Azure SQL Database. Uruchom polecenie [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715\(v=azure.300\).aspx), aby utworzyć serwer. Nazwa serwera musi być unikatowa dla wszystkich serwerów usługi Azure SQL Database. Jeśli nazwa serwera jest już zajęta, zostanie wyświetlony komunikat o błędzie. Warto także zauważyć, że wykonanie tego polecenia może potrwać kilka minut. Możesz edytować polecenie, aby użyć dowolnie wybranej prawidłowej lokalizacji, ale najlepiej użyć lokalizacji zastosowanej do grupy zasobów utworzonej w poprzednim kroku.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Po uruchomieniu tego polecenia zostanie wyświetlony monit o podanie nazwy użytkownika i hasła. Nie wprowadzaj swoich poświadczeń platformy Azure. Zamiast tego wprowadź nazwę użytkownika i hasło do utworzenia jako administrator serwera. Skrypt w dolnej części tego artykułu pokazuje, jak ustawić poświadczenia serwera w kodzie.

Szczegóły serwera są wyświetlane po jego pomyślnym utworzeniu.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Konfigurowanie reguły zapory serwera, aby zezwolić na dostęp do serwera
Aby uzyskać dostęp do serwera, należy ustanowić regułę zapory. Uruchom polecenie [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx), zastępując początkowy i końcowy adres IP prawidłowymi wartościami dla danego komputera.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Szczegóły reguły zapory są wyświetlane po jej pomyślnym utworzeniu.

Aby zezwolić innym usługom platformy Azure na dostęp do serwera, dodaj regułę zapory i ustaw wartość 0.0.0.0 dla parametrów StartIpAddress i EndIpAddress. Ta reguła pozwala na przepływ do serwera ruchu pochodzącego z dowolnej subskrypcji platformy Azure.

Aby uzyskać więcej informacji, zobacz artykuł [Azure SQL Database Firewall](sql-database-firewall-configure.md) (Zapora usługi Azure SQL Database)

## <a name="create-a-sql-database"></a>Tworzenie bazy danych SQL
Skoro masz już grupę zasobów, serwer i skonfigurowaną regułę zapory, możesz uzyskać dostęp do serwera.

Następujące polecenie [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339\(v=azure.300\).aspx) tworzy (pustą) bazę danych SQL w warstwie usług Standardowa na poziomie wydajności S1:

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Szczegóły bazy danych są wyświetlane po jej pomyślnym utworzeniu.

## <a name="create-a-sql-database-powershell-script"></a>Skrypt programu PowerShell tworzący bazę danych SQL
Poniższy skrypt programu PowerShell tworzy bazę danych SQL i jej wszystkie zasoby zależne. Zamień wszystkie ciągi `{variables}` na wartości specyficzne dla Twojej subskrypcji i Twoich zasobów (usuń znaki **{}** podczas ustawiania wartości).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation

    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"

    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds

    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip

    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp


    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"

    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo


    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Następne kroki
Po utworzeniu bazy danych SQL i wykonaniu podstawowych zadań dotyczących jej konfiguracji możesz przystąpić do następujących czynności:

* [Zarządzanie usługą SQL Database za pomocą programu PowerShell](sql-database-manage-powershell.md)
* [Nawiązywanie połączenia z usługą SQL Database za pomocą programu SQL Server Management Studio i wykonywanie przykładowego zapytania T-SQL](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Polecenia cmdlet usługi Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx)
* [Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)




<!--HONumber=Nov16_HO2-->


