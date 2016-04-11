<properties
    pageTitle="Azure AD B2C プレビュー | Microsoft Azure"
    description="Azure AD B2C を使用し、認証に OAuth 2.0 アクセス トークンを使用してセキュリティ保護された .NET Web API を構築する方法。"
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="msmbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/22/2015"
    ms.author="dastrock"/>
    
# Azure AD B2C プレビュー: .NET Web API を構築する

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure AD B2C では、OAuth 2.0 アクセス トークンを使用して Web API をセキュリティで保護しながら、Azure AD B2C を使用するクライアント アプリを API に対して認証することができます。  この記事では説明する方法
.NET MVC「to do リスト」アプリケーションを作成するユーザーのサインアップ、サインイン、およびプロファイルの管理を含みます。  タスク サービス - web API が認証によって各ユーザーの to-do リストを格納します。
ユーザーを作成し、to do リストにタスクの読み取り。

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## 1.Azure AD B2C ディレクトリの取得

Azure AD B2C を使用するには、ディレクトリ (つまり、テナント) を作成しておく必要があります。  ディレクトリは、ユーザー、アプリ、グループなどをすべて格納するためのコンテナーです。  ない場合
既に、やり [B2C ディレクトリを作成する](active-directory-b2c-get-started.md) に進む前にします。

## 2.アプリケーションの作成

次に、B2C ディレクトリ内にアプリを作成する必要があります。これによって、 アプリと安全に通信するために必要ないくつかの情報が Azure AD に提供されます。  アプリを作成するには
次の [手順](active-directory-b2c-app-registration.md)します。  このとき、

- 含める、 **web アプリ]、[web api** アプリケーションにします。
- Web アプリを使用して、 **リダイレクト Uri** `https://localhost:44316/` -このコード サンプルについては、web アプリのクライアントの既定の場所です。
- 下のコピー、 **アプリケーション ID** アプリに割り当てられています。  このプロジェクトはすぐに必要になります。

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## 3.ポリシーの作成

Azure AD の B2C によってすべてのユーザー エクスペリエンスを定義、 [**ポリシー**](active-directory-b2c-reference-policies.md)します。  このサンプル コードでクライアントには、3 つが含まれています 
id は、エクスペリエンス - サインアップ、サインイン、およびプロファイルを編集します。  」の説明に従って、各種類の 1 つのポリシーを作成する必要が、 
[ポリシーのリファレンス資料](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)します。  3 つのポリシーを作成するときは、以下の点に注意してください。

- いずれかを選択 **ユーザー ID サインアップ** または **電子メール サインアップ** 、id プロバイダー] ブレードにします。
- 選択、 **表示名** し、サインアップのポリシーでその他のいくつかのサインアップ属性です。
- 選択、 **表示名** と **オブジェクト ID** アプリケーションとしてクレームがポリシーに要求します。  その他のクレームも選択できます。
- 下のコピー、 **名前** 作成した後は、各ポリシーのです。  これらのポリシー名はすぐに必要になります。 

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

3 つのポリシーの作成が正常に完了したら、いつでもアプリをビルドできます。

## 4.コードのダウンロード

このチュートリアルのコードは保持 [github](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet)します。  移動するようにサンプルをビルドするには 
[zip としてスケルトン プロジェクトをダウンロード](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip) またはスケルトンを複製します。

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

完成したアプリケーションも [.zip として利用可能な](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) または、
`complete` 同じリポジトリの分岐。

サンプル コードをダウンロードしたら、Visual Studio の `.sln` ファイルを開いて作業を開始します。  ソリューションに `TaskWebApp` プロジェクトと `TaskService` プロジェクトの 2 つのプロジェクトがあることがわかります。   `TaskWebApp` 、MVC は、 
ユーザーと対話する web アプリケーションです。   `TaskService` はアプリのバックエンド web API を各ユーザーの to do リストに格納します。 

## 5.タスク Web アプリの構成

ユーザーが `TaskWebApp` と対話する際、クライアントは Azure AD に要求を送信し、`TaskService` Web API の呼び出しに使用できるトークンを受け取ります。
ユーザーのサインインとトークンの取得を行うためには、アプリに関する情報を `TaskWebApp` に提供する必要があります。   `TaskWebApp` プロジェクトを開き、 `web.config` ルートにあるファイル 
プロジェクトの値を置き換えると、 `<appSettings>` セクション。

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="{Enter the name of your B2C directory, e.g. contoso.onmicrosoft.com}" />
    <add key="ida:ClientId" value="{Enter the Application Id assinged to your app by the Azure portal, e.g.580e250c-8f26-49d0-bee8-1c078add1609}" />
    <add key="ida:ClientSecret" value="{Enter the Application Secret you created in the Azure portal, e.g. yGNYWwypRS4Sj1oYXd0443n}" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="[Enter your sign up policy name, e.g. b2c_1_sign_up]" />
    <add key="ida:SignInPolicyId" value="[Enter your sign in policy name, e.g. b2c_1_sign_in]" />
    <add key="ida:UserProfilePolicyId" value="[Enter your edit profile policy name, e.g. b2c_1_profile_edit" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

また、サインイン ポリシー名の指定が必要な 2 つの `[PolicyAuthorize]` デコレータもあります。   `[PolicyAuthorize]` 属性は、特定の呼び出しに使用されます。
ポリシー、ユーザーが認証を必要とするアプリのページにアクセスしようとするとします。

```C#
// Controllers\HomeController.cs

[PolicyAuthorize(Policy = "{Enter the name of your sign in policy, e.g. b2c_1_my_sign_in}")]
public ActionResult Claims()
{
```

```C#
// Controllers\TasksController.cs

[PolicyAuthorize(Policy = "{Enter the name of your sign in policy, e.g. b2c_1_my_sign_in}")]
public class TasksController : Controller
{
```

Web アプリのようにする方法について説明する場合、 `TaskWebApp` Azure AD B2C、チェック アウトを使用して、
[Web アプリ記号で作業の開始の記事](active-directory-b2c-devquickstarts-web-dotnet.md)します。

## 6.API のセキュリティ保護

ユーザーの代わりに API を呼び出すクライアントが用意できたので、OAuth 2.0 ベアラー トークンを使用して `TaskService` をセキュリティで保護することができます。  API を受け入れ、検証 
Microsoft の OWIN ライブラリを使用してトークンです。

#### OWIN をインストールする
まず、OWIN OAuth 認証パイプラインをインストールします。

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

#### B2C の詳細情報を入力する
`TaskService` プロジェクトのルートにある `web.config` ファイルを開き、`<appSettings>` セクションの値を置き換えます。  API ではこれらの値を使用し、
OWIN ライブラリです。

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/{1}/{2}?p={3}" />
    <add key="ida:Tenant" value="{Enter the name of your B2C tenant - it usually looks like constoso.onmicrosoft.com}" />
    <add key="ida:ClientId" value="{Enter the Application ID assigned to your app by the Azure Portal}" />
    <add key="ida:PolicyId" value="{Enter the name of one of the policies you created, like `b2c_1_my_sign_in_policy`}" />
</appSettings> 
```

#### OWIN Startup クラスを追加する
OWIN Startup クラスを `TaskService` プロジェクト (`Startup.cs`) に追加します。  プロジェクトを右クリックの--> **追加** --> **[新しい項目の** "OWIN"を検索します。
  

```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

#### OAuth 2.0 認証を構成する
`App_Start\Startup.Auth.cs` ファイルを開いて、`ConfigureAuth(...)` メソッドを実装します。

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string commonPolicy = ConfigurationManager.AppSettings["ida:PolicyId"];
    private const string discoverySuffix = ".well-known/openid-configuration";

    public void ConfigureAuth(IAppBuilder app)
    {   
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
        };

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {   
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, "v2.0", discoverySuffix, commonPolicy)))
        });
    }
}
```

#### タスク コントローラーをセキュリティで保護する
OAuth 2.0 認証を使用するようにアプリが構成されたので、Web API をセキュリティで保護するために必要なのは、`[Authorize]` タグをタスク コントローラーに追加することだけです。
このコントローラーでは To Do リストのすべての操作が実行されるため、コントローラー全体をクラス レベルでセキュリティ保護します。
よりきめ細かく制御するために、`[Authorize]` タグを個々のアクションに追加することもできます。

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

#### トークンからユーザー情報を取得する
`TaskController` はデータベースにタスクを格納します。このデータベースでは、各タスクに、そのタスクを "所有" するユーザーが関連付けられています。  所有者は、ユーザーが識別 **オブジェクト ID** (
オブジェクト ID をアプリケーションとして追加する必要がある。 理由は、すべてのポリシー内のクレーム)。

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## 7.サンプル アプリの実行

最後に、`TaskWebApp` と `TaskService` の両方をビルドして実行します。  電子メール アドレスまたはユーザー名を使用して、アプリにサインアップします。  [ユーザーの to-do リストでは、いくつかのタスクを作成します。
これらの保存方法、API で停止し、クライアントを再起動した後でもに注目してください。

## 8.ポリシーの編集

Azure AD B2C を使用して API をセキュリティで保護したので、アプリのポリシーでさまざまな設定を試し、その効果 (または不足部分) を API で確認することができます。  Visual Studio Online アカウントは <!--add **identity providers**
to the policies, allowing you users to sign into the Task Client using social accounts.  You can also -->操作、 **アプリケーション クレーム** ポリシー、および変更
Web API で提供されるユーザーの情報。  その他のクレームを追加した場合、そのクレームは、前述のように .NET MVC Web API の `ClaimsPrincipal` オブジェクト内で利用できます。

<!--

## Next Steps

You can now move onto more advanced B2C topics.  You may want to try:

[Calling a Web API from a Web App >>]()

[Customizing the your B2C App's UX >>]()

-->


