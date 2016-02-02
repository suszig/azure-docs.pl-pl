<properties
    pageTitle="アプリ モデル v2.0 .NET Web アプリ | Microsoft Azure"
    description="サインインに個人の Microsoft アカウントと会社/学校アカウントの両方を使用する .NET MVC Web アプリを構築する方法を説明します。"
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="12/09/2015"
    ms.author="dastrock"/>


# アプリ モデル v2.0 プレビュー: .NET MVC Web アプリへのサインインの追加

v2.0 アプリ モデルを使用すると、Microsoft の個人および職場/学校アカウントの両方に対応した Web アプリに認証をすばやく追加できます。 ASP.NET Web アプリでは、.NET Framework 4.5 に含まれる Microsoft の OWIN ミドルウェアを使用することにより、これを達成できます。
  > [AZURE.NOTE]
    この情報は、v2.0 アプリ モデルのパブリック プレビューに関するものです。 一般的に使用できる Azure AD と統合する方法については、サービスを参照してください、 [Azure Active Directory 開発者ガイド 』](active-directory-developers-guide.md)します。

 ここでは、OWIN を使用して次のことを行います。
-   Azure AD と v2.0 アプリ モデルを使用するアプリにユーザーをサインインします。
-   ユーザーについての情報を表示します。
-   ユーザーをアプリからサインアウトします。

これを行うには、次の手順を実行する必要があります。

1. アプリを登録します。
2. OWIN 認証パイプラインを使用するようにアプリをセットアップする
3. OWIN を使用して、サインイン要求およびサインアウト要求を Azure AD に発行する
4. ユーザーに関するデータを印刷する

このチュートリアルのコードは保持 [github](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet)します。 理解するには [.zip としてアプリのスケルトンをダウンロードする](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) またはスケルトンを複製。

`git クローン--ブランチ スケルトン https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git`

完成したアプリは、このチュートリアルの終わりにも示しています。

## 1.アプリを登録します

新しいアプリを作成する [apps.dev.microsoft.com](https://apps.dev.microsoft.com), 、次の [詳細な手順について](active-directory-v2-app-registration.md)します。 次のことを確認します。

- アプリに割り当てられた**アプリケーション ID** をメモしておきます。これは後で必要になります。
- アプリの **Web** プラットフォームを追加します。
- 適切な**リダイレクト URI** を入力します。 リダイレクト uri は、Azure AD 認証の応答が送られる、このチュートリアルでは既定値は、そこに示す `https://localhost:44326/`します。

## 2.アプリで OWIN 認証パイプラインを使用するよう設定します。

ここでは、OpenID Connect 認証プロトコルを使用するように、OWIN ミドルウェアを構成します。 OWIN は、サインイン要求またはサインアウト要求の発行、ユーザー セッションの管理、ユーザーに関する情報の取得などを行うために使用されます。

-   最初に、開く、 `web.config` のファイルは、プロジェクトのルートで、アプリの構成に値を入力、 `< appSettings >` セクションです。
    -   `Ida: ClientId` は、 **アプリケーション Id** 登録ポータルでアプリに割り当てられます。
    -   `Ida: RedirectUri` は、 **リダイレクト Uri** 、ポータルで入力します。

-   次に、パッケージ マネージャー コンソールを使用して、プロジェクトに OWIN ミドルウェア NuGet パッケージを追加します。

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   クラスを追加、"OWIN スタートアップ"プロジェクトと呼ばれる `Startup.cs`  右プロジェクトをクリックして **追加** --> **[新しい項目の** "OWIN"を検索します。 OWIN ミドルウェアを呼び出す、 `Configuration(...)` メソッドは、アプリケーションの起動時とします。
-   クラス宣言を変更する `パブリック部分クラスに Startup` -既にで実装していますこのクラスの一部を別のファイルです。  `Configuration(...)` メソッドを呼び出して ConfigureAuth(...) web アプリの認証を設定するには

```C#
[assembly: OwinStartup(typeof(Startup))]

namespace TodoList_WebApp
{
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
}```

-   Open the file `App_Start\Startup.Auth.cs` and implement the `ConfigureAuth(...)` method.  The parameters you provide in `OpenIdConnectAuthenticationOptions` will serve as coordinates for your app to communicate with Azure AD.  You'll also need to set up Cookie Authentication - the OpenID Connect middleware uses cookies underneath the covers.

```C#
public void ConfigureAuth(IAppBuilder app)
             {
                     app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

                     app.UseCookieAuthentication(new CookieAuthenticationOptions());

                     app.UseOpenIdConnectAuthentication(
                             new OpenIdConnectAuthenticationOptions
                             {
                                     // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                                     // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                                     // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                                     ClientId = clientId,
                                     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
                                     RedirectUri = redirectUri,
                                     Scope = "openid",
                                     ResponseType = "id_token",
                                     PostLogoutRedirectUri = redirectUri,
                                     TokenValidationParameters = new TokenValidationParameters
                                     {
                                             ValidateIssuer = false,
                                     },
                                     Notifications = new OpenIdConnectAuthenticationNotifications
                                     {
                                             AuthenticationFailed = OnAuthenticationFailed,
                                     }
                             });
             }
```

## 3.OWIN を使用して、サインイン要求およびサインアウト要求を Azure AD に発行する

これまでに、アプリは、OpenID Connect 認証プロトコルを使用して v2.0 エンドポイントと適切に通信するように構成されています。 OWIN は、認証メッセージの構築、Azure AD からのトークンの検証、およびユーザー セッションの維持を行うためのすべての煩わしい処理を実行します。 OWIN により処理されないのは、ユーザーにサインインおよびサインアウトの方法を提供する処理のみです。

- コントローラーで承認タグを使用することにより、特定のページでは、サインインしてからでないとアクセスできないようにすることができます。 開いている `controllers \homecontroller.cs`, 、追加し、 `[Authorize]` タグを About コント ローラーです。

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   OWIN を使用することにより、コード内から認証要求を直接発行することもできます。 開いている `controllers \accountcontroller.cs`します。 SignIn() アクションおよび SignOut() アクションで、それぞれ OpenID Connect チャレンジおよびサインアウト要求を発行します。

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-   これで、開く `Views\Shared\_LoginPartial.cshtml`します。 ここは、ユーザーに、アプリのサインイン リンクおよびサインアウト リンクを表示し、ユーザーの名前をビュー内に表示する場所です。

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## 4.ユーザー情報を表示する

OpenID Connect を持つユーザーを認証するときに、バージョン 2.0 エンドポイントを返します。 id_token、アプリケーションが含まれていますが、 [クレーム](active-directory-v2-tokens.md#id_tokens), 、またはユーザーについてのアサーションです。 これらの要求を使用して、アプリを個人向けにカスタマイズすることができます。

- `Controllers\HomeController.cs` ファイルを開きます。 を介して、コント ローラーで、ユーザーのクレームにアクセスすることができます、 `ClaimsPrincipal.Current` セキュリティ プリンシパル オブジェクト。

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

最後に、アプリを構築して実行します。 Microsoft の個人または職場/学校アカウントのいずれかでサインインすると、ユーザーの ID が上部のナビゲーション バーにどのように反映されるかがわかります。 これで、Web アプリが業界標準のプロトコルで保護され、個人および職場/学校アカウントの両方でユーザーを認証できるようになりました。

リファレンスについては、完全なサンプル (構成値) を除く [.zip をここでは現状](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), 、または GitHub から複製することができます。

`git クローン--ブランチの完全な https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git`

## 次のステップ

これ以降は、さらに高度なトピックに進むことができます。 次のチュートリアルを試してみてください。

[Web API v2.0 アプリ モデルをセキュリティで保護された >>](active-directory-devquickstarts-webapi-dotnet.md)

その他のリソースについては、以下を参照してください。
- [アプリ モデル v2.0 プレビュー >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow"azure active directory"タグ >>](http://stackoverflow.com/questions/tagged/azure-active-directory)





