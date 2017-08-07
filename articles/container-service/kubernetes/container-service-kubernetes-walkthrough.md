---
title: "Szybki start — klaster Azure Kubernetes dla systemu Linux | Microsoft Docs"
description: "Szybka nauka tworzenia klastra Kubernetes dla kontenerów systemu Linux w usłudze Azure Container Service za pomocą interfejsu wiersza polecenia platformy Azure."
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: a278f76fc0ac2aa42633ed0ce2ad4fbc4e7290da
ms.contentlocale: pl-pl
ms.lasthandoff: 08/02/2017

---

# <a name="deploy-kubernetes-cluster-for-linux-containers"></a>Wdrażanie klastra Kubernetes dla kontenerów systemu Linux

W tym przewodniku Szybki start klaster Kubernetes jest wdrażany za pomocą interfejsu wiersza polecenia platformy Azure. Następnie w klastrze jest wdrażana i uruchamiana aplikacja obsługująca wiele kontenerów, która składa się z frontonu internetowego i wystąpienia pamięci podręcznej Redis. Po ukończeniu aplikacja będzie dostępna w Internecie.

![Obraz przedstawiający przechodzenie do aplikacji Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

Na potrzeby tego przewodnika Szybki start założono, że masz podstawową wiedzę na temat pojęć związanych z rozwiązaniem Kubernetes. Aby uzyskać szczegółowe informacje na jego temat, zapoznaj się z [dokumentacją rozwiązania Kubernetes]( https://kubernetes.io/docs/home/).

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przewodnik szybkiego startu będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczna grupa przeznaczona do wdrażania zasobów platformy Azure i zarządzania nimi. 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Dane wyjściowe:

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-kubernetes-cluster"></a>Tworzenie klastra Kubernetes

Utwórz klaster Kubernetes w usłudze Azure Container Service za pomocą polecenia [az acs create](/cli/azure/acs#create). W poniższym przykładzie tworzony jest klaster o nazwie *myK8sCluster* z jednym węzłem głównym systemu Linux i trzema węzłami agenta systemu Linux.

```azurecli-interactive 
az acs create --orchestrator-type=kubernetes --resource-group myResourceGroup --name=myK8sCluster --generate-ssh-keys 
```

Po kilku minutach polecenie zostanie zakończone i zwróci informacje o klastrze sformatowanym przy użyciu formatu JSON. 

## <a name="connect-to-the-cluster"></a>Łączenie z klastrem

Aby zarządzać klastrem Kubernetes, należy użyć klienta wiersza polecenia usługi Kubernetes, narzędzia [kubectl](https://kubernetes.io/docs/user-guide/kubectl/). 

Jeśli korzystasz z usługi Azure CloudShell, narzędzie kubectl jest już zainstalowane. Jeśli chcesz zainstalować je lokalnie, możesz użyć polecenia [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli).

Aby skonfigurować narzędzie kubectl w celu nawiązania połączenia z klastrem Kubernetes, uruchom polecenie [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials). Ten krok umożliwia pobranie poświadczeń i skonfigurowanie interfejsu wiersza polecenia Kubernetes do ich użycia.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

Aby sprawdzić połączenie z klastrem, użyj polecenia [kubectl get](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get), aby powrócić do listy węzłów klastra.

```azurecli-interactive
kubectl get nodes
```

Dane wyjściowe:

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-14ad53a1-0    Ready                      10m       v1.6.6
k8s-agent-14ad53a1-1    Ready                      10m       v1.6.6
k8s-agent-14ad53a1-2    Ready                      10m       v1.6.6
k8s-master-14ad53a1-0   Ready,SchedulingDisabled   10m       v1.6.6
```

## <a name="run-the-application"></a>Uruchamianie aplikacji

Plik manifestu rozwiązania Kubernetes definiuje żądany stan klastra, w tym na przykład informacje o obrazach kontenera, które powinny zostać uruchomione. W tym przykładzie manifest służy do tworzenia wszystkich obiektów potrzebnych do uruchomienia aplikacji Azure Vote. 

Utwórz plik o nazwie `azure-vote.yaml` i skopiuj go do poniższego kodu YAML.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:redis-v1
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Użyj polecenia [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create), aby uruchomić aplikację.

```azurecli-interactive
kubectl create -f azure-vote.yaml
```

Dane wyjściowe:

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Testowanie aplikacji

Podczas uruchamiania aplikacji tworzona jest [usługa Kubernetes](https://kubernetes.io/docs/concepts/services-networking/service/), która uwidacznia fronton aplikacji w Internecie. Ten proces może potrwać kilka minut. 

Aby monitorować postęp, użyj polecenia [kubectl get-service](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) z argumentem `--watch`.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Początkowo adres **EXTERNAL-IP** dla usługi *azure-vote-front* pojawia się jako *oczekujący*. Po zmianie adresu EXTERNAL-IP z *oczekującego* na *adres IP*, użyj polecenia `CTRL-C`, aby zatrzymać proces śledzenia narzędzia kubectl. 
  
```bash
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Teraz możesz przejść do zewnętrznego adresu IP, aby wyświetlić aplikację Azure Vote.

![Obraz przedstawiający przechodzenie do aplikacji Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)  

## <a name="delete-cluster"></a>Usuwanie klastra
Gdy klaster nie będzie już potrzebny, możesz usunąć grupę zasobów, usługę kontenera i wszystkie pokrewne zasoby za pomocą polecenia [az group delete](/cli/azure/group#delete).

```azurecli-interactive 
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-the-code"></a>Uzyskiwanie kodu

W tym przewodniku Szybki start wcześniej utworzone obrazy kontenera zostały użyte w celu utworzenia wdrożenia rozwiązania Kubernetes. Powiązany kod aplikacji, plik Dockerfile i plik manifestu rozwiązania Kubernetes są dostępne w serwisie GitHub.

[https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wdrożono klaster Kubernetes oraz wdrożono w nim aplikację obsługującą wiele kontenerów. 

Aby dowiedzieć się więcej o usłudze Azure Container Service i poznać dokładnie proces od kompletnego kodu do wdrożenia, przejdź do samouczka dotyczącego klastra Kubernetes.

> [!div class="nextstepaction"]
> [Zarządzanie klastrem Kubernetes usługi ASC](./container-service-tutorial-kubernetes-prepare-app.md)
