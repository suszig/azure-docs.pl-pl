---
title: "SaaS w pudełku (przykładowa aplikacja SaaS korzystająca z usługi Azure SQL Database) | Dokumentacja firmy Microsoft"
description: "Tworzenie aplikacji SaaS przy użyciu usługi SQL Database"
keywords: "samouczek usługi sql database"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: bf5745a788cd9ab6bf2ea8d5d97b8c04f083fc5d
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017


---
# <a name="introduction-to-the-wingtip-tickets-platform-wtp-sample-saas-application"></a>Wprowadzenie do przykładowej aplikacji SaaS o nazwie WTP (Wingtip Tickets Platform)

Aplikacja SaaS o nazwie WTP (Wingtip Tickets Platform) jest przykładową aplikacją z wieloma dzierżawami, która pokazuje unikatowe korzyści ze stosowania usługi SQL Database. Aplikacja używa wzorca aplikacji SaaS database-per-tenant (baza danych dla dzierżawy) do obsługi wielu dzierżaw. Aplikacja WTP jest przeznaczona do prezentowania funkcji usługi Azure SQL Database umożliwiających stosowanie scenariuszy SaaS, w tym wzorców projektowania i zarządzania SaaS. [Czas wdrożenia aplikacji WTP jest krótszy niż pięć minut](sql-database-saas-tutorial.md)!

