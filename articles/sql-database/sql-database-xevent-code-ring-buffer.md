<properties 
    pageTitle="SQL Database の XEvent リング バッファー コード | Microsoft Azure" 
    description="Azure SQL Database で、リング バッファー ターゲットの使用によって簡素化された TRANSACT-SQL のコード サンプルを提供します。" 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jeffreyg" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/22/2015" 
    ms.author="genemi"/>


# SQL Database での拡張イベント向けリング バッファー ターゲット コード


テスト中の拡張イベントに関する情報を、最も簡単な方法で取得およびレポートするための完全なコード サンプルが必要です。 拡張イベント データの最も簡単なターゲットが、 [リング バッファー ターゲット](http://msdn.microsoft.com/library/ff878182.aspx)します。


このトピックでは、以下を実行する TRANSACT-SQL コードのサンプルについて説明します。


1. 表示させるデータでテーブルを作成する。

2. つまり、既存の拡張イベントのセッションが作成され **sqlserver.sql_statement_starting**します。
 - イベントは、特定の更新プログラムの文字列を含む SQL ステートメントに制限: **'% 更新 tabEmployee %' のようなステートメント**します。
 - つまり、リング バッファーの種類の対象に、イベントの出力を送信する  **package0.ring_buffer**します。

3. イベント セッションを開始する。

4. 単純な SQL UPDATE ステートメントをいくつか発行する。

5. リング バッファーからイベント出力を取得する SQL SELECT を発行する。
 - **sys.dm_xe_database_session_targets** 他の動的管理ビュー (Dmv) が参加しているとします。

6. イベント セッションを停止する。

7. リング バッファー ターゲットを削除して、そのリソースを解放する。

8. イベント セッションとデモ テーブルを削除する。


## 前提条件


- Azure アカウントとサブスクリプション。 サインアップできる、 [無料評価版](http://azure.microsoft.com/pricing/free-trial/)します。


- テーブルを作成できるデータベース。
 - ことができます必要に応じて [を作成、 **AdventureWorksLT** デモ データベース](sql-database-get-started.md) (分) です。


- SQL Server Management Studio (ssms.exe)、2015 年 8 月のプレビューまたはそれ以降のバージョン。 
最新の ssms.exe をダウンロードすることができる。
 - [トピック内のリンク。](http://msdn.microsoft.com/library/mt238290.aspx)
 - [ダウンロードへの直接リンク。](http://go.microsoft.com/fwlink/?linkid=616025)
 - マイクロソフトでは、ssms.exe を定期的に更新することをお勧めします。


## サンプル コード


わずかな変更を加えると、以下のリング バッファーのコード サンプルを、Azure SQL Database または Microsoft SQL Server のいずれかで実行できます。 異なる点は、手順 5. の FROM 句に含まれるいくつかの動的管理ビュー (DMV) の名前の中に「_database」というノード名があることです。 次に例を示します。

- sys.dm_xe**(_d)**_session_targets
- sys.dm_xe_session_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
        se.name  AS [session-name],
        ev.event_name,
        ac.action_name,
        st.target_name,
        se.session_source,
        st.target_data,
        CAST(st.target_data AS XML)  AS [target_data_XML]
    FROM
                   sys.dm_xe_database_session_event_actions  AS ac
        INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name            = ac.event_name
                                                                   AND ev.event_session_address = ac.event_session_address
        INNER JOIN sys.dm_xe_database_session_object_columns AS oc  ON oc.event_session_address = ac.event_session_address
        INNER JOIN sys.dm_xe_database_session_targets        AS st  ON st.event_session_address = ac.event_session_address
        INNER JOIN sys.dm_xe_database_sessions               AS se  ON ac.event_session_address = se.address
    WHERE
        oc.column_name = 'occurrence_number'
        AND
        se.name        = 'eventsession_gm_azuresqldb51'
        AND
        ac.action_name = 'sql_text'
    ORDER BY
        se.name,
        ev.event_name,
        ac.action_name,
        st.target_name,
        se.session_source;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## リング バッファーの内容


ここでは、ssms.exe を使用してコード サンプルを実行しました。


結果を表示するには、列見出しの下のセルをクリックして **target_data_XML**します。

結果ウィンドウで [列のヘッダー セルをクリックして、 **target_data_XML**します。 これにより、ssms.exe にもう 1 つのファイル タブが作成され、結果のセルの内容が XML として表示されました。


出力は、次のブロックに示されています。 長い、見た目が 2 つの操作を **<event>** 要素。


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### リング バッファーが保持するリソースの解放


リング バッファーに完了したときに、それを削除および発行する、リソースを解放する **ALTER** 次のようにします。


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


イベント セッションの定義は更新されますが、削除はされません。 後で、イベント セッションにリング バッファーの別のインスタンスを追加できます。


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## 詳細情報


Azure SQL Database での拡張イベントに関する主なトピックは次のとおりです。


- [SQL データベースにおけるイベントの考慮事項を拡張](sql-database-xevent-db-diff-from-svr.md), 、Microsoft SQL Server と Azure SQL Database の間で異なる拡張イベントの一部の機能を比較します。


拡張イベントのその他のコード サンプルのトピックは、次のリンクから参照できます。 ただし、対象が Azure SQL Database または Microsoft SQL Server のどちらかを確認するために、サンプルを定期的にチェックする必要があります。 これにより、変更がサンプル実行に十分であるかを判断できます。


- Azure SQL Database 用のコード サンプル: [SQL データベースでの拡張イベントのイベント ファイル ターゲット コード](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->


