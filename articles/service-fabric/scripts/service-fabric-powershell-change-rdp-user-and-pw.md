---
title: "Skrypt programu PowerShell Azure przykładowe — aktualizacja RDP nazwy użytkownika i hasła | Dokumentacja firmy Microsoft"
description: "Skrypt programu PowerShell Azure przykładowe — aktualizacja RDP nazwy użytkownika i hasła dla wszystkich węzłów klastra usługi sieć szkieletowa typu określonego węzła."
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
ms.date: 11/17/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: c4a02bd06b8d0b3b99055760d345b5a824bbb946
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="update-the-admin-username-and-password-of-the-vms-in-a-cluster"></a>Nazwa użytkownika i hasło maszyn wirtualnych w klastrze aktualizacji

Każdy typ węzła w klastrze usługi sieć szkieletowa jest zestaw skali maszyny wirtualnej. Ten przykładowy skrypt aktualizacji, nazwa użytkownika i hasło dla maszyn wirtualnych klastra w typie określonym węźle.  Dodać rozszerzenie VMAccessAgent w zestawie skali, ponieważ hasło administratora nie jest zestawu skali można modyfikować właściwości.  Zastosuj zmiany nazwy użytkownika i hasła dla wszystkich węzłów w zestawie skalowania. Dostosuj parametry zgodnie z potrzebami.

W razie potrzeby zainstalować program Azure PowerShell przy użyciu instrukcji w [Przewodnik programu Azure PowerShell](/powershell/azure/overview). 

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/service-fabric/change-rdp-user-and-pw/change-rdp-user-and-pw.ps1 "Updates a RDP username and password for cluster nodes")]

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń: każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Pobiera właściwości typu węzła klastra (zestaw skali maszyny wirtualnej).   |
| [Dodaj AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension)| Dodaje rozszerzenie do zestawu skalowania maszyn wirtualnych.|
| [AzureRmVmss aktualizacji](/powershell/module/azurerm.compute/update-azurermvmss)|Aktualizuje stan skali maszyny wirtualnej w stanie obiektu VMSS lokalnego.|

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](/powershell/azure/overview).

Dodatkowe przykłady programu Azure Powershell dla usługi sieć szkieletowa usług Azure można znaleźć w [przykłady programu Azure PowerShell](../service-fabric-powershell-samples.md).
