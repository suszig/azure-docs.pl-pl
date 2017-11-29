---
title: Azure CLI Azure skali skryptu bazy danych PostgreSQL | Dokumentacja firmy Microsoft
description: "Przykładowym skrypcie usługi Azure CLI - skalowania bazy danych Azure PostgreSQL serwera do poziomu wydajności różnych po zapytań metryki."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 11/27/2017
ms.openlocfilehash: a5a24e9aeea193df28bd49d5c428a72b5ec75d1b
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="monitor-and-scale-a-single-postgresql-server-using-azure-cli"></a>Monitorowanie i skalować pojedynczy serwer PostgreSQL przy użyciu wiersza polecenia platformy Azure
Ten przykładowy skrypt CLI skaluje pojedynczej bazy danych Azure PostgreSQL serwera do poziomu wydajności różnych po zapytań metryki. 

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten artykuł będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt
Ten przykładowy skrypt Zmień wyróżnione wiersze w celu dostosowania nazwa użytkownika i hasło. Zastąp identyfikator subskrypcji używane w poleceniach monitor az z identyfikatorem subskrypcji.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/scale-postgresql-server.sh?highlight=15-16 "Create and scale Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia
Po uruchomieniu przykładowy skrypt następującego polecenia można usunąć grupy zasobów i wszystkie zasoby skojarzone z nim.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/scale-postgresql-server/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Wyjaśnienie skryptu
Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| **Polecenie** | **Uwagi** |
|---|---|
| [Tworzenie grupy az](/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [utworzenie przez serwer postgres az](/cli/azure/postgres/server#az_postgres_server_create) | Tworzy serwer PostgreSQL, który jest hostem bazy danych. |
| [Lista metryki monitor az](/cli/azure/monitor/metrics#az_monitor_metrics_list) | Wyświetl listę wartość metryki dla zasobów. |
| [Usuwanie grupy az](/cli/azure/group#az_group_delete) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Następne kroki
- Przeczytaj więcej informacji na temat interfejsu wiersza polecenia Azure: [dokumentacji interfejsu wiersza polecenia Azure](/cli/azure/overview)
- Spróbuj dodatkowych skryptów: [przykłady wiersza polecenia platformy Azure dla bazy danych Azure dla PostgreSQL](../sample-scripts-azure-cli.md)
- Przeczytaj więcej na temat skalowania: [warstwy usług](../concepts-service-tiers.md) i [obliczeniowe i jednostek magazynu](../concepts-compute-unit-and-storage.md)
