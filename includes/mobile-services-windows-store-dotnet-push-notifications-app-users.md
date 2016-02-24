
次に、登録が試行される前にユーザーが認証されていることを確認するために、プッシュ通知が登録される方法を変更する必要があります。 クライアント アプリケーションの更新は、プッシュ通知を実装する方法によって異なります。

###Visual Studio 2013 Update 2 以降のバージョンでプッシュ通知の追加ウィザードを使用する

このメソッドでは、ウィザードによってプロジェクトに新しい push.register.cs ファイルが作成されています。

1. ソリューション エクスプ ローラーで Visual Studio で app.xaml.cs プロジェクト ファイルを開くし、[、 **OnLaunched** イベント ハンドラーのコメント アウトまたはへの呼び出しを削除、 **UploadChannel** メソッドです。 

2. Push.register.cs プロジェクト ファイルを開き、置換、 **UploadChannel** メソッドを次のコード。

        public async static void UploadChannel()
        {
            var channel = 
                await Windows.Networking.PushNotifications.PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();
        
            try
            {
                // Create a native push notification registration.
                await App.MobileService.GetPush().RegisterNativeAsync(channel.Uri);             
        
            }
            catch (Exception exception)
            {
                HandleRegisterException(exception);
            }
        }

    これにより、認証されたユーザー資格情報を有する同じクライアント インスタンスによって登録が確実に行われます。 そうしなければ、登録は失敗し、認証エラー (401) を返します。

3. 共有 MainPage.cs プロジェクト ファイルを開くし、置換、 **ButtonLogin_Click** を次のハンドラー。

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile service.
            await AuthenticateAsync();
            todolistPush.UploadChannel();

            // Hide the login button and load items from the mobile service.
            this.ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
            await RefreshTodoItems();
        }

    これにより、プッシュ登録が試行される前に、認証が行われるようになります。

4.  前のコードでは、生成されたプッシュ クラス名 (`todolistPush`) をウィザードによって生成されるクラス名 (形式は通常、<code><em>mobile_service</em>Push</code>) に置き換える必要があります。 

###手動で有効にされたプッシュ通知      

このメソッドでは、チュートリアルから、直接 app.xaml.cs プロジェクト ファイルに登録コードを追加しました。

1. ソリューション エクスプ ローラーで Visual Studio で app.xaml.cs プロジェクト ファイルを開くと、 **OnLaunched** イベント ハンドラー コメント アウトまたは削除する呼び出し **InitNotificationsAsync**します。 
 
2. 変更、 **InitNotificationsAsync** メソッドから `private` に `public` を追加し、 `static` 修飾子です。 

3. 共有 MainPage.cs プロジェクト ファイルを開くし、置換、 **ButtonLogin_Click** を次のハンドラー。

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile service.
            await AuthenticateAsync();
            App.InitNotificationsAsync();

            // Hide the login button and load items from the mobile service.
            this.ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
            await RefreshTodoItems();
        }
    
    これにより、プッシュ登録が試行される前に、認証が行われるようになります。
