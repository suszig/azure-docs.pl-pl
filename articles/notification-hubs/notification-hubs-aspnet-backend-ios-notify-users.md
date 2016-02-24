<properties
    pageTitle="Azure Notification Hubs と .NET バックエンドによる iOS ユーザーへの通知"
    description="Azure のユーザーにプッシュ通知を送信する方法について説明します。 コード サンプルは Objective-C で記述されています。また、バックエンドには .NET API を使用しています。"
    documentationCenter="ios"
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="12/16/2015"
    ms.author="wesmc"/>

#Azure Notification Hubs と .NET バックエンドによる iOS ユーザーへの通知

[AZURE.INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

##概要

Azure でプッシュ通知がサポートされたことで、マルチプラットフォームに対応し、簡単に使用できる、スケールアウトされたプッシュ通知インフラストラクチャを利用できるようになりました。これにより、モバイル プラットフォーム向けアプリケーション (コンシューマー用途およびエンタープライズ用途) にプッシュ通知機能を実装する作業が大幅に簡略化されます。 このチュートリアルでは、Azure Notification Hubs を使用して特定のデバイスで特定のアプリケーション ユーザーにプッシュ通知を送信する方法について説明します。 ASP.NET WebAPI バックエンドを使用してクライアントを認証し、通知を生成するガイダンス トピック「 [アプリ バックエンドから登録](notification-hubs-registration-management.md#registration-management-from-a-backend)します。

> [AZURE.NOTE] このチュートリアルを作成し、」の説明に従って通知ハブを構成 [Notification Hubs (iOS) の概要](notification-hubs-ios-get-started.md)します。 このチュートリアルは、前提条件ではまた、 [安全なプッシュ (iOS)](notification-hubs-aspnet-backend-ios-secure-push.md) チュートリアルです。
> バックエンド サービスとして Mobile Services を使用している場合、 [Mobile Services バージョン](../mobile-services-javascript-backend-ios-push-notifications-app-users.md) このチュートリアルのです。



[AZURE.INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## iOS アプリを変更する

1. 作成した単一枠ビュー アプリケーションを開き、 [Notification Hubs (iOS) の概要](notification-hubs-ios-get-started.md) チュートリアルです。

    > [AZURE.NOTE] このセクションで、プロジェクトが、組織名は空で構成されていると想定しています。 そうでない場合は、すべてのクラス名の前にその組織名を付けてください。

2. Main.storyboard で、オブジェクト ライブラリから、以下のスクリーンショットに表示されているコンポーネントを追加します。

    ![][1]

    + **ユーザー名**: プレース ホルダー テキストを含む uitextfield。 A *Enter Username*, 送信の結果にラベルを付けると、左右の余白に制限のすぐ下にある、および結果の送信ラベルの下にします。
    + **パスワード**: プレース ホルダー テキストを含む uitextfield。 A *パスワードの入力*, 、ユーザー名のすぐ下にあるテキスト フィールドであり、左右の余白と、ユーザー名のテキスト フィールドの下に制約します。 チェック、 **Secure Text Entry** 属性インスペクターで下にあるオプション *Return Key*します。
    + **ログイン**: UIButton パスワードのテキスト フィールドの下にあるラベル付きおよびオフにして、 **有効** Attributes Inspector の [オプション *コントロール コンテンツ*
    + **WNS**。 ハブ上でセットアップを経過している場合は、Windows 通知サービスの通知を送信するためには、ラベルとスイッチです。 参照してください、 [Windows 概要](notification-hubs-windows-store-dotnet-get-started.md) チュートリアルです。
    + **GCM**: スイッチとそのラベル ハブ上でセットアップを経過している場合、Google Cloud Messaging に通知を送信するためです。 参照してください [Android 作業の開始](notification-hubs-android-get-started.md) チュートリアルです。
    + **APNS**: Apple Platform Notification Service に通知を送信するためには、ラベルとスイッチです。
    + **Recipent Username**: プレース ホルダー テキストを含む uitextfield。 A *Recipient username tag*, 、ラベルのすぐ下、GCM および GCM ラベルの下であり、左右の余白に制約します。


    Some components were added in the [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-get-started.md) tutorial.

3. **Ctrl キーを押し** ViewController.h をビュー内のコンポーネントからドラッグし、新しいアウトレットが追加されます。

        @property (weak, nonatomic) IBOutlet UITextField *UsernameField;
        @property (weak, nonatomic) IBOutlet UITextField *PasswordField;
        @property (weak, nonatomic) IBOutlet UITextField *RecipientField;
        @property (weak, nonatomic) IBOutlet UITextField *NotificationField;

        // Used to enable the buttons on the UI
        @property (weak, nonatomic) IBOutlet UIButton *LogInButton;
        @property (weak, nonatomic) IBOutlet UIButton *SendNotificationButton;

        // Used to enabled sending notifications across platforms
        @property (weak, nonatomic) IBOutlet UISwitch *WNSSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *GCMSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *APNSSwitch;

        - (IBAction)LogInAction:(id)sender;

4. ViewController.h で、インポート ステートメントのすぐ下に以下の `#define` を追加します。 代替、 *< Your バックエンドのエンドポイントを入力してください >* プレース ホルダーを前のセクションでアプリケーション バックエンドをデプロイするために使用するリンク先の URL。 たとえば、 *http://you_backend.azurewebsites.net*します。

        #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"

4. プロジェクトで、作成、新しい **Cocoa Touch クラス** という名前 **RegisterClient** 作成した ASP.NET バックエンドとやり取りします。 `NSObject` から継承するクラスを作成したら、 RegisterClient.h に以下のコードを追加します。

        @interface RegisterClient : NSObject

        @property (strong, nonatomic) NSString* authenticationHeader;

        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
            andCompletion:(void(^)(NSError*))completion;

        -(instancetype) initWithEndpoint:(NSString*)Endpoint;

        @end

5. RegisterClient.m で、`@interface` セクションを次のように更新します。

        @interface RegisterClient ()

        @property (strong, nonatomic) NSURLSession* session;
        @property (strong, nonatomic) NSURLSession* endpoint;

        -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                    andCompletion:(void(^)(NSError*))completion;
        -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                    completion:(void(^)(NSString*, NSError*))completion;
        -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSString*)token
                    tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion;

        @end

6. RegisterClient.m の `@implementation` セクションを、以下のコードで置き換えます。


        @implementation RegisterClient

        // Globals used by RegisterClient
        NSString *const RegistrationIdLocalStorageKey = @"RegistrationId";

        -(instancetype) initWithEndpoint:(NSString*)Endpoint
        {
            self = [super init];
            if (self) {
                NSURLSessionConfiguration* config = [NSURLSessionConfiguration defaultSessionConfiguration];
                _session = [NSURLSession sessionWithConfiguration:config delegate:nil delegateQueue:nil];
                _endpoint = Endpoint;
            }
            return self;
        }

        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
                    andCompletion:(void(^)(NSError*))completion
        {
            [self tryToRegisterWithDeviceToken:token tags:tags retry:YES andCompletion:completion];
        }

        -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                    andCompletion:(void(^)(NSError*))completion
        {
            NSSet* tagsSet = tags?tags:[[NSSet alloc] init];

            NSString *deviceTokenString = [[token description]
                stringByTrimmingCharactersInSet:[NSCharacterSet characterSetWithCharactersInString:@"<>"]];
            deviceTokenString = [[deviceTokenString stringByReplacingOccurrencesOfString:@" " withString:@""]
                                    uppercaseString];

            [self retrieveOrRequestRegistrationIdWithDeviceToken: deviceTokenString
                completion:^(NSString* registrationId, NSError *error) {
                NSLog(@"regId: %@", registrationId);
                if (error) {
                    completion(error);
                    return;
                }

                [self upsertRegistrationWithRegistrationId:registrationId deviceToken:deviceTokenString
                    tags:tagsSet andCompletion:^(NSURLResponse * response, NSError *error) {
                    if (error) {
                        completion(error);
                        return;
                    }

                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if (httpResponse.statusCode == 200) {
                        completion(nil);
                    } else if (httpResponse.statusCode == 410 && retry) {
                        [self tryToRegisterWithDeviceToken:token tags:tags retry:NO andCompletion:completion];
                    } else {
                        NSLog(@"Registration error with response status: %ld", (long)httpResponse.statusCode);

                        completion([NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                    userInfo:nil]);
                    }

                }];
            }];
        }

        -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSData*)token
                    tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion
        {
            NSDictionary* deviceRegistration = @{@"Platform" : @"apns", @"Handle": token,
                                                    @"Tags": [tags allObjects]};
            NSData* jsonData = [NSJSONSerialization dataWithJSONObject:deviceRegistration
                                options:NSJSONWritingPrettyPrinted error:nil];

            NSLog(@"JSON registration: %@", [[NSString alloc] initWithData:jsonData
                                                encoding:NSUTF8StringEncoding]);

            NSString* endpoint = [NSString stringWithFormat:@"%@/api/register/%@", _endpoint,
                                    registrationId];
            NSURL* requestURL = [NSURL URLWithString:endpoint];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"PUT"];
            [request setHTTPBody:jsonData];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                    self.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];
            [request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];

            NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                if (!error)
                {
                    completion(response, error);
                }
                else
                {
                    NSLog(@"Error request: %@", error);
                    completion(nil, error);
                }
            }];
            [dataTask resume];
        }

        -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                    completion:(void(^)(NSString*, NSError*))completion
        {
            NSString* registrationId = [[NSUserDefaults standardUserDefaults]
                                        objectForKey:RegistrationIdLocalStorageKey];

            if (registrationId)
            {
                completion(registrationId, nil);
                return;
            }

            // request new one & save
            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/api/register?handle=%@",
                                    _endpoint, token]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"POST"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                    self.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSString* registrationId = [[NSString alloc] initWithData:data
                        encoding:NSUTF8StringEncoding];

                    // remove quotes
                    registrationId = [registrationId substringWithRange:NSMakeRange(1,
                                        [registrationId length]-2)];

                    [[NSUserDefaults standardUserDefaults] setObject:registrationId
                        forKey:RegistrationIdLocalStorageKey];
                    [[NSUserDefaults standardUserDefaults] synchronize];

                    completion(registrationId, nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                    userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        @end

    The code above implements the logic explained in the guidance article [Registering from your app backend](notification-hubs-registration-management.md#registration-management-from-a-backend) using NSURLSession to perform REST calls to your app backend, and NSUserDefaults to locally store the registrationId returned by the notification hub.

    Note that this class requires its property **authorizationHeader** to be set in order to work properly. This property is set by the **ViewController** class after the log in.

7. ViewController.h で、RegisterClient.h の `#import` ステートメントを追加し、 デバイス トークンの宣言と、`@interface` セクションの`RegisterClient` インスタンスに対する参照を追加します。

        #import "RegisterClient.h"

        @property (strong, nonatomic) NSData* deviceToken;
        @property (strong, nonatomic) RegisterClient* registerClient;

8. ViewController.m で、以下のように `@interface` セクションのプライベート メソッドの宣言を追加します。

        @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>

        // create the Authorization header to perform Basic authentication with your app back-end
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;

        @end

> [AZURE.NOTE] 次のスニペットは、安全な認証スキームではないの実装を置き換える必要があります、 **createAndSetAuthenticationHeaderWithUsername:AndPassword:** 、登録クライアント クラス、たとえば OAuth、Active Directory で使用される認証トークンを生成する特定の認証メカニズムでします。

9. 次に、ViewController.m の `@implementation` セクションに以下のコードを追加します。これは、デバイス トークンと認証ヘッダーに関する設定の実装を追加するものです。

        -(void) setDeviceToken: (NSData*) deviceToken
        {
            _deviceToken = deviceToken;
            self.LogInButton.enabled = YES;
        }

        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
        {
            NSString* headerValue = [NSString stringWithFormat:@"%@:%@", username, password];

            NSData* encodedData = [[headerValue dataUsingEncoding:NSUTF8StringEncoding] base64EncodedDataWithOptions:NSDataBase64EncodingEndLineWithCarriageReturn];

            self.registerClient.authenticationHeader = [[NSString alloc] initWithData:encodedData
                                                        encoding:NSUTF8StringEncoding];
        }

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }

    ログイン ボタンを有効にするデバイス トークンの設定方法に注意してください。 これは、ログイン アクションの一部として、View Controller がアプリケーション バックエンドでプッシュ通知を登録するためです。 そのため、デバイス トークンが適切に設定される前にユーザーがログイン ボタンを押すのを防ぐ必要があります。 プッシュ登録の前にログインが発生する場合には、プッシュ登録からログインを切り離す必要があります。

10. ViewController.m で、次のスニペットを使用してのアクション メソッドを実装する、 **Log In** ] ボタンをクリックし、ASP.NET バックエンドを使用して通知メッセージを送信する方法です。

        - (IBAction)LogInAction:(id)sender {
            // create authentication header and set it in register client
            NSString* username = self.UsernameField.text;
            NSString* password = self.PasswordField.text;

            [self createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];

            __weak ViewController* selfie = self;
            [self.registerClient registerWithDeviceToken:self.deviceToken tags:nil
                andCompletion:^(NSError* error) {
                if (!error) {
                    dispatch_async(dispatch_get_main_queue(),
                    ^{
                        selfie.SendNotificationButton.enabled = YES;
                        [self MessageBox:@"Success" message:@"Registered successfully!"];
                    });
                }
            }];
        }


        - (void)SendNotificationASPNETBackend:(NSString*)pns UsernameTag:(NSString*)usernameTag
                    Message:(NSString*)message
        {
            NSURLSession* session = [NSURLSession
                sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration] delegate:nil
                delegateQueue:nil];

            // Pass the pns and username tag as parameters with the REST URL to the ASP.NET backend
            NSURL* requestURL = [NSURL URLWithString:[NSString
                stringWithFormat:@"%@/api/notifications?pns=%@&to_tag=%@", BACKEND_ENDPOINT, pns,
                usernameTag]];

            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"POST"];

            // Get the mock authenticationheader from the register client
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                self.registerClient.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
            [request setHTTPBody:[message dataUsingEncoding:NSUTF8StringEncoding]];

            // Execute the send notification REST API on the ASP.NET Backend
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || httpResponse.statusCode != 200)
                {
                    NSString* status = [NSString stringWithFormat:@"Error Status for %@: %d\nError: %@\n",
                                        pns, httpResponse.statusCode, error];
                    dispatch_async(dispatch_get_main_queue(),
                    ^{
                        // Append text because all 3 PNS calls may also have information to view
                        [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];
                    });
                    NSLog(status);
                }

                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


