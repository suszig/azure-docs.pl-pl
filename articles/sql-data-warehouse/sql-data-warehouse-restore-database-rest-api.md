---
title: "Przywróć magazyn danych Azure SQL (interfejsu API REST) | Dokumentacja firmy Microsoft"
description: "Interfejs API REST zadania przywracania usługi Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: fca922c6-b675-49c7-907e-5dcf26d451dd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: 8739429342d3c8bbe0f09041976758276a9c3228
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>Przywróć magazyn danych Azure SQL (interfejsu API REST)
> [!div class="op_single_selector"]
> * [Omówienie][Overview]
> * [Portal][Portal]
> * [Środowiska PowerShell][PowerShell]
> * [REST][REST]
> 
> 

W tym artykule dowiesz się, jak przywrócić Azure SQL Data Warehouse przy użyciu interfejsu API REST.

## <a name="before-you-begin"></a>Przed rozpoczęciem
**Sprawdź, czy pojemność jednostek dtu w warstwie.** Każdy magazyn danych SQL jest obsługiwana przez serwer SQL (np. myserver.database.windows.net), który ma domyślnego przydziału jednostek dtu w warstwie.  Zanim będzie można przywrócić magazyn danych SQL, upewnij się, że program SQL server ma wystarczającą ilość pozostałych limit przydziału jednostek DTU dla przywracanej bazy danych. Aby dowiedzieć się, jak można obliczyć jednostek dtu w warstwie potrzebne również z prośbami więcej jednostek dtu w warstwie, zobacz [żądanie zmiany limitu przydziału jednostek dtu w warstwie][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Przywróć bazę danych aktywnej lub wstrzymana
Przywracanie bazy danych:

1. Pobierz listę punktów przywracania bazy danych przy użyciu operacji punkty przywracania bazy danych Get.
2. Rozpocznij przywracania przy użyciu [żądanie przywracania bazy danych utwórz] [ Create database restore request] operacji.
3. Śledź stan przywracania przy użyciu [operacji stan bazy danych] [ Database operation status] operacji.

> [!NOTE]
> Po ukończeniu przywracania można skonfigurować odzyskanej bazy danych, wykonując [konfiguracji bazy danych po odzyskaniu][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Przywracanie usuniętej bazy danych
Aby przywrócić usunięte bazy danych:

1. Lista wszystkich dostępnych usuniętych baz danych przy użyciu [listy dostępnych porzucić bazy danych] [ List restorable dropped databases] operacji.
2. Szczegółowe informacje dla usuniętej bazy danych, aby przywrócić za pomocą [Get dostępnych porzucić bazy danych] [ Get restorable dropped database] operacji.
3. Rozpocznij przywracania przy użyciu [żądanie przywracania bazy danych utwórz] [ Create database restore request] operacji.
4. Śledź stan przywracania przy użyciu [operacji stan bazy danych] [ Database operation status] operacji.

> [!NOTE]
> Aby skonfigurować bazę danych, po ukończeniu przywracania, zobacz [konfiguracji bazy danych po odzyskaniu][Configure your database after recovery].
> 
> 

## <a name="next-steps"></a>Kolejne kroki
Aby dowiedzieć się więcej o funkcjach ciągłości biznesowej wersji bazy danych SQL Azure, przeczytaj [omówienie ciągłości działalności biznesowej usługi Azure SQL Database][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Create database restore request]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Database operation status]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Get restorable dropped database]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[List restorable dropped databases]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
