1. Visual Studio のソリューション エクスプローラーで、モバイル サービス プロジェクト内の **Controllers** フォルダーを右クリックします。 TodoItemController.cs を開きます。 ファイルの上部にある次のコードを追加 `を使用して` ステートメント。

        using System;
        using System.Collections.Generic;

2. 更新プログラム、 `PostTodoItem` メソッド定義を次のコード。

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




