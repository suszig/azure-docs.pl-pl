
次に、メモリ内コレクションではなく Azure Mobile Services に項目を格納するようにアプリケーションを更新します。

*  **TodoService.h**, 、次の行を見つけます。

```
// TODO - create an MSClient property
```

このコメントを次の行に置き換えます。 これで、サービスに接続される `MSClient` を表すプロパティが作成されます。

```
@property (nonatomic, strong)   MSClient *client;
```


*  **TodoService.m**, 、次の行を見つけます。

```
// TODO - create an MSTable property for your items
```

このコメントを、`@interface` 宣言内の次の行に置き換えます。 これで、モバイル サービス テーブルのプロパティ表現が作成されます。

```
@property (nonatomic, strong)   MSTable *table;
```


*  [Azure クラシック ポータル](https://manage.windowsazure.com/), 、] をクリックして **Mobile Services**, をモバイル サービスをクリックします。 クリックして、 **ダッシュ ボード** タブし、メモ、 **サイトの URL**します。 クリックし、 **キーの管理** し、メモ、 **アプリケーション キー**します。 これらの値は、アプリケーション コードからモバイル サービスにアクセスするときに必要になります。


*  **TodoService.m**, 、次の行を見つけます。

```
// Initialize the Mobile Service client with your URL and key.
```

このコメントの後ろに、次のコード行を追加します。 `APPURL` と `APPKEY` を、前の手順で取得したサイト URL とアプリケーション キーに置き換えます。

```
self.client = [MSClient clientWithApplicationURLString:@"APPURL" applicationKey:@"APPKEY"];
```


*  **TodoService.m**, 、次の行を見つけます。

```
// Create an MSTable instance to allow us to work with the TodoItem table.
```

このコメントを次の行に置き換えます。 これで、TodoItem テーブルのインスタンスが作成されます。

```
self.table = [self.client tableWithName:@"TodoItem"];
```


*  **TodoService.m**, 、次の行を見つけます。

```
// Create a predicate that finds items where complete is false
```

このコメントを次の行に置き換えます。 これで、まだ完了していないタスクをすべて返すクエリが作成されます。

```
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
```


* 次の行を見つけます。

```
// Query the TodoItem table and update the items property with the results from the service
```

コメントとそれに続くを置き換える **完了** ブロックの呼び出しを次のコード。

```
[self.table readWhere:predicate completion:^(NSArray *results, NSInteger totalCount, NSError *error)
{
self.items = [results mutableCopy];
   completion();
}];
```


* 検索、 **addItem** メソッドの本体を次のコードに置き換えます。 このコードにより、挿入要求がモバイル サービスに送信されます。

```
// Insert the item into the TodoItem table and add to the items array on completion
[self.table insert:item completion:^(NSDictionary *result, NSError *error) {
    NSUInteger index = [items count];
    [(NSMutableArray *)items insertObject:item atIndex:index];

    // Let the caller know that we finished
    completion(index);
}];
```


* 検索、 **completeItem** メソッド、コードの次のコメントがある行を見つけます。

```
// Update the item in the TodoItem table and remove from the items array on completion
```

メソッド本文のその位置から末尾までを次のコードに置き換えます。 このコードでは、Todo 項目に完了マークが付けられたら、それを削除します。

```
// Update the item in the TodoItem table and remove from the items array on completion
[self.table update:mutable completion:^(NSDictionary *item, NSError *error) {

    // Get a fresh index in case the list has changed
    NSUInteger index = [items indexOfObjectIdenticalTo:mutable];

    [mutableItems removeObjectAtIndex:index];

    // Let the caller know that we have finished
    completion(index);
}];
```


* Todolistcontroller.m で、 **onAdd** メソッドと、次のコードで上書きします。

```
- (IBAction)onAdd:(id)sender
{
    if (itemText.text.length  == 0)
    {
        return;
    }

    NSDictionary *item = @{ @"text" : itemText.text, @"complete" : @NO };
    UITableView *view = self.tableView;
    [self.todoService addItem:item completion:^(NSUInteger index)
    {
        NSIndexPath *indexPath = [NSIndexPath indexPathForRow:index inSection:0];
        [view insertRowsAtIndexPaths:@[ indexPath ]
                    withRowAnimation:UITableViewRowAnimationTop];
    }];

    itemText.text = @"";
}
```

