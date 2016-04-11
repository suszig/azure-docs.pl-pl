<properties 
    pageTitle="シャード マップ管理 | Microsoft Azure" 
    description="Elastic Database クライアント ライブラリの ShardMapManager の使用方法" 
    services="sql-database" 
    documentationCenter="" 
    manager="jeffreyg" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/04/2015" 
    ms.author="ddove;sidneyh"/>

# シャード マップの管理

使用して、 [Elastic Database クライアント ライブラリ](sql-database-elastic-database-client-library.md) シャード化されたアプリケーションを管理します。 シャード化データベース環境で、 [**シャード マップ**](sql-database-elastic-scale-glossary.md) 維持については、アプリケーションの値に基づいて、適切なデータベースに接続することにより、 **シャーディング キー**します。 これらのマップの構造を理解することは、シャード マップ管理に不可欠です。

## シャード マップとシャードのマッピング
 
### シャーディング キーでサポートされる .Net 型

Elastic Scale では、シャーディング キーとして次の .NET Framework 型がサポートされます。

* integer
* long
* guid
* byte[]  
* datetime
* timespan
* datetimeoffset

### リスト シャード マップと範囲シャード マップ
使用してシャード マップを作成できます **リストの個々 のシャーディング キー値**, を使用して構築されるまたは **の範囲のシャーディング キー値**です。 

###リスト シャード マップ
**シャード** を含む **シャードレット** とシャードにシャードレットのマッピングは、シャード マップによって管理されます。 A **リスト シャード マップ** は、シャードレットを識別する個々 のキー値とシャードとして動作するデータベースの間のアソシエーションです。  **マッピングを一覧表示** は明示的であり (たとえば、キー 1 データベース A にマップする)、異なるキー値は、同じデータベース (キー値 3 と 6 の両方がデータベース B を参照) にマップできます。

| キー | シャードの場所 |
|-----|----------------|
| 1   | データベース A     |
| 3   | データベース B     |
| 4   | データベース C     |
| 6   | データベース B     |
| ... | ...            |
 

### 範囲シャード マップ 
 **範囲シャード マップ**, 、キーの範囲がペアで説明されている **[Low Value, High Value)** 場所、 *価値の低い* 、範囲内の最小のキーは、および *価値の高い* 範囲を超える最初の値は、です。 

たとえば、 **[0, 100)** 以上の値の 0 から 100 未満のすべての整数が含まれています。 複数の範囲が同じデータベースをポイントでき、隣接していない範囲もサポートされます (たとえば、次の例では、[100,200) と [400,600) は、両方ともデータベース C をポイントしています)。

| キー       | シャードの場所 |
|-----------|----------------|
| [1,50)    | データベース A     |
| [50,100)  | データベース B     |
| [100,200) | データベース C     |
| [400,600) | データベース C     |
| ...       | ...            

概念上の例は、各テーブルの前に示した、 **ShardMap** オブジェクトです。  それぞれの行は、1 つの簡単な例 **PointMapping** (リスト シャード マップ) 用または **RangeMapping** (範囲シャード マップ) のオブジェクト。

## シャード マップ マネージャー 

クライアント ライブラリでは、シャード マップ マネージャーはシャード マップのコレクションです。 によって管理されるデータ、 **ShardMapManager** 3 か所で .Net オブジェクトを保持します。 

1. **グローバル シャード マップ (GSM)**: 作成するときに、 **ShardMapManager**, 、すべてのシャード マップとマッピングのリポジトリとして機能するデータベースを指定します。 この情報を管理するために、特殊なテーブルとストアド プロシージャが自動的に作成されます。 通常、これは小さなデータベースで簡単にアクセスできますが、アプリケーションの他の目的には使用されません。 という名前の特殊なスキーマにテーブルがある **_shardmanagement**します。 

2. **ローカル シャード マップ (LSM)**: シャード マップ内でシャードをいくつかの小さなテーブルと特殊なストアド プロシージャおよびシャードに固有のシャード マップ情報を管理を追加して変更が指定したすべてのデータベースです。 この情報は GSM 内の情報を冗長化したものになりますが、これにより、アプリケーションは、GSM に負荷をかけることなくキャッシュされたシャード マップ情報を検証できます。アプリケーションは、LSM を使用して、キャッシュされたマッピングがまだ有効であるかどうかを判定できます。 各シャードの LSM に対応するテーブルがスキーマに含まれて **_shardmanagement**します。

