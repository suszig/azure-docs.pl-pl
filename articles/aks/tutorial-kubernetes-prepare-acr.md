---
title: "Kubernertes na Azure samouczek — przygotowanie ACR | Dokumentacja firmy Microsoft"
description: "Samouczek AKS — przygotowanie ACR"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: "Docker, kontenery, mikrousługi, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3cf0007f09c40fbccf58fbaee32df7be9bb2fa1a
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Wdrażanie i użytkowanie rejestru kontenera platformy Azure

Rejestru kontenera platformy Azure (ACR) to Azure, prywatnego rejestru dla obrazów kontenera Docker. Ten samouczek, część dwa ośmiu przeprowadzi Cię przez wdrażanie wystąpienia rejestru kontenera Azure i wypychanie obrazu kontenera do niego. Ukończono kroki obejmują:

> [!div class="checklist"]
> * Wdrażanie wystąpienia rejestru kontenera platformy Azure (ACR)
> * Znakowanie obrazu kontener dla ACR
> * Przekazywanie obrazu na ACR

W kolejnych samouczkach tego wystąpienia ACR jest zintegrowana z klastrem Kubernetes w AKS. 

## <a name="before-you-begin"></a>Przed rozpoczęciem

W [poprzedniego samouczek](./tutorial-kubernetes-prepare-app.md), obrazu kontener został utworzony dla prostą aplikację Azure głosu. Jeśli nie utworzono obraz aplikacji Azure głosowania, wróć do [samouczek 1 — Tworzenie kontenera obrazów](./tutorial-kubernetes-prepare-app.md).

Ten samouczek wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.20 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure]( /cli/azure/install-azure-cli). 

## <a name="deploy-azure-container-registry"></a>Wdrażanie rejestru kontenera platformy Azure

W przypadku wdrażania rejestru kontenera platformy Azure, musisz najpierw grupę zasobów. Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi.

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). W tym przykładzie grupy zasobów o nazwie `myResourceGroup` jest tworzony w `westus2` regionu.

```azurecli
az group create --name myResourceGroup --location westus2
```

Tworzenie kontenera Azure rejestru [az acr utworzyć](/cli/azure/acr#create) polecenia. Nazwa rejestru kontenera **muszą być unikatowe**.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

W dalszej części tego samouczka, używamy `<acrname>` jako nazwę rejestru kontenera.

## <a name="container-registry-login"></a>Kontener rejestru logowania

Użyj [logowania acr az](https://docs.microsoft.com/en-us/cli/azure/acr#az_acr_login) polecenie, aby zalogować się do wystąpienia ACR. Musisz podać unikatową nazwę podane w rejestrze kontenera podczas jej tworzenia.

```azurecli
az acr login --name <acrName>
```

Polecenie zwraca komunikat "Pomyślnie logowania" po ukończeniu.

## <a name="tag-container-images"></a>Tag kontener obrazów

Aby wyświetlić listę bieżącego obrazów, użyj [obrazy usługi docker](https://docs.docker.com/engine/reference/commandline/images/) polecenia.

```console
docker images
```

Dane wyjściowe:

```
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

Obraz każdego kontenera musi być oznakowane o nazwie loginServer rejestru. Ten tag jest używane do przesyłania przypadku wypychania kontener obrazów w rejestrze obrazu.

Aby uzyskać nazwę loginServer, uruchom następujące polecenie.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Teraz, tag `azure-vote-front` obrazu o loginServer rejestru kontenera. Ponadto Dodaj `:redis-v1` na końcu nazwy obrazu. Znacznik określa wersję obrazu.

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

Po oznakowany, uruchom [obrazy usługi docker] (https://docs.docker.com/engine/reference/commandline/images/) aby zweryfikować działanie.

```console
docker images
```

Dane wyjściowe:

```
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   redis-v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a>Wypychanie obrazów do rejestru

Wypychanie `azure-vote-front` obrazu w rejestrze. 

Korzystając z następującego przykładu, Zamień nazwa ACR loginServer loginServer ze środowiska.

```console
docker push <acrLoginServer>/azure-vote-front:redis-v1
```

Trwa to kilka minut.

## <a name="list-images-in-registry"></a>Listy obrazów w rejestrze

Aby powrócić do listy obrazów, do których został przypisany do rejestru kontenera platformy Azure, użytkownik [listy repozytorium acr az](/cli/azure/acr/repository#list) polecenia. Zaktualizuj polecenia o nazwie ACR.

```azurecli
az acr repository list --name <acrName> --output table
```

Dane wyjściowe:

```azurecli
Result
----------------
azure-vote-front
```

A następnie wyświetlić tagi dla określonego obrazu, użyj [az acr repozytorium Pokaż znaczniki](/cli/azure/acr/repository#show-tags) polecenia.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Dane wyjściowe:

```azurecli
Result
--------
redis-v1
```

Po ukończeniu samouczka obrazu kontenera przechowywanych w prywatnej wystąpienia rejestru kontenera platformy Azure. Ten obraz jest rozmieszczany z ACR klastrowi Kubernetes w kolejnych samouczkach.

## <a name="next-steps"></a>Następne kroki

W tym samouczku rejestru kontenera Azure zostało przygotowane do użycia w klastrze AKS. Wykonano następujące czynności:

> [!div class="checklist"]
> * Wdrożone wystąpienie rejestru kontenera platformy Azure
> * Tagged image kontenera dla ACR
> * Przekazać obraz do awaryjnego

Przejdź do następnego samouczek, aby dowiedzieć się więcej o wdrażaniu klastra Kubernetes na platformie Azure.

> [!div class="nextstepaction"]
> [Wdrażanie klastra Kubernetes](./tutorial-kubernetes-deploy-cluster.md)