---
title: Klaster Azure Kubernetes dla systemu Windows | Microsoft Docs
description: "Wdrażanie klastra Kubernetes dla kontenerów systemu Windows i rozpoczynanie z nim pracy w usłudze Azure Container Service"
services: container-service
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
ms.date: 05/04/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 4e730b65a98af05ea00c5f8ebd9914e3367b66a7
ms.contentlocale: pl-pl
ms.lasthandoff: 05/09/2017


---

# <a name="get-started-with-kubernetes-and-windows-containers-in-container-service"></a>Rozpoczynanie pracy z kontenerami Kubernetes i Windows w usłudze Container Service


W tym artykule przedstawiono sposób tworzenia w usłudze Azure Container Service klastra Kubernetes, który zawiera węzły systemu Windows umożliwiające uruchamianie kontenerów systemu Windows. Zacznij korzystać z poleceń `az acs` interfejsu wiersza polecenia platformy Azure 2.0 w celu utworzenia klastra Kubernetes w usłudze Azure Container Service. Następnie za pomocą narzędzia wiersza polecenia Kubernetes `kubectl` zacznij korzystać z kontenerów systemu Windows utworzonych z obrazów platformy Docker. 

> [!NOTE]
> Obsługa kontenerów systemu Windows przy użyciu usługi Kubernetes w usłudze Azure Container Service jest dostępna w wersji zapoznawczej. 
>



Na poniższej ilustracji przedstawiono architekturę klastra Kubernetes w usłudze Azure Container Service z jednym węzłem głównym systemu Linux i dwoma węzłami agenta systemu Windows. 

![Obraz przedstawiający klaster Kubernetes na platformie Azure](media/container-service-kubernetes-windows-walkthrough/kubernetes-windows.png)

* Węzeł główny systemu Linux obsługuje interfejs API REST usługi Kubernetes i jest dostępny za pośrednictwem protokołu SSH w porcie 22 lub za pomocą narzędzia `kubectl` w porcie 443. 
* Węzły agenta systemu Windows są zgrupowane w zestawie dostępności platformy Azure i odpowiadają za uruchamianie kontenerów. Węzły systemu Windows są dostępne przez tunel protokołu RDP SSH za pośrednictwem węzła głównego. Reguły usługi Azure Load Balancer są dodawane dynamicznie do klastra w zależności od uwidocznionych usług.



Wszystkie maszyny wirtualne znajdują w tej samej prywatnej sieci wirtualnej i są dla siebie w pełni dostępne. Wszystkie maszyny wirtualne uruchamiają agenta kubelet, usługę Docker i serwer proxy.

