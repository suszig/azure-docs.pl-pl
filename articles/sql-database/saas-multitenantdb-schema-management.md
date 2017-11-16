---
title: "Zarządzanie schematami usługi Azure SQL Database w aplikacji z wieloma dzierżawami | Microsoft Docs"
description: "Zarządzanie schematami wielu dzierżaw w aplikacji z wieloma dzierżawami, która korzysta z usługi Azure SQL Database"
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
ms.author: billgib
ms.openlocfilehash: 346177be29ec196464f4f441858222ac5d5eb8c3
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>Zarządzanie schematu dla wielu dzierżawców w aplikacji wielodostępnych, która używa bazy danych SQL Azure

[Pierwszy samouczek Wingtip biletów SaaS wielodostępne Database](saas-multitenantdb-get-started-deploy.md) pokazuje, jak udostępnić bazy danych podzielonej wielodostępnej i zarejestruj go w katalogu aplikacji. Podobnie jak wszelkie aplikacje aplikacji SaaS biletów Wingtip rozpoczyna się w czasie, a w czasie będzie wymagać zmiany w bazie danych. Zmiany mogą obejmować nowy lub zmieniony schemat, nowe lub zmienione dane referencyjne, oraz rutynowe zadania konserwacji bazy danych zapewniające optymalną wydajność aplikacji. W przypadku aplikacji SaaS zmiany te muszą zostać wprowadzone w sposób skoordynowany — potencjalnie w bardzo wielu bazach danych dzierżaw. Aby te zmiany można w przyszłości dzierżawy baz danych muszą należy włączyć do procesu inicjowania obsługi administracyjnej.

