---
title: Przenoszenie zasobu maszyny Wirtualnej systemu Windows na platformie Azure | Dokumentacja firmy Microsoft
description: "Przenieś Maszynę wirtualną systemu Windows do innego Azure subskrypcji lub grupy zasobów w modelu wdrażania usługi Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 1db25a5d9ff5cb6aa2787a0cafa40cfb010e3b06
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Przenieś Maszynę wirtualną systemu Windows do innego Azure subskrypcji lub grupy zasobów
W tym artykule przedstawiono sposób przenoszenia maszyny Wirtualnej systemu Windows między grupami zasobów lub subskrypcji. Przenoszenie między subskrypcjami można skorzystać, jeśli pierwotnie utworzono Maszynę wirtualną w osobistych subskrypcji i chcesz teraz Przenieś go do subskrypcji w firmie, aby kontynuować pracę.

> [!IMPORTANT]
>Nie można przenieść dysków zarządzanych w tej chwili. 
>
>Nowych identyfikatorów zasobów są tworzone w ramach przeniesienia. Po przeniesieniu maszyny Wirtualnej należy zaktualizować narzędzia i skrypty do używania nowych identyfikatorów zasobów. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Aby przenieść Maszynę wirtualną za pomocą programu Powershell
Aby przenieść maszynę wirtualną w innej grupie zasobów, musisz upewnij się, że można również przenieść wszystkie zasoby zależne. Korzystanie z polecenia cmdlet Move-AzureRMResource wymaga nazwy zasobu i typu zasobu. Możesz uzyskać zarówno z polecenia cmdlet AzureRMResource Znajdź.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"


Aby przenieść Maszynę wirtualną, trzeba przenieść wiele zasobów. Firma Microsoft wystarczy utworzyć oddzielne zmienne dla każdego zasobu, a następnie na liście. W tym przykładzie zawiera większość podstawowych zasobów dla maszyny Wirtualnej, ale można dodać więcej zgodnie z potrzebami.

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"

    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"

    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

Przeniesienie zasobów do innej subskrypcji, obejmują **- DestinationSubscriptionId** parametru. 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Użytkownik jest proszony o potwierdzenie, że chcesz przenieść określonych zasobów. Typ **Y** aby upewnić się, że chcesz przenieść zasoby.

## <a name="next-steps"></a>Następne kroki
Wiele różnych typów zasobów można przenosić między grupami zasobów i subskrypcje. Aby uzyskać więcej informacji, zobacz [Move resources to new resource group or subscription](../../resource-group-move-resources.md) (Przenoszenie zasobów do nowej grupy lub subskrypcji).    

