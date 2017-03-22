---
title: "Tworzenie pierwszej aplikacji sieci Web w języku Java na platformie Azure w ciągu pięciu minut | Microsoft Docs"
description: "Dowiedz się, jak łatwo można uruchamiać aplikacje sieci Web w usłudze App Service, wdrażając przykładową aplikację."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/08/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: e48e03e86a325b8f39809a49cdd19820dfa78bdc
ms.lasthandoff: 03/10/2017


---
# <a name="create-your-first-java-web-app-in-azure-in-five-minutes"></a>Tworzenie pierwszej aplikacji sieci Web w języku Java na platformie Azure w ciągu pięciu minut
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)]

Ten przewodnik szybkiego startu pomaga w ciągu kilku minut wdrożyć pierwszą aplikację sieci Web w języku Java w [usłudze Azure App Service](../app-service/app-service-value-prop-what-is.md).

Przed uruchomieniem tego przewodnika szybkiego startu upewnij się, że masz na maszynie [zainstalowany interfejs wiersza polecenia platformy Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

## <a name="create-a-java-web-app-in-azure"></a>Tworzenie aplikacji sieci Web w języku Java na platformie Azure
2. Zaloguj się do platformy Azure, uruchamiając polecenie `az login` i postępując zgodnie ze wskazówkami na ekranie.
   
    ```azurecli
    az login
    ```
   
3. Utwórz [grupę zasobów](../azure-resource-manager/resource-group-overview.md). W niej umieścisz wszystkie zasoby platformy Azure, którymi chcesz zarządzać razem, takie jak aplikacja sieci Web i jej zaplecze w usłudze SQL Database.

    ```azurecli
    az group create --location "West Europe" --name myResourceGroup
    ```

    Aby sprawdzić, jakich wartości można użyć dla lokalizacji `---location`, użyj polecenia interfejsu wiersza polecenia platformy Azure `az appservice list-locations`.

3. Utwórz [plan usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) w warstwie BEZPŁATNA. 

    ```azurecli
    az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
    ```

4. Utwórz nową aplikację sieci Web o unikatowej nazwie wprowadzonej w tagu `<app_name>`.

    ```azurecli
    az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
    ```

4. Wdróż przykładową aplikację w języku Java z usługi GitHub.

    ```azurecli
    az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
    --repo-url "https://github.com/azure-appservice-samples/JavaCoffeeShopTemplate.git" --branch master --manual-integration 
    ```


5. Aby zobaczyć aplikację działającą na platformie Azure, uruchom to polecenie.

    ```azurecli
    az appservice web browse --name <app_name> --resource-group myResourceGroup
    ```

Gratulacje, Twoja pierwsza aplikacja sieci Web w języku Java działa na żywo w usłudze Azure App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z wcześniej utworzonymi [skryptami interfejsu wiersza polecenia aplikacji sieci Web](app-service-cli-samples.md).

