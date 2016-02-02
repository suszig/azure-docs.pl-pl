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

Azure AD B2C では、OAuth 2.0 アクセス トークンを使用して Web API をセキュリティで保護しながら、Azure AD B2C を使用するクライアント アプリを API に対して認証することができます。 この記事では説明する方法
.NET MVC「to do リスト」アプリケーションを作成するユーザーのサインアップ、サインイン、およびプロファイルの管理を含みます。 タスク サービス - web API が認証によって各ユーザーの to-do リストを格納します。
ユーザーを作成し、to do リストにタスクの読み取り。

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## 1.Azure AD B2C ディレクトリの取得

Azure AD B2C を使用するには、ディレクトリ (つまり、テナント) を作成しておく必要があります。 ディレクトリは、ユーザー、アプリ、グループなどをすべて格納するためのコンテナーです。 ない場合
既に、やり [B2C ディレクトリを作成する](active-directory-b2c-get-started.md) に進む前にします。

## 2.アプリケーションの作成

次に、B2C ディレクトリ内にアプリを作成する必要があります。これによって、アプリと安全に通信するために必要ないくつかの情報が Azure AD に提供されます。 アプリを作成するには
次の [手順](active-directory-b2c-app-registration.md)します。 このとき、

- アプリケーションに **Web アプリまたは Web API** を含めます。
- Web アプリを使用して、 **リダイレクト Uri** `https://localhost:44316/` -このコード サンプルについては、web アプリのクライアントの既定の場所です。
- アプリに割り当てられた**アプリケーション ID** をメモしておきます。 このプロジェクトはすぐに必要になります。

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## 3.ポリシーの作成

Azure AD の B2C によってすべてのユーザー エクスペリエンスを定義、 [* * * * ポリシー](active-directory-b2c-reference-policies.md)します。 このサンプル コードでクライアントには、3 つが含まれています 
id は、エクスペリエンス - サインアップ、サインイン、およびプロファイルを編集します。 」の説明に従って、各種類の 1 つのポリシーを作成する必要が、 
[ポリシーのリファレンス資料](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)します。 3 つのポリシーを作成するときは、必ず以下の操作を行ってください。

- ID プロバイダーのブレードで、**[ユーザー ID サインアップ]** または **[電子メール サインアップ]** を選択します。
- サインアップ ポリシーで、**表示名**と他のいくつかのサインアップ属性を選択します。
- すべてのポリシーで、アプリケーション クレームとして **表示名**と**オブジェクト ID** を選択します。 その他の要求も選択できます。
- ポリシーの作成後、各ポリシーの**名前**をメモしておきます。 これらのポリシー名はすぐに必要になります。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

3 つのポリシーの作成が正常に完了したら、いつでもアプリをビルドできます。

## 4.コードのダウンロード

このチュートリアルのコードは保持 [github](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet)します。 移動するようにサンプルをビルドするには 
[.zip としてスケルトン プロジェクトをダウンロード](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip) またはスケルトンを複製します。

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

完成したアプリケーションも [.zip として利用可能な](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) または、
`完了` 同じリポジトリの分岐します。

サンプル コードをダウンロードした後は、Visual Studio を開き `.sln` ファイルを開始します。 ソリューションに 2 つのプロジェクトがあることがわかります。、 `TaskWebApp` プロジェクトと `TaskService` プロジェクトです。  `TaskWebApp` 、MVC は、 
ユーザーと対話する web アプリケーションです。  `TaskService` はアプリのバックエンド web API を各ユーザーの to do リストに格納します。

## 5.タスク Web アプリの構成

ユーザーが操作すると、 `TaskWebApp`, 、クライアントが Azure AD に要求を送信しは通話に使用できるバックのトークンを受け取ります、 `TaskService` web API です。
ユーザーをサインインし、トークンを取得するために必要な `TaskWebApp` アプリに関する情報を使用します。  `TaskWebApp` プロジェクトを開き、 `web.config` ルートにあるファイル 
プロジェクトの値を置き換えると、 `< appSettings >` セクション。

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

2 つ `[PolicyAuthorize]` デコレータのサインインでポリシー名を入力する必要があります。  `[PolicyAuthorize]` 属性は、特定の呼び出しに使用されます。
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
[Web アプリ記号の概要記事](active-directory-b2c-devquickstarts-web-dotnet.md)します。

## 6.API のセキュリティ保護

ある場合、ユーザーに代わって、API を呼び出すクライアントをセキュリティを強化できます、 `TaskService` OAuth 2.0 ベアラー トークンを使用します。 API を受け入れ、検証 
Microsoft の OWIN ライブラリを使用してトークンです。

#### OWIN をインストールする

まず、OWIN OAuth 認証パイプラインをインストールします。

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

#### B2C の詳細情報を入力する

開いている、 `web.config` のルートにファイル、 `TaskService` プロジェクトし、の値を置き換える、 `< appSettings >` セクションです。API ではこれらの値を使用し、
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

OWIN Startup クラスを追加、 `TaskService` というプロジェクト `Startup.cs`します。 プロジェクトを右クリックし、**[追加]**、**[新しいアイテム]** の順にクリックして、"OWIN" を検索します。


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

ファイルを開く `App_Start\Startup.Auth.cs`, を実装し、 `ConfigureAuth(...)` メソッド。

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

これで、アプリケーションは、OAuth 2.0 認証を使用する構成は、web API をセキュリティで保護するために必要なは追加、 `[Authorize]` タスク コント ローラーへのタグ。
このコントローラーでは To Do リストのすべての操作が実行されるため、コントローラー全体をクラス レベルでセキュリティ保護します。
追加することで、 `[Authorize]` タグより細かい制御を個々 のアクションにします。

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

#### トークンからユーザー情報を取得する

`TaskController` タスクを「所有」する関連付けられたユーザーは、データベース内にタスクが格納されます。 所有者は、ユーザーが識別 **オブジェクト ID** (
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

最後に、ビルドし、両方を `TaskWebApp` と `TaskService`します。 電子メール アドレスまたはユーザー名を使用して、アプリにサインアップします。 [ユーザーの to-do リストでは、いくつかのタスクを作成します。
これらの保存方法、API で停止し、クライアントを再起動した後でもに注目してください。

## 8.ポリシーの編集

Azure AD B2C を使用して API をセキュリティで保護したので、アプリのポリシーでさまざまな設定を試し、その効果 (または不足部分) を API で確認することができます。 操作することができます、 **アプリケーション クレーム** では、ポリシー、および変更
Web API で提供されるユーザーの情報。 追加する追加のクレームができるように .NET MVC web api で、 `ClaimsPrincipal` オブジェクト、前述のとおりです。







