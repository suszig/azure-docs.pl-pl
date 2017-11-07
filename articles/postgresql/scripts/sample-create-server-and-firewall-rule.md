---
title: Azure CLI Script - tworzenie bazy danych platformy Azure dla PostgreSQL | Dokumentacja firmy Microsoft
description: "Azure CLI przykładowym skrypcie — tworzy Azure bazy danych serwera PostgreSQL i konfiguruje regułę zapory poziomu serwera."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: sample
ms.date: 11/03/2017
ms.openlocfilehash: c83c5e86ea99a25ff106e0238f4d294bec100c32
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>Utwórz bazę danych Azure dla serwera PostgreSQL i skonfigurować regułę zapory przy użyciu wiersza polecenia platformy Azure
Ten przykładowy skrypt interfejsu wiersza polecenia Azure bazy danych serwera PostgreSQL tworzy i konfiguruje regułę zapory poziomu serwera. Po pomyślnym uruchomieniu skryptu PostgreSQL serwera są dostępne z wszystkich usług platformy Azure i skonfigurowanego adresu IP.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Jeśli do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym artykule, wymaga czy korzystasz z wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt
W ten przykładowy skrypt edytować wyróżnione wiersze w celu dostosowania nazwa użytkownika i hasło.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for PostgreSQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia
Po uruchomieniu przykładowy skrypt następującego polecenia można usunąć grupy zasobów i wszystkie zasoby skojarzone z nim.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Wyjaśnienie skryptu
Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| **Polecenie** | **Uwagi** |
|---|---|
| [Tworzenie grupy az](/cli/azure/group#create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [utworzenie przez serwer postgres az](/cli/azure/postgres/server#create) | Tworzy serwer PostgreSQL, który jest hostem bazy danych. |
| [Utwórz az postgres Serwer zapory](/cli/azure/postgres/server/firewall-rule#create) | Tworzy regułę zapory, aby zezwolić na dostęp do serwera i baz danych z wprowadzony zakres adresów IP. |
| [Usuwanie grupy az](/cli/azure/group#delete) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Następne kroki
- Przeczytaj więcej informacji na temat interfejsu wiersza polecenia Azure: [dokumentacji interfejsu wiersza polecenia Azure](/cli/azure/overview)
- Spróbuj dodatkowych skryptów: [przykłady wiersza polecenia platformy Azure dla bazy danych Azure dla PostgreSQL](../sample-scripts-azure-cli.md)
