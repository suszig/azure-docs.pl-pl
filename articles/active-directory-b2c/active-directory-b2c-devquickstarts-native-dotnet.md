<properties
    pageTitle="Azure AD B2C プレビュー | Microsoft Azure"
    description="Azure AD B2C を使用し、サインイン、サインアップ、プロファイル管理を備えた Windows デスクトップ アプリケーションを構築する方法。"
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


# Azure AD B2C プレビュー: Windows デスクトップ アプリを作成する

With Azure AD B2C を使用すると、簡単な手順で強力なセルフサービスの ID 管理機能をデスクトップ アプリに追加できます。 この記事では説明する方法
.NET WPF「to do リスト」アプリケーションを作成するユーザーのサインアップ、サインイン、およびプロファイルの管理を含みます。サインアップとサインインで使用するユーザー名のサポートが含まれる
または、電子メールだけでなく Facebook や Google などのソーシャル アカウントです。

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## 1.Azure AD B2C ディレクトリの取得

Azure AD B2C を使用するには、ディレクトリ (つまり、テナント) を作成しておく必要があります。 ディレクトリは、ユーザー、アプリ、グループなどをすべて格納するためのコンテナーです。 ない場合
既に、やり [B2C ディレクトリを作成する](active-directory-b2c-get-started.md) に進む前にします。

## 2.アプリケーションの作成

次に、B2C ディレクトリ内にアプリを作成する必要があります。これによって、アプリと安全に通信するために必要ないくつかの情報が Azure AD に提供されます。 アプリを作成するには
次の [手順](active-directory-b2c-app-registration.md)します。 このとき、

- アプリケーションに**ネイティブ クライアント**を含めます。
- コピー、 **リダイレクト Uri** `urn: ietf:wg:oauth:2.0:oob` -このコード サンプルの既定の URL であります。
- アプリに割り当てられた**アプリケーション ID** をメモしておきます。 このプロジェクトはすぐに必要になります。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## 3.ポリシーの作成

Azure AD の B2C によってすべてのユーザー エクスペリエンスを定義、 [* * * * ポリシー](active-directory-b2c-reference-policies.md)します。 このコード サンプルでは、3 つ含まれています 
id は、エクスペリエンス - サインアップ、サインイン、およびプロファイルを編集します。 」の説明に従って、各種類の 1 つのポリシーを作成する必要が、 
[ポリシーのリファレンス資料](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)します。 3 つのポリシーを作成するときは、必ず以下の操作を行ってください。

- ID プロバイダーのブレードで、**[ユーザー ID サインアップ]** または **[電子メール サインアップ]** を選択します。
- サインアップ ポリシーで、**表示名**と他のいくつかのサインアップ属性を選択します。
- すべてのポリシーで、アプリケーション クレームとして **表示名**と**オブジェクト ID** を選択します。 その他の要求も選択できます。
- ポリシーの作成後、各ポリシーの**名前**をメモしておきます。 プレフィックスを持つ必要があります `b2c_1_`します。 これらのポリシー名はすぐに必要になります。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

3 つのポリシーの作成が正常に完了したら、いつでもアプリをビルドできます。

## 4.コードのダウンロード

このチュートリアルのコードは保持 [github](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet)します。 移動するようにサンプルをビルドするには 
[.zip としてスケルトン プロジェクトをダウンロード](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip) またはスケルトンを複製します。

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

完成したアプリケーションも [.zip として利用可能な](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) または、
`完了` 同じリポジトリの分岐します。

サンプル コードをダウンロードした後は、Visual Studio を開き `.sln` ファイルを開始します。 ソリューションに 2 つのプロジェクトがあることがわかります。、 `TaskClient` プロジェクトと `TaskService` プロジェクトです。  `TaskClient` 、WPF は、 
ユーザーと対話するデスクトップ アプリケーションです。  `TaskService` はアプリのバックエンド web API を各ユーザーの to do リストに格納します。 両方の `TaskClient` と `TaskService` は、1 つで表されます **アプリケーション ID**
この場合は、1 つの論理アプリケーションを構成する、どちらもので、します。

## 5.タスク サービスを構成する

ときに、 `TaskService` receieves 要求、 `TaskClient`, 、要求の認証に有効なアクセス トークンを確認します。 アクセス トークンを検証するために 
必要な `TaskService` アプリに関する情報を使用します。  `TaskService` プロジェクトを開き、 `web.config` ルートにあるファイル 
プロジェクトの値を置き換えると、 `< appSettings >` セクション。

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/{1}/{2}?p={3}" />
    <add key="ida:Tenant" value="{Enter the name of your B2C tenant - it usually looks like constoso.onmicrosoft.com}" />
    <add key="ida:ClientId" value="{Enter the Application ID assigned to your app by the Azure Portal}" />
    <add key="ida:PolicyId" value="{Enter the name of one of the policies you created, like `b2c_1_my_sign_in_policy`}" />
  </appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Web API が安全に Azure AD B2C を使用して要求を認証する方法について学習するには、チェック アウト、
