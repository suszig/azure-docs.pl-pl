---
title: "Zarządzanie wielu baz danych przy użyciu elastycznej puli platformy Azure | Dokumentacja firmy Microsoft"
description: "Zarządzanie i skalowanie wielu baz danych SQL — setki i tysięcy - wykorzystujących pule elastyczne. Jedna cena dla zasobów, którą można dystrybuować, w razie potrzeby."
keywords: "wiele baz danych, bazy danych zasobów, wydajność bazy danych"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: b46e7fdc-2238-4b3b-a944-8ab36c5bdb8e
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.date: 10/11/2017
ms.author: carlrab
ms.workload: Active
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 2f1ff7a7c2ecf04069ffa6afcc66e2f0f9915b35
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="elastic-pools-help-you-manage-and-scale-multiple-azure-sql-databases"></a>Elastyczne pule pozwalają na zarządzanie i skalowania wielu baz danych Azure SQL

Pule elastyczne bazy danych SQL są proste i ekonomiczne rozwiązanie do zarządzania i skalowanie wielu baz danych, które mają wymagania użycia różnych i nieprzewidywalnych. Bazy danych w puli elastycznej znajdują się na jednym serwerze bazy danych SQL Azure i udostępnianie jest określona liczba zasobów ([elastycznych jednostkach transakcji bazy danych](sql-database-what-is-a-dtu.md) (Edtu)) w cenie zestawu. Pule elastyczne pule w usłudze Azure SQL Database umożliwiają deweloperom SaaS optymalizację stosunku ceny do wydajności dla grupy baz danych w ramach określonego budżetu, zapewniając jednocześnie elastyczność wydajności dla każdej bazy danych. 

> [!NOTE]
> Pule elastyczne są ogólnodostępne we wszystkich regionach platformy Azure oprócz Indii Zachodnich, gdzie są obecnie dostępne w wersji zapoznawczej. Pule elastyczne zostaną udostępnione ogólnie w tych regionach tak szybko, jak to możliwe.
>

## <a name="what-are-sql-elastic-pools"></a>Co to są pule elastyczne SQL? 

Deweloperzy SaaS tworzą aplikacje w oparciu o warstwy danych w dużej skali składające się z wielu baz danych. Typowym wzorcem aplikacji jest udostępnianie jednej bazy danych dla każdego klienta. Jednak różni klienci często mają różne i nieprzewidywalne wzorce użycia i trudno przewidzieć wymagania dotyczące zasobów dla każdego indywidualnego użytkownika bazy danych. Tradycyjnie masz dwie opcje: 

- Nadmiernego udostępniania zasobów opartych na szczytowe użycie i za pośrednictwem płatności, lub
- Niepełną należy zapisać koszt kosztem wydajności i odbiorcy zadowolenia użytkowników podczas pików. 

Pule elastyczne rozwiązać ten problem, zapewniając, że bazy danych uzyskać zasobów wydajności, które są im potrzebne, gdy są one wymagane. Udostępniają one prosty mechanizm alokacji zasobów w ramach przewidywalnego budżetu. Aby dowiedzieć się więcej na temat wzorców projektowych dla aplikacji SaaS wykorzystujących pule elastyczne, zobacz artykuł [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) (Wzorce projektowe dla wielodostępnych aplikacji SaaS korzystających z usługi Azure SQL Database).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Pule elastyczne Włączanie deweloperowi zakupu [elastycznych jednostkach transakcji bazy danych](sql-database-what-is-a-dtu.md) (Edtu) dla puli współużytkowane przez wiele baz danych do uwzględnienia nieprzewidywalne okresów użycia przez pojedyncze bazy danych. Wymagania dotyczące liczby jednostek eDTU dla puli są określane na podstawie zagregowanego użycia jej baz danych. Dostępna liczba jednostek eDTU dla puli jest kontrolowana przez budżet dewelopera. Deweloper po prostu dodaje bazy danych do puli, ustawia minimalną i maksymalną liczbę jednostek eDTU dla baz danych, a następnie ustawia liczbę jednostek eDTU puli na podstawie swojego budżetu. Korzystając z pul, deweloper może bezproblemowo rozwijać swoją usługę od niewielkiego startupu do dojrzałego biznesu w coraz większej skali.

Poszczególne bazy danych w ramach puli mają możliwość elastycznego skalowania automatycznego w określonym zakresie parametrów. W przypadku dużego obciążenia baza danych może wykorzystywać więcej jednostek eDTU w odpowiedzi na zapotrzebowanie. Bazy danych pod niskim obciążeniem wykorzystują mniej jednostek eDTU, a bazy danych bez obciążenia nie zużywają jednostek eDTU. Aprowizacja zasobów dla całej puli zamiast pojedynczych baz danych upraszcza zadania związane z zarządzaniem. Ponadto pule mają przewidywalny budżet. Kolejne jednostki eDTU można dodać do istniejącej puli bez przestoju bazy danych z tym zastrzeżeniem, że może zaistnieć konieczność przeniesienia baz danych w celu zapewnienia dodatkowych zasobów obliczeniowych dla nowej rezerwacji jednostek eDTU. Podobnie, jeśli dodatkowe jednostki eDTU nie są już potrzebne, można je usunąć z istniejącej puli w dowolnej chwili. Można również dodawać bazy danych do puli lub odejmować je z niej. Jeśli baza danych przewidywalnie niewystarczająco wykorzystuje zasoby, należy ją przenieść.

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>Kiedy należy rozważyć puli elastycznej bazy danych SQL

