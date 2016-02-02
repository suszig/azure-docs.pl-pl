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


このガイドでは、**Azure Redis Cache** の基本的な使用方法について説明します。 Microsoft Azure Redis Cache は、広く支持されているオープン ソースの Redis Cache がベースとなっています。 マイクロソフトによって管理されている、セキュリティで保護された専用の Redis cache にアクセスすることができます。 Azure Redis Cache を使用して作成されたキャッシュには、Microsoft Azure 内のあらゆるアプリケーションからアクセスすることができます。

Microsoft Azure Redis Cache には、次のレベルがあります。

-   **Basic** – 単一ノード、 複数のサイズ、最大 53 GB
-   **Standard** – 2 ノード (プライマリ/レプリカ)。 複数のサイズ、最大 53 GB99.9% の SLA。
-   **Premium** – 最大 10 個のシャードがある 2 ノード (プライマリ/レプリカ)。 6 GB から 530 GB までの複数のサイズ (詳細はお問い合わせください)。 Standard レベルのすべての機能と詳細を含むサポート [Redis クラスター](cache-how-to-premium-clustering.md), 、[Redis の永続化](cache-how-to-premium-persistence.md), 、および [Azure Virtual Network](cache-how-to-premium-vnet.md)します。 99.9% の SLA。

各レベルは、機能と料金ごとに異なります。 料金については、次を参照してください。 [キャッシュの料金の詳細 []][]します。

このガイドでは、使用する方法、 [StackExchange.Redis:operator[]][] \# のコードを使用してクライアントをします。 紹介するシナリオは、**キャッシュの作成と構成**、**キャッシュ クライアントの構成**、**キャッシュでのオブジェクトの追加と削除**などです。 Azure Redis Cache の使用に関する詳細についてを参照してください、 [次のステップ][] セクションです。

<a name="getting-started-cache-service"></a>
## Azure Redis Cache の使用

Azure Redis Cache の導入は簡単です。 使い始めるには、キャッシュをプロビジョニングして構成します。 次に、キャッシュ クライアントを構成してキャッシュにアクセスできるようにします。 キャッシュ クライアントを構成すると、使い始めることができます。

-   [キャッシュを作成します。][]
-   [キャッシュ クライアントのを構成します。][]

<a name="create-cache"></a>
## キャッシュの作成

キャッシュを作成するには、最初にサインイン、 [Azure ポータルの][], 、] をクリック **新規**, 、**データ + ストレージ**, 、**Redis Cache**します。

![新しいキャッシュ][newcachemenu]
>[AZURE.NOTE] Azure アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、「 [Azure 無料評価版の []][]します。

**[Redis Cache の新規作成]** ブレードで、必要なキャッシュ構成を指定します。

![キャッシュの作成][cachecreate]

-   キャッシュ エンドポイントに使用するサブドメイン名を **[DNS 名]** に入力します。 エンドポイントは、数字と小文字のみを含む、先頭が文字の 6 ～ 12 文字の文字列にしてください。
-   **[サブスクリプション]** で、キャッシュに使用する Azure サブスクリプションを選択します。 アカウントにサブスクリプションが 1 つしかない場合は自動的に選択されるため、**[サブスクリプション]** ドロップダウン リストは表示されません。
-   **[リソース グループ]** で、キャッシュのリソース グループを選択または作成します。 詳細については、次を参照してください。 [を使用してリソース グループを、Azure リソースの [] を管理][]します。
-   **[場所]** を使用して、キャッシュのホストの地理的位置を指定します。 パフォーマンスを最大限に引き出すために、キャッシュは、キャッシュ クライアント アプリケーションと同じリージョンに作成することを強くお勧めします。
-   **[価格レベル]** を使用して、必要なキャッシュ サイズと機能を選択します。
-   **Redis クラスター**では、53 GB を超えるキャッシュを作成でき、複数の Redis ノード間でデータを共有することもできます。 詳細については、次を参照してください。 [Premium Azure Redis Cache のクラスタ リングを構成する方法](cache-how-to-premium-clustering.md)します。
-   **Redis の永続化**を使用して、Azure ストレージ アカウントにキャッシュを保持できます。 永続化の構成方法の詳細については、次を参照してください。 [Premium Azure Redis Cache 用の永続化を構成する方法](cache-how-to-premium-persistence.md)します。
-   **Virtual Network** では、指定された Azure Virtual Network 内にあるクライアントのみにキャッシュへのアクセス権を制限することで、セキュリティと分離が強化されます。 サブネット、アクセス制御ポリシー、およびその他の Redis へのアクセスをさらに制限する機能を始め、VNet のすべての機能を使用できます。 詳細については、次を参照してください。 [Premium Azure Redis Cache 用の仮想ネットワークのサポートを構成する方法](cache-how-to-premium-vnet.md)します。
-   **診断**を使用して、キャッシュ メトリックにストレージ アカウントを指定できます。 詳細を構成して、キャッシュのメトリックを表示するのには、次を参照してください。 [Azure Redis Cache の監視方法](cache-how-to-monitor.md)します。

新しいキャッシュ オプションを構成したら、**[作成]** をクリックします。 キャッシュが作成されるまで数分かかる場合があります。 状態を確認するには、スタートボードで進行状況を監視してください。 キャッシュが作成されると、新しいキャッシュの状態が "**実行中**" になって、既定の設定で使用できるようになります。

![Cache created][cachecreated]

作成されたキャッシュには、**[参照]** ブレードからアクセスすることができます。

![ブレードの表示][browsecaches]

**[Redis Caches]** をクリックしてキャッシュを表示します。

![キャッシュ][caches]

<a name="NuGet"></a>
## キャッシュ クライアントの構成

Azure Redis Cache を使用して作成されたキャッシュには、あらゆる Azure アプリケーションからアクセスすることができます。 Visual Studio で開発した .NET アプリケーションであれば、**StackExchange.Redis** キャッシュ クライアントを使用できます。キャッシュ クライアント アプリケーションの構成は、NuGet パッケージを使用すると簡単です。
>[AZURE.NOTE] 詳細については、次を参照してください。、 [StackExchange.Redis:operator[]][] github ページおよび [StackExchange.Redis キャッシュ クライアントのドキュメントの []][]します。

Visual Studio で StackExchange.Redis NuGet パッケージを使用してクライアント アプリケーションを構成するには、**ソリューション エクスプローラー**でプロジェクトを右クリックし、**[NuGet パッケージの管理]** をクリックします。

![Manage NuGet packages][nugetmenu]

**[オンライン検索]** ボックスに「**StackExchange.Redis**」または「**StackExchange.Redis.StrongName**」と入力し、結果の中から必要なバージョンを選択して、**[インストール]** をクリックします。
>[AZURE.NOTE] 厳密な名前を持つバージョンの **StackExchange.Redis** クライアント ライブラリを希望する場合は、**[StackExchange.Redis.StrongName]** を選択してください。それ以外の場合は、**[StackExchange.Redis]** を選択します。

![StackExchange.Redis NuGet package][stackexchangenuget]

クライアント アプリケーションから StackExchange.Redis Cache クライアントを使用して Azure Redis Cache にアクセスするために必要なアセンブリ参照が NuGet パッケージによってダウンロードされ追加されます。

クライアント プロジェクトをキャッシュ用に構成できたら、以降のセクションで説明されている、キャッシュを操作するための技法を使用できます。

<a name="working-with-caches"></a>
## キャッシュの操作

このセクションの手順では、キャッシュに対する一般的なタスクを行う方法について説明します。

-   [キャッシュへの接続します。][]
-   [追加オブジェクトをキャッシュ キャッシュから][]
-   [キャッシュ内の .NET オブジェクトを操作します。](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## キャッシュに接続する

プログラムでキャッシュを操作するには、キャッシュへの参照が必要です。 StackExchange.Redis クライアントを使用して Azure Redis Cache にアクセスするすべてのファイルの先頭に次のコードを追加します。

    using StackExchange.Redis;

>[AZURE.NOTE] StackExchange.Redis クライアントには、.NET Framework 4 以降が必要です。

Azure Redis Cache への接続は管理、 `ConnectionMultiplexer` クラスです。 このクラスはクライアント アプリケーションの開始から終了まで共有、再利用する前提で設計されており、操作単位で作成する必要はありません。

Azure Redis Cache に接続し、接続済みのインスタンスを取得する `ConnectionMultiplexer`, 、静的に呼び出す `接続` メソッドと、キャッシュ エンドポイントとキーのパスは次の例と同様にします。 password パラメーターには、Azure ポータルから生成されたキーを使用してください。

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] 警告: ソース コード内に資格情報を保存することは絶対に避けてください。 このサンプルでは、単純化するためにあえてソース コード内に記述しています。 参照してください [アプリケーション文字列と接続文字列の動作の][] 資格情報を格納する方法の詳細。

