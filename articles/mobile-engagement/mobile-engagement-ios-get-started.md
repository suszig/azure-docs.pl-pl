<properties
    pageTitle="Objective C で IOS の Azure Mobile Engagement を開始する"
    description="iOS アプリの分析やプッシュ通知で Azure Mobile Engagement を使用する方法を説明します。"
    services="mobile-engagement"
    documentationCenter="Mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="09/22/2015"
    ms.author="piyushjo" />

# Objective C で IOS アプリ の Azure Mobile Engagement を開始する

> [AZURE.SELECTOR]
- [ユニバーサル Windows](mobile-engagement-windows-store-dotnet-get-started.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-get-started.md)
- [iOS |Obj C](mobile-engagement-ios-get-started.md)
- [iOS |Swift](mobile-engagement-ios-swift-get-started.md)
- [Android](mobile-engagement-android-get-started.md)
- [Cordova](mobile-engagement-cordova-get-started.md)

このトピックでは、Azure Mobile Engagement を使用してアプリの使用状況を把握し、iOS アプリケーションのセグメント化されたユーザーにプッシュ通知を送信する方法について説明します。
このチュートリアルでは、Apple プッシュ通知システム (APNS) を使用して基本データを収集し、プッシュ通知を受信する空の iOS アプリケーションを作成します。

このチュートリアルには、次のものが必要です。

+ XCode 6 または XCode 7。Mac アプリ ストアからインストールできます。
+ [モバイル エンゲージメント iOS SDK]

このチュートリアルを完了することは、iOS アプリケーションの他のすべての Mobile Engagement チュートリアルの前提条件です。

> [AZURE.IMPORTANT] IOS アプリの他のすべてのモバイル エンゲージメント チュートリアルの前提条件このチュートリアルを完了することは、これを完了するには、アクティブな Azure アカウントが必要です。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、<a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure の無料試用版サイト</a>をご覧ください。

##<a id="setup-azme"></a>IOS アプリ用のモバイル エンゲージメントをセットアップします。

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>アプリを Mobile Engagement のバックエンドに接続する

このチュートリアルでは、データを収集してプッシュ通知を送信するために必要な最小限のセットである「基本的な統合」について説明します。 完全な統合のドキュメントについては記載されて、 [モバイル エンゲージメント iOS SDK の統合](../mobile-engagement-ios-sdk-overview/)

統合のデモンストレーションを行うために、XCode で基本的なアプリを作成します。

###新しい iOS プロジェクトを作成する

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###アプリを Mobile Engagement のバックエンドに接続する

1. [モバイル エンゲージメント iOS SDK] をダウンロードします。
2. .tar.gz ファイルをコンピューター上のフォルダーに展開します。
3. プロジェクトを右クリックし、[ **ファイルを追加する**です。

    ![][1]

4. SDK では、選択を抽出したフォルダーに移動、 `EngagementSDK` フォルダー、およびキーを押します **OK**します。

    ![][2]

5. 開いている、 **Build Phases** ] タブの [し、[、 **Link Binary With Libraries** ] メニューの [次のように、フレームワークを追加します。

    ![][3]

6.  **XCode 7** -追加 `libxml2.tbd` の代わりに `libxml2.dylib`します。

7. アプリの Azure ポータルに戻り **接続情報]** ページし、接続文字列をコピーします。

    ![][4]

8. 次のコードの行を追加、 **AppDelegate.m** ファイルです。

        #import "EngagementAgent.h"

9. ここで、接続文字列を `didFinishLaunchingWithOptions` デリゲートに貼り付けます。

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]
            //[EngagementAgent setTestLogEnabled:YES];
   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

10. `setTestLogEnabled` は、問題を識別するための SDK ログを有効にするステートメントです (省略可能)。 

##<a id="monitor"></a>リアルタイム監視を有効にします。

データを送信してユーザーがアクティブであることを確認するには、少なくとも 1 つの画面 (アクティビティ) を Mobile Engagement のバックエンドに送信する必要があります。

1. 開いている、 **ViewController.h** ファイルし、インポート **EngagementViewController.h**:

    `# import "EngagementViewController.h"`

2. ここでのスーパークラスを置き換えます、 **ViewController** によってインターフェイス `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

##<a id="monitor"></a>リアルタイム監視を使用してアプリを接続する

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>プッシュ通知とアプリ内メッセージングを有効にします。

Mobile Engagement により、ユーザーと通信を行い、キャンペーンに関するプッシュ通知とアプリ内メッセージングを届けることができます。 このモジュールは、Mobile Engagement ポータルで REACH として呼び出されます。
次のセクションでは、それらを受信するようにアプリをセットアップします。

### アプリがサイレント プッシュ通知を受信できるようにする

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### リーチ ライブラリをプロジェクトに追加する

1. プロジェクトを右クリックします。
2. 選択 **[Add file to**します。
3. SDK を抽出したフォルダーに移動します。
4. `EngagementReach` フォルダーを選択します
5. クリックして **追加**します。

### アプリケーション デリゲートを変更する

1. 戻り **AppDeletegate.m** ファイルで、Engagement Reach モジュールをインポートします。

        #import "AEReachModule.h"

2. `application:didFinishLaunchingWithOptions` 内に Reach モジュールを作成し、それをEngagement を初期化する既存の行に渡します。

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###アプリで APNS プッシュ通知を受信できるようにする

1. 次の行を `application:didFinishLaunchingWithOptions` メソッドに追加します。

        if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
            [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
            [application registerForRemoteNotifications];
        }
        else {

            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

2. 次のように、`application:didRegisterForRemoteNotificationsWithDeviceToken` メソッドを追加します。

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. 次のように、`didFailToRegisterForRemoteNotificationsWithError` メソッドを追加します。

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. 次のように、`didReceiveRemoteNotification:fetchCompletionHandler` メソッドを追加します。

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile Engagement iOS SDK]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png


