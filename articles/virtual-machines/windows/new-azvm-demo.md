---
title: "Tworzenie maszyny Wirtualnej systemu Windows za pomocą uproszczonego AzureRMVM nowe polecenia cmdlet w powłoce chmury Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się szybko Tworzenie maszyn wirtualnych systemu Windows za pomocą polecenia cmdlet New-AzureRMVM uproszczony w powłoce chmury Azure."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 94eb6232cf59d502a9d70545785c3788398f4d27
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/15/2017
---
# <a name="create-a-windows-virtual-machine-with-the-simplified-new-azurermvm-cmdlet-in-cloud-shell"></a>Utwórz maszynę wirtualną systemu Windows za pomocą polecenia cmdlet New-AzureRMVM uproszczony w powłoce chmury 

[AzureRMVM nowy](/powershell/module/azurerm.resources/new-azurermvm) polecenia cmdlet został dodany uproszczony zestaw parametry w celu utworzenia nowej maszyny Wirtualnej przy użyciu programu PowerShell. W tym temacie przedstawiono z najnowszą wersją polecenia cmdlet New-AzureVM preinstalowany, aby utworzyć nową maszynę Wirtualną przy użyciu programu PowerShell w powłoce chmury Azure. Używamy parametru uproszczony zestaw, który automatycznie tworzy niezbędne zasoby przy użyciu wartości domyślnych inteligentne. 

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).


[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]

Jeśli wybierzesz do zainstalowania i używania programu PowerShell lokalnie, ten samouczek wymaga programu Azure PowerShell w wersji modułu 5.1.1 lub nowszym. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure.

## <a name="create-the-vm"></a>Tworzenie maszyny wirtualnej

Można użyć [AzureRMVM nowy](/powershell/module/azurerm.resources/new-azurermvm) polecenia cmdlet, aby utworzyć Maszynę wirtualną przy użyciu ustawień domyślnych inteligentne, które obejmują użycie obrazu systemu Windows Server 2016 w centrum danych z portalu Azure Marketplace. New-AzureRMVM z można użyć tylko **— nazwa** parametru i użycie tej wartości dla wszystkich nazw zasobów. W tym przykładzie parametr **-Nazwa** zostanie ustawiony na wartość *myVM*. 

Upewnij się, że w usłudze Cloud Shell wybrano program **PowerShell** i wpisz:

```azurepowershell-interactive
New-AzureRMVm -Name myVM
```

Pojawi się monit o utworzenie nazwy użytkownika i hasła dla maszyny wirtualnej. Dane te będą używane do połączenia się z maszyną wirtualną w dalszej części tego tematu. Hasło musi mieć długość od 12 do 123 znaków i spełniać trzy z czterech następujących wymagań dotyczących złożoności: mała litera, wielka litera, cyfra i znak specjalny.

Utworzenie maszyny wirtualnej i skojarzonych zasobów trwa około minutę. Po zakończeniu tej operacji można wyświetlić wszystkie utworzone zasoby przy użyciu polecenia cmdlet [Find-AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource).

```azurepowershell-interactive
Find-AzureRmResource `
    -ResourceGroupNameEquals myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>Łączenie z maszyną wirtualną

Po zakończeniu wdrożenia utwórz połączenie pulpitu zdalnego z maszyną wirtualną.

Wróć do publicznego adresu IP maszyny wirtualnej za pomocą polecenia [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Zanotuj ten adres IP.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

Na komputerze lokalnym, otwórz wiersz polecenia i użyj **mstsc** polecenie, aby uruchomić sesję pulpitu zdalnego z nowej maszyny Wirtualnej. Zamień &lt;publiczny adres IP&gt; na adres IP maszyny wirtualnej. Po wyświetleniu monitu wprowadź nazwę użytkownika i hasło podane podczas tworzenia maszyny wirtualnej.

```
mstsc /v:<publicIpAddress>
```
## <a name="specify-different-resource-names"></a>Określ inny zasób nazwy

Można również podać nazwy opisowej dla zasobów i nadal są tworzone automatycznie. Oto przykład, gdy firma Microsoft ma nazwanych wielu zasobów dla nowej maszyny Wirtualnej, w tym nową grupę zasobów.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 3389
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy grupa zasobów, maszyna wirtualna i wszystkie pokrewne zasoby nie będą już potrzebne, można je usunąć za pomocą polecenia [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVM
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tym temacie wdrożyliśmy prostą maszynę wirtualną przy użyciu polecenia New-AzVM, a następnie połączyliśmy się z nią za pośrednictwem protokołu RDP. Aby dowiedzieć się więcej o maszynach wirtualnych platformy Azure, przejdź do samouczka dla maszyn wirtualnych z systemem Windows.

> [!div class="nextstepaction"]
> [Samouczki dla maszyny wirtualnej platformy Azure z systemem Windows](./tutorial-manage-vm.md)
