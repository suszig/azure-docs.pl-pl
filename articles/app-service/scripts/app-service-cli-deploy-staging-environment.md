---
title: "Przykładowy skrypt interfejsu wiersza polecenia Azure — tworzenie aplikacji sieci web i wdrażanie kodu tymczasowej środowiska | Dokumentacja firmy Microsoft"
description: "Przykładowy skrypt interfejsu wiersza polecenia Azure — tworzenie aplikacji sieci web i wdrażania kodu w środowisku przemieszczania"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 2b995dcd-e471-4355-9fda-00babcdb156e
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 449e5729a15e619c43e5f4a0643915c2d3114d17
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-web-app-and-deploy-code-to-a-staging-environment"></a>Tworzenie aplikacji sieci web i wdrażanie kodu w środowisku przemieszczania

Ten przykładowy skrypt tworzy aplikację sieci web w usłudze App Service z gniazdem dodatkowe wdrożenia o nazwie "tymczasowe", a następnie wdraża przykładową aplikację do miejsca "tymczasowe".

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten temat będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "Create a web app and deploy code to a staging environment")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Tworzenie planu usług aplikacji az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Tworzy plan usługi App Service. |
| [Tworzenie aplikacji sieci Web az](https://docs.microsoft.com/cli/azure/webapp#az_webapp_create) | Tworzenie aplikacji sieci web platformy Azure. |
| [Tworzenie miejsca wdrożenia aplikacji sieci Web az](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#az_webapp_deployment_slot_create) | Tworzenie miejsca wdrożenia. |
| [AZ aplikacji sieci Web wdrożenia źródło konfiguracji](https://docs.microsoft.com/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) | Kojarzy aplikacji sieci web platformy Azure z repozytorium Git lub Mercurial. |
| [AZ przeglądania aplikacji sieci Web](https://docs.microsoft.com/cli/azure/webapp#az_webapp_browse) | Otwórz aplikację sieci web platformy Azure w przeglądarce. |
| [wymiany gniazd wdrożenia aplikacji sieci Web az](https://docs.microsoft.com/cli/azure/webapp/deployment/slot#az_webapp_deployment_slot_swap) | Zamień miejsce wdrożenia do środowiska produkcyjnego. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe przykłady skryptów aplikacji usługi interfejsu wiersza polecenia można znaleźć w [dokumentacji usługi Azure App Service](../app-service-cli-samples.md).
