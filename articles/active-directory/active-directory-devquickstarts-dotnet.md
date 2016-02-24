<properties
    pageTitle="Azure AD .NET の概要 | Microsoft Azure"
    description="サインインし、OAuth を使用して Azure AD で保護されている API を呼び出すために Azure AD と連携する .NET Windows デスクトップ アプリケーションを構築する方法を説明します。"
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


# Windows デスクトップ WPF アプリの Azure AD への統合

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

デスクトップ アプリケーションを開発する場合、Azure AD を使用すると、Active Directory アカウントを使用してユーザーの認証処理を容易に行うことができます。  また、Office 365 API や Azure API などの Azure AD によって保護された任意の Web API をアプリケーションで安全に使用することもできます。

保護されたリソースにアクセスする必要のある .NET ネイティブ クライアントに対しては、Azure AD により、Active Directory 認証ライブラリ (ADAL) が提供されます。  ADAL の唯一の目的は、アプリからアクセス トークンを容易に取得できるようにすることです。  それがどれほど簡単であるかを示すために、ここで、次のような、.NET WPF To-Do List アプリケーションを構築します。

-   アクセスを使用して、Azure AD Graph API を呼び出すためのトークンを取得、 [OAuth 2.0 認証プロトコル](https://msdn.microsoft.com/library/azure/dn645545.aspx)します。
-   指定されたエイリアスを持つユーザーをディレクトリで検索します。
-   ユーザーのサインアウト処理を行います。

完全に動作するアプリケーションを構築するには、次の手順を行う必要があります。

2. Azure AD にアプリケーションを登録する
3. ADAL をインストールおよび構成する
5. ADAL を使用して、Azure AD からトークンを取得する

最初に、 [アプリのスケルトンをダウンロードする](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) または [、完全なサンプルをダウンロード](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip)します。  また、ユーザーを作成し、アプリケーションを登録することを可能にするための Azure AD テナントも必要です。  場合は、テナントをまだ持っていない [いずれかを取得する方法について](active-directory-howto-tenant.md)します。

## *1.DirectorySearcher アプリケーションを登録します。*
アプリでトークンを取得できるようにするには、まず、アプリを Azure AD テナントに登録し、Azure AD Graph API にアクセスするためのアクセス許可を付与する必要があります。

-   Microsoft Azure の管理ポータルにサインインします。
-   左側のナビゲーションで] をクリックして **Active Directory**
-   アプリケーションの登録先となるテナントを選択します。
-   クリックして、 **アプリケーション** タブをクリックし、クリックして **追加** 下部のドロアーでします。
-   画面の指示に従ってされ、新しい作成 **ネイティブ クライアント アプリケーション**します。
    -    **名前** アプリケーションのエンドユーザーに、アプリケーションの説明は
    -    **リダイレクト Uri** スキームと文字列の組み合わせで、Azure AD がトークン応答を返すために使用されます。  アプリケーション固有の値 (たとえば、`http://DirectorySearcher`) を入力します。
-   登録が完了すると、AAD により、アプリケーションに一意のクライアント ID が割り当てられます。  この値が必要する次のセクションでのでコピーから、 **構成** ] タブをクリックします。
- また、 **構成** ] タブで、"その他のアプリケーションに対するアクセス許可] セクションを探します。  "Azure Active Directory"アプリケーションを追加、 **アクセス組織のディレクトリ** 下 **委任されたアクセス許可**します。  これにより、アプリケーションが Graph API を使用してユーザーをクエリできるようになります。

## *2.ADAL のインストールと構成*
アプリケーションを Azure AD に登録したので、ADAL をインストールし、ID 関連のコードを記述できます。  ADAL が Azure AD と通信できるようにするためには、アプリケーションの登録に関するいくつかの情報を提供する必要があります。
-   まず、パッケージ マネージャー コンソールを使用して DirectorySearcher プロジェクトに ADAL を追加します。

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   DirectorySearcher プロジェクトで、`app.config` を開きます。  Azure ポータルで入力した値が反映されるように、`<appSettings>` セクションの要素の値を置き換えます。  これらの値は、コードで ADAL を使用する際に常に参照されます。
    -   `ida:Tenant` は、Azure AD テナントのドメイン (たとえば、contoso.onmicrosoft.com) です。
    -   `ida:ClientId` は、ポータルからコピーしたアプリケーションのクライアント ID である必要があります。
    -   `ida:RedirectUri` は、ポータルに登録したリダイレクト URL です。

