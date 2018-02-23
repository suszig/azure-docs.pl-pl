---
title: "Używanie programu Log Analytics dla aplikacji SQL Database z wieloma dzierżawami | Microsoft Docs"
description: "Konfiguracja i analiza dzienników (OMS) za pomocą wielodostępnych aplikacji SaaS bazy danych SQL Azure"
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
ms.date: 11/13/2017
ms.author: billgib; sstein
ms.openlocfilehash: 90510520e5f6bbfa8aea4026d7437a4a4881984f
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/22/2018
---
# <a name="set-up-and-use-log-analytics-oms-with-a-multi-tenant-azure-sql-database-saas-app"></a>Konfigurowanie i używanie analizy dzienników (OMS) z wielodostępnych aplikacji SaaS bazy danych SQL Azure

W tym samouczku, konfigurowania i używania *analizy dzienników ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* monitorowania pule elastyczne i baz danych. W tym samouczku opiera się na [Samouczek zarządzania i monitorowania wydajności](saas-dbpertenant-performance-monitoring.md). Przedstawiono użycie *analizy dzienników* można rozszerzyć monitorowanie i alerty w portalu Azure. Analiza dzienników obsługuje monitorowania tysiące pule elastyczne i setkami tysięcy baz danych. Analiza dzienników udostępnia jedno rozwiązanie monitorowania, które można zintegrować monitorowanie różnych aplikacji i usług Azure, w wielu subskrypcji platformy Azure.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Instalowanie i konfigurowanie programu Log Analytics (OMS)
> * Używanie programu Log Analytics do monitorowania pul i baz danych

Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Aplikacja Wingtip biletów SaaS bazy danych dla dzierżawy jest wdrażana. Aby wdrożyć w mniej niż 5 minut, zobacz [Wdróż i eksplorowanie aplikacji Wingtip biletów SaaS bazy danych dla dzierżawcy](saas-dbpertenant-get-started-deploy.md)
* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

W [samouczku monitorowania i zarządzania wydajnością](saas-dbpertenant-performance-monitoring.md) znajdziesz omówienie wzorców i scenariuszy SaaS oraz ich wpływu na wymagania dotyczące monitorowania.

## <a name="monitoring-and-managing-database-and-elastic-pool-performance-with-log-analytics-or-operations-management-suite-oms"></a>Monitorowanie i zarządzanie wydajnością puli bazy danych i elastyczna z analizy dzienników lub Operations Management Suite (OMS)

Baza danych SQL monitorowanie i alerty jest dostępna dla baz danych i pul w portalu Azure. Ten wbudowaną funkcję monitorowania i alertów jest wygodne, ale stanowi określonych zasobów, mniej dobrze nadaje się do monitorowania duże instalacje lub zapewniające ujednoliconego podglądu między zasobami i subskrypcje.

W przypadku scenariuszy dużych analizy dzienników może służyć do monitorowania i alertów. Analiza dzienników jest oddzielny usługa Azure, która umożliwia analytics przez dzienniki diagnostyczne i dane telemetryczne, które są zbierane w obszarze roboczym z potencjalnie wiele usług. Analiza dzienników zawiera wbudowaną kwerendę języka i danych narzędzi wizualizacji, stosowanie analizy danych operacyjnych. Rozwiązania analizy SQL zawiera kilka wstępnie zdefiniowanych puli elastycznej i bazy danych monitorowania i alertów widoków i zapytań. Usługa OMS udostępnia także projektanta widoków niestandardowych.

Obszary robocze programu Log Analytics i rozwiązania analityczne można otwierać zarówno w witrynie Azure Portal, jak i w usłudze OMS. Witryna Azure Portal jest nowszym rodzajem punktu dostępu, ale w niektórych obszarach pozostaje w tyle za portalem usługi OMS.

### <a name="create-performance-diagnostic-data-by-simulating-a-workload-on-your-tenants"></a>Tworzenie danych diagnostycznych wydajności symulując obciążenie dzierżawców 

1. W **PowerShell ISE**, Otwórz *... \\WingtipTicketsSaaS MultiTenantDb wzorca\\modułów szkoleniowych\\zarządzania i monitorowania wydajności\\** PerformanceMonitoringAndManagement.ps1*** demonstracyjnej. Nie zamykaj tego skryptu, jeśli w czasie pracy z samouczkiem chcesz uruchomić kilka scenariuszy generowania obciążenia.
1. Jeśli jeszcze tego nie zrobiono, należy udostępnić partii dzierżawcy, aby zapewnić bardziej interesującego kontekstu monitorowania. Trwa to kilka minut:
   1. Ustaw **$DemoScenario = 1**, _udostępnić partii dzierżawcy_
   1. Aby uruchomić skrypt i wdrożyć dodatkowe dzierżaw 17, naciśnij klawisz **F5**.  

1. Teraz uruchomić generatora obciążenia, aby uruchomić symulowanym obciążeniem na wszystkich dzierżawców.  
    1. Ustaw **$DemoScenario = 2**, _Generuj normalnym natężeniu obciążenia (średnio 30 DTU)_.
    1. Aby uruchomić skrypt, naciśnij klawisz **F5**.

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Pobierz skrypty aplikacji Wingtip biletów SaaS bazy danych dla dzierżawcy

Skrypty Wingtip biletów SaaS wielodostępne w bazie danych i kodu źródłowego aplikacji są dostępne w [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) repozytorium GitHub. Zapoznaj się z [ogólne wskazówki](saas-tenancy-wingtip-app-guidance-tips.md) kroki pobrać i odblokować skryptów PowerShell biletów Wingtip.

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a>Instalowanie i konfigurowanie programu Log Analytics i rozwiązania Azure SQL Analytics

Program Log Analytics jest oddzielną usługą, którą należy skonfigurować. Analiza dzienników umożliwia zbieranie danych dziennika, telemetrii i metryki w obszarze roboczym analizy dzienników. Obszar roboczy analizy dzienników jest zasobem, podobnie jak w innych zasobów na platformie Azure i musi zostać utworzony. Gdy obszar roboczy nie muszą zostać utworzone w tej samej grupie zasobów co aplikacje monitoruje, sposób często sprawia, że najbardziej odpowiednie. Dla aplikacji biletów Wingtip przy użyciu pojedynczej grupy zasobów gwarantuje, że obszar roboczy jest usuwany z aplikacją.

1. W **PowerShell ISE**, Otwórz *... \\WingtipTicketsSaaS MultiTenantDb wzorca\\modułów szkoleniowych\\zarządzania i monitorowania wydajności\\dziennika analizy\\** LogAnalytics.ps1*** demonstracyjnej.
1. Aby uruchomić skrypt, naciśnij klawisz **F5**.

W tym momencie powinno być możliwe Otwórz Log Analytics w portalu Azure (lub portalu OMS). Trwa kilka minut, aż telemetrii mają być zbierane w obszarze roboczym analizy dzienników i stają się widoczne. Już pozostanie system zbierania danych diagnostycznych jest bardziej interesującego środowisko. Teraz możesz zrobić sobie przerwę — upewnij się tylko, że generator obciążenia nadal działa!

## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Używanie programu Log Analytics i rozwiązania SQL Analytics do monitorowana pul i baz danych


W tym ćwiczeniu Otwórz analizy dzienników i portalu OMS, aby przyjrzeć się telemetrii zbierana dla baz danych i pul.

