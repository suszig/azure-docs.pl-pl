---
title: "Skryptu interfejsu wiersza polecenia platformy Azure: Zmień konfigurację serwera"
description: "Ten przykładowy skrypt interfejsu wiersza polecenia zawiera wszystkie opcje konfiguracji dostępne serwera i aktualizuje wartość jednej z opcji."
services: postgres
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: 334322557c83d5576c9a11def6bb0dc0e3ad1fec
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2018
---
# <a name="list-and-update-configurations-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Lista i aktualizacji konfiguracji bazy danych Azure dla serwera PostgreSQL przy użyciu wiersza polecenia platformy Azure
Ten przykładowy skrypt interfejsu wiersza polecenia zawiera wszystkie parametry konfiguracji dostępności, a także ich dopuszczalnych wartości w bazie danych Azure dla serwera PostgreSQL i ustawia *log_retention_days* jest inna niż domyślna, jedną wartość.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten artykuł będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt
Ten przykładowy skrypt Zmień wyróżnione wiersze w celu dostosowania nazwa użytkownika i hasło.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for PostgreSQL.")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia
Po uruchomieniu przykładowy skrypt następującego polecenia można usunąć grupy zasobów i wszystkie zasoby skojarzone z nim.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/change-server-configurations/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Wyjaśnienie skryptu
Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| **Polecenie** | **Uwagi** |
|---|---|
| [Tworzenie grupy az](/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [utworzenie przez serwer postgres az](/cli/azure/postgres/server#az_postgres_server_create) | Tworzy serwer PostgreSQL, który jest hostem bazy danych. |
| [AZ postgres serwera konfiguracji na liście](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) | Listy konfiguracji bazy danych Azure PostgreSQL serwera. |
| [AZ postgres serwera konfiguracji](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) | Zaktualizuj konfigurację bazy danych Azure PostgreSQL serwera. |
| [Pokaż konfiguracji serwera postgres az](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_show) | Pokaż konfiguracji bazy danych Azure PostgreSQL serwera. |
| [Usuwanie grupy az](/cli/azure/group#az_group_delete) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Kolejne kroki
- Przeczytaj więcej informacji na temat interfejsu wiersza polecenia Azure: [dokumentacji interfejsu wiersza polecenia Azure](/cli/azure/overview).
- Spróbuj dodatkowych skryptów: [przykłady wiersza polecenia platformy Azure dla bazy danych Azure dla PostgreSQL](../sample-scripts-azure-cli.md)
- Aby uzyskać więcej informacji na parametry serwera, zobacz [parametrów w portalu Azure, w jaki sposób można skonfigurować serwera](../howto-configure-server-parameters-using-portal.md).
