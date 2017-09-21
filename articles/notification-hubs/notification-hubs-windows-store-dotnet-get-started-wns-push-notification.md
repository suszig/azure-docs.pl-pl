---
title: "Rozpoczynanie pracy z usługą Azure Notification Hubs dla aplikacji platformy uniwersalnej systemu Windows | Microsoft Docs"
description: "Korzystając z tego samouczka, dowiesz się, jak wypychać powiadomienia do aplikacji platformy uniwersalnej systemu Windows przy użyciu usługi Azure Notification Hubs."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: erikre
ms.assetid: cf307cf3-8c58-4628-9c63-8751e6a0ef43
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.translationtype: HT
ms.sourcegitcommit: 9b7316a5bffbd689bdb26e9524129ceed06606d5
ms.openlocfilehash: e18a810bcdbd97c79418f53c647df8723ecb6076
ms.contentlocale: pl-pl
ms.lasthandoff: 09/08/2017

---
# <a name="get-started-with-notification-hubs-for-universal-windows-platform-apps"></a>Rozpoczynanie pracy z usługą Notification Hubs dla aplikacji platformy uniwersalnej systemu Windows

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Omówienie
W tym artykule wyjaśniono, jak wysyłać powiadomienia wypychane do aplikacji platformy uniwersalnej systemu Windows (UWP, Universal Windows Platform) przy użyciu usługi Azure Notification Hubs.

Korzystając z tego artykułu, utworzysz pustą aplikację ze Sklepu Windows, która odbiera powiadomienia wypychane przy użyciu usługi powiadomień WNS (Windows Push Notification Service). Po zakończeniu będzie można za pomocą centrum powiadomień wysyłać powiadomienia wypychane do wszystkich urządzeń z tą aplikacją.

## <a name="before-you-begin"></a>Przed rozpoczęciem
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Kompletny kod dla tego samouczka można znaleźć [w witrynie GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).

## <a name="prerequisites"></a>Wymagania wstępne
Dla tego samouczka wymagane są następujące elementy:

* Program [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) lub nowszy
* [Zainstalowane narzędzia do programowania aplikacji platformy UWP](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* Aktywne konto platformy Azure  
    Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać więcej informacji, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).
* Aktywne konto Sklepu Windows

Wykonanie czynności opisanych w tym samouczku jest wymaganiem wstępnym dla wszystkich innych samouczków usługi Notification Hubs dotyczących aplikacji platformy UWP.

## <a name="register-your-app-for-the-windows-store"></a>Rejestrowanie aplikacji w Sklepie Windows
Aby wysyłać powiadomienia wypychane do aplikacji platformy UWP, skojarz aplikację ze Sklepem Windows. Następnie skonfiguruj integrację centrum powiadomień z usługą WNS.