1. Przejdź do [portalu Azure](https://portal.azure.com) i otworzyć, klikając analizy dzienników **wszystkich usług**, następnie wyszukaj analizy dzienników:

   ![otwieranie programu log analytics](media/saas-dbpertenant-log-analytics/log-analytics-open.png)

1. Wybierz obszar roboczy o nazwie _wtploganalytics -&lt;użytkownika&gt;_.

1. Wybierz opcję **Przegląd**, aby otworzyć rozwiązanie Log Analytics w witrynie Azure Portal.

   ![overview-link](media/saas-dbpertenant-log-analytics/click-overview.png)

    > [!IMPORTANT]
    > Może upłynąć kilka minut, zanim rozwiązanie będzie aktywny. Zachowaj cierpliwość.

1. Kliknij kafelek rozwiązania Azure SQL Analytics, aby je otworzyć.

    ![overview](media/saas-dbpertenant-log-analytics/overview.png)

    ![analiza](media/saas-dbpertenant-log-analytics/log-analytics-overview.png)

1. Widoki w rozwiązaniu bok, przewijane wraz z ich własnych paska przewijania wewnętrzny u dołu (Odśwież stronę, jeśli to konieczne).

1. Poznaj strony podsumowania, klikając na kafelkach lub poszczególne bazy danych, aby otworzyć Eksploratora Przechodzenie do szczegółów.

1. Zmień ustawienie, aby zmodyfikować zakres czasu - dla tego samouczka wybierz filtr _ostatni 1 godzina_

    ![Filtr czasu](media/saas-dbpertenant-log-analytics/log-analytics-time-filter.png)

1. Wybierz pojedynczej bazy danych, aby zapoznać się z użycia zapytania i metryki dla tej bazy danych.

    ![Analiza bazy danych](media/saas-dbpertenant-log-analytics/log-analytics-database.png)

1. Aby zobaczyć użycie metryki przewiń stronę analytics z prawej strony.
 
     ![metryki bazy danych](media/saas-dbpertenant-log-analytics/log-analytics-database-metrics.png)

1. Przewiń strony analytics po lewej stronie i kliknij na kafelku serwera na liście informacje o zasobie. Spowoduje to otwarcie strony przedstawiający pul i baz danych na serwerze. 

     ![informacje o zasobie](media/saas-dbpertenant-log-analytics/log-analytics-resource-info.png)

 
     ![Serwer z puli i baz danych](media/saas-dbpertenant-log-analytics/log-analytics-server.png)

1. Na serwerze strony, którego kliknięcie spowoduje otwarcie, który zawiera pul i baz danych na serwerze, kliknij w puli.  Na stronie puli przewiń w prawo, aby widzieć metryki puli.  

     ![metryki puli](media/saas-dbpertenant-log-analytics/log-analytics-pool-metrics.png)



1. Powrót do obszaru roboczego analizy dzienników, wybierz **portalu OMS** do Otwórz obszar roboczy.

    ![oms](media/saas-dbpertenant-log-analytics/log-analytics-workspace-oms-portal.png)

W portalu OMS można eksplorować dane dziennika i metryki w obszarze roboczym dalej.  

Monitorowanie i alerty analizy dzienników i OMS na podstawie kwerend danych w obszarze roboczym, w odróżnieniu od alerty zdefiniowane na każdy zasób w portalu Azure. Użycie alerty dla zapytań, można określić pojedynczy alert, która wygląda przez wszystkie bazy danych, a nie definiującego po jednym dla każdej bazy danych. Zapytania są ograniczone tylko przez ilość danych dostępnych w obszarze roboczym.

Aby uzyskać więcej informacji na temat używania OMS zapytań, Ustaw alerty Zobacz, [Praca z reguły alertów w analizy dzienników](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-alerts-creating).

Opłaty za programy Log Analytics i SQL Database zależą od ilości danych w obszarze roboczym. W tym samouczku utworzono obszar roboczy wolne, która jest ograniczona do 500 MB na dzień. Po osiągnięciu tego limitu danych jest już dodany do obszaru roboczego.


## <a name="next-steps"></a>Kolejne kroki

W tym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Instalowanie i konfigurowanie programu Log Analytics (OMS)
> * Używanie programu Log Analytics do monitorowania pul i baz danych

[Samouczek analiz dzierżaw](saas-dbpertenant-log-analytics.md)

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Dodatkowe samouczki, które zależą od momentu pierwszego wdrożenia aplikacji Wingtip biletów SaaS bazy danych dla dzierżawcy](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Program Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
