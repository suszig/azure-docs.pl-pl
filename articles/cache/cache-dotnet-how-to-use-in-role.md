<properties 
    pageTitle="In-Role Cache の使用方法 (.NET) | Microsoft Azure" 
    description="Azure の In-Role Cache の使用方法について説明します。 サンプルは C# コードで記述され、.NET API を利用しています。" 
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
**Azure Cache 用 In-role Cache**します。 サンプルは C\# コードで記述し、
.NET API を使用します。 紹介するシナリオ **キャッシュ クラスターの構成**, 、**キャッシュ クライアントの構成**, 、**の追加と削除
オブジェクトをキャッシュ内の ASP.NET セッション状態を格納するキャッシュから**,、
 **ASP.NET ページ出力キャッシュのキャッシュを使用して有効にすると**です。 サブスクリプションの
インロール キャッシュの使用の詳細についてを参照してください、 [次のステップ][] セクションです。

>[AZURE.IMPORTANT]Azure マネージ キャッシュ サービスと Azure ロール内キャッシュの [2016 年 11 月 30 日、提供終了発表します。 提供終了に備えて、Azure Redis Cache に移行することをお勧めします。 日付と移行のガイドラインの詳細については、次を参照してください。 [どの Azure Cache を利用に最適なでしょうか。](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

<a name="what-is"></a>
## In-Role Cache とは

In-Role Cache は、Azure アプリケーションにキャッシュ レイヤーを提供します。 インメモリで情報を一時的に保存してキャッシュのパフォーマンスが向上します
他のバックエンド ソースとデータベースに関連するコストを削減できます
クラウド内のトランザクション。 次のキャッシュのインロール キャッシュ
機能:

-   セッション状態とページ出力の構築済みの ASP.NET プロバイダー
    キャッシュ、しなくても web アプリケーションの機能を有効にします。
    アプリケーション コードを変更します。
-   たとえば、シリアル化可能なマネージ オブジェクトをキャッシュ: CLR オブジェクト、行、XML、
    バイナリ データです。
-   Azure と Windows の両方で一貫性のある開発モデル
    Server AppFabric します。

In-Role Cache では、Azure クラウド サービス (ホステッド サービスとも呼ばれます) のロール インスタンスをホストしている仮想マシンのメモリの一部を使用してキャッシュを行う方法が提供されます。 デプロイのオプションの点で柔軟性が向上し、キャッシュのサイズは非常に大きくすることができ、キャッシュ固有のクォータの制限がなくなりました。

>[AZURE.IMPORTANT] Azure SDK 2.6 以降、インロール キャッシュでは、Microsoft Azure Storage SDK バージョン 4.3 を使用されています。 以前のバージョンの Azure SDK では、In-Role Cache は Azure Storage SDK 1.7 を使用していました。 Azure SDK 2.6 より前のバージョンで In-Role Cache を使用しているアプリケーションは、2016 年 8 月 1 日に Azure Storage バージョン 2011-08-18 が使用停止になる前に、Azure SDK 2.6 に移行する必要があります。 詳細については、次を参照してください。 [Azure SDK 2.6 リリース ノート - インロール キャッシュ](../azure-sdk-dotnet-release-notes-2_6.md#in-role-cache-updates) と [Microsoft Azure ストレージ サービスのバージョンの削除の更新: 2016 に拡張子](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx)します。

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

このガイドでは、In-Role Cache の基本的な概要について説明します。 この概要ガイドでは扱われていない機能についてより詳細なを参照してください。 [インロール キャッシュの概要に関するページ][]します。

<a name="getting-started-cache-role-instance"></a>
## In-Role Cache の概要

In-Role Cache は、ロール インスタンスをホストしている仮想マシン上のメモリを使用して、キャッシュを利用できるようにします。 キャッシュと呼ばれるホストする、ロール インスタンス、 **キャッシュ クラスター**します。 ロール インスタンスでのキャッシュには、次の 2 つのデプロイ トポロジがあります。

-   **専用ロール** キャッシュ - ロール インスタンスは、キャッシュにのみ使用されます。
-   **併置ロール** キャッシュ - キャッシュは、アプリケーションには VM リソース (帯域幅、CPU、およびメモリ) を共有します。

ロール インスタンスでキャッシュを使用するには、キャッシュ クラスターを構成してから、キャッシュ クラスターにアクセスできるようにキャッシュ クライアントを構成する必要があります。

-   [キャッシュ クラスターの構成][]
-   [キャッシュ クライアントの構成][]

<a name="enable-caching"></a>
## キャッシュ クラスターの構成

構成する、 **併置型ロール** キャッシュ クラスターで、キャッシュ クラスターをホストするロールを選択します。 ロールのプロパティを右クリックして **ソリューション エクスプ ローラー** 選択 **プロパティ**します。

![RoleCache1][RoleCache1]

切り替えて、 **キャッシュ** チェック] タブ、 **キャッシュを有効にする** チェック ボックスをオンし、必要なキャッシュ オプションを指定します。 キャッシュが有効な場合に、 **ワーカー ロール** または **ASP.NET Web ロール**, 、既定の構成は **併置型ロール** キャッシュでキャッシュに割り当てられているロール インスタンスのメモリの 30% です。 既定のキャッシュは自動的に構成され、必要であれば追加の名前付きキャッシュを作成できます。これらのキャッシュは、割り当てられたメモリを共有します。

![RoleCache2][RoleCache2]

構成する、 **専用ロール** キャッシュ クラスターを追加、 **キャッシュ ワーカー ロール** をプロジェクトにします。

![RoleCache7][RoleCache7]

ときに、 **キャッシュ ワーカー ロール** 既定の構成は、プロジェクトに追加 **専用ロール** キャッシュします。

![RoleCache8][RoleCache8]

キャッシュが有効になると、キャッシュ クラスターのストレージ アカウントを構成できるようになります。 In-Role Cache には、Azure のストレージ アカウントが必要です。 このストレージ アカウントは、キャッシュ クラスターを構成するすべての仮想マシンからアクセスされる、キャッシュ クラスターについての構成データを保持するために使用されます。 このストレージ アカウントの指定は、 **キャッシュ** ] タブをクリックして、キャッシュ クラスター ロールのプロパティ ページで、すぐ上、 **名前付きキャッシュ設定**します。

![RoleCache10][RoleCache10]

>このストレージ アカウントが構成されていない場合、ロールは起動できません。 

キャッシュのサイズは、ロールの VM サイズ、ロールのインスタンス数、およびキャッシュ クラスターが専用ロールと併置ロールのどちらとして構成されているかの組み合わせによって決まります。

>このセクションでは、キャッシュ サイズの構成についての簡単な概要を示します。 キャッシュ サイズおよびその他の容量計画に関する考慮事項の詳細については、次を参照してください。 [インロール キャッシュの容量計画に関する考慮事項][]します。

仮想マシンのサイズとロール インスタンスの数を構成するのでロールのプロパティを右クリックし **ソリューション エクスプ ローラー** 選択 **プロパティ**します。

![RoleCache1][RoleCache1]

切り替えて、 **構成** ] タブをクリックします。 既定値 **インスタンス数** 1、および既定値は、 **VM サイズ** は **小さな**します。

