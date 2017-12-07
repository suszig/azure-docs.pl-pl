---
title: "Szybki start — Tworzenie pierwszego kontenera usługi Azure Container Instances"
description: "Wdrażaj i rozpocznij pracę z usługą Azure Container Instances"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: quickstart
ms.date: 11/29/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 22f7ec70b5fc9d04de18a8b6029f0e24d3f6a83f
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Tworzenie pierwszego kontenera w usłudze Azure Container Instances
Usługa Azure Container Instances ułatwia tworzenie kontenerów Docker na platformie Azure oraz zarządzanie nimi bez konieczności inicjowania obsługi maszyn wirtualnych czy adoptowania usługi wyższego poziomu. Podczas pracy z tym przewodnikiem Szybki start utworzysz kontener na platformie Azure i ujawnisz go w Internecie przy użyciu publicznego adresu IP. Ta operacja jest wykonywana za pomocą jednego polecenia. W ciągu kilku sekund w przeglądarce zobaczysz następujący wynik:

![Widziana w przeglądarce aplikacja wdrożona za pomocą usługi Azure Container Instances][aci-app-browser]

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Podczas pracy z tym przewodnikiem Szybki start możesz użyć usługi Azure Cloud Shell lub lokalnej instalacji interfejsu wiersza polecenia platformy Azure. Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przewodnik Szybki start będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.21 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Wystąpienia Azure Container Instances są zasobami platformy Azure i należy je umieścić w grupie zasobów platformy Azure, czyli logicznej kolekcji, w której zasoby platformy Azure są wdrażane i zarządzane.

Utwórz grupę zasobów za pomocą polecenia [az group create][az-group-create].

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Tworzenie kontenera

Kontener można utworzyć, podając nazwę obrazu usługi Docker i grupy zasobów platformy Azure w poleceniu [az container create][az-container-create]. Opcjonalnie można ujawnić kontener w Internecie za pomocą publicznego adresu IP. W tym przewodniku Szybki start wdrożysz kontener hostujący małą aplikację internetową napisaną w języku [Node.js](http://nodejs.org).

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public --ports 80
```

W ciągu kilku sekund powinna pojawić się odpowiedź na żądanie. Początkowo kontener ma stan **Tworzenie**, ale powinien zostać uruchomiony w ciągu kilku sekund. Stan możesz sprawdzić przy użyciu polecenia [az container show][az-container-show]:

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

W dolnej części danych wyjściowych zostanie wyświetlony stan aprowizowania kontenera i jego adres IP:

```json
...
"ipAddress": {
      "ip": "13.88.176.27",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

Po przejściu kontenera do stanu **Powodzenie** możesz uzyskiwać dostęp go niego w przeglądarce przy użyciu podanego adresu IP.

![Widziana w przeglądarce aplikacja wdrożona za pomocą usługi Azure Container Instances][aci-app-browser]

## <a name="pull-the-container-logs"></a>Ściąganie dzienników kontenera

Dzienniki dla utworzonego kontenera możesz ściągnąć przy użyciu polecenia [az container logs][az-container-logs]:

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

Dane wyjściowe:

```bash
listening on port 80
::ffff:10.240.255.107 - - [29/Nov/2017:20:48:50 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36"
::ffff:10.240.255.107 - - [29/Nov/2017:20:48:50 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://52.224.178.107/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/62.0.3202.94 Safari/537.36"
```

## <a name="delete-the-container"></a>Usuwanie kontenera

Po zakończeniu pracy z kontenerem możesz go usunąć przy użyciu polecenia [az container delete][az-container-delete]:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

Aby się upewnić, że kontener został usunięty, uruchom polecenie [az container list](/cli/azure/container#az_container_list):

```azurecli-interactive
az container list --resource-group myResourceGroup -o table
```

W danych wyjściowych polecenia nie powinien pojawić się kontener **mycontainer**. Jeśli nie ma innych kontenerów w grupie zasobów, nie powinny zostać wyświetlone żadne dane wyjściowe.

## <a name="next-steps"></a>Następne kroki

Cały kod dla kontenera, użyty w tym przewodniku Szybki start, jest dostępny [w serwisie GitHub][app-github-repo] wraz z odpowiednim plikiem Dockerfile. Jeśli chcesz spróbować samodzielnie skompilować kontener i wdrożyć go w usłudze Azure Container Instances za pomocą usługi Azure Container Registry, przejdź do samouczka dotyczącego usługi Azure Container Instances.

> [!div class="nextstepaction"]
> [Samouczki dotyczące usługi Azure Container Instances](./container-instances-tutorial-prepare-app.md)

Aby wypróbować opcje uruchamiania kontenerów w systemie aranżacji na platformie Azure, zobacz przewodnik Szybki start [Service Fabric][service-fabric] lub [Azure Container Service (AKS)][container-service].

<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git
[az-group-create]: /cli/azure/group?view=azure-cli-latest#az_group_create
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az_container_create
[az-container-delete]: /cli/azure/container?view=azure-cli-latest#az_container_delete
[az-container-list]: /cli/azure/container?view=azure-cli-latest#az_container_list
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az_container_logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az_container_show
[service-fabric]: ../service-fabric/service-fabric-quickstart-containers.md
[container-service]: ../aks/kubernetes-walkthrough.md


<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
