---
title: "Azure CLI przykładowym skrypcie - mapy niestandardową domenę do aplikacji funkcji | Dokumentacja firmy Microsoft"
description: "Przykład skryptu usługi Azure CLI — mapy niestandardową domenę do aplikacji funkcji na platformie Azure."
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
tags: azure-service-management
ms.assetid: d127e347-7581-47d7-b289-e0f51f2fbfbc
ms.service: functions
ms.workload: na
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/01/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 87b79d6222f40e3dc1306ecace51bae50b06e484
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="map-a-custom-domain-to-a-function-app"></a>Zamapować niestandardową domenę do aplikacji funkcji

Ten przykładowy skrypt tworzy aplikacji funkcji z powiązanych zasobów, a następnie mapuje `www.<yourdomain>` do niego. Do mapowania na domenę niestandardową, należy utworzyć aplikacji funkcji w planie usługi aplikacji, a nie w planie zużycia. Środowisko Azure Functions obsługuje tylko mapowania domeny niestandardowej przy użyciu rekordu A.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, musi użyć wiersza polecenia platformy Azure w wersji 2.0 lub nowszej wersji. Aby dowiedzieć się, jaka wersja jest używana, uruchom polecenie `az --version`. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/configure-custom-domain/configure-custom-domain.sh?highlight=3 "Map a custom domain to a function app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń: każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Tworzenie konta magazynu az](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_create) | Tworzy wymagane przez aplikację funkcja konto magazynu. |
| [Tworzenie planu usług aplikacji az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Tworzy plan usługi aplikacji potrzebne do mapowania domeny niestandardowej. |
| [Utwórz az functionapp]() | Tworzy aplikację funkcji. |
| [Dodaj nazwę az usługi aplikacji sieci web konfiguracji hosta](https://docs.microsoft.com/cli/azure/appservice/web/config/hostname#az_appservice_web_config_hostname_add) | Mapuje domeny niestandardowej aplikacji funkcji. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe przykłady skryptów funkcji interfejsu wiersza polecenia można znaleźć w [dokumentacji usługi Azure Functions]().
