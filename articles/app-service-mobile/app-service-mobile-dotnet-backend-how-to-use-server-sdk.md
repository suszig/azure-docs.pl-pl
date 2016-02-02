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
>[AZURE.TIP] [.NET のサーバーの Azure Mobile Apps SDK](https://github.com/Azure/azure-mobile-apps-net-server) GitHub のオープン ソースです。 リポジトリには、いくつかのサンプル プロジェクトと同様に、サーバー全体の SDK ユニット テスト スイートが含まれています。

## リファレンス ドキュメント

サーバー SDK のリファレンス ドキュメントは、: [Azure モバイル アプリの .NET リファレンス](https://msdn.microsoft.com/library/azure/dn961176.aspx)します。

## <a name="create-app"></a>方法: モバイル アプリケーションの .NET バックエンドを作成します。

[Azure ポータル] を使用して App Service アプリケーションを作成することができます、新しいプロジェクトを開始する場合、または Visual Studio です。 このセクションでは、このいずれかを使用して、シンプルな To Do List API をホストするモバイル アプリケーション バックエンドを新規作成する方法を紹介します。 これをローカルで実行することも、プロジェクトをクラウド ベースの App Service モバイル アプリに発行することもできます。

既存のプロジェクトにモバイル機能を追加する場合を参照してください、 [sdk をダウンロードして](#install-sdk) 以下のセクションです。

### Azure ポータルで .NET バックエンドを作成する

[Azure ポータル] で、新しいモバイル アプリケーションの権限を作成できます。 次の手順に従うか、新しいクライアントおよびサーバーをに従って共同作成、 [モバイル アプリを作成する](app-service-mobile-ios-get-started.md) チュートリアルです。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

&nbsp;&nbsp;9. _Get started_ ブレードで、[バックアップ **テーブル API を作成する**, 、選択 **c#** として、 **のバックエンド言語**します。

&nbsp;&nbsp;10. [ダウンロード] をクリックし、圧縮されたプロジェクト ファイルをローカル コンピューターに抽出して、Visual Studio でソリューションを開きます。

### Visual Studio 2013 または Visual Studio 2015 を使用して .NET バックエンドを作成する

Visual Studio でモバイル アプリ プロジェクトを作成するためにインストールする必要がある、 [Azure SDK for .NET](https://azure.microsoft.com/downloads/), 、2.8.1 のバージョンまたはそれ以降。 SDK のインストールが完了したら、ASP.NET アプリケーションを新規作成します。

1. **[新しいプロジェクト]** ダイアログを開きます (*[ファイル]*、**[新規作成]**、**[プロジェクト...]** の順にクリックします)。

2. **[テンプレート]**、**[Visual C#]** の順に展開し、**[Web]** を選択します。

3. **[ASP.NET Web アプリケーション]** を選択します。

4. プロジェクト名を入力します。 次に、 **[OK]** をクリックします

5. _ASP.NET 4.5.2 [Templates_ 選択 **Azure モバイル アプリ**します。 このプロジェクトを発行できるクラウドに新しいモバイル アプリを作成するために、**[クラウドでホストする]** チェック ボックスをオンにします。

6. **[OK]** をクリックします。 アプリケーションが作成され、ソリューション エクスプローラーに表示されます。

## <a name="install-sdk"></a>方法: ダウンロードし、sdk の開始

この SDK は、[NuGet.org] でご確認いただけます。 このパッケージには、SDK の使用を開始するために必要な基礎機能が含まれています。 SDK を初期化するには、**HttpConfiguration** オブジェクトに対して操作を実行する必要があります。

### SDK のインストール

Visual Studio で、選択、サーバー プロジェクトを右クリックし、SDK をインストールする **NuGet パッケージの管理**, 、検索、 [Microsoft.Azure.Mobile.Server](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) パッケージでをクリックして **インストール**します。

### <a name="server-project-setup"></a> サーバー プロジェクトを初期化します。

.NET バックエンド サーバー プロジェクトは、他の ASP.NET プロジェクトと同じように、OWIN スタートアップ クラスを組み込むことによって初期化します。 NuGet パッケージが参照されていることを確認 `Microsoft.Owin.Host.SystemWeb`します。 Visual Studio でこのクラスを追加するには、サーバー プロジェクトを右クリックして、**[追加]**、**[新しい項目]**、**[Web]**、**[全般]**、**[OWIN スタートアップ クラス]** の順に選択します。

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

個別の機能を有効化するには、**ApplyTo** を呼び出す前に、**MobileAppConfiguration** オブジェクトに対して拡張機能メソッドを呼び出す必要があります。 たとえば、次のコードは属性を持つすべての API コント ローラーに既定のルートを追加 `[MobileAppController]` の初期化中にします。

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

なお `MapApiControllers` のみ属性を持つコント ローラーにマップ `[MobileAppController]`します。

機能拡張メソッドの多くは、含めることが可能な追加の NuGet パッケージから入手できます。これらについては以下のセクションで説明します。

Azure ポータルからのサーバーのクイックスタートは **UseDefaultConfiguration()** を呼び出します。 これは次のセットアップと同等です。

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

次の NuGet ベースの拡張機能パッケージでは、アプリケーションで使用可能な各種モバイル機能が提供されます。 拡張機能の有効化は、初期化中に **MobileAppConfiguration** オブジェクトを使用して行います。

- [Microsoft.Azure.Mobile.Server.Quickstart]  
     モバイル アプリの基本的なセットアップをサポートしています。 構成に追加するには、初期化中に **UseDefaultConfiguration** 拡張機能メソッドを呼び出します。 この拡張機能には、Notifications、Authentication、Entity、Tables、Crossdomain、および Home パッケージが含まれています。 これは、Azure ポータル からダウンロードするクイックスタート サーバー プロジェクトに相当します。

- [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/)   
    既定値を実装する *このモバイル アプリが稼働しているページ* の web サイトのルートです。 構成に追加するには、**AddMobileAppHomeController** 拡張メソッドを呼び出します。

- [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/)  
    データを使用するためのクラスが含まれるセットをしてデータ パイプラインです。 構成に追加するには、**AddTables** 拡張メソッドを呼び出します。

- [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/)   
    SQL データベースのデータにアクセスする Entity Framework を有効にします。 構成に追加するには、**AddTablesWithEntityFramework** 拡張メソッドを呼び出します。

- [Microsoft.Azure.Mobile.Server.Authentication]  
    認証を有効にしセット アップ、OWIN ミドルウェアのトークンを検証するために使用します。 構成に追加するには、**AddAppServiceAuthentication** 拡張メソッドと **IAppBuilder**.**UseMobileAppAuthentication** 拡張メソッドを呼び出します。

- [Microsoft.Azure.Mobile.Server.Notifications]
    プッシュ通知を有効にし、プッシュ登録エンドポイントを定義します。 構成に追加するには、**AddPushNotifications** 拡張メソッドを呼び出します。

- [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/)  
    モバイル アプリからデータを従来の web ブラウザーに機能するコント ローラーを作成します。 構成に追加するには、**MapLegacyCrossDomainController** 拡張メソッドを呼び出します。

- [Microsoft.Azure.Mobile.Server.Login] 
     MobileAppLoginHandler.CreateToken() メソッドを使用したカスタム認証のプレビューのサポートを提供します。 これは静的メソッドであり、この構成で有効にする必要はありません。

## 方法: サーバー プロジェクトを発行する

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service](../../includes/app-service-mobile-dotnet-backend-publish-service.md)]

「その他方法のいずれかを使用することも、 [Azure App Service 展開のドキュメント](../app-service-web/web-site-deploy.md)します。

## 方法: テーブル コントローラーを定義する

テーブル コントローラーは、テーブル ベースのデータ ストア (SQL Database や Azure Table Storage など) にあるエンティティ データへのアクセスを提供します。 テーブル コントローラーは、次のように **TableController** ジェネリック クラスから継承します。このクラスでは、ジェネリック型はテーブル スキーマを表すモデルのエンティティです。

    public class TodoItemController : TableController<TodoItem>
    {  
        //...
    }

テーブル コントローラーは、**AddTables** 拡張メソッドを使用して初期化します。 これにより、追加ルートを `/tables/` のすべてのサブクラスの `TableController`します。

次の例では、データ アクセスに Entity Framework を使用するテーブル コントローラーを初期化します。

    new MobileAppConfiguration().AddTables(
        new MobileAppTableConfiguration()
        .MapTableControllers()
        .AddEntityFramework()).ApplyTo(config);

Azure SQL Database のデータへのアクセスに Entity Framework を使用するテーブル コントローラーの例については、Azure ポータル からダウンロードしたクイックスタート サーバー プロジェクトの **TodoItemController** クラスを参照してください。

## 方法: カスタム API コントローラーを定義する

カスタム API コントローラーは、エンドポイントを公開して、モバイル アプリのバックエンドに最も基本的な機能を提供します。 属性を使用してモバイル固有 API コント ローラーを登録する `MobileAppControllerAttribute`します。 この属性は、ルートを登録し、Mobile Apps JSON シリアライザーも設定します。

1. Visual Studio で、Controllers フォルダーを右クリックして、**[追加]**、**[コントローラー]** の順にクリックし、**[Web API 2 コントローラー - 空]** を選択して **[追加]** をクリックします。

2. 指定する **コント ローラー名**, など `CustomController`, 、] をクリック **追加**します。 これで、**ApiController** から継承した新しい **CustomController** クラスが作成されます。

3. 新しいコントローラーのクラス ファイルに、次の using ステートメントを追加します。

        using Microsoft.Azure.Mobile.Server.Config;

4. 次の例に従って、API コントローラーのクラス定義に **MobileAppControllerAttribute** を適用します。

        [MobileAppController] 
        public class CustomController : ApiController
        {
              //...
        }

4. App_Start/Startup.MobileApp.cs ファイルへの呼び出しを追加、  **MapApiControllers** 拡張メソッドを次の例に示すようにします。

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

    代わりに、すべての機能を初期化する **UseDefaultConfiguration** を呼び出す場合は、**MapApiControllers** を呼び出す必要はありません。

クライアントは **MobileAppControllerAttribute** が適用されていないコントローラーにもアクセスできますが、こうしたコントローラーはモバイル アプリ クライアント SDK を使用するクライアントでは適切に使用されない場合があります。


## 方法: サーバー プロジェクトに認証を追加する

**MobileAppConfiguration** オブジェクトを拡張し、OWIN ミドルウェアを構成すると、サーバー プロジェクトに認証を追加することができます。 [Microsoft.Azure.Mobile.Server.Quickstart] パッケージと呼び出しをインストールすると、 **UseDefaultConfiguration** 拡張メソッドでは、手順 3 にスキップできます。

1. Visual Studio で、[Microsoft.Azure.Mobile.Server.Authentication] パッケージをインストールします。

2. Startup.cs プロジェクト ファイルで、**Configuration** メソッドの先頭に次のコード行を追加します。

        app.UseMobileAppAuthentication(config);

    これにより、関連付けられた App Service ゲートウェイで発行されたトークンを Azure モバイル アプリが検証できるようにする、OWIN ミドルウェア コンポーネントが追加されます。

3. 追加、 `[Authorize]` 属性を任意のコント ローラーまたは認証が必要なメソッドです。 これで、ユーザーはこのエンドポイントや特定の API にアクセスするときに認証が必須になりました。

モバイル アプリ バックエンドにクライアントを認証する方法の詳細については、次を参照してください。 [アプリへの認証の追加](app-service-mobile-ios-get-started-users.md)します。

## <a name="custom-auth"></a>方法: アプリケーションのカスタム認証を使用します。

App Service Authentication/Authorization プロバイダーの中に使用したいものがない場合は、自身でログイン システムを用意することもできます。 このようにするには、[Microsoft.Azure.Mobile.Server.Login] パッケージをインストールします。

ユーザーをサインインするかどうかを判断するための独自のロジックを提供する必要があります。 たとえば、データベース内のソルトを使用してハッシュ化されたパスワードと照合することができます。 次の例で、 `isValidAssertion()` メソッドはこれらのチェックを担当するであり、他の場所で定義します。

カスタム認証を公開するには、ApiController を新規作成し、以下のような登録とログインのアクションを公開します。 クライアントは、ユーザーから関連情報を収集し、本文にユーザー情報を含めた HTTPS POST を API に送信することでログインを試行できます。 使用して、トークンを発行できる情報が検証されたら、 `MobileAppLoginHandler.CreateToken()` メソッドです。

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

`MobileAppLoginHandler.CreateToken()` メソッドに含まれる、 _対象ユーザー_ と _発行者_ パラメーター。 どちらも通常は、HTTPS スキームを使用して、アプリケーション ルートの URL に設定します。 同様に、_secretKey_ をアプリケーションの署名キーの値に設定する必要があります。 これは機密性の高い値なので、共有したりクライアントに保存したりしないでください。 この値を取得するには、App Service でホストされているときに _WEBSITE_AUTH_SIGNING_KEY_ 環境変数を参照します。 ローカル デバッグ コンテキストで必要な場合の指示に従って、 [認証のローカル デバッグ](#local-debug) セクションにキーを取得し、アプリケーション設定として保存します。

また、発行されたトークンの有効期間と、含めたい要求を指定する必要があります。 サンプル コードに示すとおり、サブジェクトの要求を指定する必要があります。

## 方法: サーバー プロジェクトにプロジェクトを追加する

**MobileAppConfiguration** オブジェクトを拡張し、Notification Hubs クライアントを作成すると、サーバー プロジェクトにプッシュ通知を追加することができます。 [Microsoft.Azure.Mobile.Server.Quickstart] パッケージと呼び出しをインストールすると、 **UseDefaultConfiguration** 拡張メソッドをステップ 3 を省略できます。

1. Visual Studio で、サーバー プロジェクトを右クリックして **[NuGet パッケージの管理]** をクリックし、Microsoft.Azure.Mobile.Server.Notifications を検索して、**[インストール]** をクリックします。 これは、[Microsoft.Azure.Mobile.Server.Notifications] パッケージをインストールします。

3. インストールするには、この手順を繰り返して、 `Microsoft.Azure.NotificationHubs` パッケージで、通知ハブのクライアント ライブラリが含まれています。

2. App_Start/Startup.MobileApp.cs で、次に示すように、初期化中に **AddPushNotifications** 拡張メソッドの呼び出しを追加します。

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

## <a name="tags"></a>方法: プッシュのタグを有効にするデバイスのインストールにタグを追加

上記の「**方法: カスタム API コントローラーを定義する**」の手順を実行したら、バックエンドにカスタム API を設定し、Notification Hubs と連動させて、タグを特定のデバイス インストールに追加します。 クライアント ローカル ストレージに保存されているインストール ID と追加するタグ (タグはバックエンドで直接指定できるため、これは任意) を指定します。 Notification Hubs と連動させ、タグをデバイス インストール ID に追加するために、次のスニペットをコントローラーに追加してください。

使用して [Azure 通知ハブ NuGet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) ([参照](https://msdn.microsoft.com/library/azure/mt414893.aspx)):

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
- [Azure App Service での診断ログを有効にします。](../app-service-web/web-sites-enable-diagnostic-log.md)
- [Visual Studio での Azure の App Service のトラブルシューティングを行う](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### ログの記録

標準の ASP.NET トレース書き込みを使用して、App Service の診断ログを作成できます。

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");  

### <a name="local-debug"></a>ローカルの認証を使用したデバッグ

クラウドに発行する前に変更をテストするために、ローカルでアプリケーションを実行できます。 多くのアプリは、Visual Studio で *F5* キーを押すだけで実行できます。 ただし、認証を使用している場合は追加の考慮事項がいくつかあります。

App Service Authentication/Authorization を使用してクラウド ベースのモバイル アプリを構成する必要があります。また、クライアントに代替ログイン ホストとしてクラウド エンドポイントを指定する必要があります。 各プラットフォームの選択したクライアントのドキュメントを参照してください ([iOS](app-service-mobile-ios-how-to-use-client-library.md), 、[Windows/Xamarin](app-service-mobile-dotnet-how-to-use-client-library.md)) の具体的な手順です。

[Microsoft.Azure.Mobile.Server.Authentication] に、アプリケーションがインストールされています。 次に、アプリケーションの OWIN スタートアップ クラスの後に、次のコードを追加 `MobileAppConfiguration` に適用されている、 `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetMobileAppTokenHandler()
        });

上記の例では、Web.config ファイル内のアプリケーション設定 _authAudience_ と _authIssuer_ を、HTTPS スキームを使用して、それぞれアプリケーション ルートの URL に構成する必要があります。 同様に、_authSigningKey_ をアプリケーションの署名キーの値に設定する必要があります。 これは機密性の高い値なので、共有したりクライアントに保存したりしないでください。 それを取得する [Azure ポータル] 内、アプリに移動し、クリックして **ツール**します。 **[Kudu]** を選択し、**[移動]** をクリックします。 この操作により、サイトの Kudu 管理エンドポイントに移動します。 **[環境]** をクリックし、_WEBSITE_AUTH_SIGNING_KEY_ で値を確認します。 これがローカル アプリ構成の _authSigningKey_ で使用する必要がある値です。

ローカルで実行されているサーバーで、クライアントがクラウド ベースのエンドポイントから取得するトークンを検証できるようになりました。



[azure portal]: https://portal.azure.com 
[nuget.org]: http://www.nuget.org/ 
[microsoft.azure.mobile.server.quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/ 
[microsoft.azure.mobile.server.authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/ 
[microsoft.azure.mobile.server.login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/ 
[microsoft.azure.mobile.server.notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/ 

