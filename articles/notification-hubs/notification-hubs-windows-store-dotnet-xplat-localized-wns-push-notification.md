---
title: "Centra powiadomień w zlokalizowanych samouczek wiadomości podziału"
description: "Dowiedz się, jak używać usługi Azure Notification Hubs wysłać powiadomienia o zlokalizowanych najważniejszych wiadomościach."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: c454f5a3-a06b-45ac-91c7-f91210889b25
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 8f205188bd68e53b187b71981ed36dcf9129ec62
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="use-notification-hubs-to-send-localized-breaking-news"></a>Wysyłanie zlokalizowanych najważniejszych wiadomości przy użyciu usługi Notification Hubs
> [!div class="op_single_selector"]
> * [Sklep Windows C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Przegląd
W tym temacie przedstawiono sposób użycia **szablonu** funkcji usługi Azure Notification Hubs wysyłać powiadomienia o najważniejszych wiadomościach które zostały zlokalizowane według języka i urządzenia. W tym samouczku początkowo utworzony w aplikacji ze Sklepu Windows [użyciu usługi Notification Hubs wysyłanie najważniejszych wiadomości]. Po zakończeniu będzie można zarejestrować dla kategorii, które planuje się, określ język, w których chcesz otrzymywać powiadomienia, a otrzymywać tylko powiadomień wypychanych do wybranych kategorii w tym języku.

Istnieją dwie części tego scenariusza:

* aplikacji ze Sklepu Windows pozwala klientów urządzeń do określania języka i subskrybować podziału różne kategorie nowości;
* zaplecza emituje powiadomień, przy użyciu **tag** i **szablonu** feautres usługi Azure Notification Hubs.

## <a name="prerequisites"></a>Wymagania wstępne
Należy zostały już wykonane [użyciu usługi Notification Hubs wysyłanie najważniejszych wiadomości] samouczka i mieć kod dostępne, ponieważ w tym samouczku bezpośrednio oparta na kod.

Należy również Visual Studio 2012 lub nowszym.

## <a name="template-concepts"></a>Pojęcia dotyczące szablonów
W [użyciu usługi Notification Hubs wysyłanie najważniejszych wiadomości] utworzono aplikację, która jest używana **tagi** do subskrybowania powiadomień dla wiadomości różnych kategorii.
Wiele aplikacji, jednak docelowy kilku rynkach i wymagają lokalizacji. Oznacza to, że zawartość powiadomień, się muszą być lokalizowany i dostarczane do odpowiednich zestawów urządzeń.
W tym temacie pokazano, jak używać **szablonu** funkcji usługi Notification Hubs łatwe udostępnianie powiadomienia o zlokalizowanych najważniejszych wiadomościach.

Uwaga: jeden sposób wysyłania powiadomień zlokalizowanych jest utworzenie wielu wersji każdego znacznika. Na przykład do obsługi w języku angielskim, francuskim i mandaryński, czy potrzebujemy trzy różne znaczniki dla wiadomości world: "world_en", "world_fr" i "world_ch". Firma Microsoft następnie musi wysłać zlokalizowanej wersji wiadomości world do każdego z tych tagów. W tym temacie używamy szablonów w celu uniknięcia mnożenie tagów i wymaganie wysyła wiele komunikatów.

Na wysokim poziomie szablony są sposób, aby określić, jak powinno zostać odebrane powiadomienie określonego urządzenia. Szablon określa format ładunku dokładne odwołując się do właściwości, które są częścią komunikat wysłany przez zaplecze Twojej aplikacji. W tym przypadku firma Microsoft wyśle komunikat niezależny od ustawień regionalnych zawierający wszystkie obsługiwane języki:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Następnie firma Microsoft zapewnia urządzenia Zarejestruj szablon, który odwołuje się do właściwości poprawne. Na przykład aplikacji Sklepu Windows, która chce otrzymywać wiadomości proste wyskakujące zarejestruje dla następującego szablonu z żadnych odpowiednich tagów:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Szablony są bardzo przydatna funkcja, możesz dowiedzieć się więcej w naszym [szablony](notification-hubs-templates-cross-platform-push-messages.md) artykułu. 

## <a name="the-app-user-interface"></a>Interfejs użytkownika aplikacji
Firma Microsoft będzie teraz zmodyfikować aplikację fundamentalne wiadomości utworzonego w temacie [użyciu usługi Notification Hubs wysyłanie najważniejszych wiadomości] wysłać zlokalizowane fundamentalne wiadomości za pomocą szablonów.

W aplikacji ze Sklepu Windows:

Zmień Twoje MainPage.xaml, aby uwzględnić combobox ustawień regionalnych:

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

## <a name="building-the-windows-store-client-app"></a>Tworzenie aplikacji ze Sklepu Windows klienta
1. W klasie powiadomień należy dodać parametr ustawień regionalnych użytkownika *StoreCategoriesAndSubscribe* i *SubscribeToCateories* metody.
   
        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }
   
        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }
   
    Należy pamiętać, że zamiast wywoływać metodę *RegisterNativeAsync* metody nazywamy *RegisterTemplateAsync*: Firma Microsoft rejestracji format określonych powiadomień, w którym szablon jest zależny od ustawień regionalnych. Również udostępniamy nazwę dla szablonu ("localizedWNSTemplateExample"), ponieważ chcemy zarejestrować więcej niż jednego szablonu (na przykład jeden dla wyskakujące powiadomienia) i drugi dla kafelków i musimy ich nazw, aby można było zaktualizować lub usunąć je.
   
    Należy pamiętać, że jeśli urządzenie rejestruje wiele szablonów z tym samym tagiem, przychodzących wiadomości przeznaczonych dla który tag spowoduje wiele powiadomień dostarczony do urządzenia, (po jednej dla każdego szablonu). To zachowanie jest przydatne, gdy ten sam komunikat logiczne ma zastosowanie w wielu powiadomienia wizualne, na przykład zawierający wskaźnika i użyciu powiadomienia wyskakującego w aplikacji Sklepu Windows.
2. Dodaj następującą metodę do pobrania przechowywane ustawienia regionalne:
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. W Twojej MainPage.xaml.cs aktualizacji przycisk kliknij programu obsługi pobierania bieżącą wartość pola kombi ustawień regionalnych i zapewnienie wywołanie klasy powiadomień, jak pokazano:
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;
   
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);
   
            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
4. Na koniec w pliku App.xaml.cs, upewnij się, że aktualizacja Twojego `InitNotificationsAsync` metody do pobierania ustawień regionalnych i użycia jej w przypadku subskrypcji:
   
        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

## <a name="send-localized-notifications-from-your-back-end"></a>Wysyłanie powiadomień zlokalizowanych z poziomu zaplecza
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[użyciu usługi Notification Hubs wysyłanie najważniejszych wiadomości]: /notification-hubs/notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
