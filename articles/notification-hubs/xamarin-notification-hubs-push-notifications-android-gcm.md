---
title: "Wprowadzenie do usługi Notification Hubs dla aplikacji platformy Xamarin.Android | Microsoft Docs"
description: "Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji platformy Xamarin Android przy użyciu usługi Azure Notification Hubs."
author: ysxu
manager: erikre
editor: 
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 09/25/2017
ms.author: yuaxu
ms.openlocfilehash: f9fef96b71e0db7b15ff5208e9bd1a0b4ecf7211
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Wprowadzenie do usługi Notification Hubs dla systemu Android na platformie Xamarin
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Omówienie
Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji platformy Xamarin.Android przy użyciu usługi Azure Notification Hubs. Utworzysz pustą aplikację platformy Xamarin.Android służącą do odbierania powiadomień wypychanych przy użyciu usługi Google Cloud Messaging (GCM). Po zakończeniu będzie można za pomocą centrum powiadomień wysyłać powiadomienia wypychane do wszystkich urządzeń z tą aplikacją. Gotowy kod jest dostępny w przykładowej aplikacji [NotificationHubs][GitHub].

W tym samouczku został omówiony prosty scenariusz wysyłania przy użyciu usługi Notification Hubs.

## <a name="before-you-begin"></a>Przed rozpoczęciem
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Kompletny kod dla tego samouczka można znaleźć w witrynie GitHub [tutaj](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).

## <a name="prerequisites"></a>Wymagania wstępne
Dla tego samouczka wymagane są następujące elementy:

* Program Visual Studio z platformą Xamarin w systemie Windows lub program Xamarin Studio w systemie Mac OS X. Kompletne instrukcje instalacji można znaleźć w temacie [Konfigurowanie i instalowanie oprogramowania Visual Studio i Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
* Aktywne konto Google
* [Składnik Azure Messaging]
* [Składnik Google Cloud Messaging Client]

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usługi Notification Hubs dotyczących aplikacji platformy Xamarin.Android.

> [!IMPORTANT]
> Do wykonania kroków tego samouczka potrzebne jest aktywne konto platformy Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).
> 
> 

## <a name="enable-google-cloud-messaging"></a>Włączanie usługi Google Cloud Messaging
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a>Konfigurowanie centrum powiadomień
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="5">

<li><p>Wybierz kartę <b>Konfiguracja</b> w górnej części ekranu, wprowadź wartość <b>Klucz interfejsu API</b> uzyskaną w poprzedniej sekcji, a następnie wybierz pozycję <b>Zapisz</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)

Centrum powiadomień jest teraz skonfigurowane do pracy z usługą GCM i uzyskano parametry połączenia do rejestrowania aplikacji zarówno w celu odbierania, jak i wysyłania powiadomień wypychanych.

## <a name="connect-your-app-to-the-notification-hub"></a>Łączenie aplikacji z centrum powiadomień
Najpierw utwórz nowy projekt.
1. W programie Xamarin Studio wybierz pozycje **New Solution (Nowe rozwiązanie)** > **Android App (Aplikacja systemu Android)**, a następnie kliknij przycisk **Next (Dalej)**.
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Wprowadź nazwę aplikacji w polu **App Name** i identyfikator w polu **Identifier**. W obszarze **Target Platforms (Platformy docelowe)** wybierz platformy, które mają być obsługiwane, a następnie wybierz kolejno pozycje **Next (Dalej)** i **Create (Utwórz)**.
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)

    Spowoduje to utworzenie nowego projektu dla systemu Android.

3. Otwórz właściwości projektu, klikając utworzony projekt prawym przyciskiem myszy w widoku Solution (Rozwiązanie) i wybierając pozycję **Options** (Opcje). Wybierz pozycję **Android Application** (Aplikacja dla systemu Android) w sekcji **Build** (Kompilacja).
   
    Upewnij się, że pierwsza litera w polu **Package name** (Nazwa pakietu) jest mała.
   
   > [!IMPORTANT]
   > Nazwa pakietu musi zaczynać się od małej litery. W przeciwnym razie podczas rejestrowania elementów **BroadcastReceiver** i **IntentFilter** dla powiadomień wypychanych w kolejnym etapie zostaną zwrócone błędy manifestu aplikacji.
   > 
   > 
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)
4. Opcjonalnie możesz ustawić wartość w polu **Minimum Android version** (Minimalna wersja systemu Android) na inny poziom interfejsu API.
5. Opcjonalnie możesz ustawić wartość w polu **Target Android version** (Docelowa wersja systemu Android) na inną docelową wersję interfejsu API (wymagany poziom 8 lub wyższy).
6. Wybierz przycisk **OK** i zamknij okno dialogowe Project Options (Opcje projektu).

### <a name="add-the-required-components-to-your-project"></a>Dodawanie wymaganych składników do projektu
Składnik Google Cloud Messaging Client dostępny w magazynie składników Xamarin Component Store upraszcza proces obsługi powiadomień wypychanych na platformie Xamarin.Android.

1. Kliknij prawym przyciskiem myszy folder Components (Składniki) w aplikacji platformy Xamarin.Android i wybierz polecenie **Get More Components** (Uzyskaj więcej składników).
2. Wyszukaj składnik **Azure Messaging** i dodaj go do projektu.
3. Wyszukaj składnik **Google Cloud Messaging Client** i dodaj go do projektu.

### <a name="set-up-notification-hubs-in-your-project"></a>Konfigurowanie centrów powiadomień w projekcie
1. Zbierz następujące informacje dotyczące aplikacji dla systemu Android i centrum powiadomień:
   
   * **GoogleProjectNumber**: wartość numeru projektu, którą znajdziesz na stronie przeglądu swojej aplikacji w portalu Google Developer Portal. Tę wartość zanotowano wcześniej — po utworzeniu aplikacji w portalu.
   * **Listen connection string (Parametry połączenia nasłuchiwania)**: na pulpicie nawigacyjnym w [klasycznej witrynie Azure Portal] kliknij pozycję **Wyświetl parametry połączeń**. Skopiuj parametr połączenia *DefaultListenSharedAccessSignature* dla tej wartości.
   * **Hub name**: nazwa centrum wprowadzona w [klasycznej witrynie Azure Portal]. Na przykład *moje_centrum_powiadomien_2*.
     
2. Utwórz klasę **Constants.cs** w projekcie Xamarin i zdefiniuj w tej klasie następujące wartości stałych. Zastąp tekst zastępczy własnymi wartościami.
     
        public static class Constants
        {
     
           public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
        }
3. W pliku **MainActivity.cs** dodaj następujące instrukcje using:
   
        using Android.Util;
        using Gcm.Client;
4. Dodaj do klasy `MainActivity` zmienną instance w celu wyświetlania okna dialogowego alertu podczas działania aplikacji:
   
        public static MainActivity instance;
5. Utwórz następującą metodę w klasie **MainActivity**:
   
        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);
   
            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }
6. W ramach metody `OnCreate` w pliku **MainActivity.cs** zainicjuj zmienną `instance` i dodaj wywołanie `RegisterWithGCM`:
   
        protected override void OnCreate (Bundle bundle)
        {
            instance = this;
   
            base.OnCreate (bundle);
   
            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);
   
            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);
   
            RegisterWithGCM();
        }
7. Utwórz nową klasę: **MyBroadcastReceiver**.
   
   > [!NOTE]
   > Poniżej znajdziesz instrukcje tworzenia klasy **BroadcastReceiver** krok po kroku. Szybszą alternatywą dla ręcznego tworzenia pliku **MyBroadcastReceiver.cs** jest skorzystanie z pliku **GcmService.cs**, który można znaleźć w przykładowym projekcie platformy Xamarin.Android dołączonym do [przykładów aplikacji NotificationHubs][GitHub]. Dobrym sposobem na rozpoczęcie może być też zduplikowanie pliku **GcmService.cs** i zmiana nazw klas.
   > 
   > 
8. Dodaj następujące instrukcje using do pliku **MyBroadcastReceiver.cs** (odwołujące się do dodanego wcześniej składnika i zestawu):
   
        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;
9. W pliku **MyBroadcastReceiver.cs** dodaj następujące żądania uprawnień pomiędzy instrukcjami **using** a deklaracją **namespace**:
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
10. W pliku **MyBroadcastReceiver.cs** zmień klasę **MyBroadcastReceiver** zgodnie z poniższym kodem:
   
        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };
   
            public const string TAG = "MyBroadcastReceiver-GCM";
        }
11. Dodaj w pliku **MyBroadcastReceiver.cs** kolejną klasę o nazwie **PushHandlerService** pochodzącą od klasy **GcmServiceBase**. Pamiętaj o zastosowaniu do klasy atrybutu **Service**:
    
         [Service] // Must use the service tag
         public class PushHandlerService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }
             private NotificationHub Hub { get; set; }
    
             public PushHandlerService() : base(Constants.SenderID)
                {
                 Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
             }
         }