Pule są odpowiednie dla wielu baz danych o określonych wzorcach użycia. Dla danej bazy danych ten wzorzec charakteryzuje się niskim średnim wykorzystaniem oraz stosunkowo rzadkimi okresami zwiększonego użycia.

Im więcej baz danych możesz dodać do puli, tym większe uzyskujesz oszczędności. W zależności od wzorca użycia aplikacji możliwe jest uzyskanie oszczędności nawet już przy dwóch bazach danych S3. 

Poniższe sekcje ułatwiają zrozumienie, jak ocenić, czy dodanie konkretnej kolekcji baz danych do puli może być korzystne. W przykładach użyto pul warstwy Standardowa, ale te same zasady mają zastosowanie także do pul w warstwach Podstawowa i Premium.

### <a name="assessing-database-utilization-patterns"></a>Ocena wzorców użycia bazy danych

Na poniższej ilustracji pokazano przykład bazy danych, która przez większość czasu pozostaje bezczynna, ale okresowo doświadcza dużego wzrostu aktywności. Jest to wzorzec użycia, który jest odpowiedni dla puli:

   ![pojedyncza baza danych odpowiednia dla puli](./media/sql-database-elastic-pool/one-database.png)

W przedstawionym okresie pięciu minut baza danych DB1 odnotowuje wzrost użycia do 90 DTU, ale jej całkowite średnie użycie jest mniejsze niż pięć DTU. Do obsługi tego obciążenia w jednej bazie danych wymagany jest poziom wydajności S3, ale spowoduje to, że większość zasobów pozostanie nieużywana w okresach niskiej aktywności.

Pula umożliwia udostępnienie tych nieużywanych jednostek DTU wielu bazom danych, a tym samym zmniejsza liczbę potrzebnych jednostek DTU i obniża koszty ogólne.

Rozwijając poprzedni przykład, załóżmy, że istnieją dodatkowe bazy danych o podobnych wzorcach użycia jak baza danych DB1. Na następnych dwóch ilustracjach poniżej użycie czterech i dwudziestu baz danych przedstawiono nałożone na tym samym wykresie, aby zilustrować nienakładający się charakter ich użycia w czasie:

   ![cztery bazy danych z wzorcem użycia odpowiednim dla puli](./media/sql-database-elastic-pool/four-databases.png)

   ![dwadzieścia baz danych z wzorcem użycia odpowiednim dla puli](./media/sql-database-elastic-pool/twenty-databases.png)

Zagregowane użycie jednostek DTU we wszystkich 20 bazach danych przedstawia czarna linia na poprzednim rysunku. Pokazuje ona, że zagregowane użycie jednostek DTU nigdy nie przekracza 100 DTU i wskazuje, że te 20 baz danych może współużytkować 100 jednostek eDTU w tym okresie. Skutkuje to dwudziestokrotnym zmniejszeniem liczby DTU i trzynastokrotnie niższą ceną w porównaniu do umieszczenia każdej z baz danych na poziomie wydajności S3 dla pojedynczej bazy danych.

Ten przykład jest idealny z następujących przyczyn:

* Istnieją duże różnice między użyciem szczytowym a średnim użyciem na bazę danych. 
* Użycie szczytowe dla poszczególnych baz danych występuje w różnych punktach w czasie.
* Jednostki eDTU są współdzielone przez wiele baz danych.

Cena puli jest funkcją jednostek eDTU puli. Chociaż cena jednostki eDTU dla puli jest 1,5 raza większa niż cena jednostki DTU dla pojedynczej bazy danych, **jednostki eDTU puli mogą być współdzielone przez wiele baz danych, a tym samym potrzebna jest mniejsza całkowita liczba jednostek eDTU**. Te różnice w cenie i współużytkowanie jednostek eDTU są podstawą potencjalnych oszczędności, które mogą zapewnić pule. 

Następujące reguły akceptacji powiązane z liczbą baz danych i użyciem baz danych pomagają upewnić się, że pula zapewnia mniejsze koszty w porównaniu z użyciem poziomów wydajności dla pojedynczych baz danych.

### <a name="minimum-number-of-databases"></a>Minimalna liczba baz danych

Jeśli suma liczby jednostek DTU poziomów wydajności dla pojedynczych baz danych jest większa niż 1,5 x liczba eDTU potrzebnych dla puli, pula elastyczna jest bardziej opłacalna. Aby sprawdzić dostępne rozmiary, zobacz [limity liczby jednostek eDTU i magazynu dla pul elastycznych i elastycznych baz danych](sql-database-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).

***Przykład***<br>
Aby pula 100 eDTU była bardziej opłacalna niż korzystanie z poziomów wydajności dla pojedynczych baz danych, potrzebne są co najmniej dwie baz danych S3 lub co najmniej 15 baz danych S0.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Maksymalna liczba baz danych z równoczesnymi szczytami użycia

