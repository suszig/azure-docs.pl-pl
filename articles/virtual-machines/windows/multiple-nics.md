---
title: "Tworzenie i zarządzanie maszynami wirtualnymi systemu Windows na platformie Azure używanego przez wiele kart sieciowych | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie tworzenia i zarządzania nimi maszyny Wirtualnej systemu Windows, który ma wiele kart sieciowych do niego dołączony przy użyciu szablonów programu Azure PowerShell lub Menedżera zasobów."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/26/2017
ms.author: iainfou
ms.openlocfilehash: 941791ba398a3abbaa5137c36391fd23789cd3b1
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/20/2017
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a>Tworzenie i zarządzanie nimi maszyny wirtualnej systemu Windows, który ma wiele kart sieciowych
Maszynach wirtualnych (VM) na platformie Azure mogą mieć wiele wirtualnych kart interfejsu sieciowego (NIC) dołączona do nich. Typowy scenariusz ma różne podsieci dla łączności frontonu i zaplecza lub sieć przeznaczona do monitorowania lub kopii zapasowej rozwiązanie. Ten artykuł zawiera szczegóły dotyczące sposobu tworzenia maszyny Wirtualnej, który ma wiele kart sieciowych do niego dołączony. Możesz również sposób dodawania lub usuwania kart sieciowych z istniejącej maszyny Wirtualnej. Różne [rozmiarów maszyn wirtualnych](sizes.md) obsługuje różną liczbę kart sieciowych, więc odpowiednio rozmiar maszyny Wirtualnej.

## <a name="prerequisites"></a>Wymagania wstępne
Upewnij się, że masz [najnowszą wersję programu Azure PowerShell zainstalowany i skonfigurowany](/powershell/azure/overview).

W poniższych przykładach Zastąp przykładowe nazwy parametrów własne wartości. Przykład nazwy parametru zawierają *myResourceGroup*, *myVnet*, i *myVM*.


## <a name="create-a-vm-with-multiple-nics"></a>Tworzenie maszyny wirtualnej z wieloma kartami sieciowymi
Najpierw utwórz grupę zasobów. Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroup* w *EastUs* lokalizacji:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a>Tworzenie sieci wirtualnej i podsieci
Typowy scenariusz dotyczy sieci wirtualnej do dwóch lub więcej podsieci. W jednej podsieci mogą być frontonu ruchu, a drugą dla ruchu zaplecza. Aby połączyć się obie podsieci, będzie używać wielu kart sieciowych na maszynie Wirtualnej.

