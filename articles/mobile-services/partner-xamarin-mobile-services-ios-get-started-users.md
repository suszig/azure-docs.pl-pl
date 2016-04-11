<properties
    pageTitle="認証の使用 (Xamarin.iOS) - Mobile Services"
    description="Xamarin.iOS 向け Azure Mobile Services アプリケーションで認証を使用する方法について説明します。"
    documentationCenter="xamarin"
    services="mobile-services"
    manager="dwrede"
    authors="lindydonna"
    editor=""/>


<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/30/2015"
    ms.author="donnam"/>

# Mobile Services アプリへの認証の追加

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

このトピックでは、アプリケーションから Azure Mobile Services のユーザーを認証する方法について説明します。  このチュートリアルでは、Mobile Services でサポートされている ID プロバイダーを使用して、クイック スタート プロジェクトに認証を追加します。 Mobile Services によって正常に認証および承認されると、ユーザー ID 値が表示されます。

このチュートリアルでは、アプリケーションでの認証を有効にするための、次の基本的な手順について説明します。

1. [アプリケーションを認証に登録し、モバイル サービスを構成する]
2. [テーブルのアクセス許可を、認証されたユーザーだけに制限する]
3. [アプリケーションに認証を追加する]

このチュートリアルは、モバイル サービスのクイック スタートに基づいています。 また、最初のチュートリアルを完了する必要があります [Get started with Mobile Services]します。

このチュートリアルを完了する必要 [Xamarin.iOS], 、XCode 6.0、および iOS 7.0 またはそれ以降のバージョン。

##<a name="register"></a>アプリケーションを認証に登録し、Mobile Services を構成する

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

##<a name="permissions"></a>アクセス許可を、認証されたユーザーだけに制限する


[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)]


3. Xcode で、チュートリアルを完了したときに作成したプロジェクトを開く [Get started with Mobile Services]します。

4. キーを押して、 **実行** プロジェクトをビルドするボタンをクリックし、iPhone エミュレーターでアプリケーションを開始。 アプリケーションの開始後に、状態コード 401 (許可されていません) のハンドルされない例外が発生することを確認します。

    これは、問題は、アプリケーションは、認証されないユーザーとしてモバイル サービスにアクセスしようとしていますが、 _TodoItem_ テーブルには、今すぐ認証が必要です。

次に、モバイル サービスのリソースを要求する前にユーザーを認証するようにアプリケーションを更新します。

##<a name="add-authentication"></a>アプリケーションに認証を追加する

1. 開いている、 **QSToDoService** プロジェクト ファイルと、次の変数を追加

        // Mobile Service logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. という名前の新しいメソッドを追加して **認証** に **ToDoService** として定義します。

        private async Task Authenticate(MonoTouch.UIKit.UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.MicrosoftAccount);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    > [AZURE.NOTE] Microsoft アカウント以外の id プロバイダーを使用している場合に渡される値を変更 **LoginAsync** 上、次のいずれか: _Facebook_, 、_Twitter_, 、_Google_, 、または _WindowsAzureActiveDirectory_します。

3. 移動の要求、 **ToDoItem** テーブルから、 **ToDoService** という名前の新しいメソッドにコンス トラクター **CreateTable**:

        private async Task CreateTable()
        {
            // Create an MSTable instance to allow us to work with the ToDoItem table
            todoTable = client.GetSyncTable<ToDoItem>();
        }

4. という名前の新しい非同期パブリック メソッドを作成する **LoginAndGetData** として定義します。

        public async Task LoginAndGetData(MonoTouch.UIKit.UIViewController view)
        {
            await Authenticate(view);
            await CreateTable();
        }

5.  **TodoListViewController** オーバーライド、 **ViewDidAppear** メソッドし、検出されるよう、以下を定義します。 場合に、ユーザーにログインこれ、 **ToDoService** ユーザーに対するハンドルがまだありません。

        public override async void ViewDidAppear(bool animated)
        {
            base.ViewDidAppear(animated);

            if (QSTodoService.DefaultService.User == null)
            {
                await QSTodoService.DefaultService.LoginAndGetData(this);
            }

            if (QSTodoService.DefaultService.User == null)
            {
                // TODO:: show error
                return;
            }


            await RefreshAsync();
        }
6. 元の呼び出しを削除 **RefreshAsync** から **TodoListViewController.ViewDidLoad**します。

7. キーを押して、 **実行** プロジェクトのビルドは、iPhone エミュレーターでアプリケーションを開始し、選択した id プロバイダーでログオン] ボタンをクリックします。

    ログインに成功すると、アプリケーションはエラーなしで実行されます。また、Mobile Services を照会してデータを更新できるようになります。

## 完成したサンプルの入手
ダウンロード、 [completed example project]します。 必ず更新して、 **applicationURL** と **applicationKey** 変数を独自の Azure 設定でします。

## <a name="next-steps"></a>次のステップ

次のチュートリアルでは、 [Authorize users with scripts], 、認証されたユーザーに基づいてモバイル サービスによって提供されるユーザー ID 値を受け取り、モバイル サービスから返されたデータをフィルター処理に使用します。

<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Next Steps]:#next-steps

<!-- Images. -->
[4]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-services-selection.png
[5]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-service-uri.png
[13]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-identity-tab.png
[14]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-portal-data-tables.png
[15]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-portal-change-table-perms.png

<!-- URLs. TODO:: update completed example project link with project download -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-xamarin-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-xamarin-ios
[Get started with push notifications]: /develop/mobile/tutorials/-get-started-with-push-xamarin-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-xamarin-ios
[completed example project]: http://go.microsoft.com/fwlink/p/?LinkId=331328
[Xamarin.iOS]: http://xamarin.com/download


