---
title: "Kubernetes na Azure samouczek — aktualizacja aplikacji"
description: "Samouczek AKS — aktualizacja aplikacji"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 95c609ab49fe478eda48b2a2eca6a772d1356d18
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="update-an-application-in-azure-container-service-aks"></a>Aktualizuj aplikację w usługi kontenera platformy Azure (AKS)

Po wdrożeniu aplikacji w Kubernetes, może zostać zaktualizowana, podając nowy obraz kontenera lub wersję obrazu. W takiej aktualizacji są przygotowywane tak, aby jednocześnie jest aktualizowany tylko część wdrożenia. Ta aktualizacja przemieszczanego umożliwia kontynuowanie działania podczas aktualizacji aplikacji. Udostępnia również mechanizm wycofywania, w przypadku niepowodzenia wdrożenia. 

W tym samouczku, część 6, 8 przykładową aplikację Azure głos jest aktualizowany. Zadania, które należy wykonać obejmują:

> [!div class="checklist"]
> * Aktualizowanie kodu aplikacji frontonu
> * Tworzenie obrazu zaktualizowane kontenera
> * Wypychanie obrazu kontenera w rejestrze kontenera platformy Azure
> * Wdrażanie obrazu zaktualizowane kontenera

W kolejnych samouczkach Operations Management Suite jest skonfigurowany do monitorowania Kubernetes klastra.

## <a name="before-you-begin"></a>Przed rozpoczęciem

W samouczkach poprzedniej aplikacji zostało umieszczone w obraz kontenera, obrazu przesłanego do rejestru kontenera Azure i utworzyć klaster Kubernetes. W klastrze Kubernetes następnie uruchomienia aplikacji. 

Aplikacja również sklonowano repozytorium w tym kodu źródłowego aplikacji i utworzone wcześniej plik rozwiązania Docker Compose używany w tym samouczku. Sprawdź utworzono klonowania repozytorium i że zostały zmienione katalogów w katalogu sklonowany. Wewnątrz jest katalog o nazwie `azure-vote` i plik o nazwie `docker-compose.yml`.

Jeśli nie zostały wykonane następujące kroki, a aby z niego skorzystać, wróć do [samouczek 1 — Tworzenie kontenera obrazów](./tutorial-kubernetes-prepare-app.md). 

## <a name="update-application"></a>Aktualizowanie aplikacji

W tym samouczku zostaną zmienione do aplikacji i zaktualizowane aplikacji wdrożonych w klastrze Kubernetes. 

Kod źródłowy aplikacji znajduje się wewnątrz `azure-vote` katalogu. Otwórz `config_file.cfg` pliku w dowolnym edytorze kodu i tekstu. W tym przykładzie `vi` jest używany.

```console
vi azure-vote/azure-vote/config_file.cfg
```

Zmień wartości `VOTE1VALUE` i `VOTE2VALUE`, a następnie zapisz plik.

```console
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Zapisz i zamknij plik.

## <a name="update-container-image"></a>Aktualizacja kontenera obrazu

Użyj [rozwiązania docker compose](https://docs.docker.com/compose/) odtworzenia frontonu obrazu i uruchomić aplikację zaktualizowane. `--build` Argument jest używany w celu poinstruowania rozwiązania Docker Compose ponownie utworzyć obraz aplikacji.

```console
docker-compose up --build -d
```

## <a name="test-application-locally"></a>Testowanie aplikacji lokalnie

Przejdź do adresem http://localhost: 8080, aby wyświetlić zaktualizowaną aplikację.

![Obraz przedstawiający klaster Kubernetes na platformie Azure](media/container-service-kubernetes-tutorials/vote-app-updated.png)

## <a name="tag-and-push-images"></a>Obrazy tagu i wypychania

Tag `azure-vote-front` obrazu o loginServer rejestru kontenera. 

Pobierz nazwę logowania serwera z [listy acr az](/cli/azure/acr#list) polecenia.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Użyj [docker tag](https://docs.docker.com/engine/reference/commandline/tag/) do tagu obrazu. Zastąp `<acrLoginServer>` z nazwą serwera logowania rejestru kontenera platformy Azure lub rejestru publicznej nazwy hosta. Również powiadomienia, który korzysta ze zaktualizowanej wersji obrazu `redis-v2`.

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v2
```

Użyj [wypychania docker](https://docs.docker.com/engine/reference/commandline/push/) do przekazania obrazu do rejestru. Zastąp `<acrLoginServer>` z rejestru kontenera Azure nazwy logowania serwera.

```console
docker push <acrLoginServer>/azure-vote-front:redis-v2
```

## <a name="deploy-update-application"></a>Wdrażanie aktualizacji aplikacji

Aby zapewnić maksymalny czas działania, wiele wystąpień pod aplikacji musi być uruchomiona. Sprawdź konfigurację tego z [kubectl Pobierz pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) polecenia.

```
kubectl get pod
```

Dane wyjściowe:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-217588096-5w632    1/1       Running   0          10m
azure-vote-front-233282510-b5pkz   1/1       Running   0          10m
azure-vote-front-233282510-dhrtr   1/1       Running   0          10m
azure-vote-front-233282510-pqbfk   1/1       Running   0          10m
```

Jeśli nie masz wiele stanowiskami systemem azure głos początku obrazu, skalowanie `azure-vote-front` wdrożenia.


```azurecli
kubectl scale --replicas=3 deployment/azure-vote-front
```

Aby zaktualizować aplikację, należy użyć [zestaw kubectl](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#set) polecenia. Aktualizacja `<acrLoginServer>` nazwą logowania serwera lub hosta rejestru kontenera.

```azurecli
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:redis-v2
```

Aby monitorować wdrożenia, należy użyć [kubectl Pobierz pod](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) polecenia. Zaktualizowano aplikacja jest wdrażana, Twoje stanowiskami są zakończone i utworzony ponownie z nowego obrazu kontenera.

```azurecli
kubectl get pod
```

Dane wyjściowe:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2978095810-gq9g0   1/1       Running   0          5m
azure-vote-front-1297194256-tpjlg   1/1       Running   0         1m
azure-vote-front-1297194256-tptnx   1/1       Running   0         5m
azure-vote-front-1297194256-zktw9   1/1       Terminating   0         1m
```

## <a name="test-updated-application"></a>Zaktualizowano test aplikacji

Uzyskaj adres IP zewnętrznego `azure-vote-front` usługi.

```azurecli
kubectl get service azure-vote-front
```

Przejdź do adresu IP, aby wyświetlić zaktualizowaną aplikację.

![Obraz przedstawiający klaster Kubernetes na platformie Azure](media/container-service-kubernetes-tutorials/vote-app-updated-external.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku aktualizacji aplikacji i wprowadzanie tej aktualizacji do klastra Kubernetes. Zakończono następujące zadania:

> [!div class="checklist"]
> * Zaktualizowany kod aplikacji frontonu
> * Utworzony obraz zaktualizowane kontenera
> * Wypychana obrazu kontenera do rejestru kontenera platformy Azure
> * Po wdrożeniu aplikacji zaktualizowane

Przejdź do następnego samouczka, aby dowiedzieć się więcej o sposobie monitorowania Kubernetes w usłudze Operations Management Suite.

> [!div class="nextstepaction"]
> [Monitorowanie rozwiązania Kubernetes za pomocą usługi Log Analytics](./tutorial-kubernetes-monitor.md)