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
ms.date: 02/27/2018
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 3a59d85ea19ba6670ffbb60aa9b764560a3567a0
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Zarządzanie sieciami wirtualnymi platformy Azure i maszyn wirtualnych z systemem Windows przy użyciu programu Azure PowerShell

Maszyny wirtualne platformy Azure korzystają z sieci platformy Azure do wewnętrznej i zewnętrznej komunikacji sieciowej. Ten samouczek przedstawia proces wdrażania dwóch maszyn wirtualnych i konfigurowania dla nich sieci platformy Azure. W przykładach w tym samouczku założono, że maszyny wirtualne hostują aplikację internetową z zapleczem bazy danych, jednak wdrożenie aplikacji nie jest omówione w samouczku. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie sieci wirtualnej i podsieci
> * Tworzenie publicznego adresu IP
> * Tworzenie maszyny wirtualnej frontonu
> * Zabezpieczanie ruchu sieciowego
> * Tworzenie maszyny wirtualnej zaplecza



Ten samouczek wymaga AzureRM.Compute wersji modułu 4.3.1 lub nowszym. Uruchom polecenie `Get-Module -ListAvailable AzureRM.Compute`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="vm-networking-overview"></a>Omówienie sieci maszyn wirtualnych

Sieci wirtualne platformy Azure umożliwiają nawiązywanie bezpiecznych połączeń sieciowych pomiędzy maszynami wirtualnymi, Internetem i innymi usługami platformy Azure, na przykład Azure SQL Database. Sieci wirtualne są podzielone na logiczne segmenty nazywane podsieciami. Podsieci są używane do sterowania przepływem sieciowym oraz pełnią funkcję granicy zabezpieczeń. Wdrażana maszyna wirtualna zwykle zawiera wirtualny interfejs sieciowy dołączony do podsieci.

Podczas pracy z tym samouczkiem zostaną utworzone następujące zasoby:

![Sieć wirtualna z dwiema podsieciami](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet* — sieć wirtualna, której maszyny wirtualne używają do komunikacji między sobą i z Internetem.
- *myFrontendSubnet* — podsieć w sieci *myVNet* używana przez zasoby frontonu.
- *myPublicIPAddress* — publiczny adres IP używany do uzyskania dostępu do maszyny *myFrontendVM* za pośrednictwem Internetu.
- *myFrontentNic* — interfejs sieciowy używany przez maszynę wirtualną *myFrontendVM* do komunikacji z maszyną *myBackendVM*.
- *myFrontendVM* — maszyna wirtualna używana do komunikacji pomiędzy Internetem a maszyną *myBackendVM*.
- *myBackendNSG* — sieciowa grupa zabezpieczeń, która kontroluje komunikację między maszyną *myFrontendVM* a maszyną *myBackendVM*.
- *myBackendSubnet* — podsieć skojarzona z grupą *myBackendNSG* i używana przez zasoby zaplecza.
- *myBackendNic* — interfejs sieciowy używany przez maszynę wirtualną *myBackendVM* do komunikacji z maszyną *myFrontendVM*.
- *myBackendVM* -maszynę Wirtualną, która używa portu 1433 do komunikowania się z *myFrontendVM*.


## <a name="create-a-virtual-network-and-subnet"></a>Tworzenie sieci wirtualnej i podsieci

W tym samouczku zostanie utworzona jedna sieć wirtualna z dwoma podsieciami. Zostanie utworzona podsieć frontonu do hostowania aplikacji internetowej oraz podsieć zaplecza do hostowania serwera bazy danych.

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

W ten sposób utworzono sieć i podzielono ją na dwie podsieci — jedną dla usług frontonu i jedną dla usług zaplecza. W kolejnej sekcji zostaną utworzone maszyny wirtualne połączone z tymi podsieciami.

## <a name="create-a-public-ip-address"></a>Tworzenie publicznego adresu IP

Publiczny adres IP umożliwia dostęp do zasobów platformy Azure w Internecie. Można skonfigurować dynamiczną lub statyczną alokację publicznego adresu IP. Domyślnie publiczny adres IP jest przydzielany dynamicznie. Dynamiczne adresy IP są zwalniane, gdy przydział maszyny wirtualnej zostaje cofnięty. Dlatego adres IP zmienia się podczas każdej operacji, która obejmuje cofnięcie przydziału maszyny wirtualnej.

Można też ustawić statyczną metodę alokacji, co gwarantuje, że adres IP pozostanie przydzielony do maszyny wirtualnej nawet wówczas, gdy jej przydział zostanie cofnięty. Podczas korzystania ze statycznie przydzielonych adresów IP nie można określić adresu IP. Zamiast tego jest on przydzielany z puli dostępnych adresów.

Utwórz publiczny adres IP o nazwie *myPublicIPAddress* przy użyciu [AzureRmPublicIpAddress nowy](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Dynamic `
  -Name myPublicIPAddress
```

Można zmienić parametru - AllocationMethod `Static` można przypisać statycznego publicznego adresu IP.

## <a name="create-a-front-end-vm"></a>Tworzenie maszyny wirtualnej frontonu

Dla maszyny Wirtualnej do komunikowania się w sieci wirtualnej musi on interfejs sieci wirtualnej (NIC). Tworzenie przy użyciu kart [AzureRmNetworkInterface nowy](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```azurepowershell-interactive
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontend `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Ustaw nazwę użytkownika i hasło potrzebne do konta administratora na maszynę Wirtualną przy użyciu [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential). Te poświadczenia umożliwiają połączenie z maszyną Wirtualną w dodatkowych czynności:

```azurepowershell-interactive
$cred = Get-Credential
```

Tworzenie maszyn wirtualnych przy użyciu [AzureRmVM nowy](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
New-AzureRmVM `
   -Credential $cred `
   -Name myFrontend `
   -PublicIpAddressName myPublicIPAddress `
   -ResourceGroupName myRGNetwork `
   -Location "EastUS" `
   -Size Standard_D1 `
   -SubnetName myFrontendSubnet `
   -VirtualNetworkName myVNet
```

## <a name="secure-network-traffic"></a>Zabezpieczanie ruchu sieciowego

Sieciowa grupa zabezpieczeń (NSG, network security group) zawiera listę reguł zabezpieczeń, które blokują lub zezwalają na ruch sieciowy do zasobów połączonych z usługami Azure Virtual Network (VNet). Sieciowe grupy zabezpieczeń można skojarzyć z podsieciami lub pojedynczymi interfejsami sieciowymi. Jeśli sieciowa grupa zabezpieczeń jest skojarzona z interfejsem sieciowym, jest stosowana wyłącznie do powiązanej maszyny wirtualnej. Jeśli sieciowa grupa zabezpieczeń jest skojarzona z podsiecią, te reguły są stosowane do wszystkich zasobów połączonych z tą podsiecią.

### <a name="network-security-group-rules"></a>Reguły sieciowych grup zabezpieczeń

Reguły sieciowych grup zabezpieczeń określają porty sieciowe, dla których ruch jest dozwolony lub niedozwolony. Reguły mogą zawierać zakresy źródłowych lub docelowych adresów IP, umożliwiając kontrolę ruchu pomiędzy określonymi systemami lub podsieciami. Reguły sieciowych grup zabezpieczeń mają również priorytet (od 1 do 4096). Priorytet określa kolejność oceny reguł. Reguła z priorytetem 100 jest oceniana przed regułą z priorytetem 200.

Wszystkie sieciowe grupy zabezpieczeń zawierają zestaw reguł domyślnych. Reguł domyślnych nie można usunąć, ale ponieważ mają przypisany najniższy priorytet, mogą być zastąpione przez tworzone zasady.

- **Sieć wirtualna** — ruch pochodzący z sieci wirtualnej i kończący się w niej jest dozwolony zarówno w kierunku przychodzącym, jak i wychodzącym.
- **Internet** — ruch wychodzący jest dozwolony, ale ruch przychodzący jest blokowany.
- **Moduł równoważenia obciążenia** — umożliwia modułowi równoważenia obciążenia platformy Azure badanie kondycji maszyn wirtualnych i wystąpień ról. Jeśli nie używasz zestawu z równoważeniem obciążenia, możesz przesłonić tę regułę.

### <a name="create-network-security-groups"></a>Tworzenie sieciowych grup zabezpieczeń

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

## <a name="create-a-back-end-vm"></a>Tworzenie maszyny wirtualnej zaplecza

Najprostszym sposobem tworzenia maszyny Wirtualnej zaplecza na potrzeby tego samouczka jest przy użyciu obrazu programu SQL Server. W tym samouczku tylko tworzy maszynę Wirtualną z serwerem bazy danych, ale nie dostarcza informacji na temat uzyskiwania dostępu do bazy danych.

Utwórz *myBackendNic*:

```azurepowershell-interactive
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackend `
  -SubnetId $vnet.Subnets[1].Id
```

Ustaw nazwę użytkownika i hasło potrzebne do konta administratora na Maszynie wirtualnej z Get-Credential:

```azurepowershell-interactive
$cred = Get-Credential
```

Utwórz *myBackendVM*.

```azurepowershell-interactive
New-AzureRmVM `
   -Credential $cred `
   -Name myBackend `
   -ImageName "MicrosoftSQLServer:SQL2016SP1-WS2016:Enterprise:latest" `
   -ResourceGroupName myRGNetwork `
   -Location "EastUS" `
   -SubnetName myFrontendSubnet `
   -VirtualNetworkName myVNet
```

Obraz, który jest użyty zainstalowany program SQL Server, ale nie jest używana w tym samouczku. Należy ono do wyświetlenia, jak można skonfigurować Maszynę wirtualną do obsługi ruchu w sieci web i maszyny Wirtualnej, aby obsłużyć Zarządzanie bazą danych.

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku utworzono i zabezpieczono sieci platformy Azure na potrzeby maszyn wirtualnych. 

> [!div class="checklist"]
> * Tworzenie sieci wirtualnej i podsieci
> * Tworzenie publicznego adresu IP
> * Tworzenie maszyny wirtualnej frontonu
> * Zabezpieczanie ruchu sieciowego
> * Tworzenie maszyny wirtualnej zaplecza

Przejdź do następnego samouczek, aby dowiedzieć się więcej o monitorowaniu Zabezpieczanie danych w przypadku maszyn wirtualnych przy użyciu kopii zapasowej systemu Azure.

> [!div class="nextstepaction"]
> [Tworzenie kopii zapasowych maszyn wirtualnych systemu Windows na platformie Azure](./tutorial-backup-vms.md)
