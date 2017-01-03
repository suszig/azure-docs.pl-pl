---
title: Co to jest pula elastyczna Azure? | Microsoft Docs
description: "Zarządzanie setkami lub tysiącami baz danych przy użyciu puli. Jedna cena za zestaw jednostek wydajności może być dystrybuowana za pośrednictwem puli. Przenoszenie baz danych do i z puli wedle uznania."
keywords: elastic database,sql databases
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: b46e7fdc-2238-4b3b-a944-8ab36c5bdb8e
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.date: 12/14/2016
ms.author: CarlRabeler
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 75bf523679c8d8ad6fbe4a8aa8a561d03008e59b
ms.openlocfilehash: c3757dadb09ba070b30820a46007a9c82490d8f2


---
# <a name="what-is-an-azure-elastic-pool"></a>Co to jest pula elastyczna Azure?
Pule elastyczne zapewniają proste i ekonomiczne rozwiązanie umożliwiające zarządzanie celami wydajności dla wielu baz danych o znacznie zróżnicowanych i nieprzewidywalnych wzorcach użycia.

> [!NOTE]
> Pule elastyczne są ogólnodostępne we wszystkich regionach platformy Azure oprócz Indii Zachodnich, gdzie są obecnie dostępne w wersji zapoznawczej.  Pule elastyczne zostaną udostępnione ogólnie w tych regionach tak szybko, jak to możliwe.
>
>

## <a name="how-it-works"></a>Jak to działa
Typowym wzorcem aplikacji SaaS jest model bazy danych z jedną dzierżawą: każdy klient otrzymuje własną bazę danych. Każdy klient (baza danych) ma nieprzewidywalne wymagania dotyczące zasobów pamięci, we/wy i procesora CPU. Jak przy tych okresowych wzrostach i spadkach zapotrzebowania przydzielić zasoby wydajnie i ekonomicznie? Tradycyjnie dostępne były dwie opcje: (1) nadmiarowa aprowizacja zasobów na podstawie szczytowego użycia i uiszczanie nadmiernych opłat lub (2) aprowizacja zasobów na niedostatecznym poziomie w celu obniżenia opłat, kosztem wydajności i zadowolenia klientów w okresach szczytowego użycia. Pule elastyczne rozwiązują ten problem przez zapewnienie, że bazy danych uzyskują potrzebne im zasoby wydajności wtedy, kiedy ich potrzebują. Udostępniają one prosty mechanizm alokacji zasobów w ramach przewidywalnego budżetu. Aby dowiedzieć się więcej na temat wzorców projektowych dla aplikacji SaaS wykorzystujących pule elastyczne, zobacz artykuł [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) (Wzorce projektowe dla wielodostępnych aplikacji SaaS korzystających z usługi Azure SQL Database).

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>
>

W usłudze SQL Database względna miara możliwości obsługi zapotrzebowania na zasoby bazy danych jest wyrażona w jednostkach transakcji bazy danych (DTU) dla pojedynczych baz danych oraz w elastycznych jednostkach DTU (eDTU) dla elastycznych baz danych w puli elastycznej. Aby dowiedzieć się więcej na temat jednostek DTU i eDTU, zobacz [Wprowadzenie do usługi SQL Database](sql-database-technical-overview.md).

Puli jest przydzielana określona liczba jednostek eDTU za określoną cenę. Poszczególne bazy danych w ramach puli mają możliwość elastycznego skalowania automatycznego w określonym zakresie parametrów. W przypadku dużego obciążenia baza danych może wykorzystywać więcej jednostek eDTU w odpowiedzi na zapotrzebowanie. Bazy danych pod niskim obciążeniem wykorzystują mniej jednostek eDTU, a bazy danych bez obciążenia nie zużywają jednostek eDTU. Aprowizacja zasobów dla całej puli zamiast pojedynczych baz danych upraszcza zadania związane z zarządzaniem. Ponadto pule mają przewidywalny budżet.

Dodatkowe jednostki eDTU można dodać do istniejącej puli bez przerwy w działaniu baz danych ani wpływu na bazy danych w puli elastycznej. Podobnie, jeśli dodatkowe jednostki eDTU nie są już potrzebne, można je usunąć z istniejącej puli w dowolnej chwili.

Można również dodawać bazy danych do puli lub odejmować je z niej. Jeśli baza danych przewidywalnie niewystarczająco wykorzystuje zasoby, należy ją przenieść.

## <a name="which-databases-go-in-a-pool"></a>Które bazy danych warto umieścić w puli?
![Bazy danych SQL współużytkujące jednostki eDTU w puli elastycznej.][1]

