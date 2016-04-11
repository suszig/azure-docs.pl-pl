<properties
    pageTitle="オフライン データ同期を Android Mobile Services アプリに追加する | Microsoft Azure"
    description="Azure Mobile Services を使用して、Android アプリケーションのオフライン データをキャッシュおよび同期する方法を学習します。"
    documentationCenter="android"
    authors="RickSaling"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="12/06/2015"
    ms.author="ricksal"/>

# オフライン データ同期を Android Mobile Services アプリケーションに追加する

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;

[AZURE.INCLUDE [mobile-services-selector-offline](../../includes/mobile-services-selector-offline.md)]

## 概要

モバイル アプリのネットワーク接続は、サービスのない地域に移動したときや、ネットワークの問題によって切断されることがあります。 たとえば、建設業界用アプリを使用する際、遠くの建設現場でスケジュール データを入力し、後で Azure に同期する場合があります。 Azure Mobile Services のオフライン同期は、ネットワーク接続が失われたときにも作業を続行できるため、多くのモバイル アプリに不可欠な機能です。 オフライン同期を使用すると、Azure SQL Server テーブルのローカル コピーで作業するため、これら 2 つが定期的に再同期されます。

このチュートリアルでアプリケーションを更新してから、 [Mobile Services Quick Start tutorial] をオフラインの同期を有効にし、オフラインでのデータを追加、オンライン データベースにそれらの項目の同期、および Azure クラシック ポータルで変更内容を確認して、アプリをテストします。

オフラインかオンラインかにかかわらず、複数の変更をデータに加えると競合が発生する場合があります。  今後のチュートリアルでは、いずれのバージョンの変更を承認すべきかを選択することになる、同期の競合を掘り下げて説明します。 このチュートリアルでは、同期の競合を想定しないで、既存データに加えたすべての変更が Azure SQL Server に直接適用されるものとします。


## はじめにやるべきこと

[AZURE.INCLUDE [mobile-services-android-prerequisites](../../includes/mobile-services-android-prerequisites.md)]

## オフライン同期をサポートするようにアプリケーションを更新する

オフライン同期に対して読み取りや書き込みを *同期テーブル* (を使用して、 *IMobileServiceSyncTable* インターフェイス)、含まれる、 **SQLite** 、デバイス上のデータベースです。

プッシュして、デバイスと Azure Mobile Services の間での変更をプル、使用、 *同期コンテキスト* (*MobileServiceClient.SyncContext*) を使用してローカルでのデータを格納するローカルのデータベースで初期化します。

1. このコードを追加して、ネットワーク接続を確認する権限を追加、 *AndroidManifest.xml* ファイル。

        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />


2. 次の追加 **インポート** ステートメントを *ToDoActivity.java*:

        import java.util.Map;

        import android.widget.Toast;

        import com.microsoft.windowsazure.mobileservices.table.query.Query;
        import com.microsoft.windowsazure.mobileservices.table.sync.MobileServiceSyncContext;
        import com.microsoft.windowsazure.mobileservices.table.sync.MobileServiceSyncTable;
        import com.microsoft.windowsazure.mobileservices.table.sync.localstore.ColumnDataType;
        import com.microsoft.windowsazure.mobileservices.table.sync.localstore.SQLiteLocalStore;

3. `ToDoActivity` クラスの上部で、`mToDoTable` 変数の宣言を `MobileServiceTable<ToDoItem>` クラスから `MobileServiceSyncTable<ToDoItem>` クラスに変更します。

         private MobileServiceSyncTable<ToDoItem> mToDoTable;

    ここでは、同期テーブルが定義されます。

4. ローカル ストアの初期化を処理するには、`onCreate` メソッドで、`MobileServiceClient` インスタンスを作成してから次の行を追加します。

            // Saves the query which will be used for pulling data
            mPullQuery = mClient.getTable(ToDoItem.class).where().field("complete").eq(false);

            SQLiteLocalStore localStore = new SQLiteLocalStore(mClient.getContext(), "ToDoItem", null, 1);
            SimpleSyncHandler handler = new SimpleSyncHandler();
            MobileServiceSyncContext syncContext = mClient.getSyncContext();

            Map<String, ColumnDataType> tableDefinition = new HashMap<String, ColumnDataType>();
            tableDefinition.put("id", ColumnDataType.String);
            tableDefinition.put("text", ColumnDataType.String);
            tableDefinition.put("complete", ColumnDataType.Boolean);
            tableDefinition.put("__version", ColumnDataType.String);

            localStore.defineTable("ToDoItem", tableDefinition);
            syncContext.initialize(localStore, handler).get();

            // Get the Mobile Service Table instance to use
            mToDoTable = mClient.getSyncTable(ToDoItem.class);

5. `try` ブロック内にある上記のコードで、`MalformedURLException` ブロックの後に `catch` ブロックを追加します。

        } catch (Exception e) {
            Throwable t = e;
            while (t.getCause() != null) {
                    t = t.getCause();
                }
            createAndShowDialog(new Exception("Unknown error: " + t.getMessage()), "Error");
        }

