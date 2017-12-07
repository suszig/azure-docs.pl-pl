---
title: "Zarządzanie statystyk dotyczących tabel w usłudze SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do korzystania z statystyk dotyczących tabel w usłudze Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 11/06/2017
ms.author: barbkess
ms.openlocfilehash: 2349708f607364c34926a2ea1baa025201934973
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a>Zarządzanie statystyk dotyczących tabel w usłudze SQL Data Warehouse
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

Im bardziej zna danych usługi SQL Data Warehouse, tym szybciej można wykonywać zapytań dotyczących danych.  Metodą Poinformuj SQL Data Warehouse o danych, jest zbieranie statystyk dotyczących danych.  Uzyskanie statystyki do danych jest jednym z najbardziej ważnych rzeczy, które można wykonać w celu zoptymalizowania zapytań.  Statystyki pomocy utworzyć plan optymalny dla zapytań usługi SQL Data Warehouse.  Jest to spowodowane Optymalizator zapytań SQL Data Warehouse jest Optymalizator na podstawie.  To, że porównuje koszt różne plany zapytań i następnie wybiera plan o najniższej cenie, należy również plan, który zostanie wykonany najszybsze.

Statystykę można tworzyć w jednej kolumnie, wiele kolumn lub indeksu tabeli.  Statystyki są przechowywane w histogram, który przechwytuje zakresu i wybieralność wartości.  Jest to szczególne znaczenie podczas Optymalizator należy ocenić sprzężenia, GROUP BY, HAVING i klauzulach WHERE w zapytaniu.  Na przykład, jeśli Optymalizator szacuje, że data filtrowania kwerendy zwróci 1 wiersz, mogą wybrać, bardzo inny plan niż w przypadku jego szacuje, że ich daty należy wybrano będzie zwracać 1 milion wierszy.  Podczas tworzenia statystyk jest bardzo ważne, równie ważne jest, że statystyki *dokładnie* odzwierciedlały bieżący stan tabeli.  Aktualne statystyki zapewnia, że dobry plan jest wybierany przez optymalizator.  Utworzonych przez optymalizator planów są tylko dobrą statystyk na podstawie danych.

Proces tworzenia i zaktualizowanie statystyk jest obecnie ręczny proces, ale jest bardzo prosty zrobić.  Jest to w przeciwieństwie do programu SQL Server, które automatycznie utworzy i aktualizuje statystyki dotyczące pojedynczego kolumn i indeksów.  Korzystając z poniższych informacji, można znacznie automatyzują zarządzanie statystyk na podstawie danych. 

## <a name="getting-started-with-statistics"></a>Wprowadzenie do statystyki
 Tworzenie statystyk próbki w każdej kolumnie jest łatwe Rozpoczynanie pracy z statystyk.  Ponieważ jest równie ważne zapewnić aktualność statystyk, tradycyjne podejście może być aktualizowanie statystyk codziennie lub po każdej obciążenia. Zawsze istnieje możliwość wypracowania kompromisu pomiędzy wydajnością a kosztami tworzenia i aktualizowania statystyk.  Jeśli okaże się, że utrzymywanie wszystkich statystyk trwa zbyt długo, można spróbować wybrać tylko niektóre kolumny, dla których mają być prowadzone statystyki, lub wybrać kolumny, które wymagają częstego aktualizowania.  Na przykład można zaktualizować kolumn dat codziennie, jak mogą być dodawane nowe wartości, a nie po każdym załadowaniu. Ponownie, uzyska najlepiej wykorzystać przez uzyskanie statystyki do kolumn uczestniczących w sprzężenia, GROUP BY, HAVING i klauzulach WHERE.  Jeśli masz tabelę z dużą liczbą kolumn, które są używane tylko w klauzuli SELECT, statystyki dla tych kolumn nie może pomóc i wydatków nieco więcej starań, aby zidentyfikować kolumny, których pomoże statystyk, można skrócić czas do obsługi statystyk.

## <a name="multi-column-statistics"></a>Statystyki wielokolumnowego
Oprócz tworzenia statystyk dla pojedynczego kolumn, może się okazać, zapytań zyskają statystyki wielokolumnowych.  Statystyka wielokolumnowego jest statystyki tworzone na liście kolumn.  Obejmują one statystyki jednej kolumny w pierwszej kolumnie na liście, a także niektóre informacje o powiązaniu między kolumny o nazwie gęstości.  Na przykład jeśli masz tabeli, w której jest przyłączany do innego dwie kolumny, może się okazać, że usługi SQL Data Warehouse lepiej można zoptymalizować planu, jeśli sam relacji między kolumnami.   Statystyki wielokolumnowego może poprawiać wydajność zapytań dla niektórych operacji, takich jak złożonych sprzężeń i group by.

## <a name="updating-statistics"></a>Zaktualizowanie statystyk
Zaktualizowanie statystyk jest ważnym elementem procedury zarządzania z bazy danych.  Zmiany danych w bazie danych dystrybucji statystyki muszą zostać zaktualizowane.  Nieaktualne statystyki doprowadzi do nieoptymalnych kwerendy wydajności.

Jeden najlepszym rozwiązaniem jest aktualizację statystyk dotyczących kolumn dat każdego dnia, gdy zostaną dodane nowe daty.  Każdy czas nowe wiersze są załadowane do magazynu danych, nowe obciążenia daty lub daty transakcji zostaną dodane. Te zmiany dystrybucji danych i utworzyć statystyki nieaktualne. Z drugiej strony statystyki dotyczące kraju kolumny w tabeli klienta może nigdy nie muszą zostać zaktualizowane, jak rozkład wartości zwykle nie ulega zmianie. Zakładając, że dystrybucja jest stałe między klientami, dodawanie nowych wierszy do zmiany tabeli nie jest będzie zmienić dystrybucji danych. Jednak jeśli magazyn danych zawiera tylko jeden kraj, należy przenieść dane z nowego kraju dane z różnych krajach przechowywane, następnie ostatecznie należy aktualizować statystyki w kolumnie kraju.

Jednym z pierwszym pytań podczas rozwiązywania problemów z kwerendy jest "Są aktualne statystyki?"

To pytanie nie jest taki, który należy odpowiedzieć przy wiek danych. Obiekt aktualne statystyki może być bardzo stare, jeśli nie ma w niej żadnych istotnych zmian w danych źródłowych. Jeśli liczba wierszy zmienił znacząco lub istotne zmiany w dystrybucji wartości dla danej kolumny *następnie* nadszedł czas, aby zaktualizować statystyk.  

Odwołania **programu SQL Server** (nie SQL Data Warehouse) automatycznie aktualizuje statystyk w takich sytuacjach:

* Jeśli masz żadnych wierszy w tabeli, podczas dodawania wierszy, otrzymasz automatycznych aktualizacji statystyk
* Po dodaniu więcej niż 500 wierszy do tabeli, począwszy od mniej niż 500 wierszy (np. na początku masz 499 i następnie dodać 500 wierszy w sumie 999 wierszy), zostanie wyświetlony aktualizacji automatycznych 
* Po wyświetleniu ponad 500 wierszy, należy dodać 500 dodatkowe wiersze + 20% rozmiar tabeli przed zobaczysz automatycznych aktualizacji na statystyki

Ponieważ nie ma żadnych DMV, aby określić, czy dane w tabeli zmienił się od czasu ostatniego statystyk czasu zostały zaktualizowane, wiedząc, wieku statystyk może umożliwić z część obrazu.  Następujące zapytanie służy do określenia czasu ostatniego statystyk w przypadku, gdy zaktualizowane w każdej tabeli.  

> [!NOTE]
> Należy pamiętać o tym, w przypadku istotnych zmian dystrybucja wartości dla danej kolumny, należy zaktualizować statystyk niezależnie od tego czasu, gdy są one zostały zaktualizowane.  
> 
> 

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

Kolumn dat w magazynie danych, na przykład zwykle wymagają częstego aktualizacji statystyk. Każdy czas nowe wiersze są załadowane do magazynu danych, nowe obciążenia daty lub daty transakcji zostaną dodane. Te zmiany dystrybucji danych i utworzyć statystyki nieaktualne.  Z drugiej strony statystyk dotyczących płci kolumny w tabeli klienta nigdy nie może być konieczne do zaktualizowania. Zakładając, że dystrybucja jest stałe między klientami, dodawanie nowych wierszy do zmiany tabeli nie jest będzie zmienić dystrybucji danych. Jednak jeśli magazyn danych zawiera tylko jeden płci i nowe wyniki wymaganie w wielu płci ostatecznie należy aktualizować statystyki w kolumnie płci.

Aby uzyskać dokładniejsze objaśnienie, zobacz [statystyki] [ Statistics] w witrynie MSDN.

