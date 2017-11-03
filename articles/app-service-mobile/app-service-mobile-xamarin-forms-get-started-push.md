---
title: "Dodawanie powiadomień wypychanych do aplikacji platformy Xamarin.Forms | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak wysyłać powiadomienia wypychane obejmującego wiele platform do aplikacji platformy Xamarin.Forms przy użyciu usług Azure."
services: app-service\mobile
documentationcenter: xamarin
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: d9b1ba9a-b3f2-4d12-affc-2ee34311538b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: 912367636f1b26b3b07fbd5fe3fe8ed053218fd5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Dodawanie powiadomień wypychanych do aplikacji platformy Xamarin.Forms
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Omówienie
W tym samouczku, dodawanie powiadomień wypychanych do projektów, które powstały w wyniku [szybki start platformy Xamarin.Forms](app-service-mobile-xamarin-forms-get-started.md). Oznacza to, że powiadomienie wypychane zostanie wysłane do wszystkich klientów i platform, za każdym razem, gdy wstawieniu rekordu.

Jeśli nie używasz Projekt serwera pobrany szybki start, konieczne będzie pakiet rozszerzenia powiadomień wypychanych. Aby uzyskać więcej informacji, zobacz [pracować z serwera wewnętrznej bazy danych .NET SDK usługi Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Wymagania wstępne
Dla systemu iOS, konieczne będzie [członkostwo w programie dla deweloperów firmy Apple](https://developer.apple.com/programs/ios/) i urządzenie fizyczne z systemem iOS. [Symulatora systemu iOS nie obsługuje powiadomień wypychanych](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

## <a name="configure-hub"></a>Konfigurowanie Centrum powiadomień
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>Aktualizowanie projektów serwera do wysyłania powiadomień wypychanych
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-the-android-project-optional"></a>Konfigurowanie i uruchamianie projektu systemu Android (opcjonalnie)
Wykonaj tę sekcję, aby włączyć powiadomień wypychanych dla projektu platformy Xamarin.Forms Droid dla systemu Android.

### <a name="enable-firebase-cloud-messaging-fcm"></a>Włącz Firebase Cloud Messaging (FCM)
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm"></a>Konfigurowanie zaplecza aplikacji mobilnej do wysyłania żądań wypychanych przy użyciu FCM
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-to-the-android-project"></a>Dodawanie powiadomień wypychanych do projektu systemu Android
Na zapleczu skonfigurowano FCM można dodać składniki i kody klienta do rejestracji w usłudze FCM. Można również zarejestrować dla powiadomień wypychanych przy użyciu usługi Azure Notification Hubs przy użyciu zaplecza aplikacji mobilnych i otrzymywać powiadomienia.

1. W **Droid** projektu, kliknij prawym przyciskiem myszy **składniki** folder, a następnie kliknij przycisk **Uzyskaj więcej składników...** . Następnie wyszukaj **Google Cloud Messaging Client** składników i dodaj go do projektu. Ten składnik obsługuje powiadomień wypychanych dla projektu dla systemu Xamarin Android.
2. Otwórz plik projektu MainActivity.cs i dodaj następującą instrukcję u góry pliku:

        using Gcm.Client;
3. Dodaj następujący kod do **OnCreate** metody po wywołaniu **LoadApplication**:

        try
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }
4. Dodaj nową **CreateAndShowDialog** metody pomocnika, w następujący sposób:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }
5. Dodaj następujący kod do **MainActivity** klasy:

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    Udostępnia to bieżącego **MainActivity** wystąpienie, dlatego można wykonać w głównym wątku interfejsu użytkownika.
6. Inicjowanie `instance` zmiennej na początku **OnCreate** metody, w następujący sposób.

        // Set the current instance of MainActivity.
        instance = this;
7. Dodaj nowy plik klasy **Droid** projektu o nazwie `GcmService.cs`i upewnij się, że następujące **przy użyciu** instrukcje znajdują się w górnej części pliku:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;
8. Dodaj następujące żądania uprawnień w górnej części pliku, po **przy użyciu** instrukcje i przed **przestrzeni nazw** deklaracji.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
9. Dodaj następującą definicję klasy do przestrzeni nazw.

       [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
       public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
       {
           public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
       }

   > [!NOTE]
   > Zastąp **< PROJECT_NUMBER >** wcześniej zapisany numer projektu.    
   >
   >
10. Zamień wszystkie puste **GcmService** klasy z następującym kodem, który używa nowego odbiornika emisji:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }
11. Dodaj następujący kod do **GcmService** klasy. Przesłania **OnRegistered** obsługi zdarzeń i implementuje **zarejestrować** metody.

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

    Należy pamiętać, że ten kod zawiera `messageParam` parametru w rejestracji szablonu.
