<properties 
    pageTitle="エラスティック データベース ツールを使用してシャードの追加 |Microsoft Azure" 
    description="Elastic Scale API を使用して新しいシャードをシャード セットに追加する方法。" 
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


# Elastic Database ツールを使用してシャードを追加する

## 新しい範囲またはキー用のシャードを追加するには

多くの場合、アプリケーションは、既に存在しているシャード マップに対し、新しいキーまたはキー範囲から期待されるデータを処理するために新しいシャードを単に追加する必要があります。 たとえば、テナント ID によってシャード化されるアプリケーションの場合、新しいテナントに対して新しいシャードをプロビジョニングすることが必要になる場合があります。また、毎月シャード化されるデータの場合、新しい月が始まる前に新しいシャードをプロビジョニングすることが必要になる場合があります。

キー値の新しい範囲が既に既存のマッピングの一部になっていない場合は、新しいシャードの追加と、そのシャードへの新しいキーまたは範囲の関連付けはとてもシンプルな作業です。

### 例: 既存のシャード マップへのシャードとその範囲の追加

次の例では、範囲 [300, 400) を保持する、**sample_shard_2** という名前のデータベースと、その中の必要なすべてのスキーマ オブジェクトが作成されています。

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 
    
    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 
    
    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 

別の方法として、Powershell を使用して新しいシャード マップ マネージャーを作成できます。 例は、 [ここ](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)します。
## 既存の範囲の空き部分用のシャードを追加するには

状況によっては、シャードに対する範囲のマッピングを既に実行し、部分的にデータが入力されている場合がありますが、以降のデータを別のシャードに宛てることができます。 たとえば、日単位で範囲のシャーディングを実行し、既に 50 日がシャードに割り当てられているが、24 日目に今後のデータを別のシャードに保存できます。 弾力性データベース [分割/マージ ツール](sql-database-elastic-scale-overview-split-and-merge.md) この操作を実行できますが、1 日 50 包括的な 25 がまだ存在しない場合は、データの移動が不要 (たとえば、[25、50 日の範囲のデータ) などで) 行うこの完全シャード マップ管理 Api を直接使用します。

### 例: 範囲の分割と新しく追加したシャードへの空き部分の割り当て

"sample_shard_2" という名前のデータベースと、その中のすべての必要なスキーマ オブジェクトが作成されています。


    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 
    
    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
    
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 
    
    // Split the Range holding Key 25 
    
    sm.SplitMapping(sm.GetMappingForKey(25), 25); 
    
    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**重要**: 更新されたマッピングが空の範囲が特定される場合のみ、この手法を使用します。 上記の方法では、移動される範囲のデータはチェックされないため、コード内にチェックを含めることが最善です。 移動される範囲内に行が存在する場合、実際のデータ分布は更新されたシャード マップと一致しなくなります。 使用して、 [分割/マージ ツール](sql-database-elastic-scale-overview-split-and-merge.md) このような場合、代わりに、操作を実行します。


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]






