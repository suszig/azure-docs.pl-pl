<properties 
    pageTitle="Azure App Service での Azure Redis Cache のセッション状態の使用" 
    description="Azure Cache Service を使用して ASP.NET のセッション状態のキャッシュをサポートする方法について説明します。" 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="12/08/2015" 
    ms.author="riande"/>


# Azure App Service での Azure Redis Cache のセッション状態の使用


このトピックでは、Azure Redis Cache Service を使用してセッション状態をサポートする方法について説明します。

ASP.NET Web アプリケーションでセッション状態を使用している場合は、外部セッション状態プロバイダー (Redis Cache Service または SQL Server セッション状態プロバイダー) を構成する必要があります。 セッション状態を使用し、外部プロバイダーを使用しない場合は、Web アプリケーションの 1 つのインスタンスに制限されます。 Redis Cache Service は、最も高速で最も簡単に実現できるキャッシュ サービスです。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

##<a id="createcache"></a>キャッシュを作成する
次の [手順](../cache-dotnet-how-to-use-azure-redis-cache.md#create-cache) キャッシュを作成します。

##<a id="configureproject"></a>RedisSessionStateProvider NuGet パッケージを Web アプリケーションに追加する
NuGet `RedisSessionStateProvider` パッケージをインストールします。  次のコマンドを使用して、パッケージ マネージャー コンソールからインストールする (**ツール** > **NuGet パッケージ マネージャー** > **パッケージ マネージャー コンソール**)。

  `PM> Install-Package Microsoft.Web.RedisSessionStateProvider`
  
インストールする **ツール** > **NuGet パッケージ マネージャー** > **ソリューションの NugGet パッケージの管理**, 、検索 `RedisSessionStateProvider`します。

詳細については、次を参照してください。、 [NuGet RedisSessionStateProvider のページ](http://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider/ ) と [キャッシュ クライアントを構成する](../cache-dotnet-how-to-use-azure-redis-cache.md#NuGet)です。

##<a id="configurewebconfig"></a>Web.Config ファイルを修正する
キャッシュのアセンブリ参照を行うだけでなく、NuGet パッケージにスタブ エントリを追加、 *web.config* ファイルです。 

1. 開いている、 *web.config* を見つけて、 **sessionState** 要素。

1. `host`、`accessKey`、`port` (SSL ポートは 6380 とする必要があります) の値を入力し、`SSL` を `true` に設定します。 これらの値を取得できます、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715) のキャッシュ インスタンスのブレードです。 詳細については、次を参照してください。 [キャッシュに接続する](../cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-cache)です。 既定では、新しいキャッシュに対して非 SSL ポートは無効になっています。 非 SSL ポートを有効にする方法の詳細については、次を参照してください。、 [アクセス ポート](https://msdn.microsoft.com/library/azure/dn793612.aspx#AccessPorts) セクション、 [Azure Redis Cache でキャッシュを構成](https://msdn.microsoft.com/library/azure/dn793612.aspx) トピックです。 次のマークアップで変更内容が表示、 *web.config* ファイル、変更を具体的には *ポート*, 、*ホスト*, 、accessKey * と *ssl*します。

          <system.web>;
            <customErrors mode="Off" />;
            <authentication mode="None" />;
            <compilation debug="true" targetFramework="4.5" />;
            <httpRuntime targetFramework="4.5" />;
            <sessionState mode="Custom" customProvider="RedisSessionProvider">;
              <providers>;  
                  <!--<add name="RedisSessionProvider" 
                    host = "127.0.0.1" [String]
                    port = "" [number]
                    accessKey = "" [String]
                    ssl = "false" [true|false]
                    throwOnError = "true" [true|false]
                    retryTimeoutInMilliseconds = "0" [number]
                    databaseId = "0" [number]
                    applicationName = "" [String]
                  />;-->;
                 <add name="RedisSessionProvider" 
                      type="Microsoft.Web.Redis.RedisSessionStateProvider" 
                      port="6380"
                      host="movie2.redis.cache.windows.net" 
                      accessKey="m7PNV60CrvKpLqMUxosC3dSe6kx9nQ6jP5del8TmADk=" 
                      ssl="true" />;
              <!--<add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider" host="127.0.0.1" accessKey="" ssl="false" />;-->;
              </providers>;
            </sessionState>;
          </system.web>;


##<a id="usesessionobject"></a> コードでセッション オブジェクトを使用する
最後に、ASP.NET コードでセッション オブジェクトを使用します。 使用してセッション状態にオブジェクトを追加、 **Session.Add** メソッドです。 このメソッドではキーと値のペアに基づいて、セッション状態キャッシュにアイテムが格納されます。

    string strValue = "yourvalue";
    Session.Add("yourkey", strValue);

次のコードは、セッション状態からこの値を取得します。

    object objValue = Session["yourkey"];
    if (objValue != null)
       strValue = (string)objValue; 

Redis Cache を使用して、Web アプリケーションのオブジェクトをキャッシュすることもできます。 詳細については、次を参照してください。 [MVC ムービー アプリを 15 分以内に Azure Redis Cache を](http://azure.microsoft.com/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/)します。
ASP.NET セッション状態を使用する方法の詳細については、次を参照してください。 [ASP.NET セッション状態の概要][]します。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

  *によって [Rick Anderson](https://twitter.com/RickAndMSFT)*
  
  [installed the latest]: http://www.windowsazure.com/downloads/?sdk=net  
  [ASP.NET Session State Overview]: http://msdn.microsoft.com/library/ms178581.aspx

  [NewIcon]: ./media/web-sites-dotnet-session-state-caching/CacheScreenshot_NewButton.png
  [NewCacheDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CreateOptions.png
  [CacheIcon]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheIcon.png
  [NuGetDialog]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_NuGet.png
  [OutputConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_OC_WebConfig.png
  [CacheConfig]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_CacheConfig.png
  [EndpointURL]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_EndpointURL.png
  [ManageKeys]: ./media/web-sites-dotnet-session-state-caching/CachingScreenshot_ManageAccessKeys.png
 