![RoleCache3][RoleCache3]

VM サイズの合計メモリは、次のとおりです。 

-   **小さな**: 1.75 GB
-   **メディア**: 3.5 GB
-   **大規模な**: 7 GB
-   **巨大な**: 14 GB


> これらのメモリ サイズは、OS、キャッシュ プロセス、キャッシュ データ、およびアプリケーション間で共有される、VM で利用できるメモリの合計量を表しています。 仮想マシンのサイズを構成する方法については、次を参照してください。 [仮想マシンのサイズを構成する方法][]します。 キャッシュがサポートされていないことに注意してください **ExtraSmall** VM のサイズ。

 **併置型ロール** キャッシュが指定された場合、キャッシュ サイズが仮想マシンのメモリの指定された割合によって決定されます。  **専用ロール** キャッシュが指定された、キャッシュのすべての仮想マシンの使用可能なメモリに使用されます。 2 つのロール インスタンスが構成されている場合は、仮想マシンの合計されたメモリが使用されます。 このようにキャッシュ クラスターが形成され、利用可能なキャッシュ メモリは複数のロール インスタンスに分散されているものの、キャッシュのクライアントには 1 つのリソースとして提示されます。 追加のロール インスタンスを構成すると、同じようにキャッシュ サイズが増加します。 希望するサイズのキャッシュをプロビジョニングするために必要な設定を確認するので示されている容量計画スプレッドシートを使用することができます [インロール キャッシュの容量計画に関する考慮事項][]します。

キャッシュ クラスターの構成が済んだら、キャッシュにアクセスできるようにキャッシュ クライアントを構成できます。

<a name="NuGet"></a>
## キャッシュ クライアントの構成

In-Role Cache のキャッシュにアクセスするには、クライアントが同じデプロイ内になければなりません。 キャッシュ クラスターが専用ロール キャッシュ クラスターである場合、クライアントはデプロイ内の他のロールです。 キャッシュ クラスターが併置型ロール キャッシュ クラスターの場合は、配置では、他のロールまたはロール キャッシュ クラスターをホストする自体のいずれかし、クライアントに可能性があります。 キャッシュにアクセスする各クライアント ロールを構成するために使用できる NuGet パッケージが用意されています。 Caching NuGet パッケージを使用してキャッシュ クラスターにアクセスするロールを構成するのでロール プロジェクトを右クリックし **ソリューション エクスプ ローラー** 選択 **NuGet パッケージの管理**します。 

![RoleCache4][RoleCache4]

選択 **In-role Cache**, 、] をクリックして **インストール**, 、順にクリック **同意**します。

>場合 **インロール キャッシュ** のリストの種類が見つからない **WindowsAzure.Caching** に、 **オンラインで検索** テキスト ボックスし、結果の中から選択します。

![RoleCache5][RoleCache5]

NuGet パッケージは、いくつかの処理を行います。必要な構成をロールの config ファイルに追加したり、キャッシュ クライアントの診断レベル設定を Azure アプリケーションの ServiceConfiguration.cscfg ファイルに追加したり、必要なアセンブリ参照を追加したりします。

>ASP.NET Web ロールでは、Caching NuGet パッケージは 2 つのコメント アウトされたセクションも web.config に追加します。 1 つ目のセクションはセッション状態をキャッシュに格納できるようにし、2 つ目のセクションは ASP.NET ページがキャッシュを出力できるようにします。 詳細については、次を参照してください。 [How To: Store ASP.NET Session State in the Cache] と [How To: ASP.NET ページ出力キャッシュ格納、キャッシュに][]します。

NuGet パッケージは、次の構成要素をロールの web.config または app.config に追加します。 A **dataCacheClients** セクションおよび **cacheDiagnostics** 下のセクションに追加されます、 **configSections** 要素。 ある場合ない **configSections** の子として作成いずれかの要素が存在、 **構成** 要素。

    <configSections>
      <!-- Existing sections omitted for clarity. -->

      <section name="dataCacheClients" 
               type="Microsoft.ApplicationServer.Caching.DataCacheClientsSection, Microsoft.ApplicationServer.Caching.Core" 
               allowLocation="true" 
               allowDefinition="Everywhere" />
    
      <section name="cacheDiagnostics" 
               type="Microsoft.ApplicationServer.Caching.AzureCommon.DiagnosticsConfigurationSection, Microsoft.ApplicationServer.Caching.AzureCommon" 
               allowLocation="true" 
               allowDefinition="Everywhere" />
    </configSections>

これらの新しいセクションを参照している、 **dataCacheClients** 要素および **cacheDiagnostics** 要素。 これらの要素に追加されます、 **構成** 要素。

    <dataCacheClients>
      <dataCacheClient name="default">
        <autoDiscover isEnabled="true" identifier="[cache cluster role name]" />
        <!--<localCache isEnabled="true" sync="TimeoutBased" objectCount="100000" ttlValue="300" />-->
      </dataCacheClient>
    </dataCacheClients>
    <cacheDiagnostics>
      <crashDump dumpLevel="Off" dumpStorageQuotaInMB="100" />
    </cacheDiagnostics>

構成が追加されたら交換 **[キャッシュ クラスター ロール名]** キャッシュ クラスターをホストしているロールの名前に置き換えます。

>場合 **[キャッシュ クラスター ロール名]** キャッシュ クラスターをホストするロールの名前に置き換えられません、 **TargetInvocationException** 内部で、キャッシュにアクセスする場合にスローされる **DatacacheException** 「そのようなロールは存在しません」メッセージを使用します。

NuGet パッケージも追加、 **ClientDiagnosticLevel** 設定、 **ConfigurationSettings** 、キャッシュ クライアント ロールの ServiceConfiguration.cscfg 内です。 次の例は、 **WebRole1** ServiceConfiguration.cscfg ファイルでセクション、 **ClientDiagnosticLevel** の既定値は 1 に **ClientDiagnosticLevel**します。

    <Role name="WebRole1">
      <Instances count="1" />
      <ConfigurationSettings>
        <!-- Existing settings omitted for clarity. -->
        <Setting name="Microsoft.WindowsAzure.Plugins.Caching.ClientDiagnosticLevel" 
                 value="1" />
      </ConfigurationSettings>
    </Role>

>In-Role Cache では、キャッシュ サーバーとキャッシュ クライアントの両方の診断レベルが用意されています。 診断レベルは、キャッシュのために収集される診断情報のレベルを構成する 1 つの設定です。 詳細については、次を参照してください [トラブルシューティングと診断に関する、キャッシュのインロール キャッシュ。][]

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

-   [方法: DataCache オブジェクトを作成する][]
-   [方法: キャッシュでオブジェクトを追加および削除する][]
-   [方法: キャッシュ内のオブジェクトの有効期限を指定する][]
-   [方法: ASP.NET セッション状態をキャッシュに格納する][]
-   [方法: ASP.NET ページ出力キャッシュをキャッシュに格納する][]

<a name="create-cache-object"></a>
## 方法: DataCache オブジェクトを作成する

プログラムでキャッシュを操作するには、キャッシュへの参照が必要です。 使用するファイルの先頭に、次の内容を追加します。
インロール キャッシュ:

    using Microsoft.ApplicationServer.Caching;

>Cache NuGet パッケージをインストールして、必要な参照が追加された後も、Visual Studio が using ステートメント内の型を認識しない場合は、
プロジェクトのターゲット プロファイルが .NET Framework 4 以降であることを確認し、
プロジェクトは .NET Framework 4.0 以上をプロファイルしが指定されていないプロファイルのいずれかを選択するようにして **Client Profile**します。 キャッシュ クライアントの構成方法の詳細については、次を参照してください。 [キャッシュ クライアントの構成][]します。

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

既定では、キャッシュ内の項目はキャッシュに置かれてから 10 分で期限切れになります。 これで構成することができます、 **Time to Live (分)** でキャッシュ クラスターをホストするロールのロールのプロパティを設定します。

![RoleCache6][RoleCache6]

次の 3 つの種類があります **有効期限の種類**: **なし**, 、**絶対**, 、および **スライディング ウィンドウ**です。 これらは、構成方法 **Time to Live (分)** を使用して有効期限を決定します。 既定値 **有効期限の種類** は **絶対**, 、アイテムがキャッシュに保存するときに、項目の有効期限のカウント ダウン タイマーが開始されることを意味します。 指定された時間が経過すると、項目は期限切れになります。 場合 **スライディング ウィンドウ** が指定すると、項目の有効期限のカウント ダウンは、キャッシュに項目がアクセスされるたびにリセットし、項目は、最後のアクセス以降に一定の時間が経過するまで期限切れです。 場合 **None** が指定されると、 **Time to Live (分)** に設定する必要があります **0**, 、およびアイテムの有効期限はありません、キャッシュ内にある限りに、有効なままです。

項目が追加またはを使用してキャッシュに更新するときに、特定の期間を指定できます役割のプロパティで構成されているよりも長いまたは短いタイムアウト間隔が必要な場合、
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

キャッシュのインロール キャッシュのセッション状態プロバイダーは、
ASP.NET アプリケーション用のプロセス外ストレージ メカニズムです。 このプロバイダー
Azure のキャッシュ内ではなく、セッション状態を格納することができます。
メモリ内や SQL Server データベース内。 キャッシュ セッション
状態プロバイダー、まずキャッシュ クラスターを構成および」の説明に従って、Caching NuGet パッケージを使用して、キャッシュの ASP.NET アプリケーションを構成し、 [インロール キャッシュの概要][]します。 Caching NuGet パッケージをインストールすると、コメント アウトされたセクションが web.config に追加されます。これには、ASP.NET アプリケーションが In-Role Cache 用のセッション状態プロバイダーを使用するために必要な構成が含まれています。

    <!--Uncomment this section to use In-Role Cache for session state caching
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

>Web.config に Caching NuGet パッケージのインストール後のこのコメント アウトされたセクションが含まれていない場合から、最新の NuGet パッケージ マネージャーがインストールされていることを確認 [NuGet パッケージ マネージャーのインストール][], をアンインストールして、パッケージを再インストールします。

In-Role Cache 用のセッション状態プロバイダーを有効にするには、指定されたセクションをコメント解除します。 既定のキャッシュは、提供されるスニペットで指定されています。 別のキャッシュを使用するには、目的のキャッシュを指定、 **cacheName** 属性です。

詳細については、Caching サービス セッション状態を使用します。
プロバイダーを参照してください [インロール キャッシュのセッション状態プロバイダー][]します。

<a name="store-page"></a>
## 方法: ASP.NET ページ出力キャッシュをキャッシュに格納する

In-Role Cache の出力キャッシュ プロバイダーは、出力キャッシュ データ用のプロセス外ストレージ メカニズムです。 このデータは完全な HTTP 専用です。
応答 (ページ出力キャッシュ)。 プロバイダーは、接続に新しい出力
ASP.NET 4 で導入されたキャッシュのプロバイダー機能拡張ポイントです。 To
出力キャッシュ プロバイダーを使用して、まずキャッシュ クラスターを構成、および Caching NuGet パッケージを使用して」の説明に従って、キャッシュの ASP.NET アプリケーションを構成し、 [インロール キャッシュの概要][]します。 Caching NuGet パッケージをインストールすると、次のコメント アウトされたセクションが web.config に追加されます。これには、ASP.NET アプリケーションが In-Role Cache 用の出力キャッシュ プロバイダーを使用するために必要な構成が含まれています。

    <!--Uncomment this section to use In-Role Cache for output caching
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

>Web.config に Caching NuGet パッケージのインストール後のこのコメント アウトされたセクションが含まれていない場合から、最新の NuGet パッケージ マネージャーがインストールされていることを確認 [NuGet パッケージ マネージャーのインストール][], をアンインストールして、パッケージを再インストールします。

In-Role Cache 用の出力キャッシュ プロバイダーを有効にするには、指定されたセクションをコメント解除します。 既定のキャッシュは、提供されるスニペットで指定されています。 別のキャッシュを使用するには、目的のキャッシュを指定、 **cacheName** 属性です。

追加、 **OutputCache** 出力をキャッシュする各ページ ディレクティブ。

    <%@ OutputCache Duration="60" VaryByParam="*" %>

この例では、キャッシュされたページ データが 60 秒間キャッシュに保持され、パラメーターの組み合わせごとに異なるバージョンのページがキャッシュされます。 使用可能なオプションの詳細については、次を参照してください。 [OutputCache ディレクティブ][]します。

詳細については、インロール キャッシュの出力キャッシュ プロバイダーを使用して、次を参照してください。 [インロール キャッシュの出力キャッシュ プロバイダー][]します。

<a name="next-steps"></a>
## 次のステップ

ここまでで、インロール キャッシュの基本について説明しました。
次のリンクでは、さらに複雑なキャッシュ タスクを実行する方法について説明します。

-   MSDN リファレンス: [インロール キャッシュ][]
-   In-role Cache に移行する方法について説明します [インロール キャッシュへの移行。][]
-   サンプルの確認: [In-role Cache のサンプル][]
-   ウォッチ、 [最大のパフォーマンス: Azure の Caching のクラウド サービスのアプリケーションを迅速化][] でインロール キャッシュに関する TechEd 2013 のセッション

<!-- INTRA-TOPIC LINKS -->
[Next Steps]: #next-steps
[What is In-Role Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Getting Started with the In-Role Cache Service]: #getting-started-cache-service
[Prepare Your Visual Studio Project to Use In-Role Cache]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Getting Started with In-Role Cache]: #getting-started-cache-role-instance
[Configure the cache cluster]: #enable-caching
[Configure the desired cache size]: #cache-size
[Configure the cache clients]: #NuGet
[Working with Caches]: #working-with-caches
[How To: Create a DataCache Object]: #create-cache-object
[How To: Add and Retrieve an Object from the Cache]: #add-object
[How To: Specify the Expiration of an Object in the Cache]: #specify-expiration
[How To: Store ASP.NET Session State in the Cache]: #store-session
[How To: Store ASP.NET Page Output Caching in the Cache]: #store-page
[Target a Supported .NET Framework Profile]: #prepare-vs-target-net
 
<!-- IMAGES --> 
[RoleCache1]: ./media/cache-dotnet-how-to-use-in-role/cache8.png
[RoleCache2]: ./media/cache-dotnet-how-to-use-in-role/cache9.png
[RoleCache3]: ./media/cache-dotnet-how-to-use-in-role/cache10.png
[RoleCache4]: ./media/cache-dotnet-how-to-use-in-role/cache11.png
[RoleCache5]: ./media/cache-dotnet-how-to-use-in-role/cache12.png
[RoleCache6]: ./media/cache-dotnet-how-to-use-in-role/cache13.png
[RoleCache7]: ./media/cache-dotnet-how-to-use-in-role/cache14.png
[RoleCache8]: ./media/cache-dotnet-how-to-use-in-role/cache15.png
[RoleCache10]: ./media/cache-dotnet-how-to-use-in-role/cache17.png
  
<!-- LINKS -->
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[In-Role Cache Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=252651
[In-Role Cache Samples]: http://msdn.microsoft.com/library/jj189876.aspx
[In-Role Cache]: http://go.microsoft.com/fwlink/?LinkId=252658
[In-Role Cache]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[Maximum Performance: Accelerate Your Cloud Services Applications with Azure Caching]: http://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/WAD-B326#fbid=kmrzkRxQ6gU
[Migrate to In-Role Cache]: http://msdn.microsoft.com/library/hh914163.aspx
[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Output Cache Provider for In-Role Cache]: http://msdn.microsoft.com/library/windowsazure/gg185662.aspx
[OutputCache Directive]: http://go.microsoft.com/fwlink/?LinkId=251979
[Overview of In-Role Cache]: http://go.microsoft.com/fwlink/?LinkId=254172
[Session State Provider for In-Role Cache]: http://msdn.microsoft.com/library/windowsazure/gg185668.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Troubleshooting and Diagnostics for In-Role Cache]: http://msdn.microsoft.com/library/windowsazure/hh914135.aspx
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx

[Which Azure Cache offering is right for me?]: cache-faq.md#which-azure-cache-offering-is-right-for-me
 


