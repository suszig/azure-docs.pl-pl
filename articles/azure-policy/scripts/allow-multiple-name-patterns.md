---
title: "Azure zasad przykładowy kod json - wiele wzorce nazw | Dokumentacja firmy Microsoft"
description: "Ta zasada próbki json wymaga zgodnych zasobów jedną wzorców podana nazwa."
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
ms.date: 11/13/2017
ms.author: banders
ms.custom: mvc
ms.openlocfilehash: 7de79f131925ffdbbe2c403563ba3aca1087f79c
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2017
---
# <a name="allow-multiple-name-patterns"></a>Zezwalaj na wiele wzorce nazw

Zezwalaj na jednym z wielu Nazwa do użycia dla zasobów. Określ wzorce nazw dozwolony w regule zasad.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Przykładowy szablon

[!code-json[main](../../../policy-templates/samples/TextPatterns/allow-multiple-name-patterns/azurepolicy.json "allow multiple name patterns")]

Przy użyciu tego szablonu można wdrożyć [portalu Azure](#deploy-with-the-portal), z [PowerShell](#deploy-with-powershell) lub [interfejsu wiersza polecenia Azure](#deploy-with-azure-cli).

## <a name="deploy-with-the-portal"></a>Wdrażanie przy użyciu portalu

[![Wdrażanie na platformie Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FTextPatterns%2Fallow-multiple-name-patterns%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>Wdrażanie przy użyciu programu PowerShell

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "allow-multiple-name-patterns" -DisplayName "Allow one of many name patterns to be used for resources." -description "Allow one of many name patterns to be used for resources." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/allow-multiple-name-patterns/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/allow-multiple-name-patterns/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope> -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>Wyczyść wdrożenia programu PowerShell

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Wdrażanie przy użyciu interfejsu wiersza polecenia platformy Azure

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```azurecli
az policy definition create --name 'allow-multiple-name-patterns' --display-name 'Allow one of many name patterns to be used for resources.' --description 'Allow one of many name patterns to be used for resources.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/allow-multiple-name-patterns/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/TextPatterns/allow-multiple-name-patterns/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "allow-multiple-name-patterns" 
```

### <a name="clean-up-azure-cli-deployment"></a>Wyczyść wdrożenia wiersza polecenia platformy Azure

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszyny Wirtualnej i wszystkie powiązane zasoby.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Następne kroki

- Dodatkowe przykłady szablonu zasad Azure są [szablony zasad Azure](../json-samples.md).
