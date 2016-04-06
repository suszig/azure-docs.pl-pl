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


前のチュートリアル「 [Notify users with Notification Hubs], 特定の認証されたユーザーが登録されているすべてのデバイスにプッシュ通知する方法を学習しました。 そのチュートリアルでは、サポートされる各クライアント プラットフォームに通知を送信するため、複数の要求が必要でした。 通知ハブでは、テンプレートがサポートされています。テンプレートを使用すると、特定のデバイスが通知を受信する方法を指定できます。 これにより、クロスプラットフォーム通知の送信が簡単になります。 このトピックでは、テンプレートを活用して、すべてのプラットフォームをターゲットとするプラットフォームにとらわれない通知を 1 つの要求で送信する方法を示します。 テンプレートについて詳細を参照してください。 [Azure 通知ハブの概要][Templates]します。

> [AZURE.NOTE] 通知ハブでは、同じタグを持つ複数のテンプレートを登録するデバイスができます。 この場合、そのタグをターゲットとするメッセージが受信されると複数の通知がデバイスに配信されます (テンプレートごとに 1 つずつ)。 これにより、複数のビジュアル通知に同じメッセージを表示することが可能になります (Windows ストア アプリケーションでバッジおよびトースト通知の両方として表示するなど)。

次の手順を実行し、テンプレートを使用してクロスプラットフォーム通知を送信します。

1. Visual Studio で、ソリューション エクスプ ローラーで、 **コント ローラー** フォルダーし、RegisterController.cs ファイルを開きます。

2. 内のコード ブロックを見つけ、 **Post** 新しい登録置換を作成するメソッド、 `switch` 内容を次のコード。

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

3.  **通知** コント ローラーの交換、 **sendNotification** メソッドを次のコード。

        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;

            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

    このコードは、ネイティブ ペイロードを指定しなくても、すべてのプラットフォームに通知を同時に送信します。 通知ハブを作成して、指定したと各デバイスに適切なペイロードを配信 _タグ_ では、登録されたテンプレートで指定された値。

4. WebApi バックエンド プロジェクトを再発行します。

5. もう一度クライアント アプリケーションを実行し、登録に成功したことを確認します。

6. (省略可能) クライアント アプリケーションを 2 つ目のデバイスにデプロイし、アプリケーションを実行します。

    各デバイスに通知が表示される点に注目してください。

## 次のステップ

このチュートリアルを完了したら、以下のトピックで通知ハブとテンプレートの詳細を参照してください。

+ **[Use Notification Hubs to send breaking news]** <br/>テンプレートと使用した別のシナリオを例示します。

+  **[Notification Hubs の概要][Templates]**<br/>この概要トピックでは、テンプレートについて詳細に説明されています。


<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Use Notification Hubs to send breaking news]: notification-hubs-windows-store-dotnet-send-breaking-news.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx


