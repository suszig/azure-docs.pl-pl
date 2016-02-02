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
.NET API を使用します。 紹介するシナリオ **の作成および構成キャッシュ**, 、**キャッシュ クライアントの構成**, 、* * の追加と削除
キャッシュ * *、ASP.NET セッション状態の格納、キャッシュ内のオブジェクト
**ASP.NET ページ出力キャッシュのキャッシュを使用して有効にすると**します。 サブスクリプションの
Azure Cache の使用の詳細についてを参照してください、 [次のステップ][] セクションです。
>[AZURE.IMPORTANT]Azure Managed Cache Service と Azure In-Role Cache は、2016 年 11 月 30 日に提供を終了する予定です。 提供終了に備えて、Azure Redis Cache に移行することをお勧めします。 日付と移行のガイドラインの詳細については、を参照してください [どの Azure Cache を利用に最適な?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)。

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

Managed Cache Service を使用すると、マイクロソフトによって管理されている、セキュリティで保護された専用キャッシュにアクセスできます。Managed Cache Service を使用して作成されたキャッシュには、Azure Websites、Web ロールと Worker ロール、および Virtual Machines で実行されている Azure 内のアプリケーションからアクセスできます。

Managed Cache Service は、次の 3 つのレベルで利用できます。

-   Basic - 128 MB ～ 1 GB のキャッシュ
-   Standard - 1 GB ～ 10 GB のキャッシュ
-   Premium - 5 GB ～ 150 GB のキャッシュ

各レベルは、機能と料金ごとに異なります。 このガイドの後半と料金の詳細については、機能はについてを参照してください [キャッシュの料金の詳細][]します。

このガイドでは、Managed Cache Service の基本的な概要について説明します。 この概要ガイドでは扱われていない機能についてより詳細なを参照してください。 [概要の Azure マネージ キャッシュ サービスの []][]します。

<a name="getting-started-cache-service"></a>
## Cache Service の概要

Managed Cache Service は、簡単に使い始めることができます。 使い始めるには、キャッシュをプロビジョニングして構成します。 次に、キャッシュ クライアントを構成してキャッシュにアクセスできるようにします。 キャッシュ クライアントを構成すると、使い始めることができます。

-   [キャッシュを作成します。][]
-   [キャッシュを構成します。][]
-   [キャッシュ クライアントのを構成します。][]

<a name="create-cache"></a>
## キャッシュの作成

Managed Cache Service のキャッシュ インスタンスは、PowerShell コマンドレットを使用して作成されます。

>表示し、で構成されているが、PowerShell コマンドレットを使用して Managed Cache Service インスタンスを作成した後、 [Azure 旧ポータルの][]します。

Managed Cache Service インスタンスを作成するには、Azure PowerShell コマンド ウィンドウを開きます。

>インストールと Azure PowerShell を使用する手順については、次を参照してください。 [をインストールして、Azure PowerShell の [] を構成する方法][]します。

呼び出す、 [Add-azureaccount:operator[]][] コマンドレット、電子メール アドレスと、アカウントに関連付けられているパスワードを入力します。 サブスクリプションが既定で選択し、起動後に表示される、 [Add-azureaccount:operator[]][] コマンドレットです。 サブスクリプションを変更するには、起動、 [Select-azuresubscription:operator[]][] コマンドレットです。

>アカウントの証明書を使用して Azure PowerShell を構成した場合は、このステップを省略できます。 Azure アカウントに Azure PowerShell を接続する方法の詳細については、次を参照してください。 [をインストールして、Azure PowerShell の [] を構成する方法][]します。

サブスクリプションが既定で選択されて表示されます。 サブスクリプションを変更するには、起動、 [Select-azuresubscription:operator[]][] コマンドレットです。

呼び出す、 [New-azuremanagedcache:operator[]][] コマンドレットし、名前、リージョン、キャッシュ オファリング、およびキャッシュのサイズを指定します。

**[名前]** に、キャッシュ エンドポイントに使用するサブドメイン名を入力します。 エンドポイントは、数字と小文字のみを含む、先頭が文字の 6 ～ 12 文字の文字列にしてください。

