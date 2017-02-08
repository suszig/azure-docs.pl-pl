---
title: "Ciągłość działalności biznesowej w chmurze — odzyskiwanie bazy danych — usługa SQL Database | Microsoft Docs"
description: "Dowiedz się, w jaki sposób usługa Azure SQL Database obsługuje ciągłość działalności biznesowej w chmurze i odzyskiwanie bazy danych oraz pomaga zapewnić działanie aplikacji o kluczowym znaczeniu w chmurze."
keywords: business continuity,cloud business continuity,database disaster recovery,database recovery
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 18e5d3f1-bfe5-4089-b6fd-76988ab29822
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/13/2016
ms.author: sashan
translationtype: Human Translation
ms.sourcegitcommit: 187954f3ddafdbc17e341ce41f5b109cb95f8a24
ms.openlocfilehash: d04eb659bcf4df53c5f44c1a4b01763844476267


---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Omówienie zagadnień dotyczących ciągłości działalności biznesowej zapewnianej przez usługę Azure SQL Database
W tym omówieniu opisano możliwości usługi Azure SQL Database w zakresie zapewniania ciągłości działalności biznesowej i odzyskiwania po awarii. Zawiera ono opcje, zalecenia i samouczki dotyczące odzyskiwania po wystąpieniu zdarzeń powodujących zakłócenia, które mogą spowodować utratę danych lub spowodować, że baza danych i aplikacja staną się niedostępne. Dyskusja dotyczy działań, które należy podjąć, gdy błąd użytkownika lub aplikacji wpływa na integralność danych, w regionie platformy Azure wystąpiła awaria lub aplikacja wymaga konserwacji. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Funkcje usługi SQL Database, których można użyć, aby zapewnić ciągłość działalności biznesowej
Usługa SQL Database udostępnia kilka funkcji zapewniających ciągłość działalności biznesowej, w tym zautomatyzowane kopie zapasowe oraz opcjonalną replikację bazy danych. Każda z tych funkcji ma różne charakterystyki dotyczące szacowanego czasu odzyskiwania (ERT, estimated recovery time) i potencjalnej utraty danych dotyczących ostatnich transakcji. Po zapoznaniu się z tymi opcjami można dokonać między nimi wyboru, a także — w większości przypadków — użyć ich razem w ramach różnych scenariuszy. Podczas opracowywania planu zapewniania ciągłości działalności biznesowej należy zrozumieć znaczenie maksymalnego dopuszczalnego czasu oczekiwania na pełne odzyskanie aplikacji po wystąpieniu zdarzenia powodującego zakłócenia — jest to cel czasu odzyskiwania (RTO, recovery time objective). Należy również zrozumieć znaczenie maksymalnej ilości najnowszych aktualizacji danych (przedział czasu), których utrata może być tolerowana przez aplikację w momencie odzyskiwania po wystąpieniu zdarzenia powodującego zakłócenia — jest to cel punktu odzyskiwania (RPO, recovery point objective). 

W poniższej tabeli znajduje się porównanie wartości ERT i RPO dla trzech najbardziej typowych scenariuszy.

| Możliwości | Warstwa Podstawowa | Warstwa standardowa | Warstwa Premium |
| --- | --- | --- | --- |
| Przywracanie do punktu w czasie z kopii zapasowej |Dowolny punkt przywracania w ciągu ostatnich 7 dni |Dowolny punkt przywracania w ciągu ostatnich 35 dni |Dowolny punkt przywracania w ciągu ostatnich 35 dni |
| Przywracanie geograficzne z kopii zapasowych z replikacją geograficzną |ERT < 12 godz., RPO < 1 godz. |ERT < 12 godz., RPO < 1 godz. |ERT < 12 godz., RPO < 1 godz. |
| Przywracanie z magazynu usługi Azure Backup |ERT < 12 godz., RPO < 1 tydz. |ERT < 12 godz., RPO < 1 tydz. |ERT < 12 godz., RPO < 1 tydz. |
| Aktywna replikacja geograficzna |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |

### <a name="use-database-backups-to-recover-a-database"></a>Używanie kopii zapasowych bazy danych w celu odzyskania bazy danych
Usługa SQL Database automatycznie przeprowadza kombinację cotygodniowego tworzenia pełnych kopii zapasowych, cogodzinnego tworzenia różnicowych kopii zapasowych bazy danych i tworzenia kopii zapasowych dziennika transakcji co pięć minut w celu ochrony Twojej firmy przed utratą danych. Te kopie zapasowe są przechowywane w magazynach geograficznie nadmiarowych przez 35 dni w przypadku baz danych w warstwach Standardowa i Premium oraz przez 7 dni w przypadku baz danych w warstwie Podstawowa. Aby uzyskać więcej informacji o warstwach usług, zobacz [Warstwy usług](sql-database-service-tiers.md). Jeśli okres przechowywania w warstwie usług nie spełnia Twoich wymagań biznesowych, można go zwiększyć, [zmieniając warstwę usług](sql-database-scale-up.md). Pełne oraz różnicowe kopie zapasowe bazy danych są także replikowane do [sparowanego centrum danych](../best-practices-availability-paired-regions.md) w celu ochrony przed awarią centrum danych. Aby uzyskać więcej szczegółów, zobacz informacje o [automatycznych kopiach zapasowych](sql-database-automated-backups.md).

Jeśli wbudowany okres przechowywania nie jest wystarczający dla aplikacji, możesz go rozszerzyć, konfigurując długoterminowe zasady przechowywania na potrzeby baz danych. Aby uzyskać więcej informacji, zobacz [Długoterminowe przechowywanie](sql-database-long-term-retention.md). 

Tych automatycznych kopii zapasowych bazy danych można użyć, aby odzyskać bazę danych po wystąpieniu różnych zdarzeń powodujących zakłócenia, zarówno w obrębie centrum danych, jak również do innego centrum danych. W przypadku korzystania z automatycznych kopii zapasowych bazy danych szacowany czas odzyskiwania zależy od kilku czynników, w tym łącznej liczby jednocześnie odzyskiwanych baz danych w tym samym regionie, rozmiaru bazy danych, rozmiaru dziennika transakcji oraz przepustowości sieci. W większości przypadków czas odzyskiwania jest krótszy niż 12 godzin. Podczas odzyskiwania do innego obszaru danych potencjalna utrata danych jest ograniczona do 1 godziny przez magazyn geograficznie nadmiarowy w ramach tworzonych co godzinę różnicowych kopii zapasowych bazy danych. 

> [!IMPORTANT]
> Aby przeprowadzić odzyskiwanie za pomocą automatycznych kopii zapasowych, użytkownik musi być członkiem roli Współautor programu SQL Server lub właścicielem subskrypcji — zobacz [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md) (RBAC: role wbudowane). Odzyskiwanie można przeprowadzić za pomocą witryny Azure Portal, programu PowerShell lub interfejsu API REST. Nie można używać języka Transact-SQL.
> 
> 

Zautomatyzowanych kopii zapasowych jako mechanizmu odzyskiwania i zapewniania ciągłości działalności biznesowej należy używać, jeśli aplikacja:

* Nie jest traktowana jako aplikacja o kluczowym znaczeniu.
* Nie ma powiązanej umowy SLA, w związku z czym przestój trwający 24 godziny lub dłużej nie spowoduje poniesienia odpowiedzialności finansowej.
* Ma niską częstotliwość zmian danych (mała liczba transakcji na godzinę) i utrata zmian z maksymalnie jednej godziny jest akceptowalna. 
* Jej koszt ma znaczenie. 

Jeśli wymagane jest szybsze odzyskiwanie, należy użyć [aktywnej replikacji geograficznej](sql-database-geo-replication-overview.md) (omówionej poniżej). Jeśli wymagane jest, aby w razie potrzeby możliwe było odzyskanie danych starszych niż 35 dni, należy rozważyć regularne archiwizowanie bazy danych do pliku BACPAC (skompresowany plik zawierający schemat bazy danych i powiązane dane) przechowywanego w magazynie Azure Blob Storage lub w innej wybranej lokalizacji. Aby uzyskać więcej informacji na temat tworzenia spójnego transakcyjnie archiwum bazy danych, zobacz informacje dotyczące [tworzenia kopii bazy danych](sql-database-copy.md) i [eksportowania kopii bazy danych](sql-database-export.md). 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Użyj aktywnej replikacji geograficznej, aby skrócić czas odzyskiwania i ograniczyć utratę danych związaną z odzyskiwaniem
Oprócz używania kopii zapasowych bazy danych na potrzeby odzyskiwania bazy danych w przypadku zakłócenia działania firmy można użyć [aktywnej replikacji geograficznej](sql-database-geo-replication-overview.md) do skonfigurowania bazy danych w taki sposób, aby w wybranych regionach istniały maksymalnie cztery pomocnicze bazy danych z możliwością odczytu. Te pomocnicze bazy danych są stale synchronizowane z podstawową bazą danych przy użyciu mechanizmu replikacji asynchronicznej. Ta funkcja jest używana do ochrony przed zakłóceniami działania firmy w wyniku awarii centrum danych lub podczas uaktualniania aplikacji. Aktywnej replikacji geograficznej można także użyć do zapewnienia lepszej wydajności zapytań tylko do odczytu geograficznie rozproszonym użytkownikom.

Jeśli podstawowa baza danych przejdzie nieoczekiwanie w tryb offline lub konieczne będzie przełączenie jej do tego trybu ze względu na prowadzone prace konserwacyjne, możliwe jest szybkie wypromowanie pomocniczej bazy danych do roli podstawowej bazy danych (nazywane również przejściem w tryb failover) i skonfigurowanie aplikacji pod kątem nawiązywania połączenia z nowo wypromowaną podstawową bazą danych. Planowane przejście w tryb failover nie spowoduje utraty żadnych danych. W przypadku nieplanowanego przejścia w tryb failover może wystąpić utrata niewielkiej ilości danych dotyczących tylko najnowszych transakcji ze względu na charakter replikacji asynchronicznej. Po przejściu w tryb failover możliwy jest późniejszy powrót po awarii — albo zgodnie z planem, albo gdy centrum danych powróci do trybu online. We wszystkich przypadkach użytkownicy doświadczą krótkiego przestoju i będą musieli ponownie nawiązać połączenie. 

> [!IMPORTANT]
> Aby korzystać z aktywnej replikacji geograficznej, użytkownik musi być właścicielem subskrypcji lub mieć uprawnienia administracyjne w programie SQL Server. Możliwe jest przeprowadzenie konfiguracji i przejście w tryb failover przy użyciu witryny Azure Portal, programu PowerShell lub interfejsu API REST za pomocą uprawnień dla subskrypcji lub przy użyciu języka Transact-SQL za pomocą uprawnień w ramach programu SQL Server.
> 
> 

Użyj aktywnej replikacji geograficznej, jeśli Twoja aplikacja spełnia dowolne z poniższych kryteriów:

* Ma kluczowe znaczenie.
* Obejmuje ją umowa dotycząca poziomu usług (SLA), zgodnie z którą przestój nie może być dłuższy niż 24 godziny.
* Przestój spowoduje poniesienie odpowiedzialności finansowej.
* Ma wysoki współczynnik zmian danych i utrata zmian z okresu jednej godziny jest niedopuszczalna.
* Dodatkowy koszt związany z aktywną replikacją geograficzną jest niższy niż potencjalna odpowiedzialność finansowa i powiązane straty biznesowe.

>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

## <a name="recover-a-database-after-a-user-or-application-error"></a>Odzyskiwanie bazy danych po błędzie użytkownika lub aplikacji
*Nikt nie jest doskonały! Użytkownik może przypadkowo usunąć pewne dane, nieodwracalnie usunąć ważną tabelę lub nawet usunąć całą bazę danych. Możliwe jest również przypadkowe zastąpienie przez aplikację poprawnych danych błędnymi danymi w wyniku wady aplikacji. 

