---
title: "Wpięć CI/CD z Kubernetes w usłudze kontenera platformy Azure | Dokumentacja firmy Microsoft"
description: "Jak zautomatyzować proces CI/CD z Wpięć do wdrożenia i uaktualnienia konteneryzowanych aplikacji na Kubernetes usługi kontenera platformy Azure"
services: container-service
documentationcenter: 
author: chzbrgr71
manager: johny
editor: 
tags: acs, azure-container-service, jenkins
keywords: "Rozwiązanie docker kontenerów, Kubernetes, Azure, Wpięć"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: briar
ms.custom: mvc
ms.openlocfilehash: c4b833b4d3234adc3f44a84f253ff9b8d78b1e23
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="jenkins-integration-with-azure-container-service-and-kubernetes"></a>Wpięć integracji z usługą kontenera Azure i Kubernetes 

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

W tym samouczku będziemy przeprowadzenie procesu umożliwia konfigurowanie ciągłej integracji aplikacji kontenera wielu do Kubernetes usługi kontenera platformy Azure przy użyciu platformy Wpięć. Przepływ pracy aktualizacji obrazu kontenera w Centrum Docker i uaktualnień stanowiskami Kubernetes przy użyciu wdrażania. 

## <a name="high-level-process"></a>Wysokiego poziomu ładowania
Podstawowe kroki szczegółowo opisane w tym artykule przedstawiono: 
- Zainstaluj Kubernetes klaster usługi kontenera
- Konfigurowanie Wpięć i konfigurowanie dostępu do usługi kontenera
- Tworzenie przepływów pracy Wpięć
- Testowanie procesu CI/CD pełnego

## <a name="install-a-kubernetes-cluster"></a>Zainstaluj klaster Kubernetes
    
Wdrażanie klastra Kubernetes w usłudze kontenera platformy Azure, wykonując następujące kroki. Pełną dokumentację znajduje się [tutaj](container-service-kubernetes-walkthrough.md).

### <a name="step-1-create-a-resource-group"></a>Krok 1: Tworzenie grupy zasobów
```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus

az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="step-2-deploy-the-cluster"></a>Krok 2: Wdrażanie klastra
> [!NOTE]
> Poniższe kroki wymagają lokalnych klucz publiczny SSH, przechowywane w folderze ~/.ssh.
>

```azurecli
RESOURCE_GROUP=my-resource-group
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name

