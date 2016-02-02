<properties
    pageTitle="Azure モバイル エンゲージメント iOS SDK リーチの統合"
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


# iOS でエンゲージメント リーチを統合する方法

> [AZURE.IMPORTANT] このガイドを実行する前に、「iOS でエンゲージメントを統合する方法」のドキュメントに記載されている統合手順に従う必要があります。


### アプリがサイレント プッシュ通知を受信できるようにする

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

## 統合手順

### iOS プロジェクトにエンゲージメント リーチ SDK を埋め込む

-   Xcode プロジェクトにリーチ SDK を追加します。 Xcode に移動 **プロジェクト \ > [プロジェクトに追加** を選択し、 `EngagementReach` フォルダーです。

### アプリケーション デリゲートを変更する

-   実装ファイルの上部で、Engagement Reach モジュールをインポートします。

        [...]
        #import "AEReachModule.h"

-   メソッドの内部 `applicationDidFinishLaunching:` または `Application:didfinishlaunchingwithoptions::`, 、reach モジュールを作成し、それを既存の Engagement 初期化行に渡します。

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [...]
    
      return YES;
    }

-   通知アイコンの **'icon.png'** 文字列とイメージ名を変更します。
-   オプションを使用する場合は、 *更新バッジ値* リーチ キャンペーンまたはネイティブ プッシュを使用するかどうかに \</SaaS/Reach API/Campaign format/Native Push\> キャンペーン、Reach モジュールにバッジ アイコン自体 (アプリケーション バッジは自動的にクリアされ、アプリケーションが起動またはフォア グラウンドで実行するたびに、Engagement が格納されている値もリセット) を管理できるようにする必要があります。これは、Reach モジュールの初期化後に、次の行を追加することによって行われます。

        [reach setAutoBadgeEnabled:YES];

-   Reach データ プッシュを処理する場合に準拠して、アプリケーション デリゲートを使用する必要があります、 `AEReachDataPushDelegate` プロトコルです。 Reach モジュールの初期化後に、次の行を追加します。

        [reach setDataPushDelegate:self];

-   メソッドを実装したり、 `onDataPushStringReceived:` と `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` 、アプリケーション デリゲート。

    -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
    {
       NSLog(@"String data push message with category <%@> received: %@", category, body);
       return YES;
    }
    
    -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
    {
       NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
       // Do something useful with decodedBody like updating an image view
       return YES;
    }


### カテゴリ

カテゴリ パラメーターは、データ プッシュ キャンペーンを作成するときに指定でき、データ プッシュをフィルター処理するために使用できます。 これはさまざまな種類をプッシュする場合に役立ちます。 の `Base64` データやを解析前にそれらの種類を識別します。

**アプリケーションは、リーチ コンテンツを受信して表示する準備ができました。**

## お知らせとポーリングを常に受信する方法

エンゲージメントは、Apple Push Notification Service を使用して、いつでもエンドユーザーにリーチ通知を送信できます。

この機能を有効にするには、Apple プッシュ通知用のアプリケーションを準備し、アプリケーション デリゲートを変更する必要があります。

### Apple プッシュ通知用アプリケーションを準備する

「How to Prepare your Application for Apple Push Notifications (Apple プッシュ通知用アプリケーションを準備する方法)」をご覧ください。

### 必要なクライアント コードを追加する

*この時点で、アプリケーションにはエンゲージメント フロントエンドで登録済みの Apple プッシュ証明書が必要になります。*

まだ登録していない場合は、プッシュ通知を受信するアプリケーションを登録する必要があります。 アプリケーションの起動時には、次の行を追加 (通常 `Application:didfinishlaunchingwithoptions::`)。

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {
        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

次に、Apple サーバーによって返されたデバイス トークンをエンゲージメントに提供する必要があります。 これはという名前のメソッドで行います `アプリケーション: didRegisterForRemoteNotificationsWithDeviceToken:` 、アプリケーション デリゲート。

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

最後に、アプリケーションがリモート通知を受信したときに、エンゲージメント SDK を通知する必要があります。 これを実行するメソッドを呼び出して `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` 、アプリケーション デリゲート。

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [AZURE.NOTE] 上記のメソッドは、iOS 7 で導入されました。IOS を対象としている場合は、<、7 は、メソッドを実装することを確認 `アプリケーション: didReceiveRemoteNotification:` 、アプリケーション デリゲートの呼び出しで `applicationDidReceiveRemoteNotification` の代わりに nil を渡して engagementagent、 `ハンドラー` 引数。

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] 既定では、エンゲージメント リーチが completionHandler を制御します。 手動で応答する場合、 `ハンドラー` 、コード内のブロックに nil を渡すことが、 `ハンドラー` ブロック引数と完了の制御を自分でします。 参照してください、 `UIBackgroundFetchResult` 一覧については、使用可能な値の型。


