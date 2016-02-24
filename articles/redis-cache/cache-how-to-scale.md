<properties 
    pageTitle="Azure Redis Cache のスケーリング方法" 
    description="Azure Redis Cache インスタンスをスケーリングする方法を学習する" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/11/2015" 
    ms.author="sdanie"/>

# Azure Redis Cache のスケーリング方法

>[AZURE.NOTE] Azure Redis Cache スケール設定機能は現在プレビュー段階にします。 プレビュー期間中は、Premium レベル キャッシュに対するスケーリングは行えませんが、Premium キャッシュ内での価格レベルは変更できます。

Azure Redis Cache は、キャッシュ サイズや機能の選択に柔軟性を持たせるために、さまざまなキャッシュ オファリングを用意しています。 使用して、キャッシュのサイズを拡張する場合は、アプリケーションの要件は、キャッシュを作成した後に変更することができます、 **価格レベルの変更** ブレードで、 [Azure ポータル](https://portal.azure.com)します。

## スケーリングするタイミング

使用することができます、 [監視](cache-how-to-monitor.md) Azure Redis Cache を稼働状態と、キャッシュ アプリケーションのパフォーマンスを監視し、キャッシュのスケーリングする必要があるかどうかを判断するための機能です。 

次のメトリックを監視して、スケーリングの必要性の判断に役立てることができます。

-   Redis サーバーの負荷
-   メモリ使用量
-   ネットワーク帯域幅
-   CPU 使用率

現在のキャッシュがアプリケーションの要件を満たさないと判断した場合は、ご利用のアプリケーションにとって適切な価格レベルのキャッシュに変更できます。 キャッシュの価格を使用するレベルを確認する方法については、次を参照してください。 [どのような Redis キャッシュ オファリングとサイズを使用する](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)です。

## キャッシュのスケーリング
キャッシュのスケーリング [キャッシュを参照](cache-configure.md#configure-redis-cache-settings) で、 [Azure ポータル](https://portal.azure.com) ] をクリック **設定**, 、**価格レベル**します。

クリックすることも、 **Standard レベル** または **Basic レベル** 部、 **Redis Cache** ブレードです。

![価格レベル][redis キャッシュの料金の層-部]

希望の価格レベルを選択して、 **価格レベル** ブレードをクリック **選択**します。

![[価格レベル]][redis-cache-pricing-tier-blade]

>[AZURE.NOTE] 次の制限で別の価格レベルに拡張できます。
>
>-  拡張することはできません、 **Premium** キャッシュします。
>-  拡大/縮小することはできません、 **標準** するためのキャッシュ、 **基本的な** キャッシュします。
>-  拡張、 **基本的な** するためのキャッシュ、 **標準** キャッシュですが、同時に、サイズは変更できません。 サイズを変更する必要がある場合、後続のスケーリング操作でサイズを変更できます。
>-  下に大きなサイズの拡大/縮小することはできません、 **C0 (250 MB)** サイズ。

キャッシュを新しい価格レベルにスケーリングするときに、 **スケーリング** 状態が表示されます、 **Redis Cache** ブレードです。

![スケーリング][redis-cache-scaling]

状態がからスケーリングが完了したら、 **スケーリング** に **を実行している**します。

## スケーリング処理を自動化する方法

Azure ポータルで Azure Redis Cache インスタンスをスケーリングだけでなくを使用してを拡張できる、 [Microsoft Azure 管理ライブラリ (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/)します。 キャッシュをスケーリングするには、`IRedisOperations.CreateOrUpdate` メソッドを呼び出して `RedisProperties.SKU.Capacity` に新しいサイズを指定します。

    static void Main(string[] args)
    {
        // For instructions on getting the access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // To scale, set a new size for the redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

詳細については、次を参照してください。、 [Redis Cache の管理 MAML を使用した](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) サンプルです。

## スケーリングに関する FAQ

次の一覧は、Azure Redis Cache のスケーリングに関するよく寄せられる質問への回答です。

## Premium キャッシュへのスケーリング、キャッシュからのスケーリング、またはキャッシュ内でのスケーリングを行えますか

-   拡張することはできません、 **Premium** キャッシュの価格レベルから **基本的な** または **標準** 価格レベル。
-   拡大/縮小することはできません、 **Premium** するためのキャッシュ、 **基本的な** または **標準** 価格レベル。
-   いずれかから拡張できます **Premium** キャッシュの価格を別のレベルです。
-   クラスタ リングを有効にした場合の作成時に、 **Premium** 、キャッシュを上下、シャード数を増減できます。

詳細については、次を参照してください。 [Premium Azure Redis Cache のクラスタ リングを構成する方法](cache-how-to-premium-clustering.md)します。

## スケーリング後にキャッシュ名やアクセス キーを変更する必要はありますか

いいえ、スケーリング処理中にキャッシュ名やキーが変更されることはありません。

## スケーリングはどのように処理されますか

ときに、 **基本的な** キャッシュがさまざまなサイズに拡大縮小はシャット ダウンされ新しいサイズを使用して、新しいキャッシュをプロビジョニングします。 この間キャッシュは使用できず、キャッシュ内のすべてのデータは失われます。

ときに、 **基本的な** にキャッシュをスケーリング、 **標準** キャッシュは、レプリカ キャッシュがプロビジョニングされ、レプリカ キャッシュをプライマリ キャッシュからデータをコピーします。 スケーリングの処理中、キャッシュは引き続き使用できます。

ときに、 **標準** キャッシュがさまざまなサイズにスケール、レプリカのいずれかがシャット ダウンされ、新しいサイズに再プロビジョニングし、データが転送、および他のレプリカが再プロビジョニングされる場合と同様に、キャッシュ ノードのいずれかの障害発生時に発生するプロセスになる前に、フェールオーバーを実行し、します。

## スケーリング中にキャッシュからデータは失われますか

ときに、 **基本的な** キャッシュが新しいサイズにスケールし、すべてのデータは失われ、スケーリング処理中には、キャッシュは使用できません。

ときに、 **基本的な** にキャッシュをスケーリング、 **標準** キャッシュ、キャッシュ内のデータは通常に保持されます。

ときに、 **標準** キャッシュが大きいサイズにスケール アップではすべてのデータは通常保持します。 スケールする場合は、 **標準** データ、小さいサイズにキャッシュが、キャッシュはスケーリング後に、新しいサイズに関連するデータの量がによって失われる可能性があります。 使用してキーを削除するスケール ダウンする際にデータが失われた場合、 [ペアを lru](http://redis.io/topics/lru-cache) 削除ポリシー。 

Standard および Premium キャッシュには可用性について 99.9% の SLA がある一方で、データの喪失については SLA がありませんのでご注意ください。

## スケーリング中にキャッシュを使用できますか

**標準的な** スケーリング処理中にキャッシュに残っています。

**基本的な** キャッシュはスケーリングを異なるサイズを操作中はオフラインがからをスケールする際に使用可能なまま **基本的な** に **標準**します。

## サポートされていない処理

拡張することはできません、 **Premium** キャッシュします。

変更することはできません、 **標準** に、 **基本的な** キャッシュします。

拡張、 **基本的な** するためのキャッシュ、 **標準** キャッシュですが、同時に、サイズは変更できません。 サイズを変更する必要がある場合、後続のスケーリング操作でサイズを変更できます。

スケール アップできますから、 **C0** より大きなサイズを 250 MB キャッシュが下に大きなサイズにスケールことはできません、 **C0** キャッシュします。

スケーリング処理でエラーが発生すると、サービスは処理を取り消してキャッシュを元のサイズに戻すように試行します。

## スケーリングにはどのくらいの時間がかかりますか

スケーリングには約 20 分かかりますが、キャッシュ内のデータ量によって変わります。

## スケーリングが完了したことをどのようにして確認できますか

スケーリング処理の進捗は Azure ポータルで確認できます。 スケーリングが完了すると、キャッシュの状態に変わります **を実行している**します。

## この機能をプレビューする理由は何ですか

私たちはフィードバックを取得する目的でこの機能をリリースしています。 フィードバックに基づき、まもなくこの機能の一般提供を開始する予定です。





  
<!-- IMAGES -->
[redis-cache-pricing-tier-part]: ./media/cache-how-to-scale/redis-cache-pricing-tier-part.png

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png




