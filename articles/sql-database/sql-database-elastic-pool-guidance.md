---
title: "Kiedy należy użyć elastycznej puli baz danych?"
description: "Elastyczna pula baz danych to zbiór dostępnych zasobów, które są współużytkowane przez grupę elastycznych baz danych. Ten dokument zawiera wskazówki ułatwiające ocenę przydatności korzystania z elastycznej puli baz danych dla grupy baz danych."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 3d3941d5-276c-4fd2-9cc1-9fe8b1e4c96c
ms.service: sql-database
ms.custom: sharded databases pool; app development
ms.devlang: NA
ms.date: 08/08/2016
ms.author: sstein
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 867f06c1fae3715ab03ae4a3ff4ec381603e32f7
ms.openlocfilehash: 408cf315f8b44c9ebf852c3ccbf2ed93c70ef22f


---
# <a name="when-should-an-elastic-database-pool-be-used"></a>Kiedy należy użyć elastycznej puli baz danych?
Oceń, czy korzystanie z elastycznej puli baz danych jest opłacalne, na podstawie wzorców użycia baz danych i różnic między cenami elastycznej puli baz danych i pojedynczych baz danych. Udostępniane są również dodatkowe wytyczne umożliwiające określenie bieżącego rozmiaru puli wymaganego dla istniejącego zestawu baz danych SQL.  

* Omówienie pul zawiera artykuł [SQL Database elastic database pools](sql-database-elastic-pool.md) (Elastyczne pule baz danych usługi SQL Database).

> [!NOTE]
> Pule elastyczne są ogólnodostępne we wszystkich regionach platformy Azure oprócz Indii Zachodnich, gdzie są obecnie dostępne w wersji zapoznawczej.  Pule elastyczne zostaną udostępnione ogólnie w tych regionach tak szybko, jak to możliwe.
> 
> 

## <a name="elastic-database-pools"></a>Elastyczne pule baz danych
Deweloperzy SaaS tworzą aplikacje w oparciu o warstwy danych w dużej skali składające się z wielu baz danych. Typowym wzorcem aplikacji jest udostępnianie jednej bazy danych dla każdego klienta. Jednak różni klienci często mają różne i nieprzewidywalne wzorce użycia i trudno przewidzieć wymagania dotyczące zasobów dla każdego indywidualnego użytkownika bazy danych. W związku z tym deweloperzy mogą nadmiarowo alokować zasoby, co wiąże się ze znaczącymi kosztami, aby zapewnić preferowaną przepływność i czasy alokacji dla wszystkich baz danych. Ewentualnie deweloper może wydać mniej i zaryzykować pogorszeniem wydajności obsługi dla swoich klientów. Aby dowiedzieć się więcej na temat wzorców projektowych dla aplikacji SaaS wykorzystujących pule elastyczne, zobacz artykuł [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) (Wzorce projektowe dla wielodostępnych aplikacji SaaS korzystających z usługi Azure SQL Database).

Pule elastyczne pule w usłudze Azure SQL Database umożliwiają deweloperom SaaS optymalizację stosunku ceny do wydajności dla grupy baz danych w ramach określonego budżetu, zapewniając jednocześnie elastyczność wydajności dla każdej bazy danych. Pule umożliwiają deweloperowi zakupienie liczby jednostek eDTU (elastic Database Transaction Unit) dla puli współdzielonej przez wiele baz danych, aby obsługiwać nieprzewidywalne okresy użycia pojedynczych bazy danych. Wymagania dotyczące liczby jednostek eDTU dla puli są określane na podstawie zagregowanego użycia jej baz danych. Dostępna liczba jednostek eDTU dla puli jest kontrolowana przez budżet dewelopera. Pule ułatwiają deweloperom rozsądne kontrolowanie wpływu budżetu na wydajność i odwrotnie dla ich puli. Deweloper po prostu dodaje bazy danych do puli, ustawia minimalną i maksymalną liczbę eDTU dla baz danych, a następnie ustawia liczbę jednostek eDTU puli na podstawie swojego budżetu. Korzystając z pul, deweloper może bezproblemowo rozwijać swoją usługę od niewielkiego startupu do dojrzałego biznesu w coraz większej skali.  

## <a name="when-to-consider-a-pool"></a>Kiedy należy rozważyć korzystanie z puli
Pule są odpowiednie dla wielu baz danych o określonych wzorcach użycia. Dla danej bazy danych ten wzorzec charakteryzuje się niskim średnim wykorzystaniem oraz stosunkowo rzadkimi okresami zwiększonego użycia.

