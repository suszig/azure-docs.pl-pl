次に、登録を試みる前にユーザーが認証されるように、プッシュ通知を登録する方法を変更する必要があります。

1. Visual Studio のソリューション エクスプローラーで、**Application_Launching** イベント ハンドラー内の app.xaml.cs プロジェクト ファイルを開き、**AcquirePushChannel** メソッドの呼び出しをコメントアウトまたは削除します。

2. 変更、 **AcquirePushChannel** からメソッド `プライベート` に `パブリック` を追加し、 `静的` 修飾子です。

3. MainPage.xaml.cs プロジェクト ファイルを開き、**OnNavigatedTo** メソッドのオーバーライドを次のコードに置き換えます。

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            await AuthenticateAsync();            
            App.AcquirePushChannel();
            RefreshTodoItems();
        }





