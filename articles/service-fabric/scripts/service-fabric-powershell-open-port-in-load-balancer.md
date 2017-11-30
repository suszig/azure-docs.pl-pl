---
title: "Przykładowy skrypt programu PowerShell Azure - port otwartej aplikacji w usłudze równoważenia obciążenia | Dokumentacja firmy Microsoft"
description: "Azure przykładowy skrypt programu PowerShell - Otwórz port w usłudze równoważenia obciążenia Azure dla aplikacji sieci szkieletowej usług."
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
ms.date: 08/15/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 9dbb0bedd02752c4735ae097a7bd64b7b5383d6e
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="open-an-application-port-in-the-azure-load-balancer"></a>Otwórz port aplikacji w usłudze równoważenia obciążenia Azure

Aplikacji usługi Service Fabric, działające na platformie Azure znajduje się za usługą równoważenia obciążenia Azure. Ten przykładowy skrypt otwiera port do modułu równoważenia obciążenia Azure, dzięki czemu aplikacji usługi Service Fabric może komunikować się z klientami zewnętrznymi. Dostosuj parametry zgodnie z potrzebami. 

Jeśli to konieczne, Zainstaluj moduł programu PowerShell usługi Service Fabric [zestawu SDK usług sieci szkieletowej](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in the load balancer")]

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji specyficzne dla polecenia.

| Polecenie | Uwagi |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Pobiera zasobów platformy Azure.  |
| [Get-AzureRmLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer) | Pobiera moduł równoważenia obciążenia Azure. |
| [Dodaj AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | Dodaje konfiguracji sondowania z modułem równoważenia obciążenia.|
| [Get-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | Pobiera konfigurację sondę modułu równoważenia obciążenia. |
| [Dodaj AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | Dodaje konfiguracji reguły równoważenia obciążenia. |
| [Zestaw AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | Ustawia stan celem usługi równoważenia obciążenia. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Dodatkowe przykłady środowiska Powershell dla usługi sieć szkieletowa usług Azure można znaleźć w [przykłady programu Azure PowerShell](../service-fabric-powershell-samples.md).
