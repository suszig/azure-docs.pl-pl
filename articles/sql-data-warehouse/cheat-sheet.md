---
title: "Ściągawka dla usługi Azure SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Znajdź łącza i najlepsze rozwiązania w celu szybkiego tworzenia rozwiązań magazyn danych SQL Azure."
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
ms.date: 02/20/2018
ms.author: acomet
ms.openlocfilehash: 6c8278be32ae6c6452283ba946a36f363dd4d76d
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="cheat-sheet-for-azure-sql-data-warehouse"></a>Ściągawka dla usługi Azure SQL Data Warehouse
Arkusz ze wskazówkami dotyczącymi ten zawiera przydatne porady i wskazówki umożliwiające tworzenie rozwiązań magazyn danych SQL Azure. Przed rozpoczęciem pracy Dowiedz się więcej o każdym kroku szczegółowo odczytując [wzorce magazynu danych Azure SQL w obciążenie i wzorce przed](https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns), który opisano, co to jest usługa SQL Data Warehouse i co nie jest.

Na poniższym rysunku przedstawiono proces projektowania magazynu danych:

![Szkicu]

## <a name="queries-and-operations-across-tables"></a>Operacje między tabel i zapytań

Jeśli wcześniej znana operacji głównych i zapytań, które mają być wykonywane w magazynie danych, można przypisać priorytety architektury magazynu danych dla tych operacji. Te zapytania i operacje mogą być następujące:
* Sprzęganie co najmniej dwie tabele faktów z tabelami wymiarów, filtrowania połączone tabeli, a następnie dołączyć wyniki do składnicy danych.
* Wprowadzanie duże lub małe aktualizacji do sprzedaży faktów.
* Dołączanie danych tylko do tabel.

Znajomość typy operacji wcześniej pomaga zoptymalizować projektowanie tabelach.

## <a name="data-migration"></a>Migracja danych

Najpierw załadować dane do [Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-store) lub magazynu obiektów Blob platformy Azure. Następnie użyj programu PolyBase, aby załadować dane do magazynu danych SQL w tabeli przemieszczania. Użyj następującej konfiguracji:

| Projektowanie | Zalecenie |
|:--- |:--- |
| Dystrybucja | Działanie okrężne |
| Indeksowanie | Sterty |
| Partycjonowanie | None |
| Klasa zasobów | largerc lub xlargerc |

Dowiedz się więcej o [migracji danych], [ładowanie danych]i [proces wyodrębniania, obciążenia i przekształcenie (ELT)](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/design-elt-data-loading). 

## <a name="distributed-or-replicated-tables"></a>Rozproszone lub zreplikowanych tabel

Użyj następujących strategii, w zależności od właściwości tabeli:

| Typ | Doskonałym rozwiązaniem dla...| Uważaj if...|
|:--- |:--- |:--- |
| Replikacja | Tabele wymiarów małych • w schemat gwiazdy z mniej niż 2 GB pamięci masowej po kompresji (kompresji ~ 5 x) |• Wiele transakcji zapisu znajdują się w tabeli (takie jak wstawianie, upsert, delete, aktualizacja)<br></br>• Zmienić udostępniania często wartości jednostki magazynu danych (DWU)<br></br>• Tylko używać kolumn 2 – 3 ale tabela zawiera wiele kolumn<br></br>• Indeksu zreplikowanej tabeli |
| Działanie okrężne (ustawienie domyślne) | • Tabeli tymczasowej/przemieszczania<br></br> • Nie oczywiste Sprzęganie kolumn klucza lub dobrej candidate |• Wydajności jest powolne z powodu przemieszczenia danych |
| Skrót | Tabele faktów •<br></br>• Wymiaru dużych tabel |Nie można zaktualizować • dystrybucji kluczy |

**Wskazówki:**
* Rozpoczynać działanie okrężne, ale Oczekuj zalet architektury równoległemu strategii dystrybucji wyznaczania wartości skrótu.
* Upewnij się, że typowe klawisze skrótów mają ten sam format danych.
* Nie dystrybucji na varchar format.
* Tabele wymiarów wspólnego klucza skrótu do tabeli faktów z operacji sprzężenia często może być rozpowszechniane skrót.
* Użyj  *[sys.dm_pdw_nodes_db_partition_stats]*  do analizowania skośność żadnych danych.
* Użyj  *[sys.dm_pdw_request_steps]*  do analizowania danych przemieszczania za zapytań, monitorowanie czasu emisji i losowa operacji. Jest to przydatne przejrzeć strategii dystrybucji.

Dowiedz się więcej o [zreplikowane tabele] i [rozproszonych tabel].

## <a name="index-your-table"></a>Indeks tabeli

Indeksowanie jest przydatne do szybkiego odczytywanie tabel. Jest unikatowy zestaw technologii, których można użyć na podstawie Twoich potrzeb:

| Typ | Doskonałym rozwiązaniem dla... | Uważaj if...|
|:--- |:--- |:--- |
| Sterty | • Tabeli przemieszczania/tymczasowej<br></br>• Małe tabele z małych wyszukiwań |• Tabeli pełna skanuje wszystkie wyszukiwania |
| Indeks klastrowany | • Tabel z maksymalnie 100 milionów wierszy<br></br>• Dużych tabel (więcej niż 100 milionów wierszy) z tylko 1 i 2 kolumny rzadko |• Używane na zreplikowanej tabeli<br></br>• Masz złożonych zapytań obejmujących wiele sprzężenia i operacje Group By<br></br>• Odpowiednie aktualizacje na indeksowanych kolumn: Trwa pamięci |
| Klastrowany indeks magazynu kolumn (WIK) (ustawienie domyślne) | • Dużych tabel (więcej niż 100 milionów wierszy) | • Używane na zreplikowanej tabeli<br></br>• Wprowadzone w bardzo dużej aktualizowanie operacji dla tabeli<br></br>• Overpartition tabeli: nie obejmują grupy wierszy na partycje i dystrybucji różnych węzłów |

**Wskazówki:**
* Na indeks klastrowany można dodać indeksu nieklastrowanego na indeks do kolumny intensywnie używane do filtrowania. 
* Należy zachować ostrożność sposób zarządzania pamięci dla tabeli z CCI. Podczas ładowania danych ma użytkownik (lub zapytanie) do korzystania z klasy dużych zasobów. Upewnij się uniknąć przycinanie i tworzenie wielu grup małych skompresowany wiersza.
* Zoptymalizowana pod kątem skały obliczeniowe warstwy z CCI.
* Dla WIK niską wydajność może się zdarzyć z powodu słabej kompresja grup wierszy. W takim przypadku odbudować lub zreorganizować WIK Twojego. Ma co najmniej 100 000 wierszy na grupy wierszy skompresowane. Idealna to 1 mln wierszy w grupy wierszy.
* Na podstawie częstotliwości przyrostowe obciążenia i rozmiaru, chcesz zautomatyzować reorganizacji lub Odbuduj indeksy użytkownika. Pomocne jest zawsze czyszczenie sprężyny.
* Być strategicznych, aby przyciąć grupę wierszy. Jak duży czy grupy wierszy Otwórz? Ilość danych będą ładowane w ciągu najbliższych dni?

Dowiedz się więcej o [indeksów].

## <a name="partitioning"></a>Partycjonowanie
Może być partycji tabeli, gdy tabela faktów duże (większe niż 1 miliard wierszy). W przypadku 99 procent klucza partycji powinny być oparte na datę. Należy zachować ostrożność nie overpartition, zwłaszcza jeśli użytkownik ma klastrowany indeks magazynu kolumn.

Z tabel, które wymagają ELT przemieszczania, mogą korzystać z partycjonowania. Ułatwia ona zarządzanie cyklem życia danych.
Nie można więc overpartition danych, szczególnie w przypadku klastrowanego indeksu magazynu kolumn.

Dowiedz się więcej o [partycji].

## <a name="incremental-load"></a>Przyrostowe obciążenia

Jw przypadku przyrostowo załadować dane, najpierw upewnij się, że Przydziel większy klasy zasobu do ładowania danych. Zaleca się przy użyciu programu PolyBase i ADF V2 do automatyzacji programu potoki ELT do usługi SQL Data Warehouse.

Dla dużych partii aktualizacji w danych historycznych należy najpierw usunąć dane danych. Następnie przełącz wstawiania zbiorczego, nowych danych. Takie podejście dwuetapowej jest bardziej wydajny.

## <a name="maintain-statistics"></a>Prowadzenie statystyk
 Dopóki auto statystyki są ogólnie dostępna, SQL Data Warehouse wymaga ręcznej obsługi statystyk. Ważne jest, aby zaktualizować statystyk jako *znaczne* wprowadzenia zmiany danych. Dzięki temu można zoptymalizować swoje plany zapytań. Jeśli okaże się, że trwa zbyt długo, aby zachować wszystkie statystyk, być kolumny, które mają statystyk. 

