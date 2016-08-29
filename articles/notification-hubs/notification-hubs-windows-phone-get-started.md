<properties
    pageTitle="Wysyłanie powiadomień wypychanych do urządzeń z systemem Windows Phone przy użyciu usługi Azure Notification Hubs | Microsoft Azure"
    description="Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji platformy Silverlight dla systemu Windows Phone 8 lub Windows Phone 8.1 przy użyciu usługi Azure Notification Hubs."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="push notification,push notification,windows phone push"
    authors="wesmc7777"
    manager="dwrede"
    editor="dwrede"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="03/10/2016"
    ms.author="wesmc"/>

# Wysyłanie powiadomień wypychanych do urządzeń z systemem Windows Phone przy użyciu usługi Azure Notification Hubs

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##Omówienie

> [AZURE.NOTE] Do wykonania kroków tego samouczka potrzebne jest aktywne konto platformy Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).

Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji platformy Silverlight dla systemu Windows Phone 8 lub Windows Phone 8.1 przy użyciu usługi Azure Notification Hubs. Jeśli aplikacja ma być przeznaczona dla systemu Windows Phone 8.1 (bez użycia platformy Silverlight), dodatkowe informacje zawiera wersja dotycząca [aplikacji uniwersalnych systemu Windows](notification-hubs-windows-store-dotnet-get-started.md).
Korzystając z tego samouczka, utworzysz pustą aplikację dla systemu Windows Phone 8, która odbiera powiadomienia wypychane przy użyciu usługi powiadomień wypychanych firmy Microsoft (MPNS, Microsoft Push Notification Service). Po zakończeniu będzie można za pomocą centrum powiadomień wysyłać powiadomienia wypychane do wszystkich urządzeń z tą aplikacją.

> [AZURE.NOTE] Zestaw SDK usługi Notification Hubs dla systemu Windows Phone nie obsługuje używania usługi WNS (Windows Push Notification Service) z aplikacjami platformy Silverlight dla systemu Windows Phone 8.1. Aby używać usługi WNS (zamiast usługi MPNS) z aplikacjami platformy Silverlight dla systemu Windows Phone 8.1, postępuj zgodnie z instrukcjami w [samouczku dotyczącym usługi Notification Hubs dla aplikacji platformy Silverlight dla systemu Windows Phone], w którym zastosowano interfejsy API REST.

W tym samouczku został omówiony prosty scenariusz wysyłania przy użyciu usługi Notification Hubs.

##Wymagania wstępne

Dla tego samouczka wymagane są następujące elementy:

+ Program [Visual Studio 2012 Express for Windows Phone] lub nowsza wersja.

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usługi Notification Hubs dotyczących aplikacji dla systemu Windows Phone 8.

##Tworzenie centrum powiadomień

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Kliknij sekcję <b>Usługi powiadomień</b> (w obszarze <i>Ustawienia</i>), kliknij pozycję <b>Windows Phone (MPNS)</b>, a następnie kliknij pole wyboru <b>Włącz nieuwierzytelnione wypychanie</b>.</p>
</li>
</ol>

&emsp;&emsp;![Portal Azure — włączanie nieuwierzytelnionych powiadomień wypychanych](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Centrum zostało utworzone i jest skonfigurowane do wysyłania nieuwierzytelnionych powiadomień do urządzeń z systemem Windows Phone.

> [AZURE.NOTE] W tym samouczku używana jest usługa MPNS w trybie nieuwierzytelnionym. W trybie nieuwierzytelnionym usługi MPNS występują ograniczenia dotyczące powiadomień, które można wysyłać do poszczególnych kanałów. Usługa Notification Hubs obsługuje [tryb uwierzytelniony usługi MPNS](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx), umożliwiając przekazanie certyfikatu.

##Łączenie aplikacji z centrum powiadomień

1. W programie Visual Studio utwórz nową aplikację dla systemu Windows Phone 8.

    ![Visual Studio — Nowy projekt — Aplikacja dla systemu Windows Phone][13]

    W programie Visual Studio 2013 Update 2 lub nowszym zamiast tego utwórz aplikację platformy Silverlight dla systemu Windows Phone.

    ![Visual Studio — Nowy projekt — Pusta aplikacja — Windows Phone Silverlight][11]

2. W programie Visual Studio kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**.

    Zostanie wyświetlone okno dialogowe **Zarządzanie pakietami NuGet**.

3. Wyszukaj pakiet `WindowsAzure.Messaging.Managed` i kliknij pozycję **Zainstaluj**, a następnie zaakceptuj warunki użytkowania.

    ![Visual Studio — menedżer pakietów NuGet][20]

    Spowoduje to pobranie i zainstalowanie biblioteki Azure Messaging dla systemu Windows oraz dodanie odwołania do niej przy użyciu <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">pakietu NuGet WindowsAzure.Messaging.Managed</a>.

4. Otwórz plik App.xaml.cs i dodaj następujące instrukcje `using`:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;

5. Dodaj następujący kod w górnej części metody **Application_Launching** w pliku App.xaml.cs:

        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }

        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());

            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });

    >[AZURE.NOTE] Wartość **MyPushChannel** jest indeksem służącym do wyszukiwania istniejącego kanału w kolekcji [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx). Jeśli nie istnieje, utwórz nowy wpis o tej nazwie.
    
    Wstaw nazwę centrum i parametry połączenia o nazwie **DefaultListenSharedAccessSignature** uzyskane w poprzedniej sekcji.
    Ten kod pobiera identyfikator URI kanału dla aplikacji z usługi MPNS, a następnie rejestruje ten identyfikator URI kanału w centrum powiadomień. Gwarantuje również to, że identyfikator URI kanału jest rejestrowany w centrum powiadomień przy każdym uruchomieniu aplikacji.

    >[AZURE.NOTE]W tym samouczku do urządzenia wysyłane jest wyskakujące powiadomienie. W przypadku wysyłania powiadomienia na kafelku należy zamiast tego wywołać metodę **BindToShellTile** na kanale. W celu obsługi wyskakujących powiadomień oraz powiadomień na kafelku należy wywołać metodę **BindToShellTile** i **BindToShellToast**.

6. W Eksploratorze rozwiązań rozwiń węzeł **Właściwości**, otwórz plik `WMAppManifest.xml`, kliknij kartę **Możliwości** i upewnij się, że możliwość **ID_CAP_PUSH_NOTIFICATION** jest zaznaczona.

    ![Visual Studio — możliwości aplikacji dla systemu Windows Phone][14]

    Dzięki temu aplikacja może odbierać powiadomienia wypychane. Bez tego wszystkie próby wysłania powiadomienia wypychanego do aplikacji zakończą się niepowodzeniem.

7. Naciśnij klawisz `F5`, aby uruchomić aplikację.

    W aplikacji zostanie wyświetlony komunikat dotyczący rejestracji.

8. Zamknij aplikację.  

   >[AZURE.NOTE] Aby otrzymywać wyskakujące powiadomienia wypychane, aplikacja nie może być uruchomiona na pierwszym planie.

##Wysyłanie powiadomień wypychanych z poziomu zaplecza

Powiadomienia wypychane przy użyciu usługi Notification Hubs można wysyłać z poziomu dowolnego zaplecza za pośrednictwem publicznego <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">interfejsu REST</a>. W tym samouczku powiadomienia wypychane są wysyłane przy użyciu aplikacji konsolowej programu .NET. 

Aby uzyskać przykład sposobu wysyłania powiadomień wypychanych z zaplecza ASP.NET WebAPI zintegrowanego z usługą Notification Hubs, zobacz [Azure Notification Hubs Notify Users with .NET backend](./notification-hubs-aspnet-backend-windows-dotnet-notify-users.md) (Powiadamianie użytkowników przy użyciu usługi Azure Notification Hubs z poziomu zaplecza programu .NET).  

