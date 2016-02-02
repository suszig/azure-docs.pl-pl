
1. Visual Studio で、Controllers フォルダーを右クリックし、**[追加]** を展開して **[新しいスキャフォールディング項目]** をクリックします。 [スキャフォールディングの追加] ダイアログ ボックスが表示されます。

2. 展開 **Azure Mobile Services**, 、] をクリックして **Azure Mobile Services カスタム コント ローラー**,  、] をクリックして **追加**, 指定した後、 **コント ローラー名** の `CompleteAllController`, 、] をクリック **追加** 再度します。

    ![Web API の ](./media/mobile-services-dotnet-backend-create-custom-api/add-custom-api-controller.png)

    **CompleteAllController** という名前の新しいからのコントローラー クラスが作成されます。
    >[AZURE.NOTE]ダイアログに Mobile Services 固有のスキャフォールディングがない場合は、代わりに新しい **Web API コントローラー - 空**を作成します。 この新しいコントローラー クラスで、**ApiServices** タイプを返すパブリックの **Services** プロパティを追加します。 このプロパティを使用して、コントローラー内からサーバー固有の設定にアクセスできます。

3. **CompleteAllController.cs** で、次の **using** ステートメントを追加します。 置換 `todolistService` 、モバイル サービス プロジェクトの名前空間がモバイル サービス名の付いたで `サービス`します。

        using System.Threading.Tasks;
        using todolistService.Models;

4. **CompleteAllController.cs** で、次のクラスを追加して、クライアントに送信される応答をラップします。

        // We use this class to keep parity with other Mobile Services
        // that use the JavaScript backend. This way the same client
        // code can call either type of Mobile Service backend.
        public class MarkAllResult
        {
            public int count;
        }

5. 新しいコントローラーに、次のコードを追加します。 置換 `todolistContext` したデータ モデルの DbContext の名前がモバイル サービス名の付いたで `コンテキスト`します。 同様に、UPDATE ステートメント内のスキーマ名を Mobile Services 名と置き換えます。 このコードを使用して、 [データベース クラス](http://msdn.microsoft.com/library/system.data.entity.database.aspx) にアクセスする、 **TodoItems** テーブルに直接すべての項目に完了のフラグを設定します。 このメソッドは POST 要求をサポートし、変更された行数が整数値としてクライアントに返されます。

     // POST api/completeall
     public async Task<MarkAllResult> Post()
     {
         using (todolistContext context = new todolistContext())
         {
             // Get the database from the context.
             var database = context.Database;
    
             // Create a SQL statement that sets all uncompleted items
             // to complete and execute the statement asynchronously.
             var sql = @"UPDATE todolist.TodoItems SET Complete = 1 " +
                         @"WHERE Complete = 0; SELECT @@ROWCOUNT as count";
    
             var result = new MarkAllResult();
             result.count = await database.ExecuteSqlCommandAsync(sql);
    
             // Log the result.
             Services.Log.Info(string.Format("{0} items set to 'complete'.",
                 result.count.ToString()));
    
             return result;
         }
     }

 > [AZURE.TIP] 既定のアクセス許可を使用して、アプリケーション キーを持つだれもがカスタム API を呼び出せます。 ただし、アプリケーション キーは安全に配布または格納されていない場合があるので、安全な資格情報とは見なされません。 セキュリティ強化のための認証されたユーザーにのみアクセスを制限することを検討します。





