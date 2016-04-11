モバイル サービスの準備が整ったら、ローカル コレクションの代わりに Mobile Services に項目を格納するようにアプリケーションを更新します。 

1. 次の行があることを確認、 **の依存関係** でタグ付け、 *build.gradle (Module app)* ファイル、および存在場合は追加されません。 これにより、Mobile Services Android クライアント SDK へのリファレンスが追加されます。

        compile 'com.android.support:support-v4:21.0.3'
        compile 'com.google.code.gson:gson:2.2.2'
        compile 'com.google.guava:guava:18.0'
        compile 'com.microsoft.azure:azure-mobile-services-android-sdk:2.0.2+'


2. 今すぐ] をクリックして、プロジェクトをリビルド **Sync Project with Gradle Files**します。

3. AndroidManifest.xml ファイルを開き、次の行を追加します。その結果、アプリケーションが Azure 上の Mobile Services にアクセスできるようになります。

        <uses-permission android:name="android.permission.INTERNET" />


4. プロジェクト エクスプ ローラーで TodoActivity.java ファイルを開く、 **GetStartedWithData = > app = > src = > java** フォルダー、次のコード行をコメント解除します。 



        import java.net.MalformedURLException;
        import android.os.AsyncTask;
        import com.google.common.util.concurrent.FutureCallback;
        import com.google.common.util.concurrent.Futures;
        import com.google.common.util.concurrent.ListenableFuture;
        import com.microsoft.windowsazure.mobileservices.MobileServiceClient;
        import com.microsoft.windowsazure.mobileservices.MobileServiceList;
        import com.microsoft.windowsazure.mobileservices.http.NextServiceFilterCallback;
        import com.microsoft.windowsazure.mobileservices.http.ServiceFilter;
        import com.microsoft.windowsazure.mobileservices.http.ServiceFilterRequest;
        import com.microsoft.windowsazure.mobileservices.http.ServiceFilterResponse;
        import com.microsoft.windowsazure.mobileservices.table.MobileServiceTable;

 
5. 次の行をコメント アウトします。

        import java.util.ArrayList;
        import java.util.List;

6. 次に、現在アプリケーションで使用されているメモリ内のリストを削除して、モバイル サービスで置き換えます。  **ToDoActivity** クラスの定義、既存のコードの次の行をコメント アウト **toDoItemList** ] ボックスの一覧です。

        public List<ToDoItem> toDoItemList = new ArrayList<ToDoItem>();

7. ファイルを保存すると、プロジェクトがビルド エラーを示します。 `toDoItemList` 変数が使用されている残りの 3 か所を検索し、該当するセクションをコメント解除します。 これで、メモリ内のリストが完全に削除されます。 

8. 次に、モバイル サービスを追加します。 次のコード行をコメント解除します。

        private MobileServiceClient mClient;
        private private MobileServiceTable<ToDoItem> mToDoTable;

9. 検索、 *ProgressFilter* ファイルの下部にあるクラスし、コメント解除します。 このクラスは、'読み込み中"インジケーターを表示 *MobileServiceClient* ネットワーク操作を実行しています。


10. Azure クラシック ポータルで、クリックして **Mobile Services**, 、し、先ほど作成したモバイル サービスをクリックします。

11. クリックして、 **ダッシュ ボード** タブし、メモ、 **サイトの URL**, 、順にクリックして **キーの管理** のメモに記録、 **アプリケーション キー**します。

    ![](./media/download-android-sample-code/mobile-dashboard-tab.png)

    これらの値は、アプリケーション コードからモバイル サービスにアクセスするときに必要になります。

12.  **OnCreate** メソッドを定義する次のコード行をコメント解除、 **MobileServiceClient** 変数。

        try {
        // Create the Mobile Service Client instance, using the provided
        // Mobile Service URL and key
            mClient = new MobileServiceClient(
                    "MobileServiceUrl",
                    "AppKey", 
                    this).withFilter(new ProgressFilter());

            // Get the Mobile Service Table instance to use
            mToDoTable = mClient.getTable(ToDoItem.class);
        } catch (MalformedURLException e) {
            createAndShowDialog(new Exception("There was an error creating the Mobile Service. Verify the URL"), "Error");
        }

    新しいインスタンスを作成 *MobileServiceClient* 、モバイル サービスにアクセスするために使用されます。 作成、 *MobileServiceTable* 、モバイル サービスにデータ ストレージをプロキシに使用されるインスタンスです。

13. 前のコードの `MobileServiceUrl` と `AppKey` を、モバイル サービスの URL とアプリケーション キーでそれぞれ置き換えます。



14. 次の行、 **checkItem** メソッド。

        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    mToDoTable.update(item).get();
                    runOnUiThread(new Runnable() {
                        public void run() {
                            if (item.isComplete()) {
                                mAdapter.remove(item);
                            }
                            refreshItemsFromTable();
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();

    これにより、項目の更新がモバイル サービスに送信され、チェックされた項目がアダプターから削除されます。
    
15. 次の行、 **addItem** メソッド。
    
        // Insert the new item
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    mToDoTable.insert(item).get();
                    if (!item.isComplete()) {
                        runOnUiThread(new Runnable() {
                            public void run() {
                                mAdapter.add(item);
                            }
                        });
                    }
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
        

    このコードでは、新しい項目を作成し、それをリモート モバイル サービスのテーブルに挿入します。

16. 次の行、 **refreshItemsFromTable** メソッド。

        // Get the items that weren't marked as completed and add them in the adapter
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final MobileServiceList<ToDoItem> result = mToDoTable.where().field("complete").eq(false).execute().get();
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            mAdapter.clear();

                            for (ToDoItem item : result) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();

    このコードでは、モバイル サービスに対するクエリを実行して、完了マークが付けられていないすべての項目を取得します。 項目は、バインド用にアダプターに追加されます。
        

<!-- URLs. -->
[Mobile Services Android SDK]: http://aka.ms/Iajk6q
