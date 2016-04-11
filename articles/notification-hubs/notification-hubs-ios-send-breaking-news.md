<properties
    pageTitle="Notification Hubs ニュース速報チュートリアル - iOS"
    description="Azure Service Bus Notification Hubs を使用して iOS デバイスにニュース速報通知を送信する方法を説明します。"
    services="notification-hubs"
    documentationCenter="ios"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="wesmc"/>

# Notification Hubs を使用したニュース速報の送信

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##概要

このトピックでは、Azure Notification Hubs を使用してニュース速報通知を iOS アプリケーションにブロードキャストする方法について説明します。 完了すると、興味のあるニュース速報カテゴリに登録し、それらのカテゴリのプッシュ通知だけを受信できるようになります。 このシナリオは、既に興味があると宣言しているユーザーのグループに通知を送信する必要がある多くのアプリケーション (RSS リーダー、音楽ファン向けアプリケーションなど) で一般的なパターンです。

ブロードキャスト シナリオは 1 つまたは複数を含めることによって有効になっている _タグ_ 、通知ハブで登録を作成するときにします。 通知がタグに送信されると、タグに登録されたすべてのデバイスが通知を受信します。 タグは文字列にすぎないため、事前にプロビジョニングする必要はありません。 タグの詳細についてを参照してください [通知ハブのルーティングとタグ式](notification-hubs-routing-tag-expressions.md)します。


##前提条件

このトピックで作成したアプリケーションが基 [Notification Hubs を使ってみる][get-started]します。 このチュートリアルを開始する前にする必要があります既に完了している [Notification Hubs を使ってみる][get-started]します。

##アプリケーションにカテゴリ選択を追加する

最初の手順として、既存のストーリーボードに UI 要素を追加して、ユーザーが登録するカテゴリを選択できるようにします。 ユーザーにより選択されるカテゴリは、デバイスに格納されます。 アプリが起動すると、通知ハブにデバイス登録が作成され、選択されたカテゴリがタグとして追加されます。

1. MainStoryboard_iPhone.storyboard で、オブジェクト ライブラリから次のコンポーネントを追加します。
    + "Breaking News" というテキストが付いたラベル
    + "World"、"Politics"、"Business"、"Technology"、"Science"、"Sports" というカテゴリ テキストが付いたラベル
    + 6 つのスイッチが、カテゴリごとに 1 つは、各スイッチを設定 **状態** する **オフ** 既定です。
    + [Subscribe] というラベルが付いた 1 個のボタン

    ストーリーボードは次のようになります。

    ![][3]

2. アシスタント エディターで、すべてのスイッチのアウトレットを作成し、"WorldSwitch"、"PoliticsSwitch"、"BusinessSwitch"、"TechnologySwitch"、"ScienceSwitch"、"SportsSwitch" という名前にします。


3. "subscribe" というボタンのアクションを作成します。 BreakingNewsViewController.h の内容は次のようになります。

        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

        - (IBAction)subscribe:(id)sender;

4. 新しい **Cocoa Touch クラス** と呼ばれる `Notifications`です。 Notifications.h の interface セクションに次のコードをコピーします。

        @property NSData* deviceToken;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;

        - (NSSet*)retrieveCategories;

        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;

5. 次の import ディレクティブを Notifications.m に追加します。

        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. Notifications.m の implementation セクションに次のコードをコピーします。

        SBNotificationHub* hub;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];

            return self;
        }

        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

            [self subscribeWithCategories:categories completion:completion];
        }


        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    このクラスは、このデバイスが受信するニュースのカテゴリの格納と取得を行うためにローカル ストレージを使用します。 またを使用してこれらのカテゴリを登録するメソッドを含む、 [テンプレート](notification-hubs-templates.md) 登録します。

7. AppDelegate.h ファイルで、Notifications.h をインポートするためのステートメントを追加し、Notifications クラスのインスタンスのプロパティを追加します。

        #import "Notifications.h"

        @property (nonatomic) Notifications* notifications;
    

8.  **DidFinishLaunchingWithOptions** AppDelegate.m でメソッドがメソッドの先頭にある通知インスタンスを初期化するコードを追加します。  
 
    `HUBNAME`  `HUBLISTENACCESS` (hubinfo.h で定義されている) する必要があります、 `<hub name>` と `<connection string with listen access>` プレース ホルダーを通知ハブの名前と接続文字列に置き換え *DefaultListenSharedAccessSignature* 既に取得してあります。

        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];

    > [AZURE.NOTE] クライアント アプリケーションを使用して配布される資格情報が一般にセキュリティで保護されないため、クライアント アプリケーションでだけリッスン アクセス用のキーを配布する必要があります。 リッスン アクセスにより、アプリケーションが通知を登録できるようになりますが、既存の登録を変更することはできないため、通知を送信できません。 通知を送信して既存の登録を変更するセキュリティで保護されたバックエンド サービスでは、フル アクセス キーが使用されます。


