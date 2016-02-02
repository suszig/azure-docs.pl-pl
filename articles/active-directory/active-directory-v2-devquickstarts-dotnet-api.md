<properties
    pageTitle ="アプリ モデル v2.0 > Net Web api |Microsoft Azure]
    description =「個人用 Microsoft アカウントの両方からのトークンを受け付ける .NET MVC Web Api を構築する方法と職場または学校のアカウントです」。
    サービス ="active directory"
    documentationCenter=".net"
    authors ="dstrockis"
    manager ="mbaldwin"
    editor=""/>

<tags
    ms.service="active directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="12/09/2015"
    ms.author="dastrock"/>

# アプリ モデル v2.0 プレビュー: MVC Web API をセキュリティで保護する

Web API を使用して、protecet では、バージョン 2.0 のアプリケーション モデルできます [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) 個人用 Microsoft アカウントの両方を持つユーザーを有効にすると、トークンにアクセスし、職場または学校アカウントに安全にアクセスする Web API です。
> [AZURE.NOTE]
この情報は、v2.0 アプリ モデルのパブリック プレビューに関するものです。 一般的に使用できる Azure AD と統合する方法については、サービスを参照してください、 [Azure Active Directory 開発者ガイド 』](active-directory-developers-guide.md)します。

ASP.NET Web API では、.NET Framework 4.5 に含まれる Microsoft の OWIN ミドルウェアを使用することにより、これを達成できます。 ここで OWIN を使用する"To Do List"MVC の Web API を構築します。
- クライアントを作成し、タスクのユーザーの To-Do リストから読み取りを許可します。
-   対象の API が保護されているかを指定します。
-   Web API の呼び出しが含まれている有効なアクセス トークンを検証します。

これを行うには、次の手順を実行する必要があります。

1. アプリを Azure AD に登録する
2. OWIN 認証パイプラインを使用するようにアプリをセットアップする
3. To Do List Web API を呼び出すようにクライアント アプリを構成する

このチュートリアルのコードは保持 [github](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet)します。 理解するには [.zip としてアプリのスケルトンをダウンロードする](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) またはスケルトンを複製。

`git クローン--ブランチ スケルトン https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git`

完成したアプリは、このチュートリアルの終わりにも示しています。


## 1.アプリを登録します

新しいアプリを作成する [apps.dev.microsoft.com](https://apps.dev.microsoft.com), 、次の [詳細な手順について](active-directory-v2-app-registration.md)します。 次のことを確認します。

- アプリに割り当てられた**アプリケーション ID** をメモしておきます。これは後で必要になります。

この Visual Studio ソリューションには、単純な WPF アプリである "TodoListClient" も含まれています。 TodoListClient は、ユーザーがどのようにサインインし、クライアントが Web API にどのように要求を発行するかを示すために使用されます。 ここでは、TodoListClient と TodoListService の両方に同じアプリが使用されています。 TodoListClient を構成するために、以下のことも行う必要があります。

- アプリ用の**モバイル** プラットフォームを追加します。
- ポータルから**リダイレクト URI** をメモしておきます。 既定値を使用する必要があります `urn: ietf:wg:oauth:2.0:oob`します。


## 2.アプリで OWIN 認証パイプラインを使用するよう設定します。

アプリの登録が完了したら、受け取った要求やトークンを検証するために、v2.0 エンドポイントと通信するようアプリを設定する必要があります。

-   まず、ソリューションを開き、パッケージ マネージャー コンソールを使用して、OWIN ミドルウェア NuGet パッケージを TodoListService プロジェクトに追加する必要があります。

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   OWIN Startup クラスを TodoListService プロジェクトに追加 `Startup.cs`します。 プロジェクトを右クリックし、**[追加]**、**[新しいアイテム]** の順にクリックして、"OWIN" を検索します。 OWIN ミドルウェアを呼び出す、 `Configuration(...)` メソッドは、アプリケーションの起動時とします。
-   クラス宣言を変更する `パブリック部分クラスに Startup` -既にで実装していますこのクラスの一部を別のファイルです。  `Configuration(...)` confgureauth (…) を呼び出して、メソッド web アプリの認証を設定します。

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   ファイルを開く `App_Start\Startup.Auth.cs` を実装し、 `ConfigureAuth(...)` メソッドで、Web API をバージョン 2.0 エンドポイントからトークンを受け入れるように設定されます。

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

-   使用できるようになりました `[Authorize]` 属性をコント ローラーとアクションは、OAuth 2.0 ベアラー認証を保護します。 装飾、 `Controllers\TodoListController.cs` 承認タグを持つクラス。 これにより、ユーザーはそのページにアクセスする前に、サインインが必要になります。

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- 承認呼び出し元が正常に呼び出す場合のいずれか、 `TodoListController` Api、操作は呼び出し元に関する情報へのアクセスを必要があります。 OWIN を通して、ベアラー トークン内のクレームへのアクセスを提供する、 `ClaimsPrincpal` オブジェクトです。

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

-   最後に、開く、 `web.config` 、TodoListService プロジェクトのルートにファイルし、の構成の値を入力して、 `< appSettings >` セクションです。
  - `Ida: 対象ユーザー` は、 **アプリケーション Id** 、ポータルで入力したアプリのです。

## 3.クライアント アプリを構成し、サービスを実行する

Todo List Service の動作を確認できるようにするには、Todo List Client を構成して、Todo List Client が v2.0 エンドポイントからトークンを取得し、サービスを呼び出せるようにする必要があります。

- TodoListClient プロジェクトで開きます `App.config` に構成値を入力し、 `< appSettings >` セクションです。
  - `Ida: ClientId` 、ポータルからコピーしたアプリケーション Id。
    - `Ida: RedirectUri` は、 **リダイレクト Uri** ポータルからです。

最後に、各プロジェクトをクリーンアップし、ビルドして、実行します。 これで、.NET MVC Web API で個人の Microsoft アカウントと職場/学校アカウントの両方からトークンが受け付けられるようになりました。 TodoListClient にサインインし、Web API を呼び出して、タスクをユーザーの To-Do リストに追加します。

リファレンスについては、完全なサンプル (構成値) を除く [.zip をここでは現状](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), 、または GitHub から複製することができます。

`git クローン--ブランチの完全な https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git`

## 次のステップ

ここからは、さらなるトピックに進むことができます。 次のチュートリアルを試してみてください。

[バージョン 2.0 のアプリケーション モデルを使用した Web アプリケーションから Web API を呼び出す >>](active-directory-devquickstarts-webapp-webapi-dotnet.md)

その他のリソースについては、以下を参照してください。
- [アプリ モデル v2.0 プレビュー >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow"azure active directory"タグ >>](http://stackoverflow.com/questions/tagged/azure-active-directory)





