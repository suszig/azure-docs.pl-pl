
1. MainPage.cs 共有プロジェクト ファイルを開き、次のコード スニペットを MainPage クラスに追加します。
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    このコードでは、Facebook ログインを使用してユーザーを認証します。 Facebook 以外の id プロバイダーを使用している場合の値を変更 **MobileServiceAuthenticationProvider** 上、プロバイダーの値にします。

3. コメント アウトまたは削除する呼び出し、 **RefreshTodoItems** 既存のメソッド **OnNavigatedTo** メソッドのオーバーライドします。

    これを行うと、ユーザーが認証されるまでデータが読み込まれなくなります。 次に、追加する、 **サインイン** 認証をトリガーするアプリにボタンをクリックします。

4. MainPage クラスに、次のコード スニペットを追加します。

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Hide the login button and load items from the mobile app.
                ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Windows ストア アプリ プロジェクトで、MainPage.xaml プロジェクト ファイルを開き、次の追加 **ボタン** 要素を定義する要素の直前に、 **保存** ] ボタンをクリックします。

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

6. Windows Phone ストア アプリ プロジェクトで、次のコードを追加 **ボタン** 内の要素、 **ContentPanel**, 後に、 **TextBox** 要素。

        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
            Margin="10, 0, 0, 0" Visibility="Visible">Sign in</Button>

8. 共有の App.xaml.cs プロジェクト ファイルを開き、次のコードを追加します。

        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif

            base.OnActivated(args);
        }

    場合、 **OnActivated** メソッドが既に存在し、追加するだけで、 `#if...#endif` コード ブロックです。

9. Windows ストア アプリを実行] をクリックするには、F5 キーを押して、 **サインイン** ] ボタン、および選択した id プロバイダーを使用してアプリケーションにサインオンします。 

    ログインに成功すると、アプリケーションはエラーなしで実行されます。また、バックエンドに照会したり、データを更新したりできるようになります。

10. Windows Phone ストア アプリ プロジェクトを右クリックし、をクリックして **スタートアップ プロジェクトとして設定**, 、Windows Phone ストア アプリも正しく実行されることを確認する前の手順を繰り返します。  

 
