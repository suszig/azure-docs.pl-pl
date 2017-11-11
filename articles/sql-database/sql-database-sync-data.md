---
title: Synchronizacja danych Azure SQL (wersja zapoznawcza) | Dokumentacja firmy Microsoft
description: "W tym omówieniu przedstawiono synchronizacji danych SQL Azure (wersja zapoznawcza)"
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: douglasl
ms.reviewer: douglasl
ms.openlocfilehash: fe11926cb7f6b2a80913895b685acfcc433e9805
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync-preview"></a>Synchronizowanie danych w wielu w chmurze i lokalnych baz danych z opcją synchronizacji danych SQL (wersja zapoznawcza)

Synchronizacja danych SQL to usługa oparta na bazie danych SQL Azure, która umożliwia synchronizowanie danych, wybranych dwukierunkowo przez wiele baz danych i wystąpień programu SQL Server.

Synchronizacja danych opiera się wokół pojęcia grupy synchronizacji. Grupy synchronizacji jest grupą baz danych, które mają być synchronizowane.

Grupy synchronizacji ma następujące właściwości:

-   **Schematu synchronizacji** w tym artykule opisano, które dane są synchronizowane.

-   **Kierunek synchronizacji** może być dwukierunkowe lub mogą przepływać tylko w jednym kierunku. Oznacza to, może być kierunek synchronizacji *koncentratora do elementu członkowskiego* lub *elementu członkowskiego do Centrum*, lub obie.

-   **Interwał synchronizacji** jest częstotliwość synchronizacji.

-   **Zasady rozpoznawania konfliktu** poziomu zasad grupy, które mogą być *wins Centrum* lub *wins elementu członkowskiego*.

Synchronizacja danych używa topologii gwiazdy do synchronizacji danych. Należy określić jednej bazy danych w grupie co baza danych Centrum. Pozostała część baz danych to element członkowski bazy danych. Synchronizacja występuje tylko między Centrum i indywidualnych elementów członkowskich.
-   **Bazy danych Centrum** musi być bazy danych SQL Azure.
-   **Baz danych elementu członkowskiego** może być bazy danych SQL, lokalnych baz danych programu SQL Server lub wystąpień programu SQL Server na maszynach wirtualnych Azure.
-   **Bazy danych usługi synchronizacji** zawiera metadanych i dziennika dla synchronizacji danych. Bazy danych usługi synchronizacji musi być bazy danych SQL Azure znajduje się w tym samym regionie co baza danych Centrum. Bazy danych usługi synchronizacji jest utworzyć klienta i należące do klientów.

> [!NOTE]
> Jeśli używasz bazy danych na lokalnym, konieczne będzie [Konfigurowanie lokalnego agenta](sql-database-get-started-sql-data-sync.md#add-on-prem).

![Synchronizowanie danych między bazami danych](media/sql-database-sync-data/sync-data-overview.png)

## <a name="when-to-use-data-sync"></a>Kiedy należy używać synchronizacji danych

Synchronizacja danych jest przydatne w sytuacjach, w którym dane muszą być na bieżąco przez wiele baz danych SQL Azure lub baz danych programu SQL Server. Poniżej przedstawiono główne zastosowania synchronizacji danych:

-   **Synchronizacja danych hybrydowego:** z opcją synchronizacji danych można przechowywać dane synchronizowane między lokalnych baz danych i baz danych SQL Azure na potrzeby aplikacji hybrydowych. Ta funkcja może odwołać się do klientów, którzy rozważa przenoszeniu do chmury, a chcesz umieścić niektórych aplikacji na platformie Azure.

-   **Aplikacje rozproszone:** w wielu przypadkach jest przydatne do oddzielania różnych obciążeń w różnych baz danych. Na przykład jeśli masz dużą produkcyjnej bazy danych, ale należy również do uruchamiania obciążeń raportowania lub analityka na tych danych, warto mieć drugi bazy danych dla tego dodatkowe obciążenie. Takie podejście minimalizuje wpływ na wydajność na obciążenie produkcji. Synchronizacji danych można użyć, aby zachować te dwie bazy danych synchronizacji.

-   **Globalny aplikacji rozproszonych:** wiele firm span kilka regionów i nawet kilka krajów. Aby zminimalizować opóźnienie sieci, jest najlepszym rozwiązaniem jest dane w regionie blisko Ciebie. Z opcją synchronizacji danych można pracować z baz danych w regionach na świecie zsynchronizowane.

Synchronizacja danych nie jest odpowiedni dla następujących scenariuszy:

-   Odzyskiwanie po awarii

-   Przeczytaj skali

-   ETL (OLTP do OLAP)

-   Migracja z lokalnego serwera SQL z bazą danych Azure SQL

## <a name="how-does-data-sync-work"></a>Jak działa synchronizacji danych? 

-   **Śledzenie zmian danych:** synchronizacji danych śledzi zmiany przy użyciu wstawiania, aktualizacji i usuwania wyzwalaczy. Zmiany są zapisywane w tabeli po stronie w bazie danych użytkownika.

-   **Synchronizowanie danych:** synchronizacji danych zaprojektowano w model gwiazdy. Koncentrator przeprowadza synchronizację z każdym elemencie członkowskim pojedynczo. Zmiany w Centrum są pobierane do elementu członkowskiego, a następnie zmiany z elementu członkowskiego są przekazywane do koncentratora.

-   **Rozwiązywanie konfliktów:** synchronizacji danych są dostępne dwie opcje do rozwiązywania konfliktów *wins Centrum* lub *wins elementu członkowskiego*.
    -   W przypadku wybrania *wins Centrum*, zmiany w Centrum zawsze zastąpienie zmian w elemencie członkowskim.
    -   W przypadku wybrania *wins elementu członkowskiego*, zmiany w zmian Zastąp elementów członkowskich w Centrum. Jeśli istnieje więcej niż jeden element członkowski, końcowa wartość zależy który element członkowski jest najpierw zsynchronizowane.

## <a name="common-questions"></a>Często zadawane pytania

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Częstotliwość synchronizacji danych można synchronizować dane? 
Minimalna częstotliwość wynosi co pięć minut.

### <a name="can-i-use-data-sync-to-sync-between-sql-server-on-premises-databases-only"></a>Synchronizacja danych można używać do synchronizacji między lokalnymi baz danych serwera SQL tylko? 
Nie bezpośrednio. Można zsynchronizować między bazami danych programu SQL Server lokalne pośrednio, jednak utworzenie Centrum bazy danych na platformie Azure, a następnie dodając lokalnych baz danych do grupy synchronizacji.
   
### <a name="can-i-use-data-sync-to-seed-data-from-my-production-database-to-an-empty-database-and-then-keep-them-synchronized"></a>Można używać synchronizacji danych do inicjatora danych z bazy danych produkcyjnej do pustej bazy danych i następnie były synchronizowane? 
Tak. Tworzenie schematu ręcznie w nowej bazy danych za pomocą skryptu go od oryginału. Po utworzeniu schemat, dodać tabele do grupy synchronizacji, skopiuj dane i zachować zsynchronizowane.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Dlaczego widzę tabel, które I nie może utworzyć?  
Synchronizacja danych tworzy tabele w bazie danych śledzenia zmian. Nie należy usuwać je lub synchronizacji danych przestanie działać.
   
### <a name="i-got-an-error-message-that-said-cannot-insert-the-value-null-into-the-column-column-column-does-not-allow-nulls-what-does-this-mean-and-how-can-i-fix-the-error"></a>Otrzymano komunikat o błędzie, który powiedział "nie można wstawić wartości NULL do kolumny \<kolumny\>. Kolumna nie dopuszcza wartości null." Co to znaczy i jak można naprawić ten błąd? 
Ten komunikat o błędzie wskazuje jeden z dwóch następujących problemów:
1.  Może istnieć bez klucza podstawowego tabeli. Aby rozwiązać ten problem, należy dodać do wszystkich tabel, które są synchronizowanie klucza podstawowego.
2.  Może być klauzuli WHERE w instrukcji CREATE INDEX. Synchronizacja nie obsługuje tego warunku. Aby rozwiązać ten problem, Usuń klauzuli WHERE lub ręcznie wprowadzić zmiany do wszystkich baz danych. 
 
### <a name="how-does-data-sync-handle-circular-references-that-is-when-the-same-data-is-synced-in-multiple-sync-groups-and-keeps-changing-as-a-result"></a>Jak synchronizacja danych obsługuje odwołania cykliczne? Oznacza to, gdy te same dane jest zsynchronizowany w wielu grupach synchronizacji i śledzi zmiany w związku z tym?
Synchronizacja danych nie obsługuje odwołań cyklicznych. Pamiętaj uniknąć ich. 

### <a name="how-can-i-export-and-import-a-database-with-data-sync"></a>Jak wyeksportować i zaimportować bazę danych z opcją synchronizacji danych?
Po wyeksportowaniu bazy danych jako `.bacpac` pliku i zaimportować plik, aby utworzyć nową bazę danych, należy wykonać następujące czynności dwa na synchronizację danych do nowej bazy danych:
1.  Czyszczenie obiektów synchronizacji danych i tabel na **nową bazę danych** za pomocą [ten skrypt](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/sql-data-sync/clean_up_data_sync_objects.sql). Ten skrypt powoduje usunięcie wszystkich wymaganych obiektów synchronizacji danych z bazy danych.
2.  Utwórz ponownie grupę synchronizacji z nową bazę danych. Jeśli stary grupy synchronizacji nie jest już potrzebny, usuń go.

## <a name="sync-req-lim"></a>Wymagania i ograniczenia

### <a name="general-requirements"></a>Wymagania ogólne

-   Każda tabela musi mieć klucz podstawowy. Nie zmieniaj wartości klucza podstawowego w dowolnym wierszu. Jeśli masz w tym celu Usuń wiersz i utwórz ją ponownie przy użyciu nowej wartości klucza podstawowego. 

-   Tabela nie może mieć kolumnę tożsamości, która nie jest kluczem podstawowym.

-   Nazwy obiektów (baz danych, tabel i kolumn) nie może zawierać znaków drukowalnych kropki (.), lewego nawiasu kwadratowego ([) lub prawo kwadratowa nawiasu (]).

-   Musi być włączona izolacji migawki. Aby uzyskać więcej informacji, zobacz [izolację migawki w programie SQL Server](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/snapshot-isolation-in-sql-server).

### <a name="general-considerations"></a>Zagadnienia ogólne

#### <a name="eventual-consistency"></a>Spójność ostateczna
Ponieważ synchronizacji danych na podstawie wyzwalacza, nie gwarantuje spójności transakcyjnej. Microsoft gwarantuje wprowadzone po pewnym czasie wszystkie zmiany i synchronizacji danych nie powoduje utraty danych.

#### <a name="performance-impact"></a>Wpływ na wydajność
Używa synchronizacji danych wstawiania, aktualizowania i usuwania wyzwalaczy do śledzenia zmian. Tworzy tabele w bazie danych użytkownika, śledzenie zmian. Te działania śledzenia zmiany mają wpływ na obciążenie bazy danych. Ocenia warstwę usługi i uaktualnienia, jeśli to konieczne.

### <a name="general-limitations"></a>Ogólne ograniczenia

#### <a name="unsupported-data-types"></a>Nieobsługiwane typy danych

-   FileStream

-   UDT SQL/CLR

-   Kolekcji XMLSchemaCollection (XML obsługiwane)

-   Kursor, Timestamp, Hierarchyid

#### <a name="limitations-on-service-and-database-dimensions"></a>Ograniczenia dotyczące wymiarów usługi i bazy danych

| **Wymiary**                                                      | **Limit**              | **Obejście problemu**              |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| Maksymalna liczba grup synchronizacji wszystkie bazy danych może należeć do.       | 5                      |                             |
| Maksymalna liczba punktów końcowych w grupie jednej synchronizacji              | 30                     | Utwórz wiele grup synchronizacji |
| Maksymalna liczba punktów końcowych lokalnie w jednej synchronizacji grupy. | 5                      | Utwórz wiele grup synchronizacji |
| Nazwy bazy danych, tabel, schematów i kolumn                       | 50 znaków według nazwy |                             |
| Tabele w grupie synchronizacji                                          | 500                    | Utwórz wiele grup synchronizacji |
| Kolumn w tabeli w grupie synchronizacji                              | 1000                   |                             |
| Rozmiar wiersza danych w tabeli                                        | 24 mb                  |                             |
| Interwał synchronizacji minimalna                                           | 5 minut              |                             |
|||

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat synchronizacji danych SQL zobacz:

-   [Konfigurowanie synchronizacji danych SQL Azure](sql-database-get-started-sql-data-sync.md)
-   [Najlepsze rozwiązania dotyczące synchronizacji danych SQL Azure](sql-database-best-practices-data-sync.md)
-   [Monitor synchronizacji danych Azure SQL z OMS analizy dzienników](sql-database-sync-monitor-oms.md)
-   [Rozwiązywanie problemów z synchronizacją danych SQL Azure](sql-database-troubleshoot-data-sync.md)

-   Wykonaj przykłady z programu PowerShell, które przedstawiają sposób konfigurowania synchronizacji danych SQL:
    -   [Synchronizacja między wiele baz danych Azure SQL przy użyciu programu PowerShell](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Synchronizacja między bazą danych SQL Azure i lokalnej bazy danych programu SQL Server przy użyciu programu PowerShell](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Pobrać dokumentację interfejsu API REST synchronizacji danych SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Aby uzyskać więcej informacji na temat bazy danych SQL zobacz:

-   [Omówienie bazy danych SQL](sql-database-technical-overview.md)
-   [Zarządzanie cyklem życia bazy danych](https://msdn.microsoft.com/library/jj907294.aspx)