6. このメソッドを追加すると、ネットワーク接続を確認できます。

        private boolean isNetworkAvailable() {
            ConnectivityManager connectivityManager
                    = (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
            NetworkInfo activeNetworkInfo = connectivityManager.getActiveNetworkInfo();
            return activeNetworkInfo != null && activeNetworkInfo.isConnected();
        }


7. ローカル間で同期するには、このメソッドを追加 *SQL Light* ストアと Azure の SQL Server:

        public void syncAsync(){
            if (isNetworkAvailable()) {
                new AsyncTask<Void, Void, Void>() {

                    @Override
                    protected Void doInBackground(Void... params) {
                        try {
                            mClient.getSyncContext().push().get();
                            mToDoTable.pull(mPullQuery).get();

                        } catch (Exception exception) {
                            createAndShowDialog(exception, "Error");
                        }
                        return null;
                    }
                }.execute();
            } else {
                Toast.makeText(this, "You are not online, re-sync later!" +
                        "", Toast.LENGTH_LONG).show();
            }
        }


8. `onCreate` メソッドで、`refreshItemsFromTable` を呼び出す直前の、最後から 2行目にこのコードを追加します。

            syncAsync();

    これにより、スタートアップしているデバイスが Azure のテーブルと同期されます。 このコードを追加しないと、ローカル ストアの最後のオフライン コンテンツが表示されます。



9. このクエリ (`try` ブロック内の最初のコード行) が使用されるように、`refreshItemsFromTable` メソッドのコードを更新します。

        final MobileServiceList<ToDoItem> result = mToDoTable.read(mPullQuery).get();

10. `onOptionsItemSelected` メソッドで、`if` ブロックのコンテンツを次のコードで置き換えます。

            syncAsync();
            refreshItemsFromTable();

    キーを押したときに、このコードが実行される、 **更新** 画面の右上隅のボタンをクリックします。 これは、スタートアップ時の同期の他に、ローカル ストアを Azure に同期させるための主な手段です。 これによって同期変更のバッチ処理が促進されるため、Azure からのプルが比較的負荷が高い操作であることを考えると、効率的です。 アプリケーションで必要ならば、`syncAsync` への呼び出しを `addItem` メソッドと `checkItem` メソッドに追加することで、変更が加えられるごとに同期されるようにこのアプリを設定することもできます。


## アプリケーションをテストする

このセクションでは、WiFi 接続した状態で動作をテストしてから、WiFi を切断してオフラインのシナリオを用意します。

データ項目を追加すると、ローカルの SQL Light ストアに保持されているがキーを押すまで、モバイル サービスに同期されていません、 **更新** ] ボタンをクリックします。 他のアプリでは、データを同期させるタイミングに関する要件が異なる場合がありますが、このチュートリアルではデモの目的で、ユーザーが明示的にデータ同期を要求したことにします。

[更新] ボタンを押すと、新しいバック グラウンド タスクが開始し、最初に、同期コンテキストを使用してローカル ストアに加えられたすべての変更がプッシュされます。次に、Azure から変更されたすべてのデータがローカル テーブルにプルされます。


### オンライン テスト

次のシナリオでテストしてみましょう。

1. デバイスにいくつかの新しい項目を追加します。
2. ポータルで項目が表示されていないことを確認します。
3. 次のキーを押して **更新** し、項目が表示されるを確認します。
4. 変更するアイテムの追加など、ポータルで、キーを押します **更新** し、変更が、デバイス上に表示されることを確認します。

### オフライン テスト

<!-- Now if you run the app and tap the refresh button, you should see all the items from the server. At that point you should be able to turn off the networking from the device by placing it in *Airplane Mode*, and continue making changes – the app will work just fine. When it’s time to sync the changes to the server, turn the network back on, and tap the **Refresh** button again.

One thing which is important to point out: if there are pending changes in the local store, a pull operation will first push those changes to the server (so that if there are changes in the same row, the push operation will fail and the application has an opportunity to handle the conflicts appropriately). That means that the push call in the code above isn’t necessarily required, but I think it’s always a good practice to be explicit about what the code is doing.
-->

1. デバイスまたはシミュレーターを [ *機内モード*します。 これでオフラインのシナリオが準備できます。

2. いくつか追加 *ToDo* アイテム、または一部の項目を完了済みとしてマークします。 デバイスまたはシミュレーターを終了して (またはアプリケーションを強制的に閉じて)、再起動します。 加えた変更がデバイスに保存されたことを確認します。これらの変更はローカルの SQL  Light ストアで保持されているためです。

3. Azure の内容を表示 *TodoItem* テーブルです。 新しい項目があることを確認して _いない_ されて、サーバーと同期します。

   - JavaScript バックエンドの場合は、Azure クラシック ポータルで [データ] タブをクリックして、`TodoItem` テーブルの内容を表示します。
   - .NET バックエンドの場合、sql ツールなど、テーブルの内容を表示 *SQL Server Management Studio*, 、やなどの REST クライアント *Fiddler* または *Postman*します。

4. デバイスまたはシミュレーターの WiFi を有効にします。 次に、キーを押して、 **更新** ] ボタンをクリックします。

5. Azure クラシック ポータルで、もう一度 TodoItem のデータを参照します。 新しく変更した TodoItems が表示されます。


## 次のステップ

* [Mobile Services での論理削除の使用方法][Soft Delete]

## その他のリソース

* [Cloud Cover: Azure Mobile Services でのオフライン同期]

* [Azure Friday: Offline-enabled apps in Azure Mobile Services] \ (注: デモは Windows では機能の説明はすべての platforms \ に適用されます)


<!-- URLs. -->

[Get the sample app]: #get-app
[Review the Core Data model]: #review-core-data
[Review the Mobile Services sync code]: #review-sync
[Change the sync behavior of the app]: #setup-sync
[Test the app]: #test-app


[Mobile Services sample repository on GitHub]: https://github.com/Azure/mobile-services-samples


[Get started with Mobile Services]: mobile-services-android-get-started.md
[Handling Conflicts with Offline Support for Mobile Services]:  mobile-services-android-handling-conflicts-offline-data.md
[Soft Delete]: mobile-services-using-soft-delete.md

[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

[Mobile Services Quick Start tutorial]: mobile-services-android-get-started.md


