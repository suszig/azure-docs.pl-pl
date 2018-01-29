---
title: "Jak skonfigurować parametry serwera w bazie danych systemu Azure dla programu MySQL | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób konfigurowania parametrów serwera MySQL w bazie danych Azure dla programu MySQL przy użyciu portalu Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/25/2018
ms.openlocfilehash: 59eeed42356a276c259bd8da55890b7ada67d729
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/25/2018
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Jak skonfigurować parametry serwera w bazie danych Azure dla programu MySQL przy użyciu portalu Azure

Bazy danych platformy Azure dla programu MySQL obsługuje konfigurowanie niektórych parametrów serwera. W tym artykule opisano sposób konfigurowania tych parametrów przy użyciu portalu Azure. Nie wszystkie parametry serwera można dostosować. 

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Przejdź do parametrów serwera w portalu Azure
1. Zaloguj się do portalu Azure, a następnie zlokalizuj bazy danych Azure, aby serwer MySQL.
2. W obszarze **ustawienia** kliknij **parametry serwera** aby otworzyć stronę parametry serwera bazy danych Azure dla programu MySQL.
3. Znajdź wszystkie ustawienia, które należy dostosować. Przegląd **opis** kolumny zrozumienie przeznaczenia i dozwolone wartości. 
4. Kliknij przycisk **zapisać** Aby zapisać zmiany.

![Strona parametrów serwera portalu Azure](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Lista parametrów można skonfigurować serwera

Lista parametrów obsługiwanym serwerze stale rośnie. Karta serwera parametrów w portalu Azure można pobrać definicji i skonfiguruj parametry serwera, w zależności od wymagań aplikacji. 

## <a name="nonconfigurable-server-parameters"></a>Parametry serwera Nonconfigurable
Pula buforów InnoDB i maksymalna liczba połączeń nie są konfigurowalne i związane z [warstwy cenowej](concepts-service-tiers.md). 

| **Warstwa cenowa** | **Pula buforów InnoDB (MB)** | **Maksymalna liczba połączeń** |
| :------------------------ | :-------- | :----------- |
| Basic 50 | 1024 | 50 | 
| Basic 100  | 2560 | 100 | 
| Standardowa 100 | 2560 | 200 | 
| Standardowa 200 | 5120 | 400 | 
| Standardowa 400 | 10240 | 800 | 
| Standardowa 800 | 20480 | 1600 |

Te parametry dodatkowego serwera nie są konfigurowane w systemie:

|**Parameter**|**Stała wartość**|
| :------------------------ | :-------- |
|innodb_file_per_table w warstwie podstawowa|WYŁ.|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512 MB|

Inne parametry serwera, które nie są wymienione w tym miejscu są ustawione na wartości domyślne out-of-box MySQL dla wersji [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) i [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html).

## <a name="next-steps"></a>Kolejne kroki
- [Biblioteki połączeń dla bazy danych Azure dla programu MySQL](concepts-connection-libraries.md).
