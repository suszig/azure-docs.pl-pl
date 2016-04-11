<properties
    pageTitle="Azure Notification Hubs のリッチなプッシュ"
    description="機能豊富なプッシュ通知を Azure から iOS アプリに送信する方法について説明します。 コード サンプルは Objective-C と C# で記述されています。"
    documentationCenter="ios"
    services="notification-hubs"
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="12/16/2015"
    ms.author="wesmc"/>

#Azure Notification Hubs のリッチなプッシュ


##概要

手軽なリッチ コンテンツでユーザーの関心を引くために、アプリケーションはプレーン テキストを超えるべきかもしれません。 これらの通知は、ユーザー インタラクションや、および、url、サウンド、画像、クーポンといったプレゼント コンテンツを昇格します。 このチュートリアルは、 [ユーザーへの通知](notification-hubs-aspnet-backend-ios-notify-users.md) トピック、ペイロード (画像など) を組み込んだプッシュ通知を送信する方法を示しています。


このチュートリアルは iOS 7 および 8 に対応しています。

  ![][IOS1]

概要:

1. アプリケーションのバックエンド:
    - バックエンド データベースやローカル ストレージにリッチ ペイロード (この場合は画像) を格納する
    - このリッチな通知の ID をデバイスに送信する
2. デバイス上のアプリケーション:
    - バックエンドにアクセスし、受け取った ID でリッチ ペイロードを要求する
    - データ取得が完了すると、デバイス上にユーザーへの通知を送り、ユーザーがタップするとすぐにペイロードを表示する


## Web API プロジェクト

1. Visual Studio で開き、 **AppBackend** で作成したプロジェクトを [ユーザーへの通知](notification-hubs-aspnet-backend-ios-notify-users.md) チュートリアルです。
2. では、ユーザーに通知しに配置したい画像を取得、 **img** プロジェクト ディレクトリのフォルダーです。
3. をクリックして **[すべてのファイル** ソリューション エクスプ ローラーでフォルダーを右クリックし、 **プロジェクトに含める**します。
4. [プロパティ] ウィンドウでビルド アクションを変更する画像が選択されている、 **埋め込まれたリソース**します。

    ![][IOS2]

5.  **Notifications.cs**, 、次の追加ステートメントを使用します。

        using System.Reflection;

6. 全体を更新 **通知** クラスを次のコードとします。 必ず、プレースホルダーを通知ハブの資格情報と画像ファイルの名前に置き換えてください。

        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }

> [AZURE.NOTE]  (省略可能)参照してください [を埋め込むし、Visual c# を使用してリソースにアクセスする方法](http://support.microsoft.com/kb/319292) を追加し、プロジェクト リソースを取得する方法の詳細。

7.  **NotificationsController.cs**, 、再定義 **NotificationsController**  次のスニペットをします。 これによりデバイスに最初のリッチなサイレント通知の ID が送信され、クライアント側で画像の取得が可能になります。

        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);

            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

            return result;
        }

        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

8. 次に、このアプリを Azure の Web サイトにもう一度デプロイして、すべてのデバイスからアクセスできるようにします。 右クリックし、 **AppBackend** プロジェクトし、選択 **発行**します。

9. 発行先として Azure の Web サイトを選択します。 Azure アカウントでログインし、既存または新規の web サイトを選択し、メモ、 **送信先 URL** プロパティに、 **接続** ] タブをクリックします。 この URL として参照、 *バックエンド エンドポイント* このチュートリアルで後述します。 クリックして **発行**します。

## iOS プロジェクトを変更する

送信するようにアプリ バックエンドを変更しただけ *id* 、通知をその id を処理し、バックエンドからリッチ メッセージを取得して iOS アプリを変更します。

1. IOS プロジェクトを開き、メイン アプリケーション ターゲットに移動して、リモート通知を有効に、 **ターゲット** セクションです。

2. をクリックして **機能**, を有効にする **Background Modes**, 、確認し、 **リモート通知** チェック ボックスをオンします。

    ![][IOS3]

3. 移動 **Main.storyboard**, 、View Controller (と呼ばれる Home View Controller このチュートリアルでは) があることを確認および [ユーザーへの通知](notification-hubs-aspnet-backend-ios-notify-users.md) チュートリアルです。

4. 追加、 **Navigation Controller** ストーリー ボード、およびコントロールをドラッグすると、Home View Controller に、 **ビューのルート** ナビゲーションのです。 確認、 **Is Initial View Controller** 属性では、インスペクターが、Navigation Controller のみに選択されています。

5. 追加、 **ビュー コント ローラー** ストーリー ボードを追加する、 **Image View**します。 これは、ユーザーが情報を得るために通知をクリックすると表示されるページです。 ストーリーボードは次のようになります。

    ![][IOS4]

6. をクリックして、 **Home View Controller** 、ストーリー ボードを確認では **homeViewController** としてその **Custom Class** と **Storyboard ID** 、Identity inspector します。

7. として Image View Controller について、同じ操作を行います **imageViewController**します。

8. という名前の新しい View Controller クラスを作成し、 **imageViewController** を作成した UI を処理します。

9.  **ImageViewController.h**, 、コント ローラーのインターフェイス宣言に次のコードを追加します。 ストーリー ボードのイメージ ビューから、これらのプロパティに Control を押しながらドラッグして、この 2 つをリンクさせます。

        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;

10.  **ImageViewController.m**, の末尾に次のコードを追加 **viewDidload**:

        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];

11.  **AppDelegate.m**, 、作成したイメージ コント ローラーをインポートします。

        #import "imageViewController.h"

12. 次の宣言を使用して interface セクションを追加します。

        @interface AppDelegate ()

        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;

        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;

        @end

13.  **AppDelegate**, 、アプリケーションがサイレント通知に登録することを確認 **application: didFinishLaunchingWithOptions**:

        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");

            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";

            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];


            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

14. 次の実装の代わりに使用し **アプリケーション: didRegisterForRemoteNotificationsWithDeviceToken** 、ストーリー ボード UI の変更します。

        // Access navigation controller which is at the root of window
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        // Get home view controller from stack on navigation controller
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        hvc.deviceToken = deviceToken;

15. 次の方法を次に、追加 **AppDelegate.m** 、エンドポイントから画像を取得し、取得の完了時にローカル通知を送信します。 必ずプレースホルダー `{backend endpoint}` をバックエンド エンドポイントに置き換えます。

        NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

        // Helper: retrieve notification content from backend with rich notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
            UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
            homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
            NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
            // Check if authenticated
            if (!authenticationHeader) return;

            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];

            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200) {
                    // From NSData to UIImage
                    self.imagePayload = [UIImage imageWithData:data];

                    completion(nil);
                }
                else {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(error);
                    else {
                        completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        // Handle silent push notifications when id is sent from backend
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
            self.userInfo = userInfo;
            int richId = [[self.userInfo objectForKey:@"richId"] intValue];
            NSString* richType = [self.userInfo objectForKey:@"richType"];

            // Retrieve image data
            if ([richType isEqualToString:@"img"]) {  
                [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                    if (!error){
                        // Send local notification
                        UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                        // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                        localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                        localNotification.userInfo = self.userInfo;
                        localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                        localNotification.timeZone = [NSTimeZone defaultTimeZone];

                        // iOS8 categories
                        if (self.iOS8) {
                            localNotification.category = @"richPush";
                        }

                        [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                        handler(UIBackgroundFetchResultNewData);
                    }
                    else{
                        handler(UIBackgroundFetchResultFailed);
                    }
                }];
            }
            // Add "else if" here to handle more types of rich content such as url, sound files, etc.
        }

16. 内のイメージ ビュー コント ローラーを開き、上記のローカル通知を処理 **AppDelegate.m** 次のメソッドで。

        // Helper: redirect users to image view controller
        - (void)redirectToImageViewWithImage: (UIImage *)img {
            UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
            UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                     bundle: nil];
            imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
            // Pass data/image to image view controller
            imgViewController.imagePayload = img;

            // Redirect
            [navigationController pushViewController:imgViewController animated:YES];
        }

        // Handle local notification sent above in didReceiveRemoteNotification
        - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
            if (application.applicationState == UIApplicationStateActive) {
                // Show in-app alert with an extra "more" button
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];

                [alert show];
            }
            // App becomes active from user's tap on notification
            else {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
        }

        // Handle buttons in in-app alerts and redirect with data/image
        - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
            // Handle "more" button
            if (buttonIndex == 1)
            {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            // Add "else if" here to handle more buttons
        }

        // Handle notification setting actions in iOS8
        - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
            // Handle richPush related buttons
            if ([identifier isEqualToString:@"richPushMore"]) {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            completionHandler();
        }

## アプリケーションの実行

1. XCode を使用して、物理 iOS デバイスでアプリケーションを実行します (プッシュ通知はシミュレーターでは機能しません)。

2. IOS アプリケーションの UI で、認証とクリックのユーザー名と同じ値のパスワードを入力 **Log In**します。

3. クリックして **Send push** とアプリ内アラートが表示されます。 クリックすると **詳細**, 、アプリ バックエンドに含めるために選択した画像にすると表示されます。

4. クリックすることも **Send push** すぐにデバイスのホーム ボタンを押すとします。 少し待つと、プッシュ通知が届きます。 タップするか、[More] をクリックすれば、アプリケーションとリッチな画像コンテンツが現れます。


[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png

