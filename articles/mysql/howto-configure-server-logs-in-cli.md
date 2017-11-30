---
title: "Dostęp do dzienników serwera w bazie danych Azure dla programu MySQL przy użyciu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób dostęp do dzienników serwera w bazie danych Azure dla programu MySQL za pomocą narzędzia wiersza polecenia wiersza polecenia platformy Azure."
services: mysql
author: rachel-msft
ms.author: raagyema
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 11/28/2017
ms.openlocfilehash: 908f28d8bd3d0dcbd03636e69cd47b5c47f3cfde
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Konfigurowanie i uzyskać dostępu do dzienników serwera przy użyciu wiersza polecenia platformy Azure
Możesz pobrać bazy danych Azure do dzienników serwera MySQL przy użyciu wiersza polecenia platformy Azure, narzędzie wiersza polecenia platformy Azure.

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków opisanych ten przewodnik, potrzebne są:
- [Azure bazy danych MySQL serwera](quickstart-create-mysql-server-database-using-azure-cli.md)
- [Azure CLI 2.0](/cli/azure/install-azure-cli) lub użyć powłoki chmury Azure w przeglądarce.

## <a name="configure-logging-for-azure-database-for-mysql"></a>Konfigurowanie rejestrowania w bazie danych Azure dla programu MySQL
Można skonfigurować serwera do dostępu do dziennika powolne zapytania MySQL.
1. Należy włączyć rejestrowanie przez ustawienie **powolne\_zapytania\_dziennika** parametru na wartość on.
2. Dostosuj inne parametry, takie jak **długi\_zapytania\_czasu** i **dziennika\_powolne\_admin\_instrukcje**.

Zobacz [sposobu konfigurowania parametrów serwera](howto-configure-server-parameters-using-cli.md) Aby dowiedzieć się, jak ustawić wartości tych parametrów, za pomocą wiersza polecenia platformy Azure.

Na przykład następujące polecenia interfejsu wiersza polecenia powoduje włączenie dziennika powolne zapytania, ustawia czas długiej kwerendy do 10 sekund i powoduje wyłączenie rejestrowania instrukcji powolne administratora. Ponadto zawiera listę opcji konfiguracji dla zapoznania się z nimi.
```azurecli-interactive
az mysql server configuration set --name slow_query_log --resource-group myresourcegroup --server myserver4demo --value ON
az mysql server configuration set --name long_query_time --resource-group myresourcegroup --server myserver4demo --value 10
az mysql server configuration set --name log_slow_admin_statements --resource-group myresourcegroup --server myserver4demo --value OFF
az mysql server configuration list --resource-group myresourcegroup --server myserver4demo
```

## <a name="list-logs-for-azure-database-for-mysql-server"></a>Lista dzienników bazy danych Azure MySQL serwera
Aby wyświetlić pliki dziennika dostępne na serwerze, uruchom [az mysql dzienniki serwera listy](/cli/azure/mysql/server-logs#az_mysql_server_logs_list) polecenia.

Można wyświetlić listę plików dziennika dla serwera **myserver4demo.mysql.database.azure.com** w grupie zasobów **myresourcegroup**i bezpośrednie jego tekstu w pliku o nazwie **dziennika\_plików \_lista.txt.**
```azurecli-interactive
az mysql server-logs list --resource-group myresourcegroup --server myserver4demo > log_files_list.txt
```
## <a name="download-logs-from-the-server"></a>Pobierz dzienniki z serwera
[Pobierz dzienniki serwera az mysql](/cli/azure/mysql/server-logs#az_mysql_server_logs_download) polecenie umożliwia pobieranie osobnych plikach dziennika dla serwera. 

W tym przykładzie pliki do pobrania określonego pliku dziennika dla serwera **myserver4demo.mysql.database.azure.com** w grupie zasobów **myresourcegroup** do środowiska lokalnego.
```azurecli-interactive
az mysql server-logs download --name 20170414-myserver4demo-mysql.log --resource-group myresourcegroup --server myserver4demo
```

## <a name="next-steps"></a>Następne kroki
- Dowiedz się więcej o [dzienników serwera w bazie danych Azure dla programu MySQL](concepts-server-logs.md)