12. Dodaj następujący kod, który implementuje **OnMessage**:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    Obsługuje przychodzące powiadomienia i wysyła je do Menedżera powiadomień, który będzie wyświetlany.
13. **GcmServiceBase** wymaga również implementować **OnUnRegistered** i **OnError** metody obsługi, które można wykonać w następujący sposób:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Teraz wszystko jest gotowe testowych powiadomień wypychanych w aplikacji uruchomionych na urządzeniu z systemem Android lub emulator.

### <a name="test-push-notifications-in-your-android-app"></a>Testowych powiadomień wypychanych w aplikacji systemu Android
Pierwsze dwa kroki są wymagane tylko wtedy, gdy w przypadku testowania emulatora.

1. Upewnij się, że wdrażania lub debugowanie na urządzeniu wirtualnym, który ma ustawioną jako docelową, interfejsy API Google, jak pokazano poniżej, w Menedżerze urządzeń wirtualnych systemu Android.
2. Dodaj konto Google na urządzeniu z systemem Android, klikając **aplikacje** > **ustawienia** > **Dodaj konto**. Następnie postępuj zgodnie z monitami dodanie istniejącego konta Google na urządzeniu lub Utwórz nową.
3. W programie Visual Studio lub Xamarin Studio kliknij prawym przyciskiem myszy **Droid** projekt i kliknij przycisk **Ustaw jako projekt startowy**.
4. Kliknij przycisk **Uruchom** Aby skompilować projekt i uruchomić aplikację na urządzeniu z systemem Android lub w emulatorze.
5. W aplikacji wpisz zadania, a następnie kliknij przycisk plus (**+**) ikona.
6. Sprawdź, czy otrzyma powiadomienie po dodaniu elementu.

## <a name="configure-and-run-the-ios-project-optional"></a>Konfigurowanie i uruchamianie projektu iOS (opcjonalnie)
Ta sekcja dotyczy uruchamiania projektu Xamarin iOS dla urządzeń z systemem iOS. Jeśli nie pracujesz z urządzeniami z systemem iOS, możesz pominąć tę sekcję.

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-the-notification-hub-for-apns"></a>Konfigurowanie Centrum powiadomień dla usługi APNS
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Następnie skonfiguruj ustawienia projektu dla systemu iOS w programie Xamarin Studio lub Visual Studio.

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-to-your-ios-app"></a>Dodawanie powiadomień wypychanych do aplikacji systemu iOS
1. W **iOS** projektu, otwórz AppDelegate.cs i dodaj następującą instrukcję na początku pliku kodu.

        using Newtonsoft.Json.Linq;
