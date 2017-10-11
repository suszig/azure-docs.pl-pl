---
title: "Przykładowym skrypcie programu PowerShell Azure - ruchu sieciowego maszyn wirtualnych filtr | Dokumentacja firmy Microsoft"
description: "Skrypt programu PowerShell Azure przykładowe — filtrowania ruchu przychodzącego i wychodzącego ruchu sieciowego maszyny Wirtualnej."
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: e871ba2f370157936c2aaabc804dc9f5aea6d7ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>Filtrowanie ruchu przychodzącego i wychodzącego ruchu sieciowego maszyny Wirtualnej

Ten przykładowy skrypt tworzy sieć wirtualną z podsieci frontonu i zaplecza. Ruch sieciowy przychodzący do podsieci frontonu może zawierać maksymalnie HTTP i HTTPS, gdy ruch wychodzący do Internetu z podsieci wewnętrznej jest niedozwolone. Po uruchomieniu skryptu, ma jedną maszynę wirtualną z dwiema kartami sieciowymi. Każda karta sieciowa jest podłączona do innej podsieci.

W razie potrzeby zainstalować program Azure PowerShell przy użyciu instrukcji w [Przewodnik programu Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), a następnie uruchom `Login-AzureRmAccount` można utworzyć połączenia z platformą Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt


[!code-powershell[główne](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "ruchu sieciowego maszyn wirtualnych filtru")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do utworzenia grupy zasobów, sieć wirtualną i grup zabezpieczeń sieci. Każde polecenie w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Nowe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Nowe AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Tworzy obiekt konfiguracji podsieci |
| [Nowy AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Tworzy sieć wirtualna platformy Azure i podsieci frontonu. |
| [Nowe AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Tworzy zasady zabezpieczeń ma być przypisany do grupy zabezpieczeń sieci. |
| [Nowe AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |Tworzy reguły NSG, które zezwala lub blokuje określone porty do określonych podsieci. |
| [Zestaw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | Kojarzy grup NSG do podsieci. |
| [Nowe AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Tworzy publiczny adres IP na dostęp do maszyny Wirtualnej z Internetu. |
| [Nowe AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Tworzy interfejsy sieci wirtualnej i dołącza je do podsieci sieci wirtualnej frontonu i zaplecza. |
| [Nowe AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Tworzy konfiguracji maszyny Wirtualnej. Ta konfiguracja zawiera informacje, takie jak nazwa maszyny Wirtualnej, system operacyjny i poświadczenia administracyjne. Konfiguracja jest używany podczas tworzenia maszyny Wirtualnej. |
| [Nowe AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Utwórz maszynę wirtualną. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Usuwa grupę zasobów i wszystkie zasoby zawarte w ciągu. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących programu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Dodatkowe przykłady skryptów PowerShell sieci można znaleźć w [Azure Przegląd dokumentacji](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).