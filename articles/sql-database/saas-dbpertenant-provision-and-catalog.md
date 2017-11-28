---
title: "Udostępnianie nowej dzierżawy w aplikacji wielodostępnych, który używa usługi Azure SQL Database | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie obsługi administracyjnej i w katalogu nowych dzierżaw w wielodostępnych aplikacji SaaS bazy danych SQL Azure"
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
ms.date: 08/11/2017
ms.author: sstein
ms.openlocfilehash: b82623f63681daff502f1e23d052da7480dda942
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="learn-how-to-provision-new-tenants-and-register-them-in-the-catalog"></a>Dowiedz się, jak udostępnić nowi dzierżawcy i zarejestruj je w katalogu

W tym samouczku informacje o wzorce SaaS udostępniania i wykaz i jak są implementowane w bazie danych SaaS biletów Wingtip na aplikację dzierżawy. Tworzenie i zainicjować nowe dzierżawy bazy danych oraz zarejestrować je w katalogu dzierżawy aplikacji. Katalog jest bazy danych, która przechowuje mapowanie między wiele dzierżaw aplikacji SaaS i ich danych. Katalog odgrywa ważną rolę w kierowanie żądań zarządzania prawidłową bazę danych i aplikacji.  

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Udostępnianie jednej nowej dzierżawy, tym krokowe to implementowania
> * Aprowizowanie partii dodatkowych dzierżaw


Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Wingtip biletów SaaS baza każdej dzierżawy aplikacji jest wdrażany. Aby wdrożyć w mniej niż 5 minut, zobacz [Wdróż i eksplorowanie Wingtip biletów SaaS bazy danych na aplikację dzierżawy](saas-dbpertenant-get-started-deploy.md)
* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-saas-catalog-pattern"></a>Wprowadzenie do wzorca wykazu SaaS

W kopii bazy danych wielodostępnych aplikacji SaaS należy wiedzieć, w którym przechowywane są informacje dla każdego dzierżawcy. We wzorcu katalogu SaaS katalogu bazy danych służy do przechowywania mapowanie między każdego dzierżawcy i bazy danych, w którym są przechowywane ich dane. Bazy danych SaaS biletów Wingtip każdej aplikacji dzierżawy używa pojedynczej dzierżawy na modelu bazy danych, ale podstawowy wzorzec przechowywania mapowania dzierżawcy w bazie danych w katalogu ma zastosowanie przy każdym danych dzierżawy jest dystrybuowana do wielu baz danych, czy wielu dzierżawców lub bazy danych z pojedynczej dzierżawy jest używany.

Poszczególni dzierżawcy jest przypisany klucz identyfikujący je w katalogu i która jest mapowana na lokalizację odpowiednie bazy danych. W aplikacji SaaS biletów Wingtip klucz został utworzony z skrót nazwy dzierżawcy. Dzięki temu dzierżawcy część adresu URL aplikacji ma być używany do utworzenia klucza. Można użyć innych systemów klucza dzierżawy.  

Katalog umożliwia nazwę lub lokalizację bazy danych zostanie zmieniony z minimalnym wpływem na aplikację.  W modelu wielodostępnym bazy danych to również bierze pod uwagę "przenoszenia" dzierżawa między bazami danych.  Katalog można również wskazać, czy dzierżawcy lub baza danych jest w trybie offline z powodu konserwacji lub innych działań. Jest to przedstawione w [Przywracanie pojedynczej dzierżawy samouczek](saas-dbpertenant-restore-single-tenant.md).

Ponadto katalog, który jest włączone zarządzanie bazy danych dla aplikacji SaaS, można przechowywać dodatkowe dzierżawy lub metadanych bazy danych, takich jak wersji bazy danych, lub warstwy wersji schematu, plan usługi lub umów SLA oferowane dzierżawców i innych informacji, które umożliwia Zarządzanie aplikacjami, dział obsługi klienta lub procesów opracowywania oprogramowania.  

Poza aplikacji SaaS katalogu można włączyć narzędzi bazy danych.  Wingtip biletów SaaS bazy danych na próbkę dzierżawy, umożliwia programowi zapytania między dzierżawy, przedstawione w katalogu [samouczek analizy ad hoc](saas-tenancy-adhoc-analytics.md). Zarządzanie zadaniami między bazami danych jest przedstawione w [Zarządzanie schematami](saas-tenancy-schema-management.md) i [dzierżawy analytics](saas-tenancy-tenant-analytics.md) samouczki. 

W przykładach SaaS biletów Wingtip katalogu jest implementowane za pomocą funkcji do zarządzania niezależnego fragmentu [elastycznej bazy danych klienta biblioteki (EDCL)](sql-database-elastic-database-client-library.md). EDCL jest dostępna w języku Java i .net Framework. EDCL włącza aplikację do tworzenia, zarządzania i użyć mapy niezależnych kopii bazy danych. Mapy niezależnego fragmentu zawiera listę odłamków (bazy danych) i mapowanie między kluczy (dzierżawcami) i fragmentów.  Funkcje EDCL można z aplikacji i skryptów programu PowerShell podczas inicjowania obsługi można utworzyć wpisów w mapie niezależnego fragmentu dzierżawy i z aplikacji do wydajnie nawiązania połączenia z poprawną bazą danych. EDCL buforuje informacje o połączeniu, aby zminimalizować ruch związany z bazą danych katalogu i przyspieszenie działania aplikacji.  

> [!IMPORTANT]
> Dane mapowania są dostępne w bazie danych wykazu, ale *nie należy ich edytować*. Dane mapowania można edytować wyłącznie za pomocą interfejsu API biblioteki klienckiej Elastic Database. Bezpośrednie manipulowanie danymi mapowania wiąże się z ryzykiem uszkodzenia wykazu i dlatego nie jest obsługiwane.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>Wprowadzenie do wzorca SaaS inicjowania obsługi administracyjnej.

Podczas przechodzenia do nowego dzierżawcy w aplikacji SaaS, która używa modelu bazy danych z pojedynczej dzierżawy nową bazę danych dzierżawcy należy udostępnić.  Muszą być tworzone w odpowiedniej lokalizacji i warstwy usług, zainicjować przy użyciu odpowiedniego schematu i danych referencyjnych, a następnie rejestrowane w katalogu w kluczu odpowiedniego dzierżawcy.  

Różne podejścia może służyć do bazy danych inicjowania obsługi, które mogą obejmować wykonywanie skryptów SQL, wdrażanie pliku bacpac lub kopiowanie szablonu bazy danych.  

Metoda inicjowania obsługi administracyjnej, używanego musi comprehended w strategii zarządzania ogólny schemat, który musi zapewnić, że nowe bazy danych są udostępniane z najnowszą schematu.  Jest to przedstawione w [Samouczek zarządzania schematu](saas-tenancy-schema-management.md).  

Baza danych SaaS biletów Wingtip każdej dzierżawy aplikacji przepisy nowi dzierżawcy przez kopiowanie szablonu bazy danych o nazwie _basetenantdb_, wdrożonych na serwerze wykazu.  Inicjowania obsługi administracyjnej może być zintegrowane z aplikacji w ramach obsługi rejestracji i/lub obsługiwane w trybie offline za pomocą skryptów. W tym samouczku Eksploruje Inicjowanie obsługi przy użyciu programu PowerShell. Skrypty inicjowania obsługi administracyjnej skopiuj basetenantdb bazę danych do utworzenia nowej dzierżawy bazy danych w puli elastycznej, a następnie go zainicjować z użyciem informacji specyficznych dla dzierżawy i zarejestruj go w katalogu niezależnych mapy.  W Wingtip biletów SaaS bazy danych dla aplikacji dzierżawy dzierżawy baz danych są podane nazwy na podstawie nazwy dzierżawy, ale nie jest krytyczną częścią wzorzec — katalogu umożliwiają dowolną nazwę można przypisać do dzierżawy baz danych. + 


## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Pobierz skrypty aplikacji Wingtip biletów SaaS bazy danych dla dzierżawcy

Skrypty Wingtip biletów SaaS wielodostępne w bazie danych i kodu źródłowego aplikacji są dostępne w [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) repozytorium GitHub. Zapoznaj się z [ogólne wskazówki](saas-tenancy-wingtip-app-guidance-tips.md) dla czynności, aby pobrać i odblokować skrypty Wingtip biletów SaaS.


## <a name="provision-and-catalog-detailed-walkthrough"></a>Szczegółowy przewodnik po aprowizacji i wykazie

Aby zrozumieć, jak aplikacja Wingtip implementuje nowej dzierżawy inicjowania obsługi, Dodaj punkt przerwania i krok przez przepływ pracy podczas inicjowania obsługi administracyjnej dzierżawcy:

1. W _PowerShell ISE_, Otwórz... \\Modułów uczenia\\ProvisionAndCatalog\\_ProvisionAndCatalog.ps1 pokaz_ i ustaw następujące parametry:
   * **$TenantName** = nazwa nowego miejsca (na przykład *Bushwillow Blues*).
   * **$VenueType** = jeden z typów wstępnie zdefiniowanych miejscową: *niebieskie*, classicalmusic, tańca, jazz, judo, motorracing uniwersalne, opera, rockmusic, nożną.
   * **$DemoScenario** = **1**, do *udostępnić pojedynczej dzierżawy*.

1. Dodawanie punktu przerwania w dowolnym ustawiając kursor na wiersz 48, informujący: *nowej dzierżawy "*i naciśnij klawisz **F9**.

   ![punkt przerwania](media/saas-dbpertenant-provision-and-catalog/breakpoint.png)

1. Aby uruchomić skrypt naciśnij **F5**.

1. Po zatrzymaniu wykonywania skryptu na punkt przerwania, naciśnij klawisz **F11** do kroku do kodu.

   ![Debugowanie](media/saas-dbpertenant-provision-and-catalog/debug.png)