12. Klasa **GcmServiceBase** implementuje metody **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()** oraz **OnError()**. Klasa implementacji **PushHandlerService** musi zastępować te metody, które będą uruchamiane w odpowiedzi na interakcję z centrum powiadomień.
13. Zastąp metodę **OnRegistered()** w klasie **PushHandlerService**, używając następującego kodu:
    
         protected override void OnRegistered(Context context, string registrationId)
         {
             Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
             RegistrationID = registrationId;
    
             createNotification("PushHandlerService-GCM Registered...",
                                 "The device has been Registered!");
    
             Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                         context);
             try
             {
                 Hub.UnregisterAll(registrationId);
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
    
             //var tags = new List<string>() { "falcons" }; // create tags if you want
             var tags = new List<string>() {};
    
             try
             {
                 var hubRegistration = Hub.Register(registrationId, tags.ToArray());
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
         }
    
    > [!NOTE]
    > W ramach powyższego kodu metody **OnRegistered()** zwróć uwagę na możliwość określenia tagów do rejestracji dla określonych kanałów komunikacji.
    > 
    > 
14. Zastąp metodę **OnMessage** w klasie **PushHandlerService**, używając następującego kodu:
    
        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");
    
            var msg = new StringBuilder();
    
            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }
    
            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }
15. Dodaj następujące metody **createNotification** i **dialogNotify** do klasy **PushHandlerService** w celu powiadomienia użytkowników w przypadku otrzymania powiadomienia.
    
    > [!NOTE]
    > Projekt powiadomień w systemie Android w wersji 5.0 i nowszych znacząco różni się od tego używanego we wcześniejszych wersjach. W przypadku testowania tej funkcji w systemie Android 5.0 lub nowszym otrzymanie powiadomienia będzie wymagało uruchomienia aplikacji. Aby uzyskać więcej informacji, zobacz [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880) (Powiadomienia w systemie Android).
    > 
    > 
    
        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;
    
            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));
    
            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);
    
            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;
    
            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));
    
            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }
    
        protected void dialogNotify(String title, String message)
        {
    
            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }
16. Zastąp abstrakcyjne elementy członkowskie **OnUnRegistered()**, **OnRecoverableError()** i **OnError()**, aby umożliwić skompilowanie kodu:
    
        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);
    
            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }
    
        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);
    
            return base.OnRecoverableError (context, errorId);
        }
    
        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }

## <a name="run-your-app-in-the-emulator"></a>Uruchamianie aplikacji w emulatorze
W przypadku uruchamiania aplikacji w emulatorze upewnij się, że korzystasz z urządzenia wirtualnego systemu Android (Android Virtual Device, AVD) obsługującego interfejsy API Google.

