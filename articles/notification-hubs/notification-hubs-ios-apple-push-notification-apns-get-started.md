---
title: "Rozpoczynanie pracy z usługą Azure Notification Hubs dla aplikacji systemu iOS | Microsoft Docs"
description: "Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji dla systemu iOS przy użyciu usługi Azure Notification Hubs."
services: notification-hubs
documentationcenter: ios
keywords: powiadomienie wypychane, powiadomienia wypychane, powiadomienia wypychane w systemie ios
author: jwhitedev
manager: kpiteira
editor: 
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 0e9e7ab196eef790b74074be319cd8122cf3ff5c
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2018
---
# <a name="get-started-with-azure-notification-hubs-for-ios-apps"></a>Rozpoczynanie pracy z usługą Azure Notification Hubs dla aplikacji systemu iOS
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Omówienie
> [!NOTE]
> Do wykonania kroków tego samouczka potrzebne jest aktywne konto platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).
> 
> 

Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji dla systemu iOS przy użyciu usługi Azure Notification Hubs. Utworzysz pustą aplikację dla systemu iOS służącą do odbierania powiadomień wypychanych przy użyciu usługi [Apple Push Notification Service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1). 

Po zakończeniu będzie można za pomocą centrum powiadomień wysyłać powiadomienia push do wszystkich urządzeń z tą aplikacją.

