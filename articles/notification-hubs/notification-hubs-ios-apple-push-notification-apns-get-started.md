---
title: Wysyłanie powiadomień wypychanych do urządzeń z systemem iOS przy użyciu usługi Azure Notification Hubs | Microsoft Docs
description: Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji dla systemu iOS przy użyciu usługi Azure Notification Hubs.
services: notification-hubs
documentationcenter: ios
keywords: powiadomienie wypychane, powiadomienia wypychane, powiadomienia wypychane w systemie ios
author: wesmc7777
manager: erikre
editor: ''

ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: wesmc

---
# Wysyłanie powiadomień wypychanych do urządzeń z systemem iOS przy użyciu usługi Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## Omówienie
> [!NOTE]
> Do wykonania kroków tego samouczka potrzebne jest aktywne konto platformy Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).
> 
> 

Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji dla systemu iOS przy użyciu usługi Azure Notification Hubs. Utworzysz pustą aplikację dla systemu iOS służącą do odbierania powiadomień wypychanych przy użyciu usługi [Apple Push Notification Service (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). 

Po zakończeniu będzie można za pomocą centrum powiadomień wysyłać powiadomienia wypychane do wszystkich urządzeń z tą aplikacją.

## Przed rozpoczęciem
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Kompletny kod dla tego samouczka można znaleźć [w witrynie GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

## Wymagania wstępne
Dla tego samouczka wymagane są następujące elementy:

* [zestaw SDK usługi Mobile Services systemu iOS w wersji 1.2.4]
* Najnowsza wersja środowiska [Xcode]
* Urządzenie zgodne z systemem iOS 8 (lub nowszą wersją)
* Członkostwo w [programie dla deweloperów firmy Apple](https://developer.apple.com/programs/)
  
  > [!NOTE]
  > Ze względu na wymagania dotyczące konfiguracji powiadomień wypychanych należy wdrożyć i przetestować powiadomienia wypychane na fizycznym urządzeniu z systemem iOS (telefonie iPhone lub tablecie iPad), a nie w symulatorze systemu iOS.
  > 
  > 

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usługi Notification Hubs dotyczących aplikacji dla systemu iOS.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## Konfigurowanie centrum powiadomień dla powiadomień wypychanych systemu iOS
W tej sekcji opisano kroki tworzenia nowego centrum powiadomień i konfigurowania uwierzytelniania w usłudze APNs przy użyciu utworzonego przez Ciebie certyfikatu powiadomień wypychanych **.p12**. Jeśli chcesz użyć już utworzonego centrum powiadomień, możesz przejść do kroku 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p>Kliknij przycisk <b>Usługi powiadomień</b> w bloku <b>Ustawienia</b>, a następnie wybierz pozycję <b>Apple (APNs)</b>. Kliknij pozycję <b>Przekaż certyfikat</b> i wybierz wcześniej wyeksportowany plik <b>.p12</b>. Upewnij się, że określono prawidłowe hasło.</p>

<p>Upewnij się, że wybrano tryb <b>Piaskownica</b> na potrzeby tworzenia aplikacji. Tryb <b>Produkcja</b> należy wybrać wyłącznie wówczas, gdy chcesz wysyłać powiadomienia wypychane do użytkowników, którzy kupili Twoją aplikację w sklepie.</p>
</li>
</ol>
&emsp;&emsp;![Konfigurowanie usługi APNS w witrynie Azure Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;![Konfigurowanie certyfikacji APNs w witrynie Azure Portal](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

Twoje centrum powiadomień jest teraz skonfigurowane do pracy z usługą APNs i uzyskano parametry połączenia służące do rejestrowania aplikacji w celu wysyłania powiadomień wypychanych.

## Łączenie aplikacji dla systemu iOS z usługą Notification Hubs
1. W środowisku Xcode utwórz nowy projekt dla systemu iOS i wybierz szablon **Single View Application** (Aplikacja z jednym widokiem).
   
    ![Xcode — aplikacja z jednym widokiem][8]
2. Podczas określania opcji dla nowego projektu upewnij się, że użyto tych samych wartości **Product Name** (Nazwa produktu) i **Organization Identifier** (Identyfikator organizacji), które zostały użyte podczas wcześniejszego ustawiania identyfikatora pakietu w portalu dla deweloperów firmy Apple.
   
    ![Xcode — opcje projektu][11]
3. W obszarze **Targets** (Elementy docelowe) kliknij nazwę projektu, kliknij kartę **Build Settings** (Ustawienia kompilacji) i rozwiń węzeł **Code Signing Identity** (Tożsamość podpisywania kodu), a następnie w obszarze **Debug** (Debugowanie) ustaw tożsamość podpisywania kodu. Zmień ustawienie opcji **Levels** (Poziomy) z **Basic** (Podstawowe) na **All** (Wszystkie) i jako **Provisioning Profile** (Profil aprowizowania) ustaw wcześniej utworzony profil aprowizowania.
   
    Jeśli nowy profil aprowizowania utworzony w programie Xcode nie jest widoczny, odśwież profile dla Twojej tożsamości podpisywania. Kliknij pozycję **Xcode** na pasku menu, kliknij pozycję **Preferences** (Preferencje), kliknij kartę **Account** (Konto), kliknij przycisk **View Details** (Pokaż szczegóły), kliknij tożsamość podpisywania, a następnie kliknij przycisk odświeżania w prawym dolnym rogu.
   
    ![Xcode — profil aprowizowania][9]
4. Pobierz [zestaw SDK usługi Mobile Services systemu iOS w wersji 1.2.4] i rozpakuj plik. W programie Xcode kliknij prawym przyciskiem myszy projekt i kliknij opcję **Add Files to** (Dodaj pliki do), aby dodać folder **WindowsAzureMessaging.framework** do projektu Xcode. Wybierz pozycję **Copy items if needed** (Skopiuj elementy w razie potrzeby), a następnie kliknij pozycję **Add** (Dodaj).
   
   > [!NOTE]
   > Zestaw SDK usługi Notification Hubs nie obsługuje obecnie kodu bitowego w programie Xcode 7.  Dla ustawienia **Enable Bitcode** (Włącz kod bitowy) należy wybrać opcję **No** (Nie) w obszarze **Build Options** (Opcje kompilacji) projektu.
   > 
   > 
   
    ![Rozpakowywanie zestawu SDK platformy Azure][10]
5. Dodaj nowy plik nagłówka do projektu o nazwie `HubInfo.h`. Ten plik będzie zawierać stałe dla centrum powiadomień.  Dodaj następujące definicje i zastąp symbole zastępcze literału ciągu przy użyciu *nazwy centrum* i wartości *DefaultListenSharedAccessSignature* zanotowanej wcześniej.
   
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
6. Otwórz plik `AppDelegate.h` i dodaj następujące dyrektywy importu:
   
         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
7. W pliku `AppDelegate.m file` dodaj następujący kod w metodzie `didFinishLaunchingWithOptions` zależnie od wersji systemu iOS. Ten kod rejestruje dojście do urządzenia w usłudze APNs:
   
    Dla systemu iOS 8:
   
        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
   
    Dla wersji systemu iOS starszych niż 8:
   
         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
8. W tym samym pliku dodaj następujące metody. Ten kod łączy się z centrum powiadomień przy użyciu informacji o połączeniu określonych w pliku HubInfo.h. Następnie przekazuje token urządzenia do centrum powiadomień, aby umożliwić wysyłanie powiadomień:
   
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
9. W tym samym pliku dodaj następującą metodę służącą do wyświetlania elementu **UIAlert** w przypadku otrzymania powiadomienia, gdy aplikacja jest aktywna:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

1. Skompiluj i uruchom aplikację na urządzeniu, aby upewnić się, że nie występują żadne błędy.

## Wysyłanie testowych powiadomień wypychanych
Odbieranie powiadomień w aplikacji możesz przetestować, wysyłając powiadomienia wypychane w witrynie [Azure Portal] za pośrednictwem sekcji **Rozwiązywanie problemów** w bloku centrum (użyj opcji *Wysyłanie testowe*).

![Portal Azure — wysyłanie testowe][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## (Opcjonalnie) Wysyłanie powiadomień wypychanych z poziomu aplikacji
> [!IMPORTANT]
> Ten przykład wysyłania powiadomień z aplikacji klienckiej został podany tylko dla celów szkoleniowych. Ponieważ wymaga to obecności `DefaultFullSharedAccessSignature` w aplikacji klienckiej, naraża centrum powiadomień na ryzyko, że użytkownik może uzyskać dostęp do wysyłania nieautoryzowanych powiadomień do Twoich klientów.
> 
> 

Jeśli chcesz wysyłać powiadomienia wypychane z poziomu aplikacji, ta sekcja zawiera przykład, jak to zrobić przy użyciu interfejsu REST.

1. W programie Xcode otwórz plik `Main.storyboard` i dodaj następujące składniki interfejsu użytkownika z biblioteki obiektów, aby umożliwić użytkownikom wysyłanie powiadomień wypychanych w aplikacji:
   
   * Etykieta bez tekstu etykiety. Będzie służyć do raportowania błędów wysyłania powiadomień. Dla właściwości **Lines** (Wiersze) należy ustawić wartość **0**, aby rozmiar elementu był automatycznie ograniczony do lewego i prawego marginesu oraz góry widoku.
   * Pole tekstowe **Placeholder** (Symbol zastępczy) z ustawionym tekstem **Enter Notification Message** (Wprowadź komunikat powiadomienia). Ogranicz pole bezpośrednio pod etykietą w sposób przedstawiony poniżej. Ustaw kontroler widoku jako delegat gniazda.
   * Przycisk zatytułowany **Send Notification** (Wyślij powiadomienie) ograniczony bezpośrednio poniżej pola tekstowego i wyśrodkowany w poziomie.
     
     Widok powinien wyglądać w następujący sposób:
     
     ![Projektant programu Xcode][32]
2. [Dodaj gniazda](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) dla etykiety i pola tekstowego połączonych z widokiem i zaktualizuj definicję `interface` w celu obsługi elementów `UITextFieldDelegate` i `NSXMLParserDelegate`. Dodaj trzy deklaracje właściwości przedstawione poniżej, aby ułatwić obsługę wywołań interfejsu API REST i analizowanie odpowiedzi.
   
    Plik ViewController.h powinien wyglądać następująco:
   
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
3. Otwórz plik `HubInfo.h` i dodaj następujące ograniczenia, które będą używane podczas wysyłania powiadomień do centrum. Zastąp symbol zastępczy literału ciągu rzeczywistymi parametrami połączenia *DefaultFullSharedAccessSignature*.
   
        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"
4. Dodaj następujące instrukcje `#import` do pliku `ViewController.h`:
   
        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"
5. W pliku `ViewController.m` dodaj następujący kod do implementacji interfejsu. Ten kod będzie analizować parametry połączenia *DefaultFullSharedAccessSignature*. Jak wspomniano w [dokumentacji interfejsu API REST](http://msdn.microsoft.com/library/azure/dn495627.aspx), te przeanalizowane informacje będą używane do generowania tokenu sygnatury dostępu współdzielonego dla nagłówka żądania **Authorization**.
   
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
6. W pliku `ViewController.m` zaktualizuj metodę `viewDidLoad`, aby parametry połączenia były analizowane po załadowaniu widoku. Dodaj również metody narzędziowe przedstawione poniżej do implementacji interfejsu.  

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





1. W pliku `ViewController.m` dodaj następujący kod do implementacji interfejsu w celu wygenerowania tokenu sygnatury dostępu współdzielonego, który zostanie podany w nagłówku **Authorization**, jak opisano w [dokumentacji interfejsu API REST](http://msdn.microsoft.com/library/azure/dn495627.aspx).
   
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
2. Naciśnij klawisz Ctrl i przeciągnij od przycisku **Send Notification** (Wyślij powiadomienie) do pliku `ViewController.m`, aby dodać akcję **SendNotificationMessage** dla zdarzenia **Touch Down** (Dotknięcie). Zaktualizuj metodę przy użyciu następującego kodu w celu wysyłania powiadomień przy użyciu interfejsu API REST.
   
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
3. W pliku `ViewController.m` dodaj następującą metodę delegata w celu obsługi zamykania klawiatury dla pola tekstowego. Naciśnij klawisz Ctrl i przeciągnij od pola tekstowego do ikony kontrolera widoku w projektancie interfejsu, aby ustawić kontroler widoku jako delegat gniazda.
   
        //===[ Implement UITextFieldDelegate methods ]===
   
        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }
4. W pliku `ViewController.m` dodaj następujące metody delegata w celu obsługi analizowania odpowiedzi przy użyciu analizatora `NSXMLParser`.
   
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
5. Skompiluj projekt i upewnij się, że nie ma żadnych błędów.

> [!NOTE]
> W przypadku wystąpienia błędu kompilacji w programie Xcode 7 dotyczącego obsługi kodu bitowego, należy zmienić ustawienie **Build Settings** (Ustawienia kompilacji)  >  **Enable Bitcode (ENABLE_BITCODE)** (Włącz kod bitowy [ENABLE_BITCODE]) na **NO** (NIE) w programie Xcode. Zestaw SDK usługi Notification Hubs nie obsługuje obecnie kodu bitowego. 
> 
> 

Wszystkie możliwe ładunki powiadomień można znaleźć w publikacji [Podręcznik programowania powiadomień lokalnych i wypychanych] firmy Apple.

## Sprawdzanie, czy aplikacja może odbierać powiadomienia wypychane
Aby przetestować powiadomienia wypychane w systemie iOS, należy wdrożyć aplikację na fizycznym urządzeniu z systemem iOS. Nie można wysłać powiadomień wypychanych firmy Apple przy użyciu symulatora systemu iOS.

1. Uruchom aplikację i upewnij się, że rejestracja zakończyła się powodzeniem, a następnie naciśnij przycisk **OK**.
   
    ![Test rejestracji powiadomień wypychanych aplikacji dla systemu iOS][33]
2. Testowe powiadomienie wypychane możesz wysłać z poziomu witryny [Azure Portal] zgodnie z powyższym opisem. Jeśli dodano kod umożliwiający wysyłanie powiadomień wypychanych z aplikacji, dotknij w polu tekstowym, aby wprowadzić komunikat powiadomienia. Następnie naciśnij przycisk **Send** (Wyślij) na klawiaturze lub przycisk **Send Notification** (Wyślij powiadomienie) w widoku, aby wysłać komunikat powiadomienia.
   
    ![Test wysyłania powiadomienia wypychanego z aplikacji dla systemu iOS][34]
3. Powiadomienie wypychane zostanie wysłane do wszystkich urządzeń zarejestrowanych do odbierania powiadomień od określonego centrum powiadomień.
   
    ![Test odbierania powiadomienia wypychanego z aplikacji dla systemu iOS][35]

## Następne kroki
W tym prostym przykładzie wysłano powiadomienia wypychane do wszystkich zarejestrowanych urządzeń z systemem iOS. Jako kolejny krok sugerujemy zapoznanie się z samouczkiem [Azure Notification Hubs Notify Users for iOS with .NET backend](Powiadamianie użytkowników urządzeń z systemem iOS przy użyciu usługi Azure Notification Hubs z zapleczem programu .NET), który zawiera instrukcje dotyczące tworzenia zaplecza służącego do wysyłania powiadomień wypychanych przy użyciu tagów. 

Jeśli chcesz podzielić użytkowników na grupy zainteresowań, możesz dodatkowo zapoznać się z samouczkiem [Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]. 

Ogólne informacje o usłudze Notification Hubs zawiera temat [Wskazówki dotyczące usługi Notification Hubs].

<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[zestaw SDK usługi Mobile Services systemu iOS w wersji 1.2.4]: http://aka.ms/kymw2g
[Zestaw SDK usługi Mobile Services systemu iOS]: http://go.microsoft.com/fwLink/?LinkID=266533
[Strona przesyłania aplikacji]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[Moje aplikacje]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Zestaw Live SDK dla systemu Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Wprowadzenie do usług Mobile Services]: /develop/mobile/tutorials/get-started-ios
[klasyczny portal Azure]: https://manage.windowsazure.com/
[Wskazówki dotyczące usługi Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Portal aprowizowania dla systemu iOS]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Wprowadzenie do powiadomień wypychanych w usłudze Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure Notification Hubs Notify Users for iOS with .NET backend (Powiadamianie użytkowników urządzeń z systemem iOS przy użyciu usługi Azure Notification Hubs z zapleczem programu .NET)]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Podręcznik programowania powiadomień lokalnych i wypychanych]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com


<!--HONumber=Oct16_HO1-->


