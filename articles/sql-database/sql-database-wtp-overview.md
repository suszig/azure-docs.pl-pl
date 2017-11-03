---
title: "Przykład wielodostępnych aplikacji bazy danych SQL Azure — Wingtip SaaS | Dokumentacja firmy Microsoft"
description: "Dowiedz się, korzystając z przykładowej aplikacji wielodostępnych, która używa usługi Azure SQL Database w przykładzie Wingtip SaaS"
keywords: "samouczek usługi sql database"
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: sstein
ms.openlocfilehash: 46c9a3eadc2c23959b4d08649c6c0215d44b493e
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="introduction-to-a-sql-database-multi-tenant-saas-app-example"></a>Wprowadzenie do przykład aplikacji SaaS wielodostępne połączenia bazy danych SQL

*Wingtip SaaS* jest aplikacja przykładowa aplikacja wielodostępne, który demonstruje wyjątkowych zalet bazy danych SQL. Aplikacja używa wzorca aplikacji SaaS database-per-tenant (baza danych dla dzierżawy) do obsługi wielu dzierżaw. Aplikacja jest przeznaczona do pokazują funkcje bazy danych SQL Azure, które umożliwiają SaaS scenariuszy, w tym kilka wzorce projektowania i zarządzania SaaS. Aby szybko rozpocząć pracę, aplikacja Wingtip SaaS wdraża w mniej niż pięć minut!