[Web API が概要記事](active-directory-b2c-devquickstarts-api-dotnet.md)します。

## 6.ポリシーの実行

これで、 `TaskService` は要求を認証する準備ができてを実装できること、 `TaskClient`します。 HTTP 認証要求を送信することによって、アプリは Azure AD B2C と通信します。
要求の一部として実行になると、ポリシーを指定します。 .NET デスクトップ アプリケーションで使用することができます、 **Active Directory 認証ライブラリ (ADAL)**
OAuth 2.0 認証メッセージを送信するポリシーを実行し、呼び出し元の web Api のトークンを取得します。

#### ADAL をインストールする

まず、Visual Studio Package Manager Console を使用して、ADAL を TaskClient プロジェクトに追加します。

```
PM> Install-Package Microsoft.Experimental.IdentityModel.Clients.ActiveDirectory -ProjectName TaskClient -IncludePrerelease
```

#### B2C の詳細情報を入力する

ファイルを開きます `Globals.cs` を独自の各プロパティ値を置き換えます。 このクラスは、全体にわたって使用、 `TaskClient` を一般的に使用される参照値。

```C#
public static class Globals
{
    public static string tenant = "{Enter the name of your B2C tenant - it usually looks like constoso.onmicrosoft.com}";
    public static string clientId = "{Enter the Application ID assigned to your app by the Azure Portal}";
    public static string signInPolicy = "{Enter the name of your sign in policy, e.g. b2c_1_sign_in}";
    public static string signUpPolicy = "{Enter the name of your sign up policy, e.g. b2c_1_sign_up}";
    public static string editProfilePolicy = "{Enter the name of your edit profile policy, e.g. b2c_1_edit_profile}";

    public static string taskServiceUrl = "https://localhost:44332";
    public static string aadInstance = "https://login.microsoftonline.com/";
    public static string redirectUri = "urn:ietf:wg:oauth:2.0:oob";

}
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


#### AuthenticationContext を作成する

ADAL のプライマリ クラスは、 `AuthenticationContext` -B2C ディレクトリで、アプリの接続を表します。 アプリケーションの起動時
インスタンスを作成、 `AuthenticationContext` で、 `MainWindow.xaml.cs`, 、これは、ウィンドウ全体で使用することができます。

```C#
public partial class MainWindow : Window
{
    private HttpClient httpClient = new HttpClient();
    private AuthenticationContext authContext = null;

    protected async override void OnInitialized(EventArgs e)
    {
        base.OnInitialized(e);

        // The authority parameter can be constructed by appending the name of your tenant to 'https://login.microsoftonline.com/'.
        // ADAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the ADAL TokenCache and created a simple FileCache in this app.
        authContext = new AuthenticationContext("https://login.microsoftonline.com/contoso.onmicrosoft.com", new FileCache());
        ...
    ...
```

#### サインアップ フローを開始する

ユーザーがサインアップ ボタンをクリックしたら、作成したサインアップ ポリシーを使用してサインアップ フローを開始します。 ADAL を使用するだけを呼び出す 
`authContext.AcquireTokenAsync(...)`します。 渡されたパラメーター `AcquireTokenAsync(...)` どのようなトークンが表示されたらで使用されているポリシーが決定されます 
認証要求およびなどです。

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that we want a token to the our own backend API
        // Use the PromptBehavior.Always flag to indicate to ADAL that it should show a sign-up UI no matter what.
        // Pass in the name of your sign-up policy to execute the sign-up experience.
        result = await authContext.AcquireTokenAsync(new string[] { Globals.clientId },
            null, Globals.clientId, new Uri(Globals.redirectUri),
            new PlatformParameters(PromptBehavior.Always, null), Globals.signUpPolicy);

        // Indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user information form the AuthenticationResult
        UsernameLabel.Content = result.UserInfo.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (AdalException ex)
    {
        if (ex.ErrorCode == "authentication_canceled")
        {
            MessageBox.Show("Sign up was canceled by the user");
        }
        else
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }

            MessageBox.Show(message);
        }

        return;
    }
}
```

#### サインイン フローを開始する

サインイン フローは、サインアップ フローと同じ方法で開始できます。 ユーザーがサインイン ボタンがクリックしたら、ADAL に対して同じ呼び出しを行いますが、今度はサインイン ポリシーを使用します。

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(new string[] { Globals.clientId },
                    null, Globals.clientId, new Uri(Globals.redirectUri),
                    new PlatformParameters(PromptBehavior.Always, null), Globals.signInPolicy);
        ...         
```

#### プロファイル編集フローを開始する

ここでも、同じ方法でプロファイル編集ポリシーを実行できます。

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(new string[] { Globals.clientId },
                    null, Globals.clientId, new Uri(Globals.redirectUri),
                    new PlatformParameters(PromptBehavior.Always, null), Globals.editProfilePolicy);
```

すべてのこのような場合、ADAL はいずれかを返す内のトークンの `AuthenticationResult` か、例外をスローします。 毎回、ADAL からトークンを取得します。 
使用することができます、 `AuthenticationResult.UserInfo` UI など、アプリのユーザー データを更新するオブジェクト。 ADAL を使用するため、トークンをキャッシュしても 
アプリケーションの他の部分では。

## 7.API を呼び出す

ADAL を使用してポリシーを実行し、トークンを取得しました。 しかし、ポリシーを実行することなく、既存のキャッシュされたトークンをチェックしたいことがよくあります。
このようなエラーは、アプリからユーザーの to-do リストを取得しようとする場合に、 `TaskService`します。 ボリュームを使用して同じ `authContext.AcquireTokenAsync(...)` 、これを行うメソッドと
使ってもう一度、 `clientId` としてスコープのパラメーターがこの時間を使用して `PromptBehavior.Never`:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = authContext.TokenCache.ReadItems().Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // We use the PromptBehavior.Never flag to indicate that ADAL should throw an exception if a token 
        // could not be acquired from the cache, rather than automatically prompting the user to sign in. 
        result = await authContext.AcquireTokenAsync(new string[] { Globals.clientId },
            null, Globals.clientId, new Uri(Globals.redirectUri),
            new PlatformParameters(PromptBehavior.Never, null), existingPolicy);

    }

    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (AdalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "user_interaction_required")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;

        }
        else
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }
            MessageBox.Show(message);
        }

        return;
    }
    ...
