このセクションでは、.NET コンソール アプリケーションからタグ付けされたテンプレート通知としてニュース速報を送信する方法について説明します。

モバイル サービスを使用している場合を参照してください、 [プッシュの](mobile-services-dotnet-backend-windows-store-dotnet-get-started-push.md) チュートリアルです。

Java を使用するかを参照してください PHP [JAVA/PHP から Notification Hubs の使用方法](../articles/notification-hubs/notification-hubs-java-backend-how-to.md)します。 使用するどのバックエンドから通知を送信することができます、 [Notification Hub REST インターフェイス](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)します。

実行したときに通知を送信するコンソール アプリケーションを作成した場合、ステップ 1. ~ 3. はスキップ [[入門] 通知ハブを使ってみる][get-started]します。

1. Visual Studio で、Visual C# の新しいコンソール アプリケーションを作成します。

    ![][13]

2. Visual Studio のメイン メニューで、**[ツール]**、**[ライブラリ パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順にクリックし、コンソール ウィンドウで次のコマンドを入力して、**Enter** キーを押します。

        Install-Package Microsoft.Azure.NotificationHubs

    使用して、Azure Notification Hubs SDK に参照が追加、 <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification ハブ NuGet パッケージ</a>します。

3. Program.cs ファイルを開き、次の追加 `を使用して` ステートメント。

        using Microsoft.Azure.NotificationHubs;

4. `プログラム` クラスで、次のメソッドを追加またはが既に存在する場合は置き換えます。

     private static async void SendTemplateNotificationAsync()
     {
         // Define the notification hub.
         NotificationHubClient hub = 
             NotificationHubClient.CreateClientFromConnectionString(
                 "<connection string with full access>", "<hub name>");
    
         // Create an array of breaking news categories.
         var categories = new string[] { "World", "Politics", "Business", 
                                         "Technology", "Science", "Sports"};
    
         // Sending the notification as a template notification. All template registrations that contain 
         // "messageParam" and the proper tags will receive the notifications. 
         // This includes APNS, GCM, WNS, and MPNS template registrations.
    
         Dictionary<string, string> templateParams = new Dictionary<string, string>();
    
         foreach (var category in categories)
         {
             templateParams["messageParam"] = "Breaking " + category + " News!";            
             await hub.SendTemplateNotificationAsync(templateParams, category);
         }
      }

 このコードでは、文字列の配列の 6 つのタグのそれぞれに対するテンプレート通知が送信されます。 タグを使用することで、デバイスは登録されているカテゴリに関する通知のみを確実に受信できます。

6. 上記のコードで置き換えます、 `< ハブの名前 >` と `< フル アクセスを持つ接続文字列 >` プレース ホルダーを通知ハブの名前との接続文字列 *DefaultFullSharedAccessSignature* 通知ハブのダッシュ ボードからです。

7. **Main** メソッド内に、次の行を追加します。

         SendTemplateNotificationAsync();
         Console.ReadLine();

8. コンソール アプリケーションをビルドします。





[from a console app]: #console 
[from mobile services]: #mobile-services 
[run the app and generate notifications]: #test-app 
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png 
[15]: ./media/notification-hubs-back-end/notification-hub-scheduler1.png 
[16]: ./media/notification-hubs-back-end/notification-hub-scheduler2.png 
[get-started]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started.md 
[use notification hubs to send notifications to users]: ../articles/tutorial-notify-users-mobileservices.md 
[get started with mobile services]: /develop/mobile/tutorials/get-started/#create-new-service 
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591 
[notification hubs guidance]: http://msdn.microsoft.com/library/jj927170.aspx 
[notification hubs how-to for windows store]: http://msdn.microsoft.com/library/jj927172.aspx 
[notification hubs rest interface]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx 

