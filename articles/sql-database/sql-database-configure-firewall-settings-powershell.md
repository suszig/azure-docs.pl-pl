---
title: "Konfigurowanie reguł zapory na poziomie serwera usługi Azure SQL Database przy użyciu programu PowerShell | Microsoft Docs"
description: "Informacje o konfigurowaniu zapory dla adresów IP uzyskujących dostęp do baz danych Azure SQL Database."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 30dcea72-61c1-48b6-8e1d-b1db2eb61567
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 890ca8d9b7a3a76c6584f9d06cc8bdf3b36f87c0


---
# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Konfigurowanie reguł zapory na poziomie serwera usługi Azure SQL Database przy użyciu programu PowerShell
> [!div class="op_single_selector"]
> * [Omówienie](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [Interfejs API REST](sql-database-configure-firewall-settings-rest.md)
> 
> 

Usługa Azure SQL Database zezwala na połączenia z serwerami i bazami danych przy użyciu reguł zapory. W celu selektywnego udzielania dostępu do bazy danych można zdefiniować ustawienia zapory na poziomie serwera i na poziomie bazy danych dla bazy danych master lub bazy danych użytkownika na serwerze usługi SQL Database.

> [!IMPORTANT]
> Aby umożliwić aplikacjom z platformy Azure łączenie się z serwerem bazy danych, należy włączyć połączenia platformy Azure. Aby uzyskać więcej informacji o regułach zapory i umożliwianiu połączeń z platformy Azure, zobacz artykuł [Azure SQL Database Firewall](sql-database-firewall-configure.md) (Zapora usługi Azure SQL Database). W przypadku nawiązywania połączeń wewnątrz granic chmury Azure może być konieczne otwarcie dodatkowych portów TCP. Aby uzyskać więcej informacji, zobacz sekcję „V12 of SQL Database: Outside vs inside” („SQL Database 12: na zewnątrz i wewnątrz”) w artykule [Ports beyond 1433 for ADO.NET 4.5 and SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md) (Porty inne niż 1433 dla platformy ADO.NET 4.5 i usługi SQL Database 12).
> 
> 

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Tworzenie reguł zapory serwera
Reguły zapory na poziomie serwera można tworzyć, aktualizować i usuwać przy użyciu programu Azure PowerShell.

Aby utworzyć nową regułę zapory na poziomie serwera, wykonaj polecenie cmdlet [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx). W poniższym przykładzie dopuszczony zostaje zakres adresów IP na serwerze Contoso.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'        

Aby zmienić istniejącą regułę zapory na poziomie serwera, wykonaj polecenie cmdlet [Set-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603789\(v=azure.300\).aspx). W poniższym przykładzie zostaje zmieniony zakres dopuszczalnych adresów IP dla reguły o nazwie ContosoFirewallRule.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –FirewallRuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Aby usunąć istniejącą regułę zapory na poziomie serwera, wykonaj polecenie cmdlet [Remove-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603588\(v=azure.300\).aspx). W poniższym przykładzie zostaje usunięta reguła o nazwie ContosoFirewallRule.

    Remove-AzureRmSqlServerFirewallRule –FirewallRuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Zarządzanie regułami zapory przy użyciu programu PowerShell
Za pomocą programu PowerShell można również zarządzać regułami zapory. Aby uzyskać więcej informacji, zobacz następujące tematy:

* [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx)
* [Remove-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603588\(v=azure.300\).aspx)
* [Set-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603789\(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603586\(v=azure.300\).aspx)

## <a name="next-steps"></a>Następne kroki
Aby uzyskać informacje o tworzeniu reguł zapory na poziomie serwera i na poziomie bazy danych przy użyciu języka Transact-SQL, zobacz artykuł [Konfigurowanie reguł zapory na poziomie serwera i na poziomie bazy danych usługi Azure SQL Database z użyciem języka T-SQL](sql-database-configure-firewall-settings-tsql.md).

Aby uzyskać informacje o tworzeniu reguł zapory na poziomie serwera za pomocą innych metod, zobacz:

* [Configure Azure SQL Database server-level firewall rules using the Azure portal](sql-database-configure-firewall-settings.md) (Konfigurowanie reguł zapory na poziomie serwera usługi Azure SQL Database za pomocą witryny Azure Portal)
* [Configure Azure SQL Database server-level firewall rules using the REST API](sql-database-configure-firewall-settings-rest.md) (Konfigurowanie reguł zapory na poziomie serwera usługi Azure SQL Database przy użyciu interfejsu API REST)

Aby uzyskać samouczek tworzenia bazy danych, zobacz [Create a SQL database in minutes using the Azure portal](sql-database-get-started.md) (Tworzenie bazy danych SQL w ciągu kilku minut za pomocą witryny Azure Portal).
Aby uzyskać pomoc podczas łączenia się z bazą danych Azure SQL z aplikacji innych firm lub aplikacji typu open source, zobacz [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx) (Przykłady kodu umożliwiające szybki start dla klienta usługi SQL Database).
Aby dowiedzieć się, jak przechodzić do baz danych, zobacz artykuł [Manage database access and login security](https://msdn.microsoft.com/library/azure/ee336235.aspx) (Zarządzanie zabezpieczeniami dostępu i logowania do bazy danych).

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Zabezpieczanie bazy danych](sql-database-security.md)
* [Usługa Security Center aparatu bazy danych programu SQL Server i bazy danych Azure SQL Database](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->



<!--HONumber=Dec16_HO1-->


