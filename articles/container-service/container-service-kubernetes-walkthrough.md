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
ms.date: 11/15/2016
ms.author: anhowe
translationtype: Human Translation
ms.sourcegitcommit: 7ed8fb75f057d5a7cfde5436e72e8fec52d07156
ms.openlocfilehash: f3b2fc301bf7083f192c0ec872c4e032472eef97


---

# <a name="microsoft-azure-container-service-engine---kubernetes-walkthrough"></a>Aparat usługi Microsoft Azure Container Service — przewodnik po rozwiązaniu Kubernetes

## <a name="prerequisites"></a>Wymagania wstępne
W tym przewodniku założono, że masz zainstalowane [narzędzie wiersza polecenia „azure-cli”](https://github.com/azure/azure-cli#installation) i utworzony [klucz publiczny SSH](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) w pliku `~/.ssh/id_rsa.pub`.

## <a name="overview"></a>Omówienie

Poniższe instrukcje służą do utworzenia klastra Kubernetes z jednym węzłem głównym i dwoma węzłami procesu roboczego.
Węzeł główny obsługuje interfejs API REST rozwiązania Kubernetes.  Węzły procesu roboczego są zgrupowane w zestawie dostępności platformy Azure i odpowiadają za uruchamianie kontenerów. Wszystkie maszyny wirtualne znajdują w tej samej prywatnej sieci wirtualnej i są dla siebie w pełni dostępne.

> [!NOTE]
> Obsługa klastra Kubernetes w usłudze Azure Container Service jest obecnie w wersji zapoznawczej.
>

Na poniższej ilustracji przedstawiono architekturę klastra usługi kontenera z jednym węzłem głównym i dwoma agentami:

![Obraz przedstawiający klaster Kubernetes na platformie Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="creating-your-kubernetes-cluster"></a>Tworzenie klastra Kubernetes

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Aby utworzyć klaster, najpierw musisz utworzyć grupę zasobów w określonej lokalizacji — możesz to zrobić przy użyciu następującego kodu:
```console
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Tworzenie klastra
Po utworzeniu grupy zasobów możesz utworzyć w tej grupie klaster za pomocą następującego kodu:
```console
DNS_PREFIX=some-unique-value
SERVICE_NAME=any-acs-service-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$SERVICE_NAME --dns-prefix=$DNS_PREFIX
```

> [!NOTE]
> Narzędzie azure-cli przekaże plik `~/.ssh/id_rsa.pub` do maszyn wirtualnych z systemem Linux.
>

Po wykonaniu tego polecenia powinien być dostępny działający klaster Kubernetes.

### <a name="configure-kubectl"></a>Konfigurowanie narzędzia kubectl
Narzędzie `kubectl` to klient wiersza polecenia rozwiązania Kubernetes.  Jeśli nie masz go jeszcze zainstalowanego, możesz go zainstalować przy użyciu polecenia:

```console
az acs kubernetes install-cli
```

Po zainstalowaniu narzędzia `kubectl` uruchomienie poniższego polecenia spowoduje pobranie głównej konfiguracji klastra Kubernetes do pliku ~/.kube/config
```console
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$SERVICE_NAME
```

Na tym etapie klaster powinien być dostępny z poziomu Twojej maszyny. Spróbuj uruchomić polecenie:
```console
kubectl get nodes
```

Sprawdź też, czy maszyny są widoczne w klastrze.

## <a name="create-your-first-kubernetes-service"></a>Tworzenie pierwszej usługi Kubernetes

Po ukończeniu tego przewodnika będziesz wiedzieć, jak:
 * wdrożyć aplikację platformy Docker i udostępnić ją światu,
 * uruchamiać polecenia w kontenerze przy użyciu polecenia `kubectl exec`, 
 * uzyskiwać dostęp do pulpitu nawigacyjnego rozwiązania Kubernetes.

### <a name="start-a-simple-container"></a>Uruchamianie prostego kontenera
Do uruchomienia prostego kontenera (w tym przypadku serwera sieci Web `nginx`) służy następujące polecenie:

```console
kubectl run nginx --image nginx
```

To polecenie uruchamia kontener platformy Docker nginx w zasobniku jednego z węzłów.

Możesz uruchomić polecenie
```console
kubectl get pods
```

w celu zobaczenia uruchomionego kontenera.

### <a name="expose-the-service-to-the-world"></a>Udostępnianie usługi dla całego świata
Aby udostępnić usługę całemu światu,  możesz utworzyć element `Service` rozwiązania Kubernetes typu `LoadBalancer`:

```console
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Spowoduje to utworzenie przez rozwiązanie Kubernetes wystąpienia usługi Azure Load Balancer z publicznym adresem IP. Zmiana ta zajmuje około 2–3 minut i powoduje przeprowadzenie propagacji do modułu równoważenia obciążenia.

Aby zobaczyć, jak stan usługi zmienia się ze stanu „pending” (oczekiwanie) na zewnętrzny adres IP, wpisz polecenie:
```console
watch 'kubectl get svc'
```

  ![Obraz przedstawiający obserwację zmiany stanu oczekiwania na zewnętrzny adres IP](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Gdy zewnętrzny adres IP zostanie wyświetlony, możesz przejść do niego w przeglądarce:

  ![Obraz przedstawiający przechodzenie do serwera nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Przeglądanie interfejsu użytkownika rozwiązania Kubernetes
Aby wyświetlić interfejs sieci Web rozwiązania Kubernetes, możesz użyć polecenia:

```console
kubectl proxy
```
Spowoduje to uruchomienie na hoście lokalnym prostego, uwierzytelnionego serwera proxy, który umożliwia wyświetlenie [interfejsu użytkownika rozwiązania Kubernetes](http://localhost:8001/ui)

![Obraz przedstawiający pulpit nawigacyjny rozwiązania Kubernetes](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Sesje zdalne wewnątrz kontenerów
Rozwiązanie Kubernetes pozwala uruchamiać polecenia w zdalnym kontenerze platformy Docker działającym w klastrze.

```console
# Get the name of your nginx pod
kubectl get pods
```

Przy użyciu nazwy zasobnika możesz uruchomić w tym zasobniku polecenie zdalne.  Na przykład:
```console
kubectl exec nginx-701339712-retbj date
```

Oprócz tego możesz uzyskać w pełni interaktywną sesję, korzystając z flag `-it`:

```console
kubectl exec nginx-701339712-retbj -it bash
```

![Obraz przedstawiający polecenie curl z adresem IP zasobnika](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)


## <a name="details"></a>Szczegóły
### <a name="installing-the-kubectl-configuration-file"></a>Instalowanie pliku konfiguracji narzędzia kubectl
Po uruchomieniu polecenia `az acs kubernetes get-credentials` plik konfiguracji rozwiązania Kubernetes na potrzeby dostępu zdalnego został umieszczony w katalogu głównym ~/.kube/config.

Jeśli będziesz potrzebować pobrać go bezpośrednio, możesz użyć polecenia `ssh` w systemie Linux lub OS X bądź programu `Putty` w systemie Windows:

#### <a name="windows"></a>Windows
Aby użyć narzędzia pscp z programu [PuTTy](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html),  upewnij się, że Twój certyfikat jest udostępniony za pośrednictwem programu [Pageant](https://github.com/Azure/acs-engine/blob/master/docs/ssh.md#key-management-and-agent-forwarding-with-windows-pageant):
  ```
  # MASTERFQDN is obtained in step1
  pscp azureuser@MASTERFQDN:.kube/config .
  SET KUBECONFIG=%CD%\config
  kubectl get nodes
  ```

#### <a name="os-x-or-linux"></a>OS X lub Linux:
  ```
  # MASTERFQDN is obtained in step1
  scp azureuser@MASTERFQDN:.kube/config .
  export KUBECONFIG=`pwd`/config
  kubectl get nodes
  ```
## <a name="learning-more"></a>Dodatkowe informacje

### <a name="azure-container-service"></a>Azure Container Service

1. [Dokumentacja usługi Azure Container Service](https://azure.microsoft.com/en-us/documentation/services/container-service/)
2. [Aparat typu open source usługi Azure Container Service](https://github.com/azure/acs-engine)

### <a name="kubernetes-community-documentation"></a>Dokumentacja społeczności rozwiązania Kubernetes

1. Scenariusz [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/) — pokazuje, jak wdrażać, skalować, aktualizować i debugować aplikacje konteneryzowane.
2. [Podręcznik użytkownika rozwiązania Kubernetes](http://kubernetes.io/docs/user-guide/) — zawiera informacje na temat uruchamiania programów w istniejącym klastrze Kubernetes.
3. [Przykłady dotyczące rozwiązania Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples) — liczne przykłady związane z uruchamianiem prawdziwych aplikacji przy użyciu rozwiązania Kubernetes.



<!--HONumber=Jan17_HO4-->


