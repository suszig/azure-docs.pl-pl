<properties 
    pageTitle="Redis へのキャッシュの移行"
    description="Managed Cache Service アプリケーションを Azure Redis Cache に移行する方法を説明します"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="dwrede"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="12/03/2015"
    ms.author="sdanie" />


# Managed Cache Service から Azure Redis Cache への移行

Azure Managed Cache Service を使用するアプリケーションの Azure Redis Cache への移行は、キャッシュ アプリケーションで使用されている Managed Cache Service の機能によっては、最小限の変更をアプリケーションに対して行うだけで実現できます。 API はまったく同じではありませんがよく似ており、Managed Cache Service を使用してキャッシュにアクセスする既存コードの多くは最小限の変更で再利用できます。 このトピックでは、Managed Cache Service アプリケーションを Azure Redis Cache を使用するように移行するために必要な構成とアプリケーションの変更を行う方法、および Azure Redis Cache の機能を使用して Managed Cache Service キャッシュの機能を実装する方法について説明します。

## 移行の手順

Managed Cache Service アプリケーションを Azure Redis Cache を使用するように移行するには、以下の手順で行う必要があります。

-   Managed Cache Service の機能を Azure Redis Cache にマップする
-   キャッシュ プランを選択する
-   キャッシュを作成する
-   キャッシュ クライアントを構成する
    -   Managed Cache Service の構成を削除する
    -   StackExchange.Redis NuGet パッケージを使用してキャッシュ クライアントを構成する
-   Managed Cache Service のコードを移行する
    -   ConnectionMultiplexer クラスを使用してキャッシュに接続する
    -   キャッシュのプリミティブ データ型にアクセスする
    -   キャッシュ内で .NET オブジェクトを使用する
-   ASP.NET のセッション状態と出力キャッシュを Azure Redis Cache に移行する

## Managed Cache Service の機能を Azure Redis Cache にマップする

Azure Managed Cache Service と Azure Redis Cache は似ていますが、一部の機能の実装方法が異なります。 このセクションでは、そのような違いのいくつかについて説明し、Managed Cache Service の機能を Azure Redis Cache で実装する場合のガイダンスを提供します。

