---
title: "Samouczek wystąpień kontenera platformy Azure — przygotowanie aplikacji"
description: "Azure wystąpień kontenera samouczek część 1 z 3 — Przygotowanie aplikacji do wdrożenia do wystąpień kontenera platformy Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: fc16be80e776d1472be775fa32354ba157d16545
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2018
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a>Tworzenie kontenera do wdrożenia w usłudze Azure Container Instances

Usługa Azure Container Instances umożliwia wdrażanie kontenerów Docker w infrastrukturze platformy Azure bez aprowizowania maszyn wirtualnych ani adaptowania usług wyższego poziomu. W tym samouczku tworzenie aplikacji sieci web małe w środowisku Node.js i pakiet go w kontenerze, który można uruchomić za pomocą wystąpień kontenera platformy Azure.

W tym artykule, należy do jednej serii, możesz:

> [!div class="checklist"]
> * Klonowanie kodu źródłowego aplikacji z usługi GitHub
> * Tworzenie obrazu kontenera ze źródła aplikacji
> * Testowanie w środowisku lokalnym Docker obrazu

W kolejnych samouczkach przekazywanie obrazu w rejestrze kontenera platformy Azure, a następnie wdrożyć do wystąpień kontenera platformy Azure.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Ten samouczek wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.23 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli musisz zainstalować lub uaktualnić, zobacz [zainstalować Azure CLI 2.0][azure-cli-install].

Ten samouczek zakłada podstawową wiedzę na temat podstawowych pojęć Docker takich jak kontenery, kontener obrazów i podstawowe `docker` poleceń. Jeśli to konieczne, zobacz [Rozpoczynanie pracy z rozwiązaniem Docker] [ docker-get-started] dla Elementarz na podstawy kontenera.

Do ukończenia tego samouczka, należy lokalnie zainstalować Środowisko deweloperskie Docker. Docker zawiera pakiety, które łatwo skonfigurować Docker na dowolnym [Mac][docker-mac], [Windows][docker-windows], lub [Linux] [ docker-linux] systemu.

Azure powłoki chmury nie zawiera składniki Docker wymagane do ukończenia każdego kroku w tym samouczku. Środowisko projektowe wiersza polecenia platformy Azure i klastrem Docker należy zainstalować na komputerze lokalnym do ukończenia tego samouczka.

## <a name="get-application-code"></a>Pobieranie kodu aplikacji

Przykładowe w tym samouczku obejmuje prostą aplikację sieci web wbudowane [Node.js][nodejs]. Aplikacja obsługuje statyczną stronę HTML i wygląda następująco:

![Samouczek aplikacji wyświetlony w przeglądarce][aci-tutorial-app]

Użyj usługi Git do pobrania przykładu:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>Kompilowanie obrazu kontenera

Plik Dockerfile znajdujący się w przykładowym repozytorium przedstawia sposób kompilowania kontenera. Rozpoczyna od [oficjalnego obrazu Node.js] [ docker-hub-nodeimage] na podstawie [Alpine Linux][alpine-linux], mała dystrybucji, które dobrze nadają się do użycia z kontenery. Kolejny krok to skopiowanie plików aplikacji do kontenera, zainstalowanie zależności za pomocą programu Node Package Manager i na koniec uruchomienie aplikacji.

```Dockerfile
FROM node:8.9.3-alpine
RUN mkdir -p /usr/src/app
COPY ./app/* /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Użyj [kompilacji docker] [ docker-build] polecenie, aby utworzyć obraz kontenera, oznaczanie go jako *aci samouczek aplikacji*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

Dane wyjściowe z [kompilacji docker] [ docker-build] polecenie jest podobne do następujących (obcięty dla czytelności):

```bash
Sending build context to Docker daemon  119.3kB
Step 1/6 : FROM node:8.9.3-alpine
8.9.3-alpine: Pulling from library/node
88286f41530e: Pull complete
84f3a4bf8410: Pull complete
d0d9b2214720: Pull complete
Digest: sha256:c73277ccc763752b42bb2400d1aaecb4e3d32e3a9dbedd0e49885c71bea07354
Status: Downloaded newer image for node:8.9.3-alpine
 ---> 90f5ee24bee2
...
Step 6/6 : CMD node /usr/src/app/index.js
 ---> Running in f4a1ea099eec
 ---> 6edad76d09e9
Removing intermediate container f4a1ea099eec
Successfully built 6edad76d09e9
Successfully tagged aci-tutorial-app:latest
```

Użyj [obrazy usługi docker] [ docker-images] polecenie, aby wyświetlić wbudowanych obrazu:

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

## <a name="next-steps"></a>Kolejne kroki

Podczas pracy z samouczkiem utworzono obraz kontenera, który można wdrożyć w usłudze Azure Container Instances. Wykonano następujące czynności:

> [!div class="checklist"]
> * Sklonowany źródłowy aplikacji z usługi GitHub
> * Obrazy utworzone kontenera ze źródła aplikacji
> * Przetestowane kontenera lokalnie

Przejdź do kolejnego samouczka, aby dowiedzieć się więcej o przechowywaniu obrazów kontenera w usłudze Azure Container Registry.

> [!div class="nextstepaction"]
> [Wypychanie obrazów do usługi Azure Container Registry](./container-instances-tutorial-prepare-acr.md)

<!--- IMAGES --->
[aci-tutorial-app]:./media/container-instances-quickstart/aci-app-browser.png
[aci-tutorial-app-local]: ./media/container-instances-tutorial-prepare-app/aci-app-browser-local.png

<!-- LINKS - External -->
[alpine-linux]: https://alpinelinux.org/
[docker-build]: https://docs.docker.com/engine/reference/commandline/build/
[docker-get-started]: https://docs.docker.com/get-started/
[docker-hub-nodeimage]: https://store.docker.com/images/node
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[nodejs]: http://nodejs.org

<!-- LINKS - Internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
