<properties
    pageTitle="Azure Notification Hubs の使用 (iOS アプリ) | Microsoft Azure"
    description="このチュートリアルでは、Azure Notification Hubs を使用して iOS アプリケーションにプッシュ通知を送信する方法について学習します。"
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
    ms.topic="hero-article"
    ms.date="12/15/2015"
    ms.author="wesmc"/>

# Notification Hubs の使用 (iOS アプリ)

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##概要

このチュートリアルでは、Azure Notification Hubs を使用して iOS アプリケーションにプッシュ通知を送信する方法について説明します。 Apple Push Notification サービス (APNs) を使用してプッシュ通知を受信する空の iOS アプリケーションを作成します。 完了すると、通知ハブを使用して、アプリケーションを実行するすべてのデバイスにプッシュ通知をブロードキャストできるようになります。

このチュートリアルでは、Notification Hubs を使用した簡単なブロードキャスト シナリオのデモンストレーションを行います。


## 開始する前に

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

このチュートリアルの完全なコードは GitHub でご覧 [ここ](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted)します。 

##前提条件

このチュートリアルには、次のものが必要です。

+ [Mobile Services iOS SDK バージョン 1.2.4]
+ [Xcode 7][Install Xcode]
+ iOS 8 (またはこれ以降のバージョン) に対応したデバイス
+ iOS Developer Program メンバーシップ

   > [AZURE.NOTE] プッシュ通知のための構成要件によりを展開して、iOS シミュレーターではなく iOS 対応デバイス (iPhone または iPad) プッシュ通知をテストします。

このチュートリアルを完了することは、iOS アプリケーションの他のすべての Notification Hubs チュートリアルの前提条件です。

> [AZURE.NOTE] このチュートリアルを完了するには、アクティブな Azure アカウントが必要です。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started)をご覧ください。

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

##通知ハブを構成する

このセクションでは、新しい通知ハブを作成して APN を使用すると、認証を構成するでは説明、 **.p12** プッシュ証明書を作成します。 既に作成した通知ハブを使用する場合は、手順 5. に進んでください。

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">
<li>
<p>クリックして、 <b>構成</b> 上部にあるタブをクリックして、 <b>アップロード</b> 証明書の拇印をアップロード、Apple 通知設定] ボタンをクリックします。 選択し、 <b>.p12</b> 前にエクスポートした証明書と証明書のパスワード。</p>
<p>選択するように <b>サンド ボックス</b> これは開発のためのモードです。 のみを使用して、 <b>運用</b> ストアからアプリケーションを購入したユーザーにプッシュ通知を送信する場合。</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-upload-cert.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png)



これで、通知ハブが APNS と連動するように構成されました。接続文字列にアプリケーションを登録し、通知を送信できます。

##通知ハブにアプリケーションを接続する

1. Xcode で、新しい iOS プロジェクトを作成し、選択、 **単一枠ビュー アプリケーション** テンプレートです。

    ![][8]

2. 新規プロジェクトのオプションを設定するときに必ず使用して同じ **製品名** と **組織 Id** 、Apple 開発者ポータルで以前にバンドル ID を設定するときに使用したことです。

    ![][11]

3. [ **ターゲット**, をプロジェクトの名前をクリックし、をクリックして、 **ビルド設定** タブをクリックし、展開 **Code Signing Identity**, 、し [ **デバッグ**, 、コード署名 id を設定します。 切り替え **レベル** から **基本的な** に **すべて**, 、設定と **Provisioning Profile** 以前に作成したプロビジョニング プロファイルにします。

    Xcode で作成した新しいプロビジョニング プロファイルが表示されない場合は、署名 ID のプロファイルを更新してみてください。 をクリックして **Xcode** メニュー バーをクリックして **の基本設定**, をクリックして、 **アカウント** ] タブをクリックして、 **の詳細を表示** ボタンをクリックして、署名 id] をクリックし、右下隅にある [更新] ボタンをクリックします。

    ![][9]

