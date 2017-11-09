---
title: "Uaktualnij do klastra usługi kontenera platformy Azure (AKS) | Dokumentacja firmy Microsoft"
description: "Uaktualnianie klastra usługi kontenera platformy Azure (AKS)"
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
ms.date: 10/24/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 0b136953adecbd049c12f102714e23f00ac0d350
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# <a name="upgrade-an-azure-container-service-aks-cluster"></a>Uaktualnianie klastra usługi kontenera platformy Azure (AKS)

Usługa kontenera platformy Azure (AKS) ułatwia wykonywanie typowych zadań zarządzania tym uaktualnianie Kubernetes klastrów.

## <a name="upgrade-an-aks-cluster"></a>Uaktualnianie klastra AKS

Przed rozpoczęciem uaktualniania klastra, użyj `az aks get-versions` polecenie, aby sprawdzić, które wersje Kubernetes są dostępne do uaktualnienia.

```azurecli-interactive
az aks get-versions --name myK8sCluster --resource-group myResourceGroup --output table
```

Dane wyjściowe:

```console
Name     ResourceGroup    MasterVersion    MasterUpgrades       AgentPoolVersion    AgentPoolUpgrades
-------  ---------------  ---------------  -------------------  ------------------  -------------------
default  myResourceGroup  1.7.7            1.8.2, 1.7.9, 1.8.1  1.7.7               1.8.2, 1.7.9, 1.8.1
```

Mamy trzy wersje dostępne do uaktualnienia: 1.7.9, 1.8.1 i 1.8.2. Możemy użyć `az aks upgrade` polecenia w celu uaktualnienia do najnowszej dostępnej wersji.  Podczas procesu uaktualniania węzły są dokładnie [cordoned i opróżnione](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/) aby zminimalizować zakłócenia dla aplikacji.

```azurecli-interactive
az aks upgrade --name myK8sCluster --resource-group myResourceGroup --kubernetes-version 1.8.2
```

Dane wyjściowe:

```json
{
  "id": "/subscriptions/4f48eeae-9347-40c5-897b-46af1b8811ec/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myK8sCluster",
  "location": "westus2",
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
    "fqdn": "myk8sclust-myresourcegroup-4f48ee-406cc140.hcp.westus2.azmk8s.io",
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

Teraz można potwierdzić uaktualnienie powiodło się z `az aks show` polecenia.

```azurecli-interactive
az aks show --name myK8sCluster --resource-group myResourceGroup --output table
```

Dane wyjściowe:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myK8sCluster  westus2     myResourceGroup  1.8.2                Succeeded            myk8sclust-myresourcegroup-3762d8-2f6ca801.hcp.westus2.azmk8s.io
```

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat wdrażania i zarządzania nimi AKS z samouczki AKS.

> [!div class="nextstepaction"]
> [Samouczek AKS](./tutorial-kubernetes-prepare-app.md)