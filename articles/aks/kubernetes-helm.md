---
title: "Wdrażanie kontenerów z Helm w Kubernetes na platformie Azure | Dokumentacja firmy Microsoft"
description: "Wdrażanie kontenerów w klastrze Kubernetes w AKS za pomocą narzędzia pakowania Helm"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: de52c2aad0f74b59970234872dfa3e4136929915
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="use-helm-with-azure-container-service-aks"></a>Helm za pomocą usługi kontenera platformy Azure (AKS)

[Helm](https://github.com/kubernetes/helm/) jest narzędziem open source tworzenia pakietów, które pomaga zainstalować i zarządzanie cyklem życia aplikacji Kubernetes. Podobnie jak menedżerów pakietu systemu Linux, takich jak *APT* i *Yum*, Helm służy do zarządzania Kubernetes wykresy, które są pakiety zasobów Kubernetes wstępnie skonfigurowane.

Kroki tego dokumentu, do konfigurowania i używania Helm w klastrze Kubernetes na AKS.

## <a name="before-you-begin"></a>Przed rozpoczęciem

W krokach szczegółowo opisanych w tym dokumencie założono, że klaster usługi AKS został utworzony i że zostało nawiązane połączenie kubectl z klastrem. Jeśli potrzebujesz tych elementów, zobacz [szybki start z usługą AKS](./kubernetes-walkthrough.md).

## <a name="install-helm-cli"></a>Zainstaluj Helm interfejsu wiersza polecenia

Helm interfejsu wiersza polecenia jest klient, który działa w systemie deweloperskim i umożliwia uruchamianie, zatrzymywanie i zarządzania aplikacjami z wykresami Helm.

Jeśli używasz Azure CloudShell Helm CLI jest już zainstalowana. Aby zainstalować Helm interfejsu wiersza polecenia przy użyciu Mac `brew`. Aby uzyskać więcej informacji, zobacz opcje [instalowanie Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md).

```console
brew install kubernetes-helm
```

Dane wyjściowe:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.6.2: 50 files, 132.4MB
```

## <a name="configure-helm"></a>Skonfiguruj Helm

[Helm init](https://docs.helm.sh/helm/#helm-init) polecenie służy do instalowania składników Helm w klastrze Kubernetes i wprowadzić zmiany konfiguracji po stronie klienta. Helm jest wstępnie zainstalowany w klastrach AKS, więc jest potrzebna tylko konfiguracji po stronie klienta. Uruchom następujące polecenie, aby skonfigurować klienta Helm.

```azurecli-interactive
helm init --client-only
```

Dane wyjściowe:

```
$HELM_HOME has been configured at /Users/neilpeterson/.helm.
Not installing Tiller due to 'client-only' flag having been set
Happy Helming!
```

## <a name="find-helm-charts"></a>Znajdź Helm wykresów

Wykresy Helm są używane do wdrażania aplikacji w klastrze Kubernetes. Aby wyszukać utworzone wcześniej Helm wykresy, należy użyć [wyszukiwania helm](https://docs.helm.sh/helm/#helm-search) polecenia.

```azurecli-interactive
helm search
```

Wyszukuje dane wyjściowe podobne do następujących, jednak o wiele więcej wykresy.

```
NAME                            VERSION DESCRIPTION
stable/acs-engine-autoscaler    2.0.0   Scales worker nodes within agent pools
stable/artifactory              6.1.0   Universal Repository Manager supporting all maj...
stable/aws-cluster-autoscaler   0.3.1   Scales worker nodes within autoscaling groups.
stable/buildkite                0.2.0   Agent for Buildkite
stable/centrifugo               2.0.0   Centrifugo is a real-time messaging server.
stable/chaoskube                0.5.0   Chaoskube periodically kills random pods in you...
stable/chronograf               0.3.0   Open-source web application written in Go and R...
stable/cluster-autoscaler       0.2.0   Scales worker nodes within autoscaling groups.
stable/cockroachdb              0.5.0   CockroachDB is a scalable, survivable, strongly...
stable/concourse                0.7.0   Concourse is a simple and scalable CI system.
stable/consul                   0.4.1   Highly available and distributed service discov...
stable/coredns                  0.5.0   CoreDNS is a DNS server that chains middleware ...
stable/coscale                  0.2.0   CoScale Agent
stable/dask-distributed         2.0.0   Distributed computation in Python
stable/datadog                  0.8.0   DataDog Agent
...
```

Aby zaktualizować listę wykresy, należy użyć [helm repozytorium aktualizacji](https://docs.helm.sh/helm/#helm-repo-update) polecenia.

```azurecli-interactive
helm repo update
```

Dane wyjściowe:

```
Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## <a name="run-helm-charts"></a>Uruchom Helm wykresów

Aby wdrożyć kontrolera wejściowych NGINX, należy użyć [instalacji helm](https://docs.helm.sh/helm/#helm-install) polecenia.

```azurecli-interactive
helm install stable/nginx-ingress
```

Dane wyjściowe wygląda podobnie do poniższego, ale zawiera dodatkowe informacje, takie jak instrukcje dotyczące sposobu używania Kubernetes wdrożenia.

```
NAME:   tufted-ocelot
LAST DEPLOYED: Thu Oct  5 00:48:04 2017
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                                    DATA  AGE
tufted-ocelot-nginx-ingress-controller  1     5s

==> v1/Service
NAME                                         CLUSTER-IP   EXTERNAL-IP  PORT(S)                     AGE
tufted-ocelot-nginx-ingress-controller       10.0.140.10  <pending>    80:30486/TCP,443:31358/TCP  5s
tufted-ocelot-nginx-ingress-default-backend  10.0.34.132  <none>       80/TCP                      5s

==> v1beta1/Deployment
NAME                                         DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
tufted-ocelot-nginx-ingress-controller       1        1        1           0          5s
tufted-ocelot-nginx-ingress-default-backend  1        1        1           1          5s
...
```

Aby uzyskać więcej informacji o używaniu NGINX kontrolera transfer danych przychodzących z Kubernetes, zobacz [kontrolera wejściowych NGINX](https://github.com/kubernetes/ingress/tree/master/controllers/nginx).

## <a name="list-helm-charts"></a>Wykresy Helm listy

Aby wyświetlić listę wykresy zainstalowany w klastrze, należy użyć [listy helm](https://docs.helm.sh/helm/#helm-list) polecenia.

```azurecli-interactive
helm list
```

Dane wyjściowe:

```
NAME            REVISION    UPDATED                     STATUS      CHART               NAMESPACE
bilging-ant     1           Thu Oct  5 00:11:11 2017    DEPLOYED    nginx-ingress-0.8.7 default
```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o zarządzaniu Kubernetes wykresy zobacz dokumentację Helm.

> [!div class="nextstepaction"]
> [Dokumentacja Helm](https://github.com/kubernetes/helm/blob/master/docs/index.md)