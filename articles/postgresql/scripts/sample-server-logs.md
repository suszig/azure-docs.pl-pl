---
title: "Skryptu interfejsu wiersza polecenia platformy Azure: pobierania do dzienników serwera w bazie danych Azure dla PostgreSQL"
description: "Ten przykładowy skrypt wiersza polecenia platformy Azure pokazuje, jak włączyć i Pobierz dzienniki serwera bazy danych Azure PostgreSQL serwera."
services: postgresql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: 8bd6e193ca52401b16e141162a76d39bdef840f1
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Włącz i Pobierz dzienniki powolne kwerendy serwera bazy danych Azure dla serwera PostgreSQL przy użyciu wiersza polecenia platformy Azure
Ten przykładowy skrypt interfejsu wiersza polecenia umożliwia i pobierane pliki dzienników powolne zapytania jednej bazy danych Azure PostgreSQL serwera.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym przykładzie wymaga, czy korzystasz z wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt
Ten przykładowy skrypt Zmień wyróżnione wiersze w celu dostosowania nazwa użytkownika i hasło. Zamień < nazwa_pliku_dziennika > w poleceniach monitor az własną nazwę pliku dziennika serwera.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia
Po uruchomieniu przykładowy skrypt następującego polecenia można usunąć grupy zasobów i wszystkie zasoby skojarzone z nim.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Wyjaśnienie skryptu
Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| **Polecenie** | **Uwagi** |
|---|---|
| [Tworzenie grupy az](/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [utworzenie przez serwer postgres az](/cli/azure/postgres/server#az_msql_server_create) | Tworzy serwer PostgreSQL, który jest hostem bazy danych. |
| [AZ postgres serwera konfiguracji na liście](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) | Lista wartości konfiguracji serwera. |
| [AZ postgres serwera konfiguracji](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) | Zaktualizuj konfigurację serwera. |
| [Lista dzienników serwera postgres az](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) | Listę plików dziennika dla serwera. |
| [Pobierz dzienniki serwera az postgres](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) | Pobierz pliki dziennika. |
| [Usuwanie grupy az](/cli/azure/group#az_group_delete) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Kolejne kroki
- Przeczytaj więcej informacji na temat interfejsu wiersza polecenia Azure: [dokumentacji interfejsu wiersza polecenia Azure](/cli/azure/overview).
- Spróbuj dodatkowych skryptów: [przykłady wiersza polecenia platformy Azure dla bazy danych Azure dla PostgreSQL](../sample-scripts-azure-cli.md)
- [Konfigurowanie i dzienników serwera dostępu w portalu Azure](../howto-configure-server-logs-in-portal.md)
