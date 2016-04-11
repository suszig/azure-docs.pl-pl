<properties
    pageTitle="Notification Hubs ローカライズ ニュース速報のチュートリアル (iOS 向け)"
    description="Azure Service Bus Notification Hubs を使用して、ローカライズしたニュース速報通知を送信する方法を説明します (iOS)。"
    services="notification-hubs"
    documentationCenter="ios"
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

# Notification Hubs を使用した iOS デバイスへのローカライズ ニュース速報の送信

> [AZURE.SELECTOR]
- [Windows ストア C#](notification-hubs-windows-store-dotnet-send-localized-breaking-news)
- [iOS](notification-hubs-ios-send-localized-breaking-news)


##概要

このトピックでは、使用する方法、 [テンプレート](notification-hubs-templates.md) を言語およびデバイスごとにローカライズしたニュース速報通知をブロードキャストする、Azure Notification Hubs の機能です。 作成した iOS アプリケーションを開始するこのチュートリアルで [Use Notification Hubs to send breaking news]します。 完了すると、興味のあるニュース速報カテゴリに登録して受信する通知の言語を指定し、選択したカテゴリのその言語のプッシュ通知だけを受信できるようになります。


このシナリオは次の 2 つに分けられます。

- iOS アプリケーションで、クライアント デバイスが言語を指定し、さまざまなニュース速報カテゴリを購読できるようになります。

- バック エンドを使用して、通知がブロードキャスト、 **タグ** と **テンプレート** Azure Notification Hubs の機能です。



##前提条件

既に完了する必要があります、 [Use Notification Hubs to send breaking news] チュートリアルこのチュートリアルは、そのコードに基づいているため、使用可能なコードがあるとします。

必要に応じて Visual Studio 2012 以降を使用できます。



##テンプレートの概念

 [Use Notification Hubs to send breaking news] ために使用するアプリケーションをビルドした **タグ** さまざまなニュース カテゴリの通知をサブスクライブします。
しかし、多くのアプリケーションは複数の市場をターゲットとしており、ローカライズが必要です。 これは、通知自体の内容をローカライズし、適切なデバイス セットに配信する必要があることを意味します。
このトピックを使用する方法を説明して、 **テンプレート** したローカライズ ニュースの通知を簡単に配信する Notification Hubs の機能です。

注: ローカライズした通知を送信する 1 つの方法として、各タグの複数のバージョンを作成する方法があります。 たとえば、ワールド ニュースで英語、フランス語、標準中国語をサポートするには、3 つのタグ ("world_en"、"world_fr"、"world_ch") が必要です。 その後、ワールド ニュースのローカライズ バージョンをこれらの各タグに送信する必要があります。 このトピックでは、テンプレートを使用することでタグの増加を抑制し、複数のメッセージを送信しなくてもよいようにします。

大まかに言えば、テンプレートとは、特定のデバイスが通知をどのように受信するかを特定するための手段です。 テンプレートは、アプリケーション バックエンドにより送信されるメッセージの一部となっているプロパティを参照することで、正確なペイロードを指定します。 このケースでは、サポートされるすべての言語を含む、ロケールにとらわれないメッセージを送信します。

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

次に、デバイスが、適切なプロパティを参照するテンプレートを登録するようにします。 たとえば、フランス語ニュースを登録する必要がある iOS アプリケーションは、次は登録します。

    {
        aps:{
            alert: "$(News_French)"
        }
    }

テンプレートは非常に強力な機能についての詳細で、 [テンプレート](notification-hubs-templates.md) 記事です。

##アプリケーションのユーザー インターフェイス

トピックで作成したニュース速報アプリケーションを今すぐ変更 [Use Notification Hubs to send breaking news] ローカライズされたテンプレートを使用したニュース速報を送信します。


MainStoryboard_iPhone.storyboard で、サポートする 3 つの言語 (英語、フランス語、標準中国語) を備えた Segmented Control を追加します。

![][13]

次に、以下に示すように ViewController.h に IBOutlet を追加します。

![][14]

##iOS アプリケーションを構築する


1. Notification.h で、追加、 *retrieveLocale* メソッド、およびストアを変更し、次のように、subscribe メソッド。

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

        - (NSSet*) retrieveCategories;

        - (int) retrieveLocale;

    Notification.m で変更、 *storeCategoriesAndSubscribe* ロケール パラメーターを追加して、ユーザーの既定値に格納することによって、メソッド。

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];

            [self subscribeWithLocale: locale categories:categories completion:completion];
        }

    変更して、 *サブスクライブ* 、ロケールに含めるメソッド。

        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }

            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }

    ここではメソッドを使用して方法に注意してください *ように*, の代わりに *registerNativeWithDeviceToken*します。 テンプレートを登録するときは、json テンプレートに加えて、テンプレートの名前も指定する必要があります (アプリケーションにより複数のテンプレートが登録されることがあるため)。 それらのニュースの通知を確実に受信できるようにするため、カテゴリをタグとして登録してください。

    ユーザーの既定の設定からロケールを取得するメソッドを追加します。

        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            int locale = [defaults integerForKey:@"BreakingNewsLocale"];

            return locale < 0?0:locale;
        }

2. ここまでで、Notifications クラスを変更したので、ViewController が新しい UISegmentControl を使用することを確認する必要があります。 次の行を追加し、 *viewDidLoad* メソッドを現在選択されているロケールが表示されることを確認します。

        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

    その後、[、 *サブスクライブ* メソッドへの呼び出しを変更する、 *storeCategoriesAndSubscribe* 次。

        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

3. 更新する必要が最後に、 *didRegisterForRemoteNotificationsWithDeviceToken* メソッド、AppDelegate.m で、アプリケーションの起動時に登録を正しく更新できるようにします。 呼び出しを変更する、 *サブスクライブ* 通知を次のメソッド。

        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

##(省略可能) ローカライズされたテンプレート通知を .NET コンソール アプリケーションから送信します。

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]



##(省略可能) ローカライズされたテンプレート通知をデバイスから送信する

Visual Studio にアクセスできない場合でも、ローカライズされたテンプレート通知をデバイス上のアプリから直接送信することをテストできます。  単に、前のチュートリアルで定義した `SendNotificationRESTAPI` メソッドにローカライズされたテンプレートのパラメーターを追加できます。

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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

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




## 次のステップ

テンプレートの使用方法の詳細については、以下を参照してください。

- [通知ハブによるユーザーへの通知: ASP.NET]
- [通知ハブによるモバイル サービスのユーザーへの通知: モバイル サービス]






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx


