---
title: "Samouczek wystąpień kontenera platformy Azure — wdrażanie aplikacji"
description: "Samouczek wystąpień kontenera platformy Azure — wdrażanie aplikacji"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 3b651526f5ee3197e7d04accb6a87e2f10bf0791
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Wdrażanie kontenera do wystąpień kontenera platformy Azure

Jest to ostatni trzech części samouczka. W poprzednich sekcjach [utworzono obrazów kontenera](container-instances-tutorial-prepare-app.md) i [do rejestru kontenera Azure](container-instances-tutorial-prepare-acr.md). W tej sekcji zakończeniu samouczka przez wdrożenie kontenera do wystąpień kontenera platformy Azure. Ukończono kroki obejmują:

> [!div class="checklist"]
> * Wdrażanie kontenera z rejestru kontenera Azure za pomocą wiersza polecenia platformy Azure
> * Wyświetlanie aplikacji w przeglądarce
> * Wyświetlanie dzienników kontenera

## <a name="before-you-begin"></a>Przed rozpoczęciem

Ten samouczek wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.20 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0](/cli/azure/install-azure-cli).

Do ukończenia tego samouczka konieczne będzie środowisko programowania Docker. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) lub [Linux](https://docs.docker.com/engine/installation/#supported-platforms).

Azure powłoki chmury nie zawiera składniki Docker wymagane do ukończenia każdego kroku w tym samouczku. Dlatego zaleca się instalacji lokalnej środowiska deweloperskiego wiersza polecenia platformy Azure i klastrem Docker.

## <a name="deploy-the-container-using-the-azure-cli"></a>Wdrażanie kontenera przy użyciu wiersza polecenia platformy Azure

Interfejsu wiersza polecenia Azure umożliwia wdrożenie kontenera do wystąpień kontenera Azure za pomocą jednego polecenia. Ponieważ obraz kontenera znajduje się w rejestrze prywatnej kontenera platformy Azure, musi zawierać poświadczenia wymagane do niego dostęp. W razie potrzeby można je zapytanie, jak pokazano poniżej.

Kontener rejestru logowania serwera (aktualizacja nazwą rejestru):

```azurecli
az acr show --name <acrName> --query loginServer
```

Kontener rejestru hasło:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Aby wdrożyć obraz kontenera z rejestru kontenera żądanie zasobu 1 rdzeń procesora CPU i 1 GB pamięci, uruchom następujące polecenie:

```azurecli
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

W ciągu kilku sekund powinien zostać wyświetlony początkową odpowiedź z usługi Azure Resource Manager. Aby wyświetlić stan wdrożenia, należy użyć:

```azurecli
az container show --name aci-tutorial-app --resource-group myResourceGroup --query state
```

Można kontynuować, wykonanie tego polecenia, aż stan zmieni się z *oczekujące* do *systemem*. Następnie można przejść.

## <a name="view-the-application-and-container-logs"></a>Wyświetl dzienniki aplikacji i kontenera

Po pomyślnym wdrożeniu, otwórz przeglądarkę z adresem IP wyświetlany w danych wyjściowych z następującego polecenia:

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

```json
"13.88.176.27"
```

![Witaj świecie aplikacji w przeglądarce][aci-app-browser]

Można również wyświetlić dziennik wyjściowy kontenera:

```azurecli
az container logs --name aci-tutorial-app -g myResourceGroup
```

Dane wyjściowe:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku wykonaniu procesu wdrażania kontenerów do wystąpień kontenera platformy Azure. Wykonano następujące czynności:

> [!div class="checklist"]
> * Wdrażanie kontenera z rejestru kontenera Azure za pomocą wiersza polecenia platformy Azure
> * Wyświetlanie aplikacji w przeglądarce
> * Wyświetlanie dzienników kontenera

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
