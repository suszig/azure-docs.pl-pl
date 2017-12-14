---
title: "Użyć niestandardowego obrazu Docker dla aplikacji sieci Web dla kontenerów - Azure | Dokumentacja firmy Microsoft"
description: "Jak używać niestandardowego obrazu Docker dla aplikacji sieci Web dla kontenerów."
keywords: "Usługa aplikacji Azure, aplikacji sieci web, linux, docker, kontenera"
services: app-service
documentationcenter: 
author: SyntaxC4
manager: SyntaxC4
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 6a89db8b93f29c29e935afd94da727d2460af889
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="use-a-custom-docker-image-for-web-app-for-containers"></a>Użyć niestandardowego obrazu Docker dla aplikacji sieci Web dla kontenerów

[Sieci Web aplikacji dla kontenerów](app-service-linux-intro.md) zawiera wbudowane obrazy usługi Docker w systemie Linux z obsługą dla określonej wersji, takich jak PHP w wersji 7.0 i Node.js 4.5. Aplikacji dla kontenerów sieci Web używa technologii kontenera Docker do obsługi zarówno obrazy wbudowanych i niestandardowych obrazów jako platforma jako usługa. W tym samouczku można dowiedzieć się, jak utworzyć niestandardowy obraz Docker i wdrożyć ją do aplikacji sieci Web dla kontenerów. Ten wzorzec przydaje się, gdy wbudowanych obrazów nie zawierają język wybór lub gdy aplikacja wymaga określonej konfiguracji, który nie został podany w obrazach wbudowanych.

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka niezbędne są następujące elementy:

