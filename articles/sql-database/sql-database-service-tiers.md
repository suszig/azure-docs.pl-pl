---
title: "Wydajność usługi SQL Database: warstwy usług | Microsoft Docs"
description: "Porównaj warstwy usług w usłudze SQL Database."
keywords: "opcje bazy danych, wydajność bazy danych"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 12/14/2016
ms.author: carlrab; janeng
translationtype: Human Translation
ms.sourcegitcommit: a40319d3e53c07a94bc34714ca7393c2747fb50c
ms.openlocfilehash: 340656b896763914c2f6d37c72ce1d5323d1411e


---
# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>Opcje i wydajność usługi SQL Database: poznaj, co jest dostępne w poszczególnych warstwach usług
Usługa [Azure SQL Database](sql-database-technical-overview.md) oferuje trzy warstwy usług, **Podstawowa**, **Standardowa** i **Premium**, o różnych poziomach wydajności do obsługi różnych obciążeń. Wyższe poziomy wydajności zapewniają coraz więcej zasobów przeznaczonych do dostarczenia coraz wyższej przepustowości. [Warstwy usług i poziomy wydajności możesz zmieniać dynamicznie](sql-database-scale-up.md) bez przestojów. Wszystkie warstwy usług (Podstawowa, Standardowa i Premium) są objęte umową SLA zapewniającą czas działania 99,99% i oferują elastyczne opcje ciągłości działania, funkcje zabezpieczeń i rozliczenia godzinowe. 

Na wybranym [poziomie wydajności](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) możesz utworzyć pojedyncze bazy danych z dedykowanymi zasobami. Wieloma bazami danych możesz także zarządzać w [puli elastycznej](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus), w której zasoby są współdzielone przez bazy danych. Zasoby dostępne dla pojedynczych baz danych są wyrażane jako liczba jednostek DTU (Database Transaction Unit), a dla pul elastycznych w postaci jednostek eDTU (elastic Database Transaction Unit). Aby uzyskać więcej informacji na temat jednostek DTU i eDTU, zobacz [Co to jest jednostka DTU?](sql-database-what-is-a-dtu.md). 

W obu przypadkach do wyboru są następujące warstwy usługi: **Podstawowa**, **Standardowa** i **Premium**. 

## <a name="choosing-a-service-tier"></a>Wybieranie warstwy usług
Tabela poniżej zawiera przykłady warstw najlepiej dopasowane do różnych obciążeń aplikacji.

| Warstwa usług | Docelowe obciążenia |
| :--- | --- |
| **Podstawowa** | Najodpowiedniejsza dla małych baz danych. Standardowo zapewnia obsługę jednej aktywnej operacji w danym momencie. Przykłady zastosowania to bazy danych używane do tworzenia i testowania oprogramowania lub niewielkie, rzadko używane aplikacje. |
| **Standardowa** |Najczęściej wybierana opcja dla aplikacji w chmurze z niewielkimi lub średnimi wymaganiami dotyczącymi wydajności operacji we/wy i obsługą wielu równoczesnych zapytań. Przykładowe zastosowania to aplikacje grupy roboczej lub sieci Web. |
| **Premium** | Przeznaczona dla dużych ilości transakcji z wysokimi wymaganiami dotyczącymi wydajności operacji we/wy przy obsłudze wielu równoczesnych użytkowników. Przykładem zastosowania mogą być bazy danych dla aplikacji o znaczeniu krytycznym. |

Najpierw zdecyduj, czy chcesz korzystać z pojedynczej bazy danych, czy chcesz utworzyć grupę baz danych, które współdzielą zasoby. Przejrzyj [zagadnienia dotyczące puli elastycznej](sql-database-elastic-pool-guidance.md). Wybierając warstwę usług, rozpocznij od ustalenia minimalnego potrzebnego zakresu funkcji bazy danych:

* Maksymalny rozmiar pojedynczej bazy danych (maksymalnie 2 GB dla warstwy Podstawowa, maksymalnie 250 GB dla warstwy Standardowa i od 500 GB do 1 TB dla warstwy Premium przy najwyższych poziomach wydajności)
* Maksymalna całkowita ilość miejsca w magazynie w przypadku puli elastycznej (117 GB dla warstwy Podstawowa, 1200 GB dla warstwy Standardowa i 750 GB dla warstwy Premium)
* Maksymalna liczba baz danych w jednej puli (400 dla warstwy Podstawowa, 400 dla warstwy Standardowa i 50 dla warstwy Premium)
* Okres przechowywania kopii zapasowej bazy danych (7 dni dla warstwy Podstawowa oraz 35 dni dla warstw Standardowa i Premium)