**[場所]** としてキャッシュのリージョンを指定します。 パフォーマンスを最高にするには、キャッシュ クライアント アプリケーションと同じリージョンにキャッシュを作成します。

**Sku** と**メモリ**の組み合わせにより、キャッシュのサイズが決定されます。 Managed Cache Service は、次の 3 つのレベルで利用できます。

-   Basic - キャッシュ サイズ 128 MB ～ 1 GB (128 MB ずつ増分)、既定で名前付きキャッシュ 1 つ
-   Standard - キャッシュ サイズ 1 GB ～ 10 GB (1 GB ずつ増分)、通知と最大 10 の名前付きキャッシュのサポート
-   Premium - キャッシュ サイズ 5 GB ～ 150 GB (5 GB ずつ増分)、通知、高可用性、および最大 10 の名前付きキャッシュのサポート

アプリケーションのニーズを満たす **Sku** と**メモリ**を選択します。 一部のキャッシュ機能 (通知や高可用性など) は、特定のキャッシュ オファリングでのみ使用できる点に注意してください。 キャッシュ オファリングおよびアプリケーションに最適なサイズを選択する方法については、次を参照してください。 [キャッシュ オファリング []][]します。

 次の例では、128 MB の Basic キャッシュが contosocache という名前で、South Central US リージョンに作成されます。

    New-AzureManagedCache -Name contosocache -Location "South Central US" -Sku Basic -Memory 128MB

>パラメーターとキャッシュを作成するときに使用できる値の完全な一覧については、 [New-azuremanagedcache:operator[]][] コマンドレットのドキュメントです。

PowerShell コマンドレットを呼び出した後、キャッシュが作成されるまで数分かかる場合があります。 キャッシュが作成されると、新しいキャッシュは、 `を実行している` 状態とは、既定の設定で使用できるよう表示、できるで構成されている、 [Azure 旧ポータルの][]します。 キャッシュの構成をカスタマイズするには、次を参照してください。 [キャッシュ [] を構成する][] セクションです。

Azure PowerShell ウィンドウで作成の進捗状況をモニタリングできます。 キャッシュが使用するための準備ができて、 [New-azuremanagedcache:operator[]][] コマンドレットは、次の例で示すように情報をキャッシュに表示されます。

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

Azure クラシック ポータルにあるキャッシュの **[構成]** タブでは、キャッシュのオプションを構成できます。 各キャッシュには**既定の**名前付きキャッシュがあり、標準およびプレミアム キャッシュ オファリングでは最大 9 つの名前付きキャッシュ (合計 10) がサポートされます。 名前付きキャッシュにはそれぞれ独自のオプション セットがあり、柔軟性の高い方法でキャッシュを構成できます。

![NamedCaches][namedcaches]

名前付きキャッシュを作成するには、新しいキャッシュの名前を **[名前]** ボックスに入力して必要なオプションを指定し、**[保存]** をクリックした後、**[はい]** をクリックして確認します。 変更を取り消すには、**[破棄]** をクリックします。

## [有効期限ポリシー] と [時間 (分)]

**[有効期限ポリシー]** と **[時間 (分)]** 設定の組み合わせにより、キャッシュされた項目の有効期限が決まります。 有効期限ポリシーには、**[絶対]**、**[スライド]**、および **[なし]** の 3 つの種類があります。

**[絶対]** が指定されている場合、項目がキャッシュに追加されると、**[時間 (分)]** により指定された有効期間が始まります。 **[時間 (分)]** により指定された期間が経過すると、項目は期限切れになります。

**[スライド]** が指定されている場合、キャッシュ内の項目にアクセスがあるたびに、**[時間 (分)]** により指定された有効期間がリセットされます。 項目への最後のアクセスから、**[時間 (分)]** により指定された期間が経過するまで、項目は期限切れになりません。

**[なし]** が指定されている場合、**[時間 (分)]** は **0** に設定する必要があります。項目は期限切れになりません。

既定の有効期限ポリシーは **[絶対]** で、**[時間 (分)]** の既定の設定は 10 分です。 有効期限ポリシーは、名前付きキャッシュ内の項目ごとに固定されますが、**[時間 (分)]** は、タイムアウト パラメーターを受け取る **Add** オーバーロードと **Put** オーバーロードを使用することで項目ごとにカスタマイズできます。

削除ポリシーと有効期限ポリシーの詳細については、次を参照してください。 [有効期限と削除の []][]します。

## 通知

キャッシュ クラスターでさまざまなキャッシュ操作が発生したときに、アプリケーションが非同期通知を受け取ることができるようにするキャッシュ通知です。 キャッシュ通知は、ローカルにキャッシュされた自動無効化も行います。 詳細については、次を参照してください。 [通知 []][]します。

>通知は、Standard および Premium キャッシュ オファリングでのみ使用できます。Basic キャッシュ オファリングでは使用できません。 詳細については、次を参照してください。 [キャッシュ オファリング []][]します。

## 高可用性

高可用性が有効な場合、バックアップ コピーはキャッシュに追加された各項目で作成されます。 項目のプライマリ コピーで予期しないエラーが発生した場合でも、バックアップ コピーを使用できます。

その名が示すとおり、高可用性を使用すると、各キャッシュ項目に必要なメモリの量が 2 倍に増えます。 容量計画を行うときは、このメモリの影響を考慮に入れてください。 詳細については、次を参照してください。 [高可用性 []][]します。

>高可用性は、Premium キャッシュ オファリングでのみ使用できます。Basic および Standard キャッシュ オファリングでは使用できません。 詳細については、次を参照してください。 [キャッシュ オファリング []][]します。

## 削除

キャッシュ内で使用可能なメモリ容量を維持するため、Least Recently Used (LRU) 削除がサポートされています。 メモリ消費量がメモリしきい値を超えると、メモリ容量に余裕が出るまで、期限切れかどうかに関係なくオブジェクトがメモリから削除されます。
削除は、既定で有効です。 削除が無効な場合、容量に到達しても項目はキャッシュから削除されないため、Put 操作と Add 操作に失敗します。

削除ポリシーと有効期限ポリシーの詳細については、次を参照してください。 [有効期限と削除の []][]します。

キャッシュの構成が済んだら、キャッシュにアクセスできるようにキャッシュ クライアントを構成できます。

<a name="NuGet"></a>
## キャッシュ クライアントの構成

Managed Cache Service を使用して作成されたキャッシュには、Azure Websites、Web ロールと Worker ロール、および Virtual Machines で実行されている Azure アプリケーションからアクセスできます。キャッシュ クライアント アプリケーションの構成を容易にする NuGet パッケージが用意されています。

Cache NuGet パッケージを使用してクライアント アプリケーションを構成するには、**ソリューション エクスプローラー**でプロジェクトを右クリックし、**[NuGet パッケージの管理]** をクリックします。

![NuGetPackageMenu][nugetpackagemenu]

型 **WindowsAzure.Caching** に、 **オンライン検索** テキスト ボックス、および選択 **Windows**  
**Azure** **キャッシュ** 結果からです。 **[インストール]** をクリックし、**[同意する]** をクリックします。

![NuGetPackage][nugetpackage]

NuGet パッケージは、いくつかの処理を行います。アプリケーションの構成ファイルに必要な構成を追加したり、必要なアセンブリ参照を追加したりします。 クラウド サービス プロジェクトの場合、Cloud Services の ServiceConfiguration.cscfg ファイルにキャッシュ クライアント診断レベル設定も追加します。

>ASP.NET Web プロジェクトでは、Cache NuGet パッケージは 2 つのコメント アウトされたセクションも web.config に追加します。 1 つ目のセクションはセッション状態をキャッシュに格納できるようにし、2 つ目のセクションは ASP.NET ページがキャッシュを出力できるようにします。 詳細については、次を参照してください。 [How To: キャッシュ内の ASP.NET セッション状態の格納] と [How To: ASP.NET ページ出力キャッシュ格納 ][]します。

NuGet パッケージは、次の構成要素をロールの web.config または app.config に追加します。 **dataCacheClients** セクションおよび **cacheDiagnostics** セクションは、**configSections** 要素の下に追加されます。 **configSections** 要素が存在しない場合は、**configuration** 要素の子として作成されます。

    <configSections>
      
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

