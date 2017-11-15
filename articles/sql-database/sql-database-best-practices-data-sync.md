---
title: "Najlepsze rozwiązania dotyczące synchronizacji danych Azure SQL | Dokumentacja firmy Microsoft"
description: "Dowiedz się, najważniejsze wskazówki dotyczące konfigurowania i uruchamiania synchronizacji danych SQL Azure"
services: sql-database
ms.date: 11/2/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: d23bd186300d451186dd4f3644290cb4178417a2
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="best-practices-for-sql-data-sync-preview"></a>Najlepsze rozwiązania dotyczące synchronizacji danych SQL (wersja zapoznawcza) 

W tym artykule opisano najlepsze rozwiązania dla synchronizacji danych SQL (wersja zapoznawcza).

Omówienie synchronizacji danych SQL, zobacz [synchronizacji danych między wieloma bazami danych chmury i lokalnych z synchronizacji danych SQL Azure (wersja zapoznawcza)](sql-database-sync-data.md).

## <a name="security-and-reliability"></a>Bezpieczeństwo i niezawodność

### <a name="client-agent"></a>Agent klienta

-   Zainstaluj agenta klienta przy użyciu co najmniej uprawnień konta z dostępem do usługi sieci.

-   Najlepiej na osobnym komputerze z komputera lokalnego programu SQL Server zainstalowany jest agent klienta.

-   Nie rejestruj lokalną bazą danych z więcej niż jednego agenta.

-   Nawet jeśli synchronizacja z różnych tabel dla różnych synchronizacji grupy.

-   Rejestrowanie lokalną bazą danych z wielu wyzwanie stanowi agentów klienta podczas usuwania jednej z grup synchronizacji.

### <a name="database-accounts-with-least-required-privilege"></a>Konto bazy danych o najniższych uprawnień wymaganych

-   **Synchronizacja ustawień**. Utwórz/Alter Table, zmiany bazy danych, Utwórz procedurę, wybierz / Alter Schema, Utwórz typ zdefiniowany przez użytkownika.

-   **Trwającej synchronizacji**. Zaznacz / wstawiania / aktualizacji / usuwania w tabelach wybrane do synchronizacji i synchronizacji metadanych i tabele śledzenia, uprawnienia do wykonywania na procedury składowane utworzone przez usługę uprawnienia Execute w typach tabel zdefiniowanych przez użytkownika.

-   **Cofnąć alokacji**. Zmiany w tabelach części synchronizacji, wybierz / Delete na tabelach Synchronizuj metadane, formantu synchronizacji śledzenia śledzenia synchronizacji tabele, procedury składowane i typy danych zdefiniowane przez użytkownika.

**Jak można użyć tych informacji po tylko jednego poświadczenia dla bazy danych w grupie synchronizacji?**

-   Zmiana poświadczeń dla różnych etapów (na przykład credential1 Konfiguracja i credential2 dla trwających).

-   Zmiana uprawnień poświadczenia (to znaczy, Zmień uprawnienia po skonfigurowaniu synchronizacji).

## <a name="locate-hub"></a>Gdzie można znaleźć bazy danych Centrum

### <a name="enterprise-to-cloud-scenario"></a>Scenariusz przedsiębiorstwa do chmury

Aby zminimalizować czas oczekiwania, pozostawienia bazy danych Centrum bliski największy koncentracja ruchu bazy danych grupy synchronizacji.

### <a name="cloud-to-cloud-scenario"></a>Scenariusz chmury do chmury

-   Gdy wszystkie bazy danych w grupie synchronizacji znajdują się w centrum danych jednego, koncentratora powinien znajdować się w tym samym centrum danych. Ta konfiguracja zmniejsza opóźnienia i koszty transferu danych między centrami danych.

-   W przypadku baz danych w grupie synchronizacji w wielu centrach danych, koncentratora powinien znajdować się w tym samym centrum danych, ponieważ większość baz danych i ruchu bazy danych.

### <a name="mixed-scenarios"></a>Scenariusze mieszanych

Zastosowanie powyższych wytyczne do bardziej złożonych konfiguracji grupy synchronizacji.

## <a name="database-considerations-and-constraints"></a>Zagadnienia dotyczące bazy danych i ograniczenia

