<properties 
    pageTitle="Konfiguracja nowej usługi SQL Database za pomocą programu PowerShell | Microsoft Azure" 
    description="Dowiedz się teraz, jak utworzyć nową bazę danych SQL za pomocą programu PowerShell. Typowe zadania dotyczące konfigurowania bazy danych można wykonywać za pomocą poleceń cmdlet programu PowerShell." 
    keywords="create new sql database,database setup"
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="05/09/2016"
    ms.author="sstein"/>

# Tworzenie nowej bazy danych SQL i wykonywanie typowych zadań dotyczących konfiguracji bazy danych przy użyciu poleceń cmdlet programu PowerShell 


> [AZURE.SELECTOR]
- [Portal Azure](sql-database-get-started.md)
- [PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Dowiedz się, jak utworzyć nową bazę danych SQL za pomocą poleceń cmdlet programu PowerShell. (W celu tworzenia elastycznych baz danych zobacz artykuł [Tworzenie nowej elastycznej puli baz danych przy użyciu programu PowerShell](sql-database-elastic-pool-create-powershell.md)).


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## Konfiguracja bazy danych: tworzenie grupy zasobów, serwera i reguły zapory

Skoro możesz już uruchamiać polecenia cmdlet w wybranej subskrypcji Azure, następnym krokiem jest ustanowienie grupy zasobów, która zawiera serwer, w którym zostanie utworzona baza danych. Możesz edytować poniższe polecenie, aby użyć dowolnej wybranej przez siebie prawidłowej lokalizacji. Uruchom polecenie **(Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" }).Location**, aby uzyskać listę prawidłowych lokalizacji.

Uruchom poniższe polecenie, aby utworzyć nową grupę zasobów:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "West US"

Po pomyślnym utworzeniu nowej grupy zasobów na ekranie pojawi się informacja zawierająca tekst **ProvisioningState: Succeeded**.


### Tworzenie serwera 

Usługi SQL Database są tworzone wewnątrz serwerów usługi SQL Database na platformie Azure. Uruchom polecenie **New-AzureRmSqlServer**, aby utworzyć nowy serwer. Jako parametr ServerName podaj nazwę serwera. Nazwa serwera musi być unikatowa w zakresie wszystkich serwerów SQL platformy Azure, więc podanie zajętej nazwy serwera spowoduje błąd. Warto także zauważyć, że wykonanie tego polecenia może potrwać kilka minut. Możesz edytować polecenie, aby użyć dowolnej wybranej prawidłowej lokalizacji, ale najlepiej użyć lokalizacji zastosowanej do grupy zasobów utworzonej w poprzednim kroku.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "West US" -ServerVersion "12.0"

Po uruchomieniu tego polecenia otworzy się okno z pytaniem o **nazwę użytkownika** i **hasło**. Nie są to poświadczenia platformy Azure. Wprowadź nazwę użytkownika i hasło, które będą poświadczeniami administratora do utworzenia na nowym serwerze.

Szczegóły serwera są wyświetlane po jego pomyślnym utworzeniu.

### Konfigurowanie reguły zapory serwera, aby zezwolić na dostęp do serwera

Należy ustanowić regułę zapory w celu uzyskania dostępu do serwera. Uruchom następujące polecenie, zastępując początkowy i końcowy adres IP prawidłowymi wartościami dla danego komputera.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Szczegóły reguły zapory są wyświetlane po jej pomyślnym utworzeniu.

Aby zezwolić innym usługom Azure na dostęp do serwera, dodaj regułę zapory i ustaw wartość 0.0.0.0 we właściwościach StartIpAddress (początkowy adres IP) i EndIpAddress (końcowy adres IP). Należy zauważyć, że pozwala to na dostęp do serwera ruchu pochodzącego z dowolnej subskrypcji platformy Azure.

Aby uzyskać więcej informacji, zobacz artykuł [Azure SQL Database Firewall](sql-database-firewall-configure.md) (Zapora usługi Azure SQL Database)


## Tworzenie nowej bazy danych SQL

Skoro masz już grupę zasobów, serwer i skonfigurowaną regułę zapory, możesz uzyskać dostęp do serwera.

Następujące polecenie tworzy nową (pustą) bazę danych SQL w warstwie usług Standardowa na poziomie wydajności S1:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Szczegóły bazy danych są wyświetlane po jej pomyślnym utworzeniu.

## Skrypt PowerShell tworzący nową bazę danych SQL

    $SubscriptionId = "4cac86b0-1e56-bbbb-aaaa-000000000000"
    $ResourceGroupName = "resourcegroupname"
    $Location = "Japan West"
    
    $ServerName = "uniqueservername"
    
    $FirewallRuleName = "rule1"
    $FirewallStartIP = "192.168.0.0"
    $FirewallEndIp = "192.168.0.0"
    
    $DatabaseName = "database1"
    $DatabaseEdition = "Standard"
    $DatabasePerfomanceLevel = "S1"
    
    
    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionId
    
    $ResourceGroup = New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
    
    $Server = New-AzureRmSqlServer -ResourceGroupName $ResourceGroupName -ServerName $ServerName -Location $Location -ServerVersion "12.0"
    
    $FirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $ServerName -FirewallRuleName $FirewallRuleName -StartIpAddress $FirewallStartIP -EndIpAddress $FirewallEndIp
    
    $SqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -Edition $DatabaseEdition -RequestedServiceObjectiveName $DatabasePerfomanceLevel
    
    $SqlDatabase
    


## Następne kroki
Po utworzeniu nowej bazy danych SQL i wykonaniu podstawowych zadań dotyczących jej konfiguracji możesz przystąpić do na następujących czynności:

- [Nawiązywanie połączenia z usługą SQL Database za pomocą programu SQL Server Management Studio i wykonywanie przykładowego zapytania T-SQL](sql-database-connect-query-ssms.md)


## Dodatkowe zasoby

- [Usługa Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)



<!--HONumber=Jun16_HO2-->


