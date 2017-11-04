---
title: "Tworzenie i zarządzanie maszynami wirtualnymi systemu Windows z modułu Azure PowerShell | Dokumentacja firmy Microsoft"
description: "Samouczek — tworzenie i zarządzanie maszynami wirtualnymi systemu Windows z modułu Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2237f2e5cb67df019d0975e764602babe7f4c8f9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-windows-vms-with-the-azure-powershell-module"></a>Tworzenie i zarządzanie maszynami wirtualnymi systemu Windows z modułu Azure PowerShell

Maszyny wirtualne platformy Azure zawierają w pełni konfigurowalne i elastyczne środowiska komputerowego. Ten samouczek obejmuje elementów wdrożenia podstawowej maszyny wirtualnej platformy Azure, takich jak rozmiar maszyny Wirtualnej, wybierając obrazu maszyny Wirtualnej i wdrażanie maszyny Wirtualnej. Omawiane kwestie:

> [!div class="checklist"]
> * Tworzenie i połączyć z maszyną wirtualną
> * Wybierz i używać obrazów maszyn wirtualnych
> * Wyświetlanie i używanie określonych rozmiarów maszyn wirtualnych
> * Zmienianie rozmiaru maszyny wirtualnej
> * Wyświetlanie i zrozumienie stanu maszyny Wirtualnej


