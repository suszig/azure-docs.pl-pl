---
title: "Wdrażanie aplikacji w zestawie skalowania maszyn wirtualnych platformy Azure | Microsoft Docs"
description: "Dowiedz się, jak wdrożyć prostą aplikację skalowania automatycznego w zestawie skalowania maszyn wirtualnych przy użyciu szablonu usługi Azure Resource Manager."
services: virtual-machine-scale-sets
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/24/2017
ms.author: ryanwi
ms.openlocfilehash: 07883a33382cc660b043c99872312a9e77228253
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-an-autoscaling-app-using-a-template"></a>Wdrażanie aplikacji skalowania automatycznego przy użyciu szablonu

[Szablony usługi Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) to doskonały sposób wdrażania grup powiązanych zasobów. Ten samouczek stanowi rozwinięcie tematu [Deploy a simple scale set](virtual-machine-scale-sets-mvss-start.md) (Wdrażanie prostego zestawu skalowania). Opisano w nim, jak wdrożyć prostą aplikację skalowania automatycznego w zestawie skalowania przy użyciu szablonu usługi Azure Resource Manager.  Skalowanie automatyczne można również skonfigurować przy użyciu programu PowerShell, interfejsu wiersza polecenia lub portalu. Aby uzyskać więcej informacji, zapoznaj się z [omówieniem skalowania automatycznego](virtual-machine-scale-sets-autoscale-overview.md).

## <a name="two-quickstart-templates"></a>Dwa szablony szybkiego startu
Podczas wdrażania zestawu skalowania można zainstalować nowe oprogramowanie na obrazie platformy przy użyciu [rozszerzenia maszyny wirtualnej](../virtual-machines/virtual-machines-windows-extensions-features.md). Rozszerzenie maszyny wirtualnej to mała aplikacja, która zapewnia konfigurację po wdrożeniu i zadania automatyzacji na maszynach wirtualnych Azure, takie jak wdrażanie aplikacji. W folderze [Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) znajdują się dwa różne przykładowe szablony, które przedstawiają, jak wdrożyć aplikację skalowania automatycznego do zestawu skalowania przy użyciu rozszerzeń maszyny wirtualnej.

### <a name="python-http-server-on-linux"></a>Serwer HTTP Python w systemie Linux
Przykładowy szablon [serwera HTTP Python w systemie Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) wdraża prostą aplikację skalowania automatycznego działającą w zestawie skalowania w systemie Linux.  [Bottle](http://bottlepy.org/docs/dev/), struktura sieci Web języka Python, oraz prosty serwer HTTP są wdrażane na każdej maszynie wirtualnej w zestawie skalowania przy użyciu rozszerzenia maszyny wirtualnej skryptu niestandardowego. Zestaw skalowania jest skalowany w górę, gdy średnie wykorzystanie procesora na wszystkich maszynach wirtualnych przekracza 60%, lub w dół, gdy średnie wykorzystanie procesora jest mniejsze niż 30%.

Oprócz zasobu zestawu skalowania przykładowy szablon *azuredeploy.json* deklaruje również sieć wirtualną, publiczny adres IP, moduł równoważenia obciążenia i zasoby ustawień skalowania automatycznego.  Aby uzyskać więcej informacji dotyczących tworzenia tych zasobów w szablonie, zobacz artykuł [Linux scale set with autoscale](virtual-machine-scale-sets-linux-autoscale.md) (Zestaw skalowania w systemie Linux obejmujący skalowanie automatyczne).

W szablonie *azuredeploy.json* właściwość `extensionProfile` zasobu `Microsoft.Compute/virtualMachineScaleSets` określa rozszerzenie skryptu niestandardowego. `fileUris` określa lokalizację skryptu. W tym przypadku są to dwa pliki: *workserver.py*, który definiuje prosty serwer HTTP, i *installserver.sh*, który instaluje program Bottle i uruchamia serwer HTTP. `commandToExecute` określa polecenie do uruchomienia po wdrożeniu zestawu skalowania.

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "lapextension",
                "properties": {
                  "publisher": "Microsoft.Azure.Extensions",
                  "type": "CustomScript",
                  "typeHandlerVersion": "2.0",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "fileUris": [
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
                      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
                    ],
                    "commandToExecute": "bash installserver.sh"
                  }
                }
              }
            ]
          }
