<properties 
    pageTitle="モバイル サービスを使用した現在のユーザーのプッシュ通知への登録 | Microsoft Azure" 
    description="Azure Mobile Services により登録が実行されるときに、iOS アプリケーションで Azure Notification Hubs へのプッシュ通知登録を要求する方法について説明します。" 
    services="notification-hubs" 
    documentationCenter="ios" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ios" 
    ms.devlang="objective-c" 
    ms.topic="article" 
    ms.date="11/01/2015" 
    ms.author="yuaxu"/>

# モバイル サービスを使用した現在のユーザーのプッシュ通知への登録

> [AZURE.SELECTOR]
[Windows ストア C#](notification-hubs-windows-store-mobile-services-register-user-push-notifications.md)
[iOS](notification-hubs-ios-mobile-services-register-user-push-notifications.md)

このトピックでは、Azure Mobile Services により登録が行われる場合、Azure Notification Hubs でプッシュ通知登録を要求する方法について説明します。 このトピックは、このチュートリアルを拡張 [Notify users with Notification Hubs]します。 認証されたモバイル サービスを作成するには、このチュートリアルの必要な手順を既に完了している必要があります。 ユーザー通知シナリオの詳細については、次を参照してください。 [Notify users with Notification Hubs]します。  

1. Xcode で、前提条件のチュートリアルを完了したときに作成したプロジェクトの QSTodoService.h ファイルを開く [Get started with authentication], 、次の追加と **deviceToken** プロパティ。

        @property (nonatomic) NSData* deviceToken;

    このプロパティには、デバイス トークンが格納されます。

2. QSTodoService.m ファイルで次のコードを追加 **getDeviceTokenInHex** メソッド。

            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }

    このメソッドは、デバイス トークンを 16 進数の文字列値に変換します。

3. QSAppDelegate.m ファイルで、次のコード行を追加、 **didFinishLaunchingWithOptions** メソッド。

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];

    これにより、アプリケーションでプッシュ通知が有効になります。

4.  QSAppDelegate.m ファイルで、次のメソッドを追加します。

            - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
                [QSTodoService defaultService].deviceToken = deviceToken;
            }

    これにより、更新、 **deviceToken** プロパティです。

    > [AZURE.NOTE] この時点がないこと、他のコードでこのメソッドです。 呼び出しが既にある場合、 **registerNativeWithDeviceToken** メソッドを実行したときに追加された、 [Notification Hubs を使ってみる](/manage/services/notification-hubs/get-started-notification-hubs-ios/"%20target="_blank") チュートリアル, する必要がありますコメント アウトまたはその呼び出しを削除します。

5.  (省略可能) QSAppDelegate.m ファイルで、次のハンドラー メソッドを追加します。

            - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
                NSLog(@"%@", userInfo);
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      [[userInfo objectForKey:@"aps"] valueForKey:@"alert"] delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            }

    アプリケーションが実行中に通知を受信すると、このメソッドは UI にアラートを表示します。

6. QSTodoListViewController.m ファイルで、追加、 **registerForNotificationsWithBackEnd** メソッド。

            - (void)registerForNotificationsWithBackEnd {
                NSString* json = [NSString  stringWithFormat:@"{\"platform\":\"ios\", \"deviceToken\":\"%@\"}", [self.todoService getDeviceTokenInHex] ];

                [self.todoService.client invokeAPI:@"register_notifications" data:[json dataUsingEncoding:NSUTF8StringEncoding] HTTPMethod:@"POST" parameters:nil headers:nil completion:^(id result, NSHTTPURLResponse *response, NSError *error) {
                    if (error != nil) {
                        NSLog(@"Registration failed: %@", error);
                    } else {
                        // display UIAlert with successful login
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    }
                }];
            }

    このメソッドは、デバイス トークンを含む json ペイロードを作成します。 その後、モバイル サービスでカスタム API を呼び出して通知に登録します。 このメソッドは、プッシュ通知のデバイス トークンを作成し、Notification Hubs で登録を作成するカスタム API メソッドに、デバイスの種類と共に送信します。 このカスタム API が定義された [Notify users with Notification Hubs]します。

7.  最後に、 **viewDidAppear** メソッド呼び出しを追加してこの新しい **registerForNotificationsWithBackEnd** メソッド、ユーザーが正常に認証されると、次の例に示すように。

            - (void)viewDidAppear:(BOOL)animated
            {
                MSClient *client = self.todoService.client;

                if (client.currentUser != nil) {
                    return;
                }

                [client loginWithProvider:@"microsoftaccount" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                    [self refresh];
                    [self registerForNotificationsWithBackEnd];
                }];
            }

    > [AZURE.NOTE] これにより、登録が要求されるたびに、ページが読み込まれることを確認します。 アプリケーションでは、この登録が常に最新の状態となるように、定期的な登録のみ行うことができます。
    
これで、クライアント アプリケーションが更新されてに戻り、 [Notify users with Notification Hubs] Notification Hubs を使用して通知を送信するモバイル サービスを更新します。

<!-- Anchors. -->

<!-- Images. -->


<!-- URLs. -->
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios/

[Get Started with Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
 


