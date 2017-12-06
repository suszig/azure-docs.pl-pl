---
title: "Zainstalować woluminu plików Azure w wystąpień kontenera platformy Azure"
description: "Dowiedz się, jak można zainstalować woluminu plików Azure, aby utrwalić stanu z wystąpień kontenera platformy Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 12/05/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: b2e8e27cecb4d1225e378690063b42f5d5242868
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="mount-an-azure-file-share-with-azure-container-instances"></a>Instalowanie udziału plików na platformę Azure z wystąpień kontenera platformy Azure

Domyślnie bezstanowe są wystąpień kontenera platformy Azure. Kontener ulegnie awarii lub przestanie, wszystkie jego stan zostanie utracone. Aby zachować stan poza okres istnienia kontenera, można zainstalować woluminu z magazynu zewnętrznego. W tym artykule przedstawiono sposób instalacji udziału plików na platformę Azure do użycia z wystąpień kontenera platformy Azure.

## <a name="create-an-azure-file-share"></a>Tworzenie udziału plików na platformę Azure

Przed rozpoczęciem korzystania z udziału plików na platformę Azure z wystąpień kontenera platformy Azure, należy utworzyć ją. Uruchom następujący skrypt, aby utworzyć konto magazynu do obsługi udziałów plików i udziału. Nazwa konta magazynu musi być unikatowe globalnie, więc skrypt ten dodaje losowych wartości do podstawowej ciągu.

```azurecli-interactive
# Change these four parameters as needed
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -l $ACI_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable. The following 'az storage share create' command
# references this environment variable when creating the Azure file share.
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -o tsv`

# Create the file share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="acquire-storage-account-access-details"></a>Możliwość nabycia szczegółów dostępu do konta magazynu

Aby zainstalować udział plików na platformę Azure jako wolumin w wystąpień kontenera platformy Azure, potrzebne są trzy wartości: Nazwa konta magazynu, nazwa udziału i klucz dostępu do magazynu.

Jeśli używany jest skrypt powyżej, nazwa konta magazynu został utworzony z losowych wartości na końcu. Aby sprawdzić, ostatni ciąg (w tym losowe części), użyj następujących poleceń:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group $ACI_PERS_RESOURCE_GROUP --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

Nazwa udziału jest już znany (zdefiniowany jako *acishare* w skrypcie powyżej), więc wszystko, że pozostaje jest klucz konta magazynu, który można znaleźć przy użyciu następującego polecenia:

```azurecli-interactive
STORAGE_KEY=$(az storage account keys list --resource-group $ACI_PERS_RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="deploy-the-container-and-mount-the-volume"></a>Wdrażanie kontenera i zainstalowania woluminu

Aby zainstalować udział plików na platformę Azure jako wolumin w kontenerze, określ udział i wolumin punktu instalacji podczas tworzenia kontenera o [utworzyć kontener az](/cli/azure/container#az_container_create). Jeśli poprzednie kroki zostały wykonane, można zainstalować utworzonego wcześniej przy użyciu następującego polecenia, aby utworzyć kontener udziału:

```azurecli-interactive
az container create \
    --resource-group $ACI_PERS_RESOURCE_GROUP \
    --name hellofiles \
    --image seanmckenna/aci-hellofiles \
    --ip-address Public \
    --ports 80 \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name $ACI_PERS_SHARE_NAME \
    --azure-file-volume-mount-path /aci/logs/
```

## <a name="manage-files-in-mounted-volume"></a>Zarządzanie plikami w woluminie

Po uruchomieniu kontenera, korzystając z aplikacji sieci web proste wdrażane za pomocą [seanmckenna/aci-hellofiles](https://hub.docker.com/r/seanmckenna/aci-hellofiles/) obrazu do zarządzania plików w udziale plików na platformę Azure w określonej ścieżce instalacji. Uzyskaj adres IP dla aplikacji sieci web z [Pokaż kontenera az](/cli/azure/container#az_container_show) polecenia:

```azurecli-interactive
az container show --resource-group $ACI_PERS_RESOURCE_GROUP --name hellofiles -o table
```

Można użyć [portalu Azure](https://portal.azure.com) lub narzędzia, takich jak [Eksploratora usługi Microsoft Azure Storage](https://storageexplorer.com) pobierania i sprawdź plik zapisywane w udziale plików.

## <a name="next-steps"></a>Następne kroki

Więcej informacji na temat relacji między [wystąpień kontenera Azure i kontener orchestrators](container-instances-orchestrator-relationship.md).
