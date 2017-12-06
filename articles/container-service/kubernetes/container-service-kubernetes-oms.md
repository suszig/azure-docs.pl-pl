---
title: "Monitorowanie Azure Kubernetes klaster - operacji zarządzania"
description: "Monitorowanie Kubernetes klastra usługi kontenera platformy Azure przy użyciu programu Microsoft Operations Management Suite"
services: container-service
author: bburns
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: e8a68896c923d785fea84cef60f8d2015906f342
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-microsoft-operations-management-suite-oms"></a>Monitor klastra usługi kontenera platformy Azure z Microsoft Operations Management Suite (OMS)

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Wymagania wstępne
W tym przewodniku założono, że [utworzony klaster Kubernetes za pomocą usługi kontenera platformy Azure](container-service-kubernetes-walkthrough.md).

Założono również, że masz `az` interfejsu wiersza polecenia platformy Azure i `kubectl` narzędzia są zainstalowane.

Możesz przetestować, jeśli masz `az` zainstalowany, uruchamiając narzędzie:

```console
$ az --version
```

Jeśli nie masz `az` narzędzie zainstalowane, nie ma instrukcji [tutaj](https://github.com/azure/azure-cli#installation).  
Alternatywnie można użyć [powłoki chmury Azure](https://docs.microsoft.com/azure/cloud-shell/overview), który ma `az` interfejsu wiersza polecenia platformy Azure i `kubectl` narzędzia już zainstalowane automatycznie.  

Możesz przetestować, jeśli masz `kubectl` zainstalowany, uruchamiając narzędzie:

```console
$ kubectl version
```

Jeśli nie masz `kubectl` zainstalowany, możesz uruchomić:
```console
$ az acs kubernetes install-cli
```

Aby sprawdzić, czy zostały zainstalowane w można uruchamiać narzędzie kubectl kluczy kubernetes:
```console
$ kubectl get nodes
```

Jeśli powyższe błędy polecenia limit, należy zainstalować kubernetes klastra klucze do własnych narzędzi kubectl. Możesz to zrobić za pomocą następującego polecenia:
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-operations-management-suite-oms"></a>Monitorowanie kontenerów w usłudze Operations Management Suite (OMS)

Microsoft Operations Management (OMS) jest firmy Microsoft w chmurze IT rozwiązania do zarządzania ułatwiające zarządzanie i ochrona lokalnej infrastruktury w chmurze. Kontener rozwiązanie to rozwiązanie w OMS analizy dzienników, które ułatwia przeglądanie spisu kontenera, wydajności i dzienniki w jednej lokalizacji. Można inspekcji, rozwiązywanie problemów z kontenerów, wyświetlając dzienniki w centralnej lokalizacji oraz znaleźć zakłócenia, wykorzystywanie nadmiarowe kontenera na hoście.

![](media/container-service-monitoring-oms/image1.png)

Aby uzyskać więcej informacji o rozwiązaniu kontenera, zapoznaj się [analizy dzienników rozwiązania kontenera](../../log-analytics/log-analytics-containers.md).

## <a name="installing-oms-on-kubernetes"></a>Instalowanie pakietu OMS na Kubernetes

### <a name="obtain-your-workspace-id-and-key"></a>Uzyskaj identyfikator i klucz
Do OMS agentowi komunikować z usługą musi mieć skonfigurowaną identyfikator obszaru roboczego i klucz obszaru roboczego. Aby uzyskać identyfikator i klucz, musisz utworzyć konto OMS w <https://mms.microsoft.com>. Wykonaj kroki, aby utworzyć konto. Po zakończeniu tworzenia konta, należy uzyskać klucz i identyfikator klikając **ustawienia**, następnie **połączonych źródeł**, a następnie **serwerów z systemem Linux**, jak pokazano poniżej.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-oms-agent-using-a-daemonset"></a>Instalowanie agenta pakietu OMS, za pomocą DaemonSet
DaemonSets są używane przez Kubernetes do uruchomienia pojedynczego wystąpienia kontenera na każdym hoście w klastrze.
Są one idealne w przypadku uruchamiania agenci monitorowania.

Oto [pliku yaml programu DaemonSet](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). Zapisz go w pliku o nazwie `oms-daemonset.yaml` i Zastąp symbol zastępczy wartości `WSID` i `KEY` z identyfikator i klucz w pliku.

Po dodaniu identyfikator i klucz konfiguracji DaemonSet agent pakietu OMS można zainstalować w klastrze z `kubectl` narzędzia wiersza polecenia:

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-oms-agent-using-a-kubernetes-secret"></a>Instalowanie przy użyciu klucza tajnego Kubernetes agent pakietu OMS
Ochrona Identyfikatora obszar roboczy OMS i klawisz można użyć klucza tajnego Kubernetes jako część pliku DaemonSet yaml programu.

 - Skopiuj skrypt, plik tajny szablon i plik yaml programu DaemonSet (z [repozytorium](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) i upewnij się, że znajdują się w tym samym katalogu. 
      - Generowanie skryptu - gen.sh klucz tajny klucz tajny
      - Szablon tajne — template.yaml klucz tajny
   - Plik yaml programu DaemonSet — omsagent — ds-secrets.yaml
 - Uruchom skrypt. Skrypt poprosi o OMS identyfikator i klucz podstawowy. Włóż który i skrypt zostanie utworzony plik tajny yaml programu, dlatego może być uruchomiony.   
   ```
   #> sudo bash ./secret-gen.sh 
   ```

   - Utwórz pod kluczy tajnych, uruchamiając następujące czynności:``` kubectl create -f omsagentsecret.yaml ```
 
   - Aby sprawdzić, uruchom następujące polecenie: 

   ``` 
   root@ubuntu16-13db:~# kubectl get secrets
   NAME                  TYPE                                  DATA      AGE
   default-token-gvl91   kubernetes.io/service-account-token   3         50d
   omsagent-secret       Opaque                                2         1d
   root@ubuntu16-13db:~# kubectl describe secrets omsagent-secret
   Name:           omsagent-secret
   Namespace:      default
   Labels:         <none>
   Annotations:    <none>

   Type:   Opaque

   Data
   ====
   WSID:   36 bytes
   KEY:    88 bytes 
   ```
 
  - Tworzenie sieci omsagent demon set, uruchamiając``` kubectl create -f omsagent-ds-secrets.yaml ```

### <a name="conclusion"></a>Podsumowanie
Gotowe. Po kilku minutach można wyświetlić dane przepływające do pulpitu nawigacyjnego OMS.
