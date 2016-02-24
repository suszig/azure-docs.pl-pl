

1. Visual Studio で、Controllers フォルダーを右クリックし、展開 **追加**, 、クリックして **新しいスキャフォールディング項目**します。 [スキャフォールディングの追加] ダイアログ ボックスが表示されます。

2. 展開 **Azure Mobile Services**, 、] をクリックして **Azure Mobile Services カスタム コント ローラー**,  、] をクリックして **追加**, 指定した後、 **コント ローラー名** の `CompleteAllController`, 、] をクリック **追加** 再度します。

    ![Web API の [スキャフォールディングの追加] ダイアログ](./media/mobile-services-dotnet-backend-create-custom-api/add-custom-api-controller.png)

    という名前の新しい空のコント ローラー クラス作成 **CompleteAllController**します。

    >[AZURE.NOTE]ダイアログには、モバイル サービス固有のスキャフォールディングを割り当てられていない、代わりに作成する新しい **Web API コント ローラー - 空**します。 この新しいコント ローラー クラスで、追加のパブリック **サービス** を返すプロパティ、 **ApiServices** 型です。 このプロパティを使用して、コントローラー内からサーバー固有の設定にアクセスできます。

3.  **CompleteAllController.cs**, 、次の追加 **を使用して** ステートメントです。     `todolistService` を Mobile Services プロジェクトの名前空間 (Mobile Services 名の後に `Service` が付いた名前) と置き換えます。

        using System.Threading.Tasks;
        using todolistService.Models;

4.  **CompleteAllController.cs**, 、クライアントに送信された応答をラップする次のクラスを追加します。

        // We use this class to keep parity with other Mobile Services
        // that use the JavaScript backend. This way the same client
        // code can call either type of Mobile Service backend.
        public class MarkAllResult
        {
            public int count;
        }

5. 新しいコントローラーに、次のコードを追加します。 `todolistContext` をデータ モデルの DbContext の名前 (Mobile Services 名の後に `Context` が付いた名前) と置き換えます。 同様に、UPDATE ステートメント内のスキーマ名を Mobile Services 名と置き換えます。 このコードを使用して、 [データベース クラス](http://msdn.microsoft.com/library/system.data.entity.database.aspx) にアクセスする、 **TodoItems** テーブルに直接すべての項目に完了のフラグを設定します。 このメソッドは POST 要求をサポートし、変更された行数が整数値としてクライアントに返されます。


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

    > [AZURE.TIP] With default permissions, anyone with the app key may call the custom API. However, the application key is not considered a secure credential because it may not be distributed or stored securely. Consider restricting access to only authenticated users for additional security.