```

ときにへの呼び出し `AcquireTokenAsync(...)` が成功して、トークンがキャッシュで見つかった、トークンを追加することができます、 `承認` HTTP 要求のヘッダーできるように、 `TaskService` を認証できます
ユーザーの to do リストの読み取りに要求します。

```C#
    ...
    // Once the token has been returned by ADAL, add it to the http authorization header, before making the call to the TaskService.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(taskServiceUrl + "/api/tasks");
    ...
```

ユーザーにサインインを求めずにトークン キャッシュ内のトークンを確認する必要があるときはいつでも、これと同じパターンを使用できます。 たとえば、アプリケーションの起動時、
チェックする、 `FileCache` 、既存のトークンのユーザーのサインイン セッションを維持するため、アプリを実行するたび。 同じを確認できます。
内のコード、 `MainWindow`の `OnInitialized` イベントで、この場合は最初の実行を処理します。

## 8.ユーザーをサインアウトさせる

最後に、ユーザーが "サインアウト" ボタンをクリックしたときに、ADAL を使用してアプリでのユーザーのセッションを終了させることができます。 ADAL を使用してからすべてのトークンをオフにすると同じくらい簡単です。
トークン キャッシュ:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    authContext.TokenCache.Clear();

    // This is a helper method that clears browser cookies in the browser control that ADAL uses, it is not part of ADAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## 9.サンプル アプリの実行

最後に、ビルドし、両方を `TaskClient` と `TaskService`します。 電子メール アドレスまたはユーザー名を使用して、アプリにサインアップします。 サインアウトし、同じユーザーとしてもう一度サインインします。 そのユーザーのプロファイルを編集します。 サインアウトし、サインアップします。
別のユーザーを使用します。

## 10.ソーシャル IDP を追加する

現時点でアプリがサポートしているのは、**ローカル アカウント** (ユーザー名とパスワードと共に B2C ディレクトリに格納されているアカウント) を使用したユーザーのサインアップとサインインのみです。Azure AD B2C と
その他のサポートを追加する **id プロバイダー**, 、またはすべてのコードを変更することがなく、Idp します。

ソーシャル IDP をアプリに追加するには、まず、次の 1 つまたは 2 つの記事に記載されている詳細な手順に従います。 サポートする各 IDP のアプリケーションを登録する必要があります。
自分のシステムとクライアント ID を取得します。

- [IDP として Facebook を設定します。](active-directory-b2c-setup-fb-app.md)
- [IDP として Google を設定します。](active-directory-b2c-setup-goog-app.md)
- [Amazon を IDP として設定します。](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn を IDP として設定します。](active-directory-b2c-setup-li-app.md)

B2C ディレクトリに id プロバイダーを追加したときに、それぞれ、3 つのポリシーに含める新しい Idp を編集する必要があります。
」の説明に従って、 [ポリシーのリファレンス資料](active-directory-b2c-reference-policies.md)します。 ポリシーを保存したら、もう一度アプリを実行するだけです。 表示されます。
ユーザー エクスペリエンスのサインインおよびサインアップ オプションとして追加する新しい Idp します。

IDP の追加や削除、アプリケーション要求の操作、サインアップ属性の変更など、ポリシーを使用して自由に実験し、サンプル アプリに与える影響を観察できます。
ポリシー、認証要求、および ADAL が互いに結び付くしくみを理解できるまで、実験を続けてみてください。

リファレンスについては、完全なサンプルは [.zip をここでは現状](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip),、
または、GitHub からプロジェクトを複製することができます。

`git クローン--ブランチの完全な https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git`







