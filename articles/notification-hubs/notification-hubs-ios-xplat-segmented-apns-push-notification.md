---
title: "Centra powiadomień w najważniejszych wiadomości Samouczek — systemu iOS"
description: "Dowiedz się, jak używać usługi Azure Service Bus Notification Hubs można wysłać powiadomienia o najważniejszych wiadomościach do urządzeń z systemem iOS."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 6ead4169-deff-4947-858c-8c6cf03cc3b2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 8aec171b46df3e0e7f2a2d3cc9d44084d064e6fd
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Omówienie
W tym temacie przedstawiono sposób użycia usługi Azure Notification Hubs wysyłać powiadomienia o najważniejszych wiadomościach do aplikacji systemu iOS. Po zakończeniu będzie można rejestrować zakłócenia w kategorii wiadomości i odbierać tylko powiadomienia wypychane dla tych kategorii. Ten scenariusz jest wspólnego wzorca dla wielu aplikacji, gdzie powiadomienia muszą być wysłane do grup użytkowników, które wcześniej zostały zadeklarowane zainteresowanie je, np. czytnik danych RSS, aplikacje wentylatory utworów muzycznych itp.

Scenariusze emisji są włączone, umieszczając w niej co najmniej jeden *tagi* podczas tworzenia rejestracji w Centrum powiadomień. Gdy powiadomienia są wysyłane do tagu, wszystkie urządzenia, które zostały zarejestrowane dla tagu będą otrzymywać powiadomienia. Ponieważ tagi są po prostu ciągów, nie mają być przygotowana z wyprzedzeniem. Aby uzyskać więcej informacji na temat tagów, zapoznaj się [routingu centra powiadomień i wyrażeń tagów](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Wymagania wstępne
W tym temacie opiera się na aplikacji utworzony w [Rozpoczynanie pracy z usługą Notification Hubs][get-started]. Przed rozpoczęciem tego samouczka należy zostały już wykonane [Rozpoczynanie pracy z usługą Notification Hubs][get-started].

## <a name="add-category-selection-to-the-app"></a>Dodaj wybrane kategorii do aplikacji
Pierwszym krokiem jest dodanie elementów interfejsu użytkownika do Twoje istniejące scenorysu, który umożliwia użytkownikowi wybierz kategorie, aby zarejestrować. Kategorie wybrane przez użytkownika są przechowywane na urządzeniu. Po uruchomieniu aplikacji, rejestracji urządzenia jest tworzony w Centrum powiadomień z wybranych kategorii jako znaczniki.

1. W Twojej MainStoryboard_iPhone.storyboard Dodaj poniższe składniki z biblioteki obiektów:
   
   * Etykiety z tekstem "Krytyczne wiadomości"
   * Etykiety z teksty kategorii "World", "Polityka", "Business", "Technologii", "Nauki", "Sport",
   * Sześć przełączników, jeden dla każdej kategorii, ustaw każdy przełącznik **stanu** jako **poza** domyślnie.
   * Jeden z przycisków z etykietą "Subskrybuj"
     
     Twoje scenorysu powinna wyglądać następująco:
     
     ![][3]
2. W edytorze Asystenta utworzyć gniazda dla wszystkich przełączników i połączeń telefonicznych z nimi "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"
3. Utwórz działanie dla przycisk o nazwie "subskrypcji". Twoje ViewController.h powinien zawierać następujące informacje:
   
        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;
   
        - (IBAction)subscribe:(id)sender;
4. Utwórz nową **klasy Touch Cocoa** o nazwie `Notifications`. Skopiuj następujący kod w sekcji interfejsu pliku Notifications.h:
   
        @property NSData* deviceToken;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;
   
        - (NSSet*)retrieveCategories;
   
        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
5. Dodaj następujące dyrektywy importu do Notifications.m:
   
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
6. Skopiuj następujący kod w sekcji implementacji pliku Notifications.m.
   
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



    Ta klasa korzysta z magazynu lokalnego do przechowywania i pobierania kategorii wiadomości, który otrzyma tego urządzenia. Ponadto zawiera metody do rejestrowania dla tych kategorii przy użyciu [szablonu](notification-hubs-templates-cross-platform-push-messages.md) rejestracji.

1. W pliku AppDelegate.h Dodaj instrukcję import Notifications.h i Dodaj właściwość do wystąpienia klasy powiadomień:
   
        #import "Notifications.h"
   
        @property (nonatomic) Notifications* notifications;
2. W **didFinishLaunchingWithOptions** metody w AppDelegate.m, Dodaj kod, aby zainicjować wystąpienia powiadomienia na początku metody.  
   
    `HUBNAME`i `HUBLISTENACCESS` (zdefiniowany w pliku hubinfo.h) ma już `<hub name>` i `<connection string with listen access>` zastąpione symbole zastępcze nazwą Centrum powiadomień i parametry połączenia dla *DefaultListenSharedAccessSignature* uzyskany wcześniej
   
        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
   
   > [!NOTE]
   > Ponieważ poświadczenia, które są dystrybuowane wraz z aplikacji przez klienta nie są zazwyczaj bezpieczna, klucz dostępu do nasłuchiwania należy dystrybuować tylko z aplikacji klienta. Nasłuchiwanie umożliwia dostęp, nie można zmodyfikować aplikację, aby zarejestrować dla powiadomień, ale istniejące rejestracje i nie mogą być wysyłane powiadomienia. Klucz pełny dostęp jest używany w usłudze zabezpieczonych wewnętrznej bazy danych do wysyłania powiadomień i zmianę istniejącego rejestracji.
   > 
   > 
3. W **didRegisterForRemoteNotificationsWithDeviceToken** metody w AppDelegate.m, Zastąp kod w metodzie następujący kod, aby przekazać token urządzenia do klasy powiadomienia. Klasa powiadomienia będzie wykonywać rejestrowanie na potrzeby powiadomień z kategorii. Jeśli użytkownik zmieni ułatwia wybieranie kategorii, nazywamy `subscribeWithCategories` metody w odpowiedzi na **subskrypcji** przycisk, aby je aktualizować.
   
   > [!NOTE]
   > Ponieważ token urządzenia przypisane przez Notification Service (APNS, Apple Push) szansy można w dowolnym momencie, należy zarejestrować powiadomień często uniknąć niepowodzeń powiadomień. W tym przykładzie rejestruje powiadomienia każdym uruchomieniu aplikacji. Dla aplikacji, które są uruchamiane często więcej niż raz dziennie, można prawdopodobnie pominąć rejestrację, aby zachować przepustowość, jeśli krótszy niż doba upłynął od czasu poprzedniej rejestracji.
   > 
   > 
   
        self.notifications.deviceToken = deviceToken;
   
        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.
   
        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

    Należy pamiętać, że w tym momencie nie powinno być nie innych kodu w **didRegisterForRemoteNotificationsWithDeviceToken** metody.

1. Następujące metody już powinien znajdować się w AppDelegate.m ukończenie [Rozpoczynanie pracy z usługą Notification Hubs] [ get-started] samouczka.  Jeśli nie, dodaj je.
   
    -(void) MessageBox:(NSString *) tytułu wiadomości:(NSString *) messageText {
   
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
        [alert show];
    }
   
   * didReceiveRemoteNotification aplikacji:(UIApplication *) aplikacji (void): (NSDictionary *) informacje o użytkowniku {NSLog (@"% @", informacje o użytkowniku);   [self MessageBox:@"Notification" wiadomości: [[informacje o użytkowniku objectForKey:@"aps"] valueForKey:@"alert"]];}
   
   Ta metoda obsługuje powiadomienia otrzymane, gdy aplikacja jest uruchomiona, wyświetlając prostą **UIAlert**.
2. W ViewController.m, Dodaj instrukcję import AppDelegate.h i skopiuj następujący kod do generowanych XCode **subskrypcji** metody. Ten kod zaktualizuje rejestracji powiadomień do używania nowych tagów kategorii, którą użytkownik wybrał w interfejsie użytkownika.
   
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
   
   Ta metoda tworzy **NSMutableArray** kategorii i używa **powiadomienia** klasę do przechowywania listy w lokalnej pamięci masowej i rejestruje odpowiednie znaczniki w Centrum powiadomień. Zmiana kategorii rejestracji zostaje odtworzone w nowej kategorii.
3. W ViewController.m, Dodaj następujący kod w **viewDidLoad** metodę w celu ustawienia interfejsu użytkownika na podstawie wcześniej zapisany kategorii.

        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



Aplikacja może teraz przechowywać zestaw kategorii w magazynie lokalnym urządzenia używane do rejestrowania w Centrum powiadomień przy każdym uruchomieniu aplikacji.  Użytkownik może zmienić wybór kategorii środowiska uruchomieniowego i kliknij przycisk **subskrypcji** metodę, aby zaktualizować rejestrację urządzenia. Następnie zostanie zaktualizowania aplikacji można wysłać powiadomienia o najważniejszych wiadomościach bezpośrednio w aplikacji.

## <a name="optional-sending-tagged-notifications"></a>(opcjonalnie) Wysyłanie powiadomień oznakowany
Jeśli nie masz dostępu do programu Visual Studio, można przejść do następnej sekcji i wysyłania powiadomień z samej aplikacji. Możesz również wysłać powiadomienie prawidłowego szablonu z [portalu Azure] za pomocą karty debugowanie w Centrum powiadomień. 

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-the-device"></a>(opcjonalnie) Wysyłanie powiadomień z urządzenia
Zwykle usługi wewnętrznej bazy danych będą wysyłane powiadomienia, ale możesz wysłać powiadomienia o najważniejszych wiadomościach bezpośrednio z aplikacji. W tym modyfikacjom `SendNotificationRESTAPI` metody zdefiniowanego w [Rozpoczynanie pracy z usługą Notification Hubs] [ get-started] samouczka.

1. W ramach aktualizacji ViewController.m `SendNotificationRESTAPI` metodę jako zgodna, który akceptuje parametr dla tagu kategorii i wysyła właściwego [szablonu](notification-hubs-templates-cross-platform-push-messages.md) powiadomień.
   
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
2. W ramach aktualizacji ViewController.m **Wyślij powiadomienie E-mail** akcji, jak pokazano w następującym kodzie. Tak, aby będzie wysyłania powiadomień przy użyciu tagami indywidualnie i wysłać do wielu platform.

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



1. Ponownie skompiluj projekt i upewnij się, że żadne błędy kompilacji.

## <a name="run-the-app-and-generate-notifications"></a>Uruchom aplikację i generować powiadomienia
1. Naciśnij przycisk Uruchom, aby skompilować projekt i uruchomić aplikację. Wybierz niektóre opcje wiadomości podziału subskrybować, a następnie naciśnij klawisz **Subskrybuj** przycisku. Powinny zostać wyświetlone okno dialogowe wskazujący, że zostały subskrypcję powiadomień.
   
    ![][1]
   
    Po wybraniu **Subskrybuj**, konwertuje wybranych kategorii do tagów i żąda nowej rejestracji urządzeń dla wybranych tagów z Centrum powiadomień aplikacji.
2. Wpisz wiadomość, aby wysłane w postaci najważniejszych wiadomości naciśnij **Wyślij powiadomienie E-mail** przycisku. Można również uruchomić aplikacji konsoli .NET do generowania powiadomień.
   
    ![][2]
3. Każdego urządzenia, które subskrybuje fundamentalne wiadomości otrzymają powiadomienia o najważniejszych wiadomościach właśnie wysłania.

## <a name="next-steps"></a>Następne kroki
W tym samouczku opisano sposób emisji najważniejszych wiadomości według kategorii. Należy rozważyć wykonanie jednej z następujących samouczków, w których są wyróżniane innych zaawansowanych scenariuszy centra powiadomień:

* **[Emisji zlokalizowanych najważniejszych wiadomości przy użyciu usługi Notification Hubs]**
  
    Dowiedz się, jak rozszerzyć aplikację wiadomości podziału, aby umożliwić wysyłanie powiadomień zlokalizowane.

<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Emisji zlokalizowanych najważniejszych wiadomości przy użyciu usługi Notification Hubs]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[portalu Azure]: https://portal.azure.com