これらの新しいセクションには、**configuration** 要素にも追加された **dataCacheClients** 要素への参照が含まれています。

    <dataCacheClients>
      <dataCacheClient name="default">
        
        
        <autoDiscover isEnabled="true" identifier="[Cache role name or Service Endpoint]" />
        
        
        
      </dataCacheClient>
    </dataCacheClients>

構成が追加されたら、新たに追加された構成で次の 2 つの項目を置き換えます。

1. **[Cache role name or Service Endpoint]** を、Azure クラシック ポータルのダッシュボードに表示されるエンドポイントに置き換えます。

    ![エンドポイント][endpoint]

2. securityProperties セクションをコメント解除し、**[Authentication Key]** を認証キーに置き換えます。認証キーは、キャッシュ ダッシュボードから **[キーの管理]** をクリックすることで Azure クラシック ポータルに表示されます。

    ![AccessKeys][accesskeys]

>これらの設定を適切に構成しないと、クライアントはキャッシュにアクセスできません。

Cloud Services プロジェクトの場合、NuGet パッケージは、キャッシュ クライアント ロールの ServiceConfiguration.cscfg 内の **ConfigurationSettings** に、**ClientDiagnosticLevel** 設定も追加します。 次の例は、ServiceConfiguration.cscfg ファイルの **WebRole1** セクションで、**ClientDiagnosticLevel** は 1 になっています (**ClientDiagnosticLevel** の既定値)。

    <Role name="WebRole1">
      <Instances count="1" />
      <ConfigurationSettings>
        
        <Setting name="Microsoft.WindowsAzure.Plugins.Caching.ClientDiagnosticLevel" 
                 value="1" />
      </ConfigurationSettings>
    </Role>

>クライアント診断レベルは、キャッシュ クライアントのために収集されるキャッシュ診断情報のレベルを構成します。 詳細については、を参照してください [のトラブルシューティングと診断 []][]。

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

-   [方法: DataCache オブジェクトを作成します。][]
-   [方法: 追加し、キャッシュのからオブジェクトを取得][]
-   [方法: ][]
-   [[キャッシュ] で、ASP.NET セッション状態を格納する方法][]
-   [ASP.NET ページ出力キャッシュでキャッシュに格納する方法][]

<a name="create-cache-object"></a>
## 方法: DataCache オブジェクトを作成する

プログラムでキャッシュを操作するには、キャッシュへの参照が必要です。 使用するファイルの先頭に、次の内容を追加します。
Azure Cache:

    using Microsoft.ApplicationServer.Caching;

>Cache NuGet パッケージをインストールして、必要な参照が追加された後も、Visual Studio が using ステートメント内の型を認識しない場合は、
>4 以降が必要な参照を追加するには、Cache NuGet パッケージをインストールしていることを確認ターゲット
>プロジェクトは .NET Framework 4 以降をプロファイルしが指定されていないプロファイルのいずれかを選択するようにして **Client Profile**します。 キャッシュ クライアントの構成方法の詳細については、次を参照してください。 [キャッシュ クライアントの [] を構成する][]します。

**DataCache** オブジェクトを作成する方法は、2 つあります。 1 つ目の方法では、目的のキャッシュの名前を渡して、単純に **DataCache** を作成します。

    DataCache cache = new DataCache("default");

**DataCache** がインスタンス化されたら、以降のセクションで説明されているように、それを使ってキャッシュを操作できます。

2 つ目の方法では、既定のコンストラクターを使用して、アプリケーション内に新しい **DataCacheFactory** オブジェクトを作成します。 このようにすると、キャッシュ クライアントが構成ファイル内の設定を使うようになります。 **DataCache** オブジェクトを返す、新しい **DataCacheFactory** インスタンスの **GetDefaultCache** メソッドを呼び出すか、**GetCache** メソッドを呼び出してキャッシュの名前を渡します。 これらのメソッドは、プログラムでキャッシュにアクセスするために使用できる **DataCache** オブジェクトを返します。

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

特定のキーのオブジェクトを取得するには、**Get** メソッドを使用できます。 オブジェクトが存在する場合
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

