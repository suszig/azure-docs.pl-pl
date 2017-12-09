---
title: "Samouczek usługi kontenera platformy Azure — wdrażanie aplikacji"
description: "Samouczek usługi kontenera platformy Azure — wdrażanie aplikacji"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c763d6867deb76f5b9d197c7062ee07d7ed6d865
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="run-applications-in-kubernetes"></a>Uruchamianie aplikacji w Kubernetes

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

W tym samouczku część cztery siedmiu, przykładowa aplikacja jest wdrażana w klastrze Kubernetes. Ukończono kroki obejmują:

> [!div class="checklist"]
> * Pliki manifestu Kubernetes aktualizacji
> * Uruchom aplikację w Kubernetes
> * Testowanie aplikacji

W kolejnych samouczkach tej aplikacji jest skalowana, aktualizacji, oraz Operations Management Suite jest skonfigurowana do monitorowania Kubernetes klastra.

Ten samouczek zakłada podstawową wiedzę na temat pojęć Kubernetes, aby uzyskać szczegółowe informacje o Zobacz Kubernetes [dokumentacji Kubernetes](https://kubernetes.io/docs/home/).

## <a name="before-you-begin"></a>Przed rozpoczęciem

W poprzednim samouczki aplikacji zostało umieszczone w obrazie kontenera, ten obraz został załadowany w rejestrze kontenera Azure i klaster Kubernetes został utworzony. 

Do ukończenia tego samouczka, należy wstępnie utworzone `azure-vote-all-in-one-redis.yml` Kubernetes pliku manifestu. Ten plik został pobrany z kodu źródłowego aplikacji w poprzednim samouczka. Sprawdź, czy zostały sklonowane repozytorium i że zostały zmienione katalogi do sklonowanego repozytorium.

Jeśli nie zostało wykonane następujące kroki, a następnie zostać z niego skorzystać, wróć do [samouczek 1 — Tworzenie kontenera obrazów](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="update-manifest-file"></a>Aktualizacja pliku manifestu

W tym samouczku rejestru kontenera platformy Azure (ACR) został użyty do przechowywania obrazu kontenera. Przed uruchomieniem aplikacji, nazwa ACR logowania serwera musi zostać zaktualizowany w pliku manifestu Kubernetes.

Pobierz nazwę serwera ACR logowania z [listy acr az](/cli/azure/acr#list) polecenia.

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Plik manifestu został wstępnie utworzone przy użyciu nazwy serwera logowania `microsoft`. Otwórz plik w dowolnym edytorze tekstu. W tym przykładzie plik zostanie otwarty z `vi`.

```bash
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

```azurecli-interactive
kubectl create -f azure-vote-all-in-one-redis.yml
```

Dane wyjściowe:

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Testowanie aplikacji

A [usługi Kubernetes](https://kubernetes.io/docs/concepts/services-networking/service/) jest tworzony, który udostępnia aplikacji w Internecie. Może to potrwać kilka minut. 

Aby monitorować postęp, użyj polecenia [kubectl get-service](https://review.docs.microsoft.com/azure/container-service/container-service-kubernetes-walkthrough?branch=pr-en-us-17681) z argumentem `--watch`.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Początkowo **IP zewnętrznego** dla `azure-vote-front` usługi pojawia się jako `pending`. Gdy adres IP zewnętrznego zmienił się z `pending` do `IP address`, użyj `CTRL-C` można zatrzymać procesu czujki kubectl.

```bash
NAME               CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   10.0.42.158   <pending>     80:31873/TCP   1m
azure-vote-front   10.0.42.158   52.179.23.131 80:31873/TCP   2m
```

Aby wyświetlić aplikację, przejdź do zewnętrznego adresu IP.

![Obraz przedstawiający klaster Kubernetes na platformie Azure](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Następne kroki

W tym samouczku aplikacji Azure głos został wdrożony do klastra Kubernetes usługi kontenera platformy Azure. Zadania ukończone obejmują:  

> [!div class="checklist"]
> * Pobierz pliki manifestu Kubernetes
> * Uruchom aplikację w Kubernetes
> * Przetestowane aplikacji

Przejdź do następnego samouczka, aby dowiedzieć się więcej na temat skalowania aplikacji Kubernetes jak podstawowej infrastruktury Kubernetes. 

> [!div class="nextstepaction"]
> [Skala Kubernetes aplikacji i infrastruktury](./container-service-tutorial-kubernetes-scale.md)