---
title: Przechowywanie kopii zapasowych bazy danych SQL Azure przez maksymalnie 10 lat | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak baza danych SQL Azure obsługuje przechowywania kopii zapasowych maksymalnie 10 lat."
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.date: 12/22/2016
ms.author: sashan
ms.openlocfilehash: 2f31e89fce2746e57d6a670aef949d0d534af4c1
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a>Przechowywanie kopii zapasowych bazy danych SQL Azure przez maksymalnie 10 lat
Wiele aplikacji ma przepisów prawnych, zgodności lub innych firm celów, które wymagają przechowywania kopii zapasowych bazy danych poza 7 35 dni pochodzącymi z bazy danych SQL Azure [automatycznego tworzenia kopii zapasowych](sql-database-automated-backups.md). Za pomocą funkcji długoterminowego przechowywania kopii zapasowych, można przechowywać kopie zapasowe bazy danych SQL w magazynie usług odzyskiwania Azure przez maksymalnie 10 lat. Można przechowywać maksymalnie 1000 baz danych na magazynie. Następnie można wybrać jakiejkolwiek kopii zapasowej w magazynie, aby przywrócić go jako nową bazę danych.

> [!IMPORTANT]
> Długoterminowego przechowywania kopii zapasowych jest obecnie w wersji zapoznawczej i jest dostępna w następujących regionach: Australia Wschodnia, Australia Południowo-Wschodnia, Brazylia Południowa, środkowe stany USA, Azja Wschodnia, wschodnie stany USA, wschodnie stany USA 2, Indie środkowe, Indie Południowe, Japonia Wschodnia, Japonia Zachodnia, północno-środkowe stany, Północna Europa, Południowej środkowe stany USA, Azja południowo-wschodnia, Europa Zachodnia i zachodnie stany USA
>

> [!NOTE]
> Maksymalnie 200 baz danych na magazyn można włączyć w 24-godzinnym przedziale czasu. Zalecane jest użycie oddzielnych magazynu dla każdego serwera, aby zminimalizować wpływ ten limit. 
> 

## <a name="how-sql-database-long-term-backup-retention-works"></a>Jak działa długoterminowego przechowywania kopii zapasowych bazy danych SQL

Z długoterminowego przechowywania kopii zapasowych można skojarzyć serwer bazy danych SQL z magazynu usług odzyskiwania Azure. 

* Należy utworzyć magazyn do tej samej subskrypcji platformy Azure, utworzony programu SQL server i w tym samym regionie geograficznym i grupy zasobów. 
* Następnie skonfiguruj zasady przechowywania dla dowolnej bazy danych. Zasad powoduje, że cotygodniowe kopie zapasowe pełnej bazy danych należy skopiować do magazynu usług odzyskiwania i przechowywane przez okres przechowywania określony (maksymalnie 10 lat). 
* Następnie można przywrócić bazy danych z dowolnego z tych kopii zapasowych do nowej bazy danych w dowolnego serwera w ramach subskrypcji. Magazynu Azure tworzy kopię na podstawie istniejących kopii zapasowych, a kopia nie ma wpływu wydajności w istniejącej bazie danych.

> [!TIP]
> Aby uzyskać przewodnik, zobacz [Konfigurowanie i przywrócenie z długoterminowego przechowywania kopii zapasowych bazy danych SQL Azure](sql-database-long-term-backup-retention-configure.md).

## <a name="enable-long-term-backup-retention"></a>Włącz długoterminowego przechowywania kopii zapasowych

Aby skonfigurować długoterminowego przechowywania kopii zapasowych bazy danych:

1. Tworzenie magazynu usług odzyskiwania Azure w tym samym regionie, subskrypcji i grupy zasobów jako serwer bazy danych SQL. 
2. Zarejestruj serwer w magazynie.
3. Tworzenie zasad ochrony usług odzyskiwania Azure.
4. Zastosowanie zasad ochrony do baz danych, które wymagają długoterminowego przechowywania kopii zapasowych.

Konfigurowanie, zarządzanie i przywracanie bazy danych z długoterminowego przechowywania kopii zapasowych automatycznych kopii zapasowych w magazynie usług odzyskiwania Azure, wykonaj jedną z następujących czynności:

