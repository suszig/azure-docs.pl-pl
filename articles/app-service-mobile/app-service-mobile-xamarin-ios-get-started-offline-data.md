<properties
    pageTitle="Azure モバイル アプリ (Xamarin iOS) のオフライン同期を有効にする"
    description="App Service Mobile App を使用して、Xamarin iOS アプリケーションのオフライン データをキャッシュおよび同期する方法を説明します。"
    documentationCenter="xamarin"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="12/02/2015"
    ms.author="wesmc"/>


# Xamarin iOS モバイル アプリのオフライン同期を有効にする

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

## 概要

このチュートリアルでは、Xamarin iOS 向けの Azure モバイル アプリのオフライン同期機能について説明します。 オフライン同期を使用すると、エンド ユーザーはネットワークにアクセスできなくても、データの表示、追加、変更など、モバイル アプリケーションとやり取りできます。 変更は、ローカル データベースに格納されます。デバイスがオンラインに戻ると、これらの変更は、リモート サービスと同期されます。

このチュートリアルでは、[Xamarin iOS アプリの作成] Azure Mobile Apps のオフライン機能をサポートするために、チュートリアルから Xamarin.iOS アプリケーション プロジェクトを更新します。 ダウンロードしたクイック スタートのサーバー プロジェクトを使用しない場合は、データ アクセス拡張機能パッケージをプロジェクトに追加する必要があります。 サーバーの拡張機能パッケージの詳細については、次を参照してください。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)します。

オフライン同期機能の詳細については、「[Azure Mobile Apps でのオフライン データ同期]」を参照してください。

## 必要条件

このチュートリアルには、次のものが必要です。

* Visual Studio 2013
* Visual Studio [Xamarin 拡張機能] **または** [Xamarin Studio] OS x
* [Xamarin iOS アプリを作成する] このチュートリアルを完了します。 このチュートリアルはそのチュートリアルでカバーされている完成したアプリを使用します。

## クライアント同期コードの確認

チュートリアル [Xamarin iOS アプリを作成する] が既に完了したときに、ダウンロードした Xamarin クライアント プロジェクトには、ローカル SQLite データベースを使用してオフラインの同期をサポートするコードが含まれています。 チュートリアルのコードにすでに含まれているものの概要を示します。 機能の概念的な概要については、[Azure Mobile Apps のオフライン データ同期] を参照してください。

* テーブル操作を実行する前に、ローカル ストアを初期化する必要があります。 ローカル ストア データベースは初期化されてとき `QSTodoListViewController.ViewDidLoad()` 実行 `QSTodoService.InitializeStoreAsync()`します。 これは、新しいローカル SQLite を使用してデータベースを作成、 `MobileServiceSQLiteStore` Azure モバイル アプリ クライアント SDK によって提供されるクラスです。

  `DefineTable` メソッドでは、テーブルを作成、提供された型のフィールドに一致するローカル ストアで `ToDoItem` ､ ホします。 この型に、リモート データベース内のすべての列を含める必要はありません。 列のサブセットのみ格納できます。

      // QSTodoService.cs
    
      public async Task InitializeStoreAsync()
      {
          var store = new MobileServiceSQLiteStore(localDbPath);
          store.DefineTable<ToDoItem>();
    
          // Uses the default conflict handler, which fails on conflict
          await client.SyncContext.InitializeAsync(store);
      }

* `TodoTable` のメンバー `QSTodoService` は、 `IMobileServiceSyncTable` 型の代わりに `IMobileServiceTable`します。 これは、テーブルの作成、読み取り、更新、および削除 (CRUD) といったすべての操作がローカル ストア データベースに行われるようにします。

  ときにそれらの変更にプッシュされます Azure Mobile App バックエンドを呼び出してを決定する `IMobileServiceSyncContext.PushAsync()` クライアント接続の同期コンテキストを使用します。 同期コンテキストでは、追跡、クライアント アプリケーションが変更されたときにすべてのテーブルの変更をプッシュしてテーブルのリレーションシップを保持するのに役立ちます `PushAsync` が呼び出されます。

  指定されたコードの呼び出し `QSTodoService.SyncAsync()` todoitem] ボックスの一覧が更新されるか、todoitem が追加または完了するたびに同期します。 同期コンテキストへのプッシュや同期テーブルへのプルを実行するようなローカルの変更があれば毎回同期が行われます。 ただし、コンテキストによって追跡された保留中のローカル更新のあるテーブルに対してプルが実行される場合、そのプルの処理は自動的にコンテキストのプッシュを最初にトリガーします。 このような場合 (更新、追加、および項目の完了) に前の例で、明示的なように `PushAsync` 呼び出します。 冗長となるからです。

  指定されたコードですべてのレコードにリモートの `TodoItem` 、テーブル、クエリが、クエリ id を渡すことでレコードをフィルター処理およびクエリを実行することも `PushAsync`します。 詳細については、セクションを参照してください。 *増分同期* [Azure Mobile Apps のオフライン データ同期] にします。


      // QSTodoService.cs
    
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



## クライアント アプリの実行

ローカル ストアのデータベースの設定をするために、少なくとも 1 回はクライアント アプリを実行してください。 次のセクションでは、オフラインのシナリオをシミュレートし、アプリがオフラインの間にローカル ストアのデータを変更します。


## クライアント アプリの同期の動作を更新する

このセクションでは、バックエンドに無効なアプリケーション URL を使用することで、クライアント プロジェクトを変更して、オフラインのシナリオをシミュレートします。 データ項目を追加または変更すると、これらの変更はローカル ストアに保持されますが、接続が再確立されるまでは、バックエンドのデータ ストアには同期されません。

1. 上部にある `QSTodoService.cs`, の初期化を変更 `applicationURL` に無効な URL をポイントします。

        const string applicationURL = @"https://your-service.azurewebsites.xxx/"; 

2. 追加 `キャッチ` の `例外` クラス `QSTodoService.SyncAsync` がコンソールに例外メッセージを書き込むことです。

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
            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
        }

3. クライアント アプリの構築と実行 新しい todo 項目を追加すると、モバイル アプリのバックエンドと同期を試みるごとに、コンソールに例外がログ記録されることに注意してください。 これらの新しい項目は、モバイル バックエンドにプッシュされるまでは、ローカル ストア内にのみ存在します。 クライアント アプリケーションは、バックエンドに接続されているかのように動作し、作成、読み取り、更新、削除 (CRUD) 操作のすべてをサポートします。

4. アプリケーションを終了し、再起動して、作成した新しい項目がローカル ストアに保存されていることを確認します。

5. (省略可能) Visual Studio を使用して、Azure SQL Database テーブルを表示し、バックエンドのデータベースのデータが変更されていないことを確認します。

    Visual Studio で、**サーバー エクスプローラー**を開きます。 **[Azure]**、**[SQL Databases]** の順に選択して、データベースに移動します。 データベースを右クリックし、**[SQL Server オブジェクト エクスプローラーで開く]** を選択します。 これで SQL データベースのテーブルとその内容を参照できます。

6. (省略可能)Fiddler や Postman のような REST ツールを使用して形式で取得クエリを使用して、モバイル バックエンドをクエリ `https://your-mobile-app-backend-name.azurewebsites.net/tables/TodoItem`します。

## クライアント アプリを更新し、モバイルのバックエンドを再接続します。

このセクションでは、アプリケーションをモバイル バックエンドに再接続して、アプリケーションがオンライン状態に戻ったときの動作をシミュレーションします。 更新ジェスチャを実行すると、データがモバイル バックエンドに同期されます。

1. 開いている `QSTodoService.cs`します。 修正、 `applicationURL` と `gatewayURL` 、適切な Url を指すようです。

2. クライアント アプリの再構築と実行 起動後、アプリは Azure モバイル アプリのバックエンドとの同期を試みます。 デバッグ コンソールに例外がログ記録されていないことを確認してください。

3. (省略可能) SQL Server Object Explorer または Fiddler などの REST ツールを使用して、更新データを表示します。 データが同期されるのは、Azure モバイル アプリのバックエンドのデータベースとローカル ストアの間であることに注意してください。

    データがデータベースとローカル ストアの間で同期されており、アプリが接続されていない状況で追加した項目が含まれていることを確認してください。

## その他のリソース

* [Azure のモバイル アプリでのオフライン データの同期]

* [カバーをクラウド: Azure Mobile Services でのオフライン同期] \ (注: ビデオは、モバイル サービスには、Azure モバイル アプリで同様の方法でのオフライン同期機能)







[create a xamarin ios app]: ../app-service-mobile-xamarin-ios-get-started.md 
[offline data sync in azure mobile apps]: ../app-service-mobile-offline-data-sync.md 
[how to use the xamarin component client for azure mobile services]: ../partner-xamarin-mobile-services-how-to-use-client-library.md 
[xamarin studio]: http://xamarin.com/download 
[xamarin extension]: http://xamarin.com/visual-studio 
[cloud cover: offline sync in azure mobile services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri 

