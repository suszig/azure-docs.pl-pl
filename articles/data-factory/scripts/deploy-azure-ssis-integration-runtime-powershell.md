---
title: "PowerShell script - wdrażania środowiska uruchomieniowego integracji usług SSIS Azure | Dokumentacja firmy Microsoft"
description: "Ten skrypt programu PowerShell tworzy środowiska uruchomieniowego integracji usług SSIS Azure, uruchamiania pakietów SSIS w chmurze."
services: data-factory
author: douglaslMS
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: douglasl
ms.openlocfilehash: 959432378d49e18fbb567ea164c2608bfa02956d
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="powershell-script---deploy-azure-ssis-integration-runtime"></a>PowerShell script - wdrażania środowiska uruchomieniowego integracji usług SSIS Azure

Ten przykładowy skrypt programu PowerShell tworzy środowisko uruchomieniowe integracji Azure SSIS, które można uruchomić pakietów SSIS na platformie Azure.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/data-factory/deploy-azure-ssis-integration-runtime/deploy-azure-ssis-integration-runtime.ps1 "Deploy Azure-SSIS Integration Runtime")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia

Po uruchomieniu przykładowy skrypt służy następujące polecenie Usuń grupę zasobów i wszystkie zasoby skojarzone z nią:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```
Aby usunąć fabryki danych z grupy zasobów, uruchom następujące polecenie: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Objaśnienia dla skryptu

W tym skrypcie użyto następujących poleceń:

| Polecenie | Uwagi |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | Tworzy grupę zasobów, w której są przechowywane wszystkie zasoby. |
| [Set-AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2) | Tworzenie fabryki danych. |
| [Set-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2integrationruntime) | Tworzy środowiska uruchomieniowego integracji usług SSIS Azure, uruchamiania pakietów SSIS w chmurze |
| [Start-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/start-azurermdatafactoryv2integrationruntime) | Uruchamia środowiska uruchomieniowego integracji usług SSIS Azure. |
| [Get-AzureRmDataFactoryV2IntegrationRuntime](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2integrationruntime) | Pobiera informacje o środowiska uruchomieniowego integracji usług SSIS Azure. |
| [Get-AzureRmDataFactoryV2IntegrationRuntimeStatus](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2integrationruntimestatus) | Pobiera informacje o stanie środowiska uruchomieniowego integracji usług SSIS Azure. |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | Usuwa grupę zasobów wraz ze wszystkimi zagnieżdżonymi zasobami. |
|||

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat programu Azure PowerShell, zobacz [dokumentację programu Azure PowerShell](https://docs.microsoft.com/powershell/).

Dodatkowe przykłady skryptów PowerShell fabryki danych Azure można znaleźć w [przykłady środowiska PowerShell fabryki danych Azure](../samples-powershell.md).