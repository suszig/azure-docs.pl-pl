---
title: "Samouczek wystąpień kontenera platformy Azure — przygotowanie aplikacji"
description: "Przygotowywanie aplikacji do wdrożenia w usłudze Azure Container Instances"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 11/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 6a168b600833c7e4544da7a5f5f4b7a0af73e0b5
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a>Tworzenie kontenera do wdrożenia w usłudze Azure Container Instances

Usługa Azure Container Instances umożliwia wdrażanie kontenerów Docker w infrastrukturze platformy Azure bez aprowizowania maszyn wirtualnych ani adaptowania usług wyższego poziomu. W tym samouczku tworzenie aplikacji sieci web małe w środowisku Node.js i pakiet go w kontenerze, który można uruchomić za pomocą wystąpień kontenera platformy Azure. Zostaną opisane:

> [!div class="checklist"]
> * Klonowanie źródła aplikacji z usługi GitHub
> * Tworzenie obrazów kontenera z poziomu źródła aplikacji
> * Testowanie obrazów w lokalnym środowisku Docker

W kolejnych samouczkach przekazywanie obrazu w rejestrze kontenera platformy Azure, a następnie wdrożyć do wystąpień kontenera platformy Azure.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Ten samouczek wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.21 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli).

Ten samouczek zakłada podstawową wiedzę na temat podstawowych pojęć Docker takich jak kontenery, kontener obrazów i podstawowe `docker` poleceń. W razie potrzeby zapoznaj się z tematem [Get starter with Docker (Rozpoczynanie pracy z platformą Docker)]( https://docs.docker.com/get-started/), aby uzyskać podstawowe informacje na temat kontenerów.

Do ukończenia tego samouczka konieczne będzie środowisko programowania Docker. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) lub [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

Azure powłoki chmury nie zawiera składniki Docker wymagane do ukończenia każdego kroku w tym samouczku. Dlatego zaleca się instalacji lokalnej środowiska deweloperskiego wiersza polecenia platformy Azure i klastrem Docker.

## <a name="get-application-code"></a>Pobieranie kodu aplikacji

Przykład podany w tym samouczku obejmuje prostą aplikację internetową skompilowaną w języku [Node.js](http://nodejs.org). Aplikacja obsługuje statyczną stronę HTML i wygląda następująco:

![Samouczek aplikacji wyświetlony w przeglądarce][aci-tutorial-app]

Użyj usługi Git do pobrania przykładu:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>Kompilowanie obrazu kontenera

Plik Dockerfile znajdujący się w przykładowym repozytorium przedstawia sposób kompilowania kontenera. Praca rozpoczyna od [oficjalnego obrazu Node.js][dockerhub-nodeimage] opartego na systemie [Alpine Linux](https://alpinelinux.org/), małej dystrybucji, która dobrze nadaje się do korzystania z kontenerów. Kolejny krok to skopiowanie plików aplikacji do kontenera, zainstalowanie zależności za pomocą programu Node Package Manager i na koniec uruchomienie aplikacji.

```Dockerfile
FROM node:8.2.0-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Użyj polecenia `docker build`, aby utworzyć obraz kontenera, oznaczając go jako *aci-tutorial-app*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

Dane wyjściowe z `docker build` polecenie jest podobne do następujących (obcięty dla czytelności):

```bash
Sending build context to Docker daemon  119.3kB
Step 1/6 : FROM node:8.2.0-alpine
8.2.0-alpine: Pulling from library/node
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
Digest: sha256:c73277ccc763752b42bb2400d1aaecb4e3d32e3a9dbedd0e49885c71bea07354
Status: Downloaded newer image for node:8.2.0-alpine
 ---> 90f5ee24bee2
...
Step 6/6 : CMD node /usr/src/app/index.js
 ---> Running in f4a1ea099eec
 ---> 6edad76d09e9
Removing intermediate container f4a1ea099eec
Successfully built 6edad76d09e9
Successfully tagged aci-tutorial-app:latest
```

Użyj polecenia `docker images`, aby wyświetlić skompilowany obraz:

```bash
docker images
```

Dane wyjściowe:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

## <a name="run-the-container-locally"></a>Uruchamianie kontenera w środowisku lokalnym

Przed przystąpieniem do wdrażania w usłudze Azure Container Instances należy uruchomić go lokalnie w celu potwierdzenia, czy działa. Przełącznik `-d` umożliwia uruchamianie kontenera w tle, a element `-p` — mapowanie dowolnego portu obliczeń do portu 80 w kontenerze.

```bash
docker run -d -p 8080:80 aci-tutorial-app
```

Otwórz przeglądarkę z adresem http://localhost:8080, aby potwierdzić, że kontener został uruchomiony.

![Uruchamianie aplikacji lokalnie w przeglądarce][aci-tutorial-app-local]

## <a name="next-steps"></a>Następne kroki

Podczas pracy z samouczkiem utworzono obraz kontenera, który można wdrożyć w usłudze Azure Container Instances. Wykonano następujące czynności:

> [!div class="checklist"]
> * Sklonowany źródłowy aplikacji z usługi GitHub
> * Obrazy utworzone kontenera ze źródła aplikacji
> * Przetestowane kontenera lokalnie

Przejdź do kolejnego samouczka, aby dowiedzieć się więcej o przechowywaniu obrazów kontenera w usłudze Azure Container Registry.

> [!div class="nextstepaction"]
> [Wypychanie obrazów do usługi Azure Container Registry](./container-instances-tutorial-prepare-acr.md)

<!-- LINKS -->
[dockerhub-nodeimage]: https://store.docker.com/images/node

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png