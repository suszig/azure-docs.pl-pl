<properties
    pageTitle="アプリ モデル v2.0 .NET Web アプリ | Microsoft Azure"
    description="サインインに Microsoft の個人および職場/学校アカウントを使用する Web サービスを呼び出す .NET MVC Web アプリを構築する方法。"
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


# アプリ モデル v2.0 プレビュー: .NET Web アプリから Web API を呼び出す

> [AZURE.NOTE]
    この情報は、v2.0 エンドポイントのパブリック プレビューに関するものです。 一般的に使用できる Azure AD と統合する方法については、サービスを参照してください、 [Azure Active Directory 開発者ガイド 』](active-directory-developers-guide.md)します。

v2.0 アプリ モデルを使用すると、Microsoft の個人および職場/学校アカウントの両方に対応した Web アプリと Web API に認証をすばやく追加できます。 ここでは、次のような機能を備えた MVC Web アプリを構築します。

- OpenID Connect と Microsoft の OWIN ミドルウェアを使用してユーザーのサインイン処理を行う機能。
- ADAL を使用して Web API 用の OAuth 2.0 アクセス トークンを取得する機能。
- ユーザーの "To-Do List" の項目を作成、読み取り、削除する機能。これは、Web API でホストされ、OAuth 2.0 で保護されます。

