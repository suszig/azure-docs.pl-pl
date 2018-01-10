---
title: "Użyj plików na platformę Azure z AKS"
description: "Używać dysków Azure AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/04/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 4873b98c8ba4f1e574be20baebef3b6860341529
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="persistent-volumes-with-azure-files---dynamic-provisioning"></a>Woluminy trwałe z plikami Azure - dynamicznego inicjowania obsługi administracyjnej.

Trwały wolumin reprezentuje fragment magazynu, które zostały udostępnione do użycia w klastrze Kubernetes. Trwały wolumin mogą być używane przez jedną lub wiele stanowiskami i można za dynamicznie lub statycznie. Ten dokument zawiera szczegóły dotyczące dynamicznego inicjowania obsługi administracyjnej udziału plików na platformę Azure jako trwały wolumin Kubernetes AKS klastra. 

Aby uzyskać więcej informacji na woluminach trwałe Kubernetes, zobacz [woluminy trwałe Kubernetes][kubernetes-volumes].

## <a name="prerequisites"></a>Wymagania wstępne

Podczas dynamicznego inicjowania obsługi administracyjnej udziału plików na platformę Azure jako wolumin Kubernetes dowolne konto magazynu może służyć tak długo, jak są zawarte w tej samej grupie zasobów co klaster AKS. Jeśli to konieczne, Utwórz konto magazynu w tej samej grupie zasobów co klaster AKS. 

Aby zidentyfikować grupę odpowiednich zasobów, użyj [listy grup az] [ az-group-list] polecenia.

```azurecli-interactive
az group list --output table
```

Następujące przykładowe dane wyjściowe zawiera grupy zasobów, zarówno skojarzony z klastrem AKS. Grupa zasobów o nazwie, takich jak *MC_myAKSCluster_myAKSCluster_eastus* zawiera AKS zasobów klastra i gdzie konta magazynu musi zostać utworzona. 

```
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

Po zidentyfikowaniu grupy zasobów należy utworzyć konta magazynu z [Tworzenie konta magazynu az] [ az-storage-account-create] polecenia.

```azurecli-interactive
az storage account create --resource-group  MC_myAKSCluster_myAKSCluster_eastus --name mystorageaccount --location eastus --sku Standard_LRS
```

## <a name="create-storage-class"></a>Tworzenie klasy magazynu

Klasa magazynu jest używany do definiowania sposobu skonfigurowania trwały wolumin utworzony dynamicznie. Elementy, takie jak nazwa konta magazynu platformy Azure, jednostki SKU i region są definiowane w obiekcie klasy magazynu. Aby uzyskać więcej informacji na Kubernetes klasy magazynu, zobacz [klasy magazynu Kubernetes][kubernetes-storage-classes].

Poniższy przykład pokazuje, że wszystkie konta magazynu dla jednostki SKU typu `Standard_LRS` w `eastus` region może być używane podczas żądania magazynu. 

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  skuName: Standard_LRS
```

Aby użyć konta określonego magazynu, `storageAccount` parametr może zostać użyty.

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
parameters:
  storageAccount: azure_storage_account_name
```

## <a name="create-persistent-volume-claim"></a>Tworzenie oświadczeń trwały wolumin

Oświadczenie trwały wolumin używa obiektu klasy magazynu, aby dynamicznie inicjują obsługę część magazynu. Podczas korzystania z usługi pliki Azure, tworzenie udziału plików Azure na koncie magazynu wybrany lub określony w obiekcie klasy magazynu.

>  [!NOTE]
>   Upewnij się, że konto magazynu odpowiednie zostało utworzone wcześniej w tej samej grupie zasobów co AKS zasobów klastra. Ta grupa zasobów ma nazwę, takich jak *MC_myAKSCluster_myAKSCluster_eastus*. Oświadczenia trwały wolumin nie będzie można udostępnić udziału plików na platformę Azure, jeśli konto magazynu nie jest dostępna. 

Manifest następujące może służyć do tworzenia oświadczeń trwały wolumin `5GB` rozmiaru w `ReadWriteOnce` dostępu. Aby uzyskać więcej informacji na PVC tryby dostępu, zobacz [tryby dostępu do][access-modes].

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: azurefile
  resources:
    requests:
      storage: 5Gi
```

## <a name="using-the-persistent-volume"></a>Przy użyciu trwały wolumin

Po oświadczeń trwały wolumin został utworzony, a Magazyn pomyślnie zainicjowano obsługę administracyjną, pod można tworzyć z dostępem do woluminu. Manifest następujących tworzy pod, używającej oświadczeń trwały wolumin `azurefile` Aby zainstalować udział plików na platformę Azure w `/var/www/html` ścieżki. 

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azurefile
```

## <a name="mount-options"></a>Opcje instalacji

Domyślną wartością parametru fileMode i dirMode wartości różnią się między wersjami Kubernetes zgodnie z opisem w poniższej tabeli. 

| wersja | wartość |
| ---- | ---- |
| V1.6.x, v1.7.x | 0777 |
| V1.8.0 v1.8.5 | 0700 |
| V1.8.6 lub nowszy | 0755 |
| V1.9.0 | 0700 |
| V1.9.1 lub nowszy | 0755 |

Jeśli przy użyciu klastra wersji 1.8.5 lub nowszego instalacji można określić opcji dla obiekt klasy magazynu. W poniższym przykładzie `0777`. 

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azurefile
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=1000
  - gid=1000
parameters:
  skuName: Standard_LRS
```

Jeśli przy użyciu klastra wersji 1.8.0 - 1.8.4, za pomocą można określić kontekstu zabezpieczeń `runAsUser` wartość `0`. Aby uzyskać więcej informacji na Pod kontekstu zabezpieczeń, zobacz [skonfigurować kontekstu zabezpieczeń][kubernetes-security-context].

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się więcej o Kubernetes woluminów trwałego za pomocą usługi pliki Azure.

> [!div class="nextstepaction"]
> [Dodatek Kubernetes plików Azure][kubernetes-files]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-create]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#create
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/
[kubernetes-files]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_file/README.md
[kubernetes-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-security-context]: https://kubernetes.io/docs/tasks/configure-pod-container/security-context/
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[az-group-create]: /cli/azure/group#az_group_create
[az-group-list]: /cli/azure/group#az_group_list
[az-storage-account-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-create]: /cli/azure/storage/account#az_storage_account_create
[az-storage-key-list]: /cli/azure/storage/account/keys#az_storage_account_keys_list
[az-storage-share-create]: /cli/azure/storage/share#az_storage_share_create