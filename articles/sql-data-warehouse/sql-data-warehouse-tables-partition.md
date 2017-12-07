---
title: "Partycjonowanie tabel w usłudze SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: Wprowadzenie do partycjonowania tabeli w magazynie danych SQL Azure.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 6cef870c-114f-470c-af10-02300c58885d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: a28cb1f8a2e48332b344566620dc49b29d9d3c99
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="partitioning-tables-in-sql-data-warehouse"></a>Partycjonowanie tabel w usłudze SQL Data Warehouse
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

Partycjonowanie jest obsługiwana dla wszystkich typów tabeli SQL Data Warehouse; w tym klastrowanego magazynu kolumn, indeks klastrowany i stosu.  Partycjonowanie jest również obsługiwany na wszystkich typach dystrybucji, w tym zarówno skrótu lub działanie okrężne rozproszonych.  Włącza partycjonowanie do dzielenia danych na mniejsze grupy danych i w większości przypadków partycjonowania odbywa się według kolumny daty.

## <a name="benefits-of-partitioning"></a>Korzyści wynikające z partycjonowania
Partycjonowanie mogą korzystać wydajność obsługi i zapytań danych.  Określa, czy przynosi korzyści zarówno lub jeden z nich jest zależna od jak załadować danych i czy tej samej kolumnie może służyć do obu celów, ponieważ Partycjonowanie jest możliwe tylko w jednej kolumnie.

### <a name="benefits-to-loads"></a>Zalety dla obciążeń
Główną zaletą partycjonowania w usłudze SQL Data Warehouse jest zwiększenie wydajności i wydajności podczas ładowania danych przy użyciu usunięcia partycji, przełączania i scalanie.  W większości przypadków danych jest podzielona na partycje w kolumnie dat, która jest ściśle związany kolejność, w którym dane zostaną załadowane do bazy danych.  Jedną z największych korzyści wynikające ze stosowania partycje do przechowywania danych o jej unikania rejestrowanie transakcji.  Po prostu Wstawianie, aktualizowanie lub usuwanie danych mogą być Najprostszym rozwiązaniem z niewielką ilością myśl i nakładu pracy, przy użyciu podziału na partycje podczas procesu obciążenia znacznie może poprawić wydajność.

Przełączanie partycji umożliwia szybkie należy usunąć lub zamienić sekcji tabeli.  Na przykład tabela faktów sprzedaży może zawierać tylko dane z ostatnich miesięcy 36.  Na koniec każdego miesiąca miesiąc najstarsze dane sprzedaży został usunięty z tabeli.  Te dane, można go usunąć za pomocą instrukcji delete umożliwia usunięcie danych najstarsze miesiąc.  Jednak usunięcie dużych ilości danych wiersz po wierszu z instrukcji delete może trwać zbyt dużo czasu, a także spowodować ryzyko duże transakcje, które można wycofać zająć dużo czasu, jeśli jakaś nieprawidłowość.  Więcej optymalne podejście jest Porzuć partycję najstarsze danych.  Gdzie usuwanie poszczególnych wierszy może zająć godziny, usunięcie całego partycji może potrwać sekund.

### <a name="benefits-to-queries"></a>Korzyści dotyczące zapytań
Partycjonowanie można również poprawić wydajność zapytań.  Zapytanie, które dotyczą filtr danych podzielonej na partycje można ograniczyć skanowanie w celu kwalifikacji partycji. Ta metoda filtrowania można uniknąć skanowania tabeli pełne i tylko skanowanie mniejszego podzestawu danych. Wraz z wprowadzeniem klastrowane indeksy magazynu kolumn zwiększenia wydajności eliminacji predykatu są mniej korzystne, ale w niektórych przypadkach może być korzyści zapytania.  Na przykład jeśli tabela faktów sprzedaży jest podzielona na partycje do 36 miesięcy, korzystając z pola Data sprzedaży, a następnie wysyła zapytanie tego filtru na dacie sprzedaży można pominąć wyszukiwanie w partycji, które nie pasuje do filtru.

## <a name="partition-sizing-guidance"></a>Wskazówki dotyczące rozmiaru partycji
Podczas partycjonowania może służyć do zwiększenia wydajności w niektórych scenariuszach, tworzenia tabeli z **zbyt wiele** partycji może pogarszać wydajność w pewnych okolicznościach.  Te problemy są szczególnie istotne w przypadku tabel klastrowanego magazynu kolumn.  Partycjonowania być pomocne, ważne jest zrozumienie, kiedy należy używać partycjonowania i liczby partycji można utworzyć.  Nie twardych szybkiego zasady, jak wiele partycji są zbyt wiele, zależy od danych i jak wiele partycji są ładowane do jednocześnie.  Pomyślne schemat partycjonowania ma zazwyczaj dziesiątki setki partycji, nie tysięcy.

