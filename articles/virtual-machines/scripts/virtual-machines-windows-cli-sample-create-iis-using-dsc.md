---
title: "Azure CLI skrypt przykładowy — Utwórz Maszynę wirtualną systemu Windows Server 2016 z usługami IIS przy użyciu usługi Konfiguracja DSC | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — Utwórz Maszynę wirtualną systemu Windows Server 2016 z usługami IIS przy użyciu usługi Konfiguracja DSC"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.custom: mvc
ms.openlocfilehash: 96516cb374841e663f831957ccbc6690e801154d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-vm-with-iis-using-dsc"></a>Utwórz maszynę Wirtualną z programem IIS za pomocą usługi Konfiguracja DSC

Ten skrypt tworzy maszynę wirtualną, a następnie używa rozszerzenia DSC maszyny wirtualnej Azure skryptu niestandardowego do instalowania i konfigurowania usług IIS. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-windows-iis-using-dsc/create-windows-iis-using-dsc.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do utworzenia grupy zasobów, maszyny wirtualnej i wszystkie powiązane zasoby. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Tworzenie maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Tworzy maszynę wirtualną i podłączony do karty sieciowej, sieci wirtualnej, podsieci i NSG. To polecenie określa również obraz maszyny wirtualnej ma być używany, a poświadczenia administracyjne.  |
| [zestaw rozszerzeń maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Dodaj niestandardowe rozszerzenie skryptu do maszyny wirtualnej, która wywołuje skrypt instalacji usług IIS. |
| [open — port az maszyny wirtualnej](https://docs.microsoft.com/cli/azure/vm#az_vm_open_port) | Tworzy reguły grupy zabezpieczeń sieci, aby zezwalać na ruch przychodzący. W tym przykładzie port 80 jest otwarty dla ruchu HTTP. |
| [Usuwanie grupy az](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Przykłady skryptów CLI dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Windows Azure](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
