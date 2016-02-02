<properties 
    pageTitle="In-Role Cache の使用方法 (.NET) | Microsoft Azure" 
    description="Azure の In-Role Cache の使用方法について説明します。サンプルは C# コードで記述され、.NET API を利用しています。" 
    services="cache" 
    documentationCenter=".net" 
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







# Azure のキャッシュの In-Role Cache の使用方法

このガイドでは、**Microsoft Azure のキャッシュのインロール キャッシュ** 
サンプルは C\# コードで記述し、
.NET API を使用します。

サブスクリプションの

>[AZURE.IMPORTANT]Azure Managed Cache Service と Azure In-Role Cache は、2016 年 11 月 30 日に提供を終了する予定です。 提供終了に備えて、Azure Redis Cache に移行することをお勧めします。

<a name="what-is"></a>
## In-Role Cache とは

In-Role Cache は、Azure アプリケーションにキャッシュ レイヤーを提供します。




-   


-   

-   


In-Role Cache では、Azure クラウド サービス (ホステッド サービスとも呼ばれます) のロール インスタンスをホストしている仮想マシンのメモリの一部を使用してキャッシュを行う方法が提供されます。 デプロイのオプションの点で柔軟性が向上し、キャッシュのサイズは非常に大きくすることができ、キャッシュ固有のクォータの制限がなくなりました。
>[AZURE.IMPORTANT] Azure SDK 2.6 以降、In-Role Cache は Microsoft Azure Storage SDK バージョン 4.3 を使用しています。 以前のバージョンの Azure SDK では、In-Role Cache は Azure Storage SDK 1.7 を使用していました。 Azure SDK 2.6 より前のバージョンで In-Role Cache を使用しているアプリケーションは、2016 年 8 月 1 日に Azure Storage バージョン 2011-08-18 が使用停止になる前に、Azure SDK 2.6 に移行する必要があります。

ロール インスタンスでのキャッシュには、次のような利点があります。

-   キャッシュのための追加料金が必要ない。 キャッシュをホストするコンピューティング リソースの分だけをお支払いいただきます。
-   キャッシュのクォータおよび調整が不要になる。
-   より高度な制御と分離を行うことができる。
-   パフォーマンスが向上する。
-   ロールがスケールインまたはスケールアウトするときに、自動的にキャッシュのサイズが調整される。 ロール インスタンスが追加または削除されるときに、キャッシュのために利用できるメモリの規模が効率的に拡大または縮小されます。
-   開発時デバッグを完全な忠実度で行うことができる。
-   memcache プロトコルがサポートされる。

また、ロール インスタンスでのキャッシュには、次のような構成オプションがあります。

-   キャッシュ専用のロールを構成するか、キャッシュを既存のロールに配置する。
-   同じクラウド サービス デプロイ内の複数のクライアントがキャッシュを利用できるようにする。
-   異なるプロパティを持つ複数の名前付きキャッシュを作成する。
-   必要に応じて個々のキャッシュに高可用性を構成する。
-   リージョン、タグ付け、通知などの拡張キャッシュ機能を使用する。

このガイドでは、In-Role Cache の基本的な概要について説明します。

<a name="getting-started-cache-role-instance"></a>
## In-Role Cache の概要

In-Role Cache は、ロール インスタンスをホストしている仮想マシン上のメモリを使用して、キャッシュを利用できるようにします。 キャッシュをホストするロール インスタンスは、**キャッシュ クラスター**と呼ばれます。 ロール インスタンスでのキャッシュには、次の 2 つのデプロイ トポロジがあります。

-   **専用ロール** キャッシュ - ロール インスタンスがキャッシュだけのために使用されます。
-   **併置ロール** キャッシュ - キャッシュは VM リソース (帯域幅、CPU、およびメモリ) をアプリケーションと共有します。

ロール インスタンスでキャッシュを使用するには、キャッシュ クラスターを構成してから、キャッシュ クラスターにアクセスできるようにキャッシュ クライアントを構成する必要があります。

-   
-   

<a name="enable-caching"></a>
## キャッシュ クラスターの構成

**併置ロール** キャッシュ クラスターを構成するには、キャッシュ クラスターをホストするロールを選択します。 **ソリューション エクスプローラー**でロール プロパティを右クリックし、**[プロパティ]** をクリックします。

![RoleCache1][rolecache1]

**[キャッシュ]** タブに切り替えて、**[キャッシュを有効にする]** チェック ボックスをオンにし、必要なキャッシュ オプションを指定します。 **ワーカー ロール**または **ASP.NET Web ロール**でキャッシュが有効になっている場合、既定の構成は**併置ロール** キャッシュで、ロール インスタンスのメモリの 30% がキャッシュ用に割り当てられています。 既定のキャッシュは自動的に構成され、必要であれば追加の名前付きキャッシュを作成できます。これらのキャッシュは、割り当てられたメモリを共有します。

![RoleCache2][rolecache2]

**専用ロール** キャッシュ クラスターを構成するには、プロジェクトに**キャッシュ ワーカー ロール**を追加します。

![RoleCache7][rolecache7]

**キャッシュ ワーカー ロール**がプロジェクトに追加される場合、既定の構成は**専用ロール キャッシュ**です。

![RoleCache8][rolecache8]

キャッシュが有効になると、キャッシュ クラスターのストレージ アカウントを構成できるようになります。 In-Role Cache には、Azure のストレージ アカウントが必要です。 このストレージ アカウントは、キャッシュ クラスターを構成するすべての仮想マシンからアクセスされる、キャッシュ クラスターについての構成データを保持するために使用されます。 このストレージ アカウントの指定は、キャッシュ クラスター ロールのプロパティ ページの **[キャッシュ]** タブで、**[名前付きキャッシュの設定]** のすぐ上で行います。

![RoleCache10][rolecache10]

>このストレージ アカウントが構成されていない場合、ロールは起動できません。

キャッシュのサイズは、ロールの VM サイズ、ロールのインスタンス数、およびキャッシュ クラスターが専用ロールと併置ロールのどちらとして構成されているかの組み合わせによって決まります。

>このセクションでは、キャッシュ サイズの構成についての簡単な概要を示します。

仮想マシン サイズとロール インスタンス数を構成するには、**ソリューション エクスプローラー**でロール プロパティを右クリックし、**[プロパティ]** をクリックします。

![RoleCache1][rolecache1]

**[構成]** タブに切り替えます。 既定の**インスタンス数**は 1 で、既定の **VM サイズ**は **S** です。

![RoleCache3][rolecache3]

VM サイズの合計メモリは、次のとおりです。

-   **S**: 1.75 GB
-   **M**: 3.5 GB
-   **L**: 7 GB
-   **XL**: 14 GB


> これらのメモリ サイズは、OS、キャッシュ プロセス、キャッシュ データ、およびアプリケーション間で共有される、VM で利用できるメモリの合計量を表しています。  キャッシュは **XS** VM サイズではサポートされていないことに注意してください。

**併置ロール** キャッシュが指定された場合、キャッシュ サイズは仮想マシン メモリの指定された割合によって決定されます。 **専用ロール** キャッシュが指定された場合、仮想マシンの利用可能なすべてのメモリがキャッシュに使用されます。 2 つのロール インスタンスが構成されている場合は、仮想マシンの合計されたメモリが使用されます。 このようにキャッシュ クラスターが形成され、利用可能なキャッシュ メモリは複数のロール インスタンスに分散されているものの、キャッシュのクライアントには 1 つのリソースとして提示されます。 追加のロール インスタンスを構成すると、同じようにキャッシュ サイズが増加します。

キャッシュ クラスターの構成が済んだら、キャッシュにアクセスできるようにキャッシュ クライアントを構成できます。

<a name="NuGet"></a>
## キャッシュ クライアントの構成

In-Role Cache のキャッシュにアクセスするには、クライアントが同じデプロイ内になければなりません。 キャッシュ クラスターが専用ロール キャッシュ クラスターである場合、クライアントはデプロイ内の他のロールです。  キャッシュにアクセスする各クライアント ロールを構成するために使用できる NuGet パッケージが用意されています。 Caching NuGet パッケージを使用して、キャッシュ クラスターにアクセスするようにロールを構成するには、**ソリューション エクスプローラー**でロール プロジェクトを右クリックし、**[NuGet パッケージの管理]** をクリックします。

![RoleCache4][rolecache4]

**[インロール キャッシュ]** を選択し、**[インストール]**、**[同意する]** の順にクリックします。

>一覧に **[インロール キャッシュ]** が表示されない場合は、**[オンライン検索]** ボックスに「**WindowsAzure.Caching**」と入力し、結果の中から選択します。

![RoleCache5][rolecache5]

NuGet パッケージは、いくつかの処理を行います。必要な構成をロールの config ファイルに追加したり、キャッシュ クライアントの診断レベル設定を Azure アプリケーションの ServiceConfiguration.cscfg ファイルに追加したり、必要なアセンブリ参照を追加したりします。

