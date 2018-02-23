---
title: "Samouczek dotyczący rozwiązania Kubernetes na platformie Azure — wdrażanie klastra"
description: "Samouczek dotyczący usługi AKS — wdrażanie klastra"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: e0d5bd57a40fca837ead42e691e1fa0c802dc013
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="deploy-an-azure-container-service-aks-cluster"></a>Wdrażanie klastra usługi Azure Container Service (AKS)

Usługa Kubernetes zapewnia rozproszoną platformę dla konteneryzowanych aplikacji. Usługa AKS umożliwia łatwe i szybkie aprowizowanie produkcyjnego klastra Kubernetes. W tym samouczku (część trzecia z ośmiu) w usłudze AKS jest wdrażany klaster Kubernetes. Wykonano następujące czynności:

> [!div class="checklist"]
> * Wdrażanie klastra AKS rozwiązania Kubernetes
> * Instalacja interfejsu wiersza polecenia rozwiązania Kubernetes (kubectl)
> * Konfiguracja narzędzia kubectl

W kolejnych samouczkach aplikacja do głosowania platformy Azure będzie wdrażana do klastra, skalowana i aktualizowana, a pakiet Operations Management Suite zostanie skonfigurowany do monitorowania klastra usługi Kubernetes.

## <a name="before-you-begin"></a>Przed rozpoczęciem

W poprzednich samouczkach utworzono obraz kontenera i przekazano go do wystąpienia usługi Azure Container Registry. Jeśli nie wykonano tych kroków, a chcesz kontynuować pracę, wróć do części [Samouczek 1 — tworzenie obrazów kontenera][aks-tutorial-prepare-app].

## <a name="enabling-aks-preview-for-your-azure-subscription"></a>Włączanie wersji zapoznawczej usługi AKS dla subskrypcji platformy Azure
W przypadku usługi AKS w wersji zapoznawczej tworzenie nowych klastrów wymaga flagi funkcji w ramach subskrypcji. Możesz zażądać tej funkcji dla dowolnej liczby subskrypcji, z których chcesz skorzystać. Użyj polecenia `az provider register`, aby zarejestrować dostawcę usług AKS:

```azurecli-interactive
az provider register -n Microsoft.ContainerService
```

Po zarejestrowaniu możesz przystąpić do tworzenia klastra Kubernetes za pomocą usługi AKS.

## <a name="create-kubernetes-cluster"></a>Tworzenie klastra Kubernetes

Poniższy przykład obejmuje tworzenie klastra o nazwie `myAKSCluster` w grupie zasobów o nazwie `myResourceGroup`. Ta grupa zasobów została utworzona w ramach [poprzedniego samouczka][aks-tutorial-prepare-acr].

```azurecli
az aks create --resource-group myResourceGroup --name myAKSCluster --node-count 1 --generate-ssh-keys
```

Po kilku minutach wdrażanie zostanie zakończone i zwróci informacje o wdrożeniu usługi AKS w formacie JSON.

## <a name="install-the-kubectl-cli"></a>Instalowanie interfejsu wiersza polecenia kubectl

Aby nawiązać połączenie z klastrem Kubernetes z komputera klienckiego, należy użyć narzędzia [kubectl][kubectl], czyli klienta wiersza polecenia usługi Kubernetes.

Jeśli korzystasz z usługi Azure CloudShell, narzędzie kubectl jest już zainstalowane. Jeśli chcesz zainstalować je lokalnie, uruchom następujące polecenie:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>Nawiązywanie połączenia przy użyciu narzędzia kubectl

Aby skonfigurować narzędzie kubectl w celu łączenia się z klastrem Kubernetes, uruchom następujące polecenie:

```azurecli
az aks get-credentials --resource-group=myResourceGroup --name=myAKSCluster
```

Aby sprawdzić połączenie z klastrem, uruchom polecenie [kubectl get nodes][kubectl-get].

```azurecli
kubectl get nodes
```

Dane wyjściowe:

```
NAME                          STATUS    AGE       VERSION
k8s-myAKSCluster-36346190-0   Ready     49m       v1.7.7
```

Po ukończeniu samouczka klaster AKS jest gotowy do użycia z obciążeniami. W kolejnych samouczkach aplikacja wielu kontenerów jest wdrażana do tego klastra, skalowana w poziomie, aktualizowana i monitorowana.

## <a name="next-steps"></a>Następne kroki

W tym samouczku w usłudze AKS został wdrożony klaster Kubernetes. Wykonano następujące czynności:

> [!div class="checklist"]
> * Wdrożony klaster AKS rozwiązania Kubernetes
> * Instalacja interfejsu wiersza polecenia rozwiązania Kubernetes (kubectl)
> * Konfigurowanie narzędzia kubectl

Przejdź do następnego samouczka, aby dowiedzieć się, jak uruchomić aplikację w klastrze.

> [!div class="nextstepaction"]
> [Wdrażanie aplikacji w rozwiązaniu Kubernetes][aks-tutorial-deploy-app]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md