---
title: "Wdrażanie aplikacji SaaS podzielonej wielodostępne bazy danych, który używa usługi Azure SQL Database | Dokumentacja firmy Microsoft"
description: "Wdrażanie i Eksploruj podzielonej Wingtip biletów wielodostępne bazy danych aplikacji SaaS, prezentującą wzorce SaaS przy użyciu bazy danych SQL Azure."
keywords: "samouczek usługi sql database"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: billgib;MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: sstein
ms.openlocfilehash: bc96221abf62677b53df43daa44a925ac5792043
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="deploy-and-explore-a-sharded-multi-tenant-application-that-uses-azure-sql-database"></a>Wdrażanie i Eksploruj podzielonej aplikacji wielodostępnych, która używa bazy danych SQL Azure

W tym samouczku wdrażanie i Eksploruj przykładowej aplikacji bazy danych z wieloma dzierżawcami SaaS o nazwie Wingtip biletów. Aplikacja Wingtip zaprojektowano w celu pokazują funkcje bazy danych SQL Azure, które upraszczająca implementację scenariusze SaaS.

Ta implementacja Wingtips korzysta ze wzorca podzielonej wielodostępne bazy danych. Dzielenia na fragmenty jest identyfikator dzierżawcy. Dane dzierżawy jest wysyłana do określonej bazy danych zgodnie z wartościami dla identyfikatora dzierżawcy. Niezależnie od tego, jak wiele dzierżaw zawiera wszystkie danego bazy danych wszystkie bazy danych są wielodostępne w tym sensie, że schematy tabeli obejmuje identyfikatora dzierżawcy. 

Ten wzorzec bazy danych pozwala przechowywać co najmniej jednego dzierżawcy w każdym niezależnego fragmentu lub bazy danych. Można zoptymalizować dla najniższy koszt o każdej być współużytkowane przez wiele dzierżaw w bazie danych. Lub aby zoptymalizować izolacji przez każdą bazę danych przechowywane tylko jednego dzierżawcy. Wybór optymalizacji mogą być dokonywane niezależnie dla każdego określonego dzierżawcy. Wybór można wprowadzić, gdy dzierżawa najpierw jest przechowywany lub później zmieni zdanie. Aplikacja jest przeznaczona do pracy oraz w obu przypadkach.

#### <a name="app-deploys-quickly"></a>Aplikacja szybko wdraża

Poniższa sekcja wdrożenia zawiera **wdrażanie na platformie Azure** przycisku. Po naciśnięciu przycisku Wingtip pełni wdrożeniu aplikacji później pięć minut. Wingtip aplikacji w chmurze Azure, korzysta z bazy danych SQL Azure. Wingtip jest wdrażana na Twojej subskrypcji platformy Azure. Masz pełny dostęp do pracy z poszczególnych składników aplikacji.

Aplikacja jest wdrażana z danymi dla trzech dzierżawców próbki. Dzierżawcy są przechowywane razem w jednym wielodostępnej bazie danych.

Każda osoba, która można pobrać kodu źródłowego C# i programu PowerShell dla biletów Wingtip z [repozytorium Github][link-github-wingtip-multitenantdb-55g].

#### <a name="learn-in-this-tutorial"></a>Dowiedz się, w tym samouczku

> [!div class="checklist"]

> - Jak wdrożyć aplikację Wingtip SaaS.
> - Gdzie można pobrać kodu źródłowego aplikacji i skrypty zarządzania.
> - Temat serwerów i baz danych, które tworzą aplikacji.
> - Jak dzierżawcy są mapowane do swoich danych z *katalogu*.
> - Sposób obsługi administracyjnej nowej dzierżawy.
> - Jak monitorować działania dzierżawy w aplikacji.

Szereg samouczków powiązane jest dostępna które zależą od tego pierwszego wdrożenia. Samouczków Poznaj wzorce projektowania i zarządzania zakresem SaaS. Podczas pracy za pośrednictwem samouczków, zachęca się do kroku za pomocą podanego skryptów, aby zobaczyć, jak są implementowane różnych wzorców SaaS.

## <a name="prerequisites"></a>Wymagania wstępne

Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

- Zainstalowano najnowsze programu Azure PowerShell. Aby uzyskać więcej informacji, zobacz [wprowadzenie do programu Azure PowerShell][link-azure-get-started-powershell-41q].

## <a name="deploy-the-wingtip-tickets-app"></a>Wdrażanie Wingtip biletów aplikacji

1. Kliknij następujące **wdrażanie na platformie Azure** przycisku.
    - Spowoduje to otwarcie portalu Azure przy użyciu szablonu wdrożenia Wingtip biletów SaaS. Szablon wymaga dwóch wartości parametru: Nazwa nowej grupy zasobów i wartości "user", która odróżnia to wdrożenie od innych wdrożeń aplikacji. Następnym krokiem szczegółowe informacje dotyczące ustawiania wartości tych parametrów.
        - Należy pamiętać dokładne wartości, których można użyć, ponieważ będą potrzebne później pliku konfiguracji.

    [![Przycisk Wdrażaj na platformie Azure.][image-deploy-to-azure-blue-48d]][link-aka-ms-deploywtp-mtapp-52k]

2. Wprowadź wartości wymaganego parametru dla wdrożenia.

    > [!IMPORTANT]
    > Niektóre uwierzytelniania i ustawienia zapory serwera, są celowo niezabezpieczone ułatwiające pokazu. Wybierz **Utwórz nową grupę zasobów**i nie należy używać istniejącej grupy zasobów, serwerami lub pul. Nie należy używać tej aplikacji lub wszystkie zasoby, które tworzy, w środowisku produkcyjnym. Po zakończeniu pracy z aplikacją usuń tę grupę zasobów, aby zatrzymać ich rozliczanie.

    Najlepiej użyć tylko małe litery, cyfry i łączniki w nazwach zasobów.

    - Dla **grupy zasobów** — wybierz tę opcję **Utwórz nowy**, a następnie podaj **nazwa** dla grupy zasobów (z uwzględnieniem wielkości liter).
        - Zaleca się, że wszystkie litery w Nazwa grupy zasobów można małe litery.
        - Firma Microsoft zaleca dołączanie kreska, następuje inicjały, a następnie cyfrę: na przykład *wingtip af1*.
        - Wybierz **lokalizacji** z listy rozwijanej.
    - Dla **użytkownika** — zaleca się, że wybierasz krótki **użytkownika** wartość, na przykład z Twoimi inicjałami plus cyfrę: na przykład *af1*.

3. **Wdróż aplikację**.

    - Kliknij, aby zaakceptować warunki i postanowienia.
    - Kliknij pozycję **Kup**.

4. Monitorowanie stanu wdrożenia, klikając **powiadomienia**, która jest ikonę dzwonka z prawej strony pola wyszukiwania. Wdrażanie aplikacji Wingtip trwa około pięciu minut.

   ![wdrażanie zakończyło się pomyślnie](media/saas-multitenantdb-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-management-scripts"></a>Pobierz i odblokować skrypty zarządzania

Gdy aplikacja jest wdrażana, Pobierz skrypty zarządzania i kodu źródłowego aplikacji.

> [!IMPORTANT]
> Zawartość pliku wykonywalnego (skrypty, biblioteki dll) mogą być blokowane przez system Windows, gdy pliki zip są pobierane z zewnętrznego źródła i wyodrębnione. Podczas wyodrębniania skryptów z pliku zip, wykonaj następujące kroki, aby odblokować przed wyodrębniania pliku zip. Przez odblokowywania pliku zip, należy zwrócić uwagę, mogą uruchamiać skrypty.

1. Przejdź do [repozytorium github WingtipTicketsSaaS MultiTenantDb](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb).
2. Kliknij przycisk **klonowania lub pobierania**.
3. Kliknij przycisk **Pobierz ZIP** i Zapisz plik.
4. Kliknij prawym przyciskiem myszy **WingtipTicketsSaaS-MultiTenantDb-master.zip** plik i wybierz **właściwości**.
5. Na **ogólne** wybierz opcję **Odblokuj**i kliknij przycisk **Zastosuj**.
6. Kliknij przycisk **OK**.
7. Wyodrębnij pliki.

Skrypty te są dostępne w *... \\WingtipTicketsSaaS MultiTenantDb wzorca\\modułów uczenia\\*  folderu.

## <a name="update-the-configuration-file-for-this-deployment"></a>Aktualizowanie pliku konfiguracji dla tego wdrożenia

Przed uruchomieniem skrypty należy ustawić *grupy zasobów* i *użytkownika* wartości w **UserConfig.psm1**. Te zmienne należy ustawić na te same wartości ustawione podczas wdrażania.

1. Otwórz plik ... \\Learning Modules\\*UserConfig.psm1* w aplikacji *PowerShell ISE*
2. Aktualizacja *ResourceGroupName* i *nazwa* z określonymi wartościami dla danego wdrożenia (wierszach 10 i 11 tylko).
3. Zapisz zmiany.

Wartości w tym pliku jest używana przez wszystkie skrypty, więc należy pamiętać, że są prawidłowe. Jeśli wdrożenie aplikacji, upewnij się, że zostanie wybrana wartość różnych zasobów, grup i użytkowników. Następnie zaktualizuj UserConfig przy użyciu nowych wartości.

## <a name="run-the-application"></a>Uruchamianie aplikacji

Aplikacja prezentuje miejsca, takie jak sale koncertowe, kluby jazzowe i kluby sportowe, w których odbywają się imprezy. Jako klientów (lub dzierżawcy) platformy Wingtip zarejestrować miejsc w prosty sposób sprzedawać bilety i Wyświetl listę zdarzeń. Każde miejsce otrzymuje spersonalizowaną aplikację sieci Web służącą do zarządzania i wyświetlania imprez na liście oraz sprzedawania biletów, w izolacji od pozostałych dzierżaw. W obszarze obejmuje każdego dzierżawcy dane są przechowywane domyślnie w bazie danych podzielonej wielodostępnej.

Centralnego **Centrum zdarzeń** zawiera listę łącza do dzierżawców w ramach określonego wdrożenia.

1. Otwórz *Centrum zdarzeń* w przeglądarce sieci web:
    - http://events.Wingtip. &lt;Użytkownika&gt;. trafficmanager.net &nbsp; *(zamiast wartości użytkownika danego wdrożenia).*

    ![centrum zdarzeń](media/saas-multitenantdb-get-started-deploy/events-hub.png)

2. Kliknij przycisk **Fabrikam Jazz Club** w *Centrum zdarzeń*.

   ![Zdarzenia](./media/saas-multitenantdb-get-started-deploy/fabrikam.png)

Aby kontrolować dystrybucję żądań przychodzących, aplikacja używa [usługi Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). Zdarzenia stron, które są specyficzne dla dzierżawy, Uwzględnij nazwę dzierżawy w adresie URL. Adresy URL także zawierać wartość określonego użytkownika i odpowiada temu formatowi:

- http://events.Wingtip. &lt;Użytkownika&gt;.trafficmanager.net/*fabrikamjazzclub*
 
Analizuje nazwę dzierżawcy z adresu URL i skróty go w celu utworzenia klucz, aby uzyskiwać dostęp za pomocą katalogu aplikacji zdarzenia [zarządzania mapy niezależnego fragmentu](sql-database-elastic-scale-shard-map-management.md). Katalog mapuje klucz dzierżawcy lokalizacji bazy danych. **Centrum zdarzeń** zawiera listę wszystkich dzierżawców, które są zarejestrowane w wykazie. **Centrum zdarzeń** używa rozszerzonych metadanych w katalogu można pobrać nazwy dzierżawcy skojarzonej z każdego mapowania do utworzenia adresów URL.

W środowisku produkcyjnym należy zwykle utworzyć rekord CNAME DNS [punktu firmowej domeny internetowej](../traffic-manager/traffic-manager-point-internet-domain.md) profilu Menedżera ruchu.

## <a name="start-generating-load-on-the-tenant-databases"></a>Rozpoczęcie generowanie obciążenia baz danych dzierżawy

Teraz, gdy aplikacja jest wdrożona umieśćmy go do pracy! *LoadGenerator pokaz* pracą dla każdego dzierżawcy uruchamia skrypt programu PowerShell. Rzeczywiste obciążenie wiele aplikacji SaaS jest zwykle sporadyczne i nieprzewidywalny. Aby symulować tego rodzaju obciążenia, generator tworzy obciążenia dystrybuowana do wszystkich dzierżawców. Obciążenie obejmuje losowe seria każdego dzierżawcy występujących w odstępach czasu losowego. Trwa kilka minut dla wzorca obciążenia się, dlatego najlepszym rozwiązaniem generator uruchamiania na co najmniej trzech lub czterech minut przed monitorowania obciążenia.

1. W *PowerShell ISE*, Otwórz... \\Modułów uczenia\\narzędzia\\*LoadGenerator.ps1 pokaz* skryptu.
2. Naciśnij klawisz **F5**, aby uruchomić skrypt i generator obciążenia (na razie pozostaw bez zmian wartości domyślne parametrów).

> [!IMPORTANT]
> *LoadGenerator.ps1 pokaz* skrypt zostanie otwarty w innej sesji programu PowerShell, którym jest uruchamiany generator obciążenia. Generator obciążenia uruchamiane w tej sesji jako zadanie pierwszego planu, które wywołuje zadań w tle generacji obciążenia, po jednej dla każdego dzierżawcy.

Po uruchomieniu zadania pierwszego planu, pozostaje w stanie wywoływania zadania. Dodatkowe zadania jest uruchamiany dla nowi dzierżawcy, które są następnie udostępniane.

Zamykanie sesji programu PowerShell zatrzymuje wszystkie zadania.

Można ponownie uruchomić sesji generator obciążenia, aby użyć wartości innym parametrem. Jeśli tak, zamknij generowania sesji programu PowerShell, a następnie ponownie uruchom *LoadGenerator.ps1 pokaz*.

## <a name="provision-a-new-tenant-into-the-sharded-database"></a>Udostępnianie nowej dzierżawy do bazy danych podzielonej

Początkowe wdrożenie obejmuje trzy próbki dzierżaw w *Tenants1* bazy danych. Utwórz innego dzierżawcę, aby zobaczyć, jak ma to wpływ wdrożonej aplikacji. W tym kroku możesz szybko utworzyć nową dzierżawę.

1. Otwórz... \\Learning Modules\Provision i wykaz\\*ProvisionTenants.ps1 pokaz* w *programu PowerShell ISE*.
2. Naciśnij klawisz **F5** do uruchomienia skryptu (pozostaw wartości domyślne dla teraz).

   > [!NOTE]
   > Użyj wielu skryptów Wingtip biletów SaaS *$PSScriptRoot* aby umożliwić przeglądanie folderów, lub inne skrypty wywołania lub zaimportuj moduły. Ta zmienna jest oceniane tylko wtedy, gdy skrypt zostanie wykonany jako całość, naciskając klawisz **F5**.  Wyróżnianie i uruchamianie zaznaczenia (**F8**) może powodować błędy, więc naciśnij **F5** podczas uruchamiania skryptów.

Nowej dzierżawy Racing czerwony klon jest dodawany do *Tenants1* bazy danych i zarejestrowane w wykazie. Na nowym dzierżawcą sprzedaży biletów *zdarzenia* lokacji otwiera w przeglądarce:

![Nowa dzierżawa](./media/saas-multitenantdb-get-started-deploy/red-maple-racing.png)

Odśwież *Centrum zdarzeń* i pojawi się na liście nowym dzierżawcą.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Udostępnianie nowej dzierżawy w własną bazę danych

Podzielonej modelu wielodostępnym pozwala wybrać, czy do udostępnienia nowej dzierżawy w bazie danych zawierający innych dzierżawców lub aby ustanowić dzierżawy w bazie danych własnych. Dzierżawca w korzyści z konieczności jego danych odizolowane od danych innych dzierżawców. Izolację umożliwia zarządzanie wydajność tej dzierżawy, niezależnie od pozostałych dzierżawców. Ponadto jest łatwiejsze przywrócenie danych do wcześniejszego stanu izolowanego dzierżawcy. Można umieścić bezpłatnych wersji próbnych lub regularnych klientów w bazie danych z wieloma dzierżawcami i premium klienci w poszczególnych bazach danych. Jeśli tworzysz wiele baz danych, że każdy zawierać tylko jeden dzierżawy, można zarządzać ich wszystkich zbiorczo w puli elastycznej, aby optymalizować koszty zasobów.  

Teraz możemy udostępnić innej dzierżawy to czas w własną bazę danych.

1. W... \\Modułów uczenia\\udostępniania i wykaz\*ProvisionTenants.ps1* demonstracyjnej, zmodyfikuj *$TenantName* do **Salsa wierzbowate**, *$VenueType*  do **taniec** i *$Scenario* do **2**.

2. Naciśnij klawisz **F5** ponownie uruchomić skrypt.
    - Ten klawisz F5 inicjuje nowej dzierżawy w oddzielnej bazy danych. Baza danych i dzierżawcy są rejestrowane w katalogu. Następnie przeglądarce otworzy stronę zdarzenia dzierżawy.

   ![Strona zdarzenia Salix Salsa](./media/saas-multitenantdb-get-started-deploy/salix-salsa.png)

   - Przewiń do dołu strony. Istnieje na transparencie Zobacz Nazwa bazy danych, w którym są przechowywane dane dzierżawy.

3. Odśwież Centrum zdarzeń i pojawi się na liście dwóch nowych dzierżaw.



## <a name="explore-the-servers-and-tenant-databases"></a>Eksploruj serwerów i baz danych dzierżawy

Teraz przyjrzymy się niektórych zasobów, które zostały wdrożone:

1. W [portalu Azure](http://portal.azure.com), przejdź do listy grup zasobów. Otwórz grupę zasobów, która utworzone podczas wdrażania aplikacji.

   ![grupa zasobów](./media/saas-multitenantdb-get-started-deploy/resource-group.png)

2. Kliknij przycisk **mt katalogu&lt;użytkownika&gt;**  serwera. Serwer katalogu zawiera dwie bazy danych o nazwie *tenantcatalog* i *basetenantdb*. *Basetenantdb* bazy danych jest bazą danych pustego szablonu. Aby utworzyć nową bazę danych dzierżawy jest kopiowany czy używany dla wielu dzierżawców lub pojedynczej dzierżawy.

   ![serwer wykazu](./media/saas-multitenantdb-get-started-deploy/catalog-server.png)

3. Wróć do grupy zasobów i wybierz *tenants1 mt* serwera baz danych dzierżawy.
    - Tenants1 bazy danych jest bazą danych wielodostępne przechowywane oryginalnego dzierżaw trzy, a także dzierżawcy pierwszy dodawane. Jest on skonfigurowany jako 50 jednostek dtu w warstwie standardowa bazy danych.
    - **Salixsalsa** bazy danych ma miejsce taniec Salsa wierzbowate jako jego tylko dzierżawy. Domyślnie jest skonfigurowany jako Standard edition baza danych o 50 Dtu.

   ![dzierżawcy serwera](./media/saas-multitenantdb-get-started-deploy/tenants-server.png)


## <a name="monitor-the-performance-of-the-database"></a>Monitorowanie wydajności bazy danych

Jeśli generator obciążenia uruchomione przez kilka minut, za mało danych telemetrii znajduje się rozpocząć wyszukiwanie informacji niektóre bazy danych monitorowania funkcji wbudowanych w portalu.

1. Przejdź do **tenants1 mt&lt;użytkownika&gt;**  serwera, a następnie kliknij przycisk **tenants1** do wyświetlania wykorzystania zasobów bazy danych, która ma cztery dzierżaw w nim. Każdy dzierżawca podlega sporadyczne duże obciążenie z generatora obciążenia:

   ![Monitor tenants1](./media/saas-multitenantdb-get-started-deploy/monitor-tenants1.png)

   Wykres wykorzystania jednostek dtu w warstwie dobrze pokazuje, jak wielodostępne bazy danych można obsługiwać nieprzewidywalne obciążenia między wiele dzierżaw. W takim przypadku generator obciążenia jest stosowanie sporadyczne obciążenie około 30 Dtu do każdego dzierżawcy. Ta operacja ładowania równa 60% wykorzystania 50 jednostek dtu w warstwie bazy danych. Wartości, które przekraczają 60% są wynikiem obciążenia są stosowane do więcej niż jednej dzierżawy, w tym samym czasie.

2. Przejdź do **tenants1 mt&lt;użytkownika&gt;**  serwera, a następnie kliknij przycisk **salixsalsa** bazy danych, aby wyświetlić wykorzystania zasobów w tej bazie danych, zawierający tylko jednego dzierżawcy.

   ![salixsalsa bazy danych](./media/saas-multitenantdb-get-started-deploy/monitor-salix.png)

Generator obciążenia jest stosowanie obciążenia podobne do każdego dzierżawcy, niezależnie od tego, bazę danych, która jest każdego dzierżawcy. Z tylko jednego dzierżawcy w **salixsalsa** bazy danych, zobaczysz, że bazy danych może kontynuować działanie wyższe obciążenie niż baza danych z wielu dzierżawców. 

> [!NOTE]
> Obciążenia tworzone przez generator obciążenia są tylko do celów ilustracyjnych.  Zasoby przydzielone do wielu dzierżawców i pojedynczej dzierżawy baz danych i liczby dzierżawców, które można obsługiwać w bazie danych wielodostępnej zależą od wzorce rzeczywistego obciążenia w aplikacji.


## <a name="next-steps"></a>Następne kroki

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> - Jak wdrożyć aplikację Wingtip biletów SaaS wielodostępne w bazie danych
> - Dotyczące serwerów i baz danych, które tworzą aplikacji
> - Mapowanie dzierżaw na ich dane przy użyciu *wykazu*
> - Sposób obsługi administracyjnej nowych dzierżaw do bazy danych z wieloma dzierżawcami i bazy danych z pojedynczej dzierżawy.
> - Wyświetlanie wykorzystania puli w celu monitorowania aktywności dzierżawy
> - Usuwanie przykładowych zasobów w celu zakończenia ich rozliczania

Teraz spróbuj [samouczek udostępniania i wykaz](sql-database-saas-tutorial-provision-and-catalog.md).



## <a name="additional-resources"></a>Dodatkowe zasoby

- Aby dowiedzieć się więcej o wielodostępnych aplikacjach SaaS, zobacz [ *Design patterns for multi-tenant SaaS applications*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications) (Wzorce projektu dla wielodostępnej aplikacji SaaS)








<!--  Link references.

A [series of related tutorials] is available that build upon this initial deployment.
[link-wtp-overivew-jumpto-saas-tutorials-97j]: saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials

-->

[link-aka-ms-deploywtp-mtapp-52k]: http://aka.ms/deploywtp-mtapp


[link-azure-get-started-powershell-41q]: https://docs.microsoft.com/powershell/azure/get-started-azureps

[link-github-wingtip-multitenantdb-55g]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB/





<!--  Image references.

[image-deploy-to-azure-blue-48d]: http://aka.ms/deploywtp-mtapp "Button for Deploy to Azure."

-->

[image-deploy-to-azure-blue-48d]: media/saas-multitenantdb-get-started-deploy/deploy.png

