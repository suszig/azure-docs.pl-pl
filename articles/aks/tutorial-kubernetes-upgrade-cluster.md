---
title: "Kubernetes na Azure samouczek — aktualizacji klastra"
description: "Kubernetes na Azure samouczek — aktualizacji klastra"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5fd9a1890c1940cdd4e79cc32e0b3984edd043e8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="upgrade-kubernetes-in-azure-container-service-aks"></a>Uaktualnij Kubernetes w usłudze kontenera platformy Azure (AKS)

Klaster usługi kontenera platformy Azure (AKS) można uaktualnić za pomocą wiersza polecenia platformy Azure. Podczas procesu uaktualniania Kubernetes węzły są dokładnie [cordoned i opróżnione] [ kubernetes-drain] aby zminimalizować zakłócenia dla aplikacji.

W tym samouczku część osiem osiem, trwa uaktualnianie klastra Kubernetes. Zadania, które należy wykonać obejmują:

> [!div class="checklist"]
> * Zidentyfikuj bieżącego i dostępne wersje Kubernetes
> * Uaktualnij węzły Kubernetes
> * Sprawdź poprawność pomyślnego uaktualnienia

## <a name="before-you-begin"></a>Przed rozpoczęciem

W samouczkach poprzedniej aplikacji zostało umieszczone w obraz kontenera, ten obraz przekazany do rejestru kontenera Azure i utworzyć klaster Kubernetes. W klastrze Kubernetes następnie uruchomienia aplikacji.

Jeśli nie zostało wykonane następujące kroki, a następnie zostać z niego skorzystać, wróć do [samouczek 1 — Tworzenie kontenera obrazy][aks-tutorial-prepare-app].


## <a name="get-cluster-versions"></a>Pobieranie wersji klastra

Przed uaktualnieniem klastra użyj polecenia `az aks get-versions`, aby sprawdzić, która wersja rozwiązania Kubernetes jest dostępna do uaktualnienia.

```azurecli-interactive
az aks get-versions --name myK8sCluster --resource-group myResourceGroup --output table
```

Tutaj można zobaczyć, że bieżąca wersja węzła jest `1.7.7` i tej wersji `1.7.9`, `1.8.1`, i `1.8.2` są dostępne.

```
Name     ResourceGroup    MasterVersion    MasterUpgrades       NodePoolVersion     NodePoolUpgrades
-------  ---------------  ---------------  -------------------  ------------------  -------------------
default  myAKSCluster     1.7.7            1.8.2, 1.7.9, 1.8.1  1.7.7               1.8.2, 1.7.9, 1.8.1
```

## <a name="upgrade-cluster"></a>Uaktualnienia klastra

Użyj `az aks upgrade` polecenie Uaktualnij węzły klastra. Poniższe przykłady aktualizuje klastra do wersji `1.8.2`.

```azurecli-interactive
az aks upgrade --name myK8sCluster --resource-group myResourceGroup --kubernetes-version 1.8.2
```

Dane wyjściowe:

```json
{
  "id": "/subscriptions/4f48eeae-9347-40c5-897b-46af1b8811ec/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myK8sCluster",
  "location": "eastus",
  "name": "myK8sCluster",
  "properties": {
    "accessProfiles": {
      "clusterAdmin": {
        "kubeConfig": "..."
      },
      "clusterUser": {
        "kubeConfig": "..."
      }
    },
    "agentPoolProfiles": [
      {
        "count": 1,
        "dnsPrefix": null,
        "fqdn": null,
        "name": "myK8sCluster",
        "osDiskSizeGb": null,
        "osType": "Linux",
        "ports": null,
        "storageProfile": "ManagedDisks",
        "vmSize": "Standard_D2_v2",
        "vnetSubnetId": null
      }
    ],
    "dnsPrefix": "myK8sClust-myResourceGroup-4f48ee",
    "fqdn": "myk8sclust-myresourcegroup-4f48ee-406cc140.hcp.eastus.azmk8s.io",
    "kubernetesVersion": "1.8.2",
    "linuxProfile": {
      "adminUsername": "azureuser",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "..."
          }
        ]
      }
    },
    "provisioningState": "Succeeded",
    "servicePrincipalProfile": {
      "clientId": "e70c1c1c-0ca4-4e0a-be5e-aea5225af017",
      "keyVaultSecretRef": null,
      "secret": null
    }
  },
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="validate-upgrade"></a>Sprawdź poprawność uaktualnienia

Teraz możesz potwierdzić, że uaktualnienie powiodło się, używając polecenia `az aks show`.

```azurecli-interactive
az aks show --name myK8sCluster --resource-group myResourceGroup --output table
```

Dane wyjściowe:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myK8sCluster  eastus     myResourceGroup  1.8.2                Succeeded            myk8sclust-myresourcegroup-3762d8-2f6ca801.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku zostanie uaktualniony Kubernetes AKS klastra. Zakończono następujące zadania:

> [!div class="checklist"]
> * Zidentyfikuj bieżącego i dostępne wersje Kubernetes
> * Uaktualnij węzły Kubernetes
> * Sprawdź poprawność pomyślnego uaktualnienia

Wykonaj to łącze, aby dowiedzieć się więcej o AKS.

> [!div class="nextstepaction"]
> [Omówienie AKS][aks-intro]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-intro]: ./intro-kubernetes.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md