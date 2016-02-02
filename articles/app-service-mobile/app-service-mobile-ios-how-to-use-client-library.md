<properties
    pageTitle="Azure Mobile Apps 向け iOS SDK の使用方法"
    description="Azure Mobile Apps 向け iOS SDK の使用方法"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="11/30/2015"
    ms.author="krisragh"/>


# Azure Mobile Apps 向け iOS クライアント ライブラリの使用方法

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]
&nbsp;

[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

このガイドでは、最新版を使用して一般的なシナリオを実行する方法について説明 [Azure モバイル アプリ iOS SDK](https://go.microsoft.com/fwLink/?LinkID=266533&clcid=0x409)します。Azure モバイル アプリに慣れていない場合は最初完了 [Azure Mobile Apps のクイック スタート] をバックエンドを作成するテーブルを作成し、構築済みの iOS Xcode プロジェクトをダウンロードします。このガイドでは、クライアント側の iOS SDK に重点を置いています。バックエンドの .NET サーバー側 SDK に関する詳細については、を参照してください [.NET バックエンドを使用](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。

## リファレンス ドキュメント

IOS クライアント SDK リファレンス ドキュメントは、: [Azure モバイル アプリ iOS クライアント リファレンス](http://azure.github.io/azure-mobile-services/iOS/v3/)します。

## <a name="Setup"></a>セットアップと前提条件

このガイドでは、バックエンドとテーブルを作成済みであることを前提としています。 このガイドでは、テーブルのスキーマが、これらのチュートリアルのテーブルの場合と同じであることを前提とします。 このも前提として、コード内を参照している `WindowsAzureMobileServices.framework` およびインポート `WindowsAzureMobileServices/WindowsAzureMobileServices.h`します。

## <a name="create-client"></a>方法: クライアントを作成します。

プロジェクトでの Azure モバイル アプリ バックエンドにアクセスするには、作成、 `MSClient`します。 置換 `AppUrl` アプリ URL を使用しています。 おく `gatewayURLString` と `applicationKey` 空です。 認証用のゲートウェイを設定する場合は入力 `gatewayURLString` ゲートウェイの URL を使用しています。

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl" gatewayURLString:@"" applicationKey:@""];
```

## <a name="table-reference"></a>方法: テーブル参照を作成します。

データへのアクセスやデータの更新を行うには、バックエンド テーブルへの参照を作成します。 置換 `TodoItem` 、テーブルの名前に置き換えます。

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

## <a name="templates"></a>方法: レジスタ プッシュ テンプレート クロスプラット フォーム通知を送信するには

テンプレートを登録するには、**client.push registerDeviceToken** メソッドを使用してクライアント アプリにテンプレートを渡すだけです。

        [client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
            ...
        }];

テンプレートは NSDictionary 型となり、次のような形式で複数のテンプレートを含めることができます。

        NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };

セキュリティのためにタグはすべて取り除かれるので注意してください。 インストールやインストール内のテンプレートには、タグを追加するを参照してください。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags)します。

これらの登録済みのテンプレートを使用して通知を送信する操作 [通知ハブの Api](https://msdn.microsoft.com/library/azure/dn495101.aspx)

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
[azure mobile apps quick start]: app-service-mobile-ios-get-started.md 
[add mobile services to existing app]: /develop/mobile/tutorials/get-started-data 
[get started with mobile services]: /develop/mobile/tutorials/get-started-ios 
[validate and modify data in mobile services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios 
[mobile services sdk]: https://go.microsoft.com/fwLink/p/?LinkID=266533 
[authentication]: /develop/mobile/tutorials/get-started-with-users-ios 
[ios sdk]: https://developer.apple.com/xcode 
[handling expired tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955 
[live connect sdk]: http://go.microsoft.com/fwlink/p/?LinkId=301960 
[permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx 
[service-side authorization]: mobile-services-javascript-backend-service-side-authorization.md 
[use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios 
[dynamic schema]: http://go.microsoft.com/fwlink/p/?LinkId=296271 
[how to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers 
[create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx 
[nsdictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965 
[ascii control codes c0 and c1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set 
[cli to manage mobile services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables 
[conflict-handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling 

