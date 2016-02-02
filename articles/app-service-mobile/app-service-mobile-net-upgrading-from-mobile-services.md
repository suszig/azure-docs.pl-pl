<properties 
    pageTitle="Mobile Services から Azure App Service にアップグレードする" 
    description="簡単に Mobile Services アプリケーションを App Service モバイル アプリにアップグレードする方法について説明します。" 
    services="app-service\mobile" 
    documentationCenter="" 
    authors="mattchenderson" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-mobile" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="11/29/2015" 
    ms.author="mahender"/>


# 既存の .NET Azure Mobile Service を App Service にアップグレードする

App Service Mobile は、Microsoft Azure を使用してモバイル アプリケーションを構築する新しい方法です。 詳細については、[Mobile Apps は何ですか?] を参照してください。

このトピックでは、既存の .NET バックエンド アプリケーションを Azure Mobile Services から新しい App Service Mobile Apps にアップグレードする方法について説明します。 このアップグレードの実行中も、既存の Mobile Services アプリケーションの動作を続行できます。

すべての App Service の機能にアクセスし、[App Service の料金] に応じて課金されます、モバイル バックエンドを Azure App Service にアップグレードするとき、モバイル サービスではなく価格設定します。

## 移行とアップグレード

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]
>[AZURE.TIP] お勧めする [ěúťsc](app-service-mobile-migrating-from-mobile-services.md) アップグレードを実行する前にします。 そうすることで、同じ App Service プランに両方のバージョンのアプリケーションを指定できるため、追加コストが発生しません。

### Mobile Apps .NET サーバー SDK の機能強化

新しいへのアップグレード [Mobile Apps SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) 、次の利点を提供します。

- NuGet の依存関係の柔軟性の向上。 ホスティング環境で独自のバージョンの NuGet パッケージが提供されなくなったので、互換性のある代わりのバージョンを使用できます。 ただし、Mobile Server SDK または依存関係に対して新しい重要なバグ修正やセキュリティ更新プログラムがある場合は、サービスを手動で更新する必要があります。

