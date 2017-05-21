---
title: "Uruchamianie zapytań analitycznych dla wielu dzierżaw (przykładowa aplikacja SaaS korzystająca z usługi Azure SQL Database) | Dokumentacja firmy Microsoft"
description: "Uruchamianie zapytań analitycznych dla wielu dzierżaw"
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
ms.openlocfilehash: b512e2f7833be1947ef7674d6e0266879789ac5a
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017


---
# <a name="run-analytics-queries-against-multiple-tenants"></a>Uruchamianie zapytań analitycznych dla wielu dzierżaw

W tym samouczku wykonywane są zapytania analityczne dla każdej dzierżawy w wykazie. Tworzone jest elastyczne zadanie, które uruchamia zapytania. To zadanie pobiera dane i ładuje je do oddzielnej analitycznej bazy danych utworzonej na serwerze wykazu. Na tej bazie danych można wykonywać zapytania, aby wydobyć informacje ukryte pośród codziennych danych operacyjnych pochodzących od wszystkich dzierżaw. Danymi wyjściowymi tego zadania jest tabela utworzona na podstawie zwracających wyniki zapytań wewnątrz analitycznej bazy danych dzierżaw.


Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie analitycznej bazy danych dzierżaw
> * Tworzenie zaplanowanego zadania do pobierania danych i wypełniania analitycznej bazy danych

Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Wdrożona jest aplikacja WTP. Aby wdrożyć tę aplikację w czasie krótszym niż pięć minut, zobacz [Wdrażanie i korzystanie z aplikacji SaaS o nazwie WTP](sql-database-saas-tutorial.md)
* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* Zainstalowano najnowszą wersję programu SSMS (SQL Server Management Studio). [Pobieranie i instalowanie programu SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a>Wzorzec operacyjnej analizy dzierżaw

Jedną z najważniejszych zalet aplikacji SaaS jest możliwość użycia dużych ilości danych dotyczących dzierżaw, które są przechowywane w chmurze. Użyj tych danych, aby uzyskać wgląd w działanie i wykorzystanie swojej aplikacji i swoich dzierżaw. Te dane mogą ukierunkować rozwój funkcji, usprawnienia w zakresie użyteczności oraz inne inwestycje w aplikacje i platformę. Uzyskiwanie dostępu do tych danych w jednej wielodostępnej bazie danych jest łatwe, ale przestaje być proste, gdy są one znacznie rozproszone, potencjalnie nawet na tysiące baz danych. Jednym z podejść do uzyskiwania dostępu do tych danych jest użycie elastycznych zadań, umożliwiających zwracanie wyników zapytania z wykonywanego zadania i ich przesyłanie do wyjściowej bazy danych i tabeli.

## <a name="get-the-wingtip-application-scripts"></a>Pobieranie skryptów aplikacji Wingtip

Skrypty i kod źródłowy aplikacji Wingtip Tickets są dostępne w repozytorium GitHub [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Pliki skryptów znajdują się w folderze [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Pobierz folder **Learning Modules** na komputer lokalny, zachowując jego strukturę folderów.

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

1. Otwórz aplikację SSMS i połącz się z serwerem catalog-\<użytkownik\>.database.windows.net
1. Otwórz plik ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*
1. Zmodyfikuj parametr \<WtpUser\>, użyj nazwy użytkownika podanej podczas wdrażania aplikacji WTP u góry skryptu, **sp\_add\_target\_group\_member** i **sp\_add\_jobstep**
1. Kliknij prawym przyciskiem myszy, wybierz opcję **Połączenie** i nawiąż połączenie z serwerem catalog-\<WtpUser.\> database.windows.net, jeśli jeszcze tego nie zrobiono
1. Upewnij się, że masz połączenie z bazą danych **jobaccount**, a następnie naciśnij klawisz **F5**, aby uruchomić skrypt.

* **sp\_add\_target\_group** tworzy nazwę grupy docelowej *TenantGroup*, teraz należy dodać docelowe elementy członkowskie.
* **sp\_add\_target\_group\_member** dodaje typ docelowych elementów członkowskich *server*, obejmujący wszystkie bazy danych znajdujące się na tym serwerze (zauważ, że jest to serwer customer1-&lt;WtpUser&gt; zawierający bazy danych dzierżaw), które w chwili wykonywania zadania powinny być uwzględnione w zadaniu.
* **sp\_add\_job** tworzy nowe cotygodniowe zaplanowane zadanie o nazwie „Ticket Purchases from all Tenants”
* **sp\_add\_jobstep** tworzy krok zadania zawierający tekst polecenia T-SQL służącego do pobierania wszystkich informacji o zakupach biletów ze wszystkich dzierżaw i kopiowania zestawu zwracanych wyników do tabeli o nazwie *AllTicketsPurchasesfromAllTenants*
* Pozostałe widoki w skrypcie dotyczą istnienia obiektów i monitorowania wykonania zadań. Sprawdź wartość stanu w kolumnie **lifecycle**, aby monitorować stan. Stan Powodzenie będzie oznaczał, że zadanie zostało zakończone pomyślnie dla wszystkich baz danych dzierżaw i dwóch dodatkowych baz danych zawierających tabelę referencyjną.

Pomyślne uruchomienie skryptu powinno zwrócić wyniki podobne do następujących:

![wyniki](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-to-retrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a>Tworzenie zadania pobierania łącznej liczby zakupów biletów we wszystkich dzierżawach

Ten skrypt tworzy zadanie pobierania informacji o sumie wszystkich zakupów biletów ze wszystkich dzierżaw.

1. Otwórz aplikację SSMS i połącz się z serwerem *catalog-&lt;Użytkownik&gt;.database.windows.net*
1. Otwórz plik …\\Learning Modules\\Provision and Catalog\\Operational Analytics\\Tenant Analytics\\*Results-TicketPurchasesfromAllTenants.sql*
1. Zmodyfikuj parametr &lt;WtpUser&gt;, używając nazwy użytkownika podanej podczas wdrażania aplikacji WTP w skrypcie, w procedurze składowanej **sp\_add\_jobstep**
1. Kliknij prawym przyciskiem myszy, wybierz opcję **Połączenie** i nawiąż połączenie z serwerem catalog-\<WtpUser.\> database.windows.net, jeśli jeszcze tego nie zrobiono
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

* [Dodatkowe samouczki nawiązujące do początkowego wdrożenia aplikacji Wingtip Tickets Platform (WTP)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Zadania elastyczne](sql-database-elastic-jobs-overview.md)
