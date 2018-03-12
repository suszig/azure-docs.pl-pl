---
title: "Samouczek dotyczący usługi Azure Container Instances — wdrażanie aplikacji"
description: "Samouczek dotyczący usługi Azure Container Instances, część 3 z 3 — wdrażanie aplikacji"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 0532d255b271b2155ae3115f8f96c4cbb53916e4
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Wdrażanie kontenera w usłudze Azure Container Instances

To jest ostatni samouczek z serii składającej się z trzech części. Wcześniej w tej serii [utworzono obraz kontenera](container-instances-tutorial-prepare-app.md) i [przekazano go do usługi Azure Container Registry](container-instances-tutorial-prepare-acr.md). Ten artykuł stanowi zakończenie serii samouczków i dotyczy wdrażania kontenera w usłudze Azure Container Instances.

W tym samouczku zostały wykonane następujące czynności:

> [!div class="checklist"]
> * Wdrażanie kontenera z usługi Azure Container Registry przy użyciu interfejsu wiersza polecenia platformy Azure
> * Wyświetlanie aplikacji w przeglądarce
> * Wyświetlanie dzienników kontenera

## <a name="before-you-begin"></a>Przed rozpoczęciem

Ten samouczek wymaga interfejsu wiersza polecenia platformy Azure w wersji 2.0.27 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0][azure-cli-install].

Do ukończenia tego samouczka konieczne będzie zainstalowane lokalnie środowisko deweloperskie platformy Docker. Środowisko Docker zawiera pakiety, które umożliwiają łatwe konfigurowanie platformy Docker w systemie [Mac][docker-mac], [Windows][docker-windows] lub [Linux][docker-linux].

Usługa Azure Cloud Shell nie zawiera składników platformy Docker wymaganych do ukończenia każdego kroku w tym samouczku. Aby ukończyć ten samouczek, należy zainstalować interfejs wiersza polecenia platformy Azure i środowisko deweloperskie platformy Docker na komputerze lokalnym.

## <a name="deploy-the-container-using-the-azure-cli"></a>Wdrażanie kontenera przy użyciu interfejsu wiersza polecenia platformy Azure

Interfejs wiersza polecenia platformy Azure umożliwia wdrożenie kontenera w usłudze Azure Container Instances za pomocą jednego polecenia. Ponieważ obraz kontenera jest hostowany w prywatnym rejestrze Azure Container Registry, musi zawierać poświadczenia wymagane do uzyskiwania dostępu do niego. Uzyskaj poświadczenia za pomocą poniższych poleceń interfejsu wiersza polecenia platformy Azure.

Serwer logowania rejestru kontenerów (zaktualizuj przy użyciu nazwy swojego rejestru):

```azurecli
az acr show --name <acrName> --query loginServer
```

Hasło rejestru kontenerów:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Aplikacja musi zostać [wcześniej przygotowana][prepare-app]. Aby wdrożyć obraz kontenera z rejestru kontenerów z żądaniem zasobu 1 rdzenia procesora i 1 GB pamięci, uruchom następujące polecenie: [az container create][az-container-create]. Zastąp elementy `<acrLoginServer>` i `<acrPassword>` wartościami uzyskanymi przy użyciu dwóch poprzednich poleceń. Zastąp element `<acrName>` nazwą rejestru kontenerów. Możesz również zastąpić element `aci-tutorial-app` nazwą, którą chcesz nadać nowej aplikacji.

```azurecli
az container create --resource-group myResourceGroup --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-username <acrName> --registry-password <acrPassword> --dns-name-label aci-demo --ports 80
```

W ciągu kilku sekund powinna pojawić się początkowa odpowiedź z usługi Azure Resource Manager. Wartość `--dns-name-label` musi być unikatowa w regionie platformy Azure, w którym tworzysz wystąpienia kontenera. Zaktualizuj wartość w poprzednim przykładzie, jeśli podczas wykonywania polecenia zostanie wyświetlony komunikat o błędzie **etykiety nazwy DNS**.

Aby wyświetlić stan wdrożenia, użyj polecenia [az container show][az-container-show]:

```azurecli
az container show --resource-group myResourceGroup --name aci-tutorial-app --query instanceView.state
```

Powtarzaj polecenie [az container show][az-container-show], aż stan zmieni się z *Oczekujące* na *Uruchomiono*. Powinno to potrwać krócej niż minutę. Jeśli kontener ma stan *Uruchomiono*, przejdź do następnego kroku.

## <a name="view-the-application-and-container-logs"></a>Wyświetlanie dzienników aplikacji i kontenerów

Po pomyślnym wdrożeniu wyświetl w pełni kwalifikowaną nazwę domeny (FQDN) kontenera przy użyciu polecenia [az container show][az-container-show]:

```bash
az container show --resource-group myResourceGroup --name aci-tutorial-app --query ipAddress.fqdn
```

Przykładowe dane wyjściowe: `"aci-demo.eastus.azurecontainer.io"`

Aby wyświetlić działającą aplikację, przejdź do wyświetlonej nazwy DNS w ulubionej przeglądarce:

![Aplikacja Hello World w przeglądarce][aci-app-browser]

Możesz również wyświetlić dane wyjściowe dziennika kontenera:

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

Jeśli nie potrzebujesz już zasobów utworzonych w tej serii samouczków, możesz wykonać polecenie [az group delete][az-group-delete], aby usunąć grupę zasobów i wszystkie należące do niej zasoby. To polecenie usuwa utworzony rejestr kontenerów, a także uruchomiony kontener i wszystkie powiązane zasoby.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku wykonaniu przeprowadzono proces wdrażania kontenerów w usłudze Azure Container Instances. Wykonano następujące czynności:

> [!div class="checklist"]
> * Wdrożono kontener z usługi Azure Container Registry przy użyciu interfejsu wiersza polecenia platformy Azure
> * Wyświetlono aplikację w przeglądarce
> * Wyświetlono dzienniki kontenera

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
[az-container-create]: /cli/azure/container#az_container_create
[az-container-show]: /cli/azure/container#az_container_show
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli-install]: /cli/azure/install-azure-cli
[prepare-app]: ./container-instances-tutorial-prepare-app.md
