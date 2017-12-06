---
title: "Użyj plików na platformę Azure z AKS"
description: "Używać dysków Azure AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/17/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 12c5d4985260c734ba813ace3143433883966712
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="using-azure-files-with-kubernetes"></a>Korzystanie z Kubernetes plików platformy Azure

Aplikacje kontenera często muszą uzyskać dostęp do utrwalenia danych w woluminie danych zewnętrznych. Usługa pliki Azure może służyć jako ten magazyn danych zewnętrznych. Szczegóły tego artykułu przy użyciu plików Azure jako wolumin Kubernetes usługi kontenera platformy Azure.

Aby uzyskać więcej informacji na woluminach Kubernetes, zobacz [woluminów Kubernetes][kubernetes-volumes].

## <a name="create-an-azure-file-share"></a>Tworzenie udziału plików na platformę Azure

Przed użyciem udział plików Azure jako wolumin Kubernetes, należy utworzyć konto usługi Azure Storage i udziału plików. Poniższy skrypt może służyć do wykonania tych zadań. Zwróć uwagę, lub zaktualizuj wartości parametrów, niektóre z nich są potrzebne podczas tworzenia woluminu Kubernetes.

```azurecli-interactive
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
```

## <a name="create-kubernetes-secret"></a>Utwórz klucz tajny Kubernetes

Kubernetes wymaga poświadczeń dostępu do udziału plików. Te poświadczenia są przechowywane w [klucz tajny Kubernetes][kubernetes-secret], który odwołuje się podczas tworzenia Kubernetes pod.

Podczas tworzenia Kubernetes poufne, tajne wartości muszą być kodowany w standardzie base64.

Po pierwsze kodowanie nazwy konta magazynu. W razie potrzeby Zastąp `$AKS_PERS_STORAGE_ACCOUNT_NAME` o nazwie konto magazynu Azure.

```azurecli-interactive
echo -n $AKS_PERS_STORAGE_ACCOUNT_NAME | base64
```

Następnie należy zakodować klucz konta magazynu. W razie potrzeby Zastąp `$STORAGE_KEY` z nazwą klucza konta magazynu Azure.

```azurecli-interactive
echo -n $STORAGE_KEY | base64
```

Utwórz plik o nazwie `azure-secret.yml` i skopiuj następujące yaml programu. Aktualizacja `azurestorageaccountname` i `azurestorageaccountkey` wartości z base64 zakodowanych wartości pobierane w ostatnim kroku.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: azure-secret
type: Opaque
data:
  azurestorageaccountname: <base64_encoded_storage_account_name>
  azurestorageaccountkey: <base64_encoded_storage_account_key>
```

Użyj [utworzyć kubectl] [ kubectl-create] polecenie, aby utworzyć klucz tajny.

```azurecli-interactive
kubectl create -f azure-secret.yml
```

## <a name="mount-file-share-as-volume"></a>Instalowanie udziału plików jako wolumin

Można zainstalować na udział plików Azure w sieci pod przez skonfigurowanie woluminu w jego specyfikację. Utwórz nowy plik o nazwie `azure-files-pod.yml` z następującą zawartość. Aktualizacja `aksshare` nazwa nadana plików Azure udostępnianie.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-files-pod
spec:
 containers:
  - image: kubernetes/pause
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
kubectl apply -f azure-files-pod.yml
```

Masz teraz kontenera uruchomiony z udziału plików Azure zainstalowany w `/mnt/azure` katalogu. Widać instalacji podczas sprawdzania z pod za pośrednictwem woluminu `kubectl describe pod azure-files-pod`.

## <a name="next-steps"></a>Następne kroki

Więcej informacji o woluminach Kubernetes za pomocą usługi pliki Azure.

> [!div class="nextstepaction"]
> [Dodatek Kubernetes plików Azure](https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md)

<!-- LINKS -->
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[az-group-create]: /cli/azure/group#az_group_create