<properties
   pageTitle="DMV を利用してワークロードを監視する | Microsoft Azure"
   description="DMV を利用してワークロードを監視するについて説明します。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sahaj08"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="12/15/2015"
   ms.author="sahajs"/>

# DMV を利用してワークロードを監視する

この記事では、動的管理ビュー (DMV) を使用し、Azure SQL Data Warehouse でワークロードを監視し、クエリの実行を調査する方法について説明します。



## 接続を監視する

使用することができます、 *sys.dm_pdw_nodes_exec_connections* ビューは、Azure SQL Data Warehouse データベースに対して確立された接続に関する情報を取得します。 さらに、 *sys.dm_exec_sessions* ビューは、すべてのアクティブなユーザー接続に関する情報を取得するときに便利です。

```

SELECT * FROM sys.dm_pdw_nodes_exec_connections;
SELECT * FROM sys.dm_pdw_nodes_exec_sessions;

```


次のクエリを利用し、現在の接続に関する情報を取得できます。

```

SELECT * 
FROM sys.dm_pdw_nodes_exec_connections AS c 
   JOIN sys.dm_pdw_nodes_exec_sessions AS s 
   ON c.session_id = s.session_id 
WHERE c.session_id = @@SPID;

```





## クエリの実行を調査する
クエリが完了しなかったり、予想以上に時間がかかったりすることがあります。 そのような場合、次の手順でデータを回収したり、問題を絞り込んだりできます。



### 手順 1: 調査するクエリを見つける

```

-- Monitor running queries
SELECT * FROM sys.dm_pdw_exec_requests WHERE status = 'Running';

-- Find the longest running queries
SELECT * FROM sys.dm_pdw_exec_requests ORDER BY total_elapsed_time DESC;

```

クエリの要求 ID を保存します。


  
### 手順 2: クエリがリソースを待っているのか確認する

```

-- Find waiting tasks for your session.
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status, 
      requests.start_time,  
      waits.type,  
      waits.object_type, 
      waits.object_name,  
      waits.state  
FROM   sys.dm_pdw_waits waits 
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id 
WHERE waits.request_id = 'QID33188'
ORDER BY waits.object_name, waits.object_type, waits.state;

```


上記のクエリの結果から要求の待機状態が表示されます。

- クエリが別のクエリからのリソースで待機しているかどうかは、状態になります **AcquireResources**します。
- クエリが必要なすべてのリソースを持っているし、待機していないかどうか、状態がある **Granted**します。 その場合、クエリ手順を確認します。




### 手順 3: クエリの最長実行手順を見つける

要求 ID を利用し、分散されているすべてのクエリ手順の一覧を取得します。 経過時間の合計を見て、実行時間の長い手順を見つけます。 

```

-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.
 
SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID33209'
ORDER BY step_index;

```

実行時間の長い手順の手順インデックスを保存します。

チェック、 *operation_type* 実行時間の長いクエリのステップの列。

- 手順 4 a を読み進める **SQL 操作**: OnOperation RemoteOperation、ReturnOperation です。
- 手順 4 b を読み進める **データの移動操作**: ShuffleMoveOperation、BroadcastMoveOperation、TrimMoveOperation、PartitionMoveOperation、MoveOperation、CopyOperation です。




### 手順 4a: SQL 手順の実行進行状況を見つける

要求 ID と手順インデックスを利用し、クエリの SQL 手順の一環として、SQL Server クエリ分散に関する情報を取得します。 分散の ID と SPID を保存します。

```

-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID33209' AND step_index = 2;

```


次のクエリを利用し、特定のノードの SQL 手順の SQL Server 実行計画を取得します。

```

-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node. 
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);

```



### 手順 4b: DMS 手順の実行進行状況を見つける

要求 ID と手順インデックスを利用し、各配布で実行されているデータ移動手順に関する情報を取得します。 

```

-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.
 
SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID33209' AND step_index = 2;

```

- チェック、 *total_elapsed_time* 列のかどうかは、特定の分布がデータ移動のため他よりも大幅に長い時間がかかってを参照してください。 
- 実行時間の長い配布チェック、 *rows_processed* その分布から移動する行数が他よりも大幅に大きいかどうかを列です。 これはクエリにデータ傾斜があることを表示されます。





## データ傾斜を調査する

```

-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED("dbo.FactInternetSales");

```


このクエリの結果は、データベースの 60 の分散のそれぞれに保存されているテーブル行の数を示します。 パフォーマンスを最適化するには、分散テーブルの行を配布全体で均等に広げる必要があります。
詳細については、[テーブルのデザイン] を参照してください。



## 次のステップ
SQL Data Warehouse の管理の詳細については、次を参照してください。 [管理の概要] []。

<!--Image references-->

<!--Article references-->
[manage overview]: sql-data-warehouse-overview-manage.md
[table design]: sql-data-warehouse-develop-table-design.md

<!--MSDN references-->



