<properties
    pageTitle="Notification Hubs の使用 (Xamarin.iOS アプリケーション) | Microsoft Azure"
    description="このチュートリアルでは、Azure Notification Hubs を使用して Xamarin.iOS アプリケーションにプッシュ通知を送信する方法について学習します。"
    services="notification-hubs"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="11/17/2015"
    ms.author="yuaxu"/>


# Notification Hubs の使用

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## 概要

このチュートリアルでは、Azure Notification Hubs を使用して iOS アプリケーションにプッシュ通知を送信する方法について説明します。
Apple Push Notification サービス (APNs) を使用してプッシュ通知を受信する空の Xamarin.iOS アプリケーションを作成します。 完了すると、通知ハブを使用して、アプリケーションを実行するすべてのデバイスにプッシュ通知をブロードキャストできるようになります。 完成したコードが含まれている、 [NotificationHubs アプリケーション ][github] サンプルです。

このチュートリアルでは、Notification Hubs を使用した簡単なブロードキャスト シナリオのデモンストレーションを行います。

## 前提条件

このチュートリアルには、次のものが必要です。

+ [Xcode 6.0 ][install xcode]
+ iOS 7.0 (またはこれ以降のバージョン) に対応したデバイス
+ iOS Developer Program メンバーシップ
+ [Xamarin.iOS]
+ [Azure Mobile Services コンポーネント]
   > [AZURE.NOTE] プッシュ通知の構成要件により、プッシュ通知のデプロイとテストは、シミュレーターではなく iOS 対応デバイス (iPhone または iPad) で行う必要があります。

このチュートリアルを完了することは、Xamarin.iOS アプリの他のすべての Notification Hubs チュートリアルの前提条件です。
> [AZURE.IMPORTANT] このチュートリアルを完了するには、アクティブな Azure アカウントが必要です。 アカウントがない場合は、無料試用版アカウントを数分で作成することができます。 詳細については、「 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started)します。

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]


## 通知ハブを構成する

このセクションでは、作成した **.p12** プッシュ証明書を使用して、新しい通知ハブを作成し、APNS での認証を構成する方法について説明します。 既に作成した通知ハブを使用する場合は、手順 5. に進んでください。

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li>
<p>[ <b>構成</b> 上部にあるタブをクリックします <b>アップロード</b> 証明書の拇印をアップロード、Apple 通知設定] ボタンをクリックします。選択し、 <b>.p12</b> 前にエクスポートした証明書と証明書のパスワード。</p>
<p>選択してください。 <b>サンド ボックス</b> このモードでは、開発用です。他のサービスがデータベースまたはサーバーを使用していない場合は、 <b>運用環境</b> ストアからアプリケーションを購入したユーザーにプッシュ通知を送信する場合は。</p>
</li>
</ol>
(& a) です。 (& a) です。![](./media/notification-hubs-ios-get-started/notification-hubs-upload-cert.png)

(& a) です。 (& a) です。![](./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png)


これで、通知ハブが APNS と連動するように構成されました。接続文字列にアプリケーションを登録し、通知を送信できます。





## 通知ハブにアプリケーションを接続する

#### 新しいプロジェクトを作成する

1. Xamarin Studio で、新しい iOS プロジェクトを作成し、**[統合 API]**、**[単一ビュー アプリケーション]** テンプレートの順に選択します。

    ![][31]

2. Azure Messaging コンポーネントへの参照を追加します。 [ソリューション] ビューで、プロジェクトの **[コンポーネント]** フォルダーを右クリックし、**[他のコンポーネントを取得する]** を選択します。 **Azure Messaging** コンポーネントを検索し、そのコンポーネントをプロジェクトに追加します。

3. **AppDelegate.cs** で、次の using ステートメントを追加します。

        using WindowsAzure.Messaging;

4. **SBNotificationHub** のインスタンスを宣言します。

        private SBNotificationHub Hub { get; set; }

5. 次の変数を使用して **Constants.cs** クラスを作成します。

        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";