W przypadku współużytkowania jednostek eDTU nie wszystkie bazy danych w puli mogą jednocześnie używać eDTU do wysokości limitu dostępnego podczas używania poziomów wydajności dla pojedynczych baz danych. Im mniejsza liczba baz danych, które jednocześnie odnotowują szczytowe użycie, tym mniejszą liczbę jednostek eDTU można ustawić dla puli i tym bardziej opłacalne staje się korzystanie z puli. Ogólnie rzecz biorąc, nie więcej niż 2/3 (lub 67%) baz danych w puli powinno jednocześnie doświadczać wzrostu aktywności do limitu liczby jednostek eDTU.

***Przykład***<br>
Aby zmniejszyć koszty dla trzech baz danych S3 w puli 200 eDTU, co najwyżej dwie z tych baz danych mogą jednocześnie osiągać szczytowe użycie. W przeciwnym razie, jeśli więcej niż dwie z tych czterech baz danych S3 jednocześnie osiągają szczytowe użycie, rozmiar puli musiałby zostać zwiększony do ponad 200 eDTU. Jeśli rozmiar puli będzie większy niż 200 eDTU, konieczne będzie dodanie większej liczby baz danych S3, aby koszty puli pozostały niższe niż w przypadku poziomów wydajności dla pojedynczych baz danych.

Należy zauważyć, że w tym przykładzie nie jest rozważane użycie innych baz danych w puli. Jeśli wszystkie bazy danych mają jakiś poziom użycia w dowolnym danym momencie czasu, wówczas mniej niż 2/3 (lub 67%) baz danych może jednocześnie osiągać szczytowe użycie.

### <a name="dtu-utilization-per-database"></a>Użycie jednostek DTU na bazę danych
Duża różnica między szczytowym i średnim użyciem bazy danych wskazuje na dłuższe okresy niewielkiego wykorzystania i krótkie okresy wysokiego wykorzystania. Ten wzorzec użycia jest idealny dla współużytkowania zasobów między bazami danych. Bazę danych należy rozważyć do umieszczenia w puli, jeśli jej szczytowe użycie jest ponad 1,5 raza większe niż jej średnie użycie.

***Przykład***<br>
Baza danych S3, która osiąga szczytowe użycie do 100 DTU, a przeciętnie używa 67 DTU lub mniej, jest dobrym kandydatem do współużytkowania jednostek eDTU w puli. Również baza danych S1, która osiąga szczytowe użycie 20 DTU, a przeciętnie używa 13 DTU lub mniej, jest dobrym kandydatem do umieszczenia w puli.

## <a name="how-do-i-choose-the-correct-pool-size"></a>Jak wybrać rozmiar puli poprawna?

Najlepszy rozmiar puli zależy od zagregowanej liczby jednostek eDTU i zasobów magazynu wymaganych dla wszystkich baz danych w puli. Obejmuje to określenie największej z następujących wielkości:

* Maksymalna liczba jednostek DTU wykorzystana przez wszystkie bazy danych w puli.
* Maksymalna liczba bajtów magazynu wykorzystana przez wszystkie bazy danych w puli.

Aby sprawdzić dostępne rozmiary, zobacz [limity liczby jednostek eDTU i magazynu dla pul elastycznych i elastycznych baz danych](sql-database-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).

Usługa SQL Database automatycznie ocenia historyczne użycie zasobów przez bazy danych na istniejącym serwerze usługi SQL Database i zaleca odpowiednią konfigurację puli w witrynie Azure Portal. Oprócz zaleceń, wbudowane środowisko szacuje użycie jednostek eDTU dla niestandardowej grupy baz danych na serwerze. Umożliwia to przeprowadzenie analizy warunkowej przez interaktywne dodawanie baz danych do puli i usuwanie ich w celu uzyskania analizy użycia zasobów i porady dotyczącej rozmiaru przed zatwierdzeniem zmian. Aby uzyskać instrukcje, zobacz [Monitorowanie puli elastycznej, zmienianie jej rozmiaru i zarządzanie nią](sql-database-elastic-pool-manage-portal.md).

W przypadkach, gdy nie można użyć narzędzi, skorzystanie z następujących instrukcji krok po kroku może ułatwić oszacowanie, czy pula jest bardziej opłacalna niż pojedyncza baza danych:

1. Oszacuj liczbę jednostek eDTU potrzebnych dla puli w następujący sposób:

   MAX(<*łączna liczba baz danych* X *średnie użycie jednostek DTU na bazę danych*>,<br>
   <*liczba baz danych jednocześnie osiągających szczyt użycia* X *użycie szczytowe jednostek DTU na bazę danych*)
2. Oszacuj miejsce do magazynowania wymagane dla puli przez dodanie liczby bajtów potrzebnych dla wszystkich baz danych w puli. Następnie określ rozmiar puli (w jednostkach eDTU), który zapewni tę ilość miejsca przechowywania. Aby uzyskać informacje o limitach magazynu puli na podstawie rozmiaru puli w jednostkach eDTU, zobacz [limity liczby jednostek eDTU i magazynu dla pul elastycznych i elastycznych baz danych](sql-database-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).
3. Weź większą z wartości szacunkowych eDTU z kroku 1 i kroku 2.
4. Wejdź na [stronę cennika usługi SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) i znajdź najmniejszy rozmiar puli jednostek eDTU większy niż szacunkowa wartość z kroku 3.
5. Porównaj cenę puli z kroku 5 z ceną za używanie odpowiednich poziomów wydajności dla pojedynczych baz danych.

