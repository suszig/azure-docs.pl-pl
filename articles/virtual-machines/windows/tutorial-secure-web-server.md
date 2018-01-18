---
title: "Zabezpieczenie usług IIS z certyfikatów SSL na platformie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zabezpieczyć serwer sieci web usług IIS z certyfikatów SSL na maszynie Wirtualnej systemu Windows na platformie Azure"
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
ms.date: 07/14/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 43f06422e1120f1c3b2a9d9d5d4be515213c0937
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="secure-iis-web-server-with-ssl-certificates-on-a-windows-virtual-machine-in-azure"></a>Zabezpieczenia serwera sieci web usług IIS z certyfikatów SSL na maszynie wirtualnej systemu Windows na platformie Azure
Do zabezpieczania serwerów sieci web, certyfikat Secure Sockets Layer (SSL) może być używany do szyfrowania ruchu w sieci web. Te certyfikaty SSL mogą być przechowywane w usłudze Azure Key Vault i Zezwalaj wdrożeń zabezpieczonych certyfikatów na maszynach wirtualnych systemu Windows (VM) na platformie Azure. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie usługi Azure Key Vault
> * Generowanie lub Przekaż certyfikat do magazynu kluczy
> * Utwórz maszynę Wirtualną i zainstaluj serwer sieci web usług IIS
> * Wstaw certyfikat do maszyny Wirtualnej i skonfiguruj program IIS wraz z powiązaniem SSL

Dla tego samouczka jest wymagany moduł Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).


## <a name="overview"></a>Przegląd
Usługa Azure Key Vault zabezpiecza kluczy kryptograficznych i kluczy tajnych takich certyfikatów lub hasła. Key Vault ułatwia uprościć proces zarządzania certyfikatu i pozwala zachować kontrolę nad kluczami, które uzyskują dostęp do tych certyfikatów. Można utworzyć certyfikatu z podpisem własnym wewnątrz usługi Key Vault, lub Przekaż istniejących, zaufanego certyfikatu, który już następującą.

Zamiast przy użyciu niestandardowego obrazu maszyny Wirtualnej, który zawiera certyfikaty rozszerzania w, wstrzyknąć certyfikatów do uruchomionej maszyny Wirtualnej. Ten proces zapewnia, że najbardziej aktualne certyfikaty są zainstalowane na serwerze sieci web podczas wdrażania. Jeśli odnowić lub Zastąp certyfikat, masz nie można utworzyć nowego niestandardowego obrazu maszyny Wirtualnej. Najnowsze certyfikaty są automatycznie dodane podczas tworzenia dodatkowych maszyn wirtualnych. W trakcie całego nigdy nie certyfikatów pozostaw platformy Azure lub są widoczne w skrypcie, Historia wiersza polecenia lub szablonu.


## <a name="create-an-azure-key-vault"></a>Tworzenie usługi Azure Key Vault
Przed utworzeniem magazyn kluczy i certyfikatów, Utwórz nową grupę zasobów o [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroupSecureWeb* w *wschodnie stany USA* lokalizacji:

```powershell
$resourceGroup = "myResourceGroupSecureWeb"
$location = "East US"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

Następnie należy utworzyć magazyn kluczy o [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault). Każdy magazyn kluczy wymaga unikatowej nazwy i powinna być małe litery. Zastąp `<mykeyvault>` w poniższym przykładzie z własną unikatową nazwę usługi Key Vault:

```powershell
$keyvaultName="<mykeyvault>"
New-AzureRmKeyVault -VaultName $keyvaultName `
    -ResourceGroup $resourceGroup `
    -Location $location `
    -EnabledForDeployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Wygeneruj certyfikat i przechowywania w magazynie kluczy
W środowisku produkcyjnym, należy zaimportować prawidłowy certyfikat podpisane przez zaufanego dostawcę z [AzureKeyVaultCertificate importu](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate). W tym samouczku, w poniższym przykładzie pokazano, jak można wygenerować certyfikatu z podpisem własnym z [AzureKeyVaultCertificate Dodaj](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) używającą domyślne zasady certyfikatów z [AzureKeyVaultCertificatePolicy nowy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy). 

```powershell
$policy = New-AzureKeyVaultCertificatePolicy `
    -SubjectName "CN=www.contoso.com" `
    -SecretContentType "application/x-pkcs12" `
    -IssuerName Self `
    -ValidityInMonths 12

Add-AzureKeyVaultCertificate `
    -VaultName $keyvaultName `
    -Name "mycert" `
    -CertificatePolicy $policy 
```


## <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej
Ustaw nazwę użytkownika i hasło administratora dla maszyny Wirtualnej z [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Teraz możesz utworzyć maszynę Wirtualną z [AzureRmVM nowy](/powershell/module/azurerm.compute/new-azurermvm). Poniższy przykład tworzy składniki wymagane sieci wirtualnej, konfiguracja systemu operacyjnego, a następnie tworzy Maszynę wirtualną o nazwie *myVM*:

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myVnet" `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleRDP"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 443
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleWWW"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name "myNic" `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2" | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName "myVM" -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2016-Datacenter" -Version "latest" | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig

# Use the Custom Script Extension to install IIS
Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server -IncludeManagementTools"}'
```

Trwa kilka minut, aż do utworzenia maszyny Wirtualnej. Ostatnim krokiem używa niestandardowe rozszerzenie skryptu Azure w celu zainstalowania serwera sieci web usług IIS z [AzureRmVmExtension zestawu](/powershell/module/azurerm.compute/set-azurermvmextension).


## <a name="add-a-certificate-to-vm-from-key-vault"></a>Dodawanie certyfikatu do maszyny Wirtualnej z magazynu kluczy
Aby dodać certyfikat z magazynu kluczy do maszyny Wirtualnej, należy uzyskać identyfikator certyfikatu z [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret). Dodawanie certyfikatu do maszyny Wirtualnej z [AzureRmVMSecret Dodaj](/powershell/module/azurerm.compute/add-azurermvmsecret):

```powershell
$certURL=(Get-AzureKeyVaultSecret -VaultName $keyvaultName -Name "mycert").id

$vm=Get-AzureRMVM -ResourceGroupName $resourceGroup -Name "myVM"
$vaultId=(Get-AzureRmKeyVault -ResourceGroupName $resourceGroup -VaultName $keyVaultName).ResourceId
$vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $vaultId -CertificateStore "My" -CertificateUrl $certURL

Update-AzureRmVM -ResourceGroupName $resourceGroup -VM $vm
```


## <a name="configure-iis-to-use-the-certificate"></a>Konfigurowanie usług IIS do używania certyfikatu
Niestandardowe rozszerzenie skryptu ponownie, podając [AzureRmVMExtension zestaw](/powershell/module/azurerm.compute/set-azurermvmextension) można zaktualizować konfiguracji usług IIS. Ta aktualizacja dotyczy certyfikatu wprowadzonym z magazynu kluczy usług IIS i konfiguruje powiązanie sieci web:

```powershell
$PublicSettings = '{
    "fileUris":["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/secure-iis.ps1"],
    "commandToExecute":"powershell -ExecutionPolicy Unrestricted -File secure-iis.ps1"
}'

Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString $publicSettings
```


### <a name="test-the-secure-web-app"></a>Testowanie aplikacji sieci web bezpieczne
Publiczny adres IP maszyny wirtualnej z [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress). Poniższy przykład uzyskuje adres IP dla `myPublicIP` utworzony wcześniej:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName $resourceGroup -Name "myPublicIP" | select "IpAddress"
```

Teraz możesz otworzyć przeglądarkę sieci web i wprowadź `https://<myPublicIP>` na pasku adresu. Aby zaakceptować zabezpieczeń ostrzeżenie, jeśli używasz certyfikatu z podpisem własnym, wybierz **szczegóły** , a następnie **przejdź do strony sieci Web**:

![Zaakceptuj ostrzeżenie o zabezpieczeniach przeglądarki sieci web](./media/tutorial-secure-web-server/browser-warning.png)

Wyświetlane są następnie zabezpieczonej witryny sieci Web usług IIS, jak w poniższym przykładzie:

![Widok działa bezpiecznej witryny usług IIS](./media/tutorial-secure-web-server/secured-iis.png)


## <a name="next-steps"></a>Kolejne kroki

W tym samouczku serwer sieci web usług IIS jest zabezpieczony za pomocą certyfikatu SSL, przechowywane w usłudze Azure Key Vault. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie usługi Azure Key Vault
> * Generowanie lub Przekaż certyfikat do magazynu kluczy
> * Utwórz maszynę Wirtualną i zainstaluj serwer sieci web usług IIS
> * Wstaw certyfikat do maszyny Wirtualnej i skonfiguruj program IIS wraz z powiązaniem SSL

Wykonaj to łącze, aby wyświetlić przykłady skryptów wbudowanych maszyny wirtualnej.

> [!div class="nextstepaction"]
> [Przykłady skryptów maszyny wirtualnej systemu Windows](./powershell-samples.md)
