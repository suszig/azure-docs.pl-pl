---
title: "Rozpoczynanie pracy z usługą Azure Notification Hubs dla aplikacji platformy Xamarin.iOS | Microsoft Docs"
description: "Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji platformy Xamarin dla systemu iOS przy użyciu usługi Azure Notification Hubs."
services: notification-hubs
keywords: "powiadomienia wypychane w systemie ios, wiadomości wypychane, powiadomienia wypychane, wiadomość wypychana"
documentationcenter: xamarin
author: jwhiteDev
manager: kpiteira
editor: 
ms.assetid: 4d4dfd42-c5a5-4360-9d70-7812f96924d2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 38ad8a15fcc4077926e735e01f877a4ee66718ef
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2018
---
# <a name="get-started-with-azure-notification-hubs-for-xamarinios-apps"></a>Rozpoczynanie pracy z usługą Azure Notification Hubs dla aplikacji platformy Xamarin.iOS
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Omówienie
> [!NOTE]
> Do wykonania kroków tego samouczka potrzebne jest aktywne konto platformy Azure. Jeśli jej nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).
> 
> 

Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji dla systemu iOS przy użyciu usługi Azure Notification Hubs. Utworzysz pustą aplikację platformy Xamarin.iOS służącą do odbierania powiadomień wypychanych przy użyciu usługi [Apple Push Notification Service (APNS)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html). 

Po zakończeniu będzie można za pomocą centrum powiadomień wysyłać powiadomienia wypychane do wszystkich urządzeń z tą aplikacją. Gotowy kod jest dostępny w przykładowej aplikacji [NotificationHubs][GitHub].

W tym samouczku został omówiony prosty scenariusz wysyłania powiadomień wypychanych przy użyciu usługi Notification Hubs.

## <a name="prerequisites"></a>Wymagania wstępne
Dla tego samouczka wymagane są następujące elementy:

