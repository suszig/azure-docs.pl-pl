---
title: "Tworzenie nowej elastycznej puli baz danych przy użyciu programu PowerShell | Microsoft Docs"
description: "Dowiedz się, jak skalować w poziomie zasoby usługi Azure SQL Database za pomocą programu PowerShell, tworząc skalowalne elastyczne pule baz danych do zarządzania wieloma bazami danych."
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 37a707ee-9223-43ae-8c35-1ccafde8b83e
ms.service: sql-database
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 05/27/2016
ms.author: srinia
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: daf8bd6421ae563e542b0874a6e7748a3ca52738


---
# <a name="create-a-new-elastic-database-pool-with-powershell"></a>Tworzenie nowej elastycznej puli baz danych przy użyciu programu PowerShell
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](sql-database-elastic-pool-create-portal.md)
> * [PowerShell](sql-database-elastic-pool-create-powershell.md)
> * [C#](sql-database-elastic-pool-create-csharp.md)
> 
> 

Dowiedz się, jak utworzyć [elastyczną pulę baz danych](sql-database-elastic-pool.md) za pomocą poleceń cmdlet programu PowerShell. 

Typowe kody błędów można znaleźć w artykule [SQL error codes for SQL Database client applications: Database connection error and other issues](sql-database-develop-error-messages.md) (Kody błędów SQL dla aplikacji klienckich usługi SQL Database: błąd połączenia z bazą danych i inne problemy).

> [!NOTE]
> Pule elastyczne są ogólnodostępne we wszystkich regionach platformy Azure oprócz Północno-środkowych stanów USA oraz Indii Zachodnich, gdzie są obecnie dostępne w wersji zapoznawczej.  Pule elastyczne zostaną udostępnione ogólnie w tych regionach tak szybko, jak to możliwe. Ponadto pule elastyczne aktualnie nie obsługują baz danych korzystających z [operacji OLTP w pamięci lub analizy w pamięci](sql-database-in-memory.md).
> 
> 

Wymagany jest działający program Azure PowerShell w wersji 1.0 lub nowszej. Aby uzyskać szczegółowe informacje, zobacz artykuł [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Instalowanie i konfigurowanie programu Azure PowerShell).

## <a name="create-a-new-pool"></a>Tworzenie nowej puli
Polecenie cmdlet [New-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378\(v=azure.300\).aspx) tworzy nową pulę. Wartości jednostek eDTU na pulę oraz minimalne i maksymalne liczby jednostek DTU są ograniczone przez wartość warstwy usługi (Podstawowa, Standardowa lub Premium). Zobacz [limity liczby jednostek eDTU i magazynu dla elastycznych pul i elastycznych baz danych](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## <a name="create-a-new-elastic-database-in-a-pool"></a>Tworzenie nowej elastycznej bazy danych w puli
Użyj polecenia cmdlet [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339\(v=azure.300\).aspx), podając pulę docelową jako parametr **ElasticPoolName**. Aby przenieść istniejącą bazę danych do puli, zobacz temat [Move a database into an elastic pool](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool) (Przenoszenie bazy danych do puli elastycznej).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="create-a-pool-and-populate-it-with-multiple-new-databases"></a>Tworzenie puli i wypełnianie jej wieloma nowymi bazami danych
Tworzenie wielu baz danych w puli może trwać długo, jeśli odbywa się za pomocą portalu lub poleceń cmdlet programu PowerShell, które tworzą tylko pojedyncze bazy danych. Aby zautomatyzować tworzenie w nowej puli, skorzystaj ze skryptu [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## <a name="example-create-a-pool-using-powershell"></a>Przykład: tworzenie puli przy użyciu programu PowerShell
Ten skrypt tworzy nową grupę zasobów Azure i nowy serwer. Po wyświetleniu monitu podaj nazwę użytkownika i hasło administratora dla nowego serwera (inne niż poświadczenia Azure).

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## <a name="next-steps"></a>Następne kroki
* [Zarządzanie pulą](sql-database-elastic-pool-manage-powershell.md)
* [Tworzenie zadań elastycznych](sql-database-elastic-jobs-overview.md) Zadania elastyczne umożliwiają uruchamianie skryptów T-SQL na dowolnej liczbie baz danych w puli.
* [Skalowanie w poziomie przy użyciu usługi Azure SQL Database](sql-database-elastic-scale-introduction.md): użyj narzędzi elastycznej bazy danych do skalowania w poziomie.




<!--HONumber=Nov16_HO2-->


