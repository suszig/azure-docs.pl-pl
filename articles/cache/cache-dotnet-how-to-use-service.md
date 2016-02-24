<properties 
    pageTitle="Azure Managed Cache Service の使用方法" 
    description="Azure Managed Cache Service を使用して Azure アプリケーションのパフォーマンスを向上させる方法を説明します" 
    services="cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="12/03/2015" 
    ms.author="sdanie"/>

# Azure Managed Cache Service の使用方法

このガイドでは、**Microsoft Azure のキャッシュのインロール キャッシュ** 
**Azure Managed Cache Service**します。 サンプルは C\# コードで記述し、
.NET API を使用します。 紹介するシナリオ **の作成および構成キャッシュ**, 、**キャッシュ クライアントの構成**, 、**の追加と削除
オブジェクトをキャッシュ内の ASP.NET セッション状態を格納するキャッシュから**,、
 **ASP.NET ページ出力キャッシュのキャッシュを使用して有効にすると**です。 サブスクリプションの
Azure Cache の使用の詳細については、[次のステップ]」を参照してください。

>[AZURE.IMPORTANT]Azure マネージ キャッシュ サービスと Azure ロール内キャッシュの [2016 年 11 月 30 日、提供終了発表します。 提供終了に備えて、Azure Redis Cache に移行することをお勧めします。 日付と移行のガイドラインの詳細については、次を参照してください。 [どの Azure Cache を利用に最適なでしょうか。](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

<a name="what-is"></a>
## Azure Managed Cache Service とは

Azure Managed Cache Service とは、拡張性の高い分散型メモリ内ソリューションです。これを使用すると、データへの超高速アクセスを提供することで、拡張性や応答性の高いアプリケーションを作成できます。

Azure マネージ キャッシュ サービスには、次が含まれています
機能:

-   セッション状態とページ出力の構築済みの ASP.NET プロバイダー
    キャッシュ、しなくても web アプリケーションの機能を有効にします。
    アプリケーション コードを変更します。
-   たとえば、シリアル化可能なマネージ オブジェクトをキャッシュ: CLR オブジェクト、行、XML、
    バイナリ データです。
-   Azure と Windows の両方で一貫性のある開発モデル
    Server AppFabric します。

Managed Cache Service を使用すると、マイクロソフトによって管理されている、セキュリティで保護された専用キャッシュにアクセスできます。 Managed Cache Service を使用して作成されたキャッシュには、Azure Websites、Web ロールと Worker ロール、および Virtual Machines で実行されている Azure 内のアプリケーションからアクセスできます。

Managed Cache Service は、次の 3 つのレベルで利用できます。

-   Basic - 128 MB ～ 1 GB のキャッシュ
-   Standard - 1 GB ～ 10 GB のキャッシュ
-   Premium - 5 GB ～ 150 GB のキャッシュ

各レベルは、機能と価格ごとに異なります。 このガイドの後半と料金の詳細については、機能はについては、[キャッシュの料金詳細] を参照してください。

このガイドでは、Managed Cache Service の基本的な概要について説明します。 この概要では扱われていない機能についてのより詳細なガイドでは、[概要 Azure マネージ キャッシュ サービスの] を参照してください。

<a name="getting-started-cache-service"></a>
## Cache Service の概要

Managed Cache Service は、簡単に使い始めることができます。 使い始めるには、キャッシュをプロビジョニングして構成します。 次に、キャッシュ クライアントを構成してキャッシュにアクセスできるようにします。 キャッシュ クライアントを構成すると、使い始めることができます。

-   [キャッシュの作成][]
-   [キャッシュの構成][]
-   [キャッシュ クライアントの構成][]

<a name="create-cache"></a>
## キャッシュの作成

Managed Cache Service のキャッシュ インスタンスは、PowerShell コマンドレットを使用して作成されます。 

>Managed Cache Service インスタンスを作成するには、表示し、[Azure クラシック ポータル] で構成されているが、PowerShell コマンドレットを使用します。

Managed Cache Service インスタンスを作成するには、Azure PowerShell コマンド ウィンドウを開きます。

>インストールと Azure PowerShell を使用する手順については、[Azure PowerShell を構成する方法] を参照してください。 []。

[Add-azureaccount] のコマンドレットを呼び出すし、電子メール アドレスと、アカウントに関連付けられているパスワードを入力します。 サブスクリプションでは、既定で選択され、[Add-azureaccount] コマンドレットを呼び出すに表示されます。 サブスクリプションを変更するには、[Select-azuresubscription] コマンドレットを呼び出します。

>アカウントの証明書を使用して Azure PowerShell を構成した場合は、このステップを省略できます。 Azure アカウントに Azure PowerShell を接続する方法の詳細については、[Azure PowerShell を構成する方法] を参照してください。 []。

サブスクリプションが既定で選択されて表示されます。 サブスクリプションを変更するには、[Select-azuresubscription] コマンドレットを呼び出します。

[New-azuremanagedcache] コマンドレットを起動し、名前、リージョン、キャッシュ オファリング、およびキャッシュのサイズを指定します。

 **名前**, 、キャッシュ エンドポイントに使用するサブドメイン名を入力します。 エンドポイントは、数字と小文字のみを含む、先頭が文字の 6 ～ 12 文字の文字列にしてください。

 **場所**, 、キャッシュのリージョンを指定します。 パフォーマンスを最高にするには、キャッシュ クライアント アプリケーションと同じリージョンにキャッシュを作成します。

**Sku** と **メモリ** 連携して、キャッシュのサイズを決定します。 Managed Cache Service は、次の 3 つのレベルで利用できます。

-   Basic - キャッシュ サイズ 128 MB ～ 1 GB (128 MB ずつ増分)、既定で名前付きキャッシュ 1 つ
-   Standard - キャッシュ サイズ 1 GB ～ 10 GB (1 GB ずつ増分)、通知と最大 10 の名前付きキャッシュのサポート
-   Premium - キャッシュ サイズ 5 GB ～ 150 GB (5 GB ずつ増分)、通知、高可用性、および最大 10 の名前付きキャッシュのサポート

選択、 **Sku** と **メモリ** 、アプリケーションのニーズを満たしています。 一部のキャッシュ機能 (通知や高可用性など) は、特定のキャッシュ オファリングでのみ使用できる点に注意してください。 アプリケーションに最適なキャッシュ オファリングとサイズを選択する方法の詳細については、[キャッシュ オファリング][] を参照してください。

 次の例では、128 MB の Basic キャッシュが contosocache という名前で、South Central US リージョンに作成されます。

    New-AzureManagedCache -Name contosocache -Location "South Central US" -Sku Basic -Memory 128MB

>パラメーターとキャッシュを作成するときに使用できる値の一覧については、[New-azuremanagedcache] コマンドレットのドキュメントを参照してください。

PowerShell コマンドレットを呼び出した後、キャッシュが作成されるまで数分かかる場合があります。 キャッシュが作成されると、新しいキャッシュは、 `Running` 状態とは、既定の設定で使用できるようと表示して [Azure クラシック ポータル] で構成されています。 キャッシュの構成をカスタマイズするには、次の [キャッシュの構成]」セクションを参照してください。

Azure PowerShell ウィンドウで作成の進捗状況をモニタリングできます。 キャッシュが使用できるように、[New-azuremanagedcache] コマンドレットで次の例に示すようにキャッシュ情報が表示されます。

    PS C:\> Add-AzureAccount
    VERBOSE: Account "user@domain.com" has been added.
    VERBOSE: Subscription "MySubscription" is selected as the default subscription.
    VERBOSE: To view all the subscriptions, please use Get-AzureSubscription.
    VERBOSE: To switch to a different subscription, please use Select-AzureSubscription.
    PS C:\> New-AzureManagedCache -Name contosocache -Location "South Central US" -Sku Basic -Memory 128MB
    VERBOSE: Intializing parameters...
    VERBOSE: Creating prerequisites...
    VERBOSE: Verify cache service name...
    VERBOSE: Creating cache service...
    VERBOSE: Waiting for cache service to be in ready state...


    Name     : contosocache
    Location : South Central US
    State    : Active
    Sku      : Basic
    Memory   : 128MB



    PS C:\>




<a name="enable-caching"></a>
## キャッシュの構成

 **構成** タブでは Azure クラシック ポータルでのキャッシュでは、キャッシュのオプションを構成します。 各キャッシュには、 **既定** 名前付きキャッシュ、および Standard および Premium キャッシュ オファリングでは、最大 9 つの名前付きキャッシュ、合計 10 をサポートします。 名前付きキャッシュにはそれぞれ独自のオプション セットがあり、柔軟性の高い方法でキャッシュを構成できます。

![NamedCaches][NamedCaches]

名前付きキャッシュを作成するに新しいキャッシュの名前を入力、 **名前** ボックスをクリックして、目的のオプションを指定 **保存**, 、] をクリック **はい** を確認します。 すべての変更を取り消すにはクリックして **破棄**します。

## [有効期限ポリシー] と [時間 (分)] ##

 **有効期限ポリシー** と連携して、 **時間 (分)** キャッシュされたアイテムの有効期限を確認する設定。 有効期限ポリシーの 3 つの種類があります: **絶対**, 、**スライド**, 、および **Never**します。 

 **絶対** が指定されている指定された有効期間 **時間 (分)** に項目をキャッシュに追加するときに開始します。 によって指定された期間後 **時間 (分)** が経過すると、項目は期限切れです。 

 **スライド** が指定されている指定された有効期間 **時間 (分)** 項目がキャッシュにアクセスされるたびにリセットされます。 により指定された期間まで、項目は期限切れいない **時間 (分)** 項目に最終アクセス後に経過するとします。

 **Never** が指定されている **時間 (分)** に設定する必要があります **0**, 、項目は期限切れになりません。

**絶対** 既定の有効期限ポリシーは、既定の設定は 10 分です **時間 (分)**します。 名前付きキャッシュ内の各項目についての有効期限ポリシーは固定しますが、 **時間 (分)** を使用して各項目用にカスタマイズできる **追加** と **配置** タイムアウト パラメーターを受け取るオーバー ロードします。

削除ポリシーと有効期限ポリシーの詳細については、[有効期限と削除] のを参照してください。

## 通知 ##

キャッシュ クラスターでさまざまなキャッシュ操作が発生したときに、アプリケーションが非同期通知を受け取ることができるようにするキャッシュ通知です。 キャッシュ通知は、ローカルにキャッシュされた自動無効化も行います。 詳細については、[通知] を参照してください。

>通知は、Standard および Premium キャッシュ オファリングでのみ使用できます。Basic キャッシュ オファリングでは使用できません。 詳細については、[キャッシュ オファリング] を参照してください。

## 高可用性 ##

高可用性が有効な場合、バックアップ コピーはキャッシュに追加された各項目で作成されます。 項目のプライマリ コピーで予期しないエラーが発生した場合でも、バックアップ コピーを使用できます。

その名が示すとおり、高可用性を使用すると、各キャッシュ項目に必要なメモリの量が 2 倍に増えます。 容量計画を行うときは、このメモリの影響を考慮に入れてください。 詳細については、[高可用性] を参照してください。

>高可用性は、Premium キャッシュ オファリングでのみ使用できます。Basic および Standard キャッシュ オファリングでは使用できません。 詳細については、[キャッシュ オファリング] を参照してください。

## 削除 ##

キャッシュ内で使用可能なメモリ容量を維持するため、Least Recently Used (LRU) 削除がサポートされています。 メモリ消費量がメモリしきい値を超えると、メモリ容量に余裕が出るまで、期限切れかどうかに関係なくオブジェクトがメモリから削除されます。
削除は、既定で有効です。 削除が無効な場合、容量に到達しても項目はキャッシュから削除されないため、Put 操作と Add 操作に失敗します。

削除ポリシーと有効期限ポリシーの詳細については、[有効期限と削除] のを参照してください。

キャッシュの構成が済んだら、キャッシュにアクセスできるようにキャッシュ クライアントを構成できます。

<a name="NuGet"></a>
## キャッシュ クライアントの構成

Managed Cache Service を使用して作成されたキャッシュには、Azure Websites、Web ロールと Worker ロール、および Virtual Machines で実行されている Azure アプリケーションからアクセスできます。 キャッシュ クライアント アプリケーションの構成を容易にする NuGet パッケージが用意されています。 

Cache NuGet パッケージを使用してクライアント アプリケーションを構成するので、プロジェクトを右クリックし **ソリューション エクスプ ローラー** 選択 **NuGet パッケージの管理**します。 

![NuGetPackageMenu][NuGetPackageMenu]

型 **WindowsAzure.Caching** に、 **オンライン検索** テキスト ボックス、および選択 **Windows**  
**Azure** **キャッシュ** 結果からです。 クリックして **インストール**, 、] をクリックし、 **同意**します。

