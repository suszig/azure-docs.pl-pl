

1. ログイン、 [Azure クラシック ポータル](https://manage.windowsazure.com/), をクリックして **Mobile Services**, 、モバイル サービスを選択します。

2. クリックして、 **API** タブをクリックし、をクリックし、 **作成**します。 これを表示、 **新しいカスタム API を作成する** ダイアログ。

3. 型 _completeall_ で **API 名**, 、新しい API を作成、チェック ボタンをクリックします。

    > [AZURE.TIP] 既定のアクセス許可では、カスタム API を呼び出すアプリケーション キーを持つすべてのユーザー可能性があります。 ただし、アプリケーション キーは安全に配布または格納されていない場合があるので、安全な資格情報とは見なされません。 セキュリティ強化のための認証されたユーザーにのみアクセスを制限することを検討します。

4. をクリックして **completeall** API テーブルでします。

5. クリックして、 **スクリプト** ] タブを既存のコードを次のコードに置き換えますしてクリックして **保存**します。    このコードを使用して、 [mssql object] にアクセスする、 **todoitem** テーブルに直接設定、 `complete` すべての項目のフラグ。  **Exports.post** 関数を使用して、クライアントが操作を実行する POST 要求を送信します。 変更された行数は整数値としてクライアントに返されます。


        exports.post = function(request, response) {
            var mssql = request.service.mssql;
            var sql = "UPDATE todoitem SET complete = 1 " +
                "WHERE complete = 0; SELECT @@ROWCOUNT as count";
            mssql.query(sql, {
                success: function(results) {
                    if(results.length == 1)
                        response.send(200, results[0]);
                }
            })
        };


> [AZURE.NOTE]  [要求](http://msdn.microsoft.com/library/windowsazure/jj554218.aspx) と [応答](http://msdn.microsoft.com/library/windowsazure/dn303373.aspx) でカスタム API の関数に指定されているオブジェクトが実装されている、 [Express.js ライブラリ](http://go.microsoft.com/fwlink/p/?LinkId=309046)します。 

<!-- Anchors. -->

<!-- Images. -->

<!-- URLs. -->
[mssql object]: http://msdn.microsoft.com/library/windowsazure/jj554212.aspx


