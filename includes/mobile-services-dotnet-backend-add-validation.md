ユーザーにより送信されたデータの長さを検証することをお勧めします。 このセクションでは、モバイル サービスに送信された文字列データの長さを検証し、長すぎる文字列 (この場合は 10 文字を超える) を拒否するコードを追加します。

1. Visual Studio を起動、 **管理者として実行** オプションし、[作業の開始] で使用したモバイル サービス プロジェクトが含まれているソリューションを開くまたは [データの](../articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md)  チュートリアルです。

2. ソリューション エクスプローラー ウィンドウで、todo list サービス プロジェクトを展開し、**[Contollers]** フォルダーを展開します。 モバイル サービス プロジェクトの一部である TodoItemController.cs ファイルを開きます。

    ![](./media/mobile-services-dotnet-backend-add-validation/mobile-services-open-todoitemcontroller.png)

3. 置き換える、 `PostTodoItem` テキスト文字列が 10 文字より長いことを検証する次のメソッドを持つメソッドです。 項目のテキストが 10 文字より長い場合は、説明メッセージをコンテンツとして含む "HTTP 状態コード 400 – Bad Request" が返されます。

        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            if (item.Text.Length > 10)
            {
                return BadRequest("The Item's Text length must not be greater than 10.");
            }
            else
            {
                TodoItem current = await InsertAsync(item);
                return CreatedAtRoute("Tables", new { id = current.Id }, current);
            } 
        }

4. サービス プロジェクトを右クリックし、**[ビルド]** をクリックしてモバイル サービス プロジェクトをビルドします。 エラーが発生しないことを確認します。

    ![](./media/mobile-services-dotnet-backend-add-validation/mobile-services-build-dotnet-service.png)

5. サービス プロジェクトを右クリックし、**[発行]** をクリックします。 [作業の開始] で使用した発行の設定を使用して、Microsoft Azure アカウントにモバイル サービスに発行または [データを使ってみる](../articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md)  チュートリアルです。
     >[AZURE.NOTE] または、IIS Express でローカルにホストされているサービスを使用してテストすることもできます。 詳細については、次を参照してください。、 [データを使ってみる](../articles/mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md) チュートリアルです。

    ![](./media/mobile-services-dotnet-backend-add-validation/mobile-services-publish-dotnet-service.png)







[getting started]: ../articles/mobile-services/mobile-services-dotnet-backend-windows-store-dotnet-get-started.md 