W tym scenariuszu opcje odzyskiwania są następujące.

### <a name="perform-a-point-in-time-restore"></a>Wykonanie przywracania do punktu w czasie
Możliwe jest użycie zautomatyzowanych kopii zapasowych, aby odzyskać kopię bazy danych do znanego prawidłowego punktu w czasie, pod warunkiem, że ten punkt w czasie przypada na okres przechowywania bazy danych. Po przywróceniu bazy danych można albo zastąpić oryginalną bazę danych przywróconą bazą danych, albo skopiować potrzebne dane z przywróconych danych do oryginalnej bazy danych. Jeśli baza danych używa aktywnej replikacji geograficznej, zaleca się skopiowanie wymaganych danych z przywróconej kopii do oryginalnej bazy danych. Jeśli oryginalna baza danych zostanie zastąpiona przywróconą bazą danych, należy ponownie skonfigurować i zsynchronizować aktywną replikację geograficzną (co w przypadku dużej bazy danych może zająć trochę czasu). 

Aby uzyskać więcej informacji i poznać szczegółowe kroki przywracania bazy danych do punktu w czasie za pomocą witryny Azure Portal lub programu PowerShell, zobacz informacje dotyczące [przywracania do punktu w czasie](sql-database-recovery-using-backups.md#point-in-time-restore). Nie można przeprowadzić odzyskiwania za pomocą języka Transact-SQL.

### <a name="restore-a-deleted-database"></a>Przywracanie usuniętej bazy danych
Jeśli baza danych została usunięta, ale serwer logiczny nie został usunięty, można przywrócić usuniętą bazę danych do punktu, w którym została ona usunięta. Spowoduje to przywrócenie kopii zapasowej bazy danych na ten sam logiczny serwer SQL, z którego została ona usunięta. Bazę danych można przywrócić przy użyciu oryginalnej nazwy; można też nadać jej nową nazwę.

Aby uzyskać więcej informacji i poznać szczegółowe kroki przywracania usuniętej bazy danych za pomocą witryny Azure Portal lub programu PowerShell, zobacz informacje dotyczące [przywracania usuniętej bazy danych](sql-database-recovery-using-backups.md#deleted-database-restore). Nie można przeprowadzić przywracania za pomocą języka Transact-SQL.

> [!IMPORTANT]
> Jeśli usunięto serwer logiczny, nie można odzyskać usuniętej bazy danych. 
> 
> 

### <a name="restore-from-azure-backup-vault"></a>Przywracanie z magazynu usługi Azure Backup
Jeśli utrata danych miała miejsce poza bieżącym okresem przechowywania dla zautomatyzowanych kopii zapasowych, a baza danych została skonfigurowana do długoterminowego przechowywania danych, możliwe jest przywrócenie z cotygodniowej kopii zapasowej z magazynu usługi Azure Backup do nowej bazy danych. W takim momencie można albo zastąpić oryginalną bazę danych przywróconą bazą danych, albo skopiować potrzebne dane z przywróconej bazy danych do oryginalnej bazy danych. Jeśli konieczne jest pobranie starej wersji bazy danych przed przeprowadzeniem istotnego uaktualnienia aplikacji, spełnieniem żądania audytorów lub wykonaniem nakazu prawnego, można utworzyć nową bazę danych przy użyciu pełnej kopii zapasowej zapisanej w magazynie usługi Azure Backup.  Aby uzyskać więcej informacji, zobacz [Długoterminowe przechowywanie](sql-database-long-term-retention.md).

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Odzyskiwanie bazy danych do innego regionu podczas awarii regionalnego centrum danych platformy Azure
<!-- Explain this scenario -->

Sporadycznie centrum danych platformy Azure może mieć awarię. Taka awaria powoduje zakłócenia działania firmy, które mogą trwać tylko kilka minut, ale mogą też trwać wiele godzin. 

* Jedną z opcji jest oczekiwanie, aż baza danych powróci do trybu online po zakończeniu awarii centrum danych. Takie rozwiązanie sprawdza się dla aplikacji, w przypadku których baza danych może być w trybie offline. Może to na przykład dotyczyć projektu tworzenia oprogramowania lub bezpłatnej wersji próbnej, nad którymi nie trzeba pracować na bieżąco. Gdy ma miejsce awaria centrum danych, nie jest wiadomo, jak długo będzie ona trwała, więc ta opcja ma zastosowanie tylko wtedy, gdy użycie bazy danych nie jest chwilowo konieczne.
* Inną możliwością jest przejście w tryb failover do innego regionu danych w przypadku korzystania z aktywnej replikacji geograficznej lub przeprowadzenie odzyskiwania za pomocą geograficznie nadmiarowych kopii zapasowych bazy danych (przywracanie geograficzne). Przejście w tryb failover trwa tylko kilka sekund, podczas gdy odzyskiwanie z kopii zapasowych trwa wiele godzin.

Po wykonaniu działania czas trwania odzyskiwania i ilość utraconych danych w wyniku awarii centrum danych zależy od tego, w jaki sposób użytkownik zdecydował się korzystać w aplikacji z omówionych powyżej funkcji zapewniania ciągłości działalności biznesowej. W rzeczywistości można użyć kombinacji kopii zapasowych bazy danych i aktywnej replikacji geograficznej w zależności od wymagań aplikacji. Dyskusję na temat zagadnień dotyczących projektowania aplikacji na potrzeby autonomicznych baz danych i pul elastycznych za pomocą tych funkcji zapewniania ciągłości działalności biznesowej zawierają tematy [Design an application for cloud disaster recovery](sql-database-designing-cloud-solutions-for-disaster-recovery.md) (Projektowanie aplikacji pod kątem odzyskiwania po awarii w chmurze) i [Elastic Pool disaster recovery strategies](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) (Strategie odzyskiwania po awarii dotyczące pul elastycznych).

W poniższych sekcjach przedstawiono omówienie kroków służących do odzyskiwania za pomocą kopii zapasowych bazy danych lub aktywnej replikacji geograficznej. Aby uzyskać szczegółowe kroki obejmujące planowanie wymagań, kroki wykonywane po odzyskiwaniu oraz informacje o sposobie symulacji awarii w celu wykonania próbnego odzyskiwanie po awarii, zobacz [Recover a SQL Database from an outage](sql-database-disaster-recovery.md) (Odzyskiwanie bazy danych SQL Database po awarii).

### <a name="prepare-for-an-outage"></a>Przygotowanie do awarii
Niezależnie od używanej funkcji zapewniania ciągłości działalności biznesowej należy:

* Zidentyfikować i przygotować serwer docelowy, w tym reguły zapory na poziomie serwera, dane logowania i uprawnienia na poziomie głównej bazy danych.
* Określić sposób przekierowania klientów i aplikacji klienckich na nowy serwer.
* Udokumentować inne zależności, takie jak ustawienia inspekcji i alerty 

Jeśli planowanie i przygotowanie nie przebiegnie prawidłowo, przywrócenie aplikacji do trybu online po przejściu w tryb failover lub odzyskiwaniu zajmie dodatkowy czas, a także prawdopodobnie będzie wiązało się z koniecznością rozwiązywania problemów w czasie obciążenia — jest to niefortunne połączenie.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Przechodzenie do trybu failover do dodatkowej bazy danych replikowanej geograficznie
Jeśli aktywna replikacja geograficzna jest używana jako mechanizm odzyskiwania, należy [wymusić przejście do trybu failover do dodatkowej bazy danych replikowanej geograficznie](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database). W ciągu kilku sekund pomocnicza baza danych zostanie wypromowana do nowej podstawowej bazy danych i będzie gotowa do rejestrowania nowych transakcji i odpowiadania na zapytania — strata danych, które nie zostały jeszcze zreplikowane, trwa tylko kilka sekund. Aby uzyskać informacje dotyczące automatyzowania procesu przechodzenia w tryb failover, zobacz [Design an application for cloud disaster recovery](sql-database-designing-cloud-solutions-for-disaster-recovery.md) (Projektowanie aplikacji pod kątem odzyskiwania po awarii w chmurze).

> [!NOTE]
> Gdy centrum danych powróci do trybu online, możliwy jest powrót po awarii do oryginalnej podstawowej bazy danych (lub nie).
> 
> 

### <a name="perform-a-geo-restore"></a>Przeprowadzanie przywracania geograficznego
Jeśli jako mechanizm odzyskiwania używane są zautomatyzowane kopie zapasowe z geograficznie nadmiarową replikacją magazynu, należy [zainicjować odzyskiwanie bazy danych przy użyciu przywracania geograficznego](sql-database-disaster-recovery.md#recover-using-geo-restore). Odzyskiwanie ma zwykle miejsce w ciągu 12 godzin, z utratą danych maksymalnie z jednej godziny, w zależności od tego, kiedy została wykonana i zreplikowana ostatnia tworzona co godzinę różnicowa kopia zapasowa. Do momentu ukończenia odzyskiwania baza danych nie może rejestrować żadnych transakcji ani odpowiadać na żadne zapytania. 

> [!NOTE]
> Jeśli centrum danych powróci do trybu online przed przełączeniem aplikacji do odzyskanej bazy danych, można po prostu anulować odzyskiwanie.  
> 
> 

### <a name="perform-post-failover--recovery-tasks"></a>Wykonywanie zadań po przejściu do trybu failover lub po odzyskiwaniu
Po odzyskaniu za pomocą dowolnego mechanizmu odzyskiwania należy wykonać następujące zadania dodatkowe, zanim będzie możliwe ponowne rozpoczęcie pracy przez użytkowników i aplikacje:

* Przekieruj klientów i aplikacje klienckie na nowy serwer i przywróconą bazę danych
* Zapewnij użycie odpowiednich reguł zapory na poziomie serwera na potrzeby nawiązywania połączenia przez użytkowników (lub użyj [zapór na poziomie bazy danych](sql-database-firewall-configure.md#creating-database-level-firewall-rules))
* Zapewnij użycie odpowiednich danych logowania i uprawnień na poziomie głównej bazy danych (lub użyj [użytkowników, którzy się w niej znajdują](https://msdn.microsoft.com/library/ff929188.aspx))
* W razie potrzeby skonfiguruj inspekcję
* W razie potrzeby skonfiguruj alerty

## <a name="upgrade-an-application-with-minimal-downtime"></a>Uaktualnianie aplikacji przy minimalnych przestojach
Czasami aplikacja musi zostać przełączona do trybu offline z powodu zaplanowanej konserwacji, które obejmuje na przykład uaktualnienie aplikacji. W temacie [Manage application upgrades](sql-database-manage-application-rolling-upgrade.md) (Zarządzanie uaktualnieniami aplikacji) opisano sposób użycia aktywnej replikacji geograficznej w celu umożliwienia przeprowadzania uaktualnienia równoległych aplikacji w chmurze, aby skrócić czas przestoju podczas uaktualnień i zapewnić ścieżkę odzyskiwania w przypadku, gdy coś się nie powiedzie. W tym artykule opisano dwa różne sposoby organizowania procesu uaktualniania i omówiono zalety i wady każdego z tych sposobów.

## <a name="next-steps"></a>Następne kroki
Dyskusję na temat zagadnień dotyczących projektowania aplikacji na potrzeby autonomicznych baz danych i pul elastycznych zawierają tematy [Design an application for cloud disaster recovery](sql-database-designing-cloud-solutions-for-disaster-recovery.md) (Projektowanie aplikacji pod kątem odzyskiwania po awarii w chmurze) i [Elastic Pool disaster recovery strategies](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) (Strategie odzyskiwania po awarii dotyczące pul elastycznych).




<!--HONumber=Jan17_HO2-->