![NuGetPackage][NuGetPackage]

NuGet パッケージは、いくつかの処理を行います。アプリケーションの構成ファイルに必要な構成を追加したり、必要なアセンブリ参照を追加したりします。 クラウド サービス プロジェクトの場合、Cloud Services の ServiceConfiguration.cscfg ファイルにキャッシュ クライアント診断レベル設定も追加します。

>ASP.NET Web プロジェクトでは、Cache NuGet パッケージは 2 つのコメント アウトされたセクションも web.config に追加します。 1 つ目のセクションはセッション状態をキャッシュに格納できるようにし、2 つ目のセクションは ASP.NET ページがキャッシュを出力できるようにします。 詳細については、次を参照してください。 [How To: キャッシュ内の ASP.NET セッション状態の格納] と [How To: ASP.NET ページ出力キャッシュ格納、キャッシュ内] []。

NuGet パッケージは、次の構成要素をロールの web.config または app.config に追加します。 A **dataCacheClients** セクションおよび **cacheDiagnostics** 下のセクションに追加されます、 **configSections** 要素。 ある場合ない **configSections** の子として作成いずれかの要素が存在、 **構成** 要素。

    <configSections>
      <!-- Existing sections omitted for clarity. -->
      <section name="dataCacheClients" 
        type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection,
              Microsoft.ApplicationServer.Caching.Core" 
        allowLocation="true" 
        allowDefinition="Everywhere" />
  
    <section name="cacheDiagnostics" 
        type="Microsoft.ApplicationServer.Caching.AzureCommon.DiagnosticsConfigurationSection,
              Microsoft.ApplicationServer.Caching.AzureCommon" 
        allowLocation="true" 
        allowDefinition="Everywhere" />


