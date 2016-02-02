<properties 
    pageTitle="エラスティック データベース分割/マージ ツールを使用したスケーリング |Microsoft Azure" 
    description="Elastic Database API を使用して、シャードを操作し、自己ホスト サービス経由でデータを移動する方法について説明します。" 
    services="sql-database" 
    documentationCenter="" 
    manager="jeffreyg" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/04/2015" 
    ms.author="ddove;sidneyh" />


# Elastic Database 分割/マージ ツールを使用したスケーリング

[エラスティック データベース ツール](sql-database-elastic-scale-introduction.md) 再調整し、データの分布とシャード化されたアプリケーションのホット スポットを管理するためのツールが含まれています。  **分割/マージ ツール** スケールインとスケール アウトの管理を追加したり、シャードから削除するデータベースを設定し、分割/マージ ツールを使用して、それらの間のシャードレットの配布を再調整します。 (用語の定義を参照してください。 [Elastic scale 用語集](sql-database-elastic-scale-glossary.md))します。

このツールは、異なるデータベース間で、必要に応じてシャードレットを移動しとの統合 [シャード マップの管理](sql-database-elastic-scale-shard-map-management.md) 一貫したマッピングを維持するためにします。

開始するを参照してください。 [Elastic database 分割/マージ ツール ](sql-database-elastic-scale-configure-deploy-split-and-merge.md)します。

## Split/Merge の新機能

分割/マージ ツールの 1.1.0 リリースには、完了した要求からメタデータを自動的にクリーンアップする機能が備わっています。 構成オプションによって、メタデータを削除する前の保持期間を制御します。

1.0.0 分割/マージ ツールのリリースには、次の機能強化が提供されています。
* .Net API は、Split/Merge のインターフェイスに含まれ、Web ロールはオプションになりました。
* シャーディング キーの日付と時刻の型がサポートされるようになりました。
* リスト シャード マップは現在サポートされています。
* 要求の範囲境界は、より簡単にシャード マップに格納されている範囲と一致できます。
* 可用性を高めるために、複数の worker ロール インスタンスがサポートされています。
* Split/Merge 操作の一部として格納されている資格情報は、REST で暗号化されます。

## アップグレードする方法