Po wdrożeniu aplikacji WTP zapoznaj się z [samouczkami](#sql-database-saas-tutorials), które są rozwinięciem początkowego wdrożenia. Każdy z samouczków skupia się na typowych zadaniach, które są implementowane w aplikacjach SaaS. Zadania są wdrażane zgodnie z wzorcami SaaS, które wykorzystują wbudowane funkcje usługi SQL Database. Wzorce te obejmują aprowizację nowych dzierżaw, odtwarzanie baz danych dzierżaw, uruchamianie zapytań rozproszonych we wszystkich dzierżawach oraz wdrażanie zmian schematu we wszystkich bazach danych dzierżaw. Każdy samouczek zawiera skrypty przeznaczone do wielokrotnego użytku wraz ze szczegółowymi wyjaśnieniami, które znacznie ułatwiają zrozumienie i implementowanie takich samych wzorców zarządzania SaaS we własnych aplikacjach.

Aplikacja WTP stanowi pełne rozwiązanie i dobrze pełni rolę aplikacji przykładowej, jednak ważne jest skupienie się na podstawowych wzorcach SaaS, gdyż dotyczą one warstwy danych. Innymi słowy, należy zwrócić szczególną uwagę na warstwę danych i nie skupiać się nadmiernie na samej aplikacji. Zrozumienie implementacji podstawowych wzorców SaaS jest kluczem do implementacji tych wzorców w aplikacjach, przy uwzględnieniu wszystkich niezbędnych modyfikacji wymuszonych konkretnymi potrzebami biznesowymi.



## <a name="application-architecture"></a>Architektura aplikacji

Aplikacja WTP używa modelu database-per-tenant (baza danych dla dzierżawy) i korzysta z elastycznych pul SQL, aby zmaksymalizować wydajność.
Wykaz dzierżaw używany do zarządzania aprowizacją i łącznością.
Zintegrowane funkcje monitorowania i alerty aplikacji, pul i bazy danych (OMS).
Schemat międzydzierżawowy i zarządzanie danymi referencyjnymi (zadania elastyczne bazy danych).
Kwerenda międzydzierżawowa, analiza operacyjna (elastyczne zapytanie).
Rozszerzanie zasięgu przy użyciu danych rozproszonych geograficznie.
Ciągłość prowadzenia działalności biznesowej odzyskiwania pojedynczej dzierżawy (PITR) odzyskiwanie danych w dużej skali (geograficzne przywracanie, replikacja geograficzna, automatyczne odzyskiwanie danych). Samoobsługowe zarządzanie dzierżawą (przy użyciu interfejsów API zarządzania). Odzyskiwanie do punktu w czasie po własnych błędach.

Główna aplikacja Wingtip używa puli z trzema przykładowymi dzierżawami oraz bazy danych wykazu.

![Architektura aplikacji WTP](media/sql-database-wtp-overview/wtp-architecture.png)


## <a name="sql-database-wtp-saas-tutorials"></a>Samouczki aplikacji SaaS o nazwie WTP korzystającej z usługi SQL Database

Poniższe samouczki dotyczą dalszych działań realizowanych po początkowym wdrożeniu [przykładowej aplikacji SaaS o nazwie Wingtip Tickets Platform (WTP)](sql-database-saas-tutorial.md):

| Obszar | Opis | Lokalizacja skryptu |
|:--|:--|:--|
|[Samouczek dotyczący aprowizacji i dzierżaw wykazu](sql-database-saas-tutorial-provision-and-catalog.md)| Aprowizowanie nowych dzierżaw i rejestrowanie ich w wykazie | [Skrypty w witrynie github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Provision%20and%20Catalog) |
|[Samouczek monitorowania i zarządzania wydajnością](sql-database-saas-tutorial-performance-monitoring.md)| Monitorowanie i zarządzanie wydajnością bazy danych i puli | [Skrypty w witrynie github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Performance%20Monitoring%20and%20Management) |
|[Samouczek przywracania pojedynczej dzierżawy](sql-database-saas-tutorial-restore-single-tenant.md)| Przywracanie baz danych dzierżawy | [Skrypty w witrynie github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Business%20Continuity%20and%20Disaster%20Recovery/RestoreTenant) |
|[Samouczek zarządzania schematami dzierżawy](sql-database-saas-tutorial-schema-management.md)| Wykonywanie zapytań we wszystkich dzierżawach  | [Skrypty w witrynie github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Schema%20Management) |
|[Samouczek uruchamiania analitycznych zapytań ad-hoc](sql-database-saas-tutorial-adhoc-analytics.md) | Tworzenie analitycznej bazy danych ad hoc i uruchamianie zapytań dla wszystkich dzierżaw  | [Skrypty w witrynie github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Operational%20Analytics/Adhoc%20Analytics) |
|[Samouczek zarządzania przy użyciu usługi Log Analytics (OMS)](sql-database-saas-tutorial-log-analytics.md) | Konfigurowanie i korzystanie z usługi Log Analytics | [Skrypty w witrynie github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Performance%20Monitoring%20and%20Management/LogAnalytics) |
|[Samouczek uruchamiania analiz dzierżaw](sql-database-saas-tutorial-tenant-analytics.md) | Konfigurowanie i uruchamianie zapytań analitycznych dzierżaw | [Skrypty w witrynie github](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules/Operational%20Analytics/Tenant%20Analytics) |

## <a name="get-the-wingtip-application-scripts"></a>Pobieranie skryptów aplikacji Wingtip

Skrypty i kod źródłowy aplikacji Wingtip Tickets są dostępne w repozytorium GitHub [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Pliki skryptów znajdują się w folderze [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Pobierz folder **Learning Modules** na komputer lokalny, zachowując jego strukturę folderów.

## <a name="working-with-the-wtp-powershell-scripts"></a>Praca ze skryptami PowerShell aplikacji WTP

Korzyści wynikające z pracy z aplikacją WTP polegają na możliwości dokładnego zapoznania się ze skryptami i sprawdzenia sposobu implementacji różnych wzorców SaaS.

Aby wyświetlić dostarczone skrypty i moduły oraz ułatwić zapoznanie się z nimi, należy użyć programu [Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Ponieważ większość skryptów oznaczonych prefiksem *Demo-* zawiera zmienne, które można zmodyfikować przed wykonaniem, użycie programu PowerShell ISE upraszcza pracę ze skryptami.

W przypadku każdego wdrożenia aplikacji WTP dostępny jest plik **UserConfig.psm1** zawierający dwa parametry służące do ustawiania wartości grupy zasobów i nazwy użytkownika, które zostały zdefiniowane podczas wdrażania. Po zakończeniu wdrożenia należy dokonać edycji modułu **UserConfig.psm1**, ustawiając wartości parametrów _ResourceGroupName_ i _Name_. Wartości te są używane przez inne skrypty do pomyślnego uruchomienia, dlatego zaleca się ich ustawienie po zakończeniu wdrożenia.



### <a name="execute-scripts-by-pressing-f5"></a>Wykonywanie skryptów poprzez naciśnięcie klawisza F5

Szereg skryptów używa zmiennej *$PSScriptRoot* umożliwiającej nawigowanie po folderach. Wartość tej zmiennej jest obliczana przy wykonywaniu skryptu po naciśnięciu klawisza **F5**.  Wyróżnienie obszaru i uruchomienie go (**F8**) może spowodować błędy, dlatego skrypty WTP należy uruchamiać, naciskając klawisz **F5**.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Przechodzenie przez skrypty w celu zapoznania się z implementacją

Prawdziwa korzyść płynąca z badania skryptów polega na przechodzeniu przez nie przy zapoznawaniu się z ich działaniem. Zapoznaj się ze skryptami pierwszego poziomu _Demo-_, które zawierają łatwy do prześledzenia wysokopoziomowy przepływ pracy, pokazujący kroki wymagane do wykonania poszczególnych zadań. Należy zagłębić się w poszczególne wywołania, aby wyświetlić szczegóły dotyczące implementacji różnych wzorców SaaS.

Porady dotyczące pracy ze skryptami PowerShell i [debugowania ich](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise):

* Otwórz i skonfiguruj skrypty demonstracyjne (Demo-) w programie PowerShell ISE.
* Wykonaj skrypt lub przejdź dalej, naciskając klawisz **F5**. Nie jest zalecane używanie klawisza **F8**, ponieważ wtedy wartość zmiennej *$PSScriptRoot* nie jest obliczana podczas uruchamiania wybranego fragmentu skryptu.
* Umieść punkty przerwania, klikając lub zaznaczając wiersz i naciskając klawisz **F9**.
* Pomiń wywołanie funkcji lub skryptu, używając klawisza **F10**.
* Wejdź do wywoływanej funkcji lub skryptu, używając klawisza **F11**.
* Wyjdź z bieżącego wywołania funkcji lub skryptu, używając kombinacji klawiszy **Shift + F11**.




## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Badanie schematu bazy danych i wykonywanie zapytań SQL za pomocą aplikacji SSMS

Użyj aplikacji [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) do nawiązywania połączenia i przeglądania serwerów i baz danych aplikacji WTP.

Przykładowa aplikacja WTP początkowo ma dwa serwery baz danych SQL Database, z którymi nawiązuje połączenie — serwer *tenants1* i serwer *catalog*:


1. Otwórz aplikację *SSMS* i połącz się z serwerem *tenants1-&lt;Użytkownik&gt;.database.windows.net*.
2. Kliknij kolejno opcje **Połącz** > **Aparat bazy danych...** :

   ![serwer wykazu](media/sql-database-wtp-overview/connect.png)

1. W wersji demonstracyjnej są używane następujące poświadczenia: Nazwa logowania = *developer*, hasło =*P@ssword1*

   ![połączenie](media\sql-database-wtp-overview\tenants1-connect.png)

1. Powtórz kroki 2 i 3 i połącz się z serwerem *catalog-&lt;Użytkownik&gt;.database.windows.net*.

Po pomyślnym nawiązaniu połączenia powinny być widoczne oba serwery. W zależności liczby aprowizowanych dzierżaw może być widoczna większa lub mniejsza liczba baz danych:

![eksplorator obiektów](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a>Następne kroki

[Wdrażanie przykładowej aplikacji SaaS o nazwie WTP (Wingtip Tickets Platform)](sql-database-saas-tutorial.md)
