---
title: "Przekaż uogólniony wirtualny dysk twardy do przykładowy skrypt programu PowerShell Azure | Dokumentacja firmy Microsoft"
description: "Przykładowy skrypt programu PowerShell do przekazywania uogólniony wirtualny dysk twardy na platformie Azure i utworzyć nową maszynę Wirtualną przy użyciu modelu wdrażania Menedżera zasobów i dysków zarządzanych."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/18/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: cd3d87bb4384971e28d3330cd5c1a3d351129036
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="sample-script-to-upload-a-vhd-to-azure-and-create-a-new-vm"></a>Przykładowy skrypt do przekazania dysku VHD na platformie Azure i utworzyć nową maszynę Wirtualną

Ten skrypt ma plik VHD lokalnego z ogólnych maszyny Wirtualnej i przekazuje ją do platformy Azure, tworzy obraz dysku zarządzane i używa do tworzenia nowej maszyny Wirtualnej.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

```powershell
# Provide values for the variables
$resourceGroup = 'myResourceGroup'
$location = 'EastUS'
$storageaccount = 'mystorageaccount'
$storageType = 'Standard_LRS'
$containername = 'mycontainer'
$localPath = 'C:\Users\Public\Documents\Hyper-V\VHDs\generalized.vhd'
$vmName = 'myVM'
$imageName = 'myImage'
$vhdName = 'myUploadedVhd.vhd'
$diskSizeGB = '128'
$subnetName = 'mySubnet'
$vnetName = 'myVnet'
$ipName = 'myPip'
$nicName = 'myNic'
$nsgName = 'myNsg'
$ruleName = 'myRdpRule'
$vmName = 'myVM'
$computerName = 'myComputerName'
$vmSize = 'Standard_DS1_v2'

# Get the username and password to be used for the administrators account on the VM. 
# This is used when connecting to the VM using RDP.

$cred = Get-Credential

# Upload the VHD
New-AzureRmResourceGroup -Name $resourceGroup -Location $location
New-AzureRmStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccount -Location $location `
    -SkuName $storageType -Kind "Storage"
$urlOfUploadedImageVhd = ('https://' + $storageaccount + '.blob.core.windows.net/' + $containername + '/' + $vhdName)
Add-AzureRmVhd -ResourceGroupName $resourceGroup -Destination $urlOfUploadedImageVhd `
    -LocalFilePath $localPath

# Note: Uploading the VHD may take awhile!

# Create a managed image from the uploaded VHD 
$imageConfig = New-AzureRmImageConfig -Location $location
$imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized `
    -BlobUri $urlOfUploadedImageVhd
$image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $resourceGroup -Image $imageConfig
 
# Create the networking resources
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $resourceGroup -Location $location `
    -AllocationMethod Dynamic
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroup -Location $location `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description 'Allow RDP' -Access Allow `
    -Protocol Tcp -Direction Inbound -Priority 110 -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName

# Start building the VM configuration
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

# Set the VM image as source image for the new VM
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id

# Finish the VM configuration and add the NIC.
$vm = Set-AzureRmVMOSDisk -VM $vm  -DiskSizeInGB $diskSizeGB -CreateOption FromImage -Caching ReadWrite
$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

# Create the VM
New-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup -Location $location

# Verify that the VM was created
$vmList = Get-AzureRmVM -ResourceGroupName $resourceGroup
$vmList.Name


```


<!-- 
[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-iis/create-windows-vm-iis.ps1 "Create VM IIS")] -->

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń w celu utworzenia wdrożenia. Każdy element w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie                                                                                                             | Uwagi                                                                                                                                                                                |
|---------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Nowe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)                           | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby.                                                                                                                          |
| [Nowe AzureRmStorageAccount](/powershell/module/azurerm.resources/new-azurermstorageaccount)                         | Tworzy konto magazynu.                                                                                                                                                           |
| [Dodaj AzureRmVhd](/powershell/module/azurerm.resources/add-azurermvhd)                                               | Przekazywanie wirtualnego dysku twardego z lokalnej maszyny wirtualnej do obiektu blob w konta magazynu w chmurze na platformie Azure.                                                                       |
| [Nowe AzureRmImageConfig](/powershell/module/azurerm.resources/new-azurermimageconfig)                               | Tworzy obiekt obrazu można konfigurować.                                                                                                                                                 |
| [Zestaw AzureRmImageOsDisk](/powershell/module/azurerm.resources/set-azurermimageosdisk)                               | Ustawia właściwości dysku systemu operacyjnego, w obiekcie image.                                                                                                                        |
| [Nowe AzureRmImage](/powershell/module/azurerm.resources/new-azurermimage)                                           | Tworzy nowy obraz.                                                                                                                                                                 |
| [Nowe AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.resources/new-azurermvirtualnetworksubnetconfig) | Tworzy konfiguracji podsieci. Ta konfiguracja jest używana z procesu tworzenia sieci wirtualnej.                                                                                |
| [Nowy AzureRmVirtualNetwork](/powershell/module/azurerm.resources/new-azurermvirtualnetwork)                         | Tworzy sieć wirtualną.                                                                                                                                                           |
| [Nowe AzureRmPublicIpAddress](/powershell/module/azurerm.resources/new-azurermpublicipaddress)                       | Tworzy publiczny adres IP.                                                                                                                                                         |
| [Nowe AzureRmNetworkInterface](/powershell/module/azurerm.resources/new-azurermnetworkinterface)                     | Tworzy interfejs sieciowy.                                                                                                                                                         |
| [Nowe AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermnetworksecurityruleconfig)   | Tworzy konfiguracji reguły grupy zabezpieczeń sieci. Ta konfiguracja jest używana do utworzenia reguły NSG, gdy grupa NSG jest tworzona.                                                       |
| [Nowe AzureRmNetworkSecurityGroup](/powershell/module/azurerm.resources/new-azurermnetworksecuritygroup)             | Tworzy sieciową grupę zabezpieczeń.                                                                                                                                                    |
| [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.resources/get-azurermvirtualnetwork)                         | Pobiera sieć wirtualną w grupie zasobów.                                                                                                                                          |
| [Nowe AzureRmVMConfig](/powershell/module/azurerm.resources/new-azurermvmconfig)                                     | Tworzy konfiguracji maszyny Wirtualnej. Ta konfiguracja zawiera informacje, takie jak nazwa maszyny Wirtualnej, system operacyjny i poświadczenia administracyjne. Konfiguracja jest używany podczas tworzenia maszyny Wirtualnej. |
| [Zestaw AzureRmVMSourceImage](/powershell/module/azurerm.resources/set-azurermvmsourceimage)                           | Określa obraz maszyny wirtualnej.                                                                                                                                            |
| [Zestaw AzureRmVMOSDisk](/powershell/module/azurerm.resources/set-azurermvmosdisk)                                     | Ustawia właściwości dysku systemu operacyjnego, na maszynie wirtualnej.                                                                                                                      |
| [Zestaw AzureRmVMOperatingSystem](/powershell/module/azurerm.resources/set-azurermvmoperatingsystem)                   | Ustawia właściwości dysku systemu operacyjnego, na maszynie wirtualnej.                                                                                                                      |
| [Dodaj AzureRmVMNetworkInterface](/powershell/module/azurerm.resources/add-azurermvmnetworkinterface)                 | Dodaje interfejs sieciowy do maszyny wirtualnej.                                                                                                                                       |
| [Nowe AzureRmVM](/powershell/module/azurerm.resources/new-azurermvm)                                                 | Utwórz maszynę wirtualną.                                                                                                                                                            |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)                     | Usuwa grupę zasobów i wszystkie zasoby zawarte w ciągu.                                                                                                                         |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Przykłady skryptów PowerShell dodatkowe maszyny wirtualnej znajdują się w [dokumentacji maszyny Wirtualnej systemu Windows Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