2. W **AppDelegate** klasy, Dodaj zastąpienie **RegisteredForRemoteNotifications** zdarzenia do zarejestrowania dla powiadomień:

        public override void RegisteredForRemoteNotifications(UIApplication application,
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }
3. W **AppDelegate**, także dodać następujące zastąpienie dla **DidReceiveRemoteNotification** obsługi zdarzeń:

        public override void DidReceiveRemoteNotification(UIApplication application,
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Ta metoda obsługuje przychodzące powiadomienia, gdy aplikacja jest uruchomiona.
4. W **AppDelegate** klasy, Dodaj następujący kod, aby **FinishedLaunching** metody:

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Umożliwia to obsługę zdalnego powiadomienia i żądania push rejestracji.

Aplikacja jest teraz zaktualizowana do obsługi powiadomień wypychanych.

#### <a name="test-push-notifications-in-your-ios-app"></a>Testowych powiadomień wypychanych w aplikacji systemu iOS
1. Kliknij prawym przyciskiem myszy projekt iOS, a następnie kliknij przycisk **Ustaw jako projekt startowy**.
2. Naciśnij klawisz **Uruchom** przycisk lub **F5** w Visual Studio, aby skompilować projekt i uruchomić aplikację w urządzeniu z systemem iOS. Następnie kliknij przycisk **OK** do akceptowania powiadomień wypychanych.

   > [!NOTE]
   > Należy jawnie zaakceptować powiadomień wypychanych z aplikacji. To żądanie występuje tylko aplikacja jest uruchamiana po raz pierwszy.
   >
   >
3. W aplikacji wpisz zadania, a następnie kliknij przycisk plus (**+**) ikona.
4. Sprawdź, czy otrzyma powiadomienie, a następnie kliknij **OK** na odrzucenie powiadomienia.

## <a name="configure-and-run-windows-projects-optional"></a>Konfigurowanie i uruchamianie projekty systemu Windows (opcjonalnie)
Ta sekcja dotyczy uruchamiania aplikacji platformy Xamarin.Forms i WinPhone81 projektów dla urządzeń z systemem Windows. Te kroki również obsługiwać projekty systemu Windows platformy Uniwersalnej. Jeśli nie pracujesz z urządzeniami z systemem Windows, możesz pominąć tę sekcję.

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a>Rejestrowanie aplikacji systemu Windows dla powiadomień wypychanych z powiadomienia usługi WNS (Windows)
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-the-notification-hub-for-wns"></a>Konfigurowanie Centrum powiadomień w przypadku usługi WNS
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-to-your-windows-app"></a>Dodawanie powiadomień wypychanych do aplikacji systemu Windows
1. W programie Visual Studio Otwórz **App.xaml.cs** w systemie Windows projektu i dodaj następujące instrukcje.

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Zastąp `<your_TodoItemManager_portable_class_namespace>` z przestrzenią nazw przenośne projektu zawierającego `TodoItemManager` klasy.
2. W pliku App.xaml.cs Dodaj następujące **InitNotificationsAsync** metody:

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS =
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Ta metoda pobiera kanału powiadomień wypychanych i rejestruje szablonu, aby otrzymywać powiadomienia szablonu z Centrum powiadomień. Powiadomienie szablonu, który obsługuje *messageParam* będą dostarczane do tego klienta.
3. W pliku App.xaml.cs aktualizacji **OnLaunched** definicję metody obsługi zdarzeń, dodając `async` modyfikator. Następnie dodaj następujący wiersz kodu na końcu metody:

        await InitNotificationsAsync();

    Dzięki temu, że rejestracji powiadomień wypychanych jest tworzony lub odświeżyć za każdym razem, gdy aplikacja jest uruchamiana. Należy to zrobić, aby zagwarantować kanału wypychanych usługi WNS jest zawsze aktywna.  
4. Otwórz w Eksploratorze rozwiązań programu Visual Studio, **Package.appxmanifest** pliku, a następnie ustaw **wyskakujące funkcją** do **tak** w obszarze **powiadomienia**.
5. Tworzenie aplikacji i sprawdź, czy użytkownik nie ma błędów. Twoja aplikacja kliencka powinna teraz rejestrować się dla szablonu powiadomień z zaplecza aplikacji mobilnej. W tej sekcji należy powtórzyć dla każdego projektu systemu Windows w rozwiązaniu.

#### <a name="test-push-notifications-in-your-windows-app"></a>Testowych powiadomień wypychanych w aplikacji systemu Windows
1. W programie Visual Studio, kliknij prawym przyciskiem myszy projekt dla systemu Windows, a następnie kliknij przycisk **Ustaw jako projekt startowy**.
2. Kliknij przycisk **Uruchom**, aby skompilować projekt i uruchomić aplikację.
3. W aplikacji wpisz nazwę nowego zadania do wykonania, a następnie kliknij przycisk plus (**+**) ikonę, aby dodać go.
4. Sprawdź, czy otrzyma powiadomienie po dodaniu elementu.

## <a name="next-steps"></a>Następne kroki
Dodatkowe informacje na temat powiadomień wypychanych:

* [Diagnozowanie problemów powiadomień wypychanych](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Istnieją różne przyczyny, dlaczego powiadomienia mogą pobrać usunięte lub nie kończą na urządzeniach. W tym temacie przedstawiono sposób analizowania i ustalić główną przyczynę niepowodzenia powiadomień wypychanych.

Możesz również na jedno z następujących samouczków:

* [Dodawanie uwierzytelniania do aplikacji](app-service-mobile-xamarin-forms-get-started-users.md)  
  Dowiedz się, jak uwierzytelniać użytkowników aplikacji przy użyciu dostawcy tożsamości.
* [Włączanie synchronizacji w trybie offline dla aplikacji](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Dowiedz się, jak dodać obsługę aplikacji w trybie offline przy użyciu zaplecza funkcji Mobile Apps. Z synchronizacji w trybie offline, użytkownicy mogą korzystać z aplikacji mobilnej&mdash;przeglądanie, dodawanie lub modyfikowanie danych&mdash;nawet wtedy, gdy istnieje połączenie sieciowe.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
