---
title: "Azure Notification Hubs Notify Users for iOS with .NET backend (Powiadamianie użytkowników urządzeń z systemem iOS przy użyciu usługi Azure Notification Hubs z zapleczem programu .NET)"
description: "Dowiedz się, jak wysyłać powiadomienia wypychane do użytkowników na platformie Azure. Przykłady kodu napisane w języku Objective C i interfejsu API programu .NET dla wewnętrznej bazy danych."
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 1f7d1410-ef93-4c4b-813b-f075eed20082
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 0fa7a886e1ecb0a90b6aebc1dbf9ef0c6ce1acf1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-notify-users-for-ios-with-net-backend"></a>Azure Notification Hubs Notify Users for iOS with .NET backend (Powiadamianie użytkowników urządzeń z systemem iOS przy użyciu usługi Azure Notification Hubs z zapleczem programu .NET)
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Omówienie
Obsługa powiadomień wypychanych na platformie Azure umożliwia dostęp do łatwego w użyciu, multiplatform i wypychanych skalowanej infrastruktury, co znacznie upraszcza implementację powiadomienia wypychane dla aplikacji zarówno konsumenckie i korporacyjne dla platform urządzeń przenośnych. W tym samouczku pokazano, jak wysyłać powiadomienia wypychane do użytkownika konkretnej aplikacji na konkretnym urządzeniu za pomocą usługi Azure Notification Hubs. Zaplecza ASP.NET WebAPI jest używany do uwierzytelniania klientów i generowania powiadomień, jak pokazano w temacie wskazówki [rejestrowanie z zaplecza aplikacji](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).

