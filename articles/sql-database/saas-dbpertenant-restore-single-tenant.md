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
ms.openlocfilehash: 46471073f88247510f45d6c4152afa43be6e1aaa
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/22/2018
---
# <a name="restore-a-single-tenant-with-a-database-per-tenant-saas-application"></a>Przywracanie pojedynczej dzierżawy z bazy danych dla każdego dzierżawcy aplikacji SaaS

Bazy danych na sprawia, że model dzierżawy jest łatwo przywrócić pojedynczej dzierżawy do wcześniejszego punktu w czasie bez wpływu na innych dzierżawców.

W tym samouczku Dowiedz się dwa wzorce odzyskiwania danych:

> [!div class="checklist"]

> * Przywróć bazę danych do bazy danych równoległej (side-by-side)
> * Przywracanie bazy danych w miejscu, zastępując istniejące bazy danych


|||
|:--|:--|
| **Przywracanie do równoległego bazy danych** | Ten wzorzec może służyć do przeglądu, inspekcji, zgodności itd. Aby umożliwić dzierżawcę, aby sprawdzić ich dane z wcześniejszego punktu.  Dzierżawcy bieżąca baza danych pozostaje bez zmian i online. |
| **Przywracanie w miejscu** | Ten wzorzec jest zwykle używane do odzyskania dzierżawy do wcześniejszego punktu, po dzierżawcy przypadkowo usuwa lub uszkodzi danych. Oryginalnej bazy danych jest przełączona w tryb offline i zastąpione przywróconej bazy danych. |
|||

Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Aplikacja Wingtip SaaS jest wdrażana. Aby wdrożyć w mniej niż 5 minut, zobacz [wdrażania i aplikacji Wingtip SaaS](saas-dbpertenant-get-started-deploy.md)
* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-saas-tenant-restore-patterns"></a>Wprowadzenie do wzorców przywracania dzierżawy SaaS

Istnieją dwa proste wzorce w celu przywrócenia danych dzierżawy usługi. Ponieważ dzierżawy baz danych są od siebie odizolowane, Przywracanie jednego dzierżawcy nie ma wpływu na dane żadnych innych dzierżawców.  Funkcja (PITR) punkt w czasie przywracania bazy danych SQL jest używana w oba wzorce.  PITR zawsze tworzy nową bazę danych.   

We wzorcu pierwszy **przywrócić równolegle**, równoległe nowej bazy danych jest tworzony obok dzierżawcy bieżącej bazy danych. Dzierżawca następnie otrzymuje dostęp tylko do odczytu do przywróconej bazy danych. Przywróconych danych można przejrzeć i potencjalnie pozwala zastąpić bieżące wartości danych. To projektanta aplikacji, aby określić sposób dzierżawcy uzyskuje dostęp do przywróconej bazy danych i jakie opcje odzyskiwania są udostępniane. Po prostu stosowanie dzierżawy przejrzeć swoje dane na wcześniejszą może być wszystkie, który jest wymagany w niektórych scenariuszach. 

Drugi wzorzec **przywracania w miejscu**, jest przydatne, jeśli dane zostały utracone lub uszkodzony i dzierżawcy chce przywrócenie do wcześniejszego punktu.  Dzierżawcy do trybu offline podczas przywróceniu bazy danych. Oryginalnej bazy danych zostanie usunięta, a następnie przywrócona baza danych zostanie zmieniona. Łańcuch kopii zapasowych oryginalnej bazy danych, pozostaje dostępne po usunięciu, co umożliwia przywracanie bazy danych do wcześniejszego punktu w czasie, w razie potrzeby.

Jeśli baza danych używa [— replikacja geograficzna](sql-database-geo-replication-overview.md) i przywracanie równolegle, zaleca się skopiowanie wszelkie wymagane dane z przywróconej kopiowanie do oryginalnej bazy danych. Można zastąpić oryginalnej bazy danych przywróconej bazy danych, musisz skonfigurować ponownie i przeprowadź ponowną synchronizację — replikacja geograficzna.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Pobierz skrypty aplikacji Wingtip biletów SaaS bazy danych dla dzierżawcy

Skrypty Wingtip biletów SaaS wielodostępne w bazie danych i kodu źródłowego aplikacji są dostępne w [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) repozytorium GitHub. Zapoznaj się z [ogólne wskazówki](saas-tenancy-wingtip-app-guidance-tips.md) dla czynności, aby pobrać i odblokować skrypty Wingtip biletów SaaS.

