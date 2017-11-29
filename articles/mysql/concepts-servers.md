---
title: "Pojęcia dotyczące serwera bazy danych Azure dla programu MySQL | Dokumentacja firmy Microsoft"
description: "Ten temat zawiera zagadnienia i wskazówki dotyczące pracy z bazą danych Azure dla serwerów MySQL."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 11/27/2017
ms.openlocfilehash: d3de3fdf28997b63321bf23443472db43ebb5c52
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="server-concepts-in-azure-database-for-mysql"></a>Pojęcia dotyczące serwera bazy danych Azure dla programu MySQL
Ten artykuł zawiera zagadnienia i wskazówki dotyczące pracy z bazą danych Azure dla serwerów MySQL.

## <a name="what-is-an-azure-database-for-mysql-server"></a>Co to jest Azure bazy danych serwera MySQL?

Baza danych Azure MySQL serwera jest centralny punkt administracyjny dla wielu baz danych. Jest tym samym konstrukcja MySQL serwera można zapoznać się z na świecie lokalnych. W szczególności bazą danych Azure dla usługi MySQL jest zarządzany, zapewnia gwarancje wydajności i ujawnia dostępu i funkcji na poziomie serwera.

Azure bazy danych MySQL serwera:

- Zostanie utworzone w subskrypcji platformy Azure.
- Jest zasobem nadrzędnej dla baz danych.
- Obejmuje przestrzeń nazw dla baz danych.
- To kontener z semantyki silne istnienia — usuwanie serwera i usuwa zawartych baz danych.
- Collocates zasoby w regionie.
- Udostępnia punkt końcowy połączenia dla serwera i dostęp do bazy danych.
- Określa zakres dla zasad zarządzania, które są stosowane do jej baz danych: logowania, zapory, użytkowników, ról, konfiguracje itd.
- Jest dostępna w różnych wersjach. Aby uzyskać więcej informacji, zobacz [obsługiwane bazy danych Azure dla wersji bazy danych MySQL](./concepts-supported-versions.md).

Na serwerze usługi Azure Database for MySQL można utworzyć jedną lub wiele baz danych. Możesz wybrać opcję tworzenia pojedynczej bazy danych na serwer, aby korzystać ze wszystkich zasobów lub utworzyć wiele baz danych, aby udostępnić zasoby. Cennik jest strukturalnych dla poszczególnych serwerów, na podstawie konfiguracji cen warstwy, jednostki obliczeniowe i magazyn (GB). Aby uzyskać więcej informacji, zobacz [warstw cenowych](./concepts-service-tiers.md).

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-mysql-server"></a>Jak połączyć i uwierzytelniania do bazy danych MySQL serwera Azure?

Następujące elementy zapewnić bezpieczny dostęp do bazy danych.
|||
| :-- | :-- |
| **Uwierzytelnianie i autoryzacja** | Azure bazy danych MySQL serwera obsługuje natywnych uwierzytelnianie MySQL. Możesz łączyć i uwierzytelniania na serwerze z nazwą logowania administratora serwera. |
| **Protokół** | Usługa obsługuje protokół oparta na komunikatach używany przez MySQL. |
| **PROTOKÓŁ TCP/IP** | Protokół jest obsługiwany za pośrednictwem protokołu TCP/IP i za pośrednictwem gniazda domeny systemu Unix. |
| **Zapora** | Aby chronić dane, reguła zapory uniemożliwia wszystkie dostęp do serwer bazy danych, do momentu określenia komputery, które ma uprawnienia. Zobacz [bazą danych Azure dla reguł zapory serwera MySQL](./concepts-firewall-rules.md). |
| **PROTOKÓŁ SSL** | Usługa obsługuje wymuszenie połączenia SSL między aplikacjami a serwerem bazy danych.  Zobacz [Konfigurowanie łączności SSL w aplikacji w celu bezpiecznego nawiązywania połączeń z usługą Azure Database for MySQL](./howto-configure-ssl.md). |

## <a name="how-do-i-manage-a-server"></a>Jak zarządzać serwerem?
Baza danych Azure dla serwerów MySQL można zarządzać za pomocą portalu Azure lub interfejsu wiersza polecenia Azure.

## <a name="next-steps"></a>Następne kroki
- Aby zapoznać się z omówieniem usługi, zobacz [Azure bazy danych MySQL — omówienie](./overview.md)
- Aby uzyskać informacje dotyczące określonego zasobu Przydziały i ograniczenia na podstawie Twojej **warstwy usług**, zobacz [warstwy usług](./concepts-service-tiers.md)
- Aby uzyskać informacje dotyczące połączenia z usługą, zobacz [biblioteki połączeń dla bazy danych Azure dla programu MySQL](./concepts-connection-libraries.md).
