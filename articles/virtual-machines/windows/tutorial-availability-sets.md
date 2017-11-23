---
title: "Samouczek zestawy dostępności dla maszyn wirtualnych systemu Windows na platformie Azure | Dokumentacja firmy Microsoft"
description: "Informacje na temat zestawów dostępności dla maszyn wirtualnych systemu Windows na platformie Azure."
documentationcenter: 
services: virtual-machines-windows
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/05/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 2345b434a51b768793c2dea4587dc0a49ab35b70
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2017
---
# <a name="how-to-use-availability-sets"></a>Jak używać zestawów dostępności

W tym samouczku Dowiedz się jak zwiększyć dostępność i niezawodność rozwiązań maszyny wirtualnej na platformie Azure przy użyciu funkcji o nazwie zestawów dostępności. Zestawy dostępności upewnij się, że maszyny wirtualne, należy wdrożyć na platformie Azure są rozproszone na wielu węzłach izolowanego sprzętu w klastrze. W ten sposób zapewnia, że jeśli awarii sprzętu lub oprogramowania w systemie Azure wykonywany, wpływ na podrzędne zestaw maszyn wirtualnych i który rozwiązania ogólną pozostaje dostępny i działa. 

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie zestawu dostępności
> * Tworzenie maszyny Wirtualnej w zestawie dostępności
> * Sprawdź dostępne rozmiary maszyny Wirtualnej
> * Sprawdź Azure Advisor

Dla tego samouczka jest wymagany moduł Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="availability-set-overview"></a>Omówienie zestawu dostępności

Zestawu dostępności jest możliwość grupę logiczną, która na platformie Azure umożliwia Sprawdź, czy zasobów maszyny Wirtualnej, który można umieścić w nim są odizolowane od siebie, gdy są wdrożone w centrum danych Azure. Azure zapewnia, że maszyny wirtualne, umieść w ramach zestawu dostępności uruchamiania na wielu serwerach fizycznych, obliczeniowe stojakami jednostek magazynowych i przełączniki sieciowe. W przypadku sprzętu lub oprogramowania Azure awarii, ma wpływ tylko podzestaw maszyn wirtualnych, a ogólną aplikacji utrzyma się i nadal będzie dostępna dla klientów. Zestawy dostępności są podstawowych możliwości do tworzenia rozwiązań chmur wiarygodne.

Zastanówmy typowe rozwiązania oparte na Maszynach którym może mieć 4 serwery frontonu sieci web i korzystać z 2 maszyny wirtualne zaplecza, które hostują bazy danych. Przy użyciu platformy Azure, należy zdefiniować dwa zestawy dostępności, przed wdrożeniem maszyn wirtualnych: jeden zbiór dostępności dla warstwy sieci web i jeden zbiór dostępności dla warstwy bazy danych. Podczas tworzenia nowej maszyny Wirtualnej można określić polecenia Utwórz zestaw jako parametr do maszyny wirtualnej az dostępności i Azure automatycznie upewnia się, że maszyny wirtualne utworzone w zestawie dostępne są izolowane między wieloma zasobami sprzętu fizycznego. Jeśli sprzęt fizyczny wykorzystywany do jednego serwera sieci Web lub maszyn wirtualnych z serwera bazy danych jest uruchomiona na ma problem, wiadomo, że inne wystąpienia serwera sieci Web i bazy danych maszyn wirtualnych będą nadal działać, ponieważ są one na inny komputer.

Użyj zbiorów dostępności Jeśli chcesz wdrożyć niezawodne rozwiązania na podstawie maszyny Wirtualnej na platformie Azure.

## <a name="create-an-availability-set"></a>Tworzenie zestawu dostępności

Możesz utworzyć zestaw przy użyciu danych o dostępności [AzureRmAvailabilitySet nowy](/powershell/module/azurerm.compute/new-azurermavailabilityset). W tym przykładzie umieszczania liczba domen aktualizacji i odporność na *2* dla zestawu o nazwie dostępności *myAvailabilitySet* w *myResourceGroupAvailability* grupy zasobów.

Utwórz grupę zasobów.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```

Tworzenie zarządzanego dostępności, ustawić za pomocą [AzureRmAvailabilitySet nowy](/powershell/module/azurerm.compute/new-azurermavailabilityset) z **- sku wyrównane** parametru.

```azurepowershell-interactive
New-AzureRmAvailabilitySet `
   -Location EastUS `
   -Name myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability `
   -sku aligned `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Tworzenie maszyn wirtualnych w zestawie dostępności

Maszyny wirtualne muszą być tworzone dostępność ustawioną upewnij się, że są one poprawnie dystrybuowana sprzętu. Nie można dodać istniejącej maszyny Wirtualnej do dostępności po jego utworzeniu. 

