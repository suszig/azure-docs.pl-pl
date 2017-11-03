---
title: "Instalowanie plików Azure woluminu w wystąpień kontenera platformy Azure"
description: "Dowiedz się, jak można zainstalować woluminu plików Azure, aby utrwalić stanu z wystąpień kontenera platformy Azure"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 41c3a449b39d6ef77e1dd0cf10699f8debcad475
ms.sourcegitcommit: 54fd091c82a71fbc663b2220b27bc0b691a39b5b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2017
---
# <a name="mounting-an-azure-file-share-with-azure-container-instances"></a>Instalowanie udziału plików na platformę Azure z wystąpień kontenera platformy Azure

Domyślnie bezstanowe są wystąpień kontenera platformy Azure. Kontener ulegnie awarii lub przestanie, wszystkie jego stan zostanie utracone. Aby zachować stan poza okres istnienia kontenera, można zainstalować woluminu z magazynu zewnętrznego. W tym artykule przedstawiono sposób instalacji udziału plików na platformę Azure do użycia z wystąpień kontenera platformy Azure.

## <a name="create-an-azure-file-share"></a>Tworzenie udziału plików na platformę Azure

Przed rozpoczęciem korzystania z udziału plików na platformę Azure z wystąpień kontenera platformy Azure, należy utworzyć ją. Uruchom następujący skrypt, aby utworzyć konto magazynu do obsługi udziałów plików i udziału. Nazwa konta magazynu musi być unikatowe globalnie, więc skrypt ten dodaje losowych wartości do podstawowej ciągu.

```azurecli-interactive
# Change these four parameters
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -l $ACI_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -o tsv`

# Create the share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="acquire-storage-account-access-details"></a>Możliwość nabycia szczegółów dostępu do konta magazynu

Aby zainstalować udział plików na platformę Azure jako wolumin w wystąpień kontenera platformy Azure, potrzebne są trzy wartości: Nazwa konta magazynu, nazwa udziału i klucz dostępu do magazynu.

Jeśli używany jest skrypt powyżej, nazwa konta magazynu został utworzony z losowych wartości na końcu. Aby sprawdzić, ostatni ciąg (w tym losowe części), użyj następujących poleceń:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

Nazwa udziału jest już znany (jest *acishare* w skrypcie powyżej), więc wszystko, że pozostaje jest klucz konta magazynu, który można znaleźć przy użyciu następującego polecenia:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="store-storage-account-access-details-with-azure-key-vault"></a>Przechowuj szczegóły dostępu konta magazynu z usługi Azure key vault

Klucze konta magazynu ochrony dostępu do danych, dlatego zalecamy przechowywanie ich w magazynie kluczy Azure.

Utwórz magazyn kluczy z wiersza polecenia platformy Azure:

```azurecli-interactive
KEYVAULT_NAME=aci-keyvault
az keyvault create -n $KEYVAULT_NAME --enabled-for-template-deployment -g $ACI_PERS_RESOURCE_GROUP
```

`enabled-for-template-deployment` Przełącznika umożliwia usługi Azure Resource Manager do kluczy tajnych ściągnięcia z magazynu kluczy w czasie wdrażania.

Magazyn klucz konta magazynu, jako nowego klucza tajnego w magazynie kluczy:

```azurecli-interactive
KEYVAULT_SECRET_NAME=azurefilesstoragekey
az keyvault secret set --vault-name $KEYVAULT_NAME --name $KEYVAULT_SECRET_NAME --value $STORAGE_KEY
```

## <a name="mount-the-volume"></a>Zainstalowania woluminu

Instalowanie udziału plików na platformę Azure jako wolumin w kontenerze jest procesem dwuetapowym. Najpierw podaj szczegóły udziału w ramach definiowania grupy kontenerów, a następnie można określić sposób wolumin zainstalowany w co najmniej jeden z kontenerów w grupie.

