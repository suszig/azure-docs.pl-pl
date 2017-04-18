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
ms.date: 04/05/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 5c529ae41b42d276d37e6103305e33ed04694e18
ms.lasthandoff: 04/07/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Rozpoczynanie pracy z klastrem Kubernetes w usłudze Container Service


W tym przewodniku pokazano sposób korzystania z poleceń interfejsu wiersza polecenia platformy Azure 2.0 w celu utworzenia klastra Kubernetes w usłudze Azure Container Service. Następnie można użyć narzędzia wiersza polecenia `kubectl`, aby rozpocząć pracę z kontenerami w klastrze.

Na poniższej ilustracji przedstawiono architekturę klastra usługi Container Service z jednym węzłem głównym i dwoma agentami. Węzeł główny obsługuje interfejs API REST rozwiązania Kubernetes. Węzły agenta są zgrupowane w zestawie dostępności platformy Azure i odpowiadają za uruchamianie kontenerów. Wszystkie maszyny wirtualne znajdują w tej samej prywatnej sieci wirtualnej i są dla siebie w pełni dostępne.

![Obraz przedstawiający klaster Kubernetes na platformie Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="prerequisites"></a>Wymagania wstępne
W tym przewodniku przyjęto założenie, że zainstalowano i skonfigurowano już [interfejs wiersza polecenia platformy Azure 2.0](/cli/azure/install-az-cli2). 

W przykładach poleceń założono, że interfejs wiersza polecenia platformy Azure jest uruchamiany w powłoce bash, zarówno w systemie Linux, jak i macOS. Jeśli interfejs wiersza polecenia platformy Azure jest uruchamiany na kliencie systemu Windows, niektóre elementy składni skryptów i pliki mogą się różnić w zależności od powłoki poleceń. 

## <a name="create-your-kubernetes-cluster"></a>Tworzenie klastra Kubernetes

Poniżej przedstawiono krótki opis poleceń powłoki używanych do tworzenia klastra za pomocą interfejsu wiersza polecenia platformy Azure w wersji 2.0. 

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Aby utworzyć klaster, najpierw musisz utworzyć grupę zasobów w określonej lokalizacji. Uruchom polecenia podobne do następujących:

```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Tworzenie klastra
Po utworzeniu grupy zasobów możesz utworzyć klaster w tej grupie. W poniższym przykładzie użyto opcji `--generate-ssh-keys`, która powoduje wygenerowanie niezbędnych plików publicznych i prywatnych kluczy SSH na potrzeby wdrożenia, jeśli nie istnieją one jeszcze w domyślnym katalogu `~/.ssh/`. 

To polecenie generuje również automatycznie [nazwę główną usługi Azure Active Directory](container-service-kubernetes-service-principal.md), z której korzysta klaster Kubernetes na platformie Azure.

```azurecli
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX --generate-ssh-keys
```


Po kilku minutach, po zakończeniu tego polecenia, powinien zostać utworzony działający klaster Kubernetes.

### <a name="connect-to-the-cluster"></a>Łączenie z klastrem

Aby nawiązać połączenie z klastrem Kubernetes z komputera klienckiego, należy użyć klienta wiersza polecenia usługi Kubernetes [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/). 

Jeśli nie masz go zainstalowanego narzędzia `kubectl`, możesz go zainstalować przy użyciu polecenia:

```azurecli
sudo az acs kubernetes install-cli
```
> [!TIP]
> Domyślnie to polecenie powoduje zainstalowanie pliku binarnego narzędzia `kubectl` w katalogu `/usr/local/bin/kubectl` w systemie Linux lub macOS bądź pliku `C:\Program Files (x86)\kubectl.exe` w systemie Windows. Aby określić inną ścieżkę instalacji, użyj parametru `--install-location`.
>

Po zainstalowaniu narzędzia `kubectl` upewnij się, że jego katalog znajduje się w ścieżce systemowej lub dodaj go do tej ścieżki. 


Następnie uruchom poniższe polecenie, aby pobrać główną konfigurację klastra Kubernetes do pliku `~/.kube/config`:

```azurecli
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

Aby poznać więcej opcji instalowania i konfigurowania narzędzia `kubectl`, zobacz [Łączenie z klastrem usługi Azure Container Service](container-service-connect.md).

Na tym etapie klaster powinien być dostępny z poziomu Twojej maszyny. Spróbuj uruchomić polecenie:

```bash
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

```bash
kubectl run nginx --image nginx
```

To polecenie uruchamia kontener platformy Docker Nginx w zasobniku jednego z węzłów.

Aby wyświetlić działający kontener, uruchom następujące polecenie:

```bash
kubectl get pods
```

### <a name="expose-the-service-to-the-world"></a>Udostępnianie usługi dla całego świata
Aby udostępnić usługę dla całego świata, utwórz element `Service` rozwiązania Kubernetes typu `LoadBalancer`:

```bash
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

To polecenie powoduje utworzenie przez rozwiązanie Kubernetes reguły usługi Azure Load Balancer z publicznym adresem IP. Zmiana ta zajmuje kilka minut i powoduje przeprowadzenie propagacji do modułu równoważenia obciążenia. Aby uzyskać więcej informacji, zobacz [Load balance containers in a Kubernetes cluster in Azure Container Service](container-service-kubernetes-load-balancing.md) (Kontenery równoważenia obciążenia w klastrze Kubernetes w usłudze Azure Container Service).

Uruchom następujące polecenie, aby obserwować zmianę usługi ze stanu `pending` w celu wyświetlenia zewnętrznego adresu IP:

```bash
watch 'kubectl get svc'
```

  ![Obraz przedstawiający obserwację zmiany stanu oczekiwania na zewnętrzny adres IP](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Gdy zewnętrzny adres IP zostanie wyświetlony, możesz przejść do niego w przeglądarce:

  ![Obraz przedstawiający przechodzenie do serwera Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Przeglądanie interfejsu użytkownika rozwiązania Kubernetes
Aby wyświetlić interfejs sieci Web rozwiązania Kubernetes, możesz użyć polecenia:

```bash
kubectl proxy
```
To polecenie powoduje uruchomienie na hoście lokalnym prostego, uwierzytelnionego serwera proxy, który umożliwia wyświetlenie interfejsu użytkownika sieci Web rozwiązania Kubernetes uruchomionego pod adresem [http://localhost:8001/ui](http://localhost:8001/ui). Aby uzyskać więcej informacji, zobacz [Using the Kubernetes web UI with Azure Container Service](container-service-kubernetes-ui.md) (Używanie interfejsu użytkownika sieci Web usługi Kubernetes za pomocą usługi Azure Container Service).

![Obraz przedstawiający pulpit nawigacyjny rozwiązania Kubernetes](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Sesje zdalne wewnątrz kontenerów
Rozwiązanie Kubernetes pozwala uruchamiać polecenia w zdalnym kontenerze platformy Docker działającym w klastrze.

```bash
# Get the name of your nginx pods
kubectl get pods
```

Przy użyciu nazwy zasobnika możesz uruchomić w tym zasobniku polecenie zdalne.  Na przykład:

```bash
kubectl exec <pod name> date
```

Oprócz tego możesz uzyskać w pełni interaktywną sesję, korzystając z flag `-it`:

```bash
kubectl exec <pod name> -it bash
```

![Sesja zdalna wewnątrz kontenera](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)



## <a name="next-steps"></a>Następne kroki

Aby wykonać więcej czynności przy użyciu klastra Kubernetes, zobacz następujące zasoby:

* Scenariusz [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/) — pokazuje, jak wdrażać, skalować, aktualizować i debugować aplikacje konteneryzowane.
* [Podręcznik użytkownika rozwiązania Kubernetes](http://kubernetes.io/docs/user-guide/) — zawiera informacje na temat uruchamiania programów w istniejącym klastrze Kubernetes.
* [Przykłady dotyczące rozwiązania Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples) — przykłady związane z uruchamianiem prawdziwych aplikacji przy użyciu rozwiązania Kubernetes.

