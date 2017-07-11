---
title: "Szybki start — klaster Azure Kubernetes dla systemu Windows | Microsoft Docs"
description: "Szybka nauka tworzenia klastra Kubernetes dla kontenerów systemu Windows w usłudze Azure Container Service za pomocą interfejsu wiersza polecenia platformy Azure."
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 929a4dec638da9488dd0b43fd123ed0cce77bcf3
ms.contentlocale: pl-pl
ms.lasthandoff: 06/20/2017


---

<a id="deploy-kubernetes-cluster-for-windows-containers" class="xliff"></a>

# Wdrażanie klastra Kubernetes dla kontenerów systemu Windows

Interfejs wiersza polecenia platformy Azure umożliwia tworzenie zasobów Azure i zarządzanie nimi z poziomu wiersza polecenia lub skryptów. W tym przewodniku zawarto szczegółowe informacje dotyczące korzystania z interfejsu wiersza polecenia platformy Azure w celu wdrożenia klastra [Kubernetes](https://kubernetes.io/docs/home/) w [usłudze Azure Container Service](container-service-intro.md). Po wdrożeniu klastra nawiążesz z nim połączenie za pomocą narzędzia wiersza polecenia `kubectl` usługi Kubernetes i wdrożysz swój pierwszy kontener systemu Windows.

Dla tego samouczka wymagany jest interfejs wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

> [!NOTE]
> Obsługa kontenerów systemu Windows w rozwiązaniu Kubernetes w usłudze Azure Container Service jest dostępna w wersji zapoznawczej. 
>

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

W poniższym przykładzie tworzony jest klaster o nazwie *myK8sCluster* z jednym węzłem głównym systemu Linux i dwoma węzłami agenta systemu Windows. Ten przykład tworzy klucze SSH potrzebne do nawiązania połączenia z węzłem głównym systemu Linux. W tym przykładzie *azureuser* to nazwa użytkownika administracyjnego, a *myPassword12* to hasło w węzłach systemu Windows. Zmień te wartości, aby pasowały do Twojego środowiska. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username azureuser \
    --admin-password myPassword12
```

Po kilku minutach polecenie zostanie zakończone i wyświetlone zostaną informacje o wdrożeniu.

<a id="install-kubectl" class="xliff"></a>

## Instalowanie narzędzia kubectl

Aby nawiązać połączenie z klastrem Kubernetes z komputera klienckiego, należy użyć klienta wiersza polecenia usługi Kubernetes [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/). 

Jeśli korzystasz z usługi Azure CloudShell, narzędzie `kubectl` jest już zainstalowane. Jeśli chcesz zainstalować je lokalnie, możesz użyć polecenia [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli).

Poniższy przykład interfejsu wiersza polecenia platformy Azure instaluje narzędzie `kubectl` w systemie. W systemie Windows należy uruchomić to polecenie jako administrator.

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

<a id="deploy-a-windows-iis-container" class="xliff"></a>

## Wdrażanie kontenera usług IIS systemu Windows

Kontener platformy Docker można uruchomić wewnątrz *zasobnika* rozwiązania Kubernetes zawierającego co najmniej jeden kontener. 

W tym podstawowym przykładzie użyto pliku JSON do określenia kontenera usług Microsoft Internet Information Server (IIS), a następnie utworzono zasobnik przy użyciu polecenia `kubctl apply`. 

Utwórz plik lokalny o nazwie `iis.json` i skopiuj poniższy tekst. Ten plik nakazuje rozwiązaniu Kubernetes uruchomienie usług IIS w systemie Windows Server 2016 Nano Server przy użyciu obrazu publicznego kontenera z witryny [Docker Hub](https://hub.docker.com/r/nanoserver/iis/). Kontener korzysta z portu 80, ale początkowo jest dostępny tylko w ramach sieci klastra.

 ```JSON
 {
  "apiVersion": "v1",
  "kind": "Pod",
  "metadata": {
    "name": "iis",
    "labels": {
      "name": "iis"
    }
  },
  "spec": {
    "containers": [
      {
        "name": "iis",
        "image": "nanoserver/iis",
        "ports": [
          {
          "containerPort": 80
          }
        ]
      }
    ],
    "nodeSelector": {
     "beta.kubernetes.io/os": "windows"
     }
   }
 }
 ```

Aby uruchomić zasobnik, wpisz:
  
```azurecli-interactive
kubectl apply -f iis.json
```  

Aby śledzić wdrożenie, wpisz:
  
```azurecli-interactive
kubectl get pods
```

Podczas wdrażania zasobnika ustawiany jest stan `ContainerCreating`. Może upłynąć kilka minut, zanim stan kontenera zmieni się na `Running`.

```azurecli-interactive
NAME     READY        STATUS        RESTARTS    AGE
iis      1/1          Running       0           32s
```

<a id="view-the-iis-welcome-page" class="xliff"></a>

## Wyświetlanie strony powitalnej usług IIS

Aby udostępnić zasobnik światu z publicznym adresem IP, wpisz następujące polecenie:

```azurecli-interactive
kubectl expose pods iis --port=80 --type=LoadBalancer
```

To polecenie spowoduje utworzenie przez rozwiązanie Kubernetes usługi i [reguły usługi Azure Load Balancer](container-service-kubernetes-load-balancing.md) z publicznym adresem IP usługi. 

Uruchom poniższe polecenie, aby wyświetlić stan usługi.

```azurecli-interactive
kubectl get svc
```

Początkowo adres IP będzie widoczny jako `pending`. Po kilku minutach zostanie ustawiony zewnętrzny adres IP zasobnika `iis`:
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
iis          10.0.111.25    13.64.158.233   80/TCP         22m
```

Możesz użyć wybranej przeglądarki internetowej, aby wyświetlić domyślną stronę powitalną usług IIS pod zewnętrznym adresem IP:

![Obraz przedstawiający przechodzenie do usług IIS](media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  


<a id="delete-cluster" class="xliff"></a>

## Usuwanie klastra
Gdy klaster nie będzie już potrzebny, możesz usunąć grupę zasobów, usługę kontenera i wszystkie pokrewne zasoby za pomocą polecenia [az group delete](/cli/azure/group#delete).

```azurecli-interactive 
az group delete --name myResourceGroup
```


<a id="next-steps" class="xliff"></a>

## Następne kroki

W tym przewodniku Szybki start wdrożono klaster Kubernetes, nawiązano połączenie z narzędziem `kubectl` i wdrożono zasobnik za pomocą kontenera usług IIS. Aby dowiedzieć się więcej na temat usługi Azure Container Service, przejdź do samouczka dotyczącego rozwiązania Kubernetes.

> [!div class="nextstepaction"]
> [Zarządzanie klastrem Kubernetes usługi ASC](./container-service-tutorial-kubernetes-prepare-app.md)