## *3.ADAL を使用して、AAD からトークンを取得するには*
ADAL を使用することの基本的なメリットは、アプリがアクセス トークンを必要とする場合、アプリは `authContext.AcquireToken(...)` を呼び出すだけで、残りの処理は ADAL が実行してくれることです。  

-   `DirectorySearcher` プロジェクトで、`MainWindow.xaml.cs` を開き、`MainWindow()` メソッドを見つけます。  最初の手順は、アプリの `AuthenticationContext` (ADAL のプライマリ クラス) を初期化することです。  ここでは、ADAL が Azure AD と通信し、トークンをキャッシュする方法を通知するために必要な調整項目を ADAL に渡します。

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());
    ...
}
```

- 次に、`Search(...)` メソッドを見つけます。このメソッドは、ユーザーがアプリの UI で [検索] ボタンをクリックすると呼び出されます。  このメソッドは、指定された検索用語で UPN が始まるユーザーをクエリするための、Azure AD Graph API に対する GET 要求を実行します。  ただし、Graph API をクエリするためには、要求の `Authorization` ヘッダーに access_token を含める必要があります。この処理を ADAL が実行します。

```C#
private void Search(object sender, RoutedEventArgs e)
{
    ...

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = authContext.AcquireToken(graphResourceId, clientId, redirectUri);
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
- アプリが `AcquireToken(...)` を呼び出すことによってトークンを要求すると、ADAL はユーザーに資格情報を要求することなく、トークンを返そうとします。  ADAL は、トークンを取得するにはユーザーのサインインが必要であると判断した場合、ログイン ダイアログを表示し、ユーザーの資格情報を収集し、認証が成功するとトークンを返します。  また、何らかの理由によりトークンを返せない場合、`AdalException` をスローします。
- `AuthenticationResult` オブジェクトには、アプリが必要とする可能性のある情報を収集するために使用される `UserInfo` オブジェクトが含まれていることに注意してください。  DirectorySearcher で、`UserInfo` は、ユーザーの ID を使用してアプリの UI をカスタマイズするために使用されます。

- ユーザーが [サインアウト] ボタンをクリックした場合、次の `AcquireToken(...)` への呼び出しでは、ユーザーにサインインするように要求する必要があります。  ADAL を使用すると、この操作は、トークン キャッシュをクリアするのと同じくらい容易に達成できます。

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- ただし、ユーザーが [サインアウト] ボタンをクリックしていない場合、ユーザーが次に DirectorySearcher を実行するときに備えて、ユーザーのセッションを維持する必要があります。  アプリが起動したら、既存のトークン用の ADAL のトークン キャッシュをチェックし、必要に応じて UI を更新できます。  戻り `MainWindow()`, 、もう 1 つの呼び出しを行う `AcquireToken(...)`, 、今回を渡して、 `PromptBehavior.Never` パラメーター。  `PromptBehavior.Never` ADAL に対して通知、ユーザーにサインイン用プロンプトされませんが、トークンを返すことがない場合、ADAL は例外をスロー代わりにする必要があります。

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = authContext.AcquireToken(graphResourceId, clientId, redirectUri, PromptBehavior.Never);
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

ご利用ありがとうございます。 これで、ユーザー認証を処理でき、OAuth 2.0 を使用して Web API を安全に呼び出すことができ、ユーザーについての基本情報を取得できる、動作する . WPF アプリケーションが完成しました。  テナントに一連のユーザーを設定します (設定していない場合)。  DirectorySearcher アプリを実行し、それらのユーザーの一人としてサインインします。  UPN に基づいて、他のユーザーを検索します。  アプリを閉じて、再び実行します。  ユーザーのセッションがそのままに維持されていることに注意します。  サインアウトし、別のユーザーとしてサインインします。

ADAL を使用することにより、これらの共通 ID 機能のすべてを容易にアプリケーションに組み込むことができます。  キャッシュ管理、OAuth プロトコル サポート、ログイン UI を使用してのユーザーの提示、有効期限切れとなったトークンの更新など、煩わしい操作を容易に実装できます。  習得する必要があるのは、単一の API 呼び出し、`authContext.AcquireToken(...)` のみです。

リファレンスについては、完全なサンプル (構成値) を除くが提供される [ここ](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip)します。  ここからは、さらなるシナリオに進むことができます。  次のチュートリアルを試してみてください。

[Protect a Web API using Bearer tokens from Azure AD (Azure AD からのベアラー トークンを使用することによる Web API の保護)](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
 
