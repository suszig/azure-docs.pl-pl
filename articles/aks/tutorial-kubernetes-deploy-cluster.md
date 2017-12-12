---
title: "Kubernetes na Azure samouczek — wdrażanie klastra"
description: "Samouczek AKS — wdrażanie klastra"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2c2318d9a5f72800f9cfbd430dca448fd1e5746f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="deploy-an-azure-container-service-aks-cluster"></a>Wdrażanie klastra usługi kontenera platformy Azure (AKS)

Usługa Kubernetes zapewnia rozproszoną platformę dla konteneryzowanych aplikacji. Z AKS inicjowania obsługi klastra produkcyjnego gotowe Kubernetes jest proste i szybkie. W tym samouczku części trzech ośmiu klastra Kubernetes zostanie wdrożona w AKS. Ukończono kroki obejmują:

> [!div class="checklist"]
> * Wdrażanie klastra Kubernetes AKS
> * Instalacja Kubernetes interfejsu wiersza polecenia (kubectl)
> * Konfiguracja kubectl

W kolejnych samouczkach aplikacji Azure głos jest wdrożone w klastrze, skalowania, aktualizacji i Operations Management Suite jest skonfigurowany do monitorowania klastrów Kubernetes.

## <a name="before-you-begin"></a>Przed rozpoczęciem

W poprzednich samouczki obrazu kontenera utworzono i przekazywanych do wystąpienia rejestru kontenera platformy Azure. Jeśli nie zostało wykonane następujące kroki, a następnie zostać z niego skorzystać, wróć do [samouczek 1 — Tworzenie kontenera obrazy][aks-tutorial-prepare-app].

## <a name="enabling-aks-preview-for-your-azure-subscription"></a>Włączenie subskrypcji platformy Azure w wersji zapoznawczej AKS
Gdy AKS jest w wersji zapoznawczej, tworzenia nowych klastrów wymaga flagi funkcji w ramach subskrypcji. Aby zażądać tej funkcji dla dowolnej liczby subskrypcje, które chcesz użyć. Użyj `az provider register` polecenie, aby zarejestrować dostawcę AKS:

```azurecli-interactive
az provider register -n Microsoft.ContainerService
```

Po zarejestrowaniu możesz teraz przystąpić do tworzenia klastra Kubernetes z AKS.

## <a name="create-kubernetes-cluster"></a>Tworzenie klastra Kubernetes

W poniższym przykładzie tworzony klaster o nazwie `myK8sCluster` w grupie zasobów o nazwie `myResourceGroup`. Ta grupa zasobów została utworzona w [poprzedniego samouczek][aks-tutorial-prepare-acr].

```azurecli
az aks create --resource-group myResourceGroup --name myK8sCluster --node-count 1 --generate-ssh-keys
```

Po kilku minutach ukończeniu wdrożenia i informacji o wdrażaniu AKS w formacie json zwraca.

## <a name="install-the-kubectl-cli"></a>Zainstaluj kubectl interfejsu wiersza polecenia

Aby połączyć się z klastrem Kubernetes z komputera klienta, należy użyć [kubectl][kubectl], Kubernetes klient wiersza polecenia.

Jeśli korzystasz z usługi Azure CloudShell, narzędzie kubectl jest już zainstalowane. Jeśli użytkownik chce go zainstalować lokalnie, uruchom następujące polecenie:

```azurecli
az aks install-cli
```

## <a name="connect-with-kubectl"></a>Nawiązywanie połączenia przy użyciu narzędzia kubectl

Aby skonfigurować kubectl do nawiązania połączenia z klastrem Kubernetes, uruchom następujące polecenie:

```azurecli
az aks get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

Aby sprawdzić połączenie z klastrem, uruchom [kubectl uzyskać węzłów] [ kubectl-get] polecenia.

```azurecli
kubectl get nodes
```

Dane wyjściowe:

```
NAME                          STATUS    AGE       VERSION
k8s-myk8scluster-36346190-0   Ready     49m       v1.7.7
```

Po ukończeniu samouczka masz klastry AKS dla obciążeń. W kolejnych samouczkach aplikacji kontenera wielu jest wdrażane do tego klastra, skalowana w poziomie, aktualizacji i monitorowane.

## <a name="next-steps"></a>Następne kroki

W tym samouczku klastra Kubernetes został wdrożony w AKS. Wykonano następujące czynności:

> [!div class="checklist"]
> * Wdrożone klastra Kubernetes AKS
> * Zainstalowane Kubernetes interfejsu wiersza polecenia (kubectl)
> * Skonfigurowany kubectl

Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat uruchamiania aplikacji w klastrze.

> [!div class="nextstepaction"]
> [Wdrażanie aplikacji w Kubernetes][aks-tutorial-deploy-app]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-acr]: ./tutorial-kubernetes-prepare-acr.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md