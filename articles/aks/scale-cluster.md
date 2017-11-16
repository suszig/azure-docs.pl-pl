---
title: "Skalowanie klastra usługi kontenera platformy Azure (AKS) | Dokumentacja firmy Microsoft"
description: "Skalowanie klastra usługi kontenera platformy Azure (AKS)."
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
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="scale-an-azure-container-service-aks-cluster"></a>Skalowanie klastra usługi kontenera platformy Azure (AKS)

Jest łatwy do skalowania liczby węzłów klastra AKS.  Wybierz liczbę węzłów i uruchom `az aks scale` polecenia.  Podczas skalowania, węzły będą dokładnie [cordoned i opróżnione](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/) aby zminimalizować zakłócenia dla aplikacji.  Podczas skalowania w górę, `az` polecenie ma czekać, aż węzły są oznaczone `Ready` przez klaster Kubernetes.

## <a name="scale-the-cluster-nodes"></a>Skalowanie węzłów klastra

Użyj `az aks scale` polecenie, aby skalować węzłów klastra. Poniższy przykład skaluje klastra o nazwie *myK8SCluster* do jednego węzła.

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

Dowiedz się więcej na temat wdrażania i zarządzania nimi AKS z samouczki AKS.

> [!div class="nextstepaction"]
> [Samouczek AKS](./tutorial-kubernetes-prepare-app.md)
