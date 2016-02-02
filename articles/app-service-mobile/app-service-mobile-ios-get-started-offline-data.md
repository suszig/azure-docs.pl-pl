<properties
    pageTitle="Azure Mobile App (iOS) に対するオフライン同期の有効化"
    description="App Service Mobile Apps を使用して、iOS アプリケーション内のオフライン データをキャッシュおよび同期する方法を説明します。"
    documentationCenter="ios"
    authors="krisragh"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="12/01/2015"
    ms.author="krisragh"/>


# iOS モバイル アプリのオフライン同期を有効にする

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

## 概要

このチュートリアルでは、Azure Mobile Apps for iOS 向けのモバイル アプリのオフライン同期機能について説明します。 オフライン同期を使用すると、エンド ユーザーはネットワークにアクセスできなくても、データの表示、追加、変更など、モバイル アプリとやり取りできます。 変更はローカル データベースに格納され、デバイスが再びオンラインになると、これらの変更がリモート バックエンドと同期されます。

Azure モバイル アプリを使用して初めての場合は、[iOS アプリの作成] に関するチュートリアルを完了する必要があります。 ダウンロードしたクイック スタートのサーバー プロジェクトを使用しない場合は、データ アクセス拡張機能パッケージをプロジェクトに追加する必要があります。 サーバーの拡張機能パッケージの詳細については、次を参照してください。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)します。

オフライン同期機能の詳細については、「[Azure Mobile Apps でのオフライン データ同期]」を参照してください。

## <a name="review-sync"></a>クライアントの同期コードを確認します。

チュートリアル [iOS アプリの作成] では、既にダウンロードしたクライアント プロジェクトには、ローカル Core Data ベースのデータベースを使用してオフラインの同期をサポートするコードが含まれています。 このセクションでは、チュートリアルのコードに既に含まれているものの概要を示します。 機能の概念的な概要については、[Azure Mobile Apps のオフライン データ同期] を参照してください。

Azure Mobile Apps のオフライン データ同期機能を使用すると、ネットワークにアクセスできない場合でもエンド ユーザーはローカル データベースとやり取りできるようになります。 同期コンテキストを初期化して、アプリケーションでこれらの機能を使用する `MSClient` ローカル ストアを参照します。 使用してテーブルを参照、 `MSSyncTable` インターフェイスです。

1. **QSTodoService.m**, 、メンバーの型に注意してください `syncTable` は `MSSyncTable`します。 オフライン同期の代わりにこの同期テーブル インターフェイスを使用して `MSTable`します。 同期テーブルが使用されると、すべての操作はローカル ストアを参照し、明示的なプッシュ操作とプル操作を使用するリモート バックエンドのみが同期されます。

    同期テーブルへの参照を取得するには、メソッドを使用 `syncTableWithName`します。 オフライン同期機能を削除するには使用 `tableWithName` 代わりにします。

2. テーブル操作を実行する前に、ローカル ストアを初期化する必要があります。 これは、内の関連コード、 `QSTodoService.init` メソッド。

        MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
       
        self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];

    インターフェイスを使用してローカル ストアを作成この `MSCoreDataStore`, 、Mobile Apps SDK で提供されます。 代わりに別のローカル ストアを指定して実装することによって、 `MSSyncContextDataSource` プロトコルです。

    最初のパラメーター `initWithDelegate` 競合ハンドラーを指定するために使用します。 渡したため `nil`, 、競合が失敗した既定の競合ハンドラーが取得されます。


3. メソッド `pullData` と `syncData` 実際の同期操作を実行: `syncData` 新しい変更をプッシュして呼び出して `pullData` リモート バックエンドからデータを取得します。

     -(void)syncData:(QSCompletionBlock)completion
     {
         // push all changes in the sync context, then pull new data
         [self.client.syncContext pushWithCompletion:^(NSError *error) {
             [self logErrorIfNotNil:error];
             [self pullData:completion];
         }];
     }

 そして、メソッド `pullData` がクエリに一致する新しいデータを取得します。

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

 `SyncData`, 、呼び出して `pushWithCompletion` 同期コンテキストにします。 このメソッドのメンバーである `MSSyncContext` (テーブルではなく、同期自体) すべてのテーブルに対して変更をプッシュするためです。 何らかの方法で (CUD 操作により) ローカルで変更されたレコードだけが、サーバー宛てに送信されます。 次に、ヘルパー `pullData` が呼び出されると、どの呼び出し `MSSyncTable.pullWithQuery` リモート データを取得し、ローカル データベースに格納します。

 この例では、プッシュ操作は必ずしも必要ありません。 プッシュ操作を行っているテーブルの同期コンテキストで保留中の変更がある場合は、プルは必ず最初にプッシュを発行します。 ただし、複数の同期テーブルを使用している場合は、関連するテーブル全体で一貫性を保つよう、明示的にプッシュを呼び出す方法が最も適切です。

 メソッド `pullWithQuery` 取得するレコードをフィルター処理するクエリを指定することができます。 この例では、クエリだけレコードをすべて取得にリモートの `TodoItem` テーブルです。

 2 番目のパラメーター `pullWithQuery` に使用されるクエリ id *増分同期*します。 増分同期レコードを使用して、前回の同期以降に変更されたレコードのみを取得する `UpdatedAt` タイムスタンプ (と呼ばれる `updatedAt` 、ローカル ストア内)。 クエリ ID は、アプリ内の各論理クエリに対して一意の、わかりやすい文字列にする必要があります。 オプトアウトの増分同期に渡す `nil` クエリ ID として これは、プル操作のたびにすべてのレコードを取得するため、非効率になる場合があります。


