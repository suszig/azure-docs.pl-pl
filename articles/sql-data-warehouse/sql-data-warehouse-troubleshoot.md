<properties
   pageTitle="トラブルシューティング |Microsoft Azure"
   description="SQL Data Warehouse のトラブルシューティングを行います。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="TwoUnder"
   manager=""
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="12/11/2015"
   ms.author="twounder"/>


# トラブルシューティング

次のトピックでは、Azure SQL Data Warehouse で発生するいくつかの一般的な問題を示します。

## 接続

Azure SQL Data Warehouse への接続は、次の 2 つの一般的な原因によって失敗する可能性があります。

- ファイアウォール ルールが設定されていない
- サポートされていないツール/プロトコルを使用している

### ファイアウォール ルール

Azure SQL Database は、既知の IP アドレスのみがデータベースにアクセスできるように、サーバーとデータベース レベルのファイアウォールによって保護されています。 ファイアウォールは、既定でセキュリティ保護されています。つまり、接続する前に、IP アドレスにアクセスを許可する必要があります。

アクセスのためにファイアウォールを構成する」の手順に従ってください、 [クライアントの ip アドレスのサーバーのファイアウォール アクセスの構成](sql-data-warehouse-get-started-provision.md/#step-4-configure-server-firewall-access-for-your-client-ip) のセクションで、 [プロビジョニング](sql-data-warehouse-get-started-provision.md) ページです。

### サポートされていないツール/プロトコルを使用している

SQL Data Warehouse はサポート [Visual Studio 2013/2015年](sql-data-warehouse-get-started-connect.md) 開発環境として、 [SQL Server Native Client (ODBC) の 10/11](https://msdn.microsoft.com/library/ms131415.aspx) クライアント接続にします。

参照してください、 [接続](sql-data-warehouse-get-started-connect.md) の詳細ページです。

## クエリのパフォーマンス

SQL Data Warehouse では、統計を含むクエリの実行で SQL Server の一般的なコンストラクトを使用します。 [統計](sql-data-warehouse-develop-statistics.md) 範囲とデータベース列の値の頻度に関する情報を含むオブジェクトです。 クエリ エンジンは、これらの統計情報を使用してクエリの実行を最適化し、クエリのパフォーマンスを向上させます。 次のクエリを使用して、統計情報が更新された最後の時刻を確認できます。

```
SELECT
    sm.[name]                                   AS [schema_name],
    tb.[name]                                   AS [table_name],
    co.[name]                                   AS [stats_column_name],
    st.[name]                                   AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id])    AS [stats_last_updated_date]
FROM
    sys.objects             AS ob
    JOIN sys.stats          AS st   ON  ob.[object_id]      = st.[object_id]
    JOIN sys.stats_columns  AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
    JOIN sys.columns        AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
    JOIN sys.types           AS ty  ON  co.[user_type_id]   = ty.[user_type_id]
    JOIN sys.tables          AS tb  ON  co.[object_id]      = tb.[object_id]
    JOIN sys.schemas         AS sm  ON  tb.[schema_id]      = sm.[schema_id]
WHERE
    1=1 
    AND st.[user_created] = 1;
```

参照してください、 [統計](sql-data-warehouse-develop-statistics.md) の詳細ページです。

## 主要なパフォーマンスの概念

その他の主要なパフォーマンスとスケールの概念を理解するために、次の記事を参照してください。

- [パフォーマンスとスケールの][]
- [同時実行モデル][]
- [テーブルのデザイン][]
- [ハッシュ分散キーのテーブル、の選択します。][]
- [のパフォーマンスを向上させる統計][]

## 次のステップ

参照してください、 [開発の概要][] に関する記事を SQL Data Warehouse ソリューションを構築するためのアドバイスします。












[performance and scale]: sql-data-warehouse-performance-scale.md 
[concurrency model]: sql-data-warehouse-develop-concurrency.md 
[designing tables]: sql-data-warehouse-develop-table-design.md 
[choose a hash distribution key for your table]: sql-data-warehouse-develop-hash-distribution-key 
[statistics to improve performance]: sql-data-warehouse-develop-statistics.md 
[development overview]: sql-data-warehouse-overview-develop.md 

