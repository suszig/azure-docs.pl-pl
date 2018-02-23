---
title: Baza danych SQL automatycznego, geograficznie nadmiarowego kopii zapasowych Azure | Dokumentacja firmy Microsoft
description: "Baza danych SQL automatycznie tworzy kopię zapasową lokalnej bazy danych co kilka minut i korzysta z magazynu geograficznie nadmiarowego Azure dostęp do odczytu dla nadmiarowość geograficzna."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3ee3d49d-16fa-47cf-a3ab-7b22aa491a8d
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Active
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 57132367faaa4c4869f2415f32897562c8056fb1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="learn-about-automatic-sql-database-backups"></a>Więcej informacji na temat automatycznego tworzenia kopii zapasowej bazy danych SQL

Baza danych SQL automatycznie tworzy kopie zapasowe bazy danych i używa magazynu geograficznie nadmiarowego Azure dostęp do odczytu (RA-GRS), aby zapewnić nadmiarowość geograficzna. Te kopie zapasowe są tworzone automatycznie i bez dodatkowych opłat. Nie trzeba wykonywać żadnych czynności, aby były w stanie. Kopie zapasowe bazy danych są integralną część każdej strategii odzyskiwania biznesowych, jak ciągłości i odzyskiwaniem po awarii, ponieważ ich ochronę danych przed przypadkowym uszkodzenia lub usunięcia. Jeśli chcesz przechowywać kopie zapasowe w własne kontenera magazynu można skonfigurować zasadę długoterminowego przechowywania kopii zapasowych. Aby uzyskać więcej informacji, zobacz [Długoterminowe przechowywanie](sql-database-long-term-retention.md).

## <a name="what-is-a-sql-database-backup"></a>Co to jest kopia zapasowa bazy danych SQL?

