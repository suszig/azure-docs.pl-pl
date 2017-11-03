---
title: "Przykładowy skrypt Azure CLI — ponowne uruchomienie maszyn wirtualnych | Dokumentacja firmy Microsoft"
description: "Azure CLI przykładowym skrypcie — ponowne uruchomienie maszyn wirtualnych, za pomocą tagu i identyfikator"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: ea114f484c774573b7d219cff9102a7308af356e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="restart-vms"></a>Ponowne uruchomienie maszyn wirtualnych

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

W tym przykładzie pokazano kilka sposobów, aby pobrać niektórych maszyn wirtualnych i uruchomić je ponownie.

Pierwszy powoduje ponowne uruchomienie wszystkich maszyn wirtualnych w grupie zasobów.

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

Drugi pobiera oznakowanych maszyn wirtualnych przy użyciu `az resouce list` filtrów do zasobów, które są maszyny wirtualne i ponowne uruchomienie tych maszyn wirtualnych.

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

W tym przykładzie działanie powłoki Bash. Aby wyświetlić opcje uruchamiania skryptów wiersza polecenia platformy Azure na kliencie systemu Windows, zobacz [działający w systemie Windows Azure CLI](../windows/cli-options.md).


## <a name="sample-script"></a>Przykładowy skrypt

Próbka ma trzy skryptów.
Pierwsza z nich inicjuje maszyn wirtualnych.
Opcja oczekiwania nie używa tak polecenie zwraca bez oczekiwania na każdej maszynie Wirtualnej na potrzeby aprowizacji.
Drugi czeka na maszynach wirtualnych być w pełni obsługiwany.
Trzeci skryptu ponowne uruchomienie wszystkich maszyn wirtualnych, które zostały udostępnione, a następnie tylko oznakowanych maszyn wirtualnych.

### <a name="provision-the-vms"></a>Zainicjuj obsługę maszyn wirtualnych

Ten skrypt tworzy grupę zasobów, a następnie tworzy trzech maszyn wirtualnych o ponowne uruchomienie.
Dwa z nich są oznaczone.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision the VMs")]

### <a name="wait"></a>Oczekiwanie

Ten skrypt sprawdza stan inicjowania obsługi administracyjnej, co 20 sekund, aż wszystkie trzy maszyny wirtualne są udostępnione lub jeden z nich nie powiedzie się, aby udostępnić.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for the VMs to be provisioned")]

### <a name="restart-the-vms"></a>Ponowne uruchomienie maszyn wirtualnych

Ten skrypt powoduje ponowne uruchomienie wszystkich maszyn wirtualnych w grupie zasobów, a następnie ponowne uruchomienie po prostu oznakowane maszyn wirtualnych.

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Po uruchomieniu przykładowym skrypcie następującego polecenia można usunąć grupy zasobów, maszyn wirtualnych i wszystkie powiązane zasoby.

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do utworzenia grupy zasobów, maszyny wirtualnej zestawu dostępności, usługi równoważenia obciążenia i wszystkie powiązane zasoby. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Tworzenie maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm/availability-set#az_vm_availability_set_create) | Tworzy maszyny wirtualne.  |
| [AZ listy maszyny wirtualnej](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | Używane z `--query` zapewnienie obsługi administracyjnej maszyn wirtualnych przed ponownym uruchomieniem je, a następnie pobrania identyfikatorów maszyn wirtualnych, aby uruchomić je ponownie. |
| [Lista zasobów az](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | Używane z `--query` pobrania identyfikatorów maszyn wirtualnych za pomocą znacznika. |
| [ponowne uruchomienie maszyny wirtualnej az](https://docs.microsoft.com/cli/azure/vm#az_vm_list) | Ponowne uruchomienie maszyn wirtualnych. |
| [Usuwanie grupy az](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Przykłady skryptów CLI dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Linux Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