既定では、キャッシュ内の項目はキャッシュに置かれてから 10 分で期限切れになります。 これは、Azure クラシック ポータルのキャッシュの [構成] タブにある **[時間 (分)]** 設定で構成できます。

![NamedCaches][namedcaches]

**有効期限ポリシー**には、[**なし**]、[**絶対**]、および [**スライド**] の 3 つの種類があります。 これらは、**[時間 (分)]** を使用して有効期限を決定する方法を構成します。 既定の **[有効期限の種類]** は **[絶対]** であり、キャッシュの項目の有効期限のカウントダウン タイマーは、キャッシュに項目が置かれたときに始まります。 指定された時間が経過すると、項目は期限切れになります。 **[スライド]** が指定されている場合、項目の有効期限のカウントダウンはキャッシュ内の項目がアクセスされるたびにリセットされ、最後のアクセス以降に指定の時間が経過するまでは期限切れになりません。 **[なし]** が指定されている場合、**[時間 (分)]** は **0** に設定されている必要があります。項目は期限切れにならずに、キャッシュ内にある限り有効のままになります。

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
状態プロバイダー、まず、キャッシュを構成および」の説明に従って、Cache NuGet パッケージを使用して、キャッシュの ASP.NET アプリケーションを構成し、 [Managed Cache Service の概要][]します。 Cache NuGet パッケージをインストールすると、コメント アウトされたセクションが web.config に追加されます。これには、ASP.NET アプリケーションが Azure のキャッシュ用のセッション状態プロバイダーを使用するために必要な構成が含まれています。


>Web.config には、Cache NuGet パッケージをインストールした後のこのコメント アウトされたセクションが含まれていない場合から、最新の NuGet パッケージ マネージャーがインストールされていることを確認 [NuGet パッケージ マネージャーのインストール][], をアンインストールして、パッケージを再インストールします。

Azure のキャッシュ用のセッション状態プロバイダーを有効にするには、指定されたセクションをコメント解除します。 既定のキャッシュは、提供されるスニペットで指定されています。 別のキャッシュを使用するには、目的のキャッシュを **cacheName** 属性で指定します。

詳細については、マネージ キャッシュ サービス セッション状態を使用します。
プロバイダーを参照してください [Azure キャッシュのセッション状態プロバイダー][]します。

<a name="store-page"></a>
## 方法: ASP.NET ページ出力キャッシュをキャッシュに格納する

Azure のキャッシュの出力キャッシュ プロバイダーは、出力キャッシュ データ用のプロセス外ストレージ メカニズムです。 このデータは完全な HTTP 専用です。
応答 (ページ出力キャッシュ)。 プロバイダーは、接続に新しい出力
ASP.NET 4 で導入されたキャッシュのプロバイダー機能拡張ポイントです。 To
出力キャッシュ プロバイダーを使用して、まずキャッシュ クラスターを構成し、Cache NuGet パッケージを使用して」の説明に従って、キャッシュの ASP.NET アプリケーションを構成 [Managed Cache Service の概要][]します。 Caching NuGet パッケージをインストールすると、次のコメント アウトされたセクションが web.config に追加されます。これには、ASP.NET アプリケーションが Azure の Caching 用の出力キャッシュ プロバイダーを使用するために必要な構成が含まれています。


>Web.config には、Cache NuGet パッケージをインストールした後のこのコメント アウトされたセクションが含まれていない場合から、最新の NuGet パッケージ マネージャーがインストールされていることを確認 [NuGet パッケージ マネージャーのインストール][], をアンインストールして、パッケージを再インストールします。

Azure のキャッシュ用の出力キャッシュ プロバイダーを有効にするには、指定されたセクションをコメント解除します。 既定のキャッシュは、提供されるスニペットで指定されています。 別のキャッシュを使用するには、目的のキャッシュを **cacheName** 属性で指定します。

出力をキャッシュする各ページに、**OutputCache** ディレクティブを追加します。

    <%@ OutputCache Duration="60" VaryByParam="*" %>

この例では、キャッシュされたページ データが 60 秒間キャッシュに保持され、パラメーターの組み合わせごとに異なるバージョンのページがキャッシュされます。 使用可能なオプションの詳細については、次を参照してください。 [OutputCache ディレクティブ []][]します。