>ASP.NET Web ロールでは、Caching NuGet パッケージは 2 つのコメント アウトされたセクションも web.config に追加します。 1 つ目のセクションはセッション状態をキャッシュに格納できるようにし、2 つ目のセクションは ASP.NET ページがキャッシュを出力できるようにします。

NuGet パッケージは、次の構成要素をロールの web.config または app.config に追加します。 **dataCacheClients** セクションおよび **cacheDiagnostics** セクションは、**configSections** 要素の下に追加されます。 **configSections** 要素が存在しない場合は、**configuration** 要素の子として作成されます。

    <configSections>
      
    
      <section name="dataCacheClients" 
               type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" 
               allowLocation="true" 
               allowDefinition="Everywhere" />
    
      <section name="cacheDiagnostics" 
               type="Microsoft.ApplicationServer.Caching.AzureCommon.DiagnosticsConfigurationSection, Microsoft.ApplicationServer.Caching.AzureCommon" 
               allowLocation="true" 
               allowDefinition="Everywhere" />
    </configSections>

これらの新しいセクションには、**dataCacheClients** 要素および **cacheDiagnostics** 要素への参照が含まれています。 これらの要素も **configuration** 要素に追加されます。

    <dataCacheClients>
      <dataCacheClient name="default">
        <autoDiscover isEnabled="true" identifier="[cache cluster role name]" />
        
      </dataCacheClient>
    </dataCacheClients>
    <cacheDiagnostics>
      <crashDump dumpLevel="Off" dumpStorageQuotaInMB="100" />
    </cacheDiagnostics>

構成が追加された後で、**[cache cluster role name]** を、キャッシュ クラスターをホストするロールの名前に置き換えます。

>**[cache cluster role name]** を、キャッシュ クラスターをホストするロールの名前に置き換えないと、キャッシュがアクセスされたときに **TargetInvocationException** がスローされます。その内部に、"そのようなロールは存在しません" というメッセージを持つ **DatacacheException** が含まれています。

NuGet パッケージは、キャッシュ クライアント ロールの ServiceConfiguration.cscfg 内の **ConfigurationSettings** に、**ClientDiagnosticLevel** 設定も追加します。 次の例は、ServiceConfiguration.cscfg ファイルの **WebRole1** セクションで、**ClientDiagnosticLevel** は 1 になっています (**ClientDiagnosticLevel** の既定値)。

    <Role name="WebRole1">
      <Instances count="1" />
      <ConfigurationSettings>
        
        <Setting name="Microsoft.WindowsAzure.Plugins.Caching.ClientDiagnosticLevel" 
                 value="1" />
      </ConfigurationSettings>
    </Role>

>In-Role Cache では、キャッシュ サーバーとキャッシュ クライアントの両方の診断レベルが用意されています。 診断レベルは、キャッシュのために収集される診断情報のレベルを構成する 1 つの設定です。

NuGet パッケージは、次のアセンブリへの参照も追加します。

-   Microsoft.ApplicationServer.Caching.Client.dll
-   Microsoft.ApplicationServer.Caching.Core.dll
-   Microsoft.WindowsFabric.Common.dll
-   Microsoft.WindowsFabric.Data.Common.dll
-   Microsoft.ApplicationServer.Caching.AzureCommon.dll
-   Microsoft.ApplicationServer.Caching.AzureClientHelper.dll

ロールが ASP.NET Web ロールである場合は、次のアセンブリ参照も追加されます。

-   Microsoft.Web.DistributedCache.dll

クライアント プロジェクトをキャッシュ用に構成できたら、以降のセクションで説明されている、キャッシュを操作するための技法を使用できます。

<a name="working-with-caches"></a>
## キャッシュの操作

このセクションの手順では、キャッシュに対する一般的なタスクを行う方法について説明します。

-   
-   
-   
-   
-   

<a name="create-cache-object"></a>
## 方法: DataCache オブジェクトを作成する

プログラムでキャッシュを操作するには、キャッシュへの参照が必要です。 使用するファイルの先頭に、次の内容を追加します。
インロール キャッシュ:

    using Microsoft.ApplicationServer.Caching;

>Cache NuGet パッケージをインストールして、必要な参照が追加された後も、Visual Studio が using ステートメント内の型を認識しない場合は、
>プロジェクトのターゲット プロファイルが .NET Framework 4 以降であることを確認し、
>

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





    // Add the string "value" to the cache, keyed by "item"
    cache.Add("item", "value");




> 
> 作成しようとしています。 キャッシュは、
> オブジェクトに対して一意のキーの値のみを受け入れます。