Bazy danych, które świetnie nadają się dla pul elastycznych, zwykle mają okresy aktywności i okresy braku aktywności. W powyższym przykładzie widać aktywność pojedynczej bazy danych, czterech bazy danych, a na końcu pulę elastyczną z dwudziestoma bazami danych. Bazy danych o zróżnicowanym poziomie aktywności w czasie są doskonałymi kandydatami dla pul elastycznych, ponieważ nie wszystkie są aktywne w tym samym czasie i mogą współużytkować jednostki eDTU. Nie wszystkie bazy danych pasują do tego wzorca. Bazy danych, które mają bardziej stałe zapotrzebowanie na zasoby, są lepiej dopasowane do warstw usługi Podstawowa, Standardowa i Premium, w których zasoby są przypisane indywidualnie.

[Zagadnienia dotyczące cen i wydajności puli elastycznej](sql-database-elastic-pool-guidance.md).

## <a name="edtu-and-storage-limits-for-elastic-pools-and-elastic-databases"></a>Limity liczby jednostek eDTU i magazynu dla pul elastycznych i elastycznych baz danych

W poniższej tabeli przedstawiono charakterystykę pul elastycznych w warstwie Podstawowa, Standardowa i Premium.

[!INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Jeśli używane są wszystkie jednostki DTU puli elastycznej, każda baza danych w puli otrzymuje taką samą ilość zasobów do przetwarzania zapytań.  Usługa SQL Database zapewnia sprawiedliwe udostępnianie zasobów między bazami danych przez zapewnienie równych okresów czasu obliczeń. Sprawiedliwe udostępnianie zasobów puli elastycznej jest wykonywane oprócz zapewniania dowolnej ilości zasobów w przeciwnym razie gwarantowanej dla każdej bazy danych, gdy minimalna liczba jednostek DTU na bazę danych jest ustawiona na wartość różną od zera.

## <a name="elastic-pool-and-elastic-database-properties"></a>Właściwości puli elastycznej i elastycznej bazy danych

W poniższej tabeli opisano limity dla pul elastycznych i elastycznych baz danych.

### <a name="limits-for-elastic-pools"></a>Limity dla pul elastycznych
| Właściwość | Opis |
|:--- |:--- |
| Warstwa usług |Podstawowa, Standardowa lub Premium. Warstwa usługi określa zakres limitów wydajności i magazynu, które można skonfigurować, a także opcje zapewniania ciągłości działalności biznesowej. Każda baza danych w puli ma tę samą warstwę usługi co pula. „Warstwa usługi” jest również nazywana „edycją”. |
| Jednostki eDTU na pulę |Maksymalna liczba jednostek eDTU, która może być udostępniana bazom danych w puli. Całkowita liczba jednostek eDTU używanych przez bazy danych w puli w tym samym momencie nie może przekroczyć tego limitu. |
| Maksymalny rozmiar magazynu na pulę (GB) |Maksymalna ilość miejsca do magazynowania (w GB), która może być udostępniana bazom danych w puli. Całkowita ilość miejsca używanego przez bazy danych w puli nie może przekroczyć tego limitu. Ten limit jest określany przez liczbę jednostek eDTU na pulę. W razie przekroczenia tego limitu wszystkie bazy danych stają się tylko do odczytu. |
| Maksymalna liczba baz danych na pulę |Maksymalna dozwolona liczba baz danych na pulę. |
| Maksymalna liczba współbieżnych procesów roboczych na pulę |Maksymalna liczba współbieżnych procesów roboczych (żądań) dostępna dla wszystkich baz danych w puli. |
| Maksymalna liczba współbieżnych logowań na pulę |Maksymalna liczba współbieżnych logowań dla wszystkich baz danych w puli. |
| Maksymalna liczba współbieżnych sesji na pulę |Maksymalna liczba sesji dostępna dla wszystkich baz danych w puli. |

### <a name="limits-for-elastic-databases"></a>Limity dla elastycznych baz danych
| Właściwość | Opis |
|:--- |:--- |
| Maksymalna liczba jednostek eDTU na bazę danych |Maksymalna liczba jednostek eDTU, z których może korzystać dowolna baza danych w puli, jeśli są dostępne na podstawie użycia innych baz danych w puli.  Maksymalna liczba jednostek eDTU na bazę danych nie jest gwarancją zasobów dla bazy danych.  To ustawienie jest ustawieniem globalnym, które ma zastosowanie do wszystkich baz danych w puli. Należy ustawić odpowiednio wysoką maksymalną liczbę jednostek eDTU na bazę danych, aby obsłużyć szczytowe użycia baz danych. Oczekiwany jest pewien stopień nadmiernego przydzielania, ponieważ pula ogólnie zakłada wzorce gorącego i zimnego użycia dla baz danych, w których nie wszystkie bazy danych jednocześnie osiągają szczytowe użycie. Załóżmy na przykład, że użycie szczytowe na bazę danych wynosi 20 eDTU i tylko 20% ze 100 baz danych w puli osiąga szczytowe użycie w tym samym czasie.  Jeśli maksymalna liczba jednostek eDTU na bazę danych wynosi 20 eDTU, zasadne jest nadmiarowe przydzielenie pięciokrotnie większej liczby eDTU dla puli i ustawienie liczby 400 jednostek eDTU na pulę. |
| Minimalna liczba jednostek eDTU na bazę danych |Minimalna liczba jednostek eDTU gwarantowana dla każdej bazy danych w puli.  To ustawienie jest ustawieniem globalnym, które ma zastosowanie do wszystkich baz danych w puli. Minimalna liczba jednostek eDTU na bazę danych może być ustawiona na 0 i jest to również wartość domyślna. Tę właściwość można ustawić na dowolną wartość między 0 a średnim użyciem jednostek eDTU na bazę danych. Iloczyn liczby baz danych w puli i minimalnej liczby jednostek eDTU na bazę danych nie może przekraczać liczby jednostek eDTU na pulę.  Na przykład jeśli pula zawiera 20 baz danych, a minimalna liczba jednostek eDTU na bazę danych wynosi 10 eDTU, liczba jednostek eDTU na pulę musi wynosić co najmniej 200 eDTU. |
| Maksymalny rozmiar magazynu na bazę danych (GB) |Maksymalny rozmiar magazynu dla bazy danych w puli. Elastyczne bazy danych współużytkują magazyn puli, a więc magazyn bazy danych jest ograniczony do rozmiaru pozostałego magazynu puli lub maksymalnego rozmiaru magazynu na bazę danych (do mniejszej z tych wartości). |

## <a name="elastic-database-jobs"></a>Zadania elastycznych baz danych
W przypadku puli zadania zarządzania są uproszczone dzięki uruchamianiu skryptów w **[zadaniach elastycznych](sql-database-elastic-jobs-overview.md)**. Zadanie elastycznej bazy danych eliminuje większość monotonnych czynności związanych z dużą liczbą baz danych. Aby rozpocząć, zobacz [Getting started with Elastic Database jobs](sql-database-elastic-jobs-getting-started.md) (Wprowadzenie do zadań elastycznych baz danych).

Aby uzyskać więcej informacji na temat innych narzędzi elastycznej bazy danych, zobacz [Scaling out with Azure SQL Database](sql-database-elastic-scale-introduction.md) (Skalowanie w poziomie za pomocą usługi Azure SQL Database).

## <a name="business-continuity-features-for-databases-in-a-pool"></a>Funkcje ciągłości działalności biznesowej dla baz danych w puli
Elastyczne bazy danych obsługują te same [funkcje ciągłości działalności biznesowej](sql-database-business-continuity.md), które są dostępne dla pojedynczych baz danych.

### <a name="point-in-time-restore"></a>Przywracanie do punktu w czasie
Przywracanie do punktu w czasie używa automatycznych kopii zapasowych bazy danych do odzyskiwania bazy danych w puli z określonego punktu w czasie. Zobacz [Przywracanie do punktu w czasie](sql-database-recovery-using-backups.md#point-in-time-restore).

### <a name="geo-restore"></a>Przywracanie geograficzne
Przywracanie geograficzne zapewnia domyślną opcję odzyskiwania, gdy baza danych jest niedostępna z powodu zdarzenia w regionie, w którym jest hostowana. Zobacz [Restore an Azure SQL Database or failover to a secondary](sql-database-disaster-recovery.md) (Przywracanie usługi Azure SQL Database lub przełączanie w trybie failover do pomocniczej bazy danych).

### <a name="active-geo-replication"></a>Aktywna replikacja geograficzna
Dla aplikacji, które wymagają bardziej agresywnego odzyskiwania niż oferowane przez funkcję przywracania geograficznego, należy skonfigurować aktywną replikację geograficzną za pomocą witryny [Azure Portal](sql-database-geo-replication-portal.md), programu [PowerShell](sql-database-geo-replication-powershell.md) lub języka [Transact-SQL](sql-database-geo-replication-transact-sql.md).

## <a name="additional-resources"></a>Dodatkowe zasoby
* [Kurs wideo w serwisie Microsoft Virtual Academy poświęcony możliwościom elastycznych baz danych](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)

<!--Image references-->
[1]: ./media/sql-database-elastic-pool/databases.png



<!--HONumber=Dec16_HO5-->


