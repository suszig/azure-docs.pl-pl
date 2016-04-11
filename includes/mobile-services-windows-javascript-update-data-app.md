

1. 次に、コメントを解除するか次のコード行を追加して、`<yourClient>` をプロジェクトをモバイル サービスに接続したときの service.js ファイルに追加された変数で置き換えます。

        var todoTable = <yourClient>.getTable('TodoItem');

    このコードは、プロキシ オブジェクトを作成 (**todoTable**)、新しいデータベース テーブルのキャッシュを使用してフィルター処理します。 

2. 置き換える、 **InsertTodoItem** 関数を次のコード。

        var insertTodoItem = function (todoItem) {
            // Inserts a new row into the database. When the operation completes
            // and Mobile Services has assigned an id, the item is added to the binding list.
            todoTable.insert(todoItem).done(function (item) {
                todoItems.push(item);
            });
        };

    このコードでは、新しい項目をテーブルに挿入します。

3. 置き換える、 **RefreshTodoItems** 関数を次のコード。

        var refreshTodoItems = function () {
            // This code refreshes the entries in the list by querying the table.
            // Results are filtered to remove completed items.
            todoTable.where({ complete: false })
                .read().done(function (results) {
                todoItems = new WinJS.Binding.List(results);
                listItems.winControl.itemDataSource = todoItems.dataSource;
            });
        };

    これにより、todoTable 内でのすべてが格納される項目のコレクションへのバインドの設定、 **TodoItem** 、モバイル サービスからオブジェクトが返されます。 

4. 置き換える、 **UpdateCheckedTodoItem** 関数を次のコード。
        
        var updateCheckedTodoItem = function (todoItem) {
            // This code takes a freshly completed TodoItem and updates the database. 
            todoTable.update(todoItem);
            // Remove the completed item from the filtered list.
            todoItems.splice(todoItems.indexOf(todoItem), 1);
        };

    これにより、項目の更新がモバイル サービスに送信されます。

バックエンド ストレージのモバイル サービスを使用するようにアプリケーションを更新した後は、モバイル サービスに対してアプリケーションをテストします。

