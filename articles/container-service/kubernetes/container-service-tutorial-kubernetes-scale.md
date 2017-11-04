---
title: "Samouczek usługi kontenera platformy Azure — skalowania aplikacji | Dokumentacja firmy Microsoft"
description: "Samouczek usługi kontenera platformy Azure — skalowania aplikacji"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontenerów, Micro-services, Kubernetes, platforma Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 25d7e56c7d7755e44fdb16e3b5585a112e495e77
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="scale-kubernetes-pods-and-kubernetes-infrastructure"></a>Infrastruktura Kubernetes i stanowiskami Kubernetes skali

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Jeśli została już po samouczków, masz działające Kubernetes klastra usługi kontenera platformy Azure i głosowania Azure aplikacja została wdrożona. 

W tym samouczku, część 5, 7 skalowanie w poziomie stanowiskami w aplikacji i spróbuj pod Skalowanie automatyczne. Możesz również sposób skalowania liczby węzłów agenta maszyny Wirtualnej platformy Azure do zmiany wydajności obsługi obciążeń klastra. Zadania ukończone obejmują:

> [!div class="checklist"]
> * Ręcznie skalowanie stanowiskami Kubernetes
> * Konfigurowanie stanowiskami skalowania automatycznego uruchamiania fronton aplikacji
> * Skalowanie węzłów agenta Kubernetes Azure

W kolejnych samouczkach aplikacji Azure głos jest aktualizowany, a usługi Operations Management Suite jest skonfigurowana do monitorowania Kubernetes klastra.

## <a name="before-you-begin"></a>Przed rozpoczęciem

W samouczkach poprzedniej aplikacji zostało umieszczone w obraz kontenera, ten obraz przekazany do rejestru kontenera Azure i utworzyć klaster Kubernetes. W klastrze Kubernetes następnie uruchomienia aplikacji. 

Jeśli nie zostało wykonane następujące kroki, a następnie zostać z niego skorzystać, wróć do [samouczek 1 — Tworzenie kontenera obrazów](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="manually-scale-pods"></a>Ręcznie skalować stanowiskami

W związku z tym daleko głos Azure frontonu i wystąpienia pamięci podręcznej Redis zostały wdrożone, każda z pojedynczą replikę. Aby sprawdzić, uruchom [kubectl uzyskać](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) polecenia.

```azurecli-interactive
kubectl get pods
```

Dane wyjściowe:

```bash
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Ręcznie zmień liczbę stanowiskami w `azure-vote-front` wdrożenia przy użyciu [skali kubectl](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#scale) polecenia. W tym przykładzie zwiększa liczbę do 5.

```azurecli-interactive
kubectl scale --replicas=5 deployment/azure-vote-front
```

Uruchom [kubectl uzyskać stanowiskami](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) Aby sprawdzić, czy Kubernetes tworzy stanowiskami. Po minucie lub tak system stanowiskami dodatkowe:

```azurecli-interactive
kubectl get pods
```

Dane wyjściowe:

```bash
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Stanowiskami skalowania automatycznego

Obsługuje Kubernetes [Skalowanie automatyczne poziome pod](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) Aby dostosować liczbę stanowiskami we wdrożeniu w zależności od użycia procesora CPU lub innych wybrać metryki. 

Aby użyć autoscaler, Twoje stanowiskami musi mieć żądań procesora CPU i zdefiniowano limitów. W `azure-vote-front` wdrożenia, frontonu kontenera żądań 0,25 procesora CPU, limit 0,5 procesora CPU. Ustawienia następującą postać:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

W poniższym przykładzie użyto [skalowania automatycznego kubectl](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#autoscale) polecenia automatycznego skalowania liczby stanowiskami w `azure-vote-front` wdrożenia. W tym miejscu Jeśli wykorzystanie procesora CPU przekracza 50%, autoscaler zwiększa stanowiskami maksymalnie 10.


```azurecli-interactive
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Aby wyświetlić stan autoscaler, uruchom następujące polecenie:

```azurecli-interactive
kubectl get hpa
```

Dane wyjściowe:

```bash
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Po upływie kilku minut przy minimalnym obciążeniu aplikacji Azure głos liczba replik pod zmniejsza się automatycznie do 3.

## <a name="scale-the-agents"></a>Skalowanie agentów

Jeśli utworzony klaster Kubernetes przy użyciu polecenia domyślne w poprzednich instrukcji ma trzy węzły agenta. Liczba agentów można dostosować ręcznie, jeśli planujesz więcej lub mniej obciążeń kontenera w klastrze. Użyj [skalować acs az](/cli/azure/acs#scale) polecenia, a następnie określ liczbę agentów z `--new-agent-count` parametru.

Poniższy przykład zwiększa liczbę węzłów agenta do 4 w klastrze Kubernetes o nazwie *myK8sCluster*. Polecenie zajmuje kilka minut.

```azurecli-interactive
az acs scale --resource-group=myResourceGroup --name=myK8SCluster --new-agent-count 4
```

Dane wyjściowe polecenia pokazuje liczbę agenta węzłów w wartości `agentPoolProfiles:count`:

```azurecli
{
  "agentPoolProfiles": [
    {
      "count": 4,
      "dnsPrefix": "myK8SCluster-myK8SCluster-e44f25-k8s-agents",
      "fqdn": "",
      "name": "agentpools",
      "vmSize": "Standard_D2_v2"
    }
  ],
...

```

## <a name="next-steps"></a>Następne kroki

W tym samouczku użyto różnych funkcji skalowania w klastrze Kubernetes. Uwzględnione objętych zadań:

> [!div class="checklist"]
> * Ręcznie skalowanie stanowiskami Kubernetes
> * Konfigurowanie stanowiskami skalowania automatycznego uruchamiania fronton aplikacji
> * Skalowanie węzłów agenta Kubernetes Azure

Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat aktualizowania aplikacji w Kubernetes.

> [!div class="nextstepaction"]
> [Aktualizuj aplikację w Kubernetes](./container-service-tutorial-kubernetes-app-update.md)

