<properties
    pageTitle="アプリ モデル v2.0 >Net Web API| Microsoft Azure"
    description="個人の Microsoft アカウントと職場/学校アカウントの両方からトークンを受け付ける .NET MVC Web API を構築する方法を説明します。"
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

# アプリ モデル v2.0 プレビュー: MVC Web API をセキュリティで保護する

Web API を使用して、protecet では、バージョン 2.0 のアプリケーション モデルできます [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) 個人用 Microsoft アカウントの両方を持つユーザーを有効にすると、トークンにアクセスし、職場または学校アカウントに安全にアクセスする Web API です。

> [AZURE.NOTE]
この情報は、v2.0 アプリ モデルのパブリック プレビューに関するものです。  一般的に使用できる Azure AD と統合する方法については、サービスを参照してください、 [Azure Active Directory 開発者ガイド 』](active-directory-developers-guide.md)します。

ASP.NET Web API では、.NET Framework 4.5 に含まれる Microsoft の OWIN ミドルウェアを使用することにより、これを達成できます。  ここで OWIN を使用する"To Do List"MVC の Web API を構築します。
- クライアントを作成し、タスクのユーザーの To-Do リストから読み取りを許可します。
-   対象の API が保護されているかを指定します。
-   Web API の呼び出しが含まれている有効なアクセス トークンを検証します。

これを行うには、次の手順を実行する必要があります。

1. アプリを Azure AD に登録する
2. OWIN 認証パイプラインを使用するようにアプリをセットアップする
3. To Do List Web API を呼び出すようにクライアント アプリを構成する

このチュートリアルのコードは保持 [github](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet)します。  理解するには [.zip としてアプリのスケルトンをダウンロードする](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) またはスケルトンを複製。

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

完成したアプリは、このチュートリアルの終わりにも示しています。


## 1.アプリを登録します
新しいアプリを作成する [apps.dev.microsoft.com](https://apps.dev.microsoft.com), 、次の [詳細な手順について](active-directory-v2-app-registration.md)します。  次のことを確認します。

- 下のコピー、 **アプリケーション Id** をアプリに割り当てる必要があります、すぐにします。

この Visual Studio ソリューションには、単純な WPF アプリである "TodoListClient" も含まれています。  TodoListClient は、ユーザーがどのようにサインインし、クライアントが Web API にどのように要求を発行するかを示すために使用されます。  ここでは、TodoListClient と TodoListService の両方に同じアプリが使用されています。  TodoListClient を構成するために、以下のことも行う必要があります。

- 追加、 **Mobile** アプリのプラットフォームです。
- 下のコピー、 **リダイレクト URI** ポータルからです。 既定値の `urn:ietf:wg:oauth:2.0:oob`を使用する必要があります。


## 2.アプリで OWIN 認証パイプラインを使用するよう設定します。

アプリの登録が完了したら、受け取った要求やトークンを検証するために、v2.0 エンドポイントと通信するようアプリを設定する必要があります。

-   まず、ソリューションを開き、パッケージ マネージャー コンソールを使用して、OWIN ミドルウェア NuGet パッケージを TodoListService プロジェクトに追加する必要があります。

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   OWIN Startup クラスを TodoListService プロジェクト (`Startup.cs`) に追加します。  プロジェクトを右クリックの--> **追加** --> **[新しい項目の** "OWIN"を検索します。  アプリが起動すると、OWIN ミドルウェアは `Configuration(…)` メソッドを呼び出します。
-   クラス宣言を `public partial class Startup` に変更します。このクラスの一部を別のファイルで既に実装しています。  `Configuration(…)` メソッドで、ConfgureAuth(…) を呼び出して、Web アプリ用の認証をセットアップします。

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   ファイル `App_Start\Startup.Auth.cs` を開き、`ConfigureAuth(…)` メソッドを実装します。これにより Web API を v2.0 エンドポイントからのトークンを受け入れるように設定できます。

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

-   次に、`[Authorize]` 属性と OAuth 2.0 ベアラー認証を使用して、コントローラーとアクションを保護します。  認証タグを使用して、`Controllers\TodoListController.cs` クラスを修飾します。  これにより、ユーザーはそのページにアクセスする前に、サインインが必要になります。

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- 承認呼び出し元が `TodoListController` API の 1 つを正常に呼び出すときに、呼び出し元についての情報にアクセスするアクションが必要な場合があります。  OWIN は `ClaimsPrincpal` オブジェクトを通して、ベアラー トークン内のクレームにアクセスできるようにします。  

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

-   最後に、TodoListService プロジェクトのルートにある `web.config` ファイルを開いて、構成値を `<appSettings>` セクションで入力します。
  -  `ida:Audience` は、 **アプリケーション Id** 、ポータルで入力したアプリのです。

## 3.クライアント アプリを構成し、サービスを実行する
Todo List Service の動作を確認できるようにするには、Todo List Client を構成して、Todo List Client が v2.0 エンドポイントからトークンを取得し、サービスを呼び出せるようにする必要があります。

- TodoListClient プロジェクトで `App.config` を開いて、`<appSettings>` セクションに構成値を入力します。
  - `ida:ClientId` は、ポータルからコピーしたアプリケーション ID です。
    -  `ida:RedirectUri` は、 **リダイレクト Uri** ポータルからです。

最後に、各プロジェクトをクリーンアップし、ビルドして、実行します。  これで、.NET MVC Web API で個人の Microsoft アカウントと職場/学校アカウントの両方からトークンが受け付けられるようになりました。  TodoListClient にサインインし、Web API を呼び出して、タスクをユーザーの To-Do リストに追加します。

リファレンスについては、完全なサンプル (構成値) を除く [.zip をここでは現状](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), 、または GitHub から複製することができます。

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## 次のステップ
ここからは、さらなるトピックに進むことができます。  次のチュートリアルを試してみてください。

[v2.0 アプリ モデルで Web アプリから Web API を呼び出す >>](active-directory-devquickstarts-webapp-webapi-dotnet.md)

その他のリソースについては、以下を参照してください。
- [アプリ モデル v2.0 プレビュー >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow"azure active directory"タグ >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