1. Zdefiniuj dwie podsieci sieci wirtualnej z [AzureRmVirtualNetworkSubnetConfig nowy](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). W poniższym przykładzie zdefiniowano podsieci dla *mySubnetFrontEnd* i *mySubnetBackEnd*:

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. Tworzenie sieci wirtualnej i podsieci z [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Poniższy przykład tworzy sieć wirtualną o nazwie *myVnet*:

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a>Tworzenie wielu kart sieciowych
Utwórz dwie karty sieciowe z [AzureRmNetworkInterface nowy](/powershell/module/azurerm.network/new-azurermnetworkinterface). Dołącz jedną kartę Sieciową do podsieci frontonu i jednej karcie Sieciowej z podsiecią zaplecza. Poniższy przykład tworzy karty sieciowe o nazwie *myNic1* i *myNic2*:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic1" `
    -Location "EastUs" `
    -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic2" `
    -Location "EastUs" `
    -SubnetId $backEnd.Id
```

Zwykle również utworzyć [sieciowej grupy zabezpieczeń](../../virtual-network/virtual-networks-nsg.md) do filtrowania ruchu sieciowego z maszyną wirtualną a [modułu równoważenia obciążenia](../../load-balancer/load-balancer-overview.md) rozłożenie ruchu między wieloma maszynami wirtualnymi.

### <a name="create-the-virtual-machine"></a>Tworzenie maszyny wirtualnej
Teraz rozpocząć tworzenie konfiguracji maszyny Wirtualnej. Rozmiar każdej maszyny Wirtualnej ma limit całkowitej liczby kart sieciowych, które można dodać do maszyny Wirtualnej. Aby uzyskać więcej informacji, zobacz [rozmiarów maszyn wirtualnych systemu Windows](sizes.md).

1. Ustaw poświadczenia maszyny Wirtualnej `$cred` zmiennej w następujący sposób:

    ```powershell
    $cred = Get-Credential
    ```

2. Zdefiniuj maszyny Wirtualnej z [nowe AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). W poniższym przykładzie zdefiniowano maszyny Wirtualnej o nazwie *myVM* i używa rozmiar maszyny Wirtualnej, która obsługuje więcej niż dwie karty sieciowe (*Standard_DS3_v2*):

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. Utwórz reszty konfiguracji maszyny Wirtualnej z [AzureRmVMOperatingSystem zestaw](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) i [AzureRmVMSourceImage zestawu](/powershell/module/azurerm.compute/set-azurermvmsourceimage). Poniższy przykład tworzy Maszynę wirtualną systemu Windows Server 2016:

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
        -Windows `
        -ComputerName "myVM" `
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
        -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" `
        -Skus "2016-Datacenter" `
        -Version "latest"
   ```

4. Dołącz dwie karty sieciowe utworzone wcześniej przy użyciu [AzureRmVMNetworkInterface Dodaj](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. Na koniec należy utworzyć maszyny Wirtualnej z [AzureRmVM nowy](/powershell/module/azurerm.compute/new-azurermvm):

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

## <a name="add-a-nic-to-an-existing-vm"></a>Dodać kartę Sieciową do istniejącej maszyny Wirtualnej
Aby dodać wirtualną kartę Sieciową do istniejącej maszyny Wirtualnej, deallocate maszyny Wirtualnej, dodać wirtualnej karty Sieciowej, a następnie uruchom maszynę Wirtualną. Różne [rozmiarów maszyn wirtualnych](sizes.md) obsługuje różną liczbę kart sieciowych, więc odpowiednio rozmiar maszyny Wirtualnej. W razie potrzeby można [Zmień rozmiar maszyny Wirtualnej](resize-vm.md).

1. Cofnięcie przydziału maszyny Wirtualnej z [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). Poniższy przykład cofa alokację maszyny Wirtualnej o nazwie *myVM* w *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Pobierz istniejącej konfiguracji maszyny Wirtualnej z [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). Poniższy przykład pobiera informacje o Maszynie wirtualnej o nazwie *myVM* w *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Poniższy przykład tworzy wirtualnej karty Sieciowej z [AzureRmNetworkInterface nowy](/powershell/module/azurerm.network/new-azurermnetworkinterface) o nazwie *myNic3* dołączona do *mySubnetBackEnd*. Wirtualnej karty Sieciowej jest następnie dołączony do maszyny Wirtualnej o nazwie *myVM* w *myResourceGroup* z [AzureRmVMNetworkInterface Dodaj](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

    ```powershell
    # Get info for the back end subnet
    $myVnet = Get-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get the ID of the new virtual NIC and add to VM
    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId | Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a>Podstawowy wirtualnych kart sieciowych
    Jedną z kart sieciowych w maszynie Wirtualnej z wieloma kartami Sieciowymi, musi być podstawowym. Jeśli jeden z istniejących wirtualnych kart sieciowych w maszynie Wirtualnej jest już ustawiony jako podstawowy, możesz pominąć ten krok. W poniższym przykładzie założono, że dwie wirtualne karty sieciowe teraz znajdują się na maszynie Wirtualnej i chcesz dodać pierwszej karty Sieciowej (`[0]`) jako serwera podstawowego:
        
    ```powershell
    # List existing NICs on the VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 to be primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update the VM state in Azure
    Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. Uruchom maszynę Wirtualną z [Start AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

## <a name="remove-a-nic-from-an-existing-vm"></a>Usuń z istniejącej maszyny Wirtualnej karty Sieciowej
Usunięcie wirtualnej karty Sieciowej z istniejącej maszyny Wirtualnej, należy cofnąć maszyny Wirtualnej, Usuń wirtualną kartę Sieciową, a następnie uruchom maszynę Wirtualną.

1. Cofnięcie przydziału maszyny Wirtualnej z [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). Poniższy przykład cofa alokację maszyny Wirtualnej o nazwie *myVM* w *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Pobierz istniejącej konfiguracji maszyny Wirtualnej z [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). Poniższy przykład pobiera informacje o Maszynie wirtualnej o nazwie *myVM* w *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Pobierz informacje o Usuń kartę Sieciową z [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface). Poniższy przykład pobiera informacje *myNic3*:

    ```powershell
    # List existing NICs on the VM if you need to determine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. Usuń z kartą Sieciową o [AzureRmVMNetworkInterface Usuń](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) , a następnie zaktualizować maszyny Wirtualnej z [AzureRmVm aktualizacji](/powershell/module/azurerm.compute/update-azurermvm). Poniższy przykład umożliwia usunięcie *myNic3* uzyskanych przez `$nicId` w poprzednim kroku:

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. Uruchom maszynę Wirtualną z [Start AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a>Tworzenie wielu kart sieciowych z szablonami
Szablony usługi Azure Resource Manager zapewnia możliwości tworzenia wielu wystąpień zasobu podczas wdrażania, takich jak tworzenie wielu kart sieciowych. Szablony Menedżera zasobów umożliwia definiowanie środowiska deklaratywne pliki w formacie JSON. Aby uzyskać więcej informacji, zobacz [Omówienie usługi Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Można użyć *kopiowania* umożliwia określenie liczby wystąpień, aby utworzyć:

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

Aby uzyskać więcej informacji, zobacz [tworzenie wielu wystąpień przy użyciu *kopiowania*](../../resource-group-create-multiple.md). 

Można również użyć `copyIndex()` dołączyć numer do nazwy zasobu. Następnie możesz utworzyć *myNic1*, *MyNic2* i tak dalej. Poniższy kod przedstawia przykład dołączanie wartość indeksu:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Pełny przykład można znaleźć [tworzenia wielu kart sieciowych przy użyciu szablonów usługi Resource Manager](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Następne kroki
Przegląd [rozmiarów maszyn wirtualnych systemu Windows](sizes.md) Jeśli próbujesz utworzyć maszynę Wirtualną, która ma wiele kart sieciowych. Należy zwrócić uwagę na maksymalną liczbę kart sieciowych obsługiwanych przez każdy rozmiar maszyny Wirtualnej. 


