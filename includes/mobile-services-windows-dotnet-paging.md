
1. Visual Studio で、「**モバイル サービスでのデータの使用**」チュートリアルを実行したときに変更したプロジェクトを開きます。

2. **F5** キーを押してアプリケーションを実行し、**[Insert a TodoItem]** ボックスにテキストを入力して、**[Save]** をクリックします。

3. 前の手順を少なくとも 3 回繰り返して、TodoItem テーブルに項目を 3 つ以上保存します。

2. MainPage.xaml.cs ファイルで、**RefreshTodoItems** メソッドを次のコードに置き換えます。

        private async void RefreshTodoItems()
        {
            // Define a filtered query that returns the top 3 items.
            IMobileServiceTableQuery<TodoItem> query = todoTable
                            .Where(todoItem => todoItem.Complete == false)
                           .Take(3);
            items = await query.ToCollectionAsync();
            ListItems.ItemsSource = items;
        }

    このクエリは、データ バインド中に実行されると、完了マークが付けられていない上位 3 つの項目を返します。

3. **F5** キーを押してアプリケーションを実行します。

    TodoItem テーブルから最初の 3 つの結果だけが表示されることに注目してください。

4. (省略可能) ブラウザー開発者ツールや [Fiddler] などのメッセージ検査ソフトウェアを使用して、モバイル サービスに送信された要求の URI を表示します。

    注意して、 `take (3)` メソッドがクエリ オプションに変換された `$top = 3` クエリの URI でします。

5. 再度 **RefreshTodoItems** メソッドを次のコードに置き換えます。

        private async void RefreshTodoItems()
        {
            // Define a filtered query that skips the first 3 items and 
            // then returns the next 3 items.
            IMobileServiceTableQuery<TodoItem> query = todoTable
                           .Where(todoItem => todoItem.Complete == false)
                           .Skip(3)
                           .Take(3);
            items = await query.ToCollectionAsync();
            ListItems.ItemsSource = items;
        }

    このクエリでは、最初の 3 つの結果をスキップし、その後の 3 つを返します。 ページ サイズが 3 つの項目である場合、これは実質的にデータの 2 番目の "ページ" になります。
    >[AZURE.NOTE]このチュートリアルは、ハードコーディングされたページング値を渡すことによって簡略化されたシナリオを使用して、 <strong>かかる</strong> と <strong>Skip</strong> メソッドです。 実際のアプリケーションでは、ユーザーが前後のページに移動できるように、ページャー コントロールまたは同等の UI と共に上記と同様のクエリを使用することができます。 呼び出すことも、  <strong>IncludeTotalCount</strong> 、ページングされたデータと共に、サーバーで使用できる項目の合計数を取得します。

6. (省略可能) 再度、モバイル サービスに送信された要求の URI を表示します。

    注意して、 `skip (3)` メソッドがクエリ オプションに変換された `$skip = 3` クエリの URI でします。



[fiddler]: http://go.microsoft.com/fwlink/?LinkID=262412 

