---
title: Sieci wirtualnych platformy Azure i maszyn wirtualnych z systemem Windows | Dokumentacja firmy Microsoft
description: "Samouczek — Zarządzanie sieciami wirtualnymi platformy Azure i maszyn wirtualnych z systemem Windows przy użyciu programu Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/12/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 21f2d586a4c468071bec55c65005b35baf323fe7
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Zarządzanie sieciami wirtualnymi platformy Azure i maszyn wirtualnych z systemem Windows przy użyciu programu Azure PowerShell

Maszyny wirtualne platformy Azure używania sieci platformy Azure do komunikacji sieciowej wewnętrznych i zewnętrznych. Ten samouczek przeprowadzi Cię przez wdrożenie dwóch maszyn wirtualnych i konfigurowanie sieci platformy Azure dla tych maszyn wirtualnych. Przykłady w tym samouczku założono, że maszyny wirtualne obsługujące aplikacji sieci web z bazy danych zaplecza, jednak aplikacja nie zostanie wdrożona w samouczku. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie sieci wirtualnej i podsieci
> * Tworzenie publicznego adresu IP
> * Tworzenie frontonu maszyny Wirtualnej
> * Zabezpieczanie ruchu sieciowego
> * Tworzenie maszyny Wirtualnej z zaplecza

Podczas wykonywania tego samouczka, można wyświetlić te zasoby utworzone:

