<properties
    pageTitle="認証の使用 (Xamarin.Android) - Mobile Services"
    description="Xamarin.Android 向け Azure Mobile Services アプリケーションで認証を使用する方法について説明します。"
    services="mobile-services"
    documentationCenter="xamarin"
    manager="dwrede"
    authors="lindydonna"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/30/2015" 
    ms.author="donnam"/>

# Mobile Services アプリへの認証の追加

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

<p>このトピックでは、Xamarin.Android アプリから Azure Mobile Services のユーザーを認証する方法について説明します。 このチュートリアルでは、Mobile Services でサポートされている ID プロバイダーを使用して、クイック スタート プロジェクトに認証を追加します。 Mobile Services によって正常に認証および承認されると、ユーザー ID 値が表示されます。</p>

このチュートリアルでは、アプリケーションでの認証を有効にするための、次の基本的な手順について説明します。

1. [アプリケーションを認証に登録し、モバイル サービスを構成する]
2. [テーブルのアクセス許可を、認証されたユーザーだけに制限する]
3. [アプリケーションに認証を追加する]

このチュートリアルは、モバイル サービスのクイック スタートに基づいています。 また、最初のチュートリアルを完了する必要があります [Get started with Mobile Services]します。

このチュートリアルを完了するには、Xamarin.Android および Android SDK 4.2 以降のバージョンが必要です。

##<a name="register"></a>アプリケーションを認証に登録し、Mobile Services を構成する

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

##<a name="permissions"></a>アクセス許可を、認証されたユーザーだけに制限する


[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)]


3. Xamarin Studio で、チュートリアルを完了したときに作成したプロジェクトを開く [Get started with Mobile Services]します。

4.  **実行** ] メニューのをクリックして **デバッグを開始** にアプリを起動します。 アプリケーションの開始後に、状態コード 401 (許可されていません) のハンドルされない例外が発生することを確認します。

     これは、問題は、アプリケーションは、認証されないユーザーとしてモバイル サービスにアクセスしようとしていますが、 _TodoItem_ テーブルには、今すぐ認証が必要です。

次に、モバイル サービスのリソースを要求する前にユーザーを認証するようにアプリケーションを更新します。

##<a name="add-authentication"></a>アプリケーションに認証を追加する

1. 次のプロパティを追加、 **ToDoActivity** クラス。

        private MobileServiceUser user;

2. 次のメソッドを追加、 **ToDoActivity** クラス。

        private async Task Authenticate()
        {
            try
            {
                user = await client.LoginAsync(this, MobileServiceAuthenticationProvider.MicrosoftAccount);
                CreateAndShowDialog(string.Format("you are now logged in - {0}", user.UserId), "Logged in!");
            }
            catch (Exception ex)
            {
                CreateAndShowDialog(ex, "Authentication failed");
            }
        }

    これで、認証プロセスを処理する新しいメソッドが作成されます。 ユーザーは、Microsoft アカウント ログインを使用して認証されます。 認証されたユーザーの ID を示すダイアログが表示されます。 認証が成功しないと、次に進むことはできません。

    > [AZURE.NOTE] Microsoft 以外の id プロバイダーを使用している場合に渡される値を変更、 **ログイン** 上、次のいずれかのメソッド: _Facebook_, 、_Google_, 、_Twitter_, 、または _WindowsAzureActiveDirectory_します。

3.  **OnCreate** メソッドをインスタンス化するコードの後に次のコード行を追加、 `MobileServiceClient` オブジェクトです。

        await Authenticate();

    この呼び出しは、認証プロセスを開始し、認証プロセスを非同期に待機します。

4. 後の残りのコードを移動 `await Authenticate();` で、 **OnCreate** を新しいメソッド **CreateTable** メソッドを次のようにします。

        private async Task CreateTable()
        {

            await InitLocalStoreAsync();

            // Get the Mobile Service Table instance to use
            toDoTable = client.GetSyncTable<ToDoItem>();

            textNewToDo = FindViewById<EditText>(Resource.Id.textNewToDo);

            // Create an adapter to bind the items with the view
            adapter = new ToDoItemAdapter(this, Resource.Layout.Row_List_To_Do);
            var listViewTodo = FindViewById<ListView>(Resource.Id.listViewToDo);
            listViewTodo.Adapter = adapter;

            // Load the items from the Mobile Service
            await RefreshItemsFromTableAsync();
        }

5. まず、新しい **CreateTable** メソッド **OnCreate** 後、 **認証** 手順 2. で追加の呼び出し。

        await CreateTable();


6.  **実行** ] メニューのをクリックして **デバッグを開始** にアプリを起動して、選択した id プロバイダーでサインインします。

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
[4]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-services-selection.png
[5]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-service-uri.png

[13]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-identity-tab.png
[14]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-portal-data-tables.png
[15]: ./media/partner-xamarin-mobile-services-android-get-started-users/mobile-portal-change-table-perms.png

<!-- URLs. -->
[Get started with Mobile Services]: partner-xamarin-mobile-services-android-get-started.md
[Authorize users with scripts]: mobile-services-javascript-backend-service-side-authorization.md
[completed example project]: http://go.microsoft.com/fwlink/p/?LinkId=331328