9.  **DidRegisterForRemoteNotificationsWithDeviceToken** AppDelegate.m で、メソッドは、notifications クラスに、デバイス トークンを渡すには、次のコードでメソッドのコードを置換します。 通知クラスは、通知のカテゴリの登録を実行します。 我々 は、ユーザーは、カテゴリの選択を変更する場合、 `subscribeWithCategories` に応じたメソッド、 **サブスクライブ** を更新する] ボタンをクリックします。

    > [AZURE.NOTE] Apple Push Notification サービス (APNS) によって割り当てられたデバイス トークンはでもいつでも変更可能性、ので通知エラーを回避するには、頻繁に通知を登録する必要があります。 この例では、アプリケーションが起動するたびに通知を登録します。 頻繁に実行されるアプリケーションの場合 (1 日に複数回など)、帯域幅を節約するため、前回の登録から 1 日経過していない場合は登録をスキップできます。

        self.notifications.deviceToken = deviceToken;

        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.

        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];


    この時点では行われないことに他のコードに注意してください、 **didRegisterForRemoteNotificationsWithDeviceToken** メソッドです。

10. 次のメソッドに既に存在する AppDelegate.m が完了しない、 [Notification Hubs を使ってみる][get-started] チュートリアルです。  ない場合は、それらを追加します。

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
            (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

    このメソッドは、単純な表示で、アプリケーションが実行中に受信した通知を処理 **UIAlert**します。

11. ViewController.m で、AppDelegate.h の import ステートメントを追加し、次のコードを XCode で生成されたコピー **サブスクライブ** メソッドです。 このコードは、ユーザーがユーザー インターフェイスで選択した新しいカテゴリ タグを使用するように通知登録を更新します。

        ```
        #import "Notifications.h"
        ```

        NSMutableArray* categories = [[NSMutableArray alloc] init];

        if (self.WorldSwitch.isOn) [categories addObject:@"World"];
        if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
        if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
        if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
        if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
        if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
            if (!error) {
                [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

    このメソッドは、作成、 **NSMutableArray** カテゴリと使用方法、 **通知** 、対応するタグを通知ハブにローカル ストレージとレジスタの一覧を格納するクラス。 カテゴリが変更されると、新しいカテゴリで登録が再作成されます。

12. ViewController.m で、次のコードを追加、 **viewDidLoad** 保存済みのカテゴリに基づいてユーザー インターフェイスを設定します。


        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



これで、アプリは、毎回の起動時に、通知ハブに登録するために使用されるカテゴリ セットをデバイスのローカル ストレージに格納できるようになりました。  ユーザーは、ランタイムとクリックにカテゴリの選択を変更できる、 **サブスクライブ** 、デバイスの登録を更新する方法です。 その場合、ニュース速報通知を送信するように、アプリ自体の中で直接アプリを更新します。


##(省略可能) タグ付けされた通知の送信

Visual Studio にアクセスできない場合は、次のセクションをスキップし、アプリ自体から通知を送信できます。 適切なテンプレート通知を送信することも、 [Azure Classic Portal] 通知ハブの [デバッグ] タブを使用します。 

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]


##(省略可能) デバイスから通知を送信する

通常、通知はバックエンド サービスによって送信されますが、アプリからニュース速報通知を直接送信できます。 更新し、これを行うには、 `SendNotificationRESTAPI` で定義したメソッド、 [Notification Hubs を使ってみる][get-started] チュートリアルです。


1. ViewController.m の更新プログラムで、 `SendNotificationRESTAPI` メソッドを次のカテゴリのタグのパラメーターを受け取り、適切な送信ように [テンプレート](notification-hubs-templates.md) 通知します。

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }



2. ViewController.m の更新プログラムで、 **通知の送信** 次のコードで示すように動作します。 これで、各タグを個別に使用して、複数のプラットフォームに通知が送信されます。



        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



3. プロジェクトをリビルドし、ビルド エラーがないことを確認します。


##アプリケーションを実行して通知を生成する

1. [実行] ボタンを押してプロジェクトをビルドし、アプリケーションを開始します。 サブスクライブし、キーを押しますニュース速報のオプションを選択して、 **購読** ] ボタンをクリックします。 通知が購読されたことを示すダイアログが表示されます。

    ![][1]

    選択すると **購読**, 、アプリケーションは、により、選択されたカテゴリをタグに変換され、通知ハブから、選択されたタグの新しいデバイス登録を要求します。

2. ニュース速報をクリックしとに送信されるメッセージを入力して、 **通知の送信** ] ボタンをクリックします。 または、通知を生成する .NET コンソール アプリケーションを実行します。

    ![][2]


3. ニュース速報を購読している各デバイスが、たった今送信したニュース速報通知を受信します。



## 次のステップ

このチュートリアルでは、ニュース速報をカテゴリごとにブロードキャストする方法について説明しました。 他の高度な通知ハブ シナリオを取り上げている、次のいずれかのチュートリアルを行うことをお勧めします。

+ **[通知ハブを使用したローカライズ ニュース速報のブロードキャスト]**

    ニュース速報アプリケーションを拡張して、ローカライズした通知を送信できるようにする方法について説明します。





<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Use Notification Hubs to broadcast localized breaking news]: notification-hubs-ios-send-localized-breaking-news.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Azure Classic Portal]: https://manage.windowsazure.com


