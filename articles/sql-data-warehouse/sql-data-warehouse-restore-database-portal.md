---
title: "Przywracanie usługi Azure SQL Data Warehouse (Azure portal) | Dokumentacja firmy Microsoft"
description: Azure portalu zadania przywracania magazyn danych SQL Azure.
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: barbkess
editor: 
ms.assetid: b0aef539-7657-4b0e-9899-74098f5c21bc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 09/21/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: f6bc8671410dc7015a8d2a4bea1ba11f9ae526c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="restore-azure-sql-data-warehouse-portal"></a>Przywracanie usługi Azure SQL Data Warehouse (portal)
> [!div class="op_single_selector"]
> * [Omówienie][Overview]
> * [Portal][Portal]
> * [Środowiska PowerShell][PowerShell]
> * [REST][REST]
>
>
W tym artykule dowiesz się, jak przywrócić Azure SQL Data Warehouse przy użyciu portalu Azure.

## <a name="before-you-begin"></a>Przed rozpoczęciem
**Sprawdź, czy pojemność jednostek dtu w warstwie.** Każde wystąpienie usługi SQL Data Warehouse jest hostowana przez serwer SQL (na przykład myserver.database.windows.net), który ma domyślnego przydziału (bazy danych DTU) jednostki przepływności danych. Zanim będzie można przywrócić SQL Data Warehouse, sprawdź, czy program SQL server ma wystarczająco pozostałych limit przydziału jednostek DTU w przypadku przywracania bazy danych. Aby dowiedzieć się jak obliczyć limit przydziału jednostek DTU lub zażądać więcej jednostek Dtu, zobacz [żądanie zmiany limitu przydziału jednostek dtu w warstwie][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Przywróć bazę danych aktywnej lub wstrzymana
Przywracanie bazy danych:

1. Zaloguj się w witrynie [Azure Portal][Azure portal].
2. W okienku po lewej stronie wybierz **Przeglądaj**, a następnie wybierz **serwerów SQL**.

    ![Wybierz opcję Przeglądaj > serwery SQL](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Znajdź serwer, a następnie wybierz go.

    ![Wybierz serwer](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. Znajdź wystąpienie programu SQL Data Warehouse, który chcesz przywrócić z, a następnie wybierz go.

    ![Wybierz wystąpienie programu SQL magazynu danych do przywrócenia](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. W górnej części bloku hurtowni danych, wybierz **przywrócić**.

    ![Wybierz przywracania](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. Określ nowy **Nazwa bazy danych**.
7. Wybierz najnowszą **punkt przywracania**.

   Upewnij się, że została wybrana do ostatniego punktu przywracania. Ponieważ punkty przywracania są wyświetlane w uniwersalnego czasu koordynowanego (UTC), opcją domyślną może nie być do ostatniego punktu przywracania.

      ![Wybierz punkt przywracania](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. Kliknij przycisk **OK**.
9. Rozpocznie się proces przywracania bazy danych, a następnie można użyć **powiadomienia** do monitorowania procesu.

> [!NOTE]
> Po zakończeniu przywracania, można skonfigurować odzyskanej bazy danych, wykonując [konfiguracji bazy danych po odzyskaniu][Configure your database after recovery].
>
>

## <a name="restore-a-deleted-database"></a>Przywracanie usuniętej bazy danych
Aby przywrócić usunięte bazy danych:

1. Zaloguj się w witrynie [Azure Portal][Azure portal].
2. W okienku po lewej stronie wybierz **Przeglądaj**, a następnie wybierz **serwerów SQL**.

    ![Wybierz opcję Przeglądaj > serwery SQL](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Znajdź serwer, a następnie wybierz go.

    ![Wybierz serwer](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. Przewiń w dół do **operacji** sekcji w bloku serwera.
5. Wybierz **usunięte bazy danych** kafelka.

    ![Wybierz Kafelek usunięte bazy danych](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. Wybierz usuniętej bazy danych, który chcesz przywrócić.

    ![Wybierz bazę danych do przywrócenia](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. Określ nowy **Nazwa bazy danych**.

    ![Dodaj nazwę bazy danych](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. Kliknij przycisk **OK**.
9. Rozpocznie się proces przywracania bazy danych, a następnie można użyć **powiadomienia** do monitorowania procesu.

> [!NOTE]
> Aby skonfigurować bazę danych, po zakończeniu przywracania, zobacz [konfiguracji bazy danych po odzyskaniu][Configure your database after recovery].
>
>

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej o funkcjach ciągłości biznesowej wersji bazy danych SQL Azure, przeczytaj [omówienie ciągłości działalności biznesowej usługi Azure SQL Database][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/