これらの新しいセクションを参照している、 **dataCacheClients** にも追加された要素、 **構成** 要素。

    <dataCacheClients>
      <dataCacheClient name="default">
        <!--To use the in-role flavor of Azure Caching, 
            set identifier to be the cache cluster role name -->
        <!--To use the Azure Caching Service,
            set identifier to be the endpoint of the cache cluster -->
        <autoDiscover isEnabled="true" identifier="[Cache role name or Service Endpoint]" />
        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
        <!--Use this section to specify security settings for connecting to your cache. 
            This section is not required if your cache is hosted on a role that is a part
            of your cloud service. -->
        <!--<securityProperties mode="Message" sslEnabled="false">
          <messageSecurity authorizationInfo="[Authentication Key]" />
        </securityProperties>-->
      </dataCacheClient>
    </dataCacheClients>


構成が追加されたら、新たに追加された構成で次の 2 つの項目を置き換えます。

1. 置換 **[キャッシュのロール名またはサービス エンドポイント]** 、エンドポイントのダッシュ ボード、Azure 旧ポータルに表示されます。

    ![エンドポイント][Endpoint]

2. SecurityProperties セクションをコメント解除し、置換 **[Authentication Key]** 認証キーに含まれている Azure クラシック ポータルで分断 **キーの管理** キャッシュ ダッシュ ボードからです。

    ![AccessKeys][AccessKeys]

