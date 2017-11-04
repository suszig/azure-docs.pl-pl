---
title: "Azure CLI skrypt przykładowy — tworzenie dysków zarządzanych z migawki | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — tworzenie dysków zarządzanych z migawki"
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
ms.openlocfilehash: ffdad7faa34fec09623a415664b5a260868e9dbc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a>Tworzenie dysku zarządzanego z migawki z interfejsu wiersza polecenia

Ten skrypt tworzy dysków zarządzanych z migawki. Umożliwia ona przywrócenie maszyny wirtualnej z migawek systemu operacyjnego i dysków z danymi. Utwórz systemu operacyjnego i danych zarządzanych dysków z odpowiednich migawki, a następnie utwórz nową maszynę wirtualną dołączając dysków zarządzanych. Można także przywrócić dysków z danymi z istniejącej maszyny Wirtualnej, dołączając dyski danych utworzone na podstawie migawki.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń w celu utworzenia dysku zarządzanego z migawki. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Pokaż migawki az](https://docs.microsoft.com/cli/azure/snapshot#az_snapshot_show) | Pobiera właściwości migawki przy użyciu nazwy i właściwości grupy zasobów migawki. Właściwość identyfikatora służy do tworzenia dysków zarządzanych.  |
| [Tworzenie dysku az](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Tworzy zarządzanego przy użyciu dysku migawki identyfikator migawki zarządzanych |

## <a name="next-steps"></a>Następne kroki

[Utwórz maszynę wirtualną, dołączając dysków zarządzanych jako dysk systemu operacyjnego](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe maszyny wirtualnej i dysków zarządzanych przykłady skryptów interfejsu wiersza polecenia można znaleźć w [dokumentacji maszyny Wirtualnej systemu Linux Azure](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