Skrypty zarządzania i kodu źródłowego aplikacji są dostępne w [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) repozytorium github. Aby uruchamiać skrypty, [folder modułów uczenia pobierania](#download-and-unblock-the-wingtip-saas-scripts) na komputerze lokalnym.

## <a name="sql-database-wingtip-saas-tutorials"></a>Samouczki SaaS Wingtip bazy danych SQL

Po wdrożeniu aplikacji, Poznaj następujące samouczki, które zależą od momentu pierwszego wdrożenia. Te samouczki Poznaj typowe wzorce SaaS, które korzystają z wbudowanych funkcji bazy danych SQL, SQL Data Warehouse i innymi usługami Azure. Samouczki zawierają skryptów programu PowerShell z szczegółowe wyjaśnienia, które znacznie upraszcza opis i wdrażanie tego samego wzorce zarządzania SaaS w aplikacji.


| Samouczek | Opis |
|:--|:--|
|[Wdrażanie i Eksploruj aplikacji Wingtip SaaS](sql-database-saas-tutorial.md)| **ZACZNIJ TUTAJ!** Wdrażanie i Eksploruj aplikacji Wingtip SaaS do subskrypcji platformy Azure. |
|[Dostarczanie i katalogu dzierżawcy](sql-database-saas-tutorial-provision-and-catalog.md)| Dowiedz się, jak aplikacja łączy się dzierżawcy przy użyciu bazy danych katalogu oraz jak katalogu mapuje dzierżawcy do swoich danych. |
|[Monitorowanie i zarządzanie nimi wydajności](sql-database-saas-tutorial-performance-monitoring.md)| Dowiedz się, jak używać funkcji monitorowania bazy danych SQL i sposobu ustawiania alertów po przekroczeniu progów wydajności. |
|[Monitorowanie za pomocą analizy dzienników (OMS)](sql-database-saas-tutorial-log-analytics.md) | Informacje o używaniu [analizy dzienników](../log-analytics/log-analytics-overview.md) do monitorowania dużej ilości zasobów, między wiele pul. |
|[Przywracanie pojedynczej dzierżawy](sql-database-saas-tutorial-restore-single-tenant.md)| Dowiedz się, jak przywracanie dzierżawy bazy danych do wcześniejszego punktu w czasie. Włącza się również kroki, aby przywrócić równoległych bazy danych w trybie online, pozostawiając istniejącej bazy danych dzierżawy. |
|[Zarządzanie schematem dzierżawy](sql-database-saas-tutorial-schema-management.md)| Dowiedz się, jak aktualizacja schematu i aktualizacji danych referencyjnych we wszystkich dzierżaw Wingtip SaaS. |
|[Uruchom analytics ad hoc](sql-database-saas-tutorial-adhoc-analytics.md) | Tworzenie bazy danych analizy ad hoc i uruchamianie zapytań rozproszonych w czasie rzeczywistym we wszystkich dzierżawców.  |
|[Uruchom analytics dzierżawy](sql-database-saas-tutorial-tenant-analytics.md) | Wyodrębnianie danych dzierżawy do analityka bazy danych lub dane magazynu na potrzeby uruchamiania zapytań analityczne w trybie offline. |



## <a name="application-architecture"></a>Architektura aplikacji

Aplikacja Wingtip SaaS korzysta z modelu bazy danych na dzierżawy i korzysta z puli elastycznej SQL Aby zmaksymalizować wydajność. Dla dzierżawcy mapowania do swoich danych i udostępniania bazy danych katalogu jest używany. Podstawowe aplikacji Wingtip SaaS używa puli z trzech dzierżawcami próbki oraz baza danych katalogu. Kończenie wiele SaaS Wingtip samouczki skutkować dodatki początkowe wdrożenie, wprowadzając baz danych analitycznych, między bazami danych zarządzania schematu itp.


![Architektura SaaS Wingtip](media/sql-database-wtp-overview/app-architecture.png)


Podczas przechodzenia przez samouczków i Praca z aplikacją, należy skoncentrować się na wzorce SaaS w odniesieniu do warstwy danych. Innymi słowy, należy zwrócić szczególną uwagę na warstwę danych i nie skupiać się nadmiernie na samej aplikacji. Opis wykonania tych SaaS wzorców jest kluczem do wykonania tych wzorców w aplikacjach, rozważając wszelkie potrzebne modyfikacje dla konkretnych potrzeb biznesowych.

## <a name="download-and-unblock-the-wingtip-saas-scripts"></a>Pobierz i odblokować skrypty Wingtip SaaS

Zawartość pliku wykonywalnego (skrypty, biblioteki dll) mogą być blokowane przez system Windows, gdy pliki zip są pobierane z zewnętrznego źródła i wyodrębnione. Podczas wyodrębniania pliku zip skrypty ***wykonaj poniższe kroki, aby odblokować plik zip przed wyodrębniania***. Dzięki temu można uruchamiać skrypty.

1. Przejdź do [repozytorium github Wingtip SaaS](https://github.com/Microsoft/WingtipSaaS).
1. Kliknij przycisk **klonowania lub pobierania**.
1. Kliknij przycisk **Pobierz ZIP** i Zapisz plik.
1. Kliknij prawym przyciskiem myszy **WingtipSaaS master.zip** pliku, a następnie wybierz **właściwości**.
1. Na **ogólne** wybierz opcję **Odblokuj**.
1. Kliknij przycisk **OK**.
1. Wyodrębnij pliki.

Skrypty znajdują się w *... \\Wzorca WingtipSaaS\\modułów uczenia* folderu.


## <a name="working-with-the-wingtip-saas-powershell-scripts"></a>Praca z skryptów programu PowerShell Wingtip SaaS

Maksymalne wykorzystanie próbki należy przejść do podanego skryptów. Użyj punktów przerwania i krok za pomocą skryptów, sprawdzając szczegóły dotyczące implementowania różnych wzorców SaaS. Aby łatwo krok po kroku podany skryptów i modułów do zrozumienia najlepsze, zaleca się używanie [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-the-configuration-file-for-your-deployment"></a>Aktualizuj plik konfiguracji wdrożenia

Edytuj **UserConfig.psm1** pliku z zasobów grup i użytkowników zostanie ustawiona wartość podczas wdrażania:

1. Otwórz *PowerShell ISE* i obciążenia... \\Modułów szkoleniowych\\*UserConfig.psm1* 
1. Aktualizacja *ResourceGroupName* i *nazwa* z określonymi wartościami dla danego wdrożenia (wierszach 10 i 11 tylko).
1. Zapisać zmiany!

Ustawienie tych wartości w tym miejscu po prostu zapewnia trzeba zaktualizować te wartości specyficzne dla wdrażania wszystkie skrypty.

### <a name="execute-scripts-by-pressing-f5"></a>Wykonywanie skryptów poprzez naciśnięcie klawisza F5

Użyj skryptów kilka *$PSScriptRoot* do przeglądania folderów, i *$PSScriptRoot* jest oceniana tylko wtedy, gdy skryptów są wykonywane przez naciśnięcie przycisku **F5**.  Wyróżnianie i uruchamianie zaznaczenia (**F8**) może powodować błędy, więc naciśnij **F5** podczas uruchamiania skryptów.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Przechodzenie przez skrypty w celu zapoznania się z implementacją

Najlepszy sposób, aby zrozumieć skryptów jest krokowe je, aby zobaczyć, co zrobić. Zapoznaj się z dołączonej **pokaz -** skrypty, które stanowią czytelna ogólny przepływ pracy. **Pokaz -** skrypty Pokaż kroki wymagane do wykonywania poszczególnych zadań, a więc Ustaw punkty przerwania i przejść głębiej do poszczególnych wywołań, aby wyświetlić szczegóły implementacji dla różnych wzorców SaaS.

Wskazówki dotyczące eksplorowania i krokowe wykonywanie skryptów programu PowerShell:

* Otwórz **pokaz -** skryptów w programie PowerShell ISE.
* Wykonanie lub kontynuować **F5** (przy użyciu **F8** nie jest zalecana, ponieważ *$PSScriptRoot* nie jest oceniany, podczas uruchamiania opcji skryptu).
* Umieść punkty przerwania, klikając lub zaznaczając wiersz i naciskając klawisz **F9**.
* Pomiń wywołanie funkcji lub skryptu, używając klawisza **F10**.
* Wejdź do wywoływanej funkcji lub skryptu, używając klawisza **F11**.
* Wyjdź z bieżącego wywołania funkcji lub skryptu, używając kombinacji klawiszy **Shift + F11**.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Badanie schematu bazy danych i wykonywanie zapytań SQL za pomocą aplikacji SSMS

Użyj [programu SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) połączenia i przeglądanie serwerów aplikacji i baz danych.

Wdrożenie początkowo ma dwa serwery bazy danych SQL, aby nawiązać połączenie — *tenants1 -&lt;użytkownika&gt;*  serwera i *katalogu -&lt;użytkownika&gt;*  serwera. W celu zapewnienia połączenia pomyślne demonstracyjnej, oba serwery mają [reguły zapory](sql-database-firewall-configure.md) stosowanie wszystkich adresów IP za pośrednictwem.


1. Otwórz aplikację *SSMS* i połącz się z serwerem *tenants1-&lt;Użytkownik&gt;.database.windows.net*.
1. Kliknij kolejno opcje **Połącz** > **Aparat bazy danych...** :

   ![serwer wykazu](media/sql-database-wtp-overview/connect.png)

1. W wersji demonstracyjnej są używane następujące poświadczenia: Nazwa logowania = *developer*, hasło =*P@ssword1*

   ![połączenie](media\sql-database-wtp-overview\tenants1-connect.png)

1. Powtórz kroki 2 i 3 i połącz się z serwerem *catalog-&lt;Użytkownik&gt;.database.windows.net*.

Po pomyślnym nawiązaniu połączenia powinny być widoczne oba serwery. Listy baz danych, które mogą być różne w zależności od dzierżawcy, które zostały udostępnione:

![eksplorator obiektów](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a>Następne kroki

[Wdrażanie aplikacji Wingtip SaaS](sql-database-saas-tutorial.md)
