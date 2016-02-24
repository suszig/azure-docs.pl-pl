<properties 
    pageTitle="SQL Database の拡張イベント | Microsoft Azure" 
    description="Azure SQL Database での拡張イベント (XEvents) について、またイベント セッションが Microsoft SQL Server におけるイベント セッションと若干異なる点について説明します。" 
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
    ms.date="10/13/2015" 
    ms.author="genemi"/>


# SQL Database の拡張イベント


このトピックでは、Azure SQL Database での拡張イベントの実装が Microsoft SQL Server の拡張イベントの場合と若干異なる点について説明します。


- SQL Database V12 では 2015 年の後半に拡張イベント機能が追加されました。
- SQL Server には 2008 年から拡張イベント機能があります。
- SQL Database での拡張イベントの機能セットは SQL Server における機能の堅牢なサブセットです。 


*Xevent* ブログやその他の非公式な場所、非公式のニックネームが拡張イベント' のためです。


> [AZURE.NOTE] 第 2015 年 10 月の時点では、拡張イベント セッション機能はプレビュー レベルでの Azure SQL データベースでアクティブ化されます。 完全一般公開 (GA) の日付はまだ設定されていません。
> 
> Azure [サービス更新プログラム](http://azure.microsoft.com/updates/?service=sql-database) GA お知らせが行われたときに、ページが投稿されます。


## 前提条件


このトピックは、以下の知識をお持ちのユーザーを想定しています。


- [Azure SQL データベース サービス](http://azure.microsoft.com/services/sql-database/)します。


- [拡張イベント](http://msdn.microsoft.com/library/bb630282.aspx) Microsoft SQL server です。
 - 拡張イベントに関するドキュメントの大部分は、SQL Server と SQL Database の両方に適用されます。


次の項目への以前の公開は、イベント ファイルを選択する場合に便利、 [ターゲット](#AzureXEventsTargets):


- [Azure Storage サービス](http://azure.microsoft.com/services/storage/)


- PowerShell
 - [Azure Storage で Azure PowerShell を使用して](storage-powershell-guide-full.md) -PowerShell と Azure ストレージ サービスに関する包括的な情報を提供します。


## コード サンプル


関連するトピックで次の 2 つのコード サンプルを提供します。


- [SQL Database での拡張イベント向けリング バッファー ターゲット コード](sql-database-xevent-code-ring-buffer.md)
 - 短く単純な Transact-SQL スクリプト。
 - コード サンプルのトピックで強調しているように、リング バッファー ターゲットの作業が完了したら、alter-drop `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` ステートメントの実行によりそのリソースを解放する必要があります。 後ほど、`ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...` によりリング バッファーの他のインスタンスを追加できます。


- [SQL Database の拡張イベントのためのイベント ファイル ターゲット コード](sql-database-xevent-code-event-file.md)
 - フェーズ 1 は PowerShell で、Azure Storage コンテナーを作成します。
 - フェーズ 2 は Trasact-SQL で、Azure Storage コンテナーを使用します。


## Transact-SQL の相違点


- 実行すると、 [CREATE EVENT SESSION](http://msdn.microsoft.com/library/bb677289.aspx) を使用する SQL Server 上でコマンド、 **ON SERVER** 句。 使用する SQL データベースでは、 **ON DATABASE** 句代わりにします。


-  **ON DATABASE** にも適用されます句、 [ALTER EVENT SESSION](http://msdn.microsoft.com/library/bb630368.aspx) と [DROP EVENT SESSION](http://msdn.microsoft.com/library/bb630257.aspx) TRANSACT-SQL コマンドです。


- イベント セッション オプションを含めるが望ましい **STARTUP_STATE = ON** で、 **CREATE EVENT SESSION**  または **ALTER EVENT SESSION** ステートメントです。
 -  **= ON** 値は、フェールオーバーのための論理データベースの再構成後に自動再起動をサポートしています。


## 新しいカタログ ビュー


拡張イベント機能がいくつかサポートされている [カタログ ビュー](http://msdn.microsoft.com/library/ms174365.aspx)します。 カタログ ビューによってについてお話し *メタデータまたは定義* 、現在のデータベースでのユーザーが作成したイベント セッションのです。 ビューでは、アクティブなイベント セッションのインスタンスについてはわかりません。


| 名前<br/>カタログ ビュー | 説明 |
| :-- | :-- |
| **sys.database_event_session_actions** | イベント セッションの各イベントに対する操作ごとに行を返します。 |
| **sys.database_event_session_events** | イベント セッションのイベントごとに行を返します。 |
| **sys.database_event_session_fields** | イベントとターゲットに明示的に設定されたカスタマイズ可能な列ごとに行を返します。 |
| **sys.database_event_session_targets** | イベント セッションに対してイベント ターゲットごとに行を返します。 |
| **sys.database_event_sessions** | SQL Database のデータベース内のイベント セッションごとに行を返します。 |


Microsoft SQL server のようなカタログ ビューがある名前が含まれる *. server\_* の代わりに *. database\_*します。 名前のパターンはのように、 **sys.server_event_%**します。


## 新しい動的管理ビュー [(Dmv)](http://msdn.microsoft.com/library/ms188754.aspx)


Azure SQL データベースが [動的管理ビュー (Dmv)](http://msdn.microsoft.com/library/bb677293.aspx) 拡張イベントをサポートします。 Dmv によってについてお話し *active* イベント セッションです。


| DMV の名前 | 説明 |
| :-- | :-- |
| **sys.dm_xe_database_session_event_actions** | イベント セッション アクションに関する情報を返します。 |
| **sys.dm_xe_database_session_events** | セッション イベントに関する情報を返します。 |
| **sys.dm_xe_database_session_object_columns** | セッションにバインドされているオブジェクトの構成の値を示します。 |
| **sys.dm_xe_database_session_targets** | セッション ターゲットに関する情報を返します。 |
| **sys.dm_xe_database_sessions** | 現在のデータベースを対象としたイベント セッションごとに行を返します。 |


なし、Microsoft SQL Server のようなカタログ ビューの名前、 *\_database* 、名前の部分など。


- **sys.dm_xe_sessions**, 、名の代わりに<br/>**sys.dm_xe_database_sessions**します。


### 両者に共通の DMV


拡張イベントについては、次のような Azure SQL Database と Microsoft SQL Server の両方に共通した DMV も存在します。


- **sys.dm_xe_map_values**
- **sys.dm_xe_object_columns**
- **sys.dm_xe_objects**
- **sys.dm_xe_packages**



 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## 使用可能な拡張イベント、アクション、ターゲットを検索


単純な SQL を実行する **選択** 使用可能なイベント、アクション、およびターゲットの一覧を取得します。


```
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```



<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a>

&nbsp;

## SQL Database イベント セッションのターゲット


SQL Database のイベント セッションから結果を取得できるターゲットを次に挙げます。


- [リング バッファー ターゲット](http://msdn.microsoft.com/library/ff878182.aspx) -メモリ内のイベント データを一時的に保持します。
- [イベント カウンター ターゲット](http://msdn.microsoft.com/library/ff878025.aspx) -拡張イベント セッション中に発生するすべてのイベントをカウントします。
- [イベント ファイル ターゲット](http://msdn.microsoft.com/library/ff878115.aspx) -Azure ストレージ コンテナーに完全なバッファーに書き込みます。


 [Event Tracing for Windows (ETW)](http://msdn.microsoft.com/library/ms751538.aspx) API は、SQL データベースで拡張イベントに利用可能です。


## 制限


SQL Database のクラウド環境に利点となるセキュリティ関連の相違点を次にいくつか挙げます。


- 拡張イベントは単一テナントの分離モデルが元になっています。 あるデータベースのイベント セッションが他のデータベースからのデータやイベントにアクセスすることはできません。

- 発行することはできません、 **CREATE EVENT SESSION** のコンテキスト内のステートメントで、 **マスター** データベースです。


## 権限モデル


必要があります **コントロール** を発行するデータベースに対する権限、 **CREATE EVENT SESSION** ステートメントです。 データベース所有者 (dbo) が **コントロール** 権限です。


### ストレージ コンテナーの承認


Azure ストレージ コンテナーを指定する必要がありますを生成する SAS トークン **rwl** のアクセス許可についてです。 これにより次のアクセスが許可されます。


- 読み取り
- 書き込み
- 一覧表示


## パフォーマンスに関する考慮事項


システム全体にとって不健全と言える程、拡張イベントの使い過ぎによるアクティブメモリの蓄積が起きるシナリオがあります。 そのため、Azure SQL Database システムはイベント セッションによって蓄積され得るアクティブ メモリの量に対する制限を動的に設定、調整します。 多くの要因が動的な計算に影響します。


メモリの最大量が使用されたというエラー メッセージを受信した場合、次の修正措置を実行することができます。


- 同時実行するイベント セッションを減少させる。


- を介して、 **作成** と **ALTER** イベント セッションのステートメントで指定したメモリの量の削減、 **MAX\_MEMORY** 句。


### ネットワーク待ち時間


 **イベント ファイル** ターゲット ネットワークの待機時間、または Azure Storage blob にデータの永続化中にエラーが発生する可能性があります。 ネットワーク通信の完了を待機する際に、SQL Database の他のイベントが遅延することがあります。 この遅延によってワークロードが遅くなる可能性があります。

- このパフォーマンス上のリスクを軽減するために設定しないでください、 **EVENT_RETENTION_MODE** オプションを **NO_EVENT_LOSS** イベント セッションの定義にします。


## 関連リンク


- [Azure Storage で Azure PowerShell を使用](storage-powershell-guide-full.md)します。
- [Azure Storage コマンドレット](http://msdn.microsoft.com/library/dn806401.aspx)


- [Azure Storage で Azure PowerShell を使用して](storage-powershell-guide-full.md) -PowerShell と Azure ストレージ サービスに関する包括的な情報を提供します。
- [.NET から Blob Storage を使用する方法](storage-dotnet-how-to-use-blobs.md)


- [CREATE CREDENTIAL (Transact-SQL)](http://msdn.microsoft.com/library/ms189522.aspx)
- [CREATE EVENT SESSION (Transact-SQL)](http://msdn.microsoft.com/library/bb677289.aspx)


- [Jonathan Kehayias の Microsoft SQL Server の拡張イベントに関するブログ投稿](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


拡張イベントの他のコード サンプル トピックは次のリンクから入手可能です。 ただし、対象が Azure SQL Database または Microsoft SQL Server のどちらかを確認するために、サンプルを定期的にチェックする必要があります。 それにより、サンプルを実行するのにわずかな変更が必要かどうか判断できます。


<!--
('lock_acquired' event.)

- SQL Server 用のコード サンプル: [を決定するクエリは保持するロック](http://msdn.microsoft.com/library/bb677357.aspx)
- SQL Server 用のコード サンプル: [上の検索、オブジェクトがある、最もロック取得します。](http://msdn.microsoft.com/library/bb630355.aspx)
-->

