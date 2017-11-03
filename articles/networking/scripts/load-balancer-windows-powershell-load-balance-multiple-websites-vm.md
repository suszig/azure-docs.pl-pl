---
title: "Przykładowy skrypt programu PowerShell Azure - Równoważenie obciążenia wielu witryn sieci Web przy użyciu programu Azure PowerShell | Dokumentacja firmy Microsoft"
description: "Przykładowy skrypt programu PowerShell Azure - Równoważenie obciążenia wielu witryn sieci Web do tej samej maszyny wirtualnej"
services: load-balancer
documentationcenter: load-balancer
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: d73cdc98ff279c3ee1b93443abe4b6c7c97786a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="load-balance-multiple-websites"></a>Równoważenie obciążenia wielu witryn sieci Web

Ten przykładowy skrypt tworzy sieć wirtualną z dwóch maszyn wirtualnych (VM), które są członkami zestawu dostępności. Moduł równoważenia obciążenia kieruje ruch dla dwóch osobnych adresów IP do dwóch maszyn wirtualnych. Po uruchomieniu skryptu, można wdrożyć oprogramowanie serwera sieci web do maszyn wirtualnych i hostów wiele witryn sieci web z własnego adresu IP.

W razie potrzeby zainstalować program Azure PowerShell przy użyciu instrukcji w [Przewodnik programu Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), a następnie uruchom `Login-AzureRmAccount` można utworzyć połączenia z platformą Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.ps1  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do utworzenia grupy zasobów, sieci wirtualnej, usługi równoważenia obciążenia i wszystkie powiązane zasoby. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Nowe AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Nowe AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Tworzy zbiór zapewnić wysoką dostępność dostępności Azure. |
| [Nowe AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Tworzy konfiguracji podsieci. Ta konfiguracja jest używana z procesu tworzenia sieci wirtualnej. |
| [Nowy AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Tworzy sieć wirtualną. |
| [Nowe AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Tworzy publiczny adres IP. |
| [Nowe AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Tworzy konfiguracji IP frontonu modułu równoważenia obciążenia. |
| [Nowe AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Tworzy Konfiguracja puli adresów zaplecza, usługi równoważenia obciążenia. |
| [Nowe AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Tworzy badanie równoważenia obciążenia Sieciowego. Sonda równoważenia obciążenia Sieciowego jest używana do monitorowania każdej maszyny Wirtualnej w zestawie równoważenia obciążenia Sieciowego. Jeśli żadnej maszyny Wirtualnej staje się niedostępny, ruch nie jest kierowany do maszyny Wirtualnej. |
| [Nowe AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Tworzy regułę równoważenia obciążenia Sieciowego. W tym przykładzie jest tworzona reguła dla portu 80. Jak ruchu HTTP dociera Równoważenie obciążenia Sieciowego, jest kierowany do portu 80 jednej z maszyn wirtualnych w zestawie równoważenia obciążenia Sieciowego. |
| [Nowe AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Tworzy moduł równoważenia obciążenia. |
| [Nowe AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/new-azurermnetworkinterfaceipconfig) | Definiuje zaawansowane funkcje dla interfejsu sieci wirtualnej. |
| [Nowe AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Tworzy interfejs sieciowy. |
| [Nowe AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Tworzy konfiguracji maszyny Wirtualnej. Ta konfiguracja zawiera informacje, takie jak nazwa maszyny Wirtualnej, system operacyjny i poświadczenia administracyjne. Konfiguracja jest używany podczas tworzenia maszyny Wirtualnej. |
| [Nowe AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Utwórz maszynę wirtualną. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Usuwa grupę zasobów i wszystkie zasoby zawarte w ciągu. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących programu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Dodatkowe przykłady skryptów PowerShell sieci można znaleźć w [Azure Przegląd dokumentacji](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
