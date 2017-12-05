---
title: Dostosowywanie systemu Windows maszyny Wirtualnej na platformie Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak dostosować maszyn wirtualnych systemu Windows na platformie Azure za pomocą niestandardowego rozszerzenia skryptu i magazyn kluczy"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: e0503cef234b01c4eefb1c1de3d88d9a812c4c39
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="how-to-customize-a-windows-virtual-machine-in-azure"></a>Dostosowywanie maszyny wirtualnej systemu Windows na platformie Azure
Aby skonfigurować maszynach wirtualnych (VM) w sposób szybki i spójny, wymagane jest zwykle jakiegoś automatyzacji. Typowym podejściem dostosować Maszynę wirtualną systemu Windows jest użycie [niestandardowe rozszerzenie skryptu systemu Windows](extensions-customscript.md). Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Użyj niestandardowe rozszerzenie skryptu, aby zainstalować usługi IIS
> * Utwórz maszynę Wirtualną, która używa niestandardowe rozszerzenie skryptu
> * Wyświetl uruchomione witryny usług IIS, po zastosowaniu rozszerzenia

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Jeśli postanowisz zainstalować program PowerShell i używać go lokalnie, ten samouczek wymaga modułu Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure. 


## <a name="custom-script-extension-overview"></a>Omówienie rozszerzenia niestandardowego skryptu
Niestandardowe rozszerzenie skryptu pobiera i uruchamia skrypty na maszynach wirtualnych Azure. To rozszerzenie jest przydatne w przypadku konfiguracji wdrożenia post, instalacja oprogramowania lub dowolnej innej konfiguracji / zadanie zarządzania. Skryptów można pobrać z magazynu Azure lub usługi GitHub lub dostarczone do portalu Azure pod adresem rozszerzenia czas wykonywania.

Rozszerzenie skryptu niestandardowego integruje się z szablonów usługi Azure Resource Manager i mogą być także uruchamiane przy użyciu wiersza polecenia platformy Azure, programu PowerShell, portalu Azure lub interfejsu API REST dla maszyny wirtualnej Azure.

Niestandardowe rozszerzenie skryptu służy w systemach Windows i maszyn wirtualnych systemu Linux.


## <a name="create-virtual-machine"></a>Tworzenie maszyny wirtualnej
Przed utworzeniem maszyny Wirtualnej, Utwórz nową grupę zasobów o [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroupAutomate* w *EastUS* lokalizacji:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupAutomate -Location EastUS
```

Ustaw nazwę użytkownika i hasło administratora dla maszyn wirtualnych o [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Teraz możesz utworzyć maszynę Wirtualną z [AzureRmVM nowy](/powershell/module/azurerm.compute/new-azurermvm). Poniższy przykład tworzy składniki wymagane sieci wirtualnej, konfiguracja systemu operacyjnego, a następnie tworzy Maszynę wirtualną o nazwie *myVM*:

```azurepowershell-interactive
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleWWW  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupAutomate `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create a virtual machine using the configuration
New-AzureRmVM -ResourceGroupName myResourceGroupAutomate -Location EastUS -VM $vmConfig
```

Trwa kilka minut, aż zasobów i maszyna wirtualna ma zostać utworzony.


## <a name="automate-iis-install"></a>Automatyzowanie instalacji usług IIS
Użyj [AzureRmVMExtension zestaw](/powershell/module/azurerm.compute/set-azurermvmextension) do zainstalowania niestandardowe rozszerzenie skryptu. Uruchamia rozszerzenia `powershell Add-WindowsFeature Web-Server` Aby zainstalować serwer sieci Web usług IIS, a następnie aktualizacje *Default.htm* stronę, aby wyświetlić nazwę hosta maszyny wirtualnej:

```azurepowershell-interactive
Set-AzureRmVMExtension -ResourceGroupName myResourceGroupAutomate `
    -ExtensionName IIS `
    -VMName myVM `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
```


## <a name="test-web-site"></a>Test witryny sieci web
Publiczny adres IP z usługi równoważenia obciążenia z [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Poniższy przykład uzyskuje adres IP dla *myPublicIP* utworzony wcześniej:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
    -ResourceGroupName myResourceGroupAutomate `
    -Name myPublicIP | select IpAddress
```

Następnie można wprowadzić publicznego adresu IP w przeglądarce sieci web. Witryna sieci Web jest wyświetlany, łącznie z nazwą hosta maszyny Wirtualnej dystrybuowanej usługi równoważenia obciążenia w ruchu, jak w poniższym przykładzie:

![Witryna sieci Web IIS uruchomiona](./media/tutorial-automate-vm-deployment/running-iis-website.png)


## <a name="next-steps"></a>Następne kroki

W tym samouczku można zautomatyzować instalacji usług IIS na maszynie Wirtualnej. W tym samouczku omówiono:

> [!div class="checklist"]
> * Użyj niestandardowe rozszerzenie skryptu, aby zainstalować usługi IIS
> * Utwórz maszynę Wirtualną, która używa niestandardowe rozszerzenie skryptu
> * Wyświetl uruchomione witryny usług IIS, po zastosowaniu rozszerzenia

Przejdź do samouczka dalej informacje na temat tworzenia niestandardowych obrazów maszyn wirtualnych.

> [!div class="nextstepaction"]
> [Tworzenie niestandardowych obrazów maszyn wirtualnych](./tutorial-custom-images.md)