6. **AppDelegate.cs** で、**FinishedLaunching()** を更新して次の内容に合わせます。

     public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
     {
         if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
             var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                    UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                    new NSSet ());
    
             UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
             UIApplication.SharedApplication.RegisterForRemoteNotifications ();
         } else {
             UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
             UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
         }
    
         return true;
     }

7. **AppDelegate.cs** で **RegisteredForRemoteNotifications()** メソッドをオーバーライドします。

     public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
     {
         Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);
    
         Hub.UnregisterAllAsync (deviceToken, (error) => {
             if (error != null)
             {
                 Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                 return;
             }
    
             NSSet tags = null; // create tags if you want
             Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                 if (errorCallback != null)
                     Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
             });
         });
     }

8. **AppDelegate.cs** で **ReceivedRemoteNotification()** メソッドをオーバーライドします。

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }

9. **AppDelegate.cs** で次の **ProcessNotification()** メソッドを作成します。

     void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
     {
         // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
         if (null != options && options.ContainsKey(new NSString("aps")))
         {
             //Get the aps dictionary
             NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
    
             string alert = string.Empty;
    
             //Extract the alert text
             // NOTE: If you're using the simple alert by just specifying
             // "  aps:{alert:"alert msg here"}  ", this will work fine.
             // But if you're using a complex alert with Localization keys, etc.,
             // your "alert" object from the aps dictionary will be another NSDictionary.
             // Basically the JSON gets dumped right into a NSDictionary,
             // so keep that in mind.
             if (aps.ContainsKey(new NSString("alert")))
                 alert = (aps [new NSString("alert")] as NSString).ToString();
    
             //If this came from the ReceivedRemoteNotification while the app was running,
             // we of course need to manually process things like the sound, badge, and alert.
             if (!fromFinishedLaunching)
             {
                 //Manually show an alert
                 if (!string.IsNullOrEmpty(alert))
                 {
                     UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                     avAlert.Show();
                 }
             }
         }
     }

 > [AZURE.NOTE] ネットワーク接続がないなどの状況に対処するために、**FailedToRegisterForRemoteNotifications()** をオーバーライドすることを選択できます。

10. デバイスでアプリケーションを実行します。


## 通知を送信する

次の画面に示すように、[Azure クラシック ポータル] で、通知ハブを [デバッグ] タブを使用して、通知を送信することによって、アプリケーションで通知を受信をテストすることができます。

![](./media/notification-hubs-ios-get-started/notification-hubs-debug-hub-ios.png)

プッシュ通知は通常、互換性のあるライブラリを使用して Mobile Services などのバックエンド サービスや ASP.NET に送信されます。 ライブラリがバックエンドで利用できない場合、REST API を直接使用して通知メッセージを送信できます。

このチュートリアルでは、バックエンド サービスではなく、コンソール アプリケーションの通知ハブに .NET SDK を使用して通知を送信することで例を単純にして、クライアント アプリのテスト手順のみを説明します。 お勧め、 [通知ハブを使用したユーザーにプッシュ通知を](notification-hubs-aspnet-backend-ios-notify-users.md) 」チュートリアルでは、次に、ASP.NET バックエンドから通知を送信するためです。 ただし、通知の送信には、次の方法も使用できます。

* **REST インターフェイス**: バックエンド プラットフォームを使用して通知をサポートすることができます、 [REST インターフェイス](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)します。

* **Microsoft Azure Notification Hubs .NET SDK**: で、Nuget パッケージ マネージャーを実行している Visual Studio の [Install-package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)します。

* **Node.js** : [Node.js から Notification Hubs の使用方法](notification-hubs-nodejs-how-to-use-notification-hubs.md)します。

* **Azure Mobile Services**: Notification Hubs と統合されている Azure Mobile Services バックエンドから通知を送信する方法の例は、作業開始 Mobile Services でプッシュ通知の使用」を参照してください ([.NET バックエンド](../mobile-services/mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md) | [JavaScript backend](../mobile-services/mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md)).

* **Java または PHP**: REST Api を使用して通知を送信する方法の例は、「JAVA/PHP から Notification Hubs を使用する方法」を参照してください ([Java](notification-hubs-java-backend-how-to.md) | [PHP](notification-hubs-php-backend-how-to.md)).


