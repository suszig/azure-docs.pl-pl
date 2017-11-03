## <a name="create-the-webapi-project"></a>Utwórz projekt WebAPI
Kolejnych sekcjach omówiono tworzenie nowych zaplecze ASP.NET WebAPI. Ten proces ma trzy główne cele:

* **Uwierzytelnianie klientów**: Dodaj program obsługi komunikatów dalej do uwierzytelniania żądań klientów i skojarzyć użytkownika z żądaniem.

* **Zarejestrowanie powiadomienia za pomocą WebAPI zaplecza**: Dodawanie kontrolera do obsługi nowej rejestracji dla urządzenia klienckiego otrzymywać powiadomienia. Nazwa uwierzytelnionego użytkownika jest automatycznie dodawany do rejestracji jako [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx).

* **Wyślij powiadomienia do klientów**: również dodać z kontrolerem, aby umożliwić użytkownikom wyzwalanie bezpieczne powiadomień wypychanych do urządzeń i klientów, skojarzonych ze znacznikiem. 

Utwórz nowego zaplecza ASP.NET WebAPI, wykonując następujące czynności: 

> [!IMPORTANT]
> Jeśli używasz programu Visual Studio 2015 lub starsze, przed rozpoczęciem tego samouczka, upewnij się, że zainstalowano najnowszą wersję Menedżera pakietów NuGet dla programu Visual Studio. 
>
>Aby to sprawdzić, uruchom program Visual Studio. Na **narzędzia** menu, wybierz opcję **rozszerzenia i aktualizacje**. Wyszukaj **Menedżera pakietów NuGet** w wersji programu Visual Studio i upewnij się, że masz najnowszą wersję. Jeśli używana wersja nie jest najnowsza wersja, odinstaluj ją i ponownie zainstalować Menedżera pakietów NuGet.
 
![][B4]

> [!NOTE]
> Upewnij się, że masz zainstalowany [zestaw Azure SDK](https://azure.microsoft.com/downloads/) programu Visual Studio na potrzeby wdrażania witryny internetowej.
> 
> 

1. Uruchom program Visual Studio lub Visual Studio Express. 

2. Wybierz **Eksploratora serwera**i zaloguj się do konta platformy Azure. Można utworzyć zasoby witryny sieci web na Twoim koncie, muszą być podpisane.

3. W programie Visual Studio, wybierz **pliku** > **nowy** > **projektu**, rozwiń węzeł **szablony**, rozwiń węzeł **Visual C#**, a następnie wybierz **Web** i **aplikacji sieci Web ASP.NET**.

4. W **nazwa** wpisz **AppBackend**, a następnie wybierz **OK**. 
   
    ![Okno nowy projekt][B1]

5. W **nowy projekt ASP.NET** wybierz **interfejsu API sieci Web** pole wyboru, a następnie wybierz **OK**.
   
    ![Okno nowy projekt ASP.NET][B2]

6. W **Konfigurowanie aplikacji sieci Web Microsoft Azure** okna, wybierz subskrypcję, a następnie w **planu usługi aplikacji** listy, wykonaj jedną z następujących czynności:

    * Wybierz plan usługi aplikacji, które zostały już utworzone. 
    * Wybierz **Utwórz nowy plan usługi aplikacji**, a następnie utworzyć jeden. 
    
  Ten samouczek nie wymaga bazy danych. Po wybraniu planu usługi aplikacji, wybierz **OK** Aby utworzyć projekt.
   
    ![Okno Konfigurowanie aplikacji sieci Web Microsoft Azure][B5]

## <a name="authenticate-clients-to-the-webapi-back-end"></a>Uwierzytelnianie klientów do WebAPI zaplecza
W tej sekcji, Utwórz nową klasę programu obsługi wiadomości o nazwie **AuthenticationTestHandler** dla nowego zaplecza. Ta klasa pochodzi od [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) i dodawane jako program obsługi komunikatów tak, aby mógł on przetwarzać wszystkie żądania tego wejdzie w wewnętrznej. 

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **AppBackend** projektu, zaznacz **Dodaj**, a następnie wybierz **klasy**. 
 
2. Nazwa nowej klasy **AuthenticationTestHandler.cs**, a następnie wybierz **Dodaj** do generowania klasy. Ta klasa służy do uwierzytelniania użytkowników za pomocą *uwierzytelnianie podstawowe* dla uproszczenia. Aplikację można używać żadnych schematu uwierzytelniania.

3. W klasie AuthenticationTestHandler.cs dodaj następujące instrukcje `using`:
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

4. W AuthenticationTestHandler.cs, Zastąp `AuthenticationTestHandler` klasy definicji z następującym kodem: 
   
    Program obsługi autoryzacji żądania, gdy spełnione są następujące trzy warunki:
   
   * Żądanie zawiera *autoryzacji* nagłówka. 
   * Żądanie używa uwierzytelniania *basic*. 
   * Ciąg nazwy użytkownika i ciąg hasła to ten sam ciąg.
     
  W przeciwnym razie żądanie zostanie odrzucone. Nie jest to rzeczywiste podejście do uwierzytelniania i autoryzacji. W tym samouczku jest tylko bardzo prosty przykład.
     
  Jeśli uwierzytelnieniu i autoryzacji przez komunikat żądania `AuthenticationTestHandler`, uwierzytelnianie podstawowe użytkownika jest dołączony do bieżącego żądania na [element HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx). Informacje o użytkowniku w element HttpContext będą korzystali innego kontrolera (RegisterController) później można dodać [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx) do żądania rejestracji powiadomienia.
     
       public class AuthenticationTestHandler : DelegatingHandler
       {
           protected override Task<HttpResponseMessage> SendAsync(
           HttpRequestMessage request, CancellationToken cancellationToken)
           {
               var authorizationHeader = request.Headers.GetValues("Authorization").First();
     
               if (authorizationHeader != null && authorizationHeader
                   .StartsWith("Basic ", StringComparison.InvariantCultureIgnoreCase))
               {
                   string authorizationUserAndPwdBase64 =
                       authorizationHeader.Substring("Basic ".Length);
                   string authorizationUserAndPwd = Encoding.Default
                       .GetString(Convert.FromBase64String(authorizationUserAndPwdBase64));
                   string user = authorizationUserAndPwd.Split(':')[0];
                   string password = authorizationUserAndPwd.Split(':')[1];
     
                   if (verifyUserAndPwd(user, password))
                   {
                       // Attach the new principal object to the current HttpContext object
                       HttpContext.Current.User =
                           new GenericPrincipal(new GenericIdentity(user), new string[0]);
                       System.Threading.Thread.CurrentPrincipal =
                           System.Web.HttpContext.Current.User;
                   }
                   else return Unauthorized();
               }
               else return Unauthorized();
     
               return base.SendAsync(request, cancellationToken);
           }
     
           private bool verifyUserAndPwd(string user, string password)
           {
               // This is not a real authentication scheme.
               return user == password;
           }
     
           private Task<HttpResponseMessage> Unauthorized()
           {
               var response = new HttpResponseMessage(HttpStatusCode.Forbidden);
               var tsc = new TaskCompletionSource<HttpResponseMessage>();
               tsc.SetResult(response);
               return tsc.Task;
           }
       }
     
     > [!NOTE]
     > Uwaga dotycząca zabezpieczeń: `AuthenticationTestHandler` klasa nie zapewnia uwierzytelniania wartość true. Jest ona używana tylko do naśladowania uwierzytelniania podstawowego i nie jest bezpieczna. W swoich aplikacjach i usługach produkcyjnych musisz zaimplementować mechanizm bezpiecznego uwierzytelniania.                
     > 
     > 
5. Aby zarejestrować program obsługi komunikatów, Dodaj następujący kod na końcu `Register` metody w **App_Start/WebApiConfig.cs** klasy:
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());

6. Zapisz zmiany.

## <a name="register-for-notifications-by-using-the-webapi-back-end"></a>Zarejestrować odbieranie powiadomień za pomocą WebAPI zaplecza
W tej sekcji możesz dodać nowego kontrolera do WebAPI zaplecza do obsługi żądań, aby zarejestrować użytkowników i urządzeń dla powiadomień za pomocą biblioteki klienta dla usługi notification hubs. Kontroler dodaje tag użytkownika dla użytkownika, który został uwierzytelniony i powiązany element HttpContext przez `AuthenticationTestHandler`. Tag będzie miał format ciągu `"username:<actual username>"`.

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **AppBackend** projektu, a następnie wybierz **Zarządzaj pakietami NuGet**.

2. W okienku po lewej stronie wybierz **Online** , a następnie w **wyszukiwania** wpisz **Microsoft.Azure.NotificationHubs**.

3. Na liście wyników wybierz **Microsoft Azure Notification Hubs**, a następnie wybierz **zainstalować**. Zakończenie instalacji, a następnie zamknij okno Menedżera pakietów NuGet.
   
    Ta akcja spowoduje dodanie odwołania do zestawu SDK usługi Azure Notification Hubs z użyciem <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakietu NuGet Microsoft.Azure.Notification Hubs</a>.

4. Utwórz nowy plik klasy, która reprezentuje połączenie z Centrum powiadomień, który służy do wysyłania powiadomień. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modele** folderu, wybierz opcję **Dodaj**, a następnie wybierz **klasy**. Nazwa nowej klasy **Notifications.cs**, a następnie wybierz **Dodaj** do generowania klasy. 
   
    ![Dodaj nowy element okna][B6]

5. W klasie Notifications.cs dodaj następującą instrukcję `using` na początku pliku:
   
        using Microsoft.Azure.NotificationHubs;

6. Zastąp `Notifications` klasy definicji z następującym kodem, a następnie zastąp symbole zastępcze dwa parametry połączenia (z pełnym dostępem) dla Centrum powiadomień i nazwy koncentratora (dostępne pod adresem [klasycznego portalu Azure](http://manage.windowsazure.com)):
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. Następnie należy utworzyć nowy kontroler o nazwie **RegisterController**. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **kontrolerów** folderu, wybierz opcję **Dodaj**, a następnie wybierz **kontrolera**. 

8. Wybierz **kontrolera 2 interfejsu API sieci Web — pusty**, a następnie wybierz **Dodaj**.
   
    ![Okno Dodawanie szkieletu][B7]
   
9. W **nazwy kontrolera** wpisz **RegisterController** nowej klasie nazwę, a następnie wybierz **Dodaj**.

    ![Okno Dodawanie kontrolera][B8]

10. W pliku RegisterController.cs dodaj następujące instrukcje `using`:
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;

11. Dodaj następujący kod wewnątrz definicji klasy `RegisterController`. Należy pamiętać, że w tym kodzie możemy dodać tag użytkownika dla użytkownika, który jest powiązany element HttpContext. Użytkownik został uwierzytelniony i powiązany element HttpContext przez filtr komunikatów, które dodano, `AuthenticationTestHandler`. Można również dodać opcjonalne sprawdzenia w celu weryfikacji, czy użytkownik ma uprawnienia do rejestrowania żądanych tagów.
   
        private NotificationHubClient hub;
   
        public RegisterController()
        {
            hub = Notifications.Instance.Hub;
        }
   
        public class DeviceRegistration
        {
            public string Platform { get; set; }
            public string Handle { get; set; }
            public string[] Tags { get; set; }
        }
   
        // POST api/register
        // This creates a registration id
        public async Task<string> Post(string handle = null)
        {
            string newRegistrationId = null;
   
            // make sure there are no existing registrations for this push handle (used for iOS and Android)
            if (handle != null)
            {
                var registrations = await hub.GetRegistrationsByChannelAsync(handle, 100);
   
                foreach (RegistrationDescription registration in registrations)
                {
                    if (newRegistrationId == null)
                    {
                        newRegistrationId = registration.RegistrationId;
                    }
                    else
                    {
                        await hub.DeleteRegistrationAsync(registration);
                    }
                }
            }
   
            if (newRegistrationId == null) 
                newRegistrationId = await hub.CreateRegistrationIdAsync();
   
            return newRegistrationId;
        }
   
        // PUT api/register/5
        // This creates or updates a registration (with provided channelURI) at the specified id
        public async Task<HttpResponseMessage> Put(string id, DeviceRegistration deviceUpdate)
        {
            RegistrationDescription registration = null;
            switch (deviceUpdate.Platform)
            {
                case "mpns":
                    registration = new MpnsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "wns":
                    registration = new WindowsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "apns":
                    registration = new AppleRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "gcm":
                    registration = new GcmRegistrationDescription(deviceUpdate.Handle);
                    break;
                default:
                    throw new HttpResponseException(HttpStatusCode.BadRequest);
            }
   
            registration.RegistrationId = id;
            var username = HttpContext.Current.User.Identity.Name;
   
            // add check if user is allowed to add these tags
            registration.Tags = new HashSet<string>(deviceUpdate.Tags);
            registration.Tags.Add("username:" + username);
   
            try
            {
                await hub.CreateOrUpdateRegistrationAsync(registration);
            }
            catch (MessagingException e)
            {
                ReturnGoneIfHubResponseIsGone(e);
            }
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        // DELETE api/register/5
        public async Task<HttpResponseMessage> Delete(string id)
        {
            await hub.DeleteRegistrationAsync(id);
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        private static void ReturnGoneIfHubResponseIsGone(MessagingException e)
        {
            var webex = e.InnerException as WebException;
            if (webex.Status == WebExceptionStatus.ProtocolError)
            {
                var response = (HttpWebResponse)webex.Response;
                if (response.StatusCode == HttpStatusCode.Gone)
                    throw new HttpRequestException(HttpStatusCode.Gone.ToString());
            }
        }
12. Zapisz zmiany.

## <a name="send-notifications-from-the-webapi-back-end"></a>Wysyłanie powiadomień z zaplecza WebAPI
W tej sekcji możesz dodać nowego kontrolera, który opisuje sposób w przypadku urządzeń klienckich do wysyłania powiadomień. Powiadomienie jest oparta na tag nazwy użytkownika, który używa zaplecza ASP.NET WebAPI Biblioteka zarządzania usługi centra powiadomień Azure.

1. Utwórz innego nowego kontrolera o nazwie **NotificationsController** taki sam sposób tworzenia **RegisterController** w poprzedniej sekcji.

2. W pliku NotificationsController.cs dodaj następujące instrukcje `using`:
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;

3. Dodaj następującą metodę do **NotificationsController** klasy:
   
    Typ powiadomienia, który jest oparty na usługi powiadomień platformy (PNS) wysyła ten kod `pns` parametru. Wartość `to_tag` służy do ustawiania tagu *username* w komunikacie. Ten tag musi być zgodny z tagiem username aktywnej rejestracji centrum powiadomień. Komunikat powiadomienia jest pobierany z treści żądania POST i formatowany dla docelowej usługi PNS. 
   
    W zależności od systemu powiadomień platformy obsługiwane urządzenia używanego do odbierania powiadomień powiadomienia są obsługiwane przez różne formaty. Na przykład urządzenia z systemem Windows, może użyć [wyskakujących powiadomień z usługą WNS](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) który bezpośrednio nie jest obsługiwany przez inny systemu powiadomień platformy. W wystąpieniu Twoje zaplecza musi format powiadomienia w obsługiwanych powiadomienie w przypadku systemu powiadomień platformy urządzeń, które mają być obsługuje. Następnie użyj odpowiedni interfejs API wysyłania na [NotificationHubClient klasy](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx).
   
        public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag)
        {
            var user = HttpContext.Current.User.Identity.Name;
            string[] userTag = new string[2];
            userTag[0] = "username:" + to_tag;
            userTag[1] = "from:" + user;
   
            Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;
            HttpStatusCode ret = HttpStatusCode.InternalServerError;
   
            switch (pns.ToLower())
            {
                case "wns":
                    // Windows 8.1 / Windows Phone 8.1
                    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" + 
                                "From " + user + ": " + message + "</text></binding></visual></toast>";
                    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
                    break;
                case "apns":
                    // iOS
                    var alert = "{\"aps\":{\"alert\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(alert, userTag);
                    break;
                case "gcm":
                    // Android
                    var notif = "{ \"data\" : {\"message\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendGcmNativeNotificationAsync(notif, userTag);
                    break;
            }
   
            if (outcome != null)
            {
                if (!((outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Abandoned) ||
                    (outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Unknown)))
                {
                    ret = HttpStatusCode.OK;
                }
            }
   
            return Request.CreateResponse(ret);
        }

4. Aby uruchomić aplikację i upewnij się, dokładność pracy wykonanej do tej pory, zaznacz **F5** klucza. Aplikacja otwiera w przeglądarce sieci web i jest wyświetlany na stronie głównej programu ASP.NET. 

## <a name="publish-the-new-webapi-back-end"></a>Publikowanie nowego zaplecza WebAPI
Następnie można wdrożyć aplikację na witrynie systemu Azure, aby udostępnić go ze wszystkich urządzeń. 

1. Kliknij prawym przyciskiem myszy **AppBackend** projektu, a następnie wybierz **publikowania**.

2. Wybierz **Microsoft Azure App Service** jako lokalizacja docelowa publikowania, a następnie wybierz **publikowania**.  
    Zostanie otwarte okno Tworzenie usługi App Service. Tutaj możesz tworzyć wszystkie niezbędne zasoby platformy Azure do uruchomienia aplikacji sieci web platformy ASP.NET na platformie Azure.

    ![Na kafelku Microsoft Azure App Service][B15]

3. W **Tworzenie usługi App Service** okna, wybierz konto platformy Azure. Wybierz **Zmień typ** > **sieci Web aplikacji**. Zachowaj ustawienie domyślne **Nazwa aplikacji sieci Web**, a następnie wybierz **subskrypcji**, **grupy zasobów**, i **planu usługi App Service**. 

4. Wybierz pozycję **Utwórz**.

5. Zanotuj wartość właściwości **Adres URL witryny** w sekcji **Podsumowanie**. Ten adres URL jest Twoje *punktu końcowego zaplecza* dalszej części samouczka. 

6. Wybierz **publikowania**.

Po zakończeniu pracy z kreatorem, publikuje aplikacji sieci web platformy ASP.NET na platformie Azure, a następnie otwiera aplikacji w domyślnej przeglądarce.  Aplikacja jest wyświetlana w usługi aplikacji Azure.

Adres URL używa określonej wcześniej nazwy aplikacji internetowej w formacie http://<nazwa_aplikacji>.azurewebsites.net.

[B1]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push1.png
[B2]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push2.png
[B3]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push3.png
[B4]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push4.png
[B5]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push5.png
[B6]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push6.png
[B7]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push7.png
[B8]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push8.png
[B14]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push14.png
[B15]: ./media/notification-hubs-aspnet-backend-notifyusers/publish-to-app-service.png
[B16]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users16.PNG
[B18]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users18.PNG
