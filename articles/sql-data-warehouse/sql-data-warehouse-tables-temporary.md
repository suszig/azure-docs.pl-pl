---
title: "Tabele tymczasowe w usłudze SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do tabel tymczasowych w usłudze Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 9b1119eb-7f54-46d0-ad74-19c85a2a555a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: e3b2f9017ecea7d9f78c07476f96c3dd8d031863
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="temporary-tables-in-sql-data-warehouse"></a>Tabele tymczasowe w usłudze SQL Data Warehouse
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

Tabele tymczasowe są przydatne podczas przetwarzania danych — szczególnie podczas transformacji skutkującej przejściowej pośrednich wyników. W usłudze SQL Data Warehouse tabele tymczasowe istnieją na poziomie sesji.  Tylko są widoczne dla sesji, w której zostały utworzone i są automatycznie usuwane po wylogowaniu tej sesji.  Tabele tymczasowe oferują poprawiać wydajność, ponieważ ich wyniki są zapisywane w lokalnym zamiast Magazyn zdalny.  Tabele tymczasowe są nieco inne w usłudze Azure SQL Data Warehouse niż baza danych SQL Azure, ponieważ są one dostępne z dowolnego miejsca w danej sesji, łącznie z wewnątrz oraz poza procedury składowanej.

Ten artykuł zawiera podstawowe wskazówki dotyczące korzystania z tabel tymczasowych i zaznacza zasadami poziomu tabel tymczasowych sesji. Korzystając z informacji w tym artykule może pomóc modularyzacji kodu, poprawy zarówno możliwość ponownego wykorzystania i łatwości obsługi kodu.

## <a name="create-a-temporary-table"></a>Tworzenie tabeli tymczasowej
Tabele tymczasowe są tworzone przez prefiksu nazwy tabeli z `#`.  Na przykład:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]        NVARCHAR(128) NOT NULL
,    [table_name]            NVARCHAR(128) NOT NULL
,    [stats_name]            NVARCHAR(128) NOT NULL
,    [stats_is_filtered]     BIT           NOT NULL
,    [seq_nmbr]              BIGINT        NOT NULL
,    [two_part_name]         NVARCHAR(260) NOT NULL
,    [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
```

Można również tworzyć tabel tymczasowych z `CTAS` przy użyciu dokładnie te same podejście:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

> [!NOTE]
> `CTAS`polecenie wydajne i ma dodatkową zaletą jest efektywne przy jego użyciu miejsca w dzienniku transakcji. 
> 
> 

## <a name="dropping-temporary-tables"></a>Porzucanie tabel tymczasowych
Po utworzeniu nowej sesji, powinny istnieć nie tabel tymczasowych.  Jednak jeśli wywołujesz tej samej procedury składowanej tworzy tymczasowy o takiej samej nazwie, upewnij się, że Twoje `CREATE TABLE` instrukcje są pomyślnie proste Sprawdzanie istnienia wstępne z `DROP` można używać jak w poniższym przykładzie:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Kodowania spójności, dobrym rozwiązaniem jest do użycia tego wzorca dla tabel i tabelach tymczasowych.  Istnieje również warto użyć `DROP TABLE` do usuwania tabel tymczasowych, gdy zostanie zakończone z nimi w kodzie.  Do rozwoju procedury składowanej jest częściej można zobaczyć, że poleceń drop powiązane ze sobą na końcu procedury, aby upewnić się, że te obiekty są czyszczone.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing kodu
Ponieważ tabele tymczasowe są widoczne dowolne miejsce w sesji użytkownika, może zostać wykorzystana ułatwiające modularyzacji kodu aplikacji.  Na przykład następującą procedurę składowaną generuje DDL, aby zaktualizować wszystkie statystyki w bazie danych o nazwie statystyki.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

Na tym etapie, jedyną akcją, którą wystąpił jest tworzenie procedury składowanej tego generatess stats_ddl #, z instrukcji DDL tabeli tymczasowej.  Tę procedurę składowaną porzuca #stats_ddl, jeśli istnieje już upewnij się, że nie zostanie uruchamiania więcej niż raz w ramach sesji.  Jednak ponieważ nie istnieje żadne `DROP TABLE` na końcu procedury składowanej, po ukończeniu procedury składowanej pozostawia utworzonej tabeli, dzięki czemu mogą być odczytywane poza procedury składowanej.  W usłudze SQL Data Warehouse w przeciwieństwie do innych baz danych programu SQL Server, jest możliwość użycia tabeli tymczasowej poza procedury, który go utworzył.  Tabele tymczasowe SQL Data Warehouse może służyć **dowolnym** wewnątrz sesji. Może to prowadzić do więcej moduły i łatwą w obsłudze, jak w poniższym przykładzie kodu:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Ograniczenia tabeli tymczasowej
Usługa SQL Data Warehouse nakładają kilka ograniczeń podczas implementowania tabel tymczasowych.  Obecnie tylko sesji zakresami tabel tymczasowych są obsługiwane.  Globalne tabele tymczasowe są nieobsługiwane.  Ponadto widoków nie można utworzyć na tabelach tymczasowych.

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej, zobacz artykuły w [omówienie tabeli][Overview], [typy danych tabeli][Data Types], [Dystrybucja tabeli] [ Distribute], [Indeksowania tabeli][Index], [partycjonowania tabeli] [ Partition] i [Utrzymania statystyk tabeli][Statistics].  Aby uzyskać więcej informacji na temat najlepszych rozwiązań, zobacz [najlepsze rozwiązania magazynu danych SQL][SQL Data Warehouse Best Practices].

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

<!--Other Web references-->
