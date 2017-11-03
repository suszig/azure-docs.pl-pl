---
title: "Odwołanie wyszukiwania Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Analiza dzienników odwołanie wyszukiwania opisuje język wyszukiwania i udostępnia opcje ogólne składnię służy do wyszukiwania danych i filtrowania wyrażenia, aby zawęzić kryteria wyszukiwania."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 402615a2-bed0-4831-ba69-53be49059718
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 264ea071dc0b15964af07c68cbf0dee896b07a3e
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2017
---
# <a name="log-analytics-search-reference"></a>Odwołanie wyszukiwania analizy dzienników

>[!NOTE]
> W tym artykule opisano dziennik wyszukiwania przy użyciu języka zapytań starszej wersji w analizy dzienników.  Jeśli obszaru roboczego został uaktualniony do [języka zapytań nowe analizy dzienników](log-analytics-log-search-upgrade.md), a następnie należy zapoznać się [odwołanie językowe dla nowego języka](https://go.microsoft.com/fwlink/?linkid=856079).

W poniższej sekcji odwołania dotyczące języków wyszukiwania opisano opcje składni zapytania ogólne, można użyć podczas wyszukiwania danych i filtrowania wyrażenia, aby zawęzić kryteria wyszukiwania. Omówiono także używane do wykonania akcji na dane pobrane polecenia.

Informacje o polach zwracane w wynikach wyszukiwania i aspektów, które ułatwiają Dowiedz się więcej o kategoriach podobne danych, w [pola wyszukiwania i aspekt odwoływać sekcji](#search-field-and-facet-reference).

## <a name="general-query-syntax"></a>Składnia zapytania ogólne
Składnia kwerendy ogólne wygląda następująco:

```
filterExpression | command1 | command2 …
```

Wyrażenie filtru (`filterExpression`) definiuje "where" warunku dla zapytania. Polecenia dotyczą wyników zwróconych przez kwerendę. Wiele poleceń muszą być oddzielone znaku kreski pionowej (|).

### <a name="general-syntax-examples"></a>Przykłady składni ogólnej
Przykłady:

```
system
```

Ta kwerenda zwraca wyniki zawierające słowo *systemu* w każde pole, które zostały zindeksowane dla pełnotekstowego lub warunki wyszukiwania.

> [!NOTE]
> Nie wszystkie pola są indeksowane w ten sposób, ale najbardziej typowe tekstowej pola (na przykład nazwy i opisy) zazwyczaj są.
>
>

```
system error
```

Ta kwerenda zwraca wyniki zawierające słowa *systemu* i *błąd*.

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

Ta kwerenda zwraca wyniki zawierające słowa *systemu* i *błąd*. Następnie sortujące wyniki według *ManagementGroupName* pola (rosnąco), a następnie według *TimeGenerated* (w kolejności malejącej). Trwa tylko 10 pierwszych wyników.

> [!IMPORTANT]
> Wszystkie nazwy pól i wartości dla pola string i tekst jest uwzględniana wielkość liter.
>
>

## <a name="filter-expressions"></a>Wyrażenia filtru
Poniższe podpunkty opisują wyrażenia filtru.

### <a name="string-literals"></a>Literały ciągu
Literał ciągu jest dowolny ciąg, który nie jest rozpoznawany przez parser jako słowo kluczowe lub typu danych wstępnie zdefiniowane (na przykład, liczby lub daty).

Przykłady:

```
These all are string literals
```

To zapytanie szuka wyniki zawierające wystąpienia wszystkich pięciu wyrazów. Aby wykonać wyszukiwanie ciągu złożonych, ujmij literał w znaki cudzysłowu. Na przykład:

```
"Windows Server"
```

To polecenie zwróci tylko wyniki z dokładne dopasowania dla *systemu Windows Server*.

### <a name="numbers"></a>Numery
Analizator obsługuje dziesiętną liczbą całkowitą i składni liczb zmiennoprzecinkowych pola liczbowego.

Przykłady:

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="dates-and-times"></a>Daty i godziny
Każdy element danych w systemie ma *TimeGenerated* właściwość, która reprezentuje oryginalnego Data i godzina rekordu. Niektóre typy danych mogą mieć dodatkowe Data i godzina (na przykład *LastModified*).

Oś czasu **wykresu/godzina** selektora w Analiza dzienników Azure zawiera dystrybucji wyników wraz z upływem czasu (w zależności od bieżącego zapytania uruchamiane). To jest oparta na *TimeGenerated* pola. Data i godzina pola mają określony ciąg formatu, który może być używane w zapytaniach Aby ograniczyć zapytanie do określonego przedziału czasu. Umożliwia także składni w odwołaniu do przedziały czasu względną (na przykład "między 3 dni temu i 2 godz. temu").

Poniżej przedstawiono prawidłowe formularze składnię daty i godziny:

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


Na przykład:

```
TimeGenerated:2013-10-01T12:20
```

Poprzednie polecenie zwraca tylko rekordy z *TimeGenerated* wartości dokładnie 12:20 na 1 października 2013.

Analizator obsługuje również skrótu wartości daty/godziny, teraz. (Jest mało prawdopodobne, że da żadnych wyników, ponieważ nie była danych za pośrednictwem systemu to szybkie.)

Te przykłady są blokami konstrukcyjnymi do użycia dla dat względnych i bezwzględnych. W trzech kolejnych podsekcjach pojawi się, jak ich używać w filtrach bardziej zaawansowanych, wraz z przykładami, korzystających z zakresów względnej daty.

### <a name="datetime-math"></a>Data/Godzina matematyczne
Operatory matematyczne daty i godziny umożliwia przesunięcie lub zaokrąglanie wartości daty/godziny przy użyciu prostych obliczeń daty/godziny.

Składnia:

```
datetime/unit
```

```
datetime[+|-]count unit
```

| Operator | Opis |
| --- | --- |
| / |Zaokrągla wartość daty i godziny do określonej jednostki. Na przykład teraz / dzień zaokrągla bieżącej daty/godziny północy bieżącego dnia. |
| + lub - |Powoduje przesunięcie daty/godziny, o określoną liczbę jednostek. Na przykład teraz + 1 godzina powoduje przesunięcie bieżącej daty/godziny o godzinę wyprzedzeniem. 2013-10-01T12:00-10-DNIOWĄ przesunięcia wartości typu Date przez 10 dni. |

Operatory matematyczne daty i godziny mogą powiązane ze sobą. Na przykład:

```
NOW+1HOUR-10MONTHS/MINUTE
```

W poniższej tabeli wymieniono obsługiwane jednostek daty/godziny.

| Jednostka daty/godziny | Opis |
| --- | --- |
| YEAR, YEARS |Zaokrągla liczbę do bieżącego roku, lub przesunięcia przez określoną liczbę lat. |
| MIESIĄC, MIESIĘCY |Zaokrągla liczbę do bieżącego miesiąca lub przesunięcia przez określoną liczbę miesięcy. |
| DZIEŃ, DNI, DATA |Powoduje zaokrąglenie do bieżącego dnia, miesiąca lub przesunięcia przez określoną liczbę dni. |
| GODZINA, GODZINY |Zaokrągla liczbę do bieżącej godziny lub przesunięcia przez określoną liczbę godzin. |
| MINUTA, MINUT |Zaokrągla liczbę do bieżącej minuty lub przesunięcia przez określoną liczbę minut. |
| SECOND, SECONDS, |Drugi powoduje zaokrąglenie do bieżącej lub przesunięcia przez określoną liczbę sekund. |
| MILISEKUNDY MILISEKUND, MILLI, MILLIS |Zaokrągla liczbę do bieżącego milisekund lub przesunięcia przez określoną liczbę milisekund. |

### <a name="field-facets"></a>Aspekty pola
Za pomocą pola aspektów, można określić warunku wyszukiwania dla określonych pól i ich dokładne wartości. To różni się od Pisanie zapytań "wolne tekst" dla różnych warunków w całym indeksie. Ta technika w poprzednich przykładach kilka ma już widoczne. Poniżej przedstawiono bardziej skomplikowane.

**Składnia**

```
field:value
```

```
field=value
```

**Opis**

Wyszukuje określoną wartość w polu. Wartość może być literał ciągu, numer, lub Data i godzina.

Na przykład:

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

**Składnia**

*pole > wartość*

*pole < wartość*

*pole > = wartość*

*pole < = wartość*

*pole! = wartość*

**Opis**

Udostępnia porównania.

Na przykład:

```
TimeGenerated>NOW+2HOURS
```

**Składnia**

```
field:[from..to]
```

**Opis**

Umożliwia tworzenie aspektów zakresu.

Na przykład:

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="in"></a>W
**IN** — słowo kluczowe można wybrać z listy wartości. W zależności od składni, którego używasz może to być prosta lista wartości, który podasz lub listę wartości z agregacji.

Składnia 1:

```
field IN {value1,value2,value3,...}
```

Ta składnia umożliwia obejmują wszystkie wartości na liście proste.



Przykłady:

```
EventID IN {1201,1204,1210}
```

```
Computer IN {"srv01.contoso.com","srv02.contoso.com"}
```

Składnia 2:

```
(Outer query) (Field to use with inner query results) IN {Inner query | measure count() by (Field to send to outer query)} (rest  of outer query)  
```

Ta składnia umożliwia tworzenie agregacji. Lista wartości może następnie dostarczyć z tym agregacji w innym zewnętrzne wyszukiwania (podstawowe), sprawdzający zdarzenia z tych wartości. W tym celu otaczającej wewnętrzny wyszukiwania w nawiasach klamrowych i skierowanie wyniki jako możliwe wartości dla pola w zewnętrznym wyszukiwania za pomocą operatora IN.

Przykład wewnętrzny zapytania: *komputerów obecnie brakujących aktualizacji zabezpieczeń* z następującej kwerendy agregacji:

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

Końcowe kwerendę, która wyszukuje *wszystkie zdarzenia systemu Windows dla komputerów, w obecnie brakujących aktualizacji zabezpieczeń* jest podobny do następującego:

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="contains"></a>Contains
**Zawiera** — słowo kluczowe umożliwia filtrowanie rekordów z polem, który zawiera określony ciąg. To jest rozróżniana wielkość liter, działa tylko w przypadku pól ciągów i nie może zawierać żadnych znaków ucieczki.

Składnia:

```
field:contains("string")
```

Przykład:

```
Type:contains("Event")
```

To polecenie zwróci rekordy z typem, który zawiera ciąg "Event". Przykłady obejmują **zdarzeń**, **SecurityEvent**, i **ServiceFabricOperationEvent**.



### <a name="regular-expressions"></a>Wyrażenia regularne
Można określić warunku wyszukiwania dla pola z wyrażeniem regularnym, za pomocą **Regex** — słowo kluczowe. Pełny opis można używać w wyrażeniach regularnych składni, zobacz [za pomocą wyrażeń regularnych do filtrowania dziennika wyszukiwania analizy dzienników](log-analytics-log-searches-regex.md).

Składnia:

```
field:Regex("Regular Expression")
```

Przykład:

```
Computer:Regex("^C.*")
```

### <a name="logical-operators"></a>Operatory logiczne
Języki zapytań obsługuje operatory logiczne (*i*, *lub*, i *nie*) i ich aliasy w stylu języka C (*&&*,  *||* , i *!*odpowiednio). Można użyć nawiasów do grupowania tych operatorów.

Przykłady:

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

Operator logiczny dla argumentów filtru najwyższego poziomu można pominąć. W takim przypadku AND operator zakłada, że.

| Wyrażenie filtru | Wartość równoważna wartości |
| --- | --- |
| Błąd systemu: |System i błędów |
| System "Windows Server" lub ważność: 1 |System i ("Windows Server" lub ważność: 1) |

### <a name="wildcarding"></a>Symbole wieloznaczne
Język zapytań obsługuje korzystanie z ( \* ) znaku do reprezentowania jeden lub więcej znaków wartości w zapytaniu.

Przykład:

 Znajdź wszystkie komputery z "SQL" w nazwie, takie jak "Redmond SQL".

```
Type=Event Computer=*SQL*
```

> [!NOTE]
> W tej chwili nie można używać symboli wieloznacznych w cudzysłowach. Na przykład komunikat `"*This text*"` uwzględnia (\*) używane jako literału (\*) znaków.


## <a name="commands"></a>Polecenia


Polecenia dotyczą wyników zwróconych przez kwerendę. Umożliwia zastosowanie polecenia do wyników pobrane znaku kreski pionowej (|). Wiele poleceń muszą być oddzielone znaku kreski pionowej.

> [!NOTE]
> Nazwy poleceń można pisać w wielkie lub małe litery, w odróżnieniu od nazwy pól i danych.
>
>

### <a name="sort"></a>Sortuj
Składnia:

    sort field1 asc|desc, field2 asc|desc, …

Sortuj wyniki według określonego pola. Sufiks asc/desc sortować wyniki w kolejności rosnącej lub malejącej jest opcjonalna. Jeśli zostanie pominięty, *asc* zakłada, że kolejność sortowania. Dla **TimeGenerated** pola *desc* zakłada się porządek sortowania, więc najpierw zwraca najnowsze wyniki domyślnie.

### <a name="toplimit"></a>TOP/Limit
Składnia:

    top number


    limit number
Ogranicza odpowiedź na pierwsze N wyników.

Przykład:

    Type:Alert errors detected | top 10

Zwraca górny 10 pasujących wyników.

### <a name="skip"></a>Pomiń
Składnia:

    skip number

Pomija liczbę wyników na liście.

Przykład:

    Type:Alert errors detected | top 10 | skip 200

Zwraca górny 10 pasujących wyników zaczynając od wyniku 200.

### <a name="select"></a>Wybierz pozycję
Składnia:

    select field1, field2, ...

Ogranicza wyniki do pola, które można wybrać.

Przykład:

    Type:Alert errors detected | select Name, Severity

Ogranicza pola zwrócone wyniki *nazwa* i *ważność*.

### <a name="measure"></a>Miary
*Miary* polecenie służy do stosowania funkcji statystycznych do wyników wyszukiwania raw. Jest to bardzo przydatne uzyskać *Grupuj według* widoków danych. Po użyciu polecenia miary, analizy dzienników zostanie wyświetlona tabela z wyników zagregowany.

**Składnia:**

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



Agreguje wyniki według *groupField*i oblicza wartości zagregowane miary za pomocą *aggregatedField*.

| Funkcja statystyczne miary | Opis |
| --- | --- |
| *Funkcja aggregateFunction* |Nazwa funkcji agregującej (bez uwzględniania wielkości liter). Obsługiwane są następujące funkcje agregujące: COUNT, MAX, MIN, SUM, AVG, STDDEV, COUNTDISTINCT, percentyl ## lub PCT ## (## jest liczbą zakresu od 1 do 99). |
| *aggregatedField* |Pole jest agregowanie. To pole jest opcjonalne dla funkcji agregującej COUNT, ale musi być istniejącym polem liczbowe SUM, MAX, MIN, AVG, STDDEV, percentyl ## lub PCT ## (## jest liczbą zakresu od 1 do 99). AggregatedField mogą również być **Rozszerz** obsługiwanych funkcji. |
| *fieldAlias* |(Opcjonalnie) alias dla obliczonej wartości zagregowanej. Jeśli nie zostanie określony, nazwa pola jest **AggregatedValue**. |
| *groupField* |Nazwa pola, które zestaw wyników są grupowane według. |
| *Interwał* |Przedział czasu, w formacie:**nnnNAME**. **nnn**jest to liczba dodatnia liczba całkowita. **Nazwa** jest nazwą interwału. Nazwy obsługiwany interwał jest uwzględniana wielkość liter i obejmują: MILISEKUNDY [S] sekundy [S] MINUTĘ [S] z godziny [S] dnia [S] [S] miesiąca i roku [S]. |

Opcja zakresu jest możliwe tylko w polach grupy daty/godziny (takich jak *TimeGenerated* i *TimeCreated*). Obecnie ta wartość nie jest wymuszana przez usługę, ale pole bez daty i godziny przekazywane do wewnętrznej powoduje błąd w czasie wykonywania. Po zaimplementowaniu sprawdzanie poprawności schematu interfejsu API usługi odrzuca zapytań, które pola bez daty/godziny na użytek interwał agregacji. Bieżący *miary* implementacja obsługuje grupowania interwał funkcji agregacji.

Jeśli w klauzuli BY zostanie pominięty, ale podano interwału (jako drugi składni), *TimeGenerated* pola przyjęto, że domyślnie.

Przykłady:

**Przykład 1**

    Type:Alert | measure count() as Count by ObjectId

Grupuje alertów według *ObjectID*i oblicza liczbę alertów dla każdej grupy. Wartości zagregowane są zwracane jako *liczba* pola (alias).

**Przykład 2**

    Type:Alert | measure count() interval 1HOUR

Grupy alerty przez 1 godzinę za pomocą *TimeGenerated* pól i zwraca liczbę alertów w każdym interwale.

**Przykład 3**

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

Co w poprzednim przykładzie, ale z aliasem pola zagregowane (*AlertsPerHour*).

**Przykład 4**

    * | Miara count() przez 5DAYS interwał TimeCreated

Grupuje wyniki według 5 dni przy użyciu *TimeCreated* pól i zwraca liczbę wyników w każdym interwale.

**Przykład 5**

    Type:Alert | measure max(Severity) by WorkflowName

Grupuje alerty według nazwy obciążenie i zwraca wartość maksymalną alert o ważności dla każdego przepływu pracy.

**Przykład 6**

    Type:Alert | measure min(Severity) by WorkflowName

Co w poprzednim przykładzie, ale z *min* zagregowane funkcji.

**Przykład 7**

    Type:Perf | measure avg(CounterValue) by Computer

Grupuje wydajności przez komputer, a następnie oblicza średnią (średni).

**Przykład 8**

    Type:Perf | measure sum(CounterValue) by Computer

Takie same, co w poprzednim przykładzie, ale używa *suma*.

**Przykład 9**

    Type:Perf | measure stddev(CounterValue) by Computer

Takie same, co w poprzednim przykładzie, ale używa *stddev*.

**Przykład 10**

    Type:Perf | measure percentile70(CounterValue) by Computer

Takie same, co w poprzednim przykładzie, ale używa *percentile70*.

**Przykład 11**

    Type:Perf | measure pct70(CounterValue) by Computer

Takie same, co w poprzednim przykładzie, ale używa *pct70*. Należy pamiętać, że *PCT ##* jest tylko aliasu dla *percentyl ##* funkcji.

**Przykład 12**

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

Grupuje wydajności najpierw przez komputer, a następnie CounterName i oblicza średnią (średni).

**Przykład 13**

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

Pobiera top pięć przepływy pracy z maksymalną liczbę alertów.

**Przykład 14**

    * | Miara countdistinct(Computer) według typu

Liczy unikatowe komputery raportowania dla każdego typu.

**Przykład 15**

    * | Miara countdistinct(Computer) interwał 1 godzina

Liczy unikatowe komputery raportowania co godzinę.

**Przykład 16**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

Grupuje % czasu procesora przez komputer i zwraca średnią dla każdej godziny.

**Przykład 17**

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

Grupuje W3CIISLog przez metodę i zwraca maksymalny co 5 minut.

**Przykład 18**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

Grupuje czas procesora (%) na komputerze i zwraca, średnia, 75 percentyl maksymalne i minimalne dla każdej godziny.

**Przykład 19**

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

Grupuje % czasu procesora najpierw przez komputer, a następnie według nazwy wystąpienia i zwraca, średnia, 75 percentyl maksymalne i minimalne dla każdej godziny.

**Przykład 20**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

Oblicza maksymalną na minutę dla każdego dysku operacji zapisu na dysku na tym komputerze.

### <a name="where"></a>gdzie
Składnia:

```
**where** AggregatedValue>20
```

Można używać tylko po *miary* polecenie, aby dokładniej przefiltrować zagregowane powoduje, że *miary* opracowała funkcji agregacji.

Przykłady:

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)



### <a name="dedup"></a>Funkcja deduplikacji
Składnia:

    Dedup FieldName

Zwraca pierwszy dokument znaleziono dla każdej unikatowej wartości tego pola.

Przykład:

    Type=Event | Dedup EventID | sort TimeGenerated DESC

W tym przykładzie zwraca jedno zdarzenie (najnowsze zdarzenie) na identyfikator zdarzenia.

### <a name="join"></a>Join
Dołącza wyniki dwa zapytania do utworzenia pojedynczy zestaw wyników.  Obsługuje wiele typów sprzężeń opisane w tabeli poniżej.

| Typ sprzężenia | Opis |
|:--|:--|
| wewnętrzny | Zwraca tylko rekordów o pasującej wartości w obu zapytania. |
| zewnętrzne | Zwraca wszystkie rekordy z obu zapytań.  |
| Po lewej  | Zwraca wszystkie rekordy z lewym zapytań i pasujące rekordy z prawej zapytania. |


- Sprzężenia nie obsługują aktualnie zapytań, które obejmują **IN** — słowo kluczowe, **miary** polecenia lub **Rozszerz** polecenia, jeśli elementem docelowym pola z prawej kwerendy.
- Obecnie można uwzględnić tylko jedno pole sprzężenia.
- Pojedynczy wyszukiwania nie może zawierać więcej niż jedno połączenie.

**Składnia**

```
<left-query> | JOIN <join-type> <left-query-field-name> (<right-query>) <right-query-field-name>
```

**Przykłady**

Aby zilustrować typów sprzężeń innego, należy rozważyć dołączenie typu danych zbieranych z dziennik niestandardowy o nazwie MyBackup_CL z pulsu dla każdego komputera.  Te typy danych mają następujące dane.

`Type = MyBackup_CL`

| TimeGenerated | Computer (Komputer) | LastBackupStatus |
|:---|:---|:---|
| 2017-4/20 01:26:32.137 AM | SRV01.contoso.com | Powodzenie |
| 2017-4/20 02:13:12.381 AM | SRV02.contoso.com | Powodzenie |
| 2017-4/20 02:13:12.381 AM | srv03.contoso.com | Niepowodzenie |

`Type = Hearbeat`(Tylko podzestaw pokazane pola)

| TimeGenerated | Computer (Komputer) | ComputerIP |
|:---|:---|:---|
| 2017-4/21 12:01:34.482 PM. | SRV01.contoso.com | 10.10.100.1 |
| 2017-4/21 12:02:21.916 PM. | SRV02.contoso.com | 10.10.100.2 |
| 2017-4/21 12:01:47.373 PM. | srv04.contoso.com | 10.10.100.4 |

#### <a name="inner-join"></a>sprzężenie wewnętrzne

`Type=MyBackup_CL | join inner Computer (Type=Heartbeat) Computer`

Zwraca następujące rekordy, jeśli pole komputera odpowiada dla obu typów danych.

| Computer (Komputer)| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| SRV01.contoso.com | 2017-4/20 01:26:32.137 AM | Powodzenie | 2017-4/21 12:01:34.482 PM. | 10.10.100.1 | Pulsu |
| SRV02.contoso.com | 2017-4/20 02:13:12.381 AM | Powodzenie | 2017-4/21 12:02:21.916 PM. | 10.10.100.2 | Pulsu |


#### <a name="outer-join"></a>sprzężenie zewnętrzne

`Type=MyBackup_CL | join outer Computer (Type=Heartbeat) Computer`

Zwraca następujące rekordy dla obu typów danych.

| Computer (Komputer)| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| SRV01.contoso.com | 2017-4/20 01:26:32.137 AM | Powodzenie  | 2017-4/21 12:01:34.482 PM. | 10.10.100.1 | Pulsu |
| SRV02.contoso.com | 2017-4/20 02:14:12.381 AM | Powodzenie  | 2017-4/21 12:02:21.916 PM. | 10.10.100.2 | Pulsu |
| srv03.contoso.com | 2017-4/20 01:33:35.974 AM | Niepowodzenie  | 2017-4/21 12:01:47.373 PM. | | |
| srv04.contoso.com |                           |          | 2017-4/21 12:01:47.373 PM. | 10.10.100.2 | Pulsu |



#### <a name="left-join"></a>Lewe sprzężenie

`Type=MyBackup_CL | join left Computer (Type=Heartbeat) Computer`

Zwraca następujące rekordy z MyBackup_CL żadnych pól zgodnych z pulsu.

| Computer (Komputer)| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| SRV01.contoso.com | 2017-4/20 01:26:32.137 AM | Powodzenie | 2017-4/21 12:01:34.482 PM. | 10.10.100.1 | Pulsu |
| SRV02.contoso.com | 2017-4/20 02:13:12.381 AM | Powodzenie | 2017-4/21 12:02:21.916 PM. | 10.10.100.2 | Pulsu |
| srv03.contoso.com | 2017-4/20 02:13:12.381 AM | Niepowodzenie | | | |


### <a name="extend"></a>Rozszerzanie
Służy do tworzenia środowiska wykonawczego pola w zapytaniach. Należy pamiętać, że czasu wykonywania nie można używać pól przy użyciu polecenia miary do wykonania agregacji.

**Przykład 1**

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
Pokazuje ważone wynik zalecenie zalecenia SQL do oceny.

**Przykład 2**

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
Zawiera wartość licznika w KB/s, zamiast bajtów.

**Przykład 3**

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
Skaluje wartość WireData TotalBytes w taki sposób, że wszystkie wyniki należą do zakresu od 0 do 100.

**Przykład 4**

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
Znaczniki wartości liczników wydajności mniej niż 50 procent jako niski, a inne jako wysoki.

**Przykład 5**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
Oblicza maksymalną na minutę dla każdego dysku operacji zapisu na dysku na tym komputerze.

**Obsługiwane funkcje**

| Funkcja | Opis | Przykłady składni |
| --- | --- | --- |
| ABS |Zwraca wartość bezwzględną liczby określonej wartości lub funkcji. |`abs(x)` <br> `abs(-5)` |
| ACOS |Zwraca arcus cosinus wartości lub funkcji. |`acos(x)` |
| i |Zwraca wartość true tylko wtedy, gdy wszystkich argumentów zwrócić wartość true. |`and(not(exists(popularity)),exists(price))` |
| ASIN |Zwraca arcus sinus wartości lub funkcji. |`asin(x)` |
| ATAN |Zwraca arcus tangens wartości lub funkcji. |`atan(x)` |
| ATAN2 |Zwraca kąt, wynikające z konwersję prostokątna współrzędne x, y na Współrzędne biegunowe. |`atan2(x,y)` |
| cbrt — |Główny moduł. |`cbrt(x)` |
| ceil |Zaokrągla w górę do liczby całkowitej. |`ceil(x)`  <br> `ceil(5.6)`Zwraca 6 |
| COS |Zwraca cosinus kąta. |`cos(x)` |
| COSH |Zwraca cosinus hiperboliczny kąta. |`cosh(x)` |
| DEF |Skrót dla domyślnego. Zwraca wartość "pola". Jeśli pole nie istnieje, zwraca wartość domyślna określona i zwraca pierwszą wartość gdzie: `exists()==true`. |`def(rating,5)`. Ta funkcja def() zwraca klasyfikacji lub jeśli klasyfikacji nie określono dokumentu, zwraca 5. <br> `def(myfield, 1.0)`jest odpowiednikiem `if(exists(myfield),myfield,1.0)`. |
| stopni |Konwertuje wartość w radianach stopni. |`deg(x)` |
| DIV |`div(x,y)`dzieli x przez y. |`div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| dystr. |Zwraca odległość między dwoma wektorami, (punkty) w n wymiarowej miejsca. Przejście w prawo, a także wystąpień ValueSource dwóch lub więcej i oblicza odległość między dwoma wektorami. Każdy ValueSource musi być liczbą. Musi być parzystą liczbą wystąpień ValueSource przekazano, a metoda przyjęto założenie, że pierwszej połowy reprezentują wektor pierwszej i drugiej połowie reprezentują wektor drugiego. |`dist(2, x, y, 0, 0)`Oblicza odległość euklidesowa między (0,0) i (x, y) dla każdego dokumentu. <br> `dist(1, x, y, 0, 0)`Oblicza odległość Manhattan (taxicab) między (0,0) i (x, y) dla każdego dokumentu. <br> `dist(2,,x,y,z,0,0,0)`Euklidesowa odległość między (0,0,0) i (x, y, z) dla każdego dokumentu.<br>`dist(1,x,y,z,e,f,g)`Manhattan odległość między (x, y, z) i (e, f, g), gdzie każda litera jest nazwą pola. |
| istnieje |Zwraca wartość TRUE, jeśli element członkowski pola istnieje. |`exists(author)`Zwraca wartość TRUE dla każdego dokumentu, który ma wartość w polu "Autor".<br>`exists(query(price:5.00))`Zwraca wartość TRUE, jeśli pasuje do "cena", "5.00". |
| EXP |Zwraca liczbę firmy Eulera podniesione do potęgi x. |`exp(x)` |
| FLOOR |Zaokrągla liczbę w dół do liczby całkowitej. |`floor(x)`  <br> `floor(5.6)`Zwraca 5 |
| hypo |Zwraca sqrt(sum(pow(x,2),pow(y,2))) bez pośredniego przepełnienie lub niedomiar. |`hypo(x,y)`  <br> ` |
| Jeśli |Umożliwia funkcja warunkowy zapytania. W `if(test,value1,value2)`, test jest lub odwołuje się do wartość logiczna lub wyrażenie, które zwraca wartość logiczną (TRUE lub FALSE). `value1`jest zwracana wartość przez funkcję Jeśli test zwraca wartość TRUE. `value2`jest zwracana wartość przez funkcję Jeśli test zwraca wartość FALSE. Wyrażenie może być dowolnej funkcji, które generuje wartości logiczne. Można także funkcji zwracającej wartości liczbowe, w których wielkość wartość 0 jest interpretowana jako wartość false lub w których wielkość pustego ciągu zwracanie ciągów, jest interpretowany jako FAŁSZ. |`if(termfreq(cat,'electronics'),popularity,42)`Ta funkcja sprawdza każdego dokumentu, aby zobaczyć, czy zawiera termin "electronics" w polu cat. Jeśli tak, jest zwracana wartość pola popularność. W przeciwnym razie jest zwracana wartość 42. |
| liniowy |Implementuje `m*x+c`, gdzie m i c są stałe i x jest dowolną funkcją. Jest to równoważne `sum(product(m,x),c)`, ale nieco bardziej wydajne zaimplementowanego jako jednej funkcji. |`linear(x,m,c) linear(x,2,4)`Zwraca`2*x+4` |
| ln |Zwraca logarytm naturalny określonej funkcji. |`ln(x)` |
| Dziennik |Zwraca dziennik określona funkcja o podstawie 10. |`log(x)   log(sum(x,100))` |
| mapy |Mapuje wartości wejściowe funkcji x, spełniających kryteria min i max, włącznie z określoną docelową. Argumenty min i max muszą być stałymi. Docelowy argumentów i domyślne może być stałe lub funkcji. Jeśli wartość x nie należy do zakresu od min i max, następnie jest zwracana wartość x albo domyślną wartość zwracana, gdy określona jako 5. argument. |`map(x,min,max,target) map(x,0,0,1)`Zmienia wartości 0 lub 1. Może to być przydatne podczas obsługi wartości domyślnej 0.<br> `map(x,min,max,target,default)    map(x,0,100,1,-1)`Zmiany wartości zakresu od 0 do 1 do 100, a wszystkie inne wartości -1.<br>  `map(x,0,100,sum(x,599),docfreq(text,solr))`Zmiany wartości od 0 do 100 x + 599 oraz innych wartości częstotliwości termin "solr, w polu tekst. |
| Maksymalna |Zwraca maksymalną wartość liczbową wielu zagnieżdżonych funkcji lub stałe, które są określane jako argumenty: `max(x,y,...)`. Max — funkcja może być przydatne przy "bottoming out" innej funkcji lub pola dla niektórych określone stała.  Użyj `field(myfield,max)` składni wybierania maksymalną wartość jednego pola wielowartościowe. |`max(myfield,myotherfield,0)` |
| min. |Zwraca minimalną wartość liczbową wiele funkcji zagnieżdżonych stałych, które są określane jako argumenty: `min(x,y,...)`. Można też używany w celu tworzenia "górna granica" dla funkcji przy użyciu stałej funkcji min. Użyj `field(myfield,min)` składni wybierania minimalną wartość jednego pola wielowartościowe. |`min(myfield,myotherfield,0)` |
| mod |Oblicza resztę funkcja x t funkcji. |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))` |
| MS |Zwraca milisekund różnica między argumenty. Daty są względem systemu Unix lub POSIX epoki czasu, północy, 1 stycznia 1970 UTC. Argumenty może być nazwa indeksowanego TrieDateField lub matematyczne daty na podstawie daty stałej lub teraz. `ms()`jest odpowiednikiem `ms(NOW)`, liczba milisekund od epoka. `ms(a)`Zwraca liczbę milisekund od epoka reprezentujący argument. `ms(a,b)`Zwraca liczbę milisekund, że b występuje przed, która jest `a - b`. |`ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| nie |Logicznie zanegowaną wartość opakowana funkcja. |`not(exists(author))`Wartość TRUE tylko wtedy, gdy `exists(author)` ma wartość false. |
| lub |Rozłączenie logiczne. |`or(value1,value2)`Wartość TRUE, jeśli wartość1 lub wartość2 ma wartość true. |
| Pow |Uruchamia określony podstawowy do określonej potęgi. `pow(x,y)`zgłasza x do potęgi y. |`pow(x,y)`<br>`pow(x,log(y))`<br>`pow(x,0.5)`Taki sam jak sqrt. |
| Produktu |Zwraca iloczyn wiele wartości lub funkcji, które są określone w postaci listy rozdzielanej przecinkami. `mul(...)`może także służyć jako alias dla tej funkcji. |`product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| recip |Wykonuje funkcję wzajemnego z `recip(x,m,a,b)` implementacja `a/(m*x+b)`, gdzie m,, b są stałe i x jest dowolną funkcję arbitralnie złożonych. Gdy i b są równe i x > = 0, ta funkcja posiada maksymalną wartość 1, wyeliminować x zwiększa. Zwiększenie wartości i b wyników razem w przepływie całą funkcję do płaska część krzywej. Te właściwości ułatwia tę funkcję nadaje się doskonale dla zwiększania nowych dokumentów, jeśli x to `rord(datefield)`. |`recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| rad |Konwertuje stopnie na radiany. |`rad(x)` |
| Drukowanie |Zaokrąglenie do najbliższej liczby całkowitej. |`rint(x)`  <br> `rint(5.6)`Zwraca 6 |
| SIN |Zwraca sinus kąta. |`sin(x)` |
| SINH |Zwraca sinus hiperboliczny kąta. |`sinh(x)` |
| Skali |Skalowanie wartości funkcji x taki sposób, że należą one między określonym minTarget i maxTarget włącznie. Bieżąca implementacja przechodzi przez wszystkie wartości funkcja uzyskanie min i max, więc można wybrać odpowiedniej skali. Bieżąca implementacja nie rozróżnia po usunięciu dokumenty lub dokumenty, niezawierające wartości. W tych przypadkach używa 0.0 wartości. Oznacza to, że jeśli wartości są zwykle wszystkie większa niż 0,0, co może nadal końcowa 0,0 jako minimalna wartość do zamapowania z. W takich przypadkach odpowiednie `map()` funkcji można jako obejście zmienić 0,0 wartość w zakresie prawdziwe, jak pokazano poniżej:`scale(map(x,0,0,5),1,2)` |`scale(x,minTarget,maxTarget)`<br>`scale(x,1,2)`Skaluje wartości x, w taki sposób, że wszystkie wartości należą do zakresu od 1 i 2 włącznie. |
| SQRT |Zwraca pierwiastek kwadratowy z określonej wartości lub funkcji. |`sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| strdist |Oblicza odległość między dwa ciągi. Używa interfejsu StringDistance moduł sprawdzania pisowni Lucene i obsługuje wszystkie dostępne w tym pakiecie implementacji. Umożliwia również aplikacji w ich własnych za pośrednictwem zasobów w Solr możliwości ładowanie do. przyjmuje strdist `(string1, string2, distance measure)`. Możliwe wartości dla miary odległości to:<ul><li>jw: Winklera Jaro</li><li>Edytuj: Levenstein lub edycji odległości</li><li>ngram: NGramDistance, jeśli jest określony, można opcjonalnie Przekaż rozmiar ngram zbyt. Domyślna to 2.</li><li>FQN: W pełni kwalifikowaną nazwę implementację interfejsu StringDistance klasy. Musi mieć konstruktora nie argumentu.</li></ul> |`strdist("SOLR",id,edit)` |
| Sub |Zwraca x i y, z `sub(x,y)`. |`sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| Suma |Zwraca sumę wiele wartości lub funkcji, które są określone w postaci listy rozdzielanej przecinkami. `add(...)`może być używany jako alias dla tej funkcji. |`sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)` |
| termfreq |Zwraca liczbę razy, termin zostanie wyświetlony w polu dla tego dokumentu. |termfreq(text,'memory') |
| tan |Zwraca tangens kąta. |`tan(x)` |
| TANH |Zwraca tangens hiperboliczny kąta. |`tanh(x)` |