5. クラスの `QSTodoService`, 、メソッド `syncData` 、データの変更操作後に呼び出されます `addItem` と `completeItem`します。 呼ばれる `QSTodoListViewController.refresh`, 、ユーザーが更新ジェスチャを実行するたびに、最新のデータを取得できるようにします。 アプリもため同期実行起動時に `QSTodoListViewController.init` 呼び出し `更新`します。

    `SyncData` データが変更されるたびに呼び出されるこのアプリでは、データが編集されるたびに、ユーザーがオンラインであると想定しています。 別のセクションでは、アプリケーションを更新して、ユーザーがオフラインのときにも編集できるようにします。

## <a name="review-core-data"></a>Core Data モデルを確認します。

Core Data オフライン ストアを使用するときは、データ モデルで特定のテーブルとフィールドを定義する必要があります。 サンプル アプリケーションには、あらかじめ適切な形式でデータ モデルが含まれています。 このセクションでは、これらのテーブルとその使用方法を説明します。

- **QSDataModel.xcdatamodeld** を開きます。 4 つのテーブルが定義済みです。3 つは SDK で使用し、1 つはそれ自体が Todo 項目になっています。
      * MS_TableOperations: サーバーと同期する必要がある項目の追跡用
      * MS_TableOperationErrors: オフライン同期中に発生するエラーの追跡用
      * MS_TableConfig: すべてのプル操作に対する最後の同期操作までの時間を更新、最後の追跡用
      * TodoItem: todo 項目を格納用します。 システム列 **createdAt**、**updatedAt**、および **version** は省略可能なシステム プロパティです。

>[AZURE.NOTE] Azure Mobile Apps SDK では、"**``**" が付く列名が予約されています。 システム列以外でこのプレフィックスを使用しないでください。使用した場合、リモート バックエンドを使用するときに列名が変更されます。

- オフライン同期機能を使用する場合は、次のようにシステム テーブルを定義する必要があります。

    ### システム テーブル

    **MS_TableOperations**

    ![][defining-core-data-tableoperations-entity]

  | 属性| 型|
  |----------- |   ------    |
  | id| Integer 64|
  | itemId| String|
  | プロパティ| Binary Data|
  | テーブル| String|
  | tableKind| Integer 16|

    <br>**MS_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

  | 属性| 型|
  |----------- |   ------    |
  | id| String|
  | operationId| Integer 64|
  | プロパティ| Binary Data|
  | tableKind| Integer 16|

    <br>**MS_TableConfig**

    ![][defining-core-data-tableconfig-entity]

  | 属性| 型|
  |----------- |   ------    |
  | id| String|
  | key| String|
  | keyType| Integer 64|
  | テーブル| String|
  | 値| String|

    ### データ テーブル

    **TodoItem**

  | 属性| 型| 注|
  |-----------   |  ------ | -------------------------------------------------------|
  | id| 文字列、必須のマーク| リモート ストア内のプライマリ キー|
  | 完了| Boolean| Todo 項目フィールド|
  | テキスト| String| Todo 項目フィールド|
  | createdAt| 日付| (省略可能) createdAt システム プロパティにマップします。|
  | updatedAt| 日付| (省略可能) updatedAt システム プロパティにマップします。|
  | version| String| (省略可能) 競合の検出に使用され、バージョンにマップします。|



## <a name="setup-sync"></a>アプリケーションの同期の動作を変更します。

このセクションでは、アプリケーションの起動時または項目の挿入や更新時ではなく、[ジェスチャの更新] ボタンの実行時にだけ、アプリケーションが同期するようアプリケーションを変更します。