[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Jeśli postanowisz zainstalować program PowerShell i używać go lokalnie, ten samouczek wymaga modułu Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure. 

## <a name="create-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów na pomocą polecenia [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). 

Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Grupy zasobów musi zostać utworzone przed maszyny wirtualnej. W tym przykładzie grupy zasobów o nazwie *myResourceGroupVM* jest tworzony w *EastUS* regionu. 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupVM -Location EastUS
```

Grupa zasobów jest określony, podczas tworzenia lub modyfikowania maszyn wirtualnych, które są widoczne w tym samouczku.

## <a name="create-virtual-machine"></a>Tworzenie maszyny wirtualnej

Maszyny wirtualnej musi być podłączony do sieci wirtualnej. Do komunikacji z maszyny wirtualnej używającej publicznego adresu IP za pośrednictwem karty interfejsu sieciowego.

### <a name="create-virtual-network"></a>Tworzenie sieci wirtualnej

Utwórz podsieć o [AzureRmVirtualNetworkSubnetConfig nowy](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
```

Tworzenie sieci wirtualnej z [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```azurepowershell-interactive
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```
### <a name="create-public-ip-address"></a>Utwórz publiczny adres IP

Utwórz publiczny adres IP z [AzureRmPublicIpAddress nowy](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupVM `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIPAddress
```

### <a name="create-network-interface-card"></a>Utwórz karty interfejsu sieciowego

Utwórz kartę sieciową z [AzureRmNetworkInterface nowy](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```azurepowershell-interactive
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroupVM  `
  -Location EastUS `
  -Name myNic `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

### <a name="create-network-security-group"></a>Utwórz grupę zabezpieczeń sieci

Azure [sieciowej grupy zabezpieczeń](../../virtual-network/virtual-networks-nsg.md) (NSG) steruje ruchu przychodzącego i wychodzącego dla jednego lub wielu maszyn wirtualnych. Reguły grupy zabezpieczeń sieci akceptować lub odrzucać ruchu sieciowego na określonym porcie lub zakres portów. Te zasady mogą również obejmować prefiks adresu źródłowego, dzięki czemu tylko ruch w źródle wstępnie zdefiniowanych może komunikować się z maszyną wirtualną. Aby uzyskać dostęp serwer sieci Web usług IIS, który jest instalowany, należy dodać regułę ruchu przychodzącego grupy NSG.

Aby utworzyć regułę ruchu przychodzącego grupy NSG, użyj [AzureRmNetworkSecurityRuleConfig Dodaj](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig). Poniższy przykład tworzy reguły NSG o nazwie *myNSGRule* który otwiera port *3389* dla maszyny wirtualnej:

```azurepowershell-interactive
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1000 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 3389 `
  -Access Allow
```

Tworzenie przy użyciu NSG *myNSGRule* z [AzureRmNetworkSecurityGroup nowy](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```azurepowershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupVM `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRule
```

Dodaj grupy NSG do podsieci w sieci wirtualnej z [AzureRmVirtualNetworkSubnetConfig zestaw](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -VirtualNetwork $vnet `
    -NetworkSecurityGroup $nsg `
    -AddressPrefix 192.168.1.0/24
```

Aktualizacja sieci wirtualnej z [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```azurepowershell-interactive
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-machine"></a>Tworzenie maszyny wirtualnej

Podczas tworzenia maszyny wirtualnej, takich jak obraz systemu operacyjnego, dysku zmiany rozmiaru i administracyjnych poświadczeń jest kilka opcji. W tym przykładzie utworzono maszynę wirtualną o nazwie *myVM* z najnowszą wersją systemu Windows Server 2016 Datacenter.

Ustaw nazwę użytkownika i hasło potrzebne do konta administratora na maszynie wirtualnej z [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Tworzenie konfiguracji początkowej dla maszyny wirtualnej o [AzureRmVMConfig nowy](/powershell/module/azurerm.compute/new-azurermvmconfig):

```azurepowershell-interactive
$vm = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1
```

Dodaj informacje dotyczące systemu operacyjnego do konfiguracji maszyny wirtualnej z [AzureRmVMOperatingSystem zestaw](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem):

```azurepowershell-interactive
$vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM `
    -Credential $cred `
    -ProvisionVMAgent -EnableAutoUpdate
```

Dodawanie informacji o obrazie w konfiguracji maszyny wirtualnej z [AzureRmVMSourceImage zestaw](/powershell/module/azurerm.compute/set-azurermvmsourceimage):

```azurepowershell-interactive
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
```

Dodaj do konfiguracji maszyny wirtualnej z ustawień dysku systemu operacyjnego [AzureRmVMOSDisk zestaw](/powershell/module/azurerm.compute/set-azurermvmosdisk):

```azurepowershell-interactive
$vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
```

Dodaj kartę sieciową utworzonego wcześniej w konfiguracji maszyny wirtualnej z [AzureRmVMNetworkInterface Dodaj](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

```azurepowershell-interactive
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Utwórz maszynę wirtualną za pomocą polecenia [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
New-AzureRmVM -ResourceGroupName myResourceGroupVM -Location EastUS -VM $vm
```

## <a name="connect-to-vm"></a>Połączenie z maszyną Wirtualną

Po zakończeniu wdrożenia utwórz połączenie pulpitu zdalnego z maszyną wirtualną.

Uruchom następujące polecenia, aby zwrócić publiczny adres IP maszyny wirtualnej. Zapisz ten adres IP, aby połączyć się z nim w przeglądarce w celu przetestowania połączenia z siecią Web w przyszłym kroku.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupVM  | Select IpAddress
```

Użyj następującego polecenia na swojej maszynie lokalnej, aby utworzyć sesję usług pulpitu zdalnego z maszyną wirtualną. Zamień adres IP na *publiczny adres IP* Twojej maszyny wirtualnej. Po wyświetleniu monitu wprowadź poświadczenia używane podczas tworzenia maszyny wirtualnej.

```powershell
mstsc /v:<publicIpAddress>
```

## <a name="understand-vm-images"></a>Zrozumienie obrazów maszyn wirtualnych

Azure marketplace zawiera wiele obrazów maszyny wirtualnej, które mogą służyć do tworzenia nowej maszyny wirtualnej. W poprzednich krokach utworzono maszynę wirtualną przy użyciu obrazu systemu Windows Server 2016-centrum danych. W tym kroku modułu programu PowerShell jest używany do wyszukiwania marketplace dla innych obrazów systemu Windows, które może również jako podstawa dla nowych maszyn wirtualnych. Proces ten składa się z wyszukiwanie wydawcy, oferty i nazwa obrazu (Sku). 

Użyj [Get AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) polecenie, aby zwrócić listę wydawców obrazu.  

```powersehll
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Użyj [Get AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) aby powrócić do listy oferty obrazu. Z tego polecenia zwracana lista jest filtrowana na wybranego wydawcy. 

```azurepowershell-interactive
Get-AzureRmVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"
```

```azurepowershell-interactive
Offer             PublisherName          Location
-----             -------------          -------- 
Windows-HUB       MicrosoftWindowsServer EastUS 
WindowsServer     MicrosoftWindowsServer EastUS   
WindowsServer-HUB MicrosoftWindowsServer EastUS   
```

[Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) polecenia zostanie następnie filtrować według nazwy wydawcy i oferty, aby powrócić do listy nazwy obrazów.

```azurepowershell-interactive
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

```azurepowershell-interactive
Skus                                      Offer         PublisherName          Location
----                                      -----         -------------          --------
2008-R2-SP1                               WindowsServer MicrosoftWindowsServer EastUS  
2008-R2-SP1-smalldisk                     WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter                        WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter-smalldisk              WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core               WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core-smalldisk     WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers           WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers-smalldisk WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-RDSH                 WindowsServer MicrosoftWindowsServer EastUS
2016-Nano-Server                          WindowsServer MicrosoftWindowsServer EastUS
```

Te informacje można wdrożyć maszyny Wirtualnej z określonego obrazu. W tym przykładzie nazwa obrazu dla obiektu maszyny Wirtualnej. W tym samouczku kroki wdrażania pełną odwoływać się do poprzednich przykładach.

```azurepowershell-interactive
$vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter-with-Containers `
    -Version latest
```

## <a name="understand-vm-sizes"></a>Zrozumienie rozmiarów maszyn wirtualnych

Rozmiar maszyny wirtualnej określa zasoby obliczeniowe np. Procesora GPU i pamięci, które są dostępne do maszyny wirtualnej. Należy maszyny wirtualne są tworzone z odpowiednią dla obciążenia pracą expect rozmiar. Jeśli zwiększa obciążenie, można zmienić rozmiar istniejącej maszyny wirtualnej.

### <a name="vm-sizes"></a>Rozmiary maszyn wirtualnych

Poniższa tabela kategoryzuje rozmiary do przypadków użycia.  

| Typ                     | Rozmiary           |    Opis       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Zastosowania ogólne         |DSv2, Dv2, DS, D, Av2, A0 7| Zrównoważonym Procesora do pamięci. Nadaje się doskonale dla deweloperów i testowania i w małych, średnich rozwiązania aplikacji i danych.  |
| Optymalizacja pod kątem obliczeń      | FS, F             | Wysoka Procesora do pamięci. Nadaje się do aplikacji średnia ruchu, urządzeń sieciowych i procesów wsadowych.        |
| Optymalizacja pod kątem pamięci       | GS, G, DSv2, DS, Dv2, D   | Wysoka pamięci do core. Doskonałe rozwiązanie dla relacyjnych baz danych, średnich i dużych pamięci podręcznych i analiza w pamięci.                 |
| Optymalizacja pod kątem magazynu       | Ls                | Wysoka przepływność dysku i operacje we/wy. Idealne rozwiązanie w przypadku danych big data oraz baz danych SQL i NoSQL.                                                         |
| Procesory GPU           | WIRTUALIZACJĄ SIECI, NC            | Celem duże Renderowanie grafiki i wideo edycji specjalne maszyn wirtualnych.       |
| Wysoka wydajność | H-A8 11          | Nasze najbardziej zaawansowanych Procesora maszyny wirtualne z interfejsami opcjonalne wysokiej przepustowości sieci (RDMA). 


### <a name="find-available-vm-sizes"></a>Znajdowanie dostępnych rozmiarów maszyny Wirtualnej

Aby wyświetlić listę dostępnych rozmiarów maszyny Wirtualnej w regionie, w szczególności, użyj [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) polecenia.

```azurepowershell-interactive
Get-AzureRmVMSize -Location EastUS
```

## <a name="resize-a-vm"></a>Zmienianie rozmiaru maszyny wirtualnej

Po wdrożeniu maszyny Wirtualnej, można zmienić rozmiaru można zwiększyć lub zmniejszyć alokacji zasobów.

Przed zmianą rozmiaru maszyny Wirtualnej, sprawdź, czy żądany rozmiar jest dostępny na bieżący klaster maszyny Wirtualnej. [Get AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) polecenie zwraca listę rozmiarów. 

```azurepowershell-interactive
Get-AzureRmVMSize -ResourceGroupName myResourceGroupVM -VMName myVM 
```

Jeśli żądany rozmiar jest dostępny, maszyny Wirtualnej można zmienić rozmiar ze stanu zasilania na, jednak podczas operacji ponownego rozruchu.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM 
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
```

Jeśli na bieżący klaster nie ma żądanego rozmiaru, maszyna wirtualna musi alokację, zanim nastąpi operacji zmiany rozmiaru. Uwaga: w przypadku maszyny Wirtualnej jest włączona ponownie, zostaną usunięte wszystkie dane na dysku tymczasowym i publiczny adres IP adresów zmiany, chyba że używana jest statyczny adres IP. 

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroupVM  -VMName myVM
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName myResourceGroupVM 
Start-AzureRmVM -ResourceGroupName myResourceGroupVM  -Name $vm.name
```

## <a name="vm-power-states"></a>Stany zasilania maszyny Wirtualnej

Maszyny Wirtualnej platformy Azure może mieć jedną z wielu stany zasilania. Ten stan reprezentuje bieżący stan maszyny wirtualnej z punktu widzenia funkcji hypervisor. 

### <a name="power-states"></a>Stany zasilania

| Stan zasilania | Opis
|----|----|
| Uruchamianie | Wskazuje, że maszyna wirtualna zostanie uruchomiona. |
| Działanie | Wskazuje, że maszyna wirtualna jest uruchomiona. |
| Zatrzymywanie | Wskazuje, że maszyna wirtualna jest zatrzymywana. | 
| Zatrzymane | Wskazuje, czy maszyna wirtualna jest zatrzymana. Należy pamiętać, że maszyny wirtualne w stanie zatrzymania nadal naliczenie opłat za obliczenia.  |
| Cofanie przydziału | Wskazuje, że cofana jest maszyny wirtualnej. |
| Cofnięcie przydziału | Wskazuje, że maszyna wirtualna jest całkowicie usunięte z funkcji hypervisor, ale nadal dostępne w płaszczyźnie formantu. Maszyny wirtualne w stanie Deallocated nie naliczenie opłat za obliczenia. |
| - | Wskazuje, że stan zasilania maszyny wirtualnej jest nieznany. |

### <a name="find-power-state"></a>Znajdź stan zasilania

Aby pobrać stan określonej maszyny Wirtualnej, użyj [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) polecenia. Należy określić prawidłową nazwę maszyny wirtualnej i grupy zasobów. 

```azurepowershell-interactive
Get-AzureRmVM `
    -ResourceGroupName myResourceGroupVM `
    -Name myVM `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

Dane wyjściowe:

```azurepowershell-interactive
Status
------
PowerState/running
```

## <a name="management-tasks"></a>Zadania zarządzania

Podczas cyklu maszyny wirtualnej można uruchomić zadania zarządzania, takie jak uruchamianie, zatrzymywanie lub usuwanie maszyny wirtualnej. Ponadto można tworzyć skrypty służące do automatyzowania zadań powtarzających się lub złożonych. Przy użyciu programu Azure PowerShell, wiele typowych zadań zarządzania można uruchomić z wiersza polecenia lub w skryptach.

### <a name="stop-virtual-machine"></a>Zatrzymaj maszynę wirtualną

Zatrzymaj i cofnięcia przydzielenia maszynie wirtualnej z [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroupVM -Name "myVM" -Force
```

Jeśli chcesz zachować maszyny wirtualnej w stanie elastycznie, użyj parametru - StayProvisioned.

### <a name="start-virtual-machine"></a>Uruchom maszynę wirtualną

```azurepowershell-interactive
Start-AzureRmVM -ResourceGroupName myResourceGroupVM -Name myVM
```

### <a name="delete-resource-group"></a>Usuwanie grupy zasobów

Usunięcie grupy zasobów powoduje usunięcie wszystkie zasoby zawarte w ciągu.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroupVM -Force
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku poznanie podstawowych tworzenia maszyny Wirtualnej i zarządzania, np.:

> [!div class="checklist"]
> * Tworzenie i połączyć z maszyną wirtualną
> * Wybierz i używać obrazów maszyn wirtualnych
> * Wyświetlanie i używanie określonych rozmiarów maszyn wirtualnych
> * Zmienianie rozmiaru maszyny wirtualnej
> * Wyświetlanie i zrozumienie stanu maszyny Wirtualnej

Przejście do następnym samouczku, aby dowiedzieć się więcej o dyski maszyny Wirtualnej.  

> [!div class="nextstepaction"]
> [Utwórz i Zarządzaj maszyny Wirtualnej dyski](./tutorial-manage-data-disk.md)
