<properties
    pageTitle="Azure Mobile App (Android) のオフライン同期の有効化"
    description="App Service Mobile Apps を使用して、Android アプリケーションのオフライン データをキャッシュおよび同期する方法について説明します。"
    documentationCenter="android"
    authors="lindydonna"
    manager="dwrede"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="12/01/2015"
    ms.author="donnam"/>

# Android モバイル アプリのオフライン同期の有効化

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

## 概要

このチュートリアルでは、Android 向け Azure Mobile Apps のオフライン同期機能について説明します。 オフライン同期を使用すると、エンド ユーザーはネットワークにアクセスできなくても、データの表示、追加、変更など、モバイル アプリとやり取りできます。 変更はローカル データベースに格納され、デバイスが再びオンラインになると、これらの変更がリモート バックエンドと同期されます。

このチュートリアルを完了してください Azure モバイル アプリを使用して初めての場合は、 [Create an Android App]します。 ダウンロードしたクイック スタートのサーバー プロジェクトを使用しない場合は、データ アクセス拡張機能パッケージをプロジェクトに追加する必要があります。 サーバーの拡張機能パッケージの詳細については、次を参照してください。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)します。 

オフライン同期機能の詳細については、トピックを参照してください。 [Offline Data Sync in Azure Mobile Apps]します。

## オフライン同期をサポートするようにアプリケーションを更新する

オフライン同期に対して読み取りや書き込みを *同期テーブル* (を使用して、 *IMobileServiceSyncTable* インターフェイス)、含まれる、 **SQLite** 、デバイス上のデータベースです。

プッシュして、デバイスと Azure Mobile Services の間での変更をプル、使用、 *同期コンテキスト* (*MobileServiceClient.SyncContext*) を使用してローカルでのデータを格納するローカルのデータベースで初期化します。

1. `TodoActivity.java` で、`mToDoTable` の既存の定義をコメント アウトし、同期テーブルのバージョンをコメント解除します。
    
        private MobileServiceSyncTable<ToDoItem> mToDoTable;

2. `onCreate` メソッドで、`mToDoTable` の既存の初期化をコメント アウトし、次の定義をコメント解除します。

        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);

3. `refreshItemsFromTable` で、`results` の定義をコメント アウトし、次の定義をコメント解除します。

        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();

4. `refreshItemsFromMobileServiceTable` の定義をコメント アウトします。

5. `refreshItemsFromMobileServiceTableSyncTable` の定義をコメント解除します。

        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }

6. `sync` の定義をコメント解除します。

        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## アプリケーションをテストする

このセクションでは、WiFi 接続した状態で動作をテストしてから、WiFi を切断してオフラインのシナリオを用意します。

データ項目を追加すると、ローカルの SQLite ストアに保持されているがキーを押すまで、モバイル サービスに同期されていません、 **更新** ] ボタンをクリックします。 他のアプリでは、データを同期させるタイミングに関する要件が異なる場合がありますが、このチュートリアルではデモの目的で、ユーザーが明示的にデータ同期を要求したことにします。

[更新] ボタンを押すと、新しいバック グラウンド タスクが開始し、最初に、同期コンテキストを使用してローカル ストアに加えられたすべての変更がプッシュされます。次に、Azure から変更されたすべてのデータがローカル テーブルにプルされます。

### オフライン テスト

1. デバイスまたはシミュレーターを [ *機内モード*します。 これでオフラインのシナリオが準備できます。

2. いくつか追加 *ToDo* アイテム、または一部の項目を完了済みとしてマークします。 デバイスまたはシミュレーターを終了して (またはアプリケーションを強制的に閉じて)、再起動します。 変更はローカルの SQLite ストアに保持されているため、変更がデバイスに保存されたことを確認します。

3. Azure の内容を表示 *TodoItem* など、SQL ツールを使用していずれかのテーブル *SQL Server Management Studio*, 、やなどの REST クライアント *Fiddler* または *Postman*します。 新しい項目があることを確認して _いない_ されて、サーバーと同期

    + Node.js バックエンドを参照してください、 [Azure ポータル](https://portal.azure.com/), 、モバイル アプリ バックエンド クリック **簡単なテーブル** > **TodoItem** の内容を表示する、 `TodoItem` テーブルです。
    + .NET バックエンドでは、テーブルの内容を sql ツールなどを表示 *SQL Server Management Studio*, 、やなどの REST クライアント *Fiddler* または *Postman*します。

4. デバイスまたはシミュレーターの WiFi を有効にします。 次に、キーを押して、 **更新** ] ボタンをクリックします。

5. Azure ポータルで、もう一度 TodoItem のデータを参照します。 新しく変更した TodoItems が表示されます。

## その他のリソース

* [Azure モバイル アプリでのオフライン データ同期]

* [Cloud Cover: Offline Sync in Azure Mobile Services] \ (注: ビデオは、モバイル サービスには、Azure モバイル アプリで同様の方法でのオフライン同期機能)


<!-- URLs. -->

[Offline Data Sync in Azure Mobile Apps]: ../app-service-mobile-offline-data-sync.md

[Create an Android App]: ../app-service-mobile-android-get-started.md

[Cloud Cover: Offline Sync in Azure Mobile Services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/