>これらの設定を適切に構成しないと、クライアントはキャッシュにアクセスできません。

クラウド サービス プロジェクトの場合、NuGet パッケージも追加、 **ClientDiagnosticLevel** 設定、 **ConfigurationSettings** 、キャッシュ クライアント ロールの ServiceConfiguration.cscfg 内です。 次の例は、 **WebRole1** ServiceConfiguration.cscfg ファイルでセクション、 **ClientDiagnosticLevel** の既定値は 1 に **ClientDiagnosticLevel**します。

    <Role name="WebRole1">
      <Instances count="1" />
      <ConfigurationSettings>
        <!-- Existing settings omitted for clarity. -->
        <Setting name="Microsoft.WindowsAzure.Plugins.Caching.ClientDiagnosticLevel" 
                 value="1" />
      </ConfigurationSettings>
    </Role>

>クライアント診断レベルは、キャッシュ クライアントのために収集されるキャッシュ診断情報のレベルを構成します。 詳細については、[トラブルシューティングと診断] のを参照してください。

NuGet パッケージは、次のアセンブリへの参照も追加します。

-   Microsoft.ApplicationServer.Caching.Client.dll
-   Microsoft.ApplicationServer.Caching.Core.dll
-   Microsoft.WindowsFabric.Common.dll
-   Microsoft.WindowsFabric.Data.Common.dll
-   Microsoft.ApplicationServer.Caching.AzureCommon.dll
-   Microsoft.ApplicationServer.Caching.AzureClientHelper.dll

