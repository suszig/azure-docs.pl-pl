---
title: "Tworzenie maszyny Wirtualnej z obrazu zarządzanych na platformie Azure | Dokumentacja firmy Microsoft"
description: "Utwórz maszynę wirtualną systemu Windows z uogólniony obraz zarządzanych za pomocą programu Azure PowerShell lub portalu Azure w modelu wdrażania usługi Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: cynthn
ms.openlocfilehash: d8986c71fd0300af385750af898d620e6d3e1f24
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="create-a-vm-from-a-managed-image"></a>Utwórz maszynę Wirtualną z zarządzanego obrazu

Możesz utworzyć wiele maszyn wirtualnych z do zarządzanego obrazu maszyny Wirtualnej przy użyciu programu PowerShell lub w portalu Azure. Do zarządzanego obrazu maszyny Wirtualnej zawiera informacje niezbędne do tworzenia maszyn wirtualnych, dysków systemu operacyjnego i danych. Wirtualne dyski twarde, które tworzą obrazu, w tym zarówno dysków systemu operacyjnego i dysków z danymi, są przechowywane jako dyski zarządzanych. 

Musisz mieć już [utworzony obraz maszyny Wirtualnej zarządzanej](capture-image-resource.md) do użycia podczas tworzenia nowej maszyny Wirtualnej. 

## <a name="use-the-portal"></a>Używanie portalu

