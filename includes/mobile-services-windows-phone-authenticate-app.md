1. mainpage.xaml.cs プロジェクト ファイルを開き、次のコード スニペットを MainPage クラスに追加します。
    
        private MobileServiceUser user;
        private async Task Authenticate()
        {
            while (user == null)
            {
                string message;
                try
                {
                    user = await App.MobileServiceDotNetClient.LoginAsync(MobileServiceAuthenticationProvider.Twitter);
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (InvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }

                var dialog = new MessageDialog(message);
                await dialog.ShowAsync();
            }
        }

    これにより、現在のユーザーを格納するためのメンバー変数と認証プロセスを処理するためのメソッドが作成されます。 ユーザーは、Facebook ログインを使用して認証されます。

    >[AZURE.NOTE]Facebook 以外の id プロバイダーを使用している場合の値を変更 <strong>MobileServiceAuthenticationProvider</strong> 上、プロバイダーの値にします。</p>
    </div>

2. 削除またはコメント アウト既存 **OnNavigatedTo** メソッドをオーバーライドし、処理する次のメソッドに置き換えます、 **Loaded** ページのイベントです。 

        async void MainPage_Loaded(object sender, RoutedEventArgs e)
        {
            await Authenticate();
            RefreshTodoItems();
        }

    このメソッドは、新しい **認証** メソッドです。 

3. MainPage コンストラクターを次のコードに置き換えます。

        // Constructor
        public MainPage()
        {
            InitializeComponent();
            this.Loaded += MainPage_Loaded;
        }

    このコンストラクターでは、Loaded イベントのハンドラーも登録されます。
        
4. F5 キーを押してアプリケーションを実行し、選択した ID プロバイダーでアプリケーションにサインインします。 

    ログインに成功すると、アプリケーションはエラーなしで実行されます。また、モバイル サービスを照会してデータを更新できるようになります。

