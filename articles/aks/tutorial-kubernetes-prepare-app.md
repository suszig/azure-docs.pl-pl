---
title: "Kubernetes na Azure samouczek — przygotowanie aplikacji | Dokumentacja firmy Microsoft"
description: "Samouczek AKS — przygotowanie aplikacji"
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
ms.openlocfilehash: 0048f9ab21c09860397989c41920562ebab2f561
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="prepare-application-for-azure-container-service-aks"></a>Przygotowanie aplikacji dla usługi kontenera platformy Azure (AKS)

W tym samouczku części ośmiu aplikacji kontenera wielu jest gotowy do użycia w Kubernetes. Ukończono kroki obejmują:  

> [!div class="checklist"]
> * Klonowanie źródła aplikacji z usługi GitHub  
> * Tworzenie obrazu kontenera ze źródła aplikacji
> * Testowanie aplikacji w środowisku lokalnym Docker

Po wykonaniu następującej aplikacji jest dostępny w środowisku projektowania lokalnego.

![Obraz przedstawiający klaster Kubernetes na platformie Azure](./media/container-service-tutorial-kubernetes-prepare-app/azure-vote.png)

W kolejnych samouczkach obrazu kontenera jest przekazywane do rejestru kontenera platformy Azure, a następnie uruchom AKS klastra.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Ten samouczek zakłada, że masz podstawową wiedzę na temat bazowych koncepcji usługi Docker, takich jak kontenery, obrazy kontenerów i podstawowe polecenia usługi Docker. W razie potrzeby zapoznaj się z tematem [Get starter with Docker (Rozpoczynanie pracy z platformą Docker)]( https://docs.docker.com/get-started/), aby uzyskać podstawowe informacje na temat kontenerów. 

Do ukończenia tego samouczka konieczne będzie środowisko programowania Docker. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) lub [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

Azure powłoki chmury nie zawiera składniki Docker wymagane do ukończenia każdego kroku w tym samouczku. Dlatego zaleca się używanie pełnego środowiska projektowania Docker.

## <a name="get-application-code"></a>Pobieranie kodu aplikacji

Przykładowa aplikacja używana w tym samouczku jest podstawowa aplikacja głosu. Aplikacja składa się z składników frontonu sieci web oraz wystąpienia pamięci podręcznej Redis zaplecza. Części sieci web jest dostarczana w obraz niestandardowy kontenera. Wystąpienie pamięci podręcznej Redis używa niezmodyfikowanego obrazu z Centrum Docker.  

Użyj git, aby pobrać kopię aplikacji w środowisku deweloperskim.

```console
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Zmień katalogi, dzięki czemu użytkownik pracuje z katalogu sklonowany.

```console
cd azure-voting-app-redis
```

W tym katalogu jest kodu źródłowego aplikacji, wstępnie utworzone rozwiązania Docker compose plików i Kubernetes pliku manifestu. Te pliki są używane w całym zestawie samouczka. 

## <a name="create-container-images"></a>Tworzenie kontenera obrazów

[Rozwiązania docker Compose](https://docs.docker.com/compose/) może służyć do automatyzowania kompilacji poza kontener obrazów i wdrożenia usługi kontenera aplikacji.

Uruchom `docker-compose.yml` plik, aby utworzyć obraz kontenera, pobranie obrazu do pamięci podręcznej Redis i uruchomić aplikację.

```console
docker-compose up -d
```

Po zakończeniu użyj [obrazy usługi docker](https://docs.docker.com/engine/reference/commandline/images/) polecenie, aby wyświetlić utworzony obrazów.

```console
docker images
```

Zwróć uwagę, że zostały pobrane lub utworzone trzy obrazy. `azure-vote-front` Obraz zawiera aplikację i używa `nginx-flask` obrazu jako podstawy. `redis` Obrazu są używane do uruchamiania wystąpienia pamięci podręcznej Redis.

```
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Uruchom [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) polecenie, aby wyświetlić uruchomionych kontenerów.

```console
docker ps
```

Dane wyjściowe:

```
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a>Testowanie aplikacji lokalnie

Przejdź do adresem http://localhost: 8080, aby zobaczyć działającej aplikacji.

![Obraz przedstawiający klaster Kubernetes na platformie Azure](./media/container-service-tutorial-kubernetes-prepare-app/azure-vote.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Teraz, funkcjonalność aplikacji została zweryfikowana, uruchomionych kontenerów można zatrzymać i usunięte. Nie należy usuwać obrazy kontenera. `azure-vote-front` Obraz jest przekazywany do wystąpienia rejestru kontenera platformy Azure w następnym samouczku.

Uruchom następujące polecenie, aby zatrzymać uruchomionych kontenerów.

```console
docker-compose stop
```

Usuń zatrzymane kontenery i zasobów przy użyciu następującego polecenia.

```console
docker-compose down
```

Po ukończeniu masz obraz kontenera, który zawiera aplikację Azure głos.

## <a name="next-steps"></a>Następne kroki

W tym samouczku przetestowano aplikacji i kontener obrazów utworzonych dla aplikacji. Wykonano następujące czynności:

> [!div class="checklist"]
> * Klonowanie źródła aplikacji z usługi GitHub  
> * Utworzony obraz kontenera ze źródła aplikacji
> * Przetestowany aplikacji w lokalnym środowisku Docker

Przejdź do kolejnego samouczka, aby dowiedzieć się więcej o przechowywaniu obrazów kontenera w usłudze Azure Container Registry.

> [!div class="nextstepaction"]
> [Wypychanie obrazów do usługi Azure Container Registry](./tutorial-kubernetes-prepare-acr.md)