Sprzęt w lokalizacji jest podzielony na wiele domen aktualizacji i domen błędów. **Domeny aktualizacji** jest grupą maszyn wirtualnych i podstawowym sprzętem fizycznym, który może zostać uruchomiony ponownie w tym samym czasie. Maszyny wirtualne w tej samej **domeny błędów** udostępnianie typowych magazynu, a także wspólnego przełącznik źródła i sieci zasilania. 

Po utworzeniu konfiguracji maszyny Wirtualnej przy użyciu [AzureRMVMConfig nowy](/powershell/module/azurerm.compute/new-azurermvmconfig) używasz `-AvailabilitySetId` parametr, aby określić identyfikator zestawu dostępności.

Utwórz dwie maszyny wirtualne z [AzureRmVM nowy](/powershell/module/azurerm.compute/new-azurermvm) w zestawie dostępności.

```azurepowershell-interactive
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAvailability `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig
    
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

$nsg = New-AzureRmNetworkSecurityGroup `
    -Location eastus `
    -Name myNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAvailability `
    -SecurityRules $nsgRuleRDP
    
# Apply the network security group to a subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name mySubnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24

# Update the virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

for ($i=1; $i -le 2; $i++)
{
   $pip = New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name "mypublicdns$(Get-Random)" `
        -AllocationMethod Static `
        -IdleTimeoutInMinutes 4

   $nic = New-AzureRmNetworkInterface `
        -Name myNic$i `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -SubnetId $vnet.Subnets[0].Id `
        -PublicIpAddressId $pip.Id `
        -NetworkSecurityGroupId $nsg.Id

   # Here is where we specify the availability set
   $vm = New-AzureRmVMConfig `
        -VMName myVM$i `
        -VMSize Standard_D1 `
        -AvailabilitySetId $availabilitySet.Id

   $vm = Set-AzureRmVMOperatingSystem `
        -ComputerName myVM$i `
        -Credential $cred `
        -VM $vm `
        -Windows `
        -EnableAutoUpdate `
        -ProvisionVMAgent
   $vm = Set-AzureRmVMSourceImage `
        -VM $vm `
        -PublisherName MicrosoftWindowsServer `
        -Offer WindowsServer `
        -Skus 2016-Datacenter `
        -Version latest
   $vm = Set-AzureRmVMOSDisk `
        -VM $vm `
        -Name myOsDisk$i `
        -DiskSizeInGB 128 `
        -CreateOption FromImage `
        -Caching ReadWrite
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -VM $vm
}

```

Trwa kilka minut, aby utworzyć i skonfigurować obie maszyny wirtualne. Po zakończeniu będziesz mieć dwie maszyny wirtualne rozproszone na używanego sprzętu. 

Jeśli przyjrzymy się zestawem dostępności w portalu, przechodząc do grupy zasobów > myResourceGroupAvailability > myAvailabilitySet, należy się w temacie jak maszyny wirtualne rozproszone na 2 błędów i aktualizować domen.

![Zestawem dostępności w portalu](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Sprawdź, czy dostępne rozmiary maszyny Wirtualnej 

Możesz dodać więcej maszyn wirtualnych do później zestaw dostępności, ale musisz wiedzieć, jaki rozmiarów maszyn wirtualnych są dostępne na sprzęcie. Użyj [Get AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) Aby wyświetlić listę wszystkich dostępnych rozmiarów na sprzęcie klastra dla zestawu dostępności.

```azurepowershell-interactive
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="check-azure-advisor"></a>Sprawdź Azure Advisor 

Aby uzyskać więcej informacji na temat sposobów poprawy dostępności maszyn wirtualnych umożliwia także Azure Advisor. Azure Advisor pomaga należy stosować najlepsze rozwiązania w celu zoptymalizowania wdrożeń platformy Azure. Analizuje konfigurację zasobu i danych telemetrycznych użycia, a następnie zaleca rozwiązania, które ułatwiają zwiększenie opłacalności, wydajność, wysoką dostępność i zabezpieczeń zasobów platformy Azure.

Zaloguj się do [portalu Azure](https://portal.azure.com), wybierz pozycję **więcej usług**i wpisz **Advisor**. Pulpit nawigacyjny usługi Advisor wyświetla spersonalizowane zalecenia dla wybranej subskrypcji. Aby uzyskać więcej informacji, zobacz [wprowadzenie do usługi Advisor Azure](../../advisor/advisor-get-started.md).


## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie zestawu dostępności
> * Tworzenie maszyny Wirtualnej w zestawie dostępności
> * Sprawdź dostępne rozmiary maszyny Wirtualnej
> * Sprawdź Azure Advisor

Przejdź do następnego samouczkiem, aby poznać zestawy skalowania maszyny wirtualnej.

> [!div class="nextstepaction"]
> [Tworzenie zestawu skalowania maszyn wirtualnych](tutorial-create-vmss.md)


