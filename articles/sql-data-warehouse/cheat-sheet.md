---
title: "Ściągawka Azure SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Łącza, najlepsze rozwiązania w celu szybkiego tworzenia rozwiązań magazyn danych SQL Azure."
services: sql-data-warehouse
documentationcenter: NA
author: acomet
manager: jhubbard
editor: 
ms.assetid: 51f1e444-9ef7-4e30-9a88-598946c45196
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 12/14/2017
ms.author: acomet
ms.openlocfilehash: 2d17385ff255ddf7b85baa81600a2af60d015540
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2018
---
# <a name="cheat-sheet-for-azure-sql-data-warehouse"></a>Ściągawka dla usługi Azure SQL Data Warehouse
Ta strona powinna ułatwić projektowanie dla głównego Użyj przypadków rozwiązania magazynu danych. Ten arkusz ze wskazówkami dotyczącymi powinien być doskonałą pomoc techniczną w podróży do utworzenia magazynu danych światowej klasy, ale zdecydowanie zaleca się dodatkowe informacje o każdym etapem szczegóły. Po pierwsze zaleca się odczytu w tym artykule dużą o jakie SQL Data Warehouse  **[i nie jest]**.

Poniżej znajduje się szkic procesu, który należy wykonać podczas uruchamiania projektu usługi SQL Data Warehouse.

![Szkicu]

## <a name="queries-and-operations-across-tables"></a>Operacje między tabel i zapytań

Jest to naprawdę musisz znać wcześniej najważniejszych operacji i zapytania miejsce w magazynie danych. Architektury magazynu danych powinny mieć priorytet dla tych operacji. Przykłady typowych operacji może być:
* Sprzęganie co najmniej dwie tabele faktów z wymiarem tabel, filtrowania tej tabeli w danym okresie czasu i dołączyć wyniki do składnicy danych
* Wprowadzanie duże lub małe aktualizacji do sprzedaży fakt
* Dołączanie danych tylko do tabel

Znajomość typu operacje pomaga zoptymalizować projektowanie tabelach.

## <a name="data-migration"></a>Migrowanie danych

Zaleca się pierwszym załadowaniu danych  **[do ADLS]**  lub magazynu obiektów Blob Azure. Następnie należy używać programu Polybase do ładowania danych do magazynu danych SQL w tabeli przemieszczania. Firma Microsoft zaleca następującej konfiguracji:

| Projektowanie | Zalecenie |
|:--- |:--- |
| Dystrybucja | Działanie okrężne |
| Indeksowanie | Sterty |
| Partycjonowanie | Brak |
| Klasa zasobów | largerc lub xlargerc |

Dowiedz się więcej o  **[migracji danych], [ładowanie danych]**  z  **[głębiej wskazówki] na ładowanie**. 

## <a name="distributed-or-replicated-tables"></a>Rozproszone lub zreplikowanych tabel

Zaleca się w zależności od właściwości tabeli następujących strategii:

| Typ | Doskonałym rozwiązaniem dla | Uważaj if...|
|:--- |:--- |:--- |
| Replikacja | Tabele wymiarów małych • w schemat gwiazdy z mniej niż 2 GB pamięci masowej po kompresji (kompresji ~ 5 x) |• Wiele transakcji zapisu w tabeli (np.: Wstaw upsert, usuwanie, aktualizacja)<br></br>• Zmienić udostępniania często wartości jednostki magazynu danych (DWU)<br></br>• Można używać tylko kolumny 2 – 3 i tabeli zawiera wiele kolumn<br></br>• Indeksu zreplikowanej tabeli |
| Działanie okrężne (ustawienie domyślne) | • Tabeli tymczasowej/przemieszczania<br></br> • Nie oczywiste Sprzęganie kolumn klucza lub dobrej candidate |• Wydajności z powodu przemieszczenia danych |
| Skrót | Tabele faktów •<br></br>• Wymiaru dużych tabel |Nie można zaktualizować • dystrybucji kluczy |

**Wskazówki:**
* Rozpoczynać działanie okrężne, ale Oczekuj strategii dystrybucji wyznaczania wartości skrótu móc korzystać z ogromną architektura równoległa
* Upewnij się, że typowe klawisze skrótów mają ten sam format danych
* Nie dystrybucji na varchar format
* Tabele wymiarów wspólny klucz skrót do tabeli faktów z operacji sprzężenia często może być rozpowszechniane skrót
* Użyj  *[sys.dm_pdw_nodes_db_partition_stats]*  do analizowania skośność żadnych danych
* Użyj  *[sys.dm_pdw_request_steps]*  do analizowania danych przemieszczania za zapytania, monitorowanie emisji czasu i losowa operacji. Warto przejrzeć strategii dystrybucji.

