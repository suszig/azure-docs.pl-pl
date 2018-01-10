---
title: "Tworzenie funkcji platformy Azure, które zostało wdrożone z serwisu GitHub | Dokumentacja firmy Microsoft"
description: "Tworzenie aplikacji funkcji i wdrażanie kodu funkcji z repozytorium GitHub, za pomocą usługi Azure Functions."
services: functions
ms.service: functions
keywords: 
ms.devlang: azurecli
author: syntaxc4
ms.author: cfowler
ms.date: 01/09/2018
ms.topic: sample
ms.custom: mvc
ms.openlocfilehash: c4224bc7973cd1e3ca36799db9f23a124fcba807
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="create-a-function-in-azure-that-is-deployed-from-github"></a>Tworzenie funkcji platformy Azure, które zostało wdrożone z usługi GitHub

Ten przykładowy skrypt tworzy aplikacji funkcji przy użyciu [planu zużycie](../functions-scale.md#consumption-plan) z jego powiązanych zasobów i stale wdraża kodu funkcji z repozytorium GitHub. W tym przykładzie należy:

* Repozytorium GitHub z kodem funkcje, które ma uprawnienia administracyjne.
* A [osobistych dostępu tokenu (PAWEŁ)](https://help.github.com/articles/creating-an-access-token-for-command-line-use) na koncie usługi GitHub.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli zamiast używasz interfejsu wiersza polecenia Azure lokalnie, musisz zainstalować i używać w wersji 2.0 lub nowszej wersji. Aby określić wersję interfejsu wiersza polecenia Azure, uruchom `az --version`. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt

Ten przykład tworzy aplikację funkcji platformy Azure i wdraża kod funkcji z usługi GitHub.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-github-continuous/deploy-function-app-with-function-github-continuous.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Każde polecenie w tabeli łącza do dokumentacji określonego polecenia. Ten skrypt używa następujących poleceń:

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Tworzenie konta magazynu az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Tworzy plan usługi App Service. |
| [Utwórz az functionapp](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [Konfiguracja kontroli źródła programu az usługi aplikacji sieci web](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Kojarzy aplikacji funkcji za pomocą Git lub repozytorium Mercurial. |

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe przykłady skryptów wiersza polecenia funkcji platformy Azure można znaleźć w [dokumentacji usługi Azure Functions](../functions-cli-samples.md).