#### (省略可能) .NET コンソール アプリケーションから通知を送信する

このセクションでは、.NET コンソール アプリケーションを使用して通知を送信します。

1. Visual C# の新しいコンソール アプリケーションを作成します。

    ![][213]

2. Visual Studio で、**[ツール]**、**[Nuget パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順にクリックします。

    Visual Studio のパッケージ マネージャー コンソールが表示されます。

3. パッケージ マネージャー コンソール ウィンドウで **[既定のプロジェクト]** に新しいコンソール アプリケーション プロジェクトを設定した後、そのコンソール ウィンドウから次のコマンドを実行します。

        Install-Package Microsoft.Azure.NotificationHubs

    使用して、Azure Notification Hubs SDK に参照が追加、 <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification ハブ NuGet パッケージ</a>します。

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Program.cs ファイルを開き、次の追加 `を使用して` ステートメント。

        using Microsoft.Azure.NotificationHubs;

3. `プログラム` クラスを次のメソッドを追加します。

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }

4. `Main` メソッド内に次の行を追加します。

         SendNotificationAsync();
         Console.ReadLine();

5. F5 キーを押してアプリケーションを実行します。 デバイスでアラートを受信します。 Wi-Fi を使用する場合は、接続が機能していることを確認します。

[ローカル and Push Notification Programming Guide] Apple で使用できるすべてのペイロードが見つかります。


#### (省略可能) モバイル サービスから通知を送信する

このセクションでは、モバイル サービスを使用してノード スクリプトから通知を送信します。

モバイル サービスを使用して通知を送信するには、次の [モバイル サービス] を使用し。

1. [Azure クラシック ポータル、] にサインインし、モバイル サービスを選択します。

2. 上部にある **[Scheduler]** タブを選択します。

    ![][215]

3. スケジュールされた新しいジョブを作成して名前を挿入し、**[要求時]** をクリックします。

    ![][216]

4. ジョブが作成されたら、ジョブ名をクリックします。 上部のバーにある **[スクリプト]** タブをクリックします。

5. スケジューラ関数内に次のスクリプトを挿入します。 必ず、プレースホルダーを、通知ハブの名前と既に取得してある *DefaultFullSharedAccessSignature* の接続文字列に置き換えてください。 **[保存]** をクリックします。

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );

6. 下部のバーにある **[一度だけ実行する]** をクリックします。 デバイスでアラートを受信します。

## 次のステップ

この簡単な例では、すべての iOS デバイスに通知をブロードキャストします。 特定のユーザーをターゲットとするには、[ユーザーにプッシュ通知を Notification Hubs を使用する] このチュートリアルを参照してください。 対象グループごとにユーザーを区分するには、[通知ハブを使用したニュース速報の送信] を読み取ることができます。 [通知ハブの概要] で、通知ハブを使用する方法について説明し、[通知ハブに関する「方法」(ios向け)] にします。






[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png 
[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png 
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png 
[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png 
[mobile services ios sdk]: http://go.microsoft.com/fwLink/?LinkID=266533 
[submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582 
[my applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039 
[live sdk for windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253 
[get started with mobile services]: /develop/mobile/tutorials/get-started-xamarin-ios 
[azure classic portal]: https://manage.windowsazure.com/ 
[notification hubs guidance]: http://msdn.microsoft.com/library/jj927170.aspx 
[notification hubs how-to for ios]: http://msdn.microsoft.com/library/jj927168.aspx 
[install xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532 
[ios provisioning portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456 
[use notification hubs to push notifications to users]: /manage/services/notification-hubs/notify-users-aspnet 
[use notification hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet 
[local and push notification programming guide]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1 
[apple push notification service]: http://go.microsoft.com/fwlink/p/?LinkId=272584 
[azure mobile services component]: http://components.xamarin.com/view/azure-mobile-services/ 
[github]: http://go.microsoft.com/fwlink/p/?LinkId=331329 
[xamarin.ios]: http://xamarin.com/download 
[windowsazure.messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS 