1. 」の説明に従って、NuGet から split-merge パッケージの最新バージョンをダウンロード [分割-結合パッケージをダウンロード](sql-database-elastic-scale-configure-deploy-split-and-merge.md#download-the-Split-Merge-packages)します。
2. Split/Merge のデプロイ用のクラウド サービス構成ファイルを変更して、新しい構成パラメーターを反映します。 新しい必須のパラメーターは、暗号化に使用される証明書に関する情報です。 これを簡単に行うには、ダウンロードした新しい構成テンプレート ファイルを既存の構成と比較します。 Web ロールと worker ロールの「DataEncryptionPrimaryCertificateThumbprint」と「DataEncryptionPrimary」に、必ず設定を追加するようにしてください。
3. Azure に更新をデプロイする前に、現在実行中のすべての Split/Merge 操作が完了していることを確認します。 これは、実行中の要求の Split/Merge メタデータのデータベースで RequestStatus と PendingWorkflows テーブルを照会することで簡単に行えます。
4. 新しいパッケージと更新されたサービス構成ファイルで、Azure サブスクリプションの Split/Merge の既存のクラウド サービスのデプロイを更新します。

アップグレードするために Split/Merge の新しいメタデータ データベースをプロビジョニングする必要はありません。 新しいバージョンは、既存のメタデータ データベースを新しいバージョンに自動的にアップグレードします。

## Split/Merge のシナリオ

アプリケーションは、次のシナリオで示すように、単一の Azure SQL DB データベースの制限を超えて柔軟に拡張できる必要があります。

* **容量の拡張 - 範囲の分割**: データ層の合計容量を拡張できる能力は、容量の拡張ニーズに対応します。 このシナリオでは、アプリケーションは、容量ニーズを満たすために、データをシャーディングし、データを分散するデータベースの数を段階的に増やすことによって追加の容量を確保します。 このシナリオに対しては、Elastic Scale の Split/Merge サービスの "分割" 機能を使用して対処できます。

* **容量の縮小 - 範囲のマージ:**: 容量は、ビジネスの季節的な性質によって変動します。 このシナリオは、ビジネスの成長が鈍化したときにより少ないスケール単位に簡単に戻すことができる機能に対するニーズを強調しています。 Elastic Scale の Split/Merge サービスの "マージ" 機能を使用すると、この要件に対処できます。

* **ホットスポットの管理 - シャードレットの移動**: データベースごとに複数のテナントがある場合、シャードレットをシャードに割り当てることが、一部のシャードの容量ボトルネックにつながります。 これには、シャードレットを再び割り当てたり、ビジー状態のシャードレットを新しいシャードや使用率の低いシャードに移動したりする操作が必要になります。

以降では、これらの機能に従うサービスのすべての処理を**分割/マージ/移動**要求と呼びます。


図 1: Split/Merge の概念の概要


![概要][1]


**注**: すべての**容量の拡張**シナリオで Split/Merge サービスが必要になるとは限りません。 たとえば、新しいデータを増え続けるシャーディング キー値と共に格納するために環境に新しいシャードを定期的に作成する場合、シャード マップの管理クライアント API を使用して、新しく作成されたシャードを新しいデータ範囲で使用することができます。 Split/Merge サービスは、既存のデータも移動する必要がある場合にのみ必要になります。

## 概念と主な機能

**お客様側でホストされるサービス**: 分割/マージは、お客様側でホストされるサービスとして提供されます。 このサービスは、Microsoft Azure サブスクリプション内でデプロイし、ホストする必要があります。 NuGet からダウンロードするパッケージには構成テンプレートが含まれていて、これに特定のデプロイの情報を入力します。 参照してください、 [分割/マージのチュートリアル](sql-database-elastic-scale-configure-deploy-split-and-merge.md) 詳細です。 このサービスは Azure サブスクリプション内で実行されるため、サービスのセキュリティに関するほとんどの側面を制御および構成できます。 既定のテンプレートには、SSL、証明書ベースのクライアント認証、保存された資格情報の暗号化、DoS 対策、IP 制限を構成するためのオプションが含まれています。 次のドキュメントでセキュリティの側面に関する詳細情報を記載できます [split-merge セキュリティ構成](sql-database-elastic-scale-split-merge-security-configuration.md)します。

既定では、デプロイされたサービスは、1 つの worker ロールと 1 つの Web ロールを使用して実行されます。 それぞれは、Azure Cloud Services の A1 VM サイズを使用します。 これらの設定は、パッケージをデプロイするときに変更することはできませんが、実行中のクラウド サービスへのデプロイが成功した後に (Azure ポータルを通じて) 変更することができます。 技術的な理由により、複数のインスタンスに worker ロールを構成しないでください。

**シャード マップの統合**: Split/Merge サービスは、アプリケーションのシャード マップと対話します。 Split/Merge サービスを使用して範囲を分割またはマージしたり、シャードレットをシャード間で移動したりするとき、サービスによってシャード マップが自動的に最新の状態に保たれます。 これを実現するために、サービスは、アプリケーションのシャード マップ マネージャー データベースに接続し、分割/マージ/移動要求の進行に伴って範囲とマッピングを管理します。 これにより、Split/Merge 操作が実行されているときにシャード マップが常に最新の状態を示すことが保証されます。 分割、マージ、およびシャードレットの移動操作は、シャードレットのバッチをソース シャードからターゲット シャードに移動することによって実装されます。 シャードレットの移動操作中、現在のバッチの対象のシャードレットは、シャード マップ内でオフラインとしてマークされ、**OpenConnectionForKey API** を使用したデータ依存ルーティング接続に使用できなくなります。

**一貫性のあるシャードレットの接続**: シャードレットの新しいバッチのデータの移動が開始されると、シャードレットを格納しているシャードへの、シャード マップによって提供されるすべてのデータ依存ルーティング接続が強制終了されます。さらに不整合を回避するために、データの移動操作中は、シャード マップ API からのこれらのシャードレットへの後続の接続がブロックされます。 同じシャード上の他のシャードレットへの接続も強制終了されますが、再試行すると成功します。 バッチが移動されると、シャードレットがターゲット シャードに対して再びオンラインとしてマークされ、ソース データがソース シャードから削除されます。 すべてのシャードレットが移動されるまで、すべてのバッチに対してこの手順が実行されます。 これにより、完全な移動/分割/マージ操作の進行中に接続の強制終了操作が複数回実行されることになります。

**シャードレットの可用性の管理**: 前述のように、接続の強制終了をシャードレットの現在のバッチに制限することで、使用不可能となるシャードレットのスコープが一度に 1 つのバッチに限定されます。 これは、分割/マージ操作の進行中にすべてのシャードレットに対してシャード全体がオフラインになるような方法よりも好ましいアプローチです。 一度に移動する個別のシャードレットの数として定義されるバッチのサイズは、1 つの構成パラメーターです。 これは、アプリケーションの可用性とパフォーマンスのニーズに応じて分割/マージ操作ごとに定義できます。 シャード マップでロックされている範囲は、指定されたバッチ サイズよりも大きくなる場合があります。 これは、サービスによって、データ内のシャーディング キー値の実際の数がバッチ サイズとほぼ一致するように範囲のサイズが選択されるためです。 これは、シャーディング キー値の数がスパースな場合に関して特に覚えておく必要があります。

**メタデータのストレージ**: Split/Merge サービスは、データベースを使用して、その状態を管理し、要求処理中のログを保持します。 ユーザーは、サブスクリプションにこのデータベースを作成し、このデータベースの接続文字列をサービス デプロイの構成ファイルに指定します。 ユーザーの組織の管理者も、要求の進行状況を確認したり、潜在的な障害に関する詳細な情報を調べるために、このデータベースに接続できます。

**シャーディング対応**: Split/Merge サービスでは、(1) シャード化テーブル、(2) 参照テーブル、(3) 通常のテーブルが区別されます。 移動/分割/マージ操作のセマンティクスは、使用されるテーブルの種類によって異なり、次のように定義されます。

* **シャード化テーブル**: 移動/分割/マージ操作により、ソース シャードからターゲット シャードにシャードレットが移動されます。 要求全体が正常に完了した後、これらのシャードレットはソース上に存在しません。 ターゲット テーブルは、ターゲット シャード上に存在する必要があり、操作の処理の前にターゲット範囲にデータが含まれていてはいけません。

* **参照テーブル**: 参照テーブルの場合、分割/マージ/移動操作により、ソース シャードからターゲット シャードにデータがコピーされます。 ただし、ターゲット上のテーブルに行が存在する場合、このテーブルのターゲット シャード上で変更はしません。 参照テーブルのコピー操作を処理するには、テーブルが空になっている必要があります。

* **その他のテーブル**: その他のテーブルは、分割/マージ操作のソースまたはターゲットのどちらかに存在することができます。 Split/Merge サービスでは、すべてのデータの移動またはコピー操作に関してこれらのテーブルは無視されます。 ただし、制約がある場合にこれらのテーブルが操作を妨げる可能性があることに注意してください。

参照テーブルとシャード化テーブルに関する情報は、シャード マップの **SchemaInfo API** によって提供されます。 次の例では、特定のシャード マップ マネージャー オブジェクト smm でこれらの API を使用しています。

    // Create the schema annotations 
    SchemaInfo schemaInfo = new SchemaInfo(); 
    
    // Reference tables 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region")); 
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation")); 
    
    // Sharded tables 
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY")); 
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY")); 
    
    // Publish 
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo); 

