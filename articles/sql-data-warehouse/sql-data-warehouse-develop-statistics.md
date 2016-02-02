<properties
   pageTitle="SQL Data Warehouse での統計の管理 | Microsoft Azure"
   description="ソリューション開発のための Azure SQL Data Warehouse での統計の管理に関するヒント。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="12/11/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>


# SQL Data Warehouse での統計の管理

 SQL Data Warehouse では、統計を使用して分散クエリのさまざまな実行方法のコストを評価します。 統計が正確であれば、クエリ オプティマイザーはクエリのパフォーマンスを向上させる高品質のクエリ プランを生成できます。

SQL Data Warehouse で実現されるクエリのパフォーマンスを得るためには、統計の作成と更新が重要となります。 このガイドでは、統計の概要を説明し、次の方法を示します。

- データベース設計の一環として統計を作成する
- データベース メンテナンスの一環として統計を更新する
- システム ビューとシステム関数を使用して統計を表示する

## 統計の概要

単一列統計は、1 つの列の値の範囲と度数に関する情報を含むオブジェクトです。 クエリ オプティマイザーでは、このヒストグラムを使用して、クエリ結果の行数を推定します。 これは、クエリを最適化する方法に関する決定に直接影響します。

複数列統計は、列のリストで作成される統計です。 この統計には、リストの最初の列の単一列統計に加え、密度と呼ばれる列間の相関関係情報が含まれます。 複合結合やグループ化などの一部の操作では、複数列統計によってクエリのパフォーマンスを向上させることができます。

詳細については、次を参照してください。 [DBCC SHOW_STATISTICS:operator[]][] MSDN にします。

## 統計が必要な理由

適切な統計がない場合、SQL Data Warehouse で実現されるパフォーマンスは得られません。 SQL Data Warehouse では、テーブルと列の統計は自動的には生成されないので、開発者が自分で作成する必要があります。 テーブルの作成時に統計を作成し、統計を設定したら統計を更新することをお勧めします。
> [AZURE.NOTE] SQL Server を使用する場合、必要に応じて単一列統計の作成と更新が自動的に実行されます。 SQL Data Warehouse はこの点で異なります。 SQL Data Warehouse では、データが分散しているため、すべての分散データにわたる統計が自動的に集計されるわけではありません。 集計された統計が生成されるのは、開発者が統計を作成および更新したときだけです。

## 統計を作成する場合

最新の統計の一貫性のあるセットは、SQL Data Warehouse の重要な部分です。 そのため、テーブルのデザインの一環として統計を作成することが重要です。

統計を開始する簡単な方法は、すべての列の単一列統計を作成することです。 ただし、パフォーマンスと統計を作成および更新するコストの間には常にトレードオフの関係が存在します。 すべての列の単一列統計を作成し、すべての統計を更新するのに時間がかかりすぎることが後でわかった場合は、統計の一部をいつでも削除できます。また、一部の統計を他の統計よりも頻繁に更新することもできます。

複数列統計は、列が複合結合句やグループ化句に含まれる場合にのみ、クエリ オプティマイザーで使用されます。 現在、複合フィルターでは複数列統計のメリットは得られません。

SQL Data Warehouse の開発を開始するときにそのためは、次のパターンを実装することをお勧めします。
- すべてのテーブルのすべての列に対する単一列統計を作成します。
- By 句に結合し、グループ内のクエリで使用される列には、複数列統計を作成します。

