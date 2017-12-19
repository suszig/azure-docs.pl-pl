---
title: "Powiadomienie koncentratory zlokalizowane fundamentalne wiadomości samouczek dla systemu iOS"
description: "Dowiedz się, jak używać usługi Notification Hubs Azure Service Bus do wysyłania powiadomień wiadomości zlokalizowanych podziału (iOS)."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: fd2b7d9dfd4f432bbcbaa3ed76f8bec0b9677e17
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>Wyślij zlokalizowanej najnowszych wiadomości do urządzeń z systemem iOS przy użyciu usługi Notification Hubs
> [!div class="op_single_selector"]
> * [Sklep Windows C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Omówienie
W tym temacie przedstawiono sposób użycia [szablony](notification-hubs-templates-cross-platform-push-messages.md) funkcji usługi Azure Notification Hubs wysyłać powiadomienia o najważniejszych wiadomościach które zostały zlokalizowane według języka i urządzenia. W tym samouczku początkowo utworzony w aplikacji systemu iOS [użyciu usługi Notification Hubs wysyłanie najważniejszych wiadomości]. Po zakończeniu będzie można zarejestrować dla kategorii, które planuje się, określ język, w których chcesz otrzymywać powiadomienia, a otrzymywać tylko powiadomień wypychanych do wybranych kategorii w tym języku.

Istnieją dwie części tego scenariusza:

* Aplikacja systemu iOS umożliwia klienta urządzeń do określania języka i subskrybować podziału różne kategorie nowości;
* zaplecza emituje powiadomień, przy użyciu **tag** i **szablonu** feautres usługi Azure Notification Hubs.

## <a name="prerequisites"></a>Wymagania wstępne
Należy zostały już wykonane [użyciu usługi Notification Hubs wysyłanie najważniejszych wiadomości] samouczka i mieć kod dostępne, ponieważ w tym samouczku bezpośrednio oparta na kod.

Visual Studio 2012 lub nowszy jest opcjonalna.

## <a name="template-concepts"></a>Pojęcia dotyczące szablonów
W [użyciu usługi Notification Hubs wysyłanie najważniejszych wiadomości] utworzono aplikację, która jest używana **tagi** do subskrybowania powiadomień dla wiadomości różnych kategorii.
Wiele aplikacji, jednak docelowy kilku rynkach i wymagają lokalizacji. Oznacza to, że zawartość powiadomień, się muszą być lokalizowany i dostarczane do odpowiednich zestawów urządzeń.
W tym temacie pokazano, jak używać **szablonu** funkcji usługi Notification Hubs łatwe udostępnianie powiadomienia o zlokalizowanych najważniejszych wiadomościach.

Uwaga: jeden sposób wysyłania powiadomień zlokalizowanych jest utworzenie wielu wersji każdego znacznika. Na przykład do obsługi w języku angielskim, francuskim i mandaryński, czy potrzebujemy trzy różne znaczniki dla wiadomości world: "world_en", "world_fr" i "world_ch". Firma Microsoft następnie musi wysłać zlokalizowanej wersji wiadomości world do każdego z tych tagów. W tym temacie używamy szablonów w celu uniknięcia mnożenie tagów i wymaganie wysyła wiele komunikatów.

Na wysokim poziomie szablony są sposób, aby określić, jak powinno zostać odebrane powiadomienie określonego urządzenia. Szablon określa format ładunku dokładne odwołując się do właściwości, które są częścią komunikat wysłany przez zaplecze Twojej aplikacji. W tym przypadku firma Microsoft wyśle komunikat niezależny od ustawień regionalnych zawierający wszystkie obsługiwane języki:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Następnie firma Microsoft zapewnia urządzenia Zarejestruj szablon, który odwołuje się do właściwości poprawne. Na przykład aplikację systemu iOS, która potrzebuje zarejestrować dla wiadomości francuskim zarejestruje następujące czynności:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Szablony są bardzo przydatna funkcja, możesz dowiedzieć się więcej w naszym [szablony](notification-hubs-templates-cross-platform-push-messages.md) artykułu.

## <a name="the-app-user-interface"></a>Interfejs użytkownika aplikacji
Firma Microsoft będzie teraz zmodyfikować aplikację fundamentalne wiadomości utworzonego w temacie [użyciu usługi Notification Hubs wysyłanie najważniejszych wiadomości] wysłać zlokalizowane fundamentalne wiadomości za pomocą szablonów.

W Twojej MainStoryboard_iPhone.storyboard dodać kontrolkę segmentu z trzech językach, które firma Microsoft będzie obsługiwać: angielski, francuski i mandaryński.

![][13]

Następnie upewnij się dodać IBOutlet w Twojej ViewController.h, jak pokazano poniżej:

![][14]

## <a name="building-the-ios-app"></a>Tworzenie aplikacji dla systemu iOS
1. W Twojej Notification.h dodać *retrieveLocale* metody i zmodyfikować magazynu oraz subskrybowania metody, jak pokazano poniżej:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    W Twojej Notification.m zmodyfikować *storeCategoriesAndSubscribe* metody, dodając parametr ustawień regionalnych i przechowywanie ich w domyślnych ustawień użytkownika:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
   
    Następnie zmodyfikuj *subskrypcji* metodę w celu uwzględnienia ustawień regionalnych:
   
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
   
    Należy zwrócić uwagę, jak firma Microsoft są teraz przy użyciu metody *registerTemplateWithDeviceToken*, zamiast *registerNativeWithDeviceToken*. Gdy firma Microsoft rejestrowania dla szablonu musimy udostępnić szablonu json, a także nazwę dla szablonu (zgodnie z naszej aplikacji może być do rejestrowania różnych szablonów). Upewnij się zarejestrować kategorie jako tagi, ponieważ chcemy upewnić się otrzymać notifciations dla tych wiadomości.
   
    Dodaj metodę można pobrać ustawień regionalnych z domyślnych ustawień użytkownika:
   
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
2. Teraz, gdy firma Microsoft zmodyfikowane klasy Nasze powiadomienia, mamy upewnij się, że nasze ViewController korzysta z nowych UISegmentControl. Dodaj następujący wiersz w *viewDidLoad* metody, aby upewnić się wyświetlić aktualnie wybranego ustawienia regionalne:
   
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
   
    Następnie w Twojej *subskrypcji* metody, zmień wywołania do *storeCategoriesAndSubscribe* do następującego:
   
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
3. Na koniec należy zaktualizować *didRegisterForRemoteNotificationsWithDeviceToken* metody w Twojej AppDelegate.m, dzięki czemu można poprawnie odświeżyć rejestracji podczas uruchamiania aplikacji. Zmień wywołania do *subskrypcji* metody powiadomienia z następujących czynności:
   
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a>(opcjonalnie) Wysyłanie powiadomień zlokalizowanego szablonu z aplikacji konsoli .NET.
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-the-device"></a>(opcjonalnie) Wysyłanie powiadomień zlokalizowanego szablonu z urządzenia
Jeśli nie masz dostępu do programu Visual Studio lub po prostu chcesz przetestować wysyłanie powiadomień zlokalizowanego szablonu bezpośrednio z aplikacji na urządzeniu.  Można w prosty dodać parametry szablonu zlokalizowanych `SendNotificationRESTAPI` metody zdefiniowanej w poprzednich instrukcji.

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




## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat używania szablonów zobacz:

* [Powiadom użytkowników z usługą Notification Hubs: ASP.NET]
* [Powiadom użytkowników z usługą Notification Hubs: Mobile Services]

<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[użyciu usługi Notification Hubs wysyłanie najważniejszych wiadomości]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Powiadom użytkowników z usługą Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Powiadom użytkowników z usługą Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
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
