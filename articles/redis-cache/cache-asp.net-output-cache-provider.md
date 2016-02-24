<properties 
   pageTitle="Cache ASP.NET 出力キャッシュ プロバイダー"
   description="Azure Redis Cache を使用して ASP.NET ページ出力をキャッシュする方法について説明します。"
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
   ms.date="10/23/2015"
   ms.author="sdanie" />

# Azure Redis Cache の ASP.NET 出力キャッシュ プロバイダー

Redis 出力キャッシュ プロバイダーは、出力キャッシュ データ用のプロセス外ストレージ メカニズムです。 このデータは、完全な HTTP 応答専用です (ページ出力キャッシュ)。 プロバイダーは、ASP.NET 4 で導入された新しい出力キャッシュ プロバイダー拡張機能ポイントに接続します。

Redis 出力キャッシュ プロバイダーを使用するには、まず対象のキャッシュを構成し、Redis 出力キャッシュ プロバイダー NuGet パッケージを使用して ASP.NET アプリケーションを構成します。 このトピックでは、Redis 出力キャッシュ プロバイダーを使用するようにアプリケーションを構成する手順について説明します。 作成して、Azure Redis Cache インスタンスの構成に関する詳細については、次を参照してください。 [キャッシュを作成する](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache)です。

## ASP.NET ページ出力をキャッシュに格納する

Redis Output Cache Provider の NuGet パッケージを使用して Visual Studio でクライアント アプリケーションを構成するで、プロジェクトを右クリックし **ソリューション エクスプ ローラー** 選択 **NuGet パッケージの管理**します。

![Azure Redis Cache 管理 NuGet パッケージ](./media/cache-asp.net-output-cache-provider/IC729541.png)

型 **RedisOutputCacheProvider** に、 **オンライン検索** テキスト ボックスに、結果の中から選択し、クリックして **インストール**します。

![Azure Redis Cache 出力キャッシュプロバイダー](./media/cache-asp.net-output-cache-provider/IC751727.jpg)

Redis 出力キャッシュ プロバイダー NuGet パッケージは、StackExchange.Redis.StrongName パッケージに依存します。 StackExchange.Redis.StrongName パッケージは、プロジェクト内に存在しなければインストールされます。 厳密な名前を持つ StackExchange.Redis.StrongName パッケージ以外に、厳密な名前を持たない StackExchange.Redis バージョンもあります。 厳密な名前を持たない StackExchange.Redis バージョンをプロジェクトで使用している場合は、Redis 出力キャッシュ プロバイダー NuGet パッケージのインストールの前か後にこのバージョンをアンインストールする必要があります。アンインストールしなかった場合は、名前の競合がプロジェクトで発生します。 これらのパッケージの詳細については、次を参照してください。 [構成 .NET キャッシュ クライアント](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients)です。

NuGet パッケージによって、必要なアセンブリ参照がダウンロードされて追加されます。さらに、web.config ファイルには、ASP.NET アプリケーションが Redis 出力キャッシュ プロバイダーを使用するために必要な構成を記述した次のセクションが追加されます。

    <caching>
      <outputCachedefaultProvider="MyRedisOutputCache">
        <providers>
          <!--
          <add name="MyRedisOutputCache" 
            host = "127.0.0.1" [String]
            port = "" [number]
            accessKey = "" [String]
            ssl = "false" [true|false]
            databaseId = "0" [number]
            applicationName = "" [String]
            connectionTimeoutInMilliseconds = "5000" [number]
            operationTimeoutInMilliseconds = "5000" [number]
          />
          -->
          <add name="MyRedisOutputCache"type="Microsoft.Web.Redis.RedisOutputCacheProvider"host="127.0.0.1"accessKey="" ssl="false"/>
        </providers>
      </outputCache>
    </caching>

コメント化されたセクションには、属性の例と各属性のサンプル設定が記述されています。

属性の構成には、Microsoft Azure ポータルのキャッシュ ブレードの値を使用してください。その他の値は適宜構成します。 キャッシュのプロパティにアクセスする方法の詳細については、次を参照してください。 [キャッシュ設定の Redis 構成](cache-configure.md#configure-redis-cache-settings)します。

-   **ホスト** – キャッシュ エンドポイントを指定します。
-   **ポート** – 非 SSL ポートまたは ssl の設定によっては、SSL ポートを使用します。
-   **accessKey** –、キャッシュのプライマリまたはセカンダリのいずれかのキーを使用します。
-   **ssl** – キャッシュ/クライアント間の通信をセキュリティで保護する場合は true。 それ以外の場合は false。 必ず適切なポートを指定してください。
    -   既定では、新しいキャッシュに対して非 SSL ポートは無効になっています。 SSL ポートを使用するには、この設定に true を指定します。 非 SSL ポートを有効にする方法の詳細については、次を参照してください。、 [アクセス ポート](cache-configure.md#access-ports) セクション、 [、キャッシュを構成する](cache-configure.md) トピックです。
-   **databaseId** – 指定されたキャッシュに使用するデータベースがデータを出力します。 指定されていない場合は、既定値の 0 が使用されます。
-   **applicationName** – としての redis のキーが格納されている <AppName>_<SessionId>_Data します。 これによって、複数のアプリケーションで同じキーを共有できます。 このパラメーターは省略可能です。指定されていない場合は、既定値が使用されます。
-   **connectionTimeoutInMilliseconds** – この設定では、StackExchange.Redis クライアントの connectTimeout 設定を上書きすることができます。 指定されていない場合は、connectTimeout 設定の既定値である 5000 が使用されます。 詳細については、次を参照してください。 [StackExchange.Redis 構成モデル](http://go.microsoft.com/fwlink/?LinkId=398705)します。
-   **operationTimeoutInMilliseconds** – この設定では、StackExchange.Redis クライアントに synctimeout を加算設定は上書きすることができます。 指定されていない場合は、syncTimeout 設定の既定値である 1000 が使用されます。 詳細については、次を参照してください。 [StackExchange.Redis 構成モデル](http://go.microsoft.com/fwlink/?LinkId=398705)します。

出力をキャッシュする各ページに、OutputCache ディレクティブを追加します。

    <%@ OutputCache Duration="60" VaryByParam="*" %>

この例では、キャッシュされたページ データが 60 秒間キャッシュに保持され、パラメーターの組み合わせごとに異なるバージョンのページがキャッシュされます。 OutputCache ディレクティブに関する詳細については、次を参照してください。 [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837)します。

ここまでの手順を実行すると、アプリケーションは Redis 出力キャッシュ プロバイダーを使用するように構成されます。

## 次のステップ

チェック アウト、 [Azure Redis Cache 用の ASP.NET セッション状態プロバイダー](cache-asp.net-session-state-provider.md)します。


