---
title: "Reguły zapory na poziomie serwera usługi Azure SQL Database przy użyciu interfejsu API REST | Microsoft Docs"
description: "Informacje o konfigurowaniu zapory dla adresów IP uzyskujących dostęp do baz danych Azure SQL Database."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: fc874f3c-c623-4924-8cb7-32a8c31e666c
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: a9b48f149427e5ceb69bcaa97b1bf08519499b6f
ms.openlocfilehash: cc0faa49daaafe19c71d2c765b8e865be04f81e2


---
# <a name="configure-azure-sql-database-server-level-firewall-rules-using-the-rest-api"></a>Konfigurowanie reguł zapory na poziomie serwera usługi Azure SQL Database przy użyciu interfejsu API REST
> [!div class="op_single_selector"]
> * [Omówienie](sql-database-firewall-configure.md)
> * [Azure Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [Interfejs API REST](sql-database-configure-firewall-settings-rest.md)
> 
> 

Usługa Microsoft Azure SQL Database zezwala na połączenia z serwerami i bazami danych przy użyciu reguł zapory. W celu selektywnego udzielania dostępu do bazy danych można zdefiniować ustawienia zapory na poziomie serwera i na poziomie bazy danych dla bazy danych master lub bazy danych użytkownika na serwerze usługi Azure SQL Database.

> [!IMPORTANT]
> Aby umożliwić aplikacjom z platformy Azure łączenie się z serwerem bazy danych, należy włączyć połączenia platformy Azure. Aby uzyskać więcej informacji o regułach zapory i umożliwianiu połączeń z platformy Azure, zobacz artykuł [Azure SQL Database Firewall](sql-database-firewall-configure.md) (Zapora usługi Azure SQL Database). W przypadku nawiązywania połączeń wewnątrz granic chmury Azure może być konieczne otwarcie dodatkowych portów TCP. Aby uzyskać więcej informacji, zobacz sekcję **V12 of SQL Database: Outside vs inside** („SQL Database 12: na zewnątrz i wewnątrz”) w artykule [Ports beyond 1433 for ADO.NET 4.5 and SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md) (Porty inne niż 1433 dla platformy ADO.NET 4.5 i usługi SQL Database 12)
> 
> 

## <a name="manage-server-level-firewall-rules-through-rest-api"></a>Zarządzanie regułami zapory na poziomie serwera za pośrednictwem interfejsu API REST
1. Zarządzanie regułami zapory za pośrednictwem interfejsu API REST wymaga uwierzytelnienia. Aby uzyskać więcej informacji, zapoznaj się z [Przewodnikiem dewelopera dotyczącym uwierzytelniania za pomocą interfejsu API usługi Azure Resource Manager](../azure-resource-manager/resource-manager-api-authentication.md).
2. Przy użyciu interfejsu API REST można tworzyć, aktualizować i usuwać reguły na poziomie serwera
   
    Aby utworzyć lub zaktualizować regułę zapory na poziomie serwera, wykonaj metodę PUT przy użyciu następującego kodu:
   
        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}
   
    Treść żądania
   
        {
         "properties": { 
            "startIpAddress": "{start-ip-address}", 
            "endIpAddress": "{end-ip-address}
            }
        } 

    Aby usunąć istniejącą regułę zapory na poziomie serwera, wykonaj metodę DELETE przy użyciu następującego kodu:

        https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/firewallRules/{rule-name}?api-version={api-version}


## <a name="manage-firewall-rules-using-the-rest-api"></a>Zarządzanie regułami zapory za pomocą interfejsu API REST
* [Create or Update Firewall Rule](https://msdn.microsoft.com/library/azure/mt445501.aspx) (Tworzenie i aktualizowanie reguły zapory)
* [Delete Firewall Rule](https://msdn.microsoft.com/library/azure/mt445502.aspx) (Usuwanie reguły zapory)
* [Get Firewall Rule](https://msdn.microsoft.com/library/azure/mt445503.aspx) (Uzyskiwanie reguły zapory)
* [List All Firewall Rules](https://msdn.microsoft.com/library/azure/mt604478.aspx) (Wyświetlanie listy wszystkich reguł zapory)

## <a name="next-steps"></a>Następne kroki
Aby zapoznać się z artykułem zawierającym instrukcje tworzenia reguł zapory na poziomie serwera i na poziomie bazy danych przy użyciu języka Transact-SQL, zobacz temat [Konfigurowanie reguł zapory na poziomie serwera i na poziomie bazy danych usługi Azure SQL Database z użyciem języka T-SQL](sql-database-configure-firewall-settings-tsql.md). 

Aby przejrzeć artykuły zawierające wskazówki na temat tworzenia reguł zapory na poziomie serwera przy użyciu innych metod, zobacz: 

* [Configure Azure SQL Database server-level firewall rules using the Azure portal](sql-database-configure-firewall-settings.md) (Konfigurowanie reguł zapory na poziomie serwera usługi Azure SQL Database za pomocą witryny Azure Portal)
* [Configure Azure SQL Database server-level firewall rules using PowerShell](sql-database-configure-firewall-settings-powershell.md) (Konfigurowanie reguł zapory na poziomie serwera usługi Azure SQL Database przy użyciu programu PowerShell)

Aby uzyskać samouczek tworzenia bazy danych, zobacz [Create a SQL database in minutes using the Azure portal](sql-database-get-started.md) (Tworzenie bazy danych SQL w ciągu kilku minut za pomocą witryny Azure Portal).
Aby uzyskać pomoc podczas łączenia się z bazą danych Azure SQL z aplikacji innych firm lub aplikacji typu open source, zobacz [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx) (Przykłady kodu umożliwiające szybki start dla klienta usługi SQL Database).
Aby dowiedzieć się, jak przechodzić do baz danych, zobacz artykuł [Manage database access and login security](https://msdn.microsoft.com/library/azure/ee336235.aspx) (Zarządzanie zabezpieczeniami dostępu i logowania do bazy danych).

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Zabezpieczanie bazy danych](sql-database-security-overview.md)
* [Usługa Security Center aparatu bazy danych programu SQL Server i bazy danych Azure SQL Database](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->





<!--HONumber=Jan17_HO1-->