3. **アプリケーション キャッシュ**: 各インスタンスにアクセスするアプリケーション、 **ShardMapManager** オブジェクトは、そのマッピングのローカル メモリ内キャッシュを保持します。 ここには、最近取得されたルーティング情報が格納されます。 

## ShardMapManager の作成
A **ShardMapManager** ファクトリ パターンを使用して、アプリケーション内のオブジェクトがインスタンス化します。  **ShardMapManagerFactory.GetSqlShardMapManager** メソッドでは、形式の資格情報 (サーバー名と、GSM を保持しているデータベースの名前を含む) を受け取り、 **ConnectionString** のインスタンスを返す、 **ShardMapManager**します。  

 **ShardMapManager** 、アプリケーションの初期化コード内のアプリケーション ドメインごと 1 回だけインスタンス化する必要があります。 A **ShardMapManager** 任意数のシャード マップを含めることができます。 多くのアプリケーションでは、シャード マップは 1 つあれば十分です。ただし、異なるスキーマ用や一意性を確保する目的のためにデータベースの異なるセットを使用する場合は、複数のシャード マップを使用することをお勧めします。 

このコードは、既存を開こうとするアプリケーション **ShardMapManager**します。  場合、Global を表す **ShardMapManager** (GSM) 存在しないデータベース内部に、クライアント ライブラリはこれを作成します。

    // Try to get a reference to the Shard Map Manager via the Shard Map Manager database.  
    // If it doesn't already exist, then create it. 
    ShardMapManager shardMapManager; 
    bool shardMapManagerExists = ShardMapManagerFactory.TryGetSqlShardMapManager(
                                        connectionString, 
                                        ShardMapManagerLoadPolicy.Lazy, 
                                        out shardMapManager); 

    if (shardMapManagerExists) 
     { 
        Console.WriteLine("Shard Map Manager already exists");
    } 
    else
    {
        // Create the Shard Map Manager. 
        ShardMapManagerFactory.CreateSqlShardMapManager(connectionString);
        Console.WriteLine("Created SqlShardMapManager"); 

        shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager(
            connectionString, 
            ShardMapManagerLoadPolicy.Lazy);

        // The connectionString contains server name, database name, and admin credentials 
        // for privileges on both the GSM and the shards themselves.
    } 
 
別の方法として、Powershell を使用して新しいシャード マップ マネージャーを作成できます。 例は、 [ここ](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)します。

### シャード マップ管理資格情報

通常、シャード マップを管理と操作するアプリケーションは、シャード マップを使用して接続をルーティングするアプリケーションとは異なります。 

シャード マップ (追加またはシャード、シャード マップ、シャード マッピングの変更など) の管理アプリケーションのインスタンス化して、 **ShardMapManager** を使用して **を持つ資格情報の読み取り/書き込みの特権、GSM データベースとシャードとして機能する各データベースに対する**します。 この資格情報により、新しいシャードに LSM テーブルが作成されるときだけでなく、シャード マップ情報が入力または変更されたときに GSM と LSM の両方のテーブルに対する書き込みが許可される必要があります。  

### 影響を受けるのはメタデータのみ 

設定または変更するために使用されるメソッド、 **ShardMapManager** データは、シャード自体に格納されているユーザー データを変更しません。 たとえばなどのメソッド **CreateShard**, 、**DeleteShard**, 、**UpdateMapping**, などシャード マップ メタデータのみに影響します。 シャードに含まれるユーザー データを削除、追加、変更することはありません。 代わりに、これらのメソッドは、実際のデータベースを作成または削除するために実行される別の操作や、シャード化環境のバランスを再調整するためにシャードで行を移動する操作と組み合わせて使用するように設計されています  (、 **分割/マージ** エラスティック データベース ツールに含まれるツールを使用するシャード間の実際のデータ移動のオーケストレーションと共にこれらの Api を使用します)。 

## シャード マップの設定: 例
 
特定のシャード マップを設定する場合の処理手順を次に示します。 このコードは、次の手順を実行します。 

1. シャード マップ マネージャー内に新しいシャード マップが作成されます。 
2. 2 つの異なるシャードのメタデータがシャード マップに追加されます。 
3. さまざまなキー範囲マッピングが追加され、シャード マップ全体の内容が表示されます。 

