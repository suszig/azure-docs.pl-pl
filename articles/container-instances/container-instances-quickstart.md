---
title: "Szybki Start - Twojego pierwszego kontener wystąpień kontenera platformy Azure"
description: "Wdrażaj i rozpocznij pracę z usługą Azure Container Instances"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 15e6127d419bb41f1b146aff147c43dce2233d8d
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Tworzenie pierwszego kontenera w usłudze Azure Container Instances

Wystąpień kontenera Azure ułatwia tworzenie i zarządzanie nimi kontenery Docker na platformie Azure, bez konieczności umieszczanie maszyn wirtualnych lub wdrożyć usługę wyższego poziomu usługi. Tego przewodnika Szybki Start służy do tworzenia kontenera na platformie Azure i uwidacznia go do Internetu za pomocą publicznego adresu IP. Ta operacja jest wykonywana za pomocą jednego polecenia. W ciągu kilku sekund zostanie wyświetlony to w przeglądarce:

![Widziana w przeglądarce aplikacja wdrożona za pomocą usługi Azure Container Instances][aci-app-browser]

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przewodnik Szybki start będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.12 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Wystąpienia Azure Container Instances są zasobami platformy Azure i należy je umieścić w grupie zasobów platformy Azure, czyli logicznej kolekcji, w której zasoby platformy Azure są wdrażane i zarządzane.

Utwórz nową grupę zasobów o [Tworzenie grupy az] [ az-group-create] polecenia.

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Tworzenie kontenera

Można utworzyć kontener, podając nazwę obrazu Docker i grupy zasobów platformy Azure do [utworzyć kontener az] [ az-container-create] polecenia. Opcjonalnie można ujawnić kontener w Internecie za pomocą publicznego adresu IP. W tym przypadku użyjemy kontenera, który jest hostem bardzo prostej aplikacji internetowej napisanej w języku [Node.js](http://nodejs.org).

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public
```

W ciągu kilku sekund powinna pojawić się odpowiedź na żądanie. Początkowo kontener będzie miał stan **Tworzenie**, ale powinien zostać uruchomiony w ciągu kilku sekund. Możesz sprawdzić stan, za pomocą [Pokaż kontenera az] [ az-container-show] polecenia:

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

W dolnej części danych wyjściowych zostanie wyświetlony stan aprowizowania kontenera i jego adres IP:

```json
...
"ipAddress": {
      "ip": "13.88.8.148",
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

Gdy przesuwa kontenera **zakończyło się pomyślnie** stanu, możesz uzyskiwać dostęp go w przeglądarce przy użyciu adresu IP, pod warunkiem.

![Widziana w przeglądarce aplikacja wdrożona za pomocą usługi Azure Container Instances][aci-app-browser]

## <a name="pull-the-container-logs"></a>Ściąganie dzienników kontenera

Można ściągnąć dzienniki dla kontenera zostały utworzone za pomocą [dzienniki kontenera az] [ az-container-logs] polecenia:

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

Dane wyjściowe:

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-the-container"></a>Usuwanie kontenera

Po zakończeniu z kontenerem, możesz je usunąć za pomocą [usuwania kontenera az] [ az-container-delete] polecenia:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

Cały kod dla kontenera, używane w tym szybkiego startu jest dostępna [w serwisie GitHub][app-github-repo], wraz z jego plik Dockerfile. Jeśli chcesz spróbować samodzielnie skompilować kontener i wdrożyć go w usłudze Azure Container Instances za pomocą usługi Azure Container Registry, przejdź do samouczka dotyczącego usługi Azure Container Instances.

> [!div class="nextstepaction"]
> [Samouczki dotyczące usługi Azure Container Instances](./container-instances-tutorial-prepare-app.md)

Aby wypróbować opcje uruchamiania kontenery w systemie aranżacji na platformie Azure, zobacz [sieci szkieletowej usług] [ service-fabric] lub [usługi kontenera platformy Azure (AKS)] [ container-service] Przewodniki Szybki Start.  

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