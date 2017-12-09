---
title: "Samouczek SaaS bazy danych dla dzierżawcy — baza danych SQL Azure | Dokumentacja firmy Microsoft"
description: "Wdrażanie i Eksploruj aplikacji wielodostępne Wingtip biletów SaaS, który demonstruje bazy danych dla każdej dzierżawy i innymi wzorami SaaS przy użyciu bazy danych SQL Azure."
keywords: "samouczek usługi sql database"
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: genemi
ms.openlocfilehash: 5342b5290fab9826a2b38cd7ada63a6736c77601
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="deploy-and-explore-a-multi-tenant-saas-application-that-uses-the-database-per-tenant-pattern-with-azure-sql-database"></a>Wdrażanie i Eksploruj wielodostępnych aplikacji SaaS, korzystającej z bazy danych na wzorzec dzierżawcy z bazy danych SQL Azure

W tym samouczku, wdrażanie i Eksploruj SaaS biletów Wingtip *bazy danych dla każdego dzierżawcy* aplikacji (Wingtip). Aplikacja korzysta z bazy danych na wzorzec dzierżawy do przechowywania danych wielu dzierżawców. Aplikacja jest przeznaczona do pokazują funkcje bazy danych SQL Azure, upraszczające włączania scenariusze SaaS.

Pięć minut po kliknięciu przycisk niebieski **wdrażanie na platformie Azure**, masz wielodostępnych aplikacji SaaS. Aplikacja zawiera bazy danych SQL Azure w chmurze Microsoft Azure. Aplikacja jest wdrażana z trzech dzierżaw próbki, każda z własną bazę danych. Wszystkie bazy danych są wdrażane w SQL *puli elastycznej*. Aplikacja jest wdrażana na Twojej subskrypcji platformy Azure. Masz pełny dostęp do eksplorowania i pracować z poszczególnych składników aplikacji. C# kodu źródłowego aplikacji i skrypty zarządzania są dostępne w [repozytorium GitHub WingtipTicketsSaaS DbPerTenant][github-wingtip-dpt].

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> - Jak wdrożyć aplikację Wingtip SaaS.
> - Gdzie można pobrać kodu źródłowego aplikacji i skrypty zarządzania.
> - Dotyczące serwerów, pul i baz danych, które tworzą aplikacji.
> - Jak dzierżawcy są mapowane do swoich danych z *katalogu*.
> - Sposób obsługi administracyjnej nowej dzierżawy.
> - Jak monitorować działania dzierżawy w aplikacji.

A [serii samouczków pokrewne](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) oferuje zapoznać się z różnych wzorców projektowania i zarządzania SaaS. Tworzenie samouczków poza tym początkowe wdrożenie.
Gdy używasz samouczków widać, implementowania różnych wzorców SaaS, sprawdzając podana skryptów.
Skrypty pokazują, jak uprościć tworzenie aplikacji SaaS w funkcji bazy danych SQL.

## <a name="prerequisites"></a>Wymagania wstępne

Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Zainstalowany jest program Azure PowerShell. Aby uzyskać więcej informacji, zobacz [wprowadzenie do programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-the-wingtip-tickets-saas-application"></a>Wdrażanie aplikacji SaaS biletów Wingtip

Wdróż aplikację:

1. Wybierz i zapamiętać wartości potrzebnych dla następujących parametrów:

    - **Użytkownik**: Wybierz wartość krótkie, na przykład z Twoimi inicjałami następuje cyfrę. Na przykład *af1*. Parametr użytkownika może zawierać tylko litery, cyfry i łączniki (bez spacji). Pierwszy i ostatni znak musi być literą lub cyfrą. Zaleca się wszystkie litery małe litery.
    - **Grupa zasobów**: zawsze możesz wdrożyć aplikację Wingtip, musisz wybrać inną unikatową nazwę dla nowej grupy zasobów. Firma Microsoft zaleca dołączyć nazwę użytkownika na nazwę podstawową dla grupy zasobów. Przykładowa nazwa grupy zasobów może być *wingtip af1*. Ponownie zaleca się wszystkie litery małe litery.

2. Otwórz bazy danych SaaS biletów Wingtip na szablon wdrożenia dzierżawy w portalu Azure, klikając niebieski **wdrażanie na platformie Azure** przycisku.

   <a href="https://aka.ms/deploywingtipdpt" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

3. Do szablonu wprowadź wartości dla wymaganych parametrów:

    > [!IMPORTANT]
    > Niektóre uwierzytelnienia i zapory serwera są celowo niezabezpieczone w celach demonstracyjnych. Zalecamy, aby użytkownik *Utwórz nową grupę zasobów*. Nie należy używać istniejącej grupy zasobów, serwerami lub pul. Nie należy używać tej aplikacji, skryptów lub wszystkie zasoby wdrożone w środowisku produkcyjnym. Po zakończeniu pracy z aplikacją usuń tę grupę zasobów, aby zatrzymać ich rozliczanie.

    - **Grupa zasobów** — wybierz tę opcję **Utwórz nowy** i podaj unikatowy **nazwa** wybrany wcześniej dla grupy zasobów. 
    - **Lokalizacja** — wybierz tę opcję **lokalizacji** z listy rozwijanej.
    - **Użytkownik** -Użyj wybranymi wartości nazwy użytkownika.

4. Wdrażanie aplikacji.

    - Kliknij, aby zaakceptować warunki i postanowienia.
    - Kliknij pozycję **Kup**.

5. Monitorowanie stanu wdrożenia, klikając **powiadomienia**, która jest ikonę dzwonka w prawej strony pola wyszukiwania. Wdrażanie aplikacji SaaS biletów Wingtip trwa około pięciu minut.

   ![wdrażanie zakończyło się pomyślnie](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-tickets-management-scripts"></a>Pobierz i odblokować biletów Wingtip skrypty zarządzania

Gdy aplikacja jest wdrażana, Pobierz skrypty kodu i zarządzania źródła.

> [!IMPORTANT]
> Zawartość pliku wykonywalnego (skrypty, biblioteki dll) mogą być blokowane przez system Windows, gdy pliki zip są pobierane z zewnętrznego źródła i wyodrębnione. Podczas wyodrębniania skryptów z pliku zip, wykonaj następujące kroki, aby odblokować przed wyodrębniania pliku zip. Odblokowanie gwarantuje, że można uruchamiać skrypty.

1. Przejdź do [repozytorium GitHub WingtipTicketsSaaS DbPerTenant][github-wingtip-dpt].
2. Kliknij przycisk **klonowania lub pobierania**.
3. Kliknij przycisk **Pobierz ZIP**, a następnie zapisz plik.
4. Kliknij prawym przyciskiem myszy **WingtipTicketsSaaS-DbPerTenant-master.zip** pliku, a następnie wybierz **właściwości**.
5. Na **ogólne** wybierz opcję **Odblokuj**, a następnie kliknij przycisk **Zastosuj**.
6. Kliknij przycisk **OK**.
7. Wyodrębnij pliki.

Skrypty znajdują się w *... \\WingtipTicketsSaaS DbPerTenant wzorca\\modułów uczenia* folderu.

## <a name="update-the-user-configuration-file-for-this-deployment"></a>Aktualizuj plik konfiguracji użytkownika dla tego wdrożenia

Przed uruchomieniem skrypty należy zaktualizować *grupy zasobów* i *użytkownika* wartości w **UserConfig.psm1**. Ustaw zmienne używane podczas wdrażania.

1. W *PowerShell ISE*, Otwórz... \\Modułów szkoleniowych\\*UserConfig.psm1* 
2. Aktualizacja *ResourceGroupName* i *nazwa* z określonymi wartościami dla danego wdrożenia (wierszach 10 i 11 tylko).
3. Zapisać zmiany!

Te wartości są wymienione w niemal każdego skryptu.

## <a name="run-the-application"></a>Uruchamianie aplikacji

Miejsc, które hostują zdarzeń aplikacji, których są wyświetlane. Właściwość typy koncertowe, jazz klubów i sportowych. W biletów Wingtip miejsc jest zarejestrowana jako dzierżaw. Trwa dzierżawcy udostępnia miejsce łatwy sposób listy zdarzeń i sprzedawać bilety klientom. Każda właściwość pobiera spersonalizowane witryny sieci web, aby wyświetlić ich zdarzenia i sprzedawać bilety.

Wewnętrznie w aplikacji, każdy dzierżawca pobiera wdrożonych w puli elastycznej SQL bazy danych SQL.

Centralnego **Centrum zdarzeń** strona zawiera listę łączy do dzierżawców w danym wdrożeniu.

1. Otwórz *Centrum zdarzeń* w przeglądarce sieci web: http://events.wingtip-dpt.&lt;użytkownika&gt;. trafficmanager.net (Zastąp &lt;użytkownika&gt; z wartością użytkownika danego wdrożenia):

    ![centrum zdarzeń](media/saas-dbpertenant-get-started-deploy/events-hub.png)

2. Kliknij przycisk **Fabrikam Jazz Club** w *Centrum zdarzeń*.

    ![Zdarzenia](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)

#### <a name="azure-traffic-manager"></a>Azure Traffic Manager

Aplikacja Wingtip używa [ *usługi Azure Traffic Manager* ](../traffic-manager/traffic-manager-overview.md) Aby kontrolować dystrybucję żądań przychodzących. Adres URL, aby dostęp do Centrum zdarzeń dla jednego dzierżawcy muszą przestrzegać następujący format:

- http://events.Wingtip-DPT.&lt;użytkownika&gt;.trafficmanager.net/fabrikamjazzclub

W poniższej tabeli opisano części poprzedniego formatu.

| Część adresu URL | Opis |
| :------- | :---------- |
| http://events.Wingtip-DPT | Zdarzenia części aplikacji Wingtip.<br /><br />***-dpt*** odróżnia części *bazy danych dla dzierżawy* implementacji Wingtip z innych implementacji nieco inne. Na przykład inne artykuły dokumentacji oferują Wingtip dla *Standalong* (*-sa*), lub *DB wielodostępne*. |
| .  *&lt;UŻYTKOWNIKA&gt;* | *af1* w naszym przykładzie. |
| .trafficmanager.NET/ | Menedżer ruchu Azure bazowy adres URL. |
| fabrikamjazzclub | Dla dzierżawcy o nazwie *klub Jazz Fabrikam*. |
| &nbsp; | &nbsp; |

1. Nazwa dzierżawy jest analizowana z adresu URL, za pomocą zdarzeń aplikacji.
2. Nazwa dzierżawy jest używany do utworzenia klucza.
3. Klucz jest używany do uzyskać dostęp do katalogu, aby uzyskać lokalizacji dzierżawcy w bazie danych.
    - Katalog jest implementowane za pomocą *zarządzania mapy niezależnego fragmentu*.
4. *Centrum zdarzeń* używa rozszerzonych metadanych w katalogu do uzyskiwania listy adresów URL zdarzenia.

W środowisku produkcyjnym zwykle należy utworzyć rekord CNAME DNS, aby [ *punktu firmowej domeny internetowej* ](../traffic-manager/traffic-manager-point-internet-domain.md) profilu Menedżera ruchu.

## <a name="start-generating-load-on-the-tenant-databases"></a>Rozpoczęcie generowanie obciążenia baz danych dzierżawy

Teraz, gdy aplikacja jest wdrożona umieśćmy go do pracy!
*LoadGenerator pokaz* pracą dzierżawy w przypadku wszystkich baz danych uruchamia skrypt programu PowerShell.
Rzeczywiste obciążenie wiele aplikacji SaaS jest sporadyczne i nieprzewidywalny.
Aby symulować tego rodzaju obciążenia, generator tworzy obciążenia losowego nagłego lub seria działania każdego dzierżawcy.
Seria wystąpić losowego odstępach czasu.
Trwa wzorzec obciążenia wyłaniać się w ciągu kilku minut. Dlatego jest najlepszym rozwiązaniem generator uruchamiania na co najmniej trzech lub czterech minut przed monitorowania obciążenia.

1. W *PowerShell ISE*, Otwórz... \\Modułów uczenia\\narzędzia\\*LoadGenerator.ps1 pokaz* skryptu.
2. Naciśnij klawisz **F5** Uruchom skrypt i uruchom generator obciążenia. (Pozostaw domyślne wartości parametrów teraz.)

Nie należy używać tego samego wystąpienia programu PowerShell ISE dla żadnych innych niż prawdopodobnie Uruchom z *LoadGenerator.ps1 pokaz*. Jeśli musisz uruchomić inne skrypty programu PowerShell, uruchom oddzielne PowerShell ISE.

#### <a name="rerun-with-different-parameters"></a>Uruchom ponownie z innymi parametrami

Jeśli chcesz ponownie uruchomić test obciążenia o innych parametrach, wykonaj następujące kroki:

1. Zatrzymaj *LoadGenerator.ps1*.
    - Użyj **klawisze Ctrl + C**, lub kliknij przycisk **zatrzymać** przycisku.
    - Ta zatrzymaniu zatrzymać ani nie wpływają na wszystkie zadania tła niekompletne, które nadal działają.

2. Uruchom ponownie *LoadGenerator.ps1 pokaz*.
    - Uruchom ten najpierw zatrzymuje tych zadań w tle, które mogą być uruchomione *sp_CpuLoadGenerator*.

Lub może obsłużyć tego wystąpienia programu PowerShell ISE zatrzymuje wszystkie zadania w tle. Następnie uruchomić nowe wystąpienie programu PowerShell ISE i ponownie uruchom *LoadGenerator.ps1 pokaz*.

#### <a name="monitor-the-background-jobs"></a>Monitorowanie zadań w tle

Jeśli chcesz kontrolować i monitorować zadań w tle, możesz użyć następujących poleceń cmdlet:

- Get-Job.
- Odbieranie zadania
- Zadanie zatrzymania

#### <a name="demo-loadgeneratorps1-actions"></a>Wersja demonstracyjna LoadGenerator.ps1 akcje

*Wersja demonstracyjna LoadGenerator.ps1* naśladuje aktywne obciążenia transakcji odbiorcy. W poniższych krokach opisano sekwencję akcji który *LoadGenerator.ps1 pokaz* inicjuje:

1. *Wersja demonstracyjna LoadGenerator.ps1* uruchamia *LoadGenerator.ps1* na pierwszym planie.
    - Obydwa typy plików .ps1 są przechowywane w folderach *modułów uczenia\\narzędzia\\*.

2. *LoadGenerator.ps1* pętlę wszystkich baz danych dzierżawy, które są zarejestrowane w wykazie.

3. Dla każdej bazy danych dzierżawy *LoadGenerator.ps1* rozpoczyna się wykonanie Transact-SQL przechowywane procedury o nazwie *sp_CpuLoadGenerator*.
    - Wykonaniami są uruchamiane w tle, wywołując *Invoke SqlAzureWithRetry* polecenia cmdlet.
    - *sp_CpuLoadGenerator* pętli wokół instrukcję SQL SELECT, domyślny czas trwania 60 sekund. Odstęp czasu między problemów wyboru zależy od wartości parametrów.
    - Plik SQL znajduje się w obszarze *WingtipTenantDB\\dbo\\StoredProcedures\\*.

4. Dla każdej bazy danych dzierżawy *LoadGenerator.ps1* również uruchamia *zadania rozpoczęcia* polecenia cmdlet.
    - *Zadanie rozpoczęcia* naśladuje obciążenia sprzedaży biletów.

5. *LoadGenerator.ps1* kontynuuje działanie, monitorowanie nowi dzierżawcy, które są obsługiwane.

&nbsp;

Przed kontynuowaniem do następnej sekcji, pozostaw generator obciążenia uruchomione w stanie wywoływania zadania.

## <a name="provision-a-new-tenant"></a>Aprowizacja nowej dzierżawy

Początkowe wdrożenie tworzy trzy próbki dzierżaw. Teraz możesz utworzyć innego dzierżawcę, aby zobaczyć wpływ na wdrożonej aplikacji. W aplikacji Wingtip przepływu pracy do obsługi administracyjnej nowych dzierżaw zostało wyjaśnione w dokumencie [samouczek udostępniania i wykaz](saas-dbpertenant-provision-and-catalog.md). Na tym etapie tworzenia nowej dzierżawy, który ma mniej niż minutę.

1. W *PowerShell ISE*, Otwórz... \\Learning Modules\Provision i wykaz\\*ProvisionAndCatalog.ps1 pokaz* .
2. Naciśnij klawisz **F5**, aby uruchomić skrypt. (Chwilowo, pozostaw wartości domyślne).

   > [!NOTE]
   > Użyj wielu skryptów Wingtip SaaS *$PSScriptRoot* do przeglądania folderów do wywołania funkcji w innych skryptów. Ta zmienna jest tylko oceniane w czasie pełnego skrypt zostanie wykonany przez naciśnięcie przycisku **F5**.  Wyróżnianie i uruchamianie wybór z **F8** może spowodować błędy. Sposób uruchamiania skryptów przez naciśnięcie przycisku **F5**.

Nowa baza danych dzierżawy jest:

- Utworzony w puli elastycznej SQL.
- Zainicjowane.
- Zarejestrowane w wykazie.

Po pomyślnym zainicjowaniu obsługi administracyjnej, *zdarzenia* lokacji z nowym dzierżawcą pojawi się w przeglądarce:

![Nowa dzierżawa](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Odśwież *Centrum zdarzeń* Aby utworzyć nową dzierżawę znajdują się na liście.

## <a name="explore-the-servers-pools-and-tenant-databases"></a>Sprawdzanie działania serwerów, pul i baz danych dzierżaw

Teraz, gdy rozpoczęciu uruchamiania obciążenia kolekcję dzierżawców przyjrzymy zasobów, które zostały wdrożone:

1. W [portalu Azure](http://portal.azure.com), przejdź do listy serwerów SQL, a następnie otwórz **katalogu-dpt -&lt;użytkownika&gt;**  serwera.
    - Serwer katalogu zawiera dwie bazy danych, **tenantcatalog** i **basetenantdb** (szablonu bazy danych, która jest kopiowana do utworzenia nowego dzierżawcy).

   ![bazy danych](./media/saas-dbpertenant-get-started-deploy/databases.png)

2. Wróć do listy serwerów SQL.

3. Otwórz **tenants1-dpt -&lt;użytkownika&gt;**  serwera baz danych dzierżawy.

4. Zobacz następujące elementy:
    - Każda baza danych dzierżawy jest *elastycznej standardowe* bazy danych w puli standardowej 50 eDTU.
    - *Racing klon czerwony* bazy danych, która jest bazą danych dzierżawy aprowizowanej wcześniej.

   ![serwer](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>Monitorowanie puli

Po *LoadGenerator.ps1* działa na kilka minut, za mało danych powinny być dostępne rozpocząć wyszukiwanie informacji niektóre możliwości monitorowania. Te możliwości są wbudowane w pule i baz danych.

Przejdź do serwera **tenants1-dpt -&lt;użytkownika&gt;**i kliknij przycisk **Pool1** do wyświetlania wykorzystania zasobów w puli. Na poniższych wykresach generator obciążenia uruchomione na godzinę.

   ![monitorowanie puli](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

- Pierwszy wykres z etykietą **wykorzystania zasobów**, użycie jednostek eDTU puli pokazuje.
- Drugi wykres przedstawia użycie eDTU top pięć baz danych w puli.

Dwa wykresy pokazują, że pule elastyczne i bazy danych SQL są dobrze nadają się do obciążeń aplikacji SaaS.
Wykresy oznaczają, że 4 bazy danych są każdego poszerzająca jak 40 jednostek Edtu, które jeszcze wszystkich baz danych wygodnie są obsługiwane przez 50 eDTU puli. 50 eDTU puli może obsługiwać nawet większych obciążeń.
Jeśli zostały one udostępniane jako autonomiczny baz danych, jak każdy muszą być S2 (50 DTU) do obsługi seria.
Koszt 4 bazy danych S2 autonomiczny będzie niemal 3 razy cen puli.
W sytuacjach rzeczywistych klientów bazy danych SQL są aktualnie uruchomione maksymalnie 500 baz danych w 200 liczbę jednostek eDTU puli.
Aby uzyskać więcej informacji, zobacz [samouczek monitorowania wydajności](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

- Dodatkowe [samouczków, z którymi kompilacji w bazie danych SaaS biletów Wingtip każdej dzierżawy aplikacji](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
- Aby dowiedzieć się więcej o elastycznych pulach, zobacz [ *Co to jest pula elastyczna Azure*](sql-database-elastic-pool.md)
- Aby dowiedzieć się więcej o zadaniach elastycznych, zobacz [ *Managing scaled-out cloud databases*](sql-database-elastic-jobs-overview.md) (Zarządzanie skalowalnymi bazami danych w chmurze)
- Aby dowiedzieć się więcej o wielodostępnych aplikacjach SaaS, zobacz [ *Design patterns for multi-tenant SaaS applications*](saas-tenancy-app-design-patterns.md) (Wzorce projektu dla wielodostępnej aplikacji SaaS)


## <a name="next-steps"></a>Następne kroki

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> - Wdrażanie aplikacji SaaS biletów Wingtip
> - Informacje o serwerach, pulach i bazach danych składających się na aplikację
> - Mapowanie dzierżaw na ich dane przy użyciu *wykazu*
> - Aprowizacja nowych dzierżaw
> - Wyświetlanie wykorzystania puli w celu monitorowania aktywności dzierżawy
> - Usuwanie przykładowych zasobów w celu zakończenia ich rozliczania

Następnie spróbuj [samouczek udostępniania i wykaz](saas-dbpertenant-provision-and-catalog.md).



<!-- Link references. -->

[github-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant 

