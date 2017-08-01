---
title: "Tworzenie pierwszego kontenera usługi Azure Container Instances | Azure Docs"
description: "Wdrażaj i rozpocznij pracę z usługą Azure Container Instances"
services: container-service
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: 
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: seanmck
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 3b15d6645b988f69f1f05b27aff6f726f34786fc
ms.openlocfilehash: 933299ce5a5d6f5b2262d40ae768019ccaf8796a
ms.contentlocale: pl-pl
ms.lasthandoff: 07/26/2017

---

# <a name="create-your-first-container-in-azure-container-instances"></a>Tworzenie pierwszego kontenera w usłudze Azure Container Instances

Usługa Azure Container Instances ułatwia tworzenie kontenerów i zarządzanie nimi na platformie Azure. Podczas pracy z tym przewodnikiem Szybki start utworzysz kontener na platformie Azure i ujawnisz go w Internecie przy użyciu publicznego adresu IP. Ta operacja jest wykonywana za pomocą jednego polecenia. W ciągu kilku sekund w przeglądarce zobaczysz następujący wynik:

![Widziana w przeglądarce aplikacja wdrożona za pomocą usługi Azure Container Instances][aci-app-browser]

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Polecenia interfejsu wiersza polecenia usługi Azure Container Instances są obecnie dostępne tylko w usłudze Azure Cloud Shell.

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Wystąpienia Azure Container Instances są zasobami platformy Azure i należy je umieścić w grupie zasobów platformy Azure, czyli logicznej kolekcji, w której zasoby platformy Azure są wdrażane i zarządzane.

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). 

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Tworzenie kontenera

Kontener można utworzyć, podając nazwę obrazu usługi Docker i grupy zasobów platformy Azure. Opcjonalnie można ujawnić kontener w Internecie za pomocą publicznego adresu IP. W tym przypadku użyjemy kontenera, który jest hostem bardzo prostej aplikacji internetowej napisanej w języku [Node.js](http://nodejs.org).

Polecenia interfejsu wiersza polecenia usługi Azure Container Instances są obecnie dostępne tylko w usłudze Azure Cloud Shell.

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public 
```

W ciągu kilku sekund powinna pojawić się odpowiedź na żądanie. Początkowo kontener będzie miał stan **Tworzenie**, ale powinien zostać uruchomiony w ciągu kilku sekund. Stan możesz sprawdzić przy użyciu polecenia `show`:

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

Po przejściu kontenera do stanu **Powodzenie** możesz uzyskiwać dostęp go niego w przeglądarce przy użyciu podanego adresu IP. 

![Widziana w przeglądarce aplikacja wdrożona za pomocą usługi Azure Container Instances][aci-app-browser]

## <a name="pull-the-container-logs"></a>Ściąganie dzienników kontenera

Dzienniki dla utworzonego kontenera możesz ściągnąć przy użyciu polecenia `logs`:

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

Po zakończeniu pracy z kontenerem możesz go usunąć przy użyciu polecenia `delete`:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a>Następne kroki

Cały kod dla kontenera, użyty w tym przewodniku Szybki start jest dostępny [w serwisie GitHub][app-github-repo] wraz z odpowiednim plikiem Dockerfile. Jeśli chcesz spróbować samodzielnie skompilować kontener i wdrożyć go w usłudze Azure Container Instances za pomocą usługi Azure Container Registry, przejdź do samouczka dotyczącego usługi Azure Container Instances.

> [!div class="nextstepaction"]
> [Samouczki dotyczące usługi Azure Container Instances](./container-instances-tutorial-prepare-app.md)


<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
