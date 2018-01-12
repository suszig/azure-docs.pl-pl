---
title: "Przykład PowerShell — synchronizacja danych SQL aktualizacja schematu synchronizacji | Dokumentacja firmy Microsoft"
description: "Azure przykładowy skrypt programu PowerShell do aktualizacji schematu synchronizacji dla synchronizacji danych SQL"
services: sql-database
documentationcenter: sql-database
author: jognanay
manager: craigg
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 01/10/2018
ms.author: jognanay
ms.reviewer: douglasl
ms.openlocfilehash: 66bf084f585b86979e6521321daf466c571de10c
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2018
---
# <a name="use-powershell-to-update-the-sync-schema-in-an-existing-sync-group"></a>Aktualizacja schematu synchronizacji w istniejącej grupie synchronizacji przy użyciu programu PowerShell

W tym przykładzie programu PowerShell aktualizacji schematu synchronizacji w istniejącej grupie synchronizacji. Ten skrypt jest synchronizowanie wielu tabel, pomaga wydajnie aktualizacji schematu synchronizacji.

W tym przykładzie przedstawiono użycie **UpdateSyncSchema** skryptu, który jest dostępny w witrynie GitHub jako [UpdateSyncSchema.ps1](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-data-sync/UpdateSyncSchema.ps1).

Omówienie synchronizacji danych SQL, zobacz [synchronizacji danych między wieloma bazami danych chmury i lokalnych z synchronizacji danych SQL Azure (wersja zapoznawcza)](../sql-database-sync-data.md).
## <a name="prerequisites"></a>Wymagania wstępne

W tym przykładzie wymaga programu Azure PowerShell modułu w wersji 4.2 lub nowszej. Uruchom `Get-Module -ListAvailable AzureRM` można odnaleźć zainstalowanej wersji. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).
 
Uruchom `Login-AzureRmAccount` można utworzyć połączenia z platformą Azure.

## <a name="examples"></a>Przykłady

### <a name="example-1---add-all-tables-to-the-sync-schema"></a>Przykład 1 - dodać wszystkie tabele do schematu synchronizacji

Poniższy przykład odświeża schemat bazy danych i dodaje wszystkie poprawne tabele w bazie danych Centrum do schematu synchronizacji.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -RefreshDatabaseSchema $true -AddAllTables $true
```

### <a name="example-2---add-and-remove-tables-and-columns"></a>Przykład 2 — Dodawanie i usuwanie tabel i kolumn

W poniższym przykładzie dodano `[dbo].[Table1]` i `[dbo].[Table2].[Column1]` schematu synchronizacji i usuwa `[dbo].[Table3]`.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -TablesAndColumnsToAdd "[dbo].[Table1],[dbo].[Table2].[Column1]" -TablesAndColumnsToRemove "[dbo].[Table3]"
```

## <a name="script-parameters"></a>Parametry skryptu

**UpdateSyncSchema** skrypt ma następujące parametry:

| Parametr | Uwagi |
|---|---|
| $SubscriptionId | Subskrypcja, w którym zostanie utworzona grupa synchronizacji. |
| $ResourceGroupName | Grupy zasobów, w którym zostanie utworzona grupa synchronizacji.|
| $ServerName | Nazwa serwera bazy danych koncentratora.|
| $DatabaseName | Nazwa bazy danych koncentratora. |
| $SyncGroupName | Nazwa grupy synchronizacji. |
| $MemberName | Określ nazwę elementu członkowskiego, jeśli chcesz załadować schemat bazy danych z elementu członkowskiego synchronizacji zamiast z bazy danych Centrum. Jeśli chcesz załadować schemat bazy danych z Centrum tego parametru należy pozostawić pusty. |
| $TimeoutInSeconds | Upłynął limit czasu podczas skrypt odświeża schematu bazy danych. Domyślna to 900 sekund. |
| $RefreshDatabaseSchema | Określ, czy skrypt ma Odśwież schemat bazy danych. Zmiana schematu bazy danych z poprzedniej konfiguracji — na przykład jeśli dodano nową tabelę lub od nowa kolumna), należy odświeżyć schematu przed ponownie go skonfigurować. Domyślna to false. |
| $AddAllTables | Jeśli ta wartość wynosi true, wszystkie poprawne tabele i kolumny zostaną dodane do schematu synchronizacji. Wartości $TablesAndColumnsToAdd i $TablesAndColumnsToRemove są ignorowane. |
| $TablesAndColumnsToAdd | Określ tabel lub kolumn, które mają zostać dodane do schematu synchronizacji. Każda nazwa tabeli lub kolumny musi być w pełni rozdzielane o nazwie schematu. Na przykład: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Wiele nazw tabeli lub kolumny można określić i rozdzielonych przecinkami (,). |
| $TablesAndColumnsToRemove | Określ tabel lub kolumn, aby były usuwane z schematu synchronizacji. Każda nazwa tabeli lub kolumny musi być w pełni rozdzielane o nazwie schematu. Na przykład: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Wiele nazw tabeli lub kolumny można określić i rozdzielonych przecinkami (,). |
|||

## <a name="script-explanation"></a>Wyjaśnienie skryptu

**UpdateSyncSchema** skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Get-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncgroup) | Zwraca informacje o grupie synchronizacji. |
| [AzureRmSqlSyncGroup aktualizacji](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncgroup) | Aktualizuje grupy synchronizacji. |
| [Get-AzureRmSqlSyncMember](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncmember) | Zwraca informacje o składniku synchronizacji. |
| [Get-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncschema) | Zwraca informacje o schemacie synchronizacji. |
| [AzureRmSqlSyncSchema aktualizacji](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncschema) | Aktualizacje schematu synchronizacji. |
|||

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat programu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Dodatkowe przykłady skryptów programu PowerShell bazy danych SQL można znaleźć w [skryptów programu PowerShell bazy danych SQL Azure](../sql-database-powershell-samples.md).

Aby uzyskać więcej informacji na temat synchronizacji danych SQL zobacz:

-   [Synchronizowanie danych w wielu w chmurze i lokalnych baz danych z synchronizacji danych SQL Azure](../sql-database-sync-data.md)
-   [Konfigurowanie synchronizacji danych SQL Azure](../sql-database-get-started-sql-data-sync.md)
-   [Najlepsze rozwiązania dotyczące synchronizacji danych SQL Azure](../sql-database-best-practices-data-sync.md)
-   [Monitor synchronizacji danych Azure SQL z OMS analizy dzienników](../sql-database-sync-monitor-oms.md)
-   [Rozwiązywanie problemów z synchronizacją danych SQL Azure](../sql-database-troubleshoot-data-sync.md)

-   Wykonaj przykłady z programu PowerShell, które przedstawiają sposób konfigurowania synchronizacji danych SQL:
    -   [Synchronizacja między wiele baz danych Azure SQL przy użyciu programu PowerShell](sql-database-sync-data-between-sql-databases.md)
    -   [Synchronizacja między bazą danych SQL Azure i lokalnej bazy danych programu SQL Server przy użyciu programu PowerShell](sql-database-sync-data-between-azure-onprem.md)

-   [Pobrać dokumentację interfejsu API REST synchronizacji danych SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Aby uzyskać więcej informacji na temat bazy danych SQL zobacz:

-   [Omówienie bazy danych SQL](../sql-database-technical-overview.md)
-   [Zarządzanie cyklem życia bazy danych](https://msdn.microsoft.com/library/jj907294.aspx)
