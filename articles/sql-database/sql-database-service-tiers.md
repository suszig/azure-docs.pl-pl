<properties
    pageTitle="Wydajność i opcje usługi SQL Database: warstwy usług | Microsoft Azure"
    description="Porównaj funkcje usługi SQL Database dotyczące wydajności i ciągłości działania w poszczególnych warstwach usług w celu zbilansowania kosztów i możliwości podczas skalowania."
    keywords="database options,database performance"
    services="sql-database"
    documentationCenter=""
    authors="carlrabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="05/13/2016"
    ms.author="carlrab"/>

# Opcje i wydajność usługi SQL Database: poznaj, co jest dostępne w poszczególnych warstwach usług

Usługa [Azure SQL Database](sql-database-technical-overview.md) ma wiele warstw do obsługi różnych obciążeń. Warstwy usług można zmieniać w dowolnym momencie bez żadnej przerwy w działaniu aplikacji. Można również [utworzyć pojedynczą bazę danych](sql-database-get-started.md) o zdefiniowanej charakterystyce i cenie albo zarządzać wieloma bazami danych dzięki [utworzeniu elastycznej puli baz danych](sql-database-elastic-pool-create-portal.md). W obu przypadkach do wyboru są warstwy: **Podstawowa**, **Standardowa** i **Premium**. Opcje bazy danych w tych warstwach są podobne dla pojedynczych baz danych i pul elastycznych, ale w przypadku pul trzeba rozważyć dodatkowe zagadnienia. Ten artykuł zawiera szczegółowe informacje na temat warstw usług dla pojedynczych i elastycznych baz danych.

## Warstwy usług i opcje baz danych
Wszystkie warstwy usług (Podstawowa, Standardowa i Premium) są objęte umową SLA zapewniającą czas działania 99,99% i oferują przewidywalną wydajność, elastyczne opcje ciągłości działania, funkcje zabezpieczeń i rozliczenia godzinowe. Tabela poniżej zawiera przykłady warstw najlepiej dopasowane do różnych obciążeń aplikacji.

| Warstwa usług | Docelowe obciążenia |
|---|---|
| **Podstawowa** | Najodpowiedniejsza dla małych baz danych. Standardowo zapewnia obsługę jednej aktywnej operacji w danym momencie. Przykłady zastosowania to bazy danych używane do tworzenia i testowania oprogramowania lub niewielkie, rzadko używane aplikacje. |
| **Standardowa** | Odpowiednia dla większości aplikacji w chmurze. Zapewnia obsługę wielu równoczesnych zapytań. Przykładowe zastosowania to aplikacje grupy roboczej lub sieci Web. |
| **Premium** | Przeznaczona do obsługi dużej ilości danych transakcyjnych. Obsługuje wielu równoczesnych użytkowników i spełnia wymagania ciągłości działalności biznesowej na najwyższym poziomie. Przykładem zastosowania mogą być bazy danych dla aplikacji o znaczeniu krytycznym. |

>[AZURE.NOTE] Wersje Sieć Web i Business zostały wycofane. Przeczytaj Często zadawane pytania w artykule [Cennik usługi Baza danych SQL w warstwach Sieć Web i Business (do wycofania)](https://azure.microsoft.com/pricing/details/sql-database/web-business/), jeśli zamierzasz nadal korzystać z tych wersji.

## Warstwy usług i poziomy wydajności pojedynczej bazy danych
Istnieje wiele poziomów wydajności dla pojedynczej bazy danych w poszczególnych warstwach usług. Możesz swobodnie wybrać poziom, który najlepiej spełnia wymagania danego obciążenia. W razie potrzeby skalowania w górę lub w dół możesz łatwo zmienić warstwę bazy danych **bez przerw w działaniu aplikacji**. Szczegółowe informacje znajdziesz w artykule [Changing Database Service Tiers and Performance Levels](sql-database-scale-up.md) (Zmiana warstw usług i poziomów wydajności bazy danych).

Charakterystyki wydajności wymienione poniżej dotyczą baz danych utworzonych przy użyciu [usługi SQL Database V12](sql-database-v12-whats-new.md). W sytuacjach, gdy sprzęt w usłudze Azure obsługuje wiele baz danych, każda z nich ma zagwarantowany zestaw zasobów, a ich oczekiwane charakterystyki nie ulegają zmianom.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

W celu lepszego zrozumienia jednostek DTU zobacz część [Jednostki DTU](#understanding-dtus) w tym artykule.

>[AZURE.NOTE] Szczegółowy opis wszystkich innych wierszy w tej tabeli warstw usług można znaleźć w [zestawieniu możliwości i ograniczeń warstw usług](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).

## Warstwy usług puli elastycznej oraz wydajność w jednostkach eDTU
Oprócz tworzenia i skalowania pojedynczej bazy danych istnieje również możliwość zarządzania wieloma bazami danych w ramach [puli elastycznej](sql-database-elastic-pool.md). Wszystkie bazy danych w puli elastycznej korzystają ze wspólnego zestawu zasobów. Charakterystyki wydajności są mierzone w *elastycznych jednostkach transakcji bazy danych* (eDTU). Podobnie jak pojedyncze bazy danych, pule są realizowane w trzech warstwach usług: **Podstawowej**, **Standardowej** i **Premium**. Te trzy warstwy usług również w przypadku pul definiują limity ogólnej wydajności i zakres funkcji.

Pule zezwalają elastycznym bazom danych na współdzielenie i używanie zasobów jednostek DTU bez konieczności przypisywania konkretnego poziomu wydajności do baz danych w puli. Na przykład pojedyncza baza danych w puli Standardowej może przejść od używania 0 jednostek eDTU do maksymalnej liczby jednostek eDTU ustalonej dla bazy danych podczas konfigurowania puli. Dzięki temu wiele baz danych z różnymi obciążeniami wydajnie korzysta z zasobów eDTU dostępnych w całej puli. Zobacz szczegółowe [zagadnienia dotyczące cen i wydajności puli elastycznej](sql-database-elastic-pool-guidance.md).

W poniższej tabeli opisano charakterystyki warstw usług dotyczących puli.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Każda baza danych w puli podlega również charakterystykom pojedynczej bazy danych w danej warstwie. Na przykład pula Podstawowa ma limit sesji dla jednej puli z zakresu 4800–28 800, ale poszczególne bazy danych w tej puli mają limit bazy danych równy 300 sesji (limit pojedynczej Podstawowej bazy danych podany w poprzedniej części).

## Jednostki DTU

[AZURE.INCLUDE [SQL DB DTU description](../../includes/sql-database-understanding-dtus.md)]

## Następne kroki
- Sprawdź ceny warstw na stronie [Cennik usługi SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).
- Poznaj lepiej [elastyczne pule baz danych](sql-database-elastic-pool-guidance.md) oraz [zagadnienia dotyczące cen i wydajności elastycznych pul baz danych](sql-database-elastic-pool-guidance.md).
- Naucz się [monitować pule elastyczne, zarządzać nimi i zmieniać ich rozmiar](sql-database-elastic-pool-manage-portal.md) oraz [monitorować wydajność pojedynczych baz danych](sql-database-single-database-monitor.md).
- Teraz, kiedy znasz warstwy usługi SQL Database, wypróbuj je, korzystając z [bezpłatnego konta](https://azure.microsoft.com/pricing/free-trial/) i dowiedz się, jak [utworzyć pierwszą bazę danych SQL](sql-database-get-started.md).



<!--HONumber=Jun16_HO2-->


