---
title: "Uaktualnianie klastra usługi Azure Container Service (AKS)"
description: "Uaktualnianie klastra usługi Azure Container Service (AKS)"
services: container-service
author: gabrtv
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/15/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 92c4850f623aea331e9834b5c8da66a7de34107f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="upgrade-an-azure-container-service-aks-cluster"></a>Uaktualnianie klastra usługi Azure Container Service (AKS)

Usługa Azure Container Service (AKS) ułatwia wykonywanie typowych zadań zarządzania, w tym uaktualniania klastrów Kubernetes.

## <a name="upgrade-an-aks-cluster"></a>Uaktualnianie klastra AKS

Przed uaktualnieniem klastra użyj polecenia `az aks get-versions`, aby sprawdzić, która wersja rozwiązania Kubernetes jest dostępna do uaktualnienia.

```azurecli-interactive
az aks get-versions --name myK8sCluster --resource-group myResourceGroup --output table
```

Dane wyjściowe:

```console
Name     ResourceGroup    MasterVersion    MasterUpgrades       NodePoolVersion     NodePoolUpgrades
-------  ---------------  ---------------  -------------------  ------------------  -------------------
default  myResourceGroup  1.7.7            1.8.2, 1.7.9, 1.8.1  1.7.7               1.8.2, 1.7.9, 1.8.1
```

Mamy trzy wersje dostępne do uaktualnienia: 1.7.9, 1.8.1 i 1.8.2. Możemy użyć polecenia `az aks upgrade` w celu przeprowadzenia uaktualnienia do najnowszej dostępnej wersji.  Podczas procesu uaktualniania węzły są dokładnie [cordoned i opróżnione] [ kubernetes-drain] aby zminimalizować zakłócenia dla aplikacji.  Przed zainicjowaniem uaktualniania klastra upewnij się, że masz na tyle dużo dodatkowej wydajności obliczeniowej, aby obsłużyć obciążenie podczas dodawania i usuwania węzłów klastra.

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

Dowiedz się więcej na temat wdrażania usługi AKS i zarządzania nią z samouczków usługi AKS.

> [!div class="nextstepaction"]
> [Samouczek AKS][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md