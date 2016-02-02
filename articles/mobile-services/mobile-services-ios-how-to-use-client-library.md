<properties
    pageTitle="Azure Mobile Services 向け iOS クライアント ライブラリの使用方法"
    description="Mobile Services 向け iOS クライアント ライブラリの使用方法"
    services="mobile-services"
    documentationCenter="ios"
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2015"
    ms.author="krisragh"/>


# Azure Mobile Services 向け iOS クライアント ライブラリの使用方法

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-client-library](../../includes/mobile-services-selector-client-library.md)]

このガイドでは、Azure Mobile Services [iOS SDK] を使用して一般的なシナリオを実行する方法について説明します。 モバイル サービスに慣れていない場合は最初完了 [Mobile Services クイック スタート] を自分のアカウントを構成するテーブルを作成し、モバイル サービスを作成します。
> [AZURE.NOTE] このガイドでは、最新 [iOS 用 Mobile Services SDK](https://go.microsoft.com/fwLink/?LinkID=266533&clcid=0x409)します。プロジェクトで古いバージョンの SDK が使用されている場合は、まず Xcode のフレームワークをアップグレードします。

[AZURE.INCLUDE [mobile-services-concepts](../../includes/mobile-services-concepts.md)]

## <a name="Setup"></a>セットアップと前提条件

このガイドでは、前提条件として、モバイル サービスとテーブルを作成してあるとします。 詳細については [テーブルを作成] を参照するか、再利用、 `TodoItem` [Mobile Services クイック スタート] で作成されたテーブル。 このガイドでは、テーブルのスキーマが、これらのチュートリアルのテーブルの場合と同じであることを前提とします。 このも前提として、Xcode を参照している `WindowsAzureMobileServices.framework` しインポート `WindowsAzureMobileServices/WindowsAzureMobileServices.h`します。

## <a name="create-client"></a>方法: モバイル サービス クライアントを作成します。

プロジェクトで Azure のモバイル サービスにアクセスするには、作成、 `MSClient` クライアント オブジェクトです。 置換 `AppUrl` と `AppKey` をモバイル サービス URL とアプリケーション キー ダッシュ_ボード値にそれぞれします。

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl" applicationKey:@"AppKey"];
```

## <a name="table-reference"></a>方法: テーブル参照を作成します。

Azure Mobile Service のデータへのアクセスまたはデータの更新を行うには、テーブルへの参照を作成します。 置換 `TodoItem` 、テーブルの名前に置き換えます。

```
    MSTable *table = [client tableWithName:@"TodoItem"];
```

## <a name="querying"></a>方法: データのクエリ

データベース クエリを作成するには、クエリ、 `MSTable` オブジェクトです。 次のクエリすべての項目を取得する `TodoItem` し、各項目のテキストをログに記録します。

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

## <a name="filtering"></a>方法: 返されるデータをフィルター処理

結果をフィルター処理するには、多くのオプションを使用できます。

フィルター処理する述語を使用して、 `NSPredicate` と `readWithPredicate`します。 次のコードは、返されたデータをフィルター処理して、不完全な Todo 項目のみを見つけます。

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table and update the items property with the results from the service
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

## <a name="query-object"></a>方法: MSQuery を使用

複雑なクエリ (並べ替えとページングを含む) を実行するには、作成、 `MSQuery` オブジェクトを直接または、述語を使用しています。

```
    MSQuery *query = [table query];
    MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

`MSQuery` 、次のような複数のクエリ動作を制御することができます。 実行、 `MSQuery` を呼び出してクエリ `readWithCompletion` で、次の例に示すようにします。
* 結果の順序を指定します。
* 取得するには、どのフィールドを制限します。
* 返されるレコードの数を制限します。
* 応答の合計数を指定します。
* 要求にカスタム クエリ文字列パラメーターを指定します。
* その他の関数を適用します。


## <a name="sorting"></a>方法: MSQuery でデータを並べ替える

結果の並べ替えの例を見てみましょう。 フィールドの昇順で `テキスト` 、フィールドを並べ替えて順 `完了`, 、呼び出す `MSQuery` 次のようにします。

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

## <a name="paging"></a>方法: MSQuery でページにデータを返す

モバイル サービスでは、1 つの応答で返されるレコードの量が制限されます。 ユーザーに表示されるレコードの数を制御するには、ページング システムを実装する必要があります。 ページングを行うには、**MSQuery** オブジェクトの次の 3 つのプロパティを使用します。

```
+   `BOOL includeTotalCount`
+   `NSInteger fetchLimit`
+   `NSInteger fetchOffset`
```

次の例では、単純な関数によって、サーバーから 5 のレコードを要求し、それを以前に読み込まれたレコードのローカル コレクションに追加しています。

```
// Create and initialize these properties
@property (nonatomic, strong)   NSMutableArray *loadedItems; // Init via [[NSMutableArray alloc] init]
@property (nonatomic)                   BOOL moreResults;
```

```
-(void)loadResults
{
    MSQuery *query = [self.table query];

    query.includeTotalCount = YES;
    query.fetchLimit = 5;
    query.fetchOffset = self.loadedItems.count;


    [query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(!error) {
            // Add the items to our local copy
            [self.loadedItems addObjectsFromArray:result.items];

            // Set a flag to keep track if there are any additional records we need to load
            self.moreResults = (self.loadedItems.count <= result.totalCount);
        }
    }];
}
```

## <a name="selecting"></a><a name="parameters"></a>方法: フィールドを制限し、MSQuery でクエリ文字列パラメーターを展開

クエリで返されるフィールドを制限するには、**selectFields** プロパティにフィールド名を指定します。 次の例では、text フィールドと completed フィールドのみが返されます。

```
    query.selectFields = @[@"text", @"completed"];
```

(たとえば、カスタム サーバー側スクリプトを使用する) ために、サーバー要求に追加のクエリ文字列パラメーターを含める設定を `query.parameters` 次のようにします。

```
    query.parameters = @{
        @"myKey1" : @"value1",
        @"myKey2" : @"value2",
    };
```

## <a name="inserting"></a>方法: データの挿入

新しいテーブル行を挿入するには、新しい作成 `NSDictionary` を呼び出すと `表の挿入`します。 モバイル サービスに基づく新しい列を自動的に生成する、 `NSDictionary` [動的スキーマ] が無効になっていない場合。

場合 `id` が提供されていない場合、バックエンドは、新しい一意の ID を自動的に生成されます 独自に提供 `id` 電子メールを使用するアドレス、ユーザー名、または、独自のカスタム値を ID として 独自の ID を指定すると、結合およびビジネス指向のデータベース ロジックを容易にすることができます。

```
    NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
    [self.table insert:newItem completion:^(NSDictionary *result, NSError *error) {
        // The result contains the new item that was inserted,
        // depending on your server scripts it may have additional or modified
        // data compared to what was passed to the server.
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
        }
    }];
```

## <a name="modifying"></a>方法: データの変更

既存の行を更新するアイテムと呼び出しを変更 `更新`:

```
    NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
    [newItem setValue:@"Updated text" forKey:@"text"];
    [self.table update:newItem completion:^(NSDictionary *item, NSError *error) {
        // Handle error or perform additional logic as needed
    }];
```

あるいは、次のように行 ID と更新フィールドを指定します。

```
    [self.table update:@{@"id":@"37BBF396-11F0-4B39-85C8-B319C729AF6D", @"Complete":@YES} completion:^(NSDictionary *item, NSError *error) {
        // Handle error or perform additional logic as needed
    }];
```

少なくとも、 `id` の更新を行った場合は、属性を設定する必要があります。

## <a name="deleting"></a>方法: データを削除します。

項目を削除するには、呼び出す `削除` 項目にします。

```
    [self.table delete:item completion:^(id itemId, NSError *error) {
        // Handle error or perform additional logic as needed
    }];
```

あるいは、次のように行 ID を指定して削除します。

```
    [self.table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
        // Handle error or perform additional logic as needed
    }];
```

少なくとも、 `id` 削除操作を行う場合は、属性を設定する必要があります。

## <a name="#custom-api"></a>方法: カスタム API 呼び出し

カスタム API を使用してカスタム エンドポイントを定義することにより、insert、update、delete、read のいずれの操作にも関連しないサーバー機能を公開することができます。 カスタム API を使用することによって、HTTP メッセージ ヘッダーの読み取りや設定、JSON 以外のメッセージ本文形式の定義など、メッセージングをより柔軟に制御することができます。 モバイル サービスでカスタム API を作成する方法の例は、次を参照してください。 [方法: カスタム API エンドポイントを定義する](mobile-services-dotnet-backend-define-custom-api.md)します。

[AZURE.INCLUDE [mobile-services-ios-call-custom-api](../../includes/mobile-services-ios-call-custom-api.md)]


## <a name="authentication"></a>方法: ユーザーを認証します。

Azure Mobile Services は、さまざまな ID プロバイダーをサポートします。 基本的なチュートリアルでは、[認証] を参照してください。

Azure Mobile Services は、次の 2 つの認証ワークフローをサポートします。

- **サーバーによって管理されるログイン**: Azure Mobile Services がアプリケーションに代わってログオン プロセスを管理します。 Azure Mobile Services は、プロバイダー固有のログイン ページを表示し、選択したプロバイダーで認証されます。

- **クライアントによって管理されるログイン**: _アプリケーション_は ID プロバイダーにトークンを要求し、認証のためにこのトークンを Azure Mobile Services に示します。

認証が成功すると、ユーザー ID の値と認証トークンを含むユーザー オブジェクトが返されます。 このユーザー ID を使用して、ユーザーを承認するために、[サービス側の承認] を参照してください。 テーブルへのアクセスを認証されたユーザーのみに制限は、[アクセス許可] を参照してください。

### サーバーによって管理されるログイン

ここでは、[Mobile Services クイック スタート] プロジェクトをサーバーによって管理されるログインを追加する方法似たようなコードは、他のプロジェクトを使用できます。 詳細については、およびエンド ツー エンド例の動作を確認するには、[認証] を参照してください。

[AZURE.INCLUDE [mobile-services-ios-authenticate-app](../../includes/mobile-services-ios-authenticate-app.md)]

### クライアントによって管理されるログイン (シングル サインオン)

ログイン プロセスが Mobile Services クライアントの外部で実行される場合もあります。このような操作は、シングル サインオン サービス機能を有効にする場合や、アプリケーションで ID プロバイダーにアクセスする場合に使用します。 このような場合は、サポートされている ID プロバイダーから個別に取得したトークンを指定することによって、Mobile Services にログインできます。

次の例では、[Live Connect SDK] を使用して、iOS アプリケーションのシングル サインオンを有効にします。 あることを前提としていますが、 **LiveConnectClient** という名前のインスタンス `liveClient` コント ローラーと、ユーザーに記録されます。

```
    [client loginWithProvider:@"microsoftaccount"
        token:@{@"authenticationToken" : self.liveClient.session.authenticationToken}
        completion:^(MSUser *user, NSError *error) {
                // Handle success and errors
    }];
```

## <a name="caching-tokens"></a>方法: 認証トークンをキャッシュします。

[Mobile Services クイック スタート] プロジェクト内のトークンをキャッシュする方法を見てみましょう任意のプロジェクトに同様の手順を適用することもできます。 [AZURE.INCLUDE [mobile-services-ios-authenticate-app-with-token](../../includes/mobile-services-ios-authenticate-app-with-token.md)]

## <a name="errors"></a>方法: エラーを処理します。

モバイル サービスを呼び出したときに、完了ブロックを格納、 `NSError * エラー` パラメーター。 エラーが発生すると、このパラメーターは null 以外の値になります。 コードでは、このパラメーターを確認し、必要に応じてエラーを処理する必要があります。

ファイル [`< WindowsAzureMobileServices/MSError.h >`] (https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h)、定数を定義する `MSErrorResponseKey`, 、`MSErrorRequestKey`, 、および `MSErrorServerItemKey` エラーに関連する複数のデータを取得します。さらに、ファイルで、エラー コードごとに定数を定義します。これらの定数を使用する方法の例を参照してください [競合ハンドラー] の使用時の `MSErrorServerItemKey` と `MSErrorPreconditionFailed`します。







[what is mobile services]: #what-is 
[concepts]: #concepts 
[setup and prerequisites]: #Setup 
[how to: create the mobile services client]: #create-client 
[how to: create a table reference]: #table-reference 
[how to: query data from a mobile service]: #querying 
[filter returned data]: #filtering 
[sort returned data]: #sorting 
[return data in pages]: #paging 
[select specific columns]: #selecting 
[how to: bind data to the user interface]: #binding 
[how to: insert data into a mobile service]: #inserting 
[how to: modify data in a mobile service]: #modifying 
[how to: authenticate users]: #authentication 
[cache authentication tokens]: #caching-tokens 
[how to: upload images and large files]: #blobs 
[how to: handle errors]: #errors 
[how to: design unit tests]: #unit-testing 
[how to: customize the client]: #customizing 
[customize request headers]: #custom-headers 
[customize data type serialization]: #custom-serialization 
[next steps]: #next-steps 
[how to: use msquery]: #query-object 
[mobile services quick start]: mobile-services-ios-get-started.md 
[get started with mobile services]: mobile-services-ios-get-started.md 
[mobile services sdk]: https://go.microsoft.com/fwLink/p/?LinkID=266533 
[authentication]: /develop/mobile/tutorials/get-started-with-users-ios 
[ios sdk]: https://developer.apple.com/xcode 
[handling expired tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955 
[live connect sdk]: http://go.microsoft.com/fwlink/p/?LinkId=301960 
[permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx 
[service-side authorization]: mobile-services-javascript-backend-service-side-authorization.md 
[dynamic schema]: http://go.microsoft.com/fwlink/p/?LinkId=296271 
[create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx 
[nsdictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965 
[ascii control codes c0 and c1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set 
[cli to manage mobile services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables 
[conflict-handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling 