* Najnowsza wersja środowiska [Xcode][Install Xcode]
* Urządzenie zgodne z systemem iOS 10 (lub nowszą wersją)
* Członkostwo w [programie dla deweloperów firmy Apple](https://developer.apple.com/programs/).
* [Visual Studio dla komputerów Mac]
  
  > [!NOTE]
  > Ze względu na wymagania dotyczące konfiguracji powiadomień wypychanych w systemie iOS należy wdrożyć i przetestować aplikację przykładową na fizycznym urządzeniu z systemem iOS (telefonie iPhone lub tablecie iPad), a nie w symulatorze.
  > 
  > 

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usług Notification Hubs dotyczących aplikacji platformy Xamarin.iOS.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>Konfigurowanie centrum powiadomień dla powiadomień wypychanych systemu iOS
W tej sekcji opisano kroki tworzenia nowego centrum powiadomień i konfigurowania uwierzytelniania w usłudze APNs przy użyciu utworzonego wcześniej certyfikatu powiadomień wypychanych **p12**. Jeśli chcesz użyć już utworzonego centrum powiadomień, możesz przejść do kroku 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p>Kliknij przycisk <b>Usługi powiadomień</b>, a następnie wybierz pozycję <b>Apple (APNs)</b>. Upewnij się, że zaznaczono pozycję <b>Certyfikat</b>, kliknij ikonę pliku i wybierz wyeksportowany wcześniej plik <b>p12</b>. Upewnij się, że określono prawidłowe hasło.</p>

<p>Upewnij się, że wybrano tryb <b>Piaskownica</b> na potrzeby tworzenia aplikacji. Tryb <b>Produkcja</b> należy wybrać wyłącznie wówczas, gdy chcesz wysyłać powiadomienia wypychane do użytkowników, którzy kupili Twoją aplikację w sklepie.</p>
</li>
</ol>

&emsp;&emsp;&emsp;&emsp;![Konfigurowanie usługi APNs w witrynie Azure Portal][6]

&emsp;&emsp;&emsp;&emsp;![Konfigurowanie certyfikacji APNs w witrynie Azure Portal][7]

Twoje centrum powiadomień jest teraz skonfigurowane do pracy z usługą APNs i uzyskano parametry połączenia służące do rejestrowania aplikacji w celu wysyłania powiadomień wypychanych.

## <a name="connect-your-app-to-the-notification-hub"></a>Łączenie aplikacji z centrum powiadomień
#### <a name="create-a-new-project"></a>Tworzenie nowego projektu
1. W programie Visual Studio utwórz nowy projekt dla systemu iOS i wybierz szablon **Single View Application** (Aplikacja z jednym widokiem), a następnie kliknij przycisk **Next** (Dalej).
   
     ![Visual Studio — wybieranie typu aplikacji][31]

2. Wprowadź nazwę aplikacji i identyfikator organizacji, kliknij przycisk **Dalej**, a następnie pozycję **Utwórz**

3. W widoku rozwiązania kliknij dwukrotnie plik *Into.plist* i w obszarze **Tożsamość** upewnij się, że identyfikator pakietu jest zgodny z użytym podczas tworzenia profilu aprowizacji. W obszarze **Podpisywanie** upewnij się, że konto dewelopera jest wybrane w obszarze **Zespół**, opcja „Automatically manage signing” (Automatycznie zarządzaj podpisywaniem) jest zaznaczona, a Twój certyfikat podpisywania i profil aprowizacji zostały automatycznie wybrane.

    ![Visual Studio — konfiguracja aplikacji systemu iOS][32]

4. Dodaj pakiet składnika Azure Messaging. W widoku Solution kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj** > **Dodawanie pakietów NuGet**. Wyszukaj pakiet **Xamarin.Azure.NotificationHubs.iOS** i dodaj go do projektu.

5. Dodaj nowy plik do klasy, nadaj mu nazwę **Constants.cs**, dodaj następujące zmienne i zastąp symbole zastępcze literału ciągu przy użyciu *nazwy centrum* i zanotowanej wcześniej wartości *DefaultListenSharedAccessSignature*.
   
    ```csharp
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
    ```

6. W pliku **AppDelegate.cs** dodaj następującą instrukcję using:
   
    ```csharp
        using WindowsAzure.Messaging;
    ```

7. Zadeklaruj wystąpienie **SBNotificationHub**:
   
    ```csharp
        private SBNotificationHub Hub { get; set; }
    ```

8. W pliku **AppDelegate.cs** zaktualizuj metodę **FinishedLaunching()** zgodnie z poniższym kodem:
   
    ```csharp
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());
   
                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }
   
            return true;
        }
    ```

9. Zastąp metodę **RegisteredForRemoteNotifications()** w pliku **AppDelegate.cs**:
   
    ```csharp
        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);
   
            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }
   
                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }
    ```

10. Zastąp metodę **ReceivedRemoteNotification()** w pliku **AppDelegate.cs**:
   
    ```csharp
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
    ```

11. Utwórz następującą metodę **ProcessNotification()** w pliku **AppDelegate.cs**:
   
    ```csharp
        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
   
                string alert = string.Empty;
   
                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();
   
                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }
    ```
   > [!NOTE]
   > Możesz również opcjonalnie zastąpić metodę **FailedToRegisterForRemoteNotifications()** w celu obsługi sytuacji takich jak brak połączenia z siecią. Jest to szczególnie istotne w przypadku, gdy użytkownik może uruchomić aplikację w trybie offline (na przykład w trybie samolotowym), a chcesz obsługiwać scenariusze powiadomień push specyficzne dla aplikacji.
  

12. Uruchom aplikację na urządzeniu.

## <a name="sending-test-push-notifications"></a>Wysyłanie testowych powiadomień push
Odbieranie powiadomień w aplikacji możesz przetestować za pomocą opcji *Wysyłanie testowe* w witrynie [Azure Portal]. Powoduje to wysłanie testowego powiadomienia push na urządzenie.

![Witryna Azure Portal — wysyłanie testowe][30]

Powiadomienia wypychane są zwykle wysyłane za pośrednictwem usługi wewnętrznej bazy danych, takiej jak Mobile Apps czy ASP.NET, przy użyciu zgodnej biblioteki. Jeśli biblioteka nie jest dostępna w danym zapleczu, powiadomienia można również wysyłać bezpośrednio za pomocą interfejsu API REST.

Zalecanym następnym krokiem jest zapoznanie się z samouczkiem [Wysyłanie powiadomień push do użytkowników przy użyciu usługi Notification Hubs](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) dotyczącym wysyłania powiadomień przy użyciu zaplecza ASP.NET. Można używać następujących metod wysyłania powiadomień:

Poniższa lista zawiera kilka innych samouczków, z którymi warto się zapoznać, planując wysyłanie powiadomień:
* Interfejs REST: powiadomienia push mogą być obsługiwane na dowolnej platformie zaplecza za pomocą [interfejsu REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **Zestaw SDK .NET dla usługi Microsoft Azure Notification Hubs**: w menedżerze pakietów NuGet dla programu Visual Studio uruchom polecenie [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* Node.js: [Jak używać usługi Notification Hubs z poziomu środowiska Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* Java/PHP\*\*: aby zapoznać się z przykładem wysyłania powiadomień push przy użyciu interfejsów API REST, zobacz „Jak używać usługi Notification Hubs za pomocą języka Java/PHP” ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

## <a name="next-steps"></a>Następne kroki
W tym prostym przykładzie wysłano powiadomienia wypychane do wszystkich urządzeń z systemem iOS. Aby skierować je do określonych użytkowników, zapoznaj się z samouczkiem [Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]. Jeśli chcesz podzielić użytkowników na grupy zainteresowań, zobacz [Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]. Aby uzyskać więcej informacji na temat korzystania z usługi Notification Hubs, zobacz [Wskazówki dotyczące usługi Notification Hubs] oraz [Poradnik dotyczący usługi Notification Hubs dla systemu iOS].

<!-- Images. -->
[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png
[32]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-app-settings.png



<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Windows Azure Messaging Framework]: http://go.microsoft.com/fwlink/?LinkID=799698&clcid=0x409
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[Wskazówki dotyczące usługi Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Poradnik dotyczący usługi Notification Hubs dla systemu iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456
[Visual Studio dla komputerów Mac]: https://www.visualstudio.com/vs/visual-studio-mac/
[Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]: /manage/services/notification-hubs/notify-users-aspnet
[Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]: /manage/services/notification-hubs/breaking-news-dotnet

[Local and Push Notification Programming Guide]:https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs
[Azure portal]: https://portal.azure.com
