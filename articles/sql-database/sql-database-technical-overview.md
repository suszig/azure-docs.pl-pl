---
title: "Co to jest SQL Database? Wprowadzenie do usługi SQL Database | Microsoft Docs"
description: 'Get an introduction to SQL Database: technical details and capabilities of Microsoft''s relational database management system (RDBMS) in the cloud.'
keywords: "wprowadzenie do usługi sql, co to jest sql database"
services: sql-database
documentationcenter: 
author: shontnew
manager: jhubbard
editor: cgronlun
ms.assetid: c561f600-a292-4e3b-b1d4-8ab89b81db48
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 12/20/2016
ms.author: shkurhek;carlrab
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: 262583a2a11c41cf7acb55599041692137165e8f

---
# <a name="what-is-sql-database-introduction-to-sql-database"></a>Co to jest SQL Database? Wprowadzenie do usługi SQL Database
Usługa SQL Database jest usługą relacyjnej bazy danych w chmurze firmy Microsoft opartą na wiodącym na rynku aparacie programu Microsoft SQL Server. Może ona obsługiwać obciążenia o znaczeniu krytycznym. Usługa SQL Database oferuje przewidywalną wydajność na wielu poziomach usługi, dynamiczną skalowalność bez przestojów, wbudowane funkcje ciągłości prowadzenia działalności biznesowej i ochronę danych — wszystko przy bliskich zeru nakładach na administrację. Te możliwości pozwalają Ci skoncentrować się na szybkim tworzeniu aplikacji i skracaniu czasu wejścia na rynek, a nie na poświęcaniu cennego czasu i cennych zasobów na zarządzanie maszynami wirtualnymi i infrastrukturą. Ponieważ usługa SQL Database jest oparta na aparacie programu [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx), usługa SQL Database obsługuje istniejące narzędzia, biblioteki i interfejsy API programu SQL Server. Dzięki temu można łatwo tworzyć nowe rozwiązania, przenosić istniejące rozwiązania programu SQL Server i rozszerzać je do chmury firmy Microsoft bez konieczności uczenia się nowych umiejętności.

Ten artykuł obejmuje wprowadzenie do podstawowych pojęć i funkcji usługi SQL Database związanych z wydajnością, skalowalnością i możliwościami zarządzania, a także zawiera linki do szczegółowych informacji. Jeśli chcesz już przejść do samouczków, otwórz artykuł [Tworzenie pierwszej bazy danych SQL](sql-database-get-started.md) lub [Tworzenie puli elastycznej](sql-database-elastic-pool-create-portal.md). Aby zapoznać się z krótki pokazem, obejrzyj to wideo.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Azure-SQL-Database-create-DBs-in-seconds/player]
> 
> 

## <a name="adjust-performance-and-scale-without-downtime"></a>Dostosowanie wydajności i skalowanie bez przestojów
Usługa SQL Database oferuje trzy warstwy usługi: Podstawowa, Standardowa i Premium. Każdej warstwa usług oferuje [różne poziomy wydajności i możliwości](sql-database-service-tiers.md) w celu obsługi różnych obciążeń bazy danych, zarówno niewielkich, jak i ogromnych. Możesz utworzyć pierwszą aplikację na małej bazie danych za niewielką sumę miesięcznie, a następnie [zmienić warstwę usługi](sql-database-scale-up.md) ręcznie lub programowo w dowolnym momencie, aby spełnić wymagania rozwiązania. Jest to możliwe bez przestoju aplikacji i przerwy w świadczeniu usługi klientom. Dynamiczna skalowalność umożliwia bazie danych przezroczyste odpowiadanie na gwałtownie zmieniające się wymagania dotyczące zasobów oraz płacenie tylko za potrzebne zasoby i tylko gdy są używane.