Aby uzyskać więcej ogólnych informacji, zobacz [Wprowadzenie do usługi Azure Container Service](container-service-intro.md) i [Dokumentacja rozwiązania Kubernetes](https://kubernetes.io/docs/home/).

## <a name="prerequisites"></a>Wymagania wstępne
Aby utworzyć klaster usługi Azure Container Service przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0, należy:
* posiadać konto platformy Azure ([skorzystaj z bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/)),
* mieć zainstalowany [interfejs wiersza polecenia platformy Azure w wersji 2.0](/cli/azure/install-az-cli2) i zalogować się do niego.

W przypadku klastra Kubernetes potrzebne są także następujące elementy. Możesz je przygotować wcześniej lub wygenerować automatycznie podczas wdrażania klastra za pomocą opcji polecenia `az acs create`. 

* **Klucz publiczny SSH RSA**: jeśli chcesz utworzyć klucze Secure Shell (SSH) RSA, zobacz wskazówki dla systemów [macOS i Linux](../virtual-machines/linux/mac-create-ssh-keys.md) lub [Windows](../virtual-machines/linux/ssh-from-windows.md). 

* **Wpis tajny i identyfikator klienta jednostki usługi**: aby uzyskać więcej informacji oraz wskazówki dotyczące tworzenia jednostki usługi Azure Active Directory, zobacz [About the service principal for a Kubernetes cluster](container-service-kubernetes-service-principal.md) (Informacje o jednostce usługi dla klastra Kubernetes).

Przykładowe polecenie w tym artykule automatycznie generuje klucze SSH i jednostkę usługi.
  
## <a name="create-your-kubernetes-cluster"></a>Tworzenie klastra Kubernetes

Oto polecenia interfejsu wiersza polecenia platformy Azure 2.0 umożliwiające utworzenie klastra. 

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Utwórz grupę zasobów w lokalizacji, w której usługa Azure Container Service jest [dostępna](https://azure.microsoft.com/regions/services/). Poniższe polecenie tworzy grupę zasobów o nazwie *myKubernetesResourceGroup* w lokalizacji *westus*:

```azurecli
az group create --name=myKubernetesResourceGroup --location=westus
```

### <a name="create-a-kubernetes-cluster-with-windows-agent-nodes"></a>Tworzenie klastra Kubernetes z węzłami agenta systemu Windows

Utwórz klaster Kubernetes w swojej grupie zasobów przy użyciu polecenia `az acs create` z opcją agenta `--orchestrator-type=kubernetes` i `--windows`. Zobacz [pomoc](/cli/azure/acs#create) dotyczącą polecenia `az acs create`, aby poznać jego składnię.

Poniższe polecenie tworzy klaster usługi Container Service o nazwie *myKubernetesClusterName* z prefiksem DNS *myPrefix* dla węzła zarządzania i podanych poświadczeń umożliwiających dotarcie do węzłów systemu Windows. Ta wersja polecenia automatycznie generuje klucze SSH RSA i jednostkę usługi dla klastra Kubernetes.


```azurecli
az acs create --orchestrator-type=kubernetes \
    --resource-group myKubernetesResourceGroup \
    --name=myKubernetesClusterName \
    --dns-prefix=myPrefix \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username myWindowsAdminName \
    --admin-password myWindowsAdminPassword
```

Po kilku minutach, po zakończeniu tego polecenia, powinien zostać utworzony działający klaster Kubernetes.

> [!IMPORTANT]
> Jeśli Twoje konto nie ma uprawnień do utworzenia jednostki usługi Azure AD, polecenie spowoduje wygenerowanie błędu podobnego do następującego: `Insufficient privileges to complete the operation.`. Aby uzyskać więcej informacji, zobacz [About the service principal for a Kubernetes cluster](container-service-kubernetes-service-principal.md) (Informacje o jednostce usługi dla klastra Kubernetes). 
> 

## <a name="connect-to-the-cluster-with-kubectl"></a>Łączenie z klastrem za pomocą narzędzia kubectl

Aby nawiązać połączenie z klastrem Kubernetes z komputera klienckiego, należy użyć klienta wiersza polecenia usługi Kubernetes [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/). 

Jeśli narzędzie `kubectl` nie jest zainstalowane lokalnie, możesz je zainstalować przy użyciu polecenia `az acs kubernetes install-cli`. (Możesz je także pobrać z [witryny rozwiązania Kubernetes](https://kubernetes.io/docs/tasks/kubectl/install/)).

**Linux lub macOS**

```azurecli
sudo az acs kubernetes install-cli
```

**Windows**
```azurecli
az acs kubernetes install-cli
```

> [!TIP]
> Domyślnie to polecenie powoduje zainstalowanie pliku binarnego narzędzia `kubectl` w katalogu `/usr/local/bin/kubectl` w systemie Linux lub macOS bądź pliku `C:\Program Files (x86)\kubectl.exe` w systemie Windows. Aby określić inną ścieżkę instalacji, użyj parametru `--install-location`.
>
> Po zainstalowaniu narzędzia `kubectl` upewnij się, że jego katalog znajduje się w ścieżce systemowej lub dodaj go do tej ścieżki. 


Następnie uruchom poniższe polecenie, aby pobrać główną konfigurację klastra Kubernetes do pliku lokalnego `~/.kube/config`:

```azurecli
az acs kubernetes get-credentials --resource-group=myKubernetesResourceGroup --name=myKubernetesClusterName
```

Na tym etapie klaster jest dostępny z poziomu Twojej maszyny. Spróbuj uruchomić polecenie:

```bash
kubectl get nodes
```

Sprawdź, czy lista maszyn są jest widoczna w klastrze.

![Węzły działające w klastrze Kubernetes](media/container-service-kubernetes-windows-walkthrough/kubectl-get-nodes.png)

## <a name="create-your-first-kubernetes-service"></a>Tworzenie pierwszej usługi Kubernetes

Po utworzeniu klastra i nawiązaniu połączenia z narzędziem `kubectl` spróbuj uruchomić aplikację systemu Windows i udostępnić ją w Internecie. W tym podstawowym przykładzie użyto pliku JSON do określenia kontenera Microsoft Internet Information Server (IIS), a następnie utworzono go przy użyciu polecenia `kubctl apply`. 

1. Utwórz plik lokalny o nazwie `iis.json` i skopiuj następujące dane. Ten plik nakazuje rozwiązaniu Kubernetes uruchomienie usług IIS w systemie Windows Server 2016 Server Core przy użyciu obrazu publicznego z witryny [Docker Hub](https://hub.docker.com/r/microsoft/iis/). Kontener korzysta z portu 80, ale początkowo jest dostępny tylko w ramach sieci klastra.

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
          "image": "microsoft/iis",
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
2. Aby uruchomić aplikację, wpisz:  
  
  ```bash
  kubectl apply -f iis.json
  ```  
3. Aby śledzić wdrażanie kontenera, wpisz:  
  ```bash
  kubectl get pods
  ```
  Podczas wdrażania kontenera jego stan zmieni się na `ContainerCreating`. 

  ![Kontener IIS w stanie ContainerCreating](media/container-service-kubernetes-windows-walkthrough/iis-pod-creating.png)   

  Ze względu na rozmiar obrazu IIS przejście kontenera do stanu `Running` może potrwać kilka minut.

  ![Kontener IIS w stanie Uruchomiony](media/container-service-kubernetes-windows-walkthrough/iis-pod-running.png)

4. Aby udostępnić kontener dla całego świata, wpisz następujące polecenie:

  ```bash
  kubectl expose pods iis --port=80 --type=LoadBalancer
  ```

  To polecenie spowoduje utworzenie przez rozwiązanie Kubernetes reguły usługi Azure Load Balancer z publicznym adresem IP. Zmiana ta zajmuje kilka minut i powoduje przeprowadzenie propagacji do modułu równoważenia obciążenia. Aby uzyskać szczegółowe informacje, zobacz [Load balance containers in a Kubernetes cluster in Azure Container Service](container-service-kubernetes-load-balancing.md) (Kontenery równoważenia obciążenia w klastrze Kubernetes w usłudze Azure Container Service).

5. Uruchom poniższe polecenie, aby wyświetlić stan usługi.

  ```bash
  kubectl get svc
  ```

  Początkowo adres IP będzie widoczny jako `pending`:

  ![Oczekujący zewnętrzny adres IP](media/container-service-kubernetes-windows-walkthrough/iis-svc-expose.png)

  Po kilku minutach adres IP zostanie ustawiony:
  
  ![Zewnętrzny adres IP dla usług IIS](media/container-service-kubernetes-windows-walkthrough/iis-svc-expose-public.png)


6. Gdy zewnętrzny adres IP zostanie udostępniony, możesz przejść do niego w przeglądarce:

  ![Obraz przedstawiający przechodzenie do usług IIS](media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  

7. Aby usunąć zasobnik IIS, wpisz:

  ```bash
  kubectl delete pods iis
  ```

## <a name="next-steps"></a>Następne kroki

* Aby korzystać z interfejsu użytkownika rozwiązania Kubernetes, uruchom polecenie `kubectl proxy`. Następnie przejdź pod adres http://localhost:8001/ui.

* Procedurę utworzenia niestandardowej witryny sieci Web usług IIS i uruchomienia jej w kontenerze systemu Windows można znaleźć w witrynie [Docker Hub](https://hub.docker.com/r/microsoft/iis/).

* Aby uzyskać dostęp do węzłów systemu Windows przez tunel SSH protokołu RDP do węzła głównego przy użyciu programu PuTTy, zobacz [dokumentację usługi ACS-Engine](https://github.com/Azure/acs-engine/blob/master/docs/ssh.md#create-port-80-tunnel-to-the-master). 

