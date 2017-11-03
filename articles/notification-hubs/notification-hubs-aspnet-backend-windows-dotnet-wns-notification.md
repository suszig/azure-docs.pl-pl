---
title: "Azure Notification Hubs Powiadom użytkowników z zaplecza programu .NET"
description: "Dowiedz się, jak wysyłać powiadomienia wypychane bezpiecznej platformie Azure. Przykłady kodu napisane w języku C# z użyciem interfejsu API programu .NET."
documentationcenter: windows
author: ysxu
manager: erikre
services: notification-hubs
editor: 
ms.assetid: 012529f2-fdbc-43c4-8634-2698164b5880
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: c0b963ef661612b1a176dd8e5f01d56e61eb5acb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-notification-hubs-notify-users-with-net-backend"></a>Azure Notification Hubs Powiadom użytkowników z zaplecza programu .NET
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Omówienie
Obsługa powiadomień wypychanych na platformie Azure umożliwia dostęp do łatwego w użyciu, multiplatform i wypychanych skalowanej infrastruktury, co znacznie upraszcza implementację powiadomienia wypychane dla aplikacji zarówno konsumenckie i korporacyjne dla platform urządzeń przenośnych. W tym samouczku pokazano, jak wysyłać powiadomienia wypychane do użytkownika konkretnej aplikacji na konkretnym urządzeniu za pomocą usługi Azure Notification Hubs. Zaplecza ASP.NET WebAPI jest używany do uwierzytelniania klientów. Za pomocą klienta uwierzytelnionego użytkownika, a znacznik zostaną automatycznie dodane przez zaplecze do rejestracji powiadomień. Ten tag będzie służyć do wysłania przez zaplecze do generowania powiadomień dla określonego użytkownika. Aby uzyskać więcej informacji o rejestrowaniu powiadomień przy użyciu zaplecza aplikacji, zobacz temat wskazówki [rejestrowanie z zaplecza aplikacji](http://msdn.microsoft.com/library/dn743807.aspx). W tym samouczku bazując na Centrum powiadomień i projektu, który został utworzony w [Rozpoczynanie pracy z usługą Notification Hubs] samouczka.

W tym samouczku jest również wymagana do [Secure Push] samouczka. Po wykonaniu kroków opisanych w tym samouczku, możesz przejść do [Secure Push] samouczku przedstawiono sposób zmodyfikować kod w tym samouczku, aby bezpiecznie wysyłać powiadomienia wypychane.

## <a name="before-you-begin"></a>Przed rozpoczęciem
Traktujemy opinie naszych użytkowników bardzo poważnie. Jeśli masz trudności z wykonaniem instrukcji w tym temacie lub uważasz, że zawartość należy poprawić, prosimy o wyrażenie opinii na dole strony.

Kompletny kod dla tego samouczka można znaleźć w witrynie GitHub [tutaj](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). 

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka należy wykonano już tych samouczków usług Mobile Services:

* [Rozpoczynanie pracy z usługą Notification Hubs]<br/>Tworzenie Centrum powiadomień i zarezerwować nazwy aplikacji i zarejestruj, aby otrzymywać powiadomienia, w tym samouczku. W tym samouczku założono, że zostały już wykonane następujące kroki. Jeśli nie, wykonaj czynności opisane w [wprowadzenie do korzystania z usługi Notification Hubs (w Sklepie Windows)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); w szczególności sekcji [zarejestrować aplikację w Sklepie Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) i [Konfigurowanie Centrum powiadomień](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). W szczególności, upewnij się, że wprowadzono **identyfikatora SID pakietu** i **klucz tajny klienta** wartości w portalu w **Konfiguruj** karty dla Centrum powiadomień. Ta procedura konfiguracja jest opisana w sekcji [Konfigurowanie Centrum powiadomień](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). Jest to ważny krok: Jeśli poświadczeń w portalu nie są zgodne z określonymi dla nazwy aplikacji zostanie wybrana, nie powiedzie się powiadomienie wypychane.

> [!NOTE]
> Jeśli używasz Mobile Apps w usłudze Azure App Service jako usługi wewnętrznej bazy danych, zobacz [wersją funkcji Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) tego samouczka.
> 
> 

&nbsp;

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-the-code-for-the-client-project"></a>Zaktualizuj kod dla projektu klienta
W tej sekcji, zaktualizuj kod w projekcie zakończone dla [Rozpoczynanie pracy z usługą Notification Hubs] samouczka. Już być skojarzone z magazynu i skonfigurowany dla Centrum powiadomień. W tej sekcji dodasz kod, aby wywołać nowego zaplecza WebAPI i użyć jej do rejestrowania i wysyłania powiadomień.

1. W programie Visual Studio Otwórz rozwiązanie, należy utworzyć dla [Rozpoczynanie pracy z usługą Notification Hubs] samouczka.
2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **(Windows 8.1)** projektu, a następnie kliknij przycisk **Zarządzaj pakietami NuGet**.
3. Po lewej stronie kliknij **Online**.
4. W **wyszukiwania** wpisz **klienta Http**.
5. Na liście wyników kliknij **bibliotek klienta HTTP Microsoft**, a następnie kliknij przycisk **zainstalować**. Ukończ instalację.
6. W polu **wyszukiwania** wpisz **Json.net**. Zainstaluj **Json.NET** pakietu, a następnie zamknij okno Menedżera pakietów NuGet.
7. Powtórz powyższe kroki dla **(Windows Phone 8.1)** projektu, aby zainstalować **JSON.NET** pakietu NuGet dla projektu Windows Phone.
8. W Eksploratorze rozwiązań w **(Windows 8.1)** projektu, kliknij dwukrotnie **MainPage.xaml** aby otworzyć go w edytorze programu Visual Studio.
9. W **MainPage.xaml** kod XML, Zastąp `<Grid>` sekcję poniższym kodem. Ten kod dodaje pole tekstowe nazwy użytkownika i hasła, który uwierzytelni użytkownika z. Dodano również pól tekstowych dla komunikatu powiadomienia i znacznik nazwy użytkownika, który powinny być przesyłane powiadomienia:
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
   
            <TextBlock Grid.Row="0" Text="Notify Users" HorizontalAlignment="Center" FontSize="48"/>
   
            <StackPanel Grid.Row="1" VerticalAlignment="Center">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                    </Grid.ColumnDefinitions>
                    <TextBlock Grid.Row="0" Grid.ColumnSpan="3" Text="Username" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="UsernameTextBox" Grid.Row="1" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
                    <TextBlock Grid.Row="2" Grid.ColumnSpan="3" Text="Password" FontSize="24" Margin="20,0,20,0" />
                    <PasswordBox Name="PasswordTextBox" Grid.Row="3" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
   
                    <Button Grid.Row="4" Grid.ColumnSpan="3" HorizontalAlignment="Center" VerticalAlignment="Center"
                                Content="1. Login and register" Click="LoginAndRegisterClick" Margin="0,0,0,20"/>
   
                    <ToggleButton Name="toggleWNS" Grid.Row="5" Grid.Column="0" HorizontalAlignment="Right" Content="WNS" IsChecked="True" />
                    <ToggleButton Name="toggleGCM" Grid.Row="5" Grid.Column="1" HorizontalAlignment="Center" Content="GCM" />
                    <ToggleButton Name="toggleAPNS" Grid.Row="5" Grid.Column="2" HorizontalAlignment="Left" Content="APNS" />
   
                    <TextBlock Grid.Row="6" Grid.ColumnSpan="3" Text="Username Tag To Send To" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="ToUserTagTextBox" Grid.Row="7" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <TextBlock Grid.Row="8" Grid.ColumnSpan="3" Text="Enter Notification Message" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="NotificationMessageTextBox" Grid.Row="9" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <Button Grid.Row="10" Grid.ColumnSpan="3" HorizontalAlignment="Center" Content="2. Send push" Click="PushClick" Name="SendPushButton" />
                </Grid>
            </StackPanel>
        </Grid>
10. W Eksploratorze rozwiązań w **(Windows Phone 8.1)** otwarciu projektu **MainPage.xaml** i Zastąp Windows Phone 8.1 `<Grid>` sekcji o tym samym kodzie powyżej. Interfejs powinien wyglądać podobnie do co pokazano poniżej.
    
    ![][13]
11. W Eksploratorze rozwiązań Otwórz **MainPage.xaml.cs** plik **(Windows 8.1)** i **(Windows Phone 8.1)** projektów. Dodaj następujące `using` instrukcje w górnej części oba pliki:
    
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Windows.Networking.PushNotifications;
        using Windows.UI.Popups;
        using System.Threading.Tasks;
12. W **MainPage.xaml.cs** dla **(Windows 8.1)** i **(Windows Phone 8.1)** projektów, Dodaj następujący element członkowski do `MainPage` klasy. Pamiętaj zastąpić `<Enter Your Backend Endpoint>` z punktu końcowego rzeczywiste wewnętrznej bazy danych uzyskany wcześniej. Na przykład `http://mybackend.azurewebsites.net`.
    
        private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
13. Dodaj poniższy kod do klasy MainPage w **MainPage.xaml.cs** dla **(Windows 8.1)** i **(Windows Phone 8.1)** projektów.
    
    `PushClick` Metoda jest obsługi kliknięcia dla **wysyłania Push** przycisku. Wywołuje zaplecza powiadomienia do wszystkich urządzeń z tagiem username pasujący wyzwalać `to_tag` parametru. Komunikat powiadomienia są wysyłane jako zawartość JSON w treści żądania.
    
    `LoginAndRegisterClick` Metoda jest obsługi kliknięcia dla **zalogować się i Zarejestruj** przycisku. Przechowuje podstawowe token uwierzytelniania w magazynie lokalnym (Zauważ, że jest to dowolny token używa schematem uwierzytelniania), następnie używa `RegisterClient` zarejestrować powiadomień za pomocą wewnętrznej bazy danych.

        private async void PushClick(object sender, RoutedEventArgs e)
        {
            if (toggleWNS.IsChecked.Value)
            {
                await sendPush("wns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleGCM.IsChecked.Value)
            {
                await sendPush("gcm", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleAPNS.IsChecked.Value)
            {
                await sendPush("apns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);

            }
        }

        private async Task sendPush(string pns, string userTag, string message)
        {
            var POST_URL = BACKEND_ENDPOINT + "/api/notifications?pns=" +
                pns + "&to_tag=" + userTag;

            using (var httpClient = new HttpClient())
            {
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                try
                {
                    await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                        System.Text.Encoding.UTF8, "application/json"));
                }
                catch (Exception ex)
                {
                    MessageDialog alert = new MessageDialog(ex.Message, "Failed to send " + pns + " message");
                    alert.ShowAsync();
                }
            }
        }

        private async void LoginAndRegisterClick(object sender, RoutedEventArgs e)
        {
            SetAuthenticationTokenInLocalStorage();

            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            // The "username:<user name>" tag gets automatically added by the message handler in the backend.
            // The tag passed here can be whatever other tags you may want to use.
            try
            {
                // The device handle used will be different depending on the device and PNS. 
                // Windows devices use the channel uri as the PNS handle.
                await new RegisterClient(BACKEND_ENDPOINT).RegisterAsync(channel.Uri, new string[] { "myTag" });

                var dialog = new MessageDialog("Registered as: " + UsernameTextBox.Text);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
                SendPushButton.IsEnabled = true;
            }
            catch (Exception ex)
            {
                MessageDialog alert = new MessageDialog(ex.Message, "Failed to register with RegisterClient");
                alert.ShowAsync();
            }
        }

        private void SetAuthenticationTokenInLocalStorage()
        {
            string username = UsernameTextBox.Text;
            string password = PasswordTextBox.Password;

            var token = Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(username + ":" + password));
            ApplicationData.Current.LocalSettings.Values["AuthenticationToken"] = token;
        }



1. W Eksploratorze rozwiązań w obszarze **Shared** otwarciu projektu **App.xaml.cs** pliku. Znajdź wywołanie `InitNotificationsAsync()` w `OnLaunched()` obsługi zdarzeń. Komentarz lub Usuń wywołanie `InitNotificationsAsync()`. Obsługa przycisku dodanych wcześniej zainicjuje rejestracji powiadomień.

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
            //InitNotificationsAsync();


1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **Shared** projektu, a następnie kliknij przycisk **Dodaj**, a następnie kliknij przycisk **klasy**. Nazwa klasy **RegisterClient.cs**, następnie kliknij przycisk **OK** do generowania klasy.
   
   Ta klasa będzie zawijany wywołania REST, wymagane do kontaktowania się z zaplecza aplikacji, aby można było zarejestrować odbieranie powiadomień wypychanych. Przechowuje także lokalnie *registrationIds* utworzone przez Centrum powiadomień, zgodnie z opisem w [rejestrowanie z zaplecza aplikacji](http://msdn.microsoft.com/library/dn743807.aspx). Uwaga używa tokenu autoryzacji przechowywanych w magazynie lokalnym po kliknięciu **zalogować się i Zarejestruj** przycisku.
2. Dodaj następujące `using` instrukcje w górnej części pliku RegisterClient.cs:
   
       using Windows.Storage;
       using System.Net;
       using System.Net.Http;
       using System.Net.Http.Headers;
       using Newtonsoft.Json;
       using System.Threading.Tasks;
       using System.Linq;
3. Dodaj następujący kod wewnątrz definicji klasy `RegisterClient`.
   
       private string POST_URL;
   
       private class DeviceRegistration
       {
           public string Platform { get; set; }
           public string Handle { get; set; }
           public string[] Tags { get; set; }
       }
   
       public RegisterClient(string backendEndpoint)
       {
           POST_URL = backendEndpoint + "/api/register";
       }
   
       public async Task RegisterAsync(string handle, IEnumerable<string> tags)
       {
           var regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
   
           var deviceRegistration = new DeviceRegistration
           {
               Platform = "wns",
               Handle = handle,
               Tags = tags.ToArray<string>()
           };
   
           var statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
   
           if (statusCode == HttpStatusCode.Gone)
           {
               // regId is expired, deleting from local storage & recreating
               var settings = ApplicationData.Current.LocalSettings.Values;
               settings.Remove("__NHRegistrationId");
               regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
               statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
           }
   
           if (statusCode != HttpStatusCode.Accepted)
           {
               // log or throw
               throw new System.Net.WebException(statusCode.ToString());
           }
       }
   
       private async Task<HttpStatusCode> UpdateRegistrationAsync(string regId, DeviceRegistration deviceRegistration)
       {
           using (var httpClient = new HttpClient())
           {
               var settings = ApplicationData.Current.LocalSettings.Values;
               httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string) settings["AuthenticationToken"]);
   
               var putUri = POST_URL + "/" + regId;
   
               string json = JsonConvert.SerializeObject(deviceRegistration);
                               var response = await httpClient.PutAsync(putUri, new StringContent(json, Encoding.UTF8, "application/json"));
               return response.StatusCode;
           }
       }
   
       private async Task<string> RetrieveRegistrationIdOrRequestNewOneAsync()
       {
           var settings = ApplicationData.Current.LocalSettings.Values;
           if (!settings.ContainsKey("__NHRegistrationId"))
           {
               using (var httpClient = new HttpClient())
               {
                   httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                   var response = await httpClient.PostAsync(POST_URL, new StringContent(""));
                   if (response.IsSuccessStatusCode)
                   {
                       string regId = await response.Content.ReadAsStringAsync();
                       regId = regId.Substring(1, regId.Length - 2);
                       settings.Add("__NHRegistrationId", regId);
                   }
                   else
                   {
                       throw new System.Net.WebException(response.StatusCode.ToString());
                   }
               }
           }
           return (string)settings["__NHRegistrationId"];
   
       }
4. Zapisz wszystkie zmiany.

## <a name="testing-the-application"></a>Testowanie aplikacji
1. Uruchom aplikację na Windows 8.1 i Windows Phone 8.1. Dla systemu Windows Phone 8.1 można uruchomić wystąpienie emulatora lub rzeczywistego urządzenia.
2. W przypadku Windows 8.1 aplikacji, wprowadź **Username** i **hasło** jak pokazano na poniższym zrzucie ekranu. Powinny różnić się od nazwy użytkownika i hasła, która została wprowadzona Windows Phone.
3. Kliknij przycisk **zalogować się i Zarejestruj** i sprawdź okno dialogowe pokazuje, że użytkownik zalogował się. To spowoduje także włączenie **wysyłania Push** przycisku.
   
    ![][14]
4. W wystąpieniu programu Windows Phone 8.1, wprowadź ciąg nazwy użytkownika w obu **Username** i **hasło** kliknięcie pola **logowania i rejestrowanie**.
5. Następnie w **Username Tag odbiorcy** wprowadź nazwę użytkownika w zarejestrowany na Windows 8.1. Wprowadź komunikat powiadomienia, a następnie kliknij przycisk **wysyłania Push**.
   
    ![][16]
6. Tylko na urządzeniach, które zostały zarejestrowane w usłudze pasującego tagu username komunikat powiadomienia.
   
    ![][15]

## <a name="next-steps"></a>Następne kroki
* Jeśli chcesz podzielić użytkowników na grupy zainteresowań, zobacz [Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs].
* Aby dowiedzieć się więcej o tym, jak używać usługi Notification Hubs, zobacz [wskazówki dotyczące usługi Notification Hubs].

[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[Rozpoczynanie pracy z usługą Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Secure Push]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md
[Wysyłanie najważniejszych wiadomości przy użyciu usługi Notification Hubs]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[wskazówki dotyczące usługi Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
