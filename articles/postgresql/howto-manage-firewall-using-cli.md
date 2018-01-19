---
title: "Tworzenie i zarządzanie bazą danych Azure dla reguł zapory PostgreSQL przy użyciu wiersza polecenia platformy Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób tworzenia i zarządzania bazą danych Azure PostgreSQL reguł zapory przy użyciu wiersza polecenia z wiersza polecenia platformy Azure."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 01/18/2018
ms.openlocfilehash: 233479bff0a0d331161b52d33f31b45c5714362b
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Tworzenie i zarządzanie bazą danych Azure dla reguł zapory PostgreSQL przy użyciu wiersza polecenia platformy Azure
Reguły zapory poziomu serwera umożliwiają administratorom zarządzanie dostępem do bazy danych Azure PostgreSQL serwera z określonego adresu IP lub zakresu adresów IP. Za pomocą wygodny poleceń interfejsu wiersza polecenia Azure, możesz utworzyć, zaktualizować, Usuń listę i Pokaż reguły zapory do zarządzania serwerem. Omówienie bazy danych Azure dla reguł zapory PostgreSQL, zobacz [bazą danych Azure dla reguł zapory serwera PostgreSQL](concepts-firewall-rules.md)

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków opisanych ten przewodnik, potrzebne są:
- [Bazy danych Azure PostgreSQL serwera i bazy danych](quickstart-create-server-database-azure-cli.md).
- Zainstaluj [Azure CLI 2.0](/cli/azure/install-azure-cli) narzędzia wiersza polecenia lub użyj powłoki chmury Azure w przeglądarce.

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>Konfigurowanie reguł zapory dla bazy danych platformy Azure dla PostgreSQL
[Az postgres reguły zapory serwera-](/cli/azure/postgres/server/firewall-rule) polecenia są używane do konfigurowania reguł zapory.

## <a name="list-firewall-rules"></a>Lista reguł zapory 
Aby wyświetlić listę istniejących reguł zapory serwera, uruchom [listy reguły zapory serwera postgres az](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_list) polecenia.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mypgserver-20170401
```
Dane wyjściowe zawiera listę reguł zapory formatowania żadnego domyślnie w formacie JSON. Można użyć przełącznika `--output table` dla bardziej czytelnym formacie tabeli jako dane wyjściowe.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server-name mypgserver-20170401 --output table
```
## <a name="create-firewall-rule"></a>Tworzenie reguły zapory
Aby utworzyć nową regułę zapory na serwerze, uruchom [az postgres reguły zapory serwera — Utwórz](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) polecenia. 

Określając 0.0.0.0 jako `--start-ip-address` i 255.255.255.255 jako `--end-ip-address` zakresu, poniższy przykład umożliwia wszystkie adresy IP na dostęp do serwera **mypgserver 20170401.postgres.database.azure.com**
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowIpRange --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
Aby zezwolić na dostęp do pojedynczej adresu IP, należy podać ten sam adres, w `--start-ip-address` i `--end-ip-address`w tym przykładzie.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowSingleIpAddress --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Na sukces dane wyjściowe polecenia Wyświetla szczegóły reguły zapory, które zostały utworzone, domyślnie w formacie JSON. W przypadku awarii, dane wyjściowe zamiast tego zawiera komunikat o błędzie.

## <a name="update-firewall-rule"></a>Aktualizuj reguły zapory 
Aktualizuj istniejącą regułę zapory na serwerze przy użyciu [aktualizacja reguły zapory serwera postgres az](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_update) polecenia. Podaj nazwę istniejącej reguły zapory jako dane wejściowe i uruchom atrybutów IP adresów IP i końcowy do aktualizacji.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowIpRange --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.255
```
Na sukces dane wyjściowe polecenia Wyświetla szczegóły reguły zapory, które zostały zaktualizowane, domyślnie w formacie JSON. W przypadku awarii, dane wyjściowe zamiast tego zawiera komunikat o błędzie.
> [!NOTE]
> Jeśli reguły zapory nie istnieje, pobiera on utworzony za pomocą polecenia aktualizacji.

## <a name="show-firewall-rule-details"></a>Pokaż szczegóły reguły zapory
Można również wyświetlać szczegóły istniejącą regułę zapory poziomu serwera, uruchamiając [Pokaż reguły zapory serwera postgres az](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_show) polecenia.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowIpRange
```
Na sukces dane wyjściowe polecenia Wyświetla szczegóły reguły zapory, który został określony, domyślnie w formacie JSON. W przypadku awarii, dane wyjściowe zamiast tego zawiera komunikat o błędzie.

## <a name="delete-firewall-rule"></a>Usuwanie reguły zapory
Aby odwołać dostępu dla zakresu adresów IP do serwera, usuń istniejącą regułę zapory, wykonując [az postgres reguły zapory serwera-Usuń](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_delete) polecenia. Podaj nazwę istniejącej reguły zapory.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server-name mypgserver-20170401 --name AllowIpRange
```
Na sukces nie ma żadnych danych wyjściowych. W przypadku awarii zwracana jest tekst komunikatu o błędzie.

## <a name="next-steps"></a>Kolejne kroki
- Analogicznie, można użyć przeglądarki sieci web [tworzenie i zarządzanie bazą danych Azure PostgreSQL reguł zapory przy użyciu portalu Azure](howto-manage-firewall-using-portal.md).
- Dowiedzieć się więcej o [bazą danych Azure dla reguł zapory serwera PostgreSQL](concepts-firewall-rules.md).
- Aby uzyskać pomoc w nawiązywania połączenia z bazą danych Azure dla serwera PostgreSQL, [biblioteki połączeń dla bazy danych Azure dla PostgreSQL](concepts-connection-libraries.md).
