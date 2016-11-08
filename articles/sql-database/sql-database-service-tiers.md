---
title: 'Wydajność i opcje usługi SQL Database: warstwy usług | Microsoft Docs'
description: Porównaj funkcje usługi SQL Database dotyczące wydajności i ciągłości działania w poszczególnych warstwach usług w celu zbilansowania kosztów i możliwości podczas skalowania.
keywords: opcje bazy danych, wydajność bazy danych
services: sql-database
documentationcenter: ''
author: CarlRabeler
manager: jhubbard
editor: CarlRabeler

ms.service: sql-database
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 08/10/2016
ms.author: carlrab

---
# Opcje i wydajność usługi SQL Database: poznaj, co jest dostępne w poszczególnych warstwach usług
Usługa [Azure SQL Database](sql-database-technical-overview.md) oferuje trzy warstwy usługi o różnych poziomach wydajności do obsługi różnych obciążeń. Każdy poziom wydajności zapewnia coraz większy zestaw zasobów w celu dostarczenia coraz wyższej przepustowości. Poszczególnymi bazami można zarządzać w ramach ich własnej [warstwy usługi](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) o określonym poziomie wydajności. Można również zarządzać wieloma bazami danych w [puli elastycznej](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) z współdzielonym zestawem zasobów. Zasoby dostępne dla autonomicznych baz danych są wyrażane jako liczba jednostek DTU (Database Transaction Unit), a dla pul elastycznych w postaci jednostek eDTU (elastic Database Transaction Unit). Aby uzyskać więcej informacji na temat jednostek DTU i eDTU, zobacz [Co to jest jednostka DTU](sql-database-what-is-a-dtu.md). 

W obu przypadkach do wyboru są następujące warstwy usługi: **Podstawowa**, **Standardowa** i **Premium**. Opcje bazy danych w tych warstwach są podobne dla autonomicznych baz danych i pul elastycznych, ale w przypadku pul elastycznych trzeba rozważyć dodatkowe zagadnienia. Ten artykuł zawiera szczegółowe informacje na temat warstw usług dla autonomicznych baz danych i pul elastycznych.

## Warstwy usług i opcje baz danych
Wszystkie warstwy usług (Podstawowa, Standardowa i Premium) są objęte umową SLA zapewniającą czas działania 99,99% i oferują przewidywalną wydajność, elastyczne opcje ciągłości działania, funkcje zabezpieczeń i rozliczenia godzinowe. Tabela poniżej zawiera przykłady warstw najlepiej dopasowane do różnych obciążeń aplikacji.

| Warstwa usług | Docelowe obciążenia |
| --- | --- |
| **Podstawowa** |Najodpowiedniejsza dla małych baz danych. Standardowo zapewnia obsługę jednej aktywnej operacji w danym momencie. Przykłady zastosowania to bazy danych używane do tworzenia i testowania oprogramowania lub niewielkie, rzadko używane aplikacje. |
| **Standardowa** |Odpowiednia dla większości aplikacji w chmurze. Zapewnia obsługę wielu równoczesnych zapytań. Przykładowe zastosowania to aplikacje grupy roboczej lub sieci Web. |
| **Premium** |Przeznaczona do obsługi dużej ilości danych transakcyjnych. Obsługuje wielu równoczesnych użytkowników i spełnia wymagania ciągłości działalności biznesowej na najwyższym poziomie. Przykładem zastosowania mogą być bazy danych dla aplikacji o znaczeniu krytycznym. |

> [!NOTE]
> Wersje Sieć Web i Business zostały wycofane. Jeśli zamierzasz nadal korzystać z wersji Sieć Web i Business, przeczytaj [Często zadawane pytania na temat wycofywanych wersji](https://azure.microsoft.com/pricing/details/sql-database/web-business/).
> 
> 

## Warstwy usług i poziomy wydajności autonomicznej bazy danych
Istnieje wiele poziomów wydajności dla autonomicznej bazy danych w poszczególnych warstwach usług. Możesz swobodnie wybrać poziom, który najlepiej spełnia wymagania danego obciążenia. W razie potrzeby skalowania w górę lub w dół możesz łatwo zmienić warstwy bazy danych. Szczegółowe informacje znajdziesz w artykule [Changing Database Service Tiers and Performance Levels](sql-database-scale-up.md) (Zmiana warstw usług i poziomów wydajności bazy danych).

Charakterystyki wydajności wymienione poniżej dotyczą baz danych utworzonych przy użyciu [usługi SQL Database V12](sql-database-v12-whats-new.md). Niezależnie od liczby hostowanych baz danych każda z nich ma zagwarantowany zestaw zasobów, a ich oczekiwane charakterystyki nie ulegają zmianom.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!NOTE]
> Szczegółowy opis wszystkich innych wierszy w tej tabeli warstw usług można znaleźć w [zestawieniu możliwości i ograniczeń warstw usług](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).
> 
> 

## Warstwy usług puli elastycznej oraz wydajność w jednostkach eDTU
Oprócz tworzenia i skalowania autonomicznej bazy danych istnieje również możliwość zarządzania wieloma bazami danych w ramach [puli elastycznej](sql-database-elastic-pool.md). Wszystkie bazy danych w puli elastycznej korzystają ze wspólnego zestawu zasobów. Charakterystyki wydajności są mierzone w *elastycznych jednostkach transakcji bazy danych* (eDTU). Podobnie jak autonomiczne bazy danych, pule są oferowane w trzech warstwach usług: **Podstawowej**, **Standardowej** i **Premium**. Te trzy warstwy usług również w przypadku pul definiują limity ogólnej wydajności i zakres funkcji.

