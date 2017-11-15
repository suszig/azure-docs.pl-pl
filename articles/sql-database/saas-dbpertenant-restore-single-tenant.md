---
title: "Przywracanie bazy danych Azure SQL w wielodostępnych aplikacji SaaS | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak przywracanie bazy danych SQL pojedynczego dzierżawcy po przypadkowym usunięciu danych"
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
ms.date: 05/10/2017
ms.author: billgib;sstein
ms.openlocfilehash: 60788b8a1b417e9bdfbe5ea8424dff9a39f36f2d
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
# <a name="restore-a-single-tenants-azure-sql-database-in-a-multi-tenant-saas-app"></a>Przywracanie bazy danych Azure SQL pojedynczego dzierżawcy w wielodostępnych aplikacji SaaS

Aplikacja Wingtip SaaS jest utworzony przy użyciu modelu bazy danych dla dzierżawy, którym każdy dzierżawca ma własnych bazy danych. Jedną z zalet tego modelu jest łatwo przywrócić dane z pojedynczej dzierżawy w izolacji bez wpływu na innych dzierżawców.

W tym samouczku Dowiedz się dwa wzorce odzyskiwania danych:

> [!div class="checklist"]

> * Przywróć bazę danych do bazy danych równoległej (side-by-side)
> * Przywracanie bazy danych w miejscu


|||
|:--|:--|
| **Przywróć dzierżawy do wcześniejszego punktu w czasie do równoległego bazy danych** | Ten wzorzec może służyć przez dzierżawcę do przeglądu, inspekcji, zgodności itd. Oryginalnej bazy danych pozostaje bez zmian i online. |
| **Przywróć dzierżawy w miejscu** | Ten wzorzec jest zwykle używane do odzyskania dzierżawy do wcześniejszego punktu w czasie, po dzierżawcy przypadkowo usunął danych. Oryginalnej bazy danych jest przełączona w tryb offline i zastąpione przywróconej bazy danych. |
|||

Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Aplikacja Wingtip SaaS jest wdrażana. Aby wdrożyć w mniej niż 5 minut, zobacz [wdrażania i aplikacji Wingtip SaaS](saas-dbpertenant-get-started-deploy.md)
* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-saas-tenant-restore-pattern"></a>Wprowadzenie do wzorca przywracania dzierżawy SaaS

Dla dzierżawcy wzorca przywracania istnieją dwa proste wzorce w celu przywrócenia danych dzierżawy usługi. Ponieważ dzierżawy baz danych są od siebie odizolowane, Przywracanie jednego dzierżawcy nie ma wpływu na dane żadnych innych dzierżawców.

We wzorcu pierwsze dane są przywracane do nowej bazy danych. Dzierżawca jest następnie uzyskać dostęp do tej bazy danych wraz z ich danych produkcyjnych. Ten wzorzec umożliwia administratora dzierżawy przejrzeć przywróconych danych i potencjalnie Użyj selektywnie zastąpić bieżące wartości danych. To projektanta aplikacji SaaS, aby określić, jak zaawansowane opcje odzyskiwania danych powinien być. Po prostu możliwość sprawdzenia danych w stanie, który znajdował się w danym punkcie w czasie może być wszystko, czego jest wymagany w niektórych scenariuszach. Jeśli baza danych używa [— replikacja geograficzna](sql-database-geo-replication-overview.md), zaleca się kopiowanie wymaganych danych z przywróconej kopii do oryginalnej bazy danych. Można zastąpić oryginalnej bazy danych przywróconej bazy danych, musisz skonfigurować ponownie i przeprowadź ponowną synchronizację — replikacja geograficzna.

W drugim wzorcu przyjęto założenie, że dzierżawy poniosła utraty lub uszkodzenia danych, dzierżawcy w produkcyjnej bazie danych zostanie przywrócona do wcześniejszego punktu w czasie. W ramach operacji przywracania we wzorcu miejsce dzierżawcy do trybu offline przez krótki czas, gdy baza danych jest przywracane i przywrócony do trybu online. Oryginalnej bazy danych zostanie usunięty, ale nadal można przywrócić z, aby wrócić do nawet wcześniejszego punktu w czasie. Zmiany tego wzorca można zmienić nazwy bazy danych zamiast usuwania, mimo że zmiana nazwy bazy danych oferuje nie dodatkowych zalet pod względem zabezpieczeń danych.

## <a name="get-the-wingtip-application-scripts"></a>Pobieranie skryptów aplikacji Wingtip

Wingtip SaaS skrypty i kod źródłowy aplikacji są dostępne w [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) repozytorium github. [Kroki, aby pobrać skrypty Wingtip SaaS](saas-dbpertenant-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Symulowanie dzierżawcy przypadkowego usunięcia danych

Aby zademonstrować tych scenariuszy odzyskiwania, musimy *przypadkowo* usuwać niektóre dane w jednej z baz danych dzierżawy. Podczas gdy można usunąć rekordu, następnym krokiem konfiguruje pokaz do nie pobrać zablokowane przez naruszenie integralności referencyjnej! Dodaje również pewne dane zakupu biletu służy później w *samouczki Wingtip SaaS Analytics*.

Uruchom skrypt generator biletu i utworzyć dodatkowe dane. Generator biletu nie celowo zakupu biletów dla poszczególnych dzierżawców ostatniego zdarzenia.

1. Otwórz... \\Modułów szkoleniowych\\narzędzia\\*TicketGenerator.ps1 pokaz* w *programu PowerShell ISE*
1. Naciśnij klawisz **F5** do uruchomienia skryptu i generowania odbiorców i biletu danych sprzedaży.


### <a name="open-the-events-app-to-review-the-current-events"></a>Otwórz aplikację zdarzeń do przeglądania zdarzeń bieżącego

1. Otwórz *Centrum zdarzeń* (http://events.wtp.&lt; Użytkownik&gt;. trafficmanager.net) i kliknij przycisk **Hall porozumieniu Contoso**:

   ![centrum zdarzeń](media/saas-dbpertenant-restore-single-tenant/events-hub.png)

1. Przewiń listę zdarzeń i zanotuj ostatnie zdarzenie na liście:

   ![Ostatnie zdarzenie](media/saas-dbpertenant-restore-single-tenant/last-event.png)


### <a name="run-the-demo-scenario-to-accidentally-delete-the-last-event"></a>Uruchom scenariusz pokaz przypadkowo usunął ostatniego zdarzenia

1. Otwórz... \\Modułów szkoleniowych\\ciągłość prowadzenia działalności biznesowej i odzyskiwania po awarii\\RestoreTenant\\*RestoreTenant.ps1 pokaz* w *programu PowerShell ISE*i ustawić następującą wartość:
   * **$DemoScenario** = **1**, ustawioną **1** -Usuń zdarzenia bez sprzedaży biletów.
1. Naciśnij klawisz **F5** Uruchom skrypt i usunąć ostatniego zdarzenia. Powinien zostać wyświetlony komunikat potwierdzający podobny do następującego:

   ```Console
   Deleting unsold events from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

1. Zostanie otwarta strona zdarzenia Contoso. Przewiń w dół i sprawdź, czy zdarzenie został usunięty. Jeśli zdarzenie jest nadal na liście, kliknij przycisk Odśwież i sprawdź, czy został on usunięty.

   ![Ostatnie zdarzenie](media/saas-dbpertenant-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Przywróć bazę danych dzierżawy równolegle z produkcyjną bazę danych

Tego ćwiczenia przywraca bazę danych Hall porozumieniu Contoso do punktu w czasie, zanim zdarzenie zostało usunięte. Po usunięciu zdarzenia w poprzednich krokach chcesz odzyskać, a następnie zobacz usunięte dane. Nie trzeba przywrócić bazę danych produkcyjnych z usunięty rekord, ale należy przywrócić bazę danych starego dostęp do starych danych z innych powodów biznesowych.

 *TenantInParallel.ps1 przywracania* skrypt tworzy równoległych dzierżawy bazy danych i równoległych wpisu katalogu zarówno o nazwie *ContosoConcertHall\_starego*. Ten wzorzec przywracania jest najodpowiedniejsze do odzyskiwania danych po utracie danych drobne lub zgodności i inspekcji scenariuszy odzyskiwania. Jest również zalecane podejście gdy używasz [— replikacja geograficzna](sql-database-geo-replication-overview.md).

1. Zakończenie [symulować użytkownika przypadkowego usunięcia danych](#simulate-a-tenant-accidentally-deleting-data) sekcji.
1. Otwórz... \\Modułów szkoleniowych\\ciągłość prowadzenia działalności biznesowej i odzyskiwania po awarii\\RestoreTenant\\_RestoreTenant.ps1 pokaz_ w *programu PowerShell ISE*.
1. Ustaw **$DemoScenario** = **2**, ustaw tę wartość na **2** do *dzierżawy przywracania równolegle*.
1. Naciśnij klawisz **F5**, aby uruchomić skrypt.

Skrypt przywraca bazę danych dzierżawy (do równoległego bazy danych) do punktu w czasie, aby usunąć zdarzenie w poprzedniej sekcji. Tworzy drugi bazę danych, spowoduje usunięcie wszystkich istniejących metadanych katalogu, które istnieje w tej bazie danych i dodaje do katalogu w bazie danych *ContosoConcertHall\_starego* wpisu.

Pokaz skryptu spowoduje otwarcie strony zdarzeń w przeglądarce. Uwaga z adresu URL: ```http://events.wtp.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` czy jest to wyświetlanie danych z przywróconej bazy danych gdzie *_old* jest dodawany do nazwy.

Przywrócono przewijania zdarzenia wyświetlane w przeglądarce, aby potwierdzić, że zdarzenie usunięte w poprzedniej sekcji.

Należy pamiętać, że udostępnianie przywróconej dzierżawy jako dodatkowe dzierżawy, z własnej aplikacji zdarzeń do przeglądania biletów, jest mało prawdopodobne, aby jak może zapewnić dzierżawcy dostęp do przywrócić dane, może jednak łatwo ilustrujący wzorzec przywracania.

W rzeczywistości ma prawdopodobnie tylko zachowanie tej przywróconej bazy danych w zdefiniowanym okresie. Można usunąć wpisu przywróconej dzierżawy, po zakończeniu przez wywołanie metody *RestoredTenant.ps1 Usuń* skryptu.

1. Ustaw **$DemoScenario** do **4** wybierz *dzierżawy Usuń przywrócić* scenariusza.
1. **Wykonanie** **przy użyciu** **F5**
1. *ContosoConcertHall\_starego* wpis jest teraz usunięty z katalogu. Przejdź dalej i zamknij stronę zdarzeń dla tej dzierżawy w przeglądarce.


## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Przywróć dzierżawcy w miejscu, zastępując istniejące bazy danych dzierżawy

Tego ćwiczenia przywraca dzierżawy Hall porozumieniu Contoso do punktu w czasie, zanim zdarzenie zostało usunięte. *TenantInPlace przywracania* skryptu przywrócenie bieżącej bazy danych dzierżawy nową bazę danych, wskazując wcześniejszego punktu w czasie i usuwa oryginalnej bazy danych. Ten wzorzec przywracania najlepiej nadaje się do odzyskiwania danych poważne uszkodzenie jako może być utracie dużej ilości danych, które musi obsłużyć dzierżawcy.

1. Otwórz **RestoreTenant.ps1 pokaz** pliku w programie PowerShell ISE
1. Ustaw **$DemoScenario** do **5** wybierz *przywrócić dzierżawy w scenariuszu miejscu*.
1. Wykonywanie za pomocą **F5**.

Skrypt przywraca bazę danych dzierżawy do punktu, pięć minut, zanim zdarzenie zostało usunięte. Robi to wykonując pierwszy dzierżawy Hall porozumieniu firmy Contoso w trybie offline, więc są dostępne żadne dodatkowe aktualizacje danych. Następnie równoległych bazy danych jest tworzone przez przywrócenie z punktu przywracania i o nazwie z sygnaturą czasową w celu zapewnienia, że nazwa bazy danych nie powoduje konfliktu z istniejącą nazwą bazy danych dzierżawy. Następnie utrzymując bazę danych dzierżawy zostanie usunięta, a następnie przywrócona baza danych została zmieniona na oryginalną nazwę bazy danych. Na koniec Hall porozumieniu Contoso jest przełączony w tryb online umożliwia aplikacji dostęp do przywróconej bazy danych.

Pomyślnie przywrócono bazy danych do punktu w czasie, zanim zdarzenie zostało usunięte. Zostanie otwarta strona zdarzeń, dlatego upewnij się, przez ostatnie zdarzenie zostało przywrócone.


## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Przywróć bazę danych do bazy danych równoległej (side-by-side)
> * Przywracanie bazy danych w miejscu

[Zarządzanie dzierżawy schematu bazy danych](saas-tenancy-schema-management.md)

## <a name="additional-resources"></a>Dodatkowe zasoby

* Dodatkowe [samouczków, z którymi aplikacji Wingtip SaaS](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Omówienie ciągłości działalności biznesowej z bazy danych SQL Azure](sql-database-business-continuity.md)
* [Więcej informacji na temat tworzenia kopii zapasowych bazy danych SQL](sql-database-automated-backups.md)