詳細については、Azure のキャッシュの出力キャッシュ プロバイダーを使用して、次を参照してください。 [Azure キャッシュの [] の出力キャッシュ プロバイダー][]します。

<a name="next-steps"></a>
## 次のステップ

これで、マネージ キャッシュ サービスの基本を学習できました。
次のリンクでは、さらに複雑なキャッシュ タスクを実行する方法について説明します。

-   MSDN リファレンス: [マネージ キャッシュ サービスの][]
-   Managed Cache Service へ移行する方法について説明します [Managed Cache Service の [] への移行][]。
-   サンプルの確認: [Managed Cache Service のサンプル][]





[next steps]: #next-steps 
[what is azure managed cache service?]: #what-is 
[create an azure cache]: #create-cache 
[which type of caching is right for me?]: #choosing-cache 
[prepare your visual studio project to use azure caching]: #prepare-vs 
[configure your application to use caching]: #configure-app 
[getting started with managed cache service]: #getting-started-cache-service 
[create the cache]: #create-cache 
[configure the cache]: #enable-caching 
[configure the cache clients]: #NuGet 
[working with caches]: #working-with-caches 
[how to: create a datacache object]: #create-cache-object 
[how to: add and retrieve an object from the cache]: #add-object 
[how to: specify the expiration of an object in the cache]: #specify-expiration 
[how to: store asp.net session state in the cache]: #store-session 
[how to: store asp.net page output caching in the cache]: #store-page 
[target a supported .net framework profile]: #prepare-vs-target-net 
[newcachemenu]: ./media/cache-dotnet-how-to-use-service/CacheServiceNewCacheMenu.png 
[quickcreate]: ./media/cache-dotnet-how-to-use-service/CacheServiceQuickCreate.png 
[endpoint]: ./media/cache-dotnet-how-to-use-service/CacheServiceEndpoint.png 
[accesskeys]: ./media/cache-dotnet-how-to-use-service/CacheServiceManageAccessKeys.png 
[nugetpackage]: ./media/cache-dotnet-how-to-use-service/CacheServiceManageNuGetPackage.png 
[nugetpackagemenu]: ./media/cache-dotnet-how-to-use-service/CacheServiceManageNuGetPackagesMenu.png 
[namedcaches]: ./media/cache-dotnet-how-to-use-service/CacheServiceNamedCaches.jpg 
[azure classic portal]: https://manage.windowsazure.com/ 
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
[overview of azure managed cache service]: http://go.microsoft.com/fwlink/?LinkId=320830 
[managed cache service]: http://go.microsoft.com/fwlink/?LinkId=320830 
[outputcache directive]: http://go.microsoft.com/fwlink/?LinkId=251979 
[troubleshooting and diagnostics]: http://go.microsoft.com/fwlink/?LinkId=320839 
[nuget package manager installation]: http://go.microsoft.com/fwlink/?LinkId=240311 
[cache pricing details]: http://www.windowsazure.com/pricing/details/cache/ 
[cache offerings]: http://go.microsoft.com/fwlink/?LinkId=317277 
[capacity planning]: http://go.microsoft.com/fwlink/?LinkId=320167 
[expiration and eviction]: http://go.microsoft.com/fwlink/?LinkId=317278 
[high availability]: http://go.microsoft.com/fwlink/?LinkId=317329 
[notifications]: http://go.microsoft.com/fwlink/?LinkId=317276 
[migrate to managed cache service]: http://go.microsoft.com/fwlink/?LinkId=317347 
[managed cache service samples]: http://go.microsoft.com/fwlink/?LinkId=320840 
[new-azuremanagedcache]: http://go.microsoft.com/fwlink/?LinkId=400495 
[azure managed cache cmdlets]: http://go.microsoft.com/fwlink/?LinkID=398555 
[how to install and configure azure powershell]: http://go.microsoft.com/fwlink/?LinkId=400494 
[add-azureaccount]: http://msdn.microsoft.com/library/dn495128.aspx 
[select-azuresubscription]: http://msdn.microsoft.com/library/dn495203.aspx 
[which azure cache offering is right for me?]: cache-faq.md#which-azure-cache-offering-is-right-for-me 

