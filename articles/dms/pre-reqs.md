---
title: "Przegląd wymagań wstępnych dotyczących używania usługi migracji bazy danych Azure | Dokumentacja firmy Microsoft"
description: "Zapoznaj się z omówieniem wymagań wstępnych do przeprowadzania migracji bazy danych przy użyciu usługi Azure bazy danych migracji."
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/25/2018
ms.openlocfilehash: a103bb4802bc4a20b6d82f0c6bb427133d9e5643
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/25/2018
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Przegląd wymagań wstępnych dotyczących używania usługi Azure bazy danych migracji
Istnieje kilka wymagań wstępnych dotyczących upewnić, że usługa migracji bazy danych Azure przeprowadzane bezproblemowego podczas przeprowadzania migracji bazy danych. Niektóre wymagania wstępne są stosowane we wszystkich scenariuszach (pary źródłowy i docelowy), obsługiwane przez usługę, podczas gdy inne wymagania wstępne są unikatowe dla danego scenariusza.

Wymagania wstępne skojarzone z korzystania z usługi migracji bazy danych Azure są wymienione w poniższych sekcjach.

## <a name="prerequisites-common-across-migration-scenarios"></a>Wymagania wstępne dotyczące typowych scenariuszy migracji
Azure wymagania wstępne usługi migracji bazy danych, które są wspólne dla wszystkich obsługiwane scenariusze migracji obejmują konieczność:
- Tworzenie sieci Wirtualnej dla usługi Azure migracji bazy danych przy użyciu modelu wdrażania usługi Azure Resource Manager, który zapewnia połączenie lokacja lokacja z serwerami lokalnymi źródła przy użyciu [ExpressRoute](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-introduction) lub [VPN](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Upewnij się, że Twoje Azure (VNET) sieciowej grupy zabezpieczeń sieci wirtualnej reguły blok następujący komunikat porty 443, 53, 9354, 445, 12000. Aby uzyskać więcej szczegółów na filtrowanie ruchu NSG sieci Wirtualnej Azure, zobacz artykuł [filtrowania ruchu sieciowego z grup zabezpieczeń sieci](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg).
- Podczas korzystania z urządzenia zapory przed baz danych z źródła, może być konieczne dodanie reguły zapory, aby umożliwić dostęp do bazy danych źródłowego do migracji z usługi migracji bazy danych Azure.

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Wymagania wstępne dotyczące migracji serwera SQL z bazą danych SQL Azure 
Oprócz wymagań wstępnych usługi migracji bazy danych Azure, które są wspólne dla wszystkich scenariuszy migracji są również wymagania wstępne, które dotyczą przede wszystkim jednego scenariusza lub innej.

Podczas korzystania z usługi migracji bazy danych Azure do wykonania programu SQL Server do migracji bazy danych SQL Azure, oprócz wymagań wstępnych, które są wspólne dla wszystkich scenariuszy migracji, należy rozwiązać następujące dodatkowe wymagania wstępne:
- Konfigurowanie sieci [zapory systemu Windows dla dostępu aparatu bazy danych](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Włącz protokół TCP/IP, które jest domyślnie wyłączone podczas instalacji programu SQL Server Express przez postępując zgodnie z instrukcjami w artykule [włączyć lub wyłączyć protokół sieciowy serwera](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Utwórz wystąpienie wystąpienia bazy danych SQL Azure, co zrobić, wykonując szczegółowo w artykule C[Utwórz bazę danych Azure SQL w portalu Azure](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-portal).
- Pobierz i zainstaluj [Asystenta migracji danych](https://www.microsoft.com/en-us/download/details.aspx?id=53595) v3.3 lub nowszym.
- Otwórz Zaporę systemu Windows tak, aby umożliwić dostęp do źródła baz danych z usługi migracji bazy danych Azure.
- Tworzenie z poziomu serwera [reguły zapory](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) dla serwera bazy danych SQL Azure, aby umożliwić dostęp z migracji bazy danych Azure do docelowymi bazami danych. Podaj zakres podsieci sieci wirtualnej używane przez usługę Azure migracji bazy danych.
- Upewnij się, że poświadczenia użyte do nawiązania połączenia źródła wystąpienia programu SQL Server [serwera kontroli](https://docs.microsoft.com/en-us/sql/t-sql/statements/grant-server-permissions-transact-sql) uprawnienia.
- Sprawdź, czy poświadczenia użyte do nawiązania połączenia docelowego wystąpienia bazy danych SQL Azure mają uprawnienia bazy danych kontroli na docelowymi bazami danych Azure SQL.

   > [!NOTE]
   > Aby uzyskać pełną listę wymagań wstępnych dotyczących korzystania z usługi migracji bazy danych Azure do przeprowadzenia migracji z programu SQL Server do bazy danych SQL Azure, zobacz samouczek [migracji programu SQL Server do bazy danych SQL Azure](https://docs.microsoft.com/en-us/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="next-steps"></a>Kolejne kroki
Omówienie usługi migracji bazy danych Azure i regionalnych dostępność w publicznej wersji zapoznawczej, zobacz artykuł [co to jest podgląd z migracji bazy danych Azure](dms-overview.md). 