---
title: "Tworzenie klastra usługi kontenera platformy Azure (AKS)"
description: Tworzenie klastra AKS z poziomu interfejsu wiersza polecenia lub w portalu Azure.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 02/12/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3e884bc16680d74801911547045deb48246afccd
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/13/2018
---
# <a name="create-an-azure-container-service-aks-cluster"></a>Tworzenie klastra usługi kontenera platformy Azure (AKS)

Klaster usługi kontenera platformy Azure (AKS) mogą być tworzone z wiersza polecenia platformy Azure lub w portalu Azure.

## <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

Użyj [utworzyć az aks] [ az-aks-create] polecenie, aby usunąć klaster AKS.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

Poniższe opcje są dostępne z `az aks create` polecenia.

| Argument | Opis | Wymagane |
|---|---|:---:|
| `--name` `-n` | Nazwa zasobu zarządzanego klastra. | tak |
| `--resource-group` `-g` | Nazwa grupy zasobów usługi kontenera platformy Azure. | tak |
| `--admin-username` `-u` | Nazwa użytkownika dla maszyn wirtualnych systemu Linux.  Domyślne: azureuser. | nie |
| ` --client-secret` | Klucz tajny skojarzony z nazwy głównej usługi. | nie |
| `--dns-name-prefix` `-p` | Prefiks DNS dla adresu publicznego adresu ip klastrów. | nie |
| `--generate-ssh-keys` | Generuj SSH publicznego i prywatnego klucza plików, jeśli brak. | nie |
| `--kubernetes-version` `-k` | Wersja Kubernetes do użycia podczas tworzenia klastra, takie jak "1.7.9" lub "1.8.2".  Default: 1.7.7. | nie |
| `--no-wait` | Nie należy czekać na zakończenie operacji długotrwałych. | nie |
| `--node-count` `-c` | Domyślna liczba węzłów dla pul węzła.  Domyślne: 3. | nie |
| `--node-osdisk-size` | Parametr osDisk rozmiar w GB puli węzła maszyny wirtualnej. | nie |
| `--node-vm-size` `-s` | Rozmiar maszyny wirtualnej.  Domyślne: Standard_D1_v2. | nie |
| `--service-principal` | Nazwy głównej usługi używany do uwierzytelniania klastra. | nie |
| `--ssh-key-value` | SSH pliku klucza wartość lub klucz ścieżka pliku.  Wartość domyślna: ~ /.ssh/id_rsa.pub. | nie |
| `--tags` | Tagi [= value] klucza spacjami format. Użyj "Wyczyść istniejące znaczniki. | nie |

## <a name="azure-portal"></a>Azure Portal

Aby uzyskać instrukcje dotyczące wdrażania klastra AKS z portalu Azure, zobacz usługi kontenera platformy Azure (AKS) [Azure Szybki Start portalu][aks-portal-quickstart]. 

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az_aks_create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md