Dowiedz się więcej o  **[zreplikowane tabele] i [rozproszonych tabel]**.

## <a name="indexing-your-table"></a>Indeksowanie tabeli

Indeksowanie **dużą** szybko odczytywanie tabel. Jest unikatowy zestaw technologii, których można używać na podstawie Twoich potrzeb:

| Typ | Doskonałym rozwiązaniem dla | Uważaj if...|
|:--- |:--- |:--- |
| Sterty | • Tabeli przemieszczania/tymczasowej<br></br>• Małe tabele z małych wyszukiwań |• Tabeli pełna skanuje wszystkie wyszukiwania |
| Indeks klastrowany | • Maksymalnie 100-m wierszy tabeli<br></br>• Dużych tabel (więcej niż 100 mln wierszy) z tylko 1 i 2 kolumny silnie są używane. |• Używane na zreplikowanej tabeli<br></br>• Obejmujące wiele sprzężenia, Group By operacji złożonych zapytań.<br></br>• Sprawdź aktualizacje na indeksowanych kolumn, przyjmuje pamięci |
| Klastrowany indeks magazynu kolumn (WIK) (ustawienie domyślne) | • Dużych tabel (więcej niż 100 mln wierszy) | • Używane na zreplikowanej tabeli<br></br>• Wprowadzone w bardzo dużej aktualizowanie operacji dla tabeli<br></br>• Nadmiernie partycji tabeli: nie obejmują grupy wierszy na partycje i dystrybucji różnych węzłów |

**Wskazówki:**
* Na indeks klastrowany można dodać indeks nieklastrowany do kolumny intensywnie używany dla filtru. 
* Należy zachować ostrożność sposób zarządzania pamięci dla tabeli z CCI. Podczas ładowania danych ma użytkownik (lub zapytanie) do korzystania z klasy dużych zasobów. Należy upewnić się uniknąć przycinanie i tworzenie wielu grup małych skompresowany wiersza
* Zoptymalizowana pod kątem skały obliczeniowe warstwy z CCI
* Dla WIK niską wydajność może się zdarzyć z powodu słabej kompresja grup wierszy, możesz ponownie utworzyć lub zreorganizować WIK Twojego. Ma co najmniej 100 wierszy k skompresowany grupy wierszy. Idealna to 1 mln wierszy w grupie wiersza.
* Na podstawie częstotliwości przyrostowe obciążenia i rozmiaru, chcesz zautomatyzować reorganizacji lub Odbuduj indeksy użytkownika. Pomocne jest zawsze czyszczenie sprężyny.
* Można strategicznych, aby przyciąć grupy wierszy: rozmiar, jaki to grupy Otwórz wiersz? Ilość danych będą ładowane w ciągu najbliższych dni?

Dowiedz się więcej o  **[indeksów]**.

## <a name="partitioning"></a>Partycjonowanie
Może partycji tabeli, gdy istnieją tabele faktów duże (> 1B wiersz tabeli). 99% przypadków, klucz partycji powinny być oparte na datę. Należy zachować ostrożność nie nadmierne partycji, zwłaszcza w przypadku klastrowanego indeksu magazynu kolumn.
Z tabel, które wymagają ETL przemieszczania, mogą korzystać z partycjonowania. Ułatwia ona zarządzanie cyklem życia danych.
Nie można więc overpartition danych, szczególnie w przypadku klastrowanego indeksu magazynu kolumn.

Dowiedz się więcej o  **[partycje]**.

## <a name="incremental-load"></a>Przyrostowe obciążenia

Należy najpierw upewnij się, że przydzielisz większe grupy zasobów do ładowania danych. Zaleca się przy użyciu programu Polybase i ADF V2 do automatyzacji programu potoki ETL do magazynu danych SQL.

Dla dużych partii aktualizacji w danych historycznych zaleca się najpierw usuwania danych danych. Następnie możesz wprowadzić zbiorczego wstawiania nowych danych. Takie podejście kroku 2 jest bardziej wydajny.

