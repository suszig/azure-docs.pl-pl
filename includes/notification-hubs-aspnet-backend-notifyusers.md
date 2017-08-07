## <a name="create-the-webapi-project"></a>Tworzenie projektu interfejsu WebAPI
W kolejnych sekcjach zostanie utworzone nowe zaplecze interfejsu WebAPI w strukturze ASP.NET, które będzie mieć trzy główne funkcje:

1. **Uwierzytelnianie klientów**: później zostanie dodana procedura obsługi komunikatów w celu uwierzytelnienia żądań klientów i kojarzenia użytkownika z żądaniem.
2. **Rejestracje powiadomień klienta**: później dodasz kontroler do obsługi nowych rejestracji, aby urządzenie klienckie mogło otrzymywać powiadomienia. Nazwa uwierzytelnionego użytkownika zostanie automatycznie dodana do rejestracji jako [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx).
3. **Wysyłanie powiadomień do klientów**: później dodasz również kontroler udostępniający użytkownikom możliwość wyzwalania bezpiecznej operacji wypychania do urządzeń i klientów skojarzonych z tagiem. 

W następujących krokach przedstawiono sposób tworzenia nowego zaplecza interfejsu WebAPI w strukturze ASP.NET: 

> [!NOTE]
> **Ważne**: Jeśli używasz programu Visual Studio 2015 lub starszego, przed rozpoczęciem tego samouczka upewnij się, że masz zainstalowaną najnowszą wersję Menedżera pakietów NuGet. Aby to sprawdzić, uruchom program Visual Studio. W menu **Narzędzia** kliknij polecenie **Rozszerzenia i aktualizacje**. Wyszukaj pozycję **Menedżer pakietów NuGet** dla używanej przez Ciebie wersji programu Visual Studio i sprawdź, czy masz najnowszą wersję. Jeśli nie, odinstaluj, a następnie ponownie zainstaluj Menedżera pakietów NuGet.
> 
> ![][B4]
> 
> [!NOTE]
> Upewnij się, że masz zainstalowany [zestaw Azure SDK](https://azure.microsoft.com/downloads/) programu Visual Studio na potrzeby wdrażania witryny internetowej.
> 
> 

1. Uruchom program Visual Studio lub Visual Studio Express. Kliknij pozycję **Eksplorator serwera** i zaloguj się do konta platformy Azure. Program Visual Studio będzie wymagał zalogowania w celu tworzenia zasobów witryny internetowej na Twoim koncie.
2. W programie Visual Studio kliknij pozycję **Plik**, a następnie kliknij polecenie **Nowy** i **Projekt**. Rozwiń węzeł **Szablony**, **Visual C#**, kliknij pozycję **Sieć Web** i **Aplikacja sieci Web ASP.NET**, wpisz nazwę **AppBackend**, a następnie kliknij przycisk **OK**. 
   
    ![][B1]
3. W oknie dialogowym **Nowy projekt ASP.NET** kliknij pozycję **Interfejs API sieci Web**, a następnie kliknij przycisk **OK**.
   
    ![][B2]
4. W oknie dialogowym **Konfiguruj aplikację sieci Web platformy Microsoft Azure** wybierz subskrypcję i **Plan usługi App Service**, który już został utworzony. Możesz również wybrać pozycję **Utwórz nowy plan usługi App Service** i utworzyć go w oknie dialogowym. Ten samouczek nie wymaga bazy danych. Po wybraniu planu usługi App Service kliknij przycisk **OK**, aby utworzyć projekt.
   
    ![][B5]

## <a name="authenticating-clients-to-the-webapi-backend"></a>Uwierzytelnianie klientów w zapleczu interfejsu WebAPI
W tej sekcji utworzysz nową klasę procedury obsługi komunikatów o nazwie **AuthenticationTestHandler** dla nowego zaplecza. Ta klasa pochodzi od klasy [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) i jest dodawana jako procedura obsługi komunikatów na potrzeby przetwarzania wszystkich żądań przychodzących do zaplecza. 

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **AppBackend**, kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Klasa**. Nadaj nowej klasie nazwę **AuthenticationTestHandler.cs**, a następnie kliknij przycisk **Dodaj**, aby wygenerować klasę. Ta klasa będzie używana do uwierzytelniania użytkowników za pomocą *Uwierzytelniania podstawowego* dla uproszczenia. Pamiętaj, że Twoja aplikacja może używać dowolnego schematu uwierzytelniania.
2. W klasie AuthenticationTestHandler.cs dodaj następujące instrukcje `using`:
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

3. W klasie AuthenticationTestHandler.cs zastąp definicję klasy `AuthenticationTestHandler` następującym kodem. 
   
    Ta procedura obsługi będzie autoryzować żądania, gdy wszystkie trzy następujące warunki będą spełnione:
   
   * Żądanie zawiera nagłówek *Authorization*. 
   * Żądanie używa uwierzytelniania *basic*. 
   * Ciąg nazwy użytkownika i ciąg hasła to ten sam ciąg.
     
     W przeciwnym razie żądanie zostanie odrzucone. Nie jest to rzeczywiste podejście do uwierzytelniania i autoryzacji. Jest to bardzo prosty przykład na potrzeby tego samouczka.
     
     Jeśli komunikat żądania zostanie uwierzytelniony i autoryzowany przez klasę `AuthenticationTestHandler`, użytkownik podstawowego uwierzytelniania zostanie dołączony do bieżącego żądania w obiekcie [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx). Informacje o użytkowniku w obiekcie HttpContext zostaną później użyte przez inny kontroler (RegisterController) w celu dodania [tagu](https://msdn.microsoft.com/library/azure/dn530749.aspx) do żądania rejestracji powiadomienia.
     
       public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();
     
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
     > **Uwaga dotycząca zabezpieczeń**: Klasa `AuthenticationTestHandler` nie zapewnia rzeczywistego uwierzytelniania. Jest ona używana tylko do naśladowania uwierzytelniania podstawowego i nie jest bezpieczna. W swoich aplikacjach i usługach produkcyjnych musisz zaimplementować mechanizm bezpiecznego uwierzytelniania.                
     > 
     > 
4. Dodaj następujący kod na końcu metody `Register` w klasie **App_Start/WebApiConfig.cs**, aby zarejestrować procedurę obsługi komunikatów:
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());
5. Zapisz zmiany.

## <a name="registering-for-notifications-using-the-webapi-backend"></a>Rejestrowanie na potrzeby powiadomień za pomocą zaplecza interfejsu WebAPI
W tej sekcji dodamy nowy kontroler do zaplecza interfejsu WebAPI w celu obsługi żądań rejestracji użytkownika i urządzenia do otrzymywania powiadomień przy użyciu biblioteki klienta dla centrów powiadomień. Kontroler doda tag użytkownika, który został uwierzytelniony i dołączony do obiektu HttpContext przez klasę `AuthenticationTestHandler`. Tag będzie miał format ciągu `"username:<actual username>"`.

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **AppBackend**, a następnie kliknij polecenie **Zarządzaj pakietami NuGet**.
2. Po lewej stronie kliknij pozycję **Online** i wyszukaj ciąg **Microsoft.Azure.NotificationHubs** w polu **Wyszukaj**.
3. Na liście wyników kliknij pozycję **Microsoft Azure Notification Hubs**, a następnie kliknij pozycję **Instaluj**. Zakończ instalację, a następnie zamknij okno menedżera pakietów NuGet.
   
    Spowoduje to dodanie odwołania do zestawu SDK usługi Azure Notification Hubs z użyciem <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">pakietu NuGet Microsoft.Azure.Notification Hubs</a>.
4. Teraz utworzymy plik nowej klasy, która reprezentuje połączenie z centrum powiadomień używane do wysyłania powiadomień. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder **Modele**, kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Klasa**. Nadaj nowej klasie nazwę **Notifications.cs**, a następnie kliknij przycisk **Dodaj**, aby wygenerować klasę. 
   
    ![][B6]
5. W klasie Notifications.cs dodaj następującą instrukcję `using` na początku pliku:
   
        using Microsoft.Azure.NotificationHubs;
6. Zastąp definicję klasy `Notifications` poniższą definicją i pamiętaj o zastąpieniu dwóch symboli zastępczych parametrami połączenia (z pełnym dostępem) dla Twojego centrum powiadomień i nazwą centrum (dostępną w [klasycznej witrynie Azure Portal](http://manage.windowsazure.com)):
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. Następnie utworzymy nowy kontroler o nazwie **RegisterController**. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder **Kontrolery**, po czym kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Kontroler**. Kliknij pozycję **Kontroler internetowego interfejsu API 2 — pusty**, a następnie kliknij przycisk **Dodaj**. Nadaj nowej klasie nazwę **RegisterController**, a następnie ponownie kliknij przycisk **Dodaj**, aby wygenerować kontroler.
   
    ![][B7]
   
    ![][B8]
8. W pliku RegisterController.cs dodaj następujące instrukcje `using`:
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
9. Dodaj następujący kod wewnątrz definicji klasy `RegisterController`. Pamiętaj, że w tym kodzie dodajemy tag użytkownika, który jest dołączony do obiektu HttpContext. Użytkownik został uwierzytelniony i dołączony do obiektu HttpContext przez dodany przez nas filtr komunikatów, `AuthenticationTestHandler`. Można również dodać opcjonalne sprawdzenia w celu weryfikacji, czy użytkownik ma uprawnienia do rejestrowania żądanych tagów.
   
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
10. Zapisz zmiany.

## <a name="sending-notifications-from-the-webapi-backend"></a>Wysyłanie powiadomień z zaplecza interfejsu WebAPI
W tej sekcji dodasz nowy kontroler, który uwidacznia sposób wysyłania powiadomień przez urządzenia klienckie w oparciu o tag nazwy użytkownika przy użyciu biblioteki zarządzania usługi Azure Notification Hubs w zapleczu interfejsu WebAPI ASP.NET.

1. Utwórz kolejny nowy kontroler o nazwie **NotificationsController**. Utwórz go w taki sam sposób, jak kontroler **RegisterController** w poprzedniej sekcji.
2. W pliku NotificationsController.cs dodaj następujące instrukcje `using`:
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
3. Dodaj następującą metodę do klasy **NotificationsController**.
   
    Ten kod wysyła typ powiadomienia na podstawie parametru `pns` usługi powiadomień platformy (PNS, Platform Notification Service). Wartość `to_tag` służy do ustawiania tagu *username* w komunikacie. Ten tag musi być zgodny z tagiem username aktywnej rejestracji centrum powiadomień. Komunikat powiadomienia jest pobierany z treści żądania POST i formatowany dla docelowej usługi PNS. 
   
    W zależności od usługi PNS, używanej do odbierania powiadomień przez obsługiwane urządzenia, różne powiadomienia są obsługiwane przy użyciu różnych formatów. Na przykład w przypadku urządzeń z systemem Windows możesz użyć [wyskakujących powiadomień za pomocą usługi WNS](https://msdn.microsoft.com/library/windows/apps/br230849.aspx), które nie są bezpośrednio obsługiwane przez inną usługę PNS. Dlatego zaplecze musi sformatować powiadomienie jako obsługiwane powiadomienie w przypadku usługi PNS urządzeń, które planujesz obsługiwać. Następnie użyj odpowiedniego interfejsu API wysyłania w klasie [NotificationHubClient](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)
   
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
4. Naciśnij klawisz **F5**, aby uruchomić aplikację i sprawdzić dokładność pracy wykonanej do tej pory. Aplikacja powinna uruchomić przeglądarkę internetową i wyświetlić stronę główną struktury ASP.NET. 

## <a name="publish-the-new-webapi-backend"></a>Publikowanie nowego zaplecza interfejsu WebAPI
1. Teraz wdrożymy tę aplikację w witrynie internetowej platformy Azure, aby udostępnić ją wszystkim urządzeniom. Kliknij prawym przyciskiem myszy projekt **AppBackend** i wybierz polecenie **Publikuj**.
2. Wybierz pozycję **Microsoft Azure App Service** jako docelową lokalizację publikacji, a następnie kliknij przycisk **Publikuj**. Spowoduje to otwarcie okna dialogowego Tworzenie usługi App Service, które ułatwi Ci utworzenie wszystkich zasobów platformy Azure niezbędnych do uruchomienia aplikacji internetowej ASP.NET na platformie Azure.

    ![][B15]
3. W oknie dialogowym **Tworzenie usługi App Service** wybierz swoje konto platformy Azure. Kliknij pozycję **Zmień typ** i wybierz pozycję **Aplikacja sieci Web**. Zachowaj wartość w polu **Nazwa aplikacji sieci Web** i wybierz wartości w polach **Subskrypcja**, **Grupa zasobów** i **Plan usługi App Service**.  Kliknij przycisk **Utwórz**.

4. Zanotuj wartość właściwości **Adres URL witryny** w sekcji **Podsumowanie**. W dalszej części tego samouczka będziemy nazywać ten adres URL *punktem końcowym zaplecza*. Kliknij przycisk **Opublikuj**.

5. Po zakończeniu działania kreatora aplikacja internetowa ASP.NET zostanie opublikowana na platformie Azure, a następnie uruchomiona w domyślnej przeglądarce.  Twoja aplikacja będzie widoczna w usłudze Azure App Service.

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