4. ダウンロード、 [Mobile Services iOS SDK version 1.2.4] ファイルを解凍します。 Xcode で、プロジェクトを右クリックし、をクリックして、 **Add Files to** を追加するオプション、 **WindowsAzureMessaging.framework** 、Xcode プロジェクトにフォルダーです。 選択 **ために必要な場合は、項目をコピー**, 、クリックして **追加**します。

    >[AZURE.NOTE] 通知ハブ SDK がサポートされていません bitcode Xcode 7。  設定する必要があります **を有効にする Bitcode** に **いいえ** で、 **ビルド オプション** 、プロジェクトのです。

    ![][10]

5. 新しいヘッダー ファイルを同じ名前のプロジェクトに追加 **HubInfo.h**します。 このファイルに通知ハブの定数が保存されます。  次の定義を追加すると文字列リテラルのプレース ホルダーを置き換えます、 *ハブ名* と *DefaultListenSharedAccessSignature* 前にメモしました。

        #ifndef HubInfo_h
        #define HubInfo_h
        
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
        
        #endif /* HubInfo_h */

6. AppDelegate.h ファイルを開き、次の import ディレクティブを追加します。

         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
        
7. AppDelegate.m ファイルで、iOS のバージョンに基づいて `didFinishLaunchingWithOptions` メソッド内に次のコードを追加します。 このコードにより、APNs にデバイス ハンドルが登録されます。

    iOS 8 の場合:

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    iOS 8 より前のバージョンの場合:

         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];


8. 同じファイルで、次のメソッドを追加します。 このコードは、HubInfo.h に指定した接続情報を使用して通知ハブに接続します。 その後、通知ハブが通知を送信できるように、通知ハブにデバイス トークンを指定します。

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];

            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }


9. 同じファイルで表示するには、次のメソッドを追加、 **UIAlert** 、アプリケーションがアクティブなときに通知が受信した場合。


        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

10. お使いのデバイスでアプリケーションをビルドして実行し、エラーがないことを確認します。

## 通知を送信する


通知を送信することによって、アプリで通知の受信をテストする、 [Azure Classic Portal] 通知ハブで、次の画面に示すように、[デバッグ] タブを使用しています。

![][30]

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]



## (省略可能) アプリから通知を送信する

アプリ内部での通知の送信が必要になる場合もあります。 このセクションでは、REST インターフェイスを使用してそれを実現する方法の例を示します。

1. Xcode で Main.storyboard を開き、次の UI コンポーネントをオブジェクト ライブラリから追加してユーザーがアプリでプッシュ通知を送信できるようにします。

    - ラベル テキストなしのラベル。 これは、通知の送信時にエラーをレポートするために使用されます。  **行** にプロパティを設定する必要があります **0** サイズを右と左右の余白とビューの上部に制限を自動的にできるようにします。
    - テキスト フィールド **プレース ホルダー** テキストに設定 **Enter Notification Message**します。 次に示すように、ラベルのすぐ下のフィールドを制限します。 ビュー コントローラーをアウトレット デリゲートに設定します。
    - というタイトルのボタン **通知の送信** テキスト フィールドのすぐ下と水平方向の中央に制限します。

    ビューは次のようになります。

    ![][32]


2. [アウトレットが追加](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) ラベルとテキストのフィールドを表示します] に接続し、更新、 `interface` をサポートする定義 `UITextFieldDelegate` と `NSXMLParserDelegate`です。 以下に示す 3 つのプロパティ宣言を追加し、REST API の呼び出しと応答の解析をサポートします。

    ViewController.h ファイルは次のようになります。

        #import <UIKit/UIKit.h>

        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }

        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;

        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;

        @end

3. HubInfo.h を開いて、次の定数を追加します。これらの定数は、ハブに通知を送信する際に使用されます。 実際のプレース ホルダーの文字列リテラルを置き換える *DefaultFullSharedAccessSignature* 接続文字列。

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"

4. ViewController.h ファイルに次の `#import` ステートメントを追加します。

        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"

5. ViewController.m で、インターフェイスの実装に次のコードを追加します。 このコードは、解析、 *DefaultFullSharedAccessSignature* 接続文字列。 説明したように、 [REST API リファレンス](http://msdn.microsoft.com/library/azure/dn495627.aspx), 、解析されたこの情報は、SaS トークンの生成に使用は、 **承認** 要求ヘッダー。

        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;

        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;

            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];

                @throw parseException;
            }

            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }

6. ViewController.m で、`viewDidLoad` メソッドを更新してビューの読み込み時に接続文字列を解析します。 また、以下に示したユーティリティ メソッドをインターフェイスの実装に追加します。  


        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





7. ViewController.m で、次のコードを追加で提供される SaS 認証トークンを生成するインターフェイスの実装を **承認** で説明したように、ヘッダー、 [REST API リファレンス](http://msdn.microsoft.com/library/azure/dn495627.aspx)します。

        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;

            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];

                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];

                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }

            return token;
        }


8. Ctrl キーを押しからドラッグ、 **通知の送信** という名前のアクションを追加する viewcontroller.m ボタン **SendNotificationMessage** の **タッチダウン**します。 REST API を使用して通知を送信するように次のコードでメソッドを更新します。

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }

        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];

            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];

            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];

            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
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


9. ViewController.m で、テキスト フィールドのキーボードを閉じる動作をサポートする次のデリゲート メソッドを追加します。 Ctrl キーを押しながらテキスト フィールドからインターフェイス デザイナーの View Controller アイコンにドラッグし、ビュー コントローラーをアウトレット デリゲートとして設定します。

        //===[ Implement UITextFieldDelegate methods ]===

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }


10. ViewController.m で、`NSXMLParser` を使用した応答の解析をサポートする次のデリゲート メソッドを追加します。

        //===[ Implement NSXMLParserDelegate methods ]===

        -(void)parserDidStartDocument:(NSXMLParser *)parser
        {
            self.statusResult = @"";
        }

        -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
            namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
            attributes:(NSDictionary *)attributeDict
        {
            NSString * element = [elementName lowercaseString];
            NSLog(@"*** New element parsed : %@ ***",element);

            if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
            {
                self.currentElement = element;
            }
        }

        -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
        {
            self.statusResult = [self.statusResult stringByAppendingString:
                [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
        }

        -(void)parserDidEndDocument:(NSXMLParser *)parser
        {
            // Set the status label text on the UI thread
            dispatch_async(dispatch_get_main_queue(),
            ^{
                [self.sendResults setText:self.statusResult];
            });
        }



11. プロジェクトをビルドし、エラーがないことを確認します。


> [AZURE.NOTE] Bitcode サポートに関する Xcode7 でビルド エラーが発生した場合は、[ビルド設定] を変更する必要があります"Bitcode"(ENABLE_BITCODE) Xcode では、[いいえ] を有効に -> します。 Notification Hubs SDK は、現在ビットコードをサポートしていません。 

Apple ですべての考えられる通知ペイロードが見つかります [Local and Push Notification Programming Guide]します。


##アプリケーションをテストする

iOS でプッシュ通知をテストするには、デバイスにアプリケーションをデプロイする必要があります。 iOS シミュレーターを使用して Apple のプッシュ通知を送信することはできません。

1. アプリケーションを実行し、登録が成功したことを確認し、キーを押します **OK**します。

    ![][33]

2. テスト通知を送信する、 [Azure Classic Portal]します。 アプリ内で通知を送信するためのコードを追加した場合は、テキスト フィールドの内部をタッチして、通知メッセージを入力します。 キーを押します、 **送信** キーボードのキーまたは **通知の送信** 通知メッセージを送信するには、ビューでボタンをクリックします。

    ![][34]

3. 通知を受信するように登録されているすべてのデバイスに通知が送信されます。

    ![][35]

このチュートリアルを向上させるために問題を指摘または推奨事項を提案するには、以下のディスカッション セクションにコメントを残してください。


##次のステップ

この簡単な例では、すべての iOS デバイスに通知をブロードキャストします。 続行すると、学習の次の手順としてお勧めします、 [Use Notification Hubs to push notifications to users] チュートリアルです。 このチュートリアルでは、タグを使用して通知を送信するためにバックエンドを作成する手順を説明します。 

対象グループごとにユーザーをセグメント化する場合は、行うことができますさらに [Use Notification Hubs to send breaking news]します。 

通知ハブの概要については、次を参照してください。 [Notification Hubs Guidance]します。



<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-debug-hub-ios.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobile Services iOS SDK version 1.2.4]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-ios-notify-users.md
[Use Notification Hubs to send breaking news]: notification-hubs-ios-send-breaking-news.md

[Local and Push Notification Programming Guide]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1


