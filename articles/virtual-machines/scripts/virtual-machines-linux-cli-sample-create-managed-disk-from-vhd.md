---
title: "Azure CLI skrypt przykładowy — tworzenie dysków zarządzanych z pliku VHD na koncie magazynu w tej samej subskrypcji | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — tworzenie dysków zarządzanych z pliku VHD na koncie magazynu w tej samej subskrypcji"
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
ms.openlocfilehash: ede9457f5843d0a8a04503779970a553c5ed4f96
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-the-same-subscription-with-cli"></a>Tworzenie dysku zarządzanego z pliku VHD na koncie magazynu w tej samej subskrypcji z interfejsu wiersza polecenia

Ten skrypt tworzy dysków zarządzanych z pliku VHD na koncie magazynu w tej samej subskrypcji. Użyj tego skryptu, aby importować specjalne (nie uogólniony/Sysprep) wirtualnego dysku twardego na dysku systemu operacyjnego zarządzanych do utworzenia maszyny wirtualnej. Lub użyj go do zaimportowania danych dysku VHD na dysk danych zarządzanych. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]


## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń w celu utworzenia dysków zarządzanych z dysku VHD. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie dysku az](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Tworzy dysków zarządzanych przy użyciu identyfikatora URI dysku VHD na koncie magazynu w tej samej subskrypcji |

## <a name="next-steps"></a>Następne kroki

[Utwórz maszynę wirtualną, dołączając dysków zarządzanych jako dysk systemu operacyjnego](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe maszyny wirtualnej i dysków zarządzanych przykłady skryptów interfejsu wiersza polecenia można znaleźć w [dokumentacji maszyny Wirtualnej systemu Linux Azure](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
