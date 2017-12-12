---
title: "Interfejs wiersza polecenia przykład — tworzenie bazy danych Azure SQL | Dokumentacja firmy Microsoft"
description: "Ten przykładowy skrypt wiersza polecenia platformy Azure umożliwiają utworzenie bazy danych SQL."
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers, mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 10/11/2017
ms.author: janeng
ms.openlocfilehash: 404d43a6f2fa38276b9517e9542f1e50a4b1980b
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="use-cli-to-create-a-single-azure-sql-database-and-configure-a-firewall-rule"></a>Użyj interfejsu wiersza polecenia, aby utworzyć pojedynczej bazy danych Azure SQL i skonfigurować reguły zapory

W tym przykładzie skrypt wiersza polecenia platformy Azure utworzy bazę danych Azure SQL i skonfigurować regułę zapory poziomu serwera. Po pomyślnym uruchomieniu skryptu bazy danych SQL są dostępne z wszystkich usług platformy Azure i skonfigurowanego adresu IP. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten temat będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh?highlight=9-10 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia

Po uruchomieniu przykładowy skrypt następującego polecenia można usunąć grupy zasobów i wszystkie zasoby skojarzone z nim.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Utwórz az programu sql server](/cli/azure/sql/server#az_sql_server_create) | Tworzy serwer logiczny, który jest hostem bazy danych SQL. |
| [Utwórz zapory serwera sql az](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create) | Tworzy regułę zapory zezwalają na dostęp do wszystkich baz danych na serwerze z wprowadzony zakres adresów IP. |
| [Tworzenie bazy danych sql az](/cli/azure/sql/db#az_sql_db_create) | Tworzy bazę danych SQL w serwera logicznego. |
| [Usuwanie grupy az](/cli/azure/resource#delete) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe przykłady skryptów bazy danych SQL interfejsu wiersza polecenia można znaleźć w [dokumentacji usługi Azure SQL Database](../sql-database-cli-samples.md).

