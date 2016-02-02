<properties 
    pageTitle="Premium Azure Redis Cache の Redis クラスタリングの構成方法" 
    description="Premium レベルの Azure Redis Cache インスタンス用に Redis を作成して管理する方法について説明します" 
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


# Premium Azure Redis Cache の Redis クラスタリングの構成方法

Azure Redis Cache には、新しい Premium レベルなど、キャッシュのサイズと機能を柔軟に選択できるさまざまなキャッシュ サービスがあります。

Azure Redis Cache の Premium レベルには、クラスタリング、永続性、および Virtual Network のサポートが含まれています。 この記事では、Premium Azure Redis Cache インスタンスでクラスタリングを構成する方法について説明します。

詳細については、その他の premium のキャッシュ機能は、次を参照してください。 [Premium Azure Redis Cache 用の永続化を構成する方法](cache-how-to-premium-persistence.md) と [Premium Azure Redis Cache 用の仮想ネットワークのサポートを構成する方法](cache-how-to-premium-vnet.md)します。

## Redis クラスターとは

Azure Redis Cache と Redis クラスターは、 [Redis に実装されている](http://redis.io/topics/cluster-tutorial)します。 Redis クラスターには次の利点があります。

-   データセットを複数のノードに自動的に分割する機能。
-   ノードのサブセットで障害が発生したとき、クラスターの他の部分と通信できないときに、操作を続行する機能。
-   より多くのスループット: シャードの数を増やすと、スループットは比例して増加します。
-   より多くのメモリ サイズ: シャードの数を増やすと比例的に増加します。

参照してください、 [Azure Redis キャッシュに関する FAQ](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) サイズ、スループット、およびプレミアム キャッシュの帯域幅の詳細についてです。

Azure では、Redis クラスターは、各シャードがプライマリ/レプリカ ペアを持つプライマリ/レプリカ モデルとして提供され、レプリケーションは Azure Redis Cache Service によって管理されます。

## クラスタリング

クラスタリングは、キャッシュの作成中に **[Redis Cache の新規作成]** ブレードで有効化されます。 キャッシュを作成するにサインインする、 [Azure ポータル](https://portal.azure.com) ] をクリック **新規**]-> [**データ + ストレージ**>**Redis Cache**します。

![Redis Cache の作成][redis-cache-new-cache-menu]

クラスタリングを構成するには、まず **[価格レベルの選択]** ブレードで **[Premium]** キャッシュのいずれかを選択します。

![価格レベルの選択][redis-cache-premium-pricing-tier]

クラスタリングは **[Redis クラスター]** ブレードで構成します。

![クラスタリング][redis-cache-clustering]

クラスター内に最大 10 個のシャードを作成できます。 **[有効]** をクリックし、クスライダーを操作するか 1 ～ 10 の値を入力して **[シャード数]** を設定し、**[OK]** をクリックします。

各シャードは Azure によって管理されるプライマリ/レプリカ キャッシュ ペアであり、キャッシュの合計サイズはシャードの数に価格レベルで選択したキャッシュ サイズを掛けることによって計算されます。

![クラスタリング][redis-cache-clustering-selected]

キャッシュを作成した後は、クラスター化されていないキャッシュと同じようにアクセスして使用でき、Redis はキャッシュのシャード全体にデータを分配します。 診断の場合 [有効になっている](cache-how-to-monitor.md#enable-cache-diagnostics), 、メトリックが各シャードとは別にキャプチャ可能であり [表示](cache-how-to-monitor.md) Redis キャッシュ ブレードにします。
>[AZURE.IMPORTANT] StackExchange.Redis を使用して有効にクラスタ リングでは、Azure Redis Cache に接続する、問題が発生して表示されるが場合があります `移動` 例外です。 これは、StackExchange.Redis キャッシュ クライアントがキャッシュ クラスターのノードに関する情報を収集するのに若干時間がかかるためです。 これらの例外は、キャッシュに初めて接続し、クライアントがこの情報の収集を終了する前に、すぐにキャッシュを呼び出した場合に発生する可能性があります。 アプリケーションでこの問題を解決する最も簡単な方法として、キャッシュに接続した後、1 秒間待機してからキャッシュを呼び出します。 追加することでこれを行う、 `Thread.Sleep(1000)` 次のサンプル コードに示すようにします。 なお、 `Thread.Sleep(1000)` キャッシュへの初期接続時にのみ発生します。 詳細については、次を参照してください。 [StackExchange.Redis.RedisServerException - 移動 #248](https://github.com/StackExchange/StackExchange.Redis/issues/248)します。 この問題の修正プログラムの開発が進められており、更新プログラムをここに投稿する予定です。

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        // Connect to the Redis cache for the first time
        var connection =  ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
        // Wait for 1 second
        Thread.Sleep(1000);
    
        // Return the connected ConnectionMultiplexer
        return connection;
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## 実行中の Premium キャッシュに対してシャードを追加または削除する

クラスタリングが有効になっている実行中の Premium キャッシュに対してシャードを追加または削除するには、**[設定]** ブレードで **[(プレビュー) Redis クラスター サイズ]** をクリックします。
>[AZURE.NOTE] Azure Redis Cache の Premium レベルは一般公開されていますが、Redis クラスター サイズ機能は現在プレビュー段階であることに注意してください。

![Redis クラスター サイズ][redis-cache-redis-cluster-size]

シャード数を変更するには、スライダーを使用するか、**[シャード数]** ボックスに 1 ～ 10 の範囲の数値を入力し、**[OK]** をクリックして保存します。

## クラスタリングの FAQ

次の一覧は、Azure Redis Cache のクラスタリングに関するよく寄せられる質問への回答です。

## クラスタリングを使用するためにクライアント アプリケーションを変更する必要がありますか

-   クラスタリングが有効になっている場合は、データベース 0 のみを使用できます。 クライアント アプリケーションが複数のデータベースを使用し、0 以外のデータベースに対する読み取りまたは書き込みを送信するとき、次の例外がスローされます。 `未処理の例外: StackExchange.Redis.RedisConnectionException:---> [GET ProtocolFailure` `StackExchange.Redis.RedisCommandException: 複数のデータベースは、このサーバーではサポートされていませんデータベースに切り替えることはできません: 6`。
-   使用している場合は、 [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) 1.0.481 を使用する必要がありますまたはそれ以降。 同じを使用してキャッシュに接続する [エンドポイント、ポート、およびキー](cache-configure.md#properties) を有効になっているクラスタ リングを持たないキャッシュに接続するときに使用します。 唯一の違いは、すべての読み取りと書き込みをデータベース 0 に対して実行する必要があることです。
    -   他のクライアントの要件は異なる場合があります。 を参照してください [すべての Redis クライアント サポートはクラスタ リング?](#do-all-redis-clients-support-clustering).
-   アプリケーションで 1 つのコマンドにバッチ処理された複数のキー操作を使用する場合は、すべてのキーを同じシャードに配置する必要があります。 これを行うには、を参照してください [クラスター内のキーの配布方法ですか?](#how-are-keys-distributed-in-a-cluster)。.
-   Redis ASP.NET セッション状態プロバイダーを使用する場合は、2.0.0 以降を使用する必要があります。 を参照してください [Redis ASP.NET セッション状態と出力キャッシュ プロバイダーとクラスタ リングを使用?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers).

## クラスターにはキーはどのように配布されるのですか

Redis あたり [キー配布モデル](http://redis.io/topics/cluster-spec#keys-distribution-model) ドキュメント: キー スペースが 16384 スロットに分割します。 各キーはハッシュされ、クラスターのノード全体に配布される、これらのスロットのいずれかに割り当てられます。 ハッシュ タグを使用して同じシャードに複数のキーが配置されていることを確認するために、キーのどの部分をハッシュするかを構成することができます。

-   ハッシュ タグのキーをキーの一部はで囲んだ場合 `{` と `}`, 、キーのハッシュのスロットを確定するためのキーの部分だけがハッシュされています。 たとえば、次の 3 つのキーは、同じシャードに配置が: `{キー} 1`, 、`{キー} 2`, 、および `{キー} 3` からのみ、 `キー` 名の一部はハッシュされています。 キーのハッシュ タグ仕様の一覧については、次を参照してください。 [キー ハッシュ タグ](http://redis.io/topics/cluster-spec#keys-hash-tags)します。
-   ハッシュ タグのないキー - キー名全体がハッシュに使用されます。 そのため、キャッシュのシャード全体で統計的に均等に配布されます。

最高のパフォーマンスとスループットを得るために、キーを均等に配布することをお勧めします。 ハッシュ タグのあるキーを使用する場合、キーが均等に配布されていることを確認するのはアプリケーションの責任です。

詳細については、次を参照してください。 [キー配布モデル](http://redis.io/topics/cluster-spec#keys-distribution-model), 、[Redis クラスターのデータ分割](http://redis.io/topics/cluster-tutorial#redis-cluster-data-sharding), 、および [キー ハッシュ タグ](http://redis.io/topics/cluster-spec#keys-hash-tags)します。

## 作成できる最大キャッシュ サイズはどれくらいですか

Premium の最大キャッシュ サイズは、53 GB です。 最大 10 個のシャードを作成できるので、最大サイズは 530 GB です。 サイズを大きくする必要がある場合は、 [以上を要求](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase)します。 詳細については、次を参照してください。 [Azure Redis Cache の料金](https://azure.microsoft.com/pricing/details/cache/)します。

## すべての Redis クライアントがクラスタリングをサポートしますか

現時点では、すべてのクライアントが Redis クラスタリングをサポートしているわけではありません。 StackExchange.Redis はサポートしているものの 1 つです。 その他のクライアントの詳細については、次を参照してください。、 [クラスターと再生](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) のセクションで、 [Redis クラスター チュートリアル](http://redis.io/topics/cluster-tutorial)します。
>[AZURE.NOTE] StackExchange.Redis クライアントとしてを使用している場合は、最新バージョンを使用していることを確認 [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) 1.0.481 または正常に動作するクラスタ リングについては後述します。

## クラスタリングが有効になっているとき、キャッシュに接続するにはどうすればよいですか

同じを使用して、キャッシュに接続できる [エンドポイント、ポート、およびキー](cache-configure.md#properties) を有効になっているクラスタ リングを持たないキャッシュに接続するときに使用します。 Redis がバックエンドのクラスタリングを管理するので、クライアントから管理する必要はありません。

## キャッシュの個々のシャードに直接接続できますか

これは公式にはサポートされていません。 つまり、各シャードはキャッシュ インスタンスと総称して呼ばれるプライマリ/レプリカ キャッシュ ペアで構成されます。 Redis cli ユーティリティを使用してこれらのキャッシュ インスタンスに接続できる、 [不安定](http://redis.io/download) GitHub で Redis リポジトリの分岐します。 このバージョンで起動したときの基本的なサポートを実装して、 `-c` 切り替えます。 詳細については、次を参照してください。 [クラスターと再生](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) に [http://redis.io](http://redis.io) で、 [Redis クラスター チュートリアル](http://redis.io/topics/cluster-tutorial)します。

SSL 以外の場合は、次のコマンドを使用します。

    Redis-cli.exe –h <<cachename>> -p 13000 (to connect to instance 0)
    Redis-cli.exe –h <<cachename>> -p 13001 (to connect to instance 1)
    Redis-cli.exe –h <<cachename>> -p 13002 (to connect to instance 2)
    ...
    Redis-cli.exe –h <<cachename>> -p 1300N (to connect to instance N)

Ssl は、置換 `1300N` と `1500N`します。

## 以前に作成したキャッシュのクラスタリングを構成できますか。

現時点では、クラスタリングを有効にできるのは、キャッシュを作成するときだけです。 キャッシュの作成後は、シャード数を変更することはできますが、Premium キャッシュに対するクラスタリングの追加または削除は実行できません。 クラスタリングが有効になっている Premium キャッシュと、クラスタリングがない同じサイズの Premium キャッシュでシャードが 1 つだけ異なるものが可能です。

## Basic または Standard キャッシュのクラスタリングを構成できますか。

クラスタリングは、Premium キャッシュでのみ使用できます。

## Redis ASP.NET セッション状態および出力キャッシュ プロバイダーでクラスタリングを使用できますか

-   **Redis 出力キャッシュ プロバイダー** - 変更する必要はありません。
-   **Redis セッション状態プロバイダー** -クラスタ リングを使用するには、使用する必要があります [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.0 上またはで例外がスローされます。 これは、互換性に影響する変更です。詳細については、次を参照してください。 [v2.0.0 互換性に影響する変更の詳細](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details)します。

## 次のステップ

Premium キャッシュ機能をさらに使用する方法を学習します。

-   [Premium Azure Redis Cache 用の永続化を構成する方法](cache-how-to-premium-persistence.md)
-   [Premium Azure Redis Cache 用の仮想ネットワークのサポートを構成する方法](cache-how-to-premium-vnet.md)




[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-clustering/redis-cache-new-cache-menu.png 
[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-clustering/redis-cache-premium-pricing-tier.png 
[newcachemenu]: ./media/cache-how-to-premium-clustering/redis-cache-new-cache-menu.png 
[cachecreate]: ./media/cache-how-to-premium-clustering/redis-cache-cache-create.png 
[redis-cache-premium-pricing-group]: ./media/cache-how-to-premium-clustering/redis-cache-premium-pricing-group.png 
[redis-cache-premium-features]: ./media/cache-how-to-premium-clustering/redis-cache-premium-features.png 
[redis-cache-clustering]: ./media/cache-how-to-premium-clustering/redis-cache-clustering.png 
[redis-cache-clustering-selected]: ./media/cache-how-to-premium-clustering/redis-cache-clustering-selected.png 
[redis-cache-redis-cluster-size]: ./media/cache-how-to-premium-clustering/redis-cache-redis-cluster-size.png 