このコードでは、予期しないエラーが発生した場合に備えてメソッド全体を安全に再実行できるようになっています。つまり、各要求は、シャードまたはマッピングの作成を試みる前に、シャードまたはマッピングが既に存在するかどうかを確認します。 次のコードは、データベースの名前と想定しています。 **sample_shard_0**, 、**sample_shard_1** と **sample_shard_2** 文字列によって参照されるサーバーで既に作成されている **shardServer**します。 

    public void CreatePopulatedRangeMap(ShardMapManager smm, string mapName) 
        {            
            RangeShardMap<long> sm = null; 

            // check if shardmap exists and if not, create it 
            if (!smm.TryGetRangeShardMap(mapName, out sm)) 
            { 
                sm = smm.CreateRangeShardMap<long>(mapName); 
            } 

            Shard shard0 = null, shard1=null; 
            // check if shard exists and if not, 
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_0"), out shard0)) 
            { 
                Shard0 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_0")); 
            } 

            if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_1"), out shard1)) 
            { 
                Shard1 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_1"));  
            } 

            RangeMapping<long> rmpg=null; 

            // Check if mapping exists and if not,
            // create it (Idempotent / tolerant of re-execute) 
            if (!sm.TryGetMappingForKey(0, out rmpg)) 
            { 
                sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                    (new Range<long>(0, 50), shard0, MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(50, out rmpg)) 
            { 
                sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                    (new Range<long>(50, 100), shard1, MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(100, out rmpg)) 
            { 
                sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                    (new Range<long>(100, 150), shard0, MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(150, out rmpg)) 
            { 
                sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                    (new Range<long>(150, 200), shard1, MappingStatus.Online)); 
            } 

            if (!sm.TryGetMappingForKey(200, out rmpg)) 
            { 
               sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                   (new Range<long>(200, 300), shard0, MappingStatus.Online)); 
            } 

            // List the shards and mappings 
            foreach (Shard s in sm.GetShards()
                                    .OrderBy(s => s.Location.DataSource)
                                    .ThenBy(s => s.Location.Database))
            { 
               Console.WriteLine("shard: "+ s.Location); 
            } 

            foreach (RangeMapping<long> rm in sm.GetMappings()) 
            { 
                Console.WriteLine("range: [" + rm.Value.Low.ToString() + ":" 
                        + rm.Value.High.ToString()+ ")  ==>" +rm.Shard.Location); 
            } 
        } 
 
別の方法として、PowerShell スクリプトを使用して同じ結果を得ることができます。 サンプルの PowerShell の例の一部は [ここ](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)します。     

シャード マップを設定した後は、データ アクセス アプリケーションを作成するか、またはマップに適合させることができます。 設定またはマップを操作行う必要はありませんまで **マップのレイアウト** を変更する必要があります。  

## データ依存ルーティング 

シャード マップ マネージャーを使用するのは、ほとんどの場合、アプリに固有のデータ操作を実行するためのデータベース接続を必要とするアプリケーションです。 シャード化されたアプリケーションでは、これらの接続を適切なターゲット データベースに関連付けする必要があります。 これは呼ば **データ依存ルーティング**します。  このようなアプリケーションの場合は、GSM データベースに対する読み取り専用アクセス権限を持つ資格情報を使用してファクトリからシャード マップ マネージャー オブジェクトをインスタンス化します。 後で個々の接続要求により、適切なシャード データベースに接続するために必要な資格情報が提供されます。

これらのアプリケーション (を使用して **ShardMapManager** 読み取り専用の資格情報で開かれている) はマップまたはマッピングを変更することはできません。  このような操作を行うには、既に説明したように、高度な特権の資格情報を提供する管理操作専用のアプリケーションまたは PowerShell スクリプトを作成します。   

詳細については、次を参照してください。 [データ依存ルーティング](sql-database-elastic-scale-data-dependent-routing.md)します。 

## シャード マップの変更 

シャード マップはさまざまな方法で変更できます。 次に示すどのメソッドを使用しでもシャードとそのマッピングを変更できますが、これらによってシャード内のメタデータが物理的に変更されたり、実際のデータベースが作成または削除されたりすることはありません。  次に示すシャード マップの管理操作の中には、物理的にデータを移動したり、シャードとして機能するデータベースを追加または削除する管理操作と組み合わせて使用したりすることが必要になる操作もあります。

これらのメソッドは、シャード化データベース環境内のデータの全体的な分散状況を変更するために使用できるビルド ブロックとして連携します。  

* 追加またはシャードを削除する: を使用して **CreateShard** と **DeleteShard**します。 
    
    これらの操作を実行するには、サーバーとターゲット シャードを表すデータベースが既に存在している必要があります。 これらのメソッドは、データベース自体には作用せず、シャード マップ内のメタデータのみに作用します。

* ポイントまたはシャードにマップされている範囲を作成または削除: を使用して **CreateRangeMapping**, 、**DeleteMapping**, 、**CreatePointMapping**します。 
    
    多くの異なるポイントまたは範囲を同じシャードにマップできます。 これらのメソッドは、メタデータにのみ作用し、シャードに既に存在するデータには作用しません。 データと一致するために、データベースから削除する必要がある場合 **DeleteMapping** 操作とは別に、これらのメソッドを使用すると共に、これらの操作を実行する必要があります。  

* 2 つは、既存の範囲を分割またはいずれかに隣接する範囲をマージする: を使用して **SplitMapping** と **MergeMappings**します。  

    分割とマージ操作注 **キーの値がマップされているシャードは変更されない**します。 分割操作を実行すると、既存の範囲が 2 つの部分に分割されます。このとき、どちらの部分も同じシャードにマップされたままになります。 マージ操作を実行すると、同じシャードに既にマップされている 2 つの隣接する範囲が 1 つの範囲に結合されます。  ポイントまたは範囲自体のシャードの間の移動を使用して調整する必要がある **UpdateMapping** 実際のデータの移動と連携します。  使用することができます、 **分割/マージ** サービスの移動が必要な場合に、データの移動とシャード マップの変更を連携させるエラスティック データベース ツールの一部です。 

* 個々 のポイントまたは範囲を別のシャードを再マップ (または移動): を使用して **UpdateMapping**します。  

    データを移動する 1 つのシャードから別と一致する必要がありますので **UpdateMapping** 操作、その動きを別個にこれらのメソッドと組み合わせて実行する必要があります。

* マッピングをオンラインとオフライン: を使用して **MarkMappingOffline** と **MarkMappingOnline** マッピングのオンライン状態を制御します。 

    マッピングが「オフライン」の状態の場合、シャード マッピングに対して特定の操作が許可のみを含む **UpdateMapping** と **DeleteMapping**します。 マッピングがオフラインのとき、そのマッピングに含まれるキーに基づくデータに依存する要求はエラーを返します。 さらに、範囲が初めてオフラインになったときは、変更が加えられている範囲に対するクエリによって一貫性のない結果または不完全な結果が生成されるのを防ぐために、影響を受けるシャードへのすべての接続が自動的に強制終了されます。 

マッピングは、.Net では不変オブジェクトです。  マッピングを変更する上記のすべてのメソッドは、コード内のこれらへの参照もすべて無効にします。   マッピングの状態を変更する一連の操作を実行しやすくするため、マッピングを変更するすべてのメソッドは新しいマッピングの参照を返します。これにより、操作を連結できます。  たとえば、キー 25 を含むシャード マップ sm で既存のマッピングを削除するには、次のように実行できます。 

        sm.DeleteMapping(sm.MarkMappingOffline(sm.GetMappingForKey(25)));

## シャードの追加 

多くの場合、アプリケーションは、既に存在しているシャード マップに対し、新しいキーまたはキー範囲から期待されるデータを処理するために新しいシャードを単に追加する必要があります。 たとえば、テナント ID によってシャード化されるアプリケーションの場合、新しいテナントに対して新しいシャードをプロビジョニングすることが必要になる場合があります。また、毎月シャード化されるデータの場合、新しい月が始まる前に新しいシャードをプロビジョニングすることが必要になる場合があります。 

キー値の新しい範囲が既に既存のマッピングの一部ではなく、データの移動が不要な場合は、新しいシャードを追加して、新しいキーまたは範囲をそのシャードに関連付けるようにすると簡単に済みます。 新しいシャードの追加の詳細については、「 [新しいシャードを追加する](sql-database-elastic-scale-add-a-shard.md)です。

ただし、データの移動を必要とするシナリオでは、必要なシャード マップの更新と共にシャード間でのデータの移動を調整するために分割/マージ ツールが必要になります。 分割/マージ yool の使用に関する詳細については、「 [分割/マージの概要](sql-database-elastic-scale-overview-split-and-merge.md) 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 

