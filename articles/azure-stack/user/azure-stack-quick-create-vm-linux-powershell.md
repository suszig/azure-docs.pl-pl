---
title: Utwórz maszynę wirtualną systemu Linux przy użyciu programu PowerShell w stosie Azure | Dokumentacja firmy Microsoft
description: Utwórz maszynę wirtualną systemu Linux przy użyciu programu PowerShell w stosie Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 03EE5929-4F05-47D7-B246-EA93D6FC47CD
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 5446f00b698fbe1fe1bae9c52bf3e73fe0d1c506
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="create-a-linux-virtual-machine-by-using-powershell-in-azure-stack"></a>Utwórz maszynę wirtualną systemu Linux przy użyciu programu PowerShell w stosie Azure 

*Dotyczy: Azure stosu zintegrowane systemy*

Program Azure PowerShell służy do tworzenia i zarządzania nimi zasobu w stosie Azure z wiersza polecenia lub w skryptach.  Szczegóły tego przewodnika przy użyciu programu PowerShell, aby utworzyć maszynę wirtualną z systemem Ubuntu server w stosie usługi Azure.

## <a name="prerequisites"></a>Wymagania wstępne 

* Upewnij się, operatorem Azure stos został dodany obrazu "Ubuntu Server 16.04 LTS" do stosu Azure marketplace.  

* Stos Azure wymaga określonej wersji programu Azure PowerShell do tworzenia i zarządzania zasobami. Jeśli nie jest skonfigurowany do stosu Azure PowerShell, zaloguj się do [zestaw deweloperski](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), lub z systemem Windows klient zewnętrzny w przypadku [połączone za pośrednictwem sieci VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) i wykonaj kroki, aby [ Zainstaluj](azure-stack-powershell-install.md) i [skonfigurować](azure-stack-powershell-configure-user.md) środowiska PowerShell.    

* Klucz publiczny SSH o nazwie id_rsa.pub powinny zostać utworzone w katalogu .ssh profilu użytkownika systemu Windows. Aby uzyskać szczegółowe informacje na temat tworzenia kluczy SSH, zobacz [klucze tworzenie SSH w systemie Windows](../../virtual-machines/linux/ssh-from-windows.md).  

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Grupa zasobów jest kontenerem logicznym, do których stosu Azure wdrożone i zarządzane zasoby. W zestawie rozwoju lub system Azure stosu zintegrowane Uruchom następujący blok kodu, aby utworzyć grupę zasobów. Firma Microsoft zostały przypisane wartości wszystkich zmiennych w tym dokumencie, można używać ich jest lub przypisać inną wartość.

```powershell
# Create variables to store the location and resource group names.
$location = "local"
$ResourceGroupName = "myResourceGroup" 

New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location 
```

## <a name="create-storage-resources"></a>Utwórz zasoby magazynu

Utwórz konto magazynu i kontener magazynu do przechowywania obrazu Ubuntu Server 16.04 LTS.

```powershell
# Create variables to store the storage account name and the storage account SKU information
$StorageAccountName = "mystorageaccount"
$SkuName = "Standard_LRS"

# Create a new storage account
$StorageAccount = New-AzureRMStorageAccount `
  -Location $location `
  -ResourceGroupName $ResourceGroupName `
  -Type $SkuName `
  -Name $StorageAccountName

Set-AzureRmCurrentStorageAccount `
  -StorageAccountName $storageAccountName `
  -ResourceGroupName $resourceGroupName

# Create a storage container to store the virtual machine image
$containerName = 'osdisks'
$container = New-AzureStorageContainer `
  -Name $containerName `
  -Permission Blob 
```

## <a name="create-networking-resources"></a>Tworzenie zasobów sieciowych

Utwórz sieć wirtualną, podsieć i publiczny adres IP. Te zasoby są używane do zapewniania łączności sieciowej do maszyny wirtualnej.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name MyVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -AllocationMethod Static `
  -IdleTimeoutInMinutes 4 `
  -Name "mypublicdns$(Get-Random)"

```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>Tworzenie sieciowej grupy zabezpieczeń i reguły sieciowej grupy zabezpieczeń

Grupy zabezpieczeń sieci zabezpiecza maszyny wirtualnej za pomocą reguł ruchu przychodzącego i wychodzącego. Utwórz regułę ruchu przychodzącego dla portu 3389 przychodzących połączeń usług pulpitu zdalnego i regułę ruchu przychodzącego dla portu 80, aby zezwolić na ruch przychodzący sieci web.

```powershell
# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleSSH  -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location local `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

### <a name="create-a-network-card-for-the-virtual-machine"></a>Tworzenie karty sieciowej dla maszyny wirtualnej
Karta sieciowa łączy maszynę wirtualną z podsiecią, sieciową grupą zabezpieczeń i publicznym adresem IP.

```powershell
# Create a virtual network card and associate it with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
  -Name myNic `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id `
  -NetworkSecurityGroupId $nsg.Id 
```

## <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej
Utwórz konfigurację maszyny wirtualnej. Ta konfiguracja zawiera ustawienia, które są używane podczas wdrażania maszyny wirtualnej, takie jak obraz maszyny wirtualnej, rozmiar i konfiguracja uwierzytelniania.

```powershell
# Define a credential object.
$UserName='demouser'
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ($UserName, $securePassword)

# Create the virtual machine configuration object
$VmName = "VirtualMachinelatest"
$VmSize = "Standard_D1"
$VirtualMachine = New-AzureRmVMConfig `
  -VMName $VmName `
  -VMSize $VmSize 

$VirtualMachine = Set-AzureRmVMOperatingSystem `
  -VM $VirtualMachine `
  -Linux `
  -ComputerName "MainComputer" `
  -Credential $cred 

$VirtualMachine = Set-AzureRmVMSourceImage `
  -VM $VirtualMachine `
  -PublisherName "Canonical" `
  -Offer "UbuntuServer" `
  -Skus "16.04-LTS" `
  -Version "latest"

$osDiskName = "OsDisk"
$osDiskUri = '{0}vhds/{1}-{2}.vhd' -f `
  $StorageAccount.PrimaryEndpoints.Blob.ToString(),`
  $vmName.ToLower(), `
  $osDiskName

# Sets the operating system disk properties on a virtual machine. 
$VirtualMachine = Set-AzureRmVMOSDisk `
  -VM $VirtualMachine `
  -Name $osDiskName `
  -VhdUri $OsDiskUri `
  -CreateOption FromImage | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id 

# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"

# Adds the SSH Key to the virtual machine
Add-AzureRmVMSshPublicKey -VM $VirtualMachine `
 -KeyData $sshPublicKey `
 -Path "/home/azureuser/.ssh/authorized_keys"

# Create the virtual machine.
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
 -Location $location `
  -VM $VirtualMachine 
```

## <a name="connect-to-the-virtual-machine"></a>Nawiązywanie połączenia z maszyną wirtualną

Po zakończeniu wdrożenia utwórz połączenie SSH z maszyną wirtualną. Wróć do publicznego adresu IP maszyny wirtualnej za pomocą polecenia [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?view=azurermps-4.3.1).

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Z systemu przy użyciu protokołu SSH zainstalowany Użyj następującego polecenia do nawiązania połączenia z maszyną wirtualną. Jeśli pracujesz w systemie Windows, możesz użyć [Putty](http://www.putty.org/) do utworzenia połączenia.

```
ssh <Public IP Address>
```

Po wyświetleniu monitu, nazwę użytkownika logowania jest azureuser. Jeśli podczas tworzenia kluczy SSH wprowadzono hasło, należy je również wprowadzić tutaj.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Gdy nie są już potrzebne, można użyć [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup?view=azurermps-4.3.1) polecenie Usuń grupę zasobów maszyny Wirtualnej, i wszystkich powiązanych zasobów:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Kolejne kroki

W tym szybkiego startu wdrożeniu proste maszyny wirtualnej systemu Linux. Aby dowiedzieć się więcej o maszynach wirtualnych Azure stosu, nadal [zagadnienia dotyczące maszyn wirtualnych w stosie Azure](azure-stack-vm-considerations.md).
