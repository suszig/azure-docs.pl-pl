
##<a name="update-app"></a>カスタム API を呼び出すようにアプリケーションを更新する

1. Visual Studio でクイック スタート プロジェクトの default.html ファイルを開き、 **] ボタンをクリック** という名前の要素 `buttonRefresh`, 、直後に次の新しい要素を追加します。 

        <button id="buttonCompleteAll" style="margin-left: 5px">Complete All</button>

    新しいボタンがページに追加されます。 

2. 内の default.js コード ファイルを開く、 `js` プロジェクト フォルダーを探し、 **refreshTodoItems** に機能し、この関数に次のコードが含まれているかどうかを確認します。

        todoTable.where({ complete: false })
           .read()
           .done(function (results) {
               todoItems = new WinJS.Binding.List(results);
               listItems.winControl.itemDataSource = todoItems.dataSource;
           });            

    ここでは、完了済みの項目が返されないように、項目をフィルターで処理しています。

3. 後に、 **refreshTodoItems** 関数の次のコードを追加します。

        var completeAllTodoItems = function () {
            var okCommand = new Windows.UI.Popups.UICommand("OK");
        
            // Asynchronously call the custom API using the POST method. 
            mobileService.invokeApi("completeall", {
                body: null,
                method: "post"
            }).done(function (results) {
                var message = results.result.count + " item(s) marked as complete.";
                var dialog = new Windows.UI.Popups.MessageDialog(message);
                dialog.commands.append(okCommand);
                dialog.showAsync().done(function () {
                    refreshTodoItems();
                });
            }, function (error) {
                var dialog = new Windows.UI.Popups
                    .MessageDialog(error.message);
                dialog.commands.append(okCommand);
                dialog.showAsync().done();
            });
        };

        buttonCompleteAll.addEventListener("click", function () {
            completeAllTodoItems();
        });

    このメソッドは、処理、 **クリックして** 新しいボタンのイベントです。  **InvokeApiAsync** メソッドを新しいカスタム API に POST 要求を送信すると、クライアントで呼び出されます。 カスタム API から返された結果は、メッセージ ダイアログに表示されます。エラーが発生した場合はそれらも表示されます。

## <a name="test-app"></a>アプリケーションをテストする

1. Visual Studio でキーを押して、 **f5 キーを押して** キーをプロジェクトをリビルドし、アプリケーションを開始します。

2. アプリケーションにテキストを入力 **Insert a TodoItem**, 、クリックして **保存**します。

3. 前の手順を繰り返して、複数の Todo 項目をリストに追加します。

4. クリックして、 **Complete All** ] ボタンをクリックします。

    ![](./media/mobile-services-windows-store-javascript-call-custom-api/mobile-custom-api-windows-store-completed.png)

    完了としてマークされた項目の数を示すメッセージ ダイアログが表示され、フィルター処理済みのクエリが再度実行されて、すべての項目がリストから消去されます。