## <a name="maintain-statistics"></a>Prowadzenie statystyk
Automatycznie statystyki mają być ogólnie dostępne wkrótce. Do tego czasu usługa SQL Data Warehouse wymaga ręcznej obsługi statystyk. Ważne jest, aby zaktualizować statystyk jako **znaczne** wprowadzenia zmiany danych. Dzięki temu można zoptymalizować swoje plany zapytań. Jeśli okaże się, że trwa zbyt długo, aby zachować wszystkie statystyk, można być kolumny, które mają statystyk. Można również zdefiniować częstotliwość aktualizacji. Można na przykład aktualizować kolumny danych, w których codziennie mogą być dodawane nowe wartości. Aby uzyskać największe korzyści, uzyskanie statystyki do kolumn uczestniczących w sprzężenia, kolumn używana w klauzuli WHERE i kolumn w GROUP BY.

Dowiedz się więcej o  **[statystyki]**.

## <a name="resource-class"></a>Klasa zasobów
Usługa SQL Data Warehouse korzysta z grup zasobów w celu przydzielania pamięci zapytaniom. Jeśli potrzebujesz większej ilości pamięci do ulepszenia zapytania i szybkość ładowania, należy przydzielić wyższej klasy zasobów. Na stronie Przerzucanie przy użyciu klasy zasobów o większych ma wpływ współbieżności. Chcesz uwzględniać go przed przeniesieniem wszystkich użytkowników do klasy dużych zasobów.

Jeśli zauważysz, że zapytania trwa zbyt długo, sprawdź, czy użytkownicy nie są uruchamiane w dużych zasobów klasy. Klasy zasobów o dużych używać wielu miejsc współbieżności. Może spowodować inne zapytania do kolejki.

Na koniec użycie warstwy obliczeniowe zoptymalizowanych pod kątem każdej klasy zasobu pobiera 2,5 x więcej pamięci niż w warstwie elastycznej zoptymalizowanych pod kątem.

Dowiedz się więcej, jak pracować z  **[klasy zasobów i współbieżność]**.

## <a name="lower-your-cost"></a>Niższy koszt
Kluczową cechą usługi SQL Data Warehouse jest możliwość jej wstrzymania w sytuacji, gdy nie jest ona używana. Pozwala to przerwać naliczanie opłat za zasoby obliczeniowe. Inną niezwykle ważną cechą jest możliwość skalowania zasobów. Wstrzymywanie i skalowanie można przeprowadzać w witrynie Azure Portal lub za pomocą poleceń programu PowerShell.

Teraz podczas automatycznego skalowania ma w środowisku Azure Functions:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

## <a name="optimize-you-architecture-for-performance"></a>Optymalizacja możesz architektura wydajności

Firma Microsoft zaleca, biorąc bazy danych SQL i usług Azure Analysis Services w architekturze koncentratora i klienci. Rozwiązania zapewniają izolację obciążenia między grupami innego użytkownika, a także wykorzystanie niektóre zaawansowane funkcje zabezpieczeń z bazy danych SQL i usług Azure Analysis Services. To jest również sposób zapewnia współbieżność nieograniczona dla użytkowników.

Dowiedz się więcej o  **[typowe architektury wykorzystaniu magazynu danych SQL]**.

Wdrażanie w kliknięcie z wymienionymi w bazach danych bazy danych SQL z magazynu danych SQL:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>


<!--Image references-->
[Szkicu]:media/sql-data-warehouse-cheat-sheet/picture-flow.png

<!--Article references-->
[ładowanie danych]:./design-elt-data-loading.md
[głębiej wskazówki]: ./guidance-for-loading-data.md
[indeksów]:./sql-data-warehouse-tables-index.md
[partycje]:./sql-data-warehouse-tables-partition.md
[statystyki]:./sql-data-warehouse-tables-statistics.md
[klasy zasobów i współbieżność]:./sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[typowe architektury wykorzystaniu magazynu danych SQL]: https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/
[i nie jest]:https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns/
[migracji danych]:https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
[zreplikowane tabele]:https://docs.microsoft.com/azure/sql-data-warehouse/design-guidance-for-replicated-tables
[rozproszonych tabel]:https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute
[do ADLS]: https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-store
[sys.dm_pdw_nodes_db_partition_stats]: https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql
[sys.dm_pdw_request_steps]:https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql
