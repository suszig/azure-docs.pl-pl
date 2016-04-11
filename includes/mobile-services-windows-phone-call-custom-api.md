##<a name="update-app"></a>カスタム API を呼び出すようにアプリケーションを更新する

1. Visual Studio でクイック スタート プロジェクトの MainPage.xaml ファイルを開き、 **ボタン** という名前の要素 `ButtonRefresh`, 、次の XAML コードに置き換えます。 

        <StackPanel Grid.Row="3" Grid.ColumnSpan="2" Orientation="Horizontal">
            <Button Width="225" Name="ButtonRefresh" 
                Click="ButtonRefresh_Click">Refresh</Button>
            <Button Width="225"  Name="ButtonCompleteAll" 
                Click="ButtonCompleteAll_Click">Complete All</Button>
        </StackPanel>

    新しいボタンがページに追加されます。 

2. MainPage.xaml.cs コード ファイルを開き、次のクラス定義コードを追加します。

        public class MarkAllResult
        {
            public int Count { get; set; }
        }

    このクラスは、カスタム API から返される行数の値を保持する目的で使用します。 

3. 検索、 **RefreshTodoItems** メソッドで、 **MainPage** クラス、およびことを確認して、 `query` 次によって定義されている **、** メソッド。

        .Where(todoItem => todoItem.Complete == false)

    ここでは、完了済みの項目が返されないように、項目をフィルターで処理しています。

3.  **MainPage** クラスを次のメソッドを追加します。

        private async void ButtonCompleteAll_Click(object sender, RoutedEventArgs e)
        {
            string message;
            try
            {
                // Asynchronously call the custom API using the POST method. 
                var result = await App.MobileService
                    .InvokeApiAsync<MarkAllResult>("completeAll", 
                    System.Net.Http.HttpMethod.Post, null);
                message =  result.Count + " item(s) marked as complete.";
                RefreshTodoItems();
            }
            catch (MobileServiceInvalidOperationException ex)
            {
                message = ex.Message;                
            }
        
            MessageBox.Show(message);  
        }

    このメソッドは、処理、 **クリックして** 新しいボタンのイベントです。  **InvokeApiAsync** メソッドを新しいカスタム API に要求を送信すると、クライアントで呼び出されます。 カスタム API から返された結果は、メッセージ ダイアログに表示されます。

## <a name="test-app"></a>アプリケーションをテストする

1. Visual Studio でキーを押して、 **f5 キーを押して** キーをプロジェクトをリビルドし、アプリケーションを開始します。

2. アプリケーションにテキストを入力 **Insert a TodoItem**, の順にタップ **保存**します。

3. 前の手順を繰り返して、複数の Todo 項目をリストに追加します。

4. タップして、 **Complete All** ] ボタンをクリックします。

    ![](./media/mobile-services-windows-phone-call-custom-api/mobile-custom-api-windows-phone-completed.png)

    完了としてマークされた項目の数を示すメッセージ ボックスが表示され、フィルター処理済みのクエリが再度実行されて、すべての項目がリストから消去されます。
