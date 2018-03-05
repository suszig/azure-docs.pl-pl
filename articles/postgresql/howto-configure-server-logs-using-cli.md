---
title: "Konfigurowanie i uzyskiwać dostęp do dzienników serwera dla PostgreSQL przy użyciu wiersza polecenia platformy Azure"
description: "W tym artykule opisano sposób konfigurowania i uzyskiwać dostęp do dzienników serwera w bazie danych Azure dla PostgreSQL przy użyciu wiersza polecenia z wiersza polecenia platformy Azure."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: e12a8907b641b4591ed5ff9fdd5d8458eb75525e
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Konfigurowanie i uzyskać dostępu do dzienników serwera przy użyciu wiersza polecenia platformy Azure
Możesz pobrać PostgreSQL dzienniki błędów serwera przy użyciu interfejsu wiersza polecenia (Azure CLI). Jednak dostęp do dzienników transakcji nie jest obsługiwany. 

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków opisanych ten przewodnik, potrzebne są:
- [PostgreSQL serwera bazy danych Azure](quickstart-create-server-database-azure-cli.md)
- Zainstaluj [Azure CLI 2.0](/cli/azure/install-azure-cli) narzędzia wiersza polecenia lub użyj powłoki chmury Azure w przeglądarce.

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Konfigurowanie rejestrowania w bazie danych Azure dla PostgreSQL
Można skonfigurować serwera dostępu do dzienników kwerend i dzienników błędów. Dzienniki błędów mogą zawierać informacje automatycznego próżni, połączenia i punkty kontrolne.
1. Włączanie rejestrowania
2. Dziennik aktualizacji\_instrukcji i dziennika\_min\_czas trwania\_instrukcji, aby włączyć rejestrowanie zapytań
3. Okres przechowywania aktualizacji

Aby uzyskać więcej informacji, zobacz [Dostosowywanie parametry konfiguracji serwera](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Lista dzienników bazy danych Azure PostgreSQL serwera
Aby wyświetlić pliki dziennika dostępne na serwerze, uruchom [az postgres dzienniki serwera listy](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) polecenia.

Można wyświetlić listę plików dziennika dla serwera **mydemoserver.postgres.database.azure.com** w grupie zasobów **myresourcegroup**i bezpośrednie jego tekstu w pliku o nazwie **dziennika\_ pliki\_lista.txt.**
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Pobierz dzienniki lokalnie z serwera
[Pobierz dzienniki serwera az postgres](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) polecenie umożliwia pobieranie osobnych plikach dziennika dla serwera. 

W tym przykładzie pliki do pobrania określonego pliku dziennika dla serwera **mydemoserver.postgres.database.azure.com** w grupie zasobów **myresourcegroup** do środowiska lokalnego.
```azurecli-interactive
az postgres server-logs download --name 20170414-mydemoserver-postgresql.log --resource-group myresourcegroup --server mydemoserver
```
## <a name="next-steps"></a>Kolejne kroki
- Aby dowiedzieć się więcej na temat dzienniki serwera, zobacz [dzienniki serwera w bazie danych PostgreSQL Azure](concepts-server-logs.md)
- Aby uzyskać więcej informacji na parametry serwera, zobacz [dostosować parametry konfiguracji serwera przy użyciu wiersza polecenia platformy Azure](howto-configure-server-parameters-using-cli.md)
