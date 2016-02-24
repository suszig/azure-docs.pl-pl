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

##概要

このチュートリアルでは、Azure Notification Hubs を使用して iOS アプリケーションにプッシュ通知を送信する方法について説明します。
Apple Push Notification サービス (APNs) を使用してプッシュ通知を受信する空の Xamarin.iOS アプリケーションを作成します。 完了すると、通知ハブを使用して、アプリケーションを実行するすべてのデバイスにプッシュ通知をブロードキャストできるようになります。 完成したコードが含まれている、 [NotificationHubs アプリケーション][GitHub] サンプルです。

このチュートリアルでは、Notification Hubs を使用した簡単なブロードキャスト シナリオのデモンストレーションを行います。

##前提条件

このチュートリアルには、次のものが必要です。

+ [XCode 6.0][Install Xcode]
+ iOS 7.0 (またはこれ以降のバージョン) に対応したデバイス
+ iOS Developer Program メンバーシップ
+ [Xamarin.iOS]
+ [Azure モバイル サービス コンポーネント]

   > [AZURE.NOTE] プッシュ通知のための構成要件によりを展開して、シミュレーターで iOS 対応デバイス (iPhone または iPad) の代わりにプッシュ通知をテストします。

このチュートリアルを完了することは、Xamarin.iOS アプリの他のすべての Notification Hubs チュートリアルの前提条件です。

> [AZURE.IMPORTANT] このチュートリアルを完了するには、アクティブな Azure アカウントが必要です。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started)をご覧ください。

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]


##通知ハブを構成する

このセクションでは、新しい通知ハブを作成して APN を使用すると、認証を構成するでは説明、 **.p12** プッシュ証明書を作成します。 既に作成した通知ハブを使用する場合は、手順 5. に進んでください。

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li>
<p>クリックして、 <b>構成</b> 上部にあるタブをクリックして、 <b>アップロード</b> 証明書の拇印をアップロード、Apple 通知設定] ボタンをクリックします。 選択し、 <b>.p12</b> 前にエクスポートした証明書と証明書のパスワード</p>
<p>。選択するように <b>サンド ボックス</b> これは開発のためのモードです。 のみを使用して、 <b>運用</b> ストアからアプリケーションを購入したユーザーにプッシュ通知を送信する場合。</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-upload-cert.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png)


これで、通知ハブが APNS と連動するように構成されました。接続文字列にアプリケーションを登録し、通知を送信できます。





##通知ハブにアプリケーションを接続する

#### 新しいプロジェクトを作成する

1. Xamarin Studio で、新しい iOS プロジェクトを作成し、選択、 **統合 API** > **単一枠ビュー アプリケーション** テンプレートです。

    ![][31]

2. Azure Messaging コンポーネントへの参照を追加します。 Solution] ビューで右クリックし、 **コンポーネント** プロジェクトのフォルダーを選択して **Get More Components**します。 検索、 **Azure メッセージング** コンポーネントとコンポーネントをプロジェクトに追加します。

3.  **AppDelegate.cs**, 、次の追加ステートメントを使用します。

        using WindowsAzure.Messaging;

4. インスタンスを宣言 **SBNotificationHub**:

        private SBNotificationHub Hub { get; set; }

5. 作成、 **Constants.cs** クラスを次の変数を使用します。

        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";


6.  **AppDelegate.cs**, 、更新 **FinishedLaunching()** 次のようにします。

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

7. オーバーライド、 **オーバーライドします** メソッド **AppDelegate.cs**:

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

8. オーバーライド、 **オーバーライドします** メソッド **AppDelegate.cs**:

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }

9. 次の作成 **ProcessNotification()** メソッド **AppDelegate.cs**:

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

    > [AZURE.NOTE] オーバーライドすることもできます **FailedToRegisterForRemoteNotifications()** ネットワーク接続がないなどの状況に対応します。


10. デバイスでアプリケーションを実行します。


## 通知を送信する


通知を送信することによって、アプリで通知の受信をテストする、 [Azure Classic Portal] 通知ハブで、次の画面に示すように、[デバッグ] タブを使用しています。

![](./media/notification-hubs-ios-get-started/notification-hubs-debug-hub-ios.png)

プッシュ通知は通常、互換性のあるライブラリを使用して Mobile Services などのバックエンド サービスや ASP.NET に送信されます。 ライブラリがバックエンドで利用できない場合、REST API を直接使用して通知メッセージを送信できます。 

このチュートリアルでは、バックエンド サービスではなく、コンソール アプリケーションの通知ハブに .NET SDK を使用して通知を送信することで例を単純にして、クライアント アプリのテスト手順のみを説明します。 お勧め、 [通知ハブを使用したユーザーにプッシュ通知を](notification-hubs-aspnet-backend-ios-notify-users.md) 」チュートリアルでは、次に、ASP.NET バックエンドから通知を送信するためです。 ただし、通知の送信には、次の方法も使用できます。

* **REST インターフェイス**: バックエンド プラットフォームを使用して通知をサポートすることができます、 [REST インターフェイス](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)します。

* **Microsoft Azure Notification Hubs .NET SDK**: で、Nuget パッケージ マネージャーを実行している Visual Studio の [Install-package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)します。

* **Node.js** : [Node.js から Notification Hubs の使用方法](notification-hubs-nodejs-how-to-use-notification-hubs.md)します。

* **Azure Mobile Services**: Notification Hubs と統合されている Azure Mobile Services バックエンドから通知を送信する方法の例は、作業開始 Mobile Services でプッシュ通知の使用」を参照してください ([.NET バックエンド](../mobile-services/mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md) | [JavaScript バックエンド](../mobile-services/mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md))。

* **Java または PHP**: REST Api を使用して通知を送信する方法の例は、「JAVA/PHP から Notification Hubs を使用する方法」を参照してください ([Java](notification-hubs-java-backend-how-to.md) | [PHP](notification-hubs-php-backend-how-to.md))。


####(省略可能) .NET コンソール アプリケーションから通知を送信する

このセクションでは、.NET コンソール アプリケーションを使用して通知を送信します。

1. Visual C# の新しいコンソール アプリケーションを作成します。

    ![][213]

2. Visual Studio で、[ **ツール**, 、クリックして **NuGet パッケージ マネージャー**, 、順にクリック **パッケージ マネージャー コンソール**します。

    Visual Studio のパッケージ マネージャー コンソールが表示されます。

3. パッケージ マネージャー コンソール] ウィンドウで、設定、 **既定のプロジェクト** 、新しいコンソール アプリケーション プロジェクトにし、コンソール ウィンドウには、次のコマンドを実行します。

        Install-Package Microsoft.Azure.NotificationHubs

    使用して、Azure Notification Hubs SDK への参照を追加します <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification ハブ NuGet パッケージ</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Program.cs ファイルを開き、次の `using` ステートメントを追加します。

        using Microsoft.Azure.NotificationHubs;

3. `Program` クラスで、次のメソッドを追加します。

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

Apple で使用できるすべてのペイロードが見つかります [Local and Push Notification Programming Guide]します。


####(省略可能) モバイル サービスから通知を送信する

このセクションでは、モバイル サービスを使用してノード スクリプトから通知を送信します。

次のモバイル サービスを使用して通知を送信する [Get started with Mobile Services], 、します。

1. サインイン、 [Azure Classic Portal], 、目的のモバイル サービスを選択します。

2. 選択、 **スケジューラ** 上部にあるタブをクリックします。

    ![][215]

3. 新しいスケジュール済みジョブを作成、名前を挿入し、 **オンデマンド**します。

    ![][216]

4. ジョブが作成されたら、ジョブ名をクリックします。 をクリックし、 **スクリプト** 上部バー タブをクリックします。

5. スケジューラ関数内に次のスクリプトを挿入します。 通知ハブの名前と接続文字列にプレース ホルダーを置換することを確認 *DefaultFullSharedAccessSignature* 既に取得してあります。 クリックして **保存**します。

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


6. クリックして **一度だけ実行** 下部のバーにします。 デバイスでアラートを受信します。

##次のステップ

この簡単な例では、すべての iOS デバイスに通知をブロードキャストします。 特定のユーザーをターゲットとするには、このチュートリアルを参照してください [Use Notification Hubs to push notifications to users]します。 対象グループごとにユーザーをセグメント化するかどうか、確認できる [Use Notification Hubs to send breaking news]します。 通知ハブを使用する方法の詳細について [Notification Hubs Guidance] し、[、 [Notification Hubs How-To for iOS]します。


<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Use Notification Hubs to push notifications to users]: /manage/services/notification-hubs/notify-users-aspnet
[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet

[Local and Push Notification Programming Guide]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin.iOS]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS

