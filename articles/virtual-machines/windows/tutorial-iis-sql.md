---
title: "Tworzenie maszyn wirtualnych pracujących SQL &#92; IIS &#92;. Stos sieci na platformie Azure | Dokumentacja firmy Microsoft"
description: "Samouczek - install Azure SQL, usługi IIS, platformy .NET stosu na maszynach wirtualnych systemu Windows."
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
ms.date: 10/24/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 6533ab205e07243e2f757ea0a66028e1d140c52b
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="install-a-sql92iis92net-stack-in-azure"></a>Instalacja programu SQL &#92; IIS &#92;. Stos sieci na platformie Azure

W tym samouczku instalujemy program SQL &#92; IIS &#92;. Stos sieci przy użyciu programu Azure PowerShell. Ten stos składa się z dwóch maszyn wirtualnych z systemem Windows Server 2016 z usług IIS i platformy .NET, a druga z programem SQL Server.

> [!div class="checklist"]
> * Utwórz maszynę Wirtualną przy użyciu nowego AzVM
> * Zainstaluj usługi IIS i platformy .NET Core SDK w maszynie Wirtualnej
> * Tworzenie maszyny Wirtualnej z uruchomionym programem SQL Server
> * Zainstaluj rozszerzenie programu SQL Server

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Jeśli postanowisz zainstalować program PowerShell i używać go lokalnie, ten samouczek wymaga modułu Azure PowerShell w wersji 5.1.1 lub nowszej. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure.

## <a name="create-a-iis-vm"></a>Tworzenie maszyny Wirtualnej usług IIS 

W tym przykładzie używamy [AzVM nowy](https://www.powershellgallery.com/packages/AzureRM.Compute.Experiments) polecenia cmdlet w powłoce PowerShell chmury szybko utworzyć Maszynę wirtualną systemu Windows Server 2016, a następnie zainstalować usługi IIS i .NET Framework. Usług IIS i maszyn wirtualnych SQL udostępnianie grupy zasobów i sieci wirtualnej, dlatego utworzymy zmienne dla nazwy.

Polecenie **spróbuj on** przycisk, aby prawym górnym rogu bloku kodu do uruchomienia powłoki w chmurze w tym oknie. Użytkownik jest proszony o podanie poświadczeń dla maszyny wirtualnej w wierszu polecenia.

```azurepowershell-interactive
$vmName = "IISVM$(Get-Random)"
$vNetName = "myIISSQLvNet"
$resourceGroup = "myIISSQLGroup"
New-AzureRMVm -Name $vmName -ResourceGroupName $resourceGroup -VirtualNetworkName $vNetName 
```

Zainstaluj usługi IIS i .NET framework za pomocą niestandardowego rozszerzenia skryptu.

```azurepowershell-interactive

Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName IIS `
    -VMName $vmName `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features"}' `
    -Location EastUS
```

## <a name="azure-sql-vm"></a>Maszyna wirtualna platformy Azure SQL

Używamy obrazu wstępnie skonfigurowane witrynę Azure marketplace, programu SQL server do utworzenia maszyny Wirtualnej SQL. Najpierw utworzymy maszyny Wirtualnej, a następnie Trwa instalowanie rozszerzenia serwera SQL na maszynie Wirtualnej. 


```azurepowershell-interactive
# Create user object. You get a pop-up prompting you to enter the credentials for the VM.
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a subnet configuration
$vNet = Get-AzureRmVirtualNetwork -Name $vNetName -ResourceGroupName $resourceGroup
Add-AzureRmVirtualNetworkSubnetConfig -Name mySQLSubnet -VirtualNetwork $vNet -AddressPrefix "192.168.2.0/24"
Set-AzureRmVirtualNetwork -VirtualNetwork $vNet


# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location eastus `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow


# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location eastus `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name mySQLNic -ResourceGroupName $resourceGroup -Location eastus `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName mySQLVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName mySQLVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftSQLServer -Offer SQL2014SP2-WS2012R2 -Skus Enterprise -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create the VM
New-AzureRmVM -ResourceGroupName $resourceGroup -Location eastus -VM $vmConfig
```

Użyj [AzureRmVMSqlServerExtension zestaw](/powershell/module/azurerm.compute/set-azurermvmsqlserverextension) można dodać [rozszerzenia SQL Server](/sql/virtual-machines-windows-sql-server-agent-extension.md) do maszyny Wirtualnej SQL.

```azurepowershell-interactive
Set-AzureRmVMSqlServerExtension -ResourceGroupName $resourceGroup -VMName mySQLVM -name "SQLExtension"
```

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku został zainstalowany SQL &#92; IIS &#92;. Stos sieci przy użyciu programu Azure PowerShell. W tym samouczku omówiono:

> [!div class="checklist"]
> * Utwórz maszynę Wirtualną przy użyciu nowego AzVM
> * Zainstaluj usługi IIS i platformy .NET Core SDK w maszynie Wirtualnej
> * Tworzenie maszyny Wirtualnej z uruchomionym programem SQL Server
> * Zainstaluj rozszerzenie programu SQL Server

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak zabezpieczyć serwer sieci web usług IIS z certyfikatów SSL.

> [!div class="nextstepaction"]
> [Zabezpieczenia serwera sieci web usług IIS z certyfikatów SSL](tutorial-secure-web-server.md)