![Sieci wirtualnej z dwoma podsieciami](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet* -sieć wirtualna, która maszyn wirtualnych używają do komunikowania się ze sobą i z Internetu.
- *myFrontendSubnet* -podsieci w *myVNet* używany przez zasoby frontonu.
- *myPublicIPAddress* -publiczny adres IP używany do dostępu *myFrontendVM* z Internetu.
- *myFrontentNic* -interfejs sieciowy używany przez *myFrontendVM* do komunikowania się z *myBackendVM*.
- *myFrontendVM* -maszyny Wirtualnej, używany do komunikacji między z Internetem i *myBackendVM*.
- *myBackendNSG* -grupy zabezpieczeń sieci, która kontroluje komunikację między *myFrontendVM* i *myBackendVM*.
- *myBackendSubnet* -podsieci skojarzone z *myBackendNSG* i używany przez zasoby zaplecza.
- *myBackendNic* -interfejs sieciowy używany przez *myBackendVM* do komunikowania się z *myFrontendVM*.
- *myBackendVM* -maszynę Wirtualną, która używa portu 1433 do komunikowania się z *myFrontendVM*.

Dla tego samouczka jest wymagany moduł Azure PowerShell w wersji 3.6 lub nowszej. Aby dowiedzieć się, jaka wersja jest używana, uruchom polecenie `Get-Module -ListAvailable AzureRM`. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="vm-networking-overview"></a>Omówienie sieci maszyny Wirtualnej

Sieci wirtualnych platformy Azure Włącz bezpiecznych połączeń sieci między maszynami wirtualnymi, internet i innymi usługami Azure, takich jak bazy danych Azure SQL. Sieci wirtualne są podzielone na segmentach logicznej podsieci. Podsieci są używane do sterowania przepływem sieci oraz funkcję granicy zabezpieczeń. Podczas wdrażania maszyny Wirtualnej, zwykle obejmuje interfejs sieci wirtualnej, który jest podłączony do podsieci.

## <a name="create-a-virtual-network-and-subnet"></a>Tworzenie sieci wirtualnej i podsieci

W tym samouczku jednej sieci wirtualnej jest tworzony z dwoma podsieciami. Podsieci frontonu do obsługi aplikacji sieci web, a podsieć zaplecza dla hostingu serwera bazy danych.

Przed utworzeniem sieci wirtualnej, Utwórz grupę zasobów za pomocą [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Poniższy przykład tworzy grupę zasobów o nazwie *myRGNetwork* w *EastUS* lokalizacji:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

### <a name="create-subnet-configurations"></a>Tworzenie konfiguracji podsieci

Tworzenie konfiguracji podsieci o nazwie *myFrontendSubnet* przy użyciu [AzureRmVirtualNetworkSubnetConfig nowy](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

I Tworzenie konfiguracji podsieci o nazwie *myBackendSubnet*:

```azurepowershell-interactive
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24
```

### <a name="create-virtual-network"></a>Tworzenie sieci wirtualnej

Tworzenie sieci Wirtualnej o nazwie *myVNet* przy użyciu *myFrontendSubnet* i *myBackendSubnet* przy użyciu [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```azurepowershell-interactive
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet, $backendSubnet
```

W tym momencie sieci zostało utworzone i podzielone na dwie podsieci, jeden dla usługi frontonu i drugi dla usług zaplecza. W następnej sekcji maszyny wirtualne są tworzone i połączone z tych podsieci.

## <a name="create-a-public-ip-address"></a>Tworzenie publicznego adresu IP

Publiczny adres IP umożliwia zasobów platformy Azure jako dostępny w Internecie. Metoda alokacji publicznego adresu IP można skonfigurować jako dynamicznej lub statycznej. Domyślnie jest dynamicznie przydzielane publicznego adresu IP. Dynamiczne adresy IP są wydawane po cofnięciu przydziału maszyny Wirtualnej. Ten problem powoduje, że adres IP zmienić podczas żadnej operacji, która obejmuje dezalokacji maszyny Wirtualnej.

Metoda alokacji można ustawić jako statyczny, który zapewnia, że adres IP jest przypisana do maszyny Wirtualnej, nawet podczas deallocated stanu. Korzystając z statycznie przydzielony adres IP, nie można określić adres IP. Zamiast tego jest ona przydzielone z puli dostępnych adresów.

Utwórz publiczny adres IP o nazwie *myPublicIPAddress* przy użyciu [AzureRmPublicIpAddress nowy](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Dynamic `
  -Name myPublicIPAddress
```

Można zmienić parametru - AllocationMethod `Static` można przypisać statycznego publicznego adresu IP.

## <a name="create-a-front-end-vm"></a>Tworzenie frontonu maszyny Wirtualnej

Dla maszyny Wirtualnej do komunikowania się w sieci wirtualnej musi on interfejs sieci wirtualnej (NIC). Tworzenie przy użyciu kart [AzureRmNetworkInterface nowy](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```azurepowershell-interactive
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Ustaw nazwę użytkownika i hasło potrzebne do konta administratora na maszynę Wirtualną przy użyciu [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential). Te poświadczenia umożliwiają połączenie z maszyną Wirtualną w dodatkowych czynności:

```azurepowershell-interactive
$cred = Get-Credential
```

Tworzenie maszyn wirtualnych przy użyciu [AzureRmVMConfig nowy](/powershell/module/azurerm.compute/new-azurermvmconfig), [AzureRmVMOperatingSystem zestaw](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem), [AzureRmVMSourceImage zestaw](/powershell/module/azurerm.compute/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk), [Dodać AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface), i [nowe AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

```azurepowershell-interactive
$frontendVM = New-AzureRmVMConfig `
    -VMName myFrontendVM `
    -VMSize Standard_D1
$frontendVM = Set-AzureRmVMOperatingSystem `
    -VM $frontendVM `
    -Windows `
    -ComputerName myFrontendVM `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$frontendVM = Set-AzureRmVMSourceImage `
    -VM $frontendVM `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
$frontendVM = Set-AzureRmVMOSDisk `
    -VM $frontendVM `
    -Name myFrontendOSDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
$frontendVM = Add-AzureRmVMNetworkInterface `
    -VM $frontendVM `
    -Id $frontendNic.Id
New-AzureRmVM `
    -ResourceGroupName myRGNetwork `
    -Location EastUS `
    -VM $frontendVM
```

## <a name="secure-network-traffic"></a>Zabezpieczanie ruchu sieciowego

Sieciowa grupa zabezpieczeń (NSG, network security group) zawiera listę reguł zabezpieczeń, które blokują lub zezwalają na ruch sieciowy do zasobów połączonych z usługami Azure Virtual Network (VNet). Grupy NSG można skojarzyć z podsieci lub sieci poszczególnych interfejsów. Grupa NSG jest skojarzona z karty sieciowej, ma zastosowanie tylko skojarzonego VM. Jeśli sieciowa grupa zabezpieczeń jest skojarzona z podsiecią, te reguły są stosowane do wszystkich zasobów połączonych z tą podsiecią.

### <a name="network-security-group-rules"></a>Reguły grupy zabezpieczeń sieci

Reguły NSG definiują portów sieciowych za pośrednictwem których ruch jest dozwolony lub niedozwolony. Zasady mogą obejmować źródłowe i docelowe zakresów adresów IP tak, aby ruch jest kontrolowany między konkretnych systemów lub podsieci. Reguły NSG obejmują także priorytet (od 1 — i 4096). Reguły są sprawdzane według ważności. Reguła o priorytecie 100 jest oceniane przed regułą z priorytetem 200.

Wszystkie sieciowe grupy zabezpieczeń zawierają zestaw reguł domyślnych. Reguł domyślnych nie można usunąć, ale ponieważ mają przypisany najniższy priorytet, mogą być zastąpione przez tworzone zasady.

- **Sieć wirtualna** — ruch pochodzący i kończenie w sieci wirtualnej jest dozwolony zarówno w kierunkach przychodzących i wychodzących.
- **Internet** — ruch wychodzący jest dozwolone, ale ruch przychodzący jest zablokowany.
- **Moduł równoważenia obciążenia** — umożliwia modułowi równoważenia obciążenia Azure badanie kondycji maszyn wirtualnych i wystąpień ról. Tę zasadę można zastąpić, jeśli nie używasz zestawu o zrównoważonym obciążeniu.

### <a name="create-network-security-groups"></a>Utwórz grupy zabezpieczeń sieci

Utwórz regułę ruchu przychodzącego o nazwie *myFrontendNSGRule* zezwalająca na ruch przychodzący sieci web na *myFrontendVM* przy użyciu [AzureRmNetworkSecurityRuleConfig nowy](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):

```azurepowershell-interactive
$nsgFrontendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myFrontendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 200 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow
```

Można ograniczyć ruch wewnętrzny *myBackendVM* tylko od *myFrontendVM* przez utworzenie grupy NSG podsieci wewnętrznej. Poniższy przykład tworzy reguły NSG o nazwie *myBackendNSGRule*:

```azurepowershell-interactive
$nsgBackendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myBackendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix 10.0.0.0/24 `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 1433 `
  -Access Allow
```

Dodaj sieciową grupę zabezpieczeń o nazwie *myFrontendNSG* przy użyciu [AzureRmNetworkSecurityGroup nowy](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```azurepowershell-interactive
$nsgFrontend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNSG `
  -SecurityRules $nsgFrontendRule
```

Teraz Dodaj sieciową grupę zabezpieczeń o nazwie *myBackendNSG* przy użyciu nowego AzureRmNetworkSecurityGroup:

```azurepowershell-interactive
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```

Dodaj grupy zabezpieczeń sieci z podsieciami:

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Name myVNet
$frontendSubnet = $vnet.Subnets[0]
$backendSubnet = $vnet.Subnets[1]
$frontendSubnetConfig = Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name myFrontendSubnet `
  -AddressPrefix $frontendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgFrontend
$backendSubnetConfig = Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name myBackendSubnet `
  -AddressPrefix $backendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgBackend
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

## <a name="create-a-back-end-vm"></a>Tworzenie maszyny Wirtualnej zaplecza

Najprostszym sposobem tworzenia maszyny Wirtualnej zaplecza na potrzeby tego samouczka jest przy użyciu obrazu programu SQL Server. W tym samouczku tylko tworzy maszynę Wirtualną z serwerem bazy danych, ale nie dostarcza informacji na temat uzyskiwania dostępu do bazy danych.

Utwórz *myBackendNic*:

```azurepowershell-interactive
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNic `
  -SubnetId $vnet.Subnets[1].Id
```

Ustaw nazwę użytkownika i hasło potrzebne do konta administratora na Maszynie wirtualnej z Get-Credential:

```azurepowershell-interactive
$cred = Get-Credential
```

Utwórz *myBackendVM*:

```azurepowershell-interactive
$backendVM = New-AzureRmVMConfig `
  -VMName myBackendVM `
  -VMSize Standard_D1
$backendVM = Set-AzureRmVMOperatingSystem `
  -VM $backendVM `
  -Windows `
  -ComputerName myBackendVM `
  -Credential $cred `
  -ProvisionVMAgent `
  -EnableAutoUpdate
$backendVM = Set-AzureRmVMSourceImage `
  -VM $backendVM `
  -PublisherName MicrosoftSQLServer `
  -Offer SQL2016SP1-WS2016 `
  -Skus Enterprise `
  -Version latest
$backendVM = Set-AzureRmVMOSDisk `
  -VM $backendVM `
  -Name myBackendOSDisk `
  -DiskSizeInGB 128 `
  -CreateOption FromImage `
  -Caching ReadWrite
$backendVM = Add-AzureRmVMNetworkInterface `
  -VM $backendVM `
  -Id $backendNic.Id
New-AzureRmVM `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -VM $backendVM
```

Obraz, który jest użyty zainstalowany program SQL Server, ale nie jest używana w tym samouczku. Należy ono do wyświetlenia, jak można skonfigurować Maszynę wirtualną do obsługi ruchu w sieci web i maszyny Wirtualnej, aby obsłużyć Zarządzanie bazą danych.

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostało utworzone i zabezpieczonej sieci platformy Azure w odniesieniu do maszyn wirtualnych. 

> [!div class="checklist"]
> * Tworzenie sieci wirtualnej i podsieci
> * Tworzenie publicznego adresu IP
> * Tworzenie frontonu maszyny Wirtualnej
> * Zabezpieczanie ruchu sieciowego
> * Tworzenie maszyny Wirtualnej zaplecza

Przejdź do następnego samouczek, aby dowiedzieć się więcej o monitorowaniu Zabezpieczanie danych w przypadku maszyn wirtualnych przy użyciu kopii zapasowej systemu Azure.

> [!div class="nextstepaction"]
> [Tworzenie kopii zapasowych maszyn wirtualnych systemu Windows na platformie Azure](./tutorial-backup-vms.md)
