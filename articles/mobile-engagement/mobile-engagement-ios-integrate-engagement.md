<properties
    pageTitle="Azure Mobile Engagement iOS SDK の統合"
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

#Engagement を iOS に統合する方法

> [AZURE.SELECTOR]
- [Windows ユニバーサル](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

この手順では、iOS アプリケーションで Engagement の分析機能、監視機能を有効化する最も簡単な方法を説明します。

> [AZURE.IMPORTANT] Engagement SDK には iOS6 以降が必要です。 アプリケーションの展開ターゲットは以上である必要があります iOS 6 です。

次の手順は、ユーザー、セッション、アクティビティ、クラッシュ、テクニカルに関するすべての統計情報を計算するのに必要なログのレポートを有効にするためのものです。 イベント、エラー、ジョブなどその他の統計の計算に必要なログのレポートは、エンゲージメント API を使用して手動で行う必要があります (を参照してください [タグ付け API を iOS アプリで高度なモバイル エンゲージメントを使用する方法](mobile-engagement-ios-use-engagement-api.md) これらの統計がアプリケーションに依存するためです。

##Engagement SDK を iOS プロジェクトに埋め込む

IOS SDK をダウンロードから [ここ](http://aka.ms/qk2rnj)します。
エンゲージメント SDK を iOS プロジェクトに追加: Xcode で、プロジェクトと選択] を右クリックして **"Add files to..."** を選択し、 `EngagementSDK` フォルダーです。

Engagement では追加のフレームワークを動作させる必要がある: プロジェクト エクスプローラーでプロジェクト ペインを開き、適切なターゲットを選択します。 実行して [、 **"Build phases"** ] タブをクリックし、[、 **"Link Binary With Libraries"** ] メニューの [これらのフレームワークを追加します。

> -   `AdSupport.framework` - リンクを `Optional` として設定します。
> -   `SystemConfiguration.framework`
> -   `CoreTelephony.framework`
> -   `CFNetwork.framework`
> -   `CoreLocation.framework`
> -   `libxml2.dylib`

> [AZURE.NOTE] AdSupport フレームワークは削除できます。 Engagement では、IDFA の収集のためこのフレームワークが必要です。 ただし、IDFA コレクションは無効できます \ < ios-sdk-engagement-idfa\ > この ID に関する新しい Apple ポリシーに準拠するには

##Engagement SDK を初期化する

アプリケーション デリゲートを変更する必要がある:

-   実装ファイルの先頭で、Engagement エージェントをインポートします。

        [...]
        #import "EngagementAgent.h"

-   メソッド内で Engagement を初期化 '**applicationDidFinishLaunching:**'または'**Application:didfinishlaunchingwithoptions::**'。

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
          [...]
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
          [...]
        }

##基本的なレポート

### 推奨される方法: `UIViewController` クラスをオーバーロードします

ユーザー、セッション、アクティビティ、クラッシュ、技術に関する統計の計算に Engagement が必要なすべてのログのレポートをアクティブ化するために行うことができます単にすべての `UIViewController` サブ クラスから継承、 `EngagementViewController` クラス (同じルール `UITableViewController` -\ > `EngagementTableViewController`)。

**Engagement なし:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**Engagement あり:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### 代替の方法: 手動で `startActivity()` を呼び出す

`UIViewController` クラスをオーバーロードできないか、オーバーロードしたくない場合は、`EngagementAgent` のメソッドを直接呼び出して、活動を開始できます。

> [AZURE.IMPORTANT] SDK は自動的に呼び出し、iOS、 `endActivity()` メソッドは、アプリケーションが閉じられるとします。 そのため、 *高* を呼び出すことをお勧め、 `startActivity` 、ユーザーのアクティビティが変化するごとにされ、 *NEVER* を呼び出す、 `endActivity` メソッドは、このメソッドを呼び出して強制的に、現在のセッションが終了します。

##場所レポート

Apple の規約では、統計の目的に限定されたアプリケーションでの場所の追跡の使用は認められていません。 このため、アプリケーションが他の目的でも場所の追跡機能を使用する場合にのみ、場所レポートを有効にすることをお勧めします。

IOS 8 以降では、アプリケーションの Info.plist ファイルにキー [NSLocationWhenInUseUsageDescription] または [NSLocationAlwaysUsageDescription] の文字列を設定して、アプリがロケーション サービスを使用する方法の説明を提供する必要があります。 Engagement を使って背景で場所をレポートするには、キー NSLocationAlwaysUsageDescription を追加します。 それ以外の場合は、キー NSLocationWhenInUseUsageDescription を追加します。

### 大まかなエリアの位置報告

大まかなエリアの位置報告では、デバイスに関連付けられた国、リージョン、地域をレポートできます。 このタイプの場所レポートでは、セル ID または WIFI に基づいたネットワークの場所のみを使用します。 デバイス エリアがセッションごとに最大 1 回レポートされます。 GPS を使用しないため、このタイプの場所レポートでは、まったくとは言わないまでも、電力消費量にほとんど影響がありません。

報告されたエリアを基に、ユーザー、セッション、イベント、エラーに関する地理的な統計をコンピューティングします。 また、リーチ キャンペーンの条件としても使用されます。 [デバイス API] に協力してくれた、デバイスで報告された最後の既知のエリアを取得できます。

大まかなエリアの位置報告を有効にするには、Engagement エージェントを初期化した後、次の行を追加します。

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### リアル タイム場所レポート

リアル タイム場所レポートでは、デバイスに関連付けられた緯度と経度をレポートできます。 既定では、このタイプの場所レポートでは (セル ID または WIFI に基づいた) ネットワークの場所のみが使用されます。レポートは、アプリケーションを前景で実行した場合 (セッション中) にのみアクティブになります。

リアルタイム場所は *いない* 統計情報を計算するために使用します。 このレポートの目的は、リーチ キャンペーンでリアル タイムの
地理フェンス操作 \ < リーチ対象ユーザー geofencing\ > リーチ キャンペーンの条件。

リアル タイム場所レポートを有効にするには、Engagement エージェントの初期化後に、次の行を追加します。

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### GPS ベースのレポート

既定では、リアル タイム場所レポートでは、ネットワーク ベースの場所のみを使用します。 GPS ベースの場所 (正確性が格段に優れています) を使用できるようにするには、以下を追加します。

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### 背景レポート

既定では、リアル タイム場所レポートは、アプリケーションを前景で実行した場合 (セッション中) にのみアクティブになります。 バックグウランドでのレポートも有効にするには、以下を追加します。

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [AZURE.NOTE] ネットワーク ベースの場所にのみ、バック グラウンドで、アプリケーションが実行するときに、報告された、GPS を有効になっている場合でもです。

この関数の実装は、アプリケーションが背景になったときに [startMonitoringSignificantLocationChanges] を呼び出します。 これにより、新しい場所イベントが発生すると、アプリケーションが背景に自動的に再起動されることに注意してください。

##詳細な報告

オプションとして、アプリケーション固有のイベント、エラー、ジョブをレポートする必要がある場合は、`EngagementAgent` クラスのメソッドを通じて Engagement API を使用します。 このクラスのオブジェクトを取得するには、`[EngagementAgent shared]` 静的メソッドを呼び出します。

Engagement API では、すべての Engagement の高度な機能を使用でき、
IOS でエンゲージメント API (およびの技術文書で、 `EngagementAgent` クラス)。

##IDFA コレクションの無効化

既定では、エンゲージメントは、ユーザーを一意に識別するために、[IDFA] を使用します。 ただし、アプリの他の場所で広告を使用していない場合、App Store のレビュー プロセスで拒否される場合があります。 IDFA コレクションは、プリプロセッサのマクロ `ENGAGEMENT_DISABLE_IDFA` を pch ファイル (またはアプリケーションの `Build Settings`) に追加することによって無効にできます。 これにより、アプリケーション ビルドの `ASIdentifierManager`、`advertisingIdentifier`、`isAdvertisingTrackingEnabled` への参照がないことを確認できます。

統合、 **prefix.pch** ファイル。

    #define ENGAGEMENT_DISABLE_IDFA
    ...

IDFA コレクションがアプリケーション内で適切に無効化されていることを確認するには、Engagement テスト ログをチェックします。 詳細については、統合テスト \ < ios-sdk-engagement-テスト-idfa\ > ドキュメントを参照してください。

##ログ レポートの無効化

### メソッド呼び出しを使用した場合

Engagement でログの送信を停止したい場合は、以下を呼び出します。

    [[EngagementAgent shared] setEnabled:NO];

この呼び出しは永続的です: `NSUserDefaults` を使用して情報を格納します。

ログ レポートは、同じ関数を `YES` でもう一度呼び出すことによって有効にすることができます。

### 設定バンドルでの統合

この機能を呼び出す代わりに、この設定を既存の `Settings.bundle` ファイルで直接統合することもできます。 文字列 `engagement_agent_enabled` は優先識別子として使用し、トグル スイッチ (`PSToggleSwitchSpecifier`) に関連付ける必要があります。

次の `Settings.bundle` の例は、その実装方法を示しています。

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier

