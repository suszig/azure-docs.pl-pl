---
title: "Tworzenie i zarządzanie serwerami Azure SQL & bazy danych | Dokumentacja firmy Microsoft"
description: "Informacje o serwerze bazy danych SQL Azure i pojęcia dotyczące bazy danych i dotyczących tworzenia i zarządzania serwerami i bazami danych."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 10/11/2017
ms.author: carlrab
ms.openlocfilehash: 3bd39b9a63cd4a18eabee3d1737daac56b3e37df
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="create-and-manage-azure-sql-database-servers-and-databases"></a>Utwórz i Zarządzaj serwerami bazy danych SQL Azure i baz danych

Baza danych Azure SQL jest zarządzany bazy danych w systemie Microsoft Azure, która jest tworzona w [grupy zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) ze zdefiniowanym zestawem [zasobów obliczeniowych i magazynu dla różnych obciążeń](sql-database-service-tiers.md). Baza danych Azure SQL jest skojarzony z serwera logicznego bazy danych SQL Azure, który jest tworzony w określonym regionie Azure. 

## <a name="an-azure-sql-database-can-be-a-single-pooled-or-partitioned-database"></a>Baza danych Azure SQL mogą być jednej puli i podzielonym na partycje bazy danych

Baza danych Azure SQL mogą być:

- A [pojedynczej bazy danych](sql-database-single-database-resources.md) z zestawem zasobów
- Część [puli elastycznej](sql-database-elastic-pool.md) który udostępnia zestaw zasobów
- częścią [skalowanego zestawu baz danych podzielonych na fragmenty](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling), które mogą być pojedynczymi bazami danych lub pulami baz danych.
- częścią zestawu baz danych uczestniczącego we [wzorcu projektowym wielodostępnych aplikacji SaaS](sql-database-design-patterns-multi-tenancy-saas-applications.md), którego bazy danych mogą być pojedynczymi bazami danych i/lub pulami baz danych. 

> [!TIP]
> Prawidłowe nazwy baz danych opisano w artykule [Database Identifiers](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers) (Identyfikatory baz danych). 
>
 
- Domyślnym sortowaniem baz danych używanym przez usługę Microsoft Azure SQL Database jest sortowanie **SQL_LATIN1_GENERAL_CP1_CI_AS**, gdzie **LATIN1_GENERAL** oznacza język angielski (Stany Zjednoczone), **CP1** oznacza stronę kodową 1252, **CI** oznacza uwzględnianie wielkości liter, a **AS** oznacza uwzględnianie akcentów. Aby uzyskać więcej informacji na temat sposobu ustawiania sortowania, zobacz instrukcję [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).
- Baza danych SQL Azure Microsoft obsługuje danych tabelarycznych (TDS) protokół klienta wersja strumienia 7.3 lub nowszej.
- Dozwolone są tylko połączenia protokołu TCP/IP.

## <a name="what-is-an-azure-sql-logical-server"></a>Co to jest serwer logiczny Azure SQL?

Serwer logiczny działa jako centralny punkt administracyjny dla wielu baz danych, w tym [pule elastyczne](sql-database-elastic-pool.md) [logowania](sql-database-manage-logins.md), [reguły zapory](sql-database-firewall-configure.md), [inspekcji reguły](sql-database-auditing.md), [zasady wykrywania zagrożeń](sql-database-threat-detection.md), i [trybu failover grupy](sql-database-geo-replication-overview.md). W regionie innym niż jego grupa zasobów może być serwerem logicznym. Serwer logiczny musi istnieć przed przystąpieniem do tworzenia bazy danych Azure SQL. Wszystkie bazy danych na serwerze są tworzone w tym samym regionie co serwer logiczny. 


> [!IMPORTANT]
> W usłudze SQL Database serwer jest konstrukcją logiczną, inną niż wystąpienie programu SQL Server, z którym można zapoznać się w środowisku lokalnym. W szczególności usługa SQL Database nie udziela żadnych gwarancji dotyczących lokalizacji baz danych w odniesieniu do ich serwerów logicznych i nie uwidacznia dostępu ani żadnych funkcji na poziomie wystąpienia.
> 

Tworzenie serwera logicznego, należy podać serwer, konto logowania i hasło, które ma prawa administracyjne do głównej bazy danych na tym serwerze i wszystkich baz danych utworzonych na tym serwerze. To konto początkowej jest konto logowania SQL. Baza danych SQL Azure obsługuje uwierzytelnianie SQL i uwierzytelniania usługi Azure Active Directory do uwierzytelniania. Aby uzyskać informacje dotyczące logowania i uwierzytelniania, zobacz [Zarządzanie bazami danych i Logowaniami w bazie danych SQL Azure](sql-database-manage-logins.md). Uwierzytelnianie systemu Windows nie jest obsługiwane. 

> [!TIP]
> Aby uzyskać prawidłowy zasób grupy i nazw serwerów, zobacz [nazewnictwa reguły i ograniczenia](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).
>

Serwer logiczny bazy danych Azure:

- Jest tworzony w ramach subskrypcji platformy Azure, ale można go przenieść wraz z zawartymi w nim zasobami do innej subskrypcji
- Jest zasobem nadrzędnym dla baz danych, pul elastycznych i magazynów danych
- Obejmuje przestrzeń nazw dla bazy danych, pule elastyczne i magazyny danych
- Jest kontenerem logicznym z semantyki silne istnienia — usuwanie serwera i usuwa zawartych baz danych, pule elastyczne i magazyny danych
- Uczestniczy w [kontroli dostępu opartej na rolach na platformie Azure (RBAC)](/active-directory/role-based-access-control-what-is) -baz danych, pule elastyczne i hurtowni danych w ramach serwera dziedziczą prawa dostępu z serwera
- Jest elementem znaczących tożsamości baz danych, pule elastyczne i magazynów danych dla zasobów platformy Azure (Zobacz Schemat adresu URL dla baz danych i pul) do celów zarządzania
- Rozmieszcza zasoby w regionie
- Udostępnia punkt końcowy połączenia dla dostępu do baz danych (<serverName>.database.windows.net)
- Zapewnia dostęp do metadanych dotyczących zawartych zasobów przy użyciu dynamicznych widoków zarządzania, łącząc się z główną bazą danych 
- Określa zakres dla zasad zarządzania, które dotyczą bazy danych — logowania, zapory, inspekcji, zagrożenia wykrywania itp. 
- Jest ograniczony przez przydział w ramach subskrypcji nadrzędnej (sześciu serwerów na subskrypcję domyślnie - [Zobacz subskrypcji w tym miejscu ogranicza](../azure-subscription-service-limits.md))
- Udostępnia zakres limit przydziału bazy danych i limit przydziału jednostek DTU dla zasoby, które zawiera (na przykład DTU 45 000)
- Zakres przechowywania wersji dla możliwościami włączonymi na zasobach ograniczonego 
- Logowania główne na poziomie serwera mogą zarządzać wszystkimi bazami danych na serwerze
- Może zawierać logowania podobne do tych w lokalnych wystąpieniach programu SQL Server, którym udzielono dostępu do co najmniej jednej bazy danych na serwerze, i może otrzymać ograniczone prawa administracyjne. Aby uzyskać więcej informacji, zobacz temat [Logowania](sql-database-manage-logins.md).

## <a name="azure-sql-databases-protected-by-sql-database-firewall"></a>Chronione przez zaporę bazy danych SQL Azure bazy danych SQL

Aby ułatwić ochronę danych, [zapory bazy danych SQL](sql-database-firewall-configure.md) uniemożliwia dostęp do serwera bazy danych lub dowolny z jej baz danych z poza połączenie z serwerem bezpośrednio za pośrednictwem połączenia subskrypcji platformy Azure. Aby włączyć dodatkowe łączności, należy najpierw [utworzyć co najmniej jedną regułę zapory](sql-database-firewall-configure.md#creating-and-managing-firewall-rules). Do tworzenia i zarządzania pule elastyczne, zobacz [pule elastyczne](sql-database-elastic-pool.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-portal"></a>Zarządzaj serwerami oraz baz danych Azure SQL a zapory przy użyciu portalu Azure

Można utworzyć grupy zasobów bazy danych Azure SQL wcześniejsze lub podczas tworzenia serwera. Istnieje wiele metod pobierania do nowego formularza serwera SQL, tworząc nowy serwer SQL lub podczas tworzenia nowej bazy danych. 

### <a name="create-a-blank-sql-server-logical-server"></a>Utwórz puste SQL server (serwer logiczny)

Do utworzenia serwera (bez bazy danych) bazy danych SQL Azure przy użyciu [portalu Azure](https://portal.azure.com), przejdź do pustego formularza programu SQL server (serwer logiczny).  

### <a name="create-a-blank-or-sample-sql-database"></a>Utwórz puste lub przykładowa baza danych SQL

Do utworzenia bazy danych Azure SQL przy użyciu [portalu Azure](https://portal.azure.com), przejdź do pusty formularz bazy danych SQL i podaj żądane informacje. Można utworzyć grupy zasobów i serwer logiczny wcześniejsze lub podczas tworzenia bazy danych bazy danych Azure SQL. Można utworzyć pustą bazę danych lub utworzyć oparty na Adventure Works LT. przykładowej bazy danych 

  ![tworzenie bazy danych 1](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> Aby uzyskać informacje dotyczące wybierania warstwy cenowej bazy danych, zobacz [warstw usług](sql-database-service-tiers.md).
>

### <a name="manage-an-existing-sql-server"></a>Zarządzanie istniejącego serwera SQL

Aby zarządzać istniejącego serwera, przejdź do serwera przy użyciu metod — przykład wyświetlanie określonej strony bazy danych SQL, **serwerów SQL** strony, lub **wszystkie zasoby** strony. 

Do zarządzania istniejącą bazę danych, przejdź do **baz danych SQL** i kliknij przycisk bazy danych chcesz zarządzać. Poniższy zrzut ekranu pokazuje, jak rozpocząć, ustawienia zapory poziomu serwera bazy danych z **omówienie** stron dla bazy danych. 

   ![reguła zapory serwera](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> Aby skonfigurować właściwości wydajności bazy danych, zobacz [warstw usług](sql-database-service-tiers.md).
>

> [!TIP]
> Samouczek szybki start dla usługi Azure portalu, zobacz [tworzenie bazy danych Azure SQL w portalu Azure](sql-database-get-started-portal.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-powershell"></a>Zarządzaj serwerami oraz baz danych Azure SQL a zapory przy użyciu programu PowerShell

Tworzenie i zarządzanie nimi Azure SQL server, baz danych i zapory przy użyciu programu Azure PowerShell, użyj następujących poleceń cmdlet programu PowerShell. Jeśli musisz zainstalować lub uaktualnić programu PowerShell, zobacz [modułu instalacji programu Azure PowerShell](/powershell/azure/install-azurerm-ps). Do tworzenia i zarządzania pule elastyczne, zobacz [pule elastyczne](sql-database-elastic-pool.md).

| Polecenie cmdlet | Opis |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Tworzy bazę danych |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Pobiera jeden lub więcej baz danych|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Ustawia właściwości dla bazy danych lub przenosi istniejącą bazę danych do puli elastycznej|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Usuwa z bazy danych|
|[Nowe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Tworzy grupę zasobów]
|[Nowe AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Tworzy serwer|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Zwraca informacje na temat serwerów|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.sql/set-azurermsqlserver)|Modyfikuje właściwości serwera|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Usuwa serwer|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Tworzy regułę zapory poziomu serwera |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Pobiera reguły zapory serwera|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Modyfikuje regułę zapory na serwerze|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Usuwa regułę zapory z serwera.|
| Nowe AzureRmSqlServerVirtualNetworkRule | Tworzy [ *reguły sieci wirtualnej*](sql-database-vnet-service-endpoint-rule-overview.md), na podstawie podsieci, która jest punkt końcowy usługi sieci wirtualnej. |

> [!TIP]
> Samouczek szybki start programu PowerShell, zobacz [tworzenia pojedynczej bazy danych Azure SQL przy użyciu programu PowerShell](sql-database-get-started-portal.md). Dla programu PowerShell przykładowe skrypty, zobacz [Użyj programu PowerShell do tworzenia pojedynczej bazy danych Azure SQL i skonfigurować regułę zapory](scripts/sql-database-create-and-configure-database-powershell.md) i [monitora i skali pojedynczego SQL bazy danych przy użyciu programu PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-cli"></a>Zarządzaj serwerami oraz baz danych Azure SQL a zapory przy użyciu wiersza polecenia platformy Azure

Tworzenie i zarządzanie nimi Azure SQL server, baz danych i zapory z [interfejsu wiersza polecenia Azure](/cli/azure/overview), należy użyć następującego [bazy danych SQL interfejsu wiersza polecenia Azure](/cli/azure/sql/db) poleceń. Używaj usługi [Cloud Shell](/azure/cloud-shell/overview), aby uruchamiać interfejs wiersza polecenia w przeglądarce, albo [zainstaluj](/cli/azure/install-azure-cli) go w systemie macOS, Linux lub Windows. Do tworzenia i zarządzania pule elastyczne, zobacz [pule elastyczne](sql-database-elastic-pool.md).

| Polecenie cmdlet | Opis |
| --- | --- |
|[Tworzenie bazy danych sql az](/cli/azure/sql/db#az_sql_db_create) |Tworzy bazę danych|
|[Lista bazy danych sql az](/cli/azure/sql/db#az_sql_db_list)|Wyświetla listę wszystkich baz danych i magazynów danych na serwerze lub wszystkie bazy danych w puli elastycznej|
|[Lista wersje az bazy danych sql](/cli/azure/sql/db#az_sql_db_list_editions)|Wyświetla dostępne usługi cele i limity magazynu|
|[bazy danych sql az listy użycia](/cli/azure/sql/db#az_sql_db_list_usages)|Zwraca bazy danych użycia|
|[Pokaż bazy danych sql az](/cli/azure/sql/db#az_sql_db_show)|Pobiera Magazyn bazy danych lub danych|
|[Aktualizacja bazy danych sql az](/cli/azure/sql/db#az_sql_db_update)|Aktualizuje bazę danych|
|[Usuwanie bazy danych sql az](/cli/azure/sql/db#az_sql_db_delete)|Usuwa z bazy danych|
|[Tworzenie grupy az](/cli/azure/group#az_group_create)|Tworzy grupę zasobów|
|[Utwórz az programu sql server](/cli/azure/sql/server#az_sql_server_create)|Tworzy serwer|
|[Lista serwerów sql az](/cli/azure/sql/server#az_sql_server_list)|Wyświetla serwery|
|[Serwer sql az listy użycia](/cli/azure/sql/server#az_sql_server_list-usages)|Zwraca użycia serwera|
|[Pokaż serwera sql az](/cli/azure/sql/server#az_sql_server_show)|Pobiera serwera|
|[Aktualizacja programu sql server az](/cli/azure/sql/server#az_sql_server_update)|Serwer aktualizacji|
|[Usuń serwer sql az](/cli/azure/sql/server#az_sql_server_delete)|Usuwa serwer|
|[Utwórz az programu sql server — reguły zapory](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Powoduje utworzenie reguły zapory serwera|
|[Lista reguł zapory serwera sql az](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Wyświetla listę reguł zapory na serwerze|
|[Pokaż reguły zapory serwera sql az](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Wyświetla szczegóły reguły zapory|
|[Aktualizacja reguły zapory programu sql server az](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Aktualizuje regułę zapory|
|[Usuwanie reguły zapory serwera sql az](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Usuwa regułę zapory|

> [!TIP]
> Samouczek szybki start wiersza polecenia platformy Azure, zobacz [tworzenia pojedynczej bazy danych Azure SQL przy użyciu interfejsu wiersza polecenia Azure](sql-database-get-started-cli.md). Dla interfejsu wiersza polecenia Azure przykładowe skrypty, zobacz [Użyj interfejsu wiersza polecenia do tworzenia pojedynczej bazy danych Azure SQL i skonfigurować regułę zapory](scripts/sql-database-create-and-configure-database-cli.md) i [Użyj interfejsu wiersza polecenia, aby monitorować i skalowania pojedynczej bazy danych SQL](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-transact-sql"></a>Zarządzaj serwerami oraz baz danych Azure SQL a zapory przy użyciu języka Transact-SQL

Tworzenie i zarządzanie nimi Azure SQL server, baz danych i zapór z Transact-SQL, użyj następujących poleceń T-SQL. Te polecenia przy użyciu portalu Azure może wystawiać [programu SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), lub inny program, który może nawiązywać połączenia z serwerem bazy danych SQL Azure i przekazać języka Transact-SQL polecenia. Do zarządzania pule elastyczne, zobacz [pule elastyczne](sql-database-elastic-pool.md).

> [!IMPORTANT]
> Nie można utworzyć ani usunąć z serwerem przy użyciu języka Transact-SQL.
>

| Polecenie | Opis |
| --- | --- |
|[Utwórz bazę danych (baza danych Azure SQL)](/sql/t-sql/statements/create-database-azure-sql-database)|Tworzy nową bazę danych. Musisz mieć połączenie z główną bazą danych, aby utworzyć nową bazę danych.|
| [ALTER DATABASE (baza danych Azure SQL)](/sql/t-sql/statements/alter-database-azure-sql-database) |Modyfikuje bazy danych Azure SQL. |
|[ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Modyfikuje magazyn danych Azure SQL.|
|[Usuwanie bazy danych (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Usuwa z bazy danych.|
|[sys.database_service_objectives (baza danych SQL Azure)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Zwraca edition (warstwy usług), celem usługi (warstwa cenowa) i nazwę puli elastycznej dla bazy danych Azure SQL lub usługi Azure SQL Data Warehouse. Jeśli zalogowany do głównej bazy danych na serwerze bazy danych SQL Azure, zwraca informacje o wszystkich baz danych. Dla usługi Azure SQL Data Warehouse musi być podłączony do bazy danych master.|
|[sys.dm_db_resource_stats (baza danych SQL Azure)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Zwraca zużycie Procesora, operacji We/Wy i pamięci dla bazy danych z bazy danych SQL Azure. Dla co 15 s istnieje jeden wiersz, nawet jeśli nic się nie w bazie danych.|
|[sys.resource_stats (baza danych SQL Azure)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|Zwraca procesora CPU, użycia i magazynu danych dla bazy danych SQL Azure. Dane są zbierane i agregowane w ciągu 5 minut.|
|[sys.database_connection_stats (baza danych SQL Azure)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|Zawiera dane statystyczne dla zdarzenia łączności bazy danych SQL Database przedstawiające przegląd bazy danych połączenia sukcesy i niepowodzenia. |
|[sys.event_log (baza danych SQL Azure)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Zwraca pomyślnego połączenia z bazą danych usługi Azure SQL Database, błędy połączeń i zakleszczenia. Te informacje można użyć do śledzenia i rozwiązywanie problemów z działania bazy danych z bazy danych SQL.|
|[procedurę składowaną sp_set_firewall_rule (baza danych SQL Azure)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Tworzy lub aktualizuje ustawienia zapory poziomu serwera dla serwera bazy danych SQL. Ta procedura składowana jest dostępna tylko w bazie danych master do głównego identyfikatora logowania poziomu serwera. Reguły zapory poziomu serwera można tworzyć tylko za pomocą języka Transact-SQL, po utworzeniu pierwszej reguły zapory poziomu serwera przez użytkownika z uprawnienia na poziomie Azure|
|[sys.firewall_rules (baza danych SQL Azure)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Zwraca informacje na temat ustawień zapory na poziomie serwera, skojarzonych z bazy danych SQL Microsoft Azure.|
|[sp_delete_firewall_rule (baza danych SQL Azure)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Usuwa ustawienia zapory poziomu serwera z serwerem bazy danych SQL. Ta procedura składowana jest dostępna tylko w bazie danych master do głównego identyfikatora logowania poziomu serwera.|
|[sp_set_database_firewall_rule (baza danych SQL Azure)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Tworzy lub aktualizuje reguły zapory poziomu bazy danych dla bazy danych SQL Azure lub usługi SQL Data Warehouse. Można skonfigurować reguł zapory bazy danych dla bazy danych master i baz danych użytkowników w bazie danych SQL. Reguły zapory bazy danych są przydatne, gdy przy użyciu zawarte bazy danych użytkowników. |
|[sys.database_firewall_rules (baza danych SQL Azure)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Zwraca informacje na temat ustawień zapory na poziomie bazy danych, skojarzonych z bazy danych SQL Microsoft Azure. |
|[sp_delete_database_firewall_rule (baza danych SQL Azure)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Ustawienia zapory na poziomie bazy danych usuwa z bazy danych SQL Azure lub SQL Data Warehouse. |


> [!TIP]
> Samouczek szybki start Microsoft Windows za pomocą programu SQL Server Management Studio, zobacz [bazy danych SQL Azure: Użyj SQL Server Management Studio do nawiązywania połączeń i zapytania na danych](sql-database-connect-query-ssms.md). Samouczek szybki start za pomocą programu Visual Studio Code na macOS, Linux lub Windows, temacie [bazy danych SQL Azure: Użyj Visual Studio Code do nawiązywania połączeń i zapytania na danych](sql-database-connect-query-vscode.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-rest-api"></a>Zarządzaj serwerami oraz baz danych Azure SQL a zapory przy użyciu interfejsu API REST

Tworzenie i zarządzanie nimi Azure SQL server, baz danych i zapór używają te żądania interfejsu API REST.

| Polecenie | Opis |
| --- | --- |
|[Serwery — Tworzenie lub aktualizowanie](/rest/api/sql/servers/createorupdate)|Tworzy lub aktualizuje nowego serwera.|
|[Serwery — Delete](/rest/api/sql/servers/delete)|Usuwa serwer SQL.|
|[Serwery — Get](/rest/api/sql/servers/get)|Pobiera serwera.|
|[Serwery — listy](/rest/api/sql/servers/list)|Zwraca listę serwerów.|
|[Serwery — Lista według grupy zasobów](/rest/api/sql/servers/listbyresourcegroup)|Zwraca listę serwerów w grupie zasobów.|
|[Serwery — aktualizacja](/rest/api/sql/servers/update)|Aktualizuje istniejący serwer.|
|[Serwery - Sql](/rest/api/sql/servers%20-%20sql)|Określa, czy można utworzyć zasobu o określonej nazwie.|
|[Bazy danych — Tworzenie lub aktualizowanie](/rest/api/sql/databases/createorupdate)|Tworzy nową bazę danych lub aktualizuje istniejącą bazę danych.|
|[Bazy danych - Get](/rest/api/sql/databases/get)|Pobiera bazy danych.|
|[Bazy danych — uzyskać za pomocą puli elastycznej](/rest/api/sql/databases/getbyelasticpool)|Pobiera bazy danych w puli elastycznej.|
|[Bazy danych — uzyskać za pomocą zalecana liczba elastycznych pul](/rest/api/sql/databases/getbyrecommendedelasticpool)|Pobiera bazy danych wewnątrz recommented puli elastycznej.|
|[Bazy danych — lista według puli elastycznej](/rest/api/sql/databases/listbyelasticpool)|Zwraca listę baz danych w puli elastycznej.|
|[Bazy danych — lista według zalecana liczba elastycznych pul](/rest/api/sql/databases/listbyrecommendedelasticpool)|Zwraca listę baz danych wewnątrz recommented puli elastycznej.|
|[Bazy danych — lista przez serwer](/rest/api/sql/databases/listbyserver)|Zwraca listę baz danych na serwerze.|
|[Bazy danych — aktualizacja](/rest/api/sql/databases/update)|Aktualizuje istniejącą bazę danych.|
|[Zapory reguł — Tworzenie lub aktualizowanie](/rest/api/sql/firewallrules/createorupdate)|Tworzy lub aktualizuje regułę zapory.|
|[Reguły zapory - Delete](/rest/api/sql/firewallrules/delete)|Usuwa regułę zapory.|
|[Reguły zapory - Get](/rest/api/sql/firewallrules/get)|Pobiera reguły zapory.|
|[Reguły zapory - listy przez serwer](/rest/api/sql/firewallrules/listbyserver)|Zwraca listę reguł zapory.|

## <a name="next-steps"></a>Następne kroki

- Aby zapoznać się z puli za pomocą elastycznych pul baz danych, zobacz [pule elastyczne](sql-database-elastic-pool.md).
- Informacje o usłudze Azure SQL Database, zobacz [co to jest SQL Database?](sql-database-technical-overview.md).
- Informacje na temat migracji bazy danych programu SQL Server na platformie Azure, zobacz [migracji do usługi Azure SQL Database](sql-database-cloud-migrate.md).
- Informacje dotyczące obsługiwanych funkcji można znaleźć w temacie [Funkcje](sql-database-features.md).
