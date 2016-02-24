<properties
    pageTitle="Azure AD Windows Phone の概要 | Microsoft Azure"
    description="サインインし、OAuth を使用して Azure AD で保護されている API を呼び出すために Azure AD と連携する Windows Phone アプリケーションを構築する方法を説明します。"
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/13/2015"
    ms.author="dastrock"/>



# Azure AD と Windows Phone アプリの統合

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Windows Phone アプリを開発する場合、Azure AD を使用すると、Active Directory アカウントを使用してユーザーの認証処理を容易に行うことができます。  また、Office 365 API や Azure API などの Azure AD によって保護された任意の Web API をアプリケーションで安全に使用することもできます。

保護されたリソースにアクセスする必要のある .NET ネイティブ クライアントに対しては、Azure AD により、Active Directory 認証ライブラリ (ADAL) が提供されます。  ADAL の唯一の目的は、アプリがアクセス トークンを容易に取得できるようにすることです。  それがどれほど簡単であるかを示すために、ここでは次のような機能を備えた「ディレクトリ検索」 Windows Phone 8.1 アプリを作成します。

-   アクセスを使用して、Azure AD Graph API を呼び出すためのトークンを取得、 [OAuth 2.0 認証プロトコル](https://msdn.microsoft.com/library/azure/dn645545.aspx)します。
-   指定された UPN を持つユーザーをディレクトリで検索します。
-   ユーザーのサインアウト処理を行います。

完全に動作するアプリケーションを構築するには、次の手順を行う必要があります。

2. Azure AD にアプリケーションを登録する
3. ADAL をインストールおよび構成する
5. ADAL を使用して、Azure AD からトークンを取得する

最初に、 [スケルトン プロジェクトをダウンロード](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) または [、完全なサンプルをダウンロード](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip)します。  両方とも、Visual Studio 2013 ソリューションです。  また、ユーザーを作成し、アプリケーションを登録することを可能にするための Azure AD テナントも必要です。  場合は、テナントをまだ持っていない [いずれかを取得する方法について](active-directory-howto-tenant.md)します。

## *1.ディレクトリ検索アプリケーションを登録します。*
アプリでトークンを取得できるようにするには、まず、アプリを Azure AD テナントに登録し、Azure AD Graph API にアクセスするためのアクセス許可を付与する必要があります。

-   サインイン、 [Azure 管理ポータル](https://manage.windowsazure.com)
-   左側のナビゲーションで] をクリックして **Active Directory**
-   アプリケーションの登録先となるテナントを選択します。
-   クリックして、 **アプリケーション** タブをクリックし、クリックして **追加** 下部のドロアーでします。
-   画面の指示に従ってされ、新しい作成 **ネイティブ クライアント アプリケーション**します。
    -    **名前** アプリケーションのエンドユーザーに、アプリケーションの説明は
    -    **リダイレクト Uri** スキームと文字列の組み合わせで、Azure AD がトークン応答を返すために使用されます。  ここでは、プレースホルダーの値を入力します (例: `http://DirectorySearcher`)。  後でこの値を置き換えます。
-   登録が完了すると、AAD により、アプリに一意のクライアント ID が割り当てられます。  この値が必要する次のセクションでのでコピーから、 **構成** ] タブをクリックします。
- また、 **構成** ] タブで、"その他のアプリケーションに対するアクセス許可] セクションを探します。  "Azure Active Directory"アプリケーションを追加、 **アクセス組織のディレクトリ** 下 **委任されたアクセス許可**します。  これにより、アプリケーションが Graph API を使用してユーザーをクエリできるようになります。

## *2.ADAL のインストールと構成*
アプリケーションを Azure AD に登録したので、ADAL をインストールし、ID 関連のコードを記述できます。  ADAL が Azure AD と通信できるようにするためには、アプリケーションの登録に関するいくつかの情報を提供する必要があります。
-   まず、パッケージ マネージャー コンソールを使用して DirectorySearcher プロジェクトに ADAL を追加します。

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   DirectorySearcher プロジェクトで、`MainPage.xaml.cs` を開きます。  Azure ポータルで入力した値が反映されるように、`Config Values` 領域の値を置き換えます。  これらの値は、コードで ADAL を使用する際に常に参照されます。
    -   `tenant` は、Azure AD テナントのドメイン (たとえば、contoso.onmicrosoft.com) です。
    -   `clientId` は、ポータルからコピーしたアプリケーションのクライアント ID である必要があります。
-   ここで、Windows Phone アプリのコールバック URI を調べる必要があります。  `MainPage` メソッドの次の行にブレークポイントを設定します。

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- アプリを実行し、ブレークポイントにヒットしたら `redirectUri` の値をコピーしておきます。  次のような内容が表示されます。

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- 戻り、 **構成** ] タブ、Azure 管理ポータルでアプリケーションの値に置き換えます、 **RedirectUri** この値を使用します。  

## *3.ADAL を使用して、AAD からトークンを取得するには*
ADAL を使用することの基本的なメリットは、アプリがアクセス トークンを必要とする場合、アプリは `authContext.AcquireToken(…)` を呼び出すだけで、残りの処理は ADAL が実行してくれることです。  

-   最初の手順は、アプリの `AuthenticationContext` (ADAL のプライマリ クラス) を初期化することです。  ここでは、ADAL が Azure AD と通信し、トークンをキャッシュする方法を通知するために必要な調整項目を ADAL に渡します。

```C#
public MainPage()
{
    ...

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

- 次に、`Search(...)` メソッドを見つけます。このメソッドは、ユーザーがアプリの UI で [検索] ボタンをクリックすると呼び出されます。  このメソッドは、指定された検索用語で UPN が始まるユーザーをクエリするための、Azure AD Graph API に対する GET 要求を実行します。  ただし、Graph API をクエリするためには、要求の `Authorization` ヘッダーに access_token を含める必要があります。この処理を ADAL が実行します。

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try to get a token without triggering any user prompt.
    // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
- ADAL が Windows Phone の Web 認証ブローカー (WAB) を使用して対話型の認証が必要な場合と [継続のモデルを](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) ページ内で、Azure AD サインインを表示します。  ユーザーがサインインすると、アプリは ADAL に WAB 対話の結果を渡す必要があります。  これは、`ContinueWebAuthentication` インターフェイスを実装するのと同じくらい簡単です。

```C#
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

- 次に、ADAL がアプリに返す `AuthenticationResult` を使用します。  `QueryGraph(...)` コールバックで、取得した access_token を承認ヘッダーの GET 要求にアタッチします。

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
- `AuthenticationResult` を使用して、ユーザーに関する情報をアプリで表示することもできます。 `QueryGraph(...)` メソッドで、結果を使用してユーザーの ID をページに表示します。

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- 最後に、ADAL を使用してアプリケーションからユーザーをサインアウトさせることができます。  ユーザーが [サインアウト] ボタンをクリックした場合、次の `AcquireTokenSilentAsync(...)` の呼び出しは失敗するようにする必要があります。  ADAL を使用すると、この操作は、トークン キャッシュをクリアするのと同じくらい容易に達成できます。

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

ご利用ありがとうございます。 これで、ユーザー認証を処理でき、OAuth 2.0 を使用して Web API を安全に呼び出すことができ、ユーザーについての基本情報を取得できる、動作する Windows Phone アプリが完成しました。  テナントに一連のユーザーを設定します (設定していない場合)。  DirectorySearcher アプリを実行し、それらのユーザーの一人としてサインインします。  UPN に基づいて、他のユーザーを検索します。  アプリを閉じて、再び実行します。  ユーザーのセッションがそのままに維持されていることに注意します。  サインアウトし、別のユーザーとしてサインインします。

ADAL を使用することにより、これらの共通 ID 機能のすべてを容易にアプリケーションに組み込むことができます。  キャッシュ管理、OAuth プロトコル サポート、ログイン UI を使用してのユーザーの提示、有効期限切れとなったトークンの更新など、煩わしい操作を容易に実装できます。  習得する必要があるのは、単一の API 呼び出し、`authContext.AcquireToken*(…)` のみです。

リファレンスについては、完全なサンプル (構成値) を除くが提供される [ここ](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip)します。  ここからは、さらに ID シナリオに進むことができます。  次のチュートリアルを試してみてください。

[Protect a Web API using Bearer tokens from Azure AD (Azure AD からのベアラー トークンを使用することによる Web API の保護)](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
 
