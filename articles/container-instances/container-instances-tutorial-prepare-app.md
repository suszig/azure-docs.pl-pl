---
title: "Samouczek usługi Azure Container Instances — przygotowywanie aplikacji"
description: "Samouczek usługi Azure Container Instances, część 1 z 3 — przygotowywanie aplikacji do wdrożenia w usłudze Azure Container Instances"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 5012412ec642a04102836274caea253635376efb
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="create-container-for-deployment-to-azure-container-instances"></a>Tworzenie kontenera do wdrożenia w usłudze Azure Container Instances

Usługa Azure Container Instances umożliwia wdrażanie kontenerów Docker w infrastrukturze platformy Azure bez aprowizowania maszyn wirtualnych ani adaptowania usług wyższego poziomu. Podczas pracy z tym samouczkiem utworzysz małą aplikację internetową w środowisku Node.js i spakujesz ją w postaci kontenera, który można uruchomić za pomocą usługi Azure Container Instances.

W tym artykule, który jest pierwszą częścią trzyczęściowej serii, wykonasz następujące działania:

> [!div class="checklist"]
> * Klonowanie kodu źródłowego aplikacji z usługi GitHub
> * Tworzenie obrazu kontenera ze źródła aplikacji
> * Testowanie obrazu w lokalnym środowisku Docker

Podczas pracy z kolejnymi samouczkami przekażesz obraz do usługi Azure Container Registry, a następnie wdrożysz go w usłudze Azure Container Instances.

## <a name="before-you-begin"></a>Przed rozpoczęciem

W tym samouczku wymagany jest interfejs wiersza polecenia platformy Azure w wersji 2.0.23 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0][azure-cli-install].

Ten samouczek zakłada, że masz podstawową wiedzę na temat najważniejszych pojęć usługi Docker, takich jak kontenery, obrazy kontenera i podstawowe polecenia `docker`. W razie potrzeby zapoznaj się z tematem [Get started with Docker (Rozpoczynanie pracy z platformą Docker)][docker-get-started], aby uzyskać podstawowe informacje na temat kontenerów.

Do ukończenia tego samouczka konieczne będzie zainstalowane lokalnie środowisko deweloperskie platformy Docker. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac][docker-mac], [Windows][docker-windows] lub [Linux][docker-linux].

Usługa Azure Cloud Shell nie zawiera składników platformy Docker wymaganych do ukończenia każdego kroku w tym samouczku. Aby ukończyć ten samouczek, należy zainstalować interfejs wiersza polecenia platformy Azure i środowisko deweloperskie platformy Docker na komputerze lokalnym.

## <a name="get-application-code"></a>Pobieranie kodu aplikacji

Przykład podany w tym samouczku obejmuje prostą aplikację internetową utworzoną za pomocą oprogramowania [Node.js][nodejs]. Aplikacja obsługuje statyczną stronę HTML i wygląda następująco:

![Samouczek aplikacji wyświetlony w przeglądarce][aci-tutorial-app]

Użyj usługi Git do pobrania przykładu:

```bash
git clone https://github.com/Azure-Samples/aci-helloworld.git
```

## <a name="build-the-container-image"></a>Kompilowanie obrazu kontenera

Plik Dockerfile znajdujący się w przykładowym repozytorium przedstawia sposób kompilowania kontenera. Praca rozpoczyna się od [oficjalnego obrazu oprogramowania Node.js][docker-hub-nodeimage] opartego na systemie [Alpine Linux][alpine-linux], małej dystrybucji, która dobrze nadaje się do korzystania z kontenerów. Kolejny krok to skopiowanie plików aplikacji do kontenera, zainstalowanie zależności za pomocą programu Node Package Manager i na koniec uruchomienie aplikacji.

```Dockerfile
FROM node:8.9.3-alpine
RUN mkdir -p /usr/src/app
COPY ./app/ /usr/src/app/
WORKDIR /usr/src/app
RUN npm install
CMD node /usr/src/app/index.js
```

Użyj polecenia [docker build][docker-build], aby utworzyć obraz kontenera, oznaczając go jako *aci-tutorial-app*:

```bash
docker build ./aci-helloworld -t aci-tutorial-app
```

Dane wyjściowe polecenia [docker build][docker-build] są podobne do następujących (tutaj obcięto je dla zwiększenia czytelności):

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

Użyj polecenia [docker images][docker-images], aby wyświetlić utworzony obraz:

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
> * Klonowanie źródła aplikacji z usługi GitHub
> * Tworzenie obrazu kontenera z poziomu źródła aplikacji
> * Testowanie kontenera w środowisku lokalnym

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
