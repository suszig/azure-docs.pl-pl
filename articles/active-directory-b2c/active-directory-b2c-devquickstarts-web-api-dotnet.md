<properties
    pageTitle="Azure AD B2C プレビュー | Microsoft Azure"
    description="Azure AD B2C を使用して Web API を呼び出す Web アプリケーションを構築する方法。"
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

# Azure AD B2C プレビュー: .NET Web アプリから Web API を呼び出す

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure AD B2C を使用すると、簡単な手順で強力なセルフサービスの ID 管理機能を Web アプリと Web API に追加できます。  この記事では説明する方法
.NET を呼び出す「to do リスト」web アプリケーションは、OAuth 2.0 ベアラー トークンを使用して API を web を .NET MVC を作成します。  Web アプリと web api を使用してユーザー id を管理する Azure AD B2C の両方
ユーザーを認証します。

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

この記事では、Azure AD B2C でサインイン、サインアップ、プロファイルの管理を実装する方法については説明しません。  ユーザーが既に認証された後での Web API の呼び出しに焦点を合わせています。
まだインストールしていない場合、これを最初にください、 [.NET Web アプリケーション チュートリアル入門](active-directory-b2c-devquickstarts-web-dotnet.md) Azure AD B2C の基本について説明します。

## 1.Azure AD B2C ディレクトリの取得

Azure AD B2C を使用するには、ディレクトリ (つまり、テナント) を作成しておく必要があります。  ディレクトリは、ユーザー、アプリ、グループなどをすべて格納するためのコンテナーです。  ない場合
既に、やり [B2C ディレクトリを作成する](active-directory-b2c-get-started.md) に進む前にします。

## 2.アプリケーションの作成

ここで、B2C ディレクトリにアプリを作成する必要があります。このディレクトリによって、アプリと安全に通信するために必要ないくつかの情報を Azure AD に提供します。  Web アプリと
web API は、1 つで表されます **アプリケーション ID** ここでは、1 つの論理アプリケーションを構成からです。  アプリを作成するには
次の [手順](active-directory-b2c-app-registration.md)します。  このとき、

- 含める、 **web アプリや web api** アプリケーションで
- 入力 `https://localhost:44316/` として、 **応答 URL** -このコード サンプルの既定の URL であります。
- 作成、 **アプリケーション シークレット** 、アプリケーションをコピーします。  このプロジェクトはすぐに必要になります。
- 下のコピー、 **アプリケーション ID** アプリに割り当てられています。  こちらもすぐに必要になります。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## 3.ポリシーの作成

Azure AD の B2C によってすべてのユーザー エクスペリエンスを定義、 [**ポリシー**](active-directory-b2c-reference-policies.md)します。  この web アプリには、3 つが含まれています 
id は、エクスペリエンス - サインアップ、サインイン、およびプロファイルを編集します。  」の説明に従って、各種類の 1 つのポリシーを作成する必要が、 
[ポリシーのリファレンス資料](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)します。  3 つのポリシーを作成するときは、以下の点に注意してください。

- 選択、 **表示名** し、サインアップのポリシーでその他のいくつかのサインアップ属性です。
- 選択、 **表示名** と **オブジェクト ID** すべてのポリシーでアプリケーションの要求。  その他のクレームも選択できます。
- 下のコピー、 **名前** 作成した後は、各ポリシーのです。  名前には、`b2c_1_` というプレフィックスが付加されています。  これらのポリシー名はすぐに必要になります。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)] 

3 つのポリシーの作成が正常に完了したら、いつでもアプリをビルドできます。

この記事では、作成したポリシーの使用方法については説明しません。  Azure AD の B2C でポリシーのしくみについて学習する場合
まず、 [.NET Web アプリケーション チュートリアル入門](active-directory-b2c-devquickstarts-web-dotnet.md)します。

## 4.コードのダウンロード

このチュートリアルのコードは保持 [github](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet)します。  移動するようにサンプルをビルドするには 
[zip としてスケルトン プロジェクトをダウンロード](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip) またはスケルトンを複製します。

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

完成したアプリケーションも [.zip として利用可能な](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip) または、
`complete` 同じリポジトリの分岐。

サンプル コードをダウンロードしたら、Visual Studio の `.sln` ファイルを開いて作業を開始します。  ソリューションに `TaskWebApp` プロジェクトと `TaskService` プロジェクトの 2 つのプロジェクトがあることがわかります。   `TaskWebApp` 、WPF は、 
web アプリのユーザーと対話するフロント エンドです。  `TaskService` は、各ユーザーの To-Do リストを格納する、アプリのバックエンド Web API です。 

## 5.タスク サービスを構成する

`TaskService` は `TaskWebApp` から要求を受け取ると、要求を認証するための有効なアクセス トークンを確認します。  アクセス トークンを検証するために 
必要な `TaskService` アプリに関する情報を使用します。   `TaskService` プロジェクトを開き、 `web.config` ルートにあるファイル 
プロジェクトの値を置き換えると、 `<appSettings>` セクション。

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
  
