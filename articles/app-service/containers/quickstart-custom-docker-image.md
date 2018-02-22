---
title: "Uruchamianie niestandardowego obrazu usługi Docker Hub w usłudze Azure Web App for Containers | Microsoft Docs"
description: "Używanie niestandardowego obrazu Docker dla usługi Azure Web App for Containers."
keywords: azure app service, web app, linux, docker, container
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/02/2017
ms.author: cephalin;wesmc
ms.custom: mvc
ms.openlocfilehash: 7f6ed6d52bea1faec9dbed96a8d7aef020aea5d9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="run-a-custom-docker-hub-image-in-azure-web-app-for-containers"></a>Uruchamianie niestandardowego obrazu usługi Docker Hub w usłudze Azure Web App for Containers

Usługa App Service zapewnia wstępnie zdefiniowane stosy aplikacji w systemie Linux z obsługą określonych wersji, takich jak środowisko PHP 7.0 i Node.js 4.5. Można także użyć niestandardowego obrazu platformy Docker, aby uruchamiać aplikację internetową na stosie aplikacji, który nie jest zdefiniowany na platformie Azure. W tym przewodniku Szybki start pokazano, jak utworzyć aplikację internetową i wdrożyć w niej [oficjalny obraz platformy Docker dla kontenera Nginx](https://hub.docker.com/r/_/nginx/). Do tworzenia aplikacji internetowej używany jest [interfejs wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

![Przykładowa aplikacja działająca na platformie Azure](media/quickstart-custom-docker-image/hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-container"></a>Tworzenie aplikacji internetowej dla kontenerów

Utwórz [aplikację internetową](../app-service-web-overview.md) w planie usługi App Service `myAppServicePlan` za pomocą polecenia [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). Nie zapomnij zastąpić elementu `<app name>` unikatową nazwą aplikacji.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name nginx
```

W poprzednim poleceniu `--deployment-container-image-name` wskazuje publiczny obraz usługi Docker Hub [https://hub.docker.com/r/_/nginx/](https://hub.docker.com/r/_/nginx/).

Po utworzeniu aplikacji internetowej w interfejsie wiersza polecenia platformy Azure zostaną wyświetlone dane wyjściowe podobne do następujących:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app name>.scm.azurewebsites.net/<app name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

## <a name="browse-to-the-app"></a>Przechodzenie do aplikacji

W przeglądarce internetowej przejdź do następującego adresu URL.

```bash
http://<app_name>.azurewebsites.net
```

![Przykładowa aplikacja działająca na platformie Azure](media/quickstart-custom-docker-image/hello-world-in-browser.png)

**Gratulacje!** Niestandardowy obraz platformy Docker został wdrożony w usłudze Web App for Containers.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Używanie niestandardowego obrazu platformy Docker](tutorial-custom-docker-image.md)
