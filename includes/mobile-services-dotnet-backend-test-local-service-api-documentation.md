
1. ソリューション エクスプ ローラーで Visual Studio でサービス プロジェクトを右クリックし、クリックして **新しいインスタンスを開始** 下にある、 **デバッグ** コンテキスト メニューです。

    ![モバイル サービス プロジェクトをローカルに開始する](./media/mobile-services-dotnet-backend-test-local-service-api-documentation/vs-start-debug-service-project.png)

    Visual Studio では、サービスの既定の Web ページが表示されます。 既定では、Visual Studio によって、モバイル サービスは IIS Express にローカルでホストされます。

2. Windows タスクバーにある IIS Express のトレイ アイコンを右クリックして、モバイル サービスが開始していることを確認します。

     ![タスクバーにあるモバイル サービスを確認する](./media/mobile-services-dotnet-backend-test-local-service-api-documentation/iis-express-tray.png)

3. .NET バックエンドの開始ページでクリックして **試してみる**します。

    ![[モバイル サービスの開始] ページ](./media/mobile-services-dotnet-backend-test-local-service-api-documentation/service-welcome-page.png)

    API ドキュメントのページが表示されます。このページを使用してモバイル サービスをテストします。

    >[AZURE.NOTE]認証は、ローカルで実行するときにこのページにアクセスする必要はありません。 Azure で実行する場合、このページにアクセスするには、(ユーザー名なしで) パスワードとしてアプリケーション キーを指定する必要があります。

4. クリックして、 **GET Tables/todoitem** リンクします。

    ![](./media/mobile-services-dotnet-backend-test-local-service-api-documentation/service-api-documentation-page.png)
    
    これにより、API の GET 応答ページが表示されます。

5. クリックして **試すことが** ] をクリックし、 **送信**します。
 
    ![](./media/mobile-services-dotnet-backend-test-local-service-api-documentation/service-try-this-out-get-todoitems.png)

    TodoItem テーブルのすべての行を返すローカル モバイル サービスに対して GET 要求を送信します。 テーブルが初期化子によってシードされているため、応答メッセージの本体に 2 つの TodoItem オブジェクトが返されます。 初期化子の詳細については、次を参照してください。 [.NET バックエンド モバイル サービスにデータ モデルの変更を加える方法](../articles/mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)します。

    ![](./media/mobile-services-dotnet-backend-test-local-service-api-documentation/service-try-this-out-get-response.png)
