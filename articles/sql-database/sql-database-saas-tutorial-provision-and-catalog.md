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
ms.openlocfilehash: eda330a7202de8a325d645b37a0d05ef8df8985b
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="learn-how-to-provision-and-catalog-new-tenants-and-register-them-in-the-catalog"></a>Dowiedz się, jak udostępnić katalogu nowi dzierżawcy i zarejestruj je w katalogu

W tym samouczku informacje o wzorce SaaS udostępniania i katalogu i jak są implementowane w aplikacji Wingtip SaaS. Tworzenie i zainicjować nowe dzierżawy bazy danych oraz zarejestrować je w katalogu dzierżawy aplikacji. Katalog jest bazy danych, która przechowuje mapowanie między wiele dzierżaw aplikacji SaaS i ich danych. Katalog odgrywa ważną rolę w kierowanie żądań aplikacji z poprawną bazą danych.  

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Udostępnianie jednej nowej dzierżawy, tym krokowe to implementowania
> * Aprowizowanie partii dodatkowych dzierżaw


Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Aplikacja Wingtip SaaS jest wdrażana. Aby wdrożyć w mniej niż 5 minut, zobacz [wdrażania i aplikacji Wingtip SaaS](sql-database-saas-tutorial.md)
* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-saas-catalog-pattern"></a>Wprowadzenie do wzorca wykazu SaaS

W kopii bazy danych wielodostępnych aplikacji SaaS należy wiedzieć, w którym przechowywane są informacje dla każdego dzierżawcy. We wzorcu katalogu SaaS katalogu bazy danych służy do przechowywania mapowanie między każdego dzierżawcy i gdzie są przechowywane ich dane. Aplikacja Wingtip SaaS używa pojedynczego dzierżawcy na Architektura bazy danych, ale podstawowy wzorzec przechowywania mapowania dzierżawcy w bazie danych w katalogu ma zastosowanie, czy ma być używana w wielu dzierżawców lub pojedynczej dzierżawy bazy danych.

Poszczególni dzierżawcy jest przypisany klucz identyfikujący je w katalogu i która jest mapowana na lokalizację odpowiednie bazy danych. W aplikacji Wingtip SaaS klucz został utworzony z skrót nazwy dzierżawcy. Dzięki temu dzierżawcy część adresu URL aplikacji ma być używany do utworzenia klucza. Można użyć innych systemów klucza dzierżawy.  

Katalog umożliwia nazwę lub lokalizację bazy danych zostanie zmieniony z minimalnym wpływem na aplikację.  W modelu wielodostępnym bazy danych to również bierze pod uwagę "przenoszenia" dzierżawa między bazami danych.  Katalog można również wskazać, czy dzierżawcy lub baza danych jest w trybie offline z powodu konserwacji lub innych działań. Jest to przedstawione w [Przywracanie pojedynczej dzierżawy samouczek](sql-database-saas-tutorial-restore-single-tenant.md).

Ponadto katalog, który jest włączone zarządzanie bazy danych dla aplikacji SaaS, można przechowywać dodatkowe dzierżawy lub metadanych bazy danych, takich jak wersji bazy danych, lub warstwy wersji schematu, plan usługi lub umów SLA oferowane dzierżawców i innych informacji, które umożliwia Zarządzanie aplikacjami, dział obsługi klienta lub procesów opracowywania oprogramowania.  

Poza aplikacji SaaS katalogu można włączyć narzędzi bazy danych.  W przykładowym Wingtip SaaS umożliwia programowi zapytania między dzierżawy, przedstawione w katalogu [samouczek analizy ad hoc](sql-database-saas-tutorial-adhoc-analytics.md). Zarządzanie zadaniami między bazami danych jest przedstawione w [Zarządzanie schematami](sql-database-saas-tutorial-schema-management.md) i [dzierżawy analytics](sql-database-saas-tutorial-tenant-analytics.md) samouczki. 

