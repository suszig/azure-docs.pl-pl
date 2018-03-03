---
title: "Skrypt programu PowerShell: kopiowanie danych w chmurze przy użyciu fabryki danych Azure | Dokumentacja firmy Microsoft"
description: "Ten skrypt programu PowerShell kopiuje dane z jednego miejsca w magazynie obiektów Blob Azure do innej lokalizacji, w tym samym magazynie obiektów Blob."
services: data-factory
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: jingwang
ms.openlocfilehash: cb452735b2f8f855a43d12c0fd50973baa3150fd
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="use-powershell-to-create-a-data-factory-pipeline-to-copy-data-in-the-cloud"></a>Aby utworzyć potok fabryki danych do skopiowania danych w chmurze za pomocą programu PowerShell

Ten przykładowy skrypt programu PowerShell tworzy potok w fabryce danych Azure, które kopiuje dane z jednej lokalizacji do innej lokalizacji w magazynie obiektów Blob Azure.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>Wymagania wstępne
* **Konto usługi Azure Storage**. Magazyn obiektów blob jest używany zarówno jako magazyn **źródła**, jak i **ujścia** danych. Jeśli nie masz konta usługi Azure Storage, zobacz [Tworzenie konta magazynu](../../storage/common/storage-create-storage-account.md#create-a-storage-account), aby uzyskać informacje o jego tworzeniu. 
* Utwórz **kontener obiektów blob** w usłudze Blob Storage, utwórz **folder** wejściowy w kontenerze i przekaż niektóre pliki do folderu. Narzędzia, takie jak [Eksplorator usługi Azure Storage](https://azure.microsoft.com/features/storage-explorer/), umożliwiają łączenie z usługą Azure Blob Storage, tworzenie kontenera obiektów blob, przekazywanie pliku wejściowego i weryfikację pliku wyjściowego.

## <a name="sample-script"></a>Przykładowy skrypt

> [!IMPORTANT]
> Ten skrypt tworzy pliki w formacie JSON, które definiują jednostek fabryki danych (połączonej usługi, zestawu danych i potoku) na dysku twardym w folderze c:\.

[!code-powershell[main](../../../powershell_scripts/data-factory/copy-from-azure-blob-to-blob/copy-from-azure-blob-to-blob.ps1 "Copy from Blob Storage -> Blob Storage")]


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
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tworzy grupę zasobów, w której są przechowywane wszystkie zasoby. |
| [Set-AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2) | Tworzenie fabryki danych. |
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | Tworzy połączonej usługi w fabryce danych. Połączona usługa łączy w magazynie danych lub obliczeniowych do fabryki danych. |
| [Set-AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | Tworzy element dataset w fabryce danych. Zestaw danych reprezentuje wejścia/wyjścia dla działania w potoku. | 
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactorv2ypipeline) | Tworzy potok w fabryce danych. Potok zawiera jeden lub więcej działań, które wykonuje określonej operacji. W tym potoku działanie kopiowania kopiuje dane z jednej lokalizacji do innej lokalizacji w magazynie obiektów Blob Azure. |
| [Invoke-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipelinerun) | Tworzy uruchomienia procesu. Innymi słowy jest uruchamiana potoku. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | Pobiera szczegóły uruchomienie działania (działanie Uruchom) w potoku. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Usuwa grupę zasobów wraz ze wszystkimi zagnieżdżonymi zasobami. |
|||

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat programu Azure PowerShell, zobacz [dokumentację programu Azure PowerShell](https://docs.microsoft.com/powershell/).

Dodatkowe przykłady skryptów PowerShell fabryki danych Azure można znaleźć w [przykłady środowiska PowerShell fabryki danych Azure](../samples-powershell.md).