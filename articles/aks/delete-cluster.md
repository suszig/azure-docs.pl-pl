---
title: "Usuń klaster usługi kontenera platformy Azure (AKS)"
description: "Usuń i AKS klastra przy użyciu interfejsu wiersza polecenia lub Azure portalu."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/05/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 78056288f45616eda427f8e708efc679f8a5202c
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/13/2018
---
# <a name="delete-an-azure-container-service-aks-cluster"></a>Usuń klaster usługi kontenera platformy Azure (AKS)

Podczas usuwania klastra usługi kontenera platformy Azure, grupy zasobów, w której wdrożono klaster pozostanie, jednak wszystkie powiązane AKS zasoby zostaną usunięte. Ten dokument pokazuje, jak usunąć klaster AKS przy użyciu wiersza polecenia platformy Azure i portalu Azure. 

Oprócz usunięcie klastra, można usunąć grupy zasobów, w którym została wdrożona, który spowoduje również usunięcie klastra AKS.

## <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

Użyj [usunąć az aks] [ az-aks-delete] polecenie, aby usunąć klaster AKS.

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster
```

Poniższe opcje są dostępne z `az aks delete` polecenia.

| Argument | Opis | Wymagane |
|---|---|:---:|
| `--name` `-n` | Nazwa zasobu zarządzanego klastra. | tak |
| `--resource-group` `-g` | Nazwa grupy zasobów usługi kontenera platformy Azure. | tak |
| `--no-wait` | Nie należy czekać na zakończenie operacji długotrwałych. | nie |
| `--yes` `-y` | Nie monituj o potwierdzenie. | nie |

## <a name="azure-portal"></a>Azure Portal

Znajduje się w portalu Azure, przejdź do grupy zasobów zawiera zasób usługi kontenera platformy Azure (AKS), wybierz zasób i kliknij przycisk **usunąć**. Monit o potwierdzenie operacji usuwania.

![Usuń AKS klastra portalu](media/container-service-delete-cluster/delete-aks-portal.png)

<!-- LINKS - internal -->
[az-aks-delete]: /cli/azure/aks?view=azure-cli-latest#az_aks_delete