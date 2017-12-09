---
title: "Szybki Start — tworzenie prywatnych rejestru Docker na platformie Azure z wiersza polecenia platformy Azure"
description: "Dowiedz się szybko utworzyć prywatnego rejestru kontenera Docker z wiersza polecenia platformy Azure."
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quicksart
ms.date: 12/07/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: f31f4e5e2b3fe5db85873894a7f2fa9c415392c1
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Tworzenie rejestru kontenera za pomocą interfejsu wiersza polecenia platformy Azure

Usługa Azure Container Registry to zarządzana usługa rejestru kontenerów platformy Docker używana do przechowywania prywatnych obrazów kontenerów Docker. Szczegóły ten Przewodnik tworzenia wystąpienia rejestru kontenera Azure za pomocą wiersza polecenia platformy Azure.

Ta opcja szybkiego startu wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.21 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli musisz zainstalować lub uaktualnić, zobacz [zainstalować Azure CLI 2.0][azure-cli].

Musi mieć również Docker zainstalowane lokalnie. Docker zawiera pakiety, które łatwo skonfigurować Docker na dowolnym [Mac][docker-mac], [Windows][docker-windows], lub [Linux] [ docker-linux] systemu.

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create][az-group-create]. Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi.

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Tworzenie rejestru kontenerów

W tym szybkiego startu, utworzymy *podstawowe* rejestru. Rejestru kontenera platformy Azure jest dostępna w kilku różne jednostki magazynowe, krótko opisane w poniższej tabeli. Rozszerzone szczegółowe na każdym, patrz [rejestru kontenera jednostek SKU][container-registry-skus].

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Tworzenie wystąpienia ACR przy użyciu [utworzyć az acr] [ az-acr-create] polecenia.

Nazwa rejestru **muszą być unikatowe**. W poniższym przykładzie *myContainerRegistry007* jest używany. Zaktualizuj to unikatowe wartości.

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

W dalszej części tego przewodnika Szybki Start, używamy `<acrName>` jako nazwę rejestru kontenera.

## <a name="log-in-to-acr"></a>Zaloguj się do awaryjnego

Przed wypychaniem i ściąganiem obrazów kontenerów musisz zalogować się do wystąpienia usługi ACR. Aby to zrobić, użyj [logowania acr az] [ az-acr-login] polecenia.

```azurecli
az acr login --name <acrName>
```

Polecenie zwraca `Login Succeeded` komunikat po ukończeniu.

## <a name="push-image-to-acr"></a>Obraz wypychania do awaryjnego

Aby wypchnąć obrazu do rejestru kontenera platformy Azure, najpierw musi mieć obraz. Jeśli nie masz jeszcze żadnych obrazów kontenera lokalnym, uruchom następujące polecenie na ściąganie istniejącego obrazu z Centrum Docker.

```bash
docker pull microsoft/aci-helloworld
```

Zanim można wypchnąć obrazu do rejestru, musi ona tagu z w pełni kwalifikowana nazwa ACR logowania serwera. Uruchom następujące polecenie, aby uzyskać nazwę serwera pełnego logowania wystąpienia ACR.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Tag przy użyciu obrazu [docker tag] [ docker-tag] polecenia. Zastąp `<acrLoginServer>` z nazwą serwera logowania wystąpienia ACR.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Na koniec użyj [wypychania docker] [ docker-push] do dystrybuowania obrazu do wystąpienia ACR. Zastąp `<acrLoginServer>` z nazwą serwera logowania wystąpienia ACR.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>Tworzenie listy obrazów kontenerów

Poniższy przykład zawiera repozytoria w rejestrze:

```azurecli
az acr repository list --name <acrName> --output table
```

Dane wyjściowe:

```bash
Result
----------------
aci-helloworld
```

Poniższy przykład zawiera tagi na **aci helloworld** repozytorium.

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

Gdy nie są już potrzebne, można użyć [usunięcie grupy az] [ az-group-delete] polecenie, aby usunąć grupę zasobów, wystąpienie ACR i wszystkie obrazy kontenera.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

Ta opcja szybkiego startu została utworzona rejestru kontenera platformy Azure z wiersza polecenia platformy Azure. Jeśli chcesz rejestru kontenera Azure za pomocą wystąpień kontenera platformy Azure, przejdź do samouczka wystąpień kontenera platformy Azure.

> [!div class="nextstepaction"]
> [Samouczek wystąpień kontenera platformy Azure][container-instances-tutorial-prepare-app]

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