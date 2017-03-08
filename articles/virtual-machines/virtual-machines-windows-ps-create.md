---
title: "Tworzenie maszyny wirtualnej platformy Azure przy użyciu programu PowerShell | Microsoft Docs"
description: "Za pomocą programu Azure PowerShell i usługi Azure Resource Manager można w łatwy sposób utworzyć maszynę wirtualną z systemem Windows Server."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 14fe9ca9-e228-4d3b-a5d8-3101e9478f6e
ms.service: virtual-machines-windows
ms.topic: get-started-article
ms.date: 02/14/2017
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 8d8dfb9b165d82e8567f6b5577d46d562f9f8db3
ms.openlocfilehash: 89e306d3e3312531878da088575c7429a941d34f
ms.lasthandoff: 02/23/2017

---

# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Tworzenie maszyny wirtualnej z systemem Windows przy użyciu usługi Resource Manager i programu PowerShell

W tym artykule pokazano, jak szybko utworzyć maszynę wirtualną platformy Azure z systemem Windows Server i wymagane przez nią zasoby przy użyciu usługi [Resource Manager](../azure-resource-manager/resource-group-overview.md) i programu Azure PowerShell.  

Wszystkie kroki przedstawione w tym artykule są wymagane do utworzenia maszyny wirtualnej, a ich wykonanie powinno zająć około 30 minut. Zamień przykładowe wartości parametrów w poleceniach nazwami odpowiednimi dla konkretnego środowiska.

## <a name="step-1-install-azure-powershell"></a>Krok 1. Instalowanie programu Azure PowerShell

Zobacz [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Jak zainstalować i skonfigurować program Azure PowerShell), aby uzyskać informacje na temat instalowania najnowszej wersji programu Azure PowerShell, wybierania subskrypcji i logowania się do konta.

> [!NOTE]
> Aby korzystać z funkcji opisanych w tym artykule, może być konieczne ponowne zainstalowanie programu Azure PowerShell. Funkcje usługi Managed Disks są dostępne w wersji 3.5 lub nowszej.
> 
> 

## <a name="step-2-create-a-resource-group"></a>Krok 2. Tworzenie grupy zasobów

Wszystkie zasoby muszą być zawarte w grupie zasobów, dlatego najpierw zajmiemy się jej tworzeniem.  

1. Pobierz listę dostępnych lokalizacji, w których można utworzyć zasoby.
   
    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```

2. Ustaw lokalizację dla zasobów. To polecenie ustawia lokalizację **centralus**.
   
    ```powershell
    $location = "centralus"
    ```

3. Utwórz grupę zasobów. To polecenie umożliwia utworzenie grupy zasobów o nazwie **myResourceGroup** w miejscu, które można ustawić.
   
    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```

## <a name="step-3-optional-create-a-storage-account"></a>Krok 3. (Opcjonalnie) tworzenie konta magazynu

Podczas tworzenia maszyny wirtualnej możesz obecnie wybrać, czy chcesz używać dysków [Azure Managed Disks](../storage/storage-managed-disks-overview.md), czy dysków niezarządzanych. W przypadku wyboru dysku niezarządzanego musisz utworzyć [konto magazynu](../storage/storage-introduction.md) w celu przechowywania wirtualnego dysku twardego używanego przez utworzoną maszynę wirtualną. Jeśli wybierzesz dysk zarządzany, konto magazynu nie jest wymagane. Nazwy kont usługi Storage muszą mieć długość od 3 do 24 znaków i mogą zawierać tylko cyfry i małe litery.

1. Przetestuj unikatowość nazwy konta magazynu. To polecenie sprawdza nazwę **myStorageAccount**.
   
    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
   
    Jeśli to polecenie zwróci wartość **True**, proponowana nazwa jest unikatowa na platformie Azure. 

2. Teraz utworzymy konto magazynu.
   
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup `
        -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```

## <a name="step-4-create-a-virtual-network"></a>Krok 4. Tworzenie sieci wirtualnej

Wszystkie maszyny wirtualne są częścią [sieci wirtualnej](../virtual-network/virtual-networks-overview.md).

1. Utwórz podsieć dla sieci wirtualnej. To polecenie tworzy podsieć o nazwie **mySubnet** z prefiksem adresu 10.0.0.0/24.
   
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```

2. Teraz utwórz sieć wirtualną. To polecenie tworzy sieć wirtualną o nazwie **myVnet** za pomocą utworzonej podsieci i z prefiksem adresu **10.0.0.0/16**.
   
    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup `
        -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```

## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Krok 5. Tworzenie publicznego adresu IP i interfejsu sieciowego

Aby umożliwić komunikację z maszyną wirtualną w sieci wirtualnej, potrzebujesz [publicznego adresu IP](../virtual-network/virtual-network-ip-addresses-overview-arm.md) i interfejsu sieciowego.

1. Utwórz publiczny adres IP. To polecenie tworzy publiczny adres IP o nazwie **myPublicIp** z metodą alokacji **Dynamic**.
   
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup `
        -Location $location -AllocationMethod Dynamic
    ```
2. Utwórz interfejs sieciowy. To polecenie umożliwia utworzenie interfejsu sieciowego o nazwie **myNIC**.
   
    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup `
        -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```

## <a name="step-6-create-a-virtual-machine"></a>Krok 6. Tworzenie maszyny wirtualnej

Teraz, gdy masz wszystkie elementy, możesz utworzyć maszynę wirtualną. Maszynę wirtualną możesz utworzyć przy użyciu [obrazu z witryny Marketplace](virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), [niestandardowego obrazu uogólnionego (przetworzonego przez program Sysprep)](virtual-machines-windows-create-vm-generalized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) lub [niestandardowego obrazu wyspecjalizowanego (nieprzetworzonego przez program Sysprep)](virtual-machines-windows-create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). W tym przykładzie użyto obrazu systemu Windows Server z witryny Marketplace. 

1. Uruchom polecenie służące do ustawiania nazwy konta administratora i hasła dla maszyny wirtualnej.

    ```powershell
    $cred = Get-Credential -Message "Type the name and password of the local administrator account."
    ```
   
    Hasło musi mieć długość od 12 do 123 znaków i musi zawierać co najmniej jedną małą literę, jedną wielką literę, jedną cyfrę i jeden znak specjalny.

2. Utwórz obiekt konfiguracji dla maszyny wirtualnej. To polecenie umożliwia utworzenie obiektu konfiguracji o nazwie **myVmConfig**, który definiuje nazwę i rozmiar maszyny wirtualnej.
   
    ```powershell
    $myVM = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
   
    Aby wyświetlić listę dostępnych rozmiarów maszyny wirtualnej, zobacz [Sizes for virtual machines in Azure](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Rozmiary maszyn wirtualnych na platformie Azure).

3. Skonfiguruj ustawienia systemu operacyjnego dla maszyny wirtualnej. To polecenie ustawia nazwę komputera, typ systemu operacyjnego oraz poświadczenia konta dla maszyny wirtualnej.
   
    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred `
        -ProvisionVMAgent -EnableAutoUpdate
    ```

4. Zdefiniuj obraz, którego należy użyć do aprowizowania maszyny wirtualnej. To polecenie definiuje obrazu systemu Windows Server, którego będzie używać maszyna wirtualna. 

    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```

5. Dodaj do konfiguracji utworzony interfejs sieciowy.
   
    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```

6. Jeśli korzystasz z dysku niezarządzanego, uruchom to polecenie, aby zdefiniować nazwę oraz lokalizację dysku twardego maszyny wirtualnej. W przeciwnym razie pomiń ten krok. Plik wirtualnego dysku twardego dla dysku niezarządzanego jest przechowywany w kontenerze. To polecenie tworzy dysk w kontenerze o nazwie **vhds/myOsDisk1.vhd** w utworzonym koncie magazynu.
   
    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```

7. Dodaj do konfiguracji maszyny wirtualnej informacje dotyczące systemu operacyjnego. To polecenie powoduje utworzenie dysku o nazwie **myOsDisk1**.
   
    Jeśli korzystasz z dysku zarządzanego, uruchom to polecenie, aby ustawić konfigurację dla dysku systemu operacyjnego:

    ```powershell
    $myVM = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -StorageAccountType PremiumLRS -DiskSizeInGB 128 -CreateOption FromImage -Caching ReadWrite
    ```

    Jeśli korzystasz z dysku niezarządzanego, uruchom to polecenie, aby ustawić konfigurację dla dysku systemu operacyjnego:

    ```powershell
    $myVM = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```

8. Na zakończenie utwórz maszynę wirtualną.
   
    ```powershell
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```

## <a name="next-steps"></a>Następne kroki

* Jeśli wystąpiły problemy dotyczące wdrożenia, następnym krokiem powinno być zapoznanie się z artykułem [Troubleshoot common Azure deployment errors with Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md) (Rozwiązywanie typowych problemów dotyczących wdrażania za pomocą usługi Azure Resource Manager)
* Aby dowiedzieć się, jak zarządzać utworzoną maszyną wirtualną, zobacz [Manage virtual machines using Azure Resource Manager and PowerShell](virtual-machines-windows-ps-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Zarządzanie maszynami wirtualnymi przy użyciu usługi Azure Resource Manager i programu PowerShell).
* Aby utworzyć maszynę wirtualną przy użyciu szablonu, skorzystaj z informacji podanych w artykule [Create a Windows virtual machine with a Resource Manager template](virtual-machines-windows-ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Tworzenie maszyny wirtualnej systemu Windows przy użyciu szablonu usługi Resource Manager).


