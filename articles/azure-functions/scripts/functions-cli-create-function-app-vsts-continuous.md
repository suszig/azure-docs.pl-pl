---
title: "Tworzenie funkcji na platformie Azure, które zostało wdrożone w Visual Studio Team Services | Dokumentacja firmy Microsoft"
description: "Tworzenie aplikacji funkcji i wdrażanie kodu funkcji z programu Visual Studio Team Services"
services: functions
keywords: 
author: syntaxc4
ms.author: cfowler
ms.date: 01/09/2018
ms.topic: sample
ms.service: functions
ms.custom: mvc
ms.openlocfilehash: bf9428f23e851bae3485ec3d724dfb9ccd2af4c1
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="create-a-function-in-azure-that-is-deployed-from-visual-studio-team-services"></a>Tworzenie funkcji na platformie Azure, które zostało wdrożone w Visual Studio Team Services

W tym temacie przedstawiono sposób używania usługi Azure Functions można utworzyć [niekorzystającą](https://azure.microsoft.com/overview/serverless-computing/) funkcji aplikacji w usłudze [planu zużycie](../functions-scale.md#consumption-plan). Funkcja aplikacji, która jest kontenerem dla funkcji, stale jest wdrażany z repozytorium programu Visual Studio Team Services (VSTS). Aby ukończyć w tym temacie, musi mieć:

* Repozytorium VSTS zawierającej projektu aplikacji funkcji, do którego masz uprawnienia administracyjne.
* A [osobisty token dostępu (PAWEŁ)](https://docs.microsoft.com/vsts/accounts/use-personal-access-tokens-to-authenticate) do uzyskania dostępu do repozytorium usługi VSTS.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli zamiast używasz interfejsu wiersza polecenia Azure lokalnie, musisz zainstalować i używać w wersji 2.0 lub nowszej wersji. Aby określić wersję interfejsu wiersza polecenia Azure, uruchom `az --version`. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt

Ten przykład tworzy aplikację funkcji platformy Azure i wdraża kod funkcji z programu Visual Studio Team Services.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń do utworzenia grupy zasobów, konto magazynu, funkcja aplikacji oraz wszystkie powiązane zasoby. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Tworzenie konta magazynu az](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) | Tworzy plan usługi App Service. |
| [Utwórz az functionapp](https://docs.microsoft.com/cli/azure/appservice/web#az_appservice_web_delete) |
| [Konfiguracja kontroli źródła programu az usługi aplikacji sieci web](https://docs.microsoft.com/cli/azure/appservice/web/source-control#az_appservice_web_source_control_config) | Kojarzy aplikacji funkcji za pomocą Git lub repozytorium Mercurial. |

## <a name="next-steps"></a>Kolejne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe przykłady skryptów wiersza polecenia funkcji platformy Azure można znaleźć w [dokumentacji usługi Azure Functions](../functions-cli-samples.md).
