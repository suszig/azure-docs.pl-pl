<properties
    pageTitle="Notification Hubs によるユーザーへのクロスプラットフォーム通知の送信 (ASP.NET)" description="Notification Hubs のテンプレートを使用して、すべてのプラットフォームをターゲットとするプラットフォームにとらわれない通知を 1 つの要求で送信する方法を説明します。"
    services="notification-hubs"
    documentationCenter=""
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="12/11/2015" 
    ms.author="wesmc"/>


# 通知ハブによるユーザーへのクロスプラットフォーム通知の送信

[通知ハブによるユーザーに通知する] 前のチュートリアルでは、特定の認証されたユーザーが登録されているすべてのデバイスに通知をプッシュする方法について説明しました。 そのチュートリアルでは、サポートされる各クライアント プラットフォームに通知を送信するため、複数の要求が必要でした。 通知ハブでは、テンプレートがサポートされています。テンプレートを使用すると、特定のデバイスが通知を受信する方法を指定できます。 これにより、クロスプラットフォーム通知の送信が簡単になります。 このトピックでは、テンプレートを活用して、すべてのプラットフォームをターゲットとするプラットフォームにとらわれない通知を 1 つの要求で送信する方法を示します。 テンプレートについて詳細を参照してください。 [Azure 通知ハブの概要 ][templates]します。
> [AZURE.NOTE] Notification Hubs を使用すると、デバイスでは同じタグを持つ複数のテンプレートを登録できます。 この場合、そのタグをターゲットとするメッセージが受信されると複数の通知がデバイスに配信されます (テンプレートごとに 1 つずつ)。 これにより、複数のビジュアル通知に同じメッセージを表示することが可能になります (Windows ストア アプリケーションでバッジおよびトースト通知の両方として表示するなど)。

次の手順を実行し、テンプレートを使用してクロスプラットフォーム通知を送信します。

1. Visual Studio のソリューション エクスプローラーで、**Controllers** フォルダーを展開し、RegisterController.cs ファイルを開きます。

2. 内のコード ブロックを見つけ、 **Post** 新しい登録置換を作成するメソッド、 `切り替える` 内容を次のコード。

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"msg\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }

    このコードは、プラットフォーム固有のメソッドを呼び出して、ネイティブ登録の代わりにテンプレート登録を作成します。 テンプレート登録がネイティブ登録から派生している場合、既存の登録を変更する必要はありません。

3. **Notifications** コントローラーで、**sendNotification** メソッドを次のコードに置き換えます。

     public async Task<HttpResponseMessage> Post()
     {
         var user = HttpContext.Current.User.Identity.Name;
         var userTag = "username:" + user;
    
         var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
         await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
    
         return Request.CreateResponse(HttpStatusCode.OK);
     }

 このコードは、ネイティブ ペイロードを指定しなくても、すべてのプラットフォームに通知を同時に送信します。 通知ハブは、登録されたテンプレートでの指定内容に従って、提供された_タグ_値を使用して適切なペイロードを作成して各デバイスに配信します。

4. WebApi バックエンド プロジェクトを再発行します。

5. もう一度クライアント アプリケーションを実行し、登録に成功したことを確認します。

6. (省略可能) クライアント アプリケーションを 2 つ目のデバイスにデプロイし、アプリケーションを実行します。

    各デバイスに通知が表示される点に注目してください。

## 次のステップ

このチュートリアルを完了したら、以下のトピックで通知ハブとテンプレートの詳細を参照してください。

+ **[通知ハブを使用したニュース速報の送信]** <br/>テンプレートを使用するもう 1 つのシナリオを示します。

+  * *[Azure 通知ハブの概要 ][templates]* *<br/>概要トピックの詳細テンプレートに関する情報を説明します。











[push to users asp.net]: /manage/services/notification-hubs/notify-users-aspnet 
[push to users mobile services]: /manage/services/notification-hubs/notify-users/ 
[visual studio 2012 express for windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546 
[use notification hubs to send breaking news]: notification-hubs-windows-store-dotnet-send-breaking-news.md 
[azure notification hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257 
[notify users with notification hubs]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md 
[templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339 
[notification hub how to for windows store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx 