* [Git](https://git-scm.com/downloads)
* Aktywny [subskrypcji platformy Azure](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/get-started/#setup)
* A [konta Centrum Docker](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Pobierz przykład

W oknie terminalu uruchom następujące polecenie do klonowania repozytorium przykładowej aplikacji na komputerze lokalnym, a następnie przejdź do katalogu, który zawiera przykładowy kod.

```bash
git clone https://github.com/Azure-Samples/docker-django-webapp-linux.git --config core.autocrlf=input
cd docker-django-webapp-linux
```

## <a name="build-the-image-from-the-docker-file"></a>Utwórz obraz z pliku Docker

W repozytorium Git, Przyjrzyjmy się _plik Dockerfile_. Ten plik zawiera opis środowiska Python, który jest wymagany do uruchamiania aplikacji. Ponadto ustawia obraz [SSH](https://www.ssh.com/ssh/protocol/) server dla zapewnienia bezpiecznej komunikacji między kontenera i hostem.

```docker
FROM python:3.4

RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/

# ssh
ENV SSH_PASSWD "root:Docker!"
RUN apt-get update \
        && apt-get install -y --no-install-recommends dialog \
        && apt-get update \
    && apt-get install -y --no-install-recommends openssh-server \
    && echo "$SSH_PASSWD" | chpasswd 

COPY sshd_config /etc/ssh/
COPY init.sh /usr/local/bin/
    
RUN chmod u+x /usr/local/bin/init.sh
EXPOSE 8000 2222
#CMD ["python", "/code/manage.py", "runserver", "0.0.0.0:8000"]
ENTRYPOINT ["init.sh"]
```

Aby utworzyć obraz Docker, uruchom `docker build` polecenia i podaj nazwę, `mydockerimage`i tagu `v1.0.0`. Zastąp `<docker-id>` w Centrum Docker konta identyfikatora.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Polecenie generuje dane wyjściowe podobne do następujących:

```
# The output from the commands in this article has been shortened for brevity.

Sending build context to Docker daemon  5.558MB
Step 1/13 : FROM python:3.4
 ---> 9ff45ddb54e9
Step 2/13 : RUN mkdir /code
 ---> Using cache
 ---> f3f3ac01db0a
Step 3/13 : WORKDIR /code
 ---> Using cache
 ---> 38b32f15b442
.
.
.
Step 13/13 : ENTRYPOINT init.sh
 ---> Running in 5904e4c70043
 ---> e7cf08275692
Removing intermediate container 5904e4c70043
Successfully built e7cf08275692
Successfully tagged cephalin/mydockerimage:v1.0.0
```

Przetestuj kompilacji działa, uruchamiając kontenera Docker. Problem [docker Uruchom](https://docs.docker.com/engine/reference/commandline/run/) poleceń i przekazywania do niej nazwy i tag obrazu. Należy określić przy użyciu portu `-p` argumentu.

```bash
docker run -p 2222:8000 <docker-ID>/mydockerimage:v1.0.0
```

Sprawdź aplikację sieci web i kontener działają poprawnie przechodząc do `http://localhost:2222`.

![Przetestuj aplikację sieci web lokalnie](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

## <a name="push-the-docker-image-to-docker-hub"></a>Wypychanie obrazu Docker do Centrum Docker

Rejestr jest aplikacja, która przechowuje obrazów i udostępnia obrazu i kontener usług. W celu udostępnienia obrazu, należy wypchnąć go do rejestru. 

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry such as Azure Container Registry. Select the appropriate tab for your scenario below (your selection will switch multiple tabs on this page). -->

> [!NOTE]
> Wypychanie do rejestru Docker prywatnej? Zapoznaj się z instrukcjami opcjonalne do [Push obrazu Docker w rejestrze prywatnej](#push-a-docker-image-to-private-registry-optional).

<!--## [Docker Hub](#tab/docker-hub)-->

Centrum docker to rejestru Docker obrazów, który umożliwia hostowanie własne repozytoriach publicznych lub prywatnych. Aby wypchnąć niestandardowego obrazu Docker do Centrum Docker publiczny, należy użyć [wypychania docker](https://docs.docker.com/engine/reference/commandline/push/) poleceń i podaj nazwę pełnego obrazu i tagów. Nazwa pełnego obrazu i tagu wygląda w poniższym przykładzie:

```
<docker-id>/image-name:tag
```

Jeśli użytkownik nie zalogował się do Centrum Docker, to zrobić przy użyciu [logowania docker](https://docs.docker.com/engine/reference/commandline/login/) polecenia przed podjęciem próby push obrazu.

```bash
docker login --username <docker-id> --password <docker-hub-password>
```

Komunikat "logowanie zakończyło się pomyślnie." potwierdza, że użytkownik jest zalogowany. Po zalogowaniu, możesz wypchnąć obrazu przy użyciu Centrum Docker [wypychania docker](https://docs.docker.com/engine/reference/commandline/push/) polecenia.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Upewnij się, że wypychania zakończyło się pomyślnie, sprawdzając polecenia danych wyjściowych.

```
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
c33197c3f6d4: Pushed
ccd2c850ee43: Pushed
02dff2853466: Pushed
6ce78153632a: Pushed
efef3f03cc58: Pushed
3439624d77fb: Pushed
3a07adfb35c5: Pushed
2fcec228e1b7: Mounted from library/python
97d2d3bae505: Mounted from library/python
95aadeabf504: Mounted from library/python
b456afdc9996: Mounted from library/python
d752a0310ee4: Mounted from library/python
db64edce4b5b: Mounted from library/python
d5d60fc34309: Mounted from library/python
c01c63c6823d: Mounted from library/python
v1.0.0: digest: sha256:21f2798b20555f4143f2ca0591a43b4f6c8138406041f2d32ec908974feced66 size: 3676
```

<!--
# [Private Registry](#tab/private-registry)

// Move Private Registry instructions here when Tabbed Conceptual bug is fixed

---
-->

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-app-to-azure"></a>Wdrażanie aplikacji na platformie Azure

Natywnych aplikacji systemu Linux w chmurze można obsługiwać przy użyciu aplikacji sieci Web Azure. Aby utworzyć aplikację sieci Web dla kontenerów, należy uruchomić polecenia interfejsu wiersza polecenia Azure, które należy utworzyć grupę, a następnie planu usługi i na koniec samej aplikacji sieci web. 

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-linux-app-service-plan"></a>Tworzenie planu usługi aplikacji systemu Linux

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)] 

### <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web

Za pomocą polecenia [az webapp create](/cli/azure/webapp#create) można utworzyć [aplikację internetową](app-service-linux-intro.md) w planie usługi App Service `myAppServicePlan` w usłudze Cloud Shell. Nie zapomnij Zastąp `<app_name>` z unikatowej nazwy aplikacji, a < docker-ID > przy użyciu Twojego identyfikatora Docker.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --deployment-container-image-name <docker-ID>/mydockerimage:v1.0.0
```

Po utworzeniu aplikacji sieci web, Azure CLI przedstawia dane wyjściowe podobne do poniższego przykładu:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="configure-environment-variables"></a>Skonfiguruj zmienne środowiskowe

Większość obrazy usługi Docker mieć zmiennych środowiskowych, które trzeba skonfigurować. Jeśli używasz istniejącego obrazu platformy Docker utworzony przez kogoś innego obrazu może używać portu innego niż 80. Opisz Azure na port, który używa obrazu przy użyciu `WEBSITES_PORT` ustawienia aplikacji. Na stronie GitHub [próbki Python w tym samouczku](https://github.com/Azure-Samples/docker-django-webapp-linux) pokazuje, które należy ustawić `WEBSITES_PORT` do _8000_.

Aby określić ustawienia aplikacji, należy użyć [zaktualizować appsettings konfiguracji aplikacji sieci Web az](/cli/azure/webapp/config/appsettings#update) polecenie w powłoce chmury. Ustawienia aplikacji są uwzględniania wielkości liter i rozdzielonej spacjami.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITES_PORT=8000
```

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry, such as Azure Container Registry. Select the appropriate tab for your scenario below: -->

> [!NOTE]
> Wdrażanie z rejestru Docker prywatnej? Zapoznaj się z instrukcjami opcjonalne do [Konfigurowanie aplikacji sieci Web do użycia z rejestru prywatnej kontenera Docker](#configure-web-app-to-use-docker-container-from-a-private-registry-optional).

<!-- # [Docker Hub](#tab/docker-hub)-->

<!-- # [Private Registry](#tab/private-registry)

// Place Private Registry text back here once Tabbed Conceptual bug is fixed

---
-->

### <a name="test-the-web-app"></a>Testowanie aplikacji sieci web

Sprawdź, czy aplikacja sieci web działa przy użyciu przeglądania (`http://<app_name>azurewebsites.net`). 

![Konfiguracja portu aplikacji sieci web testu](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="change-web-app-and-redeploy"></a>Zmień aplikacji sieci web i utwórz je ponownie

W lokalnym repozytorium Git Otwórz app/templates/app/index.html. Znajdź pierwszy element HTML i zmień, aby.

```python
<nav class="navbar navbar-inverse navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">         
        <a class="navbar-brand" href="#">Azure App Service - Updated Here!</a>       
      </div>            
    </div>
  </nav> 
```

Po został zmodyfikowany plik Python i zapisany, należy odbudować i push nowy obraz Docker. Następnie uruchom ponownie aplikację sieci web, aby zmiany zaczęły obowiązywać. Użyj tych samych poleceń, które wcześniej były używane w tym samouczku. Można odwołać się do [utworzyć obraz z pliku Docker](#build-the-image-from-the-docker-file) i [wypychanie obrazu Docker do Centrum Docker](#push-the-docker-image-to-docker-hub). Testowanie aplikacji sieci web zgodnie z instrukcjami w [testów aplikacji sieci web](#test-the-web-app).

## <a name="connect-to-web-app-for-containers-using-ssh"></a>Łączenie aplikacji sieci Web dla kontenerów przy użyciu protokołu SSH

SSH umożliwia bezpiecznej komunikacji między kontenerem klientem. Aby Docker obrazu niestandardowego do obsługi protokołu SSH należy utworzyć ją na plik Dockerfile. Musisz włączyć SSH w samym pliku Docker. Instrukcje SSH już zostały dodane do przykładowy plik dockerfile, aby można było wykonywać te instrukcje z niestandardowego obrazu:

* A [Uruchom](https://docs.docker.com/engine/reference/builder/#run) instrukcji, która wywołuje `apt-get`, następnie ustawia hasło dla konta głównego na `"Docker!"`.

    ```docker
    ENV SSH_PASSWD "root:Docker!"
    RUN apt-get update \
            && apt-get install -y --no-install-recommends dialog \
            && apt-get update \
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "$SSH_PASSWD" | chpasswd 
    ```

    > [!NOTE]
    > Ta konfiguracja nie zezwala na połączenia zewnętrzne do kontenera. SSH jest dostępna tylko za pośrednictwem witryny Kudu/SCM. Witryna Kudu/SCM jest uwierzytelniany przy użyciu poświadczeń publikowania.

* A [KOPIOWANIA](https://docs.docker.com/engine/reference/builder/#copy) instrukcji, która sprawia, że aparat Docker można skopiować [sshd_config](http://man.openbsd.org/sshd_config) pliku */itp/ssh/* katalogu. Plik konfiguracji powinny być oparte na [ten plik sshd_config](https://github.com/Azure-App-Service/node/blob/master/6.11.1/sshd_config).

    ```docker
    COPY sshd_config /etc/ssh/
    ```

    > [!NOTE]
    > *Sshd_config* plik musi zawierać następujące elementy: 
    > * `Ciphers`musi zawierać co najmniej jeden element na liście: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs`musi zawierać co najmniej jeden element na liście: `hmac-sha1,hmac-sha1-96`.

* [UJAWNIA](https://docs.docker.com/engine/reference/builder/#expose) instrukcji czy ujawnia portu 2222 w kontenerze. Chociaż hasła głównego jest znany, port 2222 nie jest dostępny z Internetu. Jest wewnętrzny port dostępny tylko przez kontenery w ramach sieci Mostek wirtualnej sieci prywatnej. Po wykonaniu tej polecenia Kopiuj szczegóły konfiguracji SSH i uruchomić `ssh` usługi.

    ```docker
    EXPOSE 8000 2222
    ```

* Upewnij się, że [uruchomić usługi ssh](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) za pomocą skryptu powłoki w katalogu/bin.
 
    ```bash
    #!/bin/bash
    service ssh start
    ```
     
### <a name="open-ssh-connection-to-container"></a>Otwarte połączenie SSH do kontenera

Aplikacji dla kontenerów sieci Web nie zezwala na połączenia zewnętrzne do kontenera. SSH jest dostępna tylko za pośrednictwem witryny Kudu, która jest dostępna w `https://<app_name>.scm.azurewebsites.net`.

Aby połączyć, przejdź do `https://<app_name>.scm.azurewebsites.net/webssh/host` i zaloguj się przy użyciu konta platformy Azure.

Możesz są przekierowywane do wyświetlania konsoli interakcyjne strony. 

Możesz zweryfikować, że niektóre aplikacje są uruchomione w kontenerze. Aby sprawdzić kontenera i sprawdź uruchomione procesy, należy wydać `top` w wierszu polecenia.

```bash
top
```

`top` Polecenie przedstawia wszystkich procesów uruchomionych w kontenerze.

```
PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1 root      20   0  945616  35372  15348 S  0.0  2.1   0:04.63 node
20 root      20   0   55180   2776   2516 S  0.0  0.2   0:00.00 sshd
42 root      20   0  944596  33340  15352 S  0.0  1.9   0:05.80 node /opt/s+
56 root      20   0   59812   5244   4512 S  0.0  0.3   0:00.93 sshd
58 root      20   0   20228   3128   2664 S  0.0  0.2   0:00.00 bash
62 root      20   0   21916   2272   1944 S  0.0  0.1   0:03.15 top
63 root      20   0   59812   5344   4612 S  0.0  0.3   0:00.03 sshd
65 root      20   0   20228   3140   2672 S  0.0  0.2   0:00.00 bash
71 root      20   0   59812   5380   4648 S  0.0  0.3   0:00.02 sshd
73 root      20   0   20228   3160   2696 S  0.0  0.2   0:00.00 bash
77 root      20   0   21920   2304   1972 R  0.0  0.1   0:00.00 top
```

Gratulacje! Obraz niestandardowy Docker skonfigurowano dla aplikacji sieci Web dla kontenerów.

## <a name="use-a-private-image-from-docker-hub-optional"></a>Korzystanie z prywatnych obrazu z Centrum Docker (opcjonalnie)

W [utworzenia aplikacji sieci web](#create-a-web-app), określony obraz w Centrum Docker w `az webapp create` polecenia. Jest to dobra publicznego obrazu. Aby korzystać z prywatnych obrazu, należy skonfigurować swoją Docker nazwę konta i hasło w aplikacji sieci web platformy Azure.

W powłoce chmury, wykonaj `az webapp create` z [zestaw kontenera konfiguracji aplikacji sieci Web az](/cli/azure/webapp/config/container#az_webapp_config_container_set). Zastąp  *\<nazwa_aplikacji >*, a także _< docker-id >_ i  _<password>_  z Docker ID i hasła.

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-registry-server-user <docker-id> --docker-registry-server-password <password>
```

Polecenie ujawnia dane wyjściowe podobne do następującego ciągu JSON, pokazujący, że zmiana konfiguracji zakończyło się pomyślnie:

```json
[
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "slotSetting": false,
    "value": "false"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
    "slotSetting": false,
    "value": "<docker-id>"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
    "slotSetting": false,
    "value": null
  },
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "DOCKER|<image-name-and-tag>"
  }
]
```

## <a name="use-a-docker-image-from-any-private-registry-optional"></a>Należy użyć obrazu Docker z dowolnego rejestru prywatnego (opcjonalnie)

W tej sekcji możesz dowiedzieć się, jak używać obrazu platformy Docker z prywatnej rejestru w aplikacji sieci Web dla kontenerów, i używa rejestru kontenera Azure jako przykład. Kroki do używania innych rejestrów prywatne są podobne. 

Rejestru kontenera platformy Azure jest zarządzanych usług Docker na platformie Azure do obsługi obrazów prywatnych. Wdrożenia może być dowolnego typu, łącznie z [Docker Swarm](https://docs.docker.com/engine/swarm/), [Kubernetes](https://kubernetes.io/)i aplikacji sieci Web dla kontenerów. 

### <a name="create-an-azure-container-registry"></a>Tworzenie rejestru Azure Container Registry

W powłoce chmury za pomocą [az acr utworzyć](https://docs.microsoft.com/cli/azure/acr#az_acr_create) polecenie, aby utworzyć rejestru kontenera platformy Azure. Podaj nazwę grupy zasobów i `Basic` dla jednostki SKU. Są dostępne jednostki SKU `Classic`, `Basic`, `Standard`, i `Premium`.

```azurecli-interactive
az acr create --name <azure-container-registry-name> --resource-group myResourceGroup --sku Basic --admin-enabled true
```

Utworzenie kontenera tworzy następujące dane wyjściowe:

```
 - Finished ..
Create a new service principal and assign access:
  az ad sp create-for-rbac --scopes /subscriptions/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name> --role Owner --password <password>

Use an existing service principal and assign access:
  az role assignment create --scope /subscriptions/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name> --role Owner --assignee <app-id>
{
  "adminUserEnabled": false,
  "creationDate": "2017-08-09T04:21:09.654153+00:00",
  "id": "/subscriptions/<subscriptionId>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name>",
  "location": "westeurope",
  "loginServer": "<azure-container-registry-name>.azurecr.io",
  "name": "<azure-container-registry-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "myazurecontainerre042025"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="log-in-to-azure-container-registry"></a>Zaloguj się do rejestru kontenera platformy Azure

Aby wypchnąć obrazu w rejestrze, musisz podać poświadczenia, więc rejestru akceptuje powiadomienia wypychanego. Te poświadczenia można pobrać za pomocą [Pokaż acr az](https://docs.microsoft.com/cli/azure/acr/credential#az_acr_credential_show) polecenie w powłoce chmury. 

```azurecli-interactive
az acr credential show --name <azure-container-registry-name>
```

Polecenie ujawnia dwóch haseł, których można użyć z nazwą użytkownika.

```json
<
  "passwords": [
    {
      "name": "password",
      "value": "{password}"
    },
    {
      "name": "password2",
      "value": "{password}"
    }
  ],
  "username": "<registry-username>"
}
```

Z lokalnym okno terminalu, zaloguj się za pomocą rejestru kontenera Azure `docker login` polecenia. Nazwa serwera jest wymagany do logowania. Użyj formatu `{azure-container-registry-name>.azurecr.io`.

```bash
docker login <azure-container-registry-name>.azurecr.io --username <registry-username> --password <password> 
```

Upewnij się, że nazwa logowania zakończyła się pomyślnie. 

### <a name="push-an-image-to-azure-container-registry"></a>Wypychanie obrazu do rejestru kontenera platformy Azure

> [!NOTE]
> Jeśli korzystasz z własnego obrazu, tag obrazu w następujący sposób:
> ```bash
> docker tag <azure-container-registry-name>.azurecr.io/mydockerimage
> ```

Wypychanie obrazu przy użyciu `docker push` polecenia. Tag obrazu o nazwie rejestru, następuje swoją nazwę obrazu i tagów.

```bash
docker push <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Sprawdź, czy naciśnięcie pomyślnie dodany kontener w rejestrze poprzez wyszczególnienie repozytoria ACR. 

```azurecli-interactive
az acr repository list -n <azure-container-registry-name>
```

Wyświetlanie obrazów w rejestrze potwierdza, że `mydockerimage` w rejestrze.

```json
[
  "mydockerimage"
]
```

### <a name="configure-web-app-to-use-the-image-from-azure-container-registry-or-any-private-registry"></a>Konfigurowanie aplikacji sieci Web przy użyciu obrazu z rejestru kontenera platformy Azure (lub dowolnego rejestru prywatne)

Można skonfigurować aplikacji sieci Web dla kontenerów wykonywania kontener przechowywane w rejestrze kontenera platformy Azure. Za pomocą rejestru kontenera Azure działa tak, jak za pomocą dowolnego prywatnej rejestru, więc jeśli chcesz użyć własnych rejestru prywatne, kroki do wykonania tego zadania są podobne.

W powłoce chmury Uruchom [Pokaż poświadczeń acr az](/cli/azure/acr/credential#az_acr_credential_show) do wyświetlenia nazwy użytkownika i hasła w rejestrze kontenera platformy Azure. Skopiuj nazwę użytkownika i haseł, aby można było ich użyć do skonfigurowania aplikacji sieci web w następnym kroku.

```bash
az acr credential show --name <azure-container-registry-name>
```

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "password"
    },
    {
      "name": "password2",
      "value": "password2"
    }
  ],
  "username": "<registry-username>"
}
```

W powłoce chmury Uruchom [zestaw kontenera konfiguracji aplikacji sieci Web az](/cli/azure/webapp/config/container#az_webapp_config_container_set) polecenie, aby przypisać niestandardowego obrazu Docker do aplikacji sieci web. Zastąp  *\<nazwa_aplikacji >*,  *\<docker rejestru server-url >*,  _\<rejestru username >_i  _\<hasło >_. Do rejestru kontenera platformy Azure  *\<docker rejestru server-url >* jest w formacie `https://<azure-container-registry-name>.azurecr.io`. 

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-custom-image-name mydockerimage --docker-registry-server-url https://<azure-container-registry-name>.azurecr.io --docker-registry-server-user <registry-username> --docker-registry-server-password <password>
```

> [!NOTE]
> `https://`jest wymagany w  *\<docker rejestru server-url >*.
>

Polecenie ujawnia dane wyjściowe podobne do następującego ciągu JSON, pokazujący, że zmiana konfiguracji zakończyło się pomyślnie:

```json
[
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "slotSetting": false,
    "value": "mydockerimage"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_URL",
    "slotSetting": false,
    "value": "<azure-container-registry-name>.azurecr.io"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
    "slotSetting": false,
    "value": "<registry-username>"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
    "slotSetting": false,
    "value": null
  }
]
```

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Tworzenie aplikacji sieci web Docker Python i PostgreSQL na platformie Azure](tutorial-docker-python-postgresql-app.md)
