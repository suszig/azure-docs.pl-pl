---
title: "Przykładowy skrypt programu PowerShell Azure - zmiany zakresu portów protokołu RDP | Dokumentacja firmy Microsoft"
description: "Przykładowy skrypt programu PowerShell Azure - zmienia zakres portów protokołu RDP wdrożonej klastra."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 11/28/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 9089d071c69830323a94140a5ebb4e9e865499c6
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="update-the-rdp-port-range-values"></a>Zaktualizuj wartości zakresu portów protokołu RDP

Ten przykładowy skrypt umożliwia zmianę wartości zakresu portów protokołu RDP w węźle klastra maszyny wirtualne po wdrożeniu klastra.  Program Azure PowerShell jest używany, aby nie wyłączać podstawowych maszynach wirtualnych.  Pobiera skrypt `Microsoft.Network/loadBalancers` zasobów w grupie zasobów klastra i aktualizacji `inboundNatPools.frontendPortRangeStart` i `inboundNatPools.frontendPortRangeEnd` wartości. Dostosuj parametry zgodnie z potrzebami.

W razie potrzeby zainstalować program Azure PowerShell przy użyciu instrukcji w [Przewodnik programu Azure PowerShell](/powershell/azure/overview). 

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-port-range/change-rdp-port-range.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Pobiera `Microsoft.Network/loadBalancers` zasobów. |
|[Zestaw AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource)|Aktualizacje `Microsoft.Network/loadBalancers` zasobów.|

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Dodatkowe przykłady programu Azure Powershell dla usługi sieć szkieletowa usług Azure można znaleźć w [przykłady programu Azure PowerShell](../service-fabric-powershell-samples.md).