region テーブルと nation テーブルは参照テーブルとして定義されていて、分割/マージ/移動操作によってコピーされます。 customer テーブルと orders テーブルは、シャード化テーブルとして定義されています。 C_CUSTKEY と O_CUSTKEY は、シャーディング キーとして機能します。

**参照整合性**: Split/Merge サービスは、テーブル間の依存関係を分析し、外部キーと主キーのリレーションシップを使用して、参照テーブルとシャードレットを移動する操作をステージングします。 一般に、最初に参照テーブルが依存関係の順にコピーされ、次にシャードレットが各バッチ内での依存関係の順にコピーされます。 これは、新しいデータが到着するときにターゲット シャード上の外部キーと主キーの制約が適用されるために必要な操作です。

**シャード マップの整合性と最終的な完了**: エラーの場合、Split/Merge サービスは停止後に操作を再開して、進行中の要求を完了しようとします。 ただし、ターゲット シャードが失われた場合や、修復できないほど危害を受けている場合など、回復できない状況もあります。 このような状況では、移動されたと考えられるシャードレットがソース シャード上に残されたままになる場合があります。 このサービスでは、必要なデータがターゲットに正常にコピーされた後でのみシャードレット マッピングが更新されることが保証されます。 ソース上のシャードレットは、すべてのデータがターゲットにコピーされ、対応するマッピングが正常に更新された後で削除されます。 削除操作は、ターゲット シャード上で範囲が既にオンラインになっているときにバックグラウンドで実行されます。 Split/Merge サービスでは、シャード マップに格納されているマッピングの正確性が常に保証されます。

## サービスのバイナリの取得

分割/マージ サービスのバイナリがによって提供される [Nuget](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)します。 手順をご覧ください [分割/マージのチュートリアル](sql-database-elastic-scale-configure-deploy-split-and-merge.md) 詳細については、バイナリがダウンロードされます。

## Split/Merge のユーザー インターフェイス

Split/Merge サービス パッケージには、worker ロールだけでなく Web ロールも含まれ、これを使用して対話的に分割/マージ要求を送信できます。 ユーザー インターフェイスの主要なコンポーネントは、次のとおりです。

-    Operation Type: The operation type is a radio button that controls the kind of operation performed by the service for this request. You can choose between the split, merge and move scenarios discussed in Concepts and Key Features. In addition, you can also cancel a previously submitted operation. You can use split, merge and move requests for range shard maps. List shard maps only support move operations.

-    Shard Map: The next section of request parameters cover information about the shard map and the database hosting your shard map. In particular, you need to provide the name of the Azure SQL Database server and database hosting the shardmap, credentials to connect to the shard map database, and finally the name of the shard map. Currently, the operation only accepts a single set of credentials. These credentials need to have sufficient permissions to perform changes to the shard map as well as to the user data on the shards.

-    Source Range (split and merge): A split and merge operation processes a range using its low and high key. To specify an operation with an unbounded high key value, check the “High key is max” check box and leave the high key field empty. The range key values that you specify do not need to precisely match a mapping and its boundaries in your shard map. If you do not specify any range boundaries at all the service will infer the closest range for you automatically. You can use the GetMappings.ps1 PowerShell script to retrieve the current mappings in a given shard map.

-    Split Source Behavior (split): For split operations, you also need to define at which point you want to split the source range. You do this by providing the sharding key where you want the split to occur. Use the radio button next to define whether you want the lower part of the range (excluding the split key) to move, or whether you want the upper part to move (including the split key).

-    Source Shardlet (move): Move operations are different from split or merge operations as they do not require a range to describe the source. A source for move is simply identified by the sharding key value that you plan to move.

-    Target Shard (split): Once you have provided the information on the source of your split operation, you need to define where you want the data to be copied to by providing the Azure SQL Db server and database name for the target.

-    Target Range (merge): Merge operations instead move shardlets to an existing shard. You identify the existing shard by providing the range boundaries of the existing range that you want to merge with.

-    Batch Size: The batch size controls the number of shardlets that will go offline at a time during the data movement. This is an integer value where you can use smaller values when you are sensitive to long periods of downtime for shardlets. Larger values will increase the time that a given shardlet is offline but may improve performance.

-    Operation Id (Cancel): If you have an ongoing operation that is no longer needed, you can cancel the operation by providing its operation ID in this field. You can retrieve the operation ID from the request status table (see Section 8.1) or from the output in the web browser where you submitted the request.



## 要件と制限

Split/Merge サービスの現在の実装には、次の要件と制限が適用されます。

* 現時点で、シャードに対して分割/マージ操作を実行する前に、シャードが存在し、シャード マップに登録されている必要があります。

* 現時点で、Split/Merge サービスは、操作の一部としてテーブルやその他のデータベース オブジェクトを自動的に作成しません。 これは、分割/マージ/移動操作を開始する前に、ターゲット シャード上にすべてのシャード化テーブルと参照テーブルのスキーマが存在している必要があることを意味します。 特に、シャード化テーブルは、移動/分割/マージ操作で新しいシャードレットが追加される範囲内で空になっている必要があります。 そうでないと、ターゲット シャードでの初期の整合性チェックで不合格になります。 また、参照データは参照テーブルが空の場合にのみコピーされる点と、参照テーブルに対する他の同時書き込み操作に関して一貫性が保証されない点にも注意してください。 分割/マージ操作を実行したときに、参照テーブルに変更を加える他の書き込み操作がないことを確認することをお勧めします。

