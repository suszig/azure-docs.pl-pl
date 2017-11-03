---
title: "Poziomy spójności w usłudze Azure DB rozwiązania Cosmos | Dokumentacja firmy Microsoft"
description: "Azure DB rozwiązania Cosmos ma pięć poziomy spójności, aby ułatwić równoważenie ostatecznego spójności, dostępnością i opóźnieniem kompromis."
keywords: "spójność ostateczna azure rozwiązania cosmos db, azure, programu Microsoft azure"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: cgronlun
documentationcenter: 
ms.assetid: 3fe51cfa-a889-4a4a-b320-16bf871fe74c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1ebec2285982c70aa9dc49950769fe18e2e2d0d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="tunable-data-consistency-levels-in-azure-cosmos-db"></a>Poziomy spójności danych dostosowywalne w usłudze Azure DB rozwiązania Cosmos
Azure DB rozwiązania Cosmos jest zaprojektowany od podstaw się z globalnego dystrybucji pamiętać dla każdego modelu danych. Jest on przeznaczony do oferują przewidywalną małych opóźnieniach gwarancje, SLA dostępności 99,99% i wielu modeli dobrze zdefiniowany swobodna spójności. Obecnie bazy danych Azure rozwiązania Cosmos zawiera pięć poziomów spójności: silne, nieaktualność, sesji, prefiks spójne i "ostateczna". 

Oprócz **silne** i **spójność ostateczna** modeli często oferowane przez rozproszonych baz danych, bazy danych rozwiązania Cosmos Azure udostępnia trzy modele spójności starannie wersja i operationalized więcej i zweryfikowała ich przydatności przed rzeczywistych przypadków użycia. Są to **ograniczone nieaktualności**, **sesji**, i **spójne prefiks** poziomy spójności. Zbiorczo te poziomy spójności pięć umożliwiają należy dobrze uzasadnione kompromis między spójności, dostępnością i opóźnieniem. 

## <a name="distributed-databases-and-consistency"></a>Rozproszone baz danych i spójności
Komercyjnie rozpowszechniane bazy danych można podzielić na dwie kategorie: bazy danych, które nie oferują żadnych dobrze zdefiniowanych, sprawdzalnych opcji spójności, i bazy danych, które oferują dwie skrajne opcje programowania (spójność silna lub ostateczna). 

