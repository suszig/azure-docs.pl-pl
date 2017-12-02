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
ms.date: 11/30/2017
ms.author: genemi
ms.openlocfilehash: d38cd108821bce05824732bbdbdd322ae8563bde
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Wdrażanie i Eksploruj aplikacja pojedynczej dzierżawy autonomiczna, która używa bazy danych SQL Azure

W tym samouczku wdrażanie i Eksploruj aplikacja autonomiczna Wingtip biletów SaaS. Aplikacja jest przeznaczona do pokazują funkcje bazy danych SQL Azure, upraszczające włączania scenariusze SaaS.

Wzorzec aplikacja autonomiczna wdraża grupy zasobów platformy Azure zawierającą aplikacji pojedynczej dzierżawy i pojedynczej dzierżawy bazy danych dla każdego dzierżawcy.  Aby zapewnić rozwiązanie dla wielu dzierżawców można zainicjować obsługi administracyjnej wiele wystąpień aplikacji.

W tym samouczku wdrażania grup zasobów dla wielu dzierżawców w ramach subskrypcji platformy Azure.  Ten wzorzec umożliwia grup zasobów do wdrożenia w subskrypcji platformy Azure dzierżawcy. Platforma Azure ma programy partnera, zezwalających na te grupy zasobów, mają być zarządzane przez dostawcę usług w imieniu dzierżawcy. Dostawca usług to administrator w subskrypcji dzierżawcy.

W dalszej części artykułu wdrożenia, istnieją trzy niebieski **wdrażanie na platformie Azure** przycisków. Każdy przycisk wdraża inne wystąpienie aplikacji. Każde wystąpienie jest dostosowany do określonych dzierżawy. Po naciśnięciu przycisku odpowiednią aplikację pełni jest wdrażana w ciągu pięciu minut.  Aplikacje są wdrażane w ramach subskrypcji platformy Azure.  Masz pełny dostęp do eksplorowania i pracować z poszczególnych składników aplikacji.

Skrypty zarządzania i kodu źródłowego aplikacji są dostępne w [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) repozytorium GitHub.


Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Jak wdrożyć aplikacji autonomicznej Wingtip biletów SaaS.
> * Gdzie można pobrać kodu źródłowego aplikacji i skrypty zarządzania.
> * Temat serwerów i baz danych, które tworzą aplikacji.

Dodatkowe samouczki będzie publikowana. Umożliwi ich Eksploruj zakresu scenariuszy zarządzania na podstawie wzorca tej aplikacji.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>Wdrażanie aplikacji SaaS autonomiczny biletów Wingtip

Wdrażanie aplikacji dla trzech dzierżaw podana:

1. Kliknij każdy blue **wdrażanie na platformie Azure** przycisk, aby otworzyć szablon wdrożenia w [portalu Azure](https://portal.azure.com). Każdy szablon wymaga dwóch wartości parametru; Nazwa nowej grupy zasobów i nazwę użytkownika, która odróżnia to wdrożenie od innych wdrożeń aplikacji. Następnym krokiem szczegółowe informacje dotyczące ustawiania tych wartości.<br><br>
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Hall porozumieniu firmy Contoso**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Dojo dereń**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Klub Fabrikam Jazz**

2. Wprowadź wartości wymaganego parametru dla każdego wdrożenia.

    > [!IMPORTANT]
    > Niektóre zapory uwierzytelniania i serwera są celowo niezabezpieczona w celach demonstracyjnych. **Utwórz nową grupę zasobów** dla każdego wdrożenia aplikacji.  Nie należy używać istniejącej grupy zasobów. Nie należy używać tej aplikacji lub wszystkie zasoby, które tworzy, w środowisku produkcyjnym. Po zakończeniu pracy z aplikacjami, aby zatrzymać rozliczeń powiązanego, należy usunąć wszystkie grupy zasobów.

    Najlepiej użyć tylko małe litery, cyfry i łączniki w nazwach zasobów.
    * Dla **grupy zasobów** — wybierz tę opcję **Utwórz nowy**, a następnie podaj jedną małą literę **nazwa** dla grupy zasobów.
        * Firma Microsoft zaleca dołączanie kreska, następuje inicjały, a następnie cyfrę: na przykład *wingtip-sa-af1*.
        * Wybierz **lokalizacji** z listy rozwijanej.

    * Dla **użytkownika** -firma Microsoft zaleca, wybierz wartość krótkich użytkownika, takie jak inicjały plus cyfrę: na przykład *af1*.


3. **Wdróż aplikację**.

    * Kliknij, aby zaakceptować warunki i postanowienia.
    * Kliknij pozycję **Kup**.

4. Monitorować stan wdrożenia wszystkich wdrożeń trzy klikając **powiadomienia** (ikonę dzwonka z prawej strony pola wyszukiwania). Wdrażanie aplikacji trwa pięć minut.


## <a name="run-the-application"></a>Uruchamianie aplikacji

Miejsc, które hostują zdarzeń aplikacji, których są wyświetlane. Właściwość typy koncertowe, jazz klubów i sportowych. Miejsc są klientami aplikacji Wingtip biletów. W biletów Wingtip miejsc są rejestrowane jako *dzierżawców*. Trwa dzierżawcy udostępnia miejsce łatwy sposób listy zdarzeń i sprzedawać bilety klientom. Każda właściwość pobiera spersonalizowane witryny sieci web, aby wyświetlić ich zdarzenia i sprzedawać bilety. Każdy dzierżawca jest odizolowany od pozostałych dzierżawców i jest niezależna od nich. W obszarze obejmuje każdego dzierżawcy pobiera wystąpienie innej aplikacji z własnych autonomiczna baza danych SQL.

1. Otwórz stronę zdarzeń dla każdego z trzech dzierżaw w osobnych kartach przeglądarki.

    - http://events.contosoconcerthall. &lt;Użytkownika&gt;. trafficmanager.net
    - http://events.dogwooddojo. &lt;Użytkownika&gt;. trafficmanager.net
    - http://events.fabrikamjazzclub. &lt;Użytkownika&gt;. trafficmanager.net

    (W przypadku każdego adresu URL, Zastąp &lt;użytkownika&gt; o wartości użytkownika w danym wdrożeniu.)

   ![Zdarzenia](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)

Aby kontrolować dystrybucję żądań przychodzących, aplikacja używa [ *usługi Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md). Każde wystąpienie aplikacji specyficznego dla dzierżawy zawiera nazwę dzierżawy jako część nazwy domeny w adresie URL. Wszystkie dzierżawy adresów URL obejmują konkretnej **użytkownika** wartość. Adresy URL wykonaj następujący format:
- http://events. &lt;venuename&gt;.&lt; Użytkownik&gt;. trafficmanager.net

Bazy danych z każdej dzierżawy **lokalizacji** znajduje się w ustawieniach aplikacji w odpowiednich wdrożonej aplikacji.

W środowisku produkcyjnym zwykle należy utworzyć rekord CNAME DNS, aby [ *punktu firmowej domeny internetowej* ](../traffic-manager/traffic-manager-point-internet-domain.md) na adres URL profilu Menedżera ruchu.


## <a name="explore-the-servers-and-tenant-databases"></a>Eksploruj serwerów i baz danych dzierżawy

Oto niektóre zasoby, które zostały wdrożone:

1. W [portalu Azure](http://portal.azure.com), przejdź do listy grup zasobów.
2. Zobacz **wingtip-sa-katalogu -&lt;użytkownika&gt;**  grupy zasobów.
    - W tej grupie zasobów **katalogu-sa -&lt;użytkownika&gt;**  wdrożyć serwer. Serwer zawiera **tenantcatalog** bazy danych.
    - Należy również sprawdzić dzierżawy trzech grup zasobów.
3. Otwórz **wingtip-sa-fabrikam -&lt;użytkownika&gt;**  grupy zasobów, która zawiera zasoby wdrożenia klub Jazz firmy Fabrikam.  **Fabrikamjazzclub -&lt;użytkownika&gt;**  serwer zawiera **fabrikamjazzclub** bazy danych.

Każda baza danych dzierżawy jest 50 DTU *autonomiczny* bazy danych.

## <a name="additional-resources"></a>Dodatkowe zasoby

<!--
* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
-->

- Aby dowiedzieć się więcej na temat wielodostępnych aplikacji SaaS, zobacz [wzorce dla aplikacji SaaS wielodostępne projektowe](saas-tenancy-app-design-patterns.md).


## <a name="next-steps"></a>Następne kroki

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Jak wdrożyć aplikacji autonomicznej Wingtip biletów SaaS.
> * Temat serwerów i baz danych, które tworzą aplikacji.
> * Jak usunąć przykładowych zasobów, aby zatrzymać rozliczeń powiązanego.

