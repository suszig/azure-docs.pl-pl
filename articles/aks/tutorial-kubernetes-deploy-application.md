---
title: "Kubernetes na Azure samouczek — wdrażanie aplikacji | Dokumentacja firmy Microsoft"
description: "Samouczek AKS — wdrażanie aplikacji"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: "Docker, kontenery, mikrousługi, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 73b943709fb3846058d4b1c09bc76b09460855ed
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="run-applications-in-azure-container-service-aks"></a>Uruchamianie aplikacji w usługi kontenera platformy Azure (AKS)

W tym samouczku część cztery osiem, przykładowa aplikacja jest wdrażana w klastrze Kubernetes. Ukończono kroki obejmują:

> [!div class="checklist"]
> * Pliki manifestu Kubernetes aktualizacji
> * Uruchom aplikację w Kubernetes
> * Testowanie aplikacji

W kolejnych samouczkach tej aplikacji jest skalowana, aktualizacji, oraz Operations Management Suite jest skonfigurowana do monitorowania Kubernetes klastra.

Ten samouczek zakłada podstawową wiedzę na temat pojęć Kubernetes, aby uzyskać szczegółowe informacje o Zobacz Kubernetes [dokumentacji Kubernetes](https://kubernetes.io/docs/home/).

## <a name="before-you-begin"></a>Przed rozpoczęciem

W poprzednim samouczki aplikacji zostało umieszczone w obrazie kontenera, ten obraz został załadowany w rejestrze kontenera Azure i klaster Kubernetes został utworzony. 

Do ukończenia tego samouczka, należy wstępnie utworzone `azure-vote-all-in-one-redis.yml` Kubernetes pliku manifestu. Ten plik został pobrany z kodu źródłowego aplikacji w poprzednim samouczka. Sprawdź, czy zostały sklonowane repozytorium i że zostały zmienione katalogi do sklonowanego repozytorium.

Jeśli nie zostało wykonane następujące kroki, a następnie zostać z niego skorzystać, wróć do [samouczek 1 — Tworzenie kontenera obrazów](./tutorial-kubernetes-prepare-app.md). 

## <a name="update-manifest-file"></a>Aktualizacja pliku manifestu

W tym samouczku rejestru kontenera platformy Azure (ACR) został użyty do przechowywania obrazu kontenera. Przed uruchomieniem aplikacji, nazwa ACR logowania serwera musi zostać zaktualizowany w pliku manifestu Kubernetes.

Pobierz nazwę serwera ACR logowania z [listy acr az](/cli/azure/acr#list) polecenia.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Plik manifestu został wstępnie utworzone przy użyciu nazwy serwera logowania `microsoft`. Otwórz plik w dowolnym edytorze tekstu. W tym przykładzie plik zostanie otwarty z `vi`.

```console
vi azure-vote-all-in-one-redis.yml
```

Zastąp `microsoft` z nazwą serwera ACR logowania. Ta wartość zostanie znaleziona w wierszu **47** pliku manifestu.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

Zapisz i zamknij plik.

## <a name="deploy-application"></a>Wdrażanie aplikacji

Użyj polecenia [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create), aby uruchomić aplikację. To polecenie analizuje pliku manifestu i tworzenia zdefiniowanych obiektów Kubernetes.

```azurecli
kubectl create -f azure-vote-all-in-one-redis.yml
```

Dane wyjściowe:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Testowanie aplikacji

A [usługi Kubernetes](https://kubernetes.io/docs/concepts/services-networking/service/) jest tworzony, który udostępnia aplikacji w Internecie. Może to potrwać kilka minut. 

Aby monitorować postęp, użyj polecenia [kubectl get-service](https://kubernetes.io/docs/user-guide/kubectl/v1.7/#get) z argumentem `--watch`.

```azurecli
kubectl get service azure-vote-front --watch
```

Początkowo adres *EXTERNAL-IP* dla usługi *azure-vote-front* pojawia się jako *oczekujący*.
  
```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

Raz *IP zewnętrznego* adres zmienił się z *oczekujące* do *adres IP*, użyj `CTRL-C` można zatrzymać procesu czujki kubectl. 

```
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Aby wyświetlić aplikację, przejdź do zewnętrznego adresu IP.

![Obraz przedstawiający klaster Kubernetes na platformie Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku aplikacji Azure głos został wdrożony do klastra Kubernetes w AKS. Zadania ukończone obejmują:  

> [!div class="checklist"]
> * Pobierz pliki manifestu Kubernetes
> * Uruchom aplikację w Kubernetes
> * Przetestowane aplikacji

Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat skalowania aplikacji Kubernetes jak podstawowej infrastruktury Kubernetes. 

> [!div class="nextstepaction"]
> [Skala Kubernetes aplikacji i infrastruktury](./tutorial-kubernetes-scale.md)
