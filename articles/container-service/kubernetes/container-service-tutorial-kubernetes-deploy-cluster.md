---
title: "Samouczek usługi kontenera platformy Azure — wdrażanie klastra"
description: "Samouczek usługi kontenera platformy Azure — wdrażanie klastra"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c91eea3734820239187bcf7b497fb06d7fd5f7ef
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a>Wdrażanie klastra Kubernetes usługi kontenera platformy Azure

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Usługa Kubernetes zapewnia rozproszoną platformę dla konteneryzowanych aplikacji. Z usługi kontenera platformy Azure inicjowania obsługi klastra produkcyjnego gotowe Kubernetes jest proste i szybkie. W tym samouczku, część 3 7, wdrożono klaster Kubernetes usługi kontenera platformy Azure. Ukończono kroki obejmują:

> [!div class="checklist"]
> * Wdrażanie klastra usług ACS Kubernetes
> * Instalacja Kubernetes interfejsu wiersza polecenia (kubectl)
> * Konfiguracja kubectl

W kolejnych samouczkach aplikacji Azure głos jest wdrożone w klastrze, skalowania, aktualizacji i Operations Management Suite jest skonfigurowany do monitorowania klastrów Kubernetes.

## <a name="before-you-begin"></a>Przed rozpoczęciem

W poprzednich samouczki obrazu kontenera utworzono i przekazywanych do wystąpienia rejestru kontenera platformy Azure. Jeśli nie zostało wykonane następujące kroki, a następnie zostać z niego skorzystać, wróć do [samouczek 1 — Tworzenie kontenera obrazów](./container-service-tutorial-kubernetes-prepare-app.md).

## <a name="create-kubernetes-cluster"></a>Tworzenie klastra Kubernetes

Utwórz klaster Kubernetes w usłudze Azure Container Service za pomocą polecenia [az acs create](/cli/azure/acs#create). 

W poniższym przykładzie tworzony klaster o nazwie `myK8sCluster` w grupie zasobów o nazwie `myResourceGroup`. Ta grupa zasobów została utworzona w [samouczek poprzedniej](./container-service-tutorial-kubernetes-prepare-acr.md).

```azurecli-interactive 
az acs create --orchestrator-type kubernetes --resource-group myResourceGroup --name myK8SCluster --generate-ssh-keys 
```

W niektórych przypadkach, np. ograniczonej wersji próbnej, subskrypcja platformy Azure ma ograniczony dostęp do zasobów platformy Azure. Jeśli wdrożenie nie powiedzie się z powodu ograniczonej liczby dostępnych rdzeni, zmniejsz domyślną liczbę agentów, dodając `--agent-count 1` do polecenia [az acs create](/cli/azure/acs#create). 

Po kilku minutach kończy wdrożenia, i w formacie json zwraca informacje dotyczące wdrażania usług ACS.

## <a name="install-the-kubectl-cli"></a>Zainstaluj kubectl interfejsu wiersza polecenia

Aby połączyć się z klastrem Kubernetes z komputera klienta, należy użyć [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), Kubernetes klient wiersza polecenia. 

Jeśli korzystasz z usługi Azure CloudShell, narzędzie kubectl jest już zainstalowane. Aby zainstalować ją lokalnie, należy użyć [az kubernetes acs install-cli](/cli/azure/acs/kubernetes#install-cli) polecenia.

Uruchomiona w systemie Linux lub macOS, może być konieczne uruchomienie z sudo. W systemie Windows upewnij się, że powłoki zostało uruchomione jako administrator.

```azurecli-interactive 
az acs kubernetes install-cli 
```

W systemie Windows, instalacja domyślna jest *c:\program files (x86)\kubectl.exe*. Należy dodać ten plik do ścieżki systemu Windows. 

## <a name="connect-with-kubectl"></a>Nawiązywanie połączenia przy użyciu narzędzia kubectl

Aby skonfigurować narzędzie kubectl w celu nawiązania połączenia z klastrem Kubernetes, uruchom polecenie [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials).

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group myResourceGroup --name myK8SCluster
```

Aby sprawdzić połączenie z klastrem, uruchom [kubectl uzyskać węzłów](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) polecenia.

```azurecli-interactive
kubectl get nodes
```

Dane wyjściowe:

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

Po ukończeniu samouczka masz klastry ACS Kubernetes dla obciążeń. W kolejnych samouczkach aplikacji kontenera wielu jest wdrażane do tego klastra, skalowana w poziomie, aktualizacji i monitorowane.

## <a name="next-steps"></a>Następne kroki

W tym samouczku klastra Kubernetes usługi kontenera platformy Azure została wdrożona. Wykonano następujące czynności:

> [!div class="checklist"]
> * Wdrożone klastra Kubernetes ACS
> * Zainstalowane Kubernetes interfejsu wiersza polecenia (kubectl)
> * Skonfigurowany kubectl

Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat uruchamiania aplikacji w klastrze.

> [!div class="nextstepaction"]
> [Wdrażanie aplikacji w Kubernetes](./container-service-tutorial-kubernetes-deploy-application.md)