Baza danych SQL używa technologii SQL Server w celu utworzenia [pełne](https://msdn.microsoft.com/library/ms186289.aspx), [różnicowej](https://msdn.microsoft.com/library/ms175526.aspx), i [dziennika transakcji](https://msdn.microsoft.com/library/ms191429.aspx) kopii zapasowych. Kopie zapasowe dziennika transakcji nastąpić zazwyczaj co 5 – 10 minut, przy częstotliwości na podstawie poziomu wydajności i ilości aktywności bazy danych. Kopie zapasowe dziennika transakcji, o pełne i różnicowe kopie zapasowe, można przywrócić bazę danych do określonego punktu w czasie na tym samym serwerze, który jest hostem bazy danych. Po przywróceniu bazy danych usługi danych liczbowych limit które dziennika pełnej, różnicowej i transakcji konieczne jest przywrócenie kopii zapasowych.


Te kopie zapasowe, można użyć:

* Przywróć bazę danych do punktu w czasie w okresie przechowywania. Ta operacja spowoduje utworzenie nowej bazy danych, w tym samym serwerze co oryginalnej bazy danych.
* Przywracanie usuniętej bazy danych do czasu, który został usunięty lub kiedykolwiek okresem przechowywania. W tym samym serwerze, na którym utworzono oryginalnej bazy danych można przywrócić tylko usunięte bazy danych.
* Przywróć bazę danych do innego regionu geograficznego. Dzięki temu można odzyskać z geograficzne po awarii, gdy nie można uzyskać dostępu do serwera i bazy danych. Tworzy nową bazę danych w dowolnym istniejącego serwera miejscu na świecie. 
* Przywróć bazę danych z kopii zapasowej określonych przechowywane w magazynie usług odzyskiwania Azure. Dzięki temu można przywrócić starą wersję bazy danych, aby wykonać żądanie zgodności lub uruchomić starą wersję aplikacji. Zobacz [przechowywania długoterminowego](sql-database-long-term-retention.md).
* Aby wykonać operację przywracania, zobacz [przywrócić bazę danych z kopii zapasowych](sql-database-recovery-using-backups.md).

> [!NOTE]
> W magazynie Azure określenie *replikacji* odwołuje się do kopiowania plików z jednej lokalizacji do innej. SQL do *replikacji bazy danych* odwołuje się do przechowywania wiele pomocniczych baz danych synchronizowane z podstawowej bazy danych. 
> 

## <a name="how-often-do-backups-happen"></a>Jak często stanie kopie zapasowe?
Kopie zapasowe pełnej bazy danych są wykonywane co tydzień, kopie zapasowe różnicowej bazy danych są zazwyczaj wykonywane co kilka godzin, a dziennik transakcji, których kopie zapasowe są zazwyczaj wykonywane co 5 – 10 minut. Pierwsza pełna kopia zapasowa jest zaplanowane, natychmiast po utworzeniu bazy danych. Zazwyczaj wykonuje w ciągu 30 minut, ale może to trwać dłużej baza danych jest znaczne rozmiary. Na przykład początkowa kopia zapasowa może trwać dłużej w przywróconej bazy danych lub kopii bazy danych. Po pierwszym pełną kopię zapasową wszystkie dodatkowe kopie zapasowe są zaplanowane automatycznie i zarządzane w trybie dyskretnym w tle. Dokładny czas wszystkie kopie zapasowe bazy danych jest określana przez usługi SQL Database go jako ogólną obciążenia systemu. 

Replikacja geograficzna magazynu kopii zapasowych występuje na podstawie harmonogramu replikacji usługi Azure Storage.

## <a name="how-long-do-you-keep-my-backups"></a>Jak długo zachować kopiach zapasowych?
Każdej kopii zapasowej bazy danych SQL ma okres przechowywania, która jest oparta na [warstwy usług](sql-database-service-tiers.md) bazy danych. Okres przechowywania bazy danych w:


* Warstwy usług podstawowa wynosi 7 dni.
* Standardowa usługa warstwa jest 35 dni.
* Warstwy usług Premium jest 35 dni.

Czy można obniżyć bazy danych z warstwy standardowa lub Premium usługi do warstwy podstawowa, kopie zapasowe są zapisywane na siedem dni. Wszystkie istniejące kopie zapasowe starsze niż 7 dni nie są już dostępne. 

Uaktualnienie bazy danych z warstwy podstawowej usługi do standardowa lub Premium, bazy danych SQL przechowuje istniejące kopie zapasowe, aż do ich 35 dni temu. Zapewnia nowe kopie zapasowe w miarę ich występowania 35 dni.

Usunięcie bazy danych, bazy danych SQL przechowuje kopie zapasowe w taki sam sposób miałoby to miejsce dla bazy danych online. Na przykład załóżmy, że należy usunąć podstawowa baza danych ma okres przechowywania wynosi siedem dni. Kopię zapasową, która jest czterech dni są zapisywane dla trzech dni.

> [!IMPORTANT]
> Usunięcie serwera Azure SQL hostującym bazy danych SQL, wszystkie bazy danych, które należą do tego serwera, również zostaną usunięte i nie może zostać odzyskany. Nie można przywrócić usuniętego serwera.
> 

## <a name="how-to-extend-the-backup-retention-period"></a>Jak rozszerzyć okres przechowywania kopii zapasowej?
Jeśli aplikacja wymaga czy kopie zapasowe będą dostępne do czasu można rozszerzyć okres przechowywania wbudowanych konfigurując długoterminowo zasad przechowywania kopii zapasowych dla pojedynczych baz danych (zasada od lewej do prawej). Dzięki temu można przedłużyć okres przechowywania wbudowane it od 35 dni do maksymalnie 10 lat. Aby uzyskać więcej informacji, zobacz [Długoterminowe przechowywanie](sql-database-long-term-retention.md).

Po dodaniu zasad od lewej do prawej z bazą danych przy użyciu portalu Azure lub interfejsu API cotygodniowe kopie zapasowe pełnej bazy danych będą automatycznie skopiowane do własnych magazynu usługi Kopia zapasowa Azure. Jeśli baza danych jest szyfrowany przy funkcji TDE kopie zapasowe są automatycznie szyfrowane, gdy.  Magazyn usług automatycznie usunie kopie zapasowe wygasłe na podstawie ich sygnatury czasowej i zasad od lewej do prawej.  Więc nie trzeba martwić usuwania starych plików oraz zarządzania nimi harmonogram tworzenia kopii zapasowych. Interfejs API przywracania obsługuje kopie zapasowe przechowywane w magazynie, dopóki magazyn jest w tej samej subskrypcji co baza danych SQL. Portalu Azure lub programu PowerShell umożliwia dostęp do tych kopii zapasowych.

> [!TIP]
> Aby uzyskać przewodnik, zobacz [Konfigurowanie i przywrócenie z długoterminowego przechowywania kopii zapasowych bazy danych SQL Azure](sql-database-long-term-backup-retention-configure.md)
>

## <a name="are-backups-encrypted"></a>Kopie zapasowe są szyfrowane?

Po włączeniu funkcji TDE dla bazy danych Azure SQL kopii zapasowych również są szyfrowane. Wszystkie nowe bazy danych Azure SQL korzystają z funkcji TDE domyślnie włączone. Aby uzyskać więcej informacji o funkcji TDE, zobacz [przezroczystego szyfrowania danych z bazy danych SQL Azure](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="next-steps"></a>Kolejne kroki

- Kopie zapasowe bazy danych są integralną część każdej strategii odzyskiwania biznesowych, jak ciągłości i odzyskiwaniem po awarii, ponieważ ich ochronę danych przed przypadkowym uszkodzenia lub usunięcia. Aby poznać inne bazy danych SQL Azure firm ciągłości rozwiązania, zobacz temat [omówienie ciągłości działalności biznesowej](sql-database-business-continuity.md).
- Aby przywrócić do punktu w czasie przy użyciu portalu Azure, zobacz [przywrócenie bazy danych do punktu w czasie przy użyciu portalu Azure](sql-database-recovery-using-backups.md).
- Aby przywrócić do punktu w czasie przy użyciu programu PowerShell, zobacz [przywrócenie bazy danych do punktu w czasie przy użyciu programu PowerShell](scripts/sql-database-restore-database-powershell.md).
- Do konfigurowania, zarządzania i przywrócenie z długoterminowego przechowywania automatycznego tworzenia kopii zapasowych w magazynie usług odzyskiwania Azure przy użyciu portalu Azure, zobacz [Zarządzanie długoterminowego przechowywania kopii zapasowych przy użyciu portalu Azure](sql-database-long-term-backup-retention-configure.md).
- Do konfigurowania, zarządzania i przywrócenie z długoterminowego przechowywania automatycznego tworzenia kopii zapasowych w magazynie usług odzyskiwania Azure przy użyciu programu PowerShell, zobacz [Zarządzanie długoterminowego przechowywania kopii zapasowych przy użyciu programu PowerShell](sql-database-long-term-backup-retention-configure.md).
