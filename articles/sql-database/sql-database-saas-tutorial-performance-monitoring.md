---
title: "Monitorowanie wydajności aplikacji SaaS usługi SQL Database | Microsoft Docs"
description: "Monitorowanie wydajności i zarządzanie nią na przykładzie aplikacji Wingtip Tickets (WTP) korzystającej z usługi Azure SQL Database"
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
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: af9511978718af10c97bee6af3a2835c9d2c1ff4
ms.contentlocale: pl-pl
ms.lasthandoff: 05/12/2017


---
# <a name="monitor-performance-of-the-wtp-sample-saas-application"></a>Monitorowanie wydajności przykładowej aplikacji SaaS o nazwie WTP

W tym samouczku przedstawiono wbudowane funkcje monitorowania i alertów usługi SQL Database i elastycznych pul, po czym zbadano kilka podstawowych scenariuszy zarządzania wydajnością używanych w aplikacjach SaaS.

Aplikacja WTP używa modelu danych z pojedynczą dzierżawą, w którym każde miejsce (dzierżawca) ma swoją własną bazę danych. Podobnie jak w przypadku wielu innych aplikacji SaaS, oczekiwany wzorzec obciążenia dzierżawy charakteryzuje się nieprzewidywalnością i sporadycznością występowania. Innymi słowy, sprzedaż biletów może nastąpić w dowolnej chwili. Aby jak najlepiej wykorzystać ten typowy wzorzec korzystania z bazy danych, bazy danych dzierżaw zostały wdrożone w elastycznych pulach baz danych. Elastyczne pule umożliwiają optymalizację kosztu rozwiązania dzięki udostępnieniu zasobów pomiędzy wieloma bazami danych. W przypadku tego typu wzorca ważne jest, aby monitorować użycie zasobów bazy danych i puli, co ma na celu rozsądne równoważenie obciążenia między pulami. Należy także upewnić się, że pojedyncze bazy danych posiadają odpowiednie zasoby, i że pule nie zbliżają się do swoich limitów liczby jednostek [eDTU](sql-database-what-is-a-dtu.md). W tym samouczku przedstawiono metody monitorowania baz danych i pul oraz zarządzania nimi, a także wykonywanie akcji naprawczych w odpowiedzi na wahania obciążenia.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]

> * Symulowanie korzystania z baz danych dzierżaw przy użyciu dostarczonego generatora obciążenia
> * Monitorowanie reakcji baz danych dzierżaw na wzrost obciążenia
> * Skalowanie w górę elastycznej puli w reakcji na zwiększone obciążenie bazy danych
> * Aprowizacja drugiej elastycznej puli w celu równoważenia aktywności baz danych


Do wykonania zadań opisanych w tym samouczku niezbędne jest spełnienie następujących wymagań wstępnych:

