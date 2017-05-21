---
title: "Aprowizowanie nowych dzierżaw i rejestrowanie ich w wykazie (przykładowa aplikacja SaaS korzystająca z usługi Azure SQL Database) | Dokumentacja firmy Microsoft"
description: "Aprowizowanie nowych dzierżaw i rejestrowanie ich w wykazie"
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
ms.openlocfilehash: 4eeada941f8615fa04624bc725efcb44f05d56c7
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017


---
# <a name="provision-new-tenants-and-register-them-in-the-catalog"></a>Aprowizowanie nowych dzierżaw i rejestrowanie ich w wykazie

W tym samouczku omówimy aprowizowanie nowych dzierżaw w aplikacji SaaS o nazwie Wingtip Ticket Platform (WTP). Utworzymy dzierżawy, bazy danych dzierżaw oraz zarejestrujemy dzierżawy w wykazie. *Wykaz* to baza danych, która utrzymuje mapowanie między wieloma dzierżawami aplikacji SaaS i ich danymi. Udostępnione tu skrypty pozwolą zapoznać się z używanymi wzorcami aprowizacji i rejestrowania w wykazie oraz dowiedzieć się, jak przebiega implementacja rejestrowania nowych dzierżaw w wykazie. Wykaz odgrywa ważną rolę w kierowaniu żądań aplikacji do właściwych baz danych.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Aprowizowanie pojedynczej nowej dzierżawy
> * Aprowizowanie partii dodatkowych dzierżaw
> * Szczegóły aprowizacji nowych dzierżaw i rejestrowania ich w wykazie


Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Wdrożona jest aplikacja WTP. Aby wdrożyć tę aplikację w czasie krótszym niż pięć minut, zobacz [Wdrażanie i korzystanie z aplikacji SaaS o nazwie WTP](sql-database-saas-tutorial.md)
* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-saas-catalog-pattern"></a>Wprowadzenie do wzorca wykazu SaaS

W przypadku aplikacji SaaS z wieloma dzierżawami opartej o bazę danych należy wiedzieć, gdzie są przechowywane informacje dla każdej dzierżawy. We wzorcu wykazu SaaS baza danych wykazu służy do przechowywania mapowania między dzierżawami i lokalizacjami przechowywania ich danych. Aplikacja WTP wykorzystuje architekturę bazy danych z pojedynczą dzierżawą, ale podstawowy wzorzec przechowywania mapowania dzierżawy na bazę danych w wykazie jest stosowany zarówno w przypadku bazy z wieloma dzierżawami, jak i z jedną dzierżawą.

Każda dzierżawa ma przypisany klucz, który wyróżnia jej dane w wykazie. W przypadku aplikacji WTP klucz jest tworzony na podstawie funkcji skrótu nazwy dzierżawy. Ten wzorzec umożliwia użycie części adresu URL będącej nazwą dzierżawy do konstruowania klucza i uzyskania połączenia z określoną dzierżawą. Użycie innych schematów opartych o identyfikator nie wpływa na ogólny wzorzec.

Wykaz aplikacji WTP został zaimplementowany przy użyciu technologii zarządzania fragmentami w znajdującej się w bibliotece [EDCL (Elastic Database Client Library)](sql-database-elastic-database-client-library.md). Biblioteka EDCL jest odpowiedzialna za tworzenie i zarządzanie opartym na bazie danych _wykazem_, w którym jest obsługiwane _dzielenie map na fragmenty_. Wykaz zawiera mapowanie pomiędzy kluczami (dzierżawami) i ich bazami danych (fragmentami).

> [!IMPORTANT]
> Dane mapowania są dostępne w bazie danych wykazu, ale *nie należy ich edytować*. Dane mapowania można edytować wyłącznie za pomocą interfejsu API biblioteki klienckiej Elastic Database. Bezpośrednie manipulowanie danymi mapowania wiąże się z ryzykiem uszkodzenia wykazu i dlatego nie jest obsługiwane.

Aplikacja SaaS o nazwie Wingtip aprowizuje nowe dzierżawy, kopiując *wzorcową* bazę danych.

## <a name="get-the-wingtip-application-scripts"></a>Pobieranie skryptów aplikacji Wingtip

