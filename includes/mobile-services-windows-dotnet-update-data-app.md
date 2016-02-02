2. TodoItem クラス定義を次のコードで置き換えます。

     public class TodoItem
     {
         public string Id { get; set; }
    
         [Newtonsoft.Json.JsonProperty(PropertyName = "text")]  
         public string Text { get; set; }
    
         [Newtonsoft.Json.JsonProperty(PropertyName = "complete")]  
         public bool Complete { get; set; }
     }

 **JsonPropertyAttribute** は、クライアント型のプロパティ名と基になるデータ テーブル内の列名との間のマッピングを定義するために使用します。
 >[AZURE.NOTE] ユニバーサル Windows アプリ プロジェクトでは、TodoItem クラスは共有する DataModel フォルダーにある別のコード ファイルで定義されます。

1. MainPage.cs ファイルで、次の using ステートメントを追加またはコメント解除します。

        using Microsoft.WindowsAzure.MobileServices;

4. コメント アウトまたは既存の items コレクションを定義する行を削除し、コメントを解除または置き換える以下の行を追加 _< yourClient >_ で、 `MobileServiceClient` 、プロジェクトをモバイル サービスに接続したときに、App.xaml.cs ファイルに追加されたフィールド。

        private MobileServiceCollection<TodoItem, TodoItem> items;
        private IMobileServiceTable<TodoItem> todoTable = 
            App.<yourClient>.GetTable<TodoItem>();

    このコードは、モバイル サービス対応のバインディング コレクション (items) とデータベース テーブルのプロキシ クラス (todoTable) を作成します。

4. **InsertTodoItem** メソッド内で、**TodoItem.Id** プロパティを設定するコード行を削除し、**async** 修飾子をメソッドに追加して、次のコード行をコメント解除します。

        await todoTable.InsertAsync(todoItem);

    このコードでは、新しい項目をテーブルに挿入します。

5. **RefreshTodoItems** メソッドを次のコードで置き換えます。

        private async void RefreshTodoItems()
        {
            MobileServiceInvalidOperationException exception = null;
            try
            {
                // Query that returns all items.   
                items = await todoTable.ToCollectionAsync();             
            }
            catch (MobileServiceInvalidOperationException e)
            {
                exception = e;
            }
            if (exception != null)
            {
                await new MessageDialog(exception.Message, "Error loading items").ShowAsync();
            }
            else
            {
                ListItems.ItemsSource = items;
                this.ButtonSave.IsEnabled = true;
            }    
        }

    これに項目のコレクションにバインディングが設定 `todoTable`, のすべてを含む、 **TodoItem** 、モバイル サービスからオブジェクトが返されます。 クエリの実行に問題がある場合、エラーを示すメッセージ ボックスが表示されます。

6. **UpdateCheckedTodoItem** メソッド内で、**async** 修飾子をメソッドに追加して、次のコード行をコメント解除します。

        await todoTable.UpdateAsync(item);

    これにより、項目の更新がモバイル サービスに送信されます。

バックエンド ストレージのモバイル サービスを使用するようにアプリケーションを更新した後は、モバイル サービスに対してアプリケーションをテストします。