## <a name="before-you-begin"></a>Przed rozpoczęciem
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Kompletny kod dla tego samouczka można znaleźć [w witrynie GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

## <a name="prerequisites"></a>Wymagania wstępne
Dla tego samouczka wymagane są następujące elementy:

* [strukturę Windows Azure Messaging]
* Najnowsza wersja środowiska [Xcode]
* Urządzenie zgodne z systemem iOS 10 (lub nowszą wersją)
* Członkostwo w [programie dla deweloperów firmy Apple](https://developer.apple.com/programs/).
  
  > [!NOTE]
  > Ze względu na wymagania dotyczące konfiguracji powiadomień wypychanych należy wdrożyć i przetestować powiadomienia wypychane na fizycznym urządzeniu z systemem iOS (telefonie iPhone lub tablecie iPad), a nie w symulatorze systemu iOS.
  > 
  > 

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usługi Notification Hubs dotyczących aplikacji dla systemu iOS.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>Konfigurowanie centrum powiadomień dla powiadomień wypychanych systemu iOS
W tej sekcji opisano kroki tworzenia nowego centrum powiadomień i konfigurowania uwierzytelniania w usłudze APNs przy użyciu utworzonego wcześniej certyfikatu powiadomień wypychanych **p12**. Jeśli chcesz użyć już utworzonego centrum powiadomień, możesz przejść do kroku 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p>W obszarze <b>Usługi powiadomień</b> wybierz pozycję <b>Apple (APNS)</b>. Upewnij się, że zaznaczono pozycję <b>Certyfikat</b>, kliknij ikonę pliku i wybierz wyeksportowany wcześniej plik <b>p12</b>. Upewnij się, że określono prawidłowe hasło.</p>

<p>Upewnij się, że wybrano tryb <b>Piaskownica</b> na potrzeby tworzenia aplikacji. Tryb <b>Produkcja</b> należy wybrać wyłącznie wówczas, gdy chcesz wysyłać powiadomienia wypychane do użytkowników, którzy kupili Twoją aplikację w sklepie.</p>
</li>
</ol>
&emsp;&emsp;&emsp;&emsp;![Konfigurowanie usługi APNs w witrynie Azure Portal][6]

&emsp;&emsp;&emsp;&emsp;![Konfigurowanie certyfikacji APNs w witrynie Azure Portal][7]

Twoje centrum powiadomień jest teraz skonfigurowane z usługą APNs i masz parametry połączenia służące do rejestrowania aplikacji oraz wysyłania powiadomień push.

## <a name="connect-your-ios-app-to-notification-hubs"></a>Łączenie aplikacji dla systemu iOS z usługą Notification Hubs
1. W środowisku Xcode utwórz nowy projekt dla systemu iOS i wybierz szablon **Single View Application** (Aplikacja z jednym widokiem).
   
    ![Xcode — aplikacja z jednym widokiem][8]
    
2. Podczas określania opcji dla nowego projektu upewnij się, że użyto tych samych wartości **Product Name** (Nazwa produktu) i **Organization Identifier** (Identyfikator organizacji), które zostały użyte podczas ustawiania identyfikatora pakietu w portalu dla deweloperów firmy Apple.
   
    ![Xcode — opcje projektu][11]
    
3. W obszarze Project Navigator (Nawigator projektów) kliknij nazwę swojego projektu, kliknij kartę **General** (Ogólne) i wyszukaj pozycję **Signing** (Podpisywanie). Upewnij się, że wybrano odpowiedni zespół dla konta dewelopera firmy Apple. Środowisko Xcode powinno automatycznie ściągnąć utworzony wcześniej profil aprowizowania na podstawie identyfikatora pakietu.
   
    Jeśli nowy profil aprowizowania utworzony w programie Xcode nie jest widoczny, odśwież profile dla Twojej tożsamości podpisywania. Kliknij pozycję **Xcode** na pasku menu, kliknij pozycję **Preferences** (Preferencje), kliknij kartę **Account** (Konto), kliknij przycisk **View Details** (Pokaż szczegóły), kliknij tożsamość podpisywania, a następnie kliknij przycisk odświeżania w prawym dolnym rogu.

    ![Xcode — profil aprowizowania][9]

4. Wybierz kartę **Capabilities** (Możliwości) i upewnij się, że powiadomienia push są włączone.

    ![Xcode — możliwości push][12]
   
5. Pobierz [strukturę Windows Azure Messaging] i rozpakuj plik. W programie Xcode kliknij prawym przyciskiem myszy projekt i kliknij opcję **Add Files to** (Dodaj pliki do), aby dodać folder **WindowsAzureMessaging.framework** do projektu Xcode. Wybierz pozycję **Options** (Opcje), upewnij się, że pozycja **Copy items if needed** (Skopiuj elementy w razie potrzeby) jest zaznaczona, a następnie kliknij pozycję **Add** (Dodaj).

    ![Rozpakowywanie zestawu SDK platformy Azure][10]

6. Dodaj nowy plik nagłówkowy do projektu o nazwie **HubInfo.h**. Ten plik będzie zawierać stałe dla centrum powiadomień. Dodaj następujące definicje i zastąp symbole zastępcze literału ciągu przy użyciu *nazwy centrum* i zanotowanej wcześniej wartości *DefaultListenSharedAccessSignature*.

    ```obj-c
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
    ```
    
7. Otwórz plik **AppDelegate.h** i dodaj następujące dyrektywy importu:

    ```obj-c
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
        #import <UserNotifications/UserNotifications.h> 
        #import "HubInfo.h"
    ```
8. W **pliku AppDelegate.m** dodaj następujący kod w metodzie **didFinishLaunchingWithOptions** zależnie od wersji systemu iOS. Ten kod rejestruje dojście do urządzenia w usłudze APNs:

    ```obj-c
        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
            UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
    ```
   
9. W tym samym pliku dodaj następujące metody:

    ```obj-c
         - (void) application:(UIApplication *) application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
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
   
        -(void)MessageBox:(NSString *) title message:(NSString *)messageText
        {
         UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }
    ```

    Ten kod łączy się z centrum powiadomień przy użyciu informacji o połączeniu określonych w pliku HubInfo.h. Następnie przekazuje token urządzenia do centrum powiadomień, aby umożliwić wysyłanie powiadomień.

10. W tym samym pliku dodaj następującą metodę służącą do wyświetlania elementu **UIAlert** w przypadku otrzymania powiadomienia, gdy aplikacja jest aktywna:

    ```obj-c
            - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
               NSLog(@"%@", userInfo);
               [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
           }
    ```

11. Skompiluj i uruchom aplikację na urządzeniu, aby upewnić się, że nie występują żadne błędy.

## <a name="send-test-push-notifications"></a>Wysyłanie testowych powiadomień wypychanych
Odbieranie powiadomień w aplikacji możesz przetestować za pomocą opcji *Wysyłanie testowe* w witrynie [Azure Portal]. Spowoduje to wysłanie testowego powiadomienia push na urządzenie.

![Witryna Azure Portal — wysyłanie testowe][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="checking-if-your-app-can-receive-push-notifications"></a>Sprawdzanie, czy aplikacja może odbierać powiadomienia wypychane
Aby przetestować powiadomienia wypychane w systemie iOS, należy wdrożyć aplikację na fizycznym urządzeniu z systemem iOS. Nie można wysłać powiadomień wypychanych firmy Apple przy użyciu symulatora systemu iOS.

1. Uruchom aplikację i upewnij się, że rejestracja zakończyła się powodzeniem, a następnie naciśnij przycisk **OK**.
   
    ![Test rejestracji powiadomień wypychanych aplikacji dla systemu iOS][33]
2. Następnie wyślij z witryny [Azure Portal] testowe powiadomienie push, jak opisano powyżej. 

3. Powiadomienie wypychane zostanie wysłane do wszystkich urządzeń zarejestrowanych do odbierania powiadomień od określonego centrum powiadomień.
   
    ![Test odbierania powiadomienia wypychanego z aplikacji dla systemu iOS][35]

## <a name="next-steps"></a>Następne kroki
W tym prostym przykładzie wysłano powiadomienia wypychane do wszystkich zarejestrowanych urządzeń z systemem iOS. Zalecanym następnym krokiem jest rozpoczęcie samouczka [Powiadamianie użytkowników urządzeń z systemem iOS przy użyciu usługi Azure Notification Hubs z zapleczem platformy .NET]. W tym przewodniku opisano tworzenie zaplecza służącego do wysyłania powiadomień push przy użyciu tagów. 

Jeśli chcesz podzielić użytkowników na grupy zainteresowań, możesz dodatkowo zapoznać się z samouczkiem [Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]. 

Ogólne informacje o usłudze Notification Hubs zawiera temat [Wskazówki dotyczące usługi Notification Hubs].

<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png
[12]: ./media/notification-hubs-ios-get-started/notification-hubs-enable-push.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[strukturę Windows Azure Messaging]: http://go.microsoft.com/fwlink/?LinkID=799698&clcid=0x409
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Wskazówki dotyczące usługi Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Powiadamianie użytkowników urządzeń z systemem iOS przy użyciu usługi Azure Notification Hubs z zapleczem platformy .NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md (Powiadamianie użytkowników urządzeń z systemem iOS przy użyciu usługi Azure Notification Hubs z zapleczem programu .NET)
[Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Local and Push Notification Programming Guide]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com
