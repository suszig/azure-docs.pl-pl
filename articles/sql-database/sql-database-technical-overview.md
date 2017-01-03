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
ms.date: 11/08/2016
ms.author: shkurhek
translationtype: Human Translation
ms.sourcegitcommit: 3ba16154857f8e7b59a1013b736d6131a4161185
ms.openlocfilehash: 2d0792046bf55d691df21e26f2df23235318665c


---
# <a name="what-is-sql-database-introduction-to-sql-database"></a>Co to jest SQL Database? Wprowadzenie do usługi SQL Database
SQL Database jest usługą relacyjnych baz danych w chmurze opartą na aparacie Microsoft SQL Server, który jest czołowym rozwiązaniem na rynku, zapewniającym możliwości realizacji najistotniejszych zadań biznesowych. Usługa SQL Database oferuje przewidywalną wydajność, skalowalność bez przestojów, ciągłość prowadzenia działalności biznesowej i ochronę danych — wszystko przy bliskich zeru nakładach na administrację. Możesz skoncentrować się na szybkim tworzeniu aplikacji i skracaniu czasu wejścia na rynek, a nie na zarządzaniu maszynami wirtualnymi i infrastrukturą. Ponieważ usługa SQL Database jest oparta na aparacie [programu SQL Server](https://msdn.microsoft.com/library/bb545450.aspx), współpracuje z istniejącymi narzędziami programu SQL Server, bibliotekami i interfejsami API, dzięki czemu możesz łatwiej przenosić rozwiązania i rozszerzać je, korzystając z chmury.

Ten artykuł obejmuje wprowadzenie do podstawowych pojęć i funkcji usługi SQL Database związanych z wydajnością, skalowalnością i możliwościami zarządzania, a także zawiera linki do szczegółowych informacji. Jeśli chcesz rozpocząć od razu, możesz [utworzyć pierwszą bazę danych SQL](sql-database-get-started.md) lub [utworzyć pulę elastyczną](sql-database-elastic-pool-create-portal.md) w ciągu kilku minut. Jeśli chcesz bardziej zgłębić temat, obejrzyj ten 30-minutowy film.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Azure-SQL-Database-create-DBs-in-seconds/player]
> 
> 

## <a name="adjust-performance-and-scale-without-downtime"></a>Dostosowanie wydajności i skalowanie bez przestojów
Bazy danych SQL są dostępne w *warstwach usług*: Podstawowej, Standardowej i Premium. Każdej warstwa usług oferuje [różne poziomy wydajności i możliwości](sql-database-service-tiers.md) w celu obsługi różnych obciążeń bazy danych, zarówno niewielkich, jak i ogromnych. Możesz utworzyć pierwszą aplikację na małej bazie danych za niewielką sumę miesięcznie, a następnie [zmienić warstwę usługi](sql-database-scale-up.md) ręcznie lub programowo w dowolnym momencie, gdy aplikacja rozprzestrzeni się na cały świat, bez przestojów w pracy aplikacji ani klientów.

W przypadku wielu firm i aplikacji możliwość tworzenia pojedynczych baz danych i dostosowywania wydajności na żądanie jest wystarczająca, zwłaszcza jeśli wzorce użycia są względnie przewidywalne. Jednak w przypadku nieprzewidywalnych wzorców zarządzanie kosztami i modelem biznesowym może być trudne.

[Elastyczne pule](sql-database-elastic-pool.md) w usłudze SQL Database rozwiązują ten problem. Koncepcja jest prosta. Przydzielasz wydajność do puli, dzięki czemu płacisz za łączną wydajność puli, a nie za wydajność pojedynczej bazy danych. Nie potrzebujesz zwiększać ani zmniejszać wydajności bazy danych. Bazy danych w puli, czyli *elastyczne bazy danych*, są skalowane w górę i w dół zgodnie z potrzebami w sposób automatyczny. Elastyczne bazy danych zużywają zasoby, ale nie przekraczają limitów puli, więc koszt pozostaje przewidywalny nawet wtedy, gdy nie można przewidzieć użycia bazy danych. Ponadto możliwe jest [dodawanie i usuwanie baz danych w puli](sql-database-elastic-pool-manage-portal.md) i skalowanie aplikacji od kilku do tysięcy baz danych — wszystko w ramach kontrolowanego budżetu. Aby dowiedzieć się więcej na temat wzorców projektowych dla aplikacji SaaS wykorzystujących pule elastyczne, zobacz artykuł [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) (Wzorce projektowe dla wielodostępnych aplikacji SaaS korzystających z usługi Azure SQL Database).

Wybrane rozwiązanie — pojedyncze lub elastyczne — nie stanowi dla użytkownika ograniczenia. Możesz w łatwy i szybki sposób łączyć pojedyncze bazy danych z pulami elastycznymi i zmieniać warstwy usług pojedynczych baz danych oraz pul, aby dostosować je do swojej sytuacji. Ponadto, korzystając z siły i zasięgu platformy Azure, możesz łączyć inne usługi Azure z usługą SQL Database i dopasowywać je do niej, aby zaspokoić potrzeby unikatowego projektu aplikacji, zwiększyć efektywność kosztową i poprawić wykorzystanie zasobów oraz otworzyć się na nowe możliwości biznesowe.

Jednak jak porównać względną wydajność baz danych i pul baz danych? Skąd wiadomo, kiedy kliknąć, aby zatrzymać regulowanie w górę i w dół we właściwym miejscu? Odpowiedzią jest wbudowane monitorowanie wydajności i narzędzia wysyłające alerty w połączeniu z klasyfikacjami wydajności na podstawie jednostek transakcji bazy danych (DTU, Database Transaction Unit) dla pojedynczych baz danych i elastycznych jednostek DTU (eDTU, elastic DTU) dla elastycznych baz danych i pul baz danych, które umożliwiają szybką ocenę wpływu skalowania w górę lub w dół na podstawie bieżącego lub projektowego zapotrzebowania w zakresie wydajności. Zobacz artykuł [Opcje i wydajność usługi SQL Database: poznaj, co jest dostępne w poszczególnych warstwach usług](sql-database-service-tiers.md), aby uzyskać szczegółowe informacje.

## <a name="keep-your-app-and-business-running"></a>Zapewnienie działania aplikacji i firmy
Umowa dotycząca poziomu usług [(SLA)](http://azure.microsoft.com/support/legal/sla/) o czołowej w branży dostępności 99,99% dla platformy Azure, która jest obsługiwana przez globalną sieć centrów danych zarządzanych przez firmę Microsoft, pomaga zapewnić działanie aplikacji przez 24 godziny na dobę, 7 dni w tygodniu. W każdej bazie danych SQL korzystasz z wbudowanych rozwiązań zabezpieczeń, odporności na uszkodzenia i ochrony danych, które w przeciwnym razie trzeba byłoby kupić lub zaprojektować, zrealizować, a także nimi zarządzać. Mimo to, w zależności od wymagań firmy, możesz potrzebować dodatkowych warstw ochrony w celu zapewnienia, że działanie aplikacji i firmy można szybko przywrócić w razie awarii, błędu lub innych zakłóceń. W usłudze SQL Database każda warstwa usług oferuje kompleksowy zestaw funkcji i opcji ciągłości działania, których można użyć, aby rozpocząć i kontynuować pracę. Możesz użyć przywracania do punktu w czasie, aby przywrócić bazę danych do wcześniejszego stanu, nawet sprzed 35 dni. Ponadto, jeśli centrum danych hostujące bazy danych ulegnie awarii, możesz przywrócić bazy danych z geograficznie nadmiarowych kopii zapasowych lub przełączyć do trybu failover na repliki bazy danych w innym regionie. Replik możesz też użyć w celu uaktualniania lub relokacji do innych regionów.

![Replikacja geograficzna usługi SQL Database](./media/sql-database-technical-overview/azure_sqldb_map.png)

Zobacz artykuł dotyczący [ciągłości biznesowej](sql-database-business-continuity.md), aby uzyskać szczegółowe informacje o funkcjach ciągłości biznesowej dostępnych w poszczególnych warstwach usług.

## <a name="secure-your-data"></a>Zabezpieczanie danych
Program SQL Server ma tradycję solidnych zabezpieczeń danych, którą baza danych SQL Database podtrzymuje dzięki funkcjom ograniczania dostępu, ochrony danych i monitorowania aktywności. Zobacz artykuł [Securing your SQL database](sql-database-security.md) (Zabezpieczanie bazy danych SQL), aby szybko zapoznać się z zabezpieczeniami w usłudze SQL Database. W [Centrum zabezpieczeń dla aparatu bazy danych programu SQL Server i usługi SQL Database](https://msdn.microsoft.com/library/bb510589) znajdziesz bardziej wyczerpujący przegląd funkcji zabezpieczeń. Odwiedź [Centrum zaufania Azure](https://azure.microsoft.com/support/trust-center/security/), aby uzyskać informacje o zabezpieczeniach platformy Azure.

## <a name="next-steps"></a>Następne kroki
Po przeczytaniu wprowadzenia do usługi SQL Database i zapoznaniu się z nią możesz:

* Zapoznać się z [cennikiem](https://azure.microsoft.com/pricing/details/sql-database/) zawierającym porównania i kalkulatory kosztów dla pojedynczej bazy danych i pul elastycznych.
* Poznać [pule elastyczne](sql-database-elastic-pool.md).
* Rozpocząć [tworzenie pierwszej bazy danych](sql-database-get-started.md).
* [Nawiązywać połączenia i wykonywać zapytania za pomocą programu SSMS](sql-database-connect-query-ssms.md).
* Utworzyć pierwszą aplikację w języku C#, Java, Node.js, PHP, Python lub Ruby, korzystając z [bibliotek połączeń dla usługi SQL Database i programu SQL Server](sql-database-libraries.md)



<!--HONumber=Dec16_HO3-->


