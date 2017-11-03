---
title: "Kubernetes na Azure samouczek — Kubernetes monitora"
description: "Samouczek AKS — Kubernetes monitora z Microsoft Operations Management Suite (OMS)"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: "Docker, kontenerów, Micro-services, Kubernetes, platforma Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: a41f699291a65129906680cbb6719c2478c0d830
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="monitor-azure-container-service-aks"></a>Monitor usługi kontenera platformy Azure (AKS)

Monitorowanie sieci klastra Kubernetes i kontenery jest krytyczny, szczególnie w przypadku uruchamiania klastra produkcyjnego na dużą skalę, z wieloma aplikacjami.

W tym samouczku, można skonfigurować monitorowanie swoją AKS klastra przy użyciu [kontenery rozwiązanie do analizy dzienników](../log-analytics/log-analytics-containers.md).

Ten samouczek, część 7, 8 obejmuje następujące zadania:

> [!div class="checklist"]
> * Konfigurowanie kontenera monitorowania rozwiązania
> * Konfigurowanie monitorowania agentów
> * Dostęp do monitorowania informacji w portalu Azure

## <a name="before-you-begin"></a>Przed rozpoczęciem

W poprzednim samouczki aplikacji zostało umieszczone w kontener obrazów, te obrazy przekazane do rejestru kontenera Azure i klastra Kubernetes utworzone.

Jeśli nie zostało wykonane następujące kroki, a następnie zostać z niego skorzystać, wróć do [samouczek 1 — Tworzenie kontenera obrazów](./tutorial-kubernetes-prepare-app.md).

## <a name="configure-the-monitoring-solution"></a>Konfigurowanie monitorowania rozwiązania

W portalu Azure wybierz **nowy** i wyszukaj `Container Monitoring Solution`. Po zlokalizowaniu, wybierz **Utwórz**.

![Dodaj rozwiązanie](./media/container-service-tutorial-kubernetes-monitor/add-solution.png)

Utwórz nowy obszar roboczy OMS, lub wybierz istniejący. Formularz obszarem roboczym pakietu OMS przeprowadzi Cię przez ten proces.

Podczas tworzenia obszaru roboczego, wybierz **Przypnij do pulpitu nawigacyjnego** ułatwia ich odnalezienie.

![Obszar roboczy OMS](./media/container-service-tutorial-kubernetes-monitor/oms-workspace.png)

Po zakończeniu wybierz **OK**. Po ukończeniu sprawdzania poprawności, wybierz **Utwórz** można utworzyć kontenera monitorowania rozwiązania.

Po utworzeniu obszaru roboczego go są prezentowane w portalu Azure.

## <a name="get-workspace-settings"></a>Pobierz ustawienia obszaru roboczego

Analiza dzienników identyfikator i klucz są potrzebne do konfigurowania agenta rozwiązania w węzłach Kubernetes.

Aby pobrać te wartości, wybierz **obszarem roboczym pakietu OMS** z menu po lewej stronie rozwiązań kontenera. Wybierz **Zaawansowane ustawienia** i zwróć uwagę na **identyfikator obszaru roboczego** i **klucz podstawowy**.

## <a name="configure-monitoring-agents"></a>Konfigurowanie monitorowania agentów

Następujący plik manifestu Kubernetes może służyć do konfigurowania kontenera agentów w klastrze Kubernetes monitorowania. Tworzy Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), która działa pod jednym w każdym węźle klastra.

Zapisz poniższy tekst do pliku o nazwie `oms-daemonset.yaml`i zastąp symbole zastępcze dla `WSID` i `KEY` z klucz i identyfikator obszaru roboczego analizy dzienników.

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
    agentVersion: 1.4.0-12
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: <WSID>
       - name: KEY
         value: <KEY>
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
        - mountPath: /var/opt/microsoft/omsagent/state/containerhostname
          name: container-hostname
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
   nodeSelector:
    beta.kubernetes.io/os: linux
   # Tolerate a NoSchedule taint on master that ACS Engine sets.
   tolerations:
    - key: "node-role.kubernetes.io/master"
      operator: "Equal"
      value: "true"
      effect: "NoSchedule"
   volumes:
    - name: docker-sock
      hostPath:
       path: /var/run/docker.sock
    - name: container-hostname
      hostPath:
       path: /etc/hostname
    - name: host-log
      hostPath:
       path: /var/log
```

Utwórz DaemonSet przy użyciu następującego polecenia:

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

Aby sprawdzić, czy DaemonSet został utworzony, uruchom polecenie:

```azurecli-interactive
kubectl get daemonset
```

Dane wyjściowe będą podobne do następujących:

```
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR                 AGE
omsagent   3         3         3         3            3           beta.kubernetes.io/os=linux   8m
```

Po agenci są uruchomione, przyjmuje OMS do pozyskiwania i przetwarzania danych w ciągu kilku minut.

## <a name="access-monitoring-data"></a>Dostęp do danych monitorowania

W portalu Azure wybierz obszar roboczy analizy dzienników, która została przypięta do pulpitu nawigacyjnego portalu. Polecenie **rozwiązanie monitorowanie kontenera** kafelka. W tym miejscu można znaleźć informacje o klastrze AKS i kontenery z klastra.

![Pulpit nawigacyjny](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Zobacz [dokumentacji usługi Analiza dzienników Azure](../log-analytics/index.yml) szczegółowe wskazówki dotyczące zapytań i analizowanie danych monitorowania.

## <a name="next-steps"></a>Następne kroki

W tym samouczku monitorowane są klastra Kubernetes z usługą OMS. Uwzględnione objętych zadań:

> [!div class="checklist"]
> * Konfigurowanie kontenera monitorowania rozwiązania
> * Konfigurowanie monitorowania agentów
> * Dostęp do monitorowania informacji w portalu Azure

Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat uaktualniania Kubernetes do nowej wersji.

> [!div class="nextstepaction"]
> [Uaktualnij Kubernetes](./tutorial-kubernetes-upgrade-cluster.md)