Po określeniu minimalnej warstwy usług możesz określić poziom wydajności bazy danych (liczbę jednostek DTU). W wielu przypadkach na początek odpowiednie są standardowe poziomy wydajności S2 i S3. W przypadku baz danych mających wysokie wymagania dotyczące procesora CPU lub operacji we/wy warto rozpocząć od poziomów wydajności Premium. Warstwa Premium oferuje więcej mocy procesora i rozpoczyna się od dziesięciokrotnie większej liczby operacji we/wy w porównaniu do najwyższego poziomu wydajności warstwy Standardowa.

Po wybraniu początkowego poziomu wydajności możesz następnie dynamicznie skalować [poszczególne bazy danych](sql-database-scale-up.md) lub [pulę elastyczną](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) w górę lub w dół na podstawie rzeczywistego użycia. Na potrzeby scenariuszy migracji możesz również obliczyć przybliżoną liczbę potrzebnych jednostek DTU za pomocą [Kalkulatora DTU](http://dtucalculator.azurewebsites.net/). 

>
> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

## <a name="single-database-service-tiers-and-performance-levels"></a>Warstwy usług i poziomy wydajności pojedynczej bazy danych
Istnieje wiele poziomów wydajności dla pojedynczej bazy danych w poszczególnych warstwach usług. Możesz swobodnie wybrać poziom, który najlepiej spełnia wymagania danego obciążenia. W razie potrzeby skalowania w górę lub w dół możesz łatwo zmienić warstwy bazy danych. Szczegółowe informacje znajdziesz w artykule [Changing Database Service Tiers and Performance Levels](sql-database-scale-up.md) (Zmiana warstw usług i poziomów wydajności bazy danych).

Niezależnie od liczby hostowanych baz danych każda z nich ma zagwarantowany zestaw zasobów, a ich oczekiwane charakterystyki nie ulegają zmianom.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!NOTE]
> Szczegółowy opis wszystkich innych wierszy w tej tabeli warstw usług można znaleźć w [zestawieniu możliwości i ograniczeń warstw usług](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).
> 

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Warstwy usług puli elastycznej oraz wydajność w jednostkach eDTU

Pule umożliwiają bazom danych współdzielenie i używanie zasobów jednostek eDTU bez konieczności przypisywania konkretnego poziomu wydajności do każdej bazy danych w puli. Na przykład pojedyncza baza danych w puli Standardowej może przejść od używania 0 jednostek eDTU do maksymalnej liczby jednostek eDTU ustalonej dla bazy danych podczas konfigurowania puli. Dzięki pulom wiele baz danych z różnymi obciążeniami wydajnie korzysta z zasobów eDTU dostępnych w całej puli. Zobacz szczegółowe [zagadnienia dotyczące cen i wydajności puli elastycznej](sql-database-elastic-pool-guidance.md).

W poniższej tabeli opisano charakterystyki warstw usług dotyczących puli.

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Każda baza danych w puli podlega również charakterystykom pojedynczej bazy danych w danej warstwie. Na przykład pula Podstawowa ma limit sesji dla jednej puli z zakresu 4800–28800, ale poszczególne bazy danych w puli Podstawowej mają limit bazy danych równy 300 sesji.

## <a name="next-steps"></a>Następne kroki

* Poznaj lepiej [pule elastyczne](sql-database-elastic-pool-guidance.md) oraz [zagadnienia dotyczące cen i wydajności pul elastycznych](sql-database-elastic-pool-guidance.md).
* Naucz się [monitować pule elastyczne, zarządzać nimi i zmieniać ich rozmiar](sql-database-elastic-pool-manage-portal.md) oraz [monitorować wydajność pojedynczych baz danych](sql-database-single-database-monitor.md).
* Teraz, kiedy znasz warstwy usługi SQL Database, wypróbuj je, korzystając z [bezpłatnego konta](https://azure.microsoft.com/pricing/free-trial/) i dowiedz się, jak [utworzyć pierwszą bazę danych SQL](sql-database-get-started.md).




<!--HONumber=Dec16_HO3-->


