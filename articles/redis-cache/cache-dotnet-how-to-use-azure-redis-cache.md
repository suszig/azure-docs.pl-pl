<properties 
    pageTitle="Azure Redis Cache の使用方法" 
    description="Azure Redis Cache を使用して Azure アプリケーションのパフォーマンスを向上させる方法を説明します" 
    services="redis-cache,app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="dotnet" 
    ms.topic="hero-article" 
    ms.date="12/03/2015" 
    ms.author="sdanie"/>

# Azure Redis Cache の使用方法

> [AZURE.SELECTOR]
- [.Net](cache-dotnet-how-to-use-azure-redis-cache.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

このガイドを使用する方法を示します **Azure Redis Cache**します。 Microsoft Azure Redis Cache は、広く支持されているオープン ソースの Redis Cache がベースとなっています。 マイクロソフトによって管理されている、セキュリティで保護された専用の Redis cache にアクセスすることができます。 Azure Redis Cache を使用して作成されたキャッシュには、Microsoft Azure 内のあらゆるアプリケーションからアクセスすることができます。

Microsoft Azure Redis Cache には、次のレベルがあります。

-   **基本的な** – 単一ノードです。 複数のサイズ、最大 53 GB
-   **標準的な** – 2 ノード プライマリ/レプリカです。 複数のサイズ、最大 53 GB 99.9% の SLA。
-   **Premium** – 2 ノード プライマリ/レプリカに最大 10 個のシャードにします。 6 GB から 530 GB までの複数のサイズ (詳細はお問い合わせください)。 Standard レベルのすべての機能と詳細を含むサポート [Redis クラスター](cache-how-to-premium-clustering.md), 、[Redis の永続化](cache-how-to-premium-persistence.md), 、および [Azure Virtual Network](cache-how-to-premium-vnet.md)します。 99.9% の SLA。

各レベルは、機能と価格ごとに異なります。 料金については、次を参照してください。 [キャッシュの料金詳細][]します。

このガイドでは、使用する方法、 [StackExchange.Redis][] \# のコードを使用してクライアントをします。 紹介するシナリオ **を作成して、キャッシュを構成する**, 、**キャッシュ クライアントの構成**, 、および **を追加して、キャッシュからオブジェクトを削除する**です。 Azure Redis Cache の使用に関する詳細についてを参照してください、 [次のステップ][] セクションです。

<a name="getting-started-cache-service"></a>
## Azure Redis Cache の使用

Azure Redis Cache の導入は簡単です。 使い始めるには、キャッシュをプロビジョニングして構成します。 次に、キャッシュ クライアントを構成してキャッシュにアクセスできるようにします。 キャッシュ クライアントを構成すると、使い始めることができます。

-   [キャッシュの作成][]
-   [キャッシュ クライアントの構成][]

<a name="create-cache"></a>
## キャッシュの作成

キャッシュを作成するには、最初にサインイン、 [Azure ポータル][], 、] をクリック **新規**, 、**データ + ストレージ**, 、**Redis Cache**します。

![新しいキャッシュ][NewCacheMenu]

>[AZURE.NOTE] Azure アカウントを取得していない場合は、ほんの数分で無料の試用アカウントを作成できます。 詳細については、[Azure の無料試用版サイト][]をご覧ください。

 **新しい Redis Cache** ブレードで、キャッシュに必要な構成を指定します。

![キャッシュの作成][CacheCreate]

-    **Dns 名**, 、キャッシュ エンドポイントに使用するサブドメイン名を入力します。 エンドポイントは、数字と小文字のみを含む、先頭が文字の 6 ～ 12 文字の文字列にしてください。
-    **サブスクリプション**, 、キャッシュに使用する Azure のサブスクリプションを選択します。 1 つのサブスクリプションを持つアカウントの場合は自動的に選択されると、 **サブスクリプション** ] ドロップダウン リストは表示されません。
-    **リソース グループ**, を選択するか、キャッシュのリソース グループを作成します。 詳細については、次を参照してください。 [を使用してリソース グループを Azure のリソースを管理][]します。 
-   使用 **場所** 、キャッシュをホストする地理的な場所を指定します。 パフォーマンスを最大限に引き出すために、キャッシュは、キャッシュ クライアント アプリケーションと同じリージョンに作成することを強くお勧めします。
-   使用 **料金レベル** 目的のキャッシュ サイズと機能を選択します。
-   **Redis クラスター** 複数の Redis ノード間で大規模なは、53 GB を超えるとシャードのデータ キャッシュを作成することができます。 詳細については、次を参照してください。 [Premium Azure Redis Cache のクラスタ リングを構成する方法](cache-how-to-premium-clustering.md)します。
-   **Redis の永続化** Azure ストレージ アカウントに、キャッシュを保持する機能を提供しています。 永続化の構成方法の詳細については、次を参照してください。 [Premium Azure Redis Cache 用の永続化を構成する方法](cache-how-to-premium-persistence.md)します。
-   **仮想ネットワーク** を指定した Azure 仮想ネットワーク内でそれらのクライアントにのみ、キャッシュへのアクセスを制限することで、強化されたセキュリティと分離を提供します。 サブネット、アクセス制御ポリシー、およびその他の Redis へのアクセスをさらに制限する機能を始め、VNet のすべての機能を使用できます。 詳細については、次を参照してください。 [Premium Azure Redis Cache 用の仮想ネットワークのサポートを構成する方法](cache-how-to-premium-vnet.md)します。
-   使用 **診断** キャッシュ メトリックのストレージ アカウントを指定します。 詳細を構成して、キャッシュのメトリックを表示するのには、次を参照してください。 [Azure Redis Cache の監視方法](cache-how-to-monitor.md)します。