```

### <a name="aspnet-mvc-application-on-windows"></a>Aplikacja platformy ASP.NET MVC w systemie Windows
Przykładowy szablon [Aplikacja platformy ASP.NET MVC w systemie Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) wdraża prostą aplikację platformy ASP.NET MVC działającą w środowisku IIS w zestawie skalowania systemu Windows.  Oprogramowanie IIS i aplikacja platformy MVC są wdrażane przy użyciu rozszerzenia maszyny wirtualnej [PowerShell Desired State Configuration (DSC)](virtual-machine-scale-sets-dsc.md).  Zestaw skalowania jest skalowany w górę (jednorazowo na jednym wystąpieniu maszyny wirtualnej), gdy wykorzystanie procesora przekracza 50% przez 5 minut. 

Oprócz zasobu zestawu skalowania przykładowy szablon *azuredeploy.json* deklaruje również sieć wirtualną, publiczny adres IP, moduł równoważenia obciążenia i zasoby ustawień skalowania automatycznego. Ten szablon przedstawia również uaktualnienie aplikacji.  Aby uzyskać więcej informacji dotyczących tworzenia tych zasobów w szablonie, zobacz artykuł [Windows scale set with autoscale](virtual-machine-scale-sets-windows-autoscale.md) (Zestaw skalowania w systemie Windows obejmujący skalowanie automatyczne).

W szablonie *azuredeploy.json* właściwość `extensionProfile` zasobu `Microsoft.Compute/virtualMachineScaleSets` określa rozszerzenie [Desired State Configuration (DSC)](virtual-machine-scale-sets-dsc.md), które instaluje program IIS i domyślną aplikację sieci Web z pakietu WebDeploy.  Skrypt *IISInstall.ps1* instaluje program IIS na maszynie wirtualnej i znajduje się w folderze *DSC*.  Aplikacja sieci web MVC znajduje się w folderze *WebDeploy*.  Ścieżki do skryptu instalacji i aplikacji sieci Web są definiowane w parametrach `powershelldscZip` i `webDeployPackage` w pliku *azuredeploy.parameters.json*. 

```json
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Powershell.DSC",
                "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.9",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('powershelldscUpdateTagVersion')]",
                  "settings": {
                    "configuration": {
                      "url": "[variables('powershelldscZipFullPath')]",
                      "script": "IISInstall.ps1",
                      "function": "InstallIIS"
                    },
                    "configurationArguments": {
                      "nodeName": "localhost",
                      "WebDeployPackagePath": "[variables('webDeployPackageFullPath')]"
                    }
                  }
                }
              }
            ]
          }
```

## <a name="deploy-the-template"></a>Wdrożenie szablonu
Najprostszym sposobem wdrażania szablonu [serwera HTTP Python w systemie Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) lub [aplikacji platformy ASP.NET MVC w systemie Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) jest użycie przycisku **Wdróż na platformie Azure** znajdującego się w plikach Readme w serwisie GitHub.  Aby wdrożyć przykładowe szablony, można również użyć programu PowerShell lub interfejsu wiersza polecenia platformy Azure.

### <a name="powershell"></a>PowerShell
Skopiuj pliki [serwera HTTP Python w systemie Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) lub [aplikacji platformy ASP.NET MVC w systemie Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-webapp-dsc-autoscale) z repozytorium serwisu GitHub do folderu na komputerze lokalnym.  Otwórz plik *azuredeploy.parameters.json* i zaktualizuj domyślne wartości parametrów `vmssName`, `adminUsername` i `adminPassword`. Zapisz poniższy skrypt programu PowerShell do pliku *deploy.ps1* w tym samym folderze co szablon *azuredeploy.json*. Aby wdrożyć przykładowy szablon, uruchom skrypt *deploy.ps1* w oknie wiersza polecenia programu PowerShell.

```powershell
param(
 [Parameter(Mandatory=$True)]
 [string]
 $subscriptionId,

 [Parameter(Mandatory=$True)]
 [string]
 $resourceGroupName,

 [string]
 $resourceGroupLocation,

 [Parameter(Mandatory=$True)]
 [string]
 $deploymentName,

 [string]
 $templateFilePath = "template.json",

 [string]
 $parametersFilePath = "parameters.json"
)

<#
.SYNOPSIS
    Registers RPs
#>
Function RegisterRP {
    Param(
        [string]$ResourceProviderNamespace
    )

    Write-Host "Registering resource provider '$ResourceProviderNamespace'";
    Register-AzureRmResourceProvider -ProviderNamespace $ResourceProviderNamespace;
}