プロジェクトが Web プロジェクトである場合は、次のアセンブリ参照も追加されます。

-   Microsoft.Web.DistributedCache.dll

クライアント プロジェクトをキャッシュ用に構成できたら、以降のセクションで説明されている、キャッシュを操作するための技法を使用できます。

<a name="working-with-caches"></a>
## キャッシュの操作

このセクションの手順では、キャッシュに対する一般的なタスクを行う方法について説明します。

-   [方法: DataCache オブジェクトを作成][]
-   [方法: 追加し、キャッシュからオブジェクトを取得][]
-   [方法: キャッシュでオブジェクトの有効期限を指定][]
-   [方法: キャッシュで ASP.NET セッション状態を格納する][]
-   [方法: ASP.NET ページ出力キャッシュにキャッシュを格納する][]

<a name="create-cache-object"></a>
## 方法: DataCache オブジェクトを作成する

プログラムでキャッシュを操作するには、キャッシュへの参照が必要です。 使用するファイルの先頭に、次の内容を追加します。
Azure Cache:

    using Microsoft.ApplicationServer.Caching;

>Cache NuGet パッケージをインストールして、必要な参照が追加された後も、Visual Studio が using ステートメント内の型を認識しない場合は、
4 以降が必要な参照を追加するには、Cache NuGet パッケージをインストールしていることを確認ターゲット
プロジェクトは .NET Framework 4 以降をプロファイルしが指定されていないプロファイルのいずれかを選択するようにして **Client Profile**します。 キャッシュ クライアントの構成方法の詳細については、次を参照してください。 [キャッシュ クライアントの構成] []。

作成する 2 つの方法がある、 **DataCache** オブジェクトです。 最初の方法は、単に作成する、 **DataCache**, 、目的のキャッシュの名前を渡しています。

    DataCache cache = new DataCache("default");

1 回、 **DataCache** がインスタンス化を使用できます、キャッシュを操作するように、次のセクションで説明します。

2 番目の方法を使用するのには、新しい作成 **DataCacheFactory** 既定のコンス トラクターを使用して、アプリケーション内のオブジェクト。 このようにすると、キャッシュ クライアントが構成ファイル内の設定を使うようになります。 いずれかを呼び出す、 **GetDefaultCache** の新しいメソッド **DataCacheFactory** インスタンス、 **DataCache** オブジェクト、または **GetCache** メソッドとキャッシュの名前のパス。 これらのメソッドを返す、 **DataCache** プログラムを使用して、キャッシュにアクセスするために使用するオブジェクト。

    // Cache client configured by settings in application configuration file.
    DataCacheFactory cacheFactory = new DataCacheFactory();
    DataCache cache = cacheFactory.GetDefaultCache();
    // Or DataCache cache = cacheFactory.GetCache("MyCache");
    // cache can now be used to add and retrieve items. 