## <a name="search-field-and-facet-reference"></a>Odwołanie wyszukiwania pola i aspektu
Korzystając z wyszukiwania dziennika można znaleźć dane, wyniki są wyświetlane różne pola i aspektami. Niektóre informacje mogą być niewidoczne bardzo opisowy. Skorzystaj z poniższych informacji, aby ułatwić zrozumienie wyników.

| Pole | Typ wyszukiwania | Opis |
| --- | --- | --- |
| Dla identyfikatora dzierżawcy |Wszystkie |Używane do danych partycji. |
| TimeGenerated |Wszystkie |Używana do osi czasu, timeselectors (wyszukiwania i w innych ekranów). Reprezentuje on, gdy element danych został wygenerowany (zwykle w agencie). Czas jest wyrażona w formacie ISO i jest zawsze UTC. W przypadku typów, które są oparte na istniejących Instrumentacji (to znaczy zdarzenia w dzienniku) jest to zazwyczaj rejestrującej wpisu/wiersz/rekord dziennika w czasie rzeczywistym. W przypadku niektórych typów, które są tworzone za pomocą pakietów administracyjnych lub w chmurze (na przykład zalecenia lub alerty) czas reprezentuje inną. Jest to nowy element danych migawka konfiguracji pewnego rodzaju zostały zebrane, lub zalecenie/alert został utworzony na jego podstawie. |
| Identyfikator zdarzenia |Wydarzenie |Identyfikator zdarzenia w dzienniku zdarzeń systemu Windows. |
| Dziennik zdarzeń |Wydarzenie |Dziennik zdarzeń, w którym zdarzenie zostało zarejestrowane przez system Windows. |
| EventLevelName |Wydarzenie |Krytyczne/ostrzeżenia/informacji/Powodzenie |
| eventLevel |Wydarzenie |Wartości liczbowej dla krytycznych/ostrzeżenia/informacji/Powodzenie (Użyj EventLevelName łatwiejsze/bardziej czytelny zapytań). |
| SourceSystem |Wszystkie |Gdy dane pochodzą z (w postaci liczby dołączyć tryb do usługi). Przykłady programu Microsoft System Center Operations Manager i usługi Azure Storage. |
| Nazwa obiektu |PerfHourly |Nazwa obiektu wydajności systemu Windows. |
| InstanceName |PerfHourly |Nazwa wystąpienia liczników wydajności systemu Windows. |
| CounteName |PerfHourly |Nazwa licznika wydajności systemu Windows. |
| ObjectDisplayName |PerfHourly, ConfigurationObjectProperty ConfigurationAlert, ConfigurationObject, |Nazwa wyświetlana obiektu objęci zasadę zbierania danych wydajności w programie Operations Manager. Mogą być również nazwę wyświetlaną obiektu wykrytych przez usługę Operational Insights lub względem został wygenerowany alert. |
| RootObjectName |PerfHourly, ConfigurationObjectProperty ConfigurationAlert, ConfigurationObject, |Wyświetlana nazwa nadrzędnego elementu nadrzędnego obiektu objęci zasadę zbierania danych wydajności w programie Operations Manager (w relacji hostingu podwójny). Mogą być również nazwę wyświetlaną obiektu wykrytych przez usługę Operational Insights lub względem został wygenerowany alert. |
| Computer (Komputer) |Większość typów |Nazwa komputera należącego do danych. |
| DeviceName |ProtectionStatus |Nazwa komputera danych należy do (tak samo, jak "Komputer"). |
| DetectionId |ProtectionStatus | |
| ThreatStatusRank |ProtectionStatus |Pozycja stan zagrożenie jest numeryczna reprezentacja stanu zagrożeń. Podobnie jak kody odpowiedzi HTTP, klasyfikację ma luki pomiędzy numery (dlatego zagrożenia nie wynosi 150 i nie 100 lub 0), pozostawiając miejsca, aby dodać nowe stany. Dla zbiorczego stanu zagrożeń i stanu ochrony jest do wyświetlenia najgorszy stan, który komputer był w trakcie wybrany okres czasu. Numery rank różne stany, więc można wyszukać rekord o najwyższym numerze. |
| ThreatStatus |ProtectionStatus |Opis ThreatStatus, mapuje 1:1 z ThreatStatusRank. |
| TypeofProtection |ProtectionStatus |Produkt ochrony przed złośliwym kodem, który zostanie wykryty na komputerze: Brak, narzędzie do usuwania złośliwego oprogramowania firmy Microsoft, Forefront itd. |
| ScanDate |ProtectionStatus | |
| SourceHealthServiceId |ProtectionStatus, RequiredUpdate |Identyfikator usługi badania kondycji agenta na tym komputerze. |
| HealthServiceId |Większość typów |Identyfikator usługi badania kondycji agenta na tym komputerze. |
| ManagementGroupName |Większość typów |Nazwa grupy zarządzania agentami dołączone do programu Operations Manager. W przeciwnym razie wartość jest równa null/puste. |
| Typ obiektu |ConfigurationObject |Typ (jak klasa/typu pakiet administracyjny programu Operations Manager) dla tego obiektu, wykryte przez oceny konfiguracji analizy dzienników. |
| UpdateTitle |RequiredUpdate |Nazwa znaleziono aktualizacji nie jest zainstalowany. |
| PublishDate |RequiredUpdate |Jeśli aktualizacja została opublikowana w witrynie Microsoft Update. |
| Serwer |RequiredUpdate |Nazwa komputera danych należy do (tak samo, jak "Komputer"). |
| Product (Produkt) |RequiredUpdate |Aktualizacja dotyczy produktu. |
| UpdateClassification |RequiredUpdate |Typ aktualizacji (na przykład pakiet zbiorczy aktualizacji lub dodatek service pack). |
| KBID |RequiredUpdate |Identyfikator artykułu KB, który opisuje najlepsze praktyki lub aktualizacji. |
| WorkflowName |ConfigurationAlert |Nazwa reguły lub monitora wytworzonego alertu. |
| Ważność |ConfigurationAlert |Ważność alertu. |
| Priorytet |ConfigurationAlert |Priorytet alertu. |
| IsMonitorAlert |ConfigurationAlert |Ten alert został wygenerowany przez monitor (true) czy zasadę (false)? |
| AlertParameters |ConfigurationAlert |Plik XML z parametrami alertu analizy dzienników. |
| Kontekst |ConfigurationAlert |Kod XML w kontekście alertu analizy dzienników. |
| Obciążenie |ConfigurationAlert |Technologia lub obciążenia, którego alert dotyczy. |
| AdvisorWorkload |Zalecenie |Technologia lub obciążenia, odnoszący się do zalecenia. |
| Opis |ConfigurationAlert |Opis alertu (short). |
| DaysSinceLastUpdate |UpdateAgent |Ile dni temu (względem TimeGenerated tego rekordu) tego agenta zainstalować aktualizację z usługi Windows Server Update Service (WSUS) lub Microsoft Update? |
| DaysSinceLastUpdateBucket |UpdateAgent |Oparte na DaysSinceLastUpdate, kategoryzacji w przedziałów czasu z jak dawno temu komputera ostatniej instalacji żadnych aktualizacji usług WSUS/usługi Microsoft Update. |
| AutomaticUpdateEnabled |UpdateAgent |Sprawdzanie aktualizacji automatycznych włączone lub wyłączone dla tego agenta? |
| AutomaticUpdateValue |UpdateAgent |Automatyczna aktualizacja sprawdza, czy ustawiono automatycznie pobrać i zainstalować, tylko pobierania lub tylko sprawdzanie? |
| WindowsUpdateAgentVersion |UpdateAgent |Numer wersji agenta usługi Microsoft Update. |
| WSUSServer |UpdateAgent |Serwer WSUS, który jest celem tej usługi Windows update agent |
| OSVersion |UpdateAgent |Wersja systemu operacyjnego tej usługi Windows update agent jest uruchomiony w. |
| Nazwa |Zalecenie, ConfigurationObjectProperty |Nazwa/tytuł zalecenia lub nazwa właściwości z analizy dzienników konfiguracji oceny. |
| Wartość |ConfigurationObjectProperty |Wartość właściwości z analizy dzienników konfiguracji oceny. |
| KBLink |Zalecenie |Adres URL do artykułu bazy wiedzy, który opisuje najlepsze praktyki lub aktualizacji. |
| RecommendationStatus |Zalecenie |Zalecenia są wśród kilka typów rekordów aktualizowany, nie tylko dodane do indeksu wyszukiwania. Ten stan zmieni się, czy zalecane jest aktywny lub open lub analizy dzienników wykryje, że został rozwiązany. |
| RenderedDescription |Wydarzenie |Renderowane opis (ponownie tekst z parametrami wypełnione) zdarzeń systemu Windows. |
| ParameterXml |Wydarzenie |Plik XML z parametrami w sekcji danych zdarzenia systemu Windows (taki jak pokazano w Podglądzie zdarzeń). |
| EventData |Wydarzenie |Plik XML z sekcji całego danych zdarzenia systemu Windows (taki jak pokazano w Podglądzie zdarzeń). |
| Element źródłowy |Wydarzenie |Źródło dziennika zdarzeń, który wygenerował zdarzenie. |
| EventCategory |Wydarzenie |Kategoria zdarzenia, bezpośrednio z dziennika zdarzeń systemu Windows. |
| Nazwa użytkownika |Wydarzenie |Nazwa użytkownika dla zdarzenia systemu Windows (zazwyczaj NT AUTHORITY\LOCALSYSTEM). |
| SampleValue |PerfHourly |Średnia wartość dla agregacji godzinowej licznika wydajności. |
| Min |PerfHourly |Minimalna wartość godzinny interwał agregacji godzinowej licznika wydajności. |
| Maks. |PerfHourly |Maksymalna wartość godzinny interwał agregacji godzinowej licznika wydajności. |
| Percentile95 |PerfHourly |Wartość percentylu 95 godzinny interwał agregacji godzinowej licznika wydajności. |
| SampleCount |PerfHourly |Jak wiele próbek licznika wydajność pierwotna były używane do tworzenia tego rekordu agregacji godzinowej. |
| Zagrożenia |ProtectionStatus |Nazwa wykrytego złośliwego oprogramowania. |
| Konto magazynu |W3CIISLog |Konto usługi Azure Storage dziennika została odczytana z. |
| AzureDeploymentID |W3CIISLog |Identyfikator Azure wdrożenia usługi w chmurze dziennika należy. |
| Rola |W3CIISLog |Przynależność roli usługi w chmurze Azure dziennika. |
| RoleInstance |W3CIISLog |RoleInstance Azure dziennika należy do roli. |
| sSiteName |W3CIISLog |Witryny sieci Web usług IIS, której dziennika należy (metabazy notacji); Pole s-sitename w oryginalnym dziennika. |
| sComputerName |W3CIISLog |Pole s-computername w oryginalnym dziennika. |
| sIP |W3CIISLog |Żądanie HTTP adres IP serwera został skierowany do. Pole s-ip w oryginalnym dziennika. |
| csMethod |W3CIISLog |Metoda HTTP (na przykład GET/POST) używany przez klienta w żądaniu HTTP. Cs metoda w dzienniku oryginalnego. |
| cIP |W3CIISLog |Pochodzi żądanie HTTP adres IP klienta. Pole c-ip w oryginalnym dziennika. |
| csUserAgent |W3CIISLog |Agent użytkownika HTTP zgłoszone przez klienta (przeglądarki lub w inny sposób). Cs-user-agent w dzienniku oryginalnego. |
| scStatus |W3CIISLog |Zwrócony przez serwer do klienta kod stanu HTTP (na przykład 200/403/500). Cs-status w oryginalnym dziennika. |
| Właściwość timeTaken |W3CIISLog |Jak długo (w milisekundach), który realizacja żądania zajęła do wykonania. Pole właściwość timetaken w oryginalnym dziennika. |
| csUriStem |W3CIISLog |Względny identyfikator URI (bez adres hosta, który jest, / wyszukaj) który odebrał żądanie. Pola cs uristem w oryginalnej dziennika. |
| csUriQuery |W3CIISLog |Zapytanie URI. Identyfikator URI zapytania są niezbędne tylko w przypadku stron dynamicznych, takich jak strony ASP, więc to pole zawiera zwykle łącznika dla strony statyczne. |
| sPort |W3CIISLog |Port serwera, który żądanie HTTP zostało wysłane do (i który program IIS nasłuchuje, ponieważ jej pobrania go). |
| csUserName |W3CIISLog |Uwierzytelnić nazwę użytkownika, jeśli żądanie jest uwierzytelniane i nie anonimowe. |
| csVersion |W3CIISLog |Wersja protokołu HTTP użyty w żądaniu (na przykład HTTP/1.1). |
| csCookie |W3CIISLog |Informacje dotyczące pliku cookie. |
| csReferer |W3CIISLog |Witryna ostatnio odwiedzona przez użytkownika. Ta witryna udostępniła link do bieżącej lokacji. |
| csHost |W3CIISLog |Nagłówek hosta (na przykład www.mysite.com) żądanego. |
| scSubStatus |W3CIISLog |Kod błędu podstanu. |
| scWin32Status |W3CIISLog |Kod stanu systemu Windows. |
| csBytes |W3CIISLog |Liczba bajtów wysłanych w żądaniu od klienta do serwera. |
| scBytes |W3CIISLog |Liczba bajtów zwrócona w odpowiedzi z serwera do klienta. |
| ConfigChangeType |Zmianakonfiguracji |Typ zmiany (na przykład WindowsServices/oprogramowania). |
| ChangeCategory |Zmianakonfiguracji |Kategoria zmiany (zmodyfikowane Added/usunięte). |
| SoftwareType |Zmianakonfiguracji |Typ oprogramowania (aplikacja/aktualizacji). |
| SoftwareName |Zmianakonfiguracji |Nazwa oprogramowania (dotyczy tylko zmiany oprogramowania). |
| Wydawca |Zmianakonfiguracji |Dostawcy, który publikuje oprogramowania (dotyczy tylko zmiany oprogramowania). |
| SvcChangeType |Zmianakonfiguracji |Typ zmiany, która została zastosowana na usługi systemu Windows (stan/StartupType/ścieżka/konto_usługi). To ma zastosowanie tylko do zmiany usług systemu Windows. |
| SvcDisplayName |Zmianakonfiguracji |Nazwa wyświetlana usługi, która została zmieniona. |
| SvcName |Zmianakonfiguracji |Nazwa usługi, która została zmieniona. |
| SvcState |Zmianakonfiguracji |Nowy (bieżący) stan usługi. |
| SvcPreviousState |Zmianakonfiguracji |Poprzedni znane stanie usługi (dotyczy tylko po zmianie stanu usługi). |
| SvcStartupType |Zmianakonfiguracji |Typ uruchomienia usługi. |
| SvcPreviousStartupType |Zmianakonfiguracji |Poprzedni typ uruchamiania usługi (dotyczy tylko po zmianie typ uruchomienia usługi). |
| SvcAccount |Zmianakonfiguracji |Konto usługi. |
| SvcPreviousAccount |Zmianakonfiguracji |Poprzedniego konta usługi (dotyczy tylko po zmianie konta usługi). |
| SvcPath |Zmianakonfiguracji |Ścieżka do pliku wykonywalnego usługi systemu Windows. |
| SvcPreviousPath |Zmianakonfiguracji |Poprzednia ścieżka pliku wykonywalnego usługi systemu Windows (dotyczy tylko jego zmiana). |
| SvcDescription |Zmianakonfiguracji |Opis usługi. |
| Poprzednie |Zmianakonfiguracji |Poprzedni stan tego oprogramowania (nie zainstalowano zainstalowana/poprzedniej wersji). |
| Bieżący |Zmianakonfiguracji |Najnowszy stan tego oprogramowania (nie zainstalowano zainstalowana/bieżącej wersji). |

## <a name="next-steps"></a>Następne kroki
Aby uzyskać dodatkowe informacje dziennika wyszukiwania:

* Zapoznaj się z [wyszukiwaniem w dziennikach](log-analytics-log-searches.md), aby wyświetlić szczegółowe informacje zebrane przez rozwiązania.
* Użyj [pola niestandardowe w analizy dzienników](log-analytics-custom-fields.md) rozszerzenie dziennik wyszukiwania.
