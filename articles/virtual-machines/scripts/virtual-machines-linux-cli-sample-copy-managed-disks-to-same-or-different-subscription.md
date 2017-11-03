---
title: "Azure CLI przykładowym skrypcie - dyskach do tych samych lub różnych subskrypcji zarządzanych kopiowania (przenoszenia) | Dokumentacja firmy Microsoft"
description: "Azure CLI przykładowym skrypcie - kopiowania (przenoszenia) zarządzanych dysków do tych samych lub różnych subskrypcji."
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
ms.openlocfilehash: 8ff34f3d0b11c47f19205b92aebfc96e5cd5a014
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>Kopiowanie dysków zarządzanych do tych samych lub różnych subskrypcji z interfejsu wiersza polecenia

Ten skrypt powoduje skopiowanie dysków zarządzanych do tych samych lub różnych subskrypcji, ale w tym samym regionie. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do tworzenia nowych dysków zarządzanych w subskrypcji docelowej przy użyciu identyfikatora dysków zarządzanych w źródłowym. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Pokaż dysku az](https://docs.microsoft.com/cli/azure/disk#az_disk_show) | Pobiera właściwości dysków zarządzanych przy użyciu nazwy i właściwości grupy zasobów dysku zarządzanego. Identyfikator jest używana do kopiowania dysków zarządzanych do innej subskrypcji.  |
| [Tworzenie dysku az](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Kopie dysków zarządzanych przez utworzenie nowego zarządzanego dysku w innej subskrypcji przy użyciu identyfikatora i nazwy dysków zarządzanych w nadrzędnej.  |

## <a name="next-steps"></a>Następne kroki

[Utwórz maszynę wirtualną z dyskiem zarządzanym](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe maszyny wirtualnej i dysków zarządzanych przykłady skryptów interfejsu wiersza polecenia można znaleźć w [dokumentacji maszyny Wirtualnej systemu Linux Azure](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
