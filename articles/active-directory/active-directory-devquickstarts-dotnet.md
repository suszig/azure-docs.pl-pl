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

デスクトップ アプリケーションを開発する場合、Azure AD を使用すると、Active Directory アカウントを使用してユーザーの認証処理を容易に行うことができます。 また、Office 365 API や Azure API などの Azure AD によって保護された任意の Web API をアプリケーションで安全に使用することもできます。

保護されたリソースにアクセスする必要のある .NET ネイティブ クライアントに対しては、Azure AD により、Active Directory 認証ライブラリ (ADAL) が提供されます。 ADAL の唯一の目的は、アプリからアクセス トークンを容易に取得できるようにすることです。 それがどれほど簡単であるかを示すために、ここで、次のような、.NET WPF To-Do List アプリケーションを構築します。

-   アクセスを使用して、Azure AD Graph API を呼び出すためのトークンを取得、 [OAuth 2.0 認証プロトコル](https://msdn.microsoft.com/library/azure/dn645545.aspx)します。
-   指定されたエイリアスを持つユーザーをディレクトリで検索します。
-   ユーザーのサインアウト処理を行います。

完全に動作するアプリケーションを構築するには、次の手順を行う必要があります。

2. Azure AD にアプリケーションを登録する
3. ADAL をインストールおよび構成する
5. ADAL を使用して、Azure AD からトークンを取得する

最初に、 [アプリのスケルトンをダウンロードする](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) または [、完全なサンプルをダウンロード](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip)します。 また、ユーザーを作成し、アプリケーションを登録することを可能にするための Azure AD テナントも必要です。 場合は、テナントをまだ持っていない [いずれかを取得する方法について](active-directory-howto-tenant.md)します。

## *1.DirectorySearcher アプリケーションを登録します。*

アプリでトークンを取得できるようにするには、まず、アプリを Azure AD テナントに登録し、Azure AD Graph API にアクセスするためのアクセス許可を付与する必要があります。

-   Microsoft Azure の管理ポータルにサインインします。
-   左側のナビゲーションで **[Active Directory]** をクリックします。
-   アプリケーションの登録先となるテナントを選択します。
-   **[アプリケーション]** タブをクリックし、下部のドロアーで **[追加]** をクリックします。
-   画面の指示に従い、新しい**ネイティブ クライアント アプリケーション**を作成します。
    -   アプリケーションの **[名前]** には、エンド ユーザーがアプリケーションの機能を把握できるような名前を設定します。
    -   **[リダイレクト URI]** には、Azure AD がトークン応答を返すために使用するスキームと文字列の組み合わせを設定します。 たとえば、アプリケーションに固有の値を入力してください `http://DirectorySearcher`します。
-   登録が完了すると、AAD により、アプリケーションに一意のクライアント ID が割り当てられます。 この値は次のセクションで必要になるので、**[構成]** タブからコピーします。
- また、**[構成]** タブで、[他のアプリケーションに対するアクセス許可] セクションに移動します。 "Azure Active Directory" アプリケーションに対して、**[委任されたアクセス許可]** の下の **[組織のディレクトリにアクセス]** アクセス許可を追加します。 これにより、アプリケーションが Graph API を使用してユーザーをクエリできるようになります。

## *2.ADAL のインストールと構成*

アプリケーションを Azure AD に登録したので、ADAL をインストールし、ID 関連のコードを記述できます。 ADAL が Azure AD と通信できるようにするためには、アプリケーションの登録に関するいくつかの情報を提供する必要があります。
-   まず、パッケージ マネージャー コンソールを使用して DirectorySearcher プロジェクトに ADAL を追加します。

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   DirectorySearcher プロジェクトで開きます `app.config`します。内の要素の値を置き換えて、 `< appSettings >` セクションで、Azure ポータルで入力した値を反映するようにします。これらの値は、コードで ADAL を使用する際に常に参照されます。
    -   `Ida: テナント` contoso.onmicrosoft.com など、Azure AD のテナントのドメインは、
    -   `Ida: ClientId` はポータルからコピーしたアプリケーションのクライアント id が必要です。
    -   `Ida: RedirectUri` リダイレクト url は、ポータルに登録します。

## *3.ADAL を使用して、AAD からトークンを取得するには*

ADAL の背後にある基本的なメリットは、アプリでは、アクセス トークンが必要があるたびに呼び出していることだけで `authContext.AcquireToken(...)`, 、残りの部分は ADAL で実行するとします。

-   `DirectorySearcher` プロジェクトを開き、 `MainWindow.xaml.cs` を検索し、 `MainWindow()` メソッドです。 アプリケーションの初期化には、まず `AuthenticationContext` -ADAL のプライマリ クラスです。 ここでは、ADAL が Azure AD と通信し、トークンをキャッシュする方法を通知するために必要な調整項目を ADAL に渡します。

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());
    ...
}
```

- 今すぐ検索、 `Search(...)` アプリの UI でユーザーの [検索] ボタンをクリックしたときに呼び出されるメソッド。 このメソッドは、指定された検索用語で UPN が始まるユーザーをクエリするための、Azure AD Graph API に対する GET 要求を実行します。 Graph API をクエリするためで、access_token を含める必要がありますが、 `承認` ヘッダー、要求のこれは、adal です。

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
- アプリが呼び出すことによって、トークンを要求したときに `AcquireToken(...)`, 、ADAL を資格情報のユーザーを求めることがなく、トークンを返そうとします。 ADAL は、トークンを取得するにはユーザーのサインインが必要であると判断した場合、ログイン ダイアログを表示し、ユーザーの資格情報を収集し、認証が成功するとトークンを返します。 スロー返せないが何らかの理由でトークンを返せない場合、 `AdalException`します。
- 注意して、 `AuthenticationResult` オブジェクトを含む、 `UserInfo` アプリが必要になる情報を収集するために使用するオブジェクト。 DirectorySearcher で、 `UserInfo` ユーザーの id を持つアプリの UI をカスタマイズするために使用します。

- 次に呼び出したことを確認するユーザーには、[サインアウト] ボタンがクリックすると、 `AcquireToken(...)` サインインを求められます。 ADAL を使用すると、この操作は、トークン キャッシュをクリアするのと同じぐらい容易に達成できます。

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- ただし、ユーザーが [サインアウト] ボタンをクリックしていない場合、ユーザーが次に DirectorySearcher を実行するときに備えて、ユーザーのセッションを維持する必要があります。 アプリが起動したら、既存のトークン用の ADAL のトークン キャッシュをチェックし、必要に応じて UI を更新できます。 戻り `MainWindow()`, 、もう 1 つの呼び出しを行う `AcquireToken(...)`, 、今回を渡して、 `PromptBehavior.Never` パラメーター。 `PromptBehavior.Never` は ADAL に対して通知、ユーザーにサインイン用プロンプトされませんが、トークンを返すことがない場合、ADAL は例外をスロー代わりにする必要があります。

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

ご利用ありがとうございます。 これで、ユーザー認証を処理でき、OAuth 2.0 を使用して Web API を安全に呼び出すことができ、ユーザーについての基本情報を取得できる、動作する . WPF アプリケーションが完成しました。 テナントに一連のユーザーを設定します (設定していない場合)。 DirectorySearcher アプリを実行し、それらのユーザーの一人としてサインインします。 UPN に基づいて、他のユーザーを検索します。 アプリを閉じて、再び実行します。 ユーザーのセッションがそのままに維持されていることに注意します。 サインアウトし、別のユーザーとしてサインインします。

ADAL を使用することにより、これらの共通 ID 機能のすべてを容易にアプリケーションに組み込むことができます。 キャッシュ管理、OAuth プロトコル サポート、ログイン UI を使用してのユーザーの提示、有効期限切れとなったトークンの更新など、面倒な操作を容易に実装できます。 本当に知っておきたいは、単一の API 呼び出し、 `authContext.AcquireToken(...)`します。

リファレンスについては、完全なサンプル (構成値) を除くが提供される [ここ](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip)します。 ここからは、さらなるシナリオに進むことができます。 次のチュートリアルを試してみてください。

[.NET Web API のセキュリティ保護 >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]





