<properties
    pageTitle="Mobile Services でのオフライン データの使用 (Xamarin iOS) | Microsoft Azure"
    description="Azure Mobile Services を使用して、Xamarin iOS アプリケーションのオフライン データをキャッシュおよび同期する方法を説明します。"
    documentationCenter="xamarin"
    authors="lindydonna"
    editor="wesmc"
    manager="dwrede"
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/02/2015"
    ms.author="donnam"/>


# Mobile Services でのオフライン データの同期の使用

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-offline](../../includes/mobile-services-selector-offline.md)]

このトピックでは、Azure Mobile Services のオフライン同期機能について、todo リスト クイックスタート アプリケーションを使用して説明します。 オフライン同期により、エンド ユーザーがネットワークにアクセスできないときでも使用できるアプリケーションを容易に作成することができます。

オフライン同期には、いくつかの潜在的な用途があります。

* サーバー データをデバイスにローカルでキャッシュすることにより、アプリケーションの応答性を向上させる。
* 断続的なネットワーク接続に対してアプリケーションに弾力性を持たせる。
* エンド ユーザーがネットワークにアクセスできなくてもデータを作成および変更できるようにすることで、接続がほとんどまたはまったく得られないような状況をサポートする。
* 複数のデバイス間でデータを同期させ、同じレコードが 2 つのデバイスによって変更されたときに競合を検出する。

>[AZURE.NOTE] このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントがない場合は、Azure 試用版にサインアップして最大 10 の無料モバイル サービスを取得し、試用期間が終わった後でも使用し続けることができます。 詳細については、「 <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=AE564AB28" target="_blank">Azure 無料試用版</a>します。
>
> 初めてのモバイル サービスを使用する場合は、完了してください [を使ってみるモバイル サービス] です。

このチュートリアルでは、次の基本的な手順について説明します。

1. [Mobile Services 同期コードの確認]
2. [アプリケーションの同期の動作を更新する]
3. [モバイル サービスに再接続するようにアプリケーションを更新する]

このチュートリアルには、次のものが必要です。

* Visual Studio [Xamarin 拡張機能] と **または** [Xamarin Studio] OS x
* XCode 4.5 および iOS 6.0 (またはそれ以降のバージョン)
* 完了、[モバイル サービスの開始を取得] チュートリアル

## <a name="review-offline"></a>Mobile Services 同期コードを確認します。

ネットワークにアクセスできない場合、エンドユーザーは Azure Mobile Services スのオフライン同期により、ローカル データベースとやり取りできるようになります。 アプリケーションでこれらの機能を使用する初期化 `MobileServiceClient.SyncContext` をローカル ストアにします。 使用してテーブルを参照、 `IMobileServiceSyncTable` インターフェイスです。
このセクションで手順、オフライン同期に関連するコード `QSTodoService.cs`します。

1. Visual Studio で完成させたプロジェクトを開き、[モバイル サービスの開始を取得] チュートリアルです。 ファイルを開く `QSTodoService.cs`します。

2. メンバーの型に注意してください `todoTable` は `IMobileServiceSyncTable`します。 オフライン同期の代わりにこの同期テーブル インターフェイスを使用して `IMobileServiceTable`します。 同期テーブルが使用されると、すべての操作はローカル ストアを参照し、明示的なプッシュ操作とプル操作を使用したリモート サービスとのみ同期されます。

    メソッドの同期テーブルへの参照を取得する `GetSyncTable()` を使用します。 オフライン同期機能を削除するには代わりに使用する `GetTable()`します。

3. テーブル操作を実行する前に、ローカル ストアを初期化する必要があります。 これには、 `InitializeStoreAsync` メソッド。

     public async Task InitializeStoreAsync()
     {
         var store = new MobileServiceSQLiteStore(localDbPath);
         store.DefineTable<ToDoItem>();
    
         // Uses the default conflict handler, which fails on conflict
         await client.SyncContext.InitializeAsync(store);
     }

 クラスを使用してローカル ストアを作成この `MobileServiceSQLiteStore`, 、Mobile Services SDK で提供されます。 実装することによって、別のローカル ストア実装を提供することもできます `IMobileServiceLocalStore`します。

 `DefineTable` メソッドでは、テーブルを作成、提供された型のフィールドに一致するローカル ストアで `ToDoItem` ､ ホします。 この型に、リモート データベース内のすべての列を含める必要はありません。列のサブセットのみ格納できます。

 このオーバー ロード `InitializeAsync` 競合が発生するたびに失敗する既定の競合ハンドラーを使用します。 カスタム競合ハンドラーを提供するには、[モバイル サービスのオフライン サポートで競合の処理] このチュートリアルを参照してください。

4. メソッド `SyncAsync` 実際の同期操作をトリガーします。

     public async Task SyncAsync()
     {
         try
         {
             await client.SyncContext.PushAsync();
             await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
         }
    
         catch (MobileServiceInvalidOperationException e)
         {
             Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
         }
     }

 まずへの呼び出しがある `IMobileServiceSyncContext.PushAsync()`します。 このメソッドのメンバーである `IMobileServicesSyncContext` 、同期ではなくテーブルのすべてのテーブルに対して変更をプッシュするためです。 何らかの方法で (CUD 操作により) ローカルで変更されたレコードだけが、サーバー宛てに送信されます。

 次に、このメソッドを呼び出す `IMobileServiceSyncTable.PullAsync()` アプリに、サーバー上のテーブルからデータをプルします。 同期のコンテキストに保留中の変更があると、プルは常にプッシュを最初に実行することに注意してください。 これは、ローカル ストア内でリレーションシップを持つすべてのテーブルに一貫性があることを確認するためです。 この場合は、明示的にプッシュを呼び出しました。

 この例では、リモートのすべてのレコードを取得 `TodoItem` が、これは、クエリを渡すことでレコードをフィルター処理することもできます。 最初のパラメーター `PullAsync()` を使用する増分同期に使用されるクエリ id、 `UpdatedAt` タイムスタンプ、前回の同期以降に変更されたレコードのみを取得します。 クエリ ID は、アプリ内の各論理クエリに対して一意の、わかりやすい文字列にする必要があります。 オプトアウトの増分同期に渡す `null` クエリ ID として これによってプル操作ごとにすべてのレコードを取得することになり、効率が悪くなる可能性があります。
 >[AZURE.NOTE] モバイル サービス データベースでレコードが削除されたときに、デバイスのローカル ストアからレコードを削除するには、[論理的な削除] を有効にする必要があります。 それ以外の場合、アプリを呼び出す必要があります定期的に `IMobileServiceSyncTable.PurgeAsync()` ローカル ストアを消去します。

 なお、 `MobileServicePushFailedException` プッシュ操作とプル操作の両方で発生することができます。 次のチュートリアルの [モバイル サービスのオフライン サポートでの競合の処理]、ですこれらの同期関連の例外を処理する方法です。

5. クラスの `QSTodoService`, 、メソッド `SyncAsync()` 、データの変更操作後に呼び出されます `InsertTodoItemAsync()` と `CompleteItemAsync`します。 呼ばれる `RefreshDataAsync()`, 、ユーザーが更新ジェスチャを実行するたびに、最新のデータを取得できるようにします。 アプリもため同期実行起動時に `QSTodoListViewController.ViewDidLoad()` 呼び出し `RefreshDataAsync()`します。

    `SyncAsync()` データが変更されるたびに呼び出されるこのアプリでは、データが編集されるたびに、ユーザーがオンラインであると想定しています。 次のセクションでは、ユーザーがオフラインの場合でも編集できるようにアプリケーションを更新します。

## <a name="update-sync"></a>更新プログラム、アプリケーションの同期の動作

このセクションでは、アプリケーションの起動または挿入と更新の操作ではアプリケーションが同期されず、更新ジェスチャが実行された場合にのみ同期されるように、アプリケーションを変更します。 その後、オフラインの状況をシミュレートするために、モバイル サービスに対するアプリケーションの接続を解除します。 データ項目を追加すると、それはローカル ストアに保持されますが、即時にモバイル サービスと同期されることはありません。

1. 開いている `QSTodoService.cs`します。 呼び出しをコメント アウト `SyncAsync()` 次のメソッドで。

    - `InsertTodoItemAsync`
    - `CompleteItemAsync`
    - `RefreshAsync`

    これで、 `RefreshAsync()` はローカル ストアからデータを読み込むだけは、アプリ バックエンドに接続しません。

2. `QSTodoService.cs`, 、メンバーの定義をコメント解除 `applicationURL` と `applicationKey`します。 次の行を追加します。これらの行は無効なモバイル サービスの URL を参照します。

        const string applicationURL = @"https://your-mobile-service.azure-mobile.xxx/";
        const string applicationKey = @"AppKey";

3. 更新ジェスチャが実行されるときに、データが同期されているためには、メソッドを編集 `QSTodoListViewController.RefreshAsync()`します。 呼び出しを追加して `SyncAsync()` を呼び出す前に `RefreshDataAsync()`:

     private async Task RefreshAsync ()
     {
         RefreshControl.BeginRefreshing ();
    
         await todoService.SyncAsync();
         await todoService.RefreshDataAsync (); // add this line
    
         RefreshControl.EndRefreshing ();
    
         TableView.ReloadData ();
     }

4. アプリケーションをビルドし、実行します。 新しい todo 項目を追加します。 これらの新しい項目は、モバイル サービスにプッシュされるまでは、ローカル ストア内にのみ存在します。 クライアント アプリケーションは、作成、読み取り、更新、削除 (CRUD) 操作のすべてをサポートするモバイル サービスに接続されているときと同様に動作します。

5. アプリケーションを終了し、再起動して、作成した新しい項目がローカル ストアに保存されていることを確認します。

## <a name="update-online-app"></a>モバイル サービスに再接続するアプリケーションを更新します。

ここでは、アプリケーションをモバイル サービスに再接続します。 これは、アプリケーションがオフライン状態から、モバイル サービスとのオンライン状態に移行したことをシミュレートします。 更新ジェスチャを実行すると、データがモバイル サービスに同期されます。

1. 開いている `QSTodoService.cs`します。 無効なモバイル サービスの URL を削除し、正しい URL とアプリケーション キーを再度追加します。

2. アプリケーションを再構築して実行します。 アプリケーションは現在モバイル サービスに接続されていますが、オフラインの状況と同じ表示になっていることに注意してください。 これは、このアプリは常に使用されるため、 `IMobileServiceSyncTable` 、ローカル ストアを指しています。

3. [Azure 旧ポータル] にログインし、モバイル サービスのデータベースを参照します。 開発中のサービスが JavaScript バックエンドを使用している場合は、モバイル サービスの [**データ**] タブからデータを参照できます。

    モバイル サービスの .NET バックエンドを使用している場合は、Visual Studio で、[**サーバー エクスプローラー**]、[**Azure**]、[**SQL データベース**] の順に移動します。 データベースを右クリックし、[**SQL Server オブジェクト エクスプローラーで開く**] を選択します。

    データベースとローカル ストアの間でデータがまだ同期されて*いない*ことを確認します。

4. アプリケーションで、項目の一覧をプルダウンして更新操作を実行します。 これにより、アプリケーションを呼び出す `RefreshDataAsync()`, 、呼び出している `SyncAsync()`します。 これによってプッシュとプルの処理が実行されます。まず、ローカル ストアの項目がモバイル サービスに送信され、次に、新しいデータがサービスから取得されます。

5. モバイル サービスのデータベースで変更が同期されていることを確認してください。

## 概要

[AZURE.INCLUDE [mobile-services-offline-summary-csharp](../../includes/mobile-services-offline-summary-csharp.md)]

## 次のステップ

* [モバイル サービスのオフライン サポートでの競合を処理する]

* [Azure Mobile Services 向け Xamarin コンポーネント クライアントを使用する方法]






[review the mobile services sync code]: #review-offline 
[update the sync behavior of the app]: #update-sync 
[update the app to reconnect your mobile service]: #update-online-app 
[handling conflicts with offline support for mobile services]: ../mobile-services-xamarin-ios-handling-conflicts-offline-data.md 
[get started with mobile services]: mobile-services-ios-get-started.md 
[how to use the xamarin component client for azure mobile services]: partner-xamarin-mobile-services-how-to-use-client-library.md 
[soft delete]: mobile-services-using-soft-delete.md 
[xamarin studio]: http://xamarin.com/download 
[xamarin extension]: http://xamarin.com/visual-studio 
[azure classic portal]: https://manage.windowsazure.com 

