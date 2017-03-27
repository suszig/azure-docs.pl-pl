---
title: "Tworzenie pierwszej statycznej aplikacji sieci Web w języku HTML na platformie Azure w ciągu pięciu minut | Microsoft Docs"
description: "Dowiedz się, jak łatwo można uruchamiać aplikacje sieci Web w usłudze App Service, wdrażając przykładową aplikację."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: 472353a2d099db869f43649cd46c8b004ebd53cc
ms.lasthandoff: 03/17/2017


---
# <a name="create-a-static-html-web-app-in-azure-in-five-minutes"></a>Tworzenie pierwszej statycznej aplikacji sieci Web w języku HTML na platformie Azure w ciągu pięciu minut
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Ten przewodnik szybkiego startu pomaga w ciągu kilku minut wdrożyć prostą witrynę w językach HTML+CSS w [usłudze Azure App Service](../app-service/app-service-value-prop-what-is.md).

Przed rozpoczęciem upewnij się, że interfejs wiersza polecenia platformy Azure został zainstalowany. Aby uzyskać więcej informacji, zobacz [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli) (Przewodnik instalacji interfejsu wiersza polecenia platformy Azure).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.
Zaloguj się do platformy Azure, uruchamiając polecenie `az login` i postępując zgodnie ze wskazówkami na ekranie.
   
```azurecli
az login
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów   
Utwórz [grupę zasobów](../azure-resource-manager/resource-group-overview.md). W niej umieścisz wszystkie zasoby platformy Azure, którymi chcesz zarządzać razem, takie jak aplikacja sieci Web i jej zaplecze w usłudze SQL Database.

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

Aby sprawdzić, jakich wartości można użyć dla lokalizacji `---location`, użyj polecenia interfejsu wiersza polecenia platformy Azure `az appservice list-locations`.


## <a name="create-an-app-service-plan"></a>Tworzenie planu usługi App Service
Utwórz [plan usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) w warstwie BEZPŁATNA. 

```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
```

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web
Utwórz aplikację sieci Web o unikatowej nazwie wprowadzonej w tagu `<app_name>`.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
```

## <a name="deploy-sample-application"></a>Wdrażanie przykładowej aplikacji
Wdróż przykładową witrynę w języku HTML z usługi GitHub.

```azurecli
az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
--repo-url "https://github.com/Azure-Samples/app-service-web-html-get-started.git" --branch master --manual-integration 
```

## <a name="browse-to-web-app"></a>Przechodzenie do aplikacji sieci Web
Aby zobaczyć aplikację działającą na platformie Azure, uruchom to polecenie.

```azurecli
az appservice web browse --name <app_name> --resource-group myResourceGroup
```

Gratulacje, Twoja pierwsza statyczna witryna w języku HTML działa na żywo w usłudze Azure App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z wcześniej utworzonymi [skryptami interfejsu wiersza polecenia aplikacji sieci Web](app-service-cli-samples.md).

