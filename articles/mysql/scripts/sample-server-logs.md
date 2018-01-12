---
title: 'Azure CLI: Pobierz dzienniki serwera w bazie danych Azure dla programu MySQL'
description: "Ten przykładowy skrypt wiersza polecenia platformy Azure pokazuje, jak włączyć i Pobierz dzienniki serwera bazy danych MySQL serwera Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 01/11/2018
ms.openlocfilehash: b0d34009d189ab136dcb6f28fdccc49b6da9e108
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2018
---
# <a name="enable-and-download-server-slow-query-logs-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Włącz i Pobierz dzienniki powolne kwerendy serwera Azure bazy danych MySQL serwera przy użyciu wiersza polecenia platformy Azure
Ten przykładowy skrypt interfejsu wiersza polecenia umożliwia i pobierane pliki dzienników powolne zapytania jednej bazy danych Azure dla serwera MySQL.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym przykładzie wymaga, czy korzystasz z wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt
Ten przykładowy skrypt Zmień wyróżnione wiersze w celu dostosowania nazwa użytkownika i hasło. Zamień < nazwa_pliku_dziennika > w poleceniach monitor az własną nazwę pliku dziennika serwera.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/server-logs.sh?highlight=15-16 "Manipulate with server logs.")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia
Po uruchomieniu przykładowy skrypt następującego polecenia można usunąć grupy zasobów i wszystkie zasoby skojarzone z nim.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/server-logs/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Wyjaśnienie skryptu
Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| **Polecenie** | **Uwagi** |
|---|---|
| [Tworzenie grupy az](/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [utworzenie przez serwer mysql az](/cli/azure/mysql/server#az_msql_server_create) | Tworzy serwer MySQL, który jest hostem bazy danych. |
| [Lista konfiguracji serwerów mysql az](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_list) | Lista wartości konfiguracji serwera. |
| [AZ mysql serwera konfiguracji](/cli/azure/mysql/server/configuration#az_mysql_server_configuration_set) | Zaktualizuj konfigurację serwera. |
| [Lista dzienników serwera mysql az](/cli/azure/mysql/server-logs#az_mysql_server_logs_list) | Listę plików dziennika dla serwera. |
| [Pobierz dzienniki serwera az mysql](/cli/azure/mysql/server-logs#az_mysql_server_logs_download) | Pobierz pliki dziennika. |
| [Usuwanie grupy az](/cli/azure/group#az_group_delete) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Kolejne kroki
- Przeczytaj więcej informacji na temat interfejsu wiersza polecenia Azure: [dokumentacji interfejsu wiersza polecenia Azure](/cli/azure/overview).
- Spróbuj dodatkowych skryptów: [przykłady wiersza polecenia platformy Azure dla bazy danych Azure dla programu MySQL](../sample-scripts-azure-cli.md)
