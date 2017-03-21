---
title: "Tworzenie pierwszej aplikacji sieci Web platformy Node.js na platformie Azure w ciągu pięciu minut | Microsoft Docs"
description: "Dowiedz się, jak łatwo można uruchamiać aplikacje sieci Web w usłudze App Service, wdrażając przykładową aplikację Node.js."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 412cc786-5bf3-4e1b-b696-6a08cf46501e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/08/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 746f697076566ce3edd970336b005e53dc4d2d39
ms.lasthandoff: 03/15/2017


---
# <a name="create-your-first-nodejs-web-app-in-azure-in-five-minutes"></a>Tworzenie pierwszej aplikacji sieci Web platformy Node.js na platformie Azure w ciągu pięciu minut
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Ten przewodnik szybkiego startu pomaga w ciągu kilku minut wdrożyć pierwszą aplikację sieci Web platformy Node.js w [usłudze Azure App Service](../app-service/app-service-value-prop-what-is.md).

Przed uruchomieniem tego przewodnika szybkiego startu upewnij się, że masz na maszynie [zainstalowany interfejs wiersza polecenia platformy Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

# <a name="create-a-nodejs-web-app"></a>Tworzenie aplikacji sieci Web Node.js
2. Zaloguj się do platformy Azure, uruchamiając polecenie `az login` i postępując zgodnie ze wskazówkami na ekranie.
   
    ```azurecli
    az login
    ```
   
3. Utwórz [grupę zasobów](../azure-resource-manager/resource-group-overview.md). W niej umieścisz wszystkie zasoby platformy Azure, którymi chcesz zarządzać razem, takie jak aplikacja sieci Web i jej zaplecze w usłudze SQL Database.

    ```azurecli
    az group create --location "West Europe" --name myResourceGroup
    ```

    Aby sprawdzić, jakich wartości można użyć dla lokalizacji `---location`, użyj polecenia interfejsu wiersza polecenia platformy Azure `az appservice list-locations`.

3. Utwórz [plan usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) w warstwie Standardowa. Warstwa Standardowa jest wymagana do uruchamiania kontenerów systemu Linux.

    ```azurecli
    az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku S1 --is-linux 
    ```

4. Utwórz aplikację sieci Web o unikatowej nazwie wprowadzonej w tagu `<app_name>`.

    ```azurecli
    az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
    ```

4. Skonfiguruj kontener systemu Linux, aby użyć domyślnego obrazu Node.js 6.9.3.

    ```azurecli
    az appservice web config update --node-version 6.9.3 --name <app_name> --resource-group myResourceGroup
    ```

4. Wdróż przykładową aplikację platformy Node.js z usługi GitHub.

    ```azurecli
    az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
    --repo-url "https://github.com/Azure-Samples/app-service-web-nodejs-get-started.git" --branch master --manual-integration 
    ```

5. Aby zobaczyć aplikację działającą na platformie Azure, uruchom to polecenie.

    ```azurecli
    az appservice web browse --name <app_name> --resource-group myResourceGroup
    ```

Gratulacje, Twoja pierwsza aplikacja sieci Web platformy Node.js działa na żywo w usłudze Azure App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z wcześniej utworzonymi [skryptami interfejsu wiersza polecenia aplikacji sieci Web](app-service-cli-samples.md).