Im więcej baz danych możesz dodać do puli, tym większe uzyskujesz oszczędności. W zależności od wzorca użycia aplikacji możliwe jest uzyskanie oszczędności nawet już przy dwóch bazach danych S3.  

Poniższe sekcje ułatwiają zrozumienie, jak ocenić, czy dodanie konkretnej kolekcji baz danych do puli będzie korzystne. W przykładach użyto pul warstwy Standardowa, ale te same zasady mają zastosowanie także do pul w warstwach Podstawowa i Premium.

### <a name="assessing-database-utilization-patterns"></a>Ocena wzorców użycia bazy danych
Na poniższej ilustracji pokazano przykład bazy danych, która przez większość czasu pozostaje bezczynna, ale okresowo doświadcza dużego wzrostu aktywności. Jest to wzorzec użycia, który jest odpowiedni dla puli:

   ![pojedyncza baza danych odpowiednia dla puli](./media/sql-database-elastic-pool-guidance/one-database.png)

W okresie pięciu minut przedstawionym powyżej baza danych DB1 odnotowuje wzrost użycia do 90 DTU, ale jej całkowite średnie użycie jest mniejsze niż pięć DTU. Do obsługi tego obciążenia w jednej bazie danych wymagany jest poziom wydajności S3, ale spowoduje to, że większość zasobów pozostanie nieużywana w okresach niskiej aktywności.

Pula umożliwia udostępnienie tych nieużywanych jednostek DTU wielu bazom danych, a tym samym zmniejsza całkowitą liczbę potrzebnych jednostek DTU i obniża koszty ogólne.

Rozwijając poprzedni przykład, załóżmy, że istnieją dodatkowe bazy danych o podobnych wzorcach użycia jak baza danych DB1. Na następnych dwóch ilustracjach poniżej użycie czterech i dwudziestu baz danych przedstawiono nałożone na tym samym wykresie, aby zilustrować nienakładający się charakter ich użycia w czasie:

   ![cztery bazy danych z wzorcem użycia odpowiednim dla puli](./media/sql-database-elastic-pool-guidance/four-databases.png)

   ![dwadzieścia baz danych z wzorcem użycia odpowiednim dla puli](./media/sql-database-elastic-pool-guidance/twenty-databases.png)

Zagregowane użycie jednostek DTU we wszystkich 20 bazach danych przedstawia czarna linia na rysunku powyżej. Pokazuje ona, że zagregowane użycie jednostek DTU nigdy nie przekracza 100 DTU i wskazuje, że te 20 baz danych może współużytkować 100 jednostek eDTU w tym okresie. Skutkuje to dwudziestokrotnym zmniejszeniem liczby DTU i trzynastokrotnie niższą ceną w porównaniu do umieszczenia każdej z baz danych na poziomie wydajności S3 dla pojedynczej bazy danych.

Ten przykład jest idealny z następujących przyczyn:

* Istnieją duże różnice między użyciem szczytowym a średnim użyciem na bazę danych.  
* Użycie szczytowe dla poszczególnych baz danych występuje w różnych punktach w czasie.
* Jednostki eDTU są współużytkowane przez dużą liczbę baz danych.

Cena puli jest funkcją jednostek eDTU puli. Chociaż cena jednostki eDTU dla puli jest 1,5 raza większa niż cena jednostki DTU dla pojedynczej bazy danych, **jednostki eDTU puli mogą być współużytkowane przez wiele baz danych, a tym samym w wielu przypadkach potrzebna jest mniejsza całkowita liczba jednostek eDTU**. Te różnice w cenie i współużytkowanie jednostek eDTU są podstawą potencjalnych oszczędności, które mogą zapewnić pule.  

Następujące reguły akceptacji powiązane z liczbą baz danych i użyciem baz danych pomagają upewnić się, że pula zapewnia mniejsze koszty w porównaniu z użyciem poziomów wydajności dla pojedynczych baz danych.

### <a name="minimum-number-of-databases"></a>Minimalna liczba baz danych
Jeśli suma liczby jednostek DTU poziomów wydajności dla pojedynczych baz danych jest większa niż 1,5 x liczba eDTU potrzebnych dla puli, pula elastyczna jest bardziej opłacalna. Aby sprawdzić dostępne rozmiary, zobacz [limity liczby jednostek eDTU i magazynu dla elastycznych pul i elastycznych baz danych](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases).

