---
title: "Za pomocą portalu wyszukiwania dziennika w Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera samouczek, który opisuje sposób tworzenia dziennik wyszukiwania i analizowania danych przechowywanych w obszarze roboczym analizy dzienników przy użyciu portalu wyszukiwania dziennika.  Samouczek obejmuje uruchamiania niektórych zapytań proste być zwracanie różnych typów danych i analiza wyników."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: bwren
ms.openlocfilehash: 3a2e8803d51d81ab0eda3dc814d01822e17bc14e
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
# <a name="create-log-searches-in-azure-log-analytics-using-the-log-search-portal"></a>Tworzenie wyszukiwań dziennika w Analiza dzienników Azure przy użyciu portalu wyszukiwania dziennika

> [!NOTE]
> W tym artykule opisano portalu wyszukiwania dziennika w przy użyciu nowego języka zapytań usługi Analiza dzienników Azure.  Możesz dowiedzieć się więcej o nowy język i uzyskać Procedura uaktualniania obszaru roboczego na [uaktualnienia obszaru roboczego analizy dzienników Azure do nowego wyszukiwania dziennika](log-analytics-log-search-upgrade.md).  
>
> Jeśli nowy język kwerendy nie została ona uaktualniona obszaru roboczego, należy zapoznać się [wyszukiwanie danych przy użyciu dziennika wyszukiwania w analizy dzienników](log-analytics-log-searches.md) Aby uzyskać informacje o bieżącej wersji portalu wyszukiwania dziennika.

