---
title: "Samouczek wystąpień kontenera platformy Azure — wdrażanie aplikacji"
description: "Samouczek usługi Azure wystąpień kontenera część 3 z 3 - wdrażanie aplikacji"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 471caa1b24dc7017c70782c072b2068f9635244b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Wdrażanie kontenera do wystąpień kontenera platformy Azure

To jest ostatnim samouczek w serii trzech części. Wcześniej w serii, [utworzono obrazów kontenera](container-instances-tutorial-prepare-app.md) i [do rejestru kontenera Azure](container-instances-tutorial-prepare-acr.md). W tym artykule zakończeniu samouczka serii przez wdrożenie kontenera do wystąpień kontenera platformy Azure.

W tym samouczku zostały wykonane następujące czynności:

> [!div class="checklist"]
> * Wdrażanie kontenera z rejestru kontenera Azure za pomocą wiersza polecenia platformy Azure
> * Wyświetl aplikację w przeglądarce
> * Sprawdź dzienniki z kontenera

## <a name="before-you-begin"></a>Przed rozpoczęciem

Ten samouczek wymaga używasz interfejsu wiersza polecenia Azure w wersji 2.0.23 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli musisz zainstalować lub uaktualnić, zobacz [zainstalować Azure CLI 2.0][azure-cli-install].

Do ukończenia tego samouczka, należy lokalnie zainstalować Środowisko deweloperskie Docker. Docker zawiera pakiety, które łatwo skonfigurować Docker na dowolnym [Mac][docker-mac], [Windows][docker-windows], lub [Linux] [ docker-linux] systemu.

Azure powłoki chmury nie zawiera składniki Docker wymagane do ukończenia każdego kroku w tym samouczku. Środowisko projektowe wiersza polecenia platformy Azure i klastrem Docker należy zainstalować na komputerze lokalnym do ukończenia tego samouczka.

## <a name="deploy-the-container-using-the-azure-cli"></a>Wdrażanie kontenera przy użyciu wiersza polecenia platformy Azure

Interfejsu wiersza polecenia Azure umożliwia wdrożenie kontenera do wystąpień kontenera Azure za pomocą jednego polecenia. Ponieważ obraz kontenera znajduje się w rejestrze prywatnej kontenera platformy Azure, musi zawierać poświadczenia wymagane do niego dostęp. Uzyskaj poświadczenia za pomocą następujących poleceń wiersza polecenia platformy Azure.

Kontener rejestru logowania serwera (aktualizacja nazwą rejestru):

```azurecli
az acr show --name <acrName> --query loginServer
```

Kontener rejestru hasło:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Aby wdrożyć obraz kontenera z rejestru kontenera żądanie zasobu 1 rdzeń procesora CPU i 1 GB pamięci, uruchom następujące polecenie. Zastąp `<acrLoginServer>` i `<acrPassword>` z wartościami uzyskane z poprzednich dwóch poleceń.

```azurecli
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public --ports 80
```

W ciągu kilku sekund powinien zostać wyświetlony początkową odpowiedź z usługi Azure Resource Manager. Aby wyświetlić stan wdrożenia, użyj [Pokaż kontenera az][az-container-show]:

```azurecli
az container show --resource-group myResourceGroup --name aci-tutorial-app --query instanceView.state
```

Powtórz [Pokaż kontenera az] [ az-container-show] poleceń, aż stan zmieni się z *oczekujące* do *systemem*, powinien zająć w kilka minut. Gdy jest kontenera *systemem*, przejdź do następnego kroku.

## <a name="view-the-application-and-container-logs"></a>Wyświetl dzienniki aplikacji i kontenera

Po pomyślnym wdrożeniu, wyświetlania kontenera na publiczny adres IP z [Pokaż kontenera az] [ az-container-show] polecenia:

```bash
az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.ip
```

Przykład danych wyjściowych:`"13.88.176.27"`

Aby wyświetlić działającej aplikacji, przejdź do publicznego adresu IP w ulubionej przeglądarce.

![Witaj świecie aplikacji w przeglądarce][aci-app-browser]

Można również wyświetlić dziennik wyjściowy kontenera:

```azurecli
az container logs --resource-group myResourceGroup --name aci-tutorial-app
```

Dane wyjściowe:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie ma potrzeby zasobów utworzone w tym samouczku, można wykonywać [usunięcie grupy az] [ az-group-delete] polecenie, aby usunąć grupę zasobów i wszystkie zasoby, które zawiera. To polecenie usuwa rejestr kontenera, który został utworzony, a także uruchomiona kontenera i wszystkie powiązane zasoby.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku wykonaniu procesu wdrażania kontenerów do wystąpień kontenera platformy Azure. Wykonano następujące czynności:

> [!div class="checklist"]
> * Wdrożony kontener z rejestru kontenera Azure za pomocą wiersza polecenia platformy Azure
> * Wyświetlanie aplikacji w przeglądarce
> * Wyświetlanie dzienników kontenera

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-container-show]: /cli/azure/container#az_container_show
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli-install]: /cli/azure/install-azure-cli
[prepare-app]: ./container-instances-tutorial-prepare-app.md