新しいキャッシュ オプションが構成されると、クリックして **作成**します。 キャッシュが作成されるまで数分かかる場合があります。 状態を確認するには、スタート画面で進行状況を監視してください。 キャッシュが作成されると、新しいキャッシュは、 **を実行している** 状態で、既定の設定で使用できるようします。

![作成されたキャッシュ][CacheCreated]

作成されたキャッシュをからアクセスできる、 **参照** ブレードです。 

![ブレードの表示][BrowseCaches]

クリックして **Redis Caches** してキャッシュを表示します。

![キャッシュ][Caches]

<a name="NuGet"></a>
## キャッシュ クライアントの構成

Azure Redis Cache を使用して作成されたキャッシュには、あらゆる Azure アプリケーションからアクセスすることができます。 Visual Studio で開発した .NET アプリケーションを使用して、 **StackExchange.Redis** キャッシュ クライアントは、キャッシュ クライアント アプリケーションの構成を簡略化する NuGet パッケージを使用して構成できます。 

>[AZURE.NOTE] 詳細については、次を参照してください。、 [StackExchange.Redis][] github ページおよび [StackExchange.Redis キャッシュ クライアントのドキュメント][]します。

StackExchange.Redis NuGet パッケージを使用して Visual Studio でクライアント アプリケーションを構成するで、プロジェクトを右クリックし **ソリューション エクスプ ローラー** 選択 **NuGet パッケージの管理**します。 

![Manage NuGet packages][NuGetMenu]

型 **StackExchange.Redis** または **StackExchange.Redis.StrongName** に、 **オンライン検索** テキスト ボックスは、結果から目的のバージョンを選択し、クリックして **インストール**します。

>[AZURE.NOTE] 厳密な名前付きのバージョンの使用を希望するかどうか、 **StackExchange.Redis** クライアント ライブラリは、選択 **StackExchange.Redis.StrongName**です。 それ以外の場合に選択 **StackExchange.Redis**します。

![StackExchange.Redis NuGet package][StackExchangeNuget]

クライアント アプリケーションから StackExchange.Redis キャッシュ クライアントを使用して Azure Redis Cache にアクセスするために必要なアセンブリ参照が NuGet パッケージによってダウンロードされ追加されます。

クライアント プロジェクトをキャッシュ用に構成できたら、以降のセクションで説明されている、キャッシュを操作するための技法を使用できます。

<a name="working-with-caches"></a>
## キャッシュの操作

このセクションの手順では、キャッシュに対する一般的なタスクを行う方法について説明します。

-   [キャッシュに接続する][]
-   [オブジェクトをキャッシュに追加する、キャッシュから削除する][]
-   [キャッシュ内で .NET オブジェクトを使用する](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## キャッシュに接続する

プログラムでキャッシュを操作するには、キャッシュへの参照が必要です。 StackExchange.Redis クライアントを使用して Azure Redis Cache にアクセスするすべてのファイルの先頭に次のコードを追加します。

    using StackExchange.Redis;

>[AZURE.NOTE] StackExchange.Redis クライアントには、.NET Framework 4 以降が必要です。

Azure Redis Cache への接続には、`ConnectionMultiplexer` クラスを使用します。 このクラスはクライアント アプリケーションの開始から終了まで共有、再利用する前提で設計されており、操作単位で作成する必要はありません。 

Azure Redis Cache に接続して、接続済みの `ConnectionMultiplexer` インスタンスを取得するには、次の例のように、静的 `Connect` メソッドを呼び出して、キャッシュのエンドポイントとキーを渡します。 password パラメーターには、Azure ポータルから生成されたキーを使用してください。

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] 警告: しないストアの資格情報のソース コードにします。 このサンプルでは、単純化するためにあえてソース コード内に記述しています。 参照してください [アプリケーション文字列と接続文字列の動作][] 資格情報を格納する方法の詳細。

SSL を使用しない場合は、`ssl=false` を設定するか、`ssl` パラメーターを省略します。