<a name="add-object"></a>
## 方法: キャッシュでオブジェクトを追加および削除する

キャッシュに項目を追加する、 **追加** メソッドまたは **配置** メソッド
使用できます。  **追加** メソッドは、キャッシュに指定したオブジェクトを追加
キーのパラメーターの値によってキー指定されます。

    // Add the string "value" to the cache, keyed by "item"
    cache.Add("item", "value");

同じキーを持つオブジェクトが既に、キャッシュ内にある場合、
**DataCacheException** が次のメッセージと共にスローされます。

> : しようと errorcode:substatus を持つオブジェクトを作成するには
> 作成しようとしています。 キャッシュは、
> オブジェクトに対して一意のキーの値のみを受け入れます。

特定のキーを持つオブジェクトを取得する、 **取得** メソッドを使用できます。 オブジェクトが存在する場合
返されると、そうでない場合は、null が返されます。

    // Add the string "value" to the cache, keyed by "key"
    object result = cache.Get("Item");
    if (result == null)
    {
        // "Item" not in cache. Obtain it from specified data source
        // and add it.
        string value = GetItemValue(...);
        cache.Add("item", value);
    }
    else
    {
        // "Item" is in cache, cast result to correct type.
    }

 **配置** メソッドは、指定のキーを持つオブジェクトをキャッシュに追加
存在しないまたはが存在する場合は、オブジェクトを置き換えます。

    // Add the string "value" to the cache, keyed by "item". If it exists,
    // replace it.
    cache.Put("item", "value");

<a name="specify-expiration"></a>
## 方法: キャッシュ内のオブジェクトの有効期限を指定する

既定では、キャッシュ内の項目はキャッシュに置かれてから 10 分で期限切れになります。 これで構成することができます、 **時間 (分)** Azure クラシック ポータルでキャッシュの構成] タブで設定します。

![NamedCaches][NamedCaches]

次の 3 つの種類があります **有効期限ポリシー**: **Never**, 、**絶対**, 、および **スライド**します。 これらは、構成方法 **時間 (分)** を使用して有効期限を決定します。 既定値 **有効期限の種類** は **絶対**, 、アイテムがキャッシュに保存するときに、項目の有効期限のカウント ダウン タイマーが開始されることを意味します。 指定された時間が経過すると、項目は期限切れになります。 場合 **スライド** が指定すると、項目の有効期限のカウント ダウンは、キャッシュに項目がアクセスされるたびにリセットし、項目は、最後のアクセス以降に一定の時間が経過するまで期限切れです。 場合 **Never** が指定されると、 **時間 (分)** に設定する必要があります **0**, 、およびアイテムの有効期限はありません、キャッシュ内にある限りに、有効なままです。

項目が追加またはを使用してキャッシュに更新するときに、特定の期間を指定することができますキャッシュ プロパティで構成されているよりも長いまたは短いタイムアウト間隔を使用する場合は、
オーバー ロード **追加** と **配置** を受け取る、 **TimeSpan** パラメーター。 イン
次の例では、文字列 **値** でキー指定されたキャッシュに追加されます
**項目**, 、30 分のタイムアウトを設定しています。

    // Add the string "value" to the cache, keyed by "item"
    cache.Add("item", "value", TimeSpan.FromMinutes(30));

キャッシュ内の項目の残りのタイムアウト間隔を表示するのには
**GetCacheItem** メソッドを使用して取得すること、 **DataCacheItem**
キャッシュ内の項目についての情報を含むオブジェクトを含む
残りのタイムアウト間隔。

    // Get a DataCacheItem object that contains information about
    // "item" in the cache. If there is no object keyed by "item" null
    // is returned. 
    DataCacheItem item = cache.GetCacheItem("item");
    TimeSpan timeRemaining = item.Timeout;

<a name="store-session"></a>
## 方法: ASP.NET セッション状態をキャッシュに格納する

