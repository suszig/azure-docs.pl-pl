
このセクションでは、.NET コンソール アプリケーションおよびその他のアプリケーションから通知を送信する方法を示します。
モバイル サービスを使用している場合を参照してください、 [プッシュの](mobile-services-dotnet-backend-windows-store-dotnet-get-started-push.md) チュートリアルです。 Java を使用するかを参照してください PHP [JAVA/PHP から Notification Hubs の使用方法](../articles/notification-hubs/notification-hubs-java-backend-how-to.md)します。 使用するどのバックエンドから通知を送信することができます、 [Notification Hub REST インターフェイス](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)します。

次のコードでは、Windows ストア デバイス、Windows Phone デバイス、iOS デバイス、Android デバイスに通知を送信します。 

完了したときに、コンソール アプリケーションを作成する場合、ステップ 1. ~ 3. はスキップ [を使ってみる Notification Hubs] [入門] です。

1. Visual Studio で、Visual C# の新しいコンソール アプリケーションを作成します。 

    ![][13]

2. Visual Studio のメイン メニューでクリックして **ツール**, 、**ライブラリ パッケージ マネージャー**, 、および **パッケージ マネージャー コンソール**, 、次に、コンソール ウィンドウで、次の入力押し **Enter**:

        Install-Package Microsoft.Azure.NotificationHubs
    
    使用して、Azure Notification Hubs SDK への参照を追加します <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification ハブ NuGet パッケージ</a>. 

3. Program.cs ファイルを開き、次の `using` ステートメントを追加します。

        using Microsoft.Azure.NotificationHubs;

4. `Program` クラス内で、次のメソッドを追加するか、既にメソッドが指定されている場合は置き換えます。

        private static async void SendNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub = 
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");
        
            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business", 
                "Technology", "Science", "Sports"};
        
            foreach (var category in categories)
            {
                try
                {
                    // Define a Windows Store toast.
                    var wnsToast = "<toast><visual><binding template=\"ToastText01\">" 
                        + "<text id=\"1\">Breaking " + category + " News!" 
                        + "</text></binding></visual></toast>";         
                    await hub.SendWindowsNativeNotificationAsync(wnsToast, category);

                    // Define a Windows Phone toast.
                    var mpnsToast =
                        "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                        "<wp:Notification xmlns:wp=\"WPNotification\">" +
                            "<wp:Toast>" +
                                "<wp:Text1>Breaking " + category + " News!</wp:Text1>" +
                            "</wp:Toast> " +
                        "</wp:Notification>";         
                    await hub.SendMpnsNativeNotificationAsync(mpnsToast, category);

                    // Define an iOS alert.
                    var alert = "{\"aps\":{\"alert\":\"Breaking " + category + " News!\"}}";
                    await hub.SendAppleNativeNotificationAsync(alert, category);

                    // Define an Android notification.
                    var notification = "{\"data\":{\"msg\":\"Breaking " + category + " News!\"}}";
                    await hub.SendGcmNativeNotificationAsync(notification, category);
                }
                catch (ArgumentException)
                {
                    // An exception is raised when the notification hub hasn't been 
                    // registered for the iOS, Windows Store, or Windows Phone platform. 
                }
            }
         }

    このコードにより、文字列配列内の 6 つのタグそれぞれに対応した通知が、Windows ストア デバイス、Windows Phone デバイス、iOS デバイスに送信されます。 タグを使用することで、デバイスは登録されているカテゴリに関する通知のみを確実に受信できます。
    
    > [AZURE.NOTE] このバックエンド コードは、Windows ストア、Windows Phone、iOS、および Android クライアントをサポートします。 send メソッドは、通知ハブが特定のクライアント プラットフォームに対してまだ構成されていない場合、エラー応答を返します。 

6. 上記のコードで置き換えます、 `<hub name>` と `<connection string with full access>` プレース ホルダーを通知ハブの名前との接続文字列 *DefaultFullSharedAccessSignature* 既に取得してあります。

7. 次の行を追加、 **Main** メソッド。

         SendNotificationAsync();
         Console.ReadLine();

<!-- Anchors -->
[From a console app]: #console
[From Mobile Services]: #mobile-services
[Run the app and generate notifications]: #test-app

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

[15]: ./media/notification-hubs-back-end/notification-hub-scheduler1.png
[16]: ./media/notification-hubs-back-end/notification-hub-scheduler2.png

<!-- URLs. -->
[get-started]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started.md
[Use Notification Hubs to send notifications to users]: ../articles/tutorial-notify-users-mobileservices.md
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Notification Hubs REST interface]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx

