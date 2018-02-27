---
title: "Skrypt interfejsu wiersza polecenia platformy Azure: pobieranie dzienników serwera w usłudze Azure Database for PostgreSQL"
description: "Ten przykładowy skrypt interfejsu wiersza polecenia platformy Azure pokazuje, jak włączyć i pobrać dzienniki serwera usługi Azure Database for PostgreSQL."
services: postgresql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/12/2018
ms.openlocfilehash: 27dad83546c5f6d5bac18d61a1e223ced1158236
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-postgresql-server-using-azure-cli"></a>Włączanie i pobieranie dzienników wolnych zapytań serwera usługi Azure Database for PostgreSQL przy użyciu interfejsu wiersza polecenia platformy Azure
Ten przykładowy skrypt interfejsu wiersza polecenia włącza i pobiera dzienniki wolnych zapytań jednego serwera usługi Azure Database for PostgreSQL.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten przykład będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt
W tym przykładowym skrypcie zmień wyróżnione wiersze w celu dostosowania nazwy użytkownika i hasła administratora. Zastąp zmienną <log_file_name> w poleceniach az monitor własną nazwą pliku dziennika serwera.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia
Po wykonaniu przykładowego skryptu możesz uruchomić następujące polecenie, aby usunąć grupę zasobów i wszystkie skojarzone z nią zasoby.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/server-logs/delete-postgresql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Objaśnienia dla skryptu
W tym skrypcie użyto następujących poleceń. Każde polecenie w tabeli stanowi link do dokumentacji polecenia.

| **Polecenie** | **Uwagi** |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w której są przechowywane wszystkie zasoby. |
| [az postgres server create](/cli/azure/postgres/server#az_msql_server_create) | Tworzy serwer PostgreSQL hostujący bazy danych. |
| [az postgres server configuration list](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_list) | Zwraca listę wartości konfiguracji dla serwera. |
| [az postgres server configuration set](/cli/azure/postgres/server/configuration#az_postgres_server_configuration_set) | Aktualizuje konfigurację serwera. |
| [az postgres server-logs list](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) | Zwraca listę plików dziennika dla serwera. |
| [az postgres server-logs download](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) | Pobiera pliki dziennika. |
| [az group delete](/cli/azure/group#az_group_delete) | Usuwa grupę zasobów wraz ze wszystkimi zagnieżdżonymi zasobami. |

## <a name="next-steps"></a>Następne kroki
- Dowiedz się więcej na temat interfejsu wiersza polecenia platformy Azure: [dokumentacja interfejsu wiersza polecenia platformy Azure](/cli/azure/overview).
- Wypróbuj dodatkowe skrypty: [Przykłady interfejsu wiersza polecenia platformy Azure dla usługi Azure Database for PostgreSQL](../sample-scripts-azure-cli.md)
- [Konfigurowanie dzienników serwera i uzyskiwanie do nich dostępu w witrynie Azure Portal](../howto-configure-server-logs-in-portal.md)
