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

このチュートリアルでアプリケーションを更新して、[Mobile Services クイック スタート チュートリアル] をオフライン同期を有効にし、オフラインでのデータを追加することで、アプリをテストからそれらの項目の同期中、オンライン データベースと Azure クラシック ポータルで変更内容を確認します。

オフラインかオンラインかにかかわらず、複数の変更をデータに加えると競合が発生する場合があります。 今後のチュートリアルでは、いずれのバージョンの変更を承認すべきかを選択することになる、同期の競合を掘り下げて説明します。 このチュートリアルでは、同期の競合を想定しないで、既存データに加えたすべての変更が Azure SQL Server に直接適用されるものとします。


## はじめにやるべきこと

[AZURE.INCLUDE [mobile-services-android-prerequisites](../../includes/mobile-services-android-prerequisites.md)]

## オフライン同期をサポートするようにアプリケーションを更新する

オフライン同期では、*同期テーブル*に対して読み取りと書き込みを行います (*IMobileServiceSyncTable* インターフェイスを使用)。このテーブルは、デバイス上の **SQLite** データベースの一部です。

デバイスと Azure Mobile Services の間で変更をプッシュしたりプルしたりするには、*同期コンテキスト* (*MobileServiceClient.SyncContext*) を使用します。この同期コンテキストは、データをローカルに格納するローカル データベースで初期化されます。

1. このコードを *AndroidManifest.xml* ファイルに追加することで、ネットワーク接続を確認する権限を追加します 。

        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

2. 次の **import** ステートメントを *ToDoActivity.java* に追加します。

     import java.util.Map;
    
     import android.widget.Toast;
    
     import com.microsoft.windowsazure.mobileservices.table.query.Query;
     import com.microsoft.windowsazure.mobileservices.table.sync.MobileServiceSyncContext;
     import com.microsoft.windowsazure.mobileservices.table.sync.MobileServiceSyncTable;
     import com.microsoft.windowsazure.mobileservices.table.sync.localstore.ColumnDataType;
     import com.microsoft.windowsazure.mobileservices.table.sync.localstore.SQLiteLocalStore;

3. 上部、 `ToDoActivity` クラスでの宣言を変更する、 `mToDoTable` から変数、 `MobileServiceTable < ToDoItem >` クラスを `MobileServiceSyncTable < ToDoItem >` クラスです。

         private MobileServiceSyncTable<ToDoItem> mToDoTable;

    ここでは、同期テーブルが定義されます。

4. ローカル ストアの初期化を処理する、 `onCreate` メソッドを作成した後、次の行を追加、 `MobileServiceClient` インスタンス。

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

5. 上記のコード内にある、 `と` ブロックを追加 `キャッチ` ブロックの後、 `MalformedURLException` いずれか。

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

7. このメソッドを追加すると、ローカルの *SQL Light* ストアと Azure SQL Server が同期されます。

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

8. `OnCreate` への呼び出しの直前のメソッドに次の最終行として次のコードを追加、 `refreshItemsFromTable`:

            syncAsync();

    これにより、スタートアップしているデバイスが Azure のテーブルと同期されます。 このコードを追加しないと、ローカル ストアの最後のオフライン コンテンツが表示されます。

9. コードを更新、 `refreshItemsFromTable` このクエリを使用する方法 (内部のコードの最初の行、 `と` ブロック)。

        final MobileServiceList<ToDoItem> result = mToDoTable.read(mPullQuery).get();

10. `OnOptionsItemSelected` メソッドの内容に置き換えます、 `場合` このコードをブロックします。

            syncAsync();
            refreshItemsFromTable();

    このコードは、右上にある [**更新**] ボタンを押すと実行されます。 これは、スタートアップ時の同期の他に、ローカル ストアを Azure に同期させるための主な手段です。 これによって同期変更のバッチ処理が促進されるため、Azure からのプルが比較的負荷が高い操作であることを考えると、効率的です。 呼び出しを追加することによって変更されるたびに同期するには、このアプリケーションを設計することもでした `syncAsync` に、 `addItem` と `checkItem` メソッドでは、アプリケーションで必要です。


## アプリケーションをテストする

このセクションでは、WiFi 接続した状態で動作をテストしてから、WiFi を切断してオフラインのシナリオを用意します。

データ項目を追加したとき、これらの項目はローカルの SQL Light ストアに保持されていますが、[**更新**] ボタンをクリックするまではモバイル サービスと同期されていません。  他のアプリでは、データを同期させるタイミングに関する要件が異なる場合がありますが、このチュートリアルではデモの目的で、ユーザーが明示的にデータ同期を要求したことにします。

[更新] ボタンを押すと、新しいバック グラウンド タスクが開始し、最初に、同期コンテキストを使用してローカル ストアに加えられたすべての変更がプッシュされます。次に、Azure から変更されたすべてのデータがローカル テーブルにプルされます。


### オンライン テスト

次のシナリオでテストしてみましょう。

1. デバイスにいくつかの新しい項目を追加します。
2. ポータルで項目が表示されていないことを確認します。
3. 次に [**更新**] を押して、これらの項目が表示されることを確認します。
4. ポータルで項目を変更するか追加してから [**更新**] を押して、変更がデバイス上に表示されることを確認します。

### オフライン テスト

1. デバイスまたはシミュレーターを [*機内モード*] に切り替えます。 これでオフラインのシナリオが準備できます。

2. *ToDo* 項目をいくつか追加するか、一部の項目を完了済みとしてマークします。 デバイスまたはシミュレーターを終了して (またはアプリケーションを強制的に閉じて)、再起動します。 加えた変更がデバイスに保存されたことを確認します。これらの変更はローカルの SQL  Light ストアで保持されているためです。

3. Azure の *TodoItem* テーブルの内容を表示します。 新しい項目が、サーバーと同期_されなかった_ことを確認します。

   - JavaScript バックエンドでは、Azure 旧ポータルに移動し、[データ] タブの内容を表示する] をクリックして、 `TodoItem` テーブルです。
   - .NET バックエンドの場合は、*SQL Server Management Studio* のような SQL ツールまたは *Fiddler* や *Postman* のような REST クライアントを使用して、テーブルの内容を表示します。

4. デバイスまたはシミュレーターの WiFi を有効にします。 次に [**更新**] ボタンを押します。

5. Azure クラシック ポータルで、もう一度 TodoItem のデータを参照します。 新しく変更した TodoItems が表示されます。


## 次のステップ

* [Mobile Services ][soft delete]

## その他のリソース

* [カバーをクラウド: Azure Mobile Services でオフラインの同期]

* [Azure Friday: Azure Mobile Services でのオフライン対応のアプリケーション] \ (注: デモは、Windows では機能の説明は、すべての platforms \ に適用されます)





[get the sample app]: #get-app 
[review the core data model]: #review-core-data 
[review the mobile services sync code]: #review-sync 
[change the sync behavior of the app]: #setup-sync 
[test the app]: #test-app 
[mobile services sample repository on github]: https://github.com/Azure/mobile-services-samples 
[get started with mobile services]: mobile-services-android-get-started.md 
[handling conflicts with offline support for mobile services]: mobile-services-android-handling-conflicts-offline-data.md 
[soft delete]: mobile-services-using-soft-delete.md 
[cloud cover: offline sync in azure mobile services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri 
[azure friday: offline-enabled apps in azure mobile services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/ 
[mobile services quick start tutorial]: mobile-services-android-get-started.md 

