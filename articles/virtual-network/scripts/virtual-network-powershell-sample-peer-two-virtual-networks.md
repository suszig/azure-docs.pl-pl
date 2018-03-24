---
title: Przykładowym skrypcie programu PowerShell Azure - Peer dwie sieci wirtualne | Dokumentacja firmy Microsoft
description: Przykładowym skrypcie programu PowerShell Azure - Peer dwie sieci wirtualne
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: c0efdf759a0bdb87de4dc8ff9566a8e817503c5e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="peer-two-virtual-networks"></a>Dwie wirtualne sieci równorzędne

Ten skrypt tworzy i łączy dwie sieci wirtualne w tym samym regionie za pośrednictwem sieci platformy Azure. Po uruchomieniu skryptu spowoduje utworzenie komunikacji równorzędnej między dwiema sieciami wirtualnymi.

Skrypt można wykonywać z platformy Azure [powłoki chmury](https://shell.azure.com/powershell), lub z lokalną instalacją programu PowerShell. Jeśli używasz programu PowerShell lokalnie, ten skrypt wymaga środowiska AzureRM PowerShell wersji modułu 5.4.1 lub nowszym. Aby znaleźć zainstalowanej wersji, uruchom `Get-Module -ListAvailable AzureRM`. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.ps1 "Peer two networks")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Objaśnienia dla skryptu

Ten skrypt zawiera następujące polecenia służące do tworzenia grupy zasobów, maszyny wirtualnej i wszystkich powiązanych zasobów. Każde polecenie w następujących łączy tabeli dokumentacji określonego polecenia:

| Polecenie | Uwagi |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Tworzy grupę zasobów, w której są przechowywane wszystkie zasoby. | 
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)| Tworzy sieć wirtualną i podsieć platformy Azure. |
| [Add-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering) | Tworzy komunikacji równorzędnej między dwiema sieciami wirtualnymi.  |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Usuwa grupę zasobów wraz ze wszystkimi zagnieżdżonymi zasobami. |

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji na temat programu Azure PowerShell, zobacz [dokumentację programu Azure PowerShell](/powershell/azure/overview).

Przykłady skryptów PowerShell dodatkowe sieci wirtualnej można znaleźć w [przykłady środowiska PowerShell sieci wirtualnej](../powershell-samples.md).