* 現在、サービスでは、大きなシャードレットのパフォーマンスと信頼性を向上させるために、シャーディング キーを含む一意のインデックスまたはキーで設定される行 ID に依存しています。 これにより、単なるシャーディング キー値よりも細かな粒度でデータを移動できます。 その結果、ログ領域と操作中に必要になるロックの量を削減できます。 移動/分割/マージ要求でテーブルを使用する場合は、シャーディング キーを含む一意のインデックスまたは主キーをテーブルに作成することを検討してください。 パフォーマンス上の理由により、シャーディング キーは、キーまたはインデックスの先頭の列である必要があります。

* 要求の処理の過程で、一部のシャードレット データがソース シャードとターゲット シャードの両方に存在することがあります。 これは、シャードレットの移動時にエラーが発生した場合に備えた、現在必要な動作です。 既に説明したように、Split/Merge と Elastic Scale シャード マップの統合により、シャード マップ上で **OpenConnectionForKey** メソッドを使用するデータ依存ルーティング API を介した接続において、中間状態の一貫性が失われることはありません。 ただし、**OpenConnectionForKey** メソッドを使用せずにソース シャードまたはターゲット シャードに接続するときは、移動/分割/マージ要求の実行中に一貫性のない中間状態が発生する可能性があります。 これらの接続では、タイミングや接続の基になるシャードによって、部分的な結果や重複する結果が生成される可能性があります。 現在、この制限には、Elastic Scale マルチシャード クエリによって確立される接続が含まれます。

* Split/Merge サービスのメタデータ データベースは、異なるロール間では共有できません。 たとえば、ステージング環境で実行されている分割/マージ サービスのロールは、実稼働環境ロールとは異なるメタデータ データベースを指し示す必要があります。


## 課金

Split/Merge サービスは、Microsoft Azure サブスクリプションのクラウド サービスとして実行されます。 そのため、クラウド サービスの料金がサービスのインスタンスに適用されます。 移動/分割/マージ操作を頻繁に実行する場合を除き、Split/Merge クラウド サービスを削除することをお勧めします。 こうすることで、実行中のまたはデプロイされたクラウド サービス インスタンスに対して発生するコストを削減できます。 Split/Merge の操作を実行する必要があるときはいつでも簡単に実行可能な構成を再デプロイして開始することができます。

## Monitoring

### 状態テーブル

分割/マージ サービスでは、完了した要求と実行中の要求を監視するための **RequestStatus** テーブルがメタデータ ストア データベースに用意されています。 このテーブルには、この Split/Merge サービスのインスタンスに送信されたそれぞれの Split/Merge 要求データが行として含まれます。 それぞれの要求に対して、次の情報が含まれます。

* **タイムスタンプ**: 要求が開始されたときの日付と時刻。

* **OperationId**: 要求を一意に識別する GUID。 この要求を使用して、まだ実行中の操作を取り消すこともできます。

* **ステータス**: 要求の現在の状態。 実行中の要求に対しては、要求の現在のフェーズも表示されます。

* **CancelRequest**: 要求が取り消されたかどうかを示すフラグ。

* **進行状況**: 推定される操作の進捗状況。 値 50 は、操作が約 50% 完了していることを示します。

* **詳細**: 詳細な進捗状況レポートを提供する XML 値。 行のセットがソースからターゲットにコピーされるときに、進捗状況レポートが定期的に更新されます。 エラーまたは例外が発生した場合、この列にはエラーに関するより詳細な情報も含まれます。


### Azure 診断

Split/Merge サービスは、監視と診断を行うために Azure SDK 2.5 に基づく Azure 診断を使用します。 ここで説明したように、診断の構成を制御します。 [Azure クラウド サービスおよび仮想マシンで診断を有効にすると](../cloud-services-dotnet-diagnostics.md)します。 ダウンロード パッケージには、Web ロール用と worker ロール用の 2 つの診断構成が含まれています。 サービスの診断構成はこれらの手順から [で、Microsoft Azure Cloud Service Fundamentals](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649)します。 これには、パフォーマンス カウンター、IIS ログ、Windows イベント ログ、および Split/Merge アプリケーション イベント ログを記録するための定義が含まれます。

## 診断のデプロイ

