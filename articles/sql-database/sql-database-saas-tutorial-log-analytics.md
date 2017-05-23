---
title: "Używanie programu Log Analytics dla aplikacji SQL Database z wieloma dzierżawami | Microsoft Docs"
description: "Konfigurowanie i używanie programu Log Analytics (OMS) dla przykładowej aplikacji Wingtip Tickets (WTP) usługi Azure SQL Database"
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
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 4ff4519ca40f036d58f82993db78fe08aa7d5733
ms.contentlocale: pl-pl
ms.lasthandoff: 05/12/2017


---
# <a name="setup-and-use-log-analytics-oms-with-the-wtp-sample-saas-app"></a>Konfigurowanie i używanie programu Log Analytics (OMS) dla przykładowej aplikacji SaaS o nazwie WTP

W tym samouczku nauczysz się konfigurować i używać aplikację *Log Analytics ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* wraz z aplikacją WTP w celu monitorowania elastycznych pul i baz danych. Ten samouczek nawiązuje do [samouczka zarządzania i monitorowania wydajności](sql-database-saas-tutorial-performance-monitoring.md) i pokazuje, jak używać aplikacji *Log Analytics* do rozszerzania, monitorowania i zgłaszania alertów w witrynie Azure Portal. Program Log Analytics jest szczególnie przydatny w przypadku monitorowania i alertów na dużą skalę, ponieważ obsługuje setki pul i setki tysięcy baz danych. Udostępnia również jedno rozwiązanie do monitorowania, w którym można zintegrować monitorowanie różnych aplikacji i usług Azure w wielu subskrypcjach Azure.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Instalowanie i konfigurowanie programu Log Analytics (OMS)
> * Używanie programu Log Analytics do monitorowania pul i baz danych

Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Wdrożona jest aplikacja WTP. Aby wdrożyć tę aplikację w czasie krótszym niż pięć minut, zobacz [Wdrażanie i korzystanie z aplikacji SaaS o nazwie WTP](sql-database-saas-tutorial.md)
* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

W [samouczku monitorowania i zarządzania wydajnością](sql-database-saas-tutorial-performance-monitoring.md) znajdziesz omówienie wzorców i scenariuszy SaaS oraz ich wpływu na wymagania dotyczące monitorowania.

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a>Monitorowanie i zarządzanie wydajnością za pomocą programu Log Analytics (usługi OMS)

W usłudze SQL Database funkcje monitorowania i zgłaszania alertów są dostępne na poziomie baz danych i pul. Wbudowane funkcje monitorowania i zgłaszania alertów są zależne od zasobów i sprawdzają się w przypadku niewielkiej ich liczby. Są jednak mniej przydatne do monitorowania dużych instalacji lub w przypadku zapewniania centralnego widoku wielu zasobów i subskrypcji.

Programu Log Analytics można używać w scenariuszach dotyczących dużych ilości danych. Jest to osobna usługa Azure dostarczająca narzędzia analityczne do przetwarzania utworzonych dzienników diagnostycznych i danych telemetrycznych zebranych w obszarze roboczym usługi Log Analytics, który może gromadzić dane telemetryczne z wielu usług i służyć do tworzenia zapytań i ustawiania alertów. Program Log Analytics udostępnia wbudowany język zapytań oraz narzędzia do wizualizacji danych, umożliwiając analizę danych operacyjnych oraz ich wizualizację. Rozwiązanie SQL Analytics zawiera kilka wstępnie zdefiniowanych zapytań i widoków monitorowania i zgłaszania alertów w elastycznych pulach i bazach danych oraz umożliwia dodawanie własnych zapytań ad hoc i zapisywanie ich w razie potrzeby. Usługa OMS udostępnia także projektanta widoków niestandardowych.

Obszary robocze programu Log Analytics i rozwiązania analityczne można otwierać zarówno w witrynie Azure Portal, jak i w usłudze OMS. Witryna Azure Portal jest nowszym rodzajem punktu dostępu, ale w niektórych obszarach pozostaje w tyle za portalem usługi OMS.