> [!IMPORTANT]
> Aby otrzymywać powiadomienia wypychane, należy skonfigurować konto Google na urządzeniu AVD. W tym celu w emulatorze przejdź do pozycji **Settings (Ustawienia)** i kliknij polecenie **Add Account (Dodaj konto)**. Upewnij się też, że emulator ma połączenie z Internetem.
> 
> [!NOTE]
> Projekt powiadomień w systemie Android w wersji 5.0 i nowszych znacząco różni się od tego używanego we wcześniejszych wersjach. Aby uzyskać więcej informacji, zobacz [Android Notifications](http://go.microsoft.com/fwlink/?LinkId=615880) (Powiadomienia w systemie Android).
> 
> 

1. W obszarze **Tools (Narzędzia)** wybierz polecenie **Open Android Emulator Manager (Otwórz menedżera emulatora systemu Android)**, zaznacz urządzenie, a następnie kliknij przycisk **Edit (Edytuj)**.
   
      ![][18]
2. Wybierz pozycję **Google APIs (Interfejsy API Google)** w polu **Target (Element docelowy)**, a następnie wybierz pozycję **OK**.
   
      ![][19]
3. Na górnym pasku narzędzi wybierz pozycję **Run (Uruchom)**, a następnie wybierz aplikację. Spowoduje to uruchomienie emulatora i aplikacji.
   
   Aplikacja pobiera wartość *registrationId* z usługi GCM i rejestruje się w centrum powiadomień.

## <a name="send-notifications-from-your-backend"></a>Wysyłanie powiadomień z poziomu zaplecza
Odbieranie powiadomień w aplikacji możesz przetestować, wysyłając powiadomienia w [klasycznej witrynie Azure Portal] za pomocą karty Debugowanie w centrum powiadomień, jak pokazano na poniższym zrzucie ekranu.

![][30]

Powiadomienia wypychane są zwykle wysyłane w usłudze zaplecza, takiej jak Mobile Services czy ASP.NET, za pośrednictwem zgodnej biblioteki. Można również wysyłać powiadomienia bezpośrednio za pomocą interfejsu API REST, jeśli biblioteka nie jest dostępna w danym zapleczu.

Poniższa lista zawiera kilka innych samouczków, z którymi warto się zapoznać, planując wysyłanie powiadomień:

* ASP.NET: zobacz [Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs].
* Zestaw SDK Java usługi Azure Notification Hubs: zobacz [Jak używać usługi Notification Hubs za pomocą języka Java](notification-hubs-java-push-notification-tutorial.md), aby zapoznać się ze sposobem wysyłania powiadomień za pomocą języka Java. To rozwiązanie przetestowano w programie Eclipse do tworzenia aplikacji dla systemu Android.
* PHP: zobacz [Jak używać usługi Notification Hubs za pomocą języka PHP](notification-hubs-php-push-notification-tutorial.md).

W kolejnych podsekcjach samouczka wyślesz powiadomienia przy użyciu aplikacji konsoli .NET oraz przy użyciu usługi mobilnej za pośrednictwem skryptu węzła.

#### <a name="optional-send-notifications-by-using-a-net-app"></a>(Opcjonalnie) Wysyłanie powiadomień za pomocą aplikacji .NET
W tej sekcji wyślemy powiadomienia za pomocą aplikacji konsoli .NET.

1. Utwórz nową aplikację konsoli języka Visual C#:
   
      ![][20]
2. W programie Visual Studio wybierz kolejno pozycje **Narzędzia** > **Menedżer pakietów NuGet**, **Konsola menedżera pakietów**.
   
    Spowoduje to wyświetlenie Konsoli menedżera pakietów w programie Visual Studio.
3. W oknie Konsola menedżera pakietów ustaw nowy projekt aplikacji konsoli jako **Projekt domyślny**, a następnie w oknie konsoli uruchom następujące polecenie:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Spowoduje to dodanie odwołania do zestawu SDK usługi Azure Notification Hubs z użyciem <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakietu NuGet Microsoft.Azure.Notification Hubs</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Otwórz plik Program.cs i dodaj następującą instrukcję `using`:
   
        using Microsoft.Azure.NotificationHubs;
5. Dodaj następującą metodę w klasie `Program`. Zmień tekst zastępczy na parametry połączenia *DefaultFullSharedAccessSignature* i nazwę centrum z [klasycznej witrynie Azure Portal].
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }
6. Dodaj następujące wiersze do metody **Main**:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Naciśnij klawisz F5, aby uruchomić aplikację. W aplikacji powinno zostać odebrane powiadomienie.
   
      ![][21]

#### <a name="optional-send-notifications-by-using-a-mobile-service"></a>(Opcjonalnie) Wysyłanie powiadomień wypychanych z poziomu usługi mobilnej
1. Wykonaj czynności opisane w temacie [Wprowadzenie do usług Mobile Services].
2. Zaloguj się do [klasycznej witrynie Azure Portal] i wybierz usługę mobilną.
3. Wybierz kartę **Harmonogram** na górnym pasku.
   
      ![][22]
4. Utwórz nowe zadanie zaplanowane, wstaw nazwę i wybierz opcję **Na żądanie**.
   
      ![][23]
5. Po utworzeniu zadania wybierz jego nazwę. Następnie wybierz kartę **Skrypt** na górnym pasku.
6. Wstaw następujący skrypt w funkcji harmonogramu. Pamiętaj o zastąpieniu tekstu zastępczego uzyskanymi wcześniej wartościami: nazwą centrum powiadomień i parametrami połączenia *DefaultFullSharedAccessSignature*. Wybierz pozycję **Zapisz**.
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );
7. Wybierz pozycję **Uruchom raz** na dolnym pasku. W aplikacji powinno zostać odebrane wyskakujące powiadomienie.

## <a name="next-steps"></a>Następne kroki
W tym prostym przykładzie wysłano powiadomienia do wszystkich urządzeń z systemem Android. Aby skierować je do określonych użytkowników, zapoznaj się z samouczkiem [Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]. Jeśli chcesz podzielić użytkowników na grupy zainteresowań, zobacz [Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]. Aby uzyskać więcej informacji na temat korzystania z usługi Notification Hubs, zobacz [Wskazówki dotyczące usługi Notification Hubs] oraz [Poradnik dotyczący usługi Notification Hubs dla systemu Android].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Wprowadzenie do usług Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[klasycznej witrynie Azure Portal]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Wskazówki dotyczące usługi Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Poradnik dotyczący usługi Notification Hubs dla systemu Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]: /manage/services/notification-hubs/notify-users-aspnet
[Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Składnik Google Cloud Messaging Client]: http://components.xamarin.com/view/GCMClient/
[Składnik Azure Messaging]: http://components.xamarin.com/view/azure-messaging
