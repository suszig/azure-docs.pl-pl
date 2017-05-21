---
title: "Wdrażanie i korzystanie z aplikacji WTP (Wingtip Tickets Platform) (przykładowa aplikacja SaaS korzystająca z usługi Azure SQL Database) | Dokumentacja firmy Microsoft"
description: "Wdrażanie i korzystanie z przykładowej aplikacji SaaS korzystającej z usługi Azure SQL Database"
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
ms.openlocfilehash: ff4dc19bb6e24ea9ceeed9721cfb3a85b4d10965
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017


---
# <a name="deploy-and-explore-a-multi-tenant-saas-application-that-uses-azure-sql-database"></a>Wdrażanie i korzystanie z wielodostępnej aplikacji SaaS korzystającej z usługi Azure SQL Database

W tym samouczku zapoznasz się z wdrażaniem i korzystaniem z aplikacji SaaS o nazwie WTP (Wingtip Ticket Platform). Aplikacja używa wzorca aplikacji SaaS database-per-tenant (baza danych na dzierżawę) do obsługi wielu dzierżaw. Aplikacja została zaprojektowana tak, aby zaprezentować funkcje usługi Azure SQL Database umożliwiające stosowanie scenariuszy SaaS oraz wzorców projektu i zarządzania SaaS.

Pięć minut po kliknięciu znajdującego się poniżej przycisku *Wdróż na platformie Azure* uzyskasz wielodostępną aplikację SaaS, korzystającą z usługi SQL Database i uruchomioną w chmurze. Aplikacja jest wdrażana z trzema przykładowymi dzierżawami, z których każda zawiera własną bazę danych, a wszystkie są wdrożone w elastycznej puli SQL. Ta aplikacja jest wdrażana w ramach Twojej subskrypcji platformy Azure, dzięki czemu masz pełny dostęp umożliwiający inspekcję poszczególnych składników aplikacji i pracę z nimi.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Wdrażanie aplikacji WTP
> * Informacje o serwerach, pulach i bazach danych składających się na aplikację
> * Mapowanie dzierżaw na ich dane przy użyciu *wykazu*
> * Aprowizacja nowych dzierżaw
> * Wyświetlanie wykorzystania puli w celu monitorowania aktywności dzierżawy
> * Usuwanie przykładowych zasobów w celu zakończenia ich rozliczania

Dostępna jest [seria powiązanych samouczków](sql-database-wtp-overview.md) umożliwiających zbadanie rozmaitych wzorców projektów i zarządzania używanych w rozwiązaniach SaaS, które są rozbudowaną wersją tego wstępnego wdrożenia. Zapoznając się z samouczkami, prześledź uważnie przedstawione skrypty i zbadaj, jak zostały zaimplementowane poszczególne wzorce SaaS. Prześledź krok po kroku skrypty zawarte w każdym samouczku, aby dobrze zrozumieć, jak zaimplementować wiele funkcji usługi SQL Database, które ułatwiają tworzenie aplikacji SaaS.

Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="deploy-the-wingtip-tickets-wtp-saas-application"></a>Wdrażanie aplikacji SaaS o nazwie WTP (Wingtip Tickets Platform)

Wdróż aplikację WTP w mniej niż pięć minut:

1. Kliknij, aby wdrożyć:

   <a href="http://aka.ms/deploywtpapp" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Wprowadź wymagane wartości parametrów wdrożenia:

    > [!IMPORTANT]
    > Niektóre uwierzytelnienia i zapory serwera są celowo niezabezpieczone w celach demonstracyjnych. **Utwórz nową grupę zasobów** i nie używaj istniejących grup zasobów, serwerów ani pul, jak również nie używaj tej aplikacji ani żadnych utworzonych przez nią zasobów do celów produkcyjnych. Po zakończeniu pracy z aplikacją usuń tę grupę zasobów, aby zatrzymać ich rozliczanie.

    * **Grupa zasobów** — wybierz opcję **Utwórz nową** i podaj wartości **Nazwa** i **Lokalizacja**.
    * **Użytkownik** — niektóre zasoby wymagają nazw, które są unikatowe dla wszystkich subskrypcji platformy Azure. W celu zapewnienia unikatowości należy podać wartość odróżniającą tworzone przez Ciebie zasoby od zasobów utworzonych przez innych użytkowników wdrażających aplikację Wingtip. Zalecane jest użycie krótkiej nazwy **Użytkownik**, takiej jak inicjały i liczba (na przykład *bg1*) i użycie jej jako nazwy grupy zasobów (na przykład *wingtip-bg1*). Parametr **Użytkownik** może zawierać tylko litery, cyfry i łączniki. Pierwszy i ostatni znak musi być literą lub cyfrą (zalecane jest stosowanie wyłącznie małych liter).

     ![szablon](./media/sql-database-saas-tutorial/template.png)

1. **Wdróż aplikację**.

    * Kliknij odpowiednią opcję, aby określić, że zgadzasz się z warunkami i postanowieniami.
    * Wybierz opcję **Przypnij do pulpitu nawigacyjnego**.
    * Kliknij pozycję **Kup**.

1. Monitoruj stan wdrożenia, klikając opcję **Powiadomienia** (ikona dzwonka z prawej strony pola wyszukiwania). Wdrażanie aplikacji WTP trwa około czterech minut.

   ![wdrażanie zakończyło się pomyślnie](media/sql-database-saas-tutorial/succeeded.png)

## <a name="explore-the-application"></a>Poznawanie aplikacji

Aplikacja prezentuje miejsca, takie jak sale koncertowe, kluby jazzowe i kluby sportowe, w których odbywają się imprezy. Te miejsca rejestrują się jako klienci (dzierżawy) aplikacji Wingtip Tickets Platform (WTP), dzięki czemu mogą w prosty sposób umieszczać imprezy na liście i sprzedawać bilety. Każde miejsce otrzymuje spersonalizowaną aplikację sieci Web służącą do zarządzania i wyświetlania imprez na liście oraz sprzedawania biletów, w izolacji od pozostałych dzierżaw. Stojący za tym mechanizm umożliwia każdej dzierżawie korzystanie z bazy danych SQL wdrożonej w elastycznej puli SQL.

Główne **Centrum zdarzeń** zawiera listę adresów URL dzierżaw odpowiednią do danego wdrożenia. Wszystkie adresy URL dzierżaw zawierają podaną przez Ciebie wartość *Użytkownik* i mają następujący format: http://events.wtp.&lt;UŻYTKOWNIK&gt;.trafficmanager.net/*fabrikamjazzclub*. 

1. Otwórz _Centrum zdarzeń_: http://events.wtp.&lt;UŻYTKOWNIK&gt;. trafficmanager.net (wstaw swoją nazwę użytkownika):

    ![centrum zdarzeń](media/sql-database-saas-tutorial/events-hub.png)

1. Kliknij przycisk **Fabrikam Jazz Club** w *Centrum zdarzeń*.

   ![Zdarzenia](./media/sql-database-saas-tutorial/fabrikam.png)

1. Kliknij przycisk **Bilety** i sprawdź, jak kupuje się bilet na imprezę.

Aplikacja WTP korzysta z usługi [ *Azure Traffic Manager* ](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) do sterowania dystrybucją ruchu przychodzącego. W przypadku stron zdarzeń, które są specyficzne dla dzierżawy, nazwa dzierżawy musi występować w adresie URL. Aplikacja zdarzeń analizuje nazwę dzierżawy zawartą w adresie URL i używa jej do utworzenia klucza dostępu do wykazu przy użyciu [ *zarządzania dzieleniem map na fragmenty*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-scale-shard-map-management). Ten wykaz służy do mapowania klucza na lokalizację bazy danych dzierżawy. **Centrum zdarzeń** używa rozszerzonych metadanych zawartych w wykazie do pobierania nazwy dzierżawy powiązanej z każdą bazą danych.

W środowisku produkcyjnym zazwyczaj jest konieczne utworzenie rekordu CNAME DNS służącego do [*wskazywania domeny internetowej firmy*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) w profilu menedżera ruchu.

## <a name="get-the-wingtip-application-scripts"></a>Pobieranie skryptów aplikacji Wingtip

Skrypty i kod źródłowy aplikacji Wingtip Tickets są dostępne w repozytorium GitHub [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Pliki skryptów znajdują się w folderze [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Pobierz folder **Learning Modules** na komputer lokalny, zachowując jego strukturę folderów.

## <a name="provision-a-new-tenant"></a>Aprowizacja nowej dzierżawy

Podczas początkowego wdrożenia są tworzone trzy przykładowe dzierżawy, ale w celu lepszego wykorzystania samouczka należy utworzyć więcej dzierżaw. W tym kroku zobaczysz, jak szybko utworzyć nową dzierżawę. Później należy dokładniej prześledzić szczegóły aprowizacji nowych dzierżaw, wykorzystując [samouczek dotyczący aprowizacji i wykazu](sql-database-saas-tutorial-provision-and-catalog.md), w którym objaśniono, jak w prosty sposób zaimplementować składnik rejestracji w aplikacji oraz automatycznie aprowizować dzierżawy, kiedy klienci się rejestrują.

### <a name="initialize-the-user-config-file-for-your-deployment"></a>Inicjowanie pliku konfiguracyjnego użytkownika dla przeprowadzanego wdrożenia

Ponieważ jest to pierwszy skrypt, który będzie uruchamiany po początkowym wdrożeniu aplikacji WTP, należy zaktualizować plik konfiguracyjny użytkownika. Zaktualizuj zmienne, podając wartości specyficzne dla wdrożenia.

   1. Otwórz plik ... \\Learning Modules\\*UserConfig.psm1* w aplikacji *PowerShell ISE*
   1. Zmodyfikuj zmienną _$userConfig.ResourceGroupName_, używając _grupy zasobów_ ustawionej podczas wdrażania aplikacji.
   1. Zmodyfikuj zmienną _$userConfig.Name_, używając nazwy _Użytkownik_ ustawionej podczas wdrażania aplikacji.

### <a name="provision-the-new-tenant"></a>Aprowizacja nowej dzierżawy

1. Otwórz plik ...\\Learning Modules\Provision and Catalog\\*Demo-ProvisionAndCatalog.ps1* w aplikacji *PowerShell ISE*.
1. Naciśnij klawisz **F5**, aby uruchomić skrypt (na razie pozostaw bez zmian wartości domyślne skryptu).

Nowa dzierżawa zostanie zarejestrowana w wykazie. Jej baza danych jest tworzona i dodawana do elastycznej puli SQL. Po pomyślnej aprowizacji witryna sprzedaży biletów nowej dzierżawy pojawi się w przeglądarce:

![Nowa dzierżawa](./media/sql-database-saas-tutorial/red-maple-racing.png)

Odśwież Centrum zdarzeń i sprawdź, czy nowa dzierżawa jest na liście.

## <a name="start-generating-load-on-the-tenant-databases"></a>Rozpoczęcie generowanie obciążenia baz danych dzierżawy

Skoro już mamy kilka baz danych dzierżaw, zacznijmy z nich korzystać! Dostarczony skrypt programu PowerShell symuluje obciążenia wszystkich baz danych dzierżaw. Obciążenie jest uruchamiane domyślnie przez 60 minut. Aplikacja WTP jest aplikacją SaaS, a w przypadku aplikacji SaaS rzeczywiste obciążenie jest zazwyczaj sporadyczne i nieprzewidywalne. Aby to zasymulować, generator w sposób losowy obciąża wszystkie dzierżawy. Potrzebnych jest kilka minut, aby stał się widoczny wzorzec, dlatego należy odczekać 5-10 minut przed próbą monitorowania obciążenia opisaną w następnych sekcjach.

1. Otwórz plik ...\\Learning Modules\\Utilities\\*Demo-LoadGenerator.psm1* w aplikacji **PowerShell ISE**
1. Naciśnij klawisz **F5**, aby uruchomić skrypt i generator obciążenia (na razie pozostaw bez zmian wartości domyślne parametrów).

> [!IMPORTANT]
> Generator obciążenia działa jako seria zadań w lokalnej sesji programu PowerShell, która musi pozostać otwarta! Jeśli zamkniesz program PowerShell lub kartę, na której został uruchomiony generator obciążenia, albo wstrzymasz pracę komputera, zadania zostaną zatrzymane.


## <a name="explore-the-servers-pools-and-tenant-databases"></a>Sprawdzanie działania serwerów, pul i baz danych dzierżaw

Teraz, gdy sprawdziliśmy działanie aplikacji, aprowizowaliśmy nową dzierżawę i uruchomiliśmy obciążenie kolekcji dzierżaw, przyjrzyjmy się niektórym wdrożonym zasobom.

1. W witrynie[Azure Portal](http://portal.azure.com) otwórz serwer **catalog-&lt;UŻYTKOWNIK&gt;**. Serwer wykazu zawiera dwie bazy danych. **tenantcatalog** i **basetenantdb** (pusta *końcowa wersja* bazy danych, która została skopiowana w celu utworzenia nowych dzierżaw).

   ![bazy danych](./media/sql-database-saas-tutorial/databases.png)

1. Otwórz serwer **tenants1 -&lt;UŻYTKOWNIK&gt;** zawierający bazy danych dzierżaw. Należy pamiętać, że każda baza danych dzierżawy jest bazą danych typu _Elastyczna — Standardowa_ znajdującą się w standardowej puli 50 jednostek eDTU. Ponadto istnieje baza danych _Red Maple Racing_, której dzierżawa została aprowizowana przez Ciebie wcześniej.

   ![serwer](./media/sql-database-saas-tutorial/server.png)

## <a name="monitor-the-pool"></a>Monitorowanie puli

Generator obciążenia działający przez kilka minut powinien dostarczyć wystarczającej ilości danych, aby można było rozpocząć zapoznawanie się z niektórymi możliwościami monitorowania wbudowanymi w pule i bazy danych.

1. Przejdź do serwera *tenants1 -&lt;UŻYTKOWNIK&gt;* i kliknij pozycję **Pool1** w celu wyświetlenia wykorzystania zasobów puli:

   ![monitorowanie puli](./media/sql-database-saas-tutorial/monitor-pool.png)

Oba wykresy pokazują, jak dobrze elastyczne pule i usługa SQL Database są dostosowane do pracy przy obciążeniu przez aplikacje SaaS. Cztery bazy danych, które są obciążane chwilowym obciążeniem o maksymalnej wartości 40 jednostek eDTU, są z łatwością obsługiwane przez pulę 50 jednostek eDTU. Gdyby każda z ich została aprowizowana jako osobna baza danych, do obsługi obciążenia chwilowego wymagany byłby poziom S2 (50 jednostek DTU), jednak koszt 4 osobnych baz danych S2 prawie trzykrotnie przekraczałby koszt puli. Poza tym w puli pozostaje sporo miejsca na dodatkowe bazy danych. W rzeczywistych warunkach klienci używają do 500 baz danych uruchamianych w pulach 200 jednostek eDTU. Aby uzyskać więcej informacji, zobacz [samouczek monitorowania wydajności](sql-database-saas-tutorial-performance-monitoring.md).


## <a name="deleting-the-resources-created-with-this-tutorial"></a>Usuwanie zasobów utworzonych za pomocą tego samouczka

Po zakończeniu poznawania aplikacji WTP i pracy z nią, przejdź do grupy zasobów aplikacji w portalu i ją usuń, aby zatrzymać wszystkie rozliczenia powiązane z tym wdrożeniem. W przypadku korzystania z dowolnego innego powiązanego samouczka, razem z aplikacją należy usunąć wszystkie utworzone zasoby.


## <a name="next-steps"></a>Następne kroki

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Wdrażanie aplikacji WTP
> * Informacje o serwerach, pulach i bazach danych składających się na aplikację
> * Mapowanie dzierżaw na ich dane przy użyciu *wykazu*
> * Aprowizacja nowych dzierżaw
> * Wyświetlanie wykorzystania puli w celu monitorowania aktywności dzierżawy
> * Usuwanie przykładowych zasobów w celu zakończenia ich rozliczania

Przejdź teraz do [samouczka dotyczącego aprowizacji i wykazu](sql-database-saas-tutorial-provision-and-catalog.md)



## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dodatkowe samouczki nawiązujące do początkowego wdrożenia aplikacji Wingtip Tickets Platform (WTP)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* Aby dowiedzieć się więcej o elastycznych pulach, zobacz [ *Co to jest pula elastyczna Azure*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* Aby dowiedzieć się więcej o zadaniach elastycznych, zobacz [ *Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview) (Zarządzanie skalowalnymi bazami danych w chmurze)
* Aby dowiedzieć się więcej o wielodostępnych aplikacjach SaaS, zobacz [ *Design patterns for multi-tenant SaaS applications*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications) (Wzorce projektu dla wielodostępnej aplikacji SaaS)

