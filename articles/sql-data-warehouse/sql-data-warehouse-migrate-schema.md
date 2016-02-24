<properties
   pageTitle="SQL Data Warehouse へのスキーマの移行 | Microsoft Azure"
   description="ソリューション開発のための Azure SQL Data Warehouse へのスキーマの移行に関するヒント。"
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
   ms.date="09/22/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>

# SQL Data Warehouse へのスキーマの移行#

次の概要を確認して、SQL Server と SQL Data Warehouse の相違を理解し、データベースの移行に役立ててください。

### テーブルの機能
SQL Data Warehouse では、次の機能は使用またはサポートされません。

- 主キー
- 外部キー
- CHECK 制約
- UNIQUE 制約
- 一意のインデックス
- 計算列
- スパース列
- ユーザー定義型
- インデックス付きビュー
- ID
- シーケンス
- トリガー
- シノニム

### データ型の相違
SQL Data Warehouse では、次の一般的なビジネス データ型がサポートされています。

- bigint
- binary
- bit
- char
- date
- datetime
- datetime2
- datetimeoffset
- decimal
- float
- int
- money
- nchar
- nvarchar
- real
- smalldatetime
- smallint
- smallmoney
- time
- tinyint
- varbinary
- varchar

次のクエリを使用して、互換性のない型を含むデータ ウェアハウス内の列を特定できます。

```
SELECT  t.[name]
,       c.[name]
,       c.[system_type_id]
,       c.[user_type_id]
,       y.[is_user_defined]
,       y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN
                (   'geography'
                ,   'geometry'
                ,   'hierarchyid'
                ,   'image'
                ,   'ntext'
                ,   'numeric'
                ,   'sql_variant'
                ,   'sysname'
                ,   'text'
                ,   'timestamp'
                ,   'uniqueidentifier'
                ,   'xml'
                )

OR  (   y.[name] IN (  'nvarchar','varchar','varbinary')
    AND c.[max_length] = -1
    )
OR  y.[is_user_defined] = 1
;

```

このクエリには、サポートされていないユーザー定義データ型も含まれています。

データベースにサポーサポートされていない型をデータベースで使用していても心配ありません。 代わりに使用できる代替の型があります。

代替のデータ型は次のとおりです。

- **ジオメトリ**, 、varbinary 型の使用
- **geography**, 、varbinary 型の使用
- **hierarchyid**, 、この CLR 型がサポートされていません
- **イメージ**, 、**テキスト**, 、**ntext**, 、/、varchar/nvarchar を使用します (小さいほど良好)
- **nvarchar (max)**, 、nvarchar (4000) を使用するか、パフォーマンス向上のためより小さい
- **数値**, 、decimal を使用します
- **sql_variant**, 分割された、厳密に型指定の列を複数の列に
- **sysname**, 、nvarchar (128) を使用して
- **テーブル**, 、一時テーブルに変換します
- **タイムスタンプ**, 、datetime2 を使用するコードを再作業と `CURRENT_TIMESTAMP` 関数です。 current_timestamp を既定の制約として指定することはできません。また、値は自動的には更新されません。 timestamp で型指定された列から rowversion 値を移行する必要がある場合は、行バージョンの値 NOT NULL または NULL に binary(8) または varbinary(8) を使用します。
- **varchar (max)**, 、varchar (8000) を使用するか、パフォーマンス向上のためより小さい
- **uniqueidentifier**, 、varbinary (8) を使用して
- **ユーザー定義型**, 、可能なネイティブ型に変換します
- **xml**, 、パフォーマンス向上のため、varchar (8000) 以下を使用します。 必要に応じて、列全体を分割します。

部分的なサポート:

- 既定の制約では、リテラルと定数のみがサポートされます。 `GETDATE()` や `CURRENT_TIMESTAMP` など、不明確な式または関数はサポートされていません。

> [AZURE.NOTE] 可変長列の全長を含め、行の最大サイズが 32,767 バイトを超えないように、テーブルを定義します。 この数値を超える可能性のある可変長データを含む行を定義することはできますが、データをテーブルに挿入できなくなります。 また、クエリを実行する際のスループットをさらに向上させるために、可変長列のサイズを制限してください。

## 次のステップ
SQLDW にデータベース スキーマを正常に移行した後、次の記事のいずれかに進むことができます。

- [データの移行][]
- [コードの移行][]

他の開発のヒントについては、[開発の概要] を参照してください。

<!--Image references-->

<!--Article references-->
[Migrate your code]: sql-data-warehouse-migrate-code.md
[Migrate your data]: sql-data-warehouse-migrate-data.md
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->


<!--Other Web references-->

