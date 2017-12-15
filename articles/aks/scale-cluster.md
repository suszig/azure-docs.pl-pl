---
title: "Skalowanie klastra usługi Azure Container Service (AKS) | Microsoft Docs"
description: "Skalowanie klastra usługi Azure Container Service (AKS)."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: Kubernetes, Docker, Containers, Microservices, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: b2fa3ebb7a22b9d19678d45cc50806627ab80e90
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="scale-an-azure-container-service-aks-cluster"></a>Skalowanie klastra usługi Azure Container Service (AKS)

Skalowanie klastra usługi AKS do innej liczby węzłów jest łatwe.  Wybierz żądaną liczbę węzłów i uruchom polecenie `az aks scale`.  Podczas skalowania w dół węzły będą dokładnie [odizolowywane i opróżniane](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), aby zminimalizować zakłócenia dla działających aplikacji.  Podczas skalowania w górę polecenie `az` czeka, aż węzły zostaną oznaczone jako `Ready` przez klaster Kubernetes.

## <a name="scale-the-cluster-nodes"></a>Skalowanie węzłów klastra

Użyj polecenia `az aks scale` w celu skalowania węzłów klastra. W poniższym przykładzie pokazano skalowanie klastra o nazwie *myK8SCluster* do jednego węzła.

```azurecli-interactive
az aks scale --name myK8sCluster --resource-group myResourceGroup --node-count 1
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
    "kubernetesVersion": "1.7.7",
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

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat wdrażania usługi AKS i zarządzania nią z samouczków usługi AKS.

> [!div class="nextstepaction"]
> [Samouczek usługi AKS](./tutorial-kubernetes-prepare-app.md)