Pule umożliwiają bazom danych współdzielenie i używanie zasobów jednostek DTU bez konieczności przypisywania konkretnego poziomu wydajności do każdej bazy danych w puli. Na przykład autonomiczna baza danych w puli Standardowej może przejść od używania 0 jednostek eDTU do maksymalnej liczby jednostek eDTU ustalonej dla bazy danych podczas konfigurowania puli. Dzięki pulom wiele baz danych z różnymi obciążeniami wydajnie korzysta z zasobów eDTU dostępnych w całej puli. Zobacz szczegółowe [zagadnienia dotyczące cen i wydajności puli elastycznej](sql-database-elastic-pool-guidance.md).

W poniższej tabeli opisano charakterystyki warstw usług dotyczących puli.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Każda baza danych w puli podlega również charakterystykom autonomicznej bazy danych w danej warstwie. Na przykład pula Podstawowa ma limit sesji dla jednej puli z zakresu 4800–28800, ale poszczególne bazy danych w puli Podstawowej mają limit bazy danych równy 300 sesji.

## Wybieranie warstwy usług
Aby wybrać warstwę usług, określ, czy baza danych ma być bazą autonomiczną, czy ma być częścią puli elastycznej. 

### Wybieranie warstwy usług dla autonomicznej bazy danych
Aby wybrać warstwę usług dla autonomicznej bazy danych, określ funkcje bazy danych potrzebne do wybrania wersji usługi SQL Database:

* Rozmiar bazy danych (maksymalnie 2 GB dla warstwy Podstawowej, maksymalnie 250 GB dla warstwy Standardowej i od 500 GB do 1 TB dla warstwy Premium — w zależności od poziomu wydajności)
* Okres przechowywania kopii zapasowej bazy danych (7 dni dla warstwy Podstawowej, 35 dni dla warstwy Standardowej i 35 dni dla warstwy Premium)

Po określeniu wersji usługi SQL Database możesz określić poziom wydajności bazy danych (liczbę jednostek DTU). Możesz wybrać dowolny poziom, a następnie [dynamicznie skalować w górę lub dół](sql-database-scale-up.md) w zależności od rzeczywistej wydajności. Możesz również obliczyć przybliżoną liczbę potrzebnych jednostek DTU za pomocą [Kalkulatora DTU](http://dtucalculator.azurewebsites.net/). 

### Wybieranie warstwy usług dla elastycznej puli baz danych
Aby wybrać warstwę usług dla elastycznej bazy danych, określ funkcje bazy danych potrzebne do wybrania warstwy usług dla puli.

* Rozmiar bazy danych (2 GB dla warstwy Podstawowej, 250 GB dla warstwy Standardowej i 500 GB dla warstwy Premium)
* Okres przechowywania kopii zapasowej bazy danych (7 dni dla warstwy Podstawowej, 35 dni dla warstwy Standardowej i 35 dni dla warstwy Premium)
* Liczba baz danych w jednej puli (400 dla warstwy Podstawowej, 400 dla warstwy Standardowej i 50 dla warstwy Premium)
* Maksymalna pojemność jednej puli (117 GB dla warstwy Podstawowej, 1200 dla warstwy Standardowej i 750 dla warstwy Premium)

Po określeniu warstwy usług dla puli możesz przystąpić do określenia poziomu wydajności dla puli (jednostki eDTU). Możesz wybrać dowolny poziom, a następnie [dynamicznie skalować w górę lub dół](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) w zależności od rzeczywistej wydajności. Aby ułatwić określenie górnego limitu dla puli, możesz obliczyć przybliżoną liczbę potrzebnych jednostek DTU dla każdej bazy danych w puli za pomocą [Kalkulatora DTU](http://dtucalculator.azurewebsites.net/).

## Następne kroki
* Sprawdź ceny warstw na stronie [Cennik usługi SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
* Poznaj lepiej [pule elastyczne](sql-database-elastic-pool-guidance.md) oraz [zagadnienia dotyczące cen i wydajności pul elastycznych](sql-database-elastic-pool-guidance.md).
* Naucz się [monitorować pule elastyczne, zarządzać nimi i zmieniać ich rozmiar](sql-database-elastic-pool-manage-portal.md) oraz [monitorować wydajność autonomicznych baz danych](sql-database-single-database-monitor.md).
* Teraz, kiedy znasz warstwy usługi SQL Database, wypróbuj je, korzystając z [bezpłatnego konta](https://azure.microsoft.com/pricing/free-trial/) i dowiedz się, jak [utworzyć pierwszą bazę danych SQL](sql-database-get-started.md).

## Dodatkowe zasoby
* [Wzorce projektowe dla wielodostępnych aplikacji SaaS wykorzystujących usługę Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Kurs wideo w serwisie Microsoft Virtual Academy poświęcony możliwościom elastycznych baz danych w usłudze Azure SQL Database](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)

<!--HONumber=Oct16_HO3-->


