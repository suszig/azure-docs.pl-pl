---
title: "Samouczek usługi Azure Container Instances — przygotowywanie usługi Azure Container Registry"
description: "Samouczek usługi Azure Container Instances (część 2 z 3) — przygotowywanie usługi Azure Container Registry"
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 94ecba44b8281460da4518c146aab814d2eaa850
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
# <a name="deploy-and-use-azure-container-registry"></a>Wdrażanie usługi Azure Container Registry i korzystanie z niej

Niniejszy samouczek jest drugą częścią trzyczęściowej serii. W [poprzednim kroku](container-instances-tutorial-prepare-app.md) utworzono obraz kontenera na potrzeby prostej aplikacji internetowej napisanej w języku [Node.js][nodejs]. W tym samouczku obraz zostanie wypchnięty do usługi Azure Container Registry. Jeśli obraz kontenera nie został utworzony, wróć do artykułu [Samouczek 1 — Tworzenie obrazu kontenera](container-instances-tutorial-prepare-app.md).

Usługa Azure Container Registry to oparty na platformie Azure rejestr prywatny na potrzeby obrazów kontenerów platformy Docker. Ten samouczek zawiera opis sposobu wdrażania wystąpienia usługi Azure Container Registry i wypychania do niego obrazu kontenera.

W tym artykule, który jest drugą częścią trzyczęściowej serii, zostaną wykonane następujące czynności:

> [!div class="checklist"]
> * Wdrożenie wystąpienia usługi Azure Container Registry
> * Otagowanie obrazu kontenera na potrzeby usługi Azure Container Registry
> * Przekazanie obrazu do rejestru

W następnym artykule, czyli ostatnim artykule z tej serii, będzie miało miejsce wdrożenie kontenera z poziomu rejestru prywatnego do usługi Azure Container Instances.

## <a name="before-you-begin"></a>Przed rozpoczęciem

W tym samouczku wymagany jest interfejs wiersza polecenia platformy Azure w wersji 2.0.23 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0][azure-cli-install].

Do ukończenia tego samouczka konieczne będzie zainstalowane lokalnie środowisko deweloperskie platformy Docker. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac][docker-mac], [Windows][docker-windows] lub [Linux][docker-linux].

Usługa Azure Cloud Shell nie zawiera składników platformy Docker wymaganych do ukończenia każdego kroku w tym samouczku. Aby ukończyć ten samouczek, należy zainstalować interfejs wiersza polecenia platformy Azure i środowisko deweloperskie platformy Docker na komputerze lokalnym.

## <a name="deploy-azure-container-registry"></a>Wdrażanie usługi Azure Container Registry

W przypadku wdrażania usługi Azure Container Registry należy najpierw posiadać grupę zasobów. Grupa zasobów platformy Azure to kolekcja logiczna przeznaczona do wdrażania zasobów platformy Azure i zarządzania nimi.

Utwórz grupę zasobów za pomocą polecenia [az group create][az-group-create]. W tym przykładzie grupa zasobów o nazwie *myResourceGroup* zostanie utworzona w regionie *eastus*.

```azurecli
az group create --name myResourceGroup --location eastus
```

Utwórz usługę Azure Container Registry za pomocą polecenia [az acr create][az-acr-create]. Nazwa rejestru kontenerów musi być unikatowa w obrębie platformy Azure i może zawierać od 5 do 50 znaków alfanumerycznych. Zamień wartość `<acrName>` na unikatową nazwę rejestru:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Aby na przykład utworzyć rejestr Azure Container Registry o nazwie *mycontainerregistry082*:

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

W dalszej części tego samouczka wartość `<acrName>` zostanie użyta jako symbol zastępczy wybranej nazwy rejestru kontenerów.

## <a name="container-registry-login"></a>Logowanie do rejestru Container Registry

Należy zalogować się do wystąpienia usługi Azure Container Registry przed wypchnięciem do niego obrazów. Aby wykonać tę operację, użyj polecenia [az acr login][az-acr-login]. Należy wprowadzić unikatową nazwę podaną na potrzeby rejestru kontenerów podczas jego tworzenia.