***Przykład***<br>
Aby pula 100 eDTU była bardziej opłacalna niż korzystanie z poziomów wydajności dla pojedynczych baz danych, potrzebne są co najmniej dwie baz danych S3 lub co najmniej 15 baz danych S0.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Maksymalna liczba baz danych z równoczesnymi szczytami użycia
W przypadku współużytkowania jednostek eDTU nie wszystkie bazy danych w puli mogą jednocześnie używać eDTU do wysokości limitu dostępnego podczas używania poziomów wydajności dla pojedynczych baz danych. Im mniejsza liczba baz danych, które jednocześnie odnotowują szczytowe użycie, tym mniejszą liczbę jednostek eDTU można ustawić dla puli i tym bardziej opłacalne staje się korzystanie z puli. Ogólnie rzecz biorąc, nie więcej niż 2/3 (lub 67%) baz danych w puli powinno jednocześnie doświadczać wzrostu aktywności do limitu liczby jednostek eDTU.

***Przykład***<br>
Aby zmniejszyć koszty dla trzech baz danych S3 w puli 200 eDTU, co najwyżej dwie z tych baz danych mogą jednocześnie osiągać szczytowe użycie.  W przeciwnym razie, jeśli więcej niż dwie z tych czterech baz danych S3 jednocześnie osiągają szczytowe użycie, rozmiar puli musiałby zostać zwiększony do ponad 200 eDTU.  Z kolei jeśli rozmiar puli będzie większy niż 200 eDTU, konieczne będzie dodanie większej liczby baz danych S3, aby koszty puli pozostały niższe niż w przypadku poziomów wydajności dla pojedynczych baz danych.  

Należy zauważyć, że w tym przykładzie nie jest rozważane użycie innych baz danych w puli. Jeśli wszystkie bazy danych mają jakiś poziom użycia w dowolnym danym momencie czasu, wówczas mniej niż 2/3 (lub 67%) baz danych może jednocześnie osiągać szczytowe użycie.

### <a name="dtu-utilization-per-database"></a>Użycie jednostek DTU na bazę danych
Duża różnica między szczytowym i średnim użyciem bazy danych wskazuje na dłuższe okresy niewielkiego wykorzystania i krótkie okresy wysokiego wykorzystania. Ten wzorzec użycia jest idealny dla współużytkowania zasobów między bazami danych. Bazę danych należy rozważyć do umieszczenia w puli, jeśli jej szczytowe użycie jest ponad 1,5 raza większe niż jej średnie użycie.

***Przykład***<br>
Baza danych S3, która osiąga szczytowe użycie do 100 DTU, a przeciętnie używa 67 DTU lub mniej, jest dobrym kandydatem do współużytkowania jednostek eDTU w puli.  Również baza danych S1, która osiąga szczytowe użycie 20 DTU, a przeciętnie używa 13 DTU lub mniej, jest dobrym kandydatem do umieszczenia w puli.

## <a name="sizing-an-elastic-pool"></a>Ustawianie rozmiaru puli elastycznej
Najlepszy rozmiar puli zależy od zagregowanej liczby jednostek eDTU i zasobów magazynu wymaganych dla wszystkich baz danych w puli. Obejmuje to określenie największej z następujących wielkości:

* Maksymalna liczba jednostek DTU wykorzystana przez wszystkie bazy danych w puli.
* Maksymalna liczba bajtów magazynu wykorzystana przez wszystkie bazy danych w puli.

Aby sprawdzić dostępne rozmiary, zobacz [limity liczby jednostek eDTU i magazynu dla elastycznych pul i elastycznych baz danych](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases).

Usługa SQL Database automatycznie ocenia historyczne użycie zasobów przez bazy danych na istniejącym serwerze usługi SQL Database i zaleca odpowiednią konfigurację puli w witrynie Azure Portal. Oprócz zaleceń, wbudowane środowisko szacuje użycie jednostek eDTU dla niestandardowej grupy baz danych na serwerze. Umożliwia to przeprowadzenie analizy warunkowej przez interaktywne dodawanie baz danych do puli i usuwanie ich w celu uzyskania analizy użycia zasobów i porady dotyczącej rozmiaru przed zatwierdzeniem zmian. Aby uzyskać instrukcje, zobacz [Monitorowanie puli elastycznej, zmienianie jej rozmiaru i zarządzanie nią](sql-database-elastic-pool-manage-portal.md).

Aby uzyskać bardziej elastyczne oceny użycia zasobów umożliwiające szacowanie ad hoc rozmiaru dla serwerów starszych niż wersja V12, a także szacowanie rozmiaru dla baz danych na różnych serwerach, zobacz temat [Skrypt programu PowerShell do identyfikowania baz danych odpowiednich dla elastycznej puli baz danych](sql-database-elastic-pool-database-assessment-powershell.md).

| Możliwości | Środowisko portalu | Skrypt programu PowerShell |
|:--- |:--- |:--- |
| Poziom szczegółowości |15 sekund |15 sekund |
| Uwzględnia różnice cenowe między pulą a poziomami wydajności dla pojedynczych baz danych |Tak |Nie |
| Umożliwia dostosowywanie listy analizowanych baz danych |Tak |Tak |
| Umożliwia dostosowywanie czasu używanego do analizy |Nie |Tak |
| Umożliwia dostosowywanie listy analizowanych baz danych na różnych serwerach |Nie |Tak |
| Umożliwia dostosowywanie listy analizowanych baz danych na serwerach w wersji v11 |Nie |Tak |

W przypadkach, gdy nie można użyć narzędzi, skorzystanie z następujących instrukcji krok po kroku może ułatwić oszacowanie, czy pula jest bardziej opłacalna niż pojedyncza baza danych:

1. Oszacuj liczbę jednostek eDTU potrzebnych dla puli w następujący sposób:
   
   MAX(<*łączna liczba baz danych* X *średnie użycie jednostek DTU na bazę danych*>,<br>
   <*liczba baz danych jednocześnie osiągających szczyt użycia* X *użycie szczytowe jednostek DTU na bazę danych*)
2. Oszacuj miejsce do magazynowania wymagane dla puli przez dodanie liczby bajtów potrzebnych dla wszystkich baz danych w puli.  Następnie określ rozmiar puli (w jednostkach eDTU), który zapewni tę ilość miejsca przechowywania.  Aby uzyskać informacje o limitach magazynu puli na podstawie rozmiaru puli w jednostkach eDTU, zobacz [limity liczby jednostek eDTU i magazynu dla elastycznych pul i elastycznych baz danych](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases).
3. Weź większą z wartości szacunkowych eDTU z kroku 1 i kroku 2.
4. Wejdź na [stronę cennika usługi SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) i znajdź najmniejszy rozmiar puli jednostek eDTU większy niż szacunkowa wartość z kroku 3.
5. Porównaj cenę puli z kroku 5 z ceną za używanie odpowiednich poziomów wydajności dla pojedynczych baz danych.

## <a name="summary"></a>Podsumowanie
Nie wszystkie pojedyncze bazy danych są optymalnymi kandydatami dla pul. Świetnymi kandydatami są bazy danych, których wzorce użycia charakteryzują się niskim średnim wykorzystaniem i stosunkowo rzadkimi okresami zwiększonego użycia. Wzorce użycia aplikacji są dynamiczne, a więc informacje i narzędzia opisane w niniejszym artykule umożliwiają początkową ocenę, czy pula jest dobrym wyborem dla niektórych lub wszystkich baz danych. Ten artykuł jest jedynie punktem wyjścia ułatwiającym podjęcie decyzji odnośnie tego, czy pula elastyczna jest dobrym rozwiązaniem. Trzeba mieć na uwadze, że konieczne jest dalsze ciągłe monitorowanie historycznego użycia zasobów i stałe dokonywanie ponownej oceny poziomów wydajności wszystkich używanych baz danych. Należy pamiętać, że łatwo można przenosić bazy danych do i z elastycznych pul baz danych, a w przypadku dużej liczby baz danych można mieć wiele pul o różnych rozmiarach, między które można podzielić bazy danych.

## <a name="next-steps"></a>Następne kroki
* [Tworzenie elastycznej puli baz danych](sql-database-elastic-pool-create-portal.md)
* [Monitorowanie elastycznej puli baz danych, zmienianie jej rozmiaru i zarządzanie nią](sql-database-elastic-pool-manage-portal.md)
* [Opcje i wydajność usługi SQL Database: omówienie możliwości dostępnych w przypadku każdej warstwy usług](sql-database-service-tiers.md)
* [Skrypt programu PowerShell do identyfikowania baz danych odpowiednich dla elastycznej puli baz danych](sql-database-elastic-pool-database-assessment-powershell.md)




<!--HONumber=Dec16_HO1-->