## <a name="implementing-statistics-management"></a>Implementowanie zarządzania statystyki
Często jest dobrym pomysłem jest rozszerzenie procesu, aby upewnić się, że statystyki są aktualizowane na końcu obciążenia ładowania danych. Ładowanie danych jest tabel najczęściej zmiany ich rozmiaru i/lub ich rozkład wartości. W związku z tym jest to logiczne miejsca do wykonania niektórych procesów zarządzania.

Niektóre wytyczne są podane poniżej w celu zaktualizowania statystyk podczas procesu obciążenia:

* Sprawdź, czy każdy załadowanej tabeli ma co najmniej jeden obiekt statystyki aktualizacji. Spowoduje to zaktualizowanie informacje o rozmiarze (liczba wierszy i liczba stron) tabel w ramach aktualizacji statystyk.
* Skupić się na kolumn uczestniczących w klauzuli sprzężenia, GROUP BY, ORDER BY i DISTINCT
* Może być konieczna aktualizacja "rosnącej klucza" kolumn, takie jak transakcji więcej często wartości te nie zostaną uwzględnione w postaci histogramu statystyki daty.
* Należy rozważyć aktualizowanie kolumn dystrybucji statycznych często.
* Należy pamiętać, że każdy obiekt Statystyka jest aktualizowany w serii. Po prostu implementacja `UPDATE STATISTICS <TABLE_NAME>` może nie być idealne — szczególnie w przypadku szerokie tabele z dużą liczbą obiektów statystyk.

> [!NOTE]
> Więcej informacji na temat [rosnącej klucza] można znaleźć w dokumencie modelu szacowania kardynalności programu SQL Server 2014.
> 
> 

Aby uzyskać dokładniejsze objaśnienie, zobacz [szacowania kardynalności] [ Cardinality Estimation] w witrynie MSDN.

## <a name="examples-create-statistics"></a>Przykłady: Tworzenie statystyk
Poniższe przykłady pokazują, jak tworzenie statystyk na użytek różnych opcji. Opcje używane dla każdej kolumny są zależne od właściwości danych i jak kolumna będzie używane w zapytaniach.

### <a name="a-create-single-column-statistics-with-default-options"></a>A. Tworzenie statystyk pojedynczej kolumny z opcji domyślnych
Aby utworzyć statystyki dla kolumny, wystarczy podać nazwę obiektu statystyk i nazwę kolumny.

Ta składnia wykorzystuje wszystkie domyślne opcje. Domyślnie 20 procent tabeli przykłady SQL Data Warehouse, podczas tworzenia statystyk.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Na przykład:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="b-create-single-column-statistics-by-examining-every-row"></a>B. Tworzenie statystyk pojedynczej kolumny, sprawdzając każdego wiersza
Domyślna częstotliwość próbkowania równy 20% jest wystarczające w większości sytuacji. Można jednak dostosować częstotliwość próbkowania.

Do próbkowania pełne tabeli, należy użyć następującej składni:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Na przykład:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="c-create-single-column-statistics-by-specifying-the-sample-size"></a>C. Tworzenie statystyk pojedynczej kolumny, określając rozmiar próbki
Alternatywnie można określić rozmiar próbki w procentach:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="d-create-single-column-statistics-on-only-some-of-the-rows"></a>D. Tworzenie statystyk pojedynczej kolumny dla niektórych wierszy
Inną opcją, można utworzyć statystyki w części wiersze w tabeli. Jest to filtrowane statystyki.

Można na przykład użyć statystykę filtrowaną, planując zapytania określonej partycji tabeli partycjonowanej duże. Tworzenie statystyk na tylko wartości partycji, prawidłowość statystyki będzie poprawić, a w związku z tym poprawiać wydajność zapytań.

Ten przykład tworzy statystyki zakresu wartości. Wartości można łatwo ustawione w taki sposób, aby odpowiadały zakres wartości w partycji.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> Optymalizator zapytań należy rozważyć użycie statystykę filtrowaną, gdy wybiera plan zapytania rozproszonego zapytanie musi mieścić się w definicji obiektu statystyk. W poprzednim przykładzie, kwerendy gdzie należy określić wartości col1 między 2000101 i 20001231 klauzuli.
> 
> 

### <a name="e-create-single-column-statistics-with-all-the-options"></a>E. Tworzenie statystyk pojedynczej kolumny przy użyciu opcji
Opcje można oczywiście łączyć ze sobą. Poniższy przykład tworzy obiekt statystykę filtrowaną z niestandardowych próbkowania:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Aby uzyskać pełną dokumentację, zobacz [instrukcji CREATE STATISTICS] [ CREATE STATISTICS] w witrynie MSDN.

