---
title: Tworzenie sieci wirtualnej na platformie Azure - PowerShell | Dokumentacja firmy Microsoft
description: "Dowiedz się szybko utworzyć sieć wirtualną przy użyciu programu PowerShell. Sieć wirtualna umożliwia wiele typów zasobów platformy Azure do prywatnie komunikują się ze sobą."
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
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 22fcdbda85f3ea336c3926e04d408935ed069c25
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="create-a-virtual-network-using-powershell"></a>Tworzenie sieci wirtualnej przy użyciu programu PowerShell

W tym artykule należy Dowiedz się, jak utworzyć sieć wirtualną. Po utworzeniu sieci wirtualnej, możesz wdrożyć dwóch maszyn wirtualnych w sieci wirtualnej, aby przetestować siecią prywatną komunikację między nimi.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Jeśli użytkownik chce zainstalować i używać środowiska PowerShell lokalnie, w tym artykule wymaga środowiska AzureRM PowerShell wersji modułu 5.1.1 lub nowszym. Aby znaleźć zainstalowanej wersji, uruchom ` Get-Module -ListAvailable AzureRM`. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure.

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów platformy Azure za pomocą polecenia [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*. Wszystkie zasoby platformy Azure są tworzone w lokalizacji platformy Azure (lub regionu).

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej

Utwórz sieć wirtualną przy użyciu polecenia [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Poniższy przykład tworzy domyślną sieci wirtualnej o nazwie *myVirtualNetwork* w *EastUS* lokalizacji:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/24
```

Wszystkie sieci wirtualne mają co najmniej jeden prefiksy adresów do nich przypisany. Przestrzeń adresowa jest określone w notacji CIDR. Adresu 10.0.0.0/24 miejsca obejmuje 10.0.0.0-10.0.0.254. Sieci wirtualne mają zero lub więcej podsieci w nich. Zasoby są wdrażane w podsieci w sieci wirtualnej. 

Tworzenie konfiguracji podsieci z [AzureRmVirtualNetworkSubnetConfig nowy](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Wszystkie podsieci mają prefiks adresu, który istnieje w ramach prefiks adresu sieci wirtualnej. W tym przykładzie jest tworzony konfiguracji podsieci z tego samego prefiksu adresu jako prefiks adresu sieci wirtualnej:

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Chociaż prefiks adresu podsieci obejmuje 10.0.0.0-10.0.0.254, tylko 10.0.0.4-10.0.0.254 adresy są dostępne, ponieważ Azure rezerwuje pierwsze cztery adresów (0-3) oraz ostatni adres w każdej podsieci. Ponieważ prefiks adresu podsieci jest taki sam jak prefiks adresu sieci wirtualnej, w tej sieci wirtualnej może istnieć tylko jedna podsieć.

Zapisać konfiguracji podsieci do sieci wirtualnej z [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), która tworzy podsieć:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="test-network-communication"></a>Test łączności sieciowej

Sieć wirtualna umożliwia kilka typów zasobów platformy Azure do prywatnie komunikują się ze sobą. Jeden typ zasobów, które można wdrożyć w sieci wirtualnej jest maszyną wirtualną. Utwórz dwie maszyny wirtualne w sieci wirtualnej, aby móc weryfikować prywatnej komunikacji między nimi w kolejnym kroku.

### <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Utwórz maszynę wirtualną z [AzureRmVM nowy](/powershell/module/azurerm.compute/new-azurermvm). Podczas wykonywania tego kroku jest wyświetlany monit o poświadczenia. Wprowadzane wartości są konfigurowane jako nazwa użytkownika i hasło dla maszyny wirtualnej. Lokalizacja, w której utworzono maszynę wirtualną w musi być lokalizację, w której sieć wirtualna istnieje w. Maszyna wirtualna nie jest wymagane w tej samej grupie zasobów co maszyny wirtualnej, chociaż w tym artykule. `-AsJob` Parametr umożliwia polecenia do uruchomienia w tle, dzięki czemu można kontynuować z następnego zadania.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "default" `
    -Name "myVm1" `
    -AsJob
```

Dane wyjściowe podobne do następujących przykładowe dane wyjściowe są zwracane, a Azure rozpoczyna tworzenie maszyny wirtualnej w tle.

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

Azure DHCP automatycznie przypisuje 10.0.0.4 do maszyny wirtualnej podczas tworzenia, ponieważ jest pierwszy dostępny adres w *domyślne* podsieci.

Tworzenie drugiej maszyny wirtualnej. 

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```
Maszyna wirtualna ma kilka minut na utworzenie. Po utworzeniu Azure zwraca dane wyjściowe dotyczące utworzony maszyny wirtualnej. Chociaż nie dane wyjściowe, Azure przypisać *10.0.0.5* do *myVm2* maszyny wirtualnej, ponieważ był następnego dostępnego adresu w podsieci.

### <a name="connect-to-a-virtual-machine"></a>Połącz z maszyną wirtualną

Użyj [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) polecenia, aby zwrócić publicznego adresu IP maszyny wirtualnej. Domyślnie Azure przypisuje publiczny, adres IP routingu internetowego każdej maszyny wirtualnej. Publiczny adres IP jest przypisany do maszyny wirtualnej z [puli adresów przypisanych do każdego regionu Azure](https://www.microsoft.com/download/details.aspx?id=41653). Azure wie, który publiczny adres IP jest przypisany do maszyny wirtualnej, system operacyjny działający na maszynie wirtualnej nie ma informacji o żadnych publicznego adresu IP, które są przypisane do niej. Poniższy przykład zwraca publicznego adresu IP *myVm1* maszyny wirtualnej:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -Name myVm1 -ResourceGroupName myResourceGroup | Select IpAddress
```

Użyj następującego polecenia, aby utworzyć sesję pulpitu zdalnego z *myVm1* maszynę wirtualną z komputera lokalnego. Zastąp `<publicIpAddress>` adres IP zwrócony z poprzednie polecenie.

```
mstsc /v:<publicIpAddress>
```

Plik protokołu Remote Desktop Protocol (RDP) jest utworzony, pobrana na komputer i otworzyć. Wprowadź nazwę użytkownika i hasło określone podczas tworzenia maszyny wirtualnej, a następnie kliknij przycisk **OK**. Podczas procesu logowania może pojawić się ostrzeżenie o certyfikacie. Kliknij przycisk **Tak** lub **Kontynuuj**, aby kontynuować nawiązywanie połączenia.

### <a name="validate-communication"></a>Sprawdź poprawność komunikacji

Próba ping systemu Windows maszyny wirtualnej nie powiedzie się, ponieważ domyślnie ping nie jest dozwolone przez zaporę systemu Windows. Aby umożliwić polecenia ping, aby *myVm1*, wprowadź następujące polecenie w wierszu polecenia:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Aby sprawdzić poprawność komunikację z *myVm2*, wprowadź następujące polecenie w wierszu polecenia *myVm1* maszyny wirtualnej. Podaj poświadczenia, które zostały użyte podczas tworzenia maszyny wirtualnej, a następnie ukończ połączenia:

```
mstsc /v:myVm2
```

Podłączanie pulpitu zdalnego zakończy się pomyślnie, ponieważ obie maszyny wirtualne mają przypisane z prywatnych adresów IP *domyślne* podsieci i dlatego pulpitu zdalnego jest otwarty przez zaporę systemu Windows, domyślnie. Możesz połączyć się z *myVm2* przez hosta o nazwie, ponieważ platforma Azure automatycznie udostępnia rozpoznawanie nazw DNS dla wszystkich hostów w sieci wirtualnej. W wierszu polecenia polecenie ping Mój *myVm1*, z *myVm2*.

```
ping myvm1
```

Polecenie ping zakończy się pomyślnie, ponieważ jego dozwolone przez zaporę systemu Windows na *myVm1* maszyny wirtualnej w poprzednim kroku. Aby potwierdzić wychodzącego komunikację z Internetem, wprowadź następujące polecenie:

```
ping bing.com
```

Otrzymasz cztery odpowiedzi z bing.com. Domyślnie ruch wychodzący do Internetu może komunikować się żadnej maszyny wirtualnej w sieci wirtualnej.

Zakończenia sesji usług pulpitu zdalnego. 

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, można użyć [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) polecenie, aby usunąć grupę zasobów i wszystkie zasoby zawiera:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Kolejne kroki

W tym artykule została wdrożona domyślna sieci wirtualnej z jedną podsiecią. Aby dowiedzieć się, jak utworzyć sieć wirtualną niestandardowe z wieloma podsieciami, nadal samouczek dotyczący tworzenia niestandardowych sieci wirtualnej.

> [!div class="nextstepaction"]
> [Tworzenie niestandardowych sieci wirtualnej](virtual-networks-create-vnet-arm-ps.md)
