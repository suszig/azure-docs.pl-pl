---
title: "Tworzenie pierwszej aplikacji sieci Web w języku PHP na platformie Azure w ciągu pięciu minut | Microsoft Docs"
description: "Dowiedz się, jak łatwo można uruchamiać aplikacje sieci Web w usłudze App Service, wdrażając przykładową aplikację PHP."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: afecc8997631bf507c797e56a9e3fc0d1df27614
ms.lasthandoff: 03/21/2017


---
# <a name="create-your-first-php-web-app-in-azure-in-five-minutes"></a>Tworzenie pierwszej aplikacji sieci Web w języku PHP na platformie Azure w ciągu pięciu minut
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)]

Ten przewodnik szybkiego startu pomaga w ciągu kilku minut wdrożyć pierwszą aplikację sieci Web w języku PHP w [usłudze Azure App Service](../app-service/app-service-value-prop-what-is.md).

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
Utwórz „Standardowy” [plan usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) na potrzeby uruchamiania kontenera systemu Linux. 

```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --is-linux --sku S1
```

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web
Utwórz aplikację sieci Web o unikatowej nazwie wprowadzonej w tagu `<app_name>`.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
```

## <a name="configure-the-linux-container"></a>Konfigurowanie kontenera systemu Linux
Skonfiguruj kontener systemu Linux, aby użyć domyślnego obrazu PHP 7.0.6.

```azurecli
az appservice web config update --php-version 7.0.6 --name <app_name> --resource-group myResourceGroup
```
## <a name="deploy-sample-application"></a>Wdrażanie przykładowej aplikacji
Wdróż przykładową aplikację w języku PHP z usługi GitHub.

```azurecli
az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
--repo-url "https://github.com/Azure-Samples/app-service-web-php-get-started.git" --branch master --manual-integration 
```

## <a name="browse-to-web-app"></a>Przechodzenie do aplikacji sieci Web
Aby zobaczyć aplikację działającą na platformie Azure, uruchom to polecenie.

```azurecli
az appservice web browse --name <app_name> --resource-group myResourceGroup
```

Gratulacje, Twoja pierwsza aplikacja sieci Web w języku PHP działa na żywo w usłudze Azure App Service.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z wcześniej utworzonymi [skryptami interfejsu wiersza polecenia aplikacji sieci Web](app-service-cli-samples.md).