W aplikacji Wingtip SaaS katalogu jest implementowane za pomocą funkcji do zarządzania niezależnego fragmentu [elastycznej bazy danych klienta biblioteki (EDCL)](sql-database-elastic-database-client-library.md). EDCL włącza aplikację do tworzenia, zarządzania i użyć mapy niezależnych kopii bazy danych. Mapy niezależnego fragmentu zawiera listę odłamków (bazy danych) i mapowanie między kluczy (dzierżawcami) i baz danych.  Funkcje EDCL można z aplikacji i skryptów programu PowerShell podczas inicjowania obsługi można utworzyć wpisów w mapie niezależnego fragmentu dzierżawy i z aplikacji do wydajnie nawiązania połączenia z poprawną bazą danych. EDCL buforuje informacje o połączeniu, aby zminimalizować ruch związany z bazą danych katalogu i przyspieszenie działania aplikacji.  

> [!IMPORTANT]
> Dane mapowania są dostępne w bazie danych wykazu, ale *nie należy ich edytować*. Dane mapowania można edytować wyłącznie za pomocą interfejsu API biblioteki klienckiej Elastic Database. Bezpośrednie manipulowanie danymi mapowania wiąże się z ryzykiem uszkodzenia wykazu i dlatego nie jest obsługiwane.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>Wprowadzenie do wzorca SaaS inicjowania obsługi administracyjnej.

Podczas przechodzenia do nowego dzierżawcy w aplikacji SaaS, która używa modelu bazy danych z pojedynczej dzierżawy nową bazę danych dzierżawcy należy udostępnić.  Muszą być tworzone w odpowiedniej lokalizacji i warstwy usług, zainicjować przy użyciu odpowiedniego schematu i danych referencyjnych, a następnie rejestrowane w katalogu w kluczu odpowiedniego dzierżawcy.  

Różne podejścia może służyć do bazy danych inicjowania obsługi, które mogą obejmować wykonywanie skryptów SQL, wdrażanie pliku bacpac lub kopiowania bazy danych "złotego" szablonu.  

Metoda inicjowania obsługi administracyjnej, używanego musi comprehended w strategii zarządzania ogólny schemat, który musi zapewnić, że nowe bazy danych są udostępniane z najnowszą schematu.  Jest to przedstawione w [Samouczek zarządzania schematu](sql-database-saas-tutorial-schema-management.md).  

Wingtip SaaS aplikacji przepisy nowi dzierżawcy przez skopiowanie złotego bazy danych o nazwie basetenantdb, wdrożony na serwerze wykazu.  Inicjowania obsługi administracyjnej może być zintegrowane z aplikacji w ramach obsługi rejestracji i/lub obsługiwane w trybie offline za pomocą skryptów. W tym samouczku może zapoznać się inicjowanie obsługi przy użyciu programu PowerShell. Skrypty inicjowania obsługi administracyjnej skopiuj basetenantdb, aby utworzyć nową bazę danych dzierżawy w puli elastycznej, a następnie go zainicjować z użyciem informacji specyficznych dla dzierżawy i zarejestrować ją w katalogu niezależnych mapy.  W przykładowej aplikacji, baz danych są podane nazwy na podstawie nazwy dzierżawy, ale nie jest krytyczną częścią wzorzec — katalogu umożliwiają dowolną nazwę można przypisać do bazy danych. + 


## <a name="get-the-wingtip-application-scripts"></a>Pobieranie skryptów aplikacji Wingtip

Wingtip SaaS skrypty i kod źródłowy aplikacji są dostępne w [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) repozytorium github. [Kroki, aby pobrać skrypty Wingtip SaaS](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).


## <a name="provision-and-catalog-detailed-walkthrough"></a>Szczegółowy przewodnik po aprowizacji i wykazie

Aby zrozumieć, jak aplikacja Wingtip implementuje nowej dzierżawy inicjowania obsługi, Dodaj punkt przerwania i krok przez przepływ pracy podczas inicjowania obsługi administracyjnej dzierżawcy:

1. Otwórz... \\Modułów uczenia\\ProvisionAndCatalog\\_ProvisionAndCatalog.ps1 pokaz_ i ustaw następujące parametry:
   * **$TenantName** = nazwa nowego miejsca (na przykład *Bushwillow Blues*).
   * **$VenueType** = jeden z typów wstępnie zdefiniowanych miejscową: *niebieskie*, classicalmusic, tańca, jazz, judo, motorracing uniwersalne, opera, rockmusic, nożną.
   * **$DemoScenario** = **1**, ustawioną **1** do *udostępnić pojedynczej dzierżawy*.

1. Dodawanie punktu przerwania w dowolnym ustawiając kursor na wiersz 48, informujący: *nowej dzierżawy "*i naciśnij klawisz **F9**.

   ![punkt przerwania](media/sql-database-saas-tutorial-provision-and-catalog/breakpoint.png)

1. Aby uruchomić skrypt naciśnij **F5**.

1. Po zatrzymaniu wykonywania skryptu na punkt przerwania, naciśnij klawisz **F11** do kroku do kodu.

   ![punkt przerwania](media/sql-database-saas-tutorial-provision-and-catalog/debug.png)



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

Nazwa bazy danych jest tworzona na podstawie nazwy dzierżawy, aby było jasne, który fragment należy do której dzierżawy. (Inne strategie nazewnictwa bazy danych może być wykorzystane.) + Szablonem usługi Resource Manager służy do tworzenia dzierżawy bazy danych przez skopiowanie złotego bazy danych (baseTenantDB) na serwerze wykazu. Informacje o innym podejściu można utworzyć pustej bazy danych i zainicjować go przez zaimportowanie pliku bacpac lub wykonać skryptu inicjowania z dobrze znanej lokalizacji.  

Szablon usługi Resource Manager znajduje się w folderze Modules\Common\ ...\Learning: *tenantdatabasecopytemplate.json*

Po utworzeniu bazy danych dzierżawy jest następnie dalsze **zainicjować o nazwie miejscową (dzierżawcy) i typie miejscową**. Można również stosować inne metody inicjowania.

**Dzierżawy baza danych jest zarejestrowana w katalogu** z *TenantDatabaseToCatalog Dodaj* przy użyciu klucza dzierżawy. Użyj klawisza **F11**, aby wejść do funkcji:

* Baza danych wykazu jest dodawana do mapy fragmentów (listy znanych baz danych).
* Tworzone jest mapowanie łączące wartość klucza z fragmentem.
* Dodatkowe metadane (właściwość name) o dzierżawcy jest dodawane do tabeli dzierżaw w katalogu.  Tabela dzierżawcy nie jest częścią schematu ShardManagement i nie jest instalowany przez EDCL.  W tej tabeli przedstawiono, jak baza danych katalogu można rozszerzyć do obsługi dodatkowe dane specyficzne dla aplikacji.   


Po ukończeniu inicjowania obsługi administracyjnej, wykonanie zwraca oryginalnej *ProvisionAndCatalog pokaz* skryptu, co spowoduje otwarcie **zdarzenia** strony dla nowej dzierżawy w przeglądarce:

   ![zdarzenia](media/sql-database-saas-tutorial-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Zainicjuj obsługę partii dzierżawcy

Tego ćwiczenia inicjuje partii 17 dzierżaw. Zaleca się, że przed rozpoczęciem samouczków Wingtip SaaS, więc ma więcej niż kilka baz danych do pracy z obsługi administracyjnej tej partii dzierżaw.

1. Otwórz... \\Modułów uczenia\\ProvisionAndCatalog\\*ProvisionAndCatalog.ps1 pokaz* w *PowerShell ISE* i zmienić *$ DemoScenario* parametr 3:
   * **$DemoScenario** = **3**, zmień **3** do *udostępnić partii dzierżaw*.
1. Naciśnij klawisz **F5** i uruchom skrypt.

Skrypt wdroży partię dodatkowych dzierżaw. Używa [szablonu usługi Azure Resource Manager](../azure-resource-manager/resource-manager-template-walkthrough.md), który kontroluje partię i następnie deleguje aprowizację każdej bazy danych do dołączonego szablonu. Ten sposób użycia szablonów umożliwia aplikacji Azure Resource Manager pełnienie roli brokera w procesie aprowizacji przy użyciu skryptu. Szablony próbują równolegle zaaprowizować bazy danych, a jeśli to konieczne, obsługują ponowne próby aprowizacji, optymalizując cały proces. Skrypt jest idempotentności więc jeśli nie powiedzie się lub zatrzymuje jakiejkolwiek przyczyny, uruchom go ponownie.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Sprawdzanie pomyślnego wdrożenia partii dzierżaw

* Otwórz *tenants1* serwer, przechodząc do listy serwerów w [portalu Azure](https://portal.azure.com), kliknij przycisk **baz danych**i sprawdź partii 17 dodatkowych baz danych są dostępne w Lista:

   ![lista baz danych](media/sql-database-saas-tutorial-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Inne wzorce aprowizacji

Inne wzorce aprowizacji, których nie uwzględniono w tym samouczku, obejmują:

**Wstępne aprowizowanie baz danych.** Wzorzec wstępnego inicjowania obsługi administracyjnej wykorzystuje fakt, że bazy danych w puli elastycznej nie należy dodawać z żadnymi dodatkowymi kosztami. Rozliczenia jest przeznaczony dla puli elastycznej nie baz danych, i baz danych w stanie bezczynności korzystać z żadnych zasobów. Wstępne inicjowanie obsługi baz danych w puli i przydzielanie je w razie potrzeby, można znacznie zmniejszyć czasu dołączenia do dzierżawy. Liczbę wstępnie aprowizowanych baz danych można dostosować do własnych potrzeb, aby zachować bufor odpowiedni dla oczekiwanego wskaźnika aprowizacji.

**Automatyczne aprowizowanie.** We wzorcu automatycznego inicjowania obsługi administracyjnej dedykowanego inicjowania obsługi usługi służy do serwerów, pul i baz danych automatycznie w razie potrzeby udostępnić — tym wstępnego inicjowania obsługi administracyjnej baz danych w pule elastyczne, w razie potrzeby. W przypadku wycofania i usunięcia baz danych luki w elastycznych pulach mogą zostać uzupełnione przez usługę aprowizacji, jeśli zachodzi taka potrzeba. Takie usługi może być prostymi lub złożonymi — na przykład obsługa inicjowania obsługi administracyjnej na wielu różnych obszarach geograficznych i można skonfigurować replikację geograficzną automatycznie Jeśli tej strategii jest używana dla odzyskiwania po awarii. Za pomocą wzorca automatycznej aprowizacji aplikacja klienta lub skrypt mogą umieścić w kolejce żądanie aprowizacji, które zostanie przetworzone przez usługę aprowizacji, a następnie mogą sondować usługę w celu wykrycia ukończenia zadania. Jeśli używana jest wstępna aprowizacja, żądania mogą zostać szybko obsłużone przez usługę zarządzającą aprowizacją zastępczej bazy danych uruchomioną w tle.



## <a name="next-steps"></a>Następne kroki

W tym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Aprowizowanie pojedynczej nowej dzierżawy
> * Aprowizowanie partii dodatkowych dzierżaw
> * Krok do szczegółów udostępnianie dzierżaw i rejestrowania ich do katalogu

Spróbuj [samouczek monitorowania wydajności](sql-database-saas-tutorial-performance-monitoring.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

* Dodatkowe [samouczków, z którymi aplikacji Wingtip SaaS](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Biblioteka kliencka Elastic Database](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library)
* [Sposób debugowania skryptów w programie Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
