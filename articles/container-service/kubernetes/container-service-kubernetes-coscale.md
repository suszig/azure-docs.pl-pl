---
title: Monitor klaster Azure Kubernetes z CoScale
description: "Monitor Kubernetes klastra usługi kontenera platformy Azure przy użyciu CoScale"
services: container-service
author: fryckbos
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 2d6757397d76b6ca87a45254cb31f34d34a42541
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a>Monitor klaster Kubernetes usługi kontenera platformy Azure z CoScale

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

W tym artykule zostanie przedstawiony zostanie sposób wdrażania [CoScale](https://www.coscale.com/) agenta monitorowania wszystkich węzłów i kontenerów w Kubernetes klastra usługi kontenera platformy Azure. Musisz mieć konto z CoScale dla tej konfiguracji. 


## <a name="about-coscale"></a>O CoScale 

CoScale to platforma monitorowania, która zbiera metryki i zdarzenia z wszystkich kontenerów w różnych platform aranżacji. CoScale oferuje pełnego stosu monitorowania dla środowisk Kubernetes. Zapewnia wszystkie warstwy stosu wizualizacje i analiza: systemu operacyjnego, Kubernetes, Docker i aplikacji uruchamianych wewnątrz kontenerów. CoScale oferuje kilka wbudowanych pulpity nawigacyjne monitorowania i ma wykrywania anomalii wbudowanych, aby umożliwić operatory i deweloperom szybko znaleźć problemy związane z infrastrukturą i aplikacji.

![CoScale interfejsu użytkownika](./media/container-service-kubernetes-coscale/coscale.png)

Jak pokazano w tym artykule, można zainstalować agentów na działanie jako rozwiązaniem SaaS CoScale klastra Kubernetes. Jeśli chcesz zachować dane na miejscu, CoScale jest również dostępny do instalacji lokalnej.


## <a name="prerequisites"></a>Wymagania wstępne

Należy najpierw [Utwórz konto CoScale](https://www.coscale.com/free-trial).

W tym przewodniku założono, że [utworzony klaster Kubernetes za pomocą usługi kontenera platformy Azure](container-service-kubernetes-walkthrough.md).

Założono również, że masz `az` wiersza polecenia platformy Azure i `kubectl` narzędzia są zainstalowane.

Możesz przetestować, jeśli masz `az` zainstalowany, uruchamiając narzędzie:

```azurecli
az --version
```

Jeśli nie masz `az` narzędzie zainstalowane, nie ma instrukcji [tutaj](/cli/azure/install-azure-cli).

Możesz przetestować, jeśli masz `kubectl` zainstalowany, uruchamiając narzędzie:

```bash
kubectl version
```

Jeśli nie masz `kubectl` zainstalowany, możesz uruchomić:

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-the-coscale-agent-with-a-daemonset"></a>Instalowanie agenta CoScale z DaemonSet
[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) są używane przez Kubernetes do uruchomienia pojedynczego wystąpienia kontenera na każdym hoście w klastrze.
Są one idealne w przypadku uruchamiania agenci monitorowania, takich jak CoScale agent.

Po zalogowaniu się na CoScale, przejdź do [agent strona](https://app.coscale.com/) do instalacji agentów CoScale w klastrze za pomocą DaemonSet. CoScale interfejsu użytkownika zawiera przewodnik konfiguracji kroki, aby utworzyć agenta i rozpocząć monitorowanie pełną Kubernetes klastra.

![CoScale konfiguracji agenta](./media/container-service-kubernetes-coscale/installation.png)

Aby uruchomić agenta w klastrze, uruchom polecenie dostarczony:

![Uruchom agenta CoScale](./media/container-service-kubernetes-coscale/agent_script.png)

Gotowe. Po skonfigurowaniu i uruchomieniu agentów danych w konsoli powinna zostać wyświetlona za kilka minut. Odwiedź stronę [agent strona](https://app.coscale.com/) Aby wyświetlić podsumowanie klastra, wykonanie dodatkowych kroków konfiguracji i znaleźć pulpitów nawigacyjnych, takich jak **Kubernetes klastra omówienie**.

![Omówienie klastrów Kubernetes](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

CoScale agent jest automatycznie wdrażane na nowych komputerach w klastrze. Aktualizacje agenta automatycznie po wydaniu nowej wersji.


## <a name="next-steps"></a>Następne kroki

Zobacz [CoScale dokumentacji](http://docs.coscale.com/) i [blogu](https://www.coscale.com/blog) uzyskać więcej informacji o CoScale monitorowanie rozwiązań. 