### 完全な例

完全な統合の例を次に示します。

    #pragma mark -
    #pragma mark Application lifecycle
    
    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];
    
      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];
    
      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];
    
      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }
    
    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }
    
    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

## キャンペーンをカスタマイズする方法

### 通知

通知には、システム通知とアプリ内通知の 2 種類があります。

システム通知は iOS によって処理され、カスタマイズすることはできません。

アプリ内通知は、現在のアプリケーション ウィンドウに動的に追加されているビューから成ります。 これは、通知オーバーレイと呼ばれます。 通知オーバーレイはアプリケーション内のビューを変更する必要がないため、迅速な統合に適しています。

#### レイアウト

アプリ内通知の外観を変更するには、ファイルだけで変更できます `AENotificationView.xib` タグの値と、既存のサブビューの種類を保持している限りのニーズにします。

既定では、アプリ内通知は画面の下部に表示されます。 画面の上部に表示する場合は、編集、提供された `AENotificationView.xib` を変更して、 `自動` そのスーパー ビューの上部にある保存できるように、メイン ビューのプロパティです。

#### カテゴリ

指定されたレイアウトを変更する場合は、すべての通知の外観を変更します。 カテゴリを使用して、通知対象のさまざまな外観 (場合によっては動作) を定義できます。 リーチ キャンペーンを作成する場合にカテゴリを指定できます。 また、カテゴリを使用してお知らせとポーリングもカスタマイズできます。これについては、このドキュメントの後半で説明します。

