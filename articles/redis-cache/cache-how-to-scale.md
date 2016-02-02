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

>[AZURE.NOTE] Azure Redis Cache のスケーリング機能は現在プレビュー中です。 プレビュー期間中は、Premium レベル キャッシュに対するスケーリングは行えませんが、Premium キャッシュ内での価格レベルは変更できます。

Azure Redis Cache は、キャッシュ サイズや機能の選択に柔軟性を持たせるために、さまざまなキャッシュ オファリングを用意しています。 使用して、キャッシュのサイズを拡張する場合は、アプリケーションの要件は、キャッシュを作成した後に変更することができます、 **価格レベルの変更** ブレードで、 [Azure ポータル](https://portal.azure.com)します。

## スケーリングするタイミング

使用することができます、 [監視](cache-how-to-monitor.md) Azure Redis Cache を稼働状態と、キャッシュ アプリケーションのパフォーマンスを監視し、キャッシュのスケーリングする必要があるかどうかを判断するための機能です。

次のメトリックを監視して、スケーリングの必要性の判断に役立てることができます。

-   Redis サーバーの負荷
-   メモリ使用量
-   ネットワーク帯域幅
-   CPU 使用率

現在のキャッシュがアプリケーションの要件を満たさないと判断した場合は、ご利用のアプリケーションにとって適切な価格レベルのキャッシュに変更できます。 キャッシュの価格を使用するレベルを確認する方法については、次を参照してください。 [どのような Redis キャッシュ オファリングとサイズを使用する](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)します。

## キャッシュのスケーリング

キャッシュのスケーリング [キャッシュを参照](cache-configure.md#configure-redis-cache-settings) で、 [Azure ポータル](https://portal.azure.com) ] をクリック **設定**, 、**価格レベル**します。

あるいは、**[Redis Cache]** ブレードの **[Standard レベル]** または **[Basic レベル]** 部分をクリックできます。

![[価格レベル]][redis-cache-pricing-tier-part]

**[価格レベル]** ブレードから希望の価格レベルを選択し、**[選択]** をクリックします。

![[価格レベル]][redis-cache-pricing-tier-blade]
>[AZURE.NOTE] 別の価格レベルにスケーリングできますが、次のような制約があります。
>
>-スケールできない間、 **Premium** キャッシュします。
>に規模の設定することはできません、 **標準** するためのキャッシュ、 **基本的な** キャッシュします。
>-からを拡張、 **基本的な** するためのキャッシュ、 **標準** キャッシュですが、同時に、サイズは変更できません。 サイズを変更する必要がある場合、後続のスケーリング操作でサイズを変更できます。
>-下に大きなサイズの拡大/縮小することはできません、 **C0 (250 MB)** サイズ。

キャッシュを新しい価格レベルにスケーリングする際に、**[Redis Cache]** ブレードで状態が **[拡大中]** と表示されます。

![スケーリング][redis-cache-scaling]

スケーリングが完了すると、状態が **[拡大中]** から **[実行中]** に変わります。

## スケーリング処理を自動化する方法

Azure ポータルで Azure Redis Cache インスタンスをスケーリングだけでなくを使用してを拡張できる、 [Microsoft Azure 管理ライブラリ (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/)します。 キャッシュのスケーリングを呼び出し、 `IRedisOperations.CreateOrUpdate` メソッドは新しいサイズを指定し、 `RedisProperties.SKU.Capacity`します。

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

-   **Basic** または **Standard** の価格レベルから **Premium** キャッシュの価格レベルにスケーリングすることはできません。
-   **Premium** キャッシュの価格レベルから **Basic** または **Standard** の価格レベルにスケーリングすることはできません。
-   ある **Premium** キャッシュの価格レベルから別の Premium キャッシュの価格レベルにスケーリングすることはできます。
-   **Premium** キャッシュを作成しているときにクラスタリングを有効にした場合、シャード数を増減できます。

詳細については、次を参照してください。 [Premium Azure Redis Cache のクラスタ リングを構成する方法](cache-how-to-premium-clustering.md)します。

## スケーリング後にキャッシュ名やアクセス キーを変更する必要はありますか

いいえ、スケーリング処理中にキャッシュ名やキーが変更されることはありません。

## スケーリングはどのように処理されますか

**Basic** キャッシュを別のサイズにスケーリングすると、キャッシュはシャット ダウンされて新しいキャッシュが新しいサイズでプロビジョニングされます。 この間キャッシュは使用できず、キャッシュ内のすべてのデータは失われます。

**Basic** キャッシュを **Standard** キャッシュにスケーリングすると、レプリカ キャッシュがプロビジョニングされ、データがプライマリ キャッシュからレプリカ キャッシュにコピーされます。 スケーリングの処理中、キャッシュは引き続き使用できます。

**Standard** キャッシュを別のサイズにスケーリングすると、一方のレプリカはシャットダウンし、新しいサイズに再プロビジョニングされてデータが転送されます。もう一方のレプリカは、キャッシュ ノードの 1 つでエラーが発生した際に実行される処理と同様に、フェールオーバーを実行してから、再プロビジョニングされます。

## スケーリング中にキャッシュからデータは失われますか

新しいサイズに **Basic** キャッシュをスケーリングすると、すべてのデータが失われ、スケーリング処理中にキャッシュは使用できなくなります。

**Basic** キャッシュを **Standard** キャッシュにスケーリングする場合、通常、キャッシュのデータは保存されます。

**Standard** キャッシュをより大きいサイズにスケールアップする場合、通常、すべてのデータが保持されます。 **Standard** キャッシュをより小さいサイズにスケールダウンする場合、キャッシュ内のデータ量によってはスケーリング後にデータが失われる可能性があります。 使用してキーを削除するスケール ダウンする際にデータが失われた場合、 [ペアを lru](http://redis.io/topics/lru-cache) 削除ポリシー。

Standard および Premium キャッシュには可用性について 99.9% の SLA がある一方で、データの喪失については SLA がありませんのでご注意ください。

## スケーリング中にキャッシュを使用できますか

**Standard** キャッシュはスケーリング処理中でも使用できます。

別のサイズにスケーリングする場合、**Basic** キャッシュはオフラインになりますが、**Basic** から **Standard** にスケーリングするときは引き続き利用できます。

## サポートされていない処理

**Premium** キャッシュへのスケーリング、またはキャッシュからのスケーリングは行えません。

**Standard** キャッシュから **Basic** キャッシュに変更することはできません。

**Basic** キャッシュから **Standard** キャッシュにスケーリングすることはできますが、同時にサイズを変更することはできません。 サイズを変更する必要がある場合、後続のスケーリング操作でサイズを変更できます。

**C0** (250 MB) キャッシュからより大きなサイズにスケールアップすることはできますが、**C0** キャッシュをより小さいサイズにスケールダウンすることはできません。

スケーリング処理でエラーが発生すると、サービスは処理を取り消してキャッシュを元のサイズに戻すように試行します。

## スケーリングにはどのくらいの時間がかかりますか

スケーリングには約 20 分かかりますが、キャッシュ内のデータ量によって変わります。

## スケーリングが完了したことをどのようにして確認できますか

スケーリング処理の進捗は Azure ポータルで確認できます。 スケーリングが完了すると、キャッシュの状態が **[実行中]** に変わります。

## この機能をプレビューする理由は何ですか

私たちはフィードバックを取得する目的でこの機能をリリースしています。 フィードバックに基づき、まもなくこの機能の一般提供を開始する予定です。








[redis-cache-pricing-tier-part]: ./media/cache-how-to-scale/redis-cache-pricing-tier-part.png 
[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png 
[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png 