データを照会する方法を把握したら、特にテーブルの幅が広い場合に、このモデルを改良することもできます。 さらに高度な手法については、「統計管理の実装」(## 統計管理の実装) をご覧ください。

## 統計を更新する場合

統計の更新をデータベース管理ルーチンに含めることが重要です。 データベース内のデータの分布が変わったら、統計を更新する必要があります。 そうしないと、クエリのパフォーマンスが十分に最適化されない可能性があり、労力をかけてクエリのトラブルシューティングをさらに行うだけの価値がなくなります。

したがって、クエリのトラブルシューティングを行うときに最初に尋ねる質問の 1 つは、「統計は最新の状態ですか」です。

この質問は時間で答えることができるものではありません。 最新の統計オブジェクトが非常に古い可能性もあります。 特定の列の値の分布で行数やデータが変わった場合は、統計を更新する必要があります。**

たとえば、データ ウェアハウスの日付列では、通常、統計を頻繁に更新する必要があります。 新しい行がデータ ウェアハウスに読み込まれるたびに、新しい読み込みの日付またはトランザクションの日付が追加されます。 これらによってデータの分布が変わり、統計が古くなります。

一方、顧客テーブルの性別列の統計は更新する必要がないと考えられます。 顧客間で分布が一定であると仮定すると、テーブル バリエーションに新しい行を追加しても、データの分布が変わることはありません。 ただし、データ ウェアハウスに 1 つの性別しか含まれておらず、新しい要件によって複数の性別が含まれるようになった場合は、性別列の統計を更新する必要があることは明らかです。

詳細については、次を参照してください。 [統計情報 []][] MSDN にします。

## 統計管理の実装

多くの場合、読み込みの終わりに統計が確実に更新されるように、データ読み込みプロセスを拡張することが推奨されます。 テーブルのサイズや値の分布が変わる頻度が最も高いのがデータの読み込み時です。 したがって、これが管理プロセスを実装する論理的な場所となります。

読み込みプロセスで統計を更新する際の基本原則は、次のとおりです。

- 読み込まれた各テーブルに更新された統計オブジェクトが少なくとも 1 つは含まれていることを確認します。 これにより、統計の更新の一環として、テーブル サイズ (行数とページ数) 情報が更新されます。
- JOIN、GROUP BY、ORDER BY、DISTINCT の各句に関与している列を重視します。
- トランザクションの日付などの "昇順キー" 列の値は、統計ヒストグラムに含まれないため、これらの列の更新頻度を増やすことを検討します。
- 静的な分布列の更新頻度を減らすことを検討します。
- 各統計オブジェクトは系列で更新されることに注意してください。実装するだけ `UPDATE STATISTICS < TABLE_NAME >` 特に、多数の統計オブジェクトの幅の広いテーブルの場合の理想的なことができない可能性があります。

> [AZURE.NOTE] [昇順キー] の詳細については、SQL Server 2014 の基数推定モデルに関するホワイト ペーパーをご覧ください。

詳細については、次を参照してください。  [基数の推定 []][] MSDN にします。

## 例: 統計の作成

以下の例では、さまざまなオプションを使用して統計を作成する方法を示します。 各列に使用するオプションは、データの特性とクエリでの列の使用方法によって異なります。

### A.既定のオプションを使用した単一列統計の作成

列の統計を作成するには、統計オブジェクトの名前と列の名前を指定するだけです。

次の構文では、既定のオプションをすべて使用しています。 既定では、SQL Data Warehouse は統計を作成するときに、テーブルの 20% をサンプリングします。

```
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

次に例を示します。

```
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### B.すべての列の検査による単一列統計の作成

ほとんどの場合、20% という既定のサンプリング レートで十分です。 ただし、サンプリング レートを調整することもできます。

テーブル全体をサンプリングするには、次の構文を使用します。

```
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

次に例を示します。

```
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### C.サンプル サイズを指定した単一列統計の作成

サンプル サイズをパーセントで指定することもできます。

```
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### D.一部の行のみの単一列統計の作成

テーブルの一部の行の統計を作成することもできます。 これは、フィルター選択された統計と呼ばれます。

たとえば、大規模なパーティション テーブルの特定のパーティションのクエリを計画するときに、フィルター選択された統計を使用できます。 パーティション値のみで統計を作成すると、統計の精度が向上するので、クエリのパフォーマンスが向上します。

次の例では、値の範囲の統計を作成します。 パーティションの値の範囲に一致する値を簡単に定義できます。

```
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [AZURE.NOTE] クエリ オプティマイザーが分散クエリ プランを選択するときに、フィルター選択された統計の使用も考慮されるようにするには、クエリが統計オブジェクトの定義の範囲内である必要があります。 前の例では、クエリの WHERE 句で col1 の値として 2000101 ～ 20001231 の値を指定する必要があります。

### E.すべてのオプションを使用した単一列統計の作成

オプションは組み合わせることができます。 次の例では、カスタム サンプル サイズを指定してフィルター選択された統計オブジェクトを作成します。

```
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

完全な参照を参照してください。 [CREATE STATISTICS []][] MSDN にします。

### F.複数列統計の作成

複数列統計を作成するには、これまでの例を使用するだけですが、複数の列を指定します。
> [AZURE.NOTE] クエリ結果の行数の推定に使用されるヒストグラムは、統計オブジェクト定義に示されている最初の列にのみ使用できます。

この例では、ヒストグラムは *product\_category*します。 列間の統計は、計算 *product\_category* と *product\_sub_c\ategory*:

```
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

間の相関関係があるため *product\_category* と *product\_sub\_category*, 、複数列統計は、これらの列が、同時にアクセスする場合に役立ちます。

### G.テーブルのすべての列の統計の作成

統計を作成する方法の 1 つとして、テーブルの作成後に CREATE STATISTICS コマンドを発行します。

```
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

### H.ストアド プロシージャを使用した、データベース内のすべての列の統計の作成

SQL Data Warehouse が、システム ストアド プロシージャと同じ [sp_create_stats:operator[]][] SQL Server でします。 このストアド プロシージャは、まだ統計がないデータベースのすべての列の単一列統計オブジェクトを作成します。

これは、データベースの設計を開始する際に役立ちます。 ニーズに合わせて、このオブジェクトを自由に変更できます。

```
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
LEFT JOIN   sys.[external_tables] e ON  e.[object_id]       = t.[object_id]
WHERE       l.[object_id] IS NULL
AND         e.[object_id] IS NULL -- not an external table
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

このプロシージャを使用して、テーブルのすべての列の統計を作成するには、プロシージャを呼び出すだけです。

```
prc_sqldw_create_stats;
```

## Examples: update statistics

To update statistics, you can:

1. Update one statistics object. Specify the name of the statistics object you wish to update.
2. Update all statistics objects on a table. Specify the name of the table instead of one specific statistics object.


### A. Update one specific statistics object ###
Use the following syntax to update a specific statistics object:
```
更新プログラムの統計情報 [schema_name] です。[table_name]([stat_name]);
```

For example:
```
更新プログラムの統計情報 [dbo] です。[table1]([stats_col1])。
```

By updating specific statistics objects, you can minimize the time and resources required to manage statistics. This requires some thought, though, to choose the best statistics objects to update.


### B. Update all statistics on a table ###
This shows a simple method for updating all the statistics objects on a table.
```
更新プログラムの統計情報 [schema_name] です。[table_name] です。
```

For example:
```
UPDATE STATISTICS dbo.table1 です。
```

This statement is easy to use. Just remember this updates all statistics on the table, and therefore might perform more work than is necessary. If the performance is not an issue, this is definitely the easiest and most complete way to guarantee statistics are up-to-date.

> [AZURE.NOTE] When updating all statistics on a table, SQL Data Warehouse does a scan to sample the table for each statistics. If the table is large, has many columns, and many statistics, it might be more efficient to udpate individual statistics based on need.

For an implementation of an `UPDATE STATISTICS` procedure please see the [temporary tables] article. The implementation method is slightly different to the `CREATE STATISTICS` procedure above but the end result is the same.

For the full syntax, see [Update Statistics][] on MSDN.

## Statistics metadata
There are several system view and functions that you can use to find information about statistics. For example, you can see if a statistics object might be out-of-date by using the stats-date function to see when statistics were last created or updated.

### Catalog views for statistics
These system views provide information about statistics:

| Catalog View | Description |
| :----------- | :---------- |
| [sys.columns][]  | One row for each column. |
| [sys.objects][]  | One row for each object in the database. |  |
| [sys.schemas][]  | One row for each schema in the database. |  |
| [sys.stats][] | One row for each statistics object. |
| [sys.stats_columns][] | One row for each column in the statistics object. Links back to sys.columns. |
| [sys.tables][] | One row for each table (includes external tables). |
| [sys.table_types][] | One row for each data type. |


### System functions for statistics
These system functions are useful for working with statistics:

| System Function | Description |
| :-------------- | :---------- |
| [STATS_DATE][]    | Date the statistics object was last updated. |
| [DBCC SHOW_STATISTICS][] | Provides summary level and detailed information about the distribution of values as understood by the statistics object. |

### Combine statistics columns and functions into one view

This view brings columns that relate to statistics, and results from the [STATS_DATE()][]function together.
```
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm. [schema_name] として [名前]
,       tb.[名前]                          [Table_name] として
、st. [stats_name] として [名前]
、st. [filter_definition] として [stats_filter_defiinition]
、st. [has_filter] として [stats_is_filtered]
、STATS_DATE(st.[object_id],st.[stats_id])
                                            [Stats_last_updated_date] として
、co. [stats_column_name] として [名前]
,       ty.[名前]                          [Column_type] として
、co. [max_length] として [column_max_length]
、co. [column_precision] として [精度]
、co. [スケール] として [column_scale]
、co. [によって is_nullable] として [column_is_nullable]
、co. [collation_name] として [column_collation_name]
、QUOTENAME(sm.[name]) +'。 '+ QUOTENAME(tb.[name])
                                            Two_part_name として
、QUOTENAME(DB_NAME()) +'。 '+ QUOTENAME(sm.[name]) +'. '+ QUOTENAME(tb.[name])
                                            Three_part_name として
Sys.objects から ob として
St ON ob. [object_id] として結合 sys.stats = st. [object_id]
Sc ON st. [stats_id] として結合 sys.stats_columns = sc. [stats_id]
                            St. [object_id] = sc. [object_id]
Co ON sc. [column_id] として結合 sys.columns = co. [column_id]
                            Sc. [object_id] = [object_id] co.
Ty ON co. [user_type_id] として結合 sys.types = ty します。[user_type_id]
Tb ON co. [object_id] として結合 sys.tables = tb です。[object_id]
Sm ON TB sys.schemas に参加します。[schema_id] = sm. [schema_id]
ここで 1 = 1 
St. [user_created] = 1
;
```

## DBCC SHOW_STATISTICS() examples

DBCC SHOW_STATISTICS() shows the data held within a statistics object. This data comes in three parts.

1. Header
2. Density Vector
3. Histogram

The header metadata about the statistics. The histogram displays the distribution of values in the first key column of the statistics object. The density vector measures cross-column correlation. SQLDW computes cardinality estimates with any of the data in the statistics object.

### Show header, density, and histogram

This simple example shows all three parts of a statistics object.
```
DBCC SHOW_STATISTICS で ([<schema_name>.<table_name>]、<stats_name>)
```

For example:
```
DBCC SHOW_STATISTICS (dbo.table1、stats_col1) です。
```

### Show one or more parts of DBCC SHOW_STATISTICS();

If you are only interested in viewing specific parts, use the `WITH` clause and specify which parts you want to see:
```
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) と stat_header、ヒストグラム、density_vector など
```

For example:
```
DBCC SHOW_STATISTICS (dbo.table1、stats_col1) WITH ヒストグラム、density_vector など
```

## DBCC SHOW_STATISTICS() の相違点

SQL Server に比べ、SQL Data Warehouse では、DBCC SHOW_STATISTICS() がより厳密に実装されています。

1. ドキュメントに記載されていない機能はサポートされていません。
- Stats_stream は使用できません。
- 統計データの特定のサブセットの結果を結合することはできません (STAT_HEADER JOIN DENSITY_VECTOR など)。
2. メッセージを抑制するために、NO_INFOMSGS を設定することはできません。
3. 統計名を囲む角かっこは使用できません。
4. 列名を使用して、統計オブジェクトを識別することはできません。
5. カスタム エラー 2767 はサポートされていません。


## 次のステップ

他の開発のヒントを参照してください。 [SQL Data Warehouse の開発の概要 []][]します。






[sql data warehouse development overview]: ./sql-data-warehouse-overview-develop.md 
[temporary tables]: ./sql-data-warehouse-develop-temporary-tables.md 
[cardinality estimation]: https://msdn.microsoft.com/library/dn600374.aspx 
[create statistics]: https://msdn.microsoft.com/library/ms188038.aspx 
[dbcc show_statistics]: https://msdn.microsoft.com/library/ms174384.aspx 
[statistics]: https://msdn.microsoft.com/library/ms190397.aspx 
[stats_date]: https://msdn.microsoft.com/library/ms190330.aspx 
[sys.columns]: https://msdn.microsoft.com/library/ms176106.aspx 
[sys.objects]: https://msdn.microsoft.com/library/ms190324.aspx 
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx 
[sys.stats]: https://msdn.microsoft.com/library/ms177623.aspx 
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx 
[sys.tables]: https://msdn.microsoft.com/library/ms187406.aspx 
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx 
[update statistics]: https://msdn.microsoft.com/library/ms187348.aspx 

