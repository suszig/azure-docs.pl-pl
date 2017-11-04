---
title: "Skrypt programu PowerShell: przyrostowo ładowanie danych przy użyciu fabryki danych Azure | Dokumentacja firmy Microsoft"
description: "Ten skrypt programu PowerShell pokazano, jak przyrostowo skopiować dane z bazy danych SQL Azure do magazynu obiektów Blob Azure przy użyciu fabryki danych Azure."
services: data-factory
author: spelluru
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: spelluru
ms.openlocfilehash: 36a8c8c4ed83a56dfd0f487ec4bcf030e3890ef6
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="powershell-script---incrementally-load-data-by-using-azure-data-factory"></a>Skrypt programu PowerShell - przyrostowo ładowanie danych przy użyciu fabryki danych Azure
Ten przykładowy skrypt programu PowerShell ładuje tylko nowe lub zaktualizowane rekordy z magazynu danych źródła do ujścia magazynu danych po początkowej pełną kopię danych ze źródła do ujścia.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

Zobacz [samouczek: przyrostowej kopii](../tutorial-incremental-copy-powershell.md#prerequisites) wymagania wstępne dotyczące uruchamiania tego przykładu. 

## <a name="sample-script"></a>Przykładowy skrypt

> [!IMPORTANT]
> Ten skrypt tworzy pliki w formacie JSON, które definiują jednostek fabryki danych (połączonej usługi, zestawu danych i potoku) na dysku twardym w folderze c:\.

[!code-powershell[main](../../../powershell_scripts/data-factory/incremental-copy-from-azure-sql-to-blob/incremental-copy-from-azure-sql-to-blob.ps1 "Incremental copy from Azure SQL Database to Azure Blob Storage")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia

Po uruchomieniu przykładowy skrypt służy następujące polecenie Usuń grupę zasobów i wszystkie zasoby skojarzone z nią:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```
Aby usunąć fabryki danych z grupy zasobów, uruchom następujące polecenie: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń: 

| Polecenie | Uwagi |
|---|---|
| [Nowe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Zestaw AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2) | Tworzenie fabryki danych. |
| [Zestaw AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | Tworzy połączonej usługi w fabryce danych. Połączona usługa łączy w magazynie danych lub obliczeniowych do fabryki danych. |
| [Zestaw AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | Tworzy element dataset w fabryce danych. Zestaw danych reprezentuje wejścia/wyjścia dla działania w potoku. | 
| [Zestaw AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactorv2ypipeline) | Tworzy potok w fabryce danych. Potok zawiera jeden lub więcej działań, które wykonuje określonej operacji. W tym potoku działanie kopiowania kopiuje dane z jednej lokalizacji do innej lokalizacji w magazynie obiektów Blob Azure. |
| [Wywołanie AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipelinerun) | Tworzy uruchomienia procesu. Innymi słowy jest uruchamiana potoku. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | Pobiera szczegóły uruchomienie działania (działanie Uruchom) w potoku. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |
|||

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących programu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](https://docs.microsoft.com/powershell/).

Dodatkowe przykłady skryptów PowerShell fabryki danych Azure można znaleźć w [skryptów programu PowerShell fabryki danych Azure](../samples-powershell.md).