Skrypty i kod źródłowy aplikacji Wingtip Tickets są dostępne w repozytorium GitHub [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Pliki skryptów znajdują się w folderze [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Pobierz folder **Learning Modules** na komputer lokalny, zachowując jego strukturę folderów.

## <a name="provision-a-new-tenant"></a>Aprowizacja nowej dzierżawy

Jeśli podczas pracy z pierwszym samouczkiem aplikacji WTP utworzono już dzierżawę, można przejść do następnej sekcji: [Aprowizowanie partii dzierżaw](#provision-a-batch-of-tenants).

Uruchom skrypt *Demo-ProvisionAndCatalog*, aby szybko utworzyć dzierżawę i zarejestrować ją w wykazie:

1. Otwórz moduł **Demo-ProvisionAndCatalog.ps1** w programie PowerShell ISE i ustaw następujące wartości:
   * **$TenantName** = nazwa nowego miejsca (na przykład *Bushwillow Blues*). 
   * **$VenueType** = jeden ze wstępnie zdefiniowanych typów miejsca: blues, classicalmusic, dance, jazz, judo, motorracing, multipurpose, opera, rockmusic, soccer.
   * **$DemoScenario** = 1, pozostaw tę wartość ustawioną na _1_, **Provision a single tenant** (Aprowizacja jednej dzierżawy).

1. Naciśnij klawisz **F5** i uruchom skrypt.

Po ukończeniu działania skryptu nowa dzierżawa zostanie zaaprowizowana, a w przeglądarce otworzy się aplikacja *Events*:

![Nowa dzierżawa](./media/sql-database-saas-tutorial-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Aprowizowanie partii dzierżaw

Tematem tego ćwiczenia jest aprowizowanie partii dodatkowych dzierżaw. Zaleca się, aby to ćwiczenie ukończyć przed ukończeniem pozostałych samouczków dotyczących aplikacji WTP.

1. Otwórz moduł ...\\Learning Modules\\Utilities\\*Demo-ProvisionAndCatalog.ps1* w programie *PowerShell ISE* i ustaw następującą wartość:
   * **$DemoScenario** = **3**, ustaw wartość **3**, **Provision a batch of tenants** (Aprowizacja partii dzierżaw).
1. Naciśnij klawisz **F5** i uruchom skrypt.

Skrypt wdroży partię dodatkowych dzierżaw. Używa [szablonu usługi Azure Resource Manager](../azure-resource-manager/resource-manager-template-walkthrough.md), który kontroluje partię i następnie deleguje aprowizację każdej bazy danych do dołączonego szablonu. Ten sposób użycia szablonów umożliwia aplikacji Azure Resource Manager pełnienie roli brokera w procesie aprowizacji przy użyciu skryptu. Szablony próbują równolegle zaaprowizować bazy danych, a jeśli to konieczne, obsługują ponowne próby aprowizacji, optymalizując cały proces. Skrypt jest idempotentny, więc po jego przerwaniu należy uruchomić go ponownie.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Sprawdzanie pomyślnego wdrożenia partii dzierżaw

* Otwórz serwer *tenants1* w witrynie [Azure Portal](https://portal.azure.com) i kliknij przycisk **Bazy danych SQL**:

   ![lista baz danych](media/sql-database-saas-tutorial-provision-and-catalog/database-list.png)


## <a name="provision-and-catalog-detailed-walkthrough"></a>Szczegółowy przewodnik po aprowizacji i wykazie

Dla lepszego zrozumienia sposobu implementacji inicjowania obsługi nowej dzierżawy przez aplikację Wingtip uruchom ponownie skrypt *Demo-ProvisionAndCatalog* i zaaprowizuj kolejną dzierżawę. Tym razem dodaj punkt przerwania i przejdź przez przepływ pracy:

1. Otwórz moduł ...\\Learning Modules\Utilities\_Demo-ProvisionAndCatalog.ps1_ i ustaw dla nowej dzierżawy następujące wartości, które nie istnieją w bieżącym wykazie:
   * **$TenantName** = ustaw nową nazwę (na przykład *Hackberry Hitters*).
   * **$VenueType** = użyj jednego ze wstępnie zdefiniowanych typów miejsca (na przykład *judo*).
   * **$DemoScenario** = 1, ustaw na **1** **Provision a single tenant** (Aprowizacja jednej dzierżawy).

1. Dodaj punkt przerwania, umieszczając kursor w dowolnym miejscu w wierszu *New-Tenant `* i naciśnij klawisz **F9**.

   ![punkt przerwania](media/sql-database-saas-tutorial-provision-and-catalog/breakpoint.png)

1. Naciśnij klawisz **F5**, aby uruchomić skrypt. Po osiągnięciu punktu przerwania naciśnij klawisz **F11**, aby wejść do funkcji. Śledź wykonywanie skryptu, używając klawiszy **F10** i **F11**, odpowiednio do pomijania lub do wchodzenia do wywoływanych funkcji. [Porady dotyczące pracy ze skryptami PowerShell i ich debugowania](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)

### <a name="examine-the-provision-and-catalog-implementation-in-detail-by-stepping-through-the-script"></a>Szczegółowe badanie implementacji aprowizacji i wykazu poprzez krokowe wykonywanie skryptu

Skrypt aprowizuje nowe dzierżawy i rejestruje je w wykazie, wykonując następujące czynności:

1. **Zaimportuj moduł SubscriptionManagement.psm1**, który zawiera funkcje logowania się w usłudze Azure i wybierania używanej subskrypcji platformy Azure.
1. **Zaimportuj moduł CatalogAndDatabaseManagement.psm1**, który tworzy warstwę abstrakcji wykazu i dzierżawy nad funkcjami [zarządzania fragmentami](sql-database-elastic-scale-shard-map-management.md). Jest to ważny moduł, który hermetyzuje znaczną część wzorca wykazu, i z tego powodu jest wart poznania.
1. **Uzyskaj szczegółowe informacje o konfiguracji**. Wejdź do funkcji _Get-Configuration_ (używając klawisza **F11**) i zobacz, jak określono konfigurację aplikacji. W tym miejscu są zdefiniowane nazwy zasobów i innych wartości aplikacji. Nie zmieniaj żadnej z nich, dopóki nie zapoznasz się ze skryptami.
1. **Pobierz obiekt wykazu**. Wejdź do funkcji *Get-Catalog* aby zobaczyć, jak przebiega inicjowanie wykazu za pomocą funkcji zarządzania fragmentami, które są importowane z modułu **AzureShardManagement.psm1**. Wykaz składa się z następujących obiektów:
   * Obiekt $catalogServerFullyQualifiedName jest konstruowany przy użyciu standardowego rdzenia uzupełnionego o nazwę użytkownika: _catalog-\<Użytkownik\>.database.windows.net_.
   * Obiekt $catalogDatabaseName jest pobierany z konfiguracji: *tenantcatalog*.
   * Obiekt $shardMapManager jest inicjowany z bazy danych wykazu.
   * Obiekt $shardMap jest inicjowany z mapy fragmentów *tenantcatalog* w bazie danych wykazu.
   Obiekt wykazu jest tworzony i zwracany oraz używany w skrypcie wyższego poziomu.
1. **Oblicz nowy klucz dzierżawy**. Funkcja skrótu służy do tworzenia klucza dzierżawy na podstawie nazwy dzierżawy.
1. **Sprawdź, czy klucz dzierżawy już istnieje**. Wykaz jest sprawdzany w celu zapewnienia, że klucz jest dostępny.
1. **Aprowizacja bazy danych jest wykonywana przy użyciu obiektu New-TenantDatabase.** Użyj klawisza **F11**, aby wejść do funkcji i zobaczyć, jak jest przeprowadzana aprowizacja bazy danych przy użyciu szablonu usługi Resource Manager.
    
Nazwa bazy danych jest tworzona na podstawie nazwy dzierżawy, aby było jasne, który fragment należy do której dzierżawy. (Można bez problemów użyć innych strategii tworzenia nazw baz danych).

Szablon usługi Resource Manager jest używany do **tworzenia bazy danych poprzez kopiowanie *wzorcowej* bazy danych** (baseTenantDB) na serwer wykazu.  Alternatywną metodą może być utworzenie pustej bazy danych i następnie zainicjowanie jej przez import obiektu bacpac.

Szablon usługi Resource Manager znajduje się w folderze …\\Learning Modules\\Common\\: *tenantdatabasecopytemplate.json*

Po utworzeniu bazy danych dzierżawy jest ona dalej inicjowana przy użyciu nazwy miejsca (dzierżawa) i typu miejsca. Można również stosować inne metody inicjowania.

Baza danych dzierżawy jest zarejestrowana w wykazie przy użyciu funkcji *Add-TenantDatabaseToCatalog* i klucza dzierżawy. Użyj klawisza **F11**, aby wejść do funkcji:

* Baza danych wykazu jest dodawana do mapy fragmentów (listy znanych baz danych).
* Tworzone jest mapowanie łączące wartość klucza z fragmentem.
* Dodawane są dodatkowe metadane dotyczące dzierżawy (nazwa miejsca).

Po ukończeniu aprowizacji wykonywanie powraca do początkowego skryptu *Demo-ProvisionAndCatalog* i w przeglądarce zostaje otwarta strona **zdarzeń** dotycząca nowej dzierżawy:

   ![zdarzenia](media/sql-database-saas-tutorial-provision-and-catalog/new-tenant2.png)


## <a name="other-provisioning-patterns"></a>Inne wzorce aprowizacji

Inne wzorce aprowizacji, których nie uwzględniono w tym samouczku, obejmują:

**Wstępne aprowizowanie baz danych.** Ten wzorzec wykorzystuje fakt, że bazy danych w elastycznej puli nie wprowadzają dodatkowych kosztów (rozliczenia dotyczą elastycznej puli, a nie baz danych), oraz że bezczynna baza danych nie korzysta z żadnych zasobów. Wstępne aprowizowanie baz danych w puli, a następnie przydzielanie ich w razie potrzeby, może znacznie skrócić czas wdrożenia dzierżawy. Liczbę wstępnie aprowizowanych baz danych można dostosować do własnych potrzeb, aby zachować bufor odpowiedni dla oczekiwanego wskaźnika aprowizacji.

**Automatyczne aprowizowanie.** W tym wzorcu dedykowana usługa aprowizacji automatycznie (w razie potrzeby) aprowizuje serwery, pule i bazy danych. Obejmuje to także wstępne aprowizowanie baz danych w elastycznej puli, jeśli zachodzi taka konieczność. W przypadku wycofania i usunięcia baz danych luki w elastycznych pulach mogą zostać uzupełnione przez usługę aprowizacji, jeśli zachodzi taka potrzeba. Taka usługa może być prosta lub złożona (np. aprowizacja w wielu regionach) i może doprowadzić do automatycznego skonfigurowania replikacji geograficznej, jeśli taka strategia jest używana przy odzyskiwaniu po awarii. Za pomocą wzorca automatycznej aprowizacji aplikacja klienta lub skrypt mogą umieścić w kolejce żądanie aprowizacji, które zostanie przetworzone przez usługę aprowizacji, a następnie mogą sondować usługę w celu wykrycia ukończenia zadania. Jeśli używana jest wstępna aprowizacja, żądania mogą zostać szybko obsłużone przez usługę zarządzającą aprowizacją zastępczej bazy danych uruchomioną w tle.


## <a name="stopping-wingtip-saas-application-related-billing"></a>Zatrzymywanie rozliczeń dotyczących aplikacji SaaS o nazwie Wingtip

Jeśli nie zamierzasz korzystać z następnych samouczków, zalecamy usunięcie wszystkich zasobów w celu wstrzymania rozliczeń. Usuń grupę zasobów, w której była wdrożona aplikacja WTP — spowoduje to także usunięcie wszystkich jej zasobów.

* Przejdź do grupy zasobów aplikacji w portalu i usuń ją, aby zatrzymać wszystkie rozliczenia powiązane z tym wdrożeniem aplikacji WTP.

## <a name="tips"></a>Porady

* Biblioteka EDCL udostępnia także ważne funkcje, które umożliwiają aplikacjom klienta nawiązywania połączenia z wykazem i wykonywanie na nim operacji. Biblioteki EDCL można użyć do uzyskania połączenia ADO.NET dla danej wartości klucza, umożliwiając aplikacji połączenie się z odpowiednią bazą danych. Informacje o połączeniu są przechowywane przez klienta, dzięki czemu jest zminimalizowany ruch do bazy danych wykazu, a aplikacja działa szybciej.



## <a name="next-steps"></a>Następne kroki

W tym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Aprowizowanie pojedynczej nowej dzierżawy
> * Aprowizowanie partii dodatkowych dzierżaw
> * Szczegóły aprowizacji nowych dzierżaw i rejestrowania ich w wykazie

[Samouczek monitorowania wydajności](sql-database-saas-tutorial-performance-monitoring.md)

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dodatkowe samouczki nawiązujące do początkowego wdrożenia aplikacji Wingtip Tickets Platform (WTP)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Biblioteka kliencka Elastic Database](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library)
* [Sposób debugowania skryptów w programie Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)

