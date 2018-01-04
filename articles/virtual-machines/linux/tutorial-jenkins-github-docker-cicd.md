---
title: "Tworzenie potoku programowanie na platformie Azure z Wpięć | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć Wpięć maszynę wirtualną na platformie Azure, która pobiera z usługi GitHub na każdym zatwierdzeniu kodu i tworzy nowy kontener Docker do uruchomienia aplikacji sieci"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 1426b7331b320397184805a6642fe6a57ca6ccb1
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="how-to-create-a-development-infrastructure-on-a-linux-vm-in-azure-with-jenkins-github-and-docker"></a>Tworzenie infrastruktury programowanie na maszynie Wirtualnej systemu Linux na platformie Azure z Wpięć, GitHub i Docker
Aby zautomatyzować fazy kompilacji i testowania projektowanie aplikacji, można użyć ciągłej integracji i wdrażania (CI/CD) potoku. W tym samouczku, możesz utworzyć potok CI/CD na maszynie Wirtualnej platformy Azure w tym jak:

> [!div class="checklist"]
> * Tworzenie maszyny Wirtualnej z Wpięć
> * Instalowanie i konfigurowanie Wpięć
> * Utwórz integrację elementu webhook GitHub i Wpięć
> * Tworzenie i zatwierdza wyzwalacza Wpięć Tworzenie zadania z usługi GitHub
> * Tworzenie obrazu Docker dla aplikacji
> * Sprawdź, czy zatwierdzenia GitHub Tworzenie nowego obrazu Docker i aktualizacje uruchamiania aplikacji


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym samouczku wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.22 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-jenkins-instance"></a>Utwórz wystąpienie Wpięć
W poprzednich samouczek dotyczący [sposobu dostosowywania maszyny wirtualnej systemu Linux, po pierwszym uruchomieniu komputera](tutorial-automate-vm-deployment.md), wiesz, jak można zautomatyzować dostosowania maszyny Wirtualnej z inicjowaniem chmury. W tym samouczku używany jest plik init chmury do zainstalowania Wpięć i Docker na maszynie Wirtualnej. Wpięć jest serwer automatyzacji popularnych typu open source, które bezproblemowo integruje się z platformy Azure, aby włączyć ciągłej integracji (CI) i ciągłego dostarczania (CD). Aby uzyskać więcej samouczków na temat sposobu korzystania z Wpięć, zobacz [Wpięć w Centrum Azure](https://docs.microsoft.com/azure/jenkins/).

W bieżącym powłoki, Utwórz plik o nazwie *chmurze init-jenkins.txt* i wklej następującą konfigurację. Na przykład utworzyć plik, w powłoce chmury nie na komputerze lokalnym. Wprowadź `sensible-editor cloud-init-jenkins.txt` do tworzenia pliku i wyświetlić listę dostępnych edytory. Upewnij się, że poprawnie skopiować pliku całego init chmury szczególnie pierwszy wiersz:

```yaml
#cloud-config
package_upgrade: true
write_files:
  - path: /etc/systemd/system/docker.service.d/docker.conf
    content: |
      [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd
  - path: /etc/docker/daemon.json
    content: |
      {
        "hosts": ["fd://","tcp://127.0.0.1:2375"]
      }
runcmd:
  - wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add -
  - sh -c 'echo deb http://pkg.jenkins-ci.org/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
  - apt-get update && apt-get install jenkins -y
  - curl -sSL https://get.docker.com/ | sh
  - usermod -aG docker azureuser
  - usermod -aG docker jenkins
  - service jenkins restart
```

Przed utworzeniem maszyny Wirtualnej, Utwórz nową grupę zasobów o [Tworzenie grupy az](/cli/azure/group#create). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroupJenkins* w *eastus* lokalizacji:

```azurecli-interactive 
az group create --name myResourceGroupJenkins --location eastus
```

Teraz Utwórz maszynę Wirtualną z [tworzenia maszyny wirtualnej az](/cli/azure/vm#create). Użyj `--custom-data` parametr do przekazania w pliku config init chmury. Podaj pełną ścieżkę do *chmurze init-jenkins.txt* po zapisaniu pliku poza istnieje katalog roboczy.

```azurecli-interactive 
az vm create --resource-group myResourceGroupJenkins \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
```

Trwa kilka minut dla maszyny Wirtualnej można tworzyć i konfigurować.

Aby zezwolić na ruch sieci web do maszyny Wirtualnej, użyj [port Otwórz az maszyny wirtualnej](/cli/azure/vm#open-port) otwierania portu *8080* Wpięć ruchu i portu *1337* aplikacji Node.js, który służy do uruchamiania przykładową aplikację:

```azurecli-interactive 
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 8080 --priority 1001
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 1337 --priority 1002
```


## <a name="configure-jenkins"></a>Skonfiguruj Wpięć
Aby uzyskać dostęp do Twojego wystąpienia Wpięć, uzyskać publicznego adresu IP maszyny Wirtualnej:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Ze względów bezpieczeństwa musisz wprowadzić hasło administratora początkowej, która jest przechowywana w pliku tekstowego na maszynie Wirtualnej, aby rozpocząć instalację Wpięć. Użyj publicznego adresu IP uzyskanych w poprzednim kroku, aby SSH do maszyny Wirtualnej:

```bash
ssh azureuser@<publicIps>
```

Widok `initialAdminPassword` Twojego Wpięć zainstalować i skopiuj go:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Jeśli plik nie jest jeszcze dostępna, poczekaj jeszcze kilka minut init chmury do ukończenia instalacji Wpięć i Docker.

Teraz Otwórz przeglądarkę sieci web i przejdź do `http://<publicIps>:8080`. Ukończenie początkowej konfiguracji Wpięć w następujący sposób:

- Wprowadź nazwę użytkownika **admin**, następnie podaj *initialAdminPassword* uzyskany z maszyny Wirtualnej w poprzednim kroku.
- Wybierz **Zarządzanie Wpięć**, następnie **Zarządzanie wtyczkami**.
- Wybierz **dostępne**, następnie wyszukaj *GitHub* w polu tekstowym u góry. Pole wyboru dla *wtyczki GitHub*, a następnie wybierz pozycję **teraz pobrać i zainstalować po ponownym uruchomieniu**.
- Pole wyboru dla **Wpięć ponownego uruchomienia po zakończeniu instalacji i są uruchomione żadne zadania**, a następnie zaczekaj, aż procesu instalacji wtyczka została zakończona.


## <a name="create-github-webhook"></a>Utwórz element webhook GitHub
Aby skonfigurować integrację z usługi GitHub, otwórz [Node.js Hello World Przykładowa aplikacja](https://github.com/Azure-Samples/nodejs-docs-hello-world) z repozytorium przykładów dla platformy Azure. Do rozwidlania repozytorium na koncie usługi GitHub, wybierz **rozwidlenia** przycisk w prawym górnym rogu.

Tworzenie elementu webhook wewnątrz rozwidlenia, utworzone:

- Wybierz **ustawienia**, a następnie wybierz pozycję **integracji i usługi** po lewej stronie.
- Wybierz **dodawania usługi**, wprowadź *Wpięć* w polu filtru.
- Wybierz *Wpięć (GitHub wtyczki)*
- Aby uzyskać **Wpięć utworzenie punktu zaczepienia adresu URL**, wprowadź `http://<publicIps>:8080/github-webhook/`. Upewnij się, że możesz wpisać kreskę końcową /
- Wybierz **dodawania usługi**

![Dodaj element webhook GitHub do Twojego repozytorium rozwidlonych](media/tutorial-jenkins-github-docker-cicd/github_webhook.png)


## <a name="create-jenkins-job"></a>Utwórz zadanie Wpięć
Aby Wpięć odpowiadanie na zdarzenia w usłudze GitHub takich jak zatwierdzania kodu, Utwórz zadanie Wpięć. 

W witrynie sieci Web Wpięć wybierz **tworzenie nowych zadań** na stronie głównej:

- Wprowadź *HelloWorld* jako nazwa zadania. Wybierz **stylu projektu**, a następnie wybierz pozycję **OK**.
- W obszarze **ogólne** zaznacz **GitHub** projektu i wprowadź adres URL repozytorium rozwidlonych, takich jak *https://github.com/iainfoulds/nodejs-docs-hello-world*
- W obszarze **źródła zarządzania kodem** zaznacz **Git**, wprowadź Twojego repozytorium rozwidlonych *.git* adres URL, takie jak *https://github.com/iainfoulds/nodejs-docs-hello-world.git*
- W obszarze **kompilacji wyzwalaczy** zaznacz **wyzwalacza haku GitHub dla sondowania GITscm**.
- W obszarze **kompilacji** wybierz **kroku kompilacji Dodaj**. Wybierz **wykonywania powłoki**, wprowadź `echo "Testing"` w oknie wiersza polecenia.
- Wybierz **zapisać** w dolnej części okna zadań.


## <a name="test-github-integration"></a>Testowanie integracji usługi GitHub
Aby przetestować integracji GitHub z Wpięć, Zatwierdź zmiany w rozwidlenia. 

W witrynie GitHub interfejs użytkownika sieci web, wybierz użytkownika rozwidlonych repozytorium, a następnie wybierz **index.js** pliku. Wybierz ikonę ołówka, aby edytować ten plik, więc odczytuje wiersz 6:

```nodejs
response.end("Hello World!");
```

Aby zatwierdzić zmiany, wybierz **Zatwierdź zmiany** znajdujący się u dołu.

W Wpięć, nowej kompilacji zaczyna się w obszarze **kompilacji historii** sekcji lewym dolnym rogu strony zadania. Wybierz łącze numer kompilacji **dane wyjściowe konsoli** po lewej stronie. Można wyświetlić kroki Wpięć przyjmuje jako kodu są pobierane z usługi GitHub i Akcja kompilacji generuje komunikat `Testing` do konsoli. Zawsze, gdy zatwierdzenie jest przeprowadzane w witrynie GitHub, elementu webhook osiągnie do Wpięć i wyzwala nową kompilację w ten sposób.


## <a name="define-docker-build-image"></a>Zdefiniuj obraz kompilacji Docker
Aby wyświetlić aplikację Node.js oparte na systemie zatwierdzenia GitHub, umożliwia tworzenie obrazów Docker, aby uruchomić aplikację. Obraz jest tworzony z plik Dockerfile, który definiuje sposób konfigurowania kontenera, który uruchamia aplikację. 

Połączenie SSH maszyny Wirtualnej przejdź do katalogu roboczego Wpięć o nazwie po zadania, który został utworzony w poprzednim kroku. W tym przykładzie, który miał nazwę *HelloWorld*.

```bash
cd /var/lib/jenkins/workspace/HelloWorld
```

Utwórz plik w bieżącym katalogu roboczym z `sudo sensible-editor Dockerfile` i Wklej poniższą zawartość. Upewnij się, że cały plik Dockerfile zostały skopiowane poprawnie, szczególnie pierwszy wiersz:

```yaml
FROM node:alpine

EXPOSE 1337

WORKDIR /var/www
COPY package.json /var/www/
RUN npm install
COPY index.js /var/www/
```

Ten plik Dockerfile używa obrazu podstawowego środowiska Node.js przy użyciu Alpine Linux, port ujawnia 1337 działa aplikacja Hello World na, następnie kopiuje pliki aplikacji i inicjuje go.


## <a name="create-jenkins-build-rules"></a>Tworzenie reguł kompilacji Wpięć
W poprzednim kroku możesz utworzyć podstawowe reguły kompilacji Wpięć, że dane wyjściowe komunikat do konsoli. Umożliwia tworzenie kroku kompilacji, aby użyć naszych plik Dockerfile i uruchom aplikację.

Ponownie w wystąpieniu Wpięć wybierz zadanie utworzony w poprzednim kroku. Wybierz **Konfiguruj** po lewej stronie i przewiń w dół do **kompilacji** sekcji:

- Usuń istniejącą `echo "Test"` kroku kompilacji. Wybierz między czerwono w prawym górnym rogu istniejącego pola kroku kompilacji.
- Wybierz **kroku kompilacji Dodaj**, a następnie wybierz pozycję **wykonania powłoki**
- W **polecenia** polu, wprowadź następujące polecenia Docker, a następnie wybierz **zapisać**:

  ```bash
  docker build --tag helloworld:$BUILD_NUMBER .
  docker stop helloworld && docker rm helloworld
  docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
  ```

Kroki procesu kompilacji Docker Utwórz obraz i tagu go z Wpięć numer kompilacji, dzięki czemu można utrzymać historii obrazów. Kontenerach istniejącą aplikację zatrzymana, a następnie usuwane. Nowy kontener jest następnie uruchamiany przy użyciu obrazu i uruchamia aplikację Node.js oparte na najnowszych zatwierdzenia w witrynie GitHub.


## <a name="test-your-pipeline"></a>Testowanie potoku sieci
Aby wyświetlić całą potoku w akcji, należy edytować *index.js* plików w sieci rozwidlonych repozytorium GitHub ponownie, a następnie wybierz **zatwierdzić zmiany**. Nowe zadanie uruchamia w Wpięć oparta na elementu webhook GitHub. Trwa kilka sekund, aby utworzyć obraz Docker i uruchomić aplikację w nowym kontenerze.

W razie potrzeby ponownie uzyskać publicznego adresu IP maszyny Wirtualnej:

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

Otwórz przeglądarkę sieci web, a następnie wprowadź `http://<publicIps>:1337`. Aplikacji Node.js jest wyświetlany i odzwierciedla najnowszej zatwierdzenia w rozwidlenia GitHub w następujący sposób:

![Uruchomionej aplikacji Node.js](media/tutorial-jenkins-github-docker-cicd/running_nodejs_app.png)

Teraz należy edytować innej *index.js* pliku w usłudze GitHub i zatwierdzić zmiany. Poczekaj kilka sekund dla zadania do wykonania w Wpięć, a następnie odśwież przeglądarkę sieci web, aby wyświetlić zaktualizowaną wersję aplikacji uruchomionej w nowym kontenerze w następujący sposób:

![Uruchomienie aplikacji Node.js po innym zatwierdzenia GitHub](media/tutorial-jenkins-github-docker-cicd/another_running_nodejs_app.png)


## <a name="next-steps"></a>Kolejne kroki
W tym samouczku należy skonfigurować GitHub do uruchomienia zadania kompilacji Wpięć na każdym zatwierdzeniu kodu, a następnie wdrożyć kontener Docker do testowania aplikacji. W tym samouczku omówiono:

> [!div class="checklist"]
> * Tworzenie maszyny Wirtualnej z Wpięć
> * Instalowanie i konfigurowanie Wpięć
> * Utwórz integrację elementu webhook GitHub i Wpięć
> * Tworzenie i zatwierdza wyzwalacza Wpięć Tworzenie zadania z usługi GitHub
> * Tworzenie obrazu Docker dla aplikacji
> * Sprawdź, czy zatwierdzenia GitHub Tworzenie nowego obrazu Docker i aktualizacje uruchamiania aplikacji

Przejdź do następnego samouczkiem, aby dowiedzieć się więcej na temat sposobu integracji Wpięć z Visual Studio Team Services.

> [!div class="nextstepaction"]
> [Wdrażanie aplikacji przy użyciu Wpięć i usługi Team Services](tutorial-build-deploy-jenkins.md)