Aby zapoznać się z przykładem wysyłania powiadomień wypychanych przy użyciu interfejsów [API REST](https://msdn.microsoft.com/library/azure/dn223264.aspx), zobacz [How to use Notification Hubs from Java](./notification-hubs-java-backend-how-to.md) (Jak używać usługi Notification Hubs za pomocą języka Java) i [How to use Notification Hubs from PHP](./notification-hubs-php-backend-how-to.md) (Jak używać usługi Notification Hubs za pomocą języka PHP).

1. Kliknij prawym przyciskiem myszy rozwiązanie, wybierz polecenie **Dodaj** i pozycję **Nowy projekt...**, a następnie w obszarze **Visual C#** kliknij pozycję **Windows** i pozycję **Aplikacja konsolowa**, a następnie kliknij przycisk **OK**.

    ![Visual Studio — Nowy projekt — Aplikacja konsolowa][6]

    Spowoduje to dodanie nowej aplikacji konsolowej w języku Visual C# do rozwiązania. Można to również zrobić w oddzielnym rozwiązaniu.

4. Kliknij pozycję **Narzędzia**, kliknij pozycję **Menedżer pakietów biblioteki**, a następnie kliknij pozycję **Konsola menedżera pakietów**.

    Spowoduje to wyświetlenie konsoli menedżera pakietów.

5.  W oknie **Konsola menedżera pakietów** ustaw nowy projekt aplikacji konsoli jako **Projekt domyślny**, a następnie w oknie konsoli uruchom następujące polecenie:

        Install-Package Microsoft.Azure.NotificationHubs

    Spowoduje to dodanie odwołania do zestawu SDK usługi Azure Notification Hubs z użyciem <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakietu NuGet Microsoft.Azure.Notification Hubs</a>.

6. Otwórz plik `Program.cs` i dodaj następującą instrukcję `using`:

        using Microsoft.Azure.NotificationHubs;

6. W klasie `Program` dodaj następującą metodę:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }

    Zastąp symbol zastępczy `<hub name>` nazwą centrum powiadomień wyświetlaną w portalu. Ponadto zastąp symbol zastępczy parametrów połączenia parametrami połączenia o nazwie **DefaultFullSharedAccessSignature** uzyskanymi w sekcji „Konfigurowanie centrum powiadomień”.

    >[AZURE.NOTE]Upewnij się, że użyto parametrów połączenia z uprawnieniami dostępu **Pełne**, a nie **Nasłuchiwanie**. Parametry połączenia z uprawnieniami dostępu do nasłuchiwania nie mają uprawnień do wysyłania powiadomień wypychanych.

4. Dodaj następujący wiersz do metody `Main`:

         SendNotificationAsync();
         Console.ReadLine();

5. Z uruchomionym emulatorem systemu Windows Phone i zamkniętą aplikacją ustaw projekt aplikacji konsolowej jako domyślny projekt startowy, a następnie naciśnij klawisz `F5`, aby uruchomić aplikację.

    Otrzymasz wyskakujące powiadomienie wypychane. Naciśnięcie baneru wyskakującego powiadomienia spowoduje załadowanie aplikacji.

Wszystkie dozwolone ładunki można znaleźć w tematach dotyczących [wykazu powiadomień wyskakujących] i [wykazu kafelków] w witrynie MSDN.

##Następne kroki

W tym prostym przykładzie wysłano powiadomienia wypychane do wszystkich urządzeń z systemem Windows Phone 8. 

Aby skierować je do określonych użytkowników, zapoznaj się z samouczkiem [Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]. 

Jeśli chcesz podzielić użytkowników na grupy zainteresowań, zobacz [Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]. 

Dowiedz się więcej o sposobie użycia usługi Notification Hubs w temacie [Wskazówki dotyczące usługi Notification Hubs].



<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express for Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Wskazówki dotyczące usługi Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Tryb uwierzytelniony usługi MPNS]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md
[Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]: notification-hubs-windows-phone-send-breaking-news.md
[wykazu powiadomień wyskakujących]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[wykazu kafelków]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[samouczku dotyczącym usługi Notification Hubs dla aplikacji platformy Silverlight dla systemu Windows Phone]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp




<!----HONumber=Jun16_HO2-->