Aby zdefiniować woluminy mają być dostępne na potrzeby instalacji, należy dodać `volumes` tablicy do kontenera grupy definicji szablonu usługi Azure Resource Manager, a następnie odwoływać je w definicji poszczególnych kontenerów.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "type": "string"
    },
    "storageaccountkey": {
      "type": "securestring"
    }
  },
  "resources":[{
    "name": "hellofiles",
    "type": "Microsoft.ContainerInstance/containerGroups",
    "apiVersion": "2017-08-01-preview",
    "location": "[resourceGroup().location]",
    "properties": {
      "containers": [{
        "name": "hellofiles",
        "properties": {
          "image": "seanmckenna/aci-hellofiles",
          "resources": {
            "requests": {
              "cpu": 1,
              "memoryInGb": 1.5
            }
          },
          "ports": [{
            "port": 80
          }],
          "volumeMounts": [{
            "name": "myvolume",
            "mountPath": "/aci/logs/"
          }]
        }
      }],
      "osType": "Linux",
      "ipAddress": {
        "type": "Public",
        "ports": [{
          "protocol": "tcp",
          "port": "80"
        }]
      },
      "volumes": [{
        "name": "myvolume",
        "azureFile": {
          "shareName": "acishare",
          "storageAccountName": "[parameters('storageaccountname')]",
          "storageAccountKey": "[parameters('storageaccountkey')]"
        }
      }]
    }
  }]
}
```

Szablon zawiera nazwy konta magazynu i klucz jako parametry, które można podać w pliku parametrów oddzielne. Aby wypełnić pliku parametrów, należy trzech wartości: Nazwa konta magazynu, identyfikator zasobu magazynu kluczy Azure, a nazwa klucza tajnego klucza magazynu używanego do przechowywania klucza magazynu. Jeśli powyższe kroki zostały wykonane, możesz uzyskać tych wartości za pomocą następującego skryptu:

```azurecli-interactive
echo $STORAGE_ACCOUNT
echo $KEYVAULT_SECRET_NAME
az keyvault show --name $KEYVAULT_NAME --query [id] -o tsv
```

Wstaw wartości w pliku parametrów:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "value": "<my_storage_account_name>"
    },
   "storageaccountkey": {
      "reference": {
        "keyVault": {
          "id": "<my_keyvault_id>"
        },
        "secretName": "<my_storage_account_key_secret_name>"
      }
    }
  }
}
```

## <a name="deploy-the-container-and-manage-files"></a>Wdrażanie kontenera i zarządzanie plikami

Z szablonem zdefiniowane można utworzyć kontenera i zainstalować jego woluminu przy użyciu wiersza polecenia platformy Azure. Przy założeniu, że plik szablon ma nazwę *azuredeploy.json* i pliku parametrów o nazwie *azuredeploy.parameters.json*, a następnie w wierszu polecenia jest:

```azurecli-interactive
az group deployment create --name hellofilesdeployment --template-file azuredeploy.json --parameters @azuredeploy.parameters.json --resource-group myResourceGroup
```

Po uruchomieniu kontenera, korzystając z aplikacji sieci web proste wdrażane za pomocą **seanmckenna/aci-hellofiles** obrazu do zarządzania plików w udziale plików na platformę Azure w określonej ścieżce instalacji. Uzyskaj adres IP dla aplikacji sieci web za pomocą następujących czynności:

```azurecli-interactive
az container show --resource-group myResourceGroup --name hellofiles -o table
```

Można użyć narzędzia, takiego jak [Eksploratora usługi Microsoft Azure Storage](http://storageexplorer.com) pobierania i sprawdź plik zapisywane w udziale plików.

>[!NOTE]
> Aby dowiedzieć się więcej o korzystaniu z szablonów usługi Azure Resource Manager, pliki parametrów i wdrożenie z wiersza polecenia platformy Azure, zobacz [wdrożenie zasobów z szablonami usługi Resource Manager i interfejsu wiersza polecenia Azure](../azure-resource-manager/resource-group-template-deploy-cli.md).

## <a name="next-steps"></a>Następne kroki

- Wdrażanie dla Twojego pierwszego kontenera za pomocą wystąpień kontenera Azure [Szybki Start](container-instances-quickstart.md)
- Dowiedz się więcej o [relacja między wystąpieniami kontenera Azure i kontener orchestrators](container-instances-orchestrator-relationship.md)
