---
title: Wyszukiwanie danych z dziennika wyszukiwania w Azure Log Analytics | Dokumentacja firmy Microsoft
description: "Wyszukiwania w dziennikach umożliwiają łączenie i korelowanie danych dowolnej maszyny z wielu źródeł w danym środowisku."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 0d7b6712-1722-423b-a60f-05389cde3625
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: bwren
ms.openlocfilehash: cef7fc282edc7396a0f26dab98ea7f1087315b23
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/13/2018
---
# <a name="find-data-using-log-searches-in-log-analytics"></a>Wyszukiwanie danych przy użyciu dziennika wyszukiwania w analizy dzienników

>[!NOTE]
> W tym artykule opisano dziennik wyszukiwania przy użyciu języka zapytań starszej wersji w analizy dzienników.  Jeśli obszaru roboczego został uaktualniony do [języka zapytań nowe analizy dzienników](log-analytics-log-search-upgrade.md), następnie należy zapoznać się [opis dziennika przeszukuje analizy dzienników (nowy)](log-analytics-log-search-new.md).


Stanowiącej podstawę analizy dzienników jest dziennika funkcji wyszukiwania, dzięki czemu można łączyć i skorelowania danych dowolnego komputera z wielu źródeł w danym środowisku. Rozwiązania są również obsługiwane przez dziennik wyszukiwania, aby wyświetlić metryki przestawiać wokół obszaru określonego problemu.

Na stronie wyszukiwania można utworzyć kwerendę, a następnie podczas wyszukiwania, możesz filtrować wyniki za pomocą formantów zestawu reguł. Można też utworzyć zaawansowanych zapytań przekształcenia filtru i raportu na wyniki.

Typowe zapytania wyszukiwania dziennika znajdują się na większości stronach rozwiązania. W portalu OMS możesz kliknąć Kafelki lub rozwinąć do innych elementów, aby wyświetlić szczegóły dotyczące elementu za pomocą wyszukiwania dziennika.

W tym samouczku zostanie omówiony przykłady wszystkie kroki te obejmują podstawy podczas wyszukiwania dziennika.

Firma Microsoft będzie rozpoczynać się od prostego, praktyczne przykłady i następnie ich tak, aby uzyskać opis praktycznych zastosowań o tym, jak używać składni w celu wyodrębnienia szczegółowych danych, które mają z danych.

Po znasz zapoznać się z techniki wyszukiwania, można przejrzeć [analizy dzienników dziennika odwołanie wyszukiwania](log-analytics-search-reference.md).

## <a name="use-basic-filters"></a>Używanie filtrów podstawowych
Jest najpierw wiedzieć, że pierwsza część wyszukiwania zapytania przed każdą "|" znak kreski pionowej, jest zawsze *filtru*. Należy go traktować jako klauzuli WHERE TSQL — Określa *co* podzbiór danych na potrzeby pobierania poza obszar roboczy analizy dzienników. Wyszukiwanie w magazynie danych dotyczy przede wszystkim określenie właściwości dane, które mają zostać wyodrębnione, dlatego jest naturalna, czy zapytanie może rozpoczynać się w klauzuli WHERE.

Filtry najbardziej podstawowym, można użyć są *słowa kluczowe*, na przykład "error" lub 'timeout' lub nazwę komputera. Tego rodzaju prostego zapytania zwracają zazwyczaj różnych kształty danych w ramach tego samego zestawu wyników. Analiza dzienników ma inną *typy* danych w systemie.

