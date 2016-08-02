<properties
    pageTitle="Rozpoczynanie pracy z usługą Azure Notification Hubs dla aplikacji ze Sklepu Windows | Microsoft Azure"
    description="Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji ze Sklepu Windows lub aplikacji dla systemu Windows Phone 8.1 (bez użycia platformy Silverlight) przy użyciu usługi Azure Notification Hubs."
    services="notification-hubs"
    documentationCenter="windows"
    authors="wesmc7777"
    manager="dwrede"
    editor="dwrede"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="03/28/2016"
    ms.author="wesmc"/>

# Rozpoczynanie pracy z usługą Azure Notification Hubs dla aplikacji ze Sklepu Windows

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##Omówienie

Korzystając z tego samouczka, dowiesz się, jak wysyłać powiadomienia wypychane do aplikacji ze Sklepu Windows lub aplikacji dla systemu Windows Phone 8.1 (bez użycia platformy Silverlight) przy użyciu usługi Azure Notification Hubs. Jeśli aplikacja ma być przeznaczona dla systemu Windows Phone 8.1 z platformą Silverlight, zapoznaj się z wersją dla systemu [Windows Phone](notification-hubs-windows-phone-get-started.md).
Korzystając z tego samouczka, utworzysz pustą aplikację ze Sklepu Windows, która odbiera powiadomienia wypychane przy użyciu usługi powiadomień WNS (Windows Push Notification Service). Po zakończeniu będzie można za pomocą centrum powiadomień wysyłać powiadomienia wypychane do wszystkich urządzeń z tą aplikacją.


## Przed rozpoczęciem

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Kompletny kod dla tego samouczka można znaleźć w witrynie GitHub [tutaj](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).



##Wymagania wstępne

Dla tego samouczka wymagane są następujące elementy:

+ Program Microsoft Visual Studio Express 2013 dla systemu Windows z aktualizacją Update 2 lub nowszy<br/>Ta wersja programu Visual Studio jest wymagana do utworzenia projektu aplikacji uniwersalnej. Jeśli chcesz tylko utworzyć aplikację ze Sklepu Windows, potrzebujesz programu Visual Studio 2012 Express dla systemu Windows 8.

+ Aktywne konto Sklepu Windows

+ Aktywne konto platformy Azure <br/>Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usługi Notification Hubs dotyczących aplikacji ze Sklepu Windows.

##Rejestrowanie aplikacji w Sklepie Windows

Aby wysyłać powiadomienia wypychane do aplikacji ze Sklepu Windows, należy skojarzyć aplikację ze Sklepem Windows. Następnie należy skonfigurować integrację centrum powiadomień z usługą WNS.

1. Jeśli nie zarejestrowano jeszcze aplikacji, przejdź do [Centrum deweloperów systemu Windows](http://go.microsoft.com/fwlink/p/?LinkID=266582), zaloguj się przy użyciu konta Microsoft, a następnie kliknij pozycję **Utwórz nową aplikację**.


2. Wpisz nazwę aplikacji i kliknij pozycję **Rezerwuj nazwę aplikacji**.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-win8-app-name.png)

    Spowoduje to utworzenie nowej rejestracji aplikacji w Sklepie Windows.

3. W programie Visual Studio utwórz nowy projekt aplikacji ze Sklepu w języku Visual C# za pomocą szablonu **Pusta aplikacja**.

    ![][2]

4. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt aplikacji ze Sklepu Windows i kliknij pozycję **Sklep**, a następnie kliknij pozycję **Skojarz aplikację ze sklepem...**.

    ![][3]

    Zostanie wyświetlony kreator **Kojarzenie aplikacji ze Sklepem Windows**.

5. W kreatorze kliknij pozycję **Zaloguj**, a następnie zaloguj się przy użyciu konta Microsoft.

6. Kliknij aplikację zarejestrowaną w kroku 2, kliknij przycisk **Dalej**, a następnie kliknij pozycję **Skojarz**.

    ![][4]

    Spowoduje to dodanie wymaganych informacji dotyczących rejestracji w Sklepie Windows do manifestu aplikacji.

7. (Opcjonalnie) Powtórz kroki 4–6 w projekcie aplikacji ze Sklepu Windows Phone.  

8. Ponownie na stronie [Centrum deweloperów systemu Windows](http://go.microsoft.com/fwlink/p/?LinkID=266582) dla nowej aplikacji kliknij pozycję **Usługi**, **Powiadomienia wypychane** i **Witryna usług Live** w obszarze **Usługi powiadomień WNS i Microsoft Azure Mobile Services**.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hubs-win8-app-live-services.png)

9. Na karcie **Ustawienia aplikacji** zanotuj wartości **Klucz tajny klienta** i **Identyfikator zabezpieczeń pakietu (SID)**.

    ![][6]

    > [AZURE.WARNING]
    Klucz tajny klienta i identyfikator SID pakietu są ważnymi poświadczeniami zabezpieczeń. Nie udostępniaj nikomu tych wartości ani nie rozpowszechniaj ich razem z aplikacją.

##Konfigurowanie centrum powiadomień

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">
<li><p>Wybierz kartę <b>Konfiguracja</b> w górnej części, wprowadź wartości <b>Klucz tajny klienta</b> i <b>Identyfikator SID pakietu</b> uzyskane z usługi WNS w poprzedniej sekcji, a następnie kliknij przycisk <b>Zapisz</b>.</p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)


Twoje centrum powiadomień jest teraz skonfigurowane do pracy z usługą WNS i uzyskano parametry połączenia na potrzeby rejestrowania aplikacji i wysyłania powiadomień.

##Łączenie aplikacji z centrum powiadomień

1. W programie Visual Studio kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**.

    Zostanie wyświetlone okno dialogowe **Zarządzanie pakietami NuGet**.

2. Wyszukaj pakiet `WindowsAzure.Messaging.Managed` i kliknij pozycję **Zainstaluj**, wybierz wszystkie projekty w rozwiązaniu i zaakceptuj warunki użytkowania.

    ![][20]

    Spowoduje to pobranie i zainstalowanie biblioteki Azure Messaging dla systemu Windows oraz dodanie odwołania do niej we wszystkich projektach przy użyciu <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">pakietu NuGet WindowsAzure.Messaging.Managed</a>.

3. Otwórz plik projektu App.xaml.cs i dodaj następujące instrukcje `using`. W projekcie dla platformy uniwersalnej ten plik znajduje się w folderze `<project_name>.Shared`.

        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;



4. Ponadto w pliku App.xaml.cs dodaj następującą definicję metody **InitNotificationsAsync** do klasy **App**:

        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            var hub = new NotificationHub("<hub name>", "<connection string with listen access>");
            var result = await hub.RegisterNativeAsync(channel.Uri);

            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }

        }

    Ten kod pobiera identyfikator URI kanału dla aplikacji z usługi WNS, a następnie rejestruje ten identyfikator URI kanału w centrum powiadomień.

    >[AZURE.NOTE]Zastąp symbol zastępczy „hub name” nazwą centrum powiadomień, która jest wyświetlana w [klasycznego portalu Azure] na karcie **Notification Hubs** (na przykład **mynotificationhub2** w poprzednim przykładzie). Zastąp również symbol zastępczy parametrów połączenia przy użyciu parametrów połączenia **DefaultListenSharedAccessSignature** uzyskanych w poprzedniej sekcji.

5. W górnej części programu obsługi zdarzeń **OnLaunched** w pliku App.xaml.cs dodaj następujące wywołanie do nowej metody **InitNotificationsAsync**:

        InitNotificationsAsync();

    Gwarantuje to, że identyfikator URI kanału jest rejestrowany w centrum powiadomień przy każdym uruchomieniu aplikacji.

