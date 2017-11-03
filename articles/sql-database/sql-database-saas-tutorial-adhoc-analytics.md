---
title: "Uruchamianie analitycznych zapytań ad-hoc w wielu bazach danych Azure SQL | Microsoft Docs"
description: "Uruchom ad hoc zapytania analityczne przez wiele baz danych w przykładzie wielodostępnych aplikacji."
keywords: "samouczek usługi sql database"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: billgib; sstein
ms.openlocfilehash: 849f0570fb1550f6c3676fc070d0f862450ade9a
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="run-ad-hoc-analytics-queries-across-multiple-azure-sql-databases"></a>Uruchamianie zapytań analytics ad hoc przez wiele baz danych Azure SQL

W tym samouczku zapytania rozproszone spotkać cały zestaw wielodostępnej baz danych, aby włączyć analytics ad hoc. Elastyczne zapytanie służy do włączania zapytań rozproszonych, co wymaga dalszej analizy bazy danych jest wdrażana (na serwerze wykazu). Te zapytania można wyodrębnić wgląd w codziennych danych operacyjnych aplikacji Wingtip SaaS stosie.


Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Widoki globalne w każdej bazie danych, informacje te widoki włączenia, wydajne zapytań między dzierżawcami
> * Jak wdrożyć bazy danych analizy ad hoc
> * Jak uruchamiać zapytania rozproszone dla wszystkich baz danych dzierżawy



Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Aplikacja Wingtip SaaS jest wdrażana. Aby wdrożyć w mniej niż 5 minut, zobacz [wdrażania i aplikacji Wingtip SaaS](sql-database-saas-tutorial.md)
* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio (SSMS) jest zainstalowany. Aby pobrać i zainstalować narzędzia SSMS, zobacz [pobierania programu SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="ad-hoc-analytics-pattern"></a>Wzorzec analytics ad hoc

Jedną z możliwości dużą z aplikacjami SaaS jest użycie dużych ilości danych dzierżawy przechowywane w chmurze. Aby uzyskać wgląd w działanie i użycia aplikacji, należy użyć tych danych. Te szczegółowe informacje można przeprowadzają opracowanie funkcji, ulepszenia użyteczność i innych inwestycji w aplikacji i usług.

Uzyskiwanie dostępu do tych danych w jednej wielodostępnej bazie danych jest łatwe, ale nie jest tak proste, gdy są one znacznie rozproszone, potencjalnie nawet na tysiące baz danych. Jednym z podejść jest użycie [elastycznej zapytania](sql-database-elastic-query-overview.md), które umożliwia wykonywanie zapytań w rozproszonej zestaw baz danych z wspólny schemat. Elastyczne zapytanie używa pojedynczego *head* bazy danych, w którym zdefiniowano tabel zewnętrznych który duplikatów tabel lub widoków w bazach danych rozproszonych (dzierżawcy). Zapytania skierowane do tej głównej bazy danych są kompilowane w celu utworzenia planu zapytania rozproszonego, gdzie części zapytania są w razie potrzeby wypychane do baz danych dzierżawy. Elastyczne zapytanie używa podzielonej na fragmenty mapy w bazie danych wykazu, aby udostępnić lokalizację baz danych dzierżawy. Instalator i zapytania są oczywiste, przy użyciu standardu [języka Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)i obsługuje zapytań ad hoc z narzędzi, takich jak usługi Power BI i Excel.

Przekazując kwerendy bazy danych dzierżawy, elastycznej zapytania zapewnia błyskawiczny wgląd w danych produkcyjnych na żywo. Jednak jak elastycznej zapytanie pobiera dane z potencjalnie wiele baz danych, opóźnienie kwerendy można czasami być wyższy niż równoważne zapytań przesłane do pojedynczej bazy danych wielu dzierżawców. Pamiętaj, aby projektowania zapytań zminimalizowanie ilości danych, która jest zwracana. Elastyczne zapytania jest często najlepiej dopasowane do badania niewielkich ilości danych w czasie rzeczywistym, w przeciwieństwie do budynku często używane lub analityka złożonych kwerend lub raportów. Jeśli kwerendy nie wykonać dobrze, obejrzyj [plan wykonania](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) jaka część zapytania jest przesuwana, do zdalnej bazy danych i ilość danych, które zostały zwrócone. Zapytania, które wymagają złożonych przetwarzania analitycznego mogą być lepiej obsłużone w niektórych przypadkach wyodrębniając danych dzierżawy na dedykowany bazy danych lub magazyn danych, zoptymalizowana pod kątem zapytania analityczne. Ten wzorzec zostało wyjaśnione w dokumencie [samouczek analizy dzierżawy](sql-database-saas-tutorial-tenant-analytics.md). 

## <a name="get-the-wingtip-application-scripts"></a>Pobieranie skryptów aplikacji Wingtip

Wingtip SaaS skrypty i kod źródłowy aplikacji są dostępne w [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) repozytorium github. [Kroki, aby pobrać skrypty Wingtip SaaS](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="create-ticket-sales-data"></a>Tworzenie danych sprzedaży biletów

Do wykonywania kwerend do bardziej interesującego zestawu danych, należy utworzyć danych sprzedaży biletów, uruchamiając generatora biletu.

1. W *PowerShell ISE*, Otwórz... \\Modułów uczenia\\operacyjne Analytics\\Analytics ad hoc\\*AdhocAnalytics.ps1 pokaz* skryptów i ustaw następujące wartości:
   * **$DemoScenario** = 1, **zakupu biletów zdarzeń na wszystkich miejsc**.
2. Naciśnij klawisz **F5** do uruchomienia skryptu i generowania sprzedaży biletów. Po uruchomieniu skryptu nadal kroki opisane w tym samouczku. Dane biletu jest poddawany kwerendzie w *uruchamianie zapytań ad hoc distributed* sekcji, więc Zaczekaj generatora biletu, aby zakończyć.

## <a name="explore-the-global-views"></a>Eksploruj widoki globalne

Aplikacja Wingtip SaaS jest utworzony przy użyciu modelu dzierżawy na bazę danych, więc dzierżawcy schematu bazy danych jest zdefiniowana z punktu widzenia pojedynczej dzierżawy. Informacje dotyczące dzierżawy istnieje w jednej tabeli, *miejscową*, który zawsze pojedynczy wiersz i jest zaimplementowany jako sterty, bez klucza podstawowego. Inne tabele w schemacie nie ma potrzeby związane z *miejscową* tabeli, ponieważ podczas normalnego użytkowania, nigdy nie jest wątpliwości, które dzierżawy danych należy.

Jednak podczas wykonywania zapytania dla wszystkich baz danych, ważne jest elastyczny zapytania można traktować danych tak, jakby była częścią jednej logicznej bazy danych podzielonej przez dzierżawcę. Aby symulować tego wzorca, zestaw widoków "global" są dodawane do bazy danych dzierżawy projektu identyfikator dzierżawcy do każdej z tabel, które będą pytani globalnie. Na przykład *VenueEvents* widoku dodaje obliczanej *VenueId* kolumny zaprojektowana z *zdarzenia* tabeli. Poprzez definiowanie tabeli zewnętrznej w bazie danych head za pośrednictwem *VenueEvents* (zamiast odpowiadającego *zdarzenia* tabeli), elastyczne zapytania jest w stanie wypchnąć sprzężenia na podstawie *VenueId* , mogą być wykonywane równolegle na każdym zdalnej bazy danych (a nie na head bazy danych). To znacznie zmniejsza ilość danych, które ma zostać zwrócona, co powoduje znaczne zwiększenie wydajności dla wielu zapytań. Widoki globalne te zostały utworzone wcześniej w wszystkich baz danych dzierżawy (i w *basetenantdb*).

1. Otwórz w programie SSMS oraz [nawiązać tenants1 -&lt;użytkownika&gt; serwera](sql-database-wtp-overview.md#explore-database-schema-and-execute-sql-queries-using-ssms).
2. Rozwiń węzeł **baz danych**, kliknij prawym przyciskiem myszy **contosoconcerthall**i wybierz **nowe zapytanie**.
3. Uruchom następujące kwerendy, aby eksplorować różnica między pojedynczej dzierżawy tabele i widoki globalne:

   ```T-SQL
   -- The base Venue table, that has no VenueId associated.
   SELECT * FROM Venue

   -- Notice the plural name 'Venues'. This view projects a VenueId column.
   SELECT * FROM Venues

   -- The base Events table, which has no VenueId column.
   SELECT * FROM Events

   -- This view projects the VenueId retrieved from the Venues table.
   SELECT * FROM VenueEvents
   ```

W tych widokach *VenueId* jest obliczany jako skrót nazwę miejsce, ale jakiejkolwiek metody może zostać użyty do wprowadzenia unikatową wartość. Ta metoda jest podobny do sposobu klucz dzierżawy jest obliczana do użycia w katalogu.

Aby sprawdzić definicji *miejsc* widoku:

1. W **Eksplorator obiektów**, rozwiń węzeł **contosoconcethall** > **widoków**:

   ![Widoki](media/sql-database-saas-tutorial-adhoc-analytics/views.png)

2. Kliknij prawym przyciskiem myszy **dbo. Miejsc**.
3. Wybierz **skryptu widoku w postaci** > **utworzyć** > **nowe okno edytora zapytań**

Skrypt żadnym innym *miejscową* widoków, aby zobaczyć, jak dodać *VenueId*.

## <a name="deploy-the-database-used-for-ad-hoc-distributed-queries"></a>Wdrażania bazy danych używane do zapytań ad hoc rozproszonych

Wdraża tego ćwiczenia *adhocanalytics* bazy danych. Jest to head bazy danych, która zawiera schematu używanego na potrzeby zapytań dla wszystkich baz danych dzierżawy. Bazy danych jest wdrażana na istniejącym serwerze katalogu, co jest serwera używanego do wszystkich związanych z zarządzaniem baz danych w przykładowej aplikacji.

1. Otwórz plik ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalytics.ps1* w środowisku *PowerShell ISE* i ustaw następujące wartości:
   * **$DemoScenario** = 2, **Deploy Ad-hoc analytics database**.

2. Naciśnij klawisz **F5** uruchomić skrypt i utworzyć *adhocanalytics* bazy danych.

W następnej sekcji możesz dodać schematu bazy danych, więc może służyć do uruchamiania zapytań rozproszonych.

## <a name="configure-the-database-for-running-distributed-queries"></a>Skonfiguruj bazę danych do uruchamiania zapytań rozproszonych

Tego ćwiczenia dodaje schematu (zewnętrznego źródła danych i definicji tabeli zewnętrznej) do bazy danych analizy ad-hoc, która umożliwia wykonywanie zapytania dla wszystkich baz danych dzierżawy.

1. Otwórz program SQL Server Management Studio i połączenia z bazą danych analizy ad hoc, utworzony w poprzednim kroku. Nazwa bazy danych jest *adhocanalytics*.
2. Otwórz ...\Learning Modules\Operational Analytics\Adhoc Analytics\ *AdhocAnalyticsDB.sql zainicjować* w programie SSMS.
3. Przejrzyj skrypt SQL i należy uwzględnić następujące informacje:

   Elastyczne zapytanie używa poświadczeń o zakresie bazy danych można uzyskać dostępu do wszystkich baz danych dzierżawy. To poświadczenie musi być dostępna w bazach danych i zwykle przyznawane minimalnych uprawnień powinien włączyć tych zapytań ad hoc.

    ![Tworzenie poświadczeń](media/sql-database-saas-tutorial-adhoc-analytics/create-credential.png)

   Źródło danych zewnętrznych zdefiniowanego można użyć mapy niezależnego fragmentu dzierżawcy w bazie danych katalogu. Korzystając z niniejszego jako zewnętrzne źródło danych, zapytania są dystrybuowane do wszystkich baz danych zarejestrowane w wykazie, po uruchomieniu kwerendy. Ponieważ nazwy serwerów są różne dla każdego wdrożenia, ten skrypt inicjacji pobiera lokalizacji bazy danych katalogu, pobierając bieżącego serwera (@@servername) gdy skrypt zostanie wykonany.

    ![Tworzenie zewnętrznego źródła danych](media/sql-database-saas-tutorial-adhoc-analytics/create-external-data-source.png)

   Tabele zewnętrzne odwołujące się do widoki globalne opisanych w poprzedniej sekcji oraz zdefiniowana z **dystrybucji = SHARDED(VenueId)**. Ponieważ każdy *VenueId* map do pojedynczej bazy danych, poprawia to wydajność w wielu scenariuszach, jak pokazano w następnej sekcji.

    ![Tworzenie tabel zewnętrznych](media/sql-database-saas-tutorial-adhoc-analytics/external-tables.png)

   Lokalnej tabeli *VenueTypes* które jest tworzone i wypełniane. Tabeli danych to odwołanie jest typowe w przypadku wszystkich dzierżawy baz danych, więc może być reprezentowany jako lokalnej tabeli i wypełnione do wspólnych danych. Dla niektórych kwerend może to zmniejszyć ilość danych przenoszone między bazami danych dzierżawy i *adhocanalytics* bazy danych.

    ![Tworzenie tabeli](media/sql-database-saas-tutorial-adhoc-analytics/create-table.png)

   Jeśli dołączysz tabele odwołań w ten sposób, należy zaktualizować schemat tabeli i dane, po zaktualizowaniu bazy danych dzierżawy.

4. Naciśnij klawisz **F5** do uruchomienia skryptu i zainicjuj *adhocanalytics* bazy danych. 

Teraz można uruchomić zapytania rozproszone i zbieranie szczegółowych informacji we wszystkich dzierżawców!

## <a name="run-ad-hoc-distributed-queries"></a>Uruchamianie zapytań ad hoc rozproszonych

Teraz, gdy *adhocanalytics* baza danych jest skonfigurowany, przejdź dalej i uruchamiania niektórych zapytań rozproszonych. Obejmują plan wykonania w celu lepszego zrozumienia, z której jest przeprowadzana przetwarzania zapytania. 

Podczas sprawdzania plan wykonania, umieść kursor nad ikony planu, aby uzyskać szczegółowe informacje. 

To ustawienie jest ważne, należy pamiętać, **dystrybucji = SHARDED(VenueId)** podczas zdefiniowanego z zewnętrznym źródłem danych, zwiększa wydajność w różnych scenariuszach. Ponieważ każdy *VenueId* map do pojedynczej bazy danych, filtrowania łatwo odbywa się zdalnie, zwraca tylko te dane, które są potrzebne.

1. Otwórz plik ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalyticsQueries.sql* w narzędziu SSMS.
2. Upewnij się, czy nawiązano **adhocanalytics** bazy danych.
3. Wybierz **zapytania** menu i kliknij przycisk **obejmują rzeczywisty Plan wykonania**
4. Wyróżnij *miejsc, które są obecnie zarejestrowane?* zapytania, a następnie naciśnij klawisz **F5**.

   Zapytanie zwraca listę całego miejsca, pokazujący, jak szybko i łatwo jest zapytania dla wszystkich dzierżawców i zwracanych danych z każdej dzierżawy.

   Zbadaj planu i kosztów całej jest zapytania zdalnego, ponieważ firma Microsoft jest po prostu do każdej dzierżawy bazy danych i wybierając informacji miejsce.

   ![Wybierz * z dbo. Miejsc](media/sql-database-saas-tutorial-adhoc-analytics/query1-plan.png)

5. Wybierz przycisk Dalej, zapytania, a następnie naciśnij klawisz **F5**.

   To zapytanie sprzężenia danych z baz danych dzierżawy i lokalnej *VenueTypes* tabeli (lokalny, w jakiej jest tabelą *adhocanalytics* bazy danych).

   Przejrzyj plan i większość koszt jest zapytania zdalnego, ponieważ możemy wysyłać kwerendy informacji o miejscu każdego dzierżawcy (dbo. Miejsc), a następnie wykonaj szybkie sprzężenie lokalne z lokalnej *VenueTypes* tabela, która ma przyjazną nazwę wyświetlaną.

   ![Dołącz do danych zdalnych i lokalnych](media/sql-database-saas-tutorial-adhoc-analytics/query2-plan.png)

6. Teraz wybierz *na dzień najbardziej biletów sprzedano?* zapytania, a następnie naciśnij klawisz **F5**.

   To zapytanie jest bardziej złożonych przyłączanie i agregacji. Należy pamiętać, jest to najbardziej przetwarzania jest wykonywane zdalnie, czy ponownie, możemy przywrócić tylko wiersze, które są potrzebne, zwracanie tylko jednego wiersza dla każdego miejscową agregacji biletu sprzedaży liczba na dzień.

   ![query](media/sql-database-saas-tutorial-adhoc-analytics/query3-plan.png)


## <a name="next-steps"></a>Następne kroki

W tym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Uruchamiania rozproszonych zapytań dla wszystkich baz danych dzierżawy
> * Wdrażanie bazy danych analizy ad hoc i dodać schematu do jego uruchomienie zapytań rozproszonych.


Teraz spróbuj [samouczek analizy dzierżawy](sql-database-saas-tutorial-tenant-analytics.md) do eksplorowania wyodrębnianie danych do bazy danych analizy oddzielne bardziej złożonych przetwarzania analizy...

## <a name="additional-resources"></a>Dodatkowe zasoby

* Dodatkowe [samouczków, z którymi aplikacji Wingtip SaaS](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastyczne zapytanie](sql-database-elastic-query-overview.md)
