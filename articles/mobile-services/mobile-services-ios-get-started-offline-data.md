<properties
    pageTitle="Mobile Services でのオフライン データ同期の使用 (iOS) | Microsoft Azure"
    description="Azure Mobile Services を使用して、iOS アプリケーションのオフライン データをキャッシュおよび同期する方法を説明します。"
    documentationCenter="ios"
    authors="krisragh"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2015"
    ms.author="krisragh;donnam"/>


# Mobile Services でのオフライン データの同期の使用

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-offline](../../includes/mobile-services-selector-offline.md)]

オフラインの同期を使用すれば、ネットワーク接続が得られない状況でも、モバイル アプリケーションでデータを表示、追加、または変更することができます。 このチュートリアルでは、アプリケーションで変更内容を自動的にオフライン データベースに格納し、オンラインに戻るたびにそれらの変更内容を同期させる方法について説明します。

オフライン同期には、次のような複数の利点があります。

* サーバー データをデバイスにローカルでキャッシュすることにより、アプリケーションの応答性を向上させる。
* 断続的なネットワーク接続に対してアプリケーションに弾力性を持たせる。
* 接続がほとんどまたはまったく得られない状況でもデータを作成し変更できる。
* 複数のデバイス間でデータを同期させる。
* 同じレコードが 2 つのデバイスによって変更されたときに競合を検出する。