6. W Eksploratorze rozwiązań kliknij dwukrotnie element **Package.appxmanifest** aplikacji ze Sklepu Windows, a następnie w obszarze **Powiadomienia** ustaw opcję **Możliwe do wyświetlania na pasku przewijania** na wartość **Tak**:

    ![][18]

    W menu **Plik** kliknij polecenie **Zapisz wszystko**.

7. (Opcjonalnie) Powtórz poprzedni krok w projekcie aplikacji ze Sklepu Windows Phone.

8. Naciśnij klawisz **F5**, aby uruchomić aplikację. Zostanie wyświetlone podręczne okno dialogowe zawierające klucz rejestracji.

    ![][19]

9. (Opcjonalnie) Powtórz poprzedni krok, aby uruchomić projekt dla systemu Windows Phone w celu zarejestrowania aplikacji na urządzeniu z systemem Windows Phone.



Aplikacja jest teraz gotowa do odbierania wyskakujących powiadomień.

##Wysyłanie powiadomień 

Odbieranie powiadomień w aplikacji możesz przetestować, wysyłając powiadomienia w [klasycznego portalu Azure] za pomocą karty Debugowanie w centrum powiadomień, jak pokazano na poniższym zrzucie ekranu.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-debug.png)

Powiadomienia wypychane są zwykle wysyłane za pośrednictwem usługi zaplecza, takiej jak Mobile Services czy ASP.NET, z użyciem zgodnej biblioteki. Powiadomienia można również wysyłać bezpośrednio za pomocą interfejsu API REST, jeśli biblioteka nie jest dostępna w danym zapleczu. 

W tym samouczku dla uproszczenia przedstawiono testowanie aplikacji klienckiej przez wysyłanie powiadomień za pomocą zestawu SDK .NET dla usługi Notification Hubs w aplikacji konsoli, a nie za pomocą usługi zaplecza. Zalecanym następnym krokiem jest zapoznanie się z samouczkiem [Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs] dotyczącym wysyłania powiadomień przy użyciu zaplecza ASP.NET. Można używać następujących metod wysyłania powiadomień:

* **Interfejs REST**: powiadomienia mogą być obsługiwane na dowolnej platformie zaplecza za pomocą [interfejsu REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Zestaw SDK .NET dla usługi Microsoft Azure Notification Hubs**: w menedżerze pakietów NuGet dla programu Visual Studio uruchom polecenie [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js**: [Jak używać usługi Notification Hubs z poziomu środowiska Node.js](notification-hubs-nodejs-how-to-use-notification-hubs.md).

* **Azure Mobile Apps**: aby zapoznać się z przykładem wysyłania powiadomień z poziomu aplikacji usługi Azure Mobile Apps zintegrowanej z usługą Notification Hubs, zobacz [Add push notifications for Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) (Dodawanie powiadomień wypychanych do aplikacji usługi Mobile Apps).

* **Java / PHP**: aby zapoznać się z przykładem wysyłania powiadomień przy użyciu interfejsów API REST, zobacz „How to use Notification Hubs from Java/PHP” (Jak używać usługi Notification Hubs za pomocą języka Java/PHP) — [Java](notification-hubs-java-backend-how-to.md) | [PHP](notification-hubs-php-backend-how-to.md).


## (Opcjonalnie) Wysyłanie powiadomień z poziomu aplikacji konsolowej


Aby wysłać powiadomienia za pomocą aplikacji konsolowej programu .NET, wykonaj następujące kroki. 

1. Kliknij prawym przyciskiem myszy rozwiązanie, wybierz polecenie **Dodaj** i pozycję **Nowy projekt...**, a następnie w obszarze **Visual C#** kliknij pozycję **Windows** i pozycję **Aplikacja konsolowa**, a następnie kliknij przycisk **OK**.

    ![][13]

    Spowoduje to dodanie nowej aplikacji konsolowej w języku Visual C# do rozwiązania. Można to również zrobić w oddzielnym rozwiązaniu.

2. W programie Visual Studio kliknij kolejno pozycje **Narzędzia**, **Menedżer pakietów NuGet**, **Konsola menedżera pakietów**.

    Spowoduje to wyświetlenie Konsoli menedżera pakietów w programie Visual Studio.

3. W oknie Konsola menedżera pakietów ustaw nowy projekt aplikacji konsoli jako **Projekt domyślny**, a następnie w oknie konsoli uruchom następujące polecenie:

        Install-Package Microsoft.Azure.NotificationHubs

    Spowoduje to dodanie odwołania do zestawu SDK usługi Azure Notification Hubs z użyciem <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakietu NuGet Microsoft.Azure.Notification Hubs</a>.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Otwórz plik Program.cs i dodaj następującą instrukcję `using`:

        using Microsoft.Azure.NotificationHubs;

5. Dodaj następującą metodę do klasy **Program**:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }

    Zastąp symbol zastępczy „hub name” nazwą centrum powiadomień, która jest wyświetlana w [klasycznego portalu Azure] na karcie **Notification Hubs**. Ponadto zastąp symbol zastępczy parametrów połączenia parametrami połączenia o nazwie **DefaultFullSharedAccessSignature** uzyskanymi w sekcji „Konfigurowanie centrum powiadomień”.

    >[AZURE.NOTE]Upewnij się, że użyto parametrów połączenia z uprawnieniami dostępu **Pełne**, a nie **Nasłuchiwanie**. Parametry połączenia z uprawnieniami dostępu do nasłuchiwania nie mają uprawnień do wysyłania powiadomień.

6. Dodaj następujące wiersze do metody **Main**:

         SendNotificationAsync();
         Console.ReadLine();

7. Kliknij prawym przyciskiem myszy projekt aplikacji konsolowej w programie Visual Studio, a następnie kliknij polecenie **Ustaw jako projekt startowy**, aby ustawić go jako projekt startowy. Następnie naciśnij klawisz **F5**, aby uruchomić aplikację.

    ![][14]

    Otrzymasz wyskakujące powiadomienie na wszystkich zarejestrowanych urządzeniach. Kliknięcie lub naciśnięcie baneru powiadomienia wyskakującego spowoduje załadowanie aplikacji.

Wszystkie obsługiwane ładunki można znaleźć w tematach dotyczących [wykazu powiadomień wyskakujących], [wykazu kafelków] i [omówienia znaczków] w witrynie MSDN.

##Następne kroki

W tym prostym przykładzie wysłano wyemitowane powiadomienia do wszystkich urządzeń z systemem Windows korzystających z portalu lub aplikacji konsolowej. Zalecanym następnym krokiem jest zapoznanie się z samouczkiem [Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]. Przedstawiono w nim sposób wysyłania powiadomień z zaplecza programu ASP.NET przy użyciu tagów służących do adresowania powiadomień do określonych użytkowników.

Jeśli chcesz podzielić użytkowników na grupy zainteresowań, zobacz [Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]. 

Więcej ogólnych informacji o usłudze Notification Hubs zawiera temat [Wskazówki dotyczące usługi Notification Hubs].






<!-- Images. -->
[2]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-windows-universal-app.png
[3]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-associate-win8-app.png
[4]: ./media/notification-hubs-windows-store-dotnet-get-started/mobile-services-select-app-name.png
[6]: ./media/notification-hubs-windows-store-dotnet-get-started/mobile-services-win8-app-push-auth.png
[11]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[15]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-scheduler1.png
[16]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-scheduler2.png
[18]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-win8-app-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->
[klasycznego portalu Azure]: https://manage.windowsazure.com/
[Wskazówki dotyczące usługi Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx

[Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-notify-users.md
[Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]: notification-hubs-windows-store-dotnet-send-breaking-news.md

[wykazu powiadomień wyskakujących]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[Wykaz kafelków]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[Omówienie znaczków]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx



<!--HONumber=Jun16_HO2-->