#******************************************************************************
# Script body
# Execution begins here
#******************************************************************************
$ErrorActionPreference = "Stop"

# sign in
Write-Host "Logging in...";
Login-AzureRmAccount;

# select subscription
Write-Host "Selecting subscription '$subscriptionId'";
Select-AzureRmSubscription -SubscriptionID $subscriptionId;

# Register RPs
$resourceProviders = @("microsoft.compute","microsoft.insights","microsoft.network");
if($resourceProviders.length) {
    Write-Host "Registering resource providers"
    foreach($resourceProvider in $resourceProviders) {
        RegisterRP($resourceProvider);
    }
}

#Create or check for existing resource group
$resourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue
if(!$resourceGroup)
{
    Write-Host "Resource group '$resourceGroupName' does not exist. To create a new resource group, please enter a location.";
    if(!$resourceGroupLocation) {
        $resourceGroupLocation = Read-Host "resourceGroupLocation";
    }
    Write-Host "Creating resource group '$resourceGroupName' in location '$resourceGroupLocation'";
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else{
    Write-Host "Using existing resource group '$resourceGroupName'";
}

# Start the deployment
Write-Host "Starting deployment...";
if(Test-Path $parametersFilePath) {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath -TemplateParameterFile $parametersFilePath;
} else {
    New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath;
}
```

### <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure
```azurecli
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'

# -e: immediately exit if any command has a non-zero exit status
# -o: prevents errors in a pipeline from being masked
# IFS new value is less likely to cause confusing bugs when looping arrays or arguments (e.g. $@)

usage() { echo "Usage: $0 -i <subscriptionId> -g <resourceGroupName> -n <deploymentName> -l <resourceGroupLocation>" 1>&2; exit 1; }

declare subscriptionId=""
declare resourceGroupName=""
declare deploymentName=""
declare resourceGroupLocation=""

# Initialize parameters specified from command line
while getopts ":i:g:n:l:" arg; do
    case "${arg}" in
        i)
            subscriptionId=${OPTARG}
            ;;
        g)
            resourceGroupName=${OPTARG}
            ;;
        n)
            deploymentName=${OPTARG}
            ;;
        l)
            resourceGroupLocation=${OPTARG}
            ;;
        esac
done
shift $((OPTIND-1))

#Prompt for parameters is some required parameters are missing
if [[ -z "$subscriptionId" ]]; then
    echo "Subscription Id:"
    read subscriptionId
    [[ "${subscriptionId:?}" ]]
fi

if [[ -z "$resourceGroupName" ]]; then
    echo "ResourceGroupName:"
    read resourceGroupName
    [[ "${resourceGroupName:?}" ]]
fi

if [[ -z "$deploymentName" ]]; then
    echo "DeploymentName:"
    read deploymentName
fi

if [[ -z "$resourceGroupLocation" ]]; then
    echo "Enter a location below to create a new resource group else skip this"
    echo "ResourceGroupLocation:"
    read resourceGroupLocation
fi

#templateFile Path - template file to be used
templateFilePath="template.json"

if [ ! -f "$templateFilePath" ]; then
    echo "$templateFilePath not found"
    exit 1
fi

#parameter file path
parametersFilePath="parameters.json"

if [ ! -f "$parametersFilePath" ]; then
    echo "$parametersFilePath not found"
    exit 1
fi

if [ -z "$subscriptionId" ] || [ -z "$resourceGroupName" ] || [ -z "$deploymentName" ]; then
    echo "Either one of subscriptionId, resourceGroupName, deploymentName is empty"
    usage
fi

#login to azure using your credentials
az account show 1> /dev/null

if [ $? != 0 ];
then
    az login
fi

#set the default subscription id
az account set --name $subscriptionId

set +e

#Check for existing RG
az group show $resourceGroupName 1> /dev/null

if [ $? != 0 ]; then
    echo "Resource group with name" $resourceGroupName "could not be found. Creating new resource group.."
    set -e
    (
        set -x
        az group create --name $resourceGroupName --location $resourceGroupLocation 1> /dev/null
    )
    else
    echo "Using existing resource group..."
fi

#Start deployment
echo "Starting deployment..."
(
    set -x
    az group deployment create --name $deploymentName --resource-group $resourceGroupName --template-file $templateFilePath --parameters $parametersFilePath
)

if [ $?  == 0 ];
 then
    echo "Template has been successfully deployed"
fi
```

## <a name="next-steps"></a>Następne kroki

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