1. **QSTodoListViewController.m**, 、変更、 **viewDidLoad** メソッド呼び出しを削除する `[自動更新]` メソッドの最後にします。 これで、アプリケーションの起動時にデータはサーバーと同期されなくなりますが、代わりにデータはローカル ストアのコンテンツになります。

2. **QSTodoService.m**, の定義を変更 `addItem` アイテムの挿入後に同期しないようにします。 削除、 `自己 syncData` をブロックし、次に置き換えます。

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. 定義を変更 `completeItem` 同様のブロックを削除する `自己 syncData` し、次に置き換えます。

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }


## <a name="test-app"></a>アプリケーションをテストします。

ここでは、無効な URL に接続してオフライン シナリオをシミュレートします。 データ項目を追加すると、ローカル Core Data ストアに保持されますが、モバイル バックエンドとは同期されません。

1. **QSTodoService.m** のモバイル アプリ URL を無効な URL に変更し、アプリを再実行します。

        self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];

2. いくつかの Todo 項目を追加するか、一部の項目を完了します。 シミュレーターを終了し (またはアプリケーションを強制的に閉じて)、再起動します。 変更内容が保存されていることを確認します。

3. リモートの TodoItem テーブルの内容を表示します。

    + Node.js バックエンドを参照してください、 [Azure ポータル](https://portal.azure.com/), 、モバイル アプリ バックエンド クリック **簡単なテーブル** > **TodoItem** の内容を表示する、 `TodoItem` テーブルです。
    + .NET バックエンドの場合は、SQL Server Management Studio などの SQL ツール、または Fiddler や Postman などの REST クライアントを使用して、テーブルの内容を表示します。

    新しい項目が、サーバーと同期*されなかった*ことを確認します。

4. **QSTodoService.m** の URL を正しい URL に変更し、アプリを再実行します。 項目の一覧をプルダウンして更新操作を実行します。 進行状況を示すスピナーとテキスト "同期中..." が表示されます。

5. TodoItem データをもう一度表示します。 新しく変更した TodoItems が表示されます。

## 概要

使用して、オフライン同期機能をサポートするために、 `MSSyncTable` インターフェイスおよび初期化さ `MSClient.syncContext` 、ローカル ストアを使用します。 この例では、ローカル ストアは、Core Data に基づいたデータベースでした。

Core Data ローカル ストアを使用する場合は、複数のテーブルを定義する必要があります、 [システムのプロパティを修正](#review-core-data)します。

Azure Mobile Apps に対する通常の CRUD 操作は、アプリケーションはまだ接続されているが、すべての操作はローカル ストアに対して発生したかのように動作します。

使用して、ローカル ストアをサーバーと同期しようとしたところ、 `MSSyncTable.pullWithQuery` と `MSClient.syncContext.pushWithCompletion` メソッドです。

*  サーバーに変更をプッシュするために呼び出しました `pushWithCompletion`します。 このメソッドのメンバーである `MSSyncContext` 、同期ではなくテーブルのすべてのテーブルに対して変更をプッシュするためです。

    何らかの方法で (CUD 操作により) ローカルで変更されたレコードだけが、サーバー宛てに送信されます。

* アプリケーション サーバーのテーブルからのデータをプルするため呼び出しました `MSSyncTable.pullWithQuery`します。

    プルは必ず、最初にプッシュを実行します。 これは、ローカル ストアのすべてのテーブルとリレーションシップの一貫性を確実に保つためです。

    なお `pullWithQuery` をカスタマイズして、クライアントに格納されているデータをフィルター処理を使用してできます、 `クエリ` パラメーター。

* 増分同期を有効にするをクエリ ID を渡します `pullWithQuery`します。 クエリ ID は、最後のプル操作の結果の最後に更新されたタイムスタンプを格納するのに使用されます。 クエリ ID は、アプリ内の各論理クエリに対して一意の、わかりやすい文字列にする必要があります。 クエリにパラメーターがある場合は、同じパラメーター値をクエリ ID の一部に含める必要があります。

    増分同期を無効にする場合は、渡す `nil` クエリ ID として 呼び出すたびにすべてのレコードを取得する場合は、 `pullWithQuery`, 、これは非効率です。



## その他のリソース

* [Azure のモバイル アプリでのオフライン データの同期]

* [カバーをクラウド: Azure Mobile Services でのオフライン同期] \ (注: ビデオは、モバイル サービスには、Azure モバイル アプリで同様の方法でのオフライン同期機能)





[create an ios app]: ../app-service-mobile-ios-get-started.md 
[offline data sync in azure mobile apps]: ../app-service-mobile-offline-data-sync.md 
[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png 
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png 
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png 
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png 
[cloud cover: offline sync in azure mobile services]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri 
[azure friday: offline-enabled apps in azure mobile services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/ 

