---
title: "Uruchom Centrum Docker obraz niestandardowy w aplikacji sieci Web dla kontenerów | Dokumentacja firmy Microsoft"
description: "Jak używać niestandardowego obrazu Docker dla aplikacji sieci Web dla kontenerów."
keywords: "Usługa aplikacji Azure, aplikacji sieci web, linux, docker, kontenera"
services: app-service
documentationcenter: 
author: naziml
manager: cfowler
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/05/2017
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: c85f79cc14cdcecd2a05fc0ff91c4864b9fba277
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="run-a-custom-docker-hub-image-in-web-app-for-containers"></a>Uruchom Centrum Docker obraz niestandardowy w aplikacji sieci Web dla kontenerów

Usługi aplikacji umożliwia stosy wstępnie zdefiniowanych aplikacji w systemie Linux obsługę dla określonej wersji, takich jak PHP w wersji 7.0 i Node.js 4.5. Aby wdrożyć aplikację sieci web stosu aplikacji, która nie jest już zdefiniowana w Azure umożliwia także niestandardowego obrazu Docker. Ta opcja szybkiego startu pokazano, jak utworzyć aplikację sieci web i wdrażanie Python, na podstawie obrazu platformy Docker. Można utworzyć aplikacji sieci web w programie [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-container"></a>Tworzenie aplikacji sieci Web dla kontenera

Utwórz [aplikację sieci Web](../app-service-web-overview.md) w `myAppServicePlan`planie usługi App Service za pomocą polecenia [az webapp create](/cli/azure/webapp#create). Nie zapomnij Zastąp `<app name>` przy użyciu unikatowej nazwy aplikacji.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name elnably/dockerimagetest
```

W poprzednim poleceniu `--deployment-container-image-name` wskazuje publicznego obraz Centrum Docker [https://hub.docker.com/r/elnably/dockerimagetest/](https://hub.docker.com/r/elnably/dockerimagetest/). Możesz sprawdzić jej zawartość w [https://github.com/ahmedelnably/dockerimagetest](https://github.com/ahmedelnably/dockerimagetest).

Po utworzeniu aplikacji sieci web, Azure CLI przedstawia dane wyjściowe podobne do poniższego przykładu:

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

Przejdź do następującego adresu URL za pomocą przeglądarki sieci web.

```bash
http://<app_name>.azurewebsites.net
```

![Przykładowa aplikacja działająca na platformie Azure](media/quickstart-custom-docker-image/hello-world-in-browser.png)

**Gratulacje!** Obraz niestandardowy Docker została wdrożona do aplikacji sieci Web dla kontenerów.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Tworzenie aplikacji sieci web Docker Python i PostgreSQL na platformie Azure](tutorial-docker-python-postgresql-app.md)
