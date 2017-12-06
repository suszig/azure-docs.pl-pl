---
title: "Samouczek usługi kontenera platformy Azure — przygotowanie aplikacji"
description: "Samouczek usługi kontenera platformy Azure — przygotowanie aplikacji"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3dcfc0d454926d6040692b0e8a9d44b13e7603c5
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="create-container-images-to-be-used-with-azure-container-service"></a>Tworzenie kontenera obrazów do użycia z usługą kontenera Azure

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

W tym samouczku, część 7, pierwsza aplikacji kontenera wielu jest gotowy do użycia w Kubernetes. Ukończono kroki obejmują:  

> [!div class="checklist"]
> * Klonowanie źródła aplikacji z usługi GitHub  
> * Tworzenie obrazu kontenera ze źródła aplikacji
> * Testowanie aplikacji w środowisku lokalnym Docker

Po wykonaniu następującej aplikacji jest dostępny w środowisku projektowania lokalnego.

![Obraz przedstawiający klaster Kubernetes na platformie Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

W kolejnych samouczkach obrazu kontenera jest przekazywany do rejestru kontenera platformy Azure, a następnie uruchom na platformie Azure hostowanej Kubernetes klastra.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Ten samouczek zakłada, że masz podstawową wiedzę na temat bazowych koncepcji usługi Docker, takich jak kontenery, obrazy kontenerów i podstawowe polecenia usługi Docker. W razie potrzeby zapoznaj się z tematem [Get starter with Docker (Rozpoczynanie pracy z platformą Docker)]( https://docs.docker.com/get-started/), aby uzyskać podstawowe informacje na temat kontenerów. 

Do ukończenia tego samouczka konieczne będzie środowisko programowania Docker. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) lub [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

Azure powłoki chmury nie zawiera składniki Docker wymagane do ukończenia każdego kroku w tym samouczku. Dlatego zaleca się używanie pełnego środowiska projektowania Docker.

## <a name="get-application-code"></a>Pobieranie kodu aplikacji

Przykładowa aplikacja używana w tym samouczku jest podstawowa aplikacja głosu. Aplikacja składa się z składników frontonu sieci web oraz wystąpienia pamięci podręcznej Redis zaplecza. Części sieci web jest dostarczana w obraz niestandardowy kontenera. Wystąpienie pamięci podręcznej Redis używa niezmodyfikowanego obrazu z Centrum Docker.  

Użyj git, aby pobrać kopię aplikacji w środowisku deweloperskim.

```bash
git clone https://github.com/Azure-Samples/azure-voting-app-redis.git
```

Zmień katalogi, dzięki czemu użytkownik pracuje z katalogu sklonowany.

```
cd azure-voting-app-redis
```

W tym katalogu jest kodu źródłowego aplikacji, wstępnie utworzone rozwiązania Docker compose plików i Kubernetes pliku manifestu. Te pliki są używane w całym zestawie samouczka. 

## <a name="create-container-images"></a>Tworzenie kontenera obrazów

[Rozwiązania docker Compose](https://docs.docker.com/compose/) może służyć do automatyzowania kompilacji poza kontener obrazów i wdrożenia usługi kontenera aplikacji.

Uruchom `docker-compose.yml` plik, aby utworzyć obraz kontenera, pobranie obrazu do pamięci podręcznej Redis i uruchomić aplikację.

```bash
docker-compose up -d
```

Po zakończeniu użyj [obrazy usługi docker](https://docs.docker.com/engine/reference/commandline/images/) polecenie, aby wyświetlić utworzony obrazów.

```bash
docker images
```

Zwróć uwagę, że zostały pobrane lub utworzone trzy obrazy. `azure-vote-front` Obraz zawiera aplikację i używa `nginx-flask` obrazu jako podstawy. `redis` Obrazu są używane do uruchamiania wystąpienia pamięci podręcznej Redis.

```bash
REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Uruchom [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) polecenie, aby wyświetlić uruchomionych kontenerów.

```bash
docker ps
```

Dane wyjściowe:

```bash
CONTAINER ID        IMAGE             COMMAND                  CREATED             STATUS              PORTS                           NAMES
82411933e8f9        azure-vote-front  "/usr/bin/supervisord"   57 seconds ago      Up 30 seconds       443/tcp, 0.0.0.0:8080->80/tcp   azure-vote-front
b68fed4b66b6        redis             "docker-entrypoint..."   57 seconds ago      Up 30 seconds       0.0.0.0:6379->6379/tcp          azure-vote-back
```

## <a name="test-application-locally"></a>Testowanie aplikacji lokalnie

Przejdź do adresem http://localhost: 8080, aby zobaczyć działającej aplikacji.

![Obraz przedstawiający klaster Kubernetes na platformie Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Teraz, funkcjonalność aplikacji została zweryfikowana, uruchomionych kontenerów można zatrzymać i usunięte. Nie należy usuwać obrazy kontenera. `azure-vote-front` Obraz jest przekazywany do wystąpienia rejestru kontenera platformy Azure w następnym samouczku.

Uruchom następujące polecenie, aby zatrzymać uruchomionych kontenerów.

```bash
docker-compose stop
```

Usuń zatrzymane kontenery i zasobów przy użyciu następującego polecenia.

```bash
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
> [Wypychanie obrazów do usługi Azure Container Registry](./container-service-tutorial-kubernetes-prepare-acr.md)
