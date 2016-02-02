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

ASP.NET Web アプリでは、.NET Framework 4.5 に含まれるコミュニティ駆動の OWIN ミドルウェアの Microsoft 実装を使用することにより、これを達成できます。 ここで OWIN を使用する"To Do List"web API を構築します。
-   対象の API が保護されているかを指定します。
-   Web API の呼び出しが含まれている有効なアクセス トークンを検証します。

これを行うには、次の手順を実行する必要があります。

1. アプリケーションを Azure AD に登録する
2. OWIN 認証パイプラインを使用するようにアプリをセットアップする
3. To Do List Web API を呼び出すように、クライアント アプリケーションを構成する

最初に、 [アプリのスケルトンをダウンロードする](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) または [、完全なサンプルをダウンロード](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip)します。 両方とも、Visual Studio 2013 ソリューションです。 アプリケーションの登録先となる Azure AD テナントも必要です。 既にがあるない場合 [いずれかを取得する方法について](active-directory-howto-tenant.md)します。


## *1.アプリケーションを Azure AD に登録する*

アプリケーションをセキュリティで保護するには、まず、テナントでアプリケーションを作成し、Azure AD にいくつかの重要な情報を提供する必要があります。

-   にサインオン [Azure 管理ポータル](https://manage.windowsazure.com)
-   左側のナビゲーションで **[Active Directory]** をクリックします。
-   アプリケーションの登録先となるテナントを選択します。
-   **[アプリケーション]** タブをクリックし、下部のドロアーで **[追加]** をクリックします。
-   画面の指示に従い、新しい **Web アプリケーションまたは WebAPI** を作成します。
    -   アプリケーションの **[名前]** には、エンド ユーザーがアプリケーションの機能を把握できるような名前を設定します。 「To Do List Service」と入力します。
    -   **[リダイレクト URI]** には、Azure AD が、アプリが要求したトークンを返すために使用するスキームと文字列の組み合わせを設定します。 入力 `https://localhost:44321/` この値にします。
-   登録が完了したら、**[構成]** タブに移動し、**[アプリケーション ID/URI]** フィールドを見つけます。 この値の例: テナント固有の識別子を入力 `https://contoso.onmicrosoft.com/TodoListService`
- 構成を保存します。 ポータルを開いたままにします。この後すぐに、クライアント アプリケーションを登録することも必要になります。

## *2.アプリで OWIN 認証パイプラインを使用するよう設定します。*

これまでに、アプリケーションを Azure AD に登録したので、受信要求と受信トークンを検証するために、Azure AD と通信するようにアプリケーションをセットアップする必要があります。

-   まず、ソリューションを開き、パッケージ マネージャー コンソールを使用して、OWIN ミドルウェア NuGet パッケージを TodoListService プロジェクトに追加する必要があります。

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
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

-   ファイルを開く `App_Start\Startup.Auth.cs` を実装し、 `ConfigureAuth(...)` メソッドです。 提供されたパラメーター `WindowsAzureActiveDirectoryBearerAuthenticationOptions` は Azure AD と通信するため、アプリの調整として機能します。

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

-   使用できるようになりました `[Authorize]` コント ローラーと JWT ベアラー認証を使って操作を保護する属性です。 装飾、 `Controllers\TodoListController.cs` 承認タグを持つクラス。 これにより、ユーザーはそのページにアクセスする前に、サインインが必要になります。

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- 承認呼び出し元が正常に呼び出す場合のいずれか、 `TodoListController` Api、操作は呼び出し元に関する情報へのアクセスを必要があります。 OWIN を通して、ベアラー トークン内のクレームへのアクセスを提供する、 `ClaimsPrincpal` オブジェクトです。
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

-   最後に、開く、 `web.config` 、TodoListService プロジェクトのルートにファイルし、の構成の値を入力して、 `< appSettings >` セクションです。
  - `Ida: テナント` "contoso.onmicrosoft.com"など、Azure AD テナントの名前を指定します。
  - `Ida: 対象ユーザー` Azure ポータルで入力したアプリケーションのアプリ ID URI です。

## *3.クライアント アプリケーションを構成して、サービスを実行するには.*

Todo List Service の動作を確認できるようにするには、Todo List Client を構成して、Todo List Client が AAD からトークンを取得し、サービスを呼び出せるようにする必要があります。

- 戻り、 [Azure 管理ポータル](https://manage.windowsazure.com)
- Azure AD テナントで新しいアプリケーションを作成し、表示されるプロンプトで **[ネイティブ クライアント アプリケーション]** を選択します。
    -   アプリケーションの **[名前]** には、エンド ユーザーがアプリケーションの機能を把握できるような名前を設定します。
    -   入力 `http://TodoListClient/` の **リダイレクト Uri** 値。
- 登録が完了すると、AAD により、アプリに一意の**アプリ ID** が割り当てられます。 この値は次の手順で必要になるので、[構成] タブからコピーします。
-   登録が完了したら、**[構成]** タブに移動し、**[アプリケーション ID/URI]** フィールドを見つけます。 この値の例: テナント固有の識別子を入力 `https://contoso.onmicrosoft.com/TodoListService`
- また、**[構成]** タブで、[他のアプリケーションに対するアクセス許可] セクションに移動します。[アプリケーションの追加] をクリックします。[表示] ドロップダウンで [その他] を選択し、上のチェック マークをクリックします。To Do List Service を見つけてクリックし、下のチェック マークをクリックしてアプリケーションを追加します。[委任されたアクセス許可] ドロップダウンで [To Do List Service へのアクセス] を選択し、構成を保存します。

- Visual Studio で開きます `App.config` 、TodoListClient プロジェクトし、の構成の値を入力して、 `< appSettings >` セクションです。
  - `Ida: テナント` "contoso.onmicrosoft.com"など、Azure AD テナントの名前を指定します。
  - `Ida: ClientId` Azure ポータルからコピーしたアプリケーション ID。
  - `Todo:TodoListResourceId` Azure ポータルで入力した、To Do List Service アプリケーションのアプリ ID URI です。

最後に、各プロジェクトをクリーンアップし、ビルドして、実行します。 *.onmicrosoft.com ドメインを使用して、テナントに新しいユーザーを作成します (作成していない場合)。 そのユーザーとして To Do List クライアントにサインインし、いくつかのタスクを To Do List に追加します。

リファレンスについては、完全なサンプル (構成値) を除くが提供される [ここ](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip)します。 しようとすることも、さらなる id シナリオ移行できます。

[Azure AD と .NET ネイティブ クライアントを構築する >>](../active-directory-devquickstarts-native-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]