| Managed Cache Service の機能| Managed Cache Service のサポート| Azure Redis Cache のサポート|
|---|---|---|
| 名前付きキャッシュ| 既定のキャッシュが構成され、Standard および Premium キャッシュ プランでは、必要に応じて最大 9 個の名前付きキャッシュを追加構成できます。| Azure Redis キャッシュには 16 個のデータベースがあり、名前付きキャッシュに似た機能の実装に使用できます。詳細については、次を参照してください。 [サーバー構成の既定の Redis](cache-configure.md#default-redis-server-configuration)します。|
| 高可用性| Standard および Premium キャッシュ プランでは、キャッシュ内のアイテムの高可用性が提供されます。アイテムが障害によって失われた場合でも、キャッシュ内のアイテムのバックアップ コピーを使用できます。セカンダリ キャッシュへの書き込みは同期的に行われます。| 高可用性は Standard および Premium キャッシュ プランで利用でき、2 ノードのプライマリ/レプリカ構成を使用します (Premium キャッシュではシャードごとにプライマリ/レプリカ ペアがあります)。レプリカへの書き込みは非同期的に行われます。詳細については、次を参照してください。 [Azure Redis Cache の料金](https://azure.microsoft.com/pricing/details/cache/)します。|
| 通知| 名前付きキャッシュでさまざまなキャッシュ操作が発生したとき、クライアントは非同期の通知を受け取ることができます。| クライアント アプリケーションは Redis のパブリッシュ/サブスクライブを使用して、または [キースペース通知](cache-configure.md#keyspace-notifications-advanced-settings) 通知に類似した機能を実現するためにします。|
| ローカル キャッシュ| 特に高速のアクセスのため、キャッシュされたオブジェクトのコピーをクライアントにローカルに格納します。| クライアント アプリケーションは、ディクショナリまたは類似のデータ構造を使用してこの機能を実装する必要があります。|
| 削除ポリシー| None または LRU。既定のポリシーは LRU です。| Azure Redis Cache は、volatile-lru、allkeys-lru、volatile-random、allkeys-random、volatile-ttl、noeviction の各削除ポリシーをサポートします。既定のポリシーは volatile-lru です。詳細については、次を参照してください。 [サーバー構成の既定の Redis](cache-configure.md#default-redis-server-configuration)します。|
| 有効期限ポリシー| 既定の有効期限ポリシーは絶対であり、既定の有効期限は 10 分です。スライド式ポリシーおよび期限なしポリシーも使用できます。| 既定ではキャッシュ内のアイテムは期限切れしませんが、キャッシュ設定のオーバーロードを使用して書き込みごとに有効期限を構成できます。詳細については、次を参照してください。 [キャッシュからオブジェクトを追加および取得](cache-dotnet-how-to-use-azure-redis-cache.md#add-and-retrieve-objects-from-the-cache)します。|
| リージョンとタグ付け| リージョンは、キャッシュされるアイテムのサブグループです。また、リージョンでは、タグと呼ばれる追加の説明文を使用してキャッシュされるアイテムに注釈を付けることもできます。リージョンでは、そのリージョン内のタグ付きアイテムに対する検索操作を実行できます。リージョン内のすべてのアイテムは、キャッシュ クラスターの 1 つのノードに格納されます。| Redis キャッシュは 1 つのノードで構成され (Redis クラスターが有効になっていない場合)、Managed Cache Service のリージョンの概念は適用されません。Redis はキーを取得するときの検索とワイルドカード操作をサポートするので、説明的なタグをキー名に埋め込み、それを使用して後でアイテムを取得できます。Redis を使用したタグ付けソリューションの実装の例は、次を参照してください。 [キャッシュの Redis でタグ付けを実装する](http://stackify.com/implementing-cache-tagging-redis/)します。|
| シリアル化| Managed Cache は、NetDataContractSerializer、BinaryFormatter、およびカスタム シリアライザーの使用をサポートします。既定値は NetDataContractSerializer です。| キャッシュに格納する前の .NET オブジェクトのシリアル化は、クライアント アプリケーションで行う必要があります。使用するシリアライザーはクライアント アプリケーションの開発者が選択します。詳細とサンプル コードは、「 [キャッシュ内で .NET オブジェクトを扱う](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache)します。|

## キャッシュ プランを選択する

Microsoft Azure Redis Cache には、次のレベルがあります。

-   **Basic** – 単一ノード、 複数のサイズ、最大 53 GB
-   **Standard** – 2 ノード (プライマリ/レプリカ)。 複数のサイズ、最大 53 GB99.9% の SLA。
-   **Premium** – 最大 10 個のシャードがある 2 ノード (プライマリ/レプリカ)。 6 GB から 530 GB までの複数のサイズ (詳細はお問い合わせください)。 Standard レベルのすべての機能と詳細を含むサポート [Redis クラスター](cache-how-to-premium-clustering.md), 、[Redis の永続化](cache-how-to-premium-persistence.md), 、および [Azure Virtual Network](cache-how-to-premium-vnet.md)します。 99.9% の SLA。

各レベルは、機能と料金ごとに異なります。 このガイドの後半と料金の詳細については、機能はについてを参照してください [キャッシュの料金詳細](https://azure.microsoft.com/pricing/details/cache/)します。

移行では最初に、前の Managed Cache Service キャッシュのサイズに合ったサイズを選択し、アプリケーションの要件に応じてスケールアップまたはスケールダウンします。 Azure Redis Cache に適したオファリングを選択する詳細については、を参照してください [どのような Redis キャッシュ オファリングとサイズを使用するでしょうか。](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)。.

## キャッシュを作成する

内で Azure Redis Cache でキャッシュを作成できる、 [Azure ポータル](https://portal.azure.com), 、または ARM テンプレート、PowerShell、または Azure CLI を使用しています。

-   Azure ポータルでキャッシュを作成するを参照してください。 [キャッシュを作成する](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache)します。
-   ARM テンプレートを使用してキャッシュを作成するを参照してください。 [テンプレートを使用して Redis cache の作成](cache-redis-cache-arm-provision.md)します。
-   Azure PowerShell を使用してキャッシュを作成するを参照してください。 [Azure PowerShell での Azure Redis Cache の管理](cache-howto-manage-redis-cache-powershell.md)します。
-   Azure CLI を使用してキャッシュを作成するを参照してください。 [を作成して、Azure コマンド ライン インターフェイス (Azure CLI) を使用して Azure Redis Cache を管理する方法](cache-manage-cli.md)します。

>[AZURE.NOTE] Azure Redis Cache を使用するには、Azure アカウントが必要です。 アカウントがない場合は、無料試用版のアカウントを数分で作成できます。 詳細については、「 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero)します。

## キャッシュ クライアントを構成する

キャッシュを作成して構成した後は、Managed Cache Service の構成を削除し、Azure Redis Cache の構成と参照を追加して、キャッシュ クライアントがキャッシュにアクセスできるようにします。

-   Managed Cache Service の構成を削除する
-   StackExchange.Redis NuGet パッケージを使用してキャッシュ クライアントを構成する

### Managed Cache Service の構成を削除する

Azure Redis Cache 用にクライアント アプリケーションを構成するには、その前に、Managed Cache Service の NuGet パッケージをアンインストールすることによって、既存の Managed Cache Service の構成とアセンブリ参照を削除する必要があります。

Managed Cache Service の NuGet パッケージをアンインストールするには、**ソリューション エクスプローラー**でクライアント プロジェクトを右クリックし、**[NuGet パッケージの管理]** をクリックします。 選択、 **インストール済みパッケージの** ノード、および種類 W**indowsAzure.Caching** インストールされているパッケージの検索ボックスにします。 **[Windows** **Azure Cache]** (または、NuGet パッケージのバージョンによっては **[Windows** **Azure Caching]**) を選択し、**[アンインストール]** をクリックして、**[閉じる]** をクリックします。

![Azure Managed Cache Service NuGet パッケージのアンインストール](./media/cache-migrate-to-redis/IC757666.jpg)

Managed Cache Service の NuGet パッケージをアンインストールすると、クライアント アプリケーションの app.config または web.config の Managed Cache Service アセンブリおよび Managed Cache Service エントリが削除されます。 NuGet パッケージをアンインストールしてもカスタマイズした設定は削除されない場合があるので、web.config または app.config を開き、次の要素が完全に削除されていることを確認します。

いることを確認、 `dataCacheClients` からエントリを削除、 `configSections` 要素。 全体を削除しないでください `configSections` 要素; を削除、 `dataCacheClients` エントリが存在する場合。

    <configSections>
      
      <section name="dataCacheClients"type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" allowLocation="true" allowDefinition="Everywhere"/>
    </configSections>

いることを確認、 `dataCacheClients` セクションを削除します。  `DataCacheClients` セクションに次の例のようになります。

    <dataCacheClients>
      <dataCacheClientname="default">
        
        
        <autoDiscoverisEnabled="true"identifier="[Cache role name or Service Endpoint]"/>
    
        
        
        
      </dataCacheClient>
    </dataCacheClients>

Managed Cache Service の構成を削除した後は、次のセクションで説明するようにキャッシュ クライアントを構成できます。

### StackExchange.Redis NuGet パッケージを使用してキャッシュ クライアントを構成する

Visual Studio で開発された .NET アプリケーションは、StackExchange.Redis キャッシュ クライアントを使用してキャッシュにアクセスできます。 Visual Studio で StackExchange.Redis NuGet パッケージを使用してクライアント アプリケーションを構成するには、**ソリューション エクスプローラー**でプロジェクトを右クリックし、**[NuGet パッケージの管理]** をクリックします。

![Azure Redis Cache Manage NuGet パッケージ](./media/cache-migrate-to-redis/IC729541.png)

**[オンライン検索]** ボックスに「**StackExchange.Redis**」と入力し、結果の中からそれを選択して **[インストール]** をクリックします。

![Azure Redis Cache StackExchange.Redis NuGet パッケージ](./media/cache-migrate-to-redis/IC746253.png)

クライアント アプリケーションから StackExchange.Redis Cache クライアントを使用して Azure Redis Cache にアクセスするために必要なアセンブリ参照が NuGet パッケージによってダウンロードされ追加されます。

詳細については、次を参照してください。 [キャッシュ クライアントの構成](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients)します。

## Managed Cache Service のコードを移行する

StackExchange.Redis キャッシュ クライアントの API は、Managed Cache Service に似ています。 このセクションでは相違点の概要を示します。

### ConnectionMultiplexer クラスを使用してキャッシュに接続する

マネージ キャッシュ サービスで処理された、キャッシュへの接続、 `DataCacheFactory` と `DataCache` クラスです。 Azure Redis Cache でこれらの接続の管理、 `ConnectionMultiplexer` クラスです。

キャッシュにアクセスするファイルの先頭に、次の using ステートメントを追加します。

    using StackExchange.Redis

この名前空間が解決しない場合は、必ず」の説明に従って、StackExchange.Redis NuGet パッケージを追加した [キャッシュ クライアントの構成](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients)します。
>[AZURE.NOTE] StackExchange.Redis クライアントでは .NET Framework 4 以降が必要であることに注意してください。

Azure Redis Cache インスタンスに接続する場合、静的なを呼び出す `ConnectionMultiplexer.Connect` メソッドと、エンドポイントとキーを渡します。 共有する 1 つの方法として、 `ConnectionMultiplexer` は、アプリケーションのインスタンスを次の例のように接続されているインスタンスを返す静的プロパティが設定されています。 これによって接続されている 1 つだけを初期化するためにスレッド セーフであることが `ConnectionMultiplexer` インスタンス。 この例では `abortConnect` が、キャッシュへの接続が確立されない場合でも、呼び出しが成功するには false に設定します。 大きな特長の 1 つ `ConnectionMultiplexer` キャッシュ、ネットワークの問題に接続を自動的に復元、またはその他の原因が解決されることです。

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

キャッシュのエンドポイント、キー、ポートは、ご利用のキャッシュ インスタンスの **[Redis Cache]** ブレードから取得できます。 詳細については、次を参照してください。 [Redis キャッシュ プロパティ](cache-configure.md#properties)します。

接続が確立されるを呼び出して Redis cache データベースへの参照を返す、 `ConnectionMultiplexer.GetDatabase` メソッドです。 返されるオブジェクト、 `GetDatabase` メソッドは、簡易パススルー オブジェクトであり保存する必要はありません。

    IDatabase cache = Connection.GetDatabase();
    
    // Perform cache operations using the cache object...
    // Simple put of integral data types into the cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);
    
    // Simple get of data types from the cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

StackExchange.Redis クライアントを使用して、 `RedisKey` と `RedisValue` にアクセスすると、キャッシュに項目を格納する型。 これらの型は文字列などのほとんどのプリミティブ言語型にマップし、多くの場合、直接は使用されません。 Redis を含むメソッドを使用する型を直接使用できますが、いない、および文字列は Redis 値の最も基本的な種類とさまざまな種類のシリアル化されたバイナリ ストリームを含むデータを含めることができます `文字列` 名前にします。 ほとんどのプリミティブ データ型の格納および使用してキャッシュから項目を取得、 `StringSet` と `StringGet` メソッド、コレクションまたはその他の Redis データ型は、キャッシュに格納する場合を除き、します。

`StringSet` と `StringGet` マネージ キャッシュ サービスとよく似ています `配置` と `取得` メソッドは、大きな違いは、設定して、.NET オブジェクトをキャッシュに入れたりする前にする必要がありますシリアル化がある、最初にします。

呼び出すときに `StringGet`, 、オブジェクトが存在する場合はそれが返され、そうでない場合は、null が返されます。 その場合、目的のデータ ソースから値を取得してキャッシュに格納しておき、後で使用することができます。 これを "キャッシュ アサイド パターン" といいます。

キャッシュ内の項目の有効期限を指定するには、使用、 `TimeSpan` のパラメーター `StringSet`します。

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

Azure Redis Cache はプリミティブ データ型に加え、.NET オブジェクトに対応していますが、.NET オブジェクトをキャッシュするためには、あらかじめシリアル化しておく必要があります。 これは、アプリケーション開発者が行う必要があります。 これにより、開発者はシリアライザーを柔軟に選択できます。 詳細とサンプル コードは、「 [キャッシュ内で .NET オブジェクトを扱う](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache)します。

## ASP.NET のセッション状態と出力キャッシュを Azure Redis Cache に移行する

Azure Redis Cache には、ASP.NET セッション状態とページ出力キャッシュの両方に対するプロバイダーがあります。 これらのプロバイダーの Managed Cache Service バージョンを使用するアプリケーションを移行するには、まず web.config から既存のセクションを削除した後、Azure Redis Cache バージョンのプロバイダーを構成します。 Azure Redis キャッシュの ASP.NET プロバイダーの使用方法の詳細については、次を参照してください。 [Azure Redis Cache 用の ASP.NET セッション状態プロバイダー](cache-asp.net-session-state-provider.md) と [Azure Redis Cache 用の ASP.NET 出力キャッシュ プロバイダー](cache-asp.net-output-cache-provider.md)します。

## 次のステップ

調査、 [Azure Redis Cache のドキュメント](https://azure.microsoft.com/documentation/services/cache/) のチュートリアル、サンプル、ビデオなどです。






