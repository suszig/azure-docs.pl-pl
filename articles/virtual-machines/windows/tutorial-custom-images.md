---
title: "Tworzenie niestandardowych obrazów maszyn wirtualnych przy użyciu programu Azure PowerShell | Dokumentacja firmy Microsoft"
description: "Samouczek — Tworzenie niestandardowego obrazu maszyny Wirtualnej przy użyciu programu Azure PowerShell."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/07/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 7001e5df235d65c531b9102f879bde9693c4f853
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a>Tworzenie niestandardowego obrazu maszyny Wirtualnej platformy Azure przy użyciu programu PowerShell

Niestandardowe obrazy są takie jak obrazy marketplace, ale można je utworzyć samodzielnie. Niestandardowe obrazy może służyć do ładowania początkowego konfiguracje, takie jak wstępnego ładowania aplikacji, konfiguracji aplikacji i inne konfiguracje systemu operacyjnego. W tym samouczku utworzysz własnego niestandardowego obrazu maszyny wirtualnej platformy Azure. Omawiane kwestie:

> [!div class="checklist"]
> * Narzędzie Sysprep i generalize maszyny wirtualne
> * Tworzenie obrazu niestandardowego
> * Utwórz maszynę Wirtualną z obrazu niestandardowego
> * Wyświetlanie listy wszystkich obrazów w ramach subskrypcji
> * Usuń obraz

Dla tego samouczka jest wymagany moduł Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Poniższe kroki szczegółowo sposób pobrać istniejącej maszyny Wirtualnej i przekształcić ją do ponownego użycia niestandardowego obrazu używanego do utworzenia nowego wystąpienia maszyny Wirtualnej.

Aby ukończyć przykład, w tym samouczku, musi mieć istniejącej maszyny wirtualnej. Jeśli to konieczne, to [przykładowym skrypcie](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) można utworzyć. Podczas pracy nad samouczek, Zastąp maszyny Wirtualnej i grupy zasobów nazwy w razie potrzeby.

## <a name="prepare-vm"></a>Przygotowywanie maszyny Wirtualnej

Aby utworzyć obraz maszyny wirtualnej, należy przygotować maszyny Wirtualnej uogólnianie maszyny Wirtualnej, dealokowanie, a następnie oznaczenie źródłowej maszyny Wirtualnej, ponieważ uogólniony na platformie Azure.

### <a name="generalize-the-windows-vm-using-sysprep"></a>Maszyny Wirtualnej systemu Windows za pomocą programu Sysprep do uogólnienia

Program Sysprep usuwa wszystkie informacje osobiste konto, między innymi i przygotowuje komputer do użycia jako obraz. Aby uzyskać więcej informacji o narzędziu Sysprep, zobacz [sposobu użycia programu Sysprep: wprowadzenie](http://technet.microsoft.com/library/bb457073.aspx).


1. Połączenie z maszyną wirtualną.
2. Otwórz okno Wiersz polecenia jako administrator. Zmień katalog na *%windir%\system32\sysprep*, a następnie uruchom *sysprep.exe*.
3. W **narzędzie przygotowania systemu** okno dialogowe, wybierz opcję *wprowadź systemu Out-of-Box Experience (OOBE)*i upewnij się, że *Generalize* pole wyboru jest zaznaczone.
4. W **opcje zamykania**, wybierz pozycję *zamknięcia* , a następnie kliknij przycisk **OK**.
5. Po zakończeniu działania programu Sysprep, zamyka maszyny wirtualnej. **Nie uruchamiaj ponownie maszynę Wirtualną**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Cofnięcie przydziału i zostać oznaczone jako uogólniona maszyna wirtualna

Aby utworzyć obraz, maszyna wirtualna musi być alokację oraz oznaczony jako uogólniony na platformie Azure.

Cofnięcie przydziału maszynę Wirtualną przy użyciu [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Force
```

Ustaw stan maszyny wirtualnej na `-Generalized` przy użyciu [AzureRmVm zestawu](/powershell/module/azurerm.compute/set-azurermvm). 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Generalized
```


## <a name="create-the-image"></a>Tworzenie obrazu

Teraz można utworzyć obrazu maszyny wirtualnej za pomocą [AzureRmImageConfig nowy](/powershell/module/azurerm.compute/new-azurermimageconfig) i [AzureRmImage nowy](/powershell/module/azurerm.compute/new-azurermimage). Poniższy przykład tworzy obraz o nazwie *myImage* z maszyny Wirtualnej o nazwie *myVM*.

Przełącz maszynę wirtualną. 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
```

Utwórz konfigurację obrazu.

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Tworzenie obrazu.

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>Tworzenie maszyn wirtualnych z obrazu

Teraz, gdy masz obrazu można utworzyć jeden lub więcej nowych maszyn wirtualnych z obrazu. Tworzenie maszyny Wirtualnej z obrazu niestandardowego jest bardzo podobny do tworzenia maszyny Wirtualnej przy użyciu obrazu z witryny Marketplace. Korzystając z obrazu z witryny Marketplace, musisz podać informacje dotyczące obrazu, dostawca obrazu, oferty, jednostki SKU i wersji. Z niestandardowego obrazu wystarczy podać identyfikator zasobu obrazu niestandardowego. 

W poniższym skrypcie, możemy utworzyć zmienną *$image* do przechowywania informacji na temat używania niestandardowego obrazu [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) , a następnie używamy [AzureRmVMSourceImage zestaw](/powershell/module/azurerm.compute/set-azurermvmsourceimage) i określić za pomocą Identyfikatora *$image* zmiennej właśnie utworzyliśmy. 

Skrypt tworzy Maszynę wirtualną o nazwie *myVMfromImage* z naszych obraz niestandardowy w nowej grupy zasobów o nazwie *myResourceGroupFromImage* w *zachodnie stany USA* lokalizacji.


```powershell
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

New-AzureRmResourceGroup -Name myResourceGroupFromImage -Location EastUS

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name "mypublicdns$(Get-Random)" `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4

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
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$vmConfig = New-AzureRmVMConfig `
    -VMName myVMfromImage `
    -VMSize Standard_D1 | Set-AzureRmVMOperatingSystem -Windows `
        -ComputerName myComputer `
        -Credential $cred 

# Here is where we create a variable to store information about the image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroup

# Here is where we specify that we want to create the VM from and image and provide the image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a>Zarządzanie obrazami 

Oto kilka przykładów typowych zadań zarządzania dla obrazu i sposobu ich ukończenia przy użyciu programu PowerShell.

Wyświetl listę wszystkich obrazów według nazwy.

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Usuń obraz. W tym przykładzie usuwa obraz o nazwie *myOldImage* z *myResourceGroup*.

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku utworzony niestandardowy obraz maszyny Wirtualnej. W tym samouczku omówiono:

> [!div class="checklist"]
> * Narzędzie Sysprep i generalize maszyny wirtualne
> * Tworzenie obrazu niestandardowego
> * Utwórz maszynę Wirtualną z obrazu niestandardowego
> * Wyświetlanie listy wszystkich obrazów w ramach subskrypcji
> * Usuń obraz

Przejdź do następnego samouczkiem, aby poznać sposób wysokiej dostępności maszyny wirtualnej.

> [!div class="nextstepaction"]
> [Tworzenie maszyn wirtualnych o wysokiej dostępności](tutorial-availability-sets.md)



