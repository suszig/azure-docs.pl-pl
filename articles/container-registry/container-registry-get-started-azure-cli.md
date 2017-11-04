---
title: "Szybki Start — tworzenie prywatnych rejestru Docker na platformie Azure z wiersza polecenia platformy Azure"
description: "Dowiedz się szybko utworzyć prywatnego rejestru kontenera Docker z wiersza polecenia platformy Azure."
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: tysonn
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/16/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6b3fb9a3ea090f0083e8f113ddf13312fe42b59a
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2017
---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Tworzenie rejestru kontenera za pomocą interfejsu wiersza polecenia platformy Azure

Usługa Azure Container Registry to zarządzana usługa rejestru kontenerów platformy Docker używana do przechowywania prywatnych obrazów kontenerów Docker. Szczegóły ten Przewodnik tworzenia wystąpienia rejestru kontenera Azure za pomocą wiersza polecenia platformy Azure.

Ta opcja szybkiego startu wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.20 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli).

Musi mieć również Docker zainstalowane lokalnie. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) lub [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi.

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Tworzenie rejestru kontenerów

W tym szybkiego startu, utworzymy *podstawowe* rejestru. Rejestru kontenera platformy Azure jest dostępna w kilku różne jednostki magazynowe, krótko opisane w poniższej tabeli. Rozszerzone szczegółowe na każdym, patrz [rejestru kontenera jednostek SKU](container-registry-skus.md).

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Utwórz wystąpienie usługi ACR za pomocą polecenia [az acr create](/cli/azure/acr#create).

Nazwa rejestru **muszą być unikatowe**. W poniższym przykładzie *myContainerRegistry007* jest używany. Zaktualizuj to unikatowe wartości.

```azurecli
az acr create --name myContainerRegistry007 --resource-group myResourceGroup --sku Basic
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
  "storageAccount": {
    "name": "mycontainerregistr223140"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

W dalszej części tego przewodnika Szybki Start, używamy `<acrname>` jako nazwę rejestru kontenera.

## <a name="log-in-to-acr"></a>Zaloguj się do awaryjnego

Przed wypychaniem i ściąganiem obrazów kontenerów musisz zalogować się do wystąpienia usługi ACR. Aby to zrobić, użyj polecenia [az acr login](/cli/azure/acr#login).

```azurecli
az acr login --name <acrname>
```

Polecenie zwraca komunikat „Logowanie pomyślne” po ukończeniu.

## <a name="push-image-to-acr"></a>Obraz wypychania do awaryjnego

Aby wypchnąć obrazu do rejestru kontenera platformy Azure, najpierw musi mieć obraz. W razie potrzeby, uruchom następujące polecenie, aby ściąganie wstępnie utworzony obraz z Centrum Docker.

```bash
docker pull microsoft/aci-helloworld
```

Obraz musi być oznaczane nazwa ACR logowania serwera. Uruchom następujące polecenie, aby zwrócić nazwę logowania serwera wystąpienia ACR.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Tag przy użyciu obrazu [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) polecenia. Zastąp  *<acrLoginServer>*  z nazwą serwera logowania wystąpienia ACR.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Na koniec użyj [wypychania docker](https://docs.docker.com/engine/reference/commandline/push/) do dystrybuowania obrazu do wystąpienia ACR. Zastąp  *<acrLoginServer>*  z nazwą serwera logowania wystąpienia ACR.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>Tworzenie listy obrazów kontenerów

Poniższy przykład zawiera repozytoria w rejestrze:

```azurecli
az acr repository list -n <acrname> -o table
```

Dane wyjściowe:

```bash
Result
----------------
aci-helloworld
```

Poniższy przykład zawiera tagi na **aci helloworld** repozytorium.

```azurecli
az acr repository show-tags -n <acrname> --repository aci-helloworld -o table
```

Dane wyjściowe:

```bash
Result
--------
v1
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie są już potrzebne, można użyć [usunięcie grupy az](/cli/azure/group#delete) polecenie, aby usunąć grupę zasobów, wystąpienie ACR i wszystkie obrazy kontenera.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

Ta opcja szybkiego startu została utworzona rejestru kontenera platformy Azure z wiersza polecenia platformy Azure. Jeśli chcesz rejestru kontenera Azure za pomocą wystąpień kontenera platformy Azure, przejdź do samouczka wystąpień kontenera platformy Azure.

> [!div class="nextstepaction"]
> [Samouczek wystąpień kontenera platformy Azure](../container-instances/container-instances-tutorial-prepare-app.md)