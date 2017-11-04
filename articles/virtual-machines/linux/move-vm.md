---
title: "Przenieś Maszynę wirtualną systemu Linux na platformie Azure | Dokumentacja firmy Microsoft"
description: "Przenieś Maszynę wirtualną systemu Linux do innego Azure subskrypcji lub grupy zasobów w modelu wdrażania usługi Resource Manager."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d635f0a5-4458-4b95-a5f8-eed4f41eb4d4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 4695a9c934f97f2b2d448c4990e7ad5533e38e9f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Przenieś Maszynę wirtualną systemu Linux do innej subskrypcji lub grupy zasobów
W tym artykule przedstawiono sposób przenieść Maszynę wirtualną systemu Linux między grupami zasobów lub subskrypcji. Przenoszenie maszyny Wirtualnej między subskrypcjami można przydatne, jeśli utworzono Maszynę wirtualną w osobistych subskrypcji i chcesz teraz Przenieś go do subskrypcji w firmie.

> [!IMPORTANT]
>Nie można przenieść dysków zarządzanych w tej chwili. 
>
>Nowych identyfikatorów zasobów są tworzone w ramach przeniesienia. Po przeniesieniu maszyny Wirtualnej należy zaktualizować narzędzia i skrypty do używania nowych identyfikatorów zasobów. 
> 
> 

## <a name="use-the-azure-cli-to-move-a-vm"></a>Użyj interfejsu wiersza polecenia Azure, aby przenieść Maszynę wirtualną
Aby pomyślnie przenieść Maszynę wirtualną, należy przenieść maszynę Wirtualną i wszystkie dodatkowe zasoby. Użyj **Pokaż grupy azure** polecenie, aby wyświetlić listę wszystkich zasobów w grupie zasobów i ich identyfikatorów. Pomaga przekazać dane wyjściowe tego polecenia do pliku, można skopiować i wkleić identyfikatorów na nowsze poleceń.

    azure group show <resourceGroupName>

Aby przenieść Maszynę wirtualną i jej zasobów w innej grupie zasobów, użyj **przenoszenia zasobów platformy azure** polecenia interfejsu wiersza polecenia. Poniższy przykład przedstawia sposób przenoszenia maszyny Wirtualnej i najbardziej typowe zasoby, które wymaga. Używamy **-i** parametr i przekaż listy rozdzielanej przecinkami (bez spacji) identyfikatorów zasobów do przeniesienia.

    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      

    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"

Jeśli chcesz przenieść maszynę Wirtualną i jej zasobów do innej subskrypcji, należy dodać **— identyfikator subskrypcji docelowej &#60; destinationSubscriptionID &#62;** parametr, aby określić subskrypcji docelowej.

Jeśli pracujesz z wiersza polecenia na komputerze z systemem Windows, należy dodać  **$**  przed nazwy zmiennych przy deklarowaniu je. To nie jest wymagana w systemie Linux.

Zostanie wyświetlona prośba o potwierdzenie, że chcesz przenieść określonego zasobu. Typ **Y** aby upewnić się, że chcesz przenieść zasoby.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Następne kroki
Wiele różnych typów zasobów można przenosić między grupami zasobów i subskrypcje. Aby uzyskać więcej informacji, zobacz [Move resources to new resource group or subscription](../../resource-group-move-resources.md) (Przenoszenie zasobów do nowej grupy lub subskrypcji).    

