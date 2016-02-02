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

このトピックでは、アプリケーションから Azure Mobile Services のユーザーを認証する方法について説明します。 このチュートリアルでは、Mobile Services でサポートされている ID プロバイダーを使用して、クイック スタート プロジェクトに認証を追加します。 Mobile Services によって正常に認証および承認されると、ユーザー ID 値が表示されます。

このチュートリアルでは、アプリケーションでの認証を有効にするための、次の基本的な手順について説明します。

1. [認証用アプリの登録し、Mobile Services を構成する]
2. [認証されたユーザーへのテーブルのアクセス許可を制限する]
3. [アプリに認証の追加]

このチュートリアルは、Mobile Services のクイック スタートに基づいています。 先にチュートリアル「Mobile Services の使用」を完了している必要があります。

このチュートリアルを完了する必要 [Xamarin.iOS]、XCode 6.0、および iOS 7.0 またはそれ以降のバージョン。

## <a name="register"></a>アプリケーションを認証に登録し、モバイル サービスを構成します。

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

## <a name="permissions"></a>認証されたユーザーへのアクセス許可を制限します。

[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)]


3. Xcode で、チュートリアルを完了したときに作成したプロジェクトを開く [を使ってみるモバイル サービス] です。

4. **[実行]** を押してプロジェクトをビルドし、iPhone エミュレーターでアプリケーションを開始します。アプリケーションの開始後に、状態コード 401 (許可されていません) のハンドルされない例外が発生することを確認します。

    この問題は、認証されないユーザーとしてアプリケーションが Mobile Services にアクセスしようとしても、_TodoItem_ テーブルでは認証が要求されるために発生します。

次に、モバイル サービスのリソースを要求する前にユーザーを認証するようにアプリケーションを更新します。

## <a name="add-authentication"></a>アプリに認証を追加します。

1. **ToDoService** プロジェクト ファイルを開き、次の変数を追加します。

        // Mobile Service logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. **ToDoService** に **Authenticate** という名前の新しいメソッドを追加し、次のように定義します。

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

    > [AZURE.NOTE] Microsoft アカウント以外の ID プロバイダーを使用している場合は、上の例の **LoginAsync** に渡される値を _Facebook_、_Twitter_、_Google_、または _WindowsAzureActiveDirectory_ のいずれかに変更します。

3. **ToDoItem** テーブルに対する要求を、**ToDoService** コンストラクターから **CreateTable** という名前の新しいメソッドに移動します。

        private async Task CreateTable()
        {
            // Create an MSTable instance to allow us to work with the ToDoItem table
            todoTable = client.GetSyncTable<ToDoItem>();
        }

4. **LoginAndGetData** という名前の新しい非同期パブリック メソッドを作成し、次のように定義します。

        public async Task LoginAndGetData(MonoTouch.UIKit.UIViewController view)
        {
            await Authenticate(view);
            await CreateTable();
        }

5. **TodoListViewController** で、**ViewDidAppear** メソッドをオーバーライドし、次のように定義します。 これにより、ユーザーに対するハンドルが **ToDoService** にまだない場合は、ユーザーにログインします。

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

6. **RefreshAsync** の元の呼び出しを **TodoListViewController.ViewDidLoad** から削除します。

7. **[Run]** を押してプロジェクトをビルドし、iPhone エミュレーターでアプリケーションを起動して、選択した ID プロバイダーでログオンします。

    ログインに成功すると、アプリケーションはエラーなしで実行されます。また、Mobile Services を照会してデータを更新できるようになります。

## 完成したサンプルの入手

[完成したサンプル プロジェクト] をダウンロードします。 **applicationURL** 変数と **applicationKey** 変数を独自の Azure 設定で更新してください。

## <a name="next-steps"></a>次のステップ

次は、チュートリアルでは、[ユーザーの認証のスクリプト] で、ユーザー ID 値になりますが、認証されたユーザーに基づいてモバイル サービスによって提供され、モバイル サービスから返されたデータをフィルター処理に使用します。





[register your app for authentication and configure mobile services]: #register 
[restrict table permissions to authenticated users]: #permissions 
[add authentication to the app]: #add-authentication 
[next steps]: #next-steps 
[4]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-services-selection.png 
[5]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-service-uri.png 
[13]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-identity-tab.png 
[14]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-portal-data-tables.png 
[15]: ./media/partner-xamarin-mobile-services-ios-get-started-users/mobile-portal-change-table-perms.png 
[submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582 
[my applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039 
[live sdk for windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253 
[get started with mobile services]: /develop/mobile/tutorials/get-started-xamarin-ios 
[get started with data]: /develop/mobile/tutorials/get-started-with-data-xamarin-ios 
[get started with authentication]: /develop/mobile/tutorials/get-started-with-users-xamarin-ios 
[get started with push notifications]: /develop/mobile/tutorials/-get-started-with-push-xamarin-ios 
[authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-xamarin-ios 
[completed example project]: http://go.microsoft.com/fwlink/p/?LinkId=331328 
[xamarin.ios]: http://xamarin.com/download 

