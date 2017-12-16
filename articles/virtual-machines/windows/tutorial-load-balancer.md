---
title: "Jak załadować saldo maszyn wirtualnych systemu Windows na platformie Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać usługi równoważenia obciążenia Azure do tworzenia aplikacji wysokiej dostępności i bezpieczne w trzech maszyn wirtualnych systemu Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/14/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 6eee852e703d25ccc4b13401c3e4ab46d09655da
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/15/2017
---
# <a name="how-to-load-balance-windows-virtual-machines-in-azure-to-create-a-highly-available-application"></a>Jak załadować saldo maszyn wirtualnych systemu Windows na platformie Azure, aby utworzyć aplikację wysokiej dostępności
Równoważenie obciążenia sieciowego zapewnia wyższy poziom dostępności dzięki rozproszeniu przychodzące żądania między wieloma maszynami wirtualnymi. W tym samouczku opisano różne składniki usługi równoważenia obciążenia Azure dystrybucji ruchu, które zapewniają wysoką dostępność. Omawiane kwestie:

> [!div class="checklist"]
> * Tworzenie usługi równoważenia obciążenia Azure
> * Utwórz kondycji sondę modułu równoważenia obciążenia
> * Tworzenie reguły ruchu usługi równoważenia obciążenia
> * Niestandardowe rozszerzenie skryptu umożliwiają utworzenie podstawowej witryny usług IIS
> * Tworzenie maszyn wirtualnych i dołączanie do usługi równoważenia obciążenia
> * Wyświetl modułu równoważenia obciążenia w akcji
> * Dodawanie i usuwanie maszyny wirtualne z modułem równoważenia obciążenia

Dla tego samouczka jest wymagany moduł Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).


## <a name="azure-load-balancer-overview"></a>Omówienie usługi równoważenia obciążenia Azure
Moduł równoważenia obciążenia Azure jest równoważenia obciążenia warstwy 4 (TCP, UDP), który zapewnia wysoką dostępność, przekazując przychodzący ruch między maszynami wirtualnymi w dobrej kondycji. Badanie kondycji modułu równoważenia obciążenia monitoruje danego portu na każdej maszynie Wirtualnej i tylko dystrybuuje ruch do operacyjnej maszyny Wirtualnej.

Należy zdefiniować frontonu konfiguracji adresu IP, który zawiera co najmniej jeden publiczny adres IP. Ta frontonu konfiguracji IP umożliwia Twojej usługi równoważenia obciążenia i aplikacje mają być dostępne za pośrednictwem Internetu. 

Maszyny wirtualne połączenia z modułem równoważenia obciążenia przy użyciu ich karty interfejsu sieci wirtualnej (NIC). Aby rozpowszechnić ruch do maszyn wirtualnych, puli adresów zaplecza zawiera adres IP adresów wirtualnych (NIC) połączony z usługą równoważenia obciążenia.

Aby sterowanie przepływem ruchu, należy zdefiniować reguły modułu równoważenia obciążenia dla określonych portów i protokołów, które mapują do maszyn wirtualnych.


