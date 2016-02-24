<properties
   pageTitle="SQL Data Warehouse の設計に関する設計上の決定とコーディング技法 | Microsoft Azure"
   description="SQL Data Warehouse に関する開発コンセプト、設計上の決定、推奨事項、およびコーディング技法。"
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

# SQL Data Warehouse の設計上の決定と コーディング技法

SQL Data Warehouse に関する主要な設計上の決定、推奨事項、およびコーディング技法をより深く理解するために、開発に関する次の記事に目を通してください。

## 主要な設計上の決定
次の記事には、SQL Data Warehouse を使用した分散データ ウェアハウスの開発を理解するのに必要ないくつかの主な概念と設計上の決定事項が概説されています。

- [接続][]
- [同時実行制御][]
- [トランザクション][]
- [ユーザー定義スキーマ][]
- [テーブルのデザイン][]
- [ハッシュ分散キー][]
- [テーブルのパーティション][]
- [CTAS][]
- [統計][]

## 開発における推奨事項とコーディング技法
次の記事には、SQL Data Warehouse を開発するための具体的なコーディング技法、ヒント、および推奨事項が概説されています。

- [ストアド プロシージャ][]
- [ラベル][]
- [ビュー][]
- [一時テーブル][]
- [動的 SQL][]
- [ループ][]
- [オブジェクトの名前変更][]
- [データのピボット][]
- [オプションを使用してグループ][]
- [変数の割り当て][]

## 次のステップ
開発に関する記事重ねてきたと見て、 [TRANSACT-SQL リファレンス][] SQL Data Warehouse でサポートされる構文の詳細についてはページです。

<!--Image references-->

<!--Article references-->
[concurrency]: sql-data-warehouse-develop-concurrency.md
[connections]: sql-data-warehouse-develop-connections.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[dynamic SQL]: sql-data-warehouse-develop-dynamic-sql.md
[group by options]: sql-data-warehouse-develop-group-by-options.md
[hash distribution keys]: sql-data-warehouse-develop-hash-distribution-key.md
[labels]: sql-data-warehouse-develop-label.md
[looping]: sql-data-warehouse-develop-loops.md
[pivoting data]: sql-data-warehouse-develop-pivot-unpivot.md
[renaming objects]: sql-data-warehouse-develop-rename.md
[statistics]: sql-data-warehouse-develop-statistics.md
[stored procedures]: sql-data-warehouse-develop-stored-procedures.md
[table design]: sql-data-warehouse-develop-table-design.md
[table partitions]: sql-data-warehouse-develop-table-partitions.md
[temporary tables]: sql-data-warehouse-develop-temporary-tables.md
[transactions]: sql-data-warehouse-develop-transactions.md
[user-defined schemas]: sql-data-warehouse-develop-user-defined-schemas.md
[variable assignment]: sql-data-warehouse-develop-variable-assignment.md
[views]: sql-data-warehouse-develop-views.md

[Transact-SQL reference]: sql-data-warehouse-overview-reference.md

<!--MSDN references-->

<!--Other Web references-->