>[AZURE.NOTE] 既定では新しいキャッシュに対して非 SSL ポートが無効です。 非 SSL ポートを有効にする手順については、次を参照してください、 [アクセス ポート](cache-configure.md#access-ports).。

アプリケーション内の `ConnectionMultiplexer` インスタンスを共有する方法の 1 つは、次の例のように、接続されたインスタンスを返す静的プロパティの設定です。 これにより、接続された `ConnectionMultiplexer` インスタンス 1 つのみがスレッド セーフな方法で初期化されます。 これらの例では、`abortConnect` が false に設定されており、Azure Redis Cache への接続が確立されていない場合でも呼び出しが成功します。 `ConnectionMultiplexer` の主な機能の 1 つは、ネットワーク問題などの原因が解決されると、キャッシュへの接続が自動的に復元されることです。

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

詳細については、構成オプションを接続に高度なを参照してください。 [StackExchange.Redis 構成モデル][]します。

キャッシュのエンドポイントとキーから取得できます、 **Redis Cache** のキャッシュ インスタンスのブレードです。

![キャッシュのプロパティ][CacheProperties]

![キーの管理][ManageKeys]

接続が確立されたら、`ConnectionMultiplexer.GetDatabase` メソッドを呼び出して Redis Cache データベースへの参照を取得します。 `GetDatabase` メソッドから返されるオブジェクトは、手付かずで受け渡しされる軽量のオブジェクトであり、保存する必要はありません。

    // Connection refers to a property that returns a ConnectionMultiplexer
    // as shown in the previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

以上、Azure Redis Cache インスタンスに接続して、キャッシュ データベースへの参照を取得する方法を説明しました。今度は実際にキャッシュを使用してみましょう。

<a name="add-object"></a>
## オブジェクトをキャッシュに追加する、キャッシュから削除する

キャッシュに項目を格納する、またはキャッシュから項目を取得するには、`StringSet` メソッドと `StringGet` メソッドを使用します。

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

Redis では、ほとんどのデータが Redis 文字列として保存されますが、これらの文字列には、さまざまなデータ型を格納することができます。シリアル化したバイナリ データもその 1 つで、.NET のオブジェクトをキャッシュに保存する際に使用することができます。

`StringGet` を呼び出すと、オブジェクトが存在する場合はそのオブジェクトが返され、存在しない場合は `null` が返されます。 その場合、目的のデータ ソースから値を取得してキャッシュに格納しておき、後で使用することができます。 これを "キャッシュ アサイド パターン" といいます。

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

キャッシュ内の項目の有効期限を指定するには、`StringSet` の `TimeSpan` パラメーターを使用します。

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## キャッシュ内で .NET オブジェクトを使用する

Azure Redis Cache はプリミティブ データ型に加え、.NET オブジェクトをキャッシュできますが、.NET オブジェクトをキャッシュするためには、あらかじめシリアル化しておく必要があります。 この処理はアプリケーション開発者が行わなければなりません。逆にそのことでシリアライザーの選択に幅が生まれ、開発者にとってのメリットとなっています。

オブジェクトをシリアル化の 1 つの簡単な方法を使用して、 `JsonConvert` でシリアル化メソッド [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) や JSON からにシリアル化とします。 次の例では、`Employee` オブジェクト インスタンスを使用した get および set を示しています。


    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    
        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## 次のステップ

これで、基本を学習できました。Azure Redis Cache の詳細については、次のリンク先を参照してください。

-   Azure Redis Cache の ASP.NET プロバイダーを参照してください。
    -   [Azure Redis セッション状態プロバイダー](cache-asp.net-session-state-provider.md)
    -   [Azure Redis Cache ASP.NET 出力キャッシュ プロバイダー](cache-asp.net-output-cache-provider.md)
-   [キャッシュ診断を有効にする](cache-how-to-monitor.md#enable-cache-diagnostics) できるように [モニター](cache-how-to-monitor.md) 、キャッシュの正常性。 Azure ポータルでメトリックを表示できるし、することもできます [をダウンロードして確認](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) 好みのツールを使用しています。
-   チェック アウト、 [StackExchange.Redis キャッシュ クライアントのドキュメント][]します。
    -   Azure Redis Cache は、さまざまな Redis クライアントや開発言語からアクセスできます。 詳細については、次を参照してください。 [http://redis.io/clients][] と [Azure Redis Cache 用のその他の言語で開発][]します。
    -   Azure Redis Cache は、Redsmin などのサービスと共に使用することもできます。 詳細については、次を参照してください。  [Azure Redis 接続文字列を取得し、Redsmin と共に使用する方法][]します。
-   参照してください、 [redis][] ドキュメントおよびについて [redis データ型][] と [Redis データ型の 15 分概要][]します。



<!-- INTRA-TOPIC LINKS -->
[Next Steps]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Create the cache]: #create-cache
[Configure the cache]: #enable-caching
[Configure the cache clients]: #NuGet
[Working with Caches]: #working-with-caches
[Connect to the cache]: #connect-to-cache
[Add and retrieve objects from the cache]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->
[NewCacheMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-new-cache-menu.png

[CacheCreate]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-cache-create.png

[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png

[CacheCreated]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-cache-created.png




   
<!-- LINKS -->
[http://redis.io/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[How to retrieve an Azure Redis connection string and use it with Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis configuration model]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache Pricing Details]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: http://azure.microsoft.com/documentation/articles/resource-group-overview/

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis cache client documentation]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis data types]: http://redis.io/topics/data-types
[a fifteen minute introduction to Redis data types]: http://redis.io/topics/data-types-intro

[How Application Strings and Connection Strings Work]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/

[Azure Free Trial]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero


