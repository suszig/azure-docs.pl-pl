---
title: "Należy użyć dysku platformy Azure z AKS"
description: "Używać dysków Azure AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/25/2018
ms.author: nepeters
ms.openlocfilehash: e1f5b68d5d39dd846ebec525d1e83a6c0ef4971a
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/25/2018
---
# <a name="persistent-volumes-with-azure-disks---dynamic-provisioning"></a>Trwałe woluminów z dysku systemu Azure - dynamicznego inicjowania obsługi administracyjnej.

Trwały wolumin reprezentuje fragment magazynu, które zostały udostępnione do użycia w klastrze Kubernetes. Trwały wolumin mogą być używane przez jedną lub wiele stanowiskami i można za dynamicznie lub statycznie. Ten dokument zawiera szczegóły dotyczące dynamicznego inicjowania obsługi administracyjnej dysku platformy Azure jako trwały wolumin Kubernetes AKS klastra. 

Aby uzyskać więcej informacji na woluminach trwałe Kubernetes, zobacz [woluminy trwałe Kubernetes][kubernetes-volumes].

## <a name="built-in-storage-classes"></a>Wbudowane klasy magazynu

Klasa magazynu jest używany do definiowania sposobu skonfigurowania trwały wolumin utworzony dynamicznie. Aby uzyskać więcej informacji na Kubernetes klasy magazynu, zobacz [klasy magazynu Kubernetes][kubernetes-storage-classes].

Każdy klaster AKS zawiera dwie klasy magazynu wstępnie utworzone, skonfigurowane do pracy z dysku systemu Azure. Użyj `kubectl get storageclass` polecenie, aby je wyświetlić.

```console
NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

Jeśli te klasy magazynu działa Twoje potrzeby, Utwórz nową nie jest konieczne.

## <a name="create-storage-class"></a>Tworzenie klasy magazynu

Jeśli chcesz utworzyć klasę magazynu skonfigurowane dla dysku systemu Azure, możesz to zrobić przy użyciu następujących manifest próbki. 

`storageaccounttype` Wartość `Standard_LRS` wskazuje, że tworzony jest dysk standardowy. Ta wartość może być zmieniona na `Premium_LRS` utworzyć [dysku premium][premium-storage]. Aby używać dysków premium, maszyna wirtualna węzła AKS musi mieć rozmiar zgodny z dysków w warstwie premium. Zobacz [tego dokumentu] [ premium-storage] listę rozmiary zgodne.

```yaml
apiVersion: storage.k8s.io/v1beta1
kind: StorageClass
metadata:
  name: azure-managed-disk
provisioner: kubernetes.io/azure-disk
parameters:
  kind: Managed
  storageaccounttype: Standard_LRS
```

## <a name="create-persistent-volume-claim"></a>Tworzenie oświadczeń trwały wolumin

Oświadczenie trwały wolumin używa obiektu klasy magazynu w celu dynamicznie inicjują obsługę część magazynu. Korzystając z dysku platformy Azure, dysk jest tworzony w tej samej grupie zasobów co AKS zasobów.

Manifest ten przykład tworzy oświadczenie trwały wolumin przy użyciu `azure-managed-disk` Klasa magazynu, aby utworzyć dysk `5GB` rozmiaru w `ReadWriteOnce` dostępu. Aby uzyskać więcej informacji na PVC tryby dostępu, zobacz [tryby dostępu do][access-modes].

> [!NOTE]
> Dysku platformy Azure może być instalowany tylko z typem tryb dostępu ReadWriteOnce, dzięki czemu można tylko jeden węzeł AKS. W przypadku konieczności nie współużytkują woluminu trwałego na wielu węzłach, warto rozważyć użycie [plików Azure][azure-files-pvc]. 

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
  annotations:
    volume.beta.kubernetes.io/storage-class: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

## <a name="using-the-persistent-volume"></a>Przy użyciu trwały wolumin

Po oświadczeń trwały wolumin został utworzony i dysku pomyślnie zainicjowano obsługę administracyjną, pod można tworzyć za pomocą dostępu do dysku. Manifest następujących tworzy pod, używającej oświadczeń trwały wolumin `azure-managed-disk` do zainstalowania dysku platformy Azure na `/var/www/html` ścieżki. 

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
        claimName: azure-managed-disk
```

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się więcej o Kubernetes woluminy trwałe przy użyciu dysku systemu Azure.

> [!div class="nextstepaction"]
> [Dodatek plug-in Kubernetes dla dysku systemu Azure][kubernetes-disk]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubernetes-disk]: https://kubernetes.io/docs/concepts/storage/storage-classes/#new-azure-disk-storage-class-starting-from-v172
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md