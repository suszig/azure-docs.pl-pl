---
title: "Klaster Kubernetes — szybki start na platformie Azure | Microsoft Docs"
description: "Wdrażanie klastra Kubernetes i rozpoczynanie z nim pracy w usłudze Azure Container Service"
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
ms.date: 03/01/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: e4f47341554e2de514c8be2f5c85983d09bbb760
ms.lasthandoff: 04/03/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Rozpoczynanie pracy z klastrem Kubernetes w usłudze Container Service


Instrukcje w tym artykule przedstawiają sposób tworzenia klastra Kubernetes za pomocą poleceń interfejsu wiersza polecenia platformy Azure w wersji 2.0. Następnie można użyć narzędzia wiersza polecenia `kubectl`, aby rozpocząć pracę z kontenerami w klastrze.

Na poniższej ilustracji przedstawiono architekturę klastra usługi kontenera z jednym węzłem głównym i dwoma agentami. Węzeł główny obsługuje interfejs API REST rozwiązania Kubernetes. Węzły agenta są zgrupowane w zestawie dostępności platformy Azure i odpowiadają za uruchamianie kontenerów. Wszystkie maszyny wirtualne znajdują w tej samej prywatnej sieci wirtualnej i są dla siebie w pełni dostępne.

![Obraz przedstawiający klaster Kubernetes na platformie Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="prerequisites"></a>Wymagania wstępne
W tym przewodniku przyjęto założenie, że zainstalowano i skonfigurowano już [interfejs wiersza polecenia platformy Azure w wersji 2.0](/cli/azure/install-az-cli2). Musisz mieć również klucz publiczny SSH RSA w pliku `~/.ssh/id_rsa.pub`. Jeśli go nie masz, zobacz czynności dla systemów [OS X i Linux](../virtual-machines/linux/mac-create-ssh-keys.md) lub [Windows](../virtual-machines/linux/ssh-from-windows.md).






## <a name="create-your-kubernetes-cluster"></a>Tworzenie klastra Kubernetes

Poniżej przedstawiono krótki opis poleceń powłoki używanych do tworzenia klastra za pomocą interfejsu wiersza polecenia platformy Azure w wersji 2.0. Aby uzyskać więcej informacji, zobacz [Tworzenie klastra usługi Azure Container Service przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0](container-service-create-acs-cluster-cli.md).

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Aby utworzyć klaster, najpierw musisz utworzyć grupę zasobów w określonej lokalizacji. Uruchom polecenia podobne do następujących:

```console
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Tworzenie klastra
Po utworzeniu grupy zasobów możesz utworzyć klaster w tej grupie:

```console
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX
```

> [!NOTE]
> W trakcie wdrażania interfejs wiersza polecenia przekazuje plik `~/.ssh/id_rsa.pub` do maszyn wirtualnych z systemem Linux.
>

Po wykonaniu tego polecenia powinien być dostępny działający klaster Kubernetes.

### <a name="connect-to-the-cluster"></a>Łączenie z klastrem

Poniżej przedstawiono polecenia interfejsu wiersza polecenia platformy Azure służące do łączenia się z klastrem Kubernetes z komputera klienckiego za pomocą narzędzia `kubectl`, klienta wiersza polecenia usługi Kubernetes. Aby uzyskać więcej informacji, zobacz [Łączenie z klastrem usługi Azure Container Service](container-service-connect.md).

Jeśli nie masz go zainstalowanego narzędzia `kubectl`, możesz go zainstalować przy użyciu polecenia:

```console
az acs kubernetes install-cli
```

Po zainstalowaniu narzędzia `kubectl` uruchom poniższe polecenie, aby pobrać główną konfigurację klastra Kubernetes do pliku ~/.kube/config:

```console
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

Na tym etapie klaster powinien być dostępny z poziomu Twojej maszyny. Spróbuj uruchomić polecenie:
```console
kubectl get nodes
```

Sprawdź, czy lista maszyn są jest widoczna w klastrze.

## <a name="create-your-first-kubernetes-service"></a>Tworzenie pierwszej usługi Kubernetes

Po ukończeniu tego przewodnika będziesz wiedzieć, jak:
 * wdrożyć aplikację platformy Docker i udostępnić ją światu,
 * uruchamiać polecenia w kontenerze przy użyciu polecenia `kubectl exec`, 
 * uzyskiwać dostęp do pulpitu nawigacyjnego rozwiązania Kubernetes.

### <a name="start-a-simple-container"></a>Uruchamianie prostego kontenera
Do uruchomienia prostego kontenera (w tym przypadku serwera sieci Web Nginx) służy następujące polecenie:

```console
kubectl run nginx --image nginx
```

To polecenie uruchamia kontener platformy Docker Nginx w zasobniku jednego z węzłów.

Aby wyświetlić działający kontener, uruchom następujące polecenie:

```console
kubectl get pods
```

### <a name="expose-the-service-to-the-world"></a>Udostępnianie usługi dla całego świata
Aby udostępnić usługę dla całego świata, utwórz element `Service` rozwiązania Kubernetes typu `LoadBalancer`:

```console
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Spowoduje to utworzenie przez rozwiązanie Kubernetes reguły usługi Azure Load Balancer z publicznym adresem IP. Zmiana ta zajmuje kilka minut i powoduje przeprowadzenie propagacji do modułu równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [Load balance containers in a Kubernetes cluster in Azure Container Service](container-service-kubernetes-load-balancing.md) (Kontenery równoważenia obciążenia w klastrze Kubernetes w usłudze Azure Container Service).

Uruchom następujące polecenie, aby obserwować zmianę usługi ze stanu `pending` w celu wyświetlenia zewnętrznego adresu IP:

```console
watch 'kubectl get svc'
```

  ![Obraz przedstawiający obserwację zmiany stanu oczekiwania na zewnętrzny adres IP](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Gdy zewnętrzny adres IP zostanie wyświetlony, możesz przejść do niego w przeglądarce:

  ![Obraz przedstawiający przechodzenie do serwera Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Przeglądanie interfejsu użytkownika rozwiązania Kubernetes
Aby wyświetlić interfejs sieci Web rozwiązania Kubernetes, możesz użyć polecenia:

```console
kubectl proxy
```
Spowoduje to uruchomienie na hoście lokalnym prostego, uwierzytelnionego serwera proxy, który umożliwia wyświetlenie interfejsu użytkownika sieci Web rozwiązania Kubernetes uruchomionego pod adresem [http://localhost:8001/ui](http://localhost:8001/ui). Aby uzyskać więcej informacji, zobacz [Using the Kubernetes web UI with Azure Container Service](container-service-kubernetes-ui.md) (Używanie interfejsu użytkownika sieci Web usługi Kubernetes za pomocą usługi Azure Container Service).

![Obraz przedstawiający pulpit nawigacyjny rozwiązania Kubernetes](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Sesje zdalne wewnątrz kontenerów
Rozwiązanie Kubernetes pozwala uruchamiać polecenia w zdalnym kontenerze platformy Docker działającym w klastrze.

```console
# Get the name of your nginx pods
kubectl get pods
```

Przy użyciu nazwy zasobnika możesz uruchomić w tym zasobniku polecenie zdalne.  Na przykład:

```console
kubectl exec <pod name> date
```

Oprócz tego możesz uzyskać w pełni interaktywną sesję, korzystając z flag `-it`:

```console
kubectl exec <pod name> -it bash
```

![Sesja zdalna wewnątrz kontenera](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)



## <a name="next-steps"></a>Następne kroki

Aby wykonać więcej czynności przy użyciu klastra Kubernetes, zobacz następujące zasoby:

* Scenariusz [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/) — pokazuje, jak wdrażać, skalować, aktualizować i debugować aplikacje konteneryzowane.
* [Podręcznik użytkownika rozwiązania Kubernetes](http://kubernetes.io/docs/user-guide/) — zawiera informacje na temat uruchamiania programów w istniejącym klastrze Kubernetes.
* [Przykłady dotyczące rozwiązania Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples) — przykłady związane z uruchamianiem prawdziwych aplikacji przy użyciu rozwiązania Kubernetes.