Śledzenie wykonywanie skryptu za pomocą **debugowania** menu Opcje - **F10** i **F11** do kroku przez lub do funkcji o nazwie. Aby uzyskać więcej informacji na temat debugowania skryptów programu PowerShell, zobacz [porady dotyczące pracy z i debugowania skryptów programu PowerShell](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Poniżej przedstawiono nie kroki do wykonania w sposób jawny, lecz wyjaśnienie przepływu pracy, które można wykonywać krokowo podczas debugowania skryptu:

1. **Zaimportuj moduł SubscriptionManagement.psm1**, który zawiera funkcje logowania się w usłudze Azure i wybierania używanej subskrypcji platformy Azure.
1. **Zaimportuj moduł CatalogAndDatabaseManagement.psm1**, który tworzy warstwę abstrakcji wykazu i dzierżawy nad funkcjami [zarządzania fragmentami](sql-database-elastic-scale-shard-map-management.md). Jest to ważny moduł, który hermetyzuje znaczną część wzorca wykazu, i z tego powodu jest wart poznania.
1. **Uzyskaj szczegółowe informacje o konfiguracji**. Wkrocz Get-konfiguracji (z F11) i zobacz, jak określono w konfiguracji aplikacji. W tym miejscu są zdefiniowane nazwy zasobów i innych wartości aplikacji. Nie zmieniaj żadnej z nich, dopóki nie zapoznasz się ze skryptami.
1. **Pobierz obiekt wykazu**. Krok do katalogu Get, która Redaguj i zwraca obiekt katalogu, który jest używany w skrypcie wyższego poziomu.  Ta funkcja korzysta z funkcji zarządzania niezależnego fragmentu, które są importowane z **AzureShardManagement.psm1**. Obiekt katalogu składa się z następujących czynności:
   * Obiekt $catalogServerFullyQualifiedName jest konstruowany przy użyciu standardowego rdzenia uzupełnionego o nazwę użytkownika: _catalog-\<Użytkownik\>.database.windows.net_.
   * Obiekt $catalogDatabaseName jest pobierany z konfiguracji: *tenantcatalog*.
   * Obiekt $shardMapManager jest inicjowany z bazy danych wykazu.
   * Obiekt $shardMap jest inicjowany z mapy fragmentów *tenantcatalog* w bazie danych wykazu.
   Obiekt wykazu jest tworzony i zwracany oraz używany w skrypcie wyższego poziomu.
1. **Oblicz nowy klucz dzierżawy**. Funkcja skrótu służy do tworzenia klucza dzierżawy na podstawie nazwy dzierżawy.
1. **Sprawdź, czy klucz dzierżawy już istnieje**. Wykaz jest sprawdzany w celu zapewnienia, że klucz jest dostępny.
1. **Aprowizacja bazy danych jest wykonywana przy użyciu obiektu New-TenantDatabase.** Użyj **F11** do środka i zobacz, jak baza danych jest udostępnione za pomocą [szablonu usługi Azure Resource Manager](../azure-resource-manager/resource-manager-template-walkthrough.md).

    Nazwa bazy danych jest tworzona na podstawie nazwy dzierżawy, aby było jasne, który fragment należy do której dzierżawy. (Można bez problemów użyć innych strategii tworzenia nazw baz danych). Szablon usługi Resource Manager służy do tworzenia dzierżawy bazy danych przez skopiowanie szablonu bazy danych (baseTenantDB) na serwerze wykazu. Informacje o innym podejściu można utworzyć pustej bazy danych i zainicjować go przez zaimportowanie pliku bacpac lub wykonać skryptu inicjowania z dobrze znanej lokalizacji.  

    Szablon usługi Resource Manager znajduje się w folderze Modules\Common\ ...\Learning: *tenantdatabasecopytemplate.json*

1. Po utworzeniu bazy danych dzierżawy jest następnie dalsze **zainicjować o nazwie miejscową (dzierżawcy) i typie miejscową**. Można również stosować inne metody inicjowania.

1. **Dzierżawy baza danych jest zarejestrowana w katalogu** z *TenantDatabaseToCatalog Dodaj* przy użyciu klucza dzierżawy. Użyj klawisza **F11**, aby wejść do funkcji:

    * Baza danych wykazu jest dodawana do mapy fragmentów (listy znanych baz danych).
    * Tworzone jest mapowanie łączące wartość klucza z fragmentem.
    * Dodatkowe metadane dotyczące dzierżawy (właściwość name) jest dodawane do tabeli dzierżaw w katalogu.  Tabela dzierżawcy nie jest częścią schematu ShardManagement i nie jest instalowany przez EDCL.  W tej tabeli przedstawiono, jak baza danych katalogu można rozszerzyć do obsługi dodatkowe dane specyficzne dla aplikacji.   


Po ukończeniu inicjowania obsługi administracyjnej, wykonanie zwraca oryginalnej *ProvisionAndCatalog pokaz* skryptu, co spowoduje otwarcie **zdarzenia** strony dla nowej dzierżawy w przeglądarce:

   ![zdarzenia](media/saas-dbpertenant-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Zainicjuj obsługę partii dzierżawcy

Tego ćwiczenia inicjuje partii 17 dzierżaw. Zaleca się, że przed rozpoczęciem drugiej Wingtip biletów SaaS bazy danych na samouczki dzierżawy, więc ma więcej niż kilka baz danych do pracy z obsługi administracyjnej tej partii dzierżaw.

1. W *PowerShell ISE*, Otwórz... \\Modułów uczenia\\ProvisionAndCatalog\\*ProvisionAndCatalog.ps1 pokaz* i zmienić *$DemoScenario* parametr 3:
   * **$DemoScenario** = **3**, do *udostępnić partii dzierżaw*.
1. Naciśnij klawisz **F5**, aby uruchomić skrypt.

Skrypt wdroży partię dodatkowych dzierżaw. Używa [szablonu usługi Azure Resource Manager](../azure-resource-manager/resource-manager-template-walkthrough.md), który kontroluje partię i następnie deleguje aprowizację każdej bazy danych do dołączonego szablonu. Ten sposób użycia szablonów umożliwia aplikacji Azure Resource Manager pełnienie roli brokera w procesie aprowizacji przy użyciu skryptu. Szablony próbują równolegle zaaprowizować bazy danych, a jeśli to konieczne, obsługują ponowne próby aprowizacji, optymalizując cały proces. Skrypt jest idempotentności więc jeśli nie powiedzie się lub zatrzymuje jakiejkolwiek przyczyny, uruchom go ponownie.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Sprawdzanie pomyślnego wdrożenia partii dzierżaw

* W [portalu Azure](https://portal.azure.com), przejdź do listy serwerów i Otwórz *tenants1* serwera. Kliknij przycisk **baz danych SQL** i sprawdź partii 17 dodatkowych baz danych są teraz na liście:

   ![lista baz danych](media/saas-dbpertenant-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Inne wzorce aprowizacji

Inne wzorce aprowizacji, których nie uwzględniono w tym samouczku, obejmują:

**Wstępne aprowizowanie baz danych.** Wzorzec wstępnego inicjowania obsługi administracyjnej wykorzystuje fakt, że bazy danych w puli elastycznej nie należy dodawać z żadnymi dodatkowymi kosztami. Rozliczenia jest przeznaczony dla puli elastycznej nie baz danych, i baz danych w stanie bezczynności korzystać z żadnych zasobów. Wstępne inicjowanie obsługi baz danych w puli i przydzielanie je w razie potrzeby, można znacznie zmniejszyć czasu dołączenia do dzierżawy. Wstępnie tworzyć baz danych można dostosować do własnych potrzeb, aby zachować odpowiedni w przypadku inicjowania obsługi administracyjnej oczekiwanego wskaźnika buforu.

**Automatyczne aprowizowanie.** We wzorcu inicjowania obsługi automatycznego inicjowania obsługi usługi przepisy automatycznie w razie potrzeby serwery, pule adresów i baz danych — tym wstępnego inicjowania obsługi administracyjnej baz danych w pule elastyczne, jeśli konieczne. A jeśli bazy danych są zlecone wyłączyć i usunąć, luk w pule elastyczne może zostać wypełniony przez usługę inicjowania obsługi administracyjnej. Takie usługi może być prostymi lub złożonymi — na przykład obsługa inicjowania obsługi administracyjnej na wielu różnych obszarach geograficznych i można skonfigurować replikację geograficzną odzyskiwania po awarii. Z wzorcem automatycznego inicjowania obsługi administracyjnej aplikacji klienckiej lub skryptu przesyła żądanie inicjowania obsługi administracyjnej do kolejki, aby zostać przetworzony przez usługę inicjowania obsługi administracyjnej, a następnie sonduje usługę, aby określić ukończenia. Jeśli używana jest wstępnie inicjowania obsługi, żądania może szybko, obsługiwane w usłudze udostępniania zastąpienia bazy danych w tle.



## <a name="next-steps"></a>Następne kroki

W tym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Aprowizowanie pojedynczej nowej dzierżawy
> * Aprowizowanie partii dodatkowych dzierżaw
> * Krok do szczegółów udostępnianie dzierżaw i rejestrowania ich do katalogu

Spróbuj [samouczek monitorowania wydajności](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

* Dodatkowe [samouczki, które zależą od bazy danych SaaS biletów Wingtip każdej dzierżawy aplikacji](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Biblioteka kliencka Elastic Database](sql-database-elastic-database-client-library.md)
* [Sposób debugowania skryptów w programie Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
