---
title: "Przykładowy skrypt programu PowerShell Azure — Ustawianie domeny niestandardowej | Dokumentacja firmy Microsoft"
description: "Przykładowy skrypt programu PowerShell Azure — Ustawianie domeny niestandardowej"
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.devlang: na
ms.topic: sample
ms.date: 11/16/2017
ms.author: apimpm
ms.custom: mvc
ms.openlocfilehash: 484805d1e31cd115d9b8a7c8ae9fdbc5899d932b
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="set-up-custom-domain"></a>Konfigurowanie domeny niestandardowej

Ten przykładowy skrypt skonfigurowanie domeny niestandardowej serwera proxy i portalu punktu końcowego usługi Zarządzanie interfejsami API.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Jeśli postanowisz zainstalować program PowerShell i używać go lokalnie, ten samouczek wymaga modułu Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure.

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/api-management/setup-custom-domain/setup_custom_domain.ps1 "Set up custom domain")]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, można użyć [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) polecenie Usuń grupę zasobów i wszystkie powiązane zasoby.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Dodatkowe przykłady programu Azure Powershell dla usługi Azure API Management znajduje się w [przykłady środowiska PowerShell](../powershell-samples.md).
