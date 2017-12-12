---
title: "Tryb failover przykład — replikacja geograficzna PowerShell pojedynczej grupy usługi Azure SQL Database | Dokumentacja firmy Microsoft"
description: "Azure przykładowy skrypt programu PowerShell do konfigurowania trybu failover aktywna replikacja geograficzna grupy dla pojedynczej bazy danych Azure SQL i przełączyć go."
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 9619cdf91197a5daeeaa4ee8d088852b11573eff
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="use-powershell-to-configure-an-active-geo-replication-failover-group-for-a-single-azure-sql-database"></a>Aby skonfigurować grupę aktywna replikacja geograficzna trybu failover dla pojedynczej bazy danych Azure SQL za pomocą programu PowerShell

Ten przykładowy skrypt programu PowerShell konfiguruje grupę aktywna replikacja geograficzna trybu failover dla pojedynczej bazy danych Azure SQL i awaryjnie do repliki pomocniczej bazy danych Azure SQL.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a>Przykładowe skrypty

[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-database/setup-geodr-and-failover-database-failover-group.ps1?highlight=19-22 "Set up failover group for single database")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia

Po uruchomieniu przykładowy skrypt następującego polecenia można usunąć grupy zasobów i wszystkie zasoby skojarzone z nim.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $primaryresourcegroupname
Remove-AzureRmResourceGroup -ResourceGroupName $secondaryresourcegroupname
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Nowe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Nowe AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Tworzy serwer logiczny, który jest hostem bazy danych lub elastyczna pula. |
| [Nowe AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | Tworzy elastycznej puli w ramach serwera logicznego. |
| [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) | Aktualizuje właściwości bazy danych lub bazy danych są przenoszone do z i między pule elastyczne. |
| [Nowe AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| Tworzy pomocniczej bazy danych dla istniejącej bazy danych i rozpoczyna się replikacja danych. |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)| Pobiera jeden lub więcej baz danych. |
| [Zestaw AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| Zmienia pomocniczej bazy danych jako głównej zainicjować trybu failover.|
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | Pobiera linki — replikacja geograficzna między bazą danych SQL Azure i grupy zasobów lub SQL Server. |
| [Usuń AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | Kończy replikacji danych między bazą danych SQL i dodatkowej określonej bazy danych. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |
|||

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących programu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Dodatkowe przykłady skryptów programu PowerShell bazy danych SQL można znaleźć w [skryptów programu PowerShell bazy danych SQL Azure](../sql-database-powershell-samples.md).