通知のカテゴリ ハンドラーを登録するには、reach モジュールが初期化された時点で呼び出しを追加する必要があります。

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier` プロトコルに準拠しているオブジェクトのインスタンスである必要があります `AENotifier`します。

自分でプロトコル メソッドを実装するか、既存のクラスを再実装することもできます `AEDefaultNotifier` を既に多くの作業を実行します。

たとえば、特定のカテゴリの通知ビューを再定義する場合は、次の例に従って操作します。

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end
    
    @implementation MyNotifier
    
    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }
    
    @end

この簡単なカテゴリ例では、という名前のファイルを使用するいると仮定 `MyNotificationView.xib` メイン アプリケーション バンドルにします。 メソッドを見つけ、対応するかどうかは `.xib`, 、通知は表示されませんし、エンゲージメントはコンソールにメッセージを出力します。

指定された nib ファイルは、次の規則を遵守する必要があります。

-   1 つのビューのみを含む。
-   サブビューは、指定された nib ファイル内のものと同じタイプという名前で `AENotificationView.xib`
-   サブビューは、指定した内のものと同じタグを持つ必要がありますという名前の nib ファイル `AENotificationView.xib`

> [AZURE.TIP] という名前の指定された nib ファイルをコピーするだけ `AENotificationView.xib`, 、し、そこから作業を開始します。 注意してください、この nib ファイル内のビューがクラスに関連付けられている `AENotificationView`します。 このクラスは、メソッドを再定義 `layoutSubViews` に移動し、コンテキストに応じてサブビューをサイズ変更します。 コードに ことも、 `UIView` やカスタム ビュー クラスです。

通知の細かくカスタマイズする必要がある場合 (インスタンスの場合、コードから直接、ビューを読み込む)、指定されたソース コードやクラスのドキュメントを確認することをお勧め `プロトコル ReferencesDefaultNotifier` と `AENotifier`します。

複数のカテゴリに対して同じ通知を使用できます。

次のように、既定の通知を再定義もできます。

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### 通知の処理

いくつかのライフ サイクル メソッドを呼び出す既定のカテゴリを使用する場合、 `AEReachContent` オブジェクトの統計情報を報告し、キャンペーンの状態を更新します。

-   アプリケーションでは、通知が表示されるときに、 `displayNotification` メソッドが呼び出されます (統計情報を報告) によって `AEReachModule` 場合 `handleNotification:` を返します `はい`します。
-   通知を閉じる場合、 `exitNotification` メソッドが呼び出されて統計情報が報告され、次のキャンペーンが処理できるようことができます。
-   通知をクリックすると、 `actionNotification` が呼び出されると、統計情報を報告し、関連付けられたアクションを実行します。

場合は、実装の `AENotifier` 既定の動作をバイパスを自分でこれらのライフ サイクル メソッドを呼び出す必要があります。 次の例では、既定の動作がバイパスされるいくつかのケースを示しています。

-   拡張しない `AEDefaultNotifier`, 、最初からカテゴリの処理を実装した場合などです。
-   オーバーライドした場合 `prepareNotificationView:forContent:`, 、ともがマッピングを必ず `onNotificationActioned` または `onNotificationExited` がいずれかの U.I コントロールです。

> [AZURE.WARNING] 場合 `handleNotification:` をスローし、例外は、コンテンツを削除し、 `ドロップ` が呼び出されると、これは統計情報で報告し、次のキャンペーンを処理できるようします。

#### 既存のビューの一部に通知を含める

オーバーレイは迅速な統合に適していますが、不便な場合や望ましくない副作用をもたらす場合があります。

一部のビューのオーバーレイ システムに不満な場合は、これらのビューをカスタマイズできます。

既存のビューに通知のレイアウトを含めることができます。 これを行うには、次の 2 つの実装スタイルがあります。

1.  インターフェイス ビルダーを使用して、通知ビューを追加します。

    -   [*インターフェイス ビルダー*] を開きます。
    -   320 x 60 (または、iPad を使用している場合) を配置 `UIView` 通知を表示します。
    -   このビューのタグ値を **36822491** に設定します。

2.  プログラムを使用して通知ビューを追加します。 ビューが初期化されたら、次のコードを追加します。

        UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
        notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
        [self.view addSubview:notificationView];


`NOTIFICATION_AREA_VIEW_TAG` マクロを参照して `AEDefaultNotifier.h`します。
> [AZURE.NOTE] 既定の通知は、通知のレイアウトがこのビューに含まれていて、オーバーレイが追加されていないことを自動的に検出します。

### お知らせとポーリング

#### レイアウト

ファイルを変更して `AEDefaultAnnouncementView.xib` と `AEDefaultPollView.xib` タグの値と、既存のサブビューの種類を保持している限りです。

#### カテゴリ

##### 代替レイアウト

通知と同様に、キャンペーンのカテゴリを使用して、お知らせとポーリングの代替レイアウトを設定できます。

お知らせのカテゴリを作成するには、**AEAnnouncementViewController** を拡張し、reach モジュールが初期化された時点でこれを登録する必要があります。

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [AZURE.NOTE] 毎回ユーザーがクリックしてカテゴリ「my \_category」のお知らせ通知に登録されているビュー コント ローラー (その場合は `MyCustomAnnouncementViewController`) メソッドを呼び出すことによって初期化される `initWithAnnouncement:` され、現在のアプリケーション ウィンドウ ビューが追加されます。

実装で、 `AEAnnouncementViewController` クラス、プロパティを読み取ることが `アナウンス` 、サブビューを初期化します。 次の例を検討してください、2 つのラベルを使用してが初期化は `タイトル` と `本文` のプロパティ、 `AEReachAnnouncement` クラス。

    -(void)loadView
    {
        [super loadView];
    
        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;
    
        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;
    
        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

自分でビューをロードしたくない場合は、既定のお知らせビューのレイアウトを再利用する、カスタム ビュー コント ローラーは作成するだけが提供されているクラスを拡張 `AEDefaultAnnouncementViewController`します。 その場合は、nib ファイルを複製 `AEDefaultAnnouncementView.xib` し、カスタム ビュー コント ローラーで読み込めるように名前を変更 (という名前のコント ローラーの `CustomAnnouncementViewController`, 、nib ファイルを呼び出す必要があります `CustomAnnouncementView.xib`)。

お知らせの既定のカテゴリを置き換えるで定義されたカテゴリ用にカスタム ビュー コント ローラーを登録するだけで `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

ポーリングは、同じようにカスタマイズできます。

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

このときに、指定された `MyCustomPollViewController` 拡張する必要があります `AEPollViewController`します。 既定のコント ローラーから拡張することができますか。 `AEDefaultPollViewController`します。
> [AZURE.IMPORTANT] いずれかを呼び出さないことを忘れないでください `アクション` (`submitAnswers:` カスタム ポーリング ビュー コント ローラーの) または `終了` メソッド ビュー コント ローラーが閉じる前にします。 これを行わないと、統計情報は送信されず (キャンペーンは分析されません)、さらに重要な次のキャンペーンが、アプリケーション プロセスが再起動するまで通知されないことになります。

##### 実装例

この実装では、カスタム アナウンス ビューが外部の xib ファイルから読み込まれます。

高度な通知カスタマイズと同様に、標準実装のソース コードをご確認ください。

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }
    
    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;
    
    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;
    
    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }
    
    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }
    
    - (void)viewDidLoad {
      [super viewDidLoad];
    
      /* Init announcement title */
      titleLabel.text = self.announcement.title;
    
      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }
    
      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];
    
      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }
    
    #pragma mark Actions
    
    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }
    
    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }
    
    @end



