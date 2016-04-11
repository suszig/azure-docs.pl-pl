<properties
    pageTitle="Mobile Apps 用 .NET バックエンド サーバー SDK を操作する方法 | Azure App Service"
    description="Azure App Service Mobile Apps 用の .NET バックエンド サーバー SDK を操作する方法について説明します。"
    keywords="app service, azure app service, mobile app, mobile service, scale, scalable, app deployment, azure app deployment"
    services="app-service\mobile"
    documentationCenter=""
    authors="ggailey777" 
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="12/03/2015"
    ms.author="glenga"/>

# Azure Mobile Apps 用 .NET バックエンド サーバー SDK の操作

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]
&nbsp;

[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

このトピックでは、Azure App Service Mobile Apps の主要なシナリオで .NET バックエンド サーバー SDK を使用する方法について説明します。 Azure Mobile Apps SDK を使用すると、ASP.NET アプリケーションからモバイル クライアントを操作することができます。

>[AZURE.TIP]  [.NET のサーバーの Azure Mobile Apps SDK](https://github.com/Azure/azure-mobile-apps-net-server) GitHub のオープン ソースです。 リポジトリには、いくつかのサンプル プロジェクトと同様に、サーバー全体の SDK ユニット テスト スイートが含まれています。

## リファレンス ドキュメント

サーバー SDK のリファレンス ドキュメントは、: [Azure モバイル アプリの .NET リファレンス](https://msdn.microsoft.com/library/azure/dn961176.aspx)します。

## <a name="create-app"></a>方法: モバイル アプリケーション用の .NET バックエンドを作成する

新しいプロジェクトを開始する場合は、いずれかを使用してアプリケーションを App Service を作成することができます、 [Azure portal] または Visual Studio です。 このセクションでは、このいずれかを使用して、シンプルな To Do List API をホストするモバイル アプリケーション バックエンドを新規作成する方法を紹介します。 これをローカルで実行することも、プロジェクトをクラウド ベースの App Service モバイル アプリに発行することもできます。

既存のプロジェクトにモバイル機能を追加する場合を参照してください、 [sdk をダウンロードして](#install-sdk) 以下のセクションです。

### Azure ポータルで .NET バックエンドを作成する

新しいモバイル アプリケーション権限を作成する、 [Azure portal]します。 次の手順に従うか、新しいクライアントおよびサーバーをに従って共同作成、 [モバイル アプリを作成する](app-service-mobile-ios-get-started.md) チュートリアルです。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

&nbsp;&nbsp;9. 戻り、 _開始_ ブレードで、 **テーブル API を作成する**, を選択 **c#** として、 **のバックエンド言語**です。

&nbsp;&nbsp;10. [ダウンロード] をクリックし、圧縮されたプロジェクト ファイルをローカル コンピューターに抽出して、Visual Studio でソリューションを開きます。

### Visual Studio 2013 または Visual Studio 2015 を使用して .NET バックエンドを作成する

Visual Studio でモバイル アプリ プロジェクトを作成するためにインストールする必要がある、 [Azure SDK for .NET](https://azure.microsoft.com/downloads/), 、2.8.1 のバージョンまたはそれ以降。 SDK のインストールが完了したら、ASP.NET アプリケーションを新規作成します。

1. 開いている、 **新しいプロジェクト** ダイアログ (から *ファイル* > **新規** > **プロジェクト...**)。

2. 展開 **テンプレート** > **Visual c#**, を選択して **Web**します。

3. 選択 **ASP.NET Web アプリケーション**します。

4. プロジェクト名を入力します。 クリックして **OK**します。

5.  _ASP.NET 4.5.2 テンプレート_, [ **Azure モバイル アプリ**します。 確認 **クラウドでホスト** このプロジェクトの発行先のクラウドの新しいモバイル アプリを作成します。

6. Click **OK**. アプリケーションが作成され、ソリューション エクスプローラーに表示されます。

## <a name="install-sdk"></a>方法: SDK をダウンロードして初期化する

この SDK を使用で [NuGet.org]します。 このパッケージには、SDK の使用を開始するために必要な基礎機能が含まれています。 SDK を初期化するには、操作を実行する必要があります、 **HttpConfiguration** オブジェクトです。 

###SDK のインストール

Visual Studio で、選択、サーバー プロジェクトを右クリックし、SDK をインストールする **NuGet パッケージの管理**, 、検索、 [Microsoft.Azure.Mobile.Server](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) パッケージでをクリックして **インストール**します。  

###<a name="server-project-setup"></a> サーバー プロジェクトの初期化

.NET バックエンド サーバー プロジェクトは、他の ASP.NET プロジェクトと同じように、OWIN スタートアップ クラスを組み込むことによって初期化します。 NuGet パッケージ `Microsoft.Owin.Host.SystemWeb` が参照されていることを確認します。 Visual Studio でこのクラスを追加するサーバー プロジェクトを右クリックし、選択 **追加** ]-> [ **[新しい項目の**, 、し **Web** ]-> [ **全般** ]-> [ **OWIN Startup クラス**します。

これにより、次の属性を持つクラスが生成されます。

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

 `Configuration()` サーバー プロジェクトを使用して、設定、OWIN startup クラスのメソッド、 **HttpConfiguration** をサービスの構成オプションを表すオブジェクト。 次の例では、機能を追加せずにサーバー プロジェクトを初期化します。 

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();
       
        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);  
        
        app.UseWebApi(config);
    }

個々 の機能を有効にするのには、拡張メソッドを呼び出す必要があります、 **MobileAppConfiguration** オブジェクトを呼び出す前に **適用先**します。 たとえば、次のコードでは、初期化中に `[MobileAppController]` 属性が設定されているすべての API コントローラーに既定のルートを追加します。

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

`MapApiControllers` は、`[MobileAppController]` 属性が設定されているコントローラーのみマップすることに注意してください。

機能拡張メソッドの多くは、含めることが可能な追加の NuGet パッケージから入手できます。これらについては以下のセクションで説明します。 

Azure ポータルの呼び出しの中から、サーバーのクイック スタート **UseDefaultConfiguration()**します。 これは次のセットアップと同等です。
    
        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);


### SDK 拡張機能

次の NuGet ベースの拡張機能パッケージでは、アプリケーションで使用可能な各種モバイル機能が提供されます。 使用して初期化中に拡張機能を有効にする、 **MobileAppConfiguration** オブジェクトです。

- [Microsoft.Azure.Mobile.Server.Quickstart] 
    モバイル アプリの基本的なセットアップをサポートしています。 構成に追加して呼び出すことによって、 **UseDefaultConfiguration** の初期化中に拡張メソッド。 この拡張機能には、Notifications、Authentication、Entity、Tables、Crossdomain、および Home パッケージが含まれています。 これは、Azure ポータル からダウンロードするクイックスタート サーバー プロジェクトに相当します。

- [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/)   
    既定値を実装する *このモバイル アプリが稼働しているページ* の web サイトのルートです。 呼び出すことによって、構成に追加、 **AddMobileAppHomeController** 拡張メソッド。

- [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/)  
    データを使用するためのクラスが含まれるセットをしてデータ パイプラインです。 呼び出すことによって、構成に追加、 **AddTables** 拡張メソッド。

- [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/)   
    SQL データベースのデータにアクセスする Entity Framework を有効にします。 呼び出すことによって、構成に追加、 **AddTablesWithEntityFramework** 拡張メソッド。

- [Microsoft.Azure.Mobile.Server.Authentication] 
   認証を有効にしセット アップ、OWIN ミドルウェアのトークンを検証するために使用します。 呼び出すことによって、構成に追加、 **AddAppServiceAuthentication** と **IAppBuilder**.**UseMobileAppAuthentication** 拡張メソッド。

- [Microsoft.Azure.Mobile.Server.Notifications]
   プッシュ通知を有効にし、プッシュ登録エンドポイントを定義します。 呼び出すことによって、構成に追加、 **AddPushNotifications** 拡張メソッド。

- [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/)  
    モバイル アプリからデータを従来の web ブラウザーに機能するコント ローラーを作成します。 呼び出すことによって、構成に追加、 **MapLegacyCrossDomainController** 拡張メソッド。

- [Microsoft.Azure.Mobile.Server.Login]
    MobileAppLoginHandler.CreateToken() メソッドを使用したカスタム認証のプレビューのサポートを提供します。 これは静的メソッドであり、この構成で有効にする必要はありません。

## 方法: サーバー プロジェクトを発行する

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service](../../includes/app-service-mobile-dotnet-backend-publish-service.md)]

「その他方法のいずれかを使用することも、 [Azure App Service の展開のドキュメント](../app-service-web/web-site-deploy.md)します。

## 方法: テーブル コントローラーを定義する

テーブル コントローラーは、テーブル ベースのデータ ストア (SQL Database や Azure Table Storage など) にあるエンティティ データへのアクセスを提供します。 テーブル コント ローラーの継承、 **TableController** ジェネリック クラスは、ジェネリック型は次のように、テーブル スキーマを表すモデル内のエンティティ。

    public class TodoItemController : TableController<TodoItem>
    {  
        //...
    }

使用してテーブル コント ローラーが初期化されて、 **AddTables** 拡張メソッド。 これにより、`TableController` のすべてのサブクラスが `/tables/` の下に追加されます。 

次の例では、データ アクセスに Entity Framework を使用するテーブル コントローラーを初期化します。

    new MobileAppConfiguration().AddTables(
        new MobileAppTableConfiguration()
        .MapTableControllers()
        .AddEntityFramework()).ApplyTo(config);
 
Azure SQL Database からデータにアクセスする Entity Framework を使用するテーブル コント ローラーの例は、次を参照してください。、 **TodoItemController** クラス、Azure ポータルからクイック スタート サーバー プロジェクトのダウンロードにします。

## 方法: カスタム API コントローラーを定義する

カスタム API コントローラーは、エンドポイントを公開して、モバイル アプリのバックエンドに最も基本的な機能を提供します。 `MobileAppControllerAttribute` 属性を使用して、モバイル固有の API コント ローラーを登録できます。 この属性は、ルートを登録し、Mobile Apps JSON シリアライザーも設定します。 

1. Visual Studio で、Controllers フォルダーを右クリックし、クリックして **追加** > **コント ローラー**, [ **Web API 2 コント ローラー & mdash;空** ] をクリック **追加**します。

2. 指定する **コント ローラー名**, など `CustomController`, 、] をクリック **追加**します。 これを新たに作成 **CustomController** から継承するクラス **ApiController**します。   

3. 新しいコントローラーのクラス ファイルに、次の using ステートメントを追加します。

        using Microsoft.Azure.Mobile.Server.Config;

4. 適用、 **MobileAppControllerAttribute** をコント ローラー クラスの API 定義の例を次に示します。

        [MobileAppController] 
        public class CustomController : ApiController
        {
              //...
        }

4. App_Start/Startup.MobileApp.cs ファイルへの呼び出しを追加、  **MapApiControllers** 拡張メソッドを次の例に示すようにします。

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);
    
    呼び出す必要はありません注 **MapApiControllers** 呼び出す必要が場合 **UseDefaultConfiguration**, 、すべての機能を初期化します。 

