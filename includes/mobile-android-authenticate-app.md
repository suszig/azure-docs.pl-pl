
1.  **プロジェクト エクスプ ローラー** Android Studio で、ToDoActivity.java ファイルを開き、次の import ステートメントを追加します。

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

2. 次のメソッドを追加、 **ToDoActivity** クラス。 
    
        private void authenticate() {
            // Login using the Google provider.
            
            ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);
    
            Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                @Override
                public void onFailure(Throwable exc) {
                    createAndShowDialog((Exception) exc, "Error");
                }           
                @Override
                public void onSuccess(MobileServiceUser user) {
                    createAndShowDialog(String.format(
                            "You are now logged in - %1$2s",
                            user.getUserId()), "Success");
                    createTable();  
                }
            });     
        }


    これで、認証プロセスを処理する新しいメソッドが作成されます。 ユーザーは、Google ログインを使用して認証されます。 認証されたユーザーの ID を示すダイアログが表示されます。 認証が成功しないと、次に進むことはできません。

    > [AZURE.NOTE] Google 以外の id プロバイダーを使用している場合に渡される値を変更、 **ログイン** 上、次のいずれかのメソッド: _MicrosoftAccount_, 、_Facebook_, 、_Twitter_, 、または _windowsazureactivedirectory_します。

3.  **OnCreate** メソッドをインスタンス化するコードの後に次のコード行を追加、 `MobileServiceClient` オブジェクトです。

        authenticate();

    この呼び出しで、認証プロセスが開始されます。

4. 後の残りのコードを移動 `authenticate();` で、 **onCreate** を新しいメソッド **createTable** メソッドを次のようにします。

        private void createTable() {
    
            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);
    
            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);
    
            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);
    
            // Load the items from Azure.
            refreshItemsFromTable();
        }

9.  **実行** ] メニューのをクリックし、 **アプリケーションを実行する** にアプリを起動して、選択した id プロバイダーでサインインします。 

    ログインに成功すると、アプリケーションはエラーなしで実行されます。また、バックエンド サービスに照会したり、データを更新したりできるようになります。