> [!NOTE]
> Ten samouczek zakłada, że utworzony i skonfigurowany Centrum powiadomień, zgodnie z opisem w [wprowadzenie do korzystania z usługi Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md). W tym samouczku jest również wymagana do [Secure Push (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) samouczka.
> Jeśli chcesz korzystać z aplikacji mobilnej jako usługi wewnętrznej bazy danych, zobacz [Mobile Apps wprowadzenie wypychania](../app-service-mobile/app-service-mobile-ios-get-started-push.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a>Modyfikowanie aplikacji systemu iOS
1. Otwórz aplikację widoku pojedynczej strony utworzone w [wprowadzenie do korzystania z usługi Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) samouczka.
   
   > [!NOTE]
   > W tej sekcji przyjęto założenie, projektu skonfigurowano nazwę organizacji puste. Jeśli nie, konieczne będzie dołączenie wartości Nazwa organizacji do wszystkich nazw klas.
   > 
   > 
2. W Twojej Main.storyboard dodać składniki pokazane na zrzucie ekranu poniżej z biblioteki obiektów.
   
    ![][1]
   
   * **Nazwa użytkownika**: UITextField A tekstem zastępczym *wprowadź nazwę użytkownika*, znajdujący się bezpośrednio pod wysyłania wyników etykiety i ograniczony do lewego i prawego marginesu i poniżej etykiety wyniki wysyłania.
   * **Hasło**: UITextField A tekstem zastępczym *wprowadź hasło*, natychmiast poniżej nazwę użytkownika tekst pola i ograniczone do lewego i prawego marginesu oraz poniżej pola tekstowego nazwy użytkownika. Sprawdź **Secure wpisywanie tekstu** w obszarze opcji Inspektora atrybutu *zwraca klucz*.
   * **Zaloguj się za**: A UIButton etykietą bezpośrednio poniżej pola tekstowego hasła i usuń zaznaczenie pola wyboru **włączone** w obszarze opcji Inspektora atrybuty *zawartości kontrolki*
   * **WNS**: etykiety i przełącznik, aby umożliwić wysyłanie powiadomień usługi powiadomień systemu Windows, jeśli został instalacji w Centrum. Zobacz [systemu Windows wprowadzenie](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) samouczka.
   * **GCM**: etykiety i przełącznika, aby włączyć wysyłanie powiadomienia do usługi Google Cloud Messaging została już Instalator koncentratora. Zobacz [Android wprowadzenie](notification-hubs-android-push-notification-google-gcm-get-started.md) samouczka.
   * **APNS**: etykiety i przełącznik, aby włączyć wysyłanie powiadomienia do usługi powiadomień platformy firmy Apple.
   * **Recipent Username**: UITextField A tekstem zastępczym *tag username odbiorcy*, natychmiast poniżej GCM etykiety i ograniczony do lewego i prawego marginesu oraz poniżej etykiety usługi GCM.

    Niektóre składniki zostały dodane w [wprowadzenie do korzystania z usługi Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) samouczka.

1. **CTRL** przeciągnij z składniki w widoku ViewController.h i dodać tych nowych punktów.
   
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
2. W ViewController.h, Dodaj następujący `#define` poniżej Twojej instrukcje importu. SUBSTITUTE *< wprowadź swój Endpoint zaplecza\>*  symbol zastępczy docelowy adres URL używany do wdrażania aplikacji zaplecza w poprzedniej sekcji. Na przykład *http://you_backend.azurewebsites.net*.
   
        #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
3. W projekcie, Utwórz nową **Cocoa Touch klasy** o nazwie **RegisterClient** do interfejsu z zaplecza ASP.NET został utworzony. Tworzenie dziedziczenia z klasy `NSObject`. Następnie dodaj następujący kod w RegisterClient.h.
   
        @interface RegisterClient : NSObject
   
        @property (strong, nonatomic) NSString* authenticationHeader;
   
        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
            andCompletion:(void(^)(NSError*))completion;
   
        -(instancetype) initWithEndpoint:(NSString*)Endpoint;
   
        @end
4. W ramach aktualizacji RegisterClient.m `@interface` sekcji:
   
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
5. Zastąp `@implementation` części RegisterClient.m następującym kodem.

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

    Powyższy kod implementuje logiki wyjaśniono w artykule wskazówki [rejestrowanie z zaplecza aplikacji](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) wykonać REST przy użyciu polecenia NSURLSession wywołań zapleczu swojej aplikacji i NSUserDefaults do przechowywania lokalnie registrationId zwrócony przez Centrum powiadomień.

    Należy pamiętać, że ta klasa wymaga jego właściwość **authorizationHeader** ustawiono prawidłowego funkcjonowania. Ta właściwość jest ustawiana przez **ViewController** klasy po stronie logowania.

1. W ViewController.h, Dodaj `#import` instrukcji dla RegisterClient.h. Następnie dodaj deklarację dla tokenu urządzenia i odwołać się do `RegisterClient` wystąpienia w `@interface` sekcji:
   
        #import "RegisterClient.h"
   
        @property (strong, nonatomic) NSData* deviceToken;
        @property (strong, nonatomic) RegisterClient* registerClient;
2. W ViewController.m, Dodaj deklarację Metoda prywatna w `@interface` sekcji:
   
        @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>
   
        // create the Authorization header to perform Basic authentication with your app back-end
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
   
        @end

> [!NOTE]
> Poniższy fragment kodu nie jest schemat bezpiecznego uwierzytelniania, należy zastąpić wykonania **createAndSetAuthenticationHeaderWithUsername:AndPassword:** z mechanizmu uwierzytelniania określonych generujący token uwierzytelniania do użycia przez klienta rejestru klasę, np. OAuth, usługi Active Directory.
> 
> 

1. Następnie w `@implementation` sekcji ViewController.m Dodaj następujący kod, który dodaje implementacji do ustawiania nagłówka tokenu i uwierzytelnienia urządzenia.
   
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
   
    Należy zwrócić uwagę, jak ustawienie token urządzenia włącza przycisk Zaloguj. Jest to spowodowane w ramach akcji logowania, kontroler widoku rejestruje dla powiadomień wypychanych przy użyciu zaplecza aplikacji. W związku z tym nie chcemy dziennika w akcji była dostępna, dopóki token urządzenia został poprawnie skonfigurowany. Logowania z rejestracji wypychania jest oddzielana, tak długo, jak wcześniejsze nastąpi przed jego.
2. W ViewController.m, należy użyć poniższej wstawki Aby zaimplementować metodę akcji dla Twojego **dziennika w** przycisk, a także metodę można wysłać komunikatu powiadomienia przy użyciu zaplecza ASP.NET.
   
       - (IBAction) LogInAction: nadawcy (id) {/ / Utwórz nagłówek uwierzytelnienia i ustaw go w rejestrze klienta NSString * nazwa użytkownika = samodzielnie. UsernameField.text;   Hasło NSString * = samodzielnie. PasswordField.text;
   
           [self createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];
   
           __weak ViewController * selfie = self;   [self.registerClient registerWithDeviceToken:self.deviceToken tagi: nil andCompletion:^(NSError* error) {Jeśli (! błędu) {dispatch_async(dispatch_get_main_queue(), ^ {selfie. SendNotificationButton.enabled = YES;               [samoobsługowego MessageBox:@"Success" message:@"Registered pomyślnie!"];});}}];}

        -SendNotificationASPNETBackend (void): (NSString*) systemu powiadomień platformy UsernameTag: (NSString*) usernameTag komunikat: (NSString*) komunikat {NSURLSession* sesji = [NSURLSession sessionWithConfiguration: delegateQueue:nil delegata: nil [NSURLSessionConfiguration defaultSessionConfiguration]];

            Przekaż tag systemu powiadomień platformy i nazwa użytkownika jako parametry z adresem URL REST do zaplecza ASP.NET nsurl OCZEKIWANEGO * Adres_url_żądania = [URLWithString nsurl OCZEKIWANEGO: [NSString stringWithFormat:@"%@/api/notifications? systemu powiadomień platformy = % @& to_tag = % @", BACKEND_ENDPOINT, pns, usernameTag]];

            Żądanie NSMutableURLRequest * = [NSMutableURLRequest requestWithURL:requestURL];    [setHTTPMethod:@"POST"żądania];

            Pobierz zasymulować authenticationheader z rejestru klienta authorizationHeaderValue NSString * = [NSString stringWithFormat:@"Basic % @", self.registerClient.authenticationHeader];    [setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"żądania];

            Dodaj treść wiadomości powiadomienia [żądania setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];    [żądanie setHTTPBody: [komunikatów dataUsingEncoding:NSUTF8StringEncoding]];

            Wykonanie wysyłania powiadomień interfejsu API REST na dataTask ASP.NET zaplecza NSURLSessionDataTask * = [sesji dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) odpowiedź;        Jeśli (błąd || httpResponse.statusCode! = 200) {NSString* stan = [NSString stringWithFormat:@"Error stanu % @: % d\nError: %@\n", pns, httpResponse.statusCode, błąd];            dispatch_async(dispatch_get_main_queue(), ^ {/ / Append tekstu, ponieważ wszystkie wywołania systemu powiadomień platformy 3 może również zawierać informacje do widoku [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }

                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];    [dataTask Wznów]; }


1. Akcja dla aktualizacji **Wyślij powiadomienie E-mail** przycisk, aby użyć zaplecza ASP.NET i wysłać do dowolnego systemu powiadomień platformy włączane przez przełącznik.

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



1. W funkcji **ViewDidLoad**, Dodaj następujące można utworzyć wystąpienia RegisterClient wystąpienia i ustawić delegata dla pól tekstowych.
   
       self.UsernameField.delegate = self;
       self.PasswordField.delegate = self;
       self.RecipientField.delegate = self;
       self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
2. Teraz w **AppDelegate.m**, Usuń całą zawartość metody **aplikacji: didRegisterForPushNotificationWithDeviceToken:** i zastąp go z następujących czynności, aby upewnić się, że kontroler widoku zawiera najnowsze token urządzenia pobierane z usługi APNs:
   
       // Add import to the top of the file
       #import "ViewController.h"
   
       - (void)application:(UIApplication *)application
                   didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
       {
           ViewController* rvc = (ViewController*) self.window.rootViewController;
           rvc.deviceToken = deviceToken;
       }
3. Na koniec w **AppDelegate.m**, upewnij się, że masz następujące metody:
   
       - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
           NSLog(@"%@", userInfo);
           [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
       }

## <a name="test-the-application"></a>Testowanie aplikacji
1. W programie XCode Uruchom aplikację na urządzenie fizyczne z systemem iOS (wypychanie powiadomień nie będzie działać w symulatorze).
2. W aplikacji systemu iOS interfejsu użytkownika wprowadź nazwę użytkownika i hasło. Mogą to być dowolny ciąg, ale oba muszą być taką samą wartość ciągu. Następnie kliknij przycisk **dziennika w**.
   
    ![][2]
3. Powinny zostać wyświetlone okno podręczne informujące o Powodzenie rejestracji. Kliknij przycisk **OK**.
   
    ![][3]
4. W **tag username odbiorcy* tekst Wprowadź tag nazwę użytkownika używane z rejestracją z innego urządzenia.
5. Wprowadź komunikat powiadomienia, a następnie kliknij przycisk **Wyślij powiadomienie E-mail**.  Tylko urządzenia, które ma rejestracji z tagiem nazwy użytkownika odbiorcy komunikat powiadomienia.  Jest wysyłane tylko do tych użytkowników.
   
    ![][4]

[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