- モバイル SDK の柔軟性の向上。 認証、テーブル API、プッシュ登録エンドポイントなど、セットアップされる機能とルートを明示的に制御できます。 詳細については、次を参照してください。 [Azure モバイル アプリの .NET server SDK を使用する方法](app-service-mobile-net-upgrading-from-mobile-services.md#server-project-setup)します。

- ASP.NET プロジェクトの他の種類とルートのサポート。 MVC および Web API コントローラーをモバイル バックエンド プロジェクトと同じプロジェクトでホストできるようになりました。

- 新しい App Service 認証機能のサポート。Web アプリとモバイル アプリの両方で共通の認証構成を使用できます。

## <a name="overview"></a>基本的なアップグレードの概要

多くの場合、新しい Mobile Apps サーバー SDK に切り替えて、コードを新しい Mobile App インスタンスに再発行するだけで、簡単にアップグレードできます。 ただし、一部のシナリオは、高度な認証シナリオやスケジュールされたジョブの操作など、いくつか追加の構成が必要になります。 これについてはそれぞれ以降のセクションで説明します。
>[AZURE.TIP] アップグレードを開始する前に、このトピックの残りの部分を読み、よく理解しておくことをお勧めします。 下に列記した機能のうち、使用する機能をすべてメモしてください。

Mobile Services クライアント SDK と新しい Mobile Apps サーバー SDK の間に互換性は**ありません**。 アプリ用にサービスを接続する場合は、発行されたクライアントを現在使用しているサイトに変更を発行しないでください。 代わりに、複製として機能する新しいモバイル アプリを作成する必要があります。 このアプリケーションを同じ App Service プランに指定することで、追加の財務費用が発生しないようにすることができます。

アプリケーションには 2 つのバージョンがあります。1 つは変わらず、発行されたアプリとしてそのままの状態で機能し、もう 1 つはユーザーがアップグレードし、新しいクライアント リリースのターゲットにすることができます。 自分のペースでコードを移動し、テストできますが、バグの修正が両方に適用されることを確認する必要があります。 必要な数の (そのままの状態の) クライアント アプリを最新バージョンに更新したら、必要に応じて、元の移行されたアプリを削除することができます。

このアップグレード プロセスの全概要は、次のとおりです。

1. 新しい Mobile App を作成する
2. 新しい Server SDK を使用するようにプロジェクトを更新する
3. クライアント アプリケーションの新しいバージョンをリリースする
4. (省略可能) 元の移行されたインスタンスを削除する

## <a name="mobile-app-version"></a>2 番目のアプリケーション インスタンスを作成します。

アップグレードの最初のステップは、新しいバージョンのアプリケーションをホストするモバイル アプリ リソースを作成することです。 既存のモバイル サービスを既に移行している場合は、同じホスティング プランでこのバージョンを作成します。 [Azure ポータル] を開き、移行済みのアプリケーションに移動します。 実行されている App Service プランをメモしてをおきます。

次に、に従って 2 番目のアプリケーション インスタンスを作成、 [.NET バックエンドの作成手順](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app)します。 App Service プランまたは "ホスティング プラン" を選択するよう求められたら、移行済みアプリケーションのプランを選択します。

Mobile Services で使用したのと同じデータベースと通知ハブを使用することもできます。 [Azure ポータル] を開き、元のアプリケーションに移動してこれらの値をコピーし、をクリックできます **設定** > **アプリケーション設定**します。  **接続文字列**, 、コピー `MS_NotificationHubConnectionString` と `MS_TableConnectionString`します。 新しいアップグレード サイトに移動し、接続文字列を貼り付けて既存の値を上書きします。 アプリに必要な他のアプリケーション設定について、このプロセスを繰り返します。 移行したサービスを使用していない場合、接続文字列とのアプリ設定を読み取ることができます、 **構成** [Azure クラシック ポータル] の [モバイル サービス] セクションのタブをクリックします。

アプリケーションの ASP.NET プロジェクトのコピーを作成し、新しいサイトに発行します。 新しい URL で更新されたクライアント アプリケーションのコピーを使用して、すべて予期したとおりに機能することを検証します。

## サーバー プロジェクトの更新

Mobile Apps は、新しい [モバイル アプリ サーバー SDK] Mobile Services ランタイムと同じ機能の多くを提供します。 最初に、Mobile Services パッケージへの参照をすべて削除する必要があります。 NuGet パッケージ マネージャーで、検索 `WindowsAzure.MobileServices.Backend`します。 ほとんどのアプリケーションが表示されますここでは、いくつかのパッケージを含む `WindowsAzure.MobileServices.Backend.Tables` と `WindowsAzure.MobileServices.Backend.Entity`します。 このような場合は、依存関係ツリーの最下位のパッケージとなど開始 `エンティティ`, 、しそれを削除します。 プロンプトが表示されても、すべての従属パッケージを削除しないでください。 削除するまで、この手順を繰り返します `WindowsAzure.MobileServices.Backend` 自体です。

この時点で、プロジェクトは Mobile Services SDK を参照しなくなります。

次に、Mobile Apps SDK の参照を追加します。 このアップグレードでは、ほとんどの開発者をダウンロードしてインストールしますが、 `Micrsoft.Azure.Mobile.Server.Quickstart` これには、プル要求セット全体の手順に従って、パッケージです。

SDK の違いにより、かなり多くのコンパイラ エラーが発生しますが、これらは簡単に対処でき、このセクションの残りの部分で説明します。

### 基本構成

次に、WebApiConfig.cs で、

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();
    
        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

を以下に置き換えることができます。

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

>[AZURE.NOTE] 新しい .NET サーバー SDK の詳細と、アプリケーションから機能の追加/削除する方法について説明したい場合は、参照をしてください、.NET サーバー SDK を使用する方法] のトピックです。