NuGet パッケージで提供される Web ロール用と worker ロール用の診断構成を使用して、監視と診断を有効にするには、Azure PowerShell を使用して次のコマンドを実行します。

    $storage_name = "<YourAzureStorageAccount>" 
    
    $key = "<YourAzureStorageAccountKey" 
    
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  
    
    
    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 
    
    
    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 
    
    $service_name = "<YourCloudServiceName>" 
    
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

構成して、診断設定を展開する方法に関する詳細情報を記載することができます: [Azure クラウド サービスおよび仮想マシンで診断を有効にすると](../cloud-services-dotnet-diagnostics.md)します。

## 診断の取得

診断には、Visual Studio サーバー エクスプローラーのサーバー エクスプローラー ツリーの Azure の部分から簡単にアクセスできます。Visual Studio インスタンスを開き、メニュー バーで [ビュー]、[サーバー エクスプローラー] の順にクリックします。Azure のアイコンをクリックして Azure サブスクリプションに接続します。Azure に移動し、ストレージ]->-> [ <your storage account> ]-> [WADLogsTable] メニューの [テーブルです。詳細については、次を参照してください。 [サーバー エクスプ ローラーを使用したストレージ リソース](http://msdn.microsoft.com/library/azure/ff683677.aspx)します。

![WADLogsTable][2]

上の図で強調表示されている WADLogsTable には、Split/Merge サービスのアプリケーション ログからの詳細なイベントが含まれています。 ダウンロードしたパッケージの既定の構成は、運用環境のデプロイを対象にしていることに注意してください。 そのため、サービス インスタンスからログおよびカウンターが取得される間隔が長くなっています (5 分)。 テストと開発用には、Web ロールまたは worker ロールの診断設定をニーズに合わせて調整して、間隔を短くすることができます。 Visual Studio サーバー エクスプローラー (上図参照) でロールを右クリックし、[診断構成] ダイアログ ボックスで、[転送間隔] の値を調整します。

![構成][3]


## パフォーマンス

一般に、Azure SQL Database の上位のより高パフォーマンスのサービス階層ほど、より高いパフォーマンスを期待できます。 上位のサービス階層で提供されるより優れた IO、CPU、およびメモリ割り当ては、Split/Merge サービスが使用する一括コピーおよび削除操作に役立ちます。 そのため、定義された一定期間のデータベースに対してのみサービス階層を増やします。

サービスでは、検証クエリが通常の操作の一部としても実行されます。 検証クエリは、ターゲット範囲に想定外のデータが存在していないことを確認して、すべての分割/マージ/移動操作が一貫性のある状態で開始されることを保証します。 これらのクエリは、操作のスコープと要求の定義の一部として提供されたバッチ サイズによって定義されたシャーディング キー範囲に作用します。 これらのクエリは、先頭の列にシャーディング キーを含むインデックスがあるときに最高のパフォーマンスを発揮します。

さらに、先頭の列にシャーディング キーを含む一意性プロパティにより、サービスは、ログ領域とメモリに関してリソースの消費を制限する最適化されたアプローチを使用できます。 (一般的に 1 GB 以上の) 大きなサイズのデータを移動するには、この一意性プロパティが必要です。

## ベスト プラクティスとトラブルシューティング

-    Define a test tenant and exercise your most important split/merge/move operations with the test tenant across several shards. Ensure that all metadata is defined correctly in your shard map and that the operations do not violate constraints or foreign keys.

-    Keep the test tenant data size above the maximum data size of your largest tenant to ensure you are not encountering data size related issues. This helps you assess an upper bound on the time it takes to move a single tenant around. 

-    Make sure that your schema allows deletions. The split-merge service requires the ability to remove data from the source shard once the data has been successfully copied to the target. For example, **delete triggers** can prevent the service from deleting the data on the source and may cause operations to fail.

-    The sharding key should be the leading column in your primary key or unique index definition. That ensures the best performance for the split or merge validation queries, and for the actual data movement and deletion operations which always operate on sharding key ranges.

-    Collocate your split-merge service in the region and data center where your databases reside. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## 参照

* [分割/マージのチュートリアル](sql-database-elastic-scale-configure-deploy-split-and-merge.md)

* [Elastic Scale セキュリティの考慮事項](sql-database-elastic-scale-split-merge-security-configuration.md)





[1]: ./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png 
[2]: ./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png 
[3]: ./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png 

