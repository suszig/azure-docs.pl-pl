---
title: "Azure json zasad przykładowe — upewnij się, ruch https tylko dla konta magazynu | Dokumentacja firmy Microsoft"
description: "Ta zasada próbki json wymaga konta magazynu do użycia ruchu HTTPS."
services: azure-policy
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 
ms.service: azure-policy
ms.devlang: 
ms.topic: sample
ms.tgt_pltfrm: 
ms.workload: 
ms.date: 10/30/2017
ms.author: banders
ms.custom: mvc
ms.openlocfilehash: 6f96cae1d429662cb3fb9c24b8701be0fb6bb774
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="ensure-https-traffic-only-for-storage-account"></a>Upewnij się, ruch https tylko dla konta magazynu

Te zasady wymagają konta magazynu do użycia ruchu HTTPS.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Przykładowy szablon

[!code-json[main](../../../policy-templates/samples/Storage/https-traffic-only/azurepolicy.json "Ensure https traffic only for storage account")]


Przy użyciu tego szablonu można wdrożyć [portalu Azure](#deploy-with-the-portal), z [PowerShell](#deploy-with-powershell) lub [interfejsu wiersza polecenia Azure](#deploy-with-azure-cli).

## <a name="deploy-with-the-portal"></a>Wdrażanie przy użyciu portalu

[![Wdrażanie na platformie Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade)

## <a name="deploy-with-powershell"></a>Wdrażanie przy użyciu programu PowerShell

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]


````powershell
$definition = New-AzureRmPolicyDefinition -Name "https-traffic-only" -DisplayName "Ensure https traffic only for storage account" -description "Ensure https traffic only for storage account" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Storage/https-traffic-only/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Storage/https-traffic-only/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope> -PolicyDefinition $definition
$assignment
````

### <a name="clean-up-powershell-deployment"></a>Wyczyść wdrożenia programu PowerShell

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```


## <a name="deploy-with-azure-cli"></a>Wdrażanie przy użyciu interfejsu wiersza polecenia platformy Azure

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]



````cli

az policy definition create --name 'https-traffic-only' --display-name 'Ensure https traffic only for storage account' --description 'Ensure https traffic only for storage account' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Storage/https-traffic-only/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Storage/https-traffic-only/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "https-traffic-only"

````

### <a name="clean-up-azure-cli-deployment"></a>Wyczyść wdrożenia wiersza polecenia platformy Azure

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Następne kroki

- Dodatkowe przykłady szablonu zasad Azure są [szablony zasad Azure](../json-samples.md).
