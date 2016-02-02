<properties
   pageTitle="SQL Data Warehouse への接続 | Microsoft Azure"
   description="ソリューションの開発のために、SQL Data Warehouse に接続する際のヒントです。"
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
   ms.date="10/21/2015"
   ms.author="JRJ@BigBangData.co.uk;barbkess"/>


# SQL Data Warehouse への接続

SQL Data Warehouse に接続するには、認証のためのセキュリティ資格情報を渡す必要があります。 接続の確立時に、特定の接続の設定が、クエリ セッションの構築の一環として構成されているのがわかります。

この記事では、SQL Data Warehouse への接続に関する次の点について説明します。

- 認証
- 接続の設定
- セッション ID および要求 ID


## 認証

SQL Data Warehouse に接続するには、次の情報を提供する必要があります。

- 完全修飾サーバー名
- SQL 認証の指定
- ユーザー名
- パスワード
- 既定のデータベース (省略可)

ユーザーの認証には SQL 認証を使用しなければならないことに注意してください。 現在、信頼済みの認証はサポートされていません。

既定では、ユーザー データベースではなく、マスター データベースに接続されます。 ユーザー データベースに接続するには、次の 2 つの操作のいずれかを選択します。

1. SQL Server オブジェクト エクスプ ローラーで、SSDT または アプリケーションの接続文字列を使用してサーバーを登録する際に、 既定のデータベースを指定する。 たとえば、ODBC 接続に InitialCatalog パラメーターを含めます。
2. SSDT でセッションを作成する前に、まずユーザー データベースを強調表示する。

> [AZURE.NOTE] SSDT の SQL Data Warehouse への接続に関するガイダンスを参照してください戻る、 [接続し、クエリの][] 概要についての記事です。 

ことが再び TRANSACT-SQL ステートメント * * 使用 <your DB>* * は、接続のデータベースを変更するのにはサポートされていません

## アプリケーションの接続プロトコル

次のプロトコルのいずれかを使用すると、SQL Data Warehouse に接続できます。

- ADO.NET
- ODBC
- PHP
- JDBC

### ADO.NET 接続文字列の例

```
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### ODBC 接続文字列の例

```
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### PHP の接続文字列の例

```
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### JDBC 接続文字列の決定

```
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## Connection settings
SQL Data Warehouse standardizes a few settings during connection and object creation. These cannot be overridden.

| Database Setting   | Value                        |
| :----------------- | :--------------------------- |
| ANSI_NULLS         | ON                           |
| QUOTED_IDENTIFIERS | ON                           |
| NO_COUNT           | OFF                          |
| DATEFORMAT         | mdy                          |
| DATEFIRST          | 7                            |
| Database Collation | SQL_Latin1_General_CP1_CI_AS |

## Sessions and requests
Once a connection has been made and a session has been established you are ready to write and submit queries to SQL Data Warehouse.

Each query will be represented by one or more request identifiers. All queries submitted on that connection are part of a single session and will therefore be represented by a single session id.

However, as SQL Data Warehouse is a distributed MPP system both session and request identifiers are exposed a little differently when compared to SQL Server. 

Sessions and requests are logically represented by their respective identifiers.

| Identifier | Example value |
| :--------- | :------------ |
| Session ID | SID123456     |
| Request ID | QID123456     |

Notice that the Session ID is prefixed by SID - shorthand for Session ID - and the requests are prefixed by QID which is shorthand for Query ID.

You will need this information to help you identify your query when monitoring your query performance. You can monitor your query performance by using either the [Azure Portal] and the dynamic management views.

To identify which session you are currently using the following function:
```
SESSION_ID() を選択します。
;
```

To view all the queries that are either running or have recently run against your data warehouse you can use a query like the one below:
```
CREATE VIEW dbo.vSessionRequests
AS
S. [session_id] AS Session_ID を選択します。
        、s. AS Session_Status の [状態]
        、s. [login_name] AS Session_LoginName
        、s. [login_time] AS Session_LoginTime
        、[request_id] AS Request_ID r です。
        、[status] AS Request_Status r です。
        、[submit_time] AS Request_SubmitTime r です。
        、[start_time] AS Request_StartTime r です。
        、[end_compile_time] AS Request_EndCompileTime r です。
        、[end_time] AS Request_EndTime r です。
        、[total_elapsed_time] AS Request_TotalElapsedDuration_ms r です。
        、DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        、DATEDIFF(ms,[start_time],[end_compile_time]) AS Request_CompileDuration_ms
        、DATEDIFF(ms,[end_compile_time],[end_time]) AS Request_ExecDuration_ms
        、[ラベル] AS Request_QueryLabel
        、AS Request_Command を [コマンド]
        、[database_id] AS Request_Database_ID
Sys.dm_pdw_exec_requests r から
結合 sys.dm_pdw_exec_sessions s ON r. [session_id] = s. [session_id]
ここで [session_id] <> SESSION_ID() を %s します。
;
```

このクエリが、ソリューションに組み込めるようにビューにカプセル化されていることにご注意ください。 ただし、結果を参照するには、ビューを作成して実行する必要があります。

## 次のステップ

接続が完了したら、テーブルの設計を開始できます。 詳細については、[テーブルのデザイン] 記事を参照してください。











[connect and query]: ./sql-data-warehouse-get-started-connect.md 
[table design]: ./sql-data-warehouse-develop-table-design.md 

