<properties
    pageTitle="Azure AD B2C プレビュー | Microsoft Azure"
    description="Azure AD B2C を使用し、サインイン、サインアップ、プロファイル管理を備えた Web アプリケーションを構築する方法。"
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

# Azure AD B2C プレビュー: .NET Web アプリを構築する

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure AD B2C を使用すると、強力なセルフサービス方式の ID 管理機能を、わずかな手順で Web アプリに追加できます。  この記事では説明する方法
.NET MVC web アプリを作成するユーザーのサインアップ、サインイン、およびプロファイルの管理を含みます。  サインアップとサインインで使用するユーザー名のサポートが含まれる
または、電子メールだけでなく Facebook や Google などのソーシャル アカウントです。

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## 1.Azure AD B2C ディレクトリの取得

Azure AD B2C を使用するには、ディレクトリ (つまり、テナント) を作成しておく必要があります。  ディレクトリは、ユーザー、アプリ、グループなどをすべて格納するためのコンテナーです。  ない場合
既に、やり [B2C ディレクトリを作成する](active-directory-b2c-get-started.md) に進む前にします。

## 2.アプリケーションの作成

ここで、B2C ディレクトリにアプリを作成する必要があります。このディレクトリによって、アプリと安全に通信するために必要ないくつかの情報を Azure AD に提供します。  アプリを作成するには
次の [手順](active-directory-b2c-app-registration.md)します。  このとき、

- 含める、 **web アプリや web api** アプリケーションで
- 入力 `https://localhost:44316/` として、 **応答 URL** -このコード サンプルの既定の URL であります。
- 下のコピー、 **アプリケーション ID** アプリに割り当てられています。  このプロジェクトはすぐに必要になります。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## 3.ポリシーの作成

Azure AD の B2C によってすべてのユーザー エクスペリエンスを定義、 [**ポリシー**](active-directory-b2c-reference-policies.md)します。  このコード サンプルでは、3 つ含まれています 
id は、エクスペリエンス - サインアップ、サインイン、およびプロファイルを編集します。  」の説明に従って、各種類の 1 つのポリシーを作成する必要が、 
[ポリシーのリファレンス資料](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)します。  3 つのポリシーを作成するときは、以下の点に注意してください。

- 選択 **ユーザー ID サインアップ** または **電子メール サインアップ** 、id プロバイダー] ブレードにします。
- 選択、 **表示名** し、サインアップのポリシーでその他のいくつかのサインアップ属性です。
- 選択、 **表示名** 要求をすべてのポリシー内のアプリケーション要求として。  その他のクレームも選択できます。
- 下のコピー、 **名前** 作成した後は、各ポリシーのです。  これらのポリシー名はすぐに必要になります。 

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

3 つのポリシーの作成が正常に完了したら、いつでもアプリをビルドできます。

## 4.コードのダウンロードと認証の構成

このサンプルのコードを維持 [github](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet)します。  移動するようにサンプルをビルドするには 
[zip としてスケルトン プロジェクトをダウンロード](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) またはスケルトンを複製します。

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet.git
```

完成したサンプルも [.zip として利用可能な](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/complete.zip) または、
`complete` 同じリポジトリの分岐。

サンプル コードをダウンロードしたら、Visual Studio の `.sln` ファイルを開いて作業を開始します。

アプリは、HTTP 認証要求を送信することで、Azure AD B2C と通信します。このとき、要求の一環として実行するポリシーを指定します。
.NET web アプリケーションの場合は、OpenID Connect 認証要求を送信する、ポリシーを実行する、管理ユーザーの Microsoft の OWIN ライブラリを使用できます。
セッション、およびなどです。

まず、Visual Studio のパッケージ マネージャー コンソールを使用して、OWIN ミドルウェア NuGet パッケージをプロジェクトに追加します。

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

次に、プロジェクトのルートにある `web.config` ファイルを開いて、アプリの構成値を `<appSettings>` セクションに入力します。

```
<configuration>
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="[Enter the name of your B2C directory, e.g. contoso.onmicrosoft.com]" /> 
    <add key="ida:ClientId" value="[Enter the Application Id assinged to your app by the Azure portal, e.g.580e250c-8f26-49d0-bee8-1c078add1609]" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="[Enter your sign up policy name, e.g. b2c_1_sign_up]" />
    <add key="ida:SignInPolicyId" value="[Enter your sign in policy name, e.g. b2c_1_sign_in]" />
    <add key="ida:UserProfilePolicyId" value="[Enter your edit profile policy name, e.g. b2c_1_profile_edit]" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

