---
title: "Uzyskuj dostęp do sieci wirtualnych sieci wirtualnej komunikacji równorzędnej - PowerShell | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak połączyć sieci wirtualnej z sieci wirtualnej komunikacji równorzędnej."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: 
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/06/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: c7b3fa2b566ab02e7fb4a03055db83f1545895e8
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-powershell"></a>Uzyskuj dostęp do sieci wirtualnych sieci wirtualnej komunikacji równorzędnej przy użyciu programu PowerShell

Sieci wirtualne można połączyć ze sobą z sieci wirtualnej komunikacji równorzędnej. Po połączyć się za pomocą sieci wirtualnych, zasobów w obie sieci wirtualne są mogły komunikować się ze sobą, z tym samym opóźnienia i przepustowości tak, jakby był zasoby w tej samej sieci wirtualnej. W tym artykule opisano tworzenie i równorzędna dwie sieci wirtualne. Omawiane kwestie:

> [!div class="checklist"]
> * Utwórz dwie sieci wirtualne
> * Utwórz komunikacji równorzędnej między sieciami wirtualnymi
> * Komunikacja równorzędna testu

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Jeśli użytkownik chce zainstalować i używać środowiska PowerShell lokalnie, w tym artykule wymaga programu Azure PowerShell w wersji modułu 3,6 lub nowszej. Uruchom ` Get-Module -ListAvailable AzureRM` można odnaleźć zainstalowanej wersji. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure. 

## <a name="create-virtual-networks"></a>Tworzenie sieci wirtualnych

Przed utworzeniem sieci wirtualnej, należy utworzyć grupę zasobów dla sieci wirtualnej i innych zasobów utworzone w tym artykule. Utwórz nową grupę zasobów o [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Utwórz sieć wirtualną przy użyciu polecenia [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Poniższy przykład tworzy sieć wirtualną o nazwie *myVirtualNetwork1* z prefiksem adresu *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork1 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork1 `
  -AddressPrefix 10.0.0.0/16
```

Tworzenie konfiguracji podsieci z [AzureRmVirtualNetworkSubnetConfig nowy](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Poniższy przykład tworzy konfiguracji podsieci o prefiksie adresu 10.0.0.0/24:

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork1
```

Zapisać konfiguracji podsieci do sieci wirtualnej z [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), która tworzy podsieć:

```azurepowershell-interactive
$virtualNetwork1 | Set-AzureRmVirtualNetwork
```

Tworzenie sieci wirtualnej z prefiksu adresu 10.1.0.0/16 i jedną podsieć:

```azurepowershell-interactive
# Create the virtual network.
$virtualNetwork2 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork2 `
  -AddressPrefix 10.1.0.0/16

# Create the subnet configuration.
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork2

# Write the subnet configuration to the virtual network.
$virtualNetwork2 | Set-AzureRmVirtualNetwork
```

Prefiks adresu *myVirtualNetwork2* sieci wirtualnej nakłada się na prefiksie adresu z *myVirtualNetwork1* sieci wirtualnej. Nie można elementów równorzędnych sieci wirtualnych o nakładających się prefiksów adresów.

## <a name="peer-virtual-networks"></a>Sieci wirtualne elementów równorzędnych

Utwórz komunikacji równorzędnej z [AzureRmVirtualNetworkPeering Dodaj](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering). Następujące elementy równorzędne przykład *myVirtualNetwork1* do *myVirtualNetwork2*.

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork1-myVirtualNetwork2 `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id
```

W danych wyjściowych zwrócona po wykonaniu poprzedniego polecenia, zobacz który **PeeringState** jest *zainicjowano*. Komunikacji równorzędnej pozostaje w *zainicjowano* stanu, dopóki nie zostaną utworzone komunikacji równorzędnej z *myVirtualNetwork2* do *myVirtualNetwork1*. Utwórz komunikacji równorzędnej z *myVirtualNetwork2* do *myVirtualNetwork1*. 

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork2-myVirtualNetwork1 `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id
```

W danych wyjściowych zwrócona po wykonaniu poprzedniego polecenia, zobacz który **PeeringState** jest *połączony*. Azure również zmienić stan komunikacji równorzędnej *myVirtualNetwork1 myVirtualNetwork2* komunikacji równorzędnej do *połączony*. Upewnij się, że stan komunikacji równorzędnej *myVirtualNetwork1 myVirtualNetwork2* równorzędna zmieniony na *połączony* z [Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering).

```azurepowershell-interactive
Get-AzureRmVirtualNetworkPeering `
  -ResourceGroupName myResourceGroup `
  -VirtualNetworkName myVirtualNetwork1 `
  | Select PeeringState
```

Zasoby w jednej sieci wirtualnej nie może komunikować się z zasobami w innych sieci wirtualnej do **PeeringState** dla komunikacji równorzędnych w obu sieci wirtualnych jest *połączony*. 

Komunikacji równorzędnych są między dwiema sieciami wirtualnymi, ale nie są przechodnie. Tak, na przykład, jeśli chcesz także elementu równorzędnego *myVirtualNetwork2* do *myVirtualNetwork3*, należy utworzyć dodatkowe komunikacji równorzędnej między sieciami wirtualnymi *myVirtualNetwork2* i *myVirtualNetwork3*. Mimo że *myVirtualNetwork1* jest połączyć za pomocą z *myVirtualNetwork2*, zasobów w ramach *myVirtualNetwork1* tylko dostęp do zasobów w  *myVirtualNetwork3* Jeśli *myVirtualNetwork1* została także połączyć za pomocą z *myVirtualNetwork3*. 

Przed równorzędna sieci wirtualnych w środowisku produkcyjnym, zalecane jest, że należy dokładnie zapoznać się z [Omówienie komunikacji równorzędnej](virtual-network-peering-overview.md), [Zarządzanie równorzędna](virtual-network-manage-peering.md), i [limity sieci wirtualnej ](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Chociaż w tym artykule przedstawiono komunikacji równorzędnej między dwie sieci wirtualne w tej samej subskrypcji i lokalizacji, również elementów równorzędnych sieci wirtualnych w [różnych regionach](#register) i [różnych subskrypcji Azure](create-peering-different-subscriptions.md#powershell). Można również utworzyć [gwiazdy sieci projektów](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) z komunikacji równorzędnej.

## <a name="test-peering"></a>Komunikacja równorzędna testu

Aby przetestować komunikację sieciową między maszynami wirtualnymi w różnych sieciach wirtualnych za pomocą komunikacji równorzędnej, Wdróż maszynę wirtualną w każdej podsieci, a następnie komunikować się między maszynami wirtualnymi. 

### <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Utwórz maszynę wirtualną w każdej sieci wirtualnej, aby móc weryfikować komunikację między nimi w kolejnym kroku.

Utwórz maszynę wirtualną z [AzureRmVM nowy](/powershell/module/azurerm.compute/new-azurermvm). Poniższy przykład tworzy maszynę wirtualną o nazwie *myVm1* w *myVirtualNetwork1* sieci wirtualnej. `-AsJob` Opcja tworzy maszynę wirtualną w tle, dzięki czemu można kontynuować do następnego kroku. Po wyświetleniu monitu wprowadź nazwę użytkownika i hasło, które chcesz zalogować się do maszyny wirtualnej z.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork1" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm1" `
  -AsJob
```

Azure automatycznie przypisuje 10.0.0.4 jako prywatny adres IP maszyny wirtualnej, ponieważ 10.0.0.4 jest pierwszy dostępny adres IP w *podsieć1* z *myVirtualNetwork1*. 

Utwórz maszynę wirtualną w *myVirtualNetwork2* sieci wirtualnej.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork2" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm2"
```

Maszyna wirtualna ma kilka minut na utworzenie. Chociaż w dane wyjściowe, Azure nieprzypisany 10.1.0.4 jako prywatny adres IP maszyny wirtualnej, ponieważ 10.1.0.4 jest pierwszy dostępny adres IP w *podsieć1* z *myVirtualNetwork2*. 

Nie Kontynuuj dalszych krokach dopóki Azure tworzy maszynę wirtualną i zwraca dane wyjściowe do programu PowerShell.

### <a name="test-virtual-machine-communication"></a>Testowanie łączności maszyny wirtualnej

Publiczny adres IP maszyny wirtualnej można połączyć z Internetu. Użyj [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) do zwrócenia publicznego adresu IP maszyny wirtualnej. Poniższy przykład zwraca publicznego adresu IP *myVm1* maszyny wirtualnej:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Użyj następującego polecenia, aby utworzyć sesję pulpitu zdalnego z *myVm1* maszynę wirtualną z komputera lokalnego. Zastąp `<publicIpAddress>` adres IP zwrócony z poprzednie polecenie.

```
mstsc /v:<publicIpAddress>
```

Plik protokołu Remote Desktop Protocol (RDP) jest utworzony, pobrana na komputer i otworzyć. Wprowadź nazwę użytkownika i hasło (musisz wybrać **więcej opcji**, następnie **korzystała z innego konta**, aby określić poświadczenia zostały wprowadzone podczas tworzenia maszyny wirtualnej), a następnie kliknij przycisk  **OK**. Podczas procesu logowania może pojawić się ostrzeżenie o certyfikacie. Kliknij przycisk **Tak** lub **Kontynuuj**, aby kontynuować nawiązywanie połączenia.

W wierszu polecenia, włącza ping przez zaporę systemu Windows, więc można zbadać poleceniem ping tę maszynę wirtualną z *myVm2* w kolejnym kroku.

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Chociaż ping jest używany podczas testowania w tym artykule, nie zaleca się stosowanie protokołu ICMP przez zaporę systemu Windows dla wdrożeń produkcyjnych.

Aby nawiązać połączenie *myVm2* maszyny wirtualnej, wprowadź następujące polecenie w wierszu polecenia *myVm1* maszyny wirtualnej:

```
mstsc /v:10.1.0.4
```

Ponieważ włączony ping *myVm1*, użytkownik może teraz wysyłać polecenia ping go za pomocą adresu IP z wiersza polecenia na *myVm2* maszyny wirtualnej:

```
ping 10.0.0.4
```

Otrzymasz cztery odpowiedzi. Jeśli wywołać według nazwy maszyny wirtualnej (*myVm1*), zamiast adresu IP ping nie powiedzie się, ponieważ *myVm1* jest nazwą Nieznany host. Rozpoznawanie nazw domyślne platformy Azure działa między maszynami wirtualnymi w tej samej sieci wirtualnej, ale nie między maszynami wirtualnymi w różnych sieciach wirtualnych. Rozpoznawanie nazw w sieciach wirtualnych, należy najpierw [wdrożenia serwera DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md) lub użyj [prywatnej domen usługi Azure DNS](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Odłączyć swoje sesje protokołu RDP do obu *myVm1* i *myVm2*.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, użyj [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) można usunąć grupy zasobów i wszystkie zasoby zawiera.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

**<a name="register"></a>Zarejestrować w wersji zapoznawczej komunikacji równorzędnej globalnej sieci wirtualnej**

Łączenie sieci wirtualnych za pomocą komunikacji równorzędnej w tym samym regionie jest ogólnie dostępne. Równorzędna sieci wirtualnych w różnych regionach jest obecnie w przeglądzie. Zobacz [aktualizacje sieci wirtualnej](https://azure.microsoft.com/updates/?product=virtual-network) dla dostępnych regionów. -To-peer sieci wirtualnych w regionach, najpierw należy zarejestrować w preview, wykonując następujące czynności (w ramach subskrypcji, w każdej sieci wirtualnej, który chcesz elementu równorzędnego jest w):

1. Zarejestruj subskrypcję, która każdej sieci wirtualnej, który chcesz elementu równorzędnego jest w wersji zapoznawczej, wprowadzając następujące polecenia:

    ```powershell-interactive
    Register-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
2. Upewnij się, że są zarejestrowane dla wersji zapoznawczej, wprowadzając następujące polecenie:

    ```powershell-interactive    
    Get-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    ```

    Jeśli spróbujesz elementów równorzędnych sieci wirtualnych w różnych regionach przed **RegistrationState** dane wyjściowe po wprowadzeniu poprzedniego polecenia jest wyświetlany **zarejestrowanej** obu subskrypcji, równorzędna kończy się niepowodzeniem .

## <a name="next-steps"></a>Kolejne kroki

W tym artykule przedstawiono sposób połączyć dwie sieci z sieci wirtualnej komunikacji równorzędnej. Możesz [połączyć swojego komputera do sieci wirtualnej](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) za pośrednictwem sieci VPN i interakcji z zasobami w sieci wirtualnej lub połączyć za pomocą sieci wirtualnych.

Nadal przykłady skryptów dla skryptów wielokrotnego użytku do wykonania wielu zadań omówione w artykułach sieci wirtualnej.

> [!div class="nextstepaction"]
> [Przykłady skryptów sieci wirtualnej](../networking/powershell-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