設定するか、SSL を使用しない場合は、 `ssl = false` か、または省略、 `ssl` パラメーター。
>[AZURE.NOTE] 既定では、新しいキャッシュに対して非 SSL ポートは無効になっています。 非 SSL ポートを有効にする手順については、次を参照してください、 [アクセス ポート](cache-configure.md#access-ports).。

共有する 1 つの方法として、 `ConnectionMultiplexer` は、アプリケーションのインスタンスを次の例のように接続されているインスタンスを返す静的プロパティが設定されています。 これによって接続されている 1 つだけを初期化するためにスレッド セーフであることが `ConnectionMultiplexer` インスタンス。 これらの例で `abortConnect` が false で、Azure Redis Cache への接続が確立されない場合でも、呼び出しが成功するように設定します。 大きな特長の 1 つ `ConnectionMultiplexer` キャッシュ、ネットワークの問題に接続を自動的に復元、またはその他の原因が解決されることです。

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

詳細については、構成オプションを接続に高度なを参照してください。 [StackExchange.Redis 構成モデル []][]します。

キャッシュのエンドポイントとキーは、ご利用のキャッシュ インスタンスの **Redis Cache** ブレードから取得できます。

![キャッシュのプロパティ][cacheproperties]

![キーの管理][managekeys]

接続が確立されるを呼び出して redis cache データベースへの参照を返す、 `ConnectionMultiplexer.GetDatabase` メソッドです。 返されるオブジェクト、 `GetDatabase` メソッドは、簡易パススルー オブジェクトであり保存する必要はありません。

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

項目に保存されを使用して、キャッシュから取得された、 `StringSet` と `StringGet` メソッドです。

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");
    
    string value = cache.StringGet("key1");

Redis では、ほとんどのデータが Redis 文字列として保存されますが、これらの文字列には、さまざまなデータ型を格納することができます。シリアル化したバイナリ データもその 1 つで、.NET のオブジェクトをキャッシュに保存する際に使用することができます。

呼び出すときに `StringGet`, 、オブジェクトが存在する場合は、返される、していない場合、 `null` が返されます。 その場合、目的のデータ ソースから値を取得してキャッシュに格納しておき、後で使用することができます。 これを "キャッシュ アサイド パターン" といいます。

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();
    
        cache.StringSet("key1", value);
    }

キャッシュ内の項目の有効期限を指定するには、使用、 `TimeSpan` のパラメーター `StringSet`します。

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## キャッシュ内で .NET オブジェクトを使用する

Azure Redis Cache はプリミティブ データ型に加え、.NET オブジェクトをキャッシュできますが、.NET オブジェクトをキャッシュするためには、あらかじめシリアル化しておく必要があります。 この処理はアプリケーション開発者が行わなければなりません。逆にそのことでシリアライザーの選択に幅が生まれ、開発者にとってのメリットとなっています。

オブジェクトをシリアル化の 1 つの簡単な方法を使用して、 `JsonConvert` でシリアル化メソッド [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) や JSON からにシリアル化とします。 次の例は、get および set を使用して、 `従業員` オブジェクト インスタンス。


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
    -   [Azure Redis キャッシュの ASP.NET 出力キャッシュ プロバイダー](cache-asp.net-output-cache-provider.md)
