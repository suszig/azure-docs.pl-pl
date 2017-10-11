---
title: "Dystrybucja tabel w usłudze SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do korzystania z dystrybucji tabel w usłudze Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: 5ed4337f-7262-4ef6-8fd6-1809ce9634fc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: d0e12bf821a81826a20b8db84e76c48fa60ad9b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="distributing-tables-in-sql-data-warehouse"></a>Dystrybucja tabel w usłudze SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Omówienie][Overview]
> * [Typy danych][Data Types]
> * [Dystrybuuj][Distribute]
> * [Indeks][Index]
> * [Partycji][Partition]
> * [Statystyki][Statistics]
> * [Tymczasowe][Temporary]
>
>

Usługa SQL Data Warehouse to system rozproszonych baz danych o architekturze masowego przetwarzana równoległego (MPP).  Dzieląc dane i możliwości przetwarzania między wiele węzłów, usługa SQL Data Warehouse może zaoferować bardzo dużą skalowalność — znacznie większą niż w przypadku jakiegokolwiek pojedynczego systemu.  Przy wyborze sposobu dystrybucji danych w magazynie danych programu SQL jest jednym z najważniejszych czynników do osiągnięcia optymalnej wydajności.   Klucz do uzyskania optymalnej wydajności to zminimalizowanie przenoszenia danych i z kolei wybiera strategii dystrybucji prawy klawisz, aby zminimalizować przenoszenia danych.

## <a name="understanding-data-movement"></a>Opis przenoszenia danych
W systemie MPP danych z każdej tabeli jest dzielona między kilka podstawowej bazy danych.  Najbardziej zoptymalizowane zapytania w systemie MPP można po prostu przekazywane do wykonania na pojedyncze rozproszonej bazy danych bez interakcji między bazami danych.  Załóżmy na przykład, że masz bazę danych danymi sprzedaży zawiera dwie tabele, sprzedaży i klientów.  Jeśli zapytanie, które należy dołączyć sprzedaży tabeli do tabeli klientów i dzielenia sprzedaży i tabele klienta się przez numer klienta umieszczanie każdego klienta w oddzielnej bazy danych, można rozwiązać żadnych zapytań, które przyłączyć sprzedaży i klienta, w każdej bazie danych z żadnych informacji na temat innych baz danych.  Z kolei danych sprzedaży rozdzielonych numer zamówienia i danych klienta przez numer klienta, następnie wszystkie danego bazy danych nie będzie zawierał odpowiednie dane dla każdego klienta, dlatego jeśli chcesz dołączyć sprzedaży danych do danych klienta, należy pobrać danych dla każdego klienta z innych baz danych.  W drugim przykładzie przepływu danych musi wystąpić przenoszenia danych klienta z danymi sprzedaży, dzięki czemu można łączyć dwie tabele.  

Przenoszenie danych zawsze jest zły element, czasami jest niezbędne dla rozwiązania zapytania.  Jednak podczas tego dodatkowego kroku można uniknąć, naturalny zapytanie będzie szybsze.  Przenoszenie danych powstaje najczęściej, gdy są połączone tabele lub agregacji są wykonywane.  Często należy wykonać, więc może być może zoptymalizować na jednym ze scenariuszy, takich jak sprzężenia, należy nadal przenoszenia danych, aby rozwiązać inne scenariusza, takich jak agregacji.  Lewy jest ustaleniem, czyli mniej pracy.  W większości przypadków dystrybucja tabele faktów dużych często dołączonego do kolumny jest najbardziej efektywną metodę zmniejszenia większości przenoszenia danych.  Dystrybucja danych na kolumn sprzężenia jest bardziej powszechnie używaną metodą Aby zmniejszyć przenoszenia danych niż dystrybucji danych kolumn uczestniczących w agregacji.

## <a name="select-distribution-method"></a>Wybieranie metody dystrybucji
Magazyn danych SQL w tle dzieli dane 60 baz danych.  Każdy jedna baza danych jest określany jako **dystrybucji**.  Po załadowaniu danych w każdej tabeli SQL Data Warehouse musi znać sposób podziału danych w tych 60 dystrybucji.  

