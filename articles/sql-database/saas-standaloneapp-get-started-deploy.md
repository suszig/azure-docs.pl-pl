---
title: "Samouczek SaaS wielodostępne — baza danych SQL Azure | Dokumentacja firmy Microsoft"
description: "Wdrażanie i Eksploruj autonomicznej aplikacji SaaS pojedynczej dzierżawy, który używa bazy danych SQL Azure."
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
ms.date: 11/14/2017
ms.author: sstein
ms.openlocfilehash: 2ca290dfcb23215ffd727500e76076ae8b14fa6f
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Wdrażanie i Eksploruj aplikacja pojedynczej dzierżawy autonomiczna, która używa bazy danych SQL Azure

W tym samouczku wdrażanie i Eksploruj aplikacja autonomiczna Wingtip biletów SaaS. Aplikacja jest przeznaczona do pokazują funkcje bazy danych SQL Azure, upraszczające włączania scenariusze SaaS.

Wzorzec aplikacja autonomiczna wdraża grupy zasobów platformy Azure zawierającą aplikacji pojedynczej dzierżawy i pojedynczej dzierżawy bazy danych dla każdego dzierżawcy.  Aby zapewnić rozwiązanie dla wielu dzierżawców można zainicjować obsługi administracyjnej wiele wystąpień aplikacji.

A w tym samouczku zostaną wdrożone grupy zasobów dla wielu dzierżawców w ramach subskrypcji platformy Azure, ten wzorzec umożliwia grup zasobów do wdrożenia w subskrypcji platformy Azure dzierżawcy.  Platforma Azure ma programy partnera, zezwalających na te grupy zasobów, mają być zarządzane przez dostawcę usług w imieniu dzierżawcy jako administrator w subskrypcji dzierżawcy.

W poniższej sekcji wdrożenia istnieją trzy Wdróż do przycisków Azure, z których każdy wdraża inne wystąpienie aplikacji dostosowywane do określonych dzierżawy. Po naciśnięciu przycisku odpowiednia aplikacja jest pełni wdrożone pięciu minut.  Aplikacje są wdrażane w ramach subskrypcji platformy Azure.  Masz pełny dostęp do eksplorowania i pracować z poszczególnych składników aplikacji.

Skrypty zarządzania i kodu źródłowego aplikacji są dostępne w [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) repozytorium GitHub.


Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Wdrażanie aplikacji autonomicznej Wingtip biletów SaaS
> * Skąd uzyskać kodu źródłowego aplikacji i skrypty zarządzania
> * Temat serwerów i baz danych, które tworzą aplikacji

Dodatkowe samouczki zostaną wydane w odpowiednim czasie będzie umożliwiające Eksploruj zakresu scenariuszy zarządzania na podstawie wzorca tej aplikacji.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>Wdrażanie aplikacji SaaS autonomiczny biletów Wingtip

Wdrażanie aplikacji dla trzech dzierżaw podana:

1. Kliknij każdą **wdrażanie na platformie Azure** przycisk, aby otworzyć szablon wdrożenia w portalu Azure. Każdy szablon wymaga dwóch wartości parametru; Nazwa nowej grupy zasobów i nazwę użytkownika, która odróżnia to wdrożenie od innych wdrożeń aplikacji. Następnym krokiem szczegółowe informacje dotyczące ustawiania tych wartości.<br><br>
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>     &nbsp;&nbsp;**Hall porozumieniu firmy Contoso**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; &nbsp; **Dojo dereń**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>    &nbsp;&nbsp;**Klub Fabrikam Jazz**

2. Wprowadź wartości wymaganego parametru dla każdego wdrożenia.

    > [!IMPORTANT]
    > Niektóre uwierzytelnienia i zapory serwera są celowo niezabezpieczone w celach demonstracyjnych. **Utwórz nową grupę zasobów** dla każdego wdrożenia aplikacji.  Nie należy używać istniejącej grupy zasobów. Nie należy używać tej aplikacji lub wszystkie zasoby, które tworzy, w środowisku produkcyjnym. Po zakończeniu pracy z aplikacjami, aby zatrzymać rozliczeń powiązanego, należy usunąć wszystkie grupy zasobów.

    Najlepiej użyć tylko małe litery, cyfry i łączniki w nazwach zasobów.
    * Aby uzyskać **grupy zasobów** — wybierz opcję Utwórz nowy, a następnie podaj nazwę grupy zasobów (z uwzględnieniem wielkości liter).
        * Zaleca się, że wszystkie litery w Nazwa grupy zasobów można małe litery.
        * Firma Microsoft zaleca dołączanie kreska, następuje inicjały, a następnie cyfrę: na przykład _wingtip-sa-af1_.
        * Wybierz lokalizację z listy rozwijanej.

    * Dla **użytkownika** — zaleca się wybrać krótszej wartości użytkownika, takie jak inicjały plus cyfrę: na przykład _af1_.


1. **Wdróż aplikację**.

    * Kliknij, aby zaakceptować warunki i postanowienia.
    * Kliknij pozycję **Kup**.

1. Monitorować stan wdrożenia wszystkich wdrożeń trzy klikając **powiadomienia** (ikonę dzwonka prawej strony pola wyszukiwania). Wdrażanie aplikacji trwa około pięciu minut.


## <a name="run-the-application"></a>Uruchamianie aplikacji

Miejsc, takich jak koncertowe, jazz klubów i sportowych, który hostował zdarzeń aplikacji, których są wyświetlane. Jako klientów (lub dzierżawcy) biletów Wingtip zarejestrować miejsc w prosty sposób sprzedawać bilety i Wyświetl listę zdarzeń. Każda właściwość pobiera spersonalizowane witryny sieci web do zarządzania i wyświetlić ich zdarzenia i sprzedawać bilety, niezależne i odizolowany od pozostałych dzierżawców. W obszarze obejmuje każdy dzierżawca pobiera wystąpienia oddzielne aplikacji i autonomiczna baza danych SQL.

1. Otwórz stronę zdarzeń dla każdego z trzech dzierżaw w osobnych kartach przeglądarki.

    http://events.contosoconcerthall. &lt;Użytkownika&gt;. trafficmanager.net <br>
    http://events.dogwooddojo. &lt;Użytkownika&gt;. trafficmanager.net<br>
    http://events.fabrikamjazzclub. &lt;Użytkownika&gt;. trafficmanager.net

    (Zastąp &lt;użytkownika&gt; z wartością użytkownika danego wdrożenia).

   ![Zdarzenia](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)


Aby kontrolować dystrybucję żądań przychodzących, aplikacja używa [ *usługi Azure Traffic Manager*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview). Każda aplikacja specyficznego dla dzierżawy zawiera nazwę dzierżawy jako część nazwy domeny w adresie URL. Wszystkie dzierżawy adresów URL obejmują konkretnej *użytkownika* wartości i odpowiada temu formatowi: http://events.&lt; venuename&gt;.&lt; Użytkownik&gt;. trafficmanager.net. Lokalizacja bazy danych z każdej dzierżawy znajduje się w ustawieniach aplikacji w odpowiednich wdrożonej aplikacji.

W środowisku produkcyjnym należy zwykle utworzyć rekord CNAME DNS [ *punktu firmowej domeny internetowej* ](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) na adres URL profilu Menedżera ruchu.


## <a name="explore-the-servers-and-tenant-databases"></a>Eksploruj serwerów i baz danych dzierżawy

Oto niektóre zasoby, które zostały wdrożone:

1. W [portalu Azure](http://portal.azure.com), przejdź do listy grup zasobów.  Powinna zostać wyświetlona **wingtip-sa — katalogu —&lt;użytkownika&gt;**  grupę zasobów, w której **katalogu-sa -&lt;użytkownika&gt;**  serwer jest wdrażany z **tenantcatalog** bazy danych. Należy również sprawdzić dzierżawy trzech grup zasobów.

1. Otwórz **wingtip-sa-fabrikam -&lt;użytkownika&gt;**  grupy zasobów, która zawiera zasoby wdrożenia klub Jazz firmy Fabrikam.  **Fabrikamjazzclub -&lt;użytkownika&gt;**  serwer zawiera **fabrikamjazzclub** bazy danych.


Każda baza danych dzierżawy jest 50 DTU _autonomiczny_ bazy danych.

## <a name="next-steps"></a>Następne kroki

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Wdrażanie aplikacji autonomicznej Wingtip biletów SaaS
> * Temat serwerów i baz danych, które tworzą aplikacji
> * Usuwanie przykładowych zasobów w celu zakończenia ich rozliczania


## <a name="additional-resources"></a>Dodatkowe zasoby

<!--* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview) -->
* Aby dowiedzieć się więcej o wielodostępnych aplikacjach SaaS, zobacz [ *Design patterns for multi-tenant SaaS applications*](saas-tenancy-app-design-patterns.md) (Wzorce projektu dla wielodostępnej aplikacji SaaS)