## <a name="create-azure-load-balancer"></a>Tworzenie usługi równoważenia obciążenia Azure
W tej sekcji Szczegóły, jak można tworzyć i konfigurować poszczególnych składników usługi równoważenia obciążenia. Przed utworzeniem przez moduł równoważenia obciążenia, Utwórz nową grupę zasobów o [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroupLoadBalancer* w *EastUS* lokalizacji:

```powershell
New-AzureRmResourceGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS
```

### <a name="create-a-public-ip-address"></a>Tworzenie publicznego adresu IP
Aby uzyskać dostęp do aplikacji w Internecie, należy publicznego adresu IP usługi równoważenia obciążenia. Utwórz publiczny adres IP z [AzureRmPublicIpAddress nowy](/powershell/module/azurerm.network/new-azurermpublicipaddress). Poniższy przykład tworzy publiczny adres IP o nazwie *myPublicIP* w *myResourceGroupLoadBalancer* grupy zasobów:

```powershell
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-a-load-balancer"></a>Tworzenie modułu równoważenia obciążenia
Tworzenie puli adresów IP frontonu, z [AzureRmLoadBalancerFrontendIpConfig nowy](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). Poniższy przykład tworzy pula IP frontonu, o nazwie *myFrontEndPool* i dołącza *myPublicIP* adres: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
```

Utwórz pulę adresów zaplecza z [AzureRmLoadBalancerBackendAddressPoolConfig nowy](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). Maszyny wirtualne dołączyć do tej puli zaplecza w pozostałych kroków. Poniższy przykład tworzy puli adresów zaplecza, o nazwie *myBackEndPool*:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Teraz Utwórz moduł równoważenia obciążenia z [AzureRmLoadBalancer nowy](/powershell/module/azurerm.network/new-azurermloadbalancer). Poniższy przykład tworzy moduł równoważenia obciążenia o nazwie *myLoadBalancer* przy użyciu pule adresów IP frontonu i wewnętrznej bazy danych utworzone w poprzednich krokach:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a>Utworzyć sondy kondycji
Aby zezwolić usłudze równoważenia obciążenia do monitorowania stanu aplikacji, należy użyć sondy kondycji. Sondy kondycji dynamicznie dodaje lub usuwa maszyn wirtualnych z oparte na ich odpowiedzi na sprawdzenie kondycji obrót usługi równoważenia obciążenia. Domyślnie maszyny Wirtualnej zostanie usunięte z dystrybucji modułu równoważenia obciążenia po dwóch kolejnych błędów na 15 sekund. Można utworzyć sondy kondycji, na podstawie protokołu lub stronę wyboru kondycji określonych aplikacji. 

Poniższy przykład tworzy sondowaniem TCP. Można również utworzyć niestandardowe sond HTTP więcej kontroli kondycji szczegółowe. Podczas korzystania z niestandardowego badanie HTTP, należy utworzyć strona sprawdzania kondycji, takich jak *healthcheck.aspx*. Sonda musi zwracać **HTTP 200 OK** odpowiedzi dla usługi równoważenia obciążenia zachować hosta w obrotu.

Aby utworzyć sondy kondycji TCP, należy użyć [AzureRmLoadBalancerProbeConfig Dodaj](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). Poniższy przykład tworzy badanie kondycji o nazwie *myHealthProbe* który monitoruje każdej maszyny Wirtualnej na *TCP* portu *80*:

```powershell
Add-AzureRmLoadBalancerProbeConfig `
  -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

Aby zastosować sondy kondycji, należy zaktualizować modułu równoważenia obciążenia z [AzureRmLoadBalancer zestaw](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a>Tworzenie reguły modułu równoważenia obciążenia
Reguły modułu równoważenia obciążenia jest używany do definiowania rozkład ruchu do maszyn wirtualnych. Należy zdefiniować konfiguracji IP frontonu dla ruchu przychodzącego i puli adresów IP zaplecza, aby odbierać ruch, wraz z wymagany port źródłowy i docelowy. Aby upewnij się, że tylko dobrej kondycji maszyn wirtualnych odbieranie ruchu, również zdefiniować sondy kondycji do użycia.

Tworzenie reguły modułu równoważenia obciążenia z [AzureRmLoadBalancerRuleConfig Dodaj](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). Poniższy przykład tworzy regułę równoważenia obciążenia o nazwie *myLoadBalancerRule* i równoważy ruchu na *TCP* portu *80*:

```powershell
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name myHealthProbe

Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

Aktualizacja usługi równoważenia obciążenia z [AzureRmLoadBalancer zestaw](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```


## <a name="configure-virtual-network"></a>Konfigurowanie sieci wirtualnej
Przed wdrożeniem niektórych maszyn wirtualnych i przetestować z usługi równoważenia, Utwórz pomocnicze zasoby sieci wirtualnej. Aby uzyskać więcej informacji o sieciach wirtualnych, zobacz [Zarządzanie sieciami wirtualnymi Azure](tutorial-virtual-network.md) samouczka.

### <a name="create-network-resources"></a>Utwórz zasoby sieciowe
Tworzenie sieci wirtualnej z [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Poniższy przykład tworzy sieć wirtualną o nazwie *myVnet* z *mySubnet*:

```powershell
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create the virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

Tworzenie reguły grupy zabezpieczeń sieci z [AzureRmNetworkSecurityRuleConfig nowy](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), należy utworzyć grupę zabezpieczeń sieci z [AzureRmNetworkSecurityGroup nowy](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Dodaj sieciową grupę zabezpieczeń do podsieci o [AzureRmVirtualNetworkSubnetConfig zestaw](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) , a następnie zaktualizować sieci wirtualnej z [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork). 

Poniższy przykład tworzy regułę grupy zabezpieczeń sieci o nazwie *myNetworkSecurityGroup* i stosuje je do *mySubnet*:

```powershell
# Create security rule config
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow

# Create the network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule

# Apply the network security group to a subnet
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24

# Update the virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

Wirtualne karty sieciowe są tworzone za pomocą [AzureRmNetworkInterface nowy](/powershell/module/azurerm.network/new-azurermnetworkinterface). Poniższy przykład tworzy trzy wirtualne karty sieciowe. (Jedną wirtualną kartę Sieciową dla każdej maszyny Wirtualnej można utworzyć dla aplikacji w poniższych krokach). Można utworzyć dodatkowe wirtualne karty sieciowe i maszyn wirtualnych w dowolnym momencie i dodaj je do usługi równoważenia obciążenia:

```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -Name myNic$i `
     -Location EastUS `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```

## <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych
Aby zwiększyć wysoką dostępność aplikacji, umieść maszyn wirtualnych w zestawie dostępności.

Utwórz zestaw o dostępności [AzureRmAvailabilitySet nowy](/powershell/module/azurerm.compute/new-azurermavailabilityset). Poniższy przykład tworzy zbiór nazwanego dostępności *myAvailabilitySet*:

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myAvailabilitySet `
  -Location EastUS `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

Ustaw nazwę użytkownika i hasło administratora dla maszyn wirtualnych o [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Teraz można tworzyć maszyn wirtualnych o [AzureRmVM nowy](/powershell/module/azurerm.compute/new-azurermvm). Poniższy przykład tworzy trzech maszyn wirtualnych:

```powershell
for ($i=1; $i -le 3; $i++)
{
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_D1 `
    -AvailabilitySetId $availabilitySet.Id
  $vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
  $vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  $vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk$i `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
  $nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic$i
  $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
  New-AzureRmVM `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Location EastUS `
    -VM $vm
}
```

Trwa kilka minut, aby utworzyć i skonfigurować wszystkie trzy maszyny wirtualne.

### <a name="install-iis-with-custom-script-extension"></a>Instalowanie usług IIS przy użyciu niestandardowego rozszerzenia skryptu
W poprzednich samouczek dotyczący [sposobu dostosowywania maszyny wirtualnej systemu Windows](tutorial-automate-vm-deployment.md), wiesz, jak można zautomatyzować dostosowania maszyny Wirtualnej z niestandardowego skryptu rozszerzenia dla systemu Windows. Te same podejście służy do instalowania i konfigurowania usług IIS na maszyny wirtualne.

Użyj [AzureRmVMExtension zestaw](/powershell/module/azurerm.compute/set-azurermvmextension) do zainstalowania niestandardowe rozszerzenie skryptu. Uruchamia rozszerzenia `powershell Add-WindowsFeature Web-Server` Aby zainstalować serwer sieci Web usług IIS, a następnie aktualizacje *Default.htm* stronę, aby wyświetlić nazwę hosta maszyny wirtualnej:

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a>Test usługi równoważenia obciążenia
Publiczny adres IP z usługi równoważenia obciążenia z [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Poniższy przykład uzyskuje adres IP dla *myPublicIP* utworzony wcześniej:

```powershell
Get-AzureRmPublicIPAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myPublicIP | select IpAddress
```

Następnie można wprowadzić publicznego adresu IP w przeglądarce sieci web. Witryna sieci Web jest wyświetlany, łącznie z nazwą hosta maszyny Wirtualnej dystrybuowanej usługi równoważenia obciążenia w ruchu, jak w poniższym przykładzie:

![Witryna sieci Web IIS uruchomiona](./media/tutorial-load-balancer/running-iis-website.png)

Aby wyświetlić Dystrybuuj ruch we wszystkich trzech maszyn wirtualnych z tą aplikacją usługi równoważenia obciążenia, możesz można życie odświeżania przeglądarki sieci web.


## <a name="add-and-remove-vms"></a>Dodawanie i usuwanie maszyny wirtualne
Może być konieczne przeprowadzenie konserwacji na maszynach wirtualnych z tą aplikacją, takich jak instalowanie aktualizacji systemu operacyjnego. Aby poradzić sobie z zwiększenie obciążenia do aplikacji, może być konieczne dodanie kolejnych maszyn wirtualnych. W tej sekcji przedstawiono sposób Usuń lub Dodaj Maszynę wirtualną z modułu równoważenia obciążenia.

### <a name="remove-a-vm-from-the-load-balancer"></a>Usuń Maszynę wirtualną z usługi równoważenia obciążenia
Pobierz karty interfejsu sieciowego z [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), a następnie ustaw *Loadbalancerbackendaddresspool* właściwość wirtualnej karty Sieciowej *$null*. Na koniec zaktualizuj wirtualnych kart sieciowych.:

```powershell
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic2
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Aby wyświetlić rozpowszechniają ruchu pozostałych dwóch maszyn wirtualnych z tą aplikacją usługi równoważenia obciążenia można można życie odświeżania przeglądarki sieci web. Teraz można przeprowadzać konserwacji na maszynie Wirtualnej, takie jak instalowanie aktualizacji systemu operacyjnego lub wykonywania ponownego uruchomienia maszyny Wirtualnej.

### <a name="add-a-vm-to-the-load-balancer"></a>Dodaj Maszynę wirtualną z usługą równoważenia obciążenia
Po konserwacja maszyny Wirtualnej, lub jeśli trzeba zwiększyć wydajność, ustaw *Loadbalancerbackendaddresspool* właściwość wirtualnej karty Sieciowej *BackendAddressPool* z [ Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):

Pobierz moduł równoważenia obciążenia:

```powershell
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku utworzony moduł równoważenia obciążenia i dołączone do maszyn wirtualnych. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie usługi równoważenia obciążenia Azure
> * Utwórz kondycji sondę modułu równoważenia obciążenia
> * Tworzenie reguły ruchu usługi równoważenia obciążenia
> * Niestandardowe rozszerzenie skryptu umożliwiają utworzenie podstawowej witryny usług IIS
> * Tworzenie maszyn wirtualnych i dołączanie do usługi równoważenia obciążenia
> * Wyświetl modułu równoważenia obciążenia w akcji
> * Dodawanie i usuwanie maszyny wirtualne z modułem równoważenia obciążenia

Przejście do następnym samouczku, aby dowiedzieć się, jak zarządzać sieci maszyny Wirtualnej.

> [!div class="nextstepaction"]
> [Zarządzanie maszynami wirtualnymi i sieciami wirtualnymi](./tutorial-virtual-network.md)
