



W przypadku wysyłania powiadomienia szablonów, które należy podać zestaw właściwości, w tym przypadku wyślemy zbiór właściwości zawierający zlokalizowanej wersji bieżącej grup dyskusyjnych, na przykład:

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }


W tej sekcji przedstawiono sposób wysyłania powiadomień za pomocą aplikacji konsoli

Uwzględnione kodu emituje urządzeniom zarówno Sklepu Windows i iOS, ponieważ wewnętrznej bazy danych można emisji do dowolnego z obsługiwanych urządzeń.

### <a name="to-send-notifications-using-a-c-console-app"></a>Aby wysłać powiadomienia za pomocą aplikacji konsolowej C#
Modyfikowanie `SendTemplateNotificationAsync` metody w aplikacji konsoli wcześniej utworzony z następującym kodem. Zwróć uwagę, jak w tym przypadku nie jest konieczne do wysyłania wielu powiadomień dla innych języków i platform.

        private static async void SendTemplateNotificationAsync()
        {
            // Define the notification hub.
            NotificationHubClient hub = 
                NotificationHubClient.CreateClientFromConnectionString(
                    "<connection string with full access>", "<hub name>");

            // Sending the notification as a template notification. All template registrations that contain 
            // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications. 
            // This includes APNS, GCM, WNS, and MPNS template registrations.
            Dictionary<string, string> templateParams = new Dictionary<string, string>();

            // Create an array of breaking news categories.
            var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
            var locales = new string[] { "English", "French", "Mandarin" };

            foreach (var category in categories)
            {
                templateParams["messageParam"] = "Breaking " + category + " News!";

                // Sending localized News for each tag too...
                foreach( var locale in locales)
                {
                    string key = "News_" + locale;

                    // Your real localized news content would go here.
                    templateParams[key] = "Breaking " + category + " News in " + locale + "!";
                }

                await hub.SendTemplateNotificationAsync(templateParams, category);
            }
        }


Należy pamiętać, że to proste wywołanie będzie dostarczać zlokalizowanych część wiadomości, aby **wszystkie** urządzenia, niezależnie od platformy, zgodnie z Centrum powiadomień kompilacji, a następnie dostarcza poprawne ładunku natywnego określonej subskrypcji do wszystkich urządzeń tag.

### <a name="sending-the-notification-with-mobile-services"></a>Wysyłanie powiadomień z usług Mobile Services
W Twojej harmonogram usługi mobilnej można użyć następującego skryptu:

    var azure = require('azure');
    var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
    var notification = {
            "News_English": "World News in English!",
            "News_French": "World News in French!",
            "News_Mandarin", "World News in Mandarin!"
    }
    notificationHubService.send('World', notification, function(error) {
        if (!error) {
            console.warn("Notification successful");
        }
    });