az acs create \
--orchestrator-type=kubernetes \
--resource-group $RESOURCE_GROUP \
--name=$CLUSTER_NAME \
--dns-prefix=$DNS_PREFIX \
--ssh-key-value ~/.ssh/id_rsa.pub \
--admin-username=azureuser \
--master-count=1 \
--agent-count=5 \
--agent-vm-size=Standard_D1_v2
```

## <a name="set-up-jenkins-and-configure-access-to-container-service"></a>Konfigurowanie Wpięć i konfigurowanie dostępu do usługi kontenera

### <a name="step-1-install-jenkins"></a>Krok 1: Instalowanie Wpięć
1. Tworzenie maszyny Wirtualnej platformy Azure z Ubuntu 16.04 LTS.  Ponieważ w dalszej części procedury należy połączyć z tej maszyny wirtualnej na komputerze lokalnym przy użyciu bash, ustaw "typ uwierzytelniania" do "Publicznego klucza SSH" i Wklej klucz publiczny SSH, który jest przechowywany lokalnie w folderze ~/.ssh.  Ponadto Zanotuj nazwa użytkownika który określisz, ponieważ ta nazwa użytkownika będzie potrzebny, aby wyświetlić pulpit nawigacyjny Wpięć i łączenia się z maszyną Wirtualną Wpięć w kolejnych krokach.
2. Zainstaluj Wpięć za pośrednictwem tych [instrukcje](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu). Bardziej szczegółowy samouczek jest przeznaczony na [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).
3. Aby wyświetlić pulpit nawigacyjny Wpięć na komputerze lokalnym, należy zaktualizować grupy zabezpieczeń sieci platformy Azure umożliwia portu 8080, dodając regułę ruchu przychodzącego, która umożliwia dostęp do portu 8080.  Alternatywnie mogą instalacji przekierowania portów, uruchamiając poniższe polecenie:`ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`
4. Połączenie z serwerem Wpięć, przechodząc do publicznego adresu IP za pomocą przeglądarki (http:// < your_jenkins_public_ip >: 8080) i Odblokuj pulpit nawigacyjny Wpięć po raz pierwszy hasło administratora początkowej.  Hasło administratora jest przechowywany w /var/lib/jenkins/secrets/initialAdminPassword na Maszynie wirtualnej Wpięć.  Najłatwiejszym sposobem uzyskania tego hasła jest SSH do maszyny Wirtualnej Wpięć: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Następnie uruchom: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
5. Zainstaluj Docker na maszynie Wpięć za pośrednictwem tych [instrukcje](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts). Dzięki temu Docker poleceń do wykonania w zadaniach Wpięć.
6. Skonfiguruj Docker uprawnienia umożliwiające Wpięć można uzyskać dostępu do punktu końcowego Docker.

    ```bash
    sudo chmod 777 /run/docker.sock
    ```
8. Zainstaluj `kubectl` CLI na Wpięć. Szczegółowe informacje są w [Instalowanie i konfigurowanie kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).  Zadania Wpięć użyje "kubectl" w celu zarządzania i wdrażania klastra Kubernetes.

    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

    chmod +x ./kubectl

    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

### <a name="step-2-set-up-access-to-the-kubernetes-cluster"></a>Krok 2: Konfigurowanie dostępu do klastra Kubernetes

> [!NOTE]
> Istnieje wiele sposobów wykonywanie zadań następujące kroki. Użyj podejście, które jest najłatwiejszym dla Ciebie.
>

1. Kopiuj `kubectl` plik konfiguracji maszyny Wpięć, dzięki czemu Wpięć zadania mają dostęp do klastra Kubernetes. W poniższych instrukcjach przyjęto, że używasz bash z innego komputera niż Wpięć maszyny Wirtualnej i lokalnej klucz publiczny SSH jest przechowywany w folderze ~/.ssh na komputerze.

```bash
export KUBE_MASTER=<your_cluster_master_fqdn>
export JENKINS_USER=<your_jenkins_user>
export JENKINS_SERVER=<your_jenkins_public_ip>
sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir -m 777 /home/$JENKINS_USER/.kube/ \
&& sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir /var/lib/jenkins/.kube/ \
&& sudo scp -3 -i ~/.ssh/id_rsa azureuser@$KUBE_MASTER:.kube/config $JENKINS_USER@$JENKINS_SERVER:~/.kube/config \
&& sudo ssh -i ~/.ssh/id_rsa $JENKINS_USER@$JENKINS_SERVER sudo cp /home/$JENKINS_USER/.kube/config /var/lib/jenkins/.kube/config \
```
        
2. Z Wpięć zweryfikować, że klaster Kubernetes jest dostępny.  W tym celu SSH do maszyny Wirtualnej Wpięć: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Następnie sprawdź Wpięć można pomyślnie nawiązać połączenie z klastrem: `kubectl cluster-info`.
    

## <a name="create-a-jenkins-workflow"></a>Tworzenie przepływów pracy Wpięć

### <a name="prerequisites"></a>Wymagania wstępne

- Konto usługi GitHub repozytorium kodu.
- Konto Centrum docker do przechowywania i aktualizowanie obrazów.
- Konteneryzowanych aplikacja, która może być ponownie skompilowany i zaktualizować. Można użyć tej przykładowej aplikacji kontenera napisana Golang: https://github.com/chzbrgr71/go-web 

> [!NOTE]
> Poniższe kroki należy wykonać na koncie usługi GitHub. Możesz także sklonować repozytorium powyżej, ale własnego konta muszą być używane do konfigurowania elementów webhook i Wpięć dostępu.
>

### <a name="step-1-deploy-initial-v1-of-application"></a>Krok 1: Wdrażanie początkowej v1 aplikacji
1. Tworzenie aplikacji na komputerze dewelopera za pomocą następujących poleceń. Zastąp `myrepo` własnymi.
    
    ```bash
    git clone https://github.com/chzbrgr71/go-web.git
    cd go-web
    docker build -t myrepo/go-web .
    ```

2. Wypchnij obrazu do Centrum Docker.

    ```bash
    docker login
    docker push myrepo/go-web
    ```

3. Wdrażanie klastra Kubernetes.
    
    > [!NOTE] 
    > Edytuj `go-web.yaml` plik, aby zaktualizować obraz kontenera i repozytorium.
    >
        
    ```bash
    kubectl create -f ./go-web.yaml --record
    ```
### <a name="step-2-configure-jenkins-system"></a>Krok 2: Konfigurowanie Wpięć systemu
1. Kliknij przycisk **Zarządzanie Wpięć** > **skonfigurować System**.
2. W obszarze **GitHub**, wybierz pozycję **Dodaj serwer GitHub**.
3. Pozostaw **adres URL interfejsu API** jako domyślny.
4. W obszarze **poświadczenia**, Dodaj poświadczenie Wpięć przy użyciu **tajny tekstu**. Firma Microsoft zaleca używanie GitHub osobiste tokeny dostępu, które są konfigurowane w ustawieniach konta użytkownika usługi GitHub. Więcej informacji na ten [tutaj.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
5. Kliknij przycisk **połączenie testowe** aby upewnić się, to jest poprawnie skonfigurowany.
6. W obszarze **globalnych właściwości**, dodać zmienną środowiskową `DOCKER_HUB` i podaj hasło do Centrum Docker. (Jest to przydatne w przypadku tego pokazu, ale scenariuszu produkcyjnym wymagają bardziej bezpieczną metodą).
7. Zapisz.

![GitHub Wpięć dostępu](./media/container-service-kubernetes-jenkins/jenkins-github-access.png)

### <a name="step-3-create-the-jenkins-workflow"></a>Krok 3: Tworzenie przepływu pracy Wpięć
1. Utwórz element Wpięć.
2. Podaj nazwę (na przykład "Przejdź sieci web") i wybierz **projektu dowolne**. 
3. Sprawdź **projektu GitHub** i podaj adres URL do Twojego repozytorium GitHub.
4. W **zarządzania kodem źródłowym**, podaj adres URL repozytorium GitHub i poświadczeń. 
5. Dodaj **krok kompilacji** typu **wykonywania powłoki** i użyj następującego tekstu:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME .
    docker login -u <your-dockerhub-username> -p ${DOCKER_HUB}
    docker push $WEB_IMAGE_NAME
    ```