Można również zdefiniować częstotliwość aktualizacji. Na przykład można zaktualizować kolumn dat, gdzie nowe wartości może zostać dodana, codziennie. Aby uzyskać największe korzyści, uzyskanie statystyki do kolumn uczestniczących w sprzężenia, kolumn używana w klauzuli WHERE i kolumn w GROUP BY.

Dowiedz się więcej o [statystyki].

## <a name="resource-class"></a>Klasa zasobów
Usługa SQL Data Warehouse korzysta z grup zasobów w celu przydzielania pamięci zapytaniom. Jeśli potrzebujesz większej ilości pamięci do ulepszenia zapytania i szybkość ładowania, należy przydzielić wyższej klasy zasobów. Na stronie Przerzucanie przy użyciu klasy zasobów o większych ma wpływ współbieżności. Chcesz uwzględnić który przed przeniesieniem wszystkich użytkowników do klasy dużych zasobów.

Jeśli zauważysz, że zapytania trwa zbyt długo, sprawdź, czy użytkownicy nie są uruchamiane w dużych zasobów klasy. Klasy zasobów o dużych używać wielu miejsc współbieżności. Może spowodować inne zapytania do kolejki.

Na koniec za pomocą warstwy zoptymalizowanych pod kątem obliczeniowe, każda klasa zasobów pobiera 2,5 raza więcej pamięci niż w warstwie elastycznej zoptymalizowane.

Dowiedz się więcej, jak pracować z [klasy zasobów i współbieżność].

## <a name="lower-your-cost"></a>Niższy koszt
Kluczowych funkcji usługi SQL Data Warehouse jest możliwość [Zarządzanie zasoby obliczeniowe](sql-data-warehouse-manage-compute-overview.md). Można wstrzymać hurtowni danych, gdy nie używasz, co uniemożliwia rozliczenia zasoby obliczeniowe. Możesz skalować zasobów do Twojej wymagań dotyczących wydajności. Aby wstrzymać, użyj [portalu Azure](pause-and-resume-compute-portal.md) lub [PowerShell](pause-and-resume-compute-powershell.md). Aby skalować, użyj [portalu Azure](quickstart-scale-compute-portal.md), [Powershell](quickstart-scale-compute-powershell.md), [T-SQL](quickstart-scale-compute-tsql.md), lub [interfejsu API REST](sql-data-warehouse-manage-compute-rest-api.md#scale-compute).

Funkcja automatycznego skalowania teraz w czasie ma w środowisku Azure Functions:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

## <a name="optimize-your-architecture-for-performance"></a>Optymalizacja wydajności architektury

Firma Microsoft zaleca, biorąc bazy danych SQL i usług Azure Analysis Services w architekturze gwiazdy. To rozwiązanie może rozdzielenia obciążenia różnych grup użytkowników podczas również przy użyciu zaawansowanych funkcji zabezpieczeń z bazy danych SQL i usług Azure Analysis Services. To jest również sposób zapewnia współbieżność nieograniczona dla użytkowników.

Dowiedz się więcej o [typowe architektury, które korzystają z usługi SQL Data Warehouse](https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/).

Wdrażanie za pomocą jednego kliknięcia z wymienionymi w baz danych z magazynu danych SQL:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>


<!--Image references-->
[Sketch]:media/sql-data-warehouse-cheat-sheet/picture-flow.png

<!--Article references-->
[ładowanie danych]:./design-elt-data-loading.md
[deeper guidance]: ./guidance-for-loading-data.md
[indeksów]:./sql-data-warehouse-tables-index.md
[partycji]:./sql-data-warehouse-tables-partition.md
[statystyki]:./sql-data-warehouse-tables-statistics.md
[klasy zasobów i współbieżność]:./sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[typical architectures that take advantage of SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/
[is and is not]:https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns/
[migracji danych]:https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
[zreplikowane tabele]:https://docs.microsoft.com/en-us/azure/sql-data-warehouse/design-guidance-for-replicated-tables
[rozproszonych tabel]:https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute
[Azure Data Lake Store]: https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-store
[sys.dm_pdw_nodes_db_partition_stats]: https://docs.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql
[sys.dm_pdw_request_steps]:https://docs.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql
