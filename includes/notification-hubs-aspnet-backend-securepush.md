## <a name="webapi-project"></a>WebAPI projektu
1. W programie Visual Studio Otwórz **AppBackend** projektu, który został utworzony w **Powiadom użytkowników** samouczka.
2. W Notifications.cs, Zastąp cały **powiadomienia** klasy następującym kodem. Należy zastąpić symbole zastępcze ciąg połączenia (z pełnym dostępem) dla Centrum powiadomień i nazwy koncentratora. Możesz uzyskać te wartości z [klasycznego portalu Azure](http://manage.windowsazure.com). Ten moduł teraz reprezentuje inną bezpieczne powiadomień, które będą wysyłane. W pełnej implementacji powiadomienia będą przechowywane w bazie danych; dla uproszczenia w tym przypadku są przechowywane ich w pamięci.
   
        public class Notification
        {
            public int Id { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications
        {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",     "{hub name}");
            }

            public Notification CreateNotification(string payload)
            {
                var notification = new Notification() {
                Id = notifications.Count,
                Payload = payload,
                Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Notification ReadNotification(int id)
            {
                return notifications.ElementAt(id);
            }
        }

1. W NotificationsController.cs, Zastąp kod wewnątrz **NotificationsController** klasy definicji z następującym kodem. Ten składnik implementuje sposób urządzenia pobrać bezpiecznie powiadomienia, a także sposób (na potrzeby tego samouczka) do wyzwolenia bezpiecznego wypychanych do urządzeń z systemem. Należy pamiętać, że podczas wysyłania powiadomień do Centrum powiadomień, tylko powiadomienie zostanie wysłane pierwotnych z Identyfikatorem powiadomienia (i żaden komunikat rzeczywiste):
   
       public NotificationsController()
       {
           Notifications.Instance.CreateNotification("This is a secure notification!");
       }
   
       // GET api/notifications/id
       public Notification Get(int id)
       {
           return Notifications.Instance.ReadNotification(id);
       }
   
       public async Task<HttpResponseMessage> Post()
       {
           var secureNotificationInTheBackend = Notifications.Instance.CreateNotification("Secure confirmation.");
           var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
           // windows
           var rawNotificationToBeSent = new Microsoft.Azure.NotificationHubs.WindowsNotification(secureNotificationInTheBackend.Id.ToString(),
                           new Dictionary<string, string> {
                               {"X-WNS-Type", "wns/raw"}
                           });
           await Notifications.Instance.Hub.SendNotificationAsync(rawNotificationToBeSent, usernameTag);
   
           // apns
           await Notifications.Instance.Hub.SendAppleNativeNotificationAsync("{\"aps\": {\"content-available\": 1}, \"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}", usernameTag);
   
           // gcm
           await Notifications.Instance.Hub.SendGcmNativeNotificationAsync("{\"data\": {\"secureId\": \"" + secureNotificationInTheBackend.Id.ToString() + "\"}}", usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }


Należy pamiętać, że `Post` metody teraz nie wysyłania wyskakujących powiadomień. Wysyła raw powiadomienie, które zawiera tylko identyfikator powiadomień, a nie poufną zawartością. Upewnij się również dodać komentarz operacji wysyłania dla platformy, dla których nie masz zostały skonfigurowane w Centrum powiadomień, jak będą powodować błędy.

1. Teraz ponownie wdrożymy tę aplikację do witryny sieci Web platformy Azure, aby udostępnić go ze wszystkich urządzeń. Kliknij prawym przyciskiem myszy projekt **AppBackend** i wybierz polecenie **Publikuj**.
2. Wybierz urządzenie docelowe publikowania witryny sieci Web platformy Azure. Zaloguj się przy użyciu konta platformy Azure i wybierz istniejącą lub nową witrynę sieci Web i zanotuj **docelowy adres URL** właściwości w **połączenia** kartę. W dalszej części tego samouczka będziemy nazywać ten adres URL *punktem końcowym zaplecza*. Kliknij przycisk **Opublikuj**.