アプリで認証機能を使用する場合は、OWIN ミドルウェアを登録する必要もあります。 この場合は、上記の構成コードを新しい OWIN スタートアップ クラスに移動する必要があります。

1. NuGet パッケージを追加 `Microsoft.Owin.Host.SystemWeb` 、プロジェクトに含まれていない場合。
2. Visual Studio で、プロジェクトを右クリックして、**[追加]**、**[新しい項目]** の順に選択します。 **[Web]**、**[全般]**、**[OWIN スタートアップ クラス]** の順に選択します。
3. MobileAppConfiguration の上記のコードを移動 `WebApiConfig.Register()` に、 `Configuration()` によって新しいスタートアップ クラスのメソッドです。

確認、 `Configuration()` でメソッドが終了します。

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

認証に関するその他の変更については、以降の完全な認証セクションで説明します。

### データの操作

Mobile Services では、モバイル アプリ名が Entity Framework セットアップの既定のスキーマ名として機能していました。

以前と同じスキーマが参照されるようにするには、以下を使用して、アプリケーションの DbContext にスキーマを設定します。

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

上記のようにする場合は、MS_MobileServiceName が設定されていることを確認してください。 アプリケーションで既にこのようにカスタマイズしている場合は、別のスキーマ名を指定することもできます。

### システム プロパティ

#### 名前を付ける

Azure Mobile Services server SDK では、システムのプロパティは、必ず二重のアンダー スコア (`_ _`) プロパティのプレフィックス。

- __createdAt
- __updatedAt
- __deleted
- __version

Mobile Services クライアント SDK には、この形式でシステム プロパティを解析するための特別なロジックがあります。

Azure Mobile Apps では、現在、システム プロパティで特別な形式は使用しておらず、以下の名前が使用されています。

- createdAt
- updatedAt
- deleted
- version

Mobile Apps クライアント SDK では新しいシステム プロパティ名を使用するため、クライアント コードを変更する必要はありません。 ただし、直接サービスへの REST 呼び出しを行う場合は、適宜クエリを変更する必要があります。

#### ローカル ストア

システム プロパティの名前を変更すると、Mobile Services のオフライン同期ローカル データベースと Mobile Apps との互換性がなくなります。 可能であれば、保留中の変更がサーバーに送信されるまで、クライアント アプリを Mobile Services から Mobile Apps にアップグレードしないでください。 したがって、アップグレードされたアプリは、新しいデータベースのファイル名を使用する必要があります。

操作キューに保留中のオフラインの変更がある状態で、クライアント アプリを Mobile Services から Mobile Apps にアップグレードする場合は、新しい列名を使用するようにシステム データベースを更新する必要があります。 iOS の場合は、軽量な移行を使用して列名を変更することで、これを行うことができます。 Android および .NET 管理対象クライアントの場合は、カスタム SQL を記述して、データ オブジェクト テーブルの列の名前を変更する必要があります。

iOS の場合は、データ エンティティのコア データ スキーマを変更し、以下の内容と一致させる必要があります。 なおプロパティ `createdAt`, 、`updatedAt` と `バージョン` が不要になった、 `ms _` プレフィックス。

| 属性| 型| 注|
|---------- |  ------ | -----------------------------------------------------|
| id| 文字列、必須のマーク| リモート ストア内のプライマリ キー|
| createdAt| 日付| (省略可能) createdAt システム プロパティにマップします。|
| updatedAt| 日付| (省略可能) updatedAt システム プロパティにマップします。|
| version| String| (省略可能) 競合の検出に使用され、バージョンにマップします。|

#### システム プロパティの照会

Azure Mobile Services でシステムのプロパティが送信されないクエリ文字列を使用して要求された場合にのみ、既定では、 `__systemProperties`します。 これに対し、Azure Mobile Apps のシステム プロパティは、サーバー SDK オブジェクト モデルの一部であるため、**常に選択**されます。