Ten samouczek analizuje dwa scenariusze — wdrażanie aktualizacji danych referencyjnych dla wszystkich dzierżaw i dostrajanie indeksu tabeli zawierającej dane referencyjne. [Zadania elastyczne](sql-database-elastic-jobs-overview.md) funkcja służy do wykonywania tych operacji na dzierżawy baz danych i *złotego* bazy danych dzierżawy, który służy jako szablon dla nowych baz danych.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Tworzenie zadania konta
> * Wysyłanie zapytań na wielu dzierżawców
> * Aktualizowanie danych we wszystkich bazach danych dzierżaw
> * Tworzenie indeksu tabeli we wszystkich bazach danych dzierżaw


Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Aplikacja Wingtip biletów SaaS wielodostępne w bazie danych jest wdrożona. Aby wdrożyć w mniej niż 5 minut, zobacz [Wdróż i eksplorowanie Wingtip biletów SaaS wielodostępne w bazie danych aplikacji](saas-multitenantdb-get-started-deploy.md)
* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* Zainstalowano najnowszą wersję programu SSMS (SQL Server Management Studio). [Pobieranie i instalowanie programu SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

> [!NOTE]
> *Ten samouczek zakłada użycie funkcji usługi SQL Database, które znajdują się w ograniczonej wersji zapoznawczej (zadania Elastic Database). Aby skorzystać z tego samouczka, prześlij identyfikator swojej subskrypcji na adres SaaSFeedback@microsoft.com z tematem Elastic Job Preview. Po otrzymaniu potwierdzenia, że Twoja subskrypcja została włączona, [pobierz i zainstaluj najnowsze polecenia cmdlet zadań w wersji wstępnej](https://github.com/jaredmoo/azure-powershell/releases). Ta wersja zapoznawcza jest ograniczony, więc skontaktuj się z SaaSFeedback@microsoft.com pytania związane z lub pomocy technicznej.*


## <a name="introduction-to-saas-schema-management-patterns"></a>Wprowadzenie do wzorców zarządzania schematami SaaS

Model podzielonej wielodostępne bazy danych używany w tym przykładzie umożliwia dzierżawcom bazę danych do zawierać dowolną liczbę dzierżaw. W tym przykładzie Eksploruje może używać różnych wielodostępnej i pojedynczej dzierżawy bazy danych, włączanie model zarządzania dzierżawy "hybrydowe". Obsługa i zarządzanie tych baz danych jest skomplikowane. Funkcja [Zadania elastyczne](sql-database-elastic-jobs-overview.md) ułatwia administrowanie i zarządzanie warstwą danych programu SQL. Zadania umożliwiają bezpieczne i niezawodne uruchamiania zleceń (skryptów T-SQL) na grupie baz danych niezależnie od interakcji z użytkownikiem lub danych wejściowych. Tej metody można użyć do wdrożenia schematu i zmiany wspólnych danych referencyjnych we wszystkich dzierżawach w aplikacji. Zadania elastyczne służą do konserwacji *wzorcowej* kopii bazy danych używanej do tworzenia nowych dzierżaw, dzięki czemu zawsze zawiera ona najnowszy schemat i dane referencyjne.

![ekran](media/saas-multitenantdb-schema-management/schema-management.png)

## <a name="elastic-jobs-limited-preview"></a>Ograniczona wersja zapoznawcza Zadań elastycznych

Istnieje nowa wersja Zadań elastycznych, która jest teraz zintegrowaną funkcją usługi Azure SQL Database (nie wymaga żadnych dodatkowych usług ani składników). Nowa wersja Zadań elastycznych jest obecnie dostępna w ograniczonej wersji zapoznawczej. Ograniczona wersja zapoznawcza obsługuje obecnie program PowerShell do tworzenia kont zadań, oraz oprogramowanie T-SQL do tworzenia zadań i zarządzania nimi.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-scripts"></a>Pobierz skrypty aplikacji Wingtip biletów SaaS wielodostępne w bazie danych

Skrypty Wingtip biletów SaaS wielodostępne w bazie danych i kodu źródłowego aplikacji są dostępne w [WingtipTicketsSaaS MultiTenantDB](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) repozytorium GitHub. <!-- [Steps to download the Wingtip Tickets SaaS Multi-tenant Database scripts](saas-multitenantdb-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-saas-scripts)-->

## <a name="create-a-job-account-database-and-new-job-account"></a>Tworzenie bazy danych konta zadania oraz nowego konta zadania

Ten samouczek wymaga użycia programu PowerShell w celu utworzenia bazy danych konta zadania oraz samego konta zadania. Podobnie jak w przypadku aplikacji MSDB i SQL Agent, Zadania elastyczne używają usługi Azure SQL Database do przechowywania definicji zadań, ich stanu i historii. Po utworzeniu konta zadania można natychmiast przystąpić do tworzenia i monitorowania zadań.

1. W **PowerShell ISE**, otwórz *... \\Modułów szkoleniowych\\Zarządzanie schematami\\SchemaManagement.ps1 pokaz*.
1. Naciśnij klawisz **F5**, aby uruchomić skrypt.

Skrypt *Demo-SchemaManagement.ps1* wywołuje skrypt *Deploy-SchemaManagement.ps1* w celu utworzenia bazy danych typu *S2* o nazwie **jobaccount** na serwerze wykazu. Następnie tworzy konto zadania, przekazując bazę danych jobaccount jako parametr wywołania tworzenia konta zadania.

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>Tworzenie zadania służącego do wdrożenia nowych danych referencyjnych we wszystkich dzierżawach

Każda baza danych dzierżaw zawiera zestaw typów miejsce w tabeli **VenueTypes** definiującą typ zdarzenia, które znajdują się w miejscu. W tym ćwiczeniu wdrożenia aktualizacji do wszystkich baz danych można dodać dwa typy dodatkowe miejsce: *Racing motocykla* i *klub pływackich*. Te typy miejsc odpowiadają obrazom tła, które widzisz w aplikacji zdarzeń dzierżawy.

Kliknij menu rozwijane typu miejsca i upewnij się, że jest dostępnych tylko 10 opcji typów miejsc — a w szczególności, że na liście brakuje opcji „Motorcycle Racing” i „Swimming Club”.

Teraz Utwórzmy zadanie do aktualizacji **VenueTypes** tabeli w wszystkich baz danych dzierżawcy i dodawanie nowych typów miejsce.

Aby utworzyć nowe zadanie, użyjemy zestawu procedur składowanych systemu zadań utworzonych w bazie danych jobaccount podczas tworzenia konta zadania.

1. W programie SSMS, nawiąż połączenie z serwerem dzierżawy: tenants1-mt -\<użytkownika\>. database.windows.net
2. Przejdź do *tenants1* bazy danych na *tenants1-mt -\<użytkownika\>. database.windows.net* serwera i zapytań *VenueTypes* do tabeli Upewnij się, że *Racing motocykla* i *klub pływackich* są **nie** na liście wyników.
3. Nawiąż połączenie z serwerem katalogu: katalog-mt -\<użytkownika\>. database.windows.net
4. Połączenia z bazą danych jobaccount na serwerze wykazu.
5. W programie SSMS Otwórz plik... \\Modułów szkoleniowych\\Zarządzanie schematami\\DeployReferenceData.sql
6. Modyfikowanie instrukcji: Ustaw @User = &lt;użytkownika&gt; i zastąp wartość użytkownika używane w przypadku wdrażania aplikacji Wingtip biletów SaaS wielodostępne w bazie danych.
7. Naciśnij klawisz **F5**, aby uruchomić skrypt.

    * **SP\_dodać\_docelowej\_grupy** tworzy grupy docelowej teraz nazwę DemoServerGroup, Dodaj członków docelowych do grupy.
    * **SP\_dodać\_docelowej\_grupy\_elementu członkowskiego** dodaje *serwera* docelowy typ elementu członkowskiego, które uzna za wszystkie bazy danych w ramach tego serwera (należy pamiętać, jest to tenants1 - mt - &lt;użytkownika&gt; server zawierającego bazę danych dzierżawcy) w czasie wykonywania zadań, które mają zostać uwzględnione w zadaniu, *bazy danych* docelowy typ elementu członkowskiego "złotego" bazy danych (basetenantdb) znajdujący się na wykaz-mt -&lt;użytkownika&gt; serwera i na końcu *bazy danych* docelowy typ elementu członkowskiego do dołączenia bazy danych adhocreporting używanego w późniejszym samouczka.
    * **SP\_dodać\_zadania** tworzy zadanie o nazwie "Odwołanie do danych wdrażania".
    * **SP\_dodać\_etap zadania** tworzy etap zadania zawierające tekst polecenia T-SQL, aby zaktualizować tabeli referencyjnej VenueTypes.
    * Pozostałe widoki w skrypcie dotyczą istnienia obiektów i monitorowania wykonania zadań. Aby przejrzeć wartość stanu, użyj tych zapytań **cyklu życia** kolumnę, aby określić, po zakończeniu wykonywania zadania został pomyślnie dzierżaw w bazie danych i dwóch dodatkowych baz danych zawierających tabeli referencyjnej.

1. W programie SSMS, przejdź do bazy danych dzierżawy na *tenants1-mt -&lt;użytkownika&gt;*  serwera i zapytań *VenueTypes* tabeli, aby potwierdzić, że *motocykla Racing* i *klub pływackich* są teraz **dodane* do tabeli.


## <a name="create-a-job-to-manage-the-reference-table-index"></a>Tworzenie zadania do zarządzania indeksem tabeli referencyjnej

Podobnie jak w poprzednim ćwiczeniu, w tym zostanie utworzone zadanie służące do odbudowania indeksu klucza podstawowego tabeli referencyjnej — typowej operacji z zakresu zarządzania bazą danych, którą administrator może wykonać po załadowaniu dużej ilości danych do tabeli.


1. W programie SSMS, nawiąż połączenie z bazy danych jobaccount w katalogu-mt -&lt;użytkownika&gt;. database.windows.net serwera.
2. W programie SSMS Otwórz... \\Modułów szkoleniowych\\Zarządzanie schematami\\OnlineReindex.sql.
3. Naciśnij klawisz **F5** do uruchomienia skryptu

    * **SP\_dodać\_zadania** tworzy nowe zadanie o nazwie "Online PK indeksowanie\_\_VenueTyp\_\_265E44FD7FD4C885".
    * **SP\_dodać\_etap zadania** tworzy etap zadania zawierające tekst polecenia T-SQL do aktualizowania indeksu.
    * Pozostałe widoków w skrypcie monitorować wykonywania zadania. Aby przejrzeć wartość stanu, użyj tych zapytań **cyklu życia** kolumnę, aby określić, kiedy zadanie pomyślnie zakończył na dla wszystkich członków grupy docelowej.

## <a name="next-steps"></a>Następne kroki

W tym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Utworzenie konta zadania do tworzenia zapytań dotyczących wielu dzierżaw
> * Aktualizowanie danych we wszystkich bazach danych dzierżaw
> * Tworzenie indeksu tabeli we wszystkich bazach danych dzierżaw

[Samouczek analitycznych zapytań ad-hoc](saas-multitenantdb-adhoc-reporting.md)


## <a name="additional-resources"></a>Dodatkowe zasoby

<!--* Additional tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application deployment (*Tutorial link to come*)
(saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Zarządzanie skalowaną w poziomie bazą danych w chmurze](sql-database-elastic-jobs-overview.md)
* [Tworzenie baz danych skalowanych w poziomie i zarządzanie nimi](sql-database-elastic-jobs-create-and-manage.md)
