<properties 
    pageTitle="Azure App Service を使用して Xamarin iOS アプリにプッシュ通知を追加する" 
    description="Azure App Service を使用して Xamarin iOS アプリにプッシュ通知を送信する方法について説明します" 
    services="app-service\mobile" 
    documentationCenter="xamarin" 
    authors="wesmc7777"
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-mobile" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-xamarin-ios" 
    ms.devlang="dotnet" 
    ms.topic="article"
    ms.date="12/01/2015" 
    ms.author="wesmc"/>

# Xamarin iOS アプリへのプッシュ通知の追加

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

##概要

このチュートリアルに基づいて、 [Xamarin.iOS のクイック スタート チュートリアル](app-service-mobile-xamarin-ios-get-started.md), 、最初に完了する必要があります。 Xamarin.iOS クイック スタート プロジェクトにプッシュ通知を追加して、レコードが挿入されるたびにプッシュ通知が送信されるようにします。 ダウンロードしたクイック スタートのサーバー プロジェクトを使用しない場合は、プッシュ通知拡張機能パッケージをプロジェクトに追加する必要があります。 サーバーの拡張機能パッケージの詳細については、次を参照してください。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)します。 

##前提条件

このチュートリアルを完了するには、以下が必要です。

* アクティブな Azure アカウント  
アカウントがない場合は、Azure 試用版にサインアップして、最大 10 件の無料モバイル アプリ バックエンドを入手してください。 このアプリは、評価終了後も使用できます。 参照してください [Azure 無料評価版](http://azure.microsoft.com/pricing/free-trial/)します。

* Mac を [Xamarin Studio] と [Xcode] v4.4 以降がインストールされています。 必要であれば Visual Studio を使用して Windows コンピューター上で Xamarin.iOS を実行できますが、Xamarin.iOS Build Host を実行するネットワーク接続された Mac にアクセスする必要があるため、少し複雑になります。 これを行うに知りたい場合は、次を参照してください。 [Installing Xamarin.iOS on Windows]します。

* 物理的な iOS デバイス iOS シミュレーターでは、プッシュ通知はサポートされていません。

*  [Apple Developer Program メンバーシップ](https://developer.apple.com/programs/ios/), 、Apple Push Notification サービス (APNS) で登録するために必要です。

* 完了、 [Xamarin.iOS のクイック スタート チュートリアル](app-service-mobile-xamarin-ios-get-started.md)します。


##Apple の開発者ポータルにプッシュ通知のアプリを登録する

[AZURE.INCLUDE [Notification Hubs Xamarin Enable Apple Push Notifications](../../includes/notification-hubs-xamarin-enable-apple-push-notifications.md)]

##プッシュ通知を送信するようにモバイル アプリを構成する

通知を送信するようにアプリを構成するには、新しいハイブリッドを作成し、使用するプラットフォームの通知サービスに合わせて構成します。  

1.  [Azure ポータル](https://portal.azure.com/), 、] をクリックして **参照** > **Mobile Apps** > モバイル アプリ > **設定** > **Mobile** > **プッシュ** > **通知ハブ** > **+ 通知ハブ**, 、新しい通知ハブの名前と名前空間を提供し、クリックして、 **[ok]** ] ボタンをクリックします。

    ![](./media/app-service-mobile-xamarin-ios-get-started-push/mobile-app-configure-notification-hub.png)

2. 通知ハブの作成] ブレードで [ **作成**します。

3. クリックして **プッシュ** > **(APNS) を Apple** > **証明書をアップロード**します。 以前にエクスポートした .p12 プッシュ証明書ファイルをアップロードします。  選択するように **サンド ボックス** 開発およびテスト用の開発プッシュ証明書を作成した場合。  それ以外の場合、選択 **運用**します。 これで、iOS のプッシュ通知と連携するようにサービスが構成されました。

    ![](./media/app-service-mobile-xamarin-ios-get-started-push/mobile-app-upload-apns-cert.png)

##プッシュ通知を送信するようにサーバー プロジェクトを更新する

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##サーバー プロジェクトを Azure にデプロイする

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service](../../includes/app-service-mobile-dotnet-backend-publish-service.md)]

##Xamarin.iOS プロジェクトを構成する

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

##アプリケーションにプッシュ通知を追加する

1.  **QSTodoService**, 、次のプロパティを追加できるように **AppDelegate** モバイル クライアントを取得することができます。
        
            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. 次の追加 `using` ステートメントの先頭に、 **AppDelegate.cs** ファイルです。

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

2.  **AppDelegate**, 、オーバーライド、 **FinishedLaunching** イベント。 

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert 
                | UIUserNotificationType.Badge 
                | UIUserNotificationType.Sound, 
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings); 
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. 同じファイルで上書き、 **RegisteredForRemoteNotifications** イベントです。 このコードでは、サーバーでサポートされているすべてのプラットフォームに送信される単純なテンプレート通知を登録します。 
 
    Notification Hubs のテンプレートの詳細については、次を参照してください。 [テンプレート](../notification-hubs/notification-hubs-templates.md)します。 


        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


4. 次に、オーバーライド、 **DidReceivedRemoteNotification** イベント。

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

これで、アプリケーションがプッシュ通知をサポートするように更新されました。

## <a name="test"></a>アプリケーションでプッシュ通知をテストする

1. キーを押して、 **実行** をクリックして、プロジェクトをビルドし、iOS 対応のデバイスでアプリケーションを開始 **OK** プッシュ通知を受け入れます。
    
    > [AZURE.NOTE] アプリからプッシュ通知を明示的に同意する必要があります。 これが必要であるのは、初めてアプリケーションを実行するときだけです。

2. アプリケーションで、タスクを入力し、プラス (**+**) のアイコン。

3. 確認通知を受信し、] をクリックして **OK** を通知を破棄します。

4. 手順 2. を繰り返してすぐにアプリケーションを閉じたら、通知が表示されることを確認します。

これで、このチュートリアルは終了です。

<!-- Images. -->

<!-- URLs. -->
[Xamarin Studio]: http://xamarin.com/platform
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[Installing Xamarin.iOS on Windows]: http://developer.xamarin.com/guides/ios/getting_started/installation/windows/


 

