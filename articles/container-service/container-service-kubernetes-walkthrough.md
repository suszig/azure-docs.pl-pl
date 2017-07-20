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
ms.date: 07/18/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: 3be2079d205d6bfd4c796e5f6abcd7ac5fe595a2
ms.contentlocale: pl-pl
ms.lasthandoff: 07/19/2017

---

# <a name="deploy-kubernetes-cluster-for-linux-containers"></a>Wdrażanie klastra Kubernetes dla kontenerów systemu Linux

Interfejs wiersza polecenia platformy Azure umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. W tym przewodniku zawarto szczegółowe informacje dotyczące korzystania z interfejsu wiersza polecenia platformy Azure w celu wdrożenia klastra [Kubernetes](https://kubernetes.io/docs/home/) w [usłudze Azure Container Service](container-service-intro.md). Po wdrożeniu klastra nawiążesz z nim połączenie za pomocą narzędzia wiersza polecenia `kubectl` usługi Kubernetes i wdrożysz swój pierwszy kontener systemu Linux.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przewodnik szybkiego startu będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczna grupa przeznaczona do wdrażania zasobów platformy Azure i zarządzania nimi. 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-kubernetes-cluster"></a>Tworzenie klastra Kubernetes
Utwórz klaster Kubernetes w usłudze Azure Container Service za pomocą polecenia [az acs create](/cli/azure/acs#create). 

W poniższym przykładzie tworzony jest klaster o nazwie *myK8sCluster* z jednym węzłem głównym systemu Linux i dwoma węzłami agenta systemu Linux. Ten przykład tworzy klucze SSH, jeśli jeszcze nie istnieją w lokalizacji domyślnej. Aby użyć określonego zestawu kluczy, użyj opcji `--ssh-key-value`. Zmień nazwę klastra, aby pasowała do Twojego środowiska. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --generate-ssh-keys 
```

Po kilku minutach polecenie zostanie zakończone i wyświetlone zostaną informacje o wdrożeniu.

## <a name="install-kubectl"></a>Instalowanie narzędzia kubectl

Aby nawiązać połączenie z klastrem Kubernetes z komputera klienckiego, należy użyć klienta wiersza polecenia usługi Kubernetes [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/). 

Jeśli korzystasz z usługi Azure CloudShell, narzędzie `kubectl` jest już zainstalowane. Jeśli chcesz zainstalować je lokalnie, możesz użyć polecenia [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli).

Poniższy przykład interfejsu wiersza polecenia platformy Azure instaluje narzędzie `kubectl` w systemie. Jeśli używasz interfejsu wiersza polecenia platformy Azure w systemie macOS lub Linux, konieczne może być uruchomienie polecenia za pomocą programu `sudo`.

```azurecli-interactive 
az acs kubernetes install-cli 
```

## <a name="connect-with-kubectl"></a>Nawiązywanie połączenia przy użyciu narzędzia kubectl

Aby skonfigurować narzędzie `kubectl` w celu nawiązania połączenia z klastrem Kubernetes, uruchom polecenie [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials). Poniższy przykład pobiera konfigurację klastra dla klastra Kubernetes.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

Aby sprawdzić połączenie z klastrem ze swojej maszyny, spróbuj uruchomić następujące polecenie:

```azurecli-interactive
kubectl get nodes
```

`kubectl` wyświetli węzeł główny i węzły agenta.

```azurecli-interactive
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.5.3
k8s-agent-98dc3136-1    Ready                      5m        v1.5.3
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.5.3

```


## <a name="deploy-an-nginx-container"></a>Wdrażanie kontenera NGINX

Kontener platformy Docker można uruchomić wewnątrz *zasobnika* rozwiązania Kubernetes zawierającego co najmniej jeden kontener. 

Poniższe polecenie uruchamia kontener platformy Docker NGINX w zasobniku Kubernetes w jednym z węzłów. W tym przypadku kontener uruchamia serwer internetowy NGINX pobierany z obrazu w usłudze [Docker Hub](https://hub.docker.com/_/nginx/).

```azurecli-interactive
kubectl run nginx --image nginx
```
Aby sprawdzić, czy kontener jest uruchomiony, uruchom polecenie:

```azurecli-interactive
kubectl get pods
```

## <a name="view-the-nginx-welcome-page"></a>Wyświetlanie strony powitalnej serwera NGINX
Aby udostępnić światu serwer NGINX z publicznym adresem IP, wpisz następujące polecenie:

```azurecli-interactive
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

To polecenie spowoduje utworzenie przez rozwiązanie Kubernetes usługi i [reguły usługi Azure Load Balancer](container-service-kubernetes-load-balancing.md) z publicznym adresem IP usługi. 

Uruchom poniższe polecenie, aby wyświetlić stan usługi.

```azurecli-interactive
kubectl get svc
```

Początkowo adres IP będzie widoczny jako `pending`. Po kilku minutach zostanie ustawiony zewnętrzny adres IP usługi:
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
nginx        10.0.111.25    52.179.3.96     80/TCP         22m
```

Możesz użyć wybranej przeglądarki internetowej, aby wyświetlić domyślną stronę powitalną serwera NGINX pod zewnętrznym adresem IP:

![Obraz przedstawiający przechodzenie do serwera Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


## <a name="delete-cluster"></a>Usuwanie klastra
Gdy klaster nie będzie już potrzebny, możesz usunąć grupę zasobów, usługę kontenera i wszystkie pokrewne zasoby za pomocą polecenia [az group delete](/cli/azure/group#delete).

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Następne kroki

W tym przewodniku Szybki start wdrożono klaster Kubernetes, nawiązano połączenie z narzędziem `kubectl` i wdrożono zasobnik za pomocą kontenera NGINX. Aby dowiedzieć się więcej na temat usługi Azure Container Service, przejdź do samouczka dotyczącego klastra Kubernetes.

> [!div class="nextstepaction"]
> [Zarządzanie klastrem Kubernetes usługi ASC](./container-service-tutorial-kubernetes-prepare-app.md)