* Przy użyciu portalu Azure: kliknij **długoterminowego przechowywania kopii zapasowych**, wybierz bazę danych, a następnie kliknij przycisk **Konfiguruj**. 

   ![Wybierz bazę danych do długoterminowego przechowywania kopii zapasowych](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

* Przy użyciu programu PowerShell: Przejdź do [Konfigurowanie i przywrócenie z długoterminowego przechowywania kopii zapasowych bazy danych SQL Azure](sql-database-long-term-backup-retention-configure.md).

## <a name="restore-a-database-thats-stored-with-the-long-term-backup-retention-feature"></a>Przywracanie bazy danych przechowywanych funkcją długoterminowego przechowywania kopii zapasowych

Aby odzyskać z kopii zapasowej długoterminowego przechowywania kopii zapasowych:

1. Wyświetl listę magazynu, w której jest przechowywana kopia zapasowa.
2. Lista kontenera, który jest zamapowany z serwerem logicznym.
3. Lista źródła danych w ramach magazynu, który jest mapowany do bazy danych.
4. Lista punktów odzyskiwania, które są dostępne do przywrócenia.
5. Przywracanie bazy danych z punktu odzyskiwania na serwerze docelowym w ramach subskrypcji.

Konfigurowanie, zarządzanie i przywracanie bazy danych z długoterminowego przechowywania kopii zapasowych automatycznych kopii zapasowych w magazynie usług odzyskiwania Azure, wykonaj jedną z następujących czynności:

* Przy użyciu portalu Azure: Przejdź do [Zarządzanie długoterminowego przechowywania kopii zapasowych przy użyciu portalu Azure](sql-database-long-term-backup-retention-configure.md). 

* Przy użyciu programu PowerShell: Przejdź do [Zarządzanie długoterminowego przechowywania kopii zapasowych przy użyciu programu PowerShell](sql-database-long-term-backup-retention-configure.md).

## <a name="get-pricing-for-long-term-backup-retention"></a>Pobierz ceny dla długoterminowego przechowywania kopii zapasowych

Długoterminowego przechowywania kopii zapasowych bazy danych SQL jest rozliczana zgodnie z [usługi kopii zapasowej Azure cennik stawki](https://azure.microsoft.com/pricing/details/backup/).

Po zarejestrowaniu serwera bazy danych SQL w magazynie są naliczane opłaty za całkowita ilość miejsca, który jest używany przez cotygodniowe kopie zapasowe przechowywane w magazynie.

## <a name="view-available-backups-that-are-stored-in-long-term-backup-retention"></a>Wyświetl dostępne kopie zapasowe, które są przechowywane w długoterminowego przechowywania kopii zapasowych

Konfigurowanie, zarządzanie i przywrócić bazę danych z długoterminowego przechowywania kopii zapasowych automatycznych kopii zapasowych w magazynie usług odzyskiwania Azure przy użyciu portalu Azure, wykonaj jedną z następujących czynności:

* Przy użyciu portalu Azure: Przejdź do [Zarządzanie długoterminowego przechowywania kopii zapasowych przy użyciu portalu Azure](sql-database-long-term-backup-retention-configure.md). 

* Przy użyciu programu PowerShell: Przejdź do [Zarządzanie długoterminowego przechowywania kopii zapasowych przy użyciu programu PowerShell](sql-database-long-term-backup-retention-configure.md).

## <a name="disable-long-term-retention"></a>Wyłącz długoterminowego przechowywania

Usługi odzyskiwania obsługuje automatycznie Oczyszczanie na podstawie zasad podanych przechowywania kopii zapasowych. 

Aby zatrzymać wysyłanie kopii zapasowych dla określonej bazy danych do magazynu, należy usunąć zasady przechowywania dla tej bazy danych.
  
```
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName 'RG1' -ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
```

> [!NOTE]
> Nie dotyczy to kopie zapasowe, które znajdują się już w magazynie. Zostaną one automatycznie usunięte przez usługi odzyskiwania po wygaśnięciu okresu przechowywania.

## <a name="long-term-backup-retention-faq"></a>Długoterminowego przechowywania kopii zapasowych — często zadawane pytania

**Można ręcznie usunąć z określonych kopii zapasowych w magazynie?**

Nie można obecnie. Magazyn automatycznie oczyszcza kopii zapasowych, po zakończeniu okresu przechowywania.

**Można zarejestrować serwer do przechowywania kopii zapasowych do więcej niż jeden Magazyn?**

Nie, w obecnie można przechowywać kopie zapasowe, aby tylko jeden magazyn, w czasie.

**W ramach różnych subskrypcji może mieć magazyn i serwera?**

Nie, obecnie magazyn i serwer musi być w tej samej subskrypcji i grupy zasobów.

**Można użyć magazynu, który został utworzony w regionie, który jest inny niż region Mój serwer?**

Nie, magazynu i serwer muszą być w tym samym regionie, aby zminimalizować czas kopiowania i zapobiec zmianom ruchu.

**Jak wiele baz danych można przechowywać w jednym magazynie?**

Obecnie usługa obsługuje maksymalnie 1000 baz danych na magazynie. 

**Jak wiele magazynów można utworzyć na subskrypcję?**

Możesz utworzyć maksymalnie 25 magazynów dla subskrypcji.

**Jak wiele baz danych można skonfigurować na dzień na magazyn?**

Można skonfigurować 200 bazy danych dziennie na magazynie.

**Długoterminowego przechowywania kopii zapasowych działa z pul elastycznych?**

Tak. Wszystkie bazy danych w puli można skonfigurować za pomocą zasad przechowywania.

**Można wybrać czas, o której została utworzona kopia zapasowa?**

Nie, baza danych SQL określa harmonogram tworzenia kopii zapasowych, aby zminimalizować wpływ na wydajność w bazach danych.

**Masz przezroczystego szyfrowania danych włączone dla bazy danych. Można jej używać w magazynie?** 

Tak, przezroczystego szyfrowania danych jest obsługiwane. Nawet jeśli oryginalnej bazy danych już nie istnieje, można przywrócić bazy danych z magazynu.

**Co się stanie w przypadku kopii zapasowej w magazynie, jeśli Moja subskrypcja jest zawieszona?** 

Jeśli subskrypcja jest zawieszona, możemy zachować istniejące bazy danych i kopii zapasowych. Nowe kopie zapasowe nie są kopiowane do magazynu. Po aktywowaniu subskrypcji usługi wznawia kopiowania do magazynu kopii zapasowych. Magazyn staje się dostępny dla operacji przywracania przy użyciu kopii zapasowych, które zostały skopiowane przed subskrypcji zostało zawieszone. 

**Czy można uzyskać dostępu do plików kopii zapasowej bazy danych SQL, można pobrać lub przywrócić je do programu SQL server?**

Nie, nie jest obecnie.

**Może mieć wielu harmonogramów (codziennie, co tydzień, co miesiąc, co rok) w ramach zasad przechowywania SQL.**

Nie, wielu harmonogramów są obecnie dostępne tylko dla kopii zapasowych maszyn wirtualnych.

**Co zrobić, jeśli skonfigurowanie długoterminowego przechowywania kopii zapasowych w bazie danych, która jest aktywna replikacja geograficzna dodatkowej bazy danych dla?**

Ponieważ firma Microsoft nie wykonuj kopie zapasowe replik, nie ma opcja długoterminowego przechowywania kopii zapasowych z dodatkowej bazy danych. Jednak ważne jest, aby umożliwić użytkownikom konfigurowanie długoterminowego przechowywania kopii zapasowej w pomocniczej bazie danych aktywna replikacja geograficzna z tego względu:
* Po przejściu w tryb failover wykonywany i baza danych będzie podstawowej bazy danych, możemy zająć pełnej kopii zapasowej, który zostanie przekazany do magazynu.
* Jest nie żadnymi dodatkowymi kosztami klientowi konfigurowania długoterminowego przechowywania kopii zapasowej w pomocniczej bazie danych.

## <a name="next-steps"></a>Kolejne kroki
Ponieważ kopie zapasowe bazy danych ochronę danych przed przypadkowym uszkodzenia lub usunięcia, są one integralną część wszelkie ciągłość prowadzenia działalności biznesowej i strategii odzyskiwania po awarii. Aby dowiedzieć się więcej o innych rozwiązań ciągłość prowadzenia działalności biznesowej bazy danych SQL, zobacz [omówienie ciągłości działalności biznesowej](sql-database-business-continuity.md).