### <a name="sql-database-instance-size"></a>Rozmiar wystąpienia bazy danych SQL

Podczas tworzenia nowego wystąpienia bazy danych SQL, Ustaw maksymalny rozmiar, aby zawsze była większa niż baza danych, którą można wdrożyć. Jeśli nie ustawisz maksymalny rozmiar większy niż wdrożonej bazy danych, synchronizacja nie powiedzie się. Gdy nie istnieje żadne automatyczne zwiększanie rozmiaru — można wykonać instrukcji ALTER DATABASE, aby zwiększyć rozmiar bazy danych, po jego utworzeniu. Upewnij się, że pozostać w limity rozmiaru wystąpienia bazy danych SQL.

> [!IMPORTANT]
> Synchronizacja danych SQL przechowuje dodatkowe metadane z każdej bazy danych. Pamiętaj uwzględnić te metadane podczas obliczania potrzebne miejsce. Ilość dodane koszty podlega szerokość tabele (na przykład wąskie tabele wymagają większe obciążenie) i ilość ruchu sieciowego.

## <a name="table-considerations-and-constraints"></a>Zagadnienia dotyczące tabeli i ograniczenia

### <a name="selecting-tables"></a>Wybieranie tabel

Nie wszystkie tabele w bazie danych muszą być w [grupy synchronizacji](#sync-group). Wybór tabel do grupy synchronizacji i do wykluczenia (lub innej grupy synchronizacji) może wpłynąć na wydajność i koszty. Uwzględnij tylko te tabele w grupy synchronizacji potrzeb biznesowych żądanie i tabele, na których są zależne.

### <a name="primary-keys"></a>Klucze podstawowe

Każda tabela w grupie synchronizacji musi mieć klucz podstawowy. Usługa synchronizacji danych SQL (wersja zapoznawcza) nie może zsynchronizować wszystkie tabele, których nie ma klucza podstawowego.

Przed udostępnieniem w środowisku produkcyjnym, należy przetestować wydajność wstępnych i bieżących synchronizacji.

## <a name="provisioning-destination-databases"></a>Inicjowanie obsługi administracyjnej docelowej bazy danych

Podgląd synchronizacji danych SQL (wersja zapoznawcza) zawiera bazy danych podstawowa automatycznego inicjowania obsługi.

W tej sekcji omówiono ograniczenia synchronizacji danych SQL (wersja zapoznawcza) do inicjowania obsługi administracyjnej.

### <a name="auto-provisioning-limitations"></a>Automatyczne inicjowanie obsługi administracyjnej ograniczenia

Poniżej wymieniono ograniczenia obsługi automatycznej synchronizacji danych SQL (wersja zapoznawcza).

-   Tylko wybrane kolumny są tworzone w tabeli docelowej.
W związku z tym jeśli niektóre kolumny nie są częścią grupy synchronizacji te kolumny nie są udostępnione w tabeli docelowej.

-   Indeksy są tworzone tylko dla wybranych kolumn.
Indeks tabeli źródłowej ma kolumn, które nie są częścią grupy synchronizacji te indeksy nie są udostępnione w tabeli docelowej.

-   Indeksy w kolumnach typu XML nie są udostępnione.

-   Sprawdź, czy ograniczenia nie są udostępnione.

-   Istniejące wyzwalacze w tabelach źródła nie są udostępnione.

-   Widoki i procedury składowane nie są tworzone w docelowej bazie danych.

### <a name="recommendations"></a>Zalecenia

-   Możliwość automatycznego inicjowania obsługi administracyjnej należy używać tylko w przypadku próby usługi.

-   W środowisku produkcyjnym należy udostępnić schemat bazy danych.

## <a name="avoid-a-slow-and-costly-initial-synchronization"></a>Unikaj powolne i kosztowne synchronizacji początkowej

W tej sekcji omówiono synchronizacji początkowej synchronizacji grupy i co można zrobić, aby uniknąć wstępnej synchronizacji trwa dłużej niż niezbędne i kosztów więcej niż powinien.

### <a name="how-initial-synchronization-works"></a>Działa jak początkowa synchronizacja

Podczas tworzenia grupy synchronizacji, należy uruchomić z danymi w bazie danych tylko jeden. Jeśli masz dane w wielu baz danych synchronizacji danych SQL (wersja zapoznawcza) traktuje każdy wiersz jako wymagającym rozwiązania konfliktu. To rozwiązanie konfliktu powoduje, że wstępnej synchronizacji przejść powoli, biorąc kilka dni do kilku miesięcy, w zależności od rozmiaru bazy danych.

Ponadto w przypadku baz danych w różnych centrach danych, kosztów wstępnej synchronizacji są wyższe niż to konieczne, ponieważ każdy wiersz musi przejść między różnych centrach danych.

### <a name="recommendation"></a>Zalecenie

Zawsze, gdy to możliwe należy rozpocząć od danych tylko jednej grupy synchronizacji baz danych.

## <a name="design-to-avoid-synchronization-loops"></a>Projekt, aby uniknąć tworzenia pętli synchronizacji

Pętla synchronizacji powoduje, gdy istnieją odwołania cykliczne w obrębie grupy synchronizacji tak, aby każdej zmiany w jednej bazy danych jest replikowana przy użyciu baz danych w grupie synchronizacji rekurencyjnie i nieskończoność. Użytkownik chce uniknąć tworzenia pętli synchronizacji, ponieważ zmniejszyć wydajność i może znacznie zwiększyć koszty.

## <a name="avoid-out-of-date-databases-and-sync-groups"></a>Unikaj nieaktualne baz danych i synchronizacji grupy

Grupy synchronizacji lub bazy danych w grupie synchronizacji może stać się nieaktualne. Gdy stan grupy synchronizacji to "przestarzałe", przestanie działać. Gdy stan bazy danych to "przestarzałe", dane mogą zostać utracone. Zaleca się uniknąć tych sytuacji zamiast trzeba odzyskać ich.

### <a name="avoid-out-of-date-databases"></a>Unikaj nieaktualne baz danych

Stan bazy danych ustawiono nieaktualne podczas była w trybie offline przez co najmniej 45 dni. Unikaj nieaktualne stan w bazie danych, zapewniając, że żaden z bazy danych nie jest w trybie offline przez 45 dni lub więcej.

### <a name="avoid-out-of-date-sync-groups"></a>Unikaj grupy nieaktualne synchronizacji

Stan grupy synchronizacji ma ustawioną nieaktualne gdy zmiany w grupie synchronizacji nie powiodło się obejmie pozostałe grupy synchronizacji 45 dni lub więcej. Uniknąć nieaktualne stanu grupy synchronizacji, sprawdzając regularnie grupy synchronizacji historii dziennika. Upewnij się, że wszystkie konflikty zostały rozwiązane i zmiany pomyślnie propagowane w całej bazy danych grupy synchronizacji.

Grupy synchronizacji mogą się niepowodzeniem zastosować zmiany przyczyny:

-   Niezgodność schematu między tabelami.

-   Niezgodność danych między tabelami.

-   Wstawienie wiersza z wartością null w kolumnie, która nie zezwala na wartości null.

-   Aktualizowanie wiersz z wartością, która narusza ograniczenie klucza obcego.

Można zapobiec nieaktualne synchronizacji grupy według:

-   Aktualizacja schematu pozwala na określenie wartości zawartych w wierszach nie powiodło się.

-   Zaktualizuj wartości klucza obcego do uwzględnienia wartości zawarte w wierszach nie powiodło się.

-   Zaktualizuj wartości danych w wierszu nie powiodło się, aby był zgodny ze schematem lub klucze obce w docelowej bazie danych.

## <a name="avoid-deprovisioning-issues"></a>Unikaj anulowania obsługi problemów

W pewnych okolicznościach wyrejestrowywania bazy danych przy użyciu agenta klienta może spowodować synchronizacje się niepowodzeniem.

### <a name="scenario"></a>Scenariusz

1. Synchronizacji grupy A utworzono wystąpienie bazy danych SQL i lokalnej bazy danych SQL Server, który jest skojarzony z lokalnego agenta 1.

2. Tę samą bazę danych lokalnych jest zarejestrowany agent lokalny 2 (tego agenta nie jest skojarzony z żadną inną grupą synchronizacji).

3. Wyrejestrowywanie lokalną bazą danych z lokalnego agenta 2 Usuwa tabele śledzenia/meta synchronizacji grupy A lokalnej bazy danych.

4. Teraz operacje A grupy synchronizacji kończą się niepowodzeniem z powodu następującego błędu — "bieżącej operacji nie można ukończyć ponieważ bazy danych nie jest przeznaczona do synchronizacji lub nie masz uprawnień do tabel konfiguracji synchronizacji."

### <a name="solution"></a>Rozwiązanie

Unikaj sytuacji całkowicie rejestrując nigdy nie bazy danych z więcej niż jednego agenta.

Aby odzyskać z tej sytuacji:

1. Usunąć bazę danych z każdej grupy synchronizacji, do której należy.

2. Bazy danych z powrotem dodać do każdej grupy synchronizacji, z poziomu tylko usunięte.

3. Wdrażanie grupach dotyczy synchronizacji (który obsługuje bazy danych).

## <a name="handling-changes-that-fail-to-propagate"></a>Obsługa zmiany, których nie można propagować

### <a name="reasons-that-changes-fail-to-propagate"></a>Przyczyn, które zmiany nie można propagować

Zmiany mogą nie być obejmie wiele przyczyn. Niektóre przyczyny mogą być następujące:

-   Niezgodność schematu/typu danych.

-   Próba wstawienia wartości null w kolumnach wartości null.

-   Naruszenie ograniczenia klucza obcego.

### <a name="what-happens-when-changes-fail-to-propagate"></a>Co się stanie w przypadku zmiany nie można propagować?

-   Grupy synchronizacji pokazuje, że jest on w stanie ostrzeżenia.

-   Szczegółowe informacje są w przeglądarce dzienników interfejsu użytkownika portalu.

-   Jeśli problem nie zostanie rozwiązany w ciągu 45 dni, bazy danych stanie się nieaktualna.

> [!NOTE]
> Te zmiany nie są propagowane. Jedynym sposobem odzyskania jest aby odtworzyć grupę synchronizacji.

### <a name="recommendation"></a>Zalecenie

Monitorowanie kondycji grupy synchronizacji i bazy danych regularnie za pomocą interfejsu portalu i dziennika.

## <a name="modifying-your-sync-group"></a>Modyfikowanie grupy synchronizacji

Nie należy próbować usunąć bazę danych z grupy synchronizacji, a następnie edytuj grupy synchronizacji bez pierwszego wdrażania zmian.

Najpierw należy usunąć bazę danych z grupy synchronizacji. Następnie Wdróż zmiany i poczekaj, aż do Zakończenie inicjowania obsługi. Po zakończeniu tej operacji może edytować grupę synchronizacji i wdrażanie zmiany.

Próba usunięcia bazy danych, a następnie Edytuj grupę synchronizacji bez wdrażania pierwszego zmiany, co lub inna operacja zakończy się niepowodzeniem, a portalu interfejsu mogą trafić w niespójnym stanie. W takim przypadku można odświeżyć stronę, aby przywrócić stan.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat synchronizacji danych SQL zobacz:

-   [Synchronizowanie danych w wielu w chmurze i lokalnych baz danych z synchronizacji danych SQL Azure](sql-database-sync-data.md)
-   [Konfigurowanie synchronizacji danych SQL Azure](sql-database-get-started-sql-data-sync.md)
-   [Monitor synchronizacji danych Azure SQL z OMS analizy dzienników](sql-database-sync-monitor-oms.md)
-   [Rozwiązywanie problemów z synchronizacją danych SQL Azure](sql-database-troubleshoot-data-sync.md)

-   Wykonaj przykłady z programu PowerShell, które przedstawiają sposób konfigurowania synchronizacji danych SQL:
    -   [Synchronizacja między wiele baz danych Azure SQL przy użyciu programu PowerShell](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Synchronizacja między bazą danych SQL Azure i lokalnej bazy danych programu SQL Server przy użyciu programu PowerShell](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Pobrać dokumentację interfejsu API REST synchronizacji danych SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Aby uzyskać więcej informacji na temat bazy danych SQL zobacz:

-   [Omówienie bazy danych SQL](sql-database-technical-overview.md)
-   [Zarządzanie cyklem życia bazy danych](https://msdn.microsoft.com/library/jj907294.aspx)
