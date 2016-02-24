
1.  [Azure クラシック ポータル](https://manage.windowsazure.com/), 、] をクリックして、 **データ** ] タブでをクリックし、 **TodoItem** テーブルです。 
 
2.  **Todoitem**, をクリックして、 **スクリプト** ] タブで選択し、 **挿入**します。
   
    これで、挿入が発生したときに呼び出される関数が表示されます、 **TodoItem** テーブルです。

3. クリックして insert 関数を次のコードに置き換えます **保存**:

        function insert(item, user, request) {
        // Define a simple payload for a GCM notification.
        var payload = {
            "data": {
                "message": item.text
            }
        };      
        request.execute({
            success: function() {
                // If the insert succeeds, send a notification.
                push.gcm.send(null, payload, {
                    success: function(pushResponse) {
                        console.log("Sent push:", pushResponse, payload);
                        request.respond();
                        },              
                    error: function (pushResponse) {
                        console.log("Error Sending push:", pushResponse);
                        request.respond(500, { error: pushResponse });
                        }
                    });
                },
            error: function(err) {
                console.log("request.execute error", err)
                request.respond();
            }
          });
        }

    これを使用してを新しい挿入スクリプトを登録、 [gcm オブジェクト](http://go.microsoft.com/fwlink/p/?LinkId=282645) 、挿入が成功した後に、すべての登録済みデバイスにプッシュ通知を送信します。 