> [AZURE.NOTE] このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントを持っていない場合、Azure 評価版にサインアップして取得 [無料の評価終了後も使用することができますが、モバイル サービス](http://azure.microsoft.com/pricing/details/mobile-services/)します。 詳細については、[Azure の無料評価版] を参照してください (http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28 target ="_blank") です。

このチュートリアルは、[Mobile Services クイック スタート チュートリアル] に基づいて最初に完了する必要があります。 まず、クイック スタートで説明したオフラインの同期に関連するコードを確認してみましょう。

## <a name="review-sync"></a>Mobile Services 同期コードのレビュー

ネットワークにアクセスできない場合、エンドユーザーは Azure Mobile Services スのオフライン同期により、ローカル データベースとやり取りできるようになります。 同期コンテキストを初期化して、アプリケーションでこれらの機能を使用する `MSClient` ローカル ストアを参照します。 使用してテーブルを参照、 `MSSyncTable` インターフェイスです。

* **QSTodoService.m**, 、メンバーの種類に注意してください `syncTable` は `MSSyncTable`します。 オフライン同期がの代わりにこれを使用して `MSTable`します。 同期テーブルが使用されると、すべての操作はローカル ストアを参照し、明示的なプッシュ操作とプル操作を使用したリモート サービスとのみ同期されます。

```
        @property (nonatomic, strong)   MSSyncTable *syncTable;
```

同期テーブルへの参照を取得するには、メソッドを使用 `syncTableWithName`します。 オフライン同期機能を削除するには使用 `tableWithName` 代わりにします。

* **QSTodoService.m**, でローカルのストアが初期化は、テーブル操作を実行する前に `QSTodoService.init`:

```
        MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
        self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
```

これを使用してローカル ストアを作成、 `MSCoreDataStore` インターフェイスです。 実装するで別のローカル ストアを提供することも、 `MSSyncContextDataSource` プロトコルです。

最初のパラメーター `initWithDelegate` 、競合ハンドラーを指定しますが、渡したので `nil`, 、競合は失敗する既定の競合ハンドラーを取得します。 カスタム競合ハンドラーを実装する方法の詳細については、[モバイル サービスのオフライン サポートで競合の処理] を参照してください。

* **QSTodoService.m**, 、`syncData` 新しい変更をプッシュして、呼び出してから、 `pullData` リモート サービスからデータを取得します。  `SyncData`, 、呼び出して `pushWithCompletion` 同期コンテキストにします。 このメソッドのメンバーである `MSSyncContext` --テーブルではなく、同期自体--すべてのテーブルに対して変更をプッシュするためです。 なんらかの方法で (作成、更新、または削除操作により) ローカルで変更されたレコードだけが、サーバー宛てに送信されます。 末尾に `syncData`, 、我々 は、ヘルパー `pullData`します。

この例では、プッシュ操作は必ずしも必要ありません。 プッシュ操作を行っているテーブルの同期コンテキストで保留中の変更がある場合、プルは必ず最初にプッシュを発行します。 ただし、同期テーブルが複数ある場合は、テーブル間で一貫性が維持されるようにプッシュを明示的に呼び出します。

```
      -(void)syncData:(QSCompletionBlock)completion
      {
          // push all changes in the sync context, then pull new data
          [self.client.syncContext pushWithCompletion:^(NSError *error) {
              [self logErrorIfNotNil:error];
              [self pullData:completion];
          }];
      }
```

* 次に、 **QSTodoService.m**, 、`pullData` がクエリに一致する新しいデータを取得します。 `pullData` 呼び出し `MSSyncTable.pullWithQuery` にリモート データを取得し、ローカルに保存します。 `pullWithQuery` 取得するレコードをフィルター処理するクエリを指定することもできます。 この例では、クエリだけレコードをすべて取得にリモートの `TodoItem` テーブルです。

2 番目のパラメーター `pullWithQuery` _incremental sync _ をクエリ id を指定します。 増分同期レコードを使用して、前回の同期以降に変更されたレコードのみを取得する `UpdatedAt` と呼ばれる、タイムスタンプ `ms_updatedAt` 、ローカル ストアにします。 クエリ ID は、アプリケーション内の各論理クエリに対して一意の、わかりやすい文字列です。 オプトアウトの増分同期に渡す `nil` クエリ ID として これは、どのプル操作でもすべてのレコードを取得するため、効率的なありません。

```
      -(void)pullData:(QSCompletionBlock)completion
      {
          MSQuery *query = [self.syncTable query];

          // Pulls data from the remote server into the local table.
          // We're pulling all items and filtering in the view
          // query ID is used for incremental sync
          [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
              [self logErrorIfNotNil:error];

              // Let the caller know that we have finished
              if (completion != nil) {
                  dispatch_async(dispatch_get_main_queue(), completion);
              }
          }];
      }
```


>[AZURE.NOTE] モバイル サービス データベースで削除されたときに、デバイスのローカル ストアからレコードを削除するには、[論理的な削除] を有効にします。 それ以外の場合、アプリを呼び出す必要があります定期的に `MSSyncTable.purgeWithQuery` ローカル ストアを消去します。


* **QSTodoService.m**, 、メソッド `addItem` と `completeItem` 呼び出す `syncData` データを変更した後です。  **QSTodoListViewController.m**, 、メソッド `更新` も呼び出し `syncData` UI の更新時および起動時に最新のデータが表示されるように (`init` 呼び出し `更新`.)

アプリを呼び出すため `syncData` アプリ データを変更するたびに、[アプリ内のデータを編集するたびに、オンラインと想定しています。

## <a name="review-core-data"></a>Core Data モデルの確認

Core Data オフライン ストアを使用するときは、データ モデルで特定のテーブルとフィールドを定義する必要があります。 サンプル アプリケーションには、あらかじめ適切な形式でデータ モデルが含まれています。 このセクションでは、これらのテーブルとその使用方法を説明します。

- **QSDataModel.xcdatamodeld** を開きます。 4 つのテーブルが定義済みです。3 つは SDK で使用し、1 つはそれ自体が Todo 項目になっています。

      * MS_TableOperations: For tracking items to be synchronized with server
      * MS_TableOperationErrors: For tracking errors that happen during offline sync
      * MS_TableConfig: For tracking last updated time for last sync operation for all pull operations
      * TodoItem: For storing todo items. The system columns **ms_createdAt**, **ms_updatedAt**, and **ms_version** are optional system properties.


>[AZURE.NOTE] モバイル サービス SDK が始まる列名を予約"* *`ms _`* *"です。 このプレフィックスは、システム列以外のものに使用しないでください。 そうしないと、リモート サービスの使用時に列名が変更されます。

- オフライン同期機能を使用する場合は、次のようにシステム テーブルを定義する必要があります。

    ### システム テーブル

    ### MS_TableOperations

  | 属性| 種類|
  |-------------- |   ------    |
  | id (必須)| Integer 64|
  | itemId| String|
  | プロパティ| Binary Data|
  | テーブル| String|
  | tableKind| Integer 16|

    ### MS_TableOperationErrors

  | 属性| 種類|
  |-------------- | ----------  |
  | id (必須)| String|
  | operationId| Integer 64|
  | プロパティ| Binary Data|
  | tableKind| Integer 16|

    ### MS_TableConfig

  | 属性| 種類|
  |-------------- | ----------  |
  | id (必須)| String|
  | key| String|
  | keyType| Integer 64|
  | テーブル| String|
  | 値| String|

    ### データ テーブル

    ### TodoItem

  | 属性| 型| 注|
  |-------------- |  ------ | -------------------------------------------------------|
  | id (必須)| String| リモート ストア内のプライマリ キー (必須)|
  | 完了| Boolean| Todo 項目フィールド|
  | テキスト| String| Todo 項目フィールド|
  | ms_createdAt| 日付| (省略可能) __createdAt システム プロパティへのマップ|
  | ms_updatedAt| 日付| (省略可能) __updatedAt システム プロパティへのマップ|
  | ms_version| String| (省略可能) 競合の検出に使用、__version へのマップ|




## <a name="setup-sync"></a>アプリの同期動作の変更

このセクションで、更新ジェスチャが実行された場合にのみ、アプリの起動または挿入すると、アイテムの更新が同期されないように、アプリケーションを変更します。

* **QSTodoListViewController.m**, 、変更 `viewDidLoad` への呼び出しを削除する `[自動更新]` メソッドの最後にします。 これで、アプリケーションの起動時にデータはサーバーと同期されなくなり、データはローカルに格納されるだけとなります。

* **QSTodoService.m**, 、変更 `addItem` アイテムの挿入後に同期しないようにします。 削除、 `自己 syncData` をブロックし、次に置き換えます。

```
        if (completion != nil) {
            dispatch_async(dispatch_get_main_queue(), completion);
        }
```

* 同様で再度 **QSTodoService.m**, で、 `completeItem`, のブロックを削除 `自己 syncData` し、次に置き換えます。

```
        if (completion != nil) {
            dispatch_async(dispatch_get_main_queue(), completion);
        }
```

## <a name="test-app"></a>アプリケーションをテストします。

このセクションでは、シミュレーターの Wi-Fi をオフにして、オフライン シナリオを作成します。 データ項目を追加すると、ローカル Core Data ストアに保持されますが、モバイル サービスとは同期されません。

1. Mac のインターネット接続を無効にします。 ホストの Mac のインターネット接続がシミュレーターに使用される可能性があるので、iOS シミュレーターの Wi-Fi のみを無効にしても効果がないことがあるので、コンピューター自体のインターネットを無効にしてください。 これは、オフライン シナリオをシミュレートします。

2. いくつかの Todo 項目を追加するか、一部の項目を完了します。 シミュレーターを終了し (またはアプリケーションを強制的に閉じて)、再起動します。 変更内容が保存されていることを確認します。 データ項目がまだ表示されるのは、データ項目がローカルの Core Data ストアに保持されているためです。

3. リモートの TodoItem テーブルの内容を表示します。 新しい項目が、サーバーと同期_されなかった_ことを確認します。

   - JavaScript バックエンドを参照してください、 [Azure クラシック ポータル](http://manage.windowsazure.com), 、[データ] タブの内容を表示する] をクリックし、 `TodoItem` テーブルです。
   - .NET バックエンドの場合は、SQL Server Management Studio のような SQL ツールまたは Fiddler や Postman のような REST クライアントを使用して、テーブルの内容を表示します。

4. iOS シミュレーターの Wi-Fi をオンにします。 次に、項目の一覧をプルダウンして更新操作を実行します。 進行状況を示すスピナーとテキスト "同期中..." が表示されます。

5. TodoItem データをもう一度表示します。 新しく変更した TodoItems が表示されます。

## 概要

使用したモバイル サービスのオフライン機能をサポートするために、 `MSSyncTable` インターフェイスおよび初期化さ `MSClient.syncContext` 、ローカル ストアを使用します。 この例では、ローカル ストアは、Core Data に基づいたデータベースでした。

複数のテーブルを定義する Core Data ローカル ストアを使用する場合、 [[core data モデルの確認]、[システムのプロパティを修正][review the core data model]します。 モバイル サービスに対する通常の操作は、アプリケーションが接続されているが、ローカル ストアに対してすべての操作が発生したかのように動作します。

使用して、サーバーとローカル ストアを同期する `MSSyncTable.pullWithQuery` と `MSClient.syncContext.pushWithCompletion`:

        * To push changes to the server, you called `pushWithCompletion`. This method is in `MSSyncContext` instead of the sync table because it will push changes across all tables. Only records that are modified in some way locally (through CUD operations) are be sent to the server.
    
        * To pull data from a table on the server to the app, you called `MSSyncTable.pullWithQuery`. A pull always issues a push first. This is to ensure all tables in the local store along with relationships remain consistent. `pullWithQuery` can also be used to filter the data that is stored on the client, by customizing the `query` parameter.

## 次のステップ

* [モバイル サービスのオフライン サポートでの競合を処理する]

* [Mobile Services ][soft delete]

## その他のリソース

* [カバーをクラウド: Azure Mobile Services でオフラインの同期]

* [Azure Friday: Azure Mobile Services でのオフライン対応のアプリケーション] \ (注: デモは、Windows では機能の説明は、すべての platforms \ に適用されます)




[get the sample app]: #get-app 
[review the core data model]: #review-core-data 
[review the mobile services sync code]: #review-sync 
[change the sync behavior of the app]: #setup-sync 
[test the app]: #test-app 
[core-data-1]: ./media/mobile-services-ios-get-started-offline-data/core-data-1.png 
[core-data-2]: ./media/mobile-services-ios-get-started-offline-data/core-data-2.png 
[core-data-3]: ./media/mobile-services-ios-get-started-offline-data/core-data-3.png 
[defining-core-data-main-screen]: ./media/mobile-services-ios-get-started-offline-data/defining-core-data-main-screen.png 
[defining-core-data-model-editor]: ./media/mobile-services-ios-get-started-offline-data/defining-core-data-model-editor.png 
[defining-core-data-tableoperationerrors-entity]: ./media/mobile-services-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png 
[defining-core-data-tableoperations-entity]: ./media/mobile-services-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png 
[defining-core-data-tableconfig-entity]: ./media/mobile-services-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png 
[defining-core-data-todoitem-entity]: ./media/mobile-services-ios-get-started-offline-data/defining-core-data-todoitem-entity.png 
[update-framework-1]: ./media/mobile-services-ios-get-started-offline-data/update-framework-1.png 
[update-framework-2]: ./media/mobile-services-ios-get-started-offline-data/update-framework-2.png 
[core data model editor help]: https://developer.apple.com/library/mac/recipes/xcode_help-core_data_modeling_tool/Articles/about_cd_modeling_tool.html 
[creating an outlet connection]: https://developer.apple.com/library/mac/recipes/xcode_help-interface_builder/articles-connections_bindings/CreatingOutlet.html 
[build a user interface]: https://developer.apple.com/library/mac/documentation/ToolsLanguages/Conceptual/Xcode_Overview/Edit_User_Interfaces/edit_user_interface.html 
[adding a segue between scenes in a storyboard]: https://developer.apple.com/library/ios/recipes/xcode_help-IB_storyboard/chapters/StoryboardSegue.html#//apple_ref/doc/uid/TP40014225-CH25-SW1 
[adding a scene to a storyboard]: https://developer.apple.com/library/ios/recipes/xcode_help-IB_storyboard/chapters/StoryboardScene.html 
[core data]: https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreData/cdProgrammingGuide.html 
[download the preview sdk here]: http://aka.ms/Gc6fex 
[how to use the mobile services client library for ios]: mobile-services-ios-how-to-use-client-library.md 
[offline ios sample]: https://github.com/Azure/mobile-services-samples/tree/master/TodoOffline/iOS/blog20140611 
[mobile services sample repository on github]: https://github.com/Azure/mobile-services-samples 
[get started with mobile services]: mobile-services-ios-get-started.md 
[handling conflicts with offline support for mobile services]: mobile-services-ios-handling-conflicts-offline-data.md 
[soft delete]: mobile-services-using-soft-delete.md 
[cloud cover: offline sync in azure mobile services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri 
[azure friday: offline-enabled apps in azure mobile services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/ 
[mobile services quick start tutorial]: mobile-services-ios-get-started.md 