### <a name="to-conduct-a-simple-search"></a>Przeprowadzenie proste wyszukiwanie
1. W portalu OMS kliknij **wyszukiwania dziennika**.  
    ![Kafelek wyszukiwania](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. W polu kwerendy wpisz `error` , a następnie kliknij przycisk **wyszukiwania**.  
    ![Błąd wyszukiwania](./media/log-analytics-log-searches/oms-search-error.png)  
    Na przykład zapytanie dla `error` na poniższej ilustracji zwrócił 100 000 **zdarzeń** rekordy (zebrane przez zarządzanie dziennikiem), 18 **ConfigurationAlert** rekordów (wygenerowanych przez ocenę konfiguracji) i 12 **Zmianakonfiguracji** rekordy (przechwycone przez śledzenia zmian).   
    ![wyniki wyszukiwania](./media/log-analytics-log-searches/oms-search-results01.png)  

Te filtry nie są naprawdę klas typów obiektów. *Typ* jest tylko tag lub właściwością lub ciąg/Nazwa/kategorii, który jest dołączony do elementu danych. Niektóre dokumenty w systemie są oznaczone jako **typu: ConfigurationAlert** i niektóre są oznaczone jako **typu: wydajności**, lub **typu: zdarzenia**i tak dalej. Każdy wynik wyszukiwania, dokument, rekordu lub wpis Wyświetla pierwotne właściwości i ich wartości dla każdego z tych fragmentów danych oraz tych nazw pól można użyć do określania w filtrze, gdy ma zostać pobrane tylko te rekordy, gdy pole nie ma podanej wartości.

*Typ* jest naprawdę tylko pola, które mają wszystkie rekordy, nie jest inny niż inne pole. Ustanowiono to oparta na wartości pola typu. Ten rekord ma inny formularz lub kształtu. Okazjonalnie **typu = wydajności**, lub **typu = zdarzeń** jest również składni, które należy poznać, aby wysyłać zapytania dotyczące danych wydajności lub zdarzeń.

Używając dwukropkiem (:) lub znakiem równości (=) po nazwie pola i przed wartością. **Typ: zdarzenie** i **typu = zdarzeń** są równoważne w rozumieniu, można wybrać styl preferowane.

Jeśli tak, typ = wydajności rekordów pole o nazwie "CounterName", a następnie można pisać zapytania podobne `Type=Perf CounterName="% Processor Time"`.

Zapewni to tylko dane dotyczące wydajności nazwy licznika wydajności w przypadku "% czasu procesora".

### <a name="to-search-for-processor-time-performance-data"></a>Aby wyszukiwać dane wydajności czas procesora
* W polu zapytania wyszukiwania wpisz `Type=Perf CounterName="% Processor Time"`

Można również być bardziej szczegółowe i używać **InstanceName = _ "Ogółem"** w zapytaniu, która jest licznika wydajności systemu Windows. Możesz też wybrać zestaw reguł, a drugi **pola: wartość**. Filtr jest automatycznie dodawany do filtru na pasku zapytania. Widać to na poniższej ilustracji. Pokazuje możliwości kliknij, aby dodać **InstanceName: "_łącznie"** zapytania bez wprowadzania żadnych czynności.

![aspekt wyszukiwania](./media/log-analytics-log-searches/oms-search-facet.png)

Zapytanie staje się teraz `Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

W tym przykładzie, nie trzeba określić **typu = wydajności** na uzyskanie dostępu do tego wyniku. Ponieważ pola CounterName i InstanceName tylko istnieje dla rekordów typu = wydajności, zapytanie jest wystarczająco konkretny, aby zwrócić takie same wyniki jak dłużej, poprzedniego:

```
CounterName="% Processor Time" InstanceName="_Total"
```

Jest to spowodowane wszystkie filtry w zapytaniu są oceniane jako *i* ze sobą. Efektywne, więcej pól, należy dodać kryteria, możesz uzyskać mniejszy, więcej określonych i precyzyjnych wyników.

Na przykład zapytanie `Type=Event EventLog="Windows PowerShell"` jest taka sama jak `Type=Event AND EventLog="Windows PowerShell"`. Zwraca wszystkie zdarzenia, które zostały zarejestrowane w i zbierane z dziennika zdarzeń programu Windows PowerShell. Jeśli dodasz filtr wielokrotnie wielokrotnie, wybierając tego samego zestawu reguł, problem jest całkowicie bardzo drobny — może zajmowały pasek wyszukiwania, ale nadal program zwraca takie same wyniki ponieważ niejawny operator i są zawsze dostępne.

Niejawny operator AND można łatwo cofnąć za pomocą operatora NOT jawnie. Na przykład:

`Type:Event NOT(EventLog:"Windows PowerShell")` lub jego odpowiednik `Type=Event EventLog!="Windows PowerShell"` zwrócić wszystkie zdarzenia ze wszystkich dzienników, które nie są w Dzienniku programu Windows PowerShell.

Można użyć innych operator logiczny takich jak "Lub". Następujące zapytanie zwraca rekordy, dla których dziennik zdarzeń jest albo aplikacji lub systemu.

```
EventLog=Application OR EventLog=System
```

Za pomocą powyższego zapytania, uzyskasz wpisy dla obu dzienników w tym samym zestawie wyników.

Jednak jeśli usuniesz lub pozostawić niejawne i w miejscu, następnie następujące zapytanie nie utworzy żadnych wyników, ponieważ nie istnieje wpis dziennika zdarzeń, który należy do obu dzienników. Każdy wpis dziennika zdarzeń został zapisany do tylko jednej z dwóch dzienników.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Używanie filtrów dodatkowe
Następujące zapytanie zwraca wpisy 2 dzienników zdarzeń dla wszystkich komputerów, które zostały wysłane dane.

```
EventLog=Application OR EventLog=System
```

![Wyniki wyszukiwania](./media/log-analytics-log-searches/oms-search-results03.png)

Wybranie jednego z pól lub filtry zostaną uściślające kwerendę do określonego komputera, z wyłączeniem innych protokołów. Wynikowe zapytanie będzie wyglądać w następujący sposób.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Jest odpowiednikiem poniższe polecenie, ze względu na niejawne koniunkcji binarnej.

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Każde zapytanie są oceniane w następującej kolejności jawnego. Należy pamiętać, nawias.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Podobnie jak pole dziennika zdarzeń można pobrać dane tylko w przypadku zestaw komputerów określonych przez dodanie lub. Na przykład:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Podobnie, to następujące zapytanie zwraca **% czasu procesora CPU** tylko wybranych komputerów dwa.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```

### <a name="field-types"></a>Typy pól
Podczas tworzenia filtrów, należy poznać różnice w pracy z różnymi typami pól zwrócony przez dziennik wyszukiwania.

**Pola z możliwością przeszukiwania** Pokaż na niebiesko w wynikach wyszukiwania.  Pól z możliwością wyszukiwania w określonych warunkach wyszukiwania służy do tego pola, takie jak następujące:

```
Type: Event EventLevelName: "Error"
Type: SecurityEvent Computer:Contains("contoso.com")
Type: Event EventLevelName IN {"Error","Warning"}
```

**Zwolnij pól z możliwością wyszukiwania tekstu** są wyświetlane w kolorze szarym w wynikach wyszukiwania.  Nie można ich używać z określonego na pole, takich jak pola z możliwością przeszukiwania warunkami wyszukiwania.  Są one tylko przeszukiwane, gdy wszystkie pola, takie jak wykonywanie zapytania.

```
"Error"
Type: Event "Exception"
```


### <a name="boolean-operators"></a>Operatory logiczne
Datetime i pól liczbowych, możesz wyszukać wartości przy użyciu *większe*, *mniejszym niż*, i *mniejsza lub równa*. Można używać operatorów prosty przykład >, <>, =, < =,! = na pasku wyszukiwania zapytania.

Umożliwia wysyłanie zapytań określonego dziennika zdarzeń w określonym okresie czasu. Na przykład ostatnich 24 godzinach jest wyrażona z następującym wyrażeniem skrótu.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Wyszukiwania za pomocą operatora logicznego
* W polu zapytania wyszukiwania wpisz `EventLog=System TimeGenerated>NOW-24HOURS`  
    ![Wyszukiwanie z wartość logiczna](./media/log-analytics-log-searches/oms-search-boolean.png)

Mimo że można kontrolować graficznie przedział czasu, a większość czasu można to zrobić, istnieją pewne zalety bezpośrednio do zapytania w tym filtr czasu. Na przykład, to rozwiązanie idealne za pomocą pulpitów nawigacyjnych, w którym można zastąpić niezależnie od tego czasu dla każdego kafelka *globalne* selektora czas na stronie pulpitu nawigacyjnego. Aby uzyskać więcej informacji, zobacz [sprawach czas na pulpicie nawigacyjnym](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Podczas filtrowania według czasu, należy pamiętać o tym, który można pobrać wyników dla *przecięcia* dwóch okresów: określona w portalu OMS (S1) i jeden w kwerendzie (S2).

![część wspólną](./media/log-analytics-log-searches/oms-search-intersection.png)

Oznacza to, że jeśli okresy nie intersect, na przykład w portalu OMS, w którym można wybrać **ten tydzień** i w zapytaniu, gdzie został zdefiniowany **ostatniego tygodnia**, to nie ma żadnych przecięcia i nie będzie otrzymywać wszystkie wyniki.

Operatory porównania pola TimeGenerated przydają się również w innych sytuacjach. Na przykład w przypadku pól liczbowych.

Podane na przykład, że konfiguracja oceny alerty mają następujące wartości ważności:

* 0 = informacji
* 1 = Ostrzeżenie
* 2 = krytyczne

Możesz wyszukać zarówno ostrzegawcze i krytyczne alerty i również wykluczyć informacyjną sieci z następującego zapytania:

```
Type=ConfigurationAlert  Severity>=1
```


Umożliwia także zakresu zapytania. Oznacza to, zapewniają od początku i na końcu zakresu wartości w sekwencji. Na przykład jeśli chcesz zdarzenia w dzienniku zdarzeń programu Operations Manager, gdzie identyfikator zdarzenia jest większa niż lub równa 2100, ale nie jest większa niż 2199 następnie następującej kwerendy zwróci je.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


> [!NOTE]
> Składnia zakres, należy użyć jest separatora pola: wartość dwukropka (:) i *nie* znak równości (=). Ujmij dolna i górna końca zakresu w nawiasach kwadratowych i oddziel dwie kropki (.).
>
>

## <a name="manipulate-search-results"></a>Opracowanie wyników wyszukiwania
Jeśli szukasz danych należy kwerendzie wyszukiwania i mieć dobrą poziom kontroli nad wyniki. Gdy pobierane są wyniki, należy zastosować polecenia, aby przetwarzać je.

Polecenia w wyszukiwaniach analizy dzienników *musi* następować po znaku kreski pionowej (|). Filtr musi być zawsze pierwsza część ciągu zapytania. Definiuje zestaw danych, z którym pracujesz z, a następnie "powoduje przekazanie w potoku" uzyskanych w poleceniu. Potoku można następnie użyć, aby dodać dodatkowe polecenia. To jest luźno podobne do potoku środowiska Windows PowerShell.

Ogólnie rzecz biorąc język wyszukiwania analizy dzienników próbuje wykonać styl środowiska PowerShell i wytycznych dokonanie podobnie jak specjaliści IT i nauki do jej obsługi ułatwiają.

Polecenia mają nazwy zleceń, dzięki czemu można łatwo sprawdzić, co zrobić.  

### <a name="sort"></a>Sortuj
Polecenie sortowania można zdefiniować kolejność sortowania według jednego lub wielu pól. Nawet jeśli nie używasz go, domyślna, czas, w kolejności malejącej jest wymuszana. Najnowsze wyniki zawsze znajdowały się u góry wyników wyszukiwania. Oznacza to, że po uruchomieniu wyszukiwania z `Type=Event EventID=1234` co naprawdę wykonaniu dla Ciebie jest:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

To, ponieważ jest typ obsługi, które znasz w dziennikach. Na przykład w Podglądzie zdarzeń systemu Windows.

Sortowanie umożliwia zmianę sposobu są zwracane. W poniższych przykładach pokazano, jak to działa.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


Proste przykłady powyżej pokazać działanie polecenia — zmienia kształt filtr wyświetlił wyniki.

### <a name="limit-and-top"></a>Limit i od góry
Inne mniej znanych polecenie jest ograniczona. Limit wynosi zlecenie przypominającej programu PowerShell. Limit jest funkcjonalnie identyczne polecenie TOP. Następujące kwerendy zwracają takie same wyniki.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Aby wyszukać góry
* W polu zapytania wyszukiwania wpisz `Type=Event EventID=600 | Top 1`   
    ![top wyszukiwania](./media/log-analytics-log-searches/oms-search-top.png)

Na powyższej ilustracji, istnieją tysiące 358 rekordy z EventID = 600. Pola, aspekty i filtry po lewej stronie zawsze zawierają informacje o wynikach *przez część filtru* zapytania, który jest część przed znakiem potoku. **Wyniki** okienko zwraca tylko najnowszy wynik 1, ponieważ przykładowe polecenie w kształcie i przekształcić wyniki.

### <a name="select"></a>Wybierz pozycję
Wybierz polecenie zachowuje się jak Select-Object w programie PowerShell. Zwraca przefiltrowane wyniki, które nie ma wszystkich swoich oryginalnych właściwości. Wybiera tylko właściwości, które określisz.

#### <a name="to-run-a-search-using-the-select-command"></a>Aby uruchomić wyszukiwanie przy użyciu polecenia wyboru
1. W polu Wyszukaj wpisz `Type=Event` , a następnie kliknij przycisk **wyszukiwania**.
2. Kliknij przycisk **+ Pokaż więcej** w jednym z rezultatów, aby wyświetlić wszystkie właściwości, które mają wyniki.
3. Wybierz niektóre z tych jawnie, a zapytanie zmienia `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Wybierz wyszukiwanie](./media/log-analytics-log-searches/oms-search-select.png)

To polecenie jest szczególnie przydatne, gdy użytkownik chce kontrolować wyszukiwania danych wyjściowych i wybierz tylko części danych, która naprawdę istotne dla podróży, co często jest pełny rekord. Umożliwia to również mieć rekordów o różnych typach *niektórych* wspólnych właściwości, ale nie *wszystkie* ich właściwości są wspólne. Generowanie danych wyjściowych, który wygląda tabeli w naturalnie więcej lub działa dobrze w przypadku, gdy wyeksportowane do pliku CSV, a następnie sprasowany w programie Excel.

## <a name="use-the-measure-command"></a>Użyj polecenia miary
MIARA jest jednym z najbardziej wszechstronny poleceń w wyszukiwaniach analizy dzienników. Umożliwia zastosowanie statystyczne *funkcji* z danymi i agregacji wyników, pogrupowanych według danego pola. Istnieje wiele funkcji statystycznych, które obsługuje miary.

### <a name="measure-count"></a>Miara count()
Pierwszej funkcji statystycznych do pracy z, a jeden z najprostszym zrozumienie jest *count()* funkcji.

Wynikiem każde zapytanie operacji wyszukiwania, takie jak `Type=Event`, pokazuje filtrów, nazywane również aspekty po lewej stronie wyników wyszukiwania. Filtry zawierają rozkład wartości w danym polu wyników w wyszukiwania.

![liczba miar wyszukiwania](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Na przykład na ilustracji powyżej zobaczysz **komputera** pola i informuje, że w zdarzeniach prawie 739 tysięcy w wynikach, umożliwiają 68 unikatowe i różne wartości **komputera** w tych rekordów. Kafelek zawiera tylko pierwsze 5, które są najbardziej typowych 5 wartości, które są zapisywane w **komputera** pól), są posortowane według liczby dokumentów, które zawierają tej określonej wartości w tym polu. W obrazie widać, że — wśród tych zdarzeń prawie 369 tysięcy — tysięcy 90 pochodzi z komputera OpsInsights04.contoso.com tysięcy 83 z komputera DB03.contoso.com i tak dalej.

Co zrobić, jeśli chcesz wyświetlić wszystkie wartości, ponieważ Kafelek zawiera tylko pierwsze 5?

To polecenie miary czynności przy użyciu funkcji count(). Ta funkcja nie używa żadnych parametrów. Wystarczy określić pola, według której chcesz grupować według — **komputera** pola w tym przypadku:

`Type=Event | Measure count() by Computer`

![liczba miar wyszukiwania](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

Jednak **komputera** jest to pole używane *w* każdego z nich dane — obejmuje nie relacyjnych baz danych i nie istnieje żadne oddzielne **komputera** obiektów w dowolnym miejscu. Tylko wartości *w* danych można opisać, jakiej encji wygenerowany je oraz wiele innych właściwości aspekty danych — dlatego termin *aspektu*. Jednak równie dobrze można grupować według innych pól. Ponieważ oryginalne wyniki prawie 739 tysięcy zdarzeń, które są przekazywane w potoku do polecenia miary również ma pole o nazwie **EventID**, można zastosować tę samą metodę Grupuj według tego pola i Pobierz liczbę zdarzeń przez identyfikator zdarzenia:

```
Type=Event | Measure count() by EventID
```

Jeśli użytkownik nie chce liczba rzeczywistego rekordu, który zawiera konkretną wartość, ale zamiast tego Jeśli chcesz tylko listę same wartości, możesz dodać *wybierz* poleceń na końcu i po prostu wybierz pierwszej kolumny:

```
Type=Event | Measure count() by EventID | Select EventID
```

Następnie można uzyskać w bardziej skomplikowanych i wstępnie sortowanie wyników w zapytaniu lub wystarczy kliknąć kolumn w siatce, zbyt.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Aby wyszukać liczba miary
* W polu zapytania wyszukiwania wpisz `Type=Event | Measure count() by EventID`
* Dołącz `| Select EventID` na końcu zapytania.
* Na koniec Dołącz `| Sort EventID asc` na końcu zapytania.

Istnieje kilka ważnych punktów zauważyć i wyróżnić:

Po pierwsze wyniki widocznej nie są oryginalne nieprzetworzone wyniki już. Zamiast tego są one wyników zagregowany — zasadniczo grup wyników. Nie jest to problem, ale należy pamiętać, że interakcja bardzo inny kształt danych, która różni się z oryginalnego raw kształtu, który pobiera stworzony w locie wyniku funkcji agregacji/statystyczne.

Drugi, **pomiaru liczby** obecnie zwraca tylko pierwszych 100 różne wyniki. To ograniczenie nie ma zastosowania do innych funkcji statystycznych. Tak zazwyczaj należy najpierw użyj bardziej dokładne filtr do wyszukiwania określonych elementów przed zastosowaniem funkcji count() miary.

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>Użyj funkcji max i min przy użyciu polecenia miary
Istnieją różne scenariusze gdzie **Max() miary** i **Min() miary** są przydatne. Jednak ponieważ każdej funkcji jest przeciwny od siebie, możemy ilustrują Max() i możesz eksperymentować z Min() samodzielnie.

Po wykonaniu zapytania do zdarzenia zabezpieczeń mają **poziom** właściwość, która może się różnić. Na przykład:

```
Type=SecurityEvent
```

![start liczba miar wyszukiwania](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Jeśli chcesz wyświetlić najwyższą wartość dla wszystkich zabezpieczeń zdarzenia danego komputera z programem, Grupuj według pola, możesz użyć

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![Wyszukiwanie miary maksymalnej komputera](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Który będzie wyświetlany dla komputerów, na których zastosowano **poziom** rekordów, większość z nich ma co najmniej poziom 8, ma wiele poziom 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![Maksymalny czas miar wyszukiwania wygenerowany komputera](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Ta funkcja działa prawidłowo w przypadku liczb, ale współdziała również z pól daty i godziny. Warto sprawdzić sygnatura czasowa ostatniego lub najnowszych dowolne dane indeksowania dla każdego komputera. Na przykład: gdy najnowsze zdarzenie zabezpieczeń zgłoszono dla każdej maszyny?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>Użyj w poleceniu miary za pomocą funkcji AVG.
Funkcji statystycznych Avg() używane z miarą pozwala obliczyć średnią wartość niektóre pola, a wyniki grupy za pomocą pola tego samego lub innego. Jest to użyteczne w wielu przypadkach, takich jak dane dotyczące wydajności.

Zaczniemy dane dotyczące wydajności. Należy pamiętać, że analizy dzienników aktualnie zbiera dane liczników wydajności dla systemu Windows i Linux maszyny.

Aby wyszukać *wszystkie* dane dotyczące wydajności, zapytanie najbardziej podstawowa jest:

```
Type=Perf
```

![wyszukiwanie start Śr.](./media/log-analytics-log-searches/oms-search-avg01.png)

Jest najpierw można zauważyć, że analizy dzienników przedstawiono trzy perspektyw: lista, na której przedstawiono przedstawiający rzeczywiste rekordów za wykresy; Tabela, która przedstawia widok tabelaryczny danych licznika wydajności. i metryki, który wskazuje wykresów liczników wydajności.

Na powyższej ilustracji istnieją dwa zestawy pola oznaczone wskazujące następujące czynności:

* Pierwszy zestaw identyfikuje nazwę licznika wydajności systemu Windows, nazwa obiektu i nazwę wystąpienia w filtrze kwerendy. Są to pola, które prawdopodobnie będą najczęściej używane jako aspekty/filtry
* **Równowartości** jest rzeczywista wartość licznika. W tym przykładzie wartość jest *75*.
* **TimeGenerated** jest 12:51 w formacie 24-godzinnym.

Oto widoku metryki na wykresie.

![wyszukiwanie start Śr.](./media/log-analytics-log-searches/oms-search-avg02.png)

Po informacji o wydajności kształtu rekordów, a o przeczytaj informacje o innych technik wyszukiwania można użyć miary Avg() agregacji ten typ danych liczbowych.

Poniżej przedstawiono prosty przykład:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![Wyszukaj samplevalue Śr.](./media/log-analytics-log-searches/oms-search-avg03.png)

W tym przykładzie należy wybrać wydajności łączny czas procesora CPU licznik i średnie przez komputer. Jeśli chcesz zawęzić wyniki ostatniego 6 godzin, można używać formantu filtr czasu lub określić w zapytaniu:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>Wyszukiwania przy użyciu polecenia miary za pomocą funkcji AVG.
* W polu zapytania wyszukiwania wpisz `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.

Można zagregować i skorelować danych *między* komputerów. Na przykład załóżmy, że masz zestawu hostów w jakieś farmy, gdzie każdy węzeł jest taki sam, inne jedno i tylko te same typ pracy oraz obciążenia powinien uwzględniać około. Można pobrać ich liczniki, które go w jednym z następującymi zapytań i uzyskać średnie dla całej farmy. Można uruchomić, wybierając komputery z następującym przykładzie:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Teraz, gdy masz komputery tylko chcesz wybrać dwa kluczowe wskaźniki wydajności (KPI): % użycia procesora CPU i % wolnego miejsca na dysku. Tak staje się części zapytania:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

Teraz można dodać komputery i liczniki z następującym przykładzie:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Ponieważ istnieje bardzo określonego zaznaczenia, **miar Avg()** polecenia przywrócić średnią nie na komputerze, ale w farmie, grupując przez CounterName. Na przykład:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

Zapewnia widok compact przydatne kilka kluczowych wskaźników wydajności w danym środowisku.

![Grupowanie avg wyszukiwania](./media/log-analytics-log-searches/oms-search-avg04.png)

Łatwo za pomocą zapytania wyszukiwania w pulpicie nawigacyjnym. Na przykład można zapisać kwerendę wyszukiwania i utworzyć pulpit nawigacyjny z niego o nazwie *kluczowych wskaźników wydajności sieci Web farmy*. Aby dowiedzieć się więcej na temat korzystania z pulpitów nawigacyjnych, zobacz [utworzyć niestandardowy pulpit nawigacyjny w analizy dzienników](log-analytics-dashboards.md).

![pulpit nawigacyjny avg wyszukiwania](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>Funkcja sum przy użyciu polecenia miary
Funkcja sum jest podobny do innych funkcji polecenia miary. Można zobaczyć przykład o sposobie używania funkcji sum w [Wyszukaj dzienniki IIS W3C w Microsoft Azure Operational Insights](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).

Można użyć Max() i Min() z liczb, daty i godziny oraz ciągów tekstowych. Ciągi tekstowe są posortowane alfabetycznie i uzyskać pierwszy i ostatni.

Jednak nie można używać Sum() z wyjątkiem pól wartości liczbowych. Dotyczy to również Avg().

### <a name="use-the-percentile-function-with-the-measure-command"></a>Użyj funkcji percentylu przy użyciu polecenia miary
Funkcji percentylu jest podobny do Avg() i Sum(), w tym można używać tylko jego pól wartości liczbowych. Można użyć dowolnego percentyl zakresu od 1 do 99 dla pola liczbowego. Możesz także używać ich obu **percentyl** i **pct** poleceń. Oto kilka przykładów:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Użyj where poleceń
W przypadku, gdy polecenie działa jak filtr, ale mogą być stosowane w potoku, aby dokładniej przefiltrować wyniki zagregowane, które zostały utworzone za pomocą polecenia miary — w przeciwieństwie do nieprzetworzone wyniki który są filtrowane na początku zapytania.

Na przykład:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

Można dodać inny potoku "|" znak gdzie i kiedy polecenia można uzyskać tylko komputerów, których średnie wykorzystanie Procesora jest powyżej 80%, z tym przykładem:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Jeśli znasz program Microsoft System Center — Operations Manager można traktować polecenia where w zakresie pakietu zarządzania. Przykład gdyby regułę, pierwsza część zapytania będą źródła danych i gdzie polecenie będzie wykrywanie warunku.

Zapytania można użyć jako Kafelek **Mój pulpit nawigacyjny**, jako monitor typu, aby zobaczyć, kiedy są nadmiernie używane procesory CPU komputera. Aby dowiedzieć się więcej na temat pulpitów nawigacyjnych, zobacz [utworzyć niestandardowy pulpit nawigacyjny w analizy dzienników](log-analytics-dashboards.md). Można również tworzyć i korzystać z pulpitów nawigacyjnych przy użyciu aplikacji mobilnej. Aby uzyskać więcej informacji, zobacz [aplikacji mobilnej OMS ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865). Kafelki dolnej dwóch następujących obrazu, zawiera monitor wyświetlane listy, a liczba. Zasadniczo zawsze ma numer zero i listy muszą być puste. W przeciwnym razie oznacza warunek alertu. W razie potrzeby można użyć jej do podglądu na komputery, które są wykorzystanie.

![mobilnego pulpitu nawigacyjnego](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>Użyj operatora w
*IN* operatora, wraz z *NOT IN* umożliwia użycie subsearches, które są wyszukiwania, które zawierają inne wyszukiwania jako argument. Znajdują się w nawiasy {}, w innym *głównej* lub *zewnętrzne* wyszukiwania. Wynik subsearch, często listę różne wyniki, następnie jest używany jako argumentu w jego wyszukiwania podstawowego.

Subsearches służy do dopasowania podzestawy danych nie opisywane bezpośrednio w wyrażeniu wyszukiwania, ale mogą być generowane z wyszukiwania. Na przykład, jeśli interesuje Cię przy użyciu jednego wyszukiwania, aby znaleźć wszystkie zdarzenia z *komputerów brakujących aktualizacji zabezpieczeń*, należy zaprojektować subsearch, która najpierw określi, że *komputerów brakujących aktualizacji zabezpieczeń* przed znajdzie zdarzenia należące do tych hostów.

Tak, można express *komputerów aktualnie brakuje wymaganych aktualizacji zabezpieczeń* z następujące zapytanie:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![W przykładzie wyszukiwania](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Po utworzeniu listy wyszukiwania można używać jako wewnętrzne wyszukiwanie ze źródłem listę komputerów w zewnętrznym wyszukiwania (podstawowe), który będzie szukać zdarzenia dla tych komputerów. W tym celu otaczającej wewnętrzny wyszukiwania w nawiasach klamrowych i podawania wyniki jako możliwych wartości filtru/pola w zewnętrznym wyszukiwania przy użyciu operatora IN. Zapytanie będzie wyglądać:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![W przykładzie wyszukiwania](./media/log-analytics-log-searches/oms-search-in02-revised.png)

Ponadto powiadomienia filtru czasu używany w wewnętrznym wyszukiwania, ponieważ oceny aktualizacji systemu tworzy migawkę wszystkich komputerów, co 24 godziny. Możesz wprowadzić wewnętrzny zapytania więcej lekkie i precyzyjne przez wyszukiwanie tylko jeden dzień. Zewnętrzne wyszukiwania użyje wybór czasu w interfejsie użytkownika podczas pobierania zdarzeń z ostatnich 7 dni. Zobacz [operatorów logicznych](#boolean-operators) uzyskać więcej informacji o czasie operatorów.

Ponieważ użytkownik naprawdę użyć tylko wyniki wyszukiwania wewnętrzny jako filtru wartości dla nich zewnętrzne, można nadal stosować poleceń w zewnętrznym wyszukiwania. Na przykład nadal można grupować powyższe zdarzenia z innego polecenia miary:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![W przykładzie wyszukiwania](./media/log-analytics-log-searches/oms-search-in03-revised.png)

Ogólnie rzecz biorąc ma wewnętrzny kwerendy można wykonać szybko, ponieważ analizy dzienników ma przekroczeń limitu czasu po stronie serwera, a także do zwrócenia z małej ilości wyników. Jeśli wewnętrzny zapytanie zwraca więcej wyników, pobiera skrócony listy wyników z potencjalnie może powodować zewnętrzne wyszukiwanie, aby zwracało niepoprawnych wyników.

Inna reguła jest, że wewnętrzny wyszukiwania należy obecnie podać *zagregowane* wyników. Innymi słowy, musi ona zawierać *miary* polecenia; nie można obecnie źródła danych pierwotnych wyników w zewnętrznym wyszukiwania.

Ponadto może istnieć tylko jeden operator IN oraz musi być ostatnim filtru w zapytaniu. Nie może mieć wielu operatorów w lub czy to zasadniczo uniemożliwia uruchomiony wielu subsearches: istotne jest tego tylko jeden wyszukiwania sub/wewnętrzny jest możliwe każdego zewnętrzne wyszukiwania.

Nawet w przypadku tych granic w umożliwia wielu rodzajów skorelowane wyszukiwania, a można zdefiniować podobny do grup, takich jak komputery, użytkowników lub pliki — niezależnie od pola danych zawierają. Poniżej przedstawiono więcej przykładów:

**Wszystkie aktualizacje Brak z komputerów z wyłączonym ustawienie automatycznej aktualizacji**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Wszystkie zdarzenia błędów z komputerów z uruchomionym programem SQL Server (=, którym dysponuje oceny SQL)**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Wszystkie zdarzenia zabezpieczeń z komputerów, które są kontrolerami domeny (=, którym dysponuje oceny AD)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Które do tych samych komputerów, których konta BACONLAND\jochan zalogował się na inne konta zalogowali się?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>Użyj polecenia różne
Jak wynika z nazwy, to polecenie Lista unikatowe wartości dla pola. Jest zaskakująco proste, ale bardzo przydatne. Taki sam można osiągnąć za pomocą polecenia count() miary również, jak pokazano poniżej.

```
Type=Event | Measure count() by Computer
```

![Przykład polecenia różne wyszukiwania](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Jeśli jednak wszystkie interesują Cię jest po prostu lista różne wartości i nie liczba dokumentów, których można podać wartości, a następnie DISTINCT czyszczący i łatwiejsze do odczytu danych wyjściowych i krótszy składni, jak pokazano poniżej.

```
Type=Event | Distinct Computer
```
![Przykład polecenia różne wyszukiwania](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>Użyj funkcji countdistinct przy użyciu polecenia miary
Funkcja countdistinct Zlicza unikatowe wartości w każdej grupie. Na przykład można użyć do zliczania unikatowych komputerów raportowania dla każdego typu:

```
* | measure countdistinct(Computer) by Type
```

![OMS-countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>Użyj polecenia interwał miary
Z niemal zbierania danych wydajności w czasie rzeczywistym, można zbierać i wizualizować wszystkie licznika wydajności w analizy dzienników. Wystarczy wprowadzić kwerendę **typu: wydajności** zwróci tysiące metryki wykresów na podstawie liczby liczników i serwerów w środowisku analizy dzienników. Z agregacją metryki na żądanie można przeglądać ogólną metryki wysokiego poziomu i nowości w bardziej szczegółowe dane potrzebne do środowiska.

Załóżmy, że chcesz wiedzieć, co to jest średnie wykorzystanie Procesora na wszystkich komputerach. Spojrzenie na średnią Procesora dla każdego komputera, może nie być pomocne ponieważ wyników może pobrać wygładzania. Umożliwia mu wgląd w szczegółowe informacje, można zagregować wyników czasu mniejsze fragmenty okna i wyglądu do szeregów czasowych w różnych wymiarach. Na przykład można wykonywać średniej godzinowej użycia procesora CPU na wszystkich komputerach w następujący sposób:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![Interwał średni miary](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Domyślnie te wyniki zostaną wyświetlone na wykresie wiersz interakcyjne wielu serii.  Ten wykres obsługuje serii przełączanie (z ponowne skalowanie osi y), powiększanie i kursora myszy.  Opcja wyświetlania tabeli jest wciąż dostępna na potrzeby przeglądania danych pierwotnych w razie potrzeby.

Można także grupować według innych pól. W tym przykładzie wyświetlane wszystkie liczniki % jednego określonego komputera, a chcę się dowiedzieć, co to jest percentylu co godzinę 70 co licznika:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Jedyną operacją, należy pamiętać, jest te zapytania nie są ograniczone do liczników wydajności. Można je zastosować do wszystkie metryki. W tym przykładzie wyświetlane dzienniki programu IIS W3C. Chcę wiedzieć, jaki jest maksymalny czas, który przejmuje on 5 minut do przetwarzania każdego żądania:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Użyj wielu wartości zagregowanych w jednym zapytaniu
Można określić wiele klauzul agregacji w poleceniu miary.  Każdy z nich można używać z aliasem niezależnie.  Jeśli nie zostanie podany alias wynikowa Nazwa pola będzie funkcji agregującej, który został użyty (tj. "avg(CounterValue)" jak avg(CounterValue)).

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS-multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Oto inny przykład:

 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać dodatkowe informacje dziennika wyszukiwania zobacz:

* Użyj [pola niestandardowe w analizy dzienników](log-analytics-custom-fields.md) rozszerzenie dziennik wyszukiwania.
* Przegląd [analizy dzienników dziennika odwołanie wyszukiwania](log-analytics-search-reference.md) do wyświetlenia wszystkich pól wyszukiwania i aspektów, które są dostępne w analizy dzienników.
