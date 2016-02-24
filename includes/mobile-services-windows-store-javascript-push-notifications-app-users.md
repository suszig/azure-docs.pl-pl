
次に、登録が試行される前にユーザーが認証されていることを確認するために、プッシュ通知が登録される方法を変更する必要があります。 クライアント アプリケーションの更新は、プッシュ通知を実装する方法によって異なります。

###Visual Studio 2013 Update 2 以降のバージョンでプッシュ通知ウィザードを使用する

このメソッドでは、ウィザードによってプロジェクトに新しい push.register.js ファイルと service.js ファイルが作成されています。

1. Visual Studio でソリューション エクスプ ローラーで、push.register.js プロジェクト ファイルをコメント アウトを開くか、呼び出しを削除 **addEventListener**します。 

2. Default.js プロジェクト ファイルで置き換える既存 **ログイン** 関数を次のコード。
 
        // Request authentication from Mobile Services using a Facebook login.
        var login = function () {
            return new WinJS.Promise(function (complete) {
                client.login("facebook").done(function (results) {
                    userId = results.userId;
                    // Request a push notification channel.
                    Windows.Networking.PushNotifications
                        .PushNotificationChannelManager
                        .createPushNotificationChannelForApplicationAsync()
                        .then(function (channel) {
                            // Register for notifications using the new channel
                            client.push.registerNative(channel.uri);
                        });
                    refreshTodoItems();
                    var message = "You are now logged in as: " + userId;
                    var dialog = new Windows.UI.Popups.MessageDialog(message);
                    dialog.showAsync().done(complete);
                }, function (error) {
                    userId = null;
                    var dialog = new Windows.UI.Popups
                        .MessageDialog("An error occurred during login", "Login Required");
                    dialog.showAsync().done(complete);
                });
            });
        }  

###手動で有効にされたプッシュ通知      

このメソッドでは、チュートリアルから、直接 default.js プロジェクト ファイルに登録コードを追加しました。

1. Visual Studio でソリューション エクスプ ローラーで、default.js プロジェクト ファイルを開くと、 **onActivated** イベント ハンドラーを呼び出すコード行を見つけます、 **createPushNotificationChannelForApplicationAsync** 関数を次のようになります。

        // Request a push notification channel.
        Windows.Networking.PushNotifications
            .PushNotificationChannelManager
            .createPushNotificationChannelForApplicationAsync()
            .then(function (channel) {
                // Register for notifications using the new channel
                client.push.registerNative(channel.uri);
            }); 
 
2. 次のコードに移動、 **ログイン** 関数の呼び出しの前に、 **refreshTodoItems** ように、 **ログイン** 関数は、次のようになります。
 
        // Request authentication from Mobile Services using a Facebook login.
        var login = function () {
            return new WinJS.Promise(function (complete) {
                client.login("facebook").done(function (results) {
                    userId = results.userId;
                    // Request a push notification channel.
                    Windows.Networking.PushNotifications
                        .PushNotificationChannelManager
                        .createPushNotificationChannelForApplicationAsync()
                        .then(function (channel) {
                            // Register for notifications using the new channel
                            client.push.registerNative(channel.uri);
                        });
                    refreshTodoItems();
                    var message = "You are now logged in as: " + userId;
                    var dialog = new Windows.UI.Popups.MessageDialog(message);
                    dialog.showAsync().done(complete);
                }, function (error) {
                    userId = null;
                    var dialog = new Windows.UI.Popups
                        .MessageDialog("An error occurred during login", "Login Required");
                    dialog.showAsync().done(complete);
                });
            });
        }  

