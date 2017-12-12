---
title: "Baza danych Azure SQL przykład — monitor skali — jednego interfejsu wiersza polecenia | Dokumentacja firmy Microsoft"
description: "Azure CLI przykładowy skrypt do monitorowania i skalowania pojedynczej bazy danych Azure SQL"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune, mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 5913c8ec1b62fc38161e553dc2364c793951e047
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="use-cli-to-monitor-and-scale-a-single-sql-database"></a>Użyj interfejsu wiersza polecenia, aby monitorować i skalowania pojedynczej bazy danych SQL

Ten przykładowy skrypt wiersza polecenia platformy Azure skaluje pojedynczej bazy danych Azure SQL do poziomu wydajności różnych po podczas badania informacji rozmiar bazy danych. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten temat będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Przykładowy skrypt

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Monitor and scale single SQL Database")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia

Po uruchomieniu przykładowy skrypt następującego polecenia można usunąć grupy zasobów i wszystkie zasoby skojarzone z nim.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Wyjaśnienie skryptu

Ten skrypt używa następujących poleceń. Każde polecenie w tabeli łącza do dokumentacji określonego polecenia.

| Polecenie | Uwagi |
|---|---|
| [Tworzenie grupy az](https://docs.microsoft.com/cli/azure/group#az_group_create) | Tworzy grupę zasobów, w którym przechowywane są wszystkie zasoby. |
| [Utwórz az programu sql server](https://docs.microsoft.com/cli/azure/sql/server#az_sql_server_create) | Tworzy serwer logiczny, który jest hostem bazy danych. |
| [bazy danych sql az Pokaż — użycie](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_show_usage) | Przedstawia informacje o użyciu rozmiar bazy danych. |
| [Aktualizacja bazy danych sql az](https://docs.microsoft.com/cli/azure/sql/db#az_sql_db_update) | Aktualizuje właściwości bazy danych (na przykład poziomu wydajności ani warstwy usług) lub przenosi bazy danych do z lub między elastyczne pule. |
| [Usuwanie grupy az](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Usuwa grupę zasobów, w tym wszystkich zagnieżdżonych zasobów. |
|||

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji dotyczących interfejsu wiersza polecenia Azure, zobacz [dokumentacji interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/overview).

Dodatkowe przykłady skryptów bazy danych SQL interfejsu wiersza polecenia można znaleźć w [dokumentacji usługi Azure SQL Database](../sql-database-cli-samples.md).