Pierwsze zmuszają deweloperów aplikacji do zajmowania się drobnymi szczegółami protokołów replikacji i wymagają dokonywania trudnych wyborów między spójnością, dostępnością, opóźnieniem i przepływnością. Drugie kładą nacisk na konieczność wyboru między dwoma skrajnościami. Pomimo mnogości badań i ponad 50 propozycji modeli spójności społeczność związana z rozpowszechnianymi bazami danych nie była w stanie skomercjalizować poziomów spójności poza spójnością silną i ostateczną. Rozwiązania cosmos DB umożliwia deweloperom wybór między pięć modeli dobrze zdefiniowany spójności wzdłuż spektrum spójności — siły, spójność powiązanej nieaktualności [sesji](http://dl.acm.org/citation.cfm?id=383631), prefiks spójne i "ostateczna". 

![Usługa Azure Cosmos DB oferuje wiele dobrze zdefiniowanych (swobodniejszych) modeli spójności do wyboru](./media/consistency-levels/five-consistency-levels.png)

W poniższej tabeli przedstawiono poszczególne gwarancje oferowane przez każdy poziom spójności.
 
**Poziomy spójności i gwarancje**

| Poziom spójności | Gwarancje |
| --- | --- |
| Silna | Operacje atomowe |
| Powiązana nieaktualność | Spójny prefiks. Odczyty opóźnione w stosunku do zapisów o k prefiksów lub interwał równy t |
| Sesja   | Spójny prefiks. Monotoniczne odczyty, monotoniczne zapisy, odczytywanie swoich zapisów, zapisy następują po odczytach |
| Spójny prefiks | Zwracane aktualizacje to pewne prefiksy ze wszystkich aktualizacji, bez przerw |
| Ostateczna  | Odczyty poza kolejnością |

Na koncie usługi Cosmos DB można skonfigurować domyślny poziom spójności (i później przesłonić spójność dla określonego żądania odczytu). Wewnętrznie domyślny poziom spójności ma zastosowanie do danych w zestawach partycji, które może obejmować regionów. Spójność sesji przy użyciu około 73% naszych dzierżawcy, a spójność powiązanej nieaktualności preferowane jest 20%. Firma Microsoft obserwować, że około 3% klientów wypróbować różne poziomy spójności początkowo przed rozpoczęciem na wybór spójności określonych aplikacji. Możemy również obserwować, czy tylko 2% naszych dzierżaw zastąpienie poziomy spójności na podstawie danego żądania. 

W DB rozwiązania Cosmos Odczyty podawane w sesji, prefiks spójne i spójność ostateczna dwukrotnie są jako tanie jako odczyty z nieaktualności silne lub ograniczonych. Rozwiązania cosmos bazy danych ma branży kompleksowe SLA 99,99%, w tym gwarancje spójności oraz dostępność, przepustowości i opóźnień. Zastosujemy [sprawdzania linearizability](http://dl.acm.org/citation.cfm?id=1806634), które działa nieprzerwanie przez naszych telemetrii usługi i jawnie raporty naruszenie spójności dla Ciebie. Dla spójność powiązanej nieaktualności możemy monitorowania i Zgłoś naruszenie do granic k i t. Dla wszystkich pięciu poziomów spójności swobodna, będziemy również zgłosić [Metryka prawdopodobieństwa spójność powiązanej nieaktualności](http://dl.acm.org/citation.cfm?id=2212359) bezpośrednio do użytkownika.  

## <a name="scope-of-consistency"></a>Zakres spójności
Poziom szczegółowości spójności obejmuje żądanie jednego użytkownika. Żądanie zapisu może odpowiadać insert, replace, upsert lub usunąć transakcji. Podobnie jak w przypadku operacji zapisu, transakcji odczytu/zapytania ma również zakres na żądanie użytkownika. Użytkownik może być konieczna w dużej z podziałem na strony zestawu wyników, obejmujących wiele partycji, ale każda odczytać transakcji jest ograniczone do jednej strony i udostępniane przez w ramach jednej partycji.

## <a name="consistency-levels"></a>Poziomy spójności
Domyślny poziom spójności można skonfigurować na Twoim koncie bazy danych, która ma zastosowanie do wszystkich kolekcji (i baz danych) na koncie DB rozwiązania Cosmos. Domyślnie wszystkie odczyty i zapytań wystawiony na podstawie zasoby zdefiniowane przez użytkownika będą używać domyślnego poziomu spójności określonego na konto bazy danych. Może osłabić poziomu spójności odczytu/zapytania określonego żądania, używając w każdej z obsługiwanych interfejsów API. Istnieje pięć typów poziomy spójności obsługiwane przez protokół replikacji bazy danych Azure rozwiązania Cosmos zapewniające wyczyść zależność między spójności określone gwarancje i wydajności, zgodnie z opisem w tej sekcji.

**Silne**: 

* Zapewnia wysoki poziom spójności [linearizability](https://aphyr.com/posts/313-strong-consistency-models) gwarancja odczyty gwarancji, aby powrócić do najnowszej wersji elementu. 
* Silna spójność gwarantuje, że zapis jest widoczny dopiero w po jest trwałym zatwierdzeniu przez większość kworum replik. Zapis jest albo synchronicznie trwałym zatwierdzeniu przez serwera podstawowego i kworum replik pomocniczych, lub jest zostało przerwane. Odczytu zawsze zostaje potwierdzony głosów kworum do odczytu, klient nigdy nie widzą zapisu niezatwierdzone lub jej część jest zawsze gwarantowane można odczytać najnowszych potwierdzonego zapisu. 
* Azure DB rozwiązania Cosmos kont, które są skonfigurowane do używania wysoki poziom spójności nie można skojarzyć więcej niż jeden region platformy Azure z użyciem konta bazy danych Azure rozwiązania Cosmos. 
* Koszt operacja odczytu (w postaci liczby [jednostek żądania](request-units.md) używane) z wysoki poziom spójności jest wyższy niż sesji i "ostateczna", ale taka sama jak spójność powiązanej nieaktualności.

**Ograniczone nieaktualności**: 

* Ograniczone nieaktualności gwarantuje spójności odczytami może opóźniona zapisy, przez co najwyżej *K* wersji lub prefiksy elementu lub *t* interwał czasu. 
* W związku z tym podczas wybierania ograniczone nieaktualności, "nieaktualności" można skonfigurować na dwa sposoby: numer wersji *K* elementu za pomocą którego odczytami opóźniona zapisami i przedział czasu *t* 
* Ograniczone nieaktualności oferty globalne zamówienia z wyjątkiem "okna nieaktualności." Istnieje monotoniczna gwarancje odczytu w obrębie regionu zarówno wewnątrz i na zewnątrz "nieaktualności okno." 
* Spójność powiązanej nieaktualności zapewnia gwarancję spójności silniejsze niż sesji lub spójność ostateczna. Dla aplikacji rozproszonych globalnie zaleca się, że używasz spójność powiązanej nieaktualności w scenariuszach, gdzie chcesz mieć wysoki poziom spójności, jednak dostępności 99,99% i małe opóźnienia. 
* Azure kont rozwiązania Cosmos bazy danych, które są skonfigurowane przy użyciu spójność powiązanej nieaktualności można skojarzyć dowolną liczbę regiony platformy Azure z użyciem konta bazy danych Azure rozwiązania Cosmos. 
* Koszt operacja odczytu (pod względem używane RUs) z spójność powiązanej nieaktualności jest wyższy niż sesji i spójność ostateczna, ale taka sama jak wysoki poziom spójności.

**Sesja**: 

* W odróżnieniu od oferowanych przez poziomy spójności nieaktualności silne i ograniczonego modeli globalne spójności spójność sesji obejmuje sesji klienta. 
* Spójność sesji jest idealny dla wszystkich scenariuszy, w którym sesję użytkownika lub urządzenia uczestniczy, ponieważ gwarantuje monotoniczna odczyty, monotoniczna zapisu i odczytu gwarancje własne zapisów (RYW). 
* Spójność sesji zapewnia przewidywalną spójność sesji i maksymalnie odczytać przepływności jednocześnie zapewniając najmniejsza opóźnienia zapisów i odczytów. 
* Azure kont rozwiązania Cosmos bazy danych, które są skonfigurowane przy użyciu spójność sesji można skojarzyć dowolną liczbę regiony platformy Azure z użyciem konta bazy danych Azure rozwiązania Cosmos. 
* Koszt operacja odczytu (pod względem używane RUs) z poziomu spójności sesji jest mniejsze niż silne i ograniczonego nieaktualności, ale spójność ostateczna więcej niż

<a id="consistent-prefix"></a>
**Prefiks spójne**: 

* Prefiks spójne gwarantuje, że w przypadku braku kolejnych zapisów repliki w grupie ostatecznie zbieżność. 
* Prefiks spójne gwarantuje, że odczyty nigdy nie zobaczyć zapisy poza kolejnością. Jeśli wykonano operacji zapisu w kolejności `A, B, C`, klient widzi albo `A`, `A,B`, lub `A,B,C`, ale nigdy nie poza kolejnością jak `A,C` lub `B,A,C`.
* Azure kont rozwiązania Cosmos bazy danych, które są skonfigurowane przy użyciu prefiksu spójne spójności można skojarzyć dowolną liczbę regiony platformy Azure z użyciem konta bazy danych Azure rozwiązania Cosmos. 

**Ewentualne**: 

* Spójność ostateczna gwarantuje, że w przypadku braku kolejnych zapisów repliki w grupie ostatecznie zbieżność. 
* Spójność ostateczna jest postaci najsłabszą spójność, których klient może pobrać wartości, które są starsze niż te, które miały zauważony.
* Spójność ostateczna zapewnia najsłabszą spójność odczytów, ale dzięki uzyskać najmniejsze opóźnienia zarówno odczytów i zapisów.
* Azure kont rozwiązania Cosmos bazy danych, które są skonfigurowane przy użyciu spójność ostateczna można skojarzyć dowolną liczbę regiony platformy Azure z użyciem konta bazy danych Azure rozwiązania Cosmos. 
* Koszt operacja odczytu (pod względem używane RUs) z spójność ostateczna poziom jest najniższa wszystkie poziomy spójności bazy danych Azure rozwiązania Cosmos.

## <a name="configuring-the-default-consistency-level"></a>Konfigurowanie poziomu spójności domyślne
1. W [portalu Azure](https://portal.azure.com/), na pasku przechodzenia kliknij **bazy danych Azure rozwiązania Cosmos**.
2. W **bazy danych Azure rozwiązania Cosmos** bloku, wybierz konto bazy danych do zmodyfikowania.
3. W bloku konta kliknij **domyślna spójność**.
4. W **domyślna spójność** bloku, wybierz nowy poziom spójności i kliknij przycisk **zapisać**.
   
    ![Zrzut ekranu: wyróżnianie ikonę ustawień i spójności domyślny wpis](./media/consistency-levels/database-consistency-level-1.png)

## <a name="consistency-levels-for-queries"></a>Poziomy spójności dla zapytań
Domyślnie dla użytkownika zasobów poziomu spójności dla zapytań jest taka sama jak poziomu spójności dla odczytów. Domyślnie indeks jest aktualizowana synchronicznie na każdym insert, Zamień lub usuń elementu do kontenera DB rozwiązania Cosmos. Dzięki temu zapytania uwzględnić poziomu spójności co odczyty punktu. Podczas zapisu zoptymalizowany i obsługuje stałej ilości zapisy, indeks synchroniczne konserwacji i obsługująca spójne zapytania bazy danych Azure rozwiązania Cosmos można skonfigurować pewne kolekcji, aby zaktualizować ich indeksu w trybie opóźnienia. Dodatkowo indeksowanie z opóźnieniem zwiększa wydajność zapisu i jest idealne dla scenariuszy wprowadzanie zbiorczego obciążenia jest głównie ciężki odczytu.  

| Tryb indeksowania | Odczytuje | Zapytania |
| --- | --- | --- |
| Spójność (ustawienie domyślne) |Wybierz jedną z nieaktualności silne, ograniczone, sesji, spójne prefiksu lub ostatecznego |Wybierz jedną z nieaktualności silne, ograniczone, sesji lub ostatecznego |
| Powolne |Wybierz jedną z nieaktualności silne, ograniczone, sesji, spójne prefiksu lub ostatecznego |Ostateczna |
| Brak |Wybierz jedną z nieaktualności silne, ograniczone, sesji, spójne prefiksu lub ostatecznego |Nie dotyczy |

Jako z żądaniami odczytu, możesz obniżyć poziom spójności żądania określonej kwerendy w każdym interfejsu API.

## <a name="next-steps"></a>Następne kroki
Jeśli chcesz zrobić więcej informacje o poziomy spójności i wady i zalety, zaleca się następujące zasoby:

* Jakub Dougowi. Wyjaśniono spójności replikowanych danych za pośrednictwem baseball (klip wideo).   
  [https://www.youtube.com/Watch?v=gluIh8zd26I](https://www.youtube.com/watch?v=gluIh8zd26I)
* Jakub Dougowi. Wyjaśniono spójności replikowanych danych za pośrednictwem baseball.   
  [http://Research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.PDF](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
* Jakub Dougowi. Gwarancje sesji słabo spójności replikowanych danych.   
  [http://DL.ACM.org/CITATION.cfm?ID=383631](http://dl.acm.org/citation.cfm?id=383631)
* Abadi Danielowi. Wady i zalety spójności w projekcie: nowoczesnych systemów bazy danych dystrybucji: zakończenie jest tylko część wątku ".   
  [http://Computer.org/CSDL/mags/co/2012/02/mco2012020037-ABS.HTML](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html)
* Peterowi Bailis, Shivaram Venkataraman, Michael J. tw, Joseph M. Hellerstein, Stoica zakres przechowywania. Prawdopodobieństwa ograniczone nieaktualności (PBS) praktyczne częściowe kworum.   
  [http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.PDF](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
* Werner Vogels. Ostateczna spójne — uruchomić ponownie.    
  [http://allthingsdistributed.com/2008/12/eventually_consistent.HTML](http://allthingsdistributed.com/2008/12/eventually_consistent.html)
* Moni Naor, wełna Avishai, obciążenia, pojemności i dostępność systemów kworum, SIAM Dziennik przetwarzania danych, v.27 n.2, 447 p.423, kwietnia 1998.
  [http://epubs.siam.org/DOI/ABS/10.1137/S0097539795281232](http://epubs.siam.org/doi/abs/10.1137/S0097539795281232)
* Burckhardt Sebastianowi, Krzysztof Dern, Macanal Musuvathi, Tan Royowi, każdy: linearizability pełny i automatyczne sprawdzanie, postępowania 2010 konferencji ACM SIGPLAN programowania języka projekt i implementację, czerwca 05 10 2010 naszej, terenem Stanów Zjednoczonych [doi > 10.1145/1806596.1806634] [http://dl.acm.org/citation.cfm?id=1806634](http://dl.acm.org/citation.cfm?id=1806634)
* Peterowi Bailis, Shivaram Venkataraman, Michael J. tw, Joseph M. Hellerstein, Stoica zakres przechowywania Probabilistically ograniczone nieaktualności do praktycznych kworum częściowe postępowania wydzielony VLDB, v.5 n.8, 787 p.776, kwietnia 2012 [http://dl.acm.org/citation.cfm?id=2212359](http://dl.acm.org/citation.cfm?id=2212359)
