---
title: "Udostępniania i katalogu nowych dzierżaw w aplikacji SaaS przy użyciu bazy danych SQL bazy danych SQL Azure wielodostępne | Dokumentacja firmy Microsoft"
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
ms.date: 11/17/2017
ms.author: billgib
ms.openlocfilehash: f6707b85cc80178da663d7e2b95eeb5c9550789c
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/18/2017
---
# <a name="provision-and-catalog-new-tenants-in-a-saas-application-using-a-multi-tenant-sql-database"></a>Dostarczanie i katalogu nowych dzierżaw w korzystanie z bazy danych SQL wielodostępnych aplikacji SaaS

Z tego samouczka dowiesz się wzorce dla inicjowania obsługi administracyjnej i skatalogowania dzierżaw podczas pracy z modelem podzielonej wielodostępne bazy danych. 

Schemat wielodostępne umożliwia danych z wieloma dzierżawcami, które mają być przechowywane w pojedynczej bazy danych. Do obsługi dużej liczby dzierżawców, danych dzierżawy jest dystrybuowana do wielu fragmentów lub baz danych. Dane dla dowolnej dzierżawy co zawsze pełni znajduje się w pojedynczej bazy danych.  Wykaz służy do przechowywania mapowania dzierżawcy do baz danych.   

Możesz też wypełnić niektóre bazy danych z pojedynczej dzierżawy. Baz danych zawierających wiele dzierżaw Preferuj niższy koszt dla każdego dzierżawcy kosztem izolacji dzierżawców.  Baz danych zawierających pojedynczej dzierżawy Preferuj izolacji za pośrednictwem kosztów.  Bazy danych z wieloma dzierżawcami i pojedynczego dzierżawcy można łączyć w jednej aplikacji SaaS do optymalizowania kosztów lub izolacji dla każdego dzierżawcy. Dzierżawcy można podać własne bazy danych podczas udostępniania lub mogą zostać przeniesione do ich własnych bazy danych później.

   ![Podzielonej wielodostępne bazy danych aplikacji z katalogu dzierżawcy](media/saas-multitenantdb-provision-and-catalog/MultiTenantCatalog.png)


## <a name="tenant-catalog-pattern"></a>Wzorzec katalogu dzierżawcy

Jeśli dane dzierżawy jest dystrybuowana do wielu baz danych, to musisz znać, w których są przechowywane dane każdego dzierżawcy. Bazy danych katalogu posiada mapowanie między dzierżawcy i bazy danych, która przechowuje dane.

Poszczególni dzierżawcy jest identyfikowany przy użyciu klucza w katalogu. W aplikacji SaaS biletów Wingtip klucz dzierżawy jest zbudowane z skrót nazwy dzierżawcy. Umożliwia to aplikacji wyodrębnić nazwę dzierżawcy z adresu URL strony sieci web i umożliwia połączenie z odpowiednią bazą danych. Można także inne systemy klucza dzierżawy.

Za pomocą wykazu umożliwia nazwę lub lokalizację bazy danych dzierżawy zostanie zmieniony po zainicjowaniu obsługi administracyjnej bez zakłócania pracy aplikacji.  W modelu wielodostępnym bazy danych to bierze pod uwagę przeniesienie dzierżawcy między bazami danych.  Katalog można również wskazać do aplikacji, czy dzierżawcy jest w trybie offline z powodu konserwacji lub innych działań.

Katalog można rozszerzyć, aby przechowywać dodatkowe metadane dzierżawy lub bazy danych, takie jak wersji bazy danych, wersja schematu bazy danych nazwa dzierżawcy i plan usługi lub umowy dotyczącej poziomu usług i inne informacje umożliwiające zarządzanie aplikacjami, dział obsługi klienta lub devops lub warstwy procesy.  

Katalog można również włączyć dzierżawy między raportowania, zarządzanie schematu i wyodrębniania danych do celów analizy. 

### <a name="elastic-database-client-library"></a>Biblioteka kliencka Elastic Database 
W aplikacji SaaS biletów Wingtip katalogu jest zaimplementowana w *tenantcatalog* bazy danych przy użyciu funkcji do zarządzania niezależnego fragmentu [elastycznej bazy danych klienta biblioteki (EDCL)](sql-database-elastic-database-client-library.md). Biblioteka włącza aplikację do tworzenia, zarządzania i użyć kopii bazy danych "niezależnych map". Mapy niezależnego fragmentu zawiera listę odłamków (bazy danych) i mapowanie między kluczy (dzierżawcami) i fragmentów.  Funkcje EDCL mogą być używane z aplikacji i skryptów programu PowerShell w okresie dzierżawy inicjowania obsługi można utworzyć wpisów w mapie niezależnego fragmentu i nowsze, aby nawiązać połączenie z poprawną bazą danych. Biblioteka buforuje informacje o połączeniu, aby zminimalizować ruch w bazie danych katalogu i przyspieszyć połączenia. 

> [!IMPORTANT]
> Dane mapowania są dostępne w bazie danych wykazu, ale *nie należy ich edytować*. Dane mapowania można edytować wyłącznie za pomocą interfejsu API biblioteki klienckiej Elastic Database. Bezpośrednie manipulowanie danymi mapowania wiąże się z ryzykiem uszkodzenia wykazu i dlatego nie jest obsługiwane.


## <a name="tenant-provisioning-pattern"></a>Wzorzec inicjowania obsługi administracyjnej dzierżawy

Podczas inicjowania obsługi administracyjnej nowej dzierżawy w modelu podzielonej wielodostępne bazy danych, najpierw muszą być określone, jeśli dzierżawcy, po której ma zostać udostępnione w udostępnionej bazy danych lub podane własną bazę danych. Jeśli udostępnionej bazy danych muszą być określone, jeśli brak miejsca w istniejącej bazy danych lub nowej bazy danych jest wymagana. Jeśli nowa baza danych nie jest konieczne, należy udostępnić w odpowiedniej lokalizacji i warstwy usług, zainicjować przy użyciu odpowiedniego schematu i danych referencyjnych, a następnie rejestrowane w katalogu. Na koniec dzierżawy mapowania można dodać do odwołania odpowiednie niezależnego fragmentu.

Inicjowanie obsługi administracyjnej bazy danych można osiągnąć przez wykonywanie skryptów SQL, wdrażanie pliku bacpac lub kopiowanie szablonu bazy danych. Aplikacji SaaS biletów Wingtip skopiuj szablonu bazy danych do utworzenia nowej dzierżawy baz danych.

Inicjowanie obsługi administracyjnej podejście bazy danych musi comprehended ogólnej strategii zarządzania schematu, które wymaga, aby upewnić się, że nowe bazy danych są udostępniane z najnowszą schematu.  Jest to przedstawione na [Samouczek zarządzania schematu](saas-tenancy-schema-management.md).  

Skrypty inicjowania obsługi administracyjnej dzierżawy, w tym samouczku obejmują zarówno udostępniania dzierżawcy do istniejącej bazy danych z wieloma dzierżawcami i tworzenie nowej bazy danych dzierżawy. Dzierżawy dane są następnie zainicjowany, a następnie rejestrowane w katalogu niezależnych mapy. W przykładowej aplikacji baz danych zawierających pojedynczej dzierżawy są podanej nazwy na podstawie nazwy dzierżawy. Bazy danych, które zawierają wiele dzierżaw podano ogólnego _tenantsN_ nazwać podczas baz danych z tylko jednym dzierżawcy podano nazwę dzierżawcy. Określonych konwencji nazewnictwa użyty w próbce nie mogą krytyczną częścią wzorzec, ponieważ wykaz umożliwiają dowolną nazwę można przypisać do bazy danych.  

## <a name="provision-and-catalog-tutorial"></a>Samouczek udostępniania i katalogu

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Zapewnij dzierżawcy do bazy danych z wieloma dzierżawcami
> * Zapewnij dzierżawcy w bazie danych pojedynczej dzierżawy
> * Udostępnianie partii dzierżawcy do baz danych zarówno wielodostępnej i pojedynczej dzierżawy
> * Zarejestruj bazy danych i dzierżawy mapowanie w katalogu

Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Aplikacja Wingtip biletów SaaS wielodostępne w bazie danych jest wdrażana. Aby wdrożyć w mniej niż 5 minut, zobacz [Wdróż i eksplorowanie Wingtip biletów SaaS wielodostępne w bazie danych aplikacji](saas-multitenantdb-get-started-deploy.md)
* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="get-the-wingtip-tickets-management-scripts"></a>Pobierz skrypty zarządzania Wingtip biletów