## <a name="before-you-start"></a>Przed rozpoczęciem

Po utworzeniu bazy danych może potrwać 10 – 15 minut, zanim będzie można przywrócić z pierwsza pełna kopia zapasowa.  Jeśli zainstalowano tylko aplikację, która może być konieczne Poczekaj kilka minut przed podjęciem próby tego scenariusza.

## <a name="simulate-a-tenant-accidentally-deleting-data"></a>Symulowanie dzierżawcy przypadkowego usunięcia danych

Aby zademonstrować tych scenariuszy odzyskiwania najpierw *"przypadkowo"* usunąć zdarzenia w jednej z baz danych dzierżawy. 

### <a name="open-the-events-app-to-review-the-current-events"></a>Otwórz aplikację zdarzeń do przeglądania zdarzeń bieżącego

1. Otwórz *Centrum zdarzeń* (http://events.wtp.&lt; Użytkownik&gt;. trafficmanager.net) i kliknij przycisk **Hall porozumieniu Contoso**:

   ![centrum zdarzeń](media/saas-dbpertenant-restore-single-tenant/events-hub.png)

1. Przewiń listę zdarzeń i zanotuj ostatnie zdarzenie na liście:

   ![Ostatnie zdarzenie](media/saas-dbpertenant-restore-single-tenant/last-event.png)


### <a name="accidentally-delete-the-last-event"></a>"Przypadkowo" Usuń ostatnie zdarzenie

1. Otwórz... \\Modułów szkoleniowych\\ciągłość prowadzenia działalności biznesowej i odzyskiwania po awarii\\RestoreTenant\\*RestoreTenant.ps1 pokaz* w *programu PowerShell ISE*i ustawić następującą wartość:
   * **$DemoScenario** = **1**, Usuń ostatnie zdarzenie (ze sprzedaży biletów).
1. Naciśnij klawisz **F5** Uruchom skrypt i usunąć ostatniego zdarzenia. Powinien zostać wyświetlony komunikat potwierdzenia followng:

   ```Console
   Deleting last unsold event from Contoso Concert Hall ...
   Deleted event 'Seriously Strauss' from Contoso Concert Hall venue.
   ```

1. Zostanie otwarta strona zdarzenia Contoso. Przewiń w dół i sprawdź, czy zdarzenie został usunięty. Jeśli zdarzenie jest nadal na liście, kliknij przycisk Odśwież i sprawdź, czy został on usunięty.

   ![Ostatnie zdarzenie](media/saas-dbpertenant-restore-single-tenant/last-event-deleted.png)


## <a name="restore-a-tenant-database-in-parallel-with-the-production-database"></a>Przywróć bazę danych dzierżawy równolegle z produkcyjną bazę danych

Tego ćwiczenia przywraca bazę danych Hall porozumieniu Contoso do punktu w czasie, zanim zdarzenie zostało usunięte. W tym scenariuszu przyjęto, że ma zostać Przejrzyj usunięte dane w bazie danych równoległych.

 *TenantInParallel.ps1 przywracania* skrypt tworzy równoległych dzierżawy bazy danych o nazwie *ContosoConcertHall\_starego*, z wpisu katalogu równoległych. Ten wzorzec przywracania najlepiej nadaje się do odzyskiwania danych po utracie danych pomocniczych, lub jeśli chcesz przejrzeć dane dotyczące zgodności i inspekcji. Jest również zalecane podejście gdy używasz [— replikacja geograficzna](sql-database-geo-replication-overview.md).

1. Zakończenie [symulować dzierżawcy przypadkowego usunięcia danych](#simulate-a-tenant-accidentally-deleting-data) sekcji.
1. W *PowerShell ISE*, Otwórz... \\Modułów szkoleniowych\\ciągłość prowadzenia działalności biznesowej i odzyskiwania po awarii\\RestoreTenant\\_RestoreTenant.ps1 pokaz_.
1. Ustaw **$DemoScenario** = **2**, *dzierżawy przywracania równolegle*.
1. Naciśnij klawisz **F5**, aby uruchomić skrypt.

Skrypt przywraca bazę danych dzierżawy do punktu w czasie, aby usunąć zdarzenia. Przywróceniu bazy danych do nowej bazy danych o nazwie _ContosoConcertHall\_starego_. Metadane katalogu, w tym przywróconej bazy danych zostanie usunięta, a następnie bazy danych jest dodawany do katalogu przy użyciu klucza utworzone na podstawie *ContosoConcertHall\_starego* nazwy.

Pokaz skryptu spowoduje otwarcie strony zdarzeń dla tej nowej bazy danych dzierżawy w przeglądarce. Uwaga z adresu URL: ```http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/contosoconcerthall_old``` że ta strona jest wyświetlanie danych z przywróconej bazy danych gdzie *_old* jest dodawany do nazwy.

Przywrócono przewijania zdarzenia wyświetlane w przeglądarce, aby potwierdzić, że zdarzenie usunięte w poprzedniej sekcji.

Należy pamiętać, że udostępnianie przywróconej dzierżawy jako dodatkowe dzierżawy, z własnej aplikacji zdarzeń jest mało prawdopodobne, aby jak może zapewnić dzierżawcy dostęp do przywrócić dane, może jednak w celu zilustrowania wzorzec przywracania. W rzeczywistości będzie prawdopodobnie zapewniają dostęp tylko do odczytu do starych danych i zachować tej przywróconej bazy danych w zdefiniowanym okresie. W przykładzie można usunąć wpis przywróconej dzierżawy, po zakończeniu przez uruchomienie _Usuń przywrócić dzierżawy_ scenariusza.

1. Ustaw **$DemoScenario** = **4**, *Usuń przywrócić dzierżawy*
1. **Wykonanie** **przy użyciu** **F5**
1. *ContosoConcertHall\_starego* wpis jest teraz usunięty z katalogu. Przejdź dalej i zamknij stronę zdarzeń dla tej dzierżawy w przeglądarce.


## <a name="restore-a-tenant-in-place-replacing-the-existing-tenant-database"></a>Przywróć dzierżawcy w miejscu, zastępując istniejące bazy danych dzierżawy

Tego ćwiczenia przywraca dzierżawy Hall porozumieniu Contoso do punktu, zanim zdarzenie zostało usunięte. *TenantInPlace przywracania* skryptu przywrócenie bazy danych dzierżawy nową bazę danych i usuwa oryginalną.  Ten wzorzec przywracania najlepiej nadaje się do odzyskiwania danych poważne uszkodzenie jako może być utracie dużej ilości danych, które musi obsłużyć dzierżawcy.

1. Otwórz **RestoreTenant.ps1 pokaz** pliku w programie PowerShell ISE
1. Ustaw **$DemoScenario** = **5**, *dzierżawy przywracania w miejscu*.
1. Wykonywanie za pomocą **F5**.

Skrypt przywraca bazę danych dzierżawy do punktu przed zdarzenie zostało usunięte. Przyjmuje najpierw dzierżawcy Hall porozumieniu firmy Contoso w trybie offline, aby uniemożliwić dalsze aktualizacje. Równoległe bazy danych jest tworzona przez przywrócenie z punktu przywracania.  Przywrócona baza danych ma nazwę z sygnaturą czasową do upewnij się, że nazwa bazy danych nie powoduje konfliktu z istniejącą nazwą bazy danych dzierżawy. Następnie utrzymując bazę danych dzierżawy zostanie usunięta, a następnie przywrócona baza danych została zmieniona na oryginalną nazwę bazy danych. Na koniec Hall porozumieniu Contoso jest przełączony w tryb online umożliwia aplikacji dostęp do przywróconej bazy danych.

Pomyślnie przywrócono bazy danych do punktu w czasie, zanim zdarzenie zostało usunięte. Zostanie otwarta strona zdarzeń, dlatego upewnij się, przez ostatnie zdarzenie zostało przywrócone.

Należy pamiętać, że po przywróceniu bazy danych potrwa dalsze 10 – 15 minut przed pierwszym pełna kopia jest dostępne do przywrócenia z ponownie. 

## <a name="next-steps"></a>Kolejne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Przywróć bazę danych do bazy danych równoległej (side-by-side)
> * Przywracanie bazy danych w miejscu

[Zarządzanie dzierżawy schematu bazy danych](saas-tenancy-schema-management.md)

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Dodatkowe samouczki, które zależą od aplikacji Wingtip SaaS](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Omówienie ciągłości działalności biznesowej z bazy danych SQL Azure](sql-database-business-continuity.md)
* [Więcej informacji na temat tworzenia kopii zapasowych bazy danych SQL](sql-database-automated-backups.md)