## <a name="using-other-sql-database-features-with-elastic-pools"></a>Używanie innych funkcji bazy danych SQL z pul elastycznych

### <a name="elastic-jobs-and-elastic-pools"></a>Zadania elastyczne i pul elastycznych

W przypadku puli zadania zarządzania są uproszczone dzięki uruchamianiu skryptów w **[zadaniach elastycznych](sql-database-elastic-jobs-overview.md)**. Zadanie elastyczne eliminuje większość monotonnych czynności związanych z dużą liczbą baz danych. Aby rozpocząć, zobacz [Wprowadzenie do zadań elastycznych](sql-database-elastic-jobs-getting-started.md).

Aby uzyskać więcej informacji na temat innych narzędzi do pracy z wieloma bazami danych, zobacz artykuł dotyczący [skalowania w poziomie za pomocą usługi Azure SQL Database](sql-database-elastic-scale-introduction.md).

### <a name="business-continuity-options-for-databases-in-an-elastic-pool"></a>Opcje ciągłości działania dla baz danych w puli elastycznej
Bazy danych w puli obsługują te same [funkcje ciągłości działalności biznesowej](sql-database-business-continuity.md), które są dostępne dla pojedynczych baz danych.

- **W momencie przywracania**: w momencie przywracania używa kopii zapasowych bazy danych automatyczne odzyskać bazę danych w puli do określonego punktu w czasie. Zobacz [Przywracanie do punktu w czasie](sql-database-recovery-using-backups.md#point-in-time-restore).

- **Geograficzne**: geograficzne zapewnia domyślna opcja odzyskiwania, gdy baza danych jest niedostępna z powodu zdarzenia w regionie, w którym hostowana jest baza danych. Zobacz [Restore an Azure SQL Database or failover to a secondary](sql-database-disaster-recovery.md) (Przywracanie usługi Azure SQL Database lub przełączanie w trybie failover do pomocniczej bazy danych).

- **Aktywna replikacja geograficzna**: w przypadku aplikacji, które są bardziej agresywną wymagania odzyskiwania niż geograficzne mogą oferować, skonfigurować [aktywna replikacja geograficzna](sql-database-geo-replication-overview.md).

## <a name="manage-elastic-pools-and-databases-using-the-azure-portal"></a>Zarządzanie pule elastyczne i baz danych przy użyciu portalu Azure

### <a name="creating-a-new-sql-database-elastic-pool-using-the-azure-portal"></a>Tworzenie nowej puli elastycznej bazy danych SQL przy użyciu portalu Azure

Istnieją dwa sposoby, w portalu Azure można utworzyć puli elastycznej. Można utworzyć ją od podstaw, znając wymagane ustawienia puli, lub uruchomić zgodnie z zaleceniami usługi. SQL Database ma wbudowane narzędzie analizy zalecane ustawienia puli elastycznej, jeśli jest to bardziej ekonomiczne na podstawie ostatnich danych telemetrii użycia baz danych. 

Tworzenie puli elastycznej z istniejącej strony serwera w portalu jest najprostszym sposobem, aby przenieść istniejące bazy danych w puli elastycznej. Można również utworzyć puli elastycznej, wyszukując **puli elastycznej SQL** w **Marketplace** lub klikając **+ Dodaj** na stronie pule elastyczne SQL. Jesteś w stanie umożliwia określenie nowego lub istniejącego serwera do obsługi przepływu pracy w tej puli.

> [!NOTE]
> Można utworzyć wiele pul na serwerze, ale nie można dodać bazy danych z różnych serwerów do tej samej puli.
> 

Warstwa cenowa puli Określa funkcje dostępne dla elastics w puli, a maksymalna liczba jednostek Edtu (maks. wartość eDTU) i dostępne dla każdej bazy danych magazynu (GB). Aby uzyskać więcej informacji, zobacz [ogranicza zasobów dla pul elastycznych](sql-database-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).

Aby zmienić warstwę cenową dla puli, kliknij przycisk **Warstwa cenowa**, kliknij wybraną warstwę cenową, a następnie kliknij przycisk **Wybierz**.

> [!IMPORTANT]
> Po wybraniu warstwy cenowej i kliknięciu przycisku **OK** w ostatnim kroku w celu zatwierdzenia wprowadzonych zmian nie można już zmienić warstwy cenowej puli. Aby zmienić warstwę cenową istniejącej puli elastycznej, tworzenie elastycznej puli w ramach żądanej warstwy cenowej i przeprowadzić migrację bazy danych do nowej puli.
>

Jeśli bazy danych, z którymi pracujesz, mają wystarczającą ilość historycznych danych telemetrycznych dotyczących użycia, aktualizacje wykresu **Szacowany poziom użycia jednostek eDTU i GB** oraz wykresu słupkowego **Rzeczywiste użycie jednostek eDTU** ułatwiają podejmowanie decyzji związanych z konfiguracją. Ponadto usługa może ułatwić wybranie odpowiedniego rozmiaru puli, wyświetlając komunikat z zaleceniami.

Usługa SQL Database ocenia historyczne dane użycia bazy danych i zaleca zastosowanie co najmniej jednej puli baz danych, jeśli okaże się, że jest to bardziej ekonomiczne rozwiązanie niż używanie pojedynczych baz danych. Każde zalecenie jest konfigurowane z unikatowym podzbiorem baz danych serwera, które są najlepiej dopasowane do puli.

![zalecana pula](./media/sql-database-elastic-pool-create-portal/recommended-pool.png) 

Zalecenie puli obejmuje:

- Warstwę cenową dla puli (podstawowa, standardowa, Premium lub Premium RS)
- Odpowiednie **Jednostki eDTU PULI** (określane również jako Maks. wartość eDTU na pulę)
- **Maks. wartość eDTU** i **Min. wartość eDTU** na bazę danych
- Listę zalecanych baz danych dla puli

> [!IMPORTANT]
> Podczas rekomendowania pul usługa uwzględnia dane telemetryczne z ostatnich 30 dni. Dla bazy danych wziąć pod uwagę jako potencjalny puli elastycznej musi istnieć co najmniej 7 dni. Bazy, które znajdują się już w puli elastycznej, nie są brane pod uwagę podczas tworzenia zaleceń dla puli elastycznej.
>

Usługa oblicza zapotrzebowanie na zasoby oraz opłacalność przenoszenia pojedynczych baz danych w każdej warstwie usług do pul w tej samej warstwie. Na przykład wszystkie bazy danych w warstwie Standardowej na serwerze są oceniane pod względem dopasowania do Standardowej puli elastycznej. Oznacza to, że usługa nie tworzy zaleceń międzywarstwowych, takich jak przeniesienie bazy danych z warstwy Standardowej do puli Premium.

Po dodaniu bazy danych do puli, zalecenia są generowane dynamicznie na podstawie historycznych danych użycia wybranych baz danych. Te zalecenia są wyświetlane w jednostek eDTU i GB wykres użycia i transparent zalecenia w górnej części **Konfigurowanie puli** strony. Zalecenia te mają na celu ułatwić tworzenie puli elastycznej zoptymalizowane pod kątem konkretnych baz danych.

![zalecenia dynamiczne](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

### <a name="manage-and-monitor-an-elastic-pool"></a>Zarządzanie i monitorowanie puli elastycznej

W portalu Azure można monitorować użycie puli elastycznej i baz danych w tej puli. Można również utworzyć zestaw zmian do puli elastycznej i Prześlij wszystkie zmiany w tym samym czasie. Te zmiany obejmują dodawanie lub usuwanie baz danych, zmiana ustawień puli elastycznej lub zmiany ustawień bazy danych.

Na poniższym rysunku przedstawiono przykład puli elastycznej. Widok zawiera:

* Wykresy do monitorowania użycia zasobów w puli elastycznej i baz danych zawartych w puli.
* **Konfiguruj** puli przycisk, aby wprowadzić zmiany w puli elastycznej.
* **Utwórz bazę danych** przycisku, który tworzy bazę danych i dodaje go do bieżącej puli elastycznej.
* Zadania elastyczne, które ułatwiają zarządzanie dużej liczby baz danych, uruchamiając skrypty Transact SQL wszystkich baz danych na liście.

![Widok puli](./media/sql-database-elastic-pool-manage-portal/basic.png)

Można przejść do określonej puli, aby wyświetlić jego wykorzystania zasobów. Domyślnie skonfigurowano puli pokazanie użycia magazynu i liczby jednostek eDTU w ciągu ostatniej godziny. Wykres można skonfigurować do wyświetlenia różnych metryk za pośrednictwem różnych okna czasowe. Kliknij przycisk **wykorzystania zasobów** wykresu w obszarze **monitorowania puli elastycznej** pokazanie wyświetlenia szczegółowych informacji o określonej metryki na określone okno czasu.

![Monitorowanie puli elastycznej](./media/sql-database-elastic-pool-manage-portal/basic-2.png)

![Metryki strony](./media/sql-database-elastic-pool-manage-portal/metric.png)

### <a name="to-customize-the-chart-display"></a>Aby dostosować wyświetlanie wykresu

Wykres i metryki strony, aby wyświetlić innych metryk, takich jak procent, procent we/wy danych i dziennika we/wy procent wykorzystania procesora CPU można edytować.

![Kliknij przycisk Edytuj](./media/sql-database-elastic-pool-manage-portal/edit-metric.png)

Na **Edytuj wykres** formularza, wybierz zakres czasu (Ostatnia godzina dzisiaj, lub ostatni tydzień), lub kliknij przycisk **niestandardowych** aby wybrać wszystkie zakres dat w ciągu ostatnich dwóch tygodni. Można wybrać pasek lub wykres liniowy, a następnie wybierz zasoby do monitorowania.

> [!Note]
> Tylko metryki z tej samej jednostki miary mogą być wyświetlane na wykresie, w tym samym czasie. Na przykład w przypadku wybrania "procent liczby jednostek eDTU" następnie można wybrać tylko innych metryk odsetku jako jednostka miary.
>

![Kliknij przycisk Edytuj](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

### <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Zarządzanie i monitorowanie baz danych w puli elastycznej

Można również monitorować pojedyncze bazy danych dla potencjalne problemy. W obszarze **elastycznej bazy danych monitorowania**, jest wykres przedstawia metryki pięć baz danych. Domyślnie wykresu wyświetla top 5 baz danych w puli przez użycie w jednostkach eDTU średni w ostatniej godziny. 

![Monitorowanie puli elastycznej](./media/sql-database-elastic-pool-manage-portal/basic-3.png)

Kliknij przycisk **użycie w jednostkach eDTU dla bazy danych dla ostatniej godziny** w obszarze **elastycznej bazy danych monitorowania**. Spowoduje to otwarcie **wykorzystania zasobów bazy danych** i udostępnia szczegółowy widok użycia bazy danych w puli. Przy użyciu siatki w dolnej części strony, można wybrać żadnych baz danych w puli, aby wyświetlić jego użycia na wykresie (maksymalnie 5 bazy danych). Można również dostosować metryki i przedziału czasowego wyświetlane na wykresie, klikając **Edytuj wykres**.

![Strona wykorzystania zasobów bazy danych](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

### <a name="to-customize-the-view"></a>Aby dostosować widok

Można edytować na wykresie, aby wybrać zakres czasu (Ostatnia godzina lub po 24 godzinach), lub kliknij przycisk **niestandardowych** wybierz inny dzień w ciągu ostatnich 2 tygodni do wyświetlenia.

![Kliknij pozycję Edytuj wykres](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

![Kliknij przycisk niestandardowe](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

Możesz również kliknąć **porównania baz danych przez** listy rozwijanej, aby wybrać inną metrykę do używania przy porównywaniu baz danych.

![Edytuj wykres](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Aby wybrać baz danych do monitorowania

Na liście bazy danych na **wykorzystania zasobów bazy danych** strony, można znaleźć określonej bazy danych przez wyszukiwanie na stronach na liście lub wpisując nazwy bazy danych. Użyj pola wyboru, aby wybrać bazę danych.

![Wyszukaj baz danych do monitorowania](./media/sql-database-elastic-pool-manage-portal/select-dbs.png)


### <a name="add-an-alert-to-an-elastic-pool-resource"></a>Dodawanie alertu do zasobu puli elastycznej

Reguły można dodać do puli elastycznej puli elastycznej trafienia ustawiony próg użycia wysyłania wiadomości e-mail do osób lub alertu ciągów do adresu URL punktów końcowych.

**Aby dodać alert do dowolnego zasobu:**

1. Kliknij przycisk **wykorzystania zasobów** wykresu, aby otworzyć **Metryka** kliknij przycisk **Dodaj alert**, a następnie wypełnij informacje w **dodać regułę alertu** strony (**zasobów** jest automatycznie ustawiony do pracy z puli).
2. Wpisz **nazwa** i **opis** , które identyfikują alert i adresaci.
3. Wybierz **Metryka** przewidzianą do alertów z listy.

   Wykres przedstawia dynamicznie wykorzystania zasobów dla tej metryki, pomaga wybrać progu.

4. Wybierz **warunku** (większe niż poniżej, itd.) i **próg**.
5. Wybierz **okres** czas, przez który metryki reguły muszą zostać spełnione przed wyzwalaczy alertu.
6. Kliknij przycisk **OK**.

Aby uzyskać więcej informacji, zobacz [tworzyć alerty bazy danych SQL w portalu Azure](sql-database-insights-alerts-portal.md).

### <a name="move-a-database-into-an-elastic-pool"></a>Przenoszenie bazy danych w puli elastycznej

Można dodać lub usunąć bazy danych z istniejącej puli. Bazy danych może być w innych pulach. Jednak można dodawać tylko baz danych, które znajdują się na tym samym serwerze logicznym.

 ![Kliknij przycisk Konfiguruj pulę](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

![Kliknij przycisk Dodaj do puli](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)

![Wybierz bazy danych do dodania](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

![Dodawanie puli oczekujące](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

![Klikanie pozycji Zapisz.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

### <a name="move-a-database-out-of-an-elastic-pool"></a>Przenoszenie bazy danych z puli elastycznej

![Wyświetlanie listy baz danych](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

### <a name="change-performance-settings-of-an-elastic-pool"></a>Zmień ustawienia wydajności puli elastycznej

Podczas monitorowania wykorzystania zasobów w puli elastycznej może stwierdzić, że pewnych zmian są potrzebne. Może być puli wymaga zmian w granicach wydajność lub pamięć masową. Prawdopodobnie chcesz zmienić ustawienia bazy danych w puli. Ustawienia puli można zmienić w dowolnym momencie można uzyskać równowagę wydajności i kosztów. Zobacz [kiedy należy puli elastycznej użyć?](sql-database-elastic-pool.md) Aby uzyskać więcej informacji.

Aby zmienić limitów jednostek Edtu i magazynu dla każdej puli i jednostek Edtu na bazę danych:

![Użycie zasobów w puli elastycznej](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

## <a name="manage-elastic-pools-and-databases-using-powershell"></a>Zarządzanie pule elastyczne i baz danych przy użyciu programu PowerShell

Tworzenie i zarządzanie nimi pule elastyczne bazy danych SQL przy użyciu programu Azure PowerShell, użyj następujących poleceń cmdlet programu PowerShell. Jeśli musisz zainstalować lub uaktualnić programu PowerShell, zobacz [modułu instalacji programu Azure PowerShell](/powershell/azure/install-azurerm-ps). Aby utworzyć i zarządzać baz danych, serwerów i reguły zapory, zobacz [Utwórz i Zarządzaj serwerami bazy danych SQL Azure i baz danych przy użyciu programu PowerShell](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell). 

> [!TIP]
> Dla programu PowerShell przykładowe skrypty, zobacz [tworzenie elastycznych pul i przenoszenia baz danych między pulami i z puli przy użyciu programu PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) i [Użyj programu PowerShell, aby monitorować i skalowania puli elastycznej SQL w bazie danych SQL Azure](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Polecenie cmdlet | Opis |
| --- | --- |
|[Nowe AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Tworzy elastycznej puli baz danych na serwerze logicznym SQL.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Pobiera pule elastyczne i ich wartości właściwości na serwerze logicznym SQL.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Modyfikuje właściwości elastycznej puli baz danych na serwerze logicznym SQL. Na przykład użyć **StorageMB** właściwości, aby zmodyfikować max magazynu elastycznej puli.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Usuwa elastycznej puli baz danych na serwerze logicznym SQL.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Pobiera stan operacji dla puli elastycznej na serwerze logicznym SQL.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Tworzy nową bazę danych w istniejącej puli lub jako pojedynczej bazy danych. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Pobiera jeden lub więcej baz danych.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Ustawia właściwości dla bazy danych lub istniejącą bazę danych są przenoszone do z i między pule elastyczne.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Usuwa z bazy danych.|


> [!TIP]
> Tworzenie wielu baz danych w puli elastycznej może trwać, jeśli odbywa się za pomocą portalu lub poleceń cmdlet programu PowerShell, który tworzenie tylko jednej bazy danych w czasie. Aby zautomatyzować tworzenie w puli elastycznej, zobacz [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="manage-elastic-pools-and-databases-using-the-azure-cli"></a>Zarządzanie pule elastyczne i baz danych przy użyciu wiersza polecenia platformy Azure

Tworzenie i zarządzanie nimi pule elastyczne bazy danych SQL z [interfejsu wiersza polecenia Azure](/cli/azure/overview), należy użyć następującego [bazy danych SQL interfejsu wiersza polecenia Azure](/cli/azure/sql/db) poleceń. Używaj usługi [Cloud Shell](/azure/cloud-shell/overview), aby uruchamiać interfejs wiersza polecenia w przeglądarce, albo [zainstaluj](/cli/azure/install-azure-cli) go w systemie macOS, Linux lub Windows. 

> [!TIP]
> Dla interfejsu wiersza polecenia Azure przykładowe skrypty, zobacz [Użyj interfejsu wiersza polecenia, aby przenieść bazę danych Azure SQL w puli elastycznej SQL](scripts/sql-database-move-database-between-pools-cli.md) i [Użyj wiersza polecenia platformy Azure do skalowania puli elastycznej SQL w bazie danych SQL Azure](scripts/sql-database-scale-pool-cli.md).
>

| Polecenie cmdlet | Opis |
| --- | --- |
|[Tworzenie puli elastyczna az sql](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create)|Tworzy puli elastycznej.|
|[Lista elastyczna pula sql az](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list)|Zwraca listę pul elastycznych na serwerze.|
|[AZ pula elastyczna listy-baz danych sql](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_dbs)|Zwraca listę baz danych w puli elastycznej.|
|[Elastyczna pula sql az listy — wersje](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_editions)|Również obejmuje ustawienia jednostek dtu w warstwie dostępnej puli, limity magazynu, a dla ustawienia bazy danych. Aby zmniejszyć poziom szczegółowości, limity dodatkowego miejsca do magazynowania i na bazę danych ustawienia są domyślnie ukryte.|
|[Aktualizacja elastyczna pula sql az](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update)|Aktualizuje puli elastycznej.|
|[Usuń elastyczna pula sql az](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_delete)|Usuwa puli elastycznej.|

## <a name="manage-databases-within-elastic-pools-using-transact-sql"></a>Zarządzanie bazami danych w puli elastycznej za pomocą języka Transact-SQL

Aby utworzyć i przenoszenia baz danych w istniejącej puli elastycznej lub do zwracania informacji dotyczących puli elastycznej bazy danych SQL z Transact-SQL, użyj następujących poleceń T-SQL. Te polecenia przy użyciu portalu Azure może wystawiać [programu SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), lub inny program, który może nawiązywać połączenia z serwerem bazy danych SQL Azure i przekazać języka Transact-SQL polecenia. Aby utworzyć i zarządzać baz danych, serwerów i reguły zapory, zobacz [Utwórz i Zarządzaj serwerami bazy danych SQL Azure i baz danych przy użyciu języka Transact-SQL](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-transact-sql).

> [!IMPORTANT]
> Nie można tworzyć, aktualizować lub usuwać puli elastycznej bazy danych SQL Azure przy użyciu języka Transact-SQL. Można dodać lub usunąć z puli elastycznej bazy danych i widoków DMV służy do zwracania informacji dotyczących istniejących pul elastycznych.
>

| Polecenie | Opis |
| --- | --- |
|[Utwórz bazę danych (baza danych Azure SQL)](/sql/t-sql/statements/create-database-azure-sql-database)|Tworzy nową bazę danych w istniejącej puli lub jako pojedynczej bazy danych. Musisz mieć połączenie z główną bazą danych, aby utworzyć nową bazę danych.|
| [ALTER DATABASE (baza danych Azure SQL)](/sql/t-sql/statements/alter-database-azure-sql-database) |Przenoszenie bazy danych do z lub między elastyczne pule.|
|[Usuwanie bazy danych (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Usuwa z bazy danych.|
|[sys.elastic_pool_resource_stats (baza danych SQL Azure)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Zwraca statystyki użycia zasobów dla wszystkich pul elastycznych baz danych serwera logicznego. Dla każdej puli elastycznej bazy danych jest jeden wiersz dla każdej sekundzie 15 raportowania okna (cztery wiersze na minutę). W tym procesora CPU, we/wy, dziennika, użycia magazynu i użycie równoczesnych żądań/sesji przez wszystkie bazy danych w puli.|
|[sys.database_service_objectives (baza danych SQL Azure)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Zwraca edition (warstwy usług), celem usługi (warstwa cenowa) i nazwę puli elastycznej dla bazy danych Azure SQL lub usługi Azure SQL Data Warehouse. Jeśli zalogowany do głównej bazy danych na serwerze bazy danych SQL Azure, zwraca informacje o wszystkich baz danych. Dla usługi Azure SQL Data Warehouse musi być podłączony do bazy danych master.|

## <a name="manage-elastic-pools-and-databases-using-the-rest-api"></a>Zarządzanie pule elastyczne i baz danych przy użyciu interfejsu API REST

Tworzenie i zarządzanie nimi elastyczna baza danych SQL pule Użyj te żądania interfejsu API REST.

| Polecenie | Opis |
| --- | --- |
|[Pule elastyczne — tworzenie lub aktualizowanie](/rest/api/sql/elasticpools/createorupdate)|Tworzy nową pulę elastyczną lub aktualizuje istniejącej puli elastycznej.|
|[Pule elastyczne - Delete](/rest/api/sql/elasticpools/delete)|Usuwa puli elastycznej.|
|[Pule elastyczne - Get](/rest/api/sql/elasticpools/get)|Pobiera puli elastycznej.|
|[Pule elastyczne — lista przez serwer](/rest/api/sql/elasticpools/listbyserver)|Zwraca listę pul elastycznych na serwerze.|
|[Pule elastyczne - aktualizacji](/rest/api/sql/elasticpools/update)|Aktualizuje istniejącej puli elastycznej.|
|[Zalecana liczba elastycznych pul - Get](/rest/api/sql/recommendedelasticpools/get)|Pobiera recommented puli elastycznej.|
|[Zalecana liczba elastycznych pul — lista przez serwer](/rest/api/sql/recommendedelasticpools/listbyserver)|Zwraca zalecana liczba elastycznych pul.|
|[Zalecana liczba elastycznych pul - metryki listy](/rest/api/sql/recommendedelasticpools/listmetrics)|Zwraca recommented metryki puli elastycznej.|
|[Działania puli elastycznej](/rest/api/sql/elasticpoolactivities)|Zwraca działania puli elastycznej.|
|[Działania bazy danych w puli elastycznej](/rest/api/sql/elasticpooldatabaseactivities)|Zwraca działania bazy danych, w ramach puli elastycznej.|
|[Bazy danych — Tworzenie lub aktualizowanie](/rest/api/sql/databases/createorupdate)|Tworzy nową bazę danych lub aktualizuje istniejącą bazę danych.|
|[Bazy danych - Get](/rest/api/sql/databases/get)|Pobiera bazy danych.|
|[Bazy danych — uzyskać za pomocą puli elastycznej](/rest/api/sql/databases/getbyelasticpool)|Pobiera bazy danych w puli elastycznej.|
|[Bazy danych — uzyskać za pomocą zalecana liczba elastycznych pul](/rest/api/sql/databases/getbyrecommendedelasticpool)|Pobiera bazy danych wewnątrz recommented puli elastycznej.|
|[Bazy danych — lista według puli elastycznej](/rest/api/sql/databases/listbyelasticpool)|Zwraca listę baz danych w puli elastycznej.|
|[Bazy danych — lista według zalecana liczba elastycznych pul](/rest/api/sql/databases/listbyrecommendedelasticpool)|Zwraca listę baz danych wewnątrz recommented puli elastycznej.|
|[Bazy danych — lista przez serwer](/rest/api/sql/databases/listbyserver)|Zwraca listę baz danych na serwerze.|
|[Bazy danych — aktualizacja](/rest/api/sql/databases/update)|Aktualizuje istniejącą bazę danych.|

## <a name="next-steps"></a>Następne kroki

* Aby obejrzeć wideo, zobacz [Microsoft Virtual Academy kursu wideo na możliwości elastycznej bazy danych SQL Azure](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* Aby dowiedzieć się więcej na temat wzorców projektowych dla aplikacji SaaS wykorzystujących pule elastyczne, zobacz artykuł [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) (Wzorce projektowe dla wielodostępnych aplikacji SaaS korzystających z usługi Azure SQL Database).
* Samouczek SaaS wykorzystujących pule elastyczne, zobacz [wprowadzenie do aplikacji Wingtip SaaS](sql-database-wtp-overview.md).
