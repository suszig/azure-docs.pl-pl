---
title: "Wdrażanie kontenera wielu grup wystąpień kontenera platformy Azure"
description: "Dowiedz się, jak wdrożyć grupę kontenera o wielu kontenerów w wystąpień kontenera platformy Azure."
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 12/19/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5de818b0b47ee3345ddbc41455f5e953c5b96aa4
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="deploy-a-container-group"></a>Wdrożenie grupy kontenera

Wystąpień kontenera platformy Azure obsługuje wdrażanie wielu kontenerów na jednym hoście przy użyciu *grupy kontenerów*. Jest to przydatne podczas kompilowania aplikacji boczną rejestrowania, monitorowania lub dowolnej innej konfiguracji gdzie usługa musi drugi dołączony proces.

Ten dokument przeprowadzi Cię przez systemem konfiguracji proste boczną wielu kontenera przez wdrożenie szablonu usługi Azure Resource Manager.

## <a name="configure-the-template"></a>Konfigurowanie szablonu

Utwórz plik o nazwie `azuredeploy.json` i skopiuj następujący ciąg JSON do niego.

W tym przykładzie zdefiniowano grupę kontenera o dwa kontenery i publiczny adres IP. Aplikacja internetowy jest uruchamiana pierwszy kontenera w grupie. Drugi kontenera boczną, zgłasza żądanie HTTP do aplikacji głównej sieci web za pośrednictwem sieci lokalnej grupy.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "microsoft/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "microsoft/aci-tutorial-sidecar"
  },
    "resources": [
      {
        "name": "myContainerGroup",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2017-08-01-preview",
        "location": "[resourceGroup().location]",
        "properties": {
          "containers": [
            {
              "name": "[variables('container1name')]",
              "properties": {
                "image": "[variables('container1image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                },
                "ports": [
                  {
                    "port": 80
                  }
                ]
              }
            },
            {
              "name": "[variables('container2name')]",
              "properties": {
                "image": "[variables('container2image')]",
                "resources": {
                  "requests": {
                    "cpu": 1,
                    "memoryInGb": 1.5
                    }
                }
              }
            }
          ],
          "osType": "Linux",
          "ipAddress": {
            "type": "Public",
            "ports": [
              {
                "protocol": "tcp",
                "port": "80"
              }
            ]
          }
        }
      }
    ],
    "outputs": {
      "containerIPv4Address": {
        "type": "string",
        "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', 'myContainerGroup')).ipAddress.ip]"
      }
    }
  }
```

Aby użyć rejestru obrazu Kontener prywatny, należy dodać obiektu do dokumentu JSON o następującym formacie.

```json
"imageRegistryCredentials": [
    {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
    }
]
```

## <a name="deploy-the-template"></a>Wdrożenie szablonu

Utwórz grupę zasobów za pomocą polecenia [az group create][az-group-create].

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Wdrażanie szablonu z [Utwórz wdrożenie grupy az] [ az-group-deployment-create] polecenia.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --name myContainerGroup --template-file azuredeploy.json
```

W ciągu kilku sekund wstępnej reakcji powinien zostać wyświetlony z platformy Azure.

## <a name="view-deployment-state"></a>Wyświetl stan wdrożenia

Aby wyświetlić stan wdrożenia, użyj [Pokaż kontenera az] [ az-container-show] polecenia. To polecenie zwróci elastycznie publiczny adres IP za pomocą której aplikacja jest możliwy.

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Dane wyjściowe:

```bash
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGroup  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a>Wyświetl dzienniki

Wyświetlanie danych wyjściowych dziennika przy użyciu kontenera [dzienniki kontenera az] [ az-container-logs] polecenia. `--container-name` Argument określa kontener, od którego do pobierania dzienników. W tym przykładzie pierwsze kontener jest określony.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Dane wyjściowe:

```bash
listening on port 80
::1 - - [18/Dec/2017:21:31:08 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [18/Dec/2017:21:31:11 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [18/Dec/2017:21:31:15 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Aby wyświetlić dzienniki dla kontenera po stronie samochodu, uruchom tego samego polecenie, określając nazwę drugiego kontenera.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Dane wyjściowe:

```bash
Every 3s: curl -I http://localhost                          2017-12-18 23:19:34

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 29 Nov 2017 06:40:40 GMT
ETag: W/"67f-16006818640"
Content-Type: text/html; charset=UTF-8
Content-Length: 1663
Date: Mon, 18 Dec 2017 23:19:34 GMT
Connection: keep-alive
```

Jak widać, boczną okresowo wysłał żądanie HTTP do aplikacji głównego sieci web za pośrednictwem sieci lokalnej grupy, aby upewnić się, że jest uruchomiona. W tym przykładzie boczną można rozszerzyć do wyzwolenia alertu, jeżeli Odebrano kod odpowiedzi HTTP innych niż 200 OK.

## <a name="next-steps"></a>Kolejne kroki

W tym artykule opisano kroki niezbędne do wdrożenia wystąpienie kontenerem usługi kontenera platformy Azure. End-to-end środowisko wystąpień kontenera platformy Azure zobacz samouczek wystąpień kontenera platformy Azure.

> [!div class="nextstepaction"]
> [Samouczek wystąpień kontenera platformy Azure][aci-tutorial]

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show
[az-group-create]: /cli/azure/group#az_group_create
[az-group-deployment-create]: /cli/azure/group/deployment#az_group_deployment_create
