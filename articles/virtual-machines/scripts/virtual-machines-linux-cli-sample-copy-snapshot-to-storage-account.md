---
title: "Azure CLI przykładowym skrypcie - migawki eksportu/kopiowania jako dysk VHD do konta magazynu w innym regionie | Dokumentacja firmy Microsoft"
description: "Azure CLI przykładowym skrypcie - migawki eksportu/kopiowania jako dysk VHD do konta magazynu w tym samym lub różnych subskrypcji"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: cd0128256e016a329b0940eec4be41426cdaf51a
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-cli"></a>Eksport/kopiowania zarządzane migawki jako dysk VHD do konta magazynu w innym regionie z interfejsu wiersza polecenia

Ten skrypt eksportuje zarządzanych migawki na konto magazynu w innym regionie. Najpierw generuje identyfikator URI sygnatury dostępu Współdzielonego migawki, a następnie używa, aby skopiować go do konta magazynu w innym regionie. Użyj tego skryptu do obsługi kopii zapasowych dysków zarządzanych w innym regionie odzyskiwania po awarii. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]


## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń, można wygenerować identyfikatora URI sygnatury dostępu Współdzielonego dla zarządzanych migawki i kopiuje migawki na konto magazynu przy użyciu identyfikatora URI połączenia SAS. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [AZ migawki Udziel dostępu](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_grant_access) | Generuje SAS tylko do odczytu, służący do skopiuj podstawowy plik VHD na konto magazynu lub pobrać go do lokalnego  |
| [Uruchom kopiowania obiektu blob magazynu az](https://docs.microsoft.com/cli/azure/storage/blob/copy#az_storage_blob_copy_start) | Asynchronicznie kopiuje obiektu blob z jednego konta magazynu do innego |

## <a name="next-steps"></a>Następne kroki

[Tworzenie dysku zarządzanego z wirtualnego dysku twardego](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[Utwórz maszynę wirtualną z dyskiem zarządzanym](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe maszyny wirtualnej i dysków zarządzanych przykłady skryptów interfejsu wiersza polecenia można znaleźć w [dokumentacji maszyny Wirtualnej systemu Linux Azure](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