特定のキーのオブジェクトを取得するには、**Get** メソッドを使用できます。


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




    // Add the string "value" to the cache, keyed by "item". If it exists,
    // replace it.
    cache.Put("item", "value");

<a name="specify-expiration"></a>
## 方法: キャッシュ内のオブジェクトの有効期限を指定する

既定では、キャッシュ内の項目はキャッシュに置かれてから 10 分で期限切れになります。 これは、キャッシュ クラスターをホストしているロールのロール プロパティの **[Time to Live (分)]** 設定で構成できます。

![RoleCache6][rolecache6]

[**有効期限の種類**] には、次の 3 つがあります。[**なし**]、[**絶対**]、および [**スライディング ウィンドウ**] です。 これらは、**[Time to Live (分)]** を使用して有効期限を決定する方法を構成します。 既定の **[有効期限の種類]** は **[絶対]** であり、キャッシュの項目の有効期限のカウントダウン タイマーは、キャッシュに項目が置かれたときに始まります。 指定された時間が経過すると、項目は期限切れになります。 **[スライディング ウィンドウ]** が指定されている場合、項目の有効期限のカウントダウンはキャッシュ内の項目がアクセスされるたびにリセットされ、最後のアクセス以降に指定の時間が経過するまでは期限切れになりません。 **[なし]** が指定されている場合、**[Time to Live (分)]** は **0** に設定されている必要があります。項目は期限切れにならずに、キャッシュ内にある限り有効のままになります。


イン



    // Add the string "value" to the cache, keyed by "item"
    cache.Add("item", "value", TimeSpan.FromMinutes(30));






    // Get a DataCacheItem object that contains information about
    // "item" in the cache. If there is no object keyed by "item" null
    // is returned. 
    DataCacheItem item = cache.GetCacheItem("item");
    TimeSpan timeRemaining = item.Timeout;

<a name="store-session"></a>
## 方法: ASP.NET セッション状態をキャッシュに格納する




キャッシュ セッション
状態プロバイダー、まずキャッシュ クラスターを構成および」の説明に従って、Caching NuGet パッケージを使用して、キャッシュの ASP.NET アプリケーションを構成し、 [インロール キャッシュの概要][]します。 Caching NuGet パッケージをインストールすると、コメント アウトされたセクションが web.config に追加されます。これには、ASP.NET アプリケーションが In-Role Cache 用のセッション状態プロバイダーを使用するために必要な構成が含まれています。


>Web.config に Caching NuGet パッケージのインストール後のこのコメント アウトされたセクションが含まれていない場合から、最新の NuGet パッケージ マネージャーがインストールされていることを確認 [NuGet パッケージ マネージャーのインストール][], をアンインストールして、パッケージを再インストールします。

In-Role Cache 用のセッション状態プロバイダーを有効にするには、指定されたセクションをコメント解除します。 既定のキャッシュは、提供されるスニペットで指定されています。 別のキャッシュを使用するには、目的のキャッシュを **cacheName** 属性で指定します。


プロバイダーを参照してください [インロール キャッシュのセッション状態プロバイダー][]します。

<a name="store-page"></a>
## 方法: ASP.NET ページ出力キャッシュをキャッシュに格納する

In-Role Cache の出力キャッシュ プロバイダーは、出力キャッシュ データ用のプロセス外ストレージ メカニズムです。
プロバイダーは、接続に新しい出力
ASP.NET 4 で導入されたキャッシュのプロバイダー機能拡張ポイントです。 To
出力キャッシュ プロバイダーを使用して、まずキャッシュ クラスターを構成、および Caching NuGet パッケージを使用して」の説明に従って、キャッシュの ASP.NET アプリケーションを構成し、 [インロール キャッシュの概要][]します。 Caching NuGet パッケージをインストールすると、次のコメント アウトされたセクションが web.config に追加されます。これには、ASP.NET アプリケーションが In-Role Cache 用の出力キャッシュ プロバイダーを使用するために必要な構成が含まれています。


>Web.config に Caching NuGet パッケージのインストール後のこのコメント アウトされたセクションが含まれていない場合から、最新の NuGet パッケージ マネージャーがインストールされていることを確認 [NuGet パッケージ マネージャーのインストール][], をアンインストールして、パッケージを再インストールします。

In-Role Cache 用の出力キャッシュ プロバイダーを有効にするには、指定されたセクションをコメント解除します。 既定のキャッシュは、提供されるスニペットで指定されています。 別のキャッシュを使用するには、目的のキャッシュを **cacheName** 属性で指定します。

出力をキャッシュする各ページに、**OutputCache** ディレクティブを追加します。

    <%@ OutputCache Duration="60" VaryByParam="*" %>

