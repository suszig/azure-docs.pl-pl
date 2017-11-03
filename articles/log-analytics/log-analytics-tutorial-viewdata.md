---
title: "Wyświetlanie i analizowanie zebranych danych Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera samouczek, który opisuje sposób tworzenia dziennik wyszukiwania i analizowania danych przechowywanych w zasobie analizy dzienników przy użyciu portalu wyszukiwania dziennika.  Samouczek obejmuje uruchamiania niektórych zapytań proste być zwracanie różnych typów danych i analiza wyników."
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/26/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: dfcbb925a16ca1e53d10b7bf70d03e62bc9dae69
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="view-or-analyze-data-collected-with-log-analytics-log-search"></a>Wyświetlanie i analizowanie danych zbieranych z wyszukiwania dziennika analizy dzienników

W analizy dzienników można wykorzystać dziennik wyszukiwania przez tworzenia zapytań analizowanie zebranych danych, należy użyć istniejącego pulpity nawigacyjne, które można dostosować z widokami graficznego najbardziej przydatna wyszukiwań.  Teraz, gdy zdefiniowano zbierania danych operacyjnych w maszynach wirtualnych platformy Azure i Dzienniki aktywności, w tym samouczku dowiesz się, jak:

> [!div class="checklist"]
> * Uaktualnij zasobu usługi Analiza dzienników Azure do nowego języka zapytań 
> * Proste wyszukiwanie danych dotyczących zdarzeń i korzystać z funkcji do modyfikacji i filtrowanie wyników 
> * Dowiedz się, jak pracować z danymi wydajności

Aby ukończyć przykład, w tym samouczku, musisz mieć istniejącej maszyny wirtualnej [podłączony do obszaru roboczego analizy dzienników](log-analytics-quick-collect-azurevm.md).  

Tworzenie i edytowanie zapytań, oprócz pracy interakcyjnie z dane zwrotne, może być zakończonej jeden z dwóch sposobów.  Podstawowe zapytań Użyj strony wyszukiwania dziennika w portalu Azure lub zaawansowanych zapytań, mogą korzystać z portalu zaawansowana analityka. Aby dowiedzieć się więcej na temat różnic w działaniu między dwoma portalami, zobacz [portali tworzenia i edytowania dziennika zapytań w programie Azure Log Analytics](log-analytics-log-search-portals.md)

W tym samouczku firma Microsoft będzie pracować z dziennika wyszukiwania w portalu Azure. 

## <a name="log-in-to-azure-portal"></a>Zaloguj się do portalu Azure
Zaloguj się do portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com). 

## <a name="open-the-log-search-portal"></a>Otwórz portal wyszukiwania dziennika 
Uruchom, otwierając portal wyszukiwania dziennika.   

1. W portalu Azure kliknij **więcej usług** znaleziono w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **dziennika analizy**.
2. W okienku subskrypcje analizy dzienników, wybierz obszar roboczy, a następnie wybierz **wyszukiwania dziennika** kafelka.<br> ![Przycisk wyszukiwania dziennika](media/log-analytics-tutorial-viewdata/azure-portal-01.png)

Można zauważyć banerze w górnej części strony zasobów Log Analytics w portalu zaproszenie do uaktualnienia.<br> ![Analiza dzienników uaktualniania powiadomienia w portalu Azure](media/log-analytics-tutorial-viewdata/log-analytics-portal-upgradebanner.png)

Analiza dzienników niedawno wprowadzone nowy język kwerendy umożliwia łatwiejsze do skonstruowania wysyła zapytanie, korelowanie danych z różnych źródeł i analizować szybką identyfikację problemów lub trendów.

Rozszerzenie jest proste.  Uruchomić proces, klikając na banerze informacją **Dowiedz się więcej i uaktualnić**.  Zapoznaj się z artykułem dodatkowych informacji na temat uaktualniania na stronie informacje o uaktualnianiu, a następnie kliknij przycisk **Uaktualnij teraz**.

Proces potrwa kilka minut, a w tym czasie można śledzić postęp w obszarze **powiadomienia** z menu. Użytkownik może dowiedzieć się więcej o [zalet nowego języka zapytań](log-analytics-log-search-upgrade.md#why-the-new-language).

## <a name="create-a-simple-search"></a>Tworzenie prostego wyszukiwania
Najszybszy sposób pobrać niektórych danych do pracy z jest proste zapytanie zwracające wszystkie rekordy w tabeli.  Jeśli masz połączone żadnych klientów z systemem Windows lub Linux do swojego obszaru roboczego, a następnie należy dane zdarzenie (system Windows) lub w tabeli dziennika systemowego (Linux).

Wpisz jedno następujące kwerendy w polu wyszukiwania, a następnie kliknij przycisk wyszukiwania.  

```
Event
```
```
Syslog
```

Dane są zwracane w domyślny widok listy, a widać, ile całkowita liczba rekordów zostały zwrócone.

![Prostego zapytania](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-01.png)

Wyświetlane są tylko pierwszy kilka właściwości każdego rekordu.  Kliknij przycisk **Pokaż więcej** do wyświetlenia wszystkich właściwości dla określonego rekordu.

## <a name="filter-results-of-the-query"></a>Filtrowanie wyników zapytania
Po lewej stronie ekranu jest okienko filtru, dzięki czemu można dodać filtrowanie do zapytania bez modyfikowania jej bezpośrednio.  Kilka właściwości rekordu są wyświetlane dla tego typu rekordu i można wybrać jedną lub więcej wartości właściwości, aby zawęzić wyniki wyszukiwania.

Jeśli pracujesz z **zdarzeń**, zaznacz pole wyboru obok pozycji **błąd** w obszarze **EVENTLEVELNAME**.   Jeśli pracujesz z **Syslog**, zaznacz pole wyboru obok pozycji **błąd** w obszarze **poziom ważności**.  Spowoduje to zmianę zapytanie do jednego z następujących czynności, aby ograniczyć wyniki do zdarzenia błędów.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filtr](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-02.png)

