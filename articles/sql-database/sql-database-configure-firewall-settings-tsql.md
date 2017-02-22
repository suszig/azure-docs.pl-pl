---
title: "T-SQL: reguły zapory usługi Azure SQL Database | Microsoft Docs"
description: "Informacje o konfigurowaniu reguł zapory na poziomie serwera i na poziomie bazy danych dla adresów IP uzyskujących dostęp do baz danych Azure SQL Database przy użyciu języka Transact-SQL."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
ms.assetid: 71e692a1-5e2f-4a18-a6d6-527b849cf68e
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/30/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: 2f24c1c43300a3c5035fbb4e90c3d13f73c1bc74
ms.openlocfilehash: ef51f63984d342422ee5f2a93acfee6357df57ca


---
# <a name="configure-azure-sql-database-server-level-and-database-level-firewall-rules-using-t-sql"></a>Konfigurowanie reguł zapory na poziomie serwera i na poziomie bazy danych usługi Azure SQL Database z użyciem języka T-SQL
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

## <a name="server-level-firewall-rules"></a>Reguły zapory na poziomie serwera
Tylko osoba z identyfikatorem logowania podmiotu zabezpieczeń na poziomie serwera lub administrator usługi Azure Active Directory może utworzyć regułę zapory na poziomie serwera przy użyciu języka Transact-SQL.

1. Uruchom okno zapytania i połącz się z wirtualną bazą danych master przy użyciu programu SQL Server Management Studio.
2. W oknie zapytania można wybierać, tworzyć, aktualizować i usuwać reguły zapory na poziomie serwera.
3. Aby utworzyć lub zaktualizować reguły zapory na poziomie serwera, wykonaj procedurę składowaną `sp_set_firewall_rule`. W poniższym przykładzie dopuszczony zostaje zakres adresów IP na serwerze Contoso.<br/>Na początek sprawdź, jakie reguły już istnieją.
   
        SELECT * FROM sys.firewall_rules ORDER BY name;
   
    Następnie dodaj regułę zapory.
   
        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
   
    Aby usunąć regułę zapory na poziomie serwera, wykonaj procedurę składowaną sp_delete_firewall_rule. W poniższym przykładzie zostaje usunięta reguła o nazwie ContosoFirewallRule.
   
        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
   
   Aby uzyskać więcej informacji o tych procedurach składowanych, zobacz artykuły dotyczące procedur [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) i [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx).

## <a name="database-level-firewall-rules"></a>Reguły zapory na poziomie bazy danych
Tylko użytkownik bazy danych z uprawnieniem **CONTROL** w bazie danych (na przykład właściciel bazy danych) może utworzyć regułę zapory na poziomie bazy danych.

1. Po utworzeniu reguły zapory na poziomie serwera dla swojego adresu IP uruchom okno zapytania za pomocą klasycznego portalu lub programu SQL Server Management Studio.
2. Połącz się z bazą danych, dla której chcesz utworzyć regułę zapory na poziomie bazy danych.
   
    Aby utworzyć nową regułę zapory na poziomie bazy danych lub zaktualizować istniejącą, wykonaj procedurę składowaną `sp_set_database_firewall_rule`. W poniższym przykładzie zostaje utworzona nowa reguła zapory o nazwie ContosoFirewallRule.
   
        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', 
            @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'
   
    Aby usunąć istniejącą regułę zapory na poziomie bazy danych, wykonaj procedurę składowaną `sp_delete_database_firewall_rule`. W poniższym przykładzie zostaje usunięta reguła o nazwie ContosoFirewallRule.
   `
   
        EXEC sp_delete_database_firewall_rule @name = N'ContosoFirewallRule'

Aby uzyskać więcej informacji o tych procedurach składowanych, zobacz artykuły dotyczące procedur [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) i [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx).

> [!NOTE]
> Aby zapoznać się z samouczkiem prezentującym sposób korzystania z zapór na poziomie bazy danych, zobacz [Samouczek usługi SQL Database: uwierzytelnianie programu SQL Server, dane logowania i konta użytkowników, role bazy danych, uprawnienia, reguły zapory na poziomie serwera oraz reguły zapory na poziomie bazy danych](sql-database-control-access-sql-authentication-get-started.md).
>


## <a name="next-steps"></a>Następne kroki
Aby przejrzeć artykuły zawierające wskazówki na temat tworzenia reguł zapory na poziomie serwera przy użyciu innych metod, zobacz: 

* [Configure Azure SQL Database server-level firewall rules using the Azure portal](sql-database-configure-firewall-settings.md) (Konfigurowanie reguł zapory na poziomie serwera usługi Azure SQL Database za pomocą witryny Azure Portal)
* [Configure Azure SQL Database server-level firewall rules using PowerShell](sql-database-configure-firewall-settings-powershell.md) (Konfigurowanie reguł zapory na poziomie serwera usługi Azure SQL Database przy użyciu programu PowerShell)
* [Configure Azure SQL Database server-level firewall rules using the REST API](sql-database-configure-firewall-settings-rest.md) (Konfigurowanie reguł zapory na poziomie serwera usługi Azure SQL Database przy użyciu interfejsu API REST)

Aby uzyskać samouczek tworzenia bazy danych, zobacz [Create a SQL database in minutes using the Azure portal](sql-database-get-started.md) (Tworzenie bazy danych SQL w ciągu kilku minut za pomocą witryny Azure Portal).
Aby uzyskać pomoc podczas łączenia się z bazą danych Azure SQL z aplikacji innych firm lub aplikacji typu open source, zobacz [Client quick-start code samples to SQL Database](https://msdn.microsoft.com/library/azure/ee336282.aspx) (Przykłady kodu umożliwiające szybki start dla klienta usługi SQL Database).
Aby dowiedzieć się, jak przechodzić do baz danych, zobacz artykuł [Manage database access and login security](https://msdn.microsoft.com/library/azure/ee336235.aspx) (Zarządzanie zabezpieczeniami dostępu i logowania do bazy danych).

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Zabezpieczanie bazy danych](sql-database-security-overview.md)
* [Usługa Security Center aparatu bazy danych programu SQL Server i bazy danych Azure SQL Database](https://msdn.microsoft.com/library/bb510589)




<!--HONumber=Jan17_HO3-->


