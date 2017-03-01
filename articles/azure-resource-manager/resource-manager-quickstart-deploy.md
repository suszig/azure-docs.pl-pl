---
title: "Wdrażanie zasobów na platformie Azure | Microsoft Docs"
description: "Wdrażanie zasobów na platformie Azure przy użyciu programu Azure PowerShell lub interfejsu wiersza polecenia platformy Azure. Zasoby są zdefiniowane w szablonie usługi Resource Manager."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 41d9476d4ce6b7afc2ba9c757e96db7e8e72d5ae
ms.openlocfilehash: a77354d0719240e5916b5ec945dad75e534802d3


---
# <a name="deploy-resources-to-azure"></a>Wdrażanie zasobów na platformie Azure

W tym temacie przedstawiono sposób wdrażania zasobów do subskrypcji platformy Azure. Do wdrożenia szablonu usługi Resource Manager definiującego infrastrukturę rozwiązania można użyć programu Azure PowerShell lub interfejsu wiersza polecenia platformy Azure.

Wprowadzenie do koncepcji usługi Resource Manager można znaleźć w temacie [Omówienie usługi Azure Resource Manager](resource-group-overview.md).

## <a name="steps-for-deployment"></a>Kroki wdrażania

W tym temacie założono, że w tym temacie wdrażasz [przykładowy szablon magazynu](#example-storage-template). Możesz użyć innego szablonu, ale przekazywane parametry są inne niż wartość podane w tym temacie.

Po utworzeniu szablonu ogólne kroki wdrażania szablonu to:

1. Logowanie się na swoim koncie
2. Wybranie subskrypcji do użycia (konieczne tylko jeśli masz wiele subskrypcji i chcesz używać jednej, innej niż subskrypcja domyślna)
3. Tworzenie grupy zasobów
4. Wdrożenie szablonu
5. Sprawdzenie stanu wdrożenia

W poniższych sekcjach opisano sposób wykonywania tych kroków przy użyciu [programu PowerShell](#powershell) lub [interfejsu wiersza polecenia platformy Azure](#azure-cli).

## <a name="powershell"></a>PowerShell

1. Aby zainstalować program Azure PowerShell, zobacz [Get started with Azure PowerShell cmdlets](/powershell/azureps-cmdlets-docs) (Rozpoczynanie pracy z poleceniami cmdlet programu Azure PowerShell).

2. Aby szybko rozpocząć wdrożenie, użyj następujących poleceń cmdlet:

  ```powershell
  Login-AzureRmAccount
  Set-AzureRmContext -SubscriptionID {subscription-id}

  New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json 
  ```

  Polecenie cmdlet `Set-AzureRmContext` jest wymagane tylko, jeśli chcesz skorzystać z subskrypcji innej niż domyślna. Aby wyświetlić wszystkie swoje subskrypcje i ich identyfikatory, użyj polecenia:

  ```powershell
  Get-AzureRmSubscription
  ```

3. Wdrożenie może potrwać kilka minut. Po zakończeniu zostanie wyświetlony komunikat podobny do następującego:

  ```powershell
  DeploymentName          : ExampleDeployment
  CorrelationId           : 07b3b233-8367-4a0b-b9bc-7aa189065665
  ResourceGroupName       : ExampleGroup
  ProvisioningState       : Succeeded
  ...
  ```

4. Aby sprawdzić, czy grupa zasobów i konto magazynu zostały wdrożone do subskrypcji, użyj poleceń:

  ```powershell
  Get-AzureRmResourceGroup -Name ExampleGroup
  Find-AzureRmResource -ResourceGroupNameEquals ExampleGroup
  ```

5. Podczas wdrażania szablonu możesz określić parametry szablonu jako parametry programu PowerShell. Wcześniejszy przykład nie zawierał żadnych parametrów szablonu, dlatego użyto wartości domyślnych w szablonie. Aby wdrożyć inne konto magazynu i podać wartości parametrów dla prefiksu nazwy magazynu i jednostki SKU konta magazynu, użyj następujących poleceń:

  ```powershell
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment2 -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json -storageNamePrefix "contoso" -storageSKU "Standard_GRS"
  ```

  Masz teraz dwa konta magazynu w grupie zasobów. 

## <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

1. Aby zainstalować interfejs wiersza polecenia platformy Azure, zobacz [Install Azure CLI 2.0](/cli/azure/install-az-cli2) (Instalowanie interfejsu wiersza polecenia platformy Azure w wersji 2.0).

2. Aby szybko rozpocząć wdrożenie, użyj następujących poleceń:

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create --name ExampleDeployment --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json
  ```

  Polecenie `az account set` jest wymagane tylko, jeśli chcesz skorzystać z subskrypcji innej niż domyślna. Aby wyświetlić wszystkie swoje subskrypcje i ich identyfikatory, użyj polecenia:

  ```azurecli
  az account list
  ```

3. Wdrożenie może potrwać kilka minut. Po zakończeniu zostanie wyświetlony komunikat podobny do następującego:

  ```azurecli
  "provisioningState": "Succeeded",
  ```

4. Aby sprawdzić, czy grupa zasobów i konto magazynu zostały wdrożone do subskrypcji, użyj poleceń:

  ```azurecli
  az group show --name ExampleGroup
  az resource list --resource-group ExampleGroup
  ```

5. Podczas wdrażania szablonu możesz określić parametry szablonu jako parametry programu PowerShell. Wcześniejszy przykład nie zawierał żadnych parametrów szablonu, dlatego użyto wartości domyślnych w szablonie. Aby wdrożyć inne konto magazynu i podać wartości parametrów dla prefiksu nazwy magazynu i jednostki SKU konta magazynu, użyj następujących poleceń:

  ```azurecli
  az group deployment create --name ExampleDeployment2 --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  Masz teraz dwa konta magazynu w grupie zasobów. 

## <a name="example-storage-template"></a>Przykładowy szablon magazynu

Użyj następującego przykładowego szablonu, aby wdrożyć konto magazynu w subskrypcji:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    },
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "The type of replication to use for the storage account."
      }
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>Następne kroki

* Aby uzyskać szczegółowe informacje na temat wdrażania szablonów przy użyciu programu PowerShell, zobacz [Deploy resources with Resource Manager templates and Azure PowerShell](/azure/azure-resource-manager/resource-group-template-deploy) (Wdrażanie zasobów za pomocą szablonów usługi Resource Manager i programu Azure PowerShell).
* Aby uzyskać szczegółowe informacje na temat wdrażania szablonów przy użyciu interfejsu wiersza polecenia platformy Azure, zobacz [Deploy resources with Resource Manager templates and Azure CLI](/azure/azure-resource-manager/resource-group-template-deploy-cli) (Wdrażanie zasobów za pomocą szablonów usługi Resource Manager i interfejsu wiersza polecenia platformy Azure).






<!--HONumber=Feb17_HO3-->