### <a name="start-the-load-generator-to-create-data-to-analyze"></a>Uruchomienie generatora obciążenia w celu utworzenia danych przeznaczonych do analizy

1. Otwórz moduł **Demo-PerformanceMonitoringAndManagement.ps1** w programie **PowerShell ISE**. Nie zamykaj tego skryptu, jeśli w czasie pracy z samouczkiem chcesz uruchomić kilka scenariuszy generowania obciążenia.
1. Jeśli masz mniej niż pięć dzierżaw, zaaprowizuj grupę dzierżaw, aby przeprowadzić monitorowanie w ciekawszym kontekście:
   1. Ustaw zmienną **$DemoScenario = 1,** **Provision a batch of tenants** (Aprowizacja partii dzierżaw).
   1. Naciśnij klawisz **F5**, aby uruchomić skrypt.

1. Ustaw zmienną **$DemoScenario** = 2, **Generate normal intensity load (approx 40 DTU)** (Generowanie obciążenia o normalnym natężeniu (ok. 40 jednostek DTU)).
1. Naciśnij klawisz **F5**, aby uruchomić skrypt.

## <a name="get-the-wingtip-application-scripts"></a>Pobieranie skryptów aplikacji Wingtip

Skrypty i kod źródłowy aplikacji Wingtip Tickets są dostępne w repozytorium GitHub [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Pliki skryptów znajdują się w [folderze Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Pobierz folder **Learning Modules** na komputer lokalny, zachowując jego strukturę folderów.

## <a name="installing-and-configuring-log-analytics-and-the-azure-sql-analytics-solution"></a>Instalowanie i konfigurowanie programu Log Analytics i rozwiązania Azure SQL Analytics

Program Log Analytics jest oddzielną usługą, którą należy skonfigurować. Program Log Analytics gromadzi dane dziennika oraz dane telemetryczne i metryki w obszarze roboczym. Obszar roboczy jest zasobem, podobnie jak inne zasoby na platformie Azure, i musi zostać utworzony. Obszar roboczy nie musi zostać utworzony w tej samej grupie zasobów co monitorowana przez niego aplikacja, ale takie rozwiązanie jest często najwygodniejsze. W przypadku aplikacji WTP dzięki temu rozwiązaniu obszar roboczy może zostać łatwo usunięty razem z aplikacją przez zwykłe usunięcie grupy zasobów.

1. Otwórz moduł ...\\Learning Modules\\Performance Monitoring and Management\\Log Analytics\\*Demo-LogAnalytics.ps1* w programie **PowerShell ISE**.
1. Naciśnij klawisz **F5**, aby uruchomić skrypt.

W tym momencie powinno być możliwe otwarcie programu Log Analytics w witrynie Azure Portal (lub w portalu usługi OMS). Zebranie danych telemetrycznych w obszarze roboczym programu Log Analytics i ich wyświetlenie potrwa kilka minut. Im dłużej system będzie zbierał dane, tym bardziej interesujące będą wyniki. Teraz możesz zrobić sobie przerwę — upewnij się tylko, że generator obciążenia nadal działa!


## <a name="use-log-analytics-and-the-sql-analytics-solution-to-monitor-pools-and-databases"></a>Używanie programu Log Analytics i rozwiązania SQL Analytics do monitorowana pul i baz danych


W tym ćwiczeniu otwórz program Log Analytics i portal usługi OMS w celu przyjrzenia się danym telemetrycznym zbieranym dla baz i pul aplikacji WTP.

1. Przejdź do witryny [Azure Portal](https://portal.azure.com) i otwórz program Log Analytics, klikając przycisk Więcej usług, a następnie wyszukując program Log Analytics:

   ![otwieranie programu log analytics](media/sql-database-saas-tutorial-log-analytics/log-analytics-open.png)

1. Wybierz obszar roboczy o nazwie *wtploganalytics-&lt;Użytkownik&gt;*.

1. Wybierz opcję **Przegląd**, aby otworzyć rozwiązanie Log Analytics w witrynie Azure Portal.
   ![overview-link](media/sql-database-saas-tutorial-log-analytics/click-overview.png)

    **WAŻNE**: aktywowanie rozwiązania może potrwać kilka minut. Zachowaj cierpliwość.

1. Kliknij kafelek rozwiązania Azure SQL Analytics, aby je otworzyć.

    ![overview](media/sql-database-saas-tutorial-log-analytics/overview.png)

    ![analiza](media/sql-database-saas-tutorial-log-analytics/analytics.png)

1. Widok w bloku rozwiązania można przewijać na boki przy użyciu osobnego paska przewijania u dołu ekranu (odśwież blok w razie potrzeby).

1. Zapoznaj się z różnymi widokami, klikając je lub ich poszczególne zasoby, aby otworzyć eksplorator szczegółów, w którym można użyć suwaka czasu w lewej górnej części lub kliknąć pionowy pasek i skoncentrować się w na krótszym okresie. W tym widoku można wybrać poszczególne bazy danych lub pule, aby skoncentrować się na określonych zasobach:

    ![wykres](media/sql-database-saas-tutorial-log-analytics/chart.png)

1. Po powrocie do bloku rozwiązania sprawdź, że przewinięcie maksymalnie w prawo ujawnia niektóre zapisane zapytania, które można kliknąć w celu ich otwarcia i zbadania. Możesz poeksperymentować, zmieniając je i zapisując wszystkie utworzone przez siebie interesujące zapytania, które następnie będzie można otworzyć i użyć z innymi zasobami.

1. Po powrocie do bloku obszaru roboczego Log Analytics wybierz portal usługi OMS, aby otworzyć w nim rozwiązanie.

    ![oms](media/sql-database-saas-tutorial-log-analytics/oms.png)

1. W portalu usługi OMS można skonfigurować alerty. W widoku jednostek DTU bazy danych kliknij część poświęconą alertom.

1. W wyświetlonym widoku Przeszukiwanie dzienników zobaczysz wykres słupkowym przedstawiający prezentowaną metrykę.

    ![przeszukiwanie dzienników](media/sql-database-saas-tutorial-log-analytics/log-search.png)

1. Kliknięcie opcji Alert na pasku narzędzi umożliwi zobaczenie konfiguracji alertów oraz ich zmianę.

    ![dodawanie reguły alertu](media/sql-database-saas-tutorial-log-analytics/add-alert.png)

Monitorowanie i zgłaszanie alertów w programach Log Analytics i OMS opiera się na zapytaniach dla danych znajdujących się w obszarze roboczym, inaczej niż w przypadku zgłaszania alertów dla każdego bloku zasobów, które jest specyficzne dla zasobów. W związku z tym można zdefiniować alert sprawdzający wszystkie bazy danych, zamiast definiować po jednym alercie dla każdej bazy danych. Można również napisać alert, który używa złożonego zapytania dla wielu typów zasobów. Zapytania są ograniczone tylko przez ilość danych dostępnych w obszarze roboczym.

Opłaty za programy Log Analytics i SQL Database zależą od ilości danych w obszarze roboczym. W tym samouczku utworzono bezpłatny obszar roboczy, który jest ograniczony do 500 MB dziennie. Po osiągnięciu tego limitu dane przestaną być dodawane do obszaru roboczego.


## <a name="next-steps"></a>Następne kroki

W tym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Instalowanie i konfigurowanie programu Log Analytics (OMS)
> * Używanie programu Log Analytics do monitorowania pul i baz danych

[Samouczek analiz dzierżaw](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dodatkowe samouczki nawiązujące do początkowego wdrożenia aplikacji Wingtip Tickets Platform (WTP)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Program Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)

