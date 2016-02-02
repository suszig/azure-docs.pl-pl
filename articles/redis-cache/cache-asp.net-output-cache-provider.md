<properties 
   pageTitle="キャッシュの ASP.NET 出力キャッシュ プロバイダー"
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


# Redis Cache Azure 用の ASP.NET 出力キャッシュ プロバイダー

Redis Output Cache Provider では、出力キャッシュ データ用のプロセス外ストレージ メカニズムです。 このデータは、完全な HTTP 応答専用です (ページ出力キャッシュ)。 プロバイダーは、ASP.NET 4 で導入された新しい出力キャッシュ プロバイダー拡張機能ポイントに接続します。

Redis Output Cache Provider を使用するのには、最初にキャッシュを構成し、Redis Output Cache Provider の NuGet パッケージを使用して ASP.NET アプリケーションを構成します。 このトピックでは、Redis 出力キャッシュ プロバイダーを使用するようにアプリケーションを構成する手順について説明します。 作成して、Azure Redis Cache インスタンスの構成に関する詳細については、次を参照してください。 [キャッシュを作成する](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache)します。

## ASP.NET ページ出力をキャッシュに格納します。

Redis Output Cache Provider の NuGet パッケージを使用して Visual Studio でクライアント アプリケーションを構成するで、プロジェクトを右クリックし **ソリューション エクスプ ローラー** 選択 **NuGet パッケージの管理**します。

![Azure Redis Cache Manage NuGet パッケージ](./media/cache-asp.net-output-cache-provider/IC729541.png)

型 **RedisOutputCacheProvider** に、 **オンライン検索** テキスト ボックスに、結果の中から選択し、クリックして **インストール**します。

![Azure Redis Cache Output Cache Provider](./media/cache-asp.net-output-cache-provider/IC751727.jpg)

Redis 出力キャッシュ プロバイダー NuGet パッケージは、StackExchange.Redis.StrongName パッケージに依存します。 StackExchange.Redis.StrongName パッケージは、プロジェクト内に存在しなければインストールされます。 厳密な名前を持つ StackExchange.Redis.StrongName パッケージ以外に、厳密な名前を持たない StackExchange.Redis バージョンもあります。 厳密な名前を持たない StackExchange.Redis バージョンをプロジェクトで使用している場合は、Redis 出力キャッシュ プロバイダー NuGet パッケージのインストールの前か後にこのバージョンをアンインストールする必要があります。アンインストールしなかった場合は、名前の競合がプロジェクトで発生します。 これらのパッケージの詳細については、次を参照してください。 [構成 .NET キャッシュ クライアント](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients)します。

NuGet パッケージは、ダウンロードおよび必要なアセンブリ参照を追加して、ASP.NET アプリケーションは、Redis Output Cache Provider を使用するために必要な構成を含む web.config ファイルに次のセクションを追加します。

    <caching>
      <outputCachedefaultProvider="MyRedisOutputCache">
        <providers>
          
          <add name="MyRedisOutputCache"type="Microsoft.Web.Redis.RedisOutputCacheProvider"host="127.0.0.1"accessKey="" ssl="false"/>
        </providers>
      </outputCache>
    </caching>

コメント化されたセクションには、属性の例と各属性のサンプル設定が記述されています。

Microsoft Azure ポータルでのキャッシュ ブレードから値を持つ属性を構成し、必要に応じてその他の値を設定します。 キャッシュのプロパティにアクセスする方法の詳細については、次を参照してください。 [キャッシュ設定の Redis 構成](cache-configure.md#configure-redis-cache-settings)します。

-   **host**: キャッシュ エンドポイントを指定します。
-   **port**: SSL の設定に応じて、非 SSL ポートまたは SSL ポートを使用します。
-   **accessKey**: キャッシュのプライマリ キーまたはセカンダリ キーを使用します。
-   **ssl**: キャッシュとクライアント間の通信を SSL で保護する場合は true、保護しない場合は false を指定します。 必ず適切なポートを指定してください。
    -   既定では、新しいキャッシュに対して非 SSL ポートは無効になっています。 SSL ポートを使用するには、この設定に true を指定します。 非 SSL ポートを有効にする方法の詳細については、次を参照してください。、 [アクセス ポート](cache-configure.md#access-ports) セクション、 [、キャッシュを構成する](cache-configure.md) トピックです。
-   **databaseId**: キャッシュ出力データに使用するデータベースを指定します。 指定されていない場合は、既定値の 0 が使用されます。
-   **applicationName** – としての redis のキーが格納されている <AppName>_<SessionId>_Data します。これによって、複数のアプリケーションで同じキーを共有できます。このパラメーターは省略可能です。指定されていない場合は、既定値が使用されます。
-   **connectionTimeoutInMilliseconds**: この設定によって、StackExchange.Redis クライアントの connectTimeout 設定を上書きすることができます。 指定されていない場合は、connectTimeout 設定の既定値である 5000 が使用されます。 詳細については、次を参照してください。 [StackExchange.Redis 構成モデル](http://go.microsoft.com/fwlink/?LinkId=398705)します。
-   **operationTimeoutInMilliseconds**: この設定によって、StackExchange.Redis クライアントの syncTimeout 設定を上書きすることができます。 指定されていない場合は、syncTimeout 設定の既定値である 1000 が使用されます。 詳細については、次を参照してください。 [StackExchange.Redis 構成モデル](http://go.microsoft.com/fwlink/?LinkId=398705)します。

出力をキャッシュする各ページに、OutputCache ディレクティブを追加します。

    <%@ OutputCache Duration="60" VaryByParam="*" %>

この例では、キャッシュされたページ データが 60 秒間キャッシュに保持され、パラメーターの組み合わせごとに異なるバージョンのページがキャッシュされます。 OutputCache ディレクティブに関する詳細については、次を参照してください。 [@OutputCache](http://go.microsoft.com/fwlink/?linkid=320837)します。

ここまでの手順を実行すると、アプリケーションは Redis 出力キャッシュ プロバイダーを使用するように構成されます。

## 次のステップ

チェック アウト、 [Azure Redis Cache 用の ASP.NET セッション状態プロバイダー](cache-asp.net-session-state-provider.md)します。