### <a name="f-create-multi-column-statistics"></a>F. Tworzenie statystyk wielokolumnowego
Tworzenie statystyk wielokolumnowego, po prostu użyć poprzednich przykładach, lecz określ większą liczbę kolumn.

> [!NOTE]
> Histogram, który jest używany do oszacować liczbę wierszy w wyniku zapytania, jest dostępna tylko dla pierwszej kolumny na liście definicji obiektu statystyk.
> 
> 

W tym przykładzie histogram znajduje się na *produktu\_kategorii*. Statystyki między kolumny są obliczane na *produktu\_kategorii* i *produktu\_sub_c\ategory*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Ponieważ korelacji między *produktu\_kategorii* i *produktu\_sub\_kategorii*, stat wielokolumnowego mogą być przydatne, jeśli te kolumny są dostępne. w tym samym czasie.

### <a name="g-create-statistics-on-all-the-columns-in-a-table"></a>G. Tworzenie statystyk dla wszystkich kolumn w tabeli
Jednym ze sposobów tworzenia statystyk ma problemy polecenia CREATE STATISTICS po utworzeniu tabeli.

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="h-use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>H. Użyj procedury składowanej utworzyć statystyki dla wszystkich kolumn w bazie danych
Magazyn danych SQL nie ma systemowej procedury składowanej odpowiednikiem [] [sp_create_stats] w programie SQL Server. Tę procedurę składowaną tworzy obiekt statystyki jednej kolumny w każdej kolumnie bazy danych, który nie ma jeszcze statystyk.

Pomoże to wprowadzenie do projektu bazy danych. Możesz dostosować go do własnych potrzeb.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN    sys.[external_tables] e    ON    e.[object_id]        = t.[object_id]
WHERE       l.[object_id] IS NULL
AND            e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

Aby utworzyć statystyki dla wszystkich kolumn w tabeli z tej procedury, po prostu wywołania tej procedury.

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a>Przykłady: Aktualizuj statystyki
Aby zaktualizować statystyk, można:

1. Zaktualizuj jeden obiekt statystyk. Określ nazwę obiektu statystyk, które chcesz zaktualizować.
2. Zaktualizuj wszystkie obiekty statystyki dla tabeli. Określ nazwę tabeli zamiast jeden obiekt poszczególnych statystyk.

### <a name="a-update-one-specific-statistics-object"></a>A. Zaktualizuj jeden obiekt poszczególnych statystyk
Aby zaktualizować obiekt poszczególnych statystyk, należy użyć następującej składni:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Na przykład:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Aktualizacja poszczególnych statystyk obiektów, można zminimalizować czas i zasoby wymagane do zarządzania statystyk. Wymaga to rozwagą, jednak, aby wybrać najlepsze obiekty statystyki aktualizacji.

### <a name="b-update-all-statistics-on-a-table"></a>B. Aktualizuj wszystkie statystyki dla tabeli
Oznacza to prosta metoda aktualizowania wszystkich obiektów statystyk tabeli.

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Na przykład:

```sql
UPDATE STATISTICS dbo.table1;
```

Ta instrukcja jest łatwy w użyciu. Pamiętaj tylko to aktualizuje wszystkie statystyki w tabeli i dlatego może wykonywać więcej pracy, niż jest to konieczne. Jeśli wydajność nie stanowi to problemu, ostatecznie to najprostszy i najbardziej kompleksowe sposób gwarantuje, że dane statystyczne są aktualne.

> [!NOTE]
> Podczas aktualizowania wszystkich statystyk dotyczących tabeli, SQL Data Warehouse nie skanowanie w celu przykładowej tabeli dla poszczególnych statystyk. Jeśli tabela jest duży, ma wiele kolumn i ilość danych statystycznych, może być bardziej wydajne, można zaktualizować poszczególnych statystyki, w zależności od potrzeb.
> 
> 

Dla implementacji `UPDATE STATISTICS` procedury można znaleźć pod adresem [tabel tymczasowych] [ Temporary] artykułu. Metody wdrażania różni się nieznacznie się `CREATE STATISTICS` powyższą procedurę, ale wynik końcowy jest taki sam.

Dla pełnej składni, zobacz [Update Statistics] [ Update Statistics] w witrynie MSDN.

## <a name="statistics-metadata"></a>Statystyki metadanych
Istnieje kilka widok systemu i funkcje, które umożliwia znalezienie informacji na temat statystyk. Na przykład widać, jeśli obiekt statystyki może być nieaktualne za pomocą funkcji daty Statystyka wyświetlać podczas statystyki ostatnio zostały utworzone lub zaktualizowane.

### <a name="catalog-views-for-statistics"></a>Widoków wykazu statystyk
Widoki te systemu zawierają informacje o statystyki:

| Przeglądanie katalogu | Opis |
|:--- |:--- |
| [sys.Columns][sys.columns] |Jeden wiersz dla każdej kolumny. |
| [sys.Objects][sys.objects] |Jeden wiersz dla każdego obiektu w bazie danych. |
| [sys.schemas][sys.schemas] |Jeden wiersz dla każdego schematu w bazie danych. |
| [sys.stats][sys.stats] |Jeden wiersz dla każdego obiektu statystyk. |
| [sys.stats_columns][sys.stats_columns] |Jeden wiersz dla każdej kolumny w obiekcie statystyk. Łącza do sys.columns. |
| [Widok sys.Tables][sys.tables] |Jeden wiersz dla każdej tabeli (w tym tabel zewnętrznych). |
| [sys.table_types][sys.table_types] |Jeden wiersz dla każdego typu danych. |

### <a name="system-functions-for-statistics"></a>Funkcje systemu statystyk
Funkcje systemu są przydatne w przypadku pracy z statystyki:

| System — funkcja | Opis |
|:--- |:--- |
| [STATS_DATE][STATS_DATE] |Data ostatniej aktualizacji obiektu statystyk. |
| [POLECENIE DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS] |Zawiera podsumowanie poziomu i szczegółowe informacje o rozkład wartości rozumieniu obiektu statystyk. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Łączenie statystyk kolumny i funkcji w jednym widoku
Ten widok zapewnia kolumn, które dotyczą statystyk i wyniki z funkcji [] [STATS_DATE()] razem.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>Przykłady DBCC SHOW_STATISTICS()
Polecenie DBCC SHOW_STATISTICS() zawiera dane przechowywane w obiekcie statystyk. Te dane składa się z trzech części.

1. Nagłówek
2. Wektor gęstość
3. Histogram

Metadane nagłówka o statystyki. Histogram przedstawia rozkład wartości z pierwszej kolumny klucza obiektu statystyk. Wektor gęstość mierzy korelacji między kolumny. SQLDW oblicza szacowania kardynalności o dane w obiekcie statystyk.

### <a name="show-header-density-and-histogram"></a>Pokaż nagłówka, gęstości i histogramu
Ten prosty przykład przedstawia wszystkich trzech części obiektu statystyk.

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Na przykład:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>Pokaż co najmniej jeden części DBCC SHOW_STATISTICS();
Jeśli interesuje Cię tylko wyświetlanie określonych części, użyj `WITH` klauzuli i określ części, które chcesz wyświetlić:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Na przykład:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>Polecenie DBCC SHOW_STATISTICS() różnic
DBCC SHOW_STATISTICS() bardziej ściśle jest zaimplementowana w usłudze SQL Data Warehouse w porównaniu z programem SQL Server.

1. Nieudokumentowanej funkcje nie są obsługiwane.
2. Nie można użyć Stats_stream
3. Nie można dołączyć wyniki konkretne podzestawy danych statystyki np. (STAT_HEADER sprzężenia DENSITY_VECTOR)
4. Nie można ustawić NO_INFOMSGS dla pomijania wiadomości
5. Nie można użyć nazwy statystyki nawiasy kwadratowe
6. Nie można użyć nazwy kolumn, aby zidentyfikować obiekty statystyki
7. Błąd niestandardowy 2767 nie jest obsługiwana.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji, zobacz [DBCC SHOW_STATISTICS] [ DBCC SHOW_STATISTICS] w witrynie MSDN.  Aby dowiedzieć się więcej, zobacz artykuły w [omówienie tabeli][Overview], [typy danych tabeli][Data Types], [Dystrybucja tabeli] [ Distribute], [Indeksowania tabeli][Index], [partycjonowania tabeli] [ Partition] i [Tabel tymczasowych][Temporary].  Aby uzyskać więcej informacji na temat najlepszych rozwiązań, zobacz [najlepsze rozwiązania magazynu danych SQL][SQL Data Warehouse Best Practices].  

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

<!--MSDN references-->  
[Cardinality Estimation]: https://msdn.microsoft.com/library/dn600374.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistics]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[sys.tables]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  
