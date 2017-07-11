---
title: "Szybki start — klaster Azure Kubernetes dla systemu Linux | Microsoft Docs"
description: "Szybka nauka tworzenia klastra Kubernetes dla kontenerów systemu Linux w usłudze Azure Container Service za pomocą interfejsu wiersza polecenia platformy Azure."
services: container-service
documentationcenter: 
author: anhowe
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
ms.date: 05/31/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 25043f6bf5e5ab3def8563bd2c096b79706bfec1
ms.contentlocale: pl-pl
ms.lasthandoff: 06/20/2017

---

<a id="deploy-kubernetes-cluster-for-linux-containers" class="xliff"></a>

# Wdrażanie klastra Kubernetes dla kontenerów systemu Linux

Interfejs wiersza polecenia platformy Azure umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. W tym przewodniku zawarto szczegółowe informacje dotyczące korzystania z interfejsu wiersza polecenia platformy Azure w celu wdrożenia klastra [Kubernetes](https://kubernetes.io/docs/home/) w [usłudze Azure Container Service](container-service-intro.md). Po wdrożeniu klastra nawiążesz z nim połączenie za pomocą narzędzia wiersza polecenia `kubectl` usługi Kubernetes i wdrożysz swój pierwszy kontener systemu Linux.

Dla tego samouczka wymagany jest interfejs wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

<a id="log-in-to-azure" class="xliff"></a>

## Zaloguj się do platformy Azure. 

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [az login](/cli/azure/#login) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```azurecli-interactive
az login
```

<a id="create-a-resource-group" class="xliff"></a>

## Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczna grupa przeznaczona do wdrażania zasobów platformy Azure i zarządzania nimi. 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

<a id="create-kubernetes-cluster" class="xliff"></a>

## Tworzenie klastra Kubernetes
Utwórz klaster Kubernetes w usłudze Azure Container Service za pomocą polecenia [az acs create](/cli/azure/acs#create). 

W poniższym przykładzie tworzony jest klaster o nazwie *myK8sCluster* z jednym węzłem głównym systemu Linux i dwoma węzłami agenta systemu Linux. Ten przykład tworzy klucze SSH, jeśli jeszcze nie istnieją w lokalizacji domyślnej. Aby użyć określonego zestawu kluczy, użyj opcji `--ssh-key-value`. Zmień nazwę klastra, aby pasowała do Twojego środowiska. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --generate-ssh-keys 
```

Po kilku minutach polecenie zostanie zakończone i wyświetlone zostaną informacje o wdrożeniu.

<a id="install-kubectl" class="xliff"></a>

## Instalowanie narzędzia kubectl

Aby nawiązać połączenie z klastrem Kubernetes z komputera klienckiego, należy użyć klienta wiersza polecenia usługi Kubernetes [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/). 

Jeśli korzystasz z usługi Azure CloudShell, narzędzie `kubectl` jest już zainstalowane. Jeśli chcesz zainstalować je lokalnie, możesz użyć polecenia [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli).

Poniższy przykład interfejsu wiersza polecenia platformy Azure instaluje narzędzie `kubectl` w systemie. Jeśli używasz interfejsu wiersza polecenia platformy Azure w systemie macOS lub Linux, konieczne może być uruchomienie polecenia za pomocą programu `sudo`.

```azurecli-interactive 
az acs kubernetes install-cli 
```

<a id="connect-with-kubectl" class="xliff"></a>

## Nawiązywanie połączenia przy użyciu narzędzia kubectl

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


<a id="deploy-an-nginx-container" class="xliff"></a>

## Wdrażanie kontenera NGINX

Kontener platformy Docker można uruchomić wewnątrz *zasobnika* rozwiązania Kubernetes zawierającego co najmniej jeden kontener. 

Poniższe polecenie uruchamia kontener platformy Docker NGINX w zasobniku Kubernetes w jednym z węzłów. W tym przypadku kontener uruchamia serwer internetowy NGINX pobierany z obrazu w usłudze [Docker Hub](https://hub.docker.com/_/nginx/).

```azurecli-interactive
kubectl run nginx --image nginx
```
Aby sprawdzić, czy kontener jest uruchomiony, uruchom polecenie:

```azurecli-interactive
kubectl get pods
```

<a id="view-the-nginx-welcome-page" class="xliff"></a>

## Wyświetlanie strony powitalnej serwera NGINX
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


<a id="delete-cluster" class="xliff"></a>

## Usuwanie klastra
Gdy klaster nie będzie już potrzebny, możesz usunąć grupę zasobów, usługę kontenera i wszystkie pokrewne zasoby za pomocą polecenia [az group delete](/cli/azure/group#delete).

```azurecli-interactive 
az group delete --name myResourceGroup
```


<a id="next-steps" class="xliff"></a>

## Następne kroki

W tym przewodniku Szybki start wdrożono klaster Kubernetes, nawiązano połączenie z narzędziem `kubectl` i wdrożono zasobnik za pomocą kontenera NGINX. Aby dowiedzieć się więcej na temat usługi Azure Container Service, przejdź do samouczka dotyczącego klastra Kubernetes.

> [!div class="nextstepaction"]
> [Zarządzanie klastrem Kubernetes usługi ASC](./container-service-tutorial-kubernetes-prepare-app.md)

