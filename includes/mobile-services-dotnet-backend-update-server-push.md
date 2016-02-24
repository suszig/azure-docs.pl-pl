
1. Visual Studio ソリューション エクスプ ローラーで、展開、 **コント ローラー** モバイル サービス プロジェクト内のフォルダーです。 TodoItemController.cs を開き、次のコードを使用して `PostTodoItem` メソッドの定義を更新します。  

        public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
        {
            TodoItem current = await InsertAsync(item);

            // Create a WNS native toast.
            WindowsPushMessage message = new WindowsPushMessage();

            // Define the XML paylod for a WNS native toast notification 
            // that contains the text of the inserted item.
            message.XmlPayload = @"<?xml version=""1.0"" encoding=""utf-8""?>" +
                                 @"<toast><visual><binding template=""ToastText01"">" +
                                 @"<text id=""1"">" + item.Text + @"</text>" +
                                 @"</binding></visual></toast>";
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

    >[AZURE.NOTE] テンプレート通知を使用して、複数のプラットフォーム上のクライアントに 1 つのプッシュ通知を送信することができます。 詳細については、次を参照してください。 [シングル モバイル サービスから複数のデバイス プラットフォームをサポートする](../articles/mobile-services-how-to-use-multiple-clients-single-service.md#push)です。

2. モバイル サービス プロジェクトを Azure に対して再発行します。



