---
title: "Tworzenie funkcji platformy Azure, który łączy do bazy danych Azure rozwiązania Cosmos | Dokumentacja firmy Microsoft"
description: "Azure CLI skrypt przykładowy — Tworzenie funkcji platformy Azure, który łączy do bazy danych Azure rozwiązania Cosmos"
services: functions
documentationcenter: functions
author: rachelappel
manager: cfowler
editor: 
tags: functions
ms.assetid: 
ms.service: functions
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 04/20/2017
ms.author: rachelap
ms.custom: mvc
ms.openlocfilehash: ce48f8dd71d5b9e48dcd506df976e07d385f34c5
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="create-an-azure-function-that-connects-to-an-azure-cosmos-db"></a>Tworzenie funkcji platformy Azure, który łączy do bazy danych Azure rozwiązania Cosmos

Ten przykładowy skrypt tworzy aplikacji funkcji platformy Azure i nawiązuje połączenie z bazą danych Azure DB rozwiązania Cosmos.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten temat będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt

Ten przykład tworzy aplikację funkcji platformy Azure i dodaje DB rozwiązania Cosmos punktu końcowego i klucz dostępu do ustawień aplikacji.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-connect-to-cosmos-db/create-function-app-connect-to-cosmos-db.sh "Create an Azure Function that connects to an Azure Cosmos DB")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia

Po uruchomieniu przykładowym skrypcie, wykonaj polecenie może służyć do Usuń grupę zasobów i wszystkie powiązane zasoby.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [AZ logowania](https://docs.microsoft.com/cli/azure/#login) | Zaloguj się do platformy Azure. |
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Utwórz grupę zasobów z lokalizacji |
| [Tworzenie konta magazynu az](https://docs.microsoft.com/cli/azure/storage/account) | Tworzenie konta magazynu |
| [Utwórz az functionapp](https://docs.microsoft.com/cli/azure/functionapp#az_functionapp_create) | Utwórz nową aplikację funkcji |
| [Utwórz az cosmosdb](https://docs.microsoft.com/cli/azure/cosmosdb#az_cosmosdb_create) | Utwórz bazę danych cosmosdb |
| [Usuwanie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_delete) | Czyszczenie |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe przykłady skryptów wiersza polecenia funkcji platformy Azure można znaleźć w [dokumentacji usługi Azure Functions](../functions-cli-samples.md).




