---
title: "Samouczek wystąpień kontenera platformy Azure — przygotowanie rejestru kontenera platformy Azure"
description: "Azure wystąpień kontenera samouczek część 2 z 3 — Przygotowanie rejestru kontenera platformy Azure"
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c0aad1f9bbaac9a456b34f75633faba92f57f498
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-and-use-azure-container-registry"></a>Wdrażanie i użytkowanie rejestru kontenera platformy Azure

Jest to część dwa trzech części samouczka. W [poprzedniego kroku](container-instances-tutorial-prepare-app.md), obrazu kontener został utworzony dla prostą aplikację sieci web napisany w [Node.js][nodejs]. W tym samouczku wypychanych obrazu w rejestrze kontenera platformy Azure. Jeśli nie utworzono obrazów kontenera, wróć do [samouczek 1 — Tworzenie kontenera obrazu](container-instances-tutorial-prepare-app.md).

Rejestr kontenera Azure jest Azure, prywatnego rejestru dla obrazów kontenera Docker. Ten samouczek przedstawia sposób wdrażania wystąpienia rejestru kontenera Azure i wypychanie obrazu kontenera do niego.

W tym artykule część dwóch serii, możesz:

> [!div class="checklist"]
> * Wdróż wystąpienie rejestru kontenera platformy Azure
> * Tag obrazu kontenera rejestru kontenera platformy Azure
> * Przekaż obraz do rejestru

W artykule końcowego samouczek z tej serii, możesz wdrożyć kontenera z rejestru prywatnych do wystąpień kontenera platformy Azure.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Ten samouczek wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.23 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli musisz zainstalować lub uaktualnić, zobacz [zainstalować Azure CLI 2.0][azure-cli-install].

Do ukończenia tego samouczka, należy lokalnie zainstalować Środowisko deweloperskie Docker. Docker zawiera pakiety, które łatwo skonfigurować Docker na dowolnym [Mac][docker-mac], [Windows][docker-windows], lub [Linux] [ docker-linux] systemu.

Azure powłoki chmury nie zawiera składniki Docker wymagane do ukończenia każdego kroku w tym samouczku. Środowisko projektowe wiersza polecenia platformy Azure i klastrem Docker należy zainstalować na komputerze lokalnym do ukończenia tego samouczka.

## <a name="deploy-azure-container-registry"></a>Wdrażanie rejestru kontenera platformy Azure

W przypadku wdrażania rejestru kontenera platformy Azure, musisz najpierw grupę zasobów. Grupy zasobów platformy Azure jest logiczną kolekcji, w której maszyny wirtualne Azure zasoby są wdrażane i zarządzane.

Utwórz grupę zasobów za pomocą polecenia [az group create][az-group-create]. W tym przykładzie grupy zasobów o nazwie *myResourceGroup* jest tworzony w *eastus* regionu.

```azurecli
az group create --name myResourceGroup --location eastus
```

Tworzenie kontenera platformy Azure rejestru [az acr utworzyć] [ az-acr-create] polecenia. Nazwa kontenera rejestru **muszą być unikatowe** w obrębie platformy Azure i musi zawierać znaki alfanumeryczne 5 – 50. Zastąp `<acrName>` o unikatowej nazwie do rejestru:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Na przykład można utworzyć kontenera platformy Azure rejestru o nazwie *mycontainerregistry082*:

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

W dalszej części tego samouczka, używamy `<acrName>` jako nazwę rejestru kontenera, który został wybrany.

## <a name="container-registry-login"></a>Kontener rejestru logowania

Musisz zalogować się do Twojego wystąpienia rejestru kontenera Azure przed wypchnięciem obrazów do niego. Użyj [logowania acr az] [ az-acr-login] polecenia do wykonania operacji. Podaj unikatową nazwę podaną w rejestrze kontenera podczas jego tworzenia.

```azurecli
az acr login --name <acrName>
```

Polecenie zwraca `Login Succeeded` komunikat po ukończeniu.

## <a name="tag-container-image"></a>Tag obrazu kontenera

Aby wdrożyć obraz kontenera z rejestru prywatne, musi tag obrazu o `loginServer` nazwa rejestru.

Aby wyświetlić listę bieżącego obrazów, użyj [obrazy usługi docker] [ docker-images] polecenia.

```bash
docker images
```

Dane wyjściowe:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

Aby uzyskać nazwę loginServer, uruchom [Pokaż acr az] [ az-acr-show] polecenia. Zastąp `<acrName>` o nazwie rejestru kontenera.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Przykładowe dane wyjściowe:

```
Result
------------------------
mycontainerregistry082.azurecr.io
```

Tag *aci samouczek aplikacji* obrazu o loginServer Twojego rejestru kontenera. Ponadto Dodaj `:v1` na końcu nazwy obrazu. Znacznik określa numer wersji obrazu. Zastąp `<acrLoginServer>` z wynikiem [Pokaż acr az] [ az-acr-show] polecenie zostanie wykonane.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Uruchom po oznakowany, `docker images` Aby zweryfikować działanie.

```bash
docker images
```

Dane wyjściowe:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Obraz wypychania do rejestru kontenera platformy Azure

Wypychania *aci samouczek aplikacji* obrazu w rejestrze z [wypychania docker] [ docker-push] polecenia. Zastąp `<acrLoginServer>` logowania pełną nazwę serwera, możesz uzyskać w poprzednim kroku.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

`push` Operacji powinien zająć kilka sekund do kilku minut w zależności od połączenia internetowego, a wynik jest podobny do następującego:

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

## <a name="list-images-in-azure-container-registry"></a>Listy obrazów w rejestrze kontenera platformy Azure

Aby powrócić do listy obrazów, do których został przypisany do rejestru kontenera platformy Azure, użyj [listy repozytorium acr az] [ az-acr-repository-list] polecenia. Zaktualizuj polecenia o nazwie rejestru kontenera.

```azurecli
az acr repository list --name <acrName> --output table
```

Dane wyjściowe:

```azurecli
Result
----------------
aci-tutorial-app
```

A następnie wyświetlić tagi dla określonego obrazu, użyj [az acr repozytorium Pokaż znaczniki] [ az-acr-repository-show-tags] polecenia.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Dane wyjściowe:

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku przygotowany rejestru kontenera Azure do użycia z wystąpień kontenera platformy Azure, a wypychana obrazu kontenera do rejestru. Wykonano następujące czynności:

> [!div class="checklist"]
> * Wdrożone wystąpienie rejestru kontenera platformy Azure
> * Oznaczone tagami obrazu kontenera rejestru kontenera platformy Azure
> * Przekazać obraz w rejestrze kontenera platformy Azure

Przejdź do następnego samouczka, aby dowiedzieć się więcej o wdrażaniu kontenera na platformie Azure przy użyciu wystąpień kontenera Azure.

> [!div class="nextstepaction"]
> [Wdrażanie kontenerów do wystąpień kontenera platformy Azure](./container-instances-tutorial-deploy-app.md)

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
