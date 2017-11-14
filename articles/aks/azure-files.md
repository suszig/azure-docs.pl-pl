---
title: "Użyj plików na platformę Azure z AKS | Dokumentacja firmy Microsoft"
description: "Używać dysków Azure AKS"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 11457e6556e6400d8f58f71c71ab1e790bcef8f1
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2017
---
# <a name="using-azure-files-with-kubernetes"></a>Korzystanie z Kubernetes plików platformy Azure

Aplikacje kontenera na podstawie często muszą uzyskać dostęp do utrwalenia danych w woluminie danych zewnętrznych. Usługa pliki Azure może służyć jako ten magazyn danych zewnętrznych. Szczegóły tego artykułu przy użyciu plików Azure jako wolumin Kubernetes usługi kontenera platformy Azure.

Aby uzyskać więcej informacji na woluminach Kubernetes, zobacz [woluminów Kubernetes][kubernetes-volumes].

## <a name="creating-a-file-share"></a>Tworzenie udziału plików

Istniejący już udział plików Azure może służyć z usługi kontenera platformy Azure. Jeśli potrzebujesz go utworzyć, użyj następujący zestaw poleceń.

Utwórz grupę zasobów do udziału plików platformy Azure przy użyciu [Tworzenie grupy az] [ az-group-create] polecenia. Grupa zasobów konta magazynu i klaster Kubernetes musi znajdować się w tym samym regionie.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Użyj [Tworzenie konta magazynu az] [ az-storage-create] polecenie, aby utworzyć konto usługi Azure Storage. Nazwa konta magazynu musi być unikatowa. Zaktualizuj wartość `--name` argumentu z unikatową wartość.

```azurecli-interactive
az storage account create --name mystorageaccount --resource-group myResourceGroup --sku Standard_LRS
```

Użyj [listy kluczy konta magazynu az ] [ az-storage-key-list] poleceniu, aby uzyskać klucza magazynu. Zaktualizuj wartość `--account-name` argument nazwy konta magazynu unikatowy.

Zwróć uwagę jedna z wartości klucza, jest on używany w kolejnych krokach.

```azurecli-interactive
az storage account keys list --account-name mystorageaccount --resource-group myResourceGroup --output table
```

Użyj [utworzyć udział magazynu az] [ az-storage-share-create] polecenie, aby utworzyć udział plików Azure. Aktualizacja `--account-key` wartości z wartością zbierane w ostatnim kroku.

```azurecli-interactive
az storage share create --name myfileshare --account-name mystorageaccount --account-key <key>
```

## <a name="create-kubernetes-secret"></a>Utwórz klucz tajny Kubernetes

Kubernetes wymaga poświadczeń dostępu do udziału plików. Zamiast przechowywania nazwy konta magazynu Azure i kluczy z każdej pod, jest ona przechowywana raz w [klucz tajny Kubernetes] [ kubernetes-secret] i odwołuje się każdego woluminu pliki Azure. 

Wartości w manifeście tajny Kubernetes musi być kodowany w standardzie base64. Użyj następujących poleceń, aby zwrócić wartości zakodowany.

Po pierwsze kodowanie nazwy konta magazynu. Zastąp `storage-account` z nazwą konta magazynu Azure.

```azurecli-interactive
echo -n <storage-account> | base64
```

Następnie jest wymagany klucz dostępu do konta magazynu. Uruchom następujące polecenie, aby zwrócić kluczu zakodowanego. Zastąp `storage-key` z kluczem zebrane w poprzednim kroku

```azurecli-interactive
echo -n <storage-key> | base64
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

Użyj [zastosować kubectl] [ kubectl-apply] polecenie, aby utworzyć klucz tajny.

```azurecli-interactive
kubectl apply -f azure-secret.yml
```

## <a name="mount-file-share-as-volume"></a>Instalowanie udziału plików jako wolumin

Można zainstalować na udział plików Azure w sieci pod przez skonfigurowanie woluminu w jego specyfikację. Utwórz nowy plik o nazwie `azure-files-pod.yml` z następującą zawartość. Aktualizacja `share-name` nazwa nadana plików Azure udostępnianie.

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
      shareName: <share-name>
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
[kubectl-apply]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#apply
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[az-group-create]: /cli/azure/group#az_group_create