この変更のカスタム実装の拡張機能などのドメインの管理者の影響を与える主に `MappedEntityDomainManager`します。 モバイル サービスでクライアントが決して、システムのプロパティを要求した場合を使用して、 `MappedEntityDomainManager` を実際にマップされないすべてのプロパティです。 ただし、Azure Mobile Apps では、これらのマップされないプロパティが原因で、GET クエリでエラーが発生します。

継承されるように、Dto を変更する、問題を解決する最も簡単な方法は、 `ITableData` の代わりに `EntityData`します。 次に、追加、 `[NotMapped]` 属性を省略する必要があるフィールドにします。

たとえば、次の定義 `TodoItem` システム プロパティを持たない。

    using System.ComponentModel.DataAnnotations.Schema;
    
    public class TodoItem : ITableData
    {
        public string Text { get; set; }
    
        public bool Complete { get; set; }
    
        public string Id { get; set; }
    
        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }
    
        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }
    
        [NotMapped]
        public bool Deleted { get; set; }
    
        [NotMapped]
        public byte[] Version { get; set; }
    }

注: エラーが発生する場合は、 `NotMapped`, 、アセンブリへの参照を追加 `System.ComponentModel.DataAnnotations`します。

### CORS

Mobile Services には、ASP.NET CORS ソリューションをラップすることによる CORS のサポートがいくつか含まれていました。 この折り返しレイヤーが直接利用して、開発者に、詳細なコントロールを与え、削除された [ASP.NET CORS のサポート](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api)します。

CORS を使用する場合は、重大な問題の主な領域が、 `eTag` と `場所` クライアント Sdk の適切に機能するためのヘッダーを許可する必要があります。

### プッシュ通知

プッシュについては、サーバー SDK に含まれていない主な項目として、PushRegistrationHandler クラスがあります。 登録処理は Mobile Apps アプリとはわずかに異なり、タグのない登録が既定で有効になっています。 カスタム API を使用してタグを管理することができます。 参照してください、 [タグに登録する](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) 詳細指示します。

### スケジュールされたジョブ

スケジュールされたジョブは、Mobile Apps には組み込まれません。そのため、.NET バックエンド内にある既存のジョブは個別にアップグレードする必要があります。 1 つのオプションでは、モバイル アプリ コード サイトで [Web ジョブ] スケジュールを作成します。 ジョブ コードを保持するコント ローラーをセットアップし、[必要なスケジュールでそのエンドポイントをヒットする [Azure スケジューラ] を構成することもできますが。

### その他の変更

モバイル クライアントでは使用ができるすべての ApiControllers が必要、 `[MobileAppController]` 属性です。 これは既定では含まれなくなったため、他の ApiController はモバイル フォーマッタによる影響を受けることはありません。

`ApiServices` オブジェクトは、SDK の一部ではありません。 Mobile App の設定にアクセスするために、次の手順を使用できます。

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings(); 

同様に、ロギングは標準の ASP.NET トレース書き込みを使用して行われるようになりました。

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

## <a name="authentication"></a>認証に関する注意点

Mobile Services の認証コンポーネントは、App Service の認証/承認機能に移動されました。 読み取ることによって、サイトに対して有効にこれについて、 [モバイル アプリへの認証の追加](app-service-mobile-ios-get-started-users.md) トピックです。

AAD、Facebook、Google などの一部のプロバイダーでは、コピー アプリケーションから既存の登録を利用できます。 ID プロバイダーのポータルに移動して、新しいリダイレクト URL を登録に追加するだけです。 その後、クライアント ID とシークレットを使用して、App Service の認証/承認を構成します。

### コントローラー アクションの認証

すべてのインスタンス、 `[AuthorizeLevel(AuthorizationLevel.User)]` 属性は、標準の ASP.NET を使用して変更できるよう `[Authorize]` 属性です。 さらに、他の ASP.NET アプリケーションと同様に、現在、コントローラーは既定では匿名になります。
管理者やアプリケーションなど、他の AuthorizeLevel オプションのいずれかを使用していた場合、これらは使用されなくなったことに注意してください。 代わりに共有シークレットに対して AuthorizationFilter をセットアップするか、サービス間呼び出しを安全に有効にするように AAD サービス プリンシパルを構成することができます。

### 追加のユーザー情報の取得

使用してアクセス トークンを含む追加のユーザー情報を取得できます、 `GetAppServiceIdentityAsync()` メソッド。

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

さらに、アプリケーションでユーザー ID をデータベースに格納しているなど、ユーザー ID と依存関係がある場合は、Mobile Services と App Service Mobile Apps のユーザー ID が異なることに注意する必要があります。 ただし、そのような場合でも Mobile Services のユーザー ID を取得することはできます。 すべての ProviderCredentials サブクラスには、UserId プロパティがあります。 ここでは、前の例を引き続き使用します。

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

ユーザー ID と依存関係がある場合は、可能であれば、ID プロバイダーへの同じ登録を使用することが重要です。 ユーザー ID は通常、使用されたアプリケーション登録に制限されるため、新しい登録を導入した場合、ユーザーとそのデータを照合する際に問題が生じる可能性があります。

### カスタム認証

アプリでカスタム認証ソリューションを使用する場合は、アップグレードされたサイトがシステムにアクセスできることを確認する必要があります。 ソリューションを統合するカスタム認証 [.NET サーバー SDK の概要] に新しい手順に従います。 カスタム認証コンポーネントはまだプレビュー段階であることに注意してください。

## <a name="updating-clients"></a>クライアントの更新

モバイル アプリ バックエンドを運用している場合は、それを利用する新しいバージョンのクライアント アプリケーションを使用できます。 Mobile Apps には新しいバージョンのクライアント SDK も含まれ、上記のサーバー アップグレードと同様に、Mobile Apps バージョンをインストールする前に Mobile Services SDK へのすべての参照を削除する必要があります。

バージョン間での主な変更の 1 つは、コンストラクターでアプリケーション キーが不要になったことです。 現在は、モバイル アプリの URL を渡すだけです。 たとえば、.NET クライアントで、 `MobileServiceClient` コンス トラクターは、今すぐ。

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

新しい SDK のインストールおよび新しい構造の使用については、以下のリンクを介して参照できます。

- [iOS バージョン 3.0.0 以降](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) バージョン 2.0.0 またはそれ以降](app-service-mobile-dotnet-how-to-use-client-library.md)

アプリケーションでプッシュ通知を使用する場合は、変更されている内容もあるため、プラットフォームごとに固有の登録手順をメモしてください。

新しいクライアント バージョンの準備ができたら、アップグレードしたサーバー プロジェクトで試してみます。 機能することを確認したら、新しいバージョンのアプリケーションを顧客にリリースできます。 最後に、顧客がこれらの更新プログラムを受け取ったら、Mobile Services バージョンのアプリを削除できます。 これで、最新の Mobile Apps サーバー SDK を使用する App Service モバイル アプリに完全にアップグレードできました。




[azure portal]: https://portal.azure.com/ 
[azure classic portal]: https://manage.windowsazure.com/ 
[what are mobile apps?]: app-service-mobile-value-prop.md 
[i already use web sites and mobile services – how does app service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services 
[mobile app server sdk]: http://www.nuget.org/packages/microsoft.azure.mobile.server 
[create a mobile app]: app-service-mobile-xamarin-ios-get-started.md 
[add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md 
[add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md 
[azure scheduler]: /en-us/documentation/services/scheduler/ 
[web job]: ../app-service-web/websites-webjobs-resources.md 
[how to use the .net server sdk]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md 
[migrate from mobile services to an app service mobile app]: app-service-mobile-migrating-from-mobile-services.md 
[migrate your existing mobile service to app service]: app-service-mobile-migrating-from-mobile-services.md 
[app service pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/ 
[.net server sdk overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md 

