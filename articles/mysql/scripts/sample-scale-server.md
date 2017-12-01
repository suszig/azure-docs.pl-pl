---
title: "Przykładów dla interfejsu wiersza polecenia platformy Azure można skalować bazy danych Azure dla serwera MySQL | Dokumentacja firmy Microsoft"
description: "Ten przykładowy skrypt interfejsu wiersza polecenia skaluje Azure bazy danych MySQL serwera do poziomu wydajności różnych po zapytań metryki."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 11/02/2017
ms.openlocfilehash: 517208b76757655990f82f50677267320eb1393c
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Monitorowanie i skalowania bazy danych Azure programu MySQL serwera przy użyciu wiersza polecenia platformy Azure
Ten przykładowy skrypt CLI skaluje pojedynczej bazy danych Azure MySQL serwera do poziomu wydajności różnych po zapytań metryki.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten artykuł będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt
Ten przykładowy skrypt Zmień wyróżnione wiersze w celu dostosowania nazwa użytkownika i hasło. Zastąp identyfikator subskrypcji używane w poleceniach monitor az z identyfikatorem subskrypcji.[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia
Po uruchomieniu przykładowym skrypcie, można usunąć grupy zasobów i wszystkie zasoby skojarzone z nią za pomocą następującego polecenia:[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Wyjaśnienie skryptu
Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| **Polecenie** | **Uwagi** |
|---|---|
| [Tworzenie grupy az](/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [utworzenie przez serwer mysql az](/cli/azure/mysql/server#az_mysql_server_create) | Tworzy serwer MySQL, który jest hostem bazy danych. |
| [Lista metryki monitor az](/cli/azure/monitor/metrics#az_monitor_metrics_list) | Wyświetl listę wartość metryki dla zasobów. |
| [Usuwanie grupy az](/cli/azure/group#az_group_delete) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Następne kroki
- Przeczytaj więcej informacji na temat interfejsu wiersza polecenia Azure: [dokumentacji interfejsu wiersza polecenia Azure](/cli/azure/overview).
- Spróbuj dodatkowych skryptów: [przykłady wiersza polecenia platformy Azure dla bazy danych Azure dla programu MySQL](../sample-scripts-azure-cli.md)
- Aby uzyskać więcej informacji na temat skalowania, zobacz [warstwy usług](../concepts-service-tiers.md) i [obliczeniowe i Magazyn jednostek](../concepts-compute-unit-and-storage.md).
