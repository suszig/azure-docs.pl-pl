---
title: "Szybki start — tworzenie rejestru prywatnego platformy Docker na platformie Azure przy użyciu interfejsu wiersza polecenia platformy Azure"
description: "Szybka nauka tworzenia rejestru prywatnego platformy Docker przy użyciu interfejsu wiersza polecenia platformy Azure."
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 12/07/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: a74a1ce5c9401d6445f5feec4af8d5cb771d2c64
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Tworzenie rejestru kontenera za pomocą interfejsu wiersza polecenia platformy Azure

Usługa Azure Container Registry to zarządzana usługa rejestru kontenerów platformy Docker używana do przechowywania prywatnych obrazów kontenerów Docker. W tym przewodniku znajdują się szczegółowe informacje dotyczące tworzenia wystąpienia usługi Azure Container Registry przy użyciu interfejsu wiersza polecenia platformy Azure.

Ten przewodnik Szybki start wymaga interfejsu wiersza polecenia platformy Azure w wersji 2.0.25 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0][azure-cli].

Musisz mieć również zainstalowane lokalnie środowisko Docker. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac][docker-mac], [Windows][docker-windows] lub [Linux][docker-linux].

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create][az-group-create]. Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi.

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Tworzenie rejestru kontenerów

W tym przewodniku Szybki start utworzymy rejestr w ramach jednostki *Podstawowa*. Usługa Azure Container Registry jest dostępna w ramach kilku różnych jednostek SKU, krótko opisanych w poniższej tabeli. Aby uzyskać szczegółowe informacji o każdej z nich, zobacz [Jednostki SKU rejestru kontenerów][container-registry-skus].

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Utwórz wystąpienie usługi ACR za pomocą polecenia [az acr create][az-acr-create].

Nazwa rejestru musi być unikatowa w obrębie platformy Azure i może zawierać od 5 do 50 znaków alfanumerycznych. W poniższym przykładzie użyto nazwy *myContainerRegistry007*. Zaktualizuj ją do unikatowej wartości.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic
```

Po utworzeniu rejestru dane wyjściowe będą podobne do następujących:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-09-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "myContainerRegistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

W dalszej części tego przewodnika Szybki start wartość `<acrName>` zostanie użyta jako symbol zastępczy nazwy rejestru kontenerów.

## <a name="log-in-to-acr"></a>Zaloguj się do usługi ACR

Przed wypychaniem i ściąganiem obrazów kontenerów musisz zalogować się do wystąpienia usługi ACR. Aby to zrobić, użyj polecenia [az acr login][az-acr-login].

```azurecli
az acr login --name <acrName>
```

Po ukończeniu polecenie zwraca komunikat `Login Succeeded`.

## <a name="push-image-to-acr"></a>Wypychanie obrazu do usługi ACR

Aby wypchnąć obraz do usługi Azure Container Registry, najpierw musisz mieć obraz. Jeśli nie masz jeszcze żadnych lokalnych obrazów kontenerów, uruchom następujące polecenie, aby ściągnąć istniejący obraz z usługi Docker Hub.

```bash
docker pull microsoft/aci-helloworld
```

Zanim będzie można wypchnąć obraz do rejestru, musisz go otagować w pełni kwalifikowaną nazwą serwera logowania usługi ACR. Uruchom następujące polecenie, aby uzyskać pełną nazwę serwera logowania wystąpienia usługi ACR.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Aby dodać tag do obrazu, użyj polecenia [docker tag][docker-tag]. Zastąp element `<acrLoginServer>` nazwą serwera logowania wystąpienia usługi ACR.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Na koniec użyj polecenia [docker push][docker-push], aby wypchnąć obraz do wystąpienia usługi ACR. Zastąp element `<acrLoginServer>` nazwą serwera logowania wystąpienia usługi ACR.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>Tworzenie listy obrazów kontenerów

Poniższy przykład wyświetla listę repozytoriów w rejestrze:

```azurecli
az acr repository list --name <acrName> --output table
```

Dane wyjściowe:

```bash
Result
----------------
aci-helloworld
```

Poniższy przykład wyświetla listę tagów w repozytorium **aci-helloworld**.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-helloworld --output table
```

Dane wyjściowe:

```bash
Result
--------
v1
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy grupa zasobów, wystąpienie usługi ACR i wszystkie obrazy kontenerów nie będą już potrzebne, można je usunąć za pomocą polecenia [az group delete][az-group-delete].

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku Szybki start utworzono usługę Azure Container Registry za pomocą interfejsu wiersza polecenia platformy Azure. Jeśli chcesz używać usługi Azure Container Registry z usługą Azure Container Instances, przejdź do samouczka dotyczącego usługi Azure Container Instances.

> [!div class="nextstepaction"]
> [Samouczek dotyczący usługi Azure Container Instances][container-instances-tutorial-prepare-app]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az_acr_create
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-group-create]: /cli/azure/group#az_group_create
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli]: /cli/azure/install-azure-cli
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md