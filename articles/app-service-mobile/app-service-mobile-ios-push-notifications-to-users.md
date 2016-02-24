<properties 
    pageTitle="iOS で特定のユーザーにクロスプラット フォーム通知を送信する" 
    description="特定のユーザーのすべてのデバイスにプッシュ通知を送信する方法について説明します。"
    services="app-service\mobile,notification-hubs" 
    documentationCenter="ios" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-mobile" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-dotnet" 
    ms.devlang="objective-c" 
    ms.topic="article" 
    ms.date="11/17/2015"
    ms.author="yuaxu"/>

# 特定のユーザーにクロスプラット フォーム通知を送信する

[AZURE.INCLUDE [app-service-mobile-selector-push-users](../../includes/app-service-mobile-selector-push-users.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

このトピックでは、特定のユーザーのすべての登録済みデバイスにモバイル バックエンドから通知を送信する方法について説明します。 [テンプレート] のクライアント アプリケーションを自由にペイロードの形式と変数プレース ホルダーの登録時に指定するようになる概念が導入されました。 送信すると、これらのプレースホルダーにより、すべてのプラットフォームにヒットし、クロスプラットフォーム通知が有効になります。

> [AZURE.NOTE] クロスプラット フォーム クライアントでプッシュ操作を取得するには有効にするには、各プラットフォームに対してこのチュートリアルを完了する必要があります。 実行する必要がありますのみ、 [モバイル バックエンドの更新](#backend) 同じモバイル バックエンドを共有しているクライアントに 1 回です。
 
##前提条件 

このチュートリアルを開始する前に、作業する各クライアント プラットフォームで次の App Service チュートリアルが既に完了している必要があります。

+ [認証の使用の開始を取得]<br/>TodoList サンプル アプリケーションにログイン要件を追加します。

+ [開始プッシュ通知の使用]<br/>プッシュ通知用に TodoList サンプル アプリを構成します。

##<a name="client"></a>クライアントを更新して、クロスプラットフォームのプッシュを処理するためのテンプレートを登録する

1. APNs 登録スニペットで移動 **QSAppDelegate.m**の **Application:didfinishlaunchingwithoptions:** への呼び出しに **loginWithProvider** で **QSTodoListViewController.m** は、認証の完了後に行われます。

        [client loginWithProvider:@"facebook" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
            [self refresh];
            
            // register iOS8 or previous devices for notifications
            if ([[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)]) {
                [[UIApplication sharedApplication] registerForRemoteNotifications];
            }
            else {
                // Register for remote notifications
                [[UIApplication sharedApplication] registerForRemoteNotificationTypes:
                 UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
            }
        }];

2. 置き換える、 **registerDeviceToken** で呼び出す **アプリケーション: didRegisterForRemoteNotificationsWithDeviceToken** テンプレートを使用するには、次のようにします。

        NSDictionary *templates = @{
                               @"testNotificationTemplate": @{ @"body" : @{ @"aps" : @{ @"alert": @"$(message)" } } }
                               };
    
        // register with templates
        [client.push registerDeviceToken:deviceToken template:templates completion:^(NSError *error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

    プッシュ通知に登録する前にユーザーを認証する必要があります。 認証されたユーザーがプッシュ通知を登録するとき、ユーザー ID を持つタグが自動的に追加されます。

##<a name="backend"></a>特定のユーザーに通知を送信するようにサービス バックエンドを更新する

[AZURE.INCLUDE [app-service-mobile-push-notifications-to-users](../../includes/app-service-mobile-push-notifications-to-users.md)]

##<a name="test"></a>アプリケーションをテストする

モバイル バックエンド プロジェクトを再発行し、設定してあるクライアント アプリケーションのいずれかを実行します。 項目を挿入すると、バックエンドにより、ユーザーがログインしているすべてのクライアント アプリケーションに通知が送信されます。

<!-- URLs. -->
[Get started with authentication]: app-service-mobile-ios-get-started-users.md
[Get started with push notifications]: app-service-mobile-ios-get-started-push.md
[templates]: https://msdn.microsoft.com/library/dn530748.aspx
 
