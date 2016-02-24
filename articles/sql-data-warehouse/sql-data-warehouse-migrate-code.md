<properties
   pageTitle="SQL Data Warehouse への SQL コードの移行 | Microsoft Azure"
   description="ソリューション開発のための Azure SQL Data Warehouse への SQL コードの移行に関するヒント"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="12/09/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>

# SQL Data Warehouse への SQL コードの移行

コードが SQL Data Warehouse と必ず準拠するようにするには、多くの場合、コード ベースに変更を加える必要があります。 一部の SQL Data Warehouse 機能は分散環境で直接機能するように設計されているため、大幅にパフォーマンスを向上できます。 ただし、パフォーマンスと拡張性を維持するには、一部の機能が使用できなくなる場合もあります。

## Transact-SQL コードの変更点

Azure SQL Data Warehouse でサポートされていない主な機能を次の表に示します。 リンクをクリックすると、サポートされていない機能に対する解決策が表示されます。

- [更新プログラムでの ANSI の join][]
- [削除時に ANSI 結合][]
- [merge ステートメント][]
- 複数データベースの JOIN
- [カーソル][]
- [SELECT..INTO][]
- [挿入]EXEC]
- OUTPUT 句
- インライン ユーザー定義関数
- 複数ステートメント関数
- [共通テーブル式](#Common-table-expressions)
- [再帰共通テーブル式 (CTE)](#Recursive-common-table-expressions-(CTE)
- CLR 関数およびプロシージャ
- $partition 関数
- テーブル変数
- テーブル値パラメーター
- 分散トランザクション
- コミット/ロールバック処理
- トランザクションの保存
- 実行コンテキスト (EXECUTE AS)
- [rollup / cube / grouping セット オプションによる句ごとのグループ化][]
- [8 を超えるの入れ子のレベル][]
- [ビューを使用した更新][]
- [変数代入のための SELECT の使用][]
- [動的 SQL 文字列の MAX 以外のデータ型][]

幸運なことに、これらの制限の大部分は回避できます。 上記の関連する開発記事に説明が記載されています。

### 共通テーブル式
SQL Data Warehouse 内の共通テーブル式 (CTE) の現在の実装には、次の機能と制限事項があります。

**CTE の機能**
+ CTE は、SELECT ステートメントで指定できます。
+ CTE は、CREATE VIEW ステートメントで指定できます。
+ CTE は、作成テーブル AS SELECT (CTAS) ステートメントで指定できます。
+ CTE は、作成リモート テーブルとして選択 (CRTAS) ステートメントで指定できます。
+ CTE は、作成外部テーブルとして選択 (CETAS) ステートメントで指定できます。
+ リモート テーブルは、CTE から参照できます。
+ 外部テーブルは、CTE から参照できます。
+ 複数の CTE クエリ定義は、CTE を定義できます。

**CTE の制限事項**
+ CTE は、単一の SELECT ステートメントの後に指定する必要があります。 INSERT、UPDATE、DELETE、MERGE ステートメントはサポートされていません。
+ (再帰共通テーブル式) 自体への参照を含む、共通テーブル式がサポートされていません (セクションの下を参照してください)。
+ 句を使用して 1 つ以上の CTE 内で指定することはできません。 たとえば、CTE_query_definition にサブクエリが含まれている場合そのサブクエリ含めることはできません、入れ子になった別の CTE を定義する句を使用します。
+ ORDER BY 句は TOP 句が指定されている場合を除く、CTE_query_definition では使用できません。
+ ステートメントがバッチの一部では、CTE を使用するときに、セミコロンで前に、ステートメントの後にする必要があります。
+ Sp_prepare から準備された、ステートメントで使用されているときに Cte は PDW で他の SELECT ステートメントと同様に動作します。 ただし、CTE が sp_prepare で準備される CETAS の一部として使用される場合、バインドを sp_prepare に対して実装する方法によって、動作が SQL Server および他の PDW ステートメントとは異なる場合があります。 CTE を参照する SELECT が CTE に存在しない間違った列を使用している場合、sp_prepare はエラーを検出せずに渡されますが、代わりに sp_execute でエラーがスローされます。

### 再帰共通テーブル式 (CTE)

これは複雑な移行シナリオで、CTE を分解したり、順番に処理したりする場合に最適です。 通常、再帰的な中間クエリの反復処理時に、ループを使用したり、一時テーブルに値を取り込んだりできます。 一時テーブルに値が取り込まれたら、単一の結果セットとしてデータを戻すことができます。 解決するために、同様のアプローチが使用された `GROUP BY WITH CUBE` で、 [による句をグループ化/キューブ/オプションを設定をグループ化][] 記事です。

### システム関数

また、サポートされていないシステム関数もいくつかあります。 データ ウェアハウジングで一般的に使用されている主なものを次に示します。

- NEWID()
- NEWSEQUENTIALID()
- @@NESTLEVEL()
- @@IDENTITY()
- @@ROWCOUNT()
- ROWCOUNT_BIG
- ERROR_LINE()

これらの問題の大部分も回避できます。 

たとえば、次のコードは、@@ROWCOUNT 情報を取得するための代替ソリューションです。

```
SELECT  SUM(row_count) AS row_count 
FROM    sys.dm_pdw_sql_requests 
WHERE   row_count <> -1 
AND     request_id IN 
                    (   SELECT TOP 1    request_id 
                        FROM            sys.dm_pdw_exec_requests 
                        WHERE           session_id = SESSION_ID() 
                        ORDER BY end_time DESC
                    )
;
``` 

## 次のステップ
コード開発についてを参照してください、 [開発の概要][]します。

<!--Image references-->

<!--Article references-->
[ANSI joins on updates]: sql-data-warehouse-develop-ctas.md
[ANSI joins on deletes]: sql-data-warehouse-develop-ctas.md
[merge statement]: sql-data-warehouse-develop-ctas.md
[INSERT..EXEC]: sql-data-warehouse-develop-temporary-tables.md

[cursors]: sql-data-warehouse-develop-loops.md
[SELECT..INTO]: sql-data-warehouse-develop-ctas.md
[group by clause with rollup / cube / grouping sets options]: sql-data-warehouse-develop-group-by-options.md
[nesting levels beyond 8]: sql-data-warehouse-develop-transactions.md
[updating through views]: sql-data-warehouse-develop-views.md
[use of select for variable assignment]: sql-data-warehouse-develop-variable-assignment.md
[no MAX data type for dynamic SQL strings]: sql-data-warehouse-develop-dynamic-sql.md
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->

