
1. Visual Studio ソリューション エクスプ ローラーで、展開、 **コント ローラー** モバイル サービス プロジェクト内のフォルダーです。 TodoItemController.cs を開きます。 ファイルの先頭に、次の `using` ステートメントを追加します。

        using System;
        using System.Collections.Generic;

2. 次のコードで `PostTodoItem` メソッドの定義を更新します。  

        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);

            Dictionary<string, string> data = new Dictionary<string, string>()
            {
                { "message", item.Text}
            };
            GooglePushMessage message = new GooglePushMessage(data, TimeSpan.FromHours(1));

            try
            {
                var result = await Services.Push.SendAsync(message);
                Services.Log.Info(result.State.ToString());
            }
            catch (System.Exception ex)
            {
                Services.Log.Error(ex.Message, null, "Push.SendAsync Error");
            }
            return CreatedAtRoute("Tables", new { id = current.Id }, current);
        }

    Todo 項目を挿入した後、このコードは (挿入された項目のテキストを使用して) プッシュ通知を送信します。 コードに表示されるエラー ログ エントリを追加、エラーが発生した場合、 **ログ** 内でモバイル サービス] タブ、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。

3. モバイル サービス プロジェクトを Azure に対して再発行します。
