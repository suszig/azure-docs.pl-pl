---
title: "SSH w węzłach klastra usługi kontenera platformy Azure (AKS)"
description: "Utwórz połączenie SSH z klastrem usługi kontenera platformy Azure (AKS) węzły"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/28/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 00affc3d1c02c477826261aeac6e092934037e81
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="ssh-into-azure-container-service-aks-cluster-nodes"></a>SSH w węzłach klastra usługi kontenera platformy Azure (AKS)

Czasami może być konieczne dostępu węzła usługi kontenera platformy Azure (AKS) konserwacji, zbierania dzienników lub inne operacje dotyczące rozwiązywania problemów. Azure usługi kontenera (AKS) węzły nie są widoczne w Internecie. Wykonaj kroki szczegółowo opisane w tym dokumencie, aby utworzyć połączenie SSH za pomocą węzła AKS.

## <a name="configure-ssh-access"></a>Konfigurowanie dostępu SSH

 Aby SSH do określonego węzła, pod jest tworzony z `hostNetwork` dostępu. Tworzona jest również usługą pod dostępu. Ta konfiguracja jest uprzywilejowany i powinna zostać usunięta po użyciu.

Utwórz plik o nazwie `aks-ssh.yaml` i skopiuj w tym manifeście. Aktualizacja nazwy węzła o nazwie AKS węzeł docelowy.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: aks-ssh
spec:
  selector:
    app: aks-ssh
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 22
    targetPort: 22
---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: aks-ssh
  labels:
    app: aks-ssh
spec:
  replicas: 1
  selector:
    matchLabels:
      app: aks-ssh
  template:
    metadata:
      labels:
        app: aks-ssh
    spec:
      containers:
      - name: alpine
        image: alpine:latest
        ports:
        - containerPort: 22
        command: ["/bin/sh", "-c", "--"]
        args: ["while true; do sleep 30; done;"]
      hostNetwork: true
      nodeName: aks-nodepool1-42032720-0
```

Uruchom manifest, aby utworzyć pod i usługi.

```azurecli-interactive
$ kubectl apply -f aks-ssh.yaml
```

Pobierz zewnętrzny adres IP narażonych usługi. Może zająć minutę do konfiguracji adresu IP zakończyć. 

```azurecli-interactive
$ kubectl get service

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)        AGE
kubernetes         ClusterIP      10.0.0.1      <none>          443/TCP        1d
aks-ssh            LoadBalancer   10.0.51.173   13.92.154.191   22:31898/TCP   17m
```

Utwórz ssh połączenia. 

Domyślna nazwa użytkownika dla klastra AKS to `azureuser`. Jeśli to konto zostało zmienione w czasie tworzenia klastra, zamiast nazwy użytkownika administratora właściwe. 

Jeśli klucz nie są obecnie `~/ssh/id_rsa`, podaj przy użyciu poprawnej lokalizacji `ssh -i` argumentu.

```azurecli-interactive
$ ssh azureuser@13.92.154.191

Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.11.0-1016-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

48 packages can be updated.
0 updates are security updates.


*** System restart required ***
Last login: Tue Feb 27 20:10:38 2018 from 167.220.99.8
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@aks-nodepool1-42032720-0:~$
```

## <a name="remove-ssh-access"></a>Usuń dostęp SSH

Po zakończeniu Usuń pod dostępu SSH i usługi.

```azurecli-interactive
kubectl delete -f aks-ssh.yaml
```