Azure のキャッシュのセッション状態プロバイダーは、
ASP.NET アプリケーション用のプロセス外ストレージ メカニズムです。 このプロバイダー
Azure のキャッシュ内ではなく、セッション状態を格納することができます。
メモリ内や SQL Server データベース内。 キャッシュ セッション
状態プロバイダーは、まず、キャッシュを構成し、[はじめにマネージ キャッシュ サービスの]」の説明に従って、Cache NuGet パッケージを使用して、キャッシュの ASP.NET アプリケーションを構成します。 Cache NuGet パッケージをインストールすると、コメント アウトされたセクションが web.config に追加されます。これには、ASP.NET アプリケーションが Azure のキャッシュ用のセッション状態プロバイダーを使用するために必要な構成が含まれています。

    <!--Uncomment this section to use Azure Caching for session state caching
    <system.web>
      <sessionState mode="Custom" customProvider="AFCacheSessionStateProvider">
        <providers>
          <add name="AFCacheSessionStateProvider" 
            type="Microsoft.Web.DistributedCache.DistributedCacheSessionStateStoreProvider,
                  Microsoft.Web.DistributedCache" 
            cacheName="default" 
            dataCacheClientName="default"/>
        </providers>
      </sessionState>
    </system.web>-->

>Web.config には、Cache NuGet パッケージをインストールした後のこのコメント アウトされたセクションが含まれていない場合、最新の NuGet パッケージ マネージャーが [NuGet パッケージ マネージャーのインストール] からインストールされていることを確認し、アンインストールしてパッケージを再インストールします。

Azure のキャッシュ用のセッション状態プロバイダーを有効にするには、指定されたセクションをコメント解除します。 既定のキャッシュは、提供されるスニペットで指定されています。 別のキャッシュを使用するには、目的のキャッシュを指定、 **cacheName** 属性です。

詳細については、マネージ キャッシュ サービス セッション状態を使用します。
プロバイダーは、の [Azure のキャッシュのセッション状態プロバイダー] を参照してください。

<a name="store-page"></a>
## 方法: ASP.NET ページ出力キャッシュをキャッシュに格納する

Azure のキャッシュの出力キャッシュ プロバイダーは、出力キャッシュ データ用のプロセス外ストレージ メカニズムです。 このデータは完全な HTTP 専用です。
応答 (ページ出力キャッシュ)。 プロバイダーは、接続に新しい出力
ASP.NET 4 で導入されたキャッシュのプロバイダー機能拡張ポイントです。 To
出力キャッシュ プロバイダーを使用して、まずキャッシュ クラスターを構成およびを ASP.NET アプリケーションに対して、Cache NuGet パッケージを使用して [はじめにマネージ キャッシュ サービスの]」の説明に従って、キャッシュを構成します。 Caching NuGet パッケージをインストールすると、次のコメント アウトされたセクションが web.config に追加されます。これには、ASP.NET アプリケーションが Azure の Caching 用の出力キャッシュ プロバイダーを使用するために必要な構成が含まれています。

    <!--Uncomment this section to use Azure Caching for output caching
    <caching>
      <outputCache defaultProvider="AFCacheOutputCacheProvider">
        <providers>
          <add name="AFCacheOutputCacheProvider" 
            type="Microsoft.Web.DistributedCache.DistributedCacheOutputCacheProvider,
                  Microsoft.Web.DistributedCache" 
            cacheName="default" 
            dataCacheClientName="default" />
        </providers>
      </outputCache>
    </caching>-->

>Web.config には、Cache NuGet パッケージをインストールした後のこのコメント アウトされたセクションが含まれていない場合、最新の NuGet パッケージ マネージャーが [NuGet パッケージ マネージャーのインストール] からインストールされていることを確認し、アンインストールしてパッケージを再インストールします。

Azure のキャッシュ用の出力キャッシュ プロバイダーを有効にするには、指定されたセクションをコメント解除します。 既定のキャッシュは、提供されるスニペットで指定されています。 別のキャッシュを使用するには、目的のキャッシュを指定、 **cacheName** 属性です。

追加、 **OutputCache** 出力をキャッシュする各ページ ディレクティブ。

    <%@ OutputCache Duration="60" VaryByParam="*" %>

この例では、キャッシュされたページ データが 60 秒間キャッシュに保持され、パラメーターの組み合わせごとに異なるバージョンのページがキャッシュされます。 使用可能なオプションの詳細については、[OutputCache ディレクティブ] を参照してください。

