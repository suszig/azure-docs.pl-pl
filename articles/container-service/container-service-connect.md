---
title: "Łączenie z klastrem usługi Azure Container Service | Microsoft Docs"
description: "Nawiązywanie połączenia z klastrem Kubernetes, DC/OS, lub Docker Swarm w usłudze Azure Container Service z komputera zdalnego"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, kontenery, mikrousługi, Kubernetes, DC/OS, Azure"
ms.assetid: ff8d9e32-20d2-4658-829f-590dec89603d
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/21/2017
ms.author: rogardle
translationtype: Human Translation
ms.sourcegitcommit: 2a381431acb6436ddd8e13c69b05423a33cd4fa6
ms.openlocfilehash: 45d399b72f8d037fb828d9ad22bbd3543847feb3
ms.lasthandoff: 02/22/2017


---
# <a name="connect-to-an-azure-container-service-cluster"></a>Łączenie z klastrem usługi kontenera platformy Azure
Po utworzeniu klastra usługi Azure Container Service należy połączyć się z klastrem, aby wdrożyć obciążenia i zarządzać nimi. W tym artykule opisano sposób nawiązywania połączenia z główną maszyną wirtualną klastra z komputera zdalnego. 

Klastry Kubernetes, DC/OS i Docker Swarm udostępniają punkty końcowe HTTP lokalnie. W przypadku klastrów Kubernetes ten punkt końcowy jest bezpiecznie uwidaczniany w Internecie i można do niego uzyskać dostęp, uruchamiając narzędzie wiersza polecenia `kubectl` z dowolnej maszyny podłączonej do Internetu. 

W przypadku klastrów DC/OS i Docker Swarm musisz utworzyć tunel Secure Shell (SSH) do systemu wewnętrznego. Po ustanowieniu tunelu możesz uruchamiać polecenia korzystające z punktów końcowych HTTP i wyświetlać interfejs sieci Web klastra z systemu lokalnego. 


## <a name="prerequisites"></a>Wymagania wstępne

* Klaster Kubernetes, DC/OS lub Swarm [wdrożony w usłudze Azure Container Service](container-service-deployment.md).
* Plik klucza prywatnego SSH RSA odpowiadający kluczowi publicznemu dodanemu do klastra podczas wdrażania. Te polecenia zakładają, że prywatny klucz SSH znajduje się w folderu `$HOME/.ssh/id_rsa` na komputerze. Zobacz te instrukcje dla systemów [OS X i Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md) lub [Windows](../virtual-machines/virtual-machines-linux-ssh-from-windows.md), aby uzyskać więcej informacji. Jeśli połączenie SSH nie działa, konieczne może być [zresetowanie kluczy SSH](../virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md).

## <a name="connect-to-a-kubernetes-cluster"></a>Nawiązywanie połączenia z klastrem Kubernetes

Wykonaj następujące kroki, aby zainstalować i skonfigurować narzędzie `kubectl` na komputerze.

> [!NOTE] 
> W systemie Linux lub OS X może być konieczne uruchomienie poleceń w tej sekcji przy użyciu `sudo`.
> 

### <a name="install-kubectl"></a>Instalowanie narzędzia kubectl
Jednym ze sposobów instalacji tego narzędzia jest użycie polecenia `az acs kubernetes install-cli` interfejsu wiersza polecenia platformy Azure 2.0. Aby uruchomić to polecenie, upewnij się, że [zainstalowano](/cli/azure/install-az-cli2) najnowszy interfejs wiersza polecenia platformy Azure 2.0 i zalogowano się na koncie platformy Azure (`az login`).

```azurecli
# Linux or OS X
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

Możesz też pobrać najnowszego klienta bezpośrednio ze [strony z wersjami usługi Kubernetes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md). Aby uzyskać więcej informacji, zobacz [Installing and Setting up kubectl](https://kubernetes.io/docs/user-guide/prereqs/) (Instalowanie i konfigurowanie narzędzia kubectl).

### <a name="download-cluster-credentials"></a>Pobieranie poświadczeń klastra
Po zainstalowaniu narzędzia `kubectl` musisz skopiować poświadczenia klastra do swojej maszyny. Jednym ze sposobów uzyskania poświadczeń jest użycie polecenia `az acs kubernetes get-credentials`. Przekaż nazwę grupy zasobów i nazwę zasobu usługi Container Service:


```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

To polecenie pobiera poświadczenia klastra do folderu `$HOME/.kube/config` zgodnie z oczekiwaniami narzędzia `kubectl`.

Możesz też użyć narzędzia `scp`, aby bezpiecznie skopiować plik z folderu `$HOME/.kube/config` na głównej maszynie wirtualnej do maszyny lokalnej. Na przykład:

```console
mkdir $HOME/.kube
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

Jeśli pracujesz w systemie Windows, musisz użyć narzędzia Bash on Ubuntu on Windows, klienta bezpiecznego kopiowania plików programu PuTTy lub podobnego narzędzia.



### <a name="use-kubectl"></a>Używanie narzędzia kubectl

Po skonfigurowaniu narzędzia `kubectl` możesz przetestować połączenie, wyświetlając listę węzłów w klastrze:

```console
kubectl get nodes
```

Możesz wypróbować inne polecenia narzędzia `kubectl`. Możesz na przykład wyświetlić pulpit nawigacyjny platformy Kubernetes. Najpierw uruchom serwer proxy do serwera interfejsu API Kubernetes:

```console
kubectl proxy
```

Interfejs użytkownika platformy Kubernetes jest obecnie dostępny pod adresem: `http://localhost:8001/ui`.

Aby uzyskać więcej informacji, zobacz [Kubernetes — szybki start](http://kubernetes.io/docs/user-guide/quick-start/).

## <a name="connect-to-a-dcos-or-swarm-cluster"></a>Nawiązywanie połączenia z klastrem DC/OS lub Swarm

Aby korzystać z klastrów DC/OS i Docker Swarm wdrożonych przez usługę Azure Container Service, postępuj zgodnie z tymi instrukcjami w celu utworzenia tunelu Secure Shell (SSH) z lokalnego systemu Linux, OS X lub Windows. 

> [!NOTE]
> Te instrukcje koncentrują się na tunelowaniu ruchu TCP przez SSH. Możesz też uruchomić interaktywną sesję SSH z jednym z wewnętrznych systemów zarządzania klastrami, ale nie zalecamy tego. Praca bezpośrednio na systemie wewnętrznym niesie za sobą ryzyko wprowadzenia niezamierzonych zmian w konfiguracji.  
> 

### <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Tworzenie tunelu SSH w systemie Linux lub OS X
Pierwszym krokiem tworzenia tunelu SSH w systemie Linux lub OS X jest zlokalizowanie publicznej nazwy DNS serwerów głównych ze zrównoważonym obciążeniem. Wykonaj następujące kroki:


1. W witrynie [Azure Portal](https://portal.azure.com) przejdź do grupy zasobów zawierającej klaster usługi Containter Service. Rozwiń grupę zasobów, aby wyświetlić poszczególne zasoby. 

2. Kliknij zasób usługi kontenera, a następnie kliknij pozycję **Przegląd**. W obszarze **Podstawy** zostanie wyświetlona **główna nazwa FQDN** klastra. Zapisz tę nazwę do późniejszego użycia. 

    ![Publiczna nazwa DNS](media/pubdns.png)

    Możesz też uruchomić polecenie `az acs show` względem usługi kontenera. Następnie poszukaj właściwości **Master Profile:fqdn** w danych wyjściowych polecenia.

3. Otwórz powłokę i uruchom polecenie `ssh`, określając następujące wartości: 

    **LOCAL_PORT** to port TCP tunelu po stronie usługi, z którym należy się połączyć. W przypadku klastra Swarm ustaw tę wartość na 2375. W przypadku klastra DC/OS ustaw tę wartość na 80.  
    **REMOTE_PORT** to port punktu końcowego, który ma zostać udostępniony. W przypadku klastra Swarm jest to port 2375. W przypadku klastra DC/OS jest to port 80.  
    **USERNAME** (nazwa_użytkownika) to nazwa użytkownika podana podczas wdrażania klastra.  
    **DNSPREFIX** (prefiks_DNS) to prefiks DNS podany podczas wdrażania klastra.  
    **REGION** to region, w którym znajduje się grupa zasobów.  
    **PATH_TO_PRIVATE_KEY** [OPCJONALNIE] to ścieżka do klucza prywatnego odpowiadającego kluczowi publicznemu podanemu podczas tworzenia klastra. Użyj tej opcji z flagą `-i`.

    ```bash
    ssh -fNL LOCAL_PORT:localhost:REMOTE_PORT -p 2200 [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com 
    ```
    > [!NOTE]
    > Port połączenia SSH to 2200, a nie standardowy port 22. W klastrze z więcej niż jedną główną maszyną wirtualną jest to port połączenia z pierwszą główną maszyną wirtualną.
    > 



Zobacz przykłady dla klastrów DC/OS i Swarm w poniższych sekcjach.    

### <a name="dcos-tunnel"></a>Tunel DC/OS
Aby otworzyć tunel dla punktów końcowych DC/OS, uruchom polecenie podobne do następującego:

```bash
sudo ssh -fNL 80:localhost:80 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com 
```

> [!NOTE]
> Możesz określić port lokalny inny niż 80, na przykład 8888. Jednak gdy ten port jest używany, niektóre linki w interfejsie użytkownika sieci Web mogą nie działać.

Możesz teraz uzyskiwać dostęp do punktów końcowych DC/OS z systemu lokalnego, używając następujących adresów URL (zakładając, że port lokalny to 80):

* DC/OS: `http://localhost:80/`
* Marathon: `http://localhost:80/marathon`
* Mesos: `http://localhost:80/mesos`

Podobnie za pomocą tego tunelu możesz uzyskiwać dostęp do interfejsów API REST dla poszczególnych aplikacji.

### <a name="swarm-tunnel"></a>Tunel Swarm
Aby otworzyć tunel do punktu końcowego Swarm, uruchom polecenie podobne do następującego:

```bash
ssh -fNL 2375:localhost:2375 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com
```

Teraz możesz ustawić zmienną środowiskową DOCKER_HOST w następujący sposób. Możesz kontynuować korzystanie z interfejsu wiersza polecenia (CLI) Docker w normalny sposób.

```bash
export DOCKER_HOST=:2375
```

### <a name="create-an-ssh-tunnel-on-windows"></a>Tworzenie tunelu SSH w systemie Windows
Istnieje wiele opcji tworzenia tuneli SSH w systemie Windows. W tej sekcji opisano sposób tworzenia tunelu przy użyciu programu PuTTY.

1. [Pobierz program PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) do systemu Windows.

2. Uruchom aplikację.

3. Wprowadź nazwę hosta złożoną z nazwy użytkownika administratora klastra i publicznej nazwy DNS pierwszego serwera głównego w klastrze. **Nazwa hosta** wygląda podobnie do `adminuser@PublicDNSName`. W polu **Port** wprowadź wartość 2200.

    ![Konfiguracja programu PuTTY 1](media/putty1.png)

4. Wybierz pozycje **SSH > Uwierzytelnianie**. Dodaj ścieżkę do pliku klucza prywatnego (format ppk) na potrzeby uwierzytelniania. Możesz użyć narzędzia takiego jak [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html), aby wygenerować ten plik z klucza SSH użytego do utworzenia klastra.

    ![Konfiguracja programu PuTTY 2](media/putty2.png)

5. Wybierz pozycje **SSH > Tunele** i skonfiguruj następujące przekazane porty:

    * **Port źródłowy:** — użyj portu 80 w przypadku opcji DC/OS lub portu 2375 w przypadku opcji Swarm.
    * **Miejsce docelowe:** — użyj wartości localhost:80 w przypadku opcji DC/OS lub wartości localhost:2375 w przypadku opcji Swarm.

    Poniższy przykład został skonfigurowany na potrzeby opcji DC/OS, ale będzie wyglądać podobnie w przypadku rozwiązania Docker Swarm.

    > [!NOTE]
    > W przypadku tworzenia tego tunelu nie można używać portu 80.
    > 

    ![Konfiguracja programu PuTTY 3](media/putty3.png)

6. Po zakończeniu kliknij pozycje **Sesja > Zapisz**, aby zapisać konfigurację połączenia.

7. Aby nawiązać połączenie z sesją programu PuTTY, kliknij pozycję **Otwórz**. Po nawiązaniu połączenia konfiguracja portów będzie dostępna w dzienniku zdarzeń programu PuTTY.

    ![Dziennik zdarzeń programu PuTTY](media/putty4.png)

Po skonfigurowaniu tunelu dla koordynatora DC/OS można uzyskiwać dostęp do powiązanych punktów końcowych w następujących lokalizacjach:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Po skonfigurowaniu tunelu dla koordynatora Docker Swarm otwórz ustawienia systemu Windows, aby skonfigurować systemową zmienną środowiskową o nazwie `DOCKER_HOST` przy użyciu wartości `:2375`. Następnie możesz uzyskać dostęp do klastra Swarm za pośrednictwem interfejsu wiersza polecenia platformy Docker.

## <a name="next-steps"></a>Następne kroki
Wdrażanie kontenerów i zarządzanie nimi w klastrze:

* [Współpraca z usługą Azure Container Service i rozwiązaniem Kubernetes](container-service-kubernetes-ui.md)
* [Współpraca z usługą Azure Container Service i rozwiązaniem DC/OS](container-service-mesos-marathon-rest.md)
* [Współpraca z usługą Azure Container Service i rozwiązaniem Docker Swarm](container-service-docker-swarm.md)


