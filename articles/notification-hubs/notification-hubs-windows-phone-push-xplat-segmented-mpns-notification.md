---
title: "Wysyłanie najważniejszych wiadomości (Windows Phone) przy użyciu usługi Notification Hubs"
description: "Za pomocą usługi Azure Notification Hubs do tagu w rejestracji wysyłanie najważniejszych wiadomości do aplikacji Windows Phone."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 42726bf5-cc82-438d-9eaa-238da3322d80
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 3a6a69bf555c7267d3fbeb03ff6c03054991960f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Omówienie
W tym temacie przedstawiono sposób użycia usługi Azure Notification Hubs wysyłać powiadomienia o najważniejszych wiadomościach do aplikacji Windows Phone 8.0/8.1 Silverlight. Jeśli ma być przeznaczona dla aplikacji Windows Phone 8.1 lub Sklepu Windows, zapoznaj się do [uniwersalnych systemu Windows](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) wersji. Po zakończeniu będzie można rejestrować zakłócenia w kategorii wiadomości i odbierać tylko powiadomienia wypychane dla tych kategorii. Ten scenariusz jest wspólnego wzorca dla wielu aplikacji, gdzie powiadomienia muszą być wysłane do grup użytkowników, które wcześniej zostały zadeklarowane zainteresowanie je, np. czytnik danych RSS, aplikacje wentylatory utworów muzycznych itp.

Scenariusze emisji są włączone, umieszczając w niej co najmniej jeden *tagi* podczas tworzenia rejestracji w Centrum powiadomień. Gdy powiadomienia są wysyłane do tagu, wszystkie urządzenia, które zostały zarejestrowane dla tagu będą otrzymywać powiadomienia. Ponieważ tagi są po prostu ciągów, nie mają być przygotowana z wyprzedzeniem. Aby uzyskać więcej informacji na temat tagów, zapoznaj się [routingu centra powiadomień i wyrażeń tagów](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Wymagania wstępne
W tym temacie opiera się na aplikacji utworzony w [Rozpoczynanie pracy z usługą Notification Hubs]. Przed rozpoczęciem tego samouczka należy zostały już wykonane [Rozpoczynanie pracy z usługą Notification Hubs].

## <a name="add-category-selection-to-the-app"></a>Dodaj wybrane kategorii do aplikacji
Pierwszym krokiem jest dodać elementy interfejsu użytkownika do strony głównej istniejących umożliwiają użytkownikowi wybierz kategorie, aby zarejestrować. Kategorie wybrane przez użytkownika są przechowywane na urządzeniu. Po uruchomieniu aplikacji, rejestracji urządzenia jest tworzony w Centrum powiadomień z wybranych kategorii jako znaczniki.

1. Otwórz plik projektu MainPage.xaml, a następnie zastąp **siatki** elementy o nazwie `TitlePanel` i `ContentPanel` z następującym kodem:
   
        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>
   
        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>
2. W projekcie, Utwórz nową klasę o nazwie **powiadomienia**, Dodaj **publicznego** modyfikator do definicji klasy, a następnie dodaj następujące **przy użyciu** instrukcje do nowego pliku kodu:
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;
3. Skopiuj następujący kod do nowej **powiadomienia** klasy:
   
        private NotificationHub hub;
   
        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;
   
        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }
   
        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }
   
        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();
   
            return await SubscribeToCategories();
        }
   
        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();
   
            var channel = HttpNotificationChannel.Find("MyPushChannel");
   
            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;
   
                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }
   
            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
   
            return await registrationTask.Task;
        }
   
        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }
   
        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";
   
            // The stored categories tags are passed with the template registration.
   
            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));
   
            return await registrationTask.Task;
        }
   
        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;
   
            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());
   
            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);
   
                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }
   
            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }

    Ta klasa używa izolowanego magazynu do przechowywania kategorii wiadomości, który ma otrzymać tego urządzenia. Zawiera również metody do rejestrowania dla tych kategorii przy użyciu [szablonu](notification-hubs-templates-cross-platform-push-messages.md) rejestracji powiadomień.


1. W pliku projektu App.xaml.cs Dodaj następującą właściwość, aby **aplikacji** klasy. Zastąp `<hub name>` i `<connection string with listen access>` symbole zastępcze nazwą Centrum powiadomień i parametry połączenia dla *DefaultListenSharedAccessSignature* uzyskany wcześniej.
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
   > [!NOTE]
   > Ponieważ poświadczenia, które są dystrybuowane wraz z aplikacji przez klienta nie są zazwyczaj bezpieczna, klucz dostępu do nasłuchiwania należy dystrybuować tylko z aplikacji klienta. Nasłuchiwanie umożliwia dostęp, nie można zmodyfikować aplikację, aby zarejestrować dla powiadomień, ale istniejące rejestracje i nie mogą być wysyłane powiadomienia. Klucz pełny dostęp jest używany w usłudze zabezpieczonych wewnętrznej bazy danych do wysyłania powiadomień i zmianę istniejącego rejestracji.
   > 
   > 
2. W Twojej MainPage.xaml.cs Dodaj następujący wiersz:
   
        using Windows.UI.Popups;
3. W pliku MainPage.xaml.cs projekt Dodaj następującą metodę:
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
   
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
   
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }
   
    Ta metoda tworzy listę kategorii i używa **powiadomienia** klasy przechowywania listy w magazynie lokalnym i rejestracji, odpowiednie znaczniki w Centrum powiadomień. Zmiana kategorii rejestracji zostaje odtworzone w nowej kategorii.

Aplikacja jest teraz możliwość przechowywania zestawu kategorii w lokalnej pamięci masowej na urządzeniu i Zarejestruj w Centrum powiadomień, zawsze, gdy użytkownik zmieni się zaznaczenie kategorii.

## <a name="register-for-notifications"></a>Rejestrowanie się w celu powiadomienia
Następujące kroki, zarejestruj się w Centrum powiadomień przy uruchamianiu przy użyciu kategorii, które były przechowywane w magazynie lokalnym.

> [!NOTE]
> Ponieważ identyfikator URI przypisane przez Microsoft Push Notification Service (MPNS) kanału można zmienić w dowolnym momencie, należy zarejestrować powiadomień często uniknąć niepowodzeń powiadomień. W tym przykładzie rejestruje powiadomienia o każdym uruchomieniu aplikacji. Dla aplikacji, które są uruchamiane często więcej niż raz dziennie, można prawdopodobnie pominąć rejestrację, aby zachować przepustowość, jeśli krótszy niż doba upłynął od czasu poprzedniej rejestracji.
> 
> 

1. Otwórz plik App.xaml.cs i Dodaj **async** modyfikator do **Application_Launching** metodę i Zastąp rejestracji usługi Notification Hubs kodu tego zostanie dodany do [Rozpoczynanie pracy z usługą Notification Hubs] następującym kodem:
   
        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();
   
            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }
   
    Dzięki temu przy każdym uruchomieniu aplikacji it pobiera kategorie z magazynu lokalnego i żądań rejestracji dla tych kategorii.
2. W pliku MainPage.xaml.cs projekt, Dodaj następujący kod, który implementuje **OnNavigatedTo** metody:
   
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();
   
            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
        }
   
    Spowoduje to zaktualizowanie strony głównej na podstawie kategorii uprzednio zapisanego stanu.

Aplikacja jest teraz ukończona i może przechowywać zestawu kategorii w magazynie lokalnym urządzenia używane do rejestrowania w Centrum powiadomień, zawsze, gdy użytkownik zmieni się zaznaczenie kategorii. Następnie zdefiniujemy wewnętrznej bazy danych, który może wysyłać powiadomienia kategorii do tej aplikacji.

## <a name="sending-tagged-notifications"></a>Wysyłanie powiadomień oznakowany
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a>Uruchom aplikację i generować powiadomienia
1. W programie Visual Studio naciśnij klawisz F5, aby skompilować i uruchomić aplikację.
   
    ![][1]
   
    Należy pamiętać, że aplikacja interfejsu użytkownika zawiera zestaw przełącza umożliwiające wybierz kategorie, aby subskrybować.
2. Włącz co najmniej jeden przełącza kategorie, a następnie kliknij przycisk **Subskrybuj**.
   
    Konwertuje wybranych kategorii do tagów i żąda nowej rejestracji urządzeń dla wybranych tagów z Centrum powiadomień aplikacji. Zarejestrowany kategorie są zwracane i wyświetlane w oknie dialogowym.
   
    ![][2]
3. Po otrzymaniu potwierdzenia, że kategorie zostały ukończone subskrypcji, uruchom aplikację konsoli do wysyłania powiadomień dla każdej kategorii. Sprawdź, czy tylko otrzymasz powiadomienie dla kategorii, które subskrybujesz.
   
    ![][3]

W tym temacie została ukończona.

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Rozpoczynanie pracy z usługą Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

