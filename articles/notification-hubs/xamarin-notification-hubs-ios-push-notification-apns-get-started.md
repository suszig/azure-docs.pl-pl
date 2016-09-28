<properties
    pageTitle="Wysyłanie powiadomień wypychanych do aplikacji platformy Xamarin dla systemu iOS przy użyciu usługi Notification Hubs | Microsoft Azure"
    description="Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji platformy Xamarin dla systemu iOS przy użyciu usługi Azure Notification Hubs."
    services="notification-hubs"
    keywords="powiadomienia wypychane w systemie ios, wiadomości wypychane, powiadomienia wypychane, wiadomość wypychana"
    documentationCenter="xamarin"
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="wesmc"/>


# Wysyłanie powiadomień wypychanych do aplikacji platformy Xamarin dla systemu iOS przy użyciu usługi Notification Hubs

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##Omówienie
> [AZURE.IMPORTANT] Do wykonania kroków tego samouczka potrzebne jest aktywne konto platformy Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji dla systemu iOS przy użyciu usługi Azure Notification Hubs.
Utworzysz pustą aplikację platformy Xamarin.iOS służącą do odbierania powiadomień wypychanych przy użyciu usługi [Apple Push Notification Service (APNS)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). Po zakończeniu będzie można za pomocą centrum powiadomień wysyłać powiadomienia wypychane do wszystkich urządzeń z tą aplikacją. Gotowy kod jest dostępny w przykładowej aplikacji [NotificationHubs][GitHub].

W tym samouczku został omówiony prosty scenariusz wysyłania powiadomień wypychanych przy użyciu usługi Notification Hubs.

##Wymagania wstępne

Dla tego samouczka wymagane są następujące elementy:

+ [Program Xcode 6.0][Zainstaluj program Xcode]
+ Urządzenie zgodne z systemem iOS 7.0 (lub nowszą wersją)
+ Członkostwo w programie dla deweloperów systemu iOS
+ [Program Xamarin Studio]

   > [AZURE.NOTE] Ze względu na wymagania dotyczące konfiguracji powiadomień wypychanych w systemie iOS należy wdrożyć i przetestować aplikację przykładową na fizycznym urządzeniu z systemem iOS (telefonie iPhone lub tablecie iPad), a nie w symulatorze.

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usług Notification Hubs dotyczących aplikacji platformy Xamarin.iOS.


[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]


##Konfigurowanie centrum powiadomień

W tej sekcji opisano kroki tworzenia nowego centrum powiadomień i konfigurowania uwierzytelniania w usłudze APNs przy użyciu utworzonego przez Ciebie certyfikatu powiadomień wypychanych **.p12**. Jeśli chcesz użyć już utworzonego centrum powiadomień, możesz przejść do kroku 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li>
<p>Ponieważ wymagane jest skonfigurowanie połączenia usługi APNS, w witrynie Azure Portal otwórz ustawienia usługi Notification Hub, kliknij pozycję <b>Usługi powiadomień</b>, a następnie kliknij pozycję <b>Apple (APNS)</b> na liście. Następnie kliknij pozycję <b>Przekaż certyfikat</b> i wybierz uprzednio wyeksportowany certyfikat <b>.p12</b> oraz hasło tego certyfikatu.</p>
<p>Pamiętaj o wybraniu trybu <b>Piaskownica</b>, ponieważ powiadomienia wypychane będą wysyłane w środowisku projektowym. Ustawienie <b>Produkcja</b> należy wybrać wyłącznie wówczas, gdy chcesz wysyłać powiadomienia wypychane do użytkowników, którzy kupili już Twoją aplikację w sklepie.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)


Twoje centrum powiadomień jest teraz skonfigurowane do pracy z usługą APNs i uzyskano parametry połączenia służące do rejestrowania aplikacji w celu wysyłania powiadomień wypychanych.


##Łączenie aplikacji z centrum powiadomień

#### Tworzenie nowego projektu

1. W programie Xamarin Studio utwórz nowy projekt dla systemu iOS i wybierz szablon **Unified API (Standaryzowany interfejs API)** > **Single View Application (Aplikacja z jednym widokiem)**.

    ![Xamarin Studio — wybieranie typu aplikacji][31]

2. Dodaj odniesienie do składnika Azure Messaging. W widoku Solution (Rozwiązanie) kliknij prawym przyciskiem myszy folder **Components** (Składniki) w projekcie i wybierz polecenie **Get More Components** (Uzyskaj więcej składników). Wyszukaj składnik **Azure Messaging** i dodaj go do projektu.

3. W pliku **AppDelegate.cs** dodaj następującą instrukcję using:

        using WindowsAzure.Messaging;

4. Zadeklaruj wystąpienie **SBNotificationHub**:

        private SBNotificationHub Hub { get; set; }

5. Utwórz klasę **Constants.cs** z następującymi zmiennymi:

        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";


6. W pliku **AppDelegate.cs** zaktualizuj metodę **FinishedLaunching()** zgodnie z poniższym kodem:

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

7. Zastąp metodę **RegisteredForRemoteNotifications()** w pliku **AppDelegate.cs**:

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

8. Zastąp metodę **ReceivedRemoteNotification()** w pliku **AppDelegate.cs**:

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }

9. Utwórz następującą metodę **ProcessNotification()** w pliku **AppDelegate.cs**:

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

    > [AZURE.NOTE] Możesz również opcjonalnie zastąpić metodę **FailedToRegisterForRemoteNotifications()** w celu obsługi sytuacji takich jak brak połączenia z siecią. Jest to szczególnie istotne w przypadku, gdy użytkownik może uruchomić aplikację w trybie offline (na przykład w trybie samolotowym), a chcesz obsługiwać scenariusze powiadomień wypychanych specyficzne dla aplikacji.


10. Uruchom aplikację na urządzeniu.


## Wysyłanie powiadomień wypychanych


Możesz przetestować odbieranie powiadomień wypychanych w aplikacji, wysyłając powiadomienia w witrynie [Azure Portal] za pomocą funkcji **Wyślij testowe** w zestawie narzędzi **Rozwiązywanie problemów** na stronie centrum powiadomień, jak pokazano na poniższym zrzucie ekranu.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Powiadomienia wypychane są zwykle wysyłane za pośrednictwem usługi zaplecza, takiej jak Mobile Services czy ASP.NET, z użyciem zgodnej biblioteki. Można również wysyłać powiadomienia wypychane bezpośrednio za pomocą interfejsu API REST, jeśli biblioteka nie jest dostępna w danym scenariuszu. 

W tym samouczku dla uproszczenia przedstawiono testowanie aplikacji klienckiej przez wysyłanie powiadomień za pomocą zestawu SDK .NET dla usługi Notification Hubs w aplikacji konsoli, a nie za pomocą usługi zaplecza. Zalecanym następnym krokiem jest zapoznanie się z samouczkiem [Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) dotyczącym wysyłania powiadomień przy użyciu zaplecza ASP.NET. Można używać następujących metod wysyłania powiadomień:

* **Interfejs REST**: powiadomienia wypychane mogą być obsługiwane na dowolnej platformie zaplecza za pomocą [interfejsu REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Zestaw SDK .NET dla usługi Microsoft Azure Notification Hubs**: w menedżerze pakietów NuGet dla programu Visual Studio uruchom polecenie [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js**: [Jak używać usługi Notification Hubs z poziomu środowiska Node.js](notification-hubs-nodejs-push-notification-tutorial.md).

* **Azure Mobile Services**: aby zapoznać się z przykładem wysyłania powiadomień wypychanych z poziomu zaplecza usługi Azure Mobile Services zintegrowanego z usługą Notification Hubs, zobacz „Wprowadzenie do powiadomień wypychanych w usługach Mobile Services” ([Zaplecze .NET](../mobile-services/mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md) | [Zaplecze JavaScript](../mobile-services/mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md)).

* **Java/PHP**: aby zapoznać się z przykładem wysyłania powiadomień wypychanych przy użyciu interfejsów API REST, zobacz „Jak używać usługi Notification Hubs za pomocą języka Java/PHP” ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).


####(Opcjonalnie) Wysyłanie powiadomień wypychanych z poziomu aplikacji konsoli .NET

W tej sekcji wyślemy powiadomienia wypychane za pomocą prostej aplikacji konsoli .NET. Na potrzeby tego przykładu przeniesiemy się do środowiska projektowego systemu Windows, w którym jest już zainstalowany program Visual Studio.

1. W programie Visual Studio utwórz nową aplikację konsoli języka Visual C#:

    ![Visual Studio — tworzenie nowej aplikacji konsoli][213]

2. W programie Visual Studio kliknij kolejno pozycje **Narzędzia**, **Menedżer pakietów NuGet**, **Konsola menedżera pakietów**.

    Powinna zostać wyświetlona Konsola menedżera pakietów zadokowana do dolnej części obszaru roboczego programu Visual Studio.

3. W oknie Konsola menedżera pakietów ustaw nowy projekt aplikacji konsoli jako **Projekt domyślny**, a następnie w oknie konsoli uruchom następujące polecenie:

        Install-Package Microsoft.Azure.NotificationHubs

    Spowoduje to dodanie odwołania do zestawu SDK usługi Azure Notification Hubs z użyciem <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakietu NuGet Microsoft.Azure.Notification Hubs</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Otwórz plik `Program.cs` i dodaj następującą instrukcję `using` umożliwiającą używanie klas i funkcji platformy Azure w ramach klasy głównej:

        using Microsoft.Azure.NotificationHubs;

3. W klasie `Program` dodaj następującą metodę (pamiętaj o wstawieniu odpowiednich **parametrów połączenia** i **nazwy centrum**):

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }

4. Dodaj następujące wiersze do metody `Main`:

         SendNotificationAsync();
         Console.ReadLine();

5. Naciśnij klawisz F5, aby uruchomić aplikację. W ciągu kilku sekund na urządzeniu powinno zostać wyświetlone powiadomienie wypychane. Niezależnie od tego, czy używasz połączenia Wi-Fi, czy sieci danych komórkowych, upewnij się, że urządzenie ma aktywne połączenie z Internetem.

Wszystkie możliwe ładunki można znaleźć w [Podręczniku programowania powiadomień lokalnych i wypychanych] firmy Apple.

####(Opcjonalnie) Wysyłanie powiadomień wypychanych z poziomu usługi mobilnej

W tej sekcji wyślemy powiadomienia wypychane za pomocą usługi mobilnej, za pośrednictwem skryptu węzła.

Aby wysłać powiadomienie przy użyciu usługi mobilnej, wykonaj czynności opisane w temacie [Wprowadzenie do usług Mobile Services], a następnie:

1. Zaloguj się do [klasyczny portal Azure] i wybierz usługę mobilną.

2. Wybierz kartę **Harmonogram** na górnym pasku.

    ![Klasyczny portal Azure — karta Harmonogram][215]

3. Utwórz nowe zadanie zaplanowane, wstaw nazwę i wybierz opcję **Na żądanie**.

    ![Klasyczny portal Azure — tworzenie nowego zadania][216]

4. Po utworzeniu zadania kliknij jego nazwę. Następnie kliknij kartę **Skrypt** na górnym pasku.

5. Wstaw następujący skrypt w funkcji harmonogramu. Pamiętaj o zastąpieniu tekstu zastępczego uzyskanymi wcześniej wartościami: nazwą centrum powiadomień i parametrami połączenia *DefaultFullSharedAccessSignature*. Kliknij pozycję **Zapisz**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );


6. Kliknij pozycję **Uruchom raz** na dolnym pasku. Na urządzeniu powinien zostać wyświetlony alert.

##Następne kroki

W tym prostym przykładzie wysłano powiadomienia wypychane do wszystkich urządzeń z systemem iOS. Aby skierować je do określonych użytkowników, zapoznaj się z samouczkiem [Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]. Jeśli chcesz podzielić użytkowników na grupy zainteresowań, zobacz [Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]. Aby uzyskać więcej informacji na temat korzystania z usługi Notification Hubs, zobacz [Wskazówki dotyczące usługi Notification Hubs] oraz [Poradnik dotyczący usługi Notification Hubs dla systemu iOS].


<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Zestaw SDK usługi Mobile Services systemu iOS]: http://go.microsoft.com/fwLink/?LinkID=266533
[Strona przesyłania aplikacji]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[Moje aplikacje]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Zestaw Live SDK dla systemu Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Wprowadzenie do usług Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[klasyczny portal Azure]: https://manage.windowsazure.com/
[Wskazówki dotyczące usługi Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Poradnik dotyczący usługi Notification Hubs dla systemu iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Zainstaluj program Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Portal aprowizowania dla systemu iOS]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]: /manage/services/notification-hubs/notify-users-aspnet
[Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]: /manage/services/notification-hubs/breaking-news-dotnet

[Podręczniku programowania powiadomień lokalnych i wypychanych]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Składnik Azure Mobile Services]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Program Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure Portal]: https://portal.azure.com



<!--HONumber=Sep16_HO3-->