Kod źródłowy aplikacji i skrypty zarządzania są dostępne w [WingtipTicketsSaaS MultiTenantDB](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) repozytorium GitHub. <!--See [Steps to download the Wingtip SaaS scripts](saas-tenancy-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-saas-scripts).-->


## <a name="provision-tenant-walkthrough-1"></a>Zainicjuj obsługę dzierżawy wskazówki #1

Aby zrozumieć, jak aplikacja biletów Wingtip implementuje nowej dzierżawy inicjowania obsługi administracyjnej z wieloma dzierżawcami bazy danych, należy dodać punkt przerwania i krok przez przepływ pracy podczas inicjowania obsługi administracyjnej dzierżawcy w udostępnionej bazy danych z innymi dzierżawcami:

1. W _PowerShell ISE_, Otwórz... \\Modułów uczenia\\ProvisionAndCatalog\\_ProvisionAndCatalog.ps1 pokaz_ i ustaw następujące parametry:
   * **$TenantName** = **niebieskie Bushwillow**, nazwę nowe miejsce.
   * **$VenueType** = **niebieskie**, jeden z typów wstępnie zdefiniowanych miejscową: niebieskie, classicalmusic, tańca, jazz judo, motorracing uniwersalne, opera, rockmusic, nożną (małe litery, bez spacji).
   * **$DemoScenario** = **1**, do *udostępnić dzierżawcy w udostępnionej bazy danych z innymi dzierżawcami*.

1. Dodawanie punktu przerwania w dowolnym umieszczając kursor w wiersz 38, informujący: *nowej dzierżawy "*i naciśnij klawisz **F9**.

   ![punkt przerwania](media/saas-multitenantdb-provision-and-catalog/breakpoint.png)

1. Aby uruchomić skrypt naciśnij **F5**.

1. Po zatrzymaniu wykonywania skryptu na punkt przerwania, naciśnij klawisz **F11** do kroku do kodu.

   ![Debugowania](media/saas-multitenantdb-provision-and-catalog/debug.png)

Śledzenie wykonywanie skryptu za pomocą **debugowania** menu Opcje - **F10** i **F11** do kroku przez lub do funkcji o nazwie. Aby uzyskać więcej informacji na temat debugowania skryptów programu PowerShell, zobacz [porady dotyczące pracy z i debugowania skryptów programu PowerShell](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Poniżej przedstawiono kluczowe elementy przepływu pracy, który krokowo podczas śledzenia skryptu:

* **Oblicz nowy klucz dzierżawy**. Funkcja skrótu służy do tworzenia klucza dzierżawy na podstawie nazwy dzierżawy.
* **Sprawdź, czy klucz dzierżawy już istnieje**. Katalog jest sprawdzane pod kątem upewnij się, że klucz nie został już zarejestrowany.
* **Inicjowanie dzierżawcy w bazie danych dzierżawy domyślne**. Baza danych dzierżawy jest zaktualizowano w celu dodania nowych informacji dzierżawy.  
* **Zarejestruj dzierżawy w katalogu** mapowanie między nowego klucza dzierżawy i z istniejącą bazą danych tenants1 jest dodane do katalogu. 
* **Nazwa dzierżawcy jest dodane do katalogu**. Nazwa miejsca zostanie dodana do tabeli dzierżaw w katalogu.  Oznacza to, jak baza danych katalogu można rozszerzyć do obsługi dodatkowe dane specyficzne dla aplikacji.
* **Otwórz stronę zdarzenia dla nowej dzierżawy**. *Niebieskie Bushwillow* strona zdarzenia jest otwarta w przeglądarce:

   ![zdarzenia](media/saas-multitenantdb-provision-and-catalog/bushwillow.png)


## <a name="provision-tenant-walkthrough-2"></a>Zainicjuj obsługę dzierżawy wskazówki #2

Teraz wskazówki procesu podczas tworzenia dzierżawcy w własną bazę danych:

1. Nadal... \\Modułów uczenia\\ProvisionAndCatalog\\_ProvisionAndCatalog.ps1 pokaz_ ustaw następujące parametry:
   * **$TenantName** = **nożną sequoia**, nazwę nowe miejsce.
   * **$VenueType** = **nożną**, jeden z typów wstępnie zdefiniowanych miejscową: niebieskie, classicalmusic, tańca, jazz judo, motorracing uniwersalne, opera, rockmusic, nożną (małe litery, bez spacji).
   * **$DemoScenario** = **2**, do *udostępnić dzierżawcy w udostępnionej bazy danych z innymi dzierżawcami*.

1. Dodać nowego punktu przerwania umieszczając kursor dowolne miejsce na wiersz 57, informujący:  *& &nbsp;$PSScriptRoot\New-TenantAndDatabase "*i naciśnij klawisz **F9**.

   ![punkt przerwania](media/saas-multitenantdb-provision-and-catalog/breakpoint2.png)

1. Naciśnij klawisz **F5**, aby uruchomić skrypt.

1. Po zatrzymaniu wykonywania skryptu na punkt przerwania, naciśnij klawisz **F11** do kroku do kodu.  Użyj **F10** i **F11** Przekrocz nad i wkraczać do funkcji do śledzenia realizacji.

Poniżej przedstawiono kluczowe elementy przepływu pracy, który krokowo podczas śledzenia skryptu:

* **Oblicz nowy klucz dzierżawy**. Funkcja skrótu służy do tworzenia klucza dzierżawy na podstawie nazwy dzierżawy.
* **Sprawdź, czy klucz dzierżawy już istnieje**. Katalog jest sprawdzane pod kątem upewnij się, że klucz nie został już zarejestrowany.
* **Utwórz nową bazę danych dzierżawy**. Baza danych została utworzona przez skopiowanie *basetenantdb* bazy danych przy użyciu szablonu usługi Resource Manager.  Nazwa bazy danych jest oparta na nazwę dzierżawcy.
* **Dodaj bazę danych do katalogu**. Baza danych dzierżawy jest zarejestrowana jako niezależnego fragmentu w katalogu.
* **Inicjowanie dzierżawcy w bazie danych dzierżawy domyślne**. Baza danych dzierżawy jest zaktualizowano w celu dodania nowych informacji dzierżawy.  
* **Zarejestruj dzierżawy w katalogu** mapowanie między nowego klucza dzierżawy i *sequoiasoccer* bazy danych jest dodane do katalogu.
* **Nazwa dzierżawcy jest dodane do katalogu**. Nazwa miejsca zostanie dodana do tabeli dzierżaw w katalogu.
* **Otwórz stronę zdarzenia dla nowej dzierżawy**. *Nożną Sequoia* strona zdarzenia jest otwarta w przeglądarce:

   ![zdarzenia](media/saas-multitenantdb-provision-and-catalog/sequoiasoccer.png)


## <a name="provision-a-batch-of-tenants"></a>Zainicjuj obsługę partii dzierżawcy

Tego ćwiczenia inicjuje szybko partii 17 dzierżaw. Zaleca się, że przed rozpoczęciem samouczków Wingtip biletów, więc ma więcej niż kilka baz danych do pracy z obsługi administracyjnej tej partii dzierżaw.

1. W *PowerShell ISE*, Otwórz... \\Modułów uczenia\\ProvisionAndCatalog\\*ProvisionAndCatalog.ps1 pokaz* i zmienić *$DemoScenario* parametr 3:
   * **$DemoScenario** = **3**, do *udostępnić partii dzierżawcom w udostępnionej bazy danych*.
1. Naciśnij klawisz **F5** i uruchom skrypt.


### <a name="verify-the-deployed-set-of-tenants"></a>Sprawdź zestaw wdrożonych dzierżawcy 
Na tym etapie masz kombinację dzierżaw wdrożonych w udostępnionej bazy danych i dzierżaw wdrożone do ich własnych baz danych. Portalu Azure można przeprowadzać inspekcję bazy danych utworzone:  

* W [portalu Azure](https://portal.azure.com), otwórz **tenants1-mt -\<użytkownika\>**  serwer, przechodząc do listy serwerów SQL.  **Baz danych SQL** lista powinna zawierać udostępnionego **tenants1** bazy danych i baz danych w przypadku dzierżaw, które znajdują się w ich własnych bazy danych:

   ![lista baz danych](media/saas-multitenantdb-provision-and-catalog/databases.png)

W trakcie portalu Azure zawiera dzierżawcy baz danych, go teraz widoczne dzierżaw w udostępnionej bazy danych. Pełną listę dzierżawców może być widoczny na stronie Centrum Wingtip biletów zdarzeń.   

* Otwórz stronę Centrum zdarzeń w przeglądarce (http:events.wingtip-mt.\<użytkownika\>. trafficmanager.net)  

Pełną listę dzierżawców i ich odpowiednia baza danych jest dostępne w wykazie. Widok SQL znajduje się w bazie danych tenantcatalog, której jest przyłączany nazwę dzierżawcy przechowywane w tabeli dzierżaw Nazwa bazy danych w tabelach zarządzania niezależnego fragmentu. Ten widok przedstawia dobrze wartość rozszerzanie metadanych przechowywanych w katalogu.

* W *programu SQL Server Management Studio (SSMS)* nawiązują połączenia z serwerem dzierżaw w **patrz hasło mt. tenants1\<użytkownika\>. database.windows.net**, z nazwą logowania: **developer** , Hasło:**P@ssword1**

    ![W oknie dialogowym połączenia SSMS](media/saas-multitenantdb-provision-and-catalog/SSMSConnection.png)

* W *Eksplorator obiektów*, przejdź do widoków *tenantcatalog* bazy danych.
* Kliknij prawym przyciskiem myszy *ExtendedTenants* i wybierz polecenie **zaznacz 1000 pierwszych wierszy** i zanotuj mapowanie między nazwa dzierżawcy i bazy danych dla różnych dzierżawców.

    ![Widok ExtendedTenants w programie SSMS](media/saas-multitenantdb-provision-and-catalog/extendedtenantsview.png)
      
## <a name="other-provisioning-patterns"></a>Inne wzorce aprowizacji

Inne wzorce aprowizacji, których nie uwzględniono w tym samouczku, obejmują:

**Wstępne inicjowanie obsługi administracyjnej bazy danych w puli elastycznej.** Wzorzec wstępnego inicjowania obsługi administracyjnej wykorzystuje fakt, że bazy danych w puli elastycznej nie należy dodawać z żadnymi dodatkowymi kosztami. Rozliczenia jest przeznaczony dla puli elastycznej nie baz danych, i baz danych w stanie bezczynności korzystać z żadnych zasobów. Wstępne inicjowanie obsługi baz danych w puli i przydzielanie je w razie potrzeby, znacznie można zmniejszyć czas potrzebny na płycie dzierżawcy do ich własnych bazy danych. Liczbę wstępnie aprowizowanych baz danych można dostosować do własnych potrzeb, aby zachować bufor odpowiedni dla oczekiwanego wskaźnika aprowizacji.

**Automatyczne aprowizowanie.** We wzorcu automatycznego inicjowania obsługi administracyjnej dedykowanego inicjowania obsługi usługi służy do serwerów, pul i baz danych automatycznie w razie potrzeby udostępnić — tym wstępnego inicjowania obsługi administracyjnej baz danych w pule elastyczne, w razie potrzeby. W przypadku wycofania i usunięcia baz danych luki w elastycznych pulach mogą zostać uzupełnione przez usługę aprowizacji, jeśli zachodzi taka potrzeba. Takie usługi może być prostymi lub złożonymi — na przykład obsługa inicjowania obsługi administracyjnej na wielu różnych obszarach geograficznych i można skonfigurować replikację geograficzną odzyskiwania po awarii. Za pomocą wzorca automatycznego inicjowania obsługi administracyjnej aplikacji klienckiej lub skrypt czy przesłać żądanie obsługi administracyjnej do kolejki do przetworzenia przez inicjowania obsługi administracyjnej usługi, a następnie będzie sondować ustalenie ukończenia. Jeśli używana jest wstępnie inicjowania obsługi, żądania może szybko, obsługiwane z usługą zarządzania, inicjowania obsługi administracyjnej zastąpienia bazy danych działającej w tle.



## <a name="next-steps"></a>Następne kroki

W tym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Aprowizowanie pojedynczej nowej dzierżawy
> * Aprowizowanie partii dodatkowych dzierżaw
> * Krok do szczegółów udostępnianie dzierżaw i rejestrowania ich do katalogu

Spróbuj [samouczek monitorowania wydajności](saas-multitenantdb-performance-monitoring.md).

## <a name="additional-resources"></a>Dodatkowe zasoby

<!--* Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Biblioteka kliencka Elastic Database](sql-database-elastic-database-client-library.md)
* [Sposób debugowania skryptów w programie Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
