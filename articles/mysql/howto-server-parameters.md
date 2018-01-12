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
ms.date: 10/10/2017
ms.openlocfilehash: f3b32c1f6b33bc60b50f1496414a300db468dc92
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Jak skonfigurować parametry serwera w bazie danych Azure dla programu MySQL przy użyciu portalu Azure

Bazy danych platformy Azure dla programu MySQL obsługuje konfigurowanie niektórych parametrów serwera. W tym temacie opisano sposób konfigurowania tych parametrów przy użyciu portalu Azure. Nie wszystkie parametry serwera można dostosować. 

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Przejdź do parametrów serwera w portalu Azure
1. Zaloguj się do portalu Azure, a następnie zlokalizuj bazy danych Azure, aby serwer MySQL.
2. W obszarze **ustawienia** kliknij **parametry serwera** aby otworzyć stronę parametry serwera bazy danych Azure dla programu MySQL.
3. Znajdź wszystkie ustawienia, które należy dostosować. Przegląd **opis** kolumny zrozumienie przeznaczenia i dozwolone wartości. 
4. Kliknij przycisk **zapisać** Aby zapisać zmiany.

![Blok parametrów serwera portalu Azure](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Lista parametrów można skonfigurować serwera

Lista parametrów obsługiwanym serwerze stale rośnie. Karta serwera parametrów w portalu Azure można pobrać definicji i skonfiguruj parametry serwera, w zależności od wymagań aplikacji. 

## <a name="nonconfigurable-server-parameters"></a>Parametry serwera Nonconfigurable
Pula buforów InnoDB i maksymalna liczba połączeń nie są konfigurowalne i związane z [warstwy cenowej](concepts-service-tiers.md). 

| **Warstwa cenowa** | **Pula buforów InnoDB (MB)** | **Maksymalna liczba połączeń** |
| :------------------------ | :-------- | :----------- |
| Podstawowe 50 | 1024 | 50 | 
| Podstawowe 100  | 2560 | 100 | 
| Standardowa 100 | 2560 | 200 | 
| Standardowa 200 | 5120 | 400 | 
| Standardowa 400 | 10240 | 800 | 
| Standardowa 800 | 20480 | 1600 |

Parametry te dodatkowego serwera są nonconfigurable w systemie <br>
 Innodb_file_per_table w warstwie podstawowa: wyłączanie<br>
 innodb_flush_log_at_trx_commit = 1<br>
 sync_binlog = 1<br>
 innodb_log_file_size = 512MB<br>
 
Inne parametry serwera, które nie są wymienione w tym miejscu są ustawione na wartości domyślne out-of-box MySQL dla wersji [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) i [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html).

## <a name="next-steps"></a>Kolejne kroki
- [Biblioteki połączeń dla bazy danych Azure dla programu MySQL](concepts-connection-libraries.md).
