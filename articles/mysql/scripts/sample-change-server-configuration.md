---
title: "Skryptu interfejsu wiersza polecenia platformy Azure — zmiany konfiguracji serwera | Dokumentacja firmy Microsoft"
description: "Ten przykładowy skrypt interfejsu wiersza polecenia zawiera listę wszystkich dostępnych serwera konfiguracji i aktualizuje wartość innodb_lock_wait_timeout."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 11/03/2017
ms.openlocfilehash: 9a94f257e5cd3534127e8594ddee3c5f837876df
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Lista i aktualizacji konfiguracji Azure bazy danych MySQL serwera przy użyciu wiersza polecenia platformy Azure
Ten przykładowy skrypt interfejsu wiersza polecenia zawiera wszystkie parametry konfiguracji dostępności, a także ich dopuszczalnych wartości w bazie danych Azure dla serwera MySQL i ustawia *innodb_lock_wait_timeout* jest inna niż domyślna, jedną wartość.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten artykuł będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt
Ten przykładowy skrypt Zmień wyróżnione wiersze w celu dostosowania nazwa użytkownika i hasło.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia
Po uruchomieniu przykładowy skrypt następującego polecenia można usunąć grupy zasobów i wszystkie zasoby skojarzone z nim.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Wyjaśnienie skryptu
Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| **Polecenie** | **Uwagi** |
|---|---|
| [Tworzenie grupy az](/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [utworzenie przez serwer mysql az](/cli/azure/mysql/server#az_msql_server_create) | Tworzy serwer MySQL, który jest hostem bazy danych. |
| [Lista konfiguracji serwerów mysql az](/cli/azure/mysql/server/configuration#az_msql_server_configuration_list) | Listy konfiguracji bazy danych Azure dla serwera MySQL. |
| [AZ mysql serwera konfiguracji](/cli/azure/mysql/server/configuration#az_msql_server_configuration_set) | Zaktualizuj konfigurację bazy danych Azure dla serwera MySQL. |
| [Pokaż konfiguracji serwera mysql az](/cli/azure/mysql/server/configuration#az_msql_server_configuration_show) | Pokaż konfiguracji bazy danych Azure dla serwera MySQL. |
| [Usuwanie grupy az](/cli/azure/group#az_group_delete) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Następne kroki
- Przeczytaj więcej informacji na temat interfejsu wiersza polecenia Azure: [dokumentacji interfejsu wiersza polecenia Azure](/cli/azure/overview).
- Spróbuj dodatkowych skryptów: [przykłady wiersza polecenia platformy Azure dla bazy danych Azure dla programu MySQL](../sample-scripts-azure-cli.md)
- Aby uzyskać więcej informacji na parametry serwera, zobacz [jak można skonfigurować parametrów serwera w bazie danych Azure dla programu MySQL](../howto-server-parameters.md).