詳細については、Azure のキャッシュの出力キャッシュ プロバイダーを使用して、[Azure のキャッシュの出力キャッシュ プロバイダー] を参照してください。

<a name="next-steps"></a>
## 次のステップ

これで、マネージ キャッシュ サービスの基本を学習できました。
次のリンクでは、さらに複雑なキャッシュ タスクを実行する方法について説明します。

-   MSDN リファレンスを参照してください: [Managed Cache Service]
-   Managed Cache Service へ移行する方法について説明します [Managed Cache Service への移行] []。
-   サンプルをチェックする: [キャッシュ サービス サンプルの管理]

<!-- INTRA-TOPIC LINKS -->
[Next Steps]: #next-steps
[What is Azure Managed Cache Service?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Getting Started with Managed Cache Service]: #getting-started-cache-service
[Create the cache]: #create-cache
[Configure the cache]: #enable-caching
[Configure the cache clients]: #NuGet
[Working with Caches]: #working-with-caches
[How To: Create a DataCache Object]: #create-cache-object
[How To: Add and Retrieve an Object from the Cache]: #add-object
[How To: Specify the Expiration of an Object in the Cache]: #specify-expiration
[How To: Store ASP.NET Session State in the Cache]: #store-session
[How To: Store ASP.NET Page Output Caching in the Cache]: #store-page
[Target a Supported .NET Framework Profile]: #prepare-vs-target-net
  
<!-- IMAGES -->
[NewCacheMenu]: ./media/cache-dotnet-how-to-use-service/CacheServiceNewCacheMenu.png

[QuickCreate]: ./media/cache-dotnet-how-to-use-service/CacheServiceQuickCreate.png

[Endpoint]: ./media/cache-dotnet-how-to-use-service/CacheServiceEndpoint.png

[AccessKeys]: ./media/cache-dotnet-how-to-use-service/CacheServiceManageAccessKeys.png

[NuGetPackage]: ./media/cache-dotnet-how-to-use-service/CacheServiceManageNuGetPackage.png

[NuGetPackageMenu]: ./media/cache-dotnet-how-to-use-service/CacheServiceManageNuGetPackagesMenu.png

[NamedCaches]: ./media/cache-dotnet-how-to-use-service/CacheServiceNamedCaches.jpg
  
   
<!-- LINKS -->
[Azure Classic Portal]: https://manage.windowsazure.com/
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
[Overview of Azure Managed Cache Service]: http://go.microsoft.com/fwlink/?LinkId=320830
[Managed Cache Service]: http://go.microsoft.com/fwlink/?LinkId=320830
[OutputCache Directive]: http://go.microsoft.com/fwlink/?LinkId=251979
[Troubleshooting and Diagnostics]: http://go.microsoft.com/fwlink/?LinkId=320839
[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache Pricing Details]: http://www.windowsazure.com/pricing/details/cache/
[Cache offerings]: http://go.microsoft.com/fwlink/?LinkId=317277
[Capacity planning]: http://go.microsoft.com/fwlink/?LinkId=320167
[Expiration and Eviction]: http://go.microsoft.com/fwlink/?LinkId=317278
[High Availability]: http://go.microsoft.com/fwlink/?LinkId=317329
[Notifications]: http://go.microsoft.com/fwlink/?LinkId=317276
[Migrate to Managed Cache Service]: http://go.microsoft.com/fwlink/?LinkId=317347
[Managed Cache Service Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[New-AzureManagedCache]: http://go.microsoft.com/fwlink/?LinkId=400495
[Azure Managed Cache Cmdlets]: http://go.microsoft.com/fwlink/?LinkID=398555
[How to install and configure Azure PowerShell]: http://go.microsoft.com/fwlink/?LinkId=400494
[Add-AzureAccount]: http://msdn.microsoft.com/library/dn495128.aspx
[Select-AzureSubscription]: http://msdn.microsoft.com/library/dn495203.aspx

[Which Azure Cache offering is right for me?]: cache-faq.md#which-azure-cache-offering-is-right-for-me
 
