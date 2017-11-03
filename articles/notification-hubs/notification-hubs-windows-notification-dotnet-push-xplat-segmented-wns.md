---
title: "Centra powiadomień Azure umożliwia wysyłanie najważniejszych wiadomości (platformy uniwersalnej systemu Windows)"
description: "Za pomocą usługi Azure Notification Hubs tagów w rejestracji wysyłanie najważniejszych wiadomości do aplikacji platformy uniwersalnej systemu Windows."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 994d2eed-f62e-433c-bf65-4afebf1c0561
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: d510e7e665adec9607aeee80802c466b363d5d5b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Omówienie
W tym temacie przedstawiono sposób użycia usługi Azure Notification Hubs wysyłać powiadomienia o najważniejszych wiadomościach do Sklepu Windows lub Windows Phone 8.1 (z systemem innym niż platformy Silverlight) aplikacji. Jeśli ma być przeznaczona dla systemu Windows Phone 8.1 Silverlight, zapoznaj się [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) wersji. 

Po zakończeniu tego procesu można zarejestrować dla kategorii wiadomości podziału, które planuje się i będzie odbierać powiadomienia wypychane dla tylko tych kategorii. Ten scenariusz jest typowy dla wielu aplikacji (na przykład czytniki danych RSS lub aplikacje dla wentylatory utworów muzycznych), której wysyłane powiadomienia dla grup użytkowników, które je zainteresowanie. 

Umożliwia realizację scenariuszy emisji, umieszczając w niej co najmniej jeden *tagi* po utworzeniu rejestracji w Centrum powiadomień. W przypadku powiadomienia są wysyłane do tagu, wszystkie urządzenia, które zostały zarejestrowane dla tagu otrzymują powiadomienia. Ponieważ tagi są po prostu ciągów, nie mają być skonfigurowane z wyprzedzeniem. Aby uzyskać więcej informacji na temat tagów, zobacz [wyrażenia routingu i tagu usługi Notification Hubs](notification-hubs-tags-segment-push-message.md).

> [!NOTE]
> Wersje projektu Sklep Windows i Windows Phone 8.1 i starsze nie są obsługiwane w programie Visual Studio 2017 r. Aby uzyskać więcej informacji, zobacz [Obsługiwane platformy i zgodność programu Visual Studio 2017](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs). 

## <a name="prerequisites"></a>Wymagania wstępne
W tym temacie opiera się na aplikacji, który został utworzony w [Rozpoczynanie pracy z usługą Notification Hubs][get-started]. Przed rozpoczęciem tego samouczka, należy wykonać [Rozpoczynanie pracy z usługą Notification Hubs][get-started].

## <a name="add-category-selection-to-the-app"></a>Dodaj wybrane kategorii do aplikacji
Pierwszym krokiem jest można dodać do istniejącej strony głównej elementy interfejsu użytkownika, dzięki czemu użytkownicy mogą wybrać kategorie, aby zarejestrować. Wybranych kategorii są przechowywane na urządzeniu. Po uruchomieniu aplikacji, rejestracji urządzenia jest tworzony w Centrum powiadomień, z wybranych kategorii jako znaczniki.

1. Otwórz plik projektu MainPage.xaml, a następnie skopiuj następujący kod w **siatki** elementu:
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>

2. Kliknij prawym przyciskiem myszy **Shared** projekt, Dodaj nową klasę o nazwie **powiadomienia**, Dodaj **publicznego** modyfikator do definicji klasy, a następnie dodaj następujące **przy użyciu** instrukcje do nowego pliku kodu:
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Skopiuj następujący kod do nowego **powiadomienia** klasy:
   
        private NotificationHub hub;
   
        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }
   
        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }
   
        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }
   
        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }
   
    Ta klasa używa magazynu lokalnego do przechowywania kategorii to urządzenie musi odebrać wiadomości. Zamiast wywoływać metodę *RegisterNativeAsync* metody, wywołaj *RegisterTemplateAsync* zarejestrować dla kategorii przy użyciu rejestracji szablonu. 
   
    Ponieważ można zarejestrować więcej niż jednego szablonu (na przykład, jeden dla wyskakujące powiadomienia) i drugi dla kafelków, należy również podać nazwę szablonu (na przykład "simpleWNSTemplateExample"). Nadaj nazwę szablonów, aby można zaktualizować lub usunąć je.
   
    >[!NOTE]
    >Jeśli urządzenie rejestruje wiele szablonów z tym samym tagiem, wiadomości przychodzących, przeznaczonego dla tagu powoduje, że wiele powiadomień dostarczanej na urządzenia (po jednej dla każdego szablonu). To zachowanie jest przydatne, gdy ten sam komunikat logicznej może skutkować wielu visual powiadomienia (na przykład zawierający wskaźnika i użyciu powiadomienia wyskakującego w aplikacji Sklepu Windows).
   
    Aby uzyskać więcej informacji, zobacz [szablony](notification-hubs-templates-cross-platform-push-messages.md).

4. W pliku projektu App.xaml.cs Dodaj następującą właściwość, aby **aplikacji** klasy:
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
    Ta właściwość jest służy do tworzenia i dostępu **powiadomienia** wystąpienia.
   
    W kodzie, Zastąp `<hub name>` i `<connection string with listen access>` symbole zastępcze nazwą Centrum powiadomień i parametry połączenia dla *DefaultListenSharedAccessSignature*, który został uzyskany wcześniej.
   
   > [!NOTE]
   > Ponieważ poświadczenia, które są dystrybuowane wraz z aplikacji przez klienta nie są zwykle bezpieczne, dystrybuować tylko klucz dla *nasłuchiwania* dostęp z aplikacji klienta. W przypadku dostępu do nasłuchiwania aplikacji mogą zarejestrować odbieranie powiadomień, ale nie można zmodyfikować istniejące rejestracje i nie można wysłać powiadomienia. Klucz pełny dostęp jest używany w zabezpieczonych usługi zaplecza do wysyłania powiadomień i zmianę istniejącego rejestracji.
   > 
   > 
5. W pliku MainPage.xaml.cs projekt Dodaj następujący wiersz:
   
        using Windows.UI.Popups;

6. W pliku MainPage.xaml.cs projekt Dodaj następującą metodę:
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
   
            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
   
    Ta metoda tworzy listę kategorii i używa **powiadomienia** klasę do przechowywania listy w magazynie lokalnym. Rejestruje również odpowiedniego tagów w Centrum powiadomień. Po zmianie kategorie, rejestracja jest tworzona ponownie przy użyciu nowych kategorii.

Aplikację teraz można przechowywać zestawu kategorii w lokalnej pamięci masowej na urządzeniu. Rejestruje aplikację Centrum powiadomień zmianie użytkownikom wybór kategorii.

## <a name="register-for-notifications"></a>Rejestrowanie się w celu powiadomienia
W tej sekcji Zarejestruj się w Centrum powiadomień przy uruchamianiu za pomocą kategorii przechowywanych w magazynie lokalnym.

> [!NOTE]
> Ponieważ kanał identyfikator URI, który jest przypisany przez usługę powiadomień systemu Windows (WNS) można zmienić w dowolnym momencie, należy zarejestrować powiadomień często uniknąć niepowodzeń powiadomień. W tym przykładzie rejestruje powiadomienia każdym uruchomieniu aplikacji. Dla aplikacji, które są uruchamiane często (więcej niż raz dziennie) prawdopodobnie można pominąć rejestrację, aby zachować przepustowość, jeśli krótszy niż doba upłynął od czasu poprzedniej rejestracji.
> 
> 

1. Aby użyć `notifications` klasy do subskrybowania na podstawie kategorii, otwórz plik App.xaml.cs, a następnie zaktualizuj **InitNotificationsAsync** metody.
   
        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
   
        var result = await notifications.SubscribeToCategories();
   
    Ten proces gwarantuje, że po uruchomieniu aplikacji, jego pobiera kategorie z magazynu lokalnego i żądanie rejestracji z tych kategorii. Możesz utworzyć **InitNotificationsAsync** metody jako część [Rozpoczynanie pracy z usługą Notification Hubs] [ get-started] samouczka.

2. W pliku MainPage.xaml.cs projekt, Dodaj następujący kod do *OnNavigatedTo* metody:
   
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();
   
            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }
   
    Ten kod aktualizuje strony głównej, na podstawie kategorii uprzednio zapisanego stanu.

Aplikacja jest teraz ukończona. Zestaw kategorii umożliwia przechowywanie w magazynie lokalnym urządzenia, które służy do rejestrowania w Centrum powiadomień, gdy użytkownicy zmienić wybór kategorii. W następnej sekcji można zdefiniować zaplecza, który może wysyłać powiadomienia kategorii do tej aplikacji.

## <a name="send-tagged-notifications"></a>Wysyłanie powiadomień oznakowany
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a>Uruchom aplikację i generować powiadomienia
1. W programie Visual Studio, wybierz **F5** Aby skompilować i uruchomić aplikację.  
    Aplikacji interfejsu użytkownika zawiera zestaw przełączników, który pozwala na wybór kategorii do subskrybowania. 
   
    ![Fundamentalne aplikacji wiadomości][1]

2. Włącz co najmniej jeden przełącza kategorii, a następnie kliknij przycisk **Subskrybuj**.
   
    Konwertuje wybranych kategorii do tagów i żąda nowej rejestracji urządzeń dla wybranych tagów z Centrum powiadomień aplikacji. Zarejestrowany kategorie są zwracane i wyświetlane w oknie dialogowym.
   
    ![Włącza lub wyłącza kategorii i Subskrybuj przycisku][19]

3. Wyślij nowe powiadomienie z wewnętrznej w jednym z następujących sposobów:

   * **Aplikacja konsoli**: uruchomić aplikację konsoli.
   * **Java/PHP**: Uruchom aplikację lub skryptu.
     
     Powiadomienia dotyczące wybranych kategorii są wyświetlane jako wyskakujące powiadomienia.
     
     ![Wyskakujące powiadomienia][14]

## <a name="next-steps"></a>Następne kroki
W tym artykule przedstawiono sposób emisji najważniejszych wiadomości według kategorii. Należy rozważyć wykonywania kroków samouczka następujące, co powoduje innego zaawansowanym scenariuszu centra powiadomień:

* [Emisji zlokalizowanych najważniejszych wiadomości przy użyciu usługi Notification Hubs] w tym samouczku omówiono sposób rozszerzyć aplikacji wiadomości podziału, aby umożliwić wysyłanie powiadomień zlokalizowanego.

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification
[Emisji zlokalizowanych najważniejszych wiadomości przy użyciu usługi Notification Hubs]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
