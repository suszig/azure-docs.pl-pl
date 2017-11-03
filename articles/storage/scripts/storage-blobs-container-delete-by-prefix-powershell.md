---
title: "Przykładowy skrypt programu PowerShell Azure - Usuń kontenery według prefiksu | Dokumentacja firmy Microsoft"
description: "Usuń kontenerów obiektów blob magazynu Azure na podstawie prefiksu nazwy kontenera."
services: storage
documentationcenter: na
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/13/2017
ms.author: robinsh
ms.openlocfilehash: 402958c4e2978630bc79557704a77e77a8b9a7e7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="delete-containers-based-on-container-name-prefix"></a>Usuń kontenerów na podstawie prefiksu nazwy kontenera

Ten skrypt usuwa kontenerów w magazynie obiektów Blob platformy Azure na podstawie prefiksu nazwy kontenera.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/storage/delete-containers-by-prefix/delete-containers-by-prefix.ps1 "Delete containers by prefix")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, pozostałe kontenerów, i wszystkich powiązanych zasobów.

```powershell
Remove-AzureRmResourceGroup -Name containerdeletetestrg
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do usuwania kontenerów na podstawie prefiksu nazwy kontenera. Każdy element w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Get-AzureRmStorageAccount](/powershell/module/azurerm.storage/get-azurermstorageaccount) | Pobiera określonego konta magazynu lub wszystkich kont magazynu w grupie zasobów lub subskrypcji. |
| [Get-AzureStorageContainer](/powershell/module/azure.storage/get-azurestoragecontainer) | Wyświetla kontenery magazynu skojarzone z kontem magazynu. |
| [Usuń AzureStorageContainer](/powershell/module/azure.storage/remove-azurestoragecontainer) | Usuwa wybrany magazyn kontenera. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Przykłady skryptów PowerShell dodatkowego miejsca do magazynowania znajdują się w [przykłady środowiska PowerShell dla usługi Azure Blob storage](../blobs/storage-samples-blobs-powershell.md).
