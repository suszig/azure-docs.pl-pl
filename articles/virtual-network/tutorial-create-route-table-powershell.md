---
title: "Kierować ruchem sieciowym - PowerShell | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można kierować ruchem sieciowym z tabelą tras za pomocą programu PowerShell."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: f91b143c75a82aa6760796770b3ae4d0e4ec53dd
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>Kierować ruchem sieciowym z tabelą tras za pomocą programu PowerShell

Azure automatycznie tras ruchu między wszystkich podsieci w sieci wirtualnej, domyślnie. Można utworzyć trasy do przesłonięcia Azure routing domyślny. Możliwość tworzenia niestandardowych tras jest przydatne, jeśli na przykład chcesz kierować ruchem między podsieciami przez zaporę. W tym artykule dowiesz się, jak:

> [!div class="checklist"]
> * Utwórz tabelę tras
> * Utwórz trasę
> * Skojarz tabelę tras z podsiecią sieci wirtualnej
> * Routing testu
> * Rozwiązywanie problemów z routingiem

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Jeśli użytkownik chce zainstalować i używać środowiska PowerShell lokalnie, w tym artykule wymaga programu Azure PowerShell w wersji modułu 5.4.1 lub nowszym. Uruchom `Get-Module -ListAvailable AzureRM` można odnaleźć zainstalowanej wersji. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure. 

## <a name="create-a-route-table"></a>Utwórz tabelę tras

Azure tras ruchu między wszystkich podsieci w sieci wirtualnej, domyślnie. Aby dowiedzieć się więcej na temat trasy domyślne platformy Azure, zobacz [tras systemowych](virtual-networks-udr-overview.md). Aby zastąpić domyślny Azure routingu, Utwórz tabelę tras, która zawiera trasy i skojarz tabeli tras z podsiecią sieci wirtualnej.

Przed utworzeniem tabelę tras, Utwórz grupę zasobów o [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroup* dla wszystkich zasobów utworzone w tym artykule. 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Utwórz tabelę tras z [AzureRmRouteTable nowy](/powershell/module/azurerm.network/new-azurermroutetable). Poniższy przykład tworzy tabelę tras o nazwie *myRouteTablePublic*.

```azurepowershell-interactive
$routeTablePublic = New-AzureRmRouteTable `
  -Name 'myRouteTablePublic' `
  -ResourceGroupName myResourceGroup `
  -location EastUS
```

## <a name="create-a-route"></a>Utwórz trasę

Tabela tras zawiera zero lub więcej trasy. Tworzona jest trasa, pobierając obiekt tabeli tras z [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable), tworzona jest trasa z [AzureRmRouteConfig Dodaj](/powershell/module/azurerm.network/add-azurermrouteconfig), następnie zapisu konfiguracji tras na tabelę tras z [AzureRmRouteTable zestawu](/powershell/module/azurerm.network/set-azurermroutetable). 

```azurepowershell-interactive
Get-AzureRmRouteTable `
  -ResourceGroupName "myResourceGroup" `
  -Name "myRouteTablePublic" `
  | Add-AzureRmRouteConfig `
  -Name "ToPrivateSubnet" `
  -AddressPrefix 10.0.1.0/24 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress 10.0.2.4 `
 | Set-AzureRmRouteTable
```

Cały ruch kierowany do prefiksu adresu 10.0.1.0/24 przez urządzenie wirtualne sieci przy użyciu adresu IP 10.0.2.4 kieruje trasy. Urządzenie wirtualne sieci i podsieci z prefiksem adresu określonego są tworzone w kolejnych krokach. Trasy zastępuje domyślne Azure routingu, który kieruje ruchem między podsieciami bezpośrednio. Każda marszruta Określa typ następnego przeskoku. Typ następnego przeskoku nakazuje Azure sposób kierowania ruchu. W tym przykładzie Typ następnego przeskoku jest *VirtualAppliance*. Aby dowiedzieć się więcej na temat wszystkich dostępnych typów następnego przeskoku platformy Azure i kiedy należy używać ich, zobacz [następnego przeskoku typy](virtual-networks-udr-overview.md#custom-routes).

## <a name="associate-a-route-table-to-a-subnet"></a>Skojarz tabelę tras z podsiecią

Aby móc skojarzyć tabelę tras z podsiecią, należy utworzyć sieć wirtualną i podsieć. Utwórz sieć wirtualną przy użyciu polecenia [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Poniższy przykład tworzy sieć wirtualną o nazwie *myVirtualNetwork* z prefiksem adresu *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Utwórz trzy podsieci, tworząc trzy konfiguracje podsieci z [AzureRmVirtualNetworkSubnetConfig nowy](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Poniższy przykład tworzy trzy konfiguracje podsieci *publicznego*, *prywatnej*, i *DMZ* podsieci:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigDmz = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name DMZ `
  -AddressPrefix 10.0.2.0/24 `
  -VirtualNetwork $virtualNetwork
```

Prefiksy adresów muszą być w prefiksie adresu zdefiniowana dla sieci wirtualnej. Prefiksy adresów podsieci nie może nakładać się ze sobą.

Zapisać konfiguracje podsieci sieci wirtualnej z [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), co powoduje podsieci w sieci wirtualnej:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

Możesz skojarzyć tabelę tras na zero lub więcej podsieci. Podsieć może mieć zero lub jedną tabelę tras skojarzony. Ruch wychodzący z podsieci jest kierowany na podstawie trasy domyślne platformy Azure i niestandardowych tras, dodane do tabeli tras, który należy powiązać z podsiecią. Skojarz *myRouteTablePublic* tabeli tras do *publicznego* podsieć o [AzureRmVirtualNetworkSubnetConfig zestaw](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) , a następnie zapisać konfiguracji podsieci sieci wirtualnej z [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzureRmVirtualNetwork
```

Przed wdrożeniem tabel tras do użytku produkcyjnego, zaleca się, że należy dokładnie zapoznać się z [routing na platformie Azure](virtual-networks-udr-overview.md) i [limity Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="test-routing"></a>Routing testu

Aby przetestować routingu, należy utworzyć maszynę wirtualną, która służy jako urządzenie wirtualne sieci, że trasy, który został utworzony w poprzednim kroku kieruje za pośrednictwem. Po utworzeniu urządzenie wirtualne sieci, będzie wdrożyć maszynę wirtualną do *publicznego* i *prywatnej* podsieci. Następnie będzie kierować ruch z *publicznego* podsieci do *prywatnej* podsieci za pośrednictwem sieci urządzenia wirtualnego.

### <a name="create-a-network-virtual-appliance"></a>Utwórz urządzenie sieci wirtualnej

Maszynę wirtualną działającą aplikację sieciową jest często określany jako urządzenie wirtualne sieci. Urządzenie wirtualne sieci zazwyczaj odbieranie ruchu w sieci, wykonanie akcji, następnie do przodu lub porzucić ruchu w sieci opartych na logice skonfigurowane w aplikacji sieci. 

#### <a name="create-a-network-interface"></a>Tworzenie interfejsu sieciowego

W poprzednim kroku utworzono trasę określone urządzenie wirtualne sieci jako typ następnego przeskoku. Maszynę wirtualną działającą aplikację sieciową jest często określany jako urządzenie wirtualne sieci. W środowiskach produkcyjnych, którą należy wdrożyć urządzenie wirtualne sieci jest często wstępnie skonfigurowane maszyny wirtualnej. Wiele sieci wirtualnych urządzeń są dostępne z [portalu Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1). W tym artykule podstawowa maszyna wirtualna jest utworzona.

Maszyna wirtualna ma co najmniej jeden interfejsy sieciowe podłączone do niego, umożliwiających komunikowanie się z siecią maszyny wirtualnej. Dla karty sieciowej można było przesłać wysyłane do niego, ruch sieciowy, który nie jest przeznaczone do adresu IP, przesyłanie dalej IP musi być włączony dla interfejsu sieciowego. Przed utworzeniem karty sieciowej, należy pobrać wirtualnej sieci z identyfikatorem odpowiadającym [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork), następnie identyfikator podsieci z [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig). Tworzenie interfejsu sieciowego z [AzureRmNetworkInterface nowy](/powershell/module/azurerm.network/new-azurermnetworkinterface) w *DMZ* podsieci IP przesyłanie dalej:

```azurepowershell-interactive
# Retrieve the virtual network object into a variable.
$virtualNetwork=Get-AzureRmVirtualNetwork `
  -Name myVirtualNetwork `
  -ResourceGroupName myResourceGroup

# Retrieve the subnet configuration into a variable.
$subnetConfigDmz = Get-AzureRmVirtualNetworkSubnetConfig `
  -Name DMZ `
  -VirtualNetwork $virtualNetwork

# Create the network interface.
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name 'myVmNva' `
  -SubnetId $subnetConfigDmz.Id `
  -EnableIPForwarding
```

#### <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej

Aby utworzyć maszynę wirtualną i dołączenia do istniejącego interfejsu sieciowego, należy najpierw utworzyć konfiguracji maszyny wirtualnej z [AzureRmVMConfig nowy](/powershell/module/azurerm.compute/new-azurermvmconfig). Konfiguracja obejmuje interfejs sieciowy utworzony w poprzednim kroku. Po wyświetleniu monitu o nazwę użytkownika i hasło, wybierz nazwę użytkownika i hasło, które chcesz zalogować się do maszyny wirtualnej z. 

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a virtual machine configuration.
$vmConfig = New-AzureRmVMConfig `
  -VMName 'myVmNva' `
  -VMSize Standard_DS2 | `
  Set-AzureRmVMOperatingSystem -Windows `
    -ComputerName 'myVmNva' `
    -Credential $cred | `
  Set-AzureRmVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id
```

Utwórz maszynę wirtualną za pomocą konfiguracji maszyny wirtualnej z [AzureRmVM nowy](/powershell/module/azurerm.compute/new-azurermvm). Poniższy przykład tworzy maszynę wirtualną o nazwie *myVmNva*. 

```azurepowershell-interactive
$vmNva = New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

`-AsJob` Opcja tworzy maszynę wirtualną w tle, dzięki czemu można kontynuować do następnego kroku. Po wyświetleniu monitu wprowadź nazwę użytkownika i hasło, które chcesz zalogować się do maszyny wirtualnej z. W środowiskach produkcyjnych, którą należy wdrożyć urządzenie wirtualne sieci jest często wstępnie skonfigurowane maszyny wirtualnej. Wiele sieci wirtualnych urządzeń są dostępne z [portalu Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1).

Azure przypisać 10.0.2.4 jako prywatny adres IP maszyny wirtualnej, ponieważ 10.0.2.4 jest pierwszy dostępny adres IP w *DMZ* podsieć *myVirtualNetwork*.

### <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Utwórz dwie maszyny wirtualne w sieci wirtualnej, aby móc weryfikować tego ruchu z *publicznego* podsieci jest kierowany do *prywatnej* podsieci za pośrednictwem sieci urządzenia wirtualnego w kolejnym kroku. 

Utwórz maszynę wirtualną w *publicznego* podsieć o [AzureRmVM nowy](/powershell/module/azurerm.compute/new-azurermvm). Poniższy przykład tworzy maszynę wirtualną o nazwie *myVmWeb* w *publicznego* podsieć *myVirtualNetwork* sieci wirtualnej. 

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmWeb" `
  -AsJob
```

Azure przypisać 10.0.0.4 jako prywatny adres IP maszyny wirtualnej, ponieważ 10.0.1.4 jest pierwszy dostępny adres IP w *publicznego* podsieć *myVirtualNetwork*.

Utwórz maszynę wirtualną w *prywatnej* podsieci.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmMgmt"
```

Maszyna wirtualna ma kilka minut na utworzenie. Azure przypisać 10.0.1.4 jako prywatny adres IP maszyny wirtualnej, ponieważ 10.0.1.4 jest pierwszy dostępny adres IP w *prywatnej* podsieć *myVirtualNetwork*. 

Nie Kontynuuj do następnego kroku, dopiero po utworzeniu maszyny wirtualnej i Azure zwraca dane wyjściowe do programu PowerShell.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>Kierować ruchem przez urządzenie wirtualne sieci

Użyj [AzureRmVMExtension zestaw](/powershell/module/azurerm.compute/set-azurermvmextension) umożliwiające ICMP przychodzącego *myVmWeb* i *myVmMgmt* maszyn wirtualnych za pośrednictwem zapory systemu Windows do testowania przy użyciu polecenia tracert Komunikacja między maszynami wirtualnymi w kolejnym kroku:

```powershell-interactive
Set-AzureRmVMExtension `
  -ResourceGroupName myResourceGroup `
  -VMName myVmWeb `
  -ExtensionName AllowICMP `
  -Publisher Microsoft.Compute `
  -ExtensionType CustomScriptExtension `
  -TypeHandlerVersion 1.8 `
  -SettingString '{"commandToExecute": "netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}' `
  -Location EastUS

Set-AzureRmVMExtension `
  -ResourceGroupName myResourceGroup `
  -VMName myVmMgmt `
  -ExtensionName AllowICMP `
  -Publisher Microsoft.Compute `
  -ExtensionType CustomScriptExtension `
  -TypeHandlerVersion 1.8 `
  -SettingString '{"commandToExecute": "netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}' `
  -Location EastUS
```

Poprzednie polecenia może potrwać kilka minut. Nie należy kontynuować do następnego kroku, aż do ukończenia polecenia i dane wyjściowe są zwracane do programu PowerShell. Chociaż tracert służy do testowania routingu w tym artykule, nie zaleca się stosowanie protokołu ICMP przez zaporę systemu Windows dla wdrożeń produkcyjnych.

Połączenie z maszyną wirtualną, publiczny adres IP z Internetu. Użyj [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) do zwrócenia publicznego adresu IP maszyny wirtualnej. Poniższy przykład zwraca publicznego adresu IP *myVmMgmt* maszyny wirtualnej:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Użyj następującego polecenia, aby utworzyć sesję pulpitu zdalnego z *myVmMgmt* maszynę wirtualną z komputera lokalnego. Zastąp `<publicIpAddress>` adres IP zwrócony z poprzednie polecenie.

```
mstsc /v:<publicIpAddress>
```

Plik protokołu Remote Desktop Protocol (RDP) jest utworzony, pobrana na komputer i otworzyć. Wprowadź nazwę użytkownika i hasło określone podczas tworzenia maszyny wirtualnej (musisz wybrać **więcej opcji**, następnie **korzystała z innego konta**, aby określić podczas obliczania wprowadzone poświadczenia możesz utworzono maszynę wirtualną), a następnie wybierz **OK**. Podczas procesu logowania może pojawić się ostrzeżenie o certyfikacie. Kliknij przycisk **Tak** lub **Kontynuuj**, aby kontynuować nawiązywanie połączenia. 

Włączone przesyłanie dalej IP w obrębie platformy Azure dla interfejsu sieciowego maszyny wirtualnej w [fowarding włączyć IP](#enable-ip-forwarding). Na maszynie wirtualnej systemu operacyjnego lub aplikacji działających w maszynie wirtualnej muszą także do przesyłania dalej ruchu sieciowego. Podczas wdrażania urządzenia wirtualnego sieci w środowisku produkcyjnym urządzenia zwykle filtry, dzienniki lub wykonuje innej funkcji przed przesłaniem ruchu. W tym artykule, system operacyjny po prostu przekazuje cały ruch, który odbiera. Włącz przesyłanie dalej IP w ramach systemu operacyjnego *myVmNva*:

W wierszu polecenia na *myVmMgmt* maszyny wirtualnej, pulpitu zdalnego do *myVmNva* maszyny wirtualnej:

``` 
mstsc /v:myVmNva
```
    
Aby włączyć przesyłanie dalej IP w ramach systemu operacyjnego *myVmNva* maszyny wirtualnej, wprowadź następujące polecenie w programie PowerShell *myVmNva* maszyny wirtualnej:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```
    
Uruchom ponownie *myVmNva* maszyny wirtualnej, co spowoduje także odłączenie sesji usług pulpitu zdalnego, pozostawiając należy w ramach sesji usług pulpitu zdalnego, należy otworzyć do *myVmMgmt* maszyny wirtualnej.

Po *myVmNva* ponownego uruchamiania maszyny wirtualnej, użyj następującego polecenia, aby przetestować routingu ruchu sieciowego do *myVmWeb* maszynę wirtualną z *myVmMgmt* wirtualnego maszyny.

```
tracert myvmweb
```

Odpowiedź jest podobny do poniższego przykładu:

```
Tracing route to myvmweb.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
over a maximum of 30 hops:
    
1     1 ms     1 ms     1 ms  10.0.0.4
  
Trace complete.
```

Widać, że ruch jest kierowany bezpośrednio z *myVmMgmt* maszyny wirtualnej do *myVmWeb* maszyny wirtualnej. Trasy domyślne platformy Azure, kierować ruchem między podsieciami.

Użyj następującego polecenia do usług pulpitu zdalnego do *myVmWeb* maszynę wirtualną z *myVmMgmt* maszyny wirtualnej:

``` 
mstsc /v:myVmWeb
```

Aby przetestować routingu ruchu sieciowego do *myVmMgmt* maszynę wirtualną z *myVmWeb* maszyny wirtualnej, wprowadź następujące polecenie w wierszu polecenia:

```
tracert myvmmgmt
```

Odpowiedź jest podobny do poniższego przykładu:

```
Tracing route to myvmmgmt.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
over a maximum of 30 hops:
        
1    <1 ms     *        1 ms  10.0.2.4
2     1 ms     1 ms     1 ms  10.0.1.4
        
Trace complete.
```

Widać, że pierwszym przeskoku jest 10.0.2.4, który jest prywatny adres IP urządzenia wirtualnego w sieci. Drugi przeskok jest 10.0.1.4, prywatnego adresu IP *myVmMgmt* maszyny wirtualnej. Trasy dodane do *myRouteTablePublic* tabeli tras i powiązanych z *publicznego* podsieci spowodował Azure, aby kierować ruchem przez analizę NVA, a nie bezpośrednio do *prywatnego* podsieci.

Zamykanie sesji pulpitu zdalnego do obu *myVmWeb* i *myVmMgmt* maszyn wirtualnych.

## <a name="troubleshoot-routing"></a>Rozwiązywanie problemów z routingiem

Jak przedstawiono w poprzednich krokach Azure stosuje trasy domyślne, które można opcjonalnie zastąpić własnego trasy. Czasami ruchu mogą być kierowane zgodnie z oczekiwaniami, należy. Użyj [AzureRmNetworkWatcher nowy](/powershell/module/azurerm.network/new-azurermnetworkwatcher) umożliwiające obserwatora sieciowego, w tym regionie EastUS, jeśli nie masz jeszcze obserwatora sieciowego w tym regionie:

```azurepowershell-interactive
# Enable network watcher for east region, if you don't already have a network watcher enabled for the region:
$nw = New-AzureRmNetworkWatcher `
 -Location eastus `
 -Name myNetworkWatcher_eastus `
 -ResourceGroupName myResourceGroup
```

Użyj [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) do ustalenia, jaki ruch jest przekierowywany między dwiema maszynami wirtualnymi. Na przykład następujące polecenie testów routingu ruchu z *myVmWeb* (10.0.0.4) maszyny wirtualnej do *myVmMgmt* (10.0.1.4) maszyny wirtualnej:

```azurepowershell-interactive
$vmWeb = Get-AzureRmVM `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup

Get-AzureRmNetworkWatcherNextHop `
  -DestinationIPAddress 10.0.1.4 `
  -NetworkWatcherName myNetworkWatcher_eastus `
  -ResourceGroupName myResourceGroup `
  -SourceIPAddress 10.0.0.4 `
  -TargetVirtualMachineId $vmWeb.Id
```
Następujące dane wyjściowe są zwracane po krótkim okresie oczekiwania:

```azurepowershell
NextHopIpAddress    NextHopType       RouteTableId
------------------  ---------------- ---------------------------------------------------------------------------------------------------------------------------
10.0.2.4            VirtualAppliance  /subscriptions/<Subscription-Id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/routeTables/myRouteTablePublic
```

Dane wyjściowe informuje, że następnego przeskoku adresu IP dla ruchu z *myVmWeb* do *myVmMgmt* jest 10.0.2.4 ( *myVmNva* maszyny wirtualnej), czy typ następnego przeskoku jest  *VirtualAppliance*, oraz że tabeli tras, która powoduje występowanie routingu jest *myRouteTablePublic*.

Skuteczne trasy dla każdego interfejsu sieciowego są kombinacją trasy domyślne platformy Azure i wszelkie tras zdefiniowanych. Aby wyświetlić wszystkie trasy dla interfejsu sieciowego w maszynie wirtualnej z [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable). Na przykład, aby wyświetlić skuteczne trasy dla *myVmWeb* interfejsu sieciowego w *myVmWeb* maszyny wirtualnej, wprowadź następujące polecenie:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVmWeb `
  -ResourceGroupName myResourceGroup `
  | Format-Table
```

Zwracane są wszystkie trasy domyślnej i trasy dodanym w poprzednim kroku.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, użyj [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) można usunąć grupy zasobów i wszystkie zasoby zawiera.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Kolejne kroki

W tym artykule możesz utworzyć tabelę tras i skojarzone go do podsieci. Utworzono sieci urządzenie wirtualne, które kierowany ruch z publicznego podsieci prywatne podsieci. Podczas wdrażania można wielu zasobów platformy Azure w ramach sieci wirtualnej, zasobów w przypadku niektórych usług Azure PaaS nie można wdrożyć w sieci wirtualnej. Możesz nadal ograniczyć dostęp do zasobów pewnych usług Azure PaaS ruch tylko z podsieci sieci wirtualnej jednak. Przejdź do następnego artykuł, aby dowiedzieć się, jak ograniczyć dostęp do sieci Azure PaaS zasobów.

> [!div class="nextstepaction"]
> [Ograniczenie dostępu do sieci do PaaS zasobów](virtual-network-service-endpoints-configure.md#azure-powershell)