```azurecli
az acr login --name <acrName>
```

Po ukończeniu polecenie zwraca komunikat `Login Succeeded`.

## <a name="tag-container-image"></a>Tagowanie obrazu kontenera

Aby wdrożyć obraz kontenera z rejestru prywatnego, należy utworzyć tag obrazu z nazwą `loginServer` rejestru.

Aby wyświetlić listę bieżących obrazów, użyj polecenia [docker images][docker-images].

```bash
docker images
```

Dane wyjściowe:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

Aby uzyskać nazwę loginServer, uruchom polecenie [az acr show][az-acr-show]. Zastąp wartość `<acrName>` nazwą rejestru kontenerów.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Przykładowe dane wyjściowe:

```
Result
------------------------
mycontainerregistry082.azurecr.io
```

Otaguj obraz *aci-tutorial-app* z wartością loginServer rejestru kontenerów. Ponadto dodaj wartość `:v1` na końcu nazwy obrazu. Ten tag wskazuje numer wersji obrazu. Zastąp wartość `<acrLoginServer>` wynikiem polecenia [az acr show][az-acr-show], które zostało właśnie wykonane.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Po otagowaniu uruchom polecenie `docker images`, aby zweryfikować operację.

```bash
docker images
```

Dane wyjściowe:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Wypychanie obrazu do usługi Azure Container Registry

Wypchnij obraz *aci-tutorial-app* do rejestru za pomocą polecenia [docker push][docker-push]. Zastąp wartość `<acrLoginServer>` pełną nazwa serwera logowania uzyskaną we wcześniejszym kroku.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

Operacja `push` może trwać od kilku sekund do kilku minut w zależności od połączenia internetowego, a jej dane wyjściowe są podobne następujących:

```bash
The push refers to a repository [mycontainerregistry082.azurecr.io/aci-tutorial-app]
3db9cac20d49: Pushed
13f653351004: Pushed
4cd158165f4d: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:ed67fff971da47175856505585dcd92d1270c3b37543e8afd46014d328f05715 size: 1576
```

## <a name="list-images-in-azure-container-registry"></a>Wyświetlanie listy obrazów w usłudze Azure Container Registry

Aby zwrócić listę obrazów, które zostały wypchnięte do usługi Azure Container Registry, użyj polecenia [az acr repository list][az-acr-repository-list]. Zaktualizuj polecenie nazwą rejestru kontenerów.

```azurecli
az acr repository list --name <acrName> --output table
```

Dane wyjściowe:

```azurecli
Result
----------------
aci-tutorial-app
```

Aby następnie wyświetlić tagi dla określonego obrazu, użyj polecenia [az acr repository show-tags][az-acr-repository-show-tags].

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Dane wyjściowe:

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku przygotowano usługę Azure Container Registry do używania z usługą Azure Container Instances oraz wypchnięto obraz kontenera do rejestru. Wykonano następujące czynności:

> [!div class="checklist"]
> * Wdrażanie wystąpienia usługi Azure Container Registry
> * Tagowanie obrazu kontenera na potrzeby usługi Azure Container Registry
> * Przekazywanie obrazu do usługi Azure Container Registry

Przejdź do kolejnego samouczka, aby uzyskać więcej informacji o wdrażaniu kontenera za pomocą usługi Azure Container Instances.

> [!div class="nextstepaction"]
> [Wdrażanie kontenerów do usługi Azure Container Instances](./container-instances-tutorial-deploy-app.md)

<!-- LINKS - External -->
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
[az-acr-create]: /cli/azure/acr#az_acr_create
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-acr-repository-list]: /cli/azure/acr/repository#az_acr_list
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az_acr_repository_show_tags
[az-acr-show]: /cli/azure/acr#az_acr_show
[az-group-create]: /cli/azure/group#az_group_create
[azure-cli-install]: /cli/azure/install-azure-cli