11. アクションを更新し、 **通知の送信** ASP.NET バックエンドを使用し、スイッチで有効になっている任意の PNS に送信] ボタンをクリックします。


        - (IBAction)SendNotificationMessage:(id)sender
        {
            //[self SendNotificationRESTAPI];
            [self SendToEnabledPlatforms];
        }


        -(void)SendToEnabledPlatforms
        {
            NSString* json = [NSString stringWithFormat:@"\"%@\"",self.notificationMessage.text];

            [self.sendResults setText:@""];

            if ([self.WNSSwitch isOn])
                [self SendNotificationASPNETBackend:@"wns" UsernameTag:self.RecipientField.text Message:json];

            if ([self.GCMSwitch isOn])
                [self SendNotificationASPNETBackend:@"gcm" UsernameTag:self.RecipientField.text Message:json];

            if ([self.APNSSwitch isOn])
                [self SendNotificationASPNETBackend:@"apns" UsernameTag:self.RecipientField.text Message:json];
        }



11. 関数で **ViewDidLoad**, 、追加、RegisterClient インスタンスを作成するには、次をインスタンス化し、テキスト フィールドに対するデリゲートを設定します。

        self.UsernameField.delegate = self;
        self.PasswordField.delegate = self;
        self.RecipientField.delegate = self;
        self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];

12. [今すぐ **AppDelegate.m**, 、メソッドの内容をすべて削除 **アプリケーション: didRegisterForPushNotificationWithDeviceToken:** 、view controller に Apn から取得した最新のデバイス トークンが含まれているかどうかを確認するには、次に置き換えます。

        // Add import to the top of the file
        #import "ViewController.h"

        - (void)application:(UIApplication *)application
                    didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            ViewController* rvc = (ViewController*) self.window.rootViewController;
            rvc.deviceToken = deviceToken;
        }

13. 最後に [ **AppDelegate.m**, 、次のメソッドがあるかどうかを確認します。

        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

## アプリケーションをテストする

1. XCode を使用して、物理 iOS デバイスでアプリケーションを実行します (プッシュ通知はシミュレーターでは機能しません)。

2. iOS アプリケーションの UI で、ユーザー名とパスワードを入力します。 文字列は任意ですが、値は同じである必要があります。 クリックして **Log In**します。

    ![][2]


3. 登録の成功を通知するポップアップが表示されます。 Click **OK**.

    ![][3]

4. **Recipient username tag* テキスト フィールドに、別のデバイスから登録を使用したユーザー名のタグを入力します。
5. 通知メッセージを入力し、クリックして **通知の送信**します。  入力したタグが登録されているデバイスのみ、通知メッセージを受信します。  通知は、該当するユーザーにのみ送信されます。

    ![][4]


[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png

