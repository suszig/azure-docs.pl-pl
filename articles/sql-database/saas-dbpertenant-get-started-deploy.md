---
title: "Samouczek SaaS wielodostępne — baza danych SQL Azure | Dokumentacja firmy Microsoft"
description: "Wdrażanie i Eksploruj aplikacji wielodostępne Wingtip biletów SaaS, który demonstruje bazy danych dla każdej dzierżawy i innymi wzorami SaaS przy użyciu bazy danych SQL Azure."
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
ms.date: 11/10/2017
ms.author: sstein
ms.openlocfilehash: f91ddff81e51e7cc3d1561dc799013764530924b
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="deploy-and-explore-a-multi-tenant-saas-application-that-uses-the-database-per-tenant-pattern-with-azure-sql-database"></a>Wdrażanie i Eksploruj wielodostępnych aplikacji SaaS, korzystającej z bazy danych na wzorzec dzierżawcy z bazy danych SQL Azure

W tym samouczku wdrażanie i Eksploruj Wingtip biletów SaaS bazy danych na aplikację dzierżawy. Aplikacja używa wzorca bazy danych dla dzierżawy, aby przechowywać dane o wielu dzierżawców. Aplikacja jest przeznaczona do pokazują funkcje bazy danych SQL Azure, upraszczające włączania scenariusze SaaS.

Pięć minut po kliknięciu znajdującego się poniżej przycisku *Wdróż na platformie Azure* uzyskasz wielodostępną aplikację SaaS, korzystającą z usługi SQL Database i uruchomioną w chmurze. Aplikacja jest wdrażana z trzech dzierżawami próbki, każda z własną bazę danych, wszystkie wdrożone w puli elastycznej SQL. Aplikacja jest wdrażana na Twojej subskrypcji platformy Azure, umożliwiając dostęp do eksplorowania i pracować z poszczególnych składników aplikacji. Skrypty zarządzania i kodu źródłowego aplikacji są dostępne w repozytorium GitHub WingtipTicketsSaaS DbPerTenant.


Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Wdrażanie aplikacji Wingtip SaaS
> * Skąd uzyskać kodu źródłowego aplikacji i skrypty zarządzania
> * Informacje o serwerach, pulach i bazach danych składających się na aplikację
> * Jak dzierżawcy są mapowane do swoich danych z *katalogu*
> * Udostępnianie nowej dzierżawy
> * Jak monitorować działania dzierżawy w aplikacji

Dostępna jest [seria powiązanych samouczków](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) umożliwiających zbadanie rozmaitych wzorców projektów i zarządzania używanych w rozwiązaniach SaaS, które są rozbudowaną wersją tego wstępnego wdrożenia. Zapoznając się z samouczkami, prześledź uważnie przedstawione skrypty i zbadaj, jak zostały zaimplementowane poszczególne wzorce SaaS. Prześledź krok po kroku skrypty zawarte w każdym samouczku, aby dobrze zrozumieć, jak zaimplementować wiele funkcji usługi SQL Database, które ułatwiają tworzenie aplikacji SaaS.

## <a name="prerequisites"></a>Wymagania wstępne

Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="deploy-the-wingtip-tickets-saas-application"></a>Wdrażanie aplikacji SaaS biletów Wingtip

Wdróż aplikację:

1. Kliknięcie przycisku **wdrażanie na platformie Azure** przycisk otwiera bazy danych SaaS biletów Wingtip na szablon wdrożenia dzierżawy w portalu Azure. Szablon wymaga dwóch wartości parametru; Nazwa nowej grupy zasobów i nazwę użytkownika, która odróżnia to wdrożenie od innych wdrożeń Wingtip biletów SaaS bazy danych dla aplikacji dzierżawy. Następnym krokiem szczegółowe informacje dotyczące ustawiania tych wartości.

   Należy zwrócić uwagę dokładne wartości, których używasz, ponieważ trzeba będzie wprowadzić je później w pliku konfiguracji.

   <a href="https://aka.ms/deploywingtipdpt" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Wprowadź wymagane wartości parametrów wdrożenia:

    > [!IMPORTANT]
    > Niektóre uwierzytelnienia i zapory serwera są celowo niezabezpieczone w celach demonstracyjnych. **Utwórz nową grupę zasobów**. Nie należy używać istniejącej grupy zasobów, serwerami lub pul. Nie należy używać tej aplikacji, skryptów lub wszystkie zasoby wdrożone w środowisku produkcyjnym. Po zakończeniu pracy z aplikacją usuń tę grupę zasobów, aby zatrzymać ich rozliczanie.

    * **Grupa zasobów** — wybierz tę opcję **Utwórz nowy** i podaj **nazwa** dla grupy zasobów. 
    * **Lokalizacja** — wybierz tę opcję **lokalizacji** z listy rozwijanej.
    * **Użytkownik** -niektórych zasobów wymagają nazw, które są globalnie unikatowe. W celu zapewnienia unikatowości, zawsze, gdy wdrażasz aplikację Podaj wartość do odróżniania zasobów tworzonych z zasobów tworzone przez inne wdrożenie aplikacji Wingtip. Zalecane jest użycie krótki **użytkownika** nazwę, np. z Twoimi inicjałami i numer (na przykład *af1*) i użyć jej w polu Nazwa grupy zasobów (na przykład *wingtip-af1*). **Użytkownika** parametr może zawierać tylko litery, cyfry i łączniki (bez spacji). Pierwszy i ostatni znak musi być literą lub cyfrą (zalecane jest stosowanie wyłącznie małych liter).


1. **Wdróż aplikację**.

    * Kliknij, aby zaakceptować warunki i postanowienia.
    * Kliknij pozycję **Kup**.

1. Monitoruj stan wdrożenia, klikając opcję **Powiadomienia** (ikona dzwonka z prawej strony pola wyszukiwania). Wdrażanie aplikacji SaaS biletów Wingtip trwa około pięciu minut.

   ![wdrażanie zakończyło się pomyślnie](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-tickets-management-scripts"></a>Pobierz i odblokować biletów Wingtip skrypty zarządzania

Gdy aplikacja jest wdrażana, Pobierz skrypty kodu i zarządzania źródła.

> [!IMPORTANT]
> Zawartość pliku wykonywalnego (skrypty, biblioteki dll) mogą być blokowane przez system Windows, gdy pliki zip są pobierane z zewnętrznego źródła i wyodrębnione. Podczas wyodrębniania skryptów z pliku zip, wykonaj poniższe kroki, aby odblokować przed wyodrębniania pliku zip. Dzięki temu można uruchamiać skrypty.

1. Przejdź do [repozytorium GitHub WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant).
1. Kliknij przycisk **klonowania lub pobierania**.
1. Kliknij przycisk **Pobierz ZIP** i Zapisz plik.
1. Kliknij prawym przyciskiem myszy **WingtipTicketsSaaS-DbPerTenant-master.zip** pliku, a następnie wybierz **właściwości**.
1. Na **ogólne** wybierz opcję **Odblokuj**i kliknij przycisk **Zastosuj**.
1. Kliknij przycisk **OK**.
1. Wyodrębnij pliki.

Skrypty znajdują się w *... \\WingtipTicketsSaaS DbPerTenant wzorca\\modułów uczenia* folderu.

## <a name="update-the-user-configuration-file-for-this-deployment"></a>Aktualizuj plik konfiguracji użytkownika dla tego wdrożenia

Przed uruchomieniem skrypty należy zaktualizować *grupy zasobów* i *użytkownika* wartości w **UserConfig.psm1**. Ustaw zmienne używane podczas wdrażania.

1. W *PowerShell ISE*, Otwórz... \\Modułów szkoleniowych\\*UserConfig.psm1* 
1. Aktualizacja *ResourceGroupName* i *nazwa* z określonymi wartościami dla danego wdrożenia (wierszach 10 i 11 tylko).
1. Zapisać zmiany!

Te wartości są wymienione w niemal każdego skryptu.

## <a name="run-the-application"></a>Uruchamianie aplikacji

Aplikacja jest przeznaczony dla miejsc, takich jak koncertowe, jazz klubów i sportowych, który obsługiwać zdarzeń. Jako klientów (lub dzierżawcy) biletów Wingtip zarejestrować miejsc w prosty sposób sprzedawać bilety i Wyświetl listę zdarzeń. Każda właściwość pobiera spersonalizowane witryny sieci web do zarządzania i wyświetlić ich zdarzenia i sprzedawać bilety, niezależne i odizolowany od pozostałych dzierżawców. W obszarze obejmuje każdy dzierżawca pobiera wdrożonych w puli elastycznej SQL bazy danych SQL.

Centralnego **Centrum zdarzeń** strona zawiera listę łączy do dzierżawców w danym wdrożeniu.

1. Otwórz _Centrum zdarzeń_ w przeglądarce sieci web: http://events.wingtip-dpt.&lt;użytkownika&gt;. trafficmanager.net (Zastąp &lt;użytkownika&gt; z wartością użytkownika danego wdrożenia):

    ![centrum zdarzeń](media/saas-dbpertenant-get-started-deploy/events-hub.png)

1. Kliknij przycisk **Fabrikam Jazz Club** w *Centrum zdarzeń*.

   ![Zdarzenia](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)


Aby kontrolować dystrybucję żądań przychodzących, aplikacja używa [ *usługi Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md). W przypadku stron zdarzeń, które są specyficzne dla dzierżawy, nazwa dzierżawy musi występować w adresie URL. Wszystkie dzierżawy adresów URL obejmują konkretnej *użytkownika* wartości i odpowiada temu formatowi: http://events.wingtipp-dpt.&lt;użytkownika&gt;.trafficmanager.net/*fabrikamjazzclub*. Analizuje nazwę dzierżawcy z adresu URL i używa go do utworzenia klucza wykaz implementowane za pomocą dostępu do aplikacji zdarzenia [ *zarządzania mapy niezależnego fragmentu*](sql-database-elastic-scale-shard-map-management.md). Katalog mapuje klucz dzierżawcy lokalizacji bazy danych. **Centrum zdarzeń** używa rozszerzonych metadanych w katalogu pobrać nazwy dzierżawcy skojarzone z każdą bazę danych, aby utworzyć listę adresów URL.

W środowisku produkcyjnym zazwyczaj jest konieczne utworzenie rekordu CNAME DNS służącego do [*wskazywania domeny internetowej firmy*](../traffic-manager/traffic-manager-point-internet-domain.md) w profilu menedżera ruchu.

## <a name="start-generating-load-on-the-tenant-databases"></a>Rozpoczęcie generowanie obciążenia baz danych dzierżawy

Teraz, gdy aplikacja jest wdrożona umieśćmy go do pracy! *LoadGenerator pokaz* pracą dzierżawy w przypadku wszystkich baz danych uruchamia skrypt programu PowerShell. Rzeczywiste obciążenie wiele aplikacji SaaS jest zwykle sporadyczne i nieprzewidywalny. Aby symulować tego rodzaju obciążenia, generator tworzy dystrybuowana do wszystkich dzierżawców z losowego seria każdego dzierżawcy występujących w odstępach czasu losowego obciążenia. W związku z tym, jaki zajmuje kilka minut dla wzorca obciążenia wyłaniać się dlatego jest najlepszym rozwiązaniem generator uruchamiania na co najmniej trzech lub czterech minut przed monitorowania obciążenia.

1. W *PowerShell ISE*, Otwórz... \\Modułów uczenia\\narzędzia\\*LoadGenerator.ps1 pokaz* skryptu.
1. Naciśnij klawisz **F5**, aby uruchomić skrypt i generator obciążenia (na razie pozostaw bez zmian wartości domyślne parametrów).

> [!IMPORTANT]
> Aby uruchomić inne skrypty, Otwórz nowe okno programu PowerShell ISE. Generator obciążenia działa jako seria zadań w lokalnej sesji programu PowerShell. *LoadGenerator.ps1 pokaz* skryptu dotyczącego skryptu generator rzeczywiste obciążenie, który jest uruchamiany jako zadanie pierwszego planu oraz szereg zadań generacji obciążenia w tle. Zadanie generator obciążenia jest wywoływana dla każdej bazy danych zarejestrowane w wykazie. Zadania działają w lokalnej sesji programu PowerShell, zamykanie sesji programu PowerShell zatrzymuje wszystkie zadania. Jeśli zawiesić komputera, generowanie obciążenia został wstrzymany i zostanie wznowiona po wznowieniu komputera.

Po generator obciążenia wywołuje obciążenia generacji zadań dla każdego dzierżawcy, zadanie pierwszego planu pozostaje w stanie wywoływania zadania, których uruchamia dodatkowe zadania dla nowi dzierżawcy, które są następnie udostępniane. Można użyć *Ctrl-C* lub naciśnij klawisz *zatrzymać* przycisk, aby zatrzymać zadanie pierwszego planu, ale istniejące zadań w tle będzie nadal generowania obciążenia w każdej bazie danych. Jeśli chcesz monitorować i kontrolować zadań w tle, użyj *Get-Job*, *Receive-Job* i *zadanie zatrzymania*. Podczas wykonywania zadania pierwszego planu nie można użyć tej samej sesji programu PowerShell do wykonywania innych skryptów. Aby uruchomić inne skrypty, Otwórz nowe okno programu PowerShell ISE.

Jeśli chcesz uruchomić ponownie sesję generator obciążenia, na przykład o innych parametrach, można zatrzymać zadania pierwszego planu i uruchom ponownie *LoadGenerator.ps1 pokaz* skryptu. Ponowne uruchomienie *LoadGenerator.ps1 pokaz* pierwszy zatrzymuje aktualnie uruchomione zadania i ponownie uruchamia generator dotyczącego nowy zestaw zadań przy użyciu bieżących parametrów.

Na razie pozostaw generator obciążenia uruchomione w stanie wywoływania zadania.


## <a name="provision-a-new-tenant"></a>Aprowizacja nowej dzierżawy

Początkowe wdrożenie tworzy trzy dzierżaw przykładowe, ale Utwórzmy innego dzierżawcę, aby zobaczyć, jak ma to wpływ wdrożonej aplikacji. Przepływu pracy obsługi dzierżawców SaaS biletów Wingtip została szczegółowo opisana w [samouczek udostępniania i wykaz](saas-dbpertenant-provision-and-catalog.md). W tym kroku możesz szybko utworzyć nową dzierżawę.

1. W *PowerShell ISE*, Otwórz... \\Learning Modules\Provision i wykaz\\*ProvisionAndCatalog.ps1 pokaz* .
1. Naciśnij klawisz **F5** do uruchomienia skryptu (pozostaw wartości domyślne dla teraz).

   > [!NOTE]
   > Użyj wielu skryptów Wingtip SaaS *$PSScriptRoot* do przeglądania folderów do wywołania funkcji w innych skryptów. Ta zmienna jest tylko oceniane w czasie pełnego skrypt zostanie wykonany przez naciśnięcie przycisku **F5**.  Wyróżnianie i uruchamianie zaznaczenia (**F8**) może powodować błędy, więc naciśnij **F5** podczas uruchamiania skryptów.

Nowej dzierżawy bazy danych jest utworzony w puli elastycznej SQL, zainicjować i zarejestrowane w wykazie. Po pomyślnym zainicjowaniu obsługi administracyjnej, nowej dzierżawy *zdarzenia* lokacji zostanie wyświetlona w przeglądarce:

![Nowa dzierżawa](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Odśwież *Centrum zdarzeń* i pojawi się na liście nowym dzierżawcą.


## <a name="explore-the-servers-pools-and-tenant-databases"></a>Sprawdzanie działania serwerów, pul i baz danych dzierżaw

Teraz, gdy rozpoczęciu uruchamiania obciążenia kolekcję dzierżawców przyjrzymy zasobów, które zostały wdrożone:

1. W [portalu Azure](http://portal.azure.com), przejdź do listy serwerów SQL i Otwórz **katalogu-dpt -&lt;użytkownika&gt;**  serwera. Serwer katalogu zawiera dwie bazy danych, **tenantcatalog** i **basetenantdb** (szablonu bazy danych, która jest kopiowana do utworzenia nowego dzierżawcy).

   ![bazy danych](./media/saas-dbpertenant-get-started-deploy/databases.png)

1. Wróć do listy serwerów SQL i otworzyć **tenants1-dpt -&lt;użytkownika&gt;**  serwera baz danych dzierżawy. Każda baza danych dzierżawy jest _elastycznej standardowe_ bazy danych w puli standardowej 50 eDTU. Ponadto istnieje _Racing klon czerwony_ bazy danych, bazy danych dzierżawy aprowizowanej wcześniej.

   ![serwer](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>Monitorowanie puli

Generator obciążenia działający przez kilka minut powinien dostarczyć wystarczającej ilości danych, aby można było rozpocząć zapoznawanie się z niektórymi możliwościami monitorowania wbudowanymi w pule i bazy danych.

Przejdź do serwera **tenants1-dpt -&lt;użytkownika&gt;**i kliknij przycisk **Pool1** do wyświetlania wykorzystania zasobów dla puli (generator obciążenia uruchomione na godzinę na poniższych wykresach):

   ![monitorowanie puli](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

Górny wykresu zawiera wykorzystanie eDTU puli, podczas wykres dolnej pokazuje użycie eDTU top 5 baz danych w puli.  Oba wykresy pokazują, jak dobrze elastyczne pule i usługa SQL Database są dostosowane do pracy przy obciążeniu przez aplikacje SaaS. Cztery bazy danych, które są obciążane chwilowym obciążeniem o maksymalnej wartości 40 jednostek eDTU, są z łatwością obsługiwane przez pulę 50 jednostek eDTU. Jeśli zostały one udostępniane jako autonomiczny baz danych, jak każdy muszą być S2 (50 DTU) do obsługi seria. Koszt 4 bazy danych S2 autonomiczny są prawie 3 razy cen puli i puli nadal ma szerokie możliwości rozbudowy dużo więcej baz danych. W sytuacjach rzeczywistych klientów bazy danych SQL są aktualnie uruchomione maksymalnie 500 baz danych w 200 liczbę jednostek eDTU puli. Aby uzyskać więcej informacji, zobacz [samouczek monitorowania wydajności](saas-dbpertenant-performance-monitoring.md).


## <a name="next-steps"></a>Następne kroki

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Wdrażanie aplikacji SaaS biletów Wingtip
> * Informacje o serwerach, pulach i bazach danych składających się na aplikację
> * Mapowanie dzierżaw na ich dane przy użyciu *wykazu*
> * Aprowizacja nowych dzierżaw
> * Wyświetlanie wykorzystania puli w celu monitorowania aktywności dzierżawy
> * Usuwanie przykładowych zasobów w celu zakończenia ich rozliczania

Teraz spróbuj [samouczek udostępniania i wykaz](saas-dbpertenant-provision-and-catalog.md).



## <a name="additional-resources"></a>Dodatkowe zasoby

* Dodatkowe [samouczków, z którymi kompilacji w bazie danych SaaS biletów Wingtip każdej dzierżawy aplikacji](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* Aby dowiedzieć się więcej o elastycznych pulach, zobacz [ *Co to jest pula elastyczna Azure*](sql-database-elastic-pool.md)
* Aby dowiedzieć się więcej o zadaniach elastycznych, zobacz [ *Managing scaled-out cloud databases*](sql-database-elastic-jobs-overview.md) (Zarządzanie skalowalnymi bazami danych w chmurze)
* Aby dowiedzieć się więcej o wielodostępnych aplikacjach SaaS, zobacz [ *Design patterns for multi-tenant SaaS applications*](saas-tenancy-app-design-patterns.md) (Wzorce projektu dla wielodostępnej aplikacji SaaS)