この例では、キャッシュされたページ データが 60 秒間キャッシュに保持され、パラメーターの組み合わせごとに異なるバージョンのページがキャッシュされます。 使用可能なオプションの詳細については、次を参照してください。 [OutputCache ディレクティブ []][]します。

詳細については、インロール キャッシュの出力キャッシュ プロバイダーを使用して、次を参照してください。 [インロール キャッシュの [] の出力キャッシュ プロバイダー][]します。

<a name="next-steps"></a>
## 次のステップ

ここまでで、インロール キャッシュの基本について説明しました。
次のリンクでは、さらに複雑なキャッシュ タスクを実行する方法について説明します。

-   MSDN リファレンス: [インロール キャッシュの][]
-   In-role Cache に移行する方法について説明します [インロール キャッシュの [] への移行][]。
-   サンプルの確認: [In-role Cache のサンプル][]
-   ウォッチ、 [最大のパフォーマンス: Azure Caching で、クラウド サービスのアプリケーションを迅速化][] でインロール キャッシュに関する TechEd 2013 のセッション



[next steps]: #next-steps 
[what is in-role cache?]: #what-is 
[create an azure cache]: #create-cache 
[which type of caching is right for me?]: #choosing-cache 
[getting started with the in-role cache service]: #getting-started-cache-service 
[prepare your visual studio project to use in-role cache]: #prepare-vs 
[configure your application to use caching]: #configure-app 
[getting started with in-role cache]: #getting-started-cache-role-instance 
[configure the cache cluster]: #enable-caching 
[configure the desired cache size]: #cache-size 
[configure the cache clients]: #NuGet 
[working with caches]: #working-with-caches 
[how to: create a datacache object]: #create-cache-object 
[how to: add and retrieve an object from the cache]: #add-object 
[how to: specify the expiration of an object in the cache]: #specify-expiration 
[how to: store asp.net session state in the cache]: #store-session 
[how to: store asp.net page output caching in the cache]: #store-page 
[target a supported .net framework profile]: #prepare-vs-target-net 
[rolecache1]: ./media/cache-dotnet-how-to-use-in-role/cache8.png 
[rolecache2]: ./media/cache-dotnet-how-to-use-in-role/cache9.png 
[rolecache3]: ./media/cache-dotnet-how-to-use-in-role/cache10.png 
[rolecache4]: ./media/cache-dotnet-how-to-use-in-role/cache11.png 
[rolecache5]: ./media/cache-dotnet-how-to-use-in-role/cache12.png 
[rolecache6]: ./media/cache-dotnet-how-to-use-in-role/cache13.png 
[rolecache7]: ./media/cache-dotnet-how-to-use-in-role/cache14.png 
[rolecache8]: ./media/cache-dotnet-how-to-use-in-role/cache15.png 
[rolecache10]: ./media/cache-dotnet-how-to-use-in-role/cache17.png 
[how to configure virtual machine sizes]: http://go.microsoft.com/fwlink/?LinkId=164387 
[how to: configure a cache client programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx 
[how to: set a page's cacheability programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx 
[how to: set the cacheability of an asp.net page declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx 
[in-role cache capacity planning considerations]: http://go.microsoft.com/fwlink/?LinkId=252651 
[in-role cache samples]: http://msdn.microsoft.com/library/jj189876.aspx 
[in-role cache]: http://go.microsoft.com/fwlink/?LinkId=252658 
[in-role cache]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching 
[maximum performance: accelerate your cloud services applications with azure caching]: http://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/WAD-B326#fbid=kmrzkRxQ6gU 
[migrate to in-role cache]: http://msdn.microsoft.com/library/hh914163.aspx 
[nuget package manager installation]: http://go.microsoft.com/fwlink/?LinkId=240311 
[output cache provider for in-role cache]: http://msdn.microsoft.com/library/windowsazure/gg185662.aspx 
[outputcache directive]: http://go.microsoft.com/fwlink/?LinkId=251979 
[overview of in-role cache]: http://go.microsoft.com/fwlink/?LinkId=254172 
[session state provider for in-role cache]: http://msdn.microsoft.com/library/windowsazure/gg185668.aspx 
[team blog]: http://blogs.msdn.com/b/windowsazure/ 
[troubleshooting and diagnostics for in-role cache]: http://msdn.microsoft.com/library/windowsazure/hh914135.aspx 
[azure appfabric cache: caching session state]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20 
[azure shared caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx 
[which azure cache offering is right for me?]: cache-faq.md#which-azure-cache-offering-is-right-for-me 

