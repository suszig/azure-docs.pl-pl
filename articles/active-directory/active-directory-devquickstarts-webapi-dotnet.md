<properties
    pageTitle="Azure AD .NET の概要 | Microsoft Azure"
    description="認証と承認のために Azure AD と連携する .NET MVC Web API を構築する方法"
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
    ms.date="10/13/2015"
    ms.author="dastrock"/>


# Azure AD からのベアラー トークンの使用による Web API の保護

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

保護されているリソースにアクセスするアプリケーションを構築する場合、認められていないアクセスからこれらのリソースを保護する方法を習得する必要があります。
Azure AD を使用すると、OAuth Bearer 2.0 アクセス トークンを使用し、わずか数行のコードを追加するだけで、Web API を簡単に保護することができます。

ASP.NET Web アプリでは、.NET Framework 4.5 に含まれるコミュニティ駆動の OWIN ミドルウェアの Microsoft 実装を使用することにより、これを達成できます。  ここで OWIN を使用する"To Do List"web API を構築します。
-   対象の API が保護されているかを指定します。
-   Web API の呼び出しが含まれている有効なアクセス トークンを検証します。

これを行うには、次の手順を実行する必要があります。

1. アプリケーションを Azure AD に登録する
2. OWIN 認証パイプラインを使用するようにアプリをセットアップする
3. To Do List Web API を呼び出すように、クライアント アプリケーションを構成する

最初に、 [アプリのスケルトンをダウンロードする](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) または [、完全なサンプルをダウンロード](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip)します。  両方とも、Visual Studio 2013 ソリューションです。  アプリケーションの登録先となる Azure AD テナントも必要です。  既にがあるない場合 [いずれかを取得する方法について](active-directory-howto-tenant.md)します。


## *1.アプリケーションを Azure AD に登録する*
アプリケーションをセキュリティで保護するには、まず、テナントでアプリケーションを作成し、Azure AD にいくつかの重要な情報を提供する必要があります。

-   サインイン、 [Azure 管理ポータル](https://manage.windowsazure.com)
-   左側のナビゲーションで] をクリックして **Active Directory**
-   アプリケーションの登録先となるテナントを選択します。
-   クリックして、 **アプリケーション** タブをクリックし、クリックして **追加** 下部のドロアーでします。
-   画面の指示に従ってされ、新しい作成 **Web アプリケーションまたは WebAPI**します。
    -    **名前** アプリケーションはエンドユーザーにアプリケーションを記述します。  「To Do List Service」と入力します。
    -    **リダイレクト Uri** スキームと文字列の組み合わせで、アプリを返すためのすべてのトークン、要求された Azure AD が使用されます。 この値として、「`https://localhost:44321/`」を入力します。
-   登録が完了すると移動 **構成** ] タブで検索し、 **App ID URI** フィールドです。  この値には、テナント固有の識別子 (たとえば、`https://contoso.onmicrosoft.com/TodoListService`) を入力します。
- 構成を保存します。  ポータルを開いたままにします。この後すぐに、クライアント アプリケーションを登録することも必要になります。

## *2.アプリで OWIN 認証パイプラインを使用するよう設定します。*

これまでに、アプリケーションを Azure AD に登録したので、受信要求と受信トークンを検証するために、Azure AD と通信するようにアプリケーションをセットアップする必要があります。

-   まず、ソリューションを開き、パッケージ マネージャー コンソールを使用して、OWIN ミドルウェア NuGet パッケージを TodoListService プロジェクトに追加する必要があります。

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
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

-   ファイル `App_Start\Startup.Auth.cs` を開いて、`ConfigureAuth(…)` メソッドを実装します。  `WindowsAzureActiveDirectoryBearerAuthenticationOptions` で提供されたパラメーターは、アプリが Azure AD と通信するための調整役として機能します。

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        });
}
```

-   次に、`[Authorize]` 属性と JWT ベアラー認証を使用して、コントローラーとアクションを保護します。  認証タグを使用して、`Controllers\TodoListController.cs` クラスを修飾します。  これにより、ユーザーはそのページにアクセスする前に、サインインが必要になります。

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- 承認呼び出し元が `TodoListController` API の 1 つを正常に呼び出すときに、呼び出し元についての情報にアクセスするアクションが必要な場合があります。  OWIN は `ClaimsPrincpal` オブジェクトを通して、ベアラー トークン内のクレームにアクセスできるようにします。  
- Web API の共通する要件は、トークンで提示される "範囲" を検証することです。これにより、Todo List Service へのアクセスに必要なアクセス許可に、エンド ユーザーが同意していることが確認されます。

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
    {
        throw new HttpResponseException(new HttpResponseMessage {
          StatusCode = HttpStatusCode.Unauthorized,
          ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
        });
    }
    ...
}
```

-   最後に、TodoListService プロジェクトのルートにある `web.config` ファイルを開いて、構成値を `<appSettings>` セクションで入力します。
  - `ida:Tenant` は、Azure AD テナントの名前 (たとえば、"contoso.onmicrosoft.com") です。
  - `ida:Audience` は、Azure ポータルで入力したアプリケーションのアプリ ID URI です。

## *3.クライアント アプリケーションを構成して、サービスを実行するには.*
Todo List Service の動作を確認できるようにするには、Todo List Client を構成して、Todo List Client が AAD からトークンを取得し、サービスを呼び出せるようにする必要があります。

- 戻り、 [Azure 管理ポータル](https://manage.windowsazure.com)
- Azure AD テナントで新しいアプリケーションを作成し、選択 **ネイティブ クライアント アプリケーション** 表示されるプロンプトでします。
    -    **名前** アプリケーションのエンドユーザーに、アプリケーションの説明は
    -   入力 `http://TodoListClient/` の **リダイレクト Uri** 値。
- 登録が完了するには、AAD が割り当てられますアプリ一意 **App Id**します。 この値は次の手順で必要になるので、[構成] タブからコピーします。
-   登録が完了すると移動 **構成** ] タブで検索し、 **App ID URI** フィールドです。  この値には、テナント固有の識別子 (たとえば、`https://contoso.onmicrosoft.com/TodoListService`) を入力します。
- また、 **構成** ] タブで、"その他のアプリケーションに対するアクセス許可] セクションを探します。 [アプリケーションの追加] をクリックします。 [表示] ドロップダウンで [その他] を選択し、上のチェック マークをクリックします。 To Do List Service を見つけてクリックし、下のチェック マークをクリックしてアプリケーションを追加します。 [委任されたアクセス許可] ドロップダウンで [To Do List Service へのアクセス] を選択し、構成を保存します。


- Visual Studio で、TodoListClient プロジェクトの `App.config` を開いて、`<appSettings>` セクションに構成値を入力します。
  - `ida:Tenant` は、Azure AD テナントの名前 (たとえば、"contoso.onmicrosoft.com") です。
  - `ida:ClientId` は、Azure ポータルからコピーしたアプリ ID です。
  - `todo:TodoListResourceId` は、Azure ポータルで入力した、To Do List Service アプリのアプリケーション ID URI です。

最後に、各プロジェクトをクリーンアップし、ビルドして、実行します。  *.onmicrosoft.com ドメインを使用して、テナントに新しいユーザーを作成します (作成していない場合)。  そのユーザーとして To Do List クライアントにサインインし、いくつかのタスクを To Do List に追加します。

リファレンスについては、完全なサンプル (構成値) を除くが提供される [ここ](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip)します。  しようとすることも、さらなる id シナリオ移行できます。

[Build a .NET Native Client with Azure AD (Azure AD の使用による .NET ネイティブ クライアントの構築)](../active-directory-devquickstarts-native-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
