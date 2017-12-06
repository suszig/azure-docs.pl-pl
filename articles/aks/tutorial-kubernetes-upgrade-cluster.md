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
ms.openlocfilehash: aa457c97292fc9f97d3bc4769ca45d55dd5829a6
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="upgrade-kubernetes-in-azure-container-service-aks"></a>Uaktualnij Kubernetes w usłudze kontenera platformy Azure (AKS)

Klaster usługi kontenera platformy Azure (AKS) można uaktualnić za pomocą wiersza polecenia platformy Azure. Podczas procesu uaktualniania Kubernetes węzły są dokładnie [cordoned i opróżnione](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/) aby zminimalizować zakłócenia dla aplikacji.

W tym samouczku część osiem osiem, trwa uaktualnianie klastra Kubernetes. Zadania, które należy wykonać obejmują:

> [!div class="checklist"]
> * Zidentyfikuj bieżącego i dostępne wersje Kubernetes
> * Uaktualnij węzły Kubernetes
> * Sprawdź poprawność pomyślnego uaktualnienia

## <a name="before-you-begin"></a>Przed rozpoczęciem

W samouczkach poprzedniej aplikacji zostało umieszczone w obraz kontenera, ten obraz przekazany do rejestru kontenera Azure i utworzyć klaster Kubernetes. W klastrze Kubernetes następnie uruchomienia aplikacji.

Jeśli nie zostało wykonane następujące kroki, a następnie zostać z niego skorzystać, wróć do [samouczek 1 — Tworzenie kontenera obrazów](./tutorial-kubernetes-prepare-app.md).


## <a name="get-cluster-versions"></a>Pobieranie wersji klastra

Przed rozpoczęciem uaktualniania klastra, użyj `az aks get-versions` polecenie, aby sprawdzić, które wersje Kubernetes są dostępne do uaktualnienia.

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

Teraz można potwierdzić uaktualnienie powiodło się z `az aks show` polecenia.

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
> [Omówienie AKS](./intro-kubernetes.md)