
W tej sekcji możesz wysyłanie najważniejszych wiadomości oznakowany szablonu powiadomienia z aplikacji konsoli .NET.

Jeśli używasz funkcji Mobile Apps usługi Microsoft Azure App Service, zapoznaj się [Dodawanie powiadomień wypychanych do aplikacji mobilnej] samouczka i wybierz platformy u góry.

Jeśli chcesz użyć Java lub PHP, zapoznaj się [jak używać usługi Notification Hubs w języku Java lub PHP]. Możesz wysłać powiadomienia z dowolnego zaplecza za pomocą [interfejsu REST centra powiadomień].

Jeśli utworzono aplikacji konsoli do wysyłania powiadomień po wykonaniu [Rozpoczynanie pracy z usługą Notification Hubs], powtórz kroki 1 – 3.

1. W programie Visual Studio utwórz nową aplikację konsoli języka Visual C#:
   
      ![Łącze aplikacji konsoli][13]

2. W menu głównym programu Visual Studio wybierz **narzędzia** > **Menedżer pakietów biblioteki** > **Konsola Menedżera pakietów** , a następnie w konsoli programu okno, wprowadź następujący ciąg:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
3. Wybierz **wprowadź**.  
    Ta akcja spowoduje dodanie odwołania do zestawu SDK usługi Azure Notification Hubs z użyciem [pakietu NuGet Microsoft.Azure.Notification Hubs].

4. Otwórz plik Program.cs i dodaj następującą `using` instrukcji:
   
        using Microsoft.Azure.NotificationHubs;

5. W `Program` klasy, dodaj następującą metodę lub zastąp ją, jeśli już istnieje:
   
        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub =
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");
   
            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business",
                                            "Technology", "Science", "Sports"};
   
            // Send the notification as a template notification. All template registrations that contain
            // "messageParam" and the proper tags will receive the notifications.
            // This includes APNS, GCM, WNS, and MPNS template registrations.
   
            Dictionary<string, string> templateParams = new Dictionary<string, string>();
   
            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";
                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
         }
   
    Ten kod wysyła powiadomienie szablonu dla każdej z sześciu tagów w tablicy ciągów. Tagi zapewnia urządzenia powiadomienia tylko w przypadku zarejestrowanych kategorii.

5. W powyższym kodzie Zamień `<hub name>` i `<connection string with full access>` symbole zastępcze nazwą Centrum powiadomień i parametry połączenia dla *DefaultFullSharedAccessSignature* na pulpicie nawigacyjnym Centrum powiadomień.

6. W metodzie **Main** dodaj następujące wiersze:
   
         SendTemplateNotificationAsync();
         Console.ReadLine();

7. Tworzenie aplikacji konsoli.

<!-- Images. -->
[13]: ./media/notification-hubs-back-end/notification-hub-create-console-app.png

<!-- URLs. -->
[Rozpoczynanie pracy z usługą Notification Hubs]: ../articles/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[interfejsu REST centra powiadomień]: http://msdn.microsoft.com/library/windowsazure/dn223264.aspx
[Dodawanie powiadomień wypychanych do aplikacji mobilnej]: ../articles/app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md
[jak używać usługi Notification Hubs w języku Java lub PHP]: ../articles/notification-hubs/notification-hubs-java-push-notification-tutorial.md
[pakietu NuGet Microsoft.Azure.Notification Hubs]: http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/