-   [キャッシュ診断を有効にする](cache-how-to-monitor.md#enable-cache-diagnostics) できるように [モニター](cache-how-to-monitor.md) 、キャッシュの正常性。 Azure ポータルでメトリックを表示できるし、することもできます [をダウンロードして確認](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) 好みのツールを使用しています。
-   チェック アウト、 [StackExchange.Redis キャッシュ クライアントのドキュメントの][]します。
    -   Azure Redis Cache は、さまざまな Redis クライアントや開発言語からアクセスできます。 詳細については、次を参照してください。 [http://redis.io/clients:operator[]][] と [Azure Redis Cache の [] のその他の言語で開発][]します。
    -   Azure Redis Cache は、Redsmin などのサービスと共に使用することもできます。 詳細については、次を参照してください。  [Azure Redis 接続文字列を取得し、Redsmin [] で使用する方法][]します。
-   参照してください、 [の redis][] ドキュメントおよびについて [redis データ型][] と [ページ Redis データ型][]します。







[next steps]: #next-steps 
[introduction to azure redis cache (video)]: #video 
[what is azure redis cache?]: #what-is 
[create an azure cache]: #create-cache 
[which type of caching is right for me?]: #choosing-cache 
[prepare your visual studio project to use azure caching]: #prepare-vs 
[configure your application to use caching]: #configure-app 
[get started with azure redis cache]: #getting-started-cache-service 
[create the cache]: #create-cache 
[configure the cache]: #enable-caching 
[configure the cache clients]: #NuGet 
[working with caches]: #working-with-caches 
[connect to the cache]: #connect-to-cache 
[add and retrieve objects from the cache]: #add-object 
[specify the expiration of an object in the cache]: #specify-expiration 
[store asp.net session state in the cache]: #store-session 
[newcachemenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-new-cache-menu.png 
[cachecreate]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-cache-create.png 
[stackexchangenuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png 
[nugetmenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png 
[cacheproperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png 
[managekeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png 
[sessionstatenuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png 
[browsecaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png 
[caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png 
[cachecreated]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-cache-created.png 
[http://redis.io/clients]: http://redis.io/clients 
[develop in other languages for azure redis cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx 
[how to retrieve an azure redis connection string and use it with redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache 
[azure redis session state provider]: http://go.microsoft.com/fwlink/?LinkId=398249 
[how to: configure a cache client programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx 
[session state provider for azure cache]: http://go.microsoft.com/fwlink/?LinkId=320835 
[azure appfabric cache: caching session state]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20 
[output cache provider for azure cache]: http://go.microsoft.com/fwlink/?LinkId=320837 
[azure shared caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx 
[team blog]: http://blogs.msdn.com/b/windowsazure/ 
[azure caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching 
[how to configure virtual machine sizes]: http://go.microsoft.com/fwlink/?LinkId=164387 
[azure caching capacity planning considerations]: http://go.microsoft.com/fwlink/?LinkId=320167 
[azure caching]: http://go.microsoft.com/fwlink/?LinkId=252658 
[how to: set the cacheability of an asp.net page declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx 
[how to: set a page's cacheability programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx 
[configure a cache in azure redis cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx 
[stackexchange.redis configuration model]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md 
[work with .net objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects 
[nuget package manager installation]: http://go.microsoft.com/fwlink/?LinkId=240311 
[cache pricing details]: http://www.windowsazure.com/pricing/details/cache/ 
[azure portal]: https://portal.azure.com/ 
[overview of azure redis cache]: http://go.microsoft.com/fwlink/?LinkId=320830 
[azure redis cache]: http://go.microsoft.com/fwlink/?LinkId=398247 
[migrate to azure redis cache]: http://go.microsoft.com/fwlink/?LinkId=317347 
[azure redis cache samples]: http://go.microsoft.com/fwlink/?LinkId=320840 
[using resource groups to manage your azure resources]: http://azure.microsoft.com/documentation/articles/resource-group-overview/ 
[stackexchange.redis]: http://github.com/StackExchange/StackExchange.Redis 
[stackexchange.redis cache client documentation]: http://github.com/StackExchange/StackExchange.Redis#documentation 
[redis]: http://redis.io/documentation 
[redis data types]: http://redis.io/topics/data-types 
[a fifteen minute introduction to redis data types]: http://redis.io/topics/data-types-intro 
[how application strings and connection strings work]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/ 
[azure free trial]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero 