任意のコント ローラーがない **MobileAppControllerAttribute** 適用される、クライアントからもアクセスできますがない使用することが正しく、モバイル アプリ クライアント SDK を使用してクライアントからです。 


## 方法: サーバー プロジェクトに認証を追加する

サーバー プロジェクトに認証を追加するには拡張することによって、 **MobileAppConfiguration** オブジェクトと OWIN ミドルウェアを構成します。 インストールすると、 [Microsoft.Azure.Mobile.Server.Quickstart] パッケージと呼び出し、 **UseDefaultConfiguration** 拡張メソッドでは、手順 3 にスキップできます。

1. Visual Studio で、インストール、 [Microsoft.Azure.Mobile.Server.Authentication] パッケージです。 

2. Startup.cs のプロジェクト ファイル内の先頭に次のコード行を追加、 **構成** メソッド。

        app.UseMobileAppAuthentication(config);

    これにより、関連付けられた App Service ゲートウェイで発行されたトークンを Azure モバイル アプリが検証できるようにする、OWIN ミドルウェア コンポーネントが追加されます。

3. 認証が必要なすべてのコントローラーまたはメソッドに `[Authorize]` 属性を追加します。 これで、ユーザーはこのエンドポイントや特定の API にアクセスするときに認証が必須になりました。

モバイル アプリ バックエンドにクライアントを認証する方法の詳細については、次を参照してください。 [アプリへの認証の追加](app-service-mobile-ios-get-started-users.md)します。

## <a name="custom-auth"></a>方法: アプリケーションにカスタム認証を使用する

App Service Authentication/Authorization プロバイダーの中に使用したいものがない場合は、自身でログイン システムを用意することもできます。 これを行うには、インストール、 [Microsoft.Azure.Mobile.Server.Login] パッケージです。

ユーザーをサインインするかどうかを判断するための独自のロジックを提供する必要があります。 たとえば、データベース内のソルトを使用してハッシュ化されたパスワードと照合することができます。 次の例では、`isValidAssertion()` メソッドがこれらの照合を実行します。このメソッドは他の場所で定義されています。 

カスタム認証を公開するには、ApiController を新規作成し、以下のような登録とログインのアクションを公開します。 クライアントは、ユーザーから関連情報を収集し、本文にユーザー情報を含めた HTTPS POST を API に送信することでログインを試行できます。 情報が検証されたら、`MobileAppLoginHandler.CreateToken()` メソッドを使用してトークンを発行できます。 

ログイン アクションの例は次のようになります。

        public HttpResponseMessage Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = MobileAppLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return this.Request.CreateResponse(HttpStatusCode.OK, new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

上記では、LoginResult と LoginResultUser は、プロパティを公開するだけのシンプルなオブジェクトです。 クライアントでは、ログインの応答が次の形式の JSON オブジェクトとして返されるものと想定されています。

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

 `MobileAppLoginHandler.CreateToken()` メソッドに含まれる、 _対象ユーザー_ と _発行者_ パラメーター。 どちらも通常は、HTTPS スキームを使用して、アプリケーション ルートの URL に設定します。 同様に設定する必要があります _秘密鍵_ 、アプリケーションの値のキーを署名します。 これは機密性の高い値なので、共有したりクライアントに保存したりしないでください。 参照することで、App Service でホストされているときに、この値を取得できます、 _WEBSITE_AUTH_SIGNING_KEY_ 環境変数です。 ローカル デバッグ コンテキストで必要な場合の指示に従って、 [認証のローカル デバッグ](#local-debug) セクションにキーを取得し、アプリケーション設定として保存します。

また、発行されたトークンの有効期間と、含めたい要求を指定する必要があります。 サンプル コードに示すとおり、サブジェクトの要求を指定する必要があります。

## 方法: サーバー プロジェクトにプロジェクトを追加する

プッシュ通知をサーバー プロジェクトに追加するには拡張することによって、 **MobileAppConfiguration** オブジェクトおよび Notification Hubs クライアントを作成します。 インストールすると、 [Microsoft.Azure.Mobile.Server.Quickstart] パッケージと呼び出し、 **UseDefaultConfiguration** 拡張メソッドをステップ 3 を省略できます。

1. Visual Studio で、サーバー プロジェクトを右クリックし、クリックして **NuGet パッケージの管理**, を Microsoft.Azure.Mobile.Server.Notifications' を検索してクリックして **インストール**します。 これにより、インストール、 [Microsoft.Azure.Mobile.Server.Notifications] パッケージです。
 
3. 上記の手順を繰り返して、Notification Hubs クライアント ライブラリが含まれる `Microsoft.Azure.NotificationHubs` パッケージをインストールします。 

2. App_Start/Startup.MobileApp.cs の呼び出しを追加し、 **AddPushNotifications** 拡張メソッドの初期化中に、次のようになります。

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);

    これにより、サーバー プロジェクトにプッシュ通知の登録エンドポイントが作成されます。 このエンドポイントは、関連する通知ハブにクライアントが登録を行う際に使用されます。 ここでは、通知の送信に使用される Notification Hub クライアントを追加する必要があります。

3. プッシュ通知の送信元にするコントローラーで、次の using ステートメントを追加します。

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;

4. Notification Hubs クライアントを作成する次のコードを追加します。

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

これで、Notification Hubs クライアントを使用して、登録済みデバイスにプッシュ通知を送信できるようになりました。 詳細については、次を参照してください。 [アプリにプッシュ通知を追加](app-service-mobile-ios-get-started-push.md)します。 Notification Hubs の使用可能なすべての詳細については、次を参照してください。 [Notification Hubs の概要](../notification-hubs/notification-hubs-overview.md)します。

##<a name="tags"></a>方法: タグへのプッシュを有効にするために、タグをデバイス インストールに追加する

上記 **する方法: カスタム API コント ローラーを定義する**, 、特定のデバイスのインストールにタグを追加する通知ハブを使用する、バックエンドのカスタム API を設定します。 クライアント ローカル ストレージに保存されているインストール ID と追加するタグ (タグはバックエンドで直接指定できるため、これは任意) を指定します。 Notification Hubs と連動させ、タグをデバイス インストール ID に追加するために、次のスニペットをコントローラーに追加してください。

使用して [Azure 通知ハブ NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) ([参照](https://msdn.microsoft.com/library/azure/mt414893.aspx))。

        var hub = NotificationHubClient.CreateClientFromConnectionString("my-connection-string", "my-hub");

        hub.PatchInstallation("my-installation-id", new[]
        {
            new PartialUpdateOperation
            {
                Operation = UpdateOperationType.Add,
                Path = "/tags",
                Value = "{my-tag}"
            }
        });

これらのタグへのプッシュと作業 [通知ハブの Api](https://msdn.microsoft.com/library/azure/dn495101.aspx)します。

カスタム API を立ち上げ、バックエンドで直接、デバイス インストールを Notification Hubs に登録することもできます。

## 方法: .NET サーバー SDK のデバッグとトラブルシューティングを実行する

Azure App Service には、ASP.NET アプリケーションのデバッグとトラブルシューティングの方法が複数用意されています。

- [Azure App Service の監視](../app-service-web/web-sites-monitor.md)
- [Azure App Service での診断ログの有効化](../app-service-web/web-sites-enable-diagnostic-log.md)
- [Visual Studio での Azure App Service のトラブルシューティング](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### ログの記録

標準の ASP.NET トレース書き込みを使用して、App Service の診断ログを作成できます。

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");  

### <a name="local-debug"></a>認証に関するローカル デバッグ

クラウドに発行する前に変更をテストするために、ローカルでアプリケーションを実行できます。 これだけを押すのとは、多くのアプリケーション、 *f5 キーを押して* Visual Studio の中にします。 ただし、認証を使用している場合は追加の考慮事項がいくつかあります。

App Service Authentication/Authorization を使用してクラウド ベースのモバイル アプリを構成する必要があります。また、クライアントに代替ログイン ホストとしてクラウド エンドポイントを指定する必要があります。 各プラットフォームの選択したクライアントのドキュメントを参照してください ([iOS](app-service-mobile-ios-how-to-use-client-library.md), 、[Windows/Xamarin](app-service-mobile-dotnet-how-to-use-client-library.md)) の具体的な手順です。

アプリケーションのことを確認 [Microsoft.Azure.Mobile.Server.Authentication] をインストールします。 次に、`MobileAppConfiguration` が `HttpConfiguration` に適用された後で、アプリケーションの OWIN Startup クラスに次のコードを追加します。
        
        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetMobileAppTokenHandler()
        });

上記の例では、構成する必要があります、 _authAudience_ と _authIssuer_ 、Web.config 内のアプリケーション設定ファイルのそれぞれに HTTPS スキームを使用して、アプリケーション ルートの URL を指定します。 同様に設定する必要があります _authSigningKey_ 、アプリケーションの値のキーを署名します。 これは機密性の高い値なので、共有したりクライアントに保存したりしないでください。 それを取得する内で、アプリに移動、 [Azure portal] ] をクリック **ツール**します。 選択し、 **Kudu** ] をクリック **移動**します。 この操作により、サイトの Kudu 管理エンドポイントに移動します。 クリックして **環境** 下の値を見つけて _WEBSITE_AUTH_SIGNING_KEY_します。 これは、値を使用してください。 _authSigningKey_ のローカルのアプリケーション構成にします。

ローカルで実行されているサーバーで、クライアントがクラウド ベースのエンドポイントから取得するトークンを検証できるようになりました。


[Azure portal]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/