Metoda dystrybucji jest zdefiniowana na poziomie tabeli, a obecnie dostępne są dwie możliwości:

1. **Działanie okrężne** który równomiernie, ale losowo dystrybucji danych.
2. **Skrótów rozproszone** która dystrybuuje oparte na tworzenie skrótów wartości z jednej kolumny danych

Domyślnie, gdy metoda dystrybucji danych, nie zostaną zdefiniowane tabeli będą dystrybuowane za pomocą **działanie okrężne** metoda dystrybucji.  Jednak jak staje się bardziej złożone w implementacji, należy rozważyć użycie **rozpowszechniane skrót** tabele, aby zminimalizować przenoszenia danych, który z kolei zoptymalizuje kwerendy wydajności.

### <a name="round-robin-tables"></a>Działanie okrężne tabel
Przy użyciu metody okrężne dystrybucji danych jest znacznie, jak ich wymowy.  Jak Twoje dane są ładowane, każdy wiersz po prostu są wysyłane do dystrybucji dalej.  Ta metoda dystrybucji danych będzie zawsze losowo równomierne danych bardzo we wszystkich dystrybucje.  Oznacza to, że istnieje sortowania nie wykonywane podczas procesu działanie okrężne, który umieszcza dane.  Działanie okrężne dystrybucji jest niekiedy nazywany losowego wyznaczania wartości skrótu z tego powodu.  Z tabelą rozproszonej okrężnego jest niepotrzebna, aby zrozumieć dane.  Z tego powodu tabel okrężnego należy często dobrym ładowanie obiektów docelowych.

Domyślnie, jeśli wybrano opcję Brak metody dystrybucji, działanie okrężne dystrybucji zostanie użyta metoda.  Jednak działanie okrężne tabele są łatwe w użyciu, ponieważ danych losowo jest dystrybuowana do systemu, oznacza to, że system nie może zagwarantować, które dystrybucji każdy wiersz jest na.  W związku z tym system musi czasami do wywołania operacji przepływu danych w celu lepszej organizacji danych przed może rozpoznać zapytania.  Ten krok dodatkowe może to spowolnić zapytań.

Rozważ użycie dystrybucji okrężnego dla tej tabeli w następujących scenariuszach:

* Gdy wprowadzenie jako punktu wyjścia proste
* Jeśli oczywiste łącząca klucz nie istnieje
* Jeśli nie ma kolumny odpowiednimi kandydatami do wyznaczania wartości skrótu dystrybucji tabeli
* Jeśli tabela nie udostępniane wspólny klucz sprzężenia innych tabel
* Jeśli sprzężenie jest mniej istotna niż inne sprzężeń w zapytaniu
* Gdy jest tymczasowe Tabela przemieszczania

Oba te przykłady spowoduje utworzenie tabeli działanie okrężne:

```SQL
-- Round Robin created by default
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
;

-- Explicitly Created Round Robin Table
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = ROUND_ROBIN
)
;
```

> [!NOTE]
> Gdy jest działanie okrężne domyślny typ tabeli jest jawne w kod DDL jest traktowany jako najlepszym rozwiązaniem tak, aby były wyczyść innym zamiarach układu tabeli.
>
>

### <a name="hash-distributed-tables"></a>Rozproszone tabele hash
Przy użyciu **rozpowszechniane skrót** algorytmu do dystrybucji tabel może poprawić wydajność w różnych scenariuszach ograniczając przenoszenie danych w czasie zapytania.  Tabele hash rozproszone są tabele, które są dzielone między rozproszonej bazy danych przy użyciu algorytmu wyznaczania wartości skrótu w jednej kolumnie, która zostanie wybrana.  Kolumna dystrybucji jest, co określa sposób podziału danych w sieci rozproszonej bazy danych.  Funkcja skrótu używa kolumny dystrybucji można przypisać podziału wierszy.  Algorytm wyznaczania wartości skrótu i wynikowy dystrybucji jest deterministyczna.  To już taką samą wartość, tym samym typie danych będzie zawsze ma do tego samego dystrybucji.    

W tym przykładzie utworzy rozproszone na identyfikator tabeli:

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
```

## <a name="select-distribution-column"></a>Wybierz kolumnę dystrybucji
Jeśli zdecydujesz się **skrótu dystrybucji** tabelę, musisz wybrać kolumnę pojedynczego dystrybucji.  Podczas wybierania kolumn dystrybucji, istnieją trzy główne czynniki, które należy wziąć pod uwagę.  

Wybrać pojedynczą kolumnę, która będzie:

1. Nie można zaktualizować
2. Równomierne danych, unikając pochylenia danych
3. Minimalizowanie przepływu danych

### <a name="select-distribution-column-which-will-not-be-updated"></a>Wybierz kolumnę dystrybucji, które nie zostaną zaktualizowane
Kolumny dystrybucji nie są aktualizowalne, w związku z tym, wybierz kolumnę o wartości statyczne.  Jeśli kolumna zostanie muszą zostać zaktualizowane, zwykle nie jest kandydatem dobrej dystrybucji.  W przypadku przypadek, w którym należy zaktualizować kolumn dystrybucji, można to zrobić przez usunięcie najpierw wiersza, a następnie wstawienia nowego wiersza.

### <a name="select-distribution-column-which-will-distribute-data-evenly"></a>Wybierz kolumnę dystrybucji, który będzie równomierne danych
Ponieważ rozproszony system przeprowadza tylko tak szybko, jak jego najwolniejsze dystrybucji, należy podzielić pracy równomiernie między dystrybucje aby osiągnąć zrównoważonym wykonywania przez system.  Sposób pracy jest podzielona na Rozproszony system jest oparty na którym mieszka danych dla poszczególnych dystrybucji.  Dzięki temu można bardzo ważne, aby wybrać kolumnę dystrybucji prawo do dystrybucji danych, dzięki czemu każdej dystrybucji ma taki sam pracy i potrwa tym samym czasie, aby ukończyć jego część pracy.  Podczas pracy dobrze jest dzielona między systemu, danych jest równoważone między dystrybucje.  Gdy dane nie jest rozmieszczana równomiernie, nazywamy to **zegara danych**.  

Do dzielenia danych równomiernie i uniknąć pochylenia danych, należy rozważyć podczas wybierania kolumny dystrybucji:

1. Wybierz kolumny, która zawiera znaczące liczbę unikatowych wartości.
2. Unikaj dystrybucji danych w kolumnach z kilku odrębnych wartości.
3. Unikaj dystrybucji danych dla kolumn o wysokiej częstotliwości zawiera wartości null.
4. Unikaj dystrybucji danych na kolumny daty.

Ponieważ każda wartość jest wartość skrótu, aby 1 60 dystrybucji, do osiągnięcia nawet dystrybucji można zaznaczyć kolumny, która jest unikatowa wysokiej i nie zawiera unikatowych wartości większej niż 60.  Aby zilustrować, należy wziąć pod uwagę przypadków, w których kolumna ma tylko 40 unikatowe wartości.  Jeśli ta kolumna została wybrana jako klucza dystrybucji, dane dla tej tabeli spowoduje grunt na 40 dystrybucje co najwyżej pozostawienie 20 dystrybucji żadnych danych i przetwarzania, nie należy.  Z drugiej strony 40 dystrybucji mają więcej pracy, aby to zrobić, jeśli dane został równomierny dystrybucje ponad 60.  Ten scenariusz jest przykładem danych pochylenia.

W systemie MPP każdego kroku zapytania czeka na wszystkie dystrybucje przeprowadzenie udziału w pracy.  Jeśli jeden dystrybucji jest więcej pracy od innych, następnie zasobu innych dystrybucji są zasadniczo niewykorzystana właśnie trwa oczekiwanie na zajęty dystrybucji.  Podczas pracy nie jest równomiernie umieszczonych na wszystkie dystrybucje, nazywamy to **przetwarzania pochylenia**.  Przetwarzanie zegara spowoduje, że zapytania, aby działać wolniej niż jeśli obciążenie może być równomierny dla wszystkich dystrybucje.  Dane pochylenia doprowadzi do przetwarzania pochylenia.

Unikaj dystrybucja wysokiej wartości Null kolumny jako wartości null spowoduje przejście na tym samym dystrybucji. Dystrybucja na kolumnę dat również może spowodować pochylenie przetwarzania, ponieważ na tym samym dystrybucji system przeniesie wszystkie dane dla określonej daty. W przypadku kilku użytkowników jest wykonywanie zapytań wszystkich filtrowania tego samego dnia, a następnie tylko 1 60 dystrybucji będzie wykonanie wszystkich pracy od podanej daty będą tylko w jednej dystrybucji. W tym scenariuszu zapytania prawdopodobnie zostanie uruchomiony 60 razy mniejsza niż Jeśli dane zostały równomiernie rozłożone wszystkie dystrybucje.

Gdy brak kolumn odpowiednimi kandydatami istnieje, a następnie należy rozważyć użycie okrężnego jako metoda dystrybucji.

### <a name="select-distribution-column-which-will-minimize-data-movement"></a>Wybierz kolumny dystrybucji, która zostanie zminimalizowane przepływu danych
Minimalizując przenoszenia danych, wybierając kolumnę w prawo dystrybucji jest jednym z najważniejszych strategii dla optymalizacji wydajności magazynu danych SQL.  Przenoszenie danych powstaje najczęściej, gdy są połączone tabele lub agregacji są wykonywane.  Kolumn używanych w `JOIN`, `GROUP BY`, `DISTINCT`, `OVER` i `HAVING` klauzule wszystkie ułatwiają **dobrej** skrótów kandydatów dystrybucji.

Z drugiej strony, kolumn w `WHERE` klauzuli **nie** wprowadzić kandydatów kolumny dobrej skrótu ponieważ ich ograniczenia, które dystrybucje uczestniczyć w zapytaniu, co powoduje przetwarzania pochylenia.  Dobrym przykładem kolumny, która może być kuszące do dystrybucji na, ale często powodują tego pochylenia przetwarzania jest kolumny daty.

Ogólnie rzecz biorąc Jeśli masz dwie tabele faktów dużych często zaangażowane w sprzężeniu, będzie uzyskać większości wydajności przez dystrybucję obu tabel w jednej z kolumn sprzężenia.  Jeśli masz tabelę, która nigdy nie jest przyłączona do innej tabeli faktów dużych, poszukaj kolumn, które są często w `GROUP BY` klauzuli.

Istnieje kilka kluczowych kryteria, które muszą zostać spełnione, aby uniknąć przenoszenia danych podczas sprzężenia:

1. Tabele uwzględnione w sprzężeniu musi być rozproszone na skrót **jeden** kolumn uczestniczących w sprzężeniu.
2. Typy danych kolumn sprzężenia musi odpowiadać między obu tabel.
3. Kolumny musi być połączony z operatora równości.
4. Typ sprzężenia nie może być `CROSS JOIN`.

## <a name="troubleshooting-data-skew"></a>Rozwiązywanie problemów z danych pochylenia
Gdy dane w tabeli są przesyłane przy użyciu metody dystrybucji skrót istnieje ryzyko, że niektórych dystrybucji będzie niesymetryczna nieproporcjonalnie więcej danych niż inne. Pochylenie nadmiernej ilości danych może wpłynąć na wydajność zapytań, ponieważ wynik końcowy zapytań rozproszonych musi czekać na najdłużej działających dystrybucji zakończyć. W zależności od stopnia pochylenie danych może być konieczne jego rozwiązania.

### <a name="identifying-skew"></a>Identyfikowanie zegara
Prosty sposób, aby zidentyfikować tabelę jako niesymetryczna jest użycie `DBCC PDW_SHOWSPACEUSED`.  Jest to bardzo szybki i prosty sposób aby wyświetlić liczbę wierszy tabeli, które są przechowywane w każdym z 60 dystrybucje bazy danych.  Należy pamiętać, że dla najbardziej zrównoważoną wydajność, wiersze w tabeli rozproszonej powinny być rozmieszczone równomiernie w obrębie wszystkie dystrybucje.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Jednak w przypadku zapytań usługi Azure SQL Data Warehouse dynamicznych widoków zarządzania (DMV) można przeprowadzić bardziej szczegółowej analizy.  Aby rozpocząć, Utwórz widok [dbo.vTableSizes] [ dbo.vTableSizes] wyświetlić przy użyciu programu SQL z [omówienie tabeli] [ Overview] artykułu.  Po utworzeniu widoku wykonania tego zapytania, aby zidentyfikować tabel, które mają więcej niż 10% danych zegara.

```sql
select *
from dbo.vTableSizes
where two_part_name in
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="resolving-data-skew"></a>Rozpoznawanie pochylenia danych
Nie wszystkie pochylenia jest wystarczająca do zapewnienia liczbę poprawkę.  W niektórych przypadkach wydajności tabeli niektórych kwerend może przeważają uszkodzenie danych pochylenia.  Podjęcie decyzji, czy należy poprawić danych pochylenia w tabeli, należy zrozumieć jak to możliwe ilości danych i zapytania w obciążenie.   Jest jednym ze sposobów przyjrzeć się wpływ zegara do wykonania czynności w [monitorowania zapytania] [ Query Monitoring] artykuł, aby monitorować wpływ pochylenia wzdłuż wydajność zapytań i w szczególności wpływu jak długo wysyła zapytanie do podjęcia w poszczególnych dystrybucji.

Dystrybucji danych polega na znajdowaniu kompromisu między minimalizując zegara danych i minimalizując przenoszenia danych. Te można przeciwne cele i czasem można przechowywać dane pochylenia w celu ograniczenia przepływu danych. Na przykład gdy kolumna dystrybucji jest często wspólnej kolumny sprzężenia i agregacji, można będzie można minimalizację przenoszenia danych. O konieczności przeniesienia minimalna ilość danych może być większe niż wpływ pochylanie danych.

Typowy sposób rozwiązania zegara danych jest ponownie utwórz tabelę z kolumną różnych dystrybucji. Ponieważ nie można zmienić kolumny dystrybucji w istniejącej tabeli, sposób dystrybucji tabeli zmień ją utworzyć go ponownie [] [CTAS].  Poniżej przedstawiono dwa przykłady tego, jak rozpoznać pochylenia danych:

### <a name="example-1-re-create-the-table-with-a-new-distribution-column"></a>Przykład 1: Ponownie utwórz tabelę z kolumną nowe dystrybucji
W tym przykładzie użyto [] [CTAS] ponownie utworzyć tabelę z kolumną dystrybucji różnych wyznaczania wartości skrótu.

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

### <a name="example-2-re-create-the-table-using-round-robin-distribution"></a>Przykład 2: Ponownie utwórz tabelę przy użyciu rozkładu okrężnego
W tym przykładzie użyto [] [CTAS], aby ponownie utworzyć tabelę z okrężnego zamiast dystrybucji wyznaczania wartości skrótu. Ta zmiana spowoduje nawet danych dystrybucji kosztem przepływu danych.

```sql
CREATE TABLE [dbo].[FactInternetSales_ROUND_ROBIN]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  ROUND_ROBIN
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_ROUND_ROBIN')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_ROUND_ROBIN] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_ROUND_ROBIN] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_ROUND_ROBIN] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_ROUND_ROBIN] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_ROUND_ROBIN] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_ROUND_ROBIN] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_ROUND_ROBIN] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_ROUND_ROBIN] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_HASH];
RENAME OBJECT [dbo].[FactInternetSales_ROUND_ROBIN] TO [FactInternetSales];
```

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej o projektowaniu tabel, zobacz [dystrybucji][Distribute], [indeksu][Index], [partycji][Partition], [typy danych][Data Types], [statystyki] [ Statistics] i [tabel tymczasowych] [ Temporary] artykułów.

Omówienie najlepszych rozwiązań, zobacz [najlepsze rozwiązania magazynu danych SQL][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md
[Query Monitoring]: ./sql-data-warehouse-manage-monitor.md
[dbo.vTableSizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries

<!--MSDN references-->
[DBCC PDW_SHOWSPACEUSED()]: https://msdn.microsoft.com/library/mt204028.aspx

<!--Other Web references-->