[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


この記事では、`TaskService` のセキュリティ保護の詳細は説明しません。  Web API が安全に Azure AD B2C を使用して要求を認証する方法について学習するには、チェック アウト、
[Web API の概要記事](active-directory-b2c-devquickstarts-api-dotnet.md)します。

## 6.タスク Web アプリの構成

`TaskWebApp` が Azure AD B2C と通信するために、指定する必要がある一般的なパラメーターがいくつかあります。   `TaskWebApp` プロジェクトを開き、
`web.config` プロジェクトのルートにファイルし、の値を置き換える、 `<appSettings>` セクションです。  これらの値は、Web アプリ全体で使用されます。

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
    <add key="ida:SignUpPolicyId" value="[Enter your sign up policy name, e.g.g b2c_1_sign_up" />
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

## 7.アクセス トークンの取得とタスク API の呼び出し

このセクションでは、Microsoft のライブラリとフレームワークを使用して、Web アプリで OAuth 2.0 トークンの交換を完了する方法を示します。  する場合
馴染みのない **認証コード** と **アクセス トークン**, に目を通しことをお勧めがある可能性があります、 [OpenID Connect プロトコル リファレンス](active-directory-b2c-reference-protocols.md)します。

#### 承認コードを取得する

通話の最初の手順、 `TaskService` web API がユーザーの認証を送受信するには、 **認証コード** Azure AD からです。
プロファイル ポリシーの編集、サインイン、サインアップなど、何らかのポリシーが正常に実行された後に Azure AD から承認コードを受信できます。

開始するには、Visual Studio パッケージ マネージャー コンソールを使用して OWIN OpenID Connect ミドルウェアをインストールします。 OWIN を使用して、送信認証
Azure AD に要求し、その応答を処理します。

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TaskWebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TaskWebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskWebApp
```

ファイル `App_Start\Startup.Auth.cs` を開きます。  これは、構成、OWIN 認証パイプライン B2C ディレクトリの詳細情報を提供
作成したアプリケーション:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    public const string AcrClaimType = "http://schemas.microsoft.com/claims/authnclassreference";
    public const string PolicyKey = "b2cpolicy";
    public const string OIDCMetadataSuffix = "/.well-known/openid-configuration";

    // App config settings
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string clientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SignUpPolicyId = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string SignInPolicyId = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string ProfilePolicyId = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        OpenIdConnectAuthenticationOptions options = new OpenIdConnectAuthenticationOptions
        {
            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = OnAuthenticationFailed,
                RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                AuthorizationCodeReceived = OnAuthorizationCodeReceived,
            },
            Scope = "openid offline_access",

            // The PolicyConfigurationManager takes care of getting the correct Azure AD authentication
            // endpoints from the OpenID Connect metadata endpoint.  It is included in the PolicyAuthHelpers folder.
            ConfigurationManager = new PolicyConfigurationManager(
                String.Format(CultureInfo.InvariantCulture, aadInstance, tenant, "/v2.0", OIDCMetadataSuffix),
                new string[] { SignUpPolicyId, SignInPolicyId, ProfilePolicyId }),

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new System.IdentityModel.Tokens.TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };

        app.UseOpenIdConnectAuthentication(options);
    }
    ...
}
```

#### 承認コードを使用したアクセス トークンの取得

Web アプリは、B2C のディレクトリを使用してユーザーを認証し、Azure AD からの承認コードを受信するように構成されました。
次の手順では、Azure AD からのアクセス トークン用にこの承認コードを交換します。 

.NET web アプリは、Azure AD からアクセス トークンを取得する必要がある、際に、使用、 **Active Directory 認証ライブラリ (ADAL)**します。  
このプロセスでは、ADAL を使用する必要はありませんが、ADAL しやすく担当 OAuth を送信するなど、多くの詳細
2.0 認証メッセージには、キャッシュとトークンを更新します。

最初に、もう一度パッケージ マネージャー コンソールを使用して ADAL を `TaskWebApp` プロジェクトにインストールします。

```
PM> Install-Package Microsoft.Experimental.IdentityModel.Clients.ActiveDirectory -ProjectName TaskWebApp -IncludePrerelease
```

今回は、承認コードを ADAL に渡してトークンを取得できるようにする必要があります。  OWIN OpenID Connect ミドルウェアは、通知を提供します。
-この認証コードを使用する通知をアプリが Azure AD から認証コードを受信するたびに発生させるされます。
`App_Start\Startup.Auth.cs` で、ADAL を使用して `OnAuthorizationCodeReceived` 通知ハンドラーを実装します。

```C#
// App_Start\Startup.Auth.cs

private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
    // The user's objectId is extracted from the claims provided in the id_token, and used to cache tokens in ADAL
    // The authority is constructed by appending your B2C directory's name to "https://login.microsoftonline.com/"
    // The client credential is where you provide your application secret, and is used to authenticate the application to Azure AD
    string userObjectID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant, string.Empty, string.Empty);
    ClientCredential credential = new ClientCredential(clientId, clientSecret);

    // We don't care which policy is used to access the TaskService, so let's use the most recent policy as indicated in the sign-in token
    string mostRecentPolicy = notification.AuthenticationTicket.Identity.FindFirst(Startup.AcrClaimType).Value;

    // The Authentication Context is ADAL's primary class, which represents your connection to your B2C directory
    // ADAL uses an in-memory token cache by default.  In this case, we've extended the default cache to use a simple per-user session cache
    AuthenticationContext authContext = new AuthenticationContext(authority, new NaiveSessionCache(userObjectID));

    // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
    // The token will be stored in the ADAL token cache, for use in our controllers
    AuthenticationResult result = await authContext.AcquireTokenByAuthorizationCodeAsync(notification.Code, new Uri(redirectUri), credential, new string[] { clientId }, mostRecentPolicy);
}
``` 

#### コントローラーでのアクセス トークンの取得

`TaskService` バックエンド用のアクセス トークンを取得し、ADAL のトークン キャッシュに保存しました。次は、これを実際に使用する必要があります。
 `TasksController` と通信を行うには、 `TaskService` を読み取ったりを作成するには、API、および API への HTTP 要求の送信
およびタスクを削除します。  HTTP 要求を送信する前に、ADAL からアクセス トークンを取得します。

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    AuthenticationResult result = null;
    try
    {
        string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, Startup.tenant, string.Empty, string.Empty);
        ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

        // We don't care which policy is used to access the TaskService, so let's use the most recent policy
        string mostRecentPolicy = ClaimsPrincipal.Current.FindFirst(Startup.AcrClaimType).Value;
        
        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        // AcquireTokenSilentAsync will return a token from the token cache, and throw an exception if it cannot do so.
        AuthenticationContext authContext = new AuthenticationContext(authority, new NaiveSessionCache(userObjectID));
        result = await authContext.AcquireTokenSilentAsync(new string[] { Startup.clientId }, credential, UserIdentifier.AnyUser, mostRecentPolicy);

        ...
    }
    catch (AdalException ee)
    {
        // If ADAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
    }
    ...
}
``` 

ADAL では、トークンのキャッシング、トークンの有効期限が切れたときの更新、例外をスローしてもう一度サインインする必要がある場合の通知を処理します。  すべて
必要な操作を呼び出すことです `AuthenticationContext.AcquireTokenSilentAsync(...)` アプリで、トークンを必要がある場合。

#### Web API からのタスク読み取り

トークンを使用できるようになったので、`Authorization` ヘッダーの HTTP GET 要求にトークンを添付して、`TaskService` を安全に呼び出すことができます。

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    ... 
    
    try 
    {
        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, then drop the current access token from the cache, 
            // and show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                var todoTokens = authContext.TokenCache.ReadItems().Where(a => a.Scope.Contains(Startup.clientId));
                foreach (TokenCacheItem tci in todoTokens)
                    authContext.TokenCache.DeleteItem(tci);

                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

#### Web API でのタスクの作成および削除

POST 要求と DELETE 要求を `TaskService` に送信するときにも、まったく同じパターンを使用できます。  呼び出すだけで `AuthenticationContext.AcquireTokenSilentAsync(...)`,、
結果として得られるトークンで要求を追加し、 `Authorization` ヘッダー。  ここでは、`Create` アクションが既に実装されています。  Try 仕上げ、 `Delete` 内のアクション
`TasksController.cs` 自分でします。

## 8.ユーザーのサインアウト

最後に、詳細な確認事項が 1 つあります。  ユーザーが Web アプリからサインアウトしたら、ADAL トークン キャッシュをクリアして、ユーザーの認証済みセッションの形跡をすべて削除する必要があります。

```C#
// Controllers/AccountController.cs

public void SignOut()
{
    if (Request.IsAuthenticated)
    {
        // When the user signs out, clear their token cache in the process
        string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, Startup.tenant, string.Empty, string.Empty);
        AuthenticationContext authContext = new AuthenticationContext(authority, new NaiveSessionCache(userObjectID));
        authContext.TokenCache.Clear();

        HttpContext.GetOwinContext().Authentication.SignOut(
        new AuthenticationProperties(
            new Dictionary<string, string> 
            { 
                {Startup.PolicyKey, ClaimsPrincipal.Current.FindFirst(Startup.AcrClaimType).Value}
            }), OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
    }
}
```

## 9.サンプル アプリを実行する

最後に、`TaskClient` と `TaskService` の両方をビルドして実行します。  アプリにサインアップまたはサインインし、サインインしているユーザーのタスクを作成します。  サインアウトして、別のユーザーとしてもう一度サインインし、そのユーザーのタスクを作成します。
API でタスクがユーザーごとに保存されたことを確認します。これは、API が、受信したアクセス トークンからユーザーID を抽出したためです。

リファレンスについては、完全なサンプルは [.zip をここでは現状](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip),、
または、GitHub からプロジェクトを複製することができます。

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## 次のステップ

さらに高度な B2C のトピックに進むことができます。  次のチュートリアルを試してみてください。

[Web アプリから Web API を呼び出す >>]()

[B2C アプリの UX のカスタマイズ]()

-->
