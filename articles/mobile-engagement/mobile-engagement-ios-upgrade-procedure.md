<properties
    pageTitle="Azure Mobile Engagement iOS SDK のアップグレード手順"
    description="Azure Mobile Engagement 用 iOS SDK の最新の更新プログラムと手順"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="MehrdadMzfr"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="08/05/2015"
    ms.author="MehrdadMzfr" />


# アップグレードの手順

既にアプリケーションに以前のバージョンのモバイル エンゲージメントを統合してある場合は、SDK をアップグレードするときに、次の点を考慮する必要があります。

まず、新しいバージョンの SDK ごとに、EngagementSDK フォルダーと EngagementReach フォルダーを置き換える (削除し、xcode で再インポートする) 必要があります。

## 2.0.0 から 3.0.0 に移行

iOS 4.X のサポートが終了。 このバージョンから、アプリケーションのデプロイ ターゲットは iOS 6 以降である必要があります。

かどうかリーチを使用して、アプリケーションでは、追加する必要あります `リモート通知` 値を `UIBackgroundModes` リモート通知を受信するために、Info.plist ファイル内の配列。

メソッド `アプリケーション: didReceiveRemoteNotification:` に置き換える必要がある `アプリケーション: didReceiveRemoteNotification:fetchCompletionHandler:` 、アプリケーション デリゲート。

AEPushDelegate.h インターフェイスは廃止されるため、すべての参照を削除する必要があります。 これは、削除が含まれています。 `[[EngagementAgent shared] setPushDelegate:self]` と、アプリケーション デリゲートからデリゲート メソッド。

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

## 1.16.0 から 2.0.0 に移行

Azure Mobile Engagement を使用するアプリに Capptain SAS によって提供される Capptain サービスから SDK の統合を移行する方法を次に示します。
以前のバージョンから移行する場合は、Capptain web サイトをご覧のうえ、まず 1.16 に移行し、次の手順を適用してください。

>[Azure.IMPORTANT] Capptain とモバイル エンゲージメントは、同じサービスではないと、次の手順では、クライアント アプリケーションを移行する方法についてのみ詳しく説明します。 アプリで SDK を移行しても、データは Capptain サーバーから Mobile Engagement のサーバーに移行されません。

### エージェント

メソッド `registerApp:` が新しいメソッドで置換された `init:`します。 その結果、アプリケーション デリゲートは更新され、接続文字列を使用する必要があります。

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

SmartAd の追跡が SDK のすべてのインスタンスを削除する必要がだけから削除された `AETrackModule` クラス

### クラス名の変更

ブランド変更の一部として、いくつかの変更が必要なクラス名とファイル名があります。

「CP」で始まるクラスは、「AE」で始まる名前に変更されました。

例:

-   `CPModule.h` の名前に変更が `AEModule.h`します。

「Capptain」で始まるクラスは、「Engagement」で始まる名前に変更されました。

次に例を示します。

-   クラス `CapptainAgent` の名前に変更が `EngagementAgent`します。
-   クラス `CapptainTableViewController` の名前に変更が `EngagementTableViewController`します。
-   クラス `CapptainUtils` の名前に変更が `EngagementUtils`します。
-   クラス `CapptainViewController` の名前に変更が `EngagementViewController`します。