ここで、"OWIN Startup" クラスをプロジェクト (`Startup.cs`) に追加します。  プロジェクトを右クリックの--> **追加** --> **[新しい項目の** "OWIN"を検索します。  
クラス宣言を `public partial class Startup` に変更します。このクラスの一部は別ファイルで実装済みです。  
OWIN ミドルウェアを呼び出す、 `Configuration(...)` メソッドでこのメソッドで、アプリケーションの起動時には、ConfigureAuth(...)、呼び出しを行う場所お
アプリの認証を設定します。

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

ファイル `App_Start\Startup.Auth.cs` を開いて、`ConfigureAuth(...)` メソッドを実装します。  提供されたパラメーター `OpenIdConnectAuthenticationOptions` 
Azure AD と通信するため、アプリの調整として使用されます。  Cookie の認証]-[OpenID Connect ミドルウェアを設定する必要も 
特に、ユーザー セッションを維持するために cookie を使用します。

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // The ACR claim is used to indicate which policy was executed
    public const string AcrClaimType = "http://schemas.microsoft.com/claims/authnclassreference";
    public const string PolicyKey = "b2cpolicy";
    public const string OIDCMetadataSuffix = "/.well-known/openid-configuration";

    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

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
                AuthenticationFailed = AuthenticationFailed,
                RedirectToIdentityProvider = OnRedirectToIdentityProvider,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // The PolicyConfigurationManager takes care of getting the correct Azure AD authentication
            // endpoints from the OpenID Connect metadata endpoint.  It is included in the PolicyAuthHelpers folder.
            // The first parameter is the metadata URL of your B2C directory
            // The second parameter is an array of the policies that your app will use.
            ConfigurationManager = new PolicyConfigurationManager(
                String.Format(CultureInfo.InvariantCulture, aadInstance, tenant, "/v2.0", OIDCMetadataSuffix),
                new string[] { SignUpPolicyId, SignInPolicyId, ProfilePolicyId }),

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {  
                NameClaimType = "name",
            },
        };

        app.UseOpenIdConnectAuthentication(options);
            
    }
```

## 5.Azure AD への認証要求の送信
ここまでの手順で、アプリは OpenID Connect 認証プロトコルを使用して Azure AD B2C と通信するよう適切に構成されています。  OWIN がすべて、煩わしいの注意します。
認証メッセージの構築、Azure AD からトークンを検証し、ユーザー セッションを維持します。  残りは、各ユーザー フローを開始する処理です。

ユーザーが Web アプリの [Sign Up]、[Sign In]、または [Edit Profile] ボタンをクリックした場合、関連付けられているアクションが `Controllers\AccountController.cs` で呼び出されます。
いずれの場合にも、組み込みの OWIN メソッドを使用して適切なポリシーをトリガーできます。

```C#
// Controllers\AccountController.cs

public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by adding it to the AuthenticationProperties using the PolicyKey provided.
    
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties (
                new Dictionary<string, string> 
                { 
                    {Startup.PolicyKey, Startup.SignInPolicyId}
                })
            { 
                RedirectUri = "/", 
            }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

public void SignUp()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties(
                new Dictionary<string, string> 
                { 
                    {Startup.PolicyKey, Startup.SignUpPolicyId}
                })
            {
                RedirectUri = "/",
            }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}


public void Profile()
{
    if (Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties(
                new Dictionary<string, string> 
                { 
                    {Startup.PolicyKey, Startup.ProfilePolicyId}
                })
            {
                RedirectUri = "/",
            }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}
```

また、コントローラーでカスタム `PolicyAuthorize` タグを使用して、ユーザーがサインイン済みでない場合に特定のポリシーを実行するように要求することもできます。
`Controllers\HomeController.cs` を開いて、`[PolicyAuthorize]` タグを Claims コントローラーに追加します。  含まれているポリシーの例を置き換えてください。
サインインがある独自ポリシーです。

```C#
// Controllers\HomeController.cs

// You can use the PolicyAuthorize decorator to execute a certain policy if the user is not already signed into the app.
[PolicyAuthorize(Policy = "b2c_1_sign_in")]
public ActionResult Claims()
{
  ...
```

アプリからユーザーをサインアウトさせる場合にも、OWIN を使用できます。  `Controllers\AccountController.cs` に戻ります。  

```C#
// Controllers\AccountController.cs

public void SignOut()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request using the last policy that the user executed.
    // This is as easy as looking up the current value of the ACR claim, adding it to the AuthenticationProperties, and making an OWIN SignOut call.

    HttpContext.GetOwinContext().Authentication.SignOut(
        new AuthenticationProperties(
            new Dictionary<string, string> 
            { 
                {Startup.PolicyKey, ClaimsPrincipal.Current.FindFirst(Startup.AcrClaimType).Value}
            }), OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

既定では、OWIN は、`AuthenticationProperties` で指定されたポリシーを Azure AD に送信しません。  ただし、OWIN が `RedirectToIdentityProvider` 通知内に生成する要求を編集することができます。
この通知を `App_Start\Startup.Auth.cs` で使用し、ポリシーのメタデータから各ポリシーの適切なエンドポイントを取得します。  これにより、アプリで実行する必要があるポリシーごとに、適切な要求が Azure AD に送信されるようになります。   

```C#
// App_Start\Startup.Auth.cs

private async Task OnRedirectToIdentityProvider(RedirectToIdentityProviderNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    PolicyConfigurationManager mgr = notification.Options.ConfigurationManager as PolicyConfigurationManager;
    if (notification.ProtocolMessage.RequestType == OpenIdConnectRequestType.LogoutRequest)
    {
        OpenIdConnectConfiguration config = await mgr.GetConfigurationByPolicyAsync(CancellationToken.None, notification.OwinContext.Authentication.AuthenticationResponseRevoke.Properties.Dictionary[Startup.PolicyKey]);
        notification.ProtocolMessage.IssuerAddress = config.EndSessionEndpoint;
    }
    else
    {
        OpenIdConnectConfiguration config = await mgr.GetConfigurationByPolicyAsync(CancellationToken.None, notification.OwinContext.Authentication.AuthenticationResponseChallenge.Properties.Dictionary[Startup.PolicyKey]);
        notification.ProtocolMessage.IssuerAddress = config.AuthorizationEndpoint;
    }
}
``` 

## 6.ユーザー情報を表示する
Azure AD が、id_token をアプリケーションが含まれていますが、返す OpenID Connect を持つユーザーを認証するときに **クレーム**, 、またはユーザーについてのアサーションです。  これらの要求を使用して、アプリを個人向けにカスタマイズすることができます。  

`Controllers\HomeController.cs` ファイルを開きます。  `ClaimsPrincipal.Current` セキュリティ プリンシパル オブジェクトを介して、コントローラー内のユーザー クレームにアクセスできます。

```C#
// Controllers\HomeController.cs

[PolicyAuthorize(Policy = "b2c_1_sign_in")]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

同じ方法で、アプリケーションが受け取るすべての要求にアクセスできます。  確認用に、アプリが受け取るすべての要求の一覧が Claims ページに出力されています。

## 7.サンプル アプリの実行

最後に、アプリを構築して実行します。   電子メール アドレスまたはユーザー名を使用して、アプリにサインアップします。  サインアウトし、同じユーザーとしてもう一度サインインします。  そのユーザーのプロファイルを編集します。  サインアウトし、サインアップします。
別のユーザーを使用します。  情報を表示する方法に注意してください、 **クレーム** ] タブは、ポリシーに構成されている情報に対応します。

## 8.ソーシャル IDP を追加する

現時点では、アプリケーションは、ユーザーがサインアップ (&) でサインインと呼ばれるもののみをサポート **ローカル アカウント** -アカウントのユーザー名とパスワードを使用した B2C ディレクトリに保存します。  Azure AD B2C と
その他のサポートを追加する **id プロバイダー**, 、またはすべてのコードを変更することがなく、Idp します。

ソーシャル IDP をアプリに追加するには、まず、次の 1 つまたは 2 つの記事に記載されている詳細な手順に従います。  サポートする各 IDP のアプリケーションを登録する必要があります。
自分のシステムとクライアント ID を取得します。

- [Facebook を IDP として設定する](active-directory-b2c-setup-fb-app.md)
- [Google を IDP として設定する](active-directory-b2c-setup-goog-app.md)
- [Amazon を IDP として設定する](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn を IDP として設定する](active-directory-b2c-setup-li-app.md) 

B2C ディレクトリに id プロバイダーを追加したときに、それぞれ、3 つのポリシーに含める新しい Idp を編集する必要があります。
」の説明に従って、 [ポリシーのリファレンス資料](active-directory-b2c-reference-policies.md)します。  ポリシーを保存したら、もう一度アプリを実行するだけです。  表示されます。
ユーザー エクスペリエンスのサインインおよびサインアップ オプションとして追加する新しい Idp します。

IDP の追加や削除、アプリケーション要求の操作、サインアップ属性の変更など、ポリシーを使用して自由に実験し、サンプル アプリに与える影響を観察できます。
ポリシー、認証要求、および OWIN すべてが互いにどのように結び付いているかを理解できるまで、いろいろ試してみてください。

リファレンスについては、完全なサンプル (構成値) を除く [.zip をここでは現状](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/complete.zip),、
または、GitHub からプロジェクトを複製することができます。

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet.git
```

<!--

## 次のステップ

さらに高度な B2C のトピックに進むことができます。  次のチュートリアルを試してみてください。

[Web アプリから Web API を呼び出す >>]()

[B2C アプリの UX のカスタマイズ]()

-->

