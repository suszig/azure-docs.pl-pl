---
title: "Skrypt programu PowerShell Azure przykładowe — Tworzenie usługi APIM | Dokumentacja firmy Microsoft"
description: "Skrypt programu PowerShell Azure przykładowe — Tworzenie usługi APIM"
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
ms.openlocfilehash: 0a93e3813fc15d5a6d3407633c65b9f12935f567
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="create-an-api-management-service"></a>Tworzenie usługi Zarządzanie interfejsami API

Ten przykładowy skrypt tworzy usługę zarządzania Developer wersji interfejsu API.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Jeśli postanowisz zainstalować program PowerShell i używać go lokalnie, ten samouczek wymaga modułu Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps). Jeśli używasz programu PowerShell lokalnie, musisz też uruchomić polecenie `Login-AzureRmAccount`, aby utworzyć połączenie z platformą Azure.

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/api-management/create-apim-service/create_apim_service.ps1 "Create a service")]

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, można użyć [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) polecenie Usuń grupę zasobów i wszystkie powiązane zasoby.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących modułu Azure PowerShell, zobacz [dokumentacji programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Dodatkowe przykłady programu Azure Powershell dla usługi Azure API Management znajduje się w [przykłady środowiska PowerShell](../powershell-samples.md).