1. Jeśli aplikacja nie została jeszcze zarejestrowana, przejdź do [Centrum deweloperów systemu Windows](https://dev.windows.com/overview), zaloguj się przy użyciu konta Microsoft, a następnie wybierz pozycję **Utwórz nową aplikację**.

2. Wpisz nazwę aplikacji i wybierz pozycję **Rezerwuj nazwę aplikacji**. Spowoduje to utworzenie nowej rejestracji aplikacji w Sklepie Windows.

3. W programie Visual Studio utwórz nowy projekt aplikacji ze Sklepu w języku Visual C# za pomocą szablonu **Pusta aplikacja** platformy UWP, a następnie wybierz pozycję **OK**.

4. Zaakceptuj wartości domyślne dla wersji platformy docelowej i minimalnej.

5. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt aplikacji ze Sklepu Windows, wybierz pozycję **Sklep**, a następnie wybierz pozycję **Skojarz aplikację ze sklepem**.  
    Zostanie wyświetlony kreator **Kojarzenie aplikacji ze Sklepem Windows**.

6. W kreatorze zaloguj się za pomocą konta Microsoft.

7. Wybierz aplikację zarejestrowaną w kroku 2, wybierz przycisk **Dalej**, a następnie wybierz pozycję **Skojarz**. Spowoduje to dodanie wymaganych informacji dotyczących rejestracji w Sklepie Windows do manifestu aplikacji.

8. Ponownie na stronie [Centrum deweloperów systemu Windows](http://dev.windows.com/overview) dla Twojej nowej aplikacji wybierz pozycję **Usługi** i **Powiadomienia wypychane**, a następnie wybierz pozycję **WNS/MPNS**.

9. Wybierz pozycję **Nowe powiadomienie**.

10. Wybierz szablon **Puste (wyskakujące)**, a następnie wybierz przycisk **OK**.

11. Wprowadź **Nazwę** powiadomienia i komunikat **Kontekst** wizualizacji, a następnie wybierz pozycję **Zapisz jako wersję roboczą**.

12. Przejdź do [portalu rejestracji aplikacji](http://apps.dev.microsoft.com) i zaloguj się.

13. Wybierz nazwę swojej aplikacji. W ustawieniach platformy **Sklep Windows** zanotuj hasło **Klucz tajny aplikacji** oraz **Identyfikator zabezpieczeń (SID) pakietu**.

    >[!WARNING]
    >Klucz tajny aplikacji i identyfikator SID pakietu są ważnymi poświadczeniami zabezpieczeń. Nie udostępniaj nikomu tych wartości ani nie rozpowszechniaj ich razem z aplikacją.

## <a name="configure-your-notification-hub"></a>Konfigurowanie centrum powiadomień
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="5">
<li><p>Wybierz pozycję <b>Usługi powiadomień</b> > <b>Windows (WNS)</b>, a następnie wprowadź klucz tajny aplikacji w polu <b>Klucz zabezpieczeń</b>. W polu <b>Identyfikator SID pakietu</b> wprowadź wartość uzyskaną z usługi WNS w poprzedniej sekcji, a następnie wybierz pozycję <b>Zapisz</b>.</p>
</li>
</ol>

![Pola Identyfikator SID pakietu i Klucz zabezpieczeń](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Twoje centrum powiadomień jest teraz skonfigurowane do pracy z usługą WNS. Masz parametry połączenia potrzebne do zarejestrowania aplikacji i wysyłania powiadomień.

## <a name="connect-your-app-to-the-notification-hub"></a>Łączenie aplikacji z centrum powiadomień
1. W programie Visual Studio kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Zarządzaj pakietami NuGet**.  
    Zostanie otwarte okno **Zarządzanie pakietami NuGet**.

2. W polu wyszukiwania wprowadź ciąg **WindowsAzure.Messaging.Managed**, wybierz pozycję **Zainstaluj** i zaakceptuj warunki użytkowania.
   
    ![Okno Zarządzanie pakietami NuGet][20]
   
    Spowoduje to pobranie i zainstalowanie biblioteki obsługi wiadomości platformy Azure dla systemu Windows oraz dodanie odwołania do niej przy użyciu [pakietu NuGet WindowsAzure.Messaging.Managed](http://nuget.org/packages/WindowsAzure.Messaging).

3. Otwórz plik projektu App.xaml.cs i dodaj następujące instrukcje `using`: 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;

4. W pliku App.xaml.cs dodaj także do klasy **App** następującą definicję metody **InitNotificationsAsync**:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("<your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
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
   
    >[!NOTE]
    >* Zastąp symbol zastępczy **nazwa centrum** nazwą centrum powiadomień wyświetlaną w witrynie Azure Portal. 
    >* Zastąp również symbol zastępczy parametrów połączenia przy użyciu parametrów połączenia **DefaultListenSharedAccessSignature** uzyskanych ze strony **Zasady dostępu** Twojego centrum powiadomień w poprzedniej sekcji.
   > 
   > 
5. W górnej części programu obsługi zdarzeń **OnLaunched** w pliku App.xaml.cs dodaj następujące wywołanie do nowej metody **InitNotificationsAsync**:
   
        InitNotificationsAsync();
   
    Ta akcja gwarantuje, że identyfikator URI kanału jest rejestrowany w centrum powiadomień przy każdym uruchomieniu aplikacji.

6. Aby uruchomić aplikację, naciśnij klawisz **F5**. Zostanie wyświetlone okno dialogowe zawierające klucz rejestracji.

Aplikacja jest teraz gotowa do odbierania wyskakujących powiadomień.

## <a name="send-notifications"></a>Wysyłanie powiadomień
Możesz szybko przetestować odbieranie powiadomień w aplikacji, wysyłając powiadomienia w witrynie [Azure Portal](https://portal.azure.com/). Użyj przycisku **Wysyłanie testowe** w centrum powiadomień, jak pokazano na poniższej ilustracji:

![Okienko Wysyłanie testowe](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Powiadomienia wypychane są zwykle wysyłane za pośrednictwem usługi zaplecza, takiej jak Mobile Services czy ASP.NET przy użyciu zgodnej biblioteki. Jeśli w danym zapleczu biblioteka nie jest dostępna, powiadomienia można również wysyłać bezpośrednio za pomocą interfejsu API REST. 

W tym samouczku przedstawiono proste testowanie aplikacji klienckiej przez wysyłanie powiadomień za pomocą zestawu SDK .NET dla usługi Notification Hubs w aplikacji konsolowej, a nie za pomocą usługi zaplecza. Zalecanym następnym krokiem jest zapoznanie się z samouczkiem [Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs] dotyczącym wysyłania powiadomień przy użyciu zaplecza ASP.NET. Możesz jednak wysłać powiadomienia, korzystając z następujących metod:

* **Interfejs REST**: powiadomienia mogą być obsługiwane na dowolnej platformie zaplecza za pomocą [interfejsu REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).

* **Zestaw SDK .NET dla usługi Microsoft Azure Notification Hubs**: w menedżerze pakietów NuGet dla programu Visual Studio uruchom polecenie [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js**: zobacz [Jak używać usługi Notification Hubs z poziomu środowiska Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* **Azure Mobile Apps**: aby zapoznać się z przykładem wysyłania powiadomień z poziomu aplikacji usługi Azure Mobile Apps zintegrowanej z usługą Notification Hubs, zobacz [Add push notifications for Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) (Dodawanie powiadomień wypychanych do aplikacji usługi Mobile Apps).

* **Java lub PHP**: aby zapoznać się z przykładami wysyłania powiadomień przy użyciu interfejsów API REST, zobacz:
    * [Java](notification-hubs-java-push-notification-tutorial.md)
    * [PHP](notification-hubs-php-push-notification-tutorial.md)

## <a name="optional-send-notifications-from-a-console-app"></a>(Opcjonalnie) Wysyłanie powiadomień z poziomu aplikacji konsolowej
Aby wysłać powiadomienia za pomocą aplikacji konsolowej programu .NET, wykonaj następujące czynności: 

1. Kliknij prawym przyciskiem myszy rozwiązanie, wybierz polecenie **Dodaj** > **Nowy projekt**, w obszarze **Visual C#** wybierz pozycję **Windows** i pozycję **Aplikacja konsolowa**, a następnie wybierz przycisk **OK**.
   
    Nowa aplikacja konsolowa w języku Visual C# zostanie dodana do rozwiązania. Można również dodać projekt w oddzielnym rozwiązaniu.

2. W programie Visual Studio wybierz kolejno pozycje **Narzędzia**, **Menedżer pakietów NuGet**, **Konsola menedżera pakietów**.
   
    W programie Visual Studio zostanie otwarta Konsola menedżera pakietów.

3. W oknie Konsola menedżera pakietów ustaw nowy projekt aplikacji konsoli jako **Projekt domyślny**, a następnie w oknie konsoli uruchom następujące polecenie:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Ta akcja spowoduje dodanie odwołania do zestawu SDK usługi Azure Notification Hubs z użyciem [pakietu NuGet Microsoft.Azure.Notification Hubs](http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
   
    ![Nazwa „Projekt domyślny”](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

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
   
    >[!NOTE]
    >* Zastąp symbol zastępczy **nazwa centrum** nazwą centrum powiadomień wyświetlaną w witrynie Azure Portal. 
    >* Zastąp symbol zastępczy parametrów połączenia przy użyciu parametrów połączenia **DefaultFullSharedAccessSignature** uzyskanych ze strony **Zasady dostępu** Centrum powiadomień w sekcji „Konfigurowanie centrum powiadomień”.
    >* Użyj parametrów połączenia z uprawnieniami dostępu *Pełne*, a nie *Nasłuchiwanie*. Parametry połączenia z uprawnieniami dostępu do nasłuchiwania nie mają uprawnień do wysyłania powiadomień.
   > 
   > 
6. W metodzie **Main** dodaj następujące wiersze:
   
         SendNotificationAsync();
         Console.ReadLine();

7. Kliknij prawym przyciskiem myszy projekt aplikacji konsolowej w programie Visual Studio, a następnie wybierz polecenie **Ustaw jako projekt startowy**, aby ustawić go jako projekt startowy. Następnie wybierz klawisz **F5**, aby uruchomić aplikację.
   
    Otrzymasz wyskakujące powiadomienie na wszystkich zarejestrowanych urządzeniach. Wybranie lub naciśnięcie baneru powiadomienia wyskakującego spowoduje załadowanie aplikacji.

Wszystkie obsługiwane ładunki można znaleźć w tematach dotyczących [wykazu powiadomień wyskakujących], [wykazu kafelków] i [omówienia znaczków] w witrynie MSDN.

## <a name="next-steps"></a>Następne kroki
W tym prostym przykładzie wysłano wyemitowane powiadomienia do wszystkich urządzeń z systemem Windows przy użyciu portalu lub aplikacji konsolowej. Zalecanym następnym krokiem jest zapoznanie się z samouczkiem [Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]. Przedstawiono w nim sposób wysyłania powiadomień z zaplecza programu ASP.NET przy użyciu tagów służących do adresowania powiadomień do określonych użytkowników.

Jeśli chcesz podzielić użytkowników na grupy zainteresowań, zobacz [Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]. 

Aby uzyskać więcej ogólnych informacji o usłudze Notification Hubs, zobacz [Wskazówki dotyczące usługi Notification Hubs](notification-hubs-push-notification-overview.md).

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

[Wysyłanie powiadomień wypychanych do użytkowników przy użyciu usługi Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

[wykazu powiadomień wyskakujących]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[wykazu kafelków]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[omówienia znaczków]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx
 