Dodawanie właściwości do okienka filtru, wybierając **dodać do filtrów** z menu właściwości na jednym z nich.

![Dodawanie do menu filtrowania](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-03.png)

Ten sam filtr można ustawić, wybierając **filtru** z menu właściwości rekordu o wartości do filtrowania.  

Masz tylko **filtru** opcji dla właściwości o nazwie ich na niebiesko po umieszczeniu nad nimi.  Są to *wyszukiwanie* pola, które są indeksowane dla warunki wyszukiwania.  Pola w kolorze szarym są *wolne tekst można wyszukiwać* pola, które mają tylko **Pokaż odwołania** opcji.  Ta opcja zwraca rekordy, które mają tę wartość w dowolnej właściwości.

W przypadku grupowania wyników dla jednej właściwości wybierając **Grupuj według** opcji z menu rekordu.  Spowoduje to dodanie [Podsumuj](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operatora w kwerendzie wyświetli wyniki na wykresie.  Można grupować w więcej niż jedną właściwość, ale trzeba edytować zapytanie bezpośrednio.  Następnie wybierz menu rekordu **komputera** właściwości i wybierz **Grupuj według "Komputer"**.  

![Grupuj według komputera](media/log-analytics-tutorial-viewdata/log-analytics-portal-eventlist-04.png)

## <a name="work-with-results"></a>Praca z wynikami
Portal wyszukiwania dziennika ma wiele funkcji do pracy z wyników zapytania.  Można sortować, filtr i wyniki grupy do analizowania danych bez modyfikowania zapytania rzeczywistego.  Domyślnie nie są sortowane wyniki zapytania.

Aby wyświetlić dane w postaci tabeli, która zapewnia dodatkowe opcje filtrowania i sortowania, kliknij przycisk **tabeli**.  

![Widok tabeli](media/log-analytics-tutorial-viewdata/log-search-portal-table-01.png)

Kliknij strzałkę przez rekord, aby wyświetlić szczegóły dla tego rekordu.

![Sortowanie wyników](media/log-analytics-tutorial-viewdata/log-search-portal-table-02.png)

Sortuj według dowolnego pola, klikając jej nagłówek kolumny.

![Sortowanie wyników](media/log-analytics-tutorial-viewdata/log-search-portal-table-03.png)

Filtrować wyniki na określoną wartość w kolumnie, klikając przycisk filtru i zapewnianie warunek filtru.

![Filtrowanie wyników](media/log-analytics-tutorial-viewdata/log-search-portal-table-04.png)

Grupy dla kolumny przez przeciągnięcie jego nagłówka kolumny na początku wyników.  Można grupować według wielu pól, przeciągając wiele kolumn do góry.

![Wyniki grupy](media/log-analytics-tutorial-viewdata/log-search-portal-table-05.png)


## <a name="work-with-performance-data"></a>Praca z danych wydajności
Dane wydajności dla agentów systemów Windows i Linux są przechowywane w obszarze roboczym analizy dzienników w **wydajności** tabeli.  Rejestruje wydajności wyglądać podobnie jak dowolny inny rekord i zamierzamy Napisz proste zapytanie, które zwraca wszystkie rekordy wydajności, podobnie jak ze zdarzeniami.

```
Perf
```

![Dane dotyczące wydajności](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-01.png)

Mimo że zwracanie miliony rekordów wszystkie obiekty i liczniki wydajności nie jest bardzo przydatne.  Można użyć tych samych metod używanych powyżej przefiltruj dane lub po prostu wpisz poniższe zapytanie bezpośrednio w polu wyszukiwania dziennika.  To polecenie zwróci tylko procesor rekordów użycia dla komputerów z systemami Windows i Linux.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Użycie procesora](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-02.png)

To ogranicza dane do określonego licznika, ale jego nadal nie umieszcza je w postaci jest szczególnie przydatne.  Możesz wyświetlić dane w wykres liniowy, ale najpierw należy go Grupuj według komputera i TimeGenerated.  Aby grupować według wielu pól, należy bezpośrednio zmodyfikować zapytanie, tak zmodyfikuj zapytanie do następującego.  Ta metoda korzysta [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) działać na **równowartości** właściwości do obliczenia średniej wartości ciągu każdej godziny.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Wykres danych wydajności](media/log-analytics-tutorial-viewdata/log-analytics-portal-perfsearch-03.png)

Teraz, gdy dane są odpowiednio grupowane, można je wyświetlić na wykresie visual przez dodanie [renderowania](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operatora.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Wykres liniowy](media/log-analytics-tutorial-viewdata/log-analytics-portal-linechart-01.png)

## <a name="next-steps"></a>Następne kroki
W tym samouczku przedstawiono sposób tworzenia podstawowych dziennik wyszukiwania do analizowania danych zdarzeń i wydajności.  Przejście do dalej samouczkiem, aby dowiedzieć się, jak wizualizacji danych przez utworzenie pulpitu nawigacyjnego.

> [!div class="nextstepaction"]
> [Tworzenie i udostępnianie pulpitów nawigacyjnych analizy dzienników](log-analytics-tutorial-dashboards.md)