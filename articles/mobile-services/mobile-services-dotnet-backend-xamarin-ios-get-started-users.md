<properties
    pageTitle="Xamarin iOS アプリケーション用 Mobile Services の認証の使用 | Microsoft Azure"
    description="Mobile Services を使用して、Google、Facebook、Twitter、Microsoft などのさまざまな ID プロバイダーを通じて Xamarin iOS アプリのユーザーを認証する方法について説明します。"
    services="mobile-services"
    documentationCenter="xamarin"
    authors="lindydonna"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="12/01/2015" 
    ms.author="donnam"/>

# Mobile Services アプリへの認証の追加

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

このトピックでは、アプリケーションから Azure Mobile Services のユーザーを認証する方法について説明します。 このチュートリアルでは、Mobile Services でサポートされている ID プロバイダーを使用して、クイック スタート プロジェクトに認証を追加します。 Mobile Services によって正常に認証および承認されると、ユーザー ID 値が表示されます。

このチュートリアルでは、アプリケーションでの認証を有効にするための、次の基本的な手順について説明します。

1. [認証用アプリの登録し、Mobile Services を構成する]
2. [認証されたユーザーへのテーブルのアクセス許可を制限する]
3. [アプリに認証の追加]

このチュートリアルは、Mobile Services のクイック スタートに基づいています。 先にチュートリアル「Mobile Services の使用」を完了している必要があります。

##<a name="register"></a>アプリケーションを認証に登録し、Mobile Services を構成する

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

[AZURE.INCLUDE [mobile-services-dotnet-backend-aad-server-extension](../../includes/mobile-services-dotnet-backend-aad-server-extension.md)]

##<a name="permissions"></a>アクセス許可を、認証されたユーザーだけに制限する

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)]

<ol start="6">
<li><p>Visual Studio または Xamarin Studio で、デバイスまたはシミュレーターを使用してクライアント プロジェクトを実行します。 アプリケーションの開始後に、状態コード 401 (許可されていません) のハンドルされない例外が発生することを確認します。</p>

    <p>This happens because the app attempts to access Mobile Services as an unauthenticated user, but the <em>TodoItem</em> table now requires authentication.</p></li>
</ol>

次に、モバイル サービスのリソースを要求する前にユーザーを認証するようにアプリケーションを更新します。

##<a name="add-authentication"></a>アプリケーションに認証を追加する

ここでは、データを表示する前にログイン画面を表示するようにアプリケーションを変更します。 アプリケーションが起動したときには、モバイル サービスには接続されず、データも表示されません。 ユーザーが最初に更新操作を実行した後で、ログイン画面が表示されます。ログインに成功すると、Todo 項目の一覧が表示されます。

1. クライアント プロジェクトでファイルを開きます **QSTodoService.cs** QSTodoService に次の宣言を追加します。

        // Mobile Service logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. 新しいメソッドを追加 **認証** に **QSTodoService** を次の定義。

        private async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

> [AZURE.NOTE] Facebook 以外の id プロバイダーを使用している場合に渡される値を変更 **LoginAsync** 上、次のいずれか: _MicrosoftAccount_, 、_Twitter_, 、_Google_, 、または _WindowsAzureActiveDirectory_します。

3. 開いている **QSTodoListViewController.cs**します。 メソッド定義を変更 **ViewDidLoad** への呼び出しを削除する **RefreshAsync()** 最後に記載します。

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;

            todoService.BusyUpdate += (bool busy) => {
                if (busy)
                    activityIndicator.StartAnimating ();
                else
                    activityIndicator.StopAnimating ();
            };

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();

            AddRefreshControl ();
        }


4. メソッドを変更 **RefreshAsync** 場合は、ログイン画面を表示して認証する、 **ユーザー** プロパティが null です。 メソッド定義の先頭に次のコードを追加します。

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method

5. キーを押して、 **実行** 、プロジェクトをビルドし、iPhone シミュレーターでアプリケーションを開始] ボタンをクリックします。 アプリケーションにデータが表示されないことを確認します。

    項目の一覧をプルダウンして更新操作を実行すると、ログイン画面が表示されます。 有効な資格情報を正しく入力すると、Todo 項目の一覧が表示され、データを更新できるようになります。

<!-- ## <a name="next-steps"> </a>Next steps

次のチュートリアルでは、[モバイル サービス ユーザーのサービス側の承認] で [スクリプトを持つユーザーを承認する] を認証されたユーザーに基づいてモバイル サービスによって提供されるユーザー ID 値を受け取りしてモバイル サービスから返されたデータをフィルター処理に使用します。
 -->

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Next Steps]:#next-steps


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: mobile-services-dotnet-backend-xamarin-ios-get-started.md
[Get started with authentication]: mobile-services-dotnet-backend-xamarin-ios-get-started-users.md
[Get started with push notifications]: mobile-services-dotnet-backend-xamarin-ios-get-started-push.md
[Authorize users with scripts]: ../mobile-services-dotnet-backend-windows-store-dotnet-authorize-users-in-scripts.md
[JavaScript and HTML]: ../mobile-services-dotnet-backend-windows-store-javascript-get-started-users.md