このチュートリアルは、主に取得して完全に説明されている web アプリケーションでアクセス トークンを使用するのには対象 [ここ](active-directory-v2-flows.md#web-apps)します。 前提条件としてをまず知りたい場合がある方法 [web アプリに基本的なサインイン追加](active-directory-v2-devquickstarts-dotnet-web.md) 方法や [web API を正しく保護して](active-directory-v2-devquickstarts-dotnet-api.md)します。

To-Do List Web API をクライアントから呼び出す基本的な手順は次のとおりです。

1. アプリを登録します
2. OpenID Connect を使用して、Web アプリに対するユーザーのサインイン処理を行います
3. ADAL を使用して、ユーザーがサインインしたときにアクセス トークンを取得します
4. アクセス トークンで To-Do List Web API を呼び出します

このチュートリアルのコードは保持 [github](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet)します。

理解するには [.zip としてアプリのスケルトンをダウンロードする](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) またはスケルトンを複製。

`git クローン--ブランチ スケルトン https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git`

または、 [.zip として完成したアプリケーションをダウンロード](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) または完成したアプリケーションを複製します。

`git クローン--ブランチの完全な https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git`

## 1.アプリを登録します

新しいアプリを作成する [apps.dev.microsoft.com](https://apps.dev.microsoft.com), 、次の [詳細な手順について](active-directory-v2-app-registration.md)します。 次のことを確認します。

- アプリに割り当てられた**アプリケーション ID** をメモしておきます。これは後で必要になります。
- **[パスワード]** の **[アプリケーション シークレット]** を作成し、後で使用するために値を書き留めます
- アプリの **Web** プラットフォームを追加します。
- 適切な**リダイレクト URI** を入力します。 リダイレクト uri は、Azure AD 認証の応答が送られる、このチュートリアルでは既定値は、そこに示す `https://localhost:44326/`します。


## 2.OpenID Connect を使用してユーザーのサインイン処理を行います

ここでは、構成、OWIN ミドルウェアを使用する、 [OpenID Connect 認証プロトコル](active-directory-v2-protocols.md#openid-connect-sign-in-flow)します。 OWIN は、サインイン要求またはサインアウト要求の発行、ユーザー セッションの管理、ユーザーに関する情報の取得などを行うために使用されます。

-   開く、 `web.config` のルートにファイル、 `TodoList WebApp` プロジェクトとに、アプリの構成値を入力、 `< appSettings >` セクションです。
    -   `Ida: ClientId` は、 **アプリケーション Id** 登録ポータルでアプリに割り当てられます。
    - `Ida: ClientSecret` は、 **App Secret** 登録ポータルで作成します。
    -   `Ida: RedirectUri` は、 **リダイレクト Uri** 、ポータルで入力します。
- 開いている、 `web.config` のルートにファイル、 `TodoList サービス` プロジェクト、および置換、 `ida: 対象ユーザー` では、同じ **アプリケーション Id** 前述と同様です。

-   今すぐには、OWIN ミドルウェア NuGet パッケージを追加、 `TodoList WebApp` プロジェクトのパッケージ マネージャー コンソールを使用します。

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

-   ファイルを開く `App_Start\Startup.Auth.cs` 追加 `を使用して` 上のライブラリのステートメントです。
- 同じファイルでは、実装、 `ConfigureAuth(...)` メソッドです。 提供されたパラメーター `OpenIDConnectAuthenticationOptions` は Azure AD と通信するため、アプリの調整として機能します。

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
                    Scope = "openid offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
...
```

## 3.ADAL を使用して、ユーザーがサインインしたときにアクセス トークンを取得します

`AuthorizationCodeReceived` を使用する通知、 [OpenID Connect と平行して OAuth 2.0](active-directory-v2-protocols.md#openid-connect-with-oauth-code-flow) を to do List Service へのアクセス トークンの authorization_code を引き換えます。 ADAL を使用すると、これを簡単に行えるようになります。

- まず、ADAL のプレビュー バージョンをインストールします。

`PM > Install-package Microsoft.Experimental.IdentityModel.Clients.ActiveDirectory - ProjectName TodoList WebApp IncludePrerelease`
- もう 1 つ追加して `を使用して` ステートメントを `App_Start\Startup.Auth.cs` ADAL のファイルです。
- これで、新しいメソッドを追加、 `OnAuthorizationCodeReceived` イベント ハンドラーです。 このハンドラーは、ADAL を使用して To Do List API へのアクセス トークンを取得し、後で使用できるように ADAL のトークン キャッシュに格納します。

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        var authContext = new Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext(authority, new NaiveSessionCache(userObjectId));
        var authResult = await authContext.AcquireTokenByAuthorizationCodeAsync(notification.Code, new Uri(redirectUri), cred, new string[] { clientId });
}
...
```

- Web アプリの ADAL には、トークンの格納に使用できる拡張可能なトークン キャッシュが用意されています。 このサンプルを実装して、 `NaiveSessionCache` トークンをキャッシュする http セッションの記憶域を使用します。




## 4.To-Do List Web API を呼び出す

次に、手順 3. で取得した access_token を実際に使用します。 Web アプリを開いて `Controllers\TodoListController.cs` ファイルで、すべての CRUD 要求 To-Do List API を行います。

- ここで再度 ADAL を使用して、ADAL キャッシュから access_tokens を取得できます。 最初に、追加、 `を使用して` ADAL のためのステートメントをこのファイルにします。

    `Microsoft.Experimental.IdentityModel.Clients.ActiveDirectory; を使用します。`

- `インデックス` アクションを使用する ADAL の `AcquireTokenSilentAsync` 、to do リスト サービスからデータを読み取るために使用する access_token を取得します。

```C#
...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
AuthenticationContext authContext = new AuthenticationContext(authority, new NaiveSessionCache(userObjectID));
result = await authContext.AcquireTokenSilentAsync(new string[] { Startup.clientId }, credential, UserIdentifier.AnyUser);
...
```

- サンプルは、発行されたトークンを HTTP GET 要求として、追加、 `承認` ヘッダーで、タスク一覧のサービス要求の認証に使用します。
- To do リスト サービスを返す場合、 `401 Unauthorized` 応答、access_tokens ADAL で無効になった何らかの理由。 この場合、ADAL キャッシュから access_tokens をドロップし、サインインし直す必要があることを示すメッセージをユーザーに表示する必要があります。これにより、トークンの取得フローが再起動されます。

```C#
...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        var todoTokens = authContext.TokenCache.ReadItems().Where(a => a.Scope.Contains(Startup.clientId));
        foreach (TokenCacheItem tci in todoTokens)
                authContext.TokenCache.DeleteItem(tci);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
...
```

- 同様に、ADAL が何らかの理由で access_token を返せない場合、ユーザーにサインインし直すよう指示する必要があります。 これは、いずれかのキャッチと同じくらい簡単 `AdalException`:

```C#
...
catch (AdalException ee)
{
        // If ADAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
...
```

- 同じ正確な `AcquireTokenSilentAsync` 呼び出しはで implementd、 `作成` と `削除` アクション。 Web アプリでは、この ADAL メソッドを使用して、アプリで必要になったときにいつでも access_tokens を取得できます。 ADAL によりトークンが自動的に取得、キャッシュ、および更新されます。

最後に、アプリを構築して実行します。Microsoft アカウントまたは Azure AD アカウントでサインインすると、ユーザーの ID が上部のナビゲーション バーにどのように反映されるかがわかります。ユーザーの To-Do List からいくつかの項目を追加、削除して、OAuth 2.0 の保護された API 呼び出しの動作を確認します。これで、Web アプリと Web API のいずれも業界標準のプロトコルで保護され、個人および職場/学校アカウントの両方でユーザーを認証できるようになりました。

リファレンスについては、完全なサンプル (構成値) を除く [はここで説明](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip)します。

## 次のステップ

その他のリソースについては、以下を参照してください。
- [アプリ モデル v2.0 プレビュー >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow"adal"タグ >>](http://stackoverflow.com/questions/tagged/adal)





