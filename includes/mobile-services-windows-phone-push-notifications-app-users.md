
次に、登録を試みる前にユーザーが認証されるように、プッシュ通知を登録する方法を変更する必要があります。 

1. ソリューション エクスプ ローラーで Visual Studio で app.xaml.cs プロジェクト ファイルを開くし、[、 **Application_Launching** イベント ハンドラーのコメント アウトまたはへの呼び出しを削除、 **AcquirePushChannel** メソッドです。 
 
2. 変更、 **AcquirePushChannel** メソッドから `private` に `public` を追加し、 `static` 修飾子です。 

3. MainPage.xaml.cs プロジェクト ファイルを開き、置換、 **OnNavigatedTo** に次のメソッドをオーバーライドします。

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            await AuthenticateAsync();            
            App.AcquirePushChannel();
            RefreshTodoItems();
        }