1. Otwórz [portal Azure](https://portal.azure.com).
2. W menu po lewej stronie wybierz **wszystkie zasoby**. Można sortować zasobów przez **typu** można łatwo odnaleźć obrazów.
3. Wybierz obraz, który ma być używany z listy. Obraz **omówienie** zostanie otwarta strona.
4. Kliknij przycisk **+ Utwórz wirtualna** z menu.
5. Wprowadź informacje o maszynie wirtualnej. Nazwa użytkownika i hasło wprowadzone w tym miejscu są używane na potrzeby logowania się do maszyny wirtualnej. Po zakończeniu kliknij przycisk **OK**. Utwórz nową maszynę Wirtualną w istniejącej grupie Resrouce lub wybierz **Utwórz nowy** Aby utworzyć nową grupę zasobów do przechowywania maszyny Wirtualnej.
6. Wybierz rozmiar maszyny wirtualnej. Aby wyświetlić więcej rozmiarów, wybierz pozycje **Wyświetl wszystkie** lub zmień filtr **Obsługiwany typ dysku**. 
7. W obszarze **ustawienia**, wprowadź wymagane zmiany i kliknij przycisk **OK**. 
8. Na stronie Podsumowanie, powinny pojawić się nazwę obrazu dla **prywatnej obrazu**. Kliknij przycisk **Ok** rozpocząć wdrażanie maszyny wirtualnej.


## <a name="use-powershell"></a>Korzystanie z programu PowerShell

### <a name="prerequisites"></a>Wymagania wstępne

Upewnij się, że najnowsze wersje modułów AzureRM.Compute i AzureRM.Network programu PowerShell. Otwórz wiersz programu PowerShell jako Administrator i uruchom następujące polecenie, aby je zainstalować.

```azurepowershell-interactive
Install-Module AzureRM.Compute,AzureRM.Network
```
Aby uzyskać więcej informacji, zobacz [przechowywanie wersji programu Azure PowerShell](/powershell/azure/overview).



### <a name="collect-information-about-the-image"></a>Zbieranie informacji o obrazie

Najpierw musimy zebrać podstawowe informacje o obrazie i utworzyć zmienną dla obrazu. W tym przykładzie użyto do zarządzanego obrazu maszyny Wirtualnej o nazwie **myImage** w **myResourceGroup** grupy zasobów w **zachodnie centralnej nam** lokalizacji. 

```azurepowershell-interactive
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

### <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej
Utwórz sieć wirtualną i podsieć [sieci wirtualnej](../../virtual-network/virtual-networks-overview.md).

Utwórz podsieć. W tym przykładzie tworzy podsieć o nazwie **mySubnet** z prefiksem adresu o **10.0.0.0/24**.  
   
```azurepowershell-interactive
$subnetName = "mySubnet"
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig `
    -Name $subnetName -AddressPrefix 10.0.0.0/24
```

Utwórz sieć wirtualną. W tym przykładzie tworzy sieć wirtualną o nazwie **myVnet** z prefiksem adresu o **10.0.0.0/16**.  
   
```azurepowershell-interactive
$vnetName = "myVnet"
$vnet = New-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $rgName `
    -Location $location `
    -AddressPrefix 10.0.0.0/16 `
    -Subnet $singleSubnet
```    

### <a name="create-a-public-ip-address-and-network-interface"></a>Tworzenie publicznego adresu IP adres i interfejsu sieciowego

Aby umożliwić komunikację z maszyną wirtualną w sieci wirtualnej, potrzebujesz [publicznego adresu IP](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) i interfejsu sieciowego.

Utwórz publiczny adres IP. W tym przykładzie jest tworzony publiczny adres IP o nazwie **myPip**. 
   
```azurepowershell-interactive
$ipName = "myPip"
$pip = New-AzureRmPublicIpAddress `
    -Name $ipName `
    -ResourceGroupName $rgName `
    -Location $location `
    -AllocationMethod Dynamic
```
       
Utwórz kartę sieciową. W tym przykładzie jest tworzony karty Sieciowej o nazwie **myNic**. 
   
```azurepowershell-interactive
$nicName = "myNic"
$nic = New-AzureRmNetworkInterface `
    -Name $nicName `
    -ResourceGroupName $rgName `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id
```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Tworzenie grupy zabezpieczeń sieci i reguły protokołu RDP

Aby móc zalogować się do maszyny Wirtualnej za pomocą protokołu RDP, musisz mieć reguły zabezpieczeń sieci (NSG), umożliwiająca dostęp RDP do portu 3389. 

W tym przykładzie tworzy grupy NSG o nazwie **myNsg** zawierający regułę o nazwie **myRdpRule** za pośrednictwem portu 3389 która zezwala na ruch RDP. Aby uzyskać więcej informacji na temat grup NSG, zobacz [Otwieranie portów dla maszyny Wirtualnej na platformie Azure przy użyciu programu PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```azurepowershell-interactive
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-the-virtual-network"></a>Utwórz zmienną dla sieci wirtualnej

Utwórz zmienną dla ukończonych sieci wirtualnej. 

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

### <a name="get-the-credentials-for-the-vm"></a>Uzyskiwanie poświadczeń dla maszyny Wirtualnej

Następujące polecenie cmdlet zostanie otwarte okno gdzie będą wprowadź nową nazwę użytkownika i hasło do użycia jako konto administratora lokalnego na zdalny dostęp do maszyny Wirtualnej. 

```azurepowershell-interactive
$cred = Get-Credential
```

### <a name="set-variables-for-the-vm-name-computer-name-and-the-size-of-the-vm"></a>Ustaw zmienne nazwę maszyny Wirtualnej, nazwy komputera i rozmiaru maszyny Wirtualnej

Utwórz zmienne dla nazwy komputera i nazwę maszyny Wirtualnej. W tym przykładzie nazwa maszyny Wirtualnej jako **myVM** i nazwę komputera, jako **Mój komputer**.

```azurepowershell-interactive
$vmName = "myVM"
$computerName = "myComputer"
```

Ustaw rozmiar maszyny wirtualnej. Ten przykład tworzy **Standard_DS1_v2** rozmiaru maszyny Wirtualnej. Zobacz [rozmiarów maszyn wirtualnych](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) dokumentacji, aby uzyskać więcej informacji.

```azurepowershell-interactive
$vmSize = "Standard_DS1_v2"
```

Dodaj nazwę maszyny Wirtualnej i rozmiar do konfiguracji maszyny Wirtualnej.

```azurepowershell-interactive
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

### <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a>Ustaw obraz maszyny Wirtualnej jako źródło obrazu dla nowej maszyny Wirtualnej

Ustaw obraz źródłowy przy użyciu Identyfikatora zarządzanego obrazu maszyny Wirtualnej.

```azurepowershell-interactive
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

### <a name="set-the-os-configuration-and-add-the-nic"></a>Ustawienia konfiguracji systemu operacyjnego i Dodaj kartę sieciową.

Wprowadź typ magazynu (PremiumLRS lub StandardLRS) i rozmiar dysku systemu operacyjnego. W tym przykładzie typ konta w **PremiumLRS**, rozmiar dysku do **128 GB** i buforowanie dysku, aby **ReadWrite**.

```azurepowershell-interactive
$vm = Set-AzureRmVMOSDisk -VM $vm  `
    -StorageAccountType PremiumLRS `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

### <a name="create-the-vm"></a>Tworzenie maszyny wirtualnej

Tworzenie nowej maszyny wirtualnej za pomocą konfiguracji, które firma Microsoft utworzone i przechowywane w **$vm** zmiennej.

```azurepowershell-interactive
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

### <a name="verify-that-the-vm-was-created"></a>Sprawdź, czy maszyna wirtualna została utworzona
Po zakończeniu powinien zostać wyświetlony nowo utworzony maszyny Wirtualnej w ramach [portalu Azure](https://portal.azure.com) w obszarze **Przeglądaj** > **maszyn wirtualnych**, lub za pomocą następujących poleceń programu PowerShell:

```azurepowershell-interactive
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a>Następne kroki
Aby zarządzać nową maszynę wirtualną w taki sposób, z programu Azure PowerShell, zobacz [tworzenia i zarządzania maszynami wirtualnymi systemu Windows za pomocą modułu Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