Podczas tworzenia partycji na **klastrowanego magazynu kolumn** tabel, ważne jest, aby wziąć pod uwagę liczbę wierszy należą do każdej partycji.  Optymalne kompresji i wydajności tabel klastrowanego magazynu kolumn co najmniej 1 milion wierszy na dystrybucji i partycja na potrzeby.  Przed utworzeniem partycji usługi SQL Data Warehouse już dzieli każdej tabeli 60 rozproszonej bazy danych.  Wszystkie partycje dodane do tabeli jest oprócz dystrybucje utworzone w tle.  Jeśli tabela faktów sprzedaży zawiera 36 miesięczne partycji i biorąc pod uwagę, że usługa SQL Data Warehouse ma 60 dystrybucje, następnie tabeli faktów sprzedaży powinien zawierać 60 mln wierszy na miesiąc lub 2.1 miliardy wierszy po wszystkich miesięcy są wypełniane przy użyciu tego przykładu.  Jeśli tabela zawiera znacznie mniejsza niż minimalna zalecana liczba wierszy przypadających na partycję, rozważ użycie mniejszej liczby partycji w celu zwiększenia liczby wierszy przypadających na partycję.  Zobacz też [indeksowanie] [ Index] artykułu, który obejmuje zapytania uruchamiane na magazyn danych SQL do oceny jakości klastra indeksy magazynu kolumn.

## <a name="syntax-difference-from-sql-server"></a>Różnica składni z programu SQL Server
Usługa SQL Data Warehouse wprowadza uproszczony sposób definiowania partycji, która jest nieco inne niż SQL Server.  Partycjonowania funkcje i schematy nie są używane w usłudze SQL Data Warehouse, ponieważ są one w programie SQL Server.  Zamiast tego jest wszystko, co należy zrobić, zidentyfikować kolumny podzielone na partycje i punkty granic.  Składnia partycjonowania może być nieco inne niż SQL Server, podstawowe koncepcje są takie same.  SQL Server i SQL Data Warehouse obsługuje jedna kolumna partycji w tabeli, które mogą być ranged partycji.  Aby dowiedzieć się więcej na temat partycjonowania, zobacz [partycjonowane tabele i indeksy][Partitioned Tables and Indexes].

Poniższy przykład przedstawia SQL Data Warehouse na partycje [CREATE TABLE] [ CREATE TABLE] instrukcji, partycje tabela FactInternetSales w kolumnie OrderDateKey:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Migrowanie partycjonowania z programu SQL Server
Aby przeprowadzić migrację definicje partycji programu SQL Server do usługi SQL Data Warehouse po prostu:

* Eliminowanie programu SQL Server [schemat partycji][partition scheme].
* Dodaj [funkcja partycji] [ partition function] definicji do tworzenia tabeli.

W przypadku migracji tabeli partycjonowanej z wystąpieniem programu SQL Server, SQL następujące może pomóc ustalić liczbę wierszy w każdej partycji.  Należy pamiętać, że użycie takiej samej szczegółowości partycjonowania na SQL Data Warehouse liczba wierszy przypadających na partycję zmniejsza się o 60.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Zarządzanie obciążeniami
Pierwsza kwestia ostatni element składników do partycji tabeli decyzja dotyczy [zarządzania obciążenia][workload management].  Zarządzanie obciążenia w usłudze SQL Data Warehouse jest głównie zarządzania pamięci i współbieżności.  W usłudze SQL Data Warehouse Maksymalna pamięć przydzielona do każdej dystrybucji podczas wykonywania zapytania podlega klasy zasobów.  W idealnym przypadku partycji są o rozmiarze, biorąc pod uwagę innych czynników, takich jak wymagania dotyczące pamięci tworzenia klastrowane indeksy magazynu kolumn.  Klastrowane korzyści indeksy magazynu kolumn znacznie, gdy są przydzielone więcej pamięci.  W związku z tym chcesz upewnić się, że odbudowywanie indeksu partycji nie jest zagłodzone pamięci. Zwiększenie ilości pamięci do zapytania można osiągnąć przełączyć domyślna rola smallrc, do jednego z innych ról, takich jak largerc.

Informacje dotyczące alokacji pamięci dla dystrybucji są dostępne, badając widoki dynamiczne zarządzanie zarządcy zasobów. W rzeczywistości Twojej przydział pamięci jest mniejsza niż poniższe rysunki. Zapewnia to jednak poziom wskazówki, które można używać podczas określania rozmiaru partycji dla danych operacji zarządzania.  Należy unikać zmiany rozmiaru partycji poza przydziału pamięci o rozmiarze udostępnianym przez klasę bardzo dużych zasobów. Jeśli partycji rosnąć poza tym rysunku grozi wykorzystania pamięci, co z kolei prowadzi do mniej optymalnej kompresji.

```sql
SELECT  rp.[name]                                AS [pool_name]
,       rp.[max_memory_kb]                        AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                    AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576                AS [mex_memory_gb]
,       rp.[max_memory_percent]                    AS [max_memory_percent]
,       wg.[name]                                AS [group_name]
,       wg.[importance]                            AS [group_importance]
,       wg.[request_max_memory_grant_percent]    AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups    wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools    rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Przełączanie partycji
Magazyn danych SQL obsługuje partycji dzielenia, łączenia i przełączania. Każda z tych funkcji jest excuted przy użyciu [instrukcji ALTER TABLE] [ ALTER TABLE] instrukcji.

Aby przełączyć partycji między dwiema tabelami musi upewnij się, że partycje są wyrównane na ich odpowiednich granice a definicje tabel są takie same. Jak ograniczenia sprawdzania nie są dostępne do wymuszania zakres wartości w tabeli tabeli źródłowej musi zawierać tej samej granice partycji w tabeli docelowej. Jeśli nie jest to możliwe, przełącznik partycji wystąpi błąd, ponieważ metadane partycji nie będą synchronizowane.

### <a name="how-to-split-a-partition-that-contains-data"></a>Sposób podziału partycji, która zawiera dane
Najbardziej efektywny sposób podział partycji, która zawiera już dane, jest użycie `CTAS` instrukcji. W przypadku tabeli partycjonowanej klastrowanego magazynu kolumn to partycji tabeli musi być pusty przed może zostać podzielony.

Poniżej znajdują się magazynu kolumn partycjonowane przykładową tabelę zawierającą po jednym wierszu w każdej partycji:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [!NOTE]
> Utworzenie obiektu Statystyka, możemy upewnij się, że tych metadanych tabeli jest bardziej dokładne. Jeśli firma Microsoft pominąć tworzenie statystyk, SQL Data Warehouse użyje wartości domyślnych. Dla Przejrzyj szczegóły dotyczące statystyk [statystyki][statistics].
> 
> 

Firma Microsoft może następnie wyszukiwać za pomocą liczby wierszy `sys.partitions` widoku katalogu:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Jeśli spróbujemy podzielenie tej tabeli, uzyskujemy błąd:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Msg 35346, 15 poziom, stan 1, wiersz 44 PODZIELONY klauzula instrukcji ALTER PARTITION nie powiodło się, ponieważ partycja nie jest pusty.  Można dzielić tylko puste partycje w gdy w tabeli istnieje indeks magazynu kolumn. Rozważ wyłączenie indeksu magazynu kolumn przed wykonaniem instrukcji ALTER PARTITION, a następnie odbudowanie indeksu magazynu kolumn po ukończeniu operacji ALTER PARTITION.

Jednak używamy `CTAS` Aby utworzyć nową tabelę do przechowywania danych.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Jako granice partycji są wyrównane przełącznik jest dozwolone. Spowoduje to pozostawienie tabeli źródłowej z pustą partycję, który następnie możemy podzielić.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Do wykonania jest, aby były wyrównane naszych danych do nowych granic partycji przy użyciu `CTAS` i przełącznik naszych danych ponownie do tabeli głównej

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Po zakończeniu przepływu danych jest dobrym pomysłem jest Odśwież statystyk dotyczących tabeli docelowej, aby upewnić się, że dokładnie odzwierciedlał nowej dystrybucji dane w ich odpowiednich partycji:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Tabela partycjonowania kontroli źródła
Aby uniknąć z definicji tabeli z **uszkodzona** w systemie kontroli źródła, warto rozważyć następujące podejście:

1. Tworzenie tabeli partycjonowanej tabeli, ale bez wartości partycji

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

1. `SPLIT`Tabela w ramach procesu wdrażania:

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

Z tej metody pozostaje statyczna kodu w kontroli źródła i partycjonowania wartości granicznych mogą być dynamiczny; ewolucji w magazynie wraz z upływem czasu.

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej, zobacz artykuły w [omówienie tabeli][Overview], [typy danych tabeli][Data Types], [Dystrybucja tabeli] [ Distribute], [Indeksowania tabeli][Index], [utrzymania statystyk tabeli] [ Statistics] i [Tabel tymczasowych][Temporary].  Aby uzyskać więcej informacji na temat najlepszych rozwiązań, zobacz [najlepsze rozwiązania magazynu danych SQL][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[workload management]: ./sql-data-warehouse-develop-concurrency.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Partitioned Tables and Indexes]: https://msdn.microsoft.com/library/ms190787.aspx
[ALTER TABLE]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[partition function]: https://msdn.microsoft.com/library/ms187802.aspx
[partition scheme]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->
