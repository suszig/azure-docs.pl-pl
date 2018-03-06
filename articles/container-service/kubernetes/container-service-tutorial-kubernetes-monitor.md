---
title: "Samouczek usługi Azure Container Service — monitorowanie usługi Kubernetes"
description: "Samouczek usługi Azure Container Service — monitorowanie usługi Kubernetes za pomocą pakietu Microsoft Operations Management Suite (OMS)"
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/26/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 965ce4b7e154684fc1d171c90f17498afc828a66
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a>Monitorowanie klastra Kubernetes za pomocą pakietu Operations Management Suite

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Monitorowanie klastra i kontenerów usługi Kubernetes ma krytyczne znaczenie, szczególnie w przypadku zarządzania klastrem produkcyjnym o dużej skali i z wieloma aplikacjami. 

Możesz korzystać z kilku rozwiązań do monitorowania usługi Kubernetes pochodzących od firmy Microsoft lub innych dostawców. W tym samouczku przedstawiono monitorowanie klastra Kubernetes za pomocą rozwiązania Containers w pakiecie [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md) — oprogramowaniu firmy Microsoft do zarządzania chmurową infrastrukturą IT. (Rozwiązanie Containers pakietu OMS jest dostępne w wersji zapoznawczej).

Ten samouczek (część 7 z 7) obejmuje następujące zadania:

> [!div class="checklist"]
> * Pobieranie ustawień obszaru roboczego pakietu OMS
> * Konfigurowanie agentów pakietu OMS w węzłach Kubernetes
> * Uzyskiwanie dostępu do informacji monitorowania w portalu pakietu OMS lub witrynie Azure Portal

## <a name="before-you-begin"></a>Przed rozpoczęciem

W poprzednich samouczkach aplikacja została spakowana w postaci obrazów kontenera, obrazy zostały przekazane do usługi Azure Container Registry i utworzono klaster usługi Kubernetes. 

Jeśli nie wykonano tych kroków, a chcesz kontynuować pracę, wróć do części [Samouczek 1 — tworzenie obrazów kontenera](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="get-workspace-settings"></a>Pobieranie ustawień obszaru roboczego

Gdy [portal pakietu OMS](https://mms.microsoft.com) będzie dostępny, przejdź do pozycji **Ustawienia** > **Połączone źródła** > **Serwery z systemem Linux**. Tu możesz znaleźć *identyfikator obszaru roboczego* i podstawowy lub pomocniczy *klucz obszaru roboczego*. Zanotuj te wartości. Będą one potrzebne do skonfigurowania agentów pakietu OMS w klastrze.

## <a name="set-up-oms-agents"></a>Konfigurowanie agentów pakietu OMS

W tym miejscu znajduje się plik YAML pozwalający na skonfigurowanie agentów pakietu OMS w węzłach klastra z systemem Linux. Umożliwia on utworzenie elementu [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) usługi Kubernetes, który uruchamia pojedyncze, identyczne zasobniki w każdym węźle klastra. Zasób DaemonSet to idealne rozwiązanie na potrzeby wdrożenia agenta monitorowania. 

Zapisz poniższy tekst do pliku o nazwie `oms-daemonset.yaml`, zamieniając symbole zastępcze *myWorkspaceID* i *myWorkspaceKey* na identyfikator i klucz obszaru roboczego pakietu OMS. (W środowisku produkcyjnym możesz zakodować te wartości jako wpisy tajne).

```YAML
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
 name: omsagent
spec:
 template:
  metadata:
   labels:
    app: omsagent
    agentVersion: v1.3.4-127
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent 
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: myWorkspaceID
       - name: KEY 
         value: myWorkspaceKey
       - name: DOMAIN
         value: opinsights.azure.com
       securityContext:
         privileged: true
       ports:
       - containerPort: 25225
         protocol: TCP 
       - containerPort: 25224
         protocol: UDP
       volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /var/log 
          name: host-log
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   volumes:
    - name: docker-sock 
      hostPath:
       path: /var/run/docker.sock
    - name: host-log
      hostPath:
       path: /var/log
```

Utwórz element DaemonSet przy użyciu następującego polecenia:

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

Aby sprawdzić, czy element DaemonSet został utworzony, uruchom polecenie:

```azurecli-interactive
kubectl get daemonset
```

Dane wyjściowe będą podobne do następujących:

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

Po uruchomieniu agentów pozyskanie i przetworzenie danych przez pakiet OMS trwa kilka minut.

## <a name="access-monitoring-data"></a>Dostęp do danych monitorowania

Wyświetlanie i analizowanie danych monitorowania kontenera pakietu OMS za pomocą [rozwiązania Containers](../../log-analytics/log-analytics-containers.md) w portalu pakietu OMS lub w witrynie Azure Portal. 

Aby zainstalować rozwiązanie Containers przy użyciu [portalu pakietu OMS](https://mms.microsoft.com), przejdź do **Galerii rozwiązań**. Następnie dodaj element **Rozwiązanie Containers**. Rozwiązanie Containers możesz też dodać z witryny [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).

W portalu pakietu OMS poszukaj kafelka podsumowania **Containers** na pulpicie nawigacyjnym pakietu OMS. Kliknij kafelek, aby wyświetlić szczegółowe informacje, w tym zdarzenia kontenera, błędy, stan, spis obrazów oraz użycie procesora i pamięci. Aby uzyskać bardziej szczegółowe informacje, kliknij wiersz w dowolnym kafelku lub [przeszukaj dzienniki](../../log-analytics/log-analytics-log-searches.md).

![Pulpit nawigacyjny rozwiązania Containers w portalu pakietu OMS](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Podobnie w witrynie Azure Portal wybierz pozycję **Log Analytics** i wybierz nazwę obszaru roboczego. Aby wyświetlić kafelek podsumowania **Containers**, kliknij pozycję **Rozwiązania** > **Containers**. Aby wyświetlić szczegóły, kliknij kafelek.

Szczegółowy przewodnik dotyczący odpytywania i analizowania danych monitorowania znajduje się w [dokumentacji usługi Azure Log Analytics](../../log-analytics/index.yml).

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób monitorowania klastra Kubernetes za pomocą pakietu OMS. Wykonano następujące zadania:

> [!div class="checklist"]
> * Pobieranie ustawień obszaru roboczego pakietu OMS
> * Konfigurowanie agentów pakietu OMS w węzłach Kubernetes
> * Uzyskiwanie dostępu do informacji monitorowania w portalu pakietu OMS lub witrynie Azure Portal


Kliknij ten link, aby wyświetlić wstępnie utworzone przykładowe skrypty dla usługi Container Service.

> [!div class="nextstepaction"]
> [Przykłady skryptów usługi Azure Container Service](cli-samples.md)