## <a name="elastic-pools-to-maximize-resource-utilization"></a>Pule elastyczne umożliwiające zmaksymalizowanie wykorzystania zasobów
W przypadku wielu firm i aplikacji możliwość tworzenia pojedynczych baz danych i dostosowywania wydajności na żądanie jest wystarczająca, zwłaszcza jeśli wzorce użycia są względnie przewidywalne. Jednak w przypadku nieprzewidywalnych wzorców zarządzanie kosztami i modelem biznesowym może być trudne. [Pule elastyczne](sql-database-elastic-pool.md) zostały zaprojektowane, aby rozwiązać ten problem. Koncepcja jest prosta. Przydzielasz zasoby wydajności do puli, a nie do pojedynczej bazy danych, i płacisz za zbiorczą wydajność puli, a nie za wydajność pojedynczej bazy danych. Pule elastyczne nie wymagają koncentrowania się na skalowaniu wydajności bazy danych w górę i w dół w miarę zmian zapotrzebowania na zasoby. Bazy danych w puli używają zasobów wydajności puli elastycznej w miarę potrzeb. Bazy danych w puli zużywają zasoby, ale nie przekraczają limitów puli, więc koszt pozostaje przewidywalny nawet wtedy, gdy nie można przewidzieć użycia poszczególnych baz danych. Ponadto możliwe jest [dodawanie i usuwanie baz danych w puli](sql-database-elastic-pool-manage-portal.md) i skalowanie aplikacji od kilku do tysięcy baz danych — wszystko w ramach kontrolowanego budżetu. W końcu możesz kontrolować minimum i maksimum zasobów dostępnych dla baz danych w puli w celu zapewnienia, że żadna baza danych w puli nie używa wszystkich zasobów, a każda baza danych w puli ma gwarantowaną minimalną ilość zasobów. Aby dowiedzieć się więcej na temat wzorców projektowych dla aplikacji SaaS wykorzystujących pule elastyczne, zobacz artykuł [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) (Wzorce projektowe dla wielodostępnych aplikacji SaaS korzystających z usługi Azure SQL Database).

## <a name="blend-single-databases-with-pooled-databases"></a>Mieszaj pojedyncze bazy danych z bazami danych w puli
W obu przypadkach — pojedyncza baza danych i pule elastyczne — nie ma ograniczenia do jednego rozwiązania. Możesz w łatwy i szybki sposób łączyć pojedyncze bazy danych z pulami elastycznymi i zmieniać warstwy usług pojedynczych baz danych oraz pul elastycznych, aby dostosować je do swojej sytuacji. Ponadto, korzystając z siły i zasięgu platformy Azure, możesz łączyć inne usługi Azure z usługą SQL Database i dopasowywać je do niej, aby zaspokoić potrzeby unikatowego projektu aplikacji, zwiększyć efektywność kosztową i poprawić wykorzystanie zasobów oraz otworzyć się na nowe możliwości biznesowe.

## <a name="monitoring-and-alerting"></a>Monitorowanie i zgłaszanie alertów
Jednak jak porównać względną wydajność pojedynczych baz danych i pul elastycznych? Skąd wiadomo, kiedy kliknąć, aby zatrzymać regulowanie w górę i w dół we właściwym miejscu? Możesz użyć [wbudowanej funkcji monitorowania wydajności](sql-database-performance.md) i narzędzi do [zgłaszania alertów](sql-database-insights-alerts-portal.md) w połączeniu z ocenami wydajności opartymi na [jednostkach DTU (Database Transaction Units) dla pojedynczych baz danych i jednostkach eDTU (elastic DTU) dla pul elastycznych](sql-database-what-is-a-dtu.md). Za pomocą tych narzędzi możesz szybko ocenić wpływ skalowania w górę lub w dół na podstawie bieżących lub przewidywanych wymagań dotyczących wydajności. Zobacz artykuł [Opcje i wydajność usługi SQL Database: poznaj, co jest dostępne w poszczególnych warstwach usług](sql-database-service-tiers.md), aby uzyskać szczegółowe informacje.