* Wdrożona jest aplikacja WTP. Aby wdrożyć tę aplikację w czasie krótszym niż pięć minut, zobacz [Wdrażanie i korzystanie z aplikacji SaaS o nazwie WTP](sql-database-saas-tutorial.md)
* Zainstalowany jest program Azure PowerShell. Aby uzyskać szczegółowe informacje, zobacz [Rozpoczynanie pracy z programem Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-saas-performance-management-patterns"></a>Wprowadzenie do wzorców zarządzania wydajnością SaaS

Zarządzanie wydajnością bazy danych polega na zbieraniu danych dotyczących wydajności i analizowaniu ich, a następnie reagowaniu na te dane przez dostosowanie parametrów w celu zachowania akceptowalnego czasu odpowiedzi aplikacji. W przypadku hostowania wielu dzierżaw elastyczne pule baz danych stanowią ekonomiczny sposób udostępniania zasobów i zarządzania nimi dla grupy baz danych obciążanych w nieprzewidywalny sposób. Przy pewnych wzorcach obciążenia korzyści mogą się pojawić przy zarządzaniu w puli zaledwie dwoma bazami danych S3. Udostępnianie puli nie tylko zmniejsza koszt zasobów, ale również eliminuje potrzebę stałego monitorowania i śledzenia poszczególnych baz danych.

![nośnik](./media/sql-database-saas-tutorial-performance-monitoring/app-diagram.png)

Pule i zawarte w nich bazy danych nadal muszą być monitorowane, aby zapewnić pozostawanie ich wydajności w dopuszczalnym zakresie. Dostosuj konfigurację puli do potrzeb zagregowanego obciążenia, zapewniając liczbę jednostek eDTU puli odpowiednią dla całkowitego obciążenia. Dostosuj minimalne i maksymalne wartości eDTU dla bazy danych do wartości odpowiadających wymaganiom konkretnych aplikacji.

### <a name="performance-management-strategies"></a>Strategie zarządzania wydajnością

* Najlepszym sposobem na uniknięcie konieczności ręcznego monitorowania wydajności jest **ustawienie alertów, które uruchamiają się, jeśli bazy danych lub pule wyjdą poza przewidziane zakresy**.
* Odpowiedzią na krótkoterminowe wahania zagregowanego poziomu wydajności puli **może być skalowanie w górę lub w dół poziomu jednostek eDTU puli**. Jeśli takie wahania występują regularnie lub są przewidywalne, **można zaplanować automatyczne skalowanie puli**. Na przykład skalowanie w dół może nastąpić, kiedy przewidywane jest niskie obciążenie — w nocy lub podczas weekendów.
* Odpowiedzią na wahania długoterminowe lub zmiany liczby baz danych jest **przeniesienie pojedynczych baz danych do innych pul**.
* Aby zareagować na krótkoterminowe zwiększenie obciążenia *pojedynczej* bazy danych**, można wyjąć z puli pojedyncze bazy danych i przypisać im indywidualny poziom wydajności** na dany okres. Po zmniejszeniu obciążenia można zwrócić bazę danych do puli. Jeśli z góry wiemy o takich zdarzeniach, można działać wyprzedzająco, przenosząc bazy danych, co zapewnia im dostępność wymaganych zasobów i pozwala uniknąć wpływu na inne bazy danych w puli. Jeśli takie wymaganie jest przewidywalne, na przykład w przypadku oczekiwania na wzmożone zakupy biletów na popularną imprezę, wówczas takie działanie funkcji zarządzania można uwzględnić w aplikacji.

Witryna [Azure Portal](https://portal.azure.com) udostępnia wbudowane funkcje monitorowania i alertów dla większości zasobów. W usłudze SQL Database funkcje monitorowania i zgłaszania alertów są dostępne na poziomie baz danych i pul. Ta wbudowana funkcja monitorowania i alertów zależy od konkretnych zasobów, dlatego jest łatwa w użyciu dla małej liczby zasobów, ale przestaje być wygodna przy pracy z wieloma zasobami.

W scenariuszach dotyczących dużych ilości danych można użyć usługi Log Analytics (znanej także jako pakiet OMS). Jest to osobna usługa Azure dostarczająca narzędzia analityczne do przetwarzania utworzonych dzienników diagnostycznych i danych telemetrycznych zebranych w obszarze roboczym usługi Log Analytics, który może gromadzić dane telemetryczne z wielu usług i służyć do tworzenia zapytań i ustawiania alertów.

## <a name="get-the-wingtip-application-scripts"></a>Pobieranie skryptów aplikacji Wingtip

Skrypty i kod źródłowy aplikacji Wingtip Tickets są dostępne w repozytorium GitHub [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS). Pliki skryptów znajdują się w [folderze Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Pobierz folder **Learning Modules** na komputer lokalny, zachowując jego strukturę folderów.

## <a name="provision-additional-tenants"></a>Aprowizacja dodatkowych dzierżaw

Pule stają się opłacalne nawet w przypadku zaledwie dwóch baz danych nas poziomie S3, a związane z nimi oszczędności wzrastają wraz ze zwiększeniem liczby baz danych w puli, co jest spowodowane efektem uśredniania. Aby dobrze zrozumieć działanie monitorowania wydajności i zarządzania nią w dużej skali, w tym samouczku zalecamy wdrożenie co najmniej 20 baz danych.

Jeśli podczas pracy z poprzednim samouczkiem aprowizowano już partię dzierżaw, można przejść do sekcji [Symulowanie użycia we wszystkich bazach danych dzierżaw](#simulate-usage-on-all-tenant-databases).

1. Otwórz plik …\\Learning Modules\\Provision and Catalog\\*Demo-PerformanceMonitoringAndManagement.ps1* w programie **PowerShell ISE**. Nie zamykaj tego skryptu, gdyż w ramach tego samouczka będzie konieczne uruchomienie kilku scenariuszy.
1. Ustaw zmienną **$DemoScenario** = **1**, **Provision a batch of tenants** (Aprowizacja partii dzierżaw)
1. Naciśnij klawisz **F5**, aby uruchomić skrypt.

Ten skrypt wdroży 17 dzierżaw w czasie krótszym niż pięć minut.

Skrypt *New-TenantBatch* używa zestawu połączonych lub zagnieżdżonych szablonów usługi [Resource Manager](../azure-resource-manager/index.md), który tworzy partię dzierżaw, domyślnie kopiujących bazę danych **baseTenantDb** na serwerze wykazu w celu utworzenia nowych baz danych dzierżawy, a następnie rejestruje je w wykazie, po czym inicjuje je przy użyciu nazwy dzierżawy i typu miejsca. Jest to zgodne ze sposobem aprowizacji nowej dzierżawy przez aplikację WTP. Wszelkie zmiany wprowadzone w bazie danych *baseTenantDB* są uwzględniane w nowych dzierżawach, których aprowizację wykonano później. Zobacz [samouczek zarządzania schematami](sql-database-saas-tutorial-schema-management.md), aby dowiedzieć się, jak wykonywać zmiany w schematach w *istniejących* bazach danych dzierżaw (w tym w *końcowej wersji* bazy danych).

## <a name="simulate-different-usage-patterns-by-generating-different-load-types"></a>Symulowanie różnych wzorców użycia przez generowanie różnych typów obciążenia

Skrypt *Demo-PerformanceMonitoringAndManagement.ps1* uruchamia generator obciążenia, używając jednego z dostępnych *typów obciążenia*:

| Demonstracja | Scenariusz |
|:--|:--|
| 2 | Generowanie obciążenia o normalnym natężeniu (ok. 40 jednostek DTU) |
| 3 | Generowanie obciążenia z dłuższymi i częstszymi skokami wartości dla każdej bazy danych|
| 4 | Generowanie obciążenia o wyższych skokach wartości DTU dla każdej bazy danych (ok. 80 jednostek DTU)|
| 5 | Generowanie normalnego obciążenia oraz wysokiego obciążenia dla jednej dzierżawy (ok. 95 jednostek DTU)|
| 6 | Generowanie niezrównoważonego obciążenia dla wielu pul|

## <a name="simulate-usage-on-all-tenant-databases"></a>Symulowanie użycia we wszystkich baz danych dzierżaw

Generator obciążenia stosuje obciążenie *syntetyczne* wyłącznie do procesorów dla każdej bazy danych dzierżawy. Generator uruchamia zadanie dla każdej bazy danych dzierżawy, co powoduje cykliczne wywołanie procedury składowanej, która generuje obciążenie. Poziomy obciążenia (mierzone w jednostkach eDTU), czas trwania i interwały są zróżnicowane dla wszystkich baz danych, co symuluje nieprzewidywalną aktywność dzierżawy.

1. Ustaw parametr **$DemoScenario** = **2**, *Generate a normal intensity load* (Generowanie obciążenia o normalnym natężeniu).
1. Naciśnij klawisz **F5**, aby zastosować obciążenie do wszystkich baz danych dzierżaw.

Z powodu sporadycznej natury obciążenia należy pozwolić generatorowi na pracę przez 10-20 minut, aby osiągnąć stan ustalony i rozpoznawalny wzorzec.

> [!IMPORTANT]
> Generator obciążenia działa jako seria zadań w lokalnej sesji programu PowerShell. Karta *Demo-PerformanceMonitoringAndManagement.ps1* musi pozostać otwarta! Zamknięcie karty lub wstrzymanie pracy komputera spowoduje zatrzymanie generatora obciążenia.

## <a name="monitor-resource-usage-using-the-portal"></a>Monitorowanie wykorzystania zasobów przy użyciu portalu

Aby móc monitorować użycie zasobów wynikające z zastosowanego obciążenia, należy otworzyć portal dla puli zawierającej bazy danych dzierżaw.

1. Otwórz witrynę [Azure Portal](https://portal.azure.com) i przejdź do serwera tenants1-&lt;UŻYTKOWNIK&gt;.
1. Powinna zostać wyświetlona lista baz danych dzierżaw zawierająca nową partię baz danych.
1. Przewiń w dół, zlokalizuj elastyczne pule i kliknij pozycję **Pool1**. Ta pula zawiera wszystkie bazy danych dzierżaw utworzone dotychczas.
1. Rozwiń blok puli, który się otworzy, i sprawdź wykres wykorzystania puli oraz wykres wykorzystania najbardziej aktywnych baz danych.

Wykorzystanie puli jest zasadniczo zagregowanym wykorzystaniem baz danych dla wszystkich baz danych należących do puli. Wykres wykorzystania baz danych zawiera dane dla 5 najbardziej aktywnych baz danych:

![](./media/sql-database-saas-tutorial-performance-monitoring/pool1.png)

Ponieważ w puli oprócz tych pięciu istnieją również inne bazy danych, wykres wykorzystania puli pokazuje aktywność, która nie jest uwzględniona na wykresie pięciu najbardziej aktywnych baz danych. Aby wyświetlić niektóre dodatkowe szczegóły, kliknij pozycję **Wykorzystanie zasobów bazy danych**:

![](./media/sql-database-saas-tutorial-performance-monitoring/database-utilization.png)


## <a name="set-performance-alerts-on-the-pool"></a>Ustawianie alertów wydajności dla puli

W następujący sposób ustaw dla puli alert, który jest wyzwalany przy wykorzystaniu \>75% trwającym nieprzerwanie przez 5 minut:

1. Otwórz blok *Pool1* (na serwerze *tenants1-\<użytkownik\>*) w witrynie [Azure Portal](https://portal.azure.com).
1. Kliknij pozycję **Reguły alertów**, a następnie kliknij przycisk **+ Dodaj alert**:

   ![dodawanie alertu](media/sql-database-saas-tutorial-performance-monitoring/add-alert.png)

1. Podaj nazwę, taką jak **Wysoki poziom jednostek DTU**, 
1. Ustaw następujące wartości:
   * **Metryka = Procent eDTU**
   * **Warunek = większa niż**.
   * **Próg = 75**.
   * **Okres = W ciągu ostatnich 30 minut**.

   ![ustawianie alertu](media/sql-database-saas-tutorial-performance-monitoring/alert-rule.png)

Można ustawić wysyłanie powiadomień na adres e-mail swojego konta Azure oraz opcjonalnie na inne adresy e-mail (nie jest to zalecane, jeśli nie jesteś właścicielem używanej subskrypcji).

> [!NOTE]
> Ponieważ alert zostanie wyzwolony w przypadku przekroczenia progu w ciągu ostatnich 30 minut, generator obciążenia musi być uruchomiony przez ponad 30 minut, aby przetestować alert.


## <a name="scale-up-a-busy-pool"></a>Skalowanie mocno obciążonej puli w górę

Jeśli poziom zagregowanego obciążenia puli zwiększy się do poziomu maksymalnych możliwości puli i osiągnie 100 % wykorzystania jednostek eDTU, będzie to miało wpływ na wydajność poszczególnych baz danych, co potencjalnie może wydłużyć czasy odpowiedzi na zapytania dla wszystkich baz danych w puli.

Rozwiązaniem krótkoterminowym jest skalowanie w górę puli w celu zapewnienia jej dodatkowych zasobów lub usunięcie baz danych z puli (przeniesienie ich do innych pul lub z puli do warstwy usług autonomicznych).

Rozwiązaniem długoterminowym jest optymalizacja zapytań lub użycie indeksu w celu poprawy wydajności bazy danych. W zależności od wrażliwości aplikacji na problemy wydajnościowe najlepszym rozwiązaniem jest skalowanie puli w górę przed osiągnięciem przez nią wykorzystania jednostek eDTU równego 100%. Użyj alertów do wczesnego ostrzegania.

Zajętość puli można zasymulować, zwiększając obciążenia wytwarzane przez generator. Powodowanie częstszych i dłuższych skoków aktywności zwiększa zagregowane obciążenie puli, nie wpływając na wymagania poszczególnych baz danych. Skalowanie puli w górę można łatwo przeprowadzić w portalu lub z programu PowerShell. W tym ćwiczeniu użyto portalu.

1. Ustaw wartość zmiennej *$DemoScenario* = **3**, _Generate load with longer and more frequent bursts per database_ (Generowanie obciążenia z dłuższymi i częstszymi skokami wartości dla każdej bazy danych) w celu zwiększenia intensywności zagregowanego obciążenia w puli bez zmiany szczytowego obciążenia wymaganego dla każdej bazy danych.
1. Naciśnij klawisz **F5**, aby zastosować obciążenie do wszystkich baz danych dzierżaw.


1. **Otwórz blok puli** **dla puli tenants1/Pool1**.


1. Monitoruj zwiększone wykorzystanie jednostek DTU puli na górnym wykresie. Upłynie kilka minut, zanim nowe wyższe obciążenie stanie się widoczne, ale szybko zauważysz zbliżanie się do 100% wykorzystania puli, zaś w miarę ustalania się nowego wzorca obciążenia nastąpi szybkie przeciążenie puli.


1. Aby przeskalować pulę w górę, kliknij przycisk **Konfiguruj pulę**
1. Przesuń suwak **Pula — eDTU** w położenie 100 (nie zalecamy wyższych wartości, aby uniknąć dodatkowych kosztów). Należy zauważyć, że zagregowana przestrzeń dyskowa dostępna dla wszystkich baz danych w puli, wskazywana przez zmienną **Pula — GB**, jest powiązana z ustawieniem eDTU i również się zwiększa. Zmiana liczby jednostek eDTU puli nie zmienia ustawienia poszczególnych baz danych (które nadal ma maksymalną wartość 50 jednostek eDTU na bazę danych). Ustawienia dla poszczególnych baz danych są widoczne po prawej stronie bloku **Konfiguruj pulę**.
1. Kliknij przycisk **Zapisz**, aby przesłać żądanie. W przypadku puli standardowej zmiana następuje zwykle po 3–5 minutach.

Wróć do ekranu **Pool1** > **Przegląd**, aby wyświetlić wykresy monitorowania. Monitoruj efekt zwiększenia zasobów puli (jednak w przypadku małej ilości baz danych i losowego obciążenia nie zawsze jest łatwo go dostrzec). Patrząc na wykresy należy sobie zdawać sprawę, że 100% na górnym wykresie odpowiada teraz 100 jednostkom eDTU, podczas gdy na dolnym wykresie 100% nadal odpowiada 50 jednostkom eDTU, gdyż maksymalna wartość dla pojedynczej bazy danych nadal wynosi 50 jednostek eDTU.

Bazy danych pozostają w trybie online i są w pełni dostępne podczas całego procesu. W ostatniej chwili przed włączeniem dla każdej bazy danych nowej wartości eDTU dla puli wszystkie aktywne połączenia zostają przerwane. Kod aplikacji powinien być napisany w taki sposób, aby zawsze próbować ponownie nawiązać przerwane połączenie, by móc ponownie połączyć się z bazą danych w puli przeskalowanej w górę.

## <a name="create-a-second-pool-and-load-balance-databases-to-handle-increased-aggregate-load"></a>Tworzenie drugiej puli i równoważenie obciążenia baz danych w celu obsługi zwiększonego zagregowanego obciążenia

Alternatywnym rozwiązaniem w stosunku do skalowania puli w górę jest utworzenie drugiej puli i przeniesienie do niej baz danych w celu zrównoważenia obciążenia między dwoma pulami. Aby to rozwiązanie zadziałało, nowa pula musi zostać utworzona na tym samym serwerze co pierwsza.

1. Otwórz **blok serwera dla serwera customers1-&lt;UŻYTKOWNIK&gt;**. Jeśli jesteś w bloku bazy danych lub puli, możesz rozwinąć podstawowy element sterujący i wybrać nazwę serwera jako skrót.
1. Kliknij przycisk **+ Nowa pula**, aby utworzyć pulę na bieżącym serwerze
1. W szablonie nowej elastycznej puli baz danych:

    1. Ustaw **Nazwa = Pool2**.
    1. Pozostaw warstwę cenową **Pula Standardowa**.
    1. Kliknij przycisk **Konfiguruj pulę**,
    1. W bloku Konfigurowanie puli, który się otworzy, ustaw **Pula — eDTU = 50 jednostek DTU**.
    1. Kliknij polecenie **Dodaj bazy danych**, aby wyświetlić listę baz danych na tym serwerze, które nie znajdują się w bieżącej puli.
    1. Na liście **zaznacz** połowę baz danych (10 z 20), aby przenieść je do nowej puli, a następnie kliknij przycisk **Wybierz**.
    1. Ponownie kliknij przycisk **Wybierz**, aby zatwierdzić zmiany w konfiguracji. Zwróć uwagę na szacowany koszt jednego miesiąca użytkowania z wybranymi opcjami.
    1. Wybierz przycisk **OK**, aby utworzyć nową pulę z nową konfiguracją i przenieść bazy danych.

Utworzenie puli i przeniesienie do niej baz danych zajmie kilka minut. Każda przenoszona baza danych pozostaje w trybie online i jest w pełni dostępna do ostatniej chwili, po czym są zamykane wszystkie otwarte połączenia. Po ponowieniu przez klienta próby nawiązania połączenia, połączy się on z bazą danych w nowej puli.

Utworzona pula pojawi się w bloku serwera customers1. Kliknij nazwę puli, aby otworzyć blok puli i monitorować jej wydajność.

Należy zwrócić uwagę, że wykorzystanie zasobów w puli Pool1 zmalało, a pula Pool2 jest podobnie obciążona.

## <a name="manage-an-increased-load-on-a-single-database"></a>Zarządzanie zwiększonym obciążeniem pojedynczej bazy danych

Jeśli stałe zwiększone obciążenie dotyczy pojedynczej bazy danych w puli, to w zależności od konfiguracji puli może ona zdominować zasoby w puli i wpływać na pozostałe bazy danych. Jeśli prawdopodobne jest, że takie zachowanie będzie się utrzymywało przez pewien czas, można tymczasowo przenieść tę bazę danych poza pulę. Dzięki temu baza danych otrzyma więcej zasobów niż pozostałe bazy w puli i jednocześnie zostanie odizolowana od innych baz danych. W tym ćwiczeniu zostanie zasymulowane zwiększone obciążenie dotyczące miejsca Contoso Concert Hall wywołane wzmożonym pobytem na bilety na popularny koncert.

1. W skrypcie …\\**Demo-PerformanceManagementAndMonitoring**.ps1
1. Ustaw wartość zmiennej **$DemoScenario = 5, Generate a normal load plus a high load on a single tenant (approx. 95 DTU)** (Generowanie normalnego obciążenia oraz wysokiego obciążenia dla jednej dzierżawy (ok. 95 jednostek DTU)).
1. Ustaw wartość zmiennej **$SingleTenantDatabaseName = contosoconcerthall**
1. Wykonaj skrypt, używając klawisza **F5**.
1. **Otwórz blok puli**  **dla puli Customers1/Pool1**.
1. Przyjrzyj się okienku **Monitorowanie pul elastycznych** u góry bloku i poszukaj puli o zwiększonym wykorzystaniu jednostek DTU. Po minucie lub dwóch powinno być widoczne wyższe obciążenie, które wkrótce osiągnie poziom 100% wykorzystania puli.
1. Należy również monitorować okienko **Monitorowanie elastycznych baz danych**, w którym są wyświetlane bazy danych najbardziej aktywne w ciągu ostatniej godziny. Baza danych contosoconcerthall wkrótce powinna zostać wyświetlona jako jedna z 5 najbardziej aktywnych baz danych.
1. **Kliknij wykres Monitorowanie elastycznych baz danych******, co spowoduje otwarcie bloku **Wykorzystanie zasobów bazy danych**, w którym można selektywnie monitorować dowolną bazę danych. Dzięki temu na ekranie można wyświetlać wyłącznie informacje dotyczące bazy danych contosoconcerthall.
1. Na liście baz danych **kliknij pozycję contosoconcerthall**, co spowoduje otwarcie odpowiedniego bloku bazy danych.
1. Kliknij pozycję **Warstwa cenowa (jednostki DTU skalowania)** w menu kontekstowym, aby otworzyć blok **Konfigurowanie wydajności**, w którym można ustawić izolowany poziom wydajności bazy danych.
1. Kliknij kartę **Standardowa**, aby otworzyć opcje skalowania w warstwie Standardowa.
1. Przesuń **suwak DTU** w prawo, aby wybrać 100 jednostek DTU. Należy zauważyć, że odpowiada to celowi usługi **S3** widocznemu w nawiasach pomiędzy miernikami jednostek DTU i przestrzeni dyskowej.
1. Kliknij przycisk **Zastosuj**, aby usunąć bazę danych z puli i skonfigurować ją jako standardową bazę danych S3.
1. Po zakończeniu wdrożenia należy monitorować jego wpływ na bazę danych contosoconcerthall oraz na pulę, z której baza została usunięta, używając bloków elastycznej puli i bazy danych.

Gdy opadnie większe niż zwykle obciążenie bazy danych contosoconcerthall, należy niezwłocznie przenieść ją z powrotem do puli, aby zmniejszyć koszt. Jeśli nie wiadomo, kiedy to nastąpi, można ustawić alert w bazie danych, który zostanie uruchomiony po spadku wykorzystania jednostek DTU poniżej maksymalnej wartości określonej dla każdej bazy danych w puli. Przenoszenie bazy danych do puli opisano w ćwiczeniu 5.

## <a name="other-performance-management-patterns"></a>Inne wzorce zarządzania wydajnością

**Skalowanie uprzedzające** W ćwiczeniu 6, w którym dokonywano skalowania izolowanej bazy danych, było wiadomo, której bazy danych poszukujemy. Gdyby zarząd firmy Contoso Concert Hall poinformował firmę WTP o zbliżającej się intensywnej sprzedaży biletów, baza mogłaby zostać zabrana z puli, uprzedzając zdarzenie. W przeciwnym razie konieczne byłoby skonfigurowanie alertów dla puli lub bazy danych w celu informowania o zmieniającej się sytuacji. Byłoby wysoce nieprzyjemne, gdyby wiadomość o zdarzeniu pochodziła od innych dzierżawców w puli, skarżących się na pogorszenie wydajności. Jeśli jednak dzierżawca może przewidzieć, jak długo będą mu potrzebne dodatkowe zasoby, możesz tak skonfigurować element Runbook usługi Azure Automation, aby przenieść bazę danych z puli, a następnie z powrotem do puli zgodnie z ustalonym harmonogramem.

**Samoobsługowe skalowanie przez dzierżawcę** Ponieważ skalowanie jest zadaniem łatwo wywoływanym za pośrednictwem interfejsu API zarządzania, można w prosty sposób wbudować możliwość skalowania baz danych dzierżawy w aplikację używaną przez dzierżawcę i zaoferować ją jako funkcję usługi SaaS. Na przykład, można umożliwić dzierżawcom samodzielne administrowanie skalowaniem w górę i w dół, być może bezpośrednio powiązane z ich rozliczeniami!

### <a name="scaling-a-pool-up-and-down-on-a-schedule-to-match-usage-patterns"></a>Skalowanie puli w górę i w dół zgodnie z harmonogramem odpowiadającym wzorcom użycia

W przypadku, gdy zagregowane wykorzystanie dzierżaw jest zgodne z przewidywalnymi wzorcami użycia można użyć usługi Azure Automation do skalowania puli w górę i w dół zgodnie z harmonogramem. Na przykład pulę można skalować w dół po godzinie 18:00 i ponownie w górę przed godziną 6:00 w dni robocze, jeśli wiadomo, że pomiędzy tymi godzinami występuje spadek wymagań dotyczących zasobów.



## <a name="next-steps"></a>Następne kroki

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Symulowanie korzystania z baz danych dzierżaw przy użyciu dostarczonego generatora obciążenia
> * Monitorowanie reakcji baz danych dzierżaw na wzrost obciążenia
> * Skalowanie w górę elastycznej puli w reakcji na zwiększone obciążenie bazy danych
> * Aprowizacja drugiej elastycznej puli w celu równoważenia aktywności baz danych

[Samouczek przywracania pojedynczej dzierżawy](sql-database-saas-tutorial-restore-single-tenant.md)


## <a name="additional-resources"></a>Dodatkowe zasoby

* [Dodatkowe samouczki nawiązujące do początkowego wdrożenia aplikacji Wingtip Tickets Platform (WTP)](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Pule elastyczne SQL](sql-database-elastic-pool.md)
* [Azure Automation](../automation/automation-intro.md)
* [Log Analytics](sql-database-saas-tutorial-log-analytics.md) — samouczek konfigurowania usługi Log Analytics i korzystania z niej
