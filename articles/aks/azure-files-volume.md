---
title: "Użyj plików na platformę Azure z AKS"
description: "Używać dysków Azure AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/08/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 1def417f97a94fa0770b99606cd3a68189d1d51b
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="volumes-with-azure-files"></a>Woluminy zawierające pliki Azure

Aplikacje kontenera często muszą uzyskać dostęp do utrwalenia danych w woluminie danych zewnętrznych. Usługa pliki Azure może służyć jako ten magazyn danych zewnętrznych. Szczegóły tego artykułu przy użyciu plików Azure jako wolumin Kubernetes usługi kontenera platformy Azure.

Aby uzyskać więcej informacji na woluminach Kubernetes, zobacz [woluminów Kubernetes][kubernetes-volumes].

## <a name="create-an-azure-file-share"></a>Tworzenie udziału plików na platformę Azure

Przed użyciem udział plików Azure jako wolumin Kubernetes, należy utworzyć konto usługi Azure Storage i udziału plików. Poniższy skrypt może służyć do wykonania tych zadań. Zwróć uwagę, lub zaktualizuj wartości parametrów, niektóre z nich są potrzebne podczas tworzenia woluminu Kubernetes.

```azurecli-interactive
#!/bin/bash

# Change these four parameters
AKS_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
AKS_PERS_RESOURCE_GROUP=myAKSShare
AKS_PERS_LOCATION=eastus
AKS_PERS_SHARE_NAME=aksshare

# Create the Resource Group
az group create --name $AKS_PERS_RESOURCE_GROUP --location $AKS_PERS_LOCATION

# Create the storage account
az storage account create -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -l $AKS_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $AKS_PERS_STORAGE_ACCOUNT_NAME -g $AKS_PERS_RESOURCE_GROUP -o tsv`

# Create the file share
az storage share create -n $AKS_PERS_SHARE_NAME

# Get storage account key
STORAGE_KEY=$(az storage account keys list --resource-group $AKS_PERS_RESOURCE_GROUP --account-name $AKS_PERS_STORAGE_ACCOUNT_NAME --query "[0].value" -o tsv)

# Echo storage account name and key
echo Storage account name: $AKS_PERS_STORAGE_ACCOUNT_NAME
echo Storgae account key: $STORAGE_KEY
```

## <a name="create-kubernetes-secret"></a>Utwórz klucz tajny Kubernetes

Kubernetes wymaga poświadczeń dostępu do udziału plików. Te poświadczenia są przechowywane w [klucz tajny Kubernetes][kubernetes-secret], który odwołuje się podczas tworzenia Kubernetes pod.

Użyj następującego polecenia, aby utworzyć klucz tajny. Zastąp `STORAGE_ACCOUNT_NAME` z nazwę konta magazynu i `STORAGE_ACCOUNT_KEY` kluczem magazynu.

```console
kubectl create secret generic azure-secret --from-literal=azurestorageaccountname=STORAGE_ACCOUNT_NAME --from-literal=azurestorageaccountkey=STORAGE_ACCOUNT_KEY
```

## <a name="mount-file-share-as-volume"></a>Instalowanie udziału plików jako wolumin

Można zainstalować na udział plików Azure w sieci pod przez skonfigurowanie woluminu w jego specyfikację. Utwórz nowy plik o nazwie `azure-files-pod.yaml` z następującą zawartość. Aktualizacja `aksshare` nazwa nadana plików Azure udostępnianie.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-files-pod
spec:
 containers:
  - image: microsoft/sample-aks-helloworld
    name: azure
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
 volumes:
  - name: azure
    azureFile:
      secretName: azure-secret
      shareName: aksshare
      readOnly: false
```

Umożliwia utworzenie pod kubectl.

```azurecli-interactive
kubectl apply -f azure-files-pod.yaml
```

Masz teraz kontenera uruchomiony z udziału plików Azure zainstalowany w `/mnt/azure` katalogu. Widać instalacji podczas sprawdzania z pod za pośrednictwem woluminu `kubectl describe pod azure-files-pod`.

## <a name="next-steps"></a>Kolejne kroki

Więcej informacji o woluminach Kubernetes za pomocą usługi pliki Azure.

> [!div class="nextstepaction"]
> [Dodatek Kubernetes plików Azure][kubernetes-files]

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
