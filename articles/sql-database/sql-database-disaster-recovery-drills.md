---
title: Testowanie odzyskiwania po awarii bazy danych SQL | Dokumentacja firmy Microsoft
description: "Dowiedz się, wskazówki i najlepsze rozwiązania dotyczące korzystania z bazy danych SQL Azure przeprowadzić testowanie odzyskiwania po awarii, aby chronić Twoje misji krytycznych aplikacji LOB odporność na awarie i awarie."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: b44a269c-fe2a-404f-b013-290030860bd1
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/31/2016
ms.author: sashan
ms.openlocfilehash: 1b1d65a41a794a566287dcffe3581ac58e2a965f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="performing-disaster-recovery-drill"></a>Wykonywanie wyszczególniania odzyskiwania po awarii
Zalecane jest okresowo wykonywać weryfikacji aplikacja jest gotowa do przepływu pracy odzyskiwania. Weryfikowanie zachowanie aplikacji oraz wpływ utraty danych i/lub przerw w działaniu obejmuje czy tryb failover jest dobrym rozwiązaniem engineering. Również jest wymagane przez większość standardy branżowe w ramach certyfikacji ciągłości biznesowej.

Obejmuje wykonywania wyszczególniania odzyskiwania po awarii:

* Symulowanie symulacje awarii warstwy danych
* Odzyskiwanie
* Sprawdź poprawność odzyskiwania post integralności aplikacji

W zależności od tego, jak możesz [przeznaczony dla ciągłość prowadzenia działalności biznesowej aplikacji](sql-database-business-continuity.md), przepływu pracy można wykonać drążenie może się różnić. Poniżej opisano najważniejsze wskazówki przeprowadzenie wyszczególniania odzyskiwania po awarii, w kontekście bazy danych SQL Azure.

## <a name="geo-restore"></a>Przywracanie geograficzne
Aby zapobiec utracie danych podczas przeprowadzania wyszczególniania odzyskiwania po awarii, zaleca się przeprowadzania Przechodzenie do szczegółów, tworząc kopię w środowisku produkcyjnym i użycie go do sprawdzenia przepływu pracy awaryjnej aplikacji przy użyciu środowiska testowego.

#### <a name="outage-simulation"></a>Symulacji awarii
Aby symulować awarii, można usunąć lub zmienić nazwy źródłowej bazy danych. Powoduje to błędów łączności aplikacji.

#### <a name="recovery"></a>Odzyskiwanie
* Wykonać geograficznie przywracania bazy danych na innym serwerze, zgodnie z opisem [tutaj](sql-database-disaster-recovery.md).
* Zmień konfigurację aplikacji, aby nawiązać połączenie odzyskanej bazy danych i postępuj zgodnie z [skonfigurować bazę danych po odzyskaniu](sql-database-disaster-recovery.md) przewodniku, aby zakończyć odzyskiwanie.

#### <a name="validation"></a>Walidacja
* Zakończ Drąż weryfikowanie odzyskiwania post integralności aplikacji (w tym parametry połączenia, logowania, podstawowych funkcji testowania lub innych operacji sprawdzania poprawności część procedur signoffs standardowej aplikacji).

## <a name="geo-replication"></a>Replikacja geograficzna
Dla bazy danych, który jest chroniony za pomocą — replikacja geograficzna wykonywania Przechodzenie do szczegółów obejmuje planowany tryb failover na dodatkowej bazy danych. Planowany tryb failover zapewnia, że podstawowej i pomocniczej bazy danych pozostają zsynchronizowane podczas przełączania ról. Inaczej niż w przypadku nieplanowanego trybu failover ta operacja nie powoduje utraty danych, więc Drąż mogą być wykonywane w środowisku produkcyjnym.

#### <a name="outage-simulation"></a>Symulacji awarii
Aby symulować awarii, można wyłączyć maszyny wirtualnej, połączony z bazą danych lub aplikacji sieci web. W efekcie awarie połączenia dla klientów sieci web.

#### <a name="recovery"></a>Odzyskiwanie
* Upewnij się, Konfiguracja aplikacji w regionie DR wskazuje była dodatkowej, która staje się dostępny w pełni nową podstawową.
* Wykonaj [planowanego trybu failover](scripts/sql-database-setup-geodr-and-failover-database-powershell.md) dokonanie pomocniczej bazie danych nowego podstawowego
* Postępuj zgodnie z [skonfigurować bazę danych po odzyskaniu](sql-database-disaster-recovery.md) przewodniku, aby zakończyć odzyskiwanie.

#### <a name="validation"></a>Walidacja
* Zakończ Drąż weryfikowanie odzyskiwania post integralności aplikacji (w tym parametry połączenia, logowania, podstawowych funkcji testowania lub innych operacji sprawdzania poprawności część procedur signoffs standardowej aplikacji).

## <a name="next-steps"></a>Następne kroki
* Aby dowiedzieć się więcej o scenariuszach ciągłości biznesowej, zobacz [ciągłości scenariuszy](sql-database-business-continuity.md)
* Aby dowiedzieć się więcej na temat usługi Azure SQL bazy danych automatycznego tworzenia kopii zapasowych, zobacz [bazy danych SQL automatycznego tworzenia kopii zapasowych](sql-database-automated-backups.md)
* Aby dowiedzieć się więcej o używaniu kopie zapasowe automatycznego odzyskiwania, zobacz [przywrócić bazę danych z kopii zapasowych inicjowane przez usługę](sql-database-recovery-using-backups.md)
* Informacje na temat opcji odzyskiwania szybsze, zobacz [aktywna replikacja geograficzna](sql-database-geo-replication-overview.md)  