Ten artykuł zawiera samouczek, który opisuje sposób tworzenia dziennik wyszukiwania i analizowania danych przechowywanych w obszarze roboczym analizy dzienników przy użyciu portalu wyszukiwania dziennika.  Samouczek obejmuje uruchamiania niektórych zapytań proste być zwracanie różnych typów danych i analiza wyników.  Głównie funkcji w portalu wyszukiwania dziennika modyfikowania zapytania, a nie ich modyfikować.  Aby uzyskać więcej informacji dotyczących bezpośredniego edytowania zapytania, zobacz [Query Language reference](https://go.microsoft.com/fwlink/?linkid=856079).

Aby utworzyć wyszukiwania w portalu analityka zaawansowane zamiast portalu wyszukiwania dziennika, zobacz [wprowadzenie do korzystania z portalu usługi analiza](https://go.microsoft.com/fwlink/?linkid=856587).  Obu portalach używany ten sam język kwerendy można uzyskać dostępu do tych samych danych, w obszarze roboczym analizy dzienników.

## <a name="prerequisites"></a>Wymagania wstępne
Ten samouczek zakłada, że masz już obszar roboczy analizy dzienników z co najmniej jedno źródło połączonych, który generuje dane dla zapytań do analizowania.  

- Jeśli nie masz obszaru roboczego, możesz utworzyć bezpłatne przy użyciu procedury na [Rozpoczynanie pracy z obszaru roboczego analizy dzienników](log-analytics-get-started.md).
- Co najmniej jednego połączenia [agenta Windows](log-analytics-windows-agent.md) lub [agenta systemu Linux](log-analytics-linux-agents.md) do obszaru roboczego.  

## <a name="open-the-log-search-portal"></a>Otwórz portal wyszukiwania dziennika
Uruchom, otwierając portal wyszukiwania dziennika. 

1. Otwórz witrynę Azure Portal.
2. Przejdź do analizy dzienników i wybierz obszar roboczy.
3. Wybierz **dziennika wyszukiwania**.

![Przycisk wyszukiwania dziennika](media/log-analytics-log-search-log-search-portal/log-search-button.png)

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

![Prostego zapytania](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

Wyświetlane są tylko pierwszy kilka właściwości każdego rekordu.  Kliknij przycisk **Pokaż więcej** do wyświetlenia wszystkich właściwości dla określonego rekordu.

![Szczegóły rekordu](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-the-time-scope"></a>Ustaw zakres czasu
Każdy rekord zebrane przez analizy dzienników ma **TimeGenerated** właściwość, która zawiera Data i godzina utworzenia rekordu.  Zapytania w portalu wyszukiwania dziennika zwraca tylko rekordy z **TimeGenerated** w zakresie czasu, który jest wyświetlany po lewej stronie ekranu.  

Filtr czasu można zmienić, wybierając z menu rozwijanego albo modyfikując suwaka.  Suwak Wyświetla wykres słupkowy przedstawiający względną liczbę rekordów dla każdego segmentu czasu w zakresie.  Ten segment będą się różnić w zależności od zakresu.

Domyślny zakres czasu to **1 dzień**.  Zmień tę wartość na **7 dni**, i zwiększyć całkowita liczba rekordów.

![Zakres czasu daty](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-the-query"></a>Filtrowanie wyników zapytania
Po lewej stronie ekranu jest okienko filtru, dzięki czemu można dodać filtrowanie do zapytania bez modyfikowania jej bezpośrednio.  Kilka właściwości zwracanych rekordów są wyświetlane z wartościami pierwszych dziesięciu z ich liczba rekordów.

Jeśli pracujesz z **zdarzeń**, zaznacz pole wyboru obok pozycji **błąd** w obszarze **EVENTLEVELNAME**.   Jeśli pracujesz z **Syslog**, zaznacz pole wyboru obok pozycji **błąd** w obszarze **poziom ważności**.  Spowoduje to zmianę zapytanie do jednego z następujących czynności, aby ograniczyć wyniki do zdarzenia błędów.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filtr](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

Dodawanie właściwości do okienka filtru, wybierając **dodać do filtrów** z menu właściwości na jednym z nich.

![Dodawanie do menu filtrowania](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

Ten sam filtr można ustawić, wybierając **filtru** z menu właściwości rekordu o wartości do filtrowania.  

Masz tylko **filtru** opcji dla właściwości o nazwie ich w kolorze niebieskim.  Są to *wyszukiwanie* pola, które są indeksowane dla warunki wyszukiwania.  Pola w kolorze szarym są *wolne tekst można wyszukiwać* pola, które mają tylko **Pokaż odwołania** opcji.  Ta opcja zwraca rekordy, które mają tę wartość w dowolnej właściwości.

![Menu filtrowania](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

W przypadku grupowania wyników dla jednej właściwości wybierając **Grupuj według** opcji z menu rekordu.  Spowoduje to dodanie [Podsumuj](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operatora w kwerendzie wyświetli wyniki na wykresie.  Można grupować w więcej niż jedną właściwość, ale trzeba edytować zapytanie bezpośrednio.  Następnie wybierz menu rekordu **komputera** właściwości i wybierz **Grupuj według "Komputer"**.  

![Grupuj według komputera](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a>Praca z wynikami
Portal wyszukiwania dziennika ma wiele funkcji do pracy z wyników zapytania.  Można sortować, filtr i wyniki grupy do analizowania danych bez modyfikowania zapytania rzeczywistego.  Domyślnie nie są sortowane wyniki zapytania.

Aby wyświetlić dane w postaci tabeli, która zapewnia dodatkowe opcje filtrowania i sortowania, kliknij przycisk **tabeli**.  

![Widok tabeli](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

Kliknij strzałkę przez rekord, aby wyświetlić szczegóły dla tego rekordu.

![Sortowanie wyników](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

Sortuj według dowolnego pola, klikając jej nagłówek kolumny.

![Sortowanie wyników](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

Filtrować wyniki na określoną wartość w kolumnie, klikając przycisk filtru i zapewnianie warunek filtru.

![Filtrowanie wyników](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

Grupy dla kolumny przez przeciągnięcie jego nagłówka kolumny na początku wyników.  Można grupować według wielu pól, przeciągając wiele kolumn do góry.

![Wyniki grupy](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a>Praca z danych wydajności
Dane wydajności dla agentów systemów Windows i Linux są przechowywane w obszarze roboczym analizy dzienników w **wydajności** tabeli.  Rejestruje wydajności wyglądać podobnie jak dowolny inny rekord i firma Microsoft może zapisywać prostego zapytania, która zwraca wszystkie rekordy wydajności, podobnie jak ze zdarzeniami.

```
Perf
```

![Dane dotyczące wydajności](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

Mimo że zwracanie miliony rekordów wszystkie obiekty i liczniki wydajności nie jest bardzo przydatne.  Można użyć tych samych metod używanych powyżej przefiltruj dane lub po prostu wpisz poniższe zapytanie bezpośrednio w polu wyszukiwania dziennika.  To polecenie zwróci tylko procesor rekordów użycia dla komputerów z systemami Windows i Linux.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Użycie procesora](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

To ogranicza dane do określonego licznika, ale jego nadal nie umieszcza je w postaci jest szczególnie przydatne.  Możesz wyświetlić dane w wykres liniowy, ale najpierw należy go Grupuj według komputera i TimeGenerated.  Aby grupować według wielu pól, należy bezpośrednio zmodyfikować zapytanie, tak zmodyfikuj zapytanie do następującego.  Ta metoda korzysta [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) działać na **równowartości** właściwości do obliczenia średniej wartości ciągu każdej godziny.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Wykres danych wydajności](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

Teraz, gdy dane są odpowiednio grupowane, można je wyświetlić na wykresie visual przez dodanie [renderowania](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operatora.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Wykres liniowy](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a>Kolejne kroki

- Dowiedz się więcej o język zapytań usługi Analiza dzienników w [wprowadzenie do korzystania z portalu usługi analiza](https://go.microsoft.com/fwlink/?linkid=856079).
- Przewodnik po użyciu samouczka [portal analityka zaawansowane](https://go.microsoft.com/fwlink/?linkid=856587) co pozwala na tej samej kwerend i dostęp do tych samych danych jako portal wyszukiwania dziennika.