## <a name="keep-your-app-and-business-running"></a>Zapewnienie działania aplikacji i firmy
Umowa dotycząca poziomu usług [(SLA)](http://azure.microsoft.com/support/legal/sla/) o czołowej w branży dostępności 99,99% dla platformy Azure, która jest obsługiwana przez globalną sieć centrów danych zarządzanych przez firmę Microsoft, pomaga zapewnić działanie aplikacji przez 24 godziny na dobę, 7 dni w tygodniu. W każdej usłudze SQL Database korzystasz z wbudowanych rozwiązań zabezpieczeń, odporności na uszkodzenia i [ochrony danych](sql-database-automated-backups.md), które w przeciwnym razie trzeba byłoby kupić lub zaprojektować, zrealizować, a także nimi zarządzać. W usłudze SQL Database każda warstwa usług oferuje kompleksowy zestaw funkcji i opcji ciągłości działania, których można użyć, aby rozpocząć i kontynuować pracę. Możesz użyć [przywracania do punktu w czasie](sql-database-recovery-using-backups.md), aby przywrócić bazę danych do wcześniejszego stanu — nawet sprzed 35 dni. Możesz skonfigurować [długoterminowe przechowywanie kopii zapasowych](sql-database-long-term-retention.md), aby przechowywać kopie zapasowe w bezpiecznym magazynie nawet przez 10 lat. Ponadto jeśli centrum danych hostujące bazy danych ulegnie awarii, możesz przywrócić bazy danych z [geograficznie nadmiarowych duplikatów najnowszych kopii zapasowych]((sql-database-recovery-using-backups.md). W razie potrzeby możesz także skonfigurować [geograficznie nadmiarowe repliki do odczytu](sql-database-geo-replication-overview.md) w jednym lub wielu regionach, aby umożliwić szybkie przechodzenie do trybu failover w przypadku niedostępności centrum danych. Możesz także użyć tych replik, aby zwiększyć wydajność odczytu w różnych regionach geograficznych lub [uaktualniać aplikacje bez przestoju](sql-database-manage-application-rolling-upgrade.md). 

![Replikacja geograficzna usługi SQL Database](./media/sql-database-technical-overview/azure_sqldb_map.png)

Zobacz artykuł dotyczący [ciągłości biznesowej](sql-database-business-continuity.md), aby uzyskać szczegółowe informacje o funkcjach ciągłości biznesowej dostępnych w poszczególnych warstwach usług.

## <a name="secure-your-data"></a>Zabezpieczanie danych
Program SQL Server ma tradycje zabezpieczeń danych, które usługa SQL Database podtrzymuje dzięki funkcjom ograniczania dostępu, ochrony danych i monitorowania aktywności. Zobacz artykuł [Securing your SQL database](sql-database-security-overview.md) (Zabezpieczanie bazy danych SQL), aby szybko zapoznać się z zabezpieczeniami w usłudze SQL Database. W [Centrum zabezpieczeń dla aparatu bazy danych programu SQL Server i usługi SQL Database](https://msdn.microsoft.com/library/bb510589) znajdziesz bardziej wyczerpujący przegląd funkcji zabezpieczeń. Odwiedź [Centrum zaufania Azure](https://azure.microsoft.com/support/trust-center/security/), aby uzyskać informacje o zabezpieczeniach platformy Azure.

## <a name="next-steps"></a>Następne kroki
Po przeczytaniu wprowadzenia do usługi SQL Database i zapoznaniu się z nią możesz:

* Zapoznać się z [cennikiem](https://azure.microsoft.com/pricing/details/sql-database/) zawierającym porównania i kalkulatory kosztów dla pojedynczej bazy danych i pul elastycznych.
* Poznać [pule elastyczne](sql-database-elastic-pool.md).
* Rozpocząć [tworzenie pierwszej bazy danych](sql-database-get-started.md).
* Utworzyć pierwszą aplikację w języku C#, Java, Node.js, PHP, Python lub Ruby, korzystając z [bibliotek połączeń dla usługi SQL Database i programu SQL Server](sql-database-libraries.md)



<!--HONumber=Dec16_HO4-->