6. Dodaj inny **krok kompilacji** typu **wykonywania powłoki** i użyj następującego tekstu:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    kubectl set image deployment/go-web go-web=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

![Kroki procesu kompilacji Wpięć](./media/container-service-kubernetes-jenkins/jenkins-build-steps.png)
    
7. Zapisz element Wpięć i testowania **kompilacji teraz**.

### <a name="step-4-connect-github-webhook"></a>Krok 4: Łączenie element webhook GitHub
1. W elemencie Wpięć został utworzony, kliknij przycisk **Konfiguruj**.
2. W obszarze **kompilacji wyzwalaczy**, wybierz pozycję **wyzwalacza haku GitHub dla sondowania GITScm** i **zapisać**. Spowoduje to automatyczne skonfigurowanie element webhook GitHub.
3. W Twojej repozytorium GitHub dla go w sieci web, kliknij przycisk **Ustawienia > elementów Webhook**.
4. Sprawdź, czy adres URL elementu webhook Wpięć został pomyślnie dodany. Adres URL powinien kończyć się "element webhook github".

![Konfiguracja elementu webhook Wpięć](./media/container-service-kubernetes-jenkins/jenkins-webhook.png)

## <a name="test-the-cicd-process-end-to-end"></a>Testowanie procesu CI/CD pełnego

1. Zaktualizuj kod do repozytorium i wypychania/synchronizacji z repozytorium GitHub.
2. Z poziomu konsoli Wpięć Sprawdź **kompilacji historii** i sprawdzić, czy zadanie zostało uruchomione. Wyświetl dane wyjściowe konsoli aby wyświetlić szczegóły.
3. Z Kubernetes wyświetlić szczegóły wdrożenia uaktualnionym:

    ```bash
    kubectl rollout history deployment/go-web
    ```

## <a name="next-steps"></a>Następne kroki

- Wdróż rejestru kontenera Azure i przechowywania w bezpiecznym repozytorium obrazów. Zobacz [docs rejestru kontenera Azure](https://docs.microsoft.com/azure/container-registry).
- Tworzyć bardziej złożone przepływu pracy, który obejmuje side-by-side wdrożenia oraz testów automatycznych w Wpięć.
- Aby uzyskać więcej informacji na temat CI/CD z Wpięć i Kubernetes, zobacz [blog Wpięć](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).
