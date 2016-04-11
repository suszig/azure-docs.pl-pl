

1. Visual Studio で、チュートリアルを完了したときに変更したプロジェクトを開く **データを使ってみる**します。

2. キーを押して、 **f5 キーを押して** キー、アプリの実行にテキストを入力して **Insert a TodoItem** ] をクリック **保存**します。

3. 前の手順を少なくとも 3 回繰り返して、TodoItem テーブルに項目を 3 つ以上保存します。 

2. Default.js ファイルに置き換える、 **RefreshTodoItems** メソッドを次のコード。

        var refreshTodoItems = function () {
            // Define a filtered query that returns the top 3 items.
            todoTable.where({ complete: false })
                .take(3)
                .read()
                .done(function (results) {
                    todoItems = new WinJS.Binding.List(results);
                    listItems.winControl.itemDataSource = todoItems.dataSource;
                });
        };

    このクエリは、データ バインド中に実行されると、完了マークが付けられていない上位 3 つの項目を返します。

3. キーを押して、 **f5 キーを押して** キーをアプリケーションを実行します。

    TodoItem テーブルから最初の 3 つの結果だけが表示されることに注目してください。 

4. (省略可能)ブラウザー開発者ツールなどのメッセージ検査ソフトウェアを使用して、モバイル サービスに送信された要求の URI を表示または [Fiddler]します。 

    注意して、 **take (3)** メソッドがクエリ オプションに変換された **$top = 3** クエリの URI でします。

5. 更新プログラム、 **RefreshTodoItems** メソッドを次のコードに置き換えます。
            
        var refreshTodoItems = function () {
            // Define a filtered query that skips the first 3 items and 
            // then returns the next 3 items.
            todoTable.where({ complete: false })
                .skip(3)
                .take(3)
                .read()
                .done(function (results) {
                    todoItems = new WinJS.Binding.List(results);
                    listItems.winControl.itemDataSource = todoItems.dataSource;
                });
        };

    このクエリでは、最初の 3 つの結果をスキップし、その後の 3 つを返します。 ページ サイズが 3 つの項目である場合、これは実質的にデータの 2 番目の "ページ" になります。

    > [AZURE.NOTE] このチュートリアルは、ハードコーディングされたページング値を渡すことによって簡略化されたシナリオを使用して、 **かかる** と **Skip** メソッドです。 実際のアプリケーションでは、ユーザーが前後のページに移動できるように、ページャー コントロールまたは同等の UI と共に上記と同様のクエリを使用することができます。  呼び出すことも、  **includeTotalCount** 、ページングされたデータと共に、サーバーで使用できる項目の合計数を取得します。

6. (省略可能) 再度、モバイル サービスに送信された要求の URI を表示します。 

    注意して、 **skip (3)** メソッドがクエリ オプションに変換された **$skip = 3** クエリの URI でします。

<!-- URLs -->
[Fiddler]: http://go.microsoft.com/fwlink/?LinkID=262412

