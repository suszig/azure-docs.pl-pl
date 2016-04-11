
Visual Studio に戻り、共有コード クライアント アプリ プロジェクトを選択します (`<your app name>.Shared` のような名前になります)。

1. 展開、 **App.xaml** ファイル、開き、 **App.xaml.cs** ファイルです。 localhost URL で初期化される `MobileService` メンバー宣言を探します。 この宣言を (`CTRL+K,CTRL+C` を押して) コメント化し、ホストされるサービスに接続する宣言を (`CTRL+K,CTRL+U` を押して) コメント解除します。

        // This MobileServiceClient has been configured to communicate with your local
        // test project for debugging purposes.
        //public static MobileServiceClient MobileService = new MobileServiceClient(
        //    "http://localhost:58454"
        //);

        // This MobileServiceClient has been configured to communicate with the Azure Mobile Service and
        // Azure Gateway using the application key. You're all set to start working with your Mobile Service!
        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://mymobileapp-code.azurewebsites.net",
            "https://myresourcegroupgateway.azurewebsites.net/Microsoft.Azure.AppService.ApiApps.Gateway",
            "XXXX-APPLICATION-KEY-XXXXX"
        );

2. F5 キーを押してプロジェクトをリビルドし、Windows ストア アプリを開始します。これが既定のスタートアップ プロジェクトになります。

2. アプリケーションで、意味のあるテキストをなど入力 *に関するチュートリアルを完了*, で、 **Insert a TodoItem** ] ボックスに、順にクリック **保存**します。

    ![](./media/app-service-mobile-windows-universal-test-app/mobile-quickstart-startup.png)

    これで、Azure でホストされている新しいモバイル アプリ バックエンドに POST 要求が送信されます。

3. デバッグを停止し、Windows Phone ストア アプリにユニバーサル Windows ソリューションにプロジェクトを既定のスタートアップを変更 (右クリックし、 `<your app name>.WindowsPhone` プロジェクトを **スタートアップ プロジェクトとして設定**) してもう一度 f5 キーを押します。

    ![](./media/app-service-mobile-windows-universal-test-app/mobile-quickstart-completed-wp8.png)

    Windows アプリの開始後に、前の手順で保存したデータがモバイル アプリから読み込まれることに注目してください。


