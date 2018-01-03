---
title: "Utwórz maszynę Wirtualną z statycznego publicznego adresu IP - szablonu usługi Azure Resource Manager | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć Maszynę wirtualną za pomocą statycznego publicznego adresu IP za pomocą szablonu usługi Azure Resource Manager."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d551085a-c7ed-4ec6-b4c3-e9e1cebb774c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2f503aa60fdd9b7cf66ef482a1041e34c88e5c01
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-an-azure-resource-manager-template"></a>Utwórz maszynę Wirtualną za pomocą statycznego publicznego adresu IP za pomocą szablonu usługi Azure Resource Manager

> [!div class="op_single_selector"]
> * [Azure portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [Program PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Interfejs wiersza polecenia platformy Azure](virtual-network-deploy-static-pip-arm-cli.md)
> * [Szablon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klasyczny)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Platforma Azure oferuje dwa różne modele wdrażania związane z tworzeniem zasobów i pracą z nimi: [model wdrażania przy użyciu usługi Azure Resource Manager i model klasyczny](../resource-manager-deployment-model.md). W tym artykule omówiono przy użyciu modelu wdrażania Menedżera zasobów, które firma Microsoft zaleca w przypadku większości nowych wdrożeń zamiast klasycznym modelu wdrażania.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-address-resources-in-a-template-file"></a>Zasoby adresów publicznych adresów IP w pliku szablonu
Można wyświetlić i pobrać [przykładowy szablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

W poniższej sekcji przedstawiono definicję publicznego zasób adresu IP, w oparciu o powyższych scenariuszy:

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('webVMSetting').pipName]",
  "location": "[variables('location')]",
  "properties": {
    "publicIPAllocationMethod": "Static"
  },
  "tags": {
    "displayName": "PublicIPAddress - Web"
  }
},
```

Powiadomienie **publicIPAllocationMethod** właściwość, która jest ustawiona na *statycznych*. Ta właściwość może być *dynamiczne* (wartość domyślna) lub *statycznych*. Ustawienie gwarantuje statycznego publicznego adresu IP przypisanego nigdy nie spowoduje zmiany.

W poniższej sekcji przedstawiono skojarzenie publicznego adresu IP z karty sieciowej:

```json
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('webVMSetting').nicName]",
    "location": "[variables('location')]",
    "tags": {
    "displayName": "NetworkInterface - Web"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
      "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
    ],
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
          "privateIPAllocationMethod": "Static",
          "privateIPAddress": "[variables('webVMSetting').ipAddress]",
          "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
          },
          "subnet": {
            "id": "[variables('frontEndSubnetRef')]"
          }
        }
      }
    ]
  }
},
```

Powiadomienie **publicIPAddress** Właściwość wskazująca **identyfikator** zasobu o nazwie **variables('webVMSetting').pipName**. Jest to nazwa publicznego adresu IP zasobu przedstawionych powyżej.

Na koniec powyżej interfejs sieciowy jest wymieniony w **networkProfile** właściwości Trwa tworzenie maszyny wirtualnej.

```json
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }
```

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Wdrażanie szablonu przy użyciu metody „kliknij, aby wdrożyć”

Przykładowy szablon dostępny w repozytorium publicznym korzysta z pliku parametrów zawierającego wartości domyślne używane do generowania scenariusza opisanego powyżej. Aby wdrożyć tego szablonu przy użyciu kliknij do wdrożenia, kliknij przycisk **wdrażanie na platformie Azure** w pliku Readme.md [maszynę Wirtualną za pomocą statycznego adresu PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) szablonu. Zastąp domyślne wartości parametrów w razie potrzeby, a następnie wprowadź wartości parametrów puste.  Postępuj zgodnie z instrukcjami w portalu, aby utworzyć maszynę wirtualną za pomocą statycznego publicznego adresu IP.

## <a name="deploy-the-template-by-using-powershell"></a>Wdrażanie szablonu przy użyciu programu PowerShell

Aby wdrożyć pobrany szablon przy użyciu programu PowerShell, wykonaj poniższe kroki.

1. Jeśli nie znasz programu Azure PowerShell, wykonaj kroki [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview) artykułu.
2. W konsoli programu PowerShell, uruchom `New-AzureRmResourceGroup` , aby utworzyć nową grupę zasobów, w razie potrzeby. Jeśli masz już utworzoną grupę zasobów, przejdź do kroku 3.

    ```powershell
    New-AzureRmResourceGroup -Name PIPTEST -Location westus
    ```

    Oczekiwane dane wyjściowe:
   
        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. W konsoli programu PowerShell, uruchom `New-AzureRmResourceGroupDeployment` polecenia cmdlet do wdrożenia szablonu.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
        -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
        -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json
    ```

    Oczekiwane dane wyjściowe:
   
        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0
   
        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         
   
        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Wdrażanie szablonu przy użyciu interfejsu wiersza polecenia platformy Azure
Aby wdrożyć szablon przy użyciu wiersza polecenia platformy Azure, wykonaj następujące czynności:

1. Jeśli po raz pierwszy używasz interfejsu wiersza polecenia Azure, postępuj zgodnie z instrukcjami [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](../cli-install-nodejs.md) artykuł, aby zainstalować i skonfigurować go.
2. Uruchom `azure config mode` polecenie, aby włączyć tryb Resource Manager, jak pokazano poniżej.

    ```azurecli
    azure config mode arm
    ```

    Oczekiwane dane wyjściowe po wprowadzeniu powyższego polecenia:

        info:    New mode is arm

3. Otwórz [pliku parametrów](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), zaznacz zawartością i zapisać go w pliku na swoim komputerze. Na przykład parametry są zapisywane w pliku o nazwie *parameters.JSON następującym kodem*. Zmiany wartości parametrów w pliku, w razie potrzeby, ale co najmniej, zalecane jest, zmień wartość parametru adminPassword na unikatowy, złożone hasło.
4. Uruchom `azure group deployment create` cmd, aby wdrożyć nową sieć wirtualną przy użyciu szablonu i parametr pliki uprzednio pobranego i zmodyfikowanego. W poniższym poleceniu zastąp <path> ze ścieżką zapisany plik. 

    ```azurecli
    azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json
    ```

    Oczekiwane dane wyjściowe (znajduje się lista wartości parametrów używane):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

