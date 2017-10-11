---
title: "Uruchamianie analitycznych zapytań w wielu bazach danych Azure SQL | Microsoft Docs"
description: "Wyodrębniania danych z baz danych dzierżawy do celów analizy offline bazy danych analizy"
keywords: "samouczek usługi sql database"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: billgib; sstein
ms.openlocfilehash: 4e32407d5f321198358e07980907c3420aaf56c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="extract-data-from-tenant-databases-into-an-analytics-database-for-offline-analysis"></a>Wyodrębniania danych z baz danych dzierżawy do celów analizy offline bazy danych analizy

W tym samouczku elastycznej zadanie służy do wykonywania kwerend do każdej dzierżawy bazy danych. To zadanie umożliwia wyodrębnianie danych sprzedaży biletów i ładuje go do bazy danych analizy (lub magazynu danych) do analizy. Baza danych analizy następnie jest poddawany kwerendzie można wyodrębnić szczegółowych informacji z tego codziennych danych operacyjnych wszystkich dzierżawców.


Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie analitycznej bazy danych dzierżaw
> * Tworzenie zaplanowanego zadania do pobierania danych i wypełniania analitycznej bazy danych

Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Aplikacja Wingtip SaaS jest wdrażana. Aby wdrożyć w mniej niż 5 minut, zobacz [wdrażania i aplikacji Wingtip SaaS](sql-database-saas-tutorial.md)
* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* Zainstalowano najnowszą wersję programu SSMS (SQL Server Management Studio). [Pobieranie i instalowanie programu SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a>Wzorzec operacyjnej analizy dzierżaw

Jedną z najważniejszych zalet aplikacji SaaS jest możliwość użycia dużych ilości danych dotyczących dzierżaw, które są przechowywane w chmurze. Użyj tych danych, aby uzyskać wgląd w działanie i wykorzystanie swojej aplikacji i swoich dzierżaw. Te dane mogą ukierunkować rozwój funkcji, usprawnienia w zakresie użyteczności oraz inne inwestycje w aplikacje i platformę. Uzyskiwanie dostępu do tych danych w jednej wielodostępnej bazie danych jest łatwe, ale przestaje być proste, gdy są one znacznie rozproszone, potencjalnie nawet na tysiące baz danych. Jednym z podejść do uzyskiwania dostępu do tych danych jest użycie elastycznych zadań, umożliwiających zwracanie wyników zapytania z wykonywanego zadania i ich przesyłanie do wyjściowej bazy danych i tabeli.

## <a name="get-the-wingtip-application-scripts"></a>Pobieranie skryptów aplikacji Wingtip

Wingtip SaaS skrypty i kod źródłowy aplikacji są dostępne w [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) repozytorium github. [Kroki, aby pobrać skrypty Wingtip SaaS](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="deploy-a-database-for-tenant-analytics-results"></a>Wdrażanie bazy danych dla wyników analizy dzierżawy

Ten samouczek wymaga posiadania bazy danych wdrożonej w celu przechwytywania wyników wykonania zadania skryptów, które zawierają zapytania zwracające wyniki. W tym celu utwórzmy bazę danych o nazwie tenantanalytics.

1. Otwórz plik ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* w programie *PowerShell ISE* i ustaw następującą wartość:
   * **$DemoScenario** = **2** *Deploy operational analytics database* (Wdrażanie bazy danych analizy operacyjnej)
1. Naciśnij klawisz **F5**, aby uruchomić skrypt pokazowy (który wywołuje skrypt *Deploy-TenantAnalyticsDB.ps1*) tworzący bazę danych analizy dzierżaw.

## <a name="create-some-data-for-the-demo"></a>Tworzenie danych demonstracyjnych

1. Otwórz plik ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* w programie *PowerShell ISE* i ustaw następującą wartość:
   * **$DemoScenario** = **1** *Purchase tickets for events at all venues* (Zakup biletów dla zdarzeń we wszystkich miejscach)
1. Naciśnij klawisz **F5**, aby uruchomić skrypt i utworzyć historię zakupów biletów.


## <a name="create-a-scheduled-job-to-retrieve-tenant-analytics-about-ticket-purchases"></a>Tworzenie zaplanowanego zadania do pobierania informacji analitycznych dotyczących zakupów biletów

Ten skrypt tworzy zadanie do pobierania informacji o zakupie biletów ze wszystkich dzierżaw. Po ich zagregowaniu do pojedynczej tabeli można uzyskać szczegółowy wgląd w metryki związane z wzorcami zakupów biletów we wszystkich dzierżawach.

1. Otwórz aplikację SSMS i połącz się z serwerem catalog-&lt;użytkownik&gt;.database.windows.net
1. Otwórz plik ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*
1. Modyfikowanie &lt;użytkownika&gt;, należy użyć nazwy użytkownika używane w przypadku wdrażania aplikacji Wingtip SaaS na początku skryptu, **sp\_dodać\_docelowej\_grupy\_elementu członkowskiego** i **sp\_dodać\_etap zadania**
1. Kliknij prawym przyciskiem myszy, wybierz **połączenia**i połącz się katalogu -&lt;użytkownika&gt;. database.windows.net serwera, jeśli nie został jeszcze połączony
1. Upewnij się, że masz połączenie z bazą danych **jobaccount**, a następnie naciśnij klawisz **F5**, aby uruchomić skrypt.

* **sp\_add\_target\_group** tworzy nazwę grupy docelowej *TenantGroup*, teraz należy dodać docelowe elementy członkowskie.
* **SP\_dodać\_docelowej\_grupy\_elementu członkowskiego** dodaje *serwera* docelowy typ elementu członkowskiego, które uzna za wszystkie bazy danych w ramach tego serwera (należy pamiętać, jest to customer1 -&lt;użytkownika&gt; serwer zawierający dzierżawy baz danych) w czasie zadania wykonywania powinny być uwzględnione w zadaniu.
* **sp\_add\_job** tworzy nowe cotygodniowe zaplanowane zadanie o nazwie „Ticket Purchases from all Tenants”
* **sp\_add\_jobstep** tworzy krok zadania zawierający tekst polecenia T-SQL służącego do pobierania wszystkich informacji o zakupach biletów ze wszystkich dzierżaw i kopiowania zestawu zwracanych wyników do tabeli o nazwie *AllTicketsPurchasesfromAllTenants*
* Pozostałe widoki w skrypcie dotyczą istnienia obiektów i monitorowania wykonania zadań. Sprawdź wartość stanu w kolumnie **lifecycle**, aby monitorować stan. Stan Powodzenie będzie oznaczał, że zadanie zostało zakończone pomyślnie dla wszystkich baz danych dzierżaw i dwóch dodatkowych baz danych zawierających tabelę referencyjną.

Pomyślne uruchomienie skryptu powinno zwrócić wyniki podobne do następujących:

![wyniki](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-to-retrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a>Tworzenie zadania pobierania łącznej liczby zakupów biletów we wszystkich dzierżawach

Ten skrypt tworzy zadanie pobierania informacji o sumie wszystkich zakupów biletów ze wszystkich dzierżaw.

1. Otwórz aplikację SSMS i połącz się z serwerem *catalog-&lt;Użytkownik&gt;.database.windows.net*
1. Otwórz plik …\\Learning Modules\\Provision and Catalog\\Operational Analytics\\Tenant Analytics\\*Results-TicketPurchasesfromAllTenants.sql*
1. Modyfikowanie &lt;użytkownika&gt;, należy użyć nazwy użytkownika używane w przypadku wdrażania aplikacji Wingtip SaaS w skrypcie w **sp\_dodać\_etap zadania** procedury składowanej
1. Kliknij prawym przyciskiem myszy, wybierz **połączenia**i połącz się katalogu -&lt;użytkownika&gt;. database.windows.net serwera, jeśli nie został jeszcze połączony
1. Upewnij się, że masz połączenie z bazą danych **tenantanalytics**, a następnie naciśnij klawisz **F5**, aby uruchomić skrypt

Pomyślne uruchomienie skryptu powinno zwrócić wyniki podobne do następujących:

![wyniki](media/sql-database-saas-tutorial-tenant-analytics/total-sales.png)



* **sp\_add\_job** tworzy nowe cotygodniowe zaplanowane zadanie o nazwie „ResultsTicketsOrders”

* **sp\_add\_jobstep** tworzy krok zadania zawierający tekst polecenia T-SQL służącego do pobierania wszystkich informacji o zakupach biletów ze wszystkich dzierżaw i kopiowania zestawu zwracanych wyników do tabeli o nazwie CountofTicketOrders

* Pozostałe widoki w skrypcie dotyczą istnienia obiektów i monitorowania wykonania zadań. Sprawdź wartość stanu w kolumnie **lifecycle**, aby monitorować stan. Stan Powodzenie będzie oznaczał, że zadanie zostało zakończone pomyślnie dla wszystkich baz danych dzierżaw i dwóch dodatkowych baz danych zawierających tabelę referencyjną.


## <a name="next-steps"></a>Następne kroki

W tym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wdrażanie analitycznej bazy danych dzierżaw
> * Tworzenie zaplanowanego zadania do pobierania danych analitycznych z dzierżaw

Gratulacje!

## <a name="additional-resources"></a>Dodatkowe zasoby

* Dodatkowe [samouczków, z którymi aplikacji Wingtip SaaS](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Zadania elastyczne](sql-database-elastic-jobs-overview.md)
