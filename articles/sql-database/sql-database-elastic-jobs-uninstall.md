---
title: "Jak odinstalować narzędzia zadania elastycznej bazy danych"
description: "Dowiedz się, jak odinstalować składniki zadania elastycznej bazy danych przy użyciu portalu Azure PowerShell."
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 10/24/2016
ms.author: sstein
ms.openlocfilehash: e6a86a3571301f6fa2a356714721daa141e696bf
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="uninstall-elastic-database-jobs-components"></a>Odinstaluj składniki zadania elastycznej bazy danych
**Zadania elastyczne bazy danych** składniki można odinstalować za pomocą portalu Azure lub programu PowerShell.

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a>Odinstaluj składniki zadania elastycznej bazy danych przy użyciu portalu Azure
1. Otwórz [portal Azure](https://portal.azure.com/).
2. Przejdź do subskrypcji, która zawiera **zadania elastycznej bazy danych** składniki, to znaczy subskrypcji, w których elastycznej bazy danych zostały zainstalowane składniki zadania.
3. Kliknij przycisk **Przeglądaj** i kliknij przycisk **grup zasobów**.
4. Wybierz grupę zasobów o nazwie "__ElasticDatabaseJob".
5. Usuń grupę zasobów.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>Odinstaluj składniki zadania elastycznej bazy danych przy użyciu programu PowerShell
1. Uruchom okno poleceń programu PowerShell usługi Microsoft Azure i przejdź do podkatalogu narzędzia w folderze Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x: typ **narzędzia cd**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > Narzędzia dysku cd
2. Wykonanie.\UninstallElasticDatabaseJobs.ps1 skrypt programu PowerShell.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1   PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1

Lub po prostu, uruchom następujący skrypt, przy założeniu, domyślne wartości, jeśli w instalacji składników:

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager

        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }

        Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a>Kolejne kroki
Aby ponownie zainstalować zadania elastycznej bazy danych, zobacz [instalowania usługi zadania elastycznej bazy danych](sql-database-elastic-jobs-service-installation.md)

Omówienie zadania elastycznej bazy danych, zobacz [omówienie zadania elastycznej bazy danych](sql-database-elastic-jobs-overview.md).

<!--Image references-->


