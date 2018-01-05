---
title: "Zarządzanie schematami usługi Azure SQL Database w aplikacji z wieloma dzierżawami | Microsoft Docs"
description: "Zarządzanie schematami wielu dzierżaw w aplikacji z wieloma dzierżawami, która korzysta z usługi Azure SQL Database"
keywords: "samouczek usługi sql database"
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.reviewers: billgib
ms.author: genemi
ms.openlocfilehash: 135764a7d89dcf711ff7fe9416850f1af9329479
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/04/2018
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Zarządzanie schematu dla wielu dzierżawców w aplikacji wielodostępnych, która używa bazy danych SQL Azure

W tym samouczku sprawdza trudności w zachowaniu potencjalnie ogromną floty baz danych w oprogramowania jako usługa (SaaS) aplikacji w chmurze. Przedstawiono w rozwiązania do zarządzania rozszerzenia schematu, które opracowany i jest zaimplementowany w trakcie cyklu życia aplikacji.

W miarę rozwoju środowisko dowolnej aplikacji, zmiany mogą wystąpić w jego kolumn tabeli lub innych schematu lub jego danych referencyjnych lub wydajność powiązanych elementów. Aplikacja SaaS należy wdrożyć te zmiany w skoordynowany sposób przez wiele baz danych z istniejącej dzierżawy. A te zmiany, muszą być uwzględnione w przyszłych dzierżawy baz danych, które zostaną dodane do aplikacji. W związku z tym zmiany również musi zostać włączone do procesu, który udostępnia nowe bazy danych.

#### <a name="two-scenarios"></a>Dwa scenariusze

W tym samouczku Eksploruje dwa następujące scenariusze:
- Wdróż aktualizacje danych odwołania dla wszystkich dzierżawców.
- Retuning indeksu dla tabeli, która zawiera dane referencyjne.

[Zadania elastyczne](sql-database-elastic-jobs-overview.md) funkcja bazy danych SQL Azure jest używana do wykonywania tych operacji dla dzierżawy baz danych. Zadania również działać w bazie danych dzierżawy złotego szablonu. Ten szablon jest używany podczas przydzielania nowych baz danych.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz konto zadania.
> * Zapytania w wielu dzierżawców.
> * Aktualizuj dane w wszystkie dzierżawy bazy danych.
> * Tworzenie indeksu dla tabeli w wszystkie dzierżawy bazy danych.

## <a name="prerequisites"></a>Wymagania wstępne

- Aplikacja biletów Wingtip już należy wdrożyć:
    - Aby uzyskać instrukcje, zobacz pierwszy samouczek wprowadza *biletów Wingtip* aplikacji bazy danych z wieloma dzierżawcami SaaS:<br />[Wdrażanie i Eksploruj podzielonej aplikacji wielodostępnych, która używa usługi Azure SQL Database](saas-multitenantdb-get-started-deploy.md).
        - Proces wdrażania jest uruchamiany na mniej niż pięć minut.
    - Musi mieć *podzielonej wielodostępne* wersji Wingtip zainstalowane. Wersje *autonomiczny* i *bazy danych dla każdego dzierżawcy* nie obsługują niniejszego samouczka.

- Musi być zainstalowana najnowsza wersja programu SQL Server Management Studio (SSMS). [Pobierz i zainstaluj narzędzia SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

- Musi być zainstalowany program Azure PowerShell. Aby uzyskać więcej informacji, zobacz [wprowadzenie do programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

> [!NOTE]
> W tym samouczku korzysta z funkcji usługi baza danych SQL Azure, które są w podglądzie ograniczone ([zadania elastycznej bazy danych](sql-database-elastic-database-client-library.md)). Jeśli chcesz zrobić w tym samouczku, podaj identyfikator subskrypcji do  *SaaSFeedback@microsoft.com*  z podmiotem = elastycznej Podgląd zadania. Po otrzymaniu potwierdzenia, że Twoja subskrypcja została włączona, [pobierz i zainstaluj najnowsze polecenia cmdlet zadań w wersji wstępnej](https://github.com/jaredmoo/azure-powershell/releases). Ta wersja zapoznawcza jest ograniczony, więc skontaktuj się z  *SaaSFeedback@microsoft.com*  pytania związane z lub pomocy technicznej.

## <a name="introduction-to-saas-schema-management-patterns"></a>Wprowadzenie do wzorców Zarządzanie schematu SaaS

Model podzielonej wielodostępne bazy danych używany w tym przykładzie umożliwia dzierżawcom bazy danych zawiera co najmniej jednego dzierżawcy. W tym przykładzie Eksploruje może używać różnych dzierżawy wielu i jednego dzierżawcy bazy danych, włączanie *hybrydowego* model zarządzania dzierżawy. Zarządzanie tych baz danych jest skomplikowane. Funkcja [Zadania elastyczne](sql-database-elastic-jobs-overview.md) ułatwia administrowanie i zarządzanie warstwą danych programu SQL. Zadania umożliwiają bezpieczne i niezawodne uruchamiać skrypty języka Transact-SQL jako zadania w odniesieniu do grupy baz danych dzierżawy. Zadania są niezależne od działań użytkownika lub danych wejściowych. Ta metoda może służyć do wdrażania zmiany do schematu lub wspólnych danych referencyjnych, we wszystkich dzierżawców w aplikacji. Zadania elastyczne mogą służyć do obsługi uwierzytelniania golden szablonu kopię bazy danych. Ten szablon służy do tworzenia nowych dzierżaw, zawsze zapewnienia najnowszej schematu i dane referencyjne są używane.

![ekran](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Ograniczona wersja zapoznawcza Zadań elastycznych

Brak nowej wersji zadania elastyczne, która jest teraz zintegrowany funkcji bazy danych SQL Azure. Przez zintegrowane możemy oznaczają, że nie wymaga żadnych dodatkowych usług lub składników. Nowa wersja Zadań elastycznych jest obecnie dostępna w ograniczonej wersji zapoznawczej. Ograniczona wersja zapoznawcza obsługuje obecnie PowerShell, aby tworzyć zadania konta i T-SQL, aby tworzyć zadania i zarządzać nimi.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Pobieranie kodu źródłowego aplikacji Wingtip biletów SaaS wielodostępne w bazie danych i skryptów

Skrypty Wingtip biletów SaaS wielodostępne w bazie danych i kodu źródłowego aplikacji są dostępne w [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) repozytorium w witrynie Github. Zobacz [ogólne wskazówki](saas-tenancy-wingtip-app-guidance-tips.md) dla czynności, aby pobrać i odblokować skrypty Wingtip biletów SaaS. 

## <a name="create-a-job-account-database-and-new-job-account"></a>Tworzenie bazy danych konta zadania oraz nowego konta zadania

Ten samouczek wymaga użycia programu PowerShell do tworzenia bazy danych zadania konta i zadania konta. Takie jak bazy danych MSDB używana przez agenta programu SQL do przechowywania definicje zadań, stan zadania i historię zadania elastyczne używane bazy danych Azure SQL. Po utworzeniu zadania konta można tworzyć i natychmiast monitorowania zadań.

1. W **PowerShell ISE**, otwórz *... \\Modułów szkoleniowych\\Zarządzanie schematami\\SchemaManagement.ps1 pokaz*.
2. Naciśnij klawisz **F5**, aby uruchomić skrypt.

*SchemaManagement.ps1 pokaz* skryptu wywołania *SchemaManagement.ps1 Wdróż* skrypt, aby utworzyć warstwę *S2* bazy danych o nazwie **jobaccount** na serwerze wykazu. Skrypt tworzy następnie konta zadania, przekazując jobaccount bazy danych jako parametr do wywołania zadania tworzenia konta.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Tworzenie zadania służącego do wdrożenia nowych danych referencyjnych we wszystkich dzierżawach

#### <a name="prepare"></a>Przygotowanie

Każda baza danych dzierżaw zawiera zestaw typów miejsce w **VenueTypes** tabeli. Typy miejscową definiują typ zdarzenia, które znajdują się w miejscu. W tym ćwiczeniu wdrożenia aktualizacji do wszystkich baz danych, aby dodać dwa typy dodatkowe miejsce: *Racing motocykla* i *klub pływackich*. Te typy miejsc odpowiadają obrazom tła, które widzisz w aplikacji zdarzeń dzierżawy.

Przed przystąpieniem do wdrażania nowych danych referencyjnych, Zanotuj liczbę typów miejsce, które już istnieją, które może być 10. Uwaga podjęcia kliknąć poniższe łącze w sieci Web Wingtip interfejsu użytkownika, a następnie klikając polecenie **typu miejscową** menu rozwijanego:
- http://events.Wingtip-MT.<USER>. trafficmanager.net

Teraz można zliczać liczba typów miejscową pierwotnego. W szczególności należy pamiętać, że ani *Racing motocykla* ani *klub pływackich* jeszcze.

#### <a name="steps"></a>Kroki

Teraz Utwórz zadanie, aby zaktualizować **VenueTypes** tabeli w bazie danych każdego dzierżawcy, dodając dwóch nowych typów miejsce.

Aby utworzyć nowe zadanie, użyć zestawu zadań systemowych procedur składowanych utworzonych w *jobaccount* bazy danych. Procedury zostały utworzone podczas tworzenia zadania konta.

1. W programie SSMS, nawiąż połączenie z serwerem dzierżawy: tenants1-mt -\<użytkownika\>. database.windows.net

2. Przejdź do *tenants1* bazy danych na *tenants1-mt -\<użytkownika\>. database.windows.net* serwera.

3. Zapytanie *VenueTypes* tabeli, aby potwierdzić, że *Racing motocykla* i *klub pływackich* nie są jeszcze na liście wyników.

4. Połączyć się z serwerem katalogu, który jest *katalogu-mt -\<użytkownika\>. database.windows.net*.

5. Połączyć się z *jobaccount* bazy danych na serwerze wykazu.

6. W programie SSMS, otwórz plik *... \\Modułów szkoleniowych\\Zarządzanie schematami\\DeployReferenceData.sql*.

7. Modyfikowanie instrukcji: Ustaw @User = &lt;użytkownika&gt; i zastąp wartość użytkownika używane w przypadku wdrażania aplikacji Wingtip biletów SaaS wielodostępne w bazie danych.

8. Naciśnij klawisz **F5**, aby uruchomić skrypt.

#### <a name="observe"></a>Obserwuj

Sprawdź następujące elementy w *DeployReferenceData.sql* skryptu:

- **SP\_dodać\_docelowej\_grupy** tworzy Nazwa grupy docelowej *DemoServerGroup*, i dodaje docelowy członków do grupy.

- **SP\_dodać\_docelowej\_grupy\_elementu członkowskiego** dodaje następujące elementy:
    - A *serwera* docelowy typ elementu członkowskiego.
        - Jest to *tenants1-mt -&lt;użytkownika&gt;*  serwera, który zawiera bazy danych dzierżawców.
        - W związku z tym wszystkie bazy danych na serwerze są uwzględniane w zadania wykonuje zadanie.
    - A *bazy danych* docelowy typ elementu członkowskiego dla bazy danych złotego (*basetenantdb*) który znajduje się na *katalogu-mt -&lt;użytkownika&gt;*  serwera
    - A *bazy danych* docelowy typ elementu członkowskiego, aby uwzględnić *adhocreporting* bazy danych, która jest używana w późniejszym samouczka.

- **SP\_dodać\_zadania** tworzy zadanie o nazwie *wdrożenia danych odwołania*.

- **SP\_dodać\_etap zadania** tworzy etap zadania zawierające tekst polecenia T-SQL, aby zaktualizować tabeli referencyjnej VenueTypes.

- Pozostałe widoki w skrypcie dotyczą istnienia obiektów i monitorowania wykonania zadań. Aby przejrzeć wartość stanu, użyj tych zapytań **cyklu życia** kolumnę, aby określić, kiedy zadanie zostało ukończone pomyślnie. Zadanie aktualizacji bazy danych dzierżawców i aktualizuje dwóch dodatkowych baz danych zawierających tabeli referencyjnej.

W programie SSMS, przejdź do bazy danych dzierżawy na *tenants1-mt -&lt;użytkownika&gt;*  serwera. Zapytanie *VenueTypes* tabeli, aby potwierdzić, że *Racing motocykla* i *klub pływackich* są teraz dodane do tabeli. Całkowita liczba typów miejscową musi wzrosnąć przez dwa.

## <a name="create-a-job-to-manage-the-reference-table-index"></a>Tworzenie zadania do zarządzania indeksem tabeli referencyjnej

Jest to podobne do poprzednim ćwiczeniu. Tego ćwiczenia tworzy zadanie, aby odbudować indeksu przy użyciu klucza podstawowego tabeli odniesienia. Odbudowywanie indeksu jest operacją zarządzania typowe bazy danych, które administrator mogą zostać uruchomione po załadowaniu dużej ilości danych do tabeli, aby zwiększyć wydajność.

1. W programie SSMS, nawiąż połączenie z *jobaccount* bazy danych w *katalogu-mt -&lt;użytkownika&gt;. database.windows.net* serwera.

2. Otwórz w programie SSMS, *... \\Modułów szkoleniowych\\Zarządzanie schematami\\OnlineReindex.sql*.

3. Naciśnij klawisz **F5**, aby uruchomić skrypt.

#### <a name="observe"></a>Obserwuj

Sprawdź następujące elementy w *OnlineReindex.sql* skryptu:

* **SP\_dodać\_zadania** tworzy nowe zadanie o nazwie *Online ponowna indeksacja PK\_\_VenueTyp\_\_265E44FD7FD4C885*.

* **SP\_dodać\_etap zadania** tworzy etap zadania zawierające tekst polecenia T-SQL do aktualizowania indeksu.

* Pozostałe widoków w skrypcie monitorować wykonywania zadania. Aby przejrzeć wartość stanu, użyj tych zapytań **cyklu życia** kolumnę, aby określić, kiedy zadanie zostało ukończone pomyślnie dla wszystkich członków grupy docelowej.

## <a name="additional-resources"></a>Zasoby dodatkowe

<!-- TODO: Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->
* [Zarządzanie skalowaną w poziomie bazą danych w chmurze](sql-database-elastic-jobs-overview.md)
* [Tworzenie baz danych skalowanych w poziomie i zarządzanie nimi](sql-database-elastic-jobs-create-and-manage.md)

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> - Utwórz konto zadania do badania między wieloma dzierżawcami.
> - Aktualizuj dane w wszystkie dzierżawy bazy danych.
> - Tworzenie indeksu dla tabeli w wszystkie dzierżawy bazy danych.

Następnie spróbuj [samouczek raportowania Ad hoc](saas-multitenantdb-adhoc-reporting.md).

