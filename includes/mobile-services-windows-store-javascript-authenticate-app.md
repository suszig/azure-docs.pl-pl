
1. default.js プロジェクト ファイルを開き、**app.OnActivated** メソッドのオーバーロード内で、**refreshTodoItems** メソッドの最後の呼び出しを次のコードで置き換えます。

     var userId = null;
    
     // Request authentication from Mobile Services using a Facebook login.
     var login = function () {
         return new WinJS.Promise(function (complete) {
             client.login("facebook").done(function (results) {
                 userId = results.userId;
                 refreshTodoItems();
                 var message = "You are now logged in as: " + userId;
                 var dialog = new Windows.UI.Popups.MessageDialog(message);
                 dialog.showAsync().done(complete);
             }, function (error) {
                 userId = null;
                 var dialog = new Windows.UI.Popups
                     .MessageDialog("An error occurred during login", "Login Required");
                 dialog.showAsync().done(complete);
             });
         });
     }            
    
     var authenticate = function () {
         login().then(function () {
             if (userId === null) {
    
                 // Authentication failed, try again.
                 authenticate();
             }
         });
     }
    
     authenticate();

 これにより、現在のユーザーを格納するためのメンバー変数と認証プロセスを処理するためのメソッドが作成されます。 ユーザーは、Facebook ログインを使用して認証されます。 Facebook 以外の id プロバイダーを使用している場合に渡される値を変更、 <strong>ログイン</strong> 上、次のいずれかのメソッド: _microsoftaccount_, 、_twitter_, 、_google_, 、または _windowsazureactivedirectory_します。
 >[AZURE.NOTE]モバイル サービスの Windows ストア アプリ パッケージ情報が登録されている場合を呼び出す必要があります、 <a href="http://go.microsoft.com/fwlink/p/?LinkId=322050" target="_blank">ログイン</a> メソッドの値を指定して <strong>true</strong> の <em>useSingleSignOn</em> パラメーター。 この操作を行わない場合、login メソッドが呼び出されるたびに、ユーザーにログイン プロンプトが表示されます。

2. F5 キーを押してアプリケーションを実行し、選択した ID プロバイダーでアプリケーションにサインインします。

    ログインに成功すると、アプリケーションはエラーなしで実行されます。また、モバイル サービスを照会してデータを更新できるようになります。




