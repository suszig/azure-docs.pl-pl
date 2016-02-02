Visual Studio に戻り、共有コード クライアント アプリ プロジェクトを選択 (のような名前は `< アプリ名 >。共有`)

1. **App.xaml** ファイルを展開し、**App.xaml.cs** ファイルを開きます。 宣言を探して、 `MobileService` localhost URL で初期化されるメンバーです。 この宣言をコメント解除 (と `CTRL + K、ctrl キーを押しながら C`)、宣言をコメント解除します (`CTRL + K、CTRL キーを押しながら U キーを押す`)、ホステッド サービスに接続します。

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

2. アプリで、**[Insert a TodoItem]** テキスト ボックスに意味のあるテキスト (たとえば、「*チュートリアルの完了*」) を入力し、**[Save]** をクリックします。

    ![](./media/app-service-mobile-windows-universal-test-app/mobile-quickstart-startup.png)

    これで、Azure でホストされている新しいモバイル アプリ バックエンドに POST 要求が送信されます。

3. デバッグを停止し、Windows Phone ストア アプリにユニバーサル Windows ソリューションにプロジェクトを既定のスタートアップを変更 (右クリックし、 `< アプリ名 >。WindowsPhone` プロジェクトを **スタートアップ プロジェクトとして設定**) してもう一度 f5 キーを押します。

    ![](./media/app-service-mobile-windows-universal-test-app/mobile-quickstart-completed-wp8.png)

    Windows アプリの開始後に、前の手順で保存したデータがモバイル アプリから読み込まれることに注目してください。






