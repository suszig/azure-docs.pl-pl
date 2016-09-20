<properties
    pageTitle="Wirtualne grodzenie powiadomień wypychanych przy użyciu usług Azure Notification Hubs i Bing Spatial Data | Microsoft Azure"
    description="Korzystając z tego samouczka, dowiesz się, jak dostarczać powiadomienia wypychane oparte na lokalizacji przy użyciu usług Azure Notification Hubs i Bing Spatial Data."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="powiadomienie wypychane, powiadomienia wypychane"
    authors="dend"
    manager="yuaxu"
    editor="dend"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="dendeli"/>
    
# Wirtualne grodzenie powiadomień wypychanych przy użyciu usług Azure Notification Hubs i Bing Spatial Data
 
 > [AZURE.NOTE] Do wykonania kroków tego samouczka potrzebne jest aktywne konto platformy Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).

Korzystając z tego samouczka, dowiesz się, jak dostarczać powiadomienia wypychane oparte na lokalizacji przy użyciu usług Azure Notification Hubs i Bing Spatial Data z wykorzystaniem aplikacji platformy uniwersalnej systemu Windows.

##Wymagania wstępne
Najpierw upewnij się, że spełniono wszystkie wymagania wstępne dotyczące oprogramowania i usług:

* Program [Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) lub nowszy (można również użyć wersji [Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409)). 
* Najnowsza wersja zestawu [Azure SDK](https://azure.microsoft.com/downloads/). 
* [Konto Centrum deweloperów Map Bing](https://www.bingmapsportal.com/) (można utworzyć je bezpłatnie i skojarzyć z kontem Microsoft). 

##Wprowadzenie

Zacznijmy od utworzenia projektu. W programie Visual Studio utwórz nowy projekt typu **Pusta aplikacja (uniwersalna aplikacja systemu Windows)**.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Po zakończeniu tworzenia projektu uzyskasz dostęp do aplikacji. Teraz skonfigurujmy wszystko pod kątem infrastruktury wirtualnego grodzenia. Ponieważ w tym celu użyjemy usług Bing, istnieje publiczny punkt końcowy interfejsu API REST umożliwiający wykonywanie zapytań dotyczących określonego zakresu lokalizacji:

    http://spatial.virtualearth.net/REST/v1/data/
    
Należy określić następujące parametry, aby przygotować go do pracy:

* **Identyfikator źródła danych** i **Nazwa źródła danych** — w interfejsie API Map Bing źródła danych zawierają różne metadane w zasobnikach, takie jak lokalizacje i godziny pracy. Możesz uzyskać więcej informacji na ten temat tutaj. 
* **Nazwa jednostki** — jednostka, która ma być używana jako punkt odniesienia dla powiadomienia. 
* **Klucz interfejsu API Map Bing** — to jest klucz uzyskany wcześniej podczas tworzenia konta Centrum deweloperów Bing.
 
Poznajmy szczegółowo sposób konfiguracji poszczególnych elementów wymienionych powyżej.

##Konfigurowanie źródła danych

W tym celu można użyć Centrum deweloperów Map Bing. Kliknij pozycję **Źródła danych** na górnym pasku nawigacyjnym i wybierz pozycję **Zarządzaj źródłami danych**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Jeśli wcześniej nie korzystano z interfejsu API Map Bing, prawdopodobnie nie będą istnieć żadne źródła danych, dlatego możesz utworzyć nowe, klikając pozycję Przekaż dane do źródła danych. Wypełnij wszystkie wymagane pola:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Możliwe, że zastanawiasz się, co to jest plik danych i co należy przekazać? Na potrzeby tego testu możemy użyć przykładowego pliku opartego na potoku, który określa obszar nabrzeża San Francisco:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))
    
Powyższy kod reprezentuje następującą jednostkę:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Po prostu skopiuj i wklej powyższy ciąg do nowego pliku i zapisz go jako **NotificationHubsGeofence.pipe**, a następnie przekaż go do Centrum deweloperów Bing.

>[AZURE.NOTE]Może zostać wyświetlony monit o określenie nowego klucza dla właściwości **Klucz główny**, który różni się od właściwości **Klucz zapytania**. Po prostu utwórz nowy klucz przy użyciu pulpitu nawigacyjnego i odśwież stronę przekazywania źródła danych.

Po przekazaniu pliku danych opublikuj źródło danych. 

Przejdź do obszaru **Zarządzanie źródłami danych**, jak opisano powyżej, znajdź źródło danych na liście i kliknij polecenie **Publikuj** w kolumnie **Akcje**. Po chwili źródło danych powinno zostać wyświetlone na karcie **Opublikowane źródła danych**:

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Jeśli klikniesz pozycję **Edytuj** możesz łatwo przeglądać wprowadzone lokalizacje:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

W tym momencie w portalu nie są wyświetlane granice utworzonego wirtualnego ogrodzenia — potrzebne jest jedynie potwierdzenie, że określona lokalizacja znajduje się w odpowiedniej okolicy.

Teraz wszystkie wymagania dotyczące źródła danych zostały spełnione. Aby uzyskać szczegółowe informacje dotyczące adresu URL żądania dla wywołania interfejsu API, w Centrum deweloperów Map Bing kliknij pozycję **Źródła danych** i wybierz pozycję **Informacje o źródle danych**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

Interesuje nas tutaj wartość właściwości **Adres URL zapytania**. To jest punkt końcowy, względem którego możemy wykonywać zapytania, aby sprawdzić, czy urządzenie znajduje się obecnie w granicach lokalizacji, czy nie. Aby to sprawdzić, należy wykonać wywołanie GET dla adresu URL zapytania z dołączonymi następującymi parametrami:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

W ten sposób określisz punkt docelowy uzyskany z urządzenia, a Mapy Bing automatycznie wykonają obliczenia w celu sprawdzenia, czy znajduje się on w wirtualnym ogrodzeniu. Po wykonaniu żądania przy użyciu przeglądarki (lub programu cURL), uzyskasz standardową odpowiedź w formacie JSON:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Ta odpowiedź jest wysyłana tylko wtedy, gdy punkt rzeczywiście znajduje się w wyznaczonych granicach. W przeciwnym razie otrzymasz pusty zasobnik **results**:

![](./media/notification-hubs-geofence/bing-maps-nores.png)

##Konfigurowanie aplikacji platformy uniwersalnej systemu Windows

Teraz, gdy źródło danych jest gotowe, możemy rozpocząć pracę nad aplikacją platformy uniwersalnej systemu Windows, którą zainicjowano wcześniej.

Najpierw musimy włączyć usługi lokalizacji dla naszej aplikacji. W tym celu kliknij dwukrotnie plik `Package.appxmanifest` w **Eksploratorze rozwiązań**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

Na karcie właściwości pakietu, która została otwarta, kliknij pozycję **Możliwości** i wybierz pozycję **Lokalizacja**:

![](./media/notification-hubs-geofence/vs-package-location.png)

Po zadeklarowaniu możliwości korzystania z lokalizacji utwórz nowy folder w rozwiązaniu o nazwie `Core` i dodaj do niego nowy plik o nazwie `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

Klasa `LocationHelper` jest obecnie dosyć prosta — umożliwia jedynie uzyskiwanie lokalizacji użytkownika przy użyciu interfejsu API systemu:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

Więcej informacji o uzyskiwaniu lokalizacji użytkownika w aplikacjach platformy uniwersalnej systemu Windows można uzyskać w oficjalnym [dokumencie MSDN](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).

Aby sprawdzić, czy uzyskiwanie lokalizacji rzeczywiście działa, otwórz stronę kodu strony głównej (`MainPage.xaml.cs`). Utwórz nowy program obsługi zdarzeń dla zdarzenia `Loaded` w konstruktorze `MainPage`:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

Implementacja programu obsługi zdarzeń wygląda następująco:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Zwróć uwagę, że program obsługi został zadeklarowany jako asynchroniczny, ponieważ metoda `GetCurrentLocation` obsługuje instrukcję await, dlatego wymaga wykonywania w kontekście asynchronicznym. Ponadto w pewnych okolicznościach możemy uzyskać lokalizację o wartości null (np. usługi lokalizacji zostały wyłączone lub aplikacji odmówiono uprawnień do uzyskiwania dostępu do lokalizacji), dlatego musimy upewnić się, że zapewniona jest prawidłowa obsługa przy użyciu sprawdzania wartości null.

Uruchom aplikację. Zezwól na dostęp do lokalizacji:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Po uruchomieniu aplikacji współrzędne powinny być widoczne w oknie **Dane wyjściowe**:

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Teraz wiesz, że pobieranie lokalizacji działa. Możesz usunąć testowy program obsługi zdarzeń Loaded, ponieważ nie będzie już używany.

Następnym krokiem jest przechwytywanie zmian lokalizacji. W tym celu wróć do klasy `LocationHelper` i dodaj program obsługi zdarzeń `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

Implementacja spowoduje wyświetlenie współrzędnych lokalizacji w oknie **Dane wyjściowe**:

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

##Konfigurowanie zaplecza

Pobierz [przykład zaplecza programu .NET z usługi GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). Po zakończeniu pobierania otwórz folder `NotifyUsers`, a następnie otwórz plik `NotifyUsers.sln`.

Ustaw projekt `AppBackend` jako **Projekt startowy**, a następnie uruchom go.

![](./media/notification-hubs-geofence/vs-startup-project.png)

Projekt jest już skonfigurowany do wysyłania powiadomień wypychanych do urządzeń docelowych, dlatego musimy wykonać tylko dwie czynności — zmienić parametry połączenia na odpowiednie dla centrum powiadomień i dodać identyfikację granic, aby powiadomienie było wysyłane tylko wtedy, gdy użytkownik znajduje się w wirtualnym ogrodzeniu.

Aby skonfigurować parametry połączenia, w folderze `Models` otwórz plik `Notifications.cs`. Funkcja `NotificationHubClient.CreateClientFromConnectionString` powinna zawierać informacje o centrum powiadomień uzyskane w witrynie [Azure Portal](https://portal.azure.com) (w bloku **Zasady dostępu** w obszarze **Ustawienia**). Zapisz zaktualizowany plik konfiguracji.

Teraz należy utworzyć model dla wyniku interfejsu API Map Bing. W tym celu najłatwiej kliknąć prawym przyciskiem myszy folder `Models`, a następnie polecenie **Dodaj** > **Klasa**. Nadaj jej nazwę `GeofenceBoundary.cs`. Po zakończeniu skopiuj kod JSON z odpowiedzi interfejsu API omówionej w pierwszej sekcji, a następnie w programie Visual Studio użyj polecenia **Edytuj** > **Wklej specjalne** > **Wklej dane JSON jako klasy**. 

W ten sposób upewniamy się, że obiekt zostanie zdeserializowany dokładnie w zamierzony sposób. Wynikowy zestaw klas powinien wyglądać następująco:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Następnie otwórz folder `Controllers` > `NotificationsController.cs`. Musimy dostosować wywołanie metody POST, aby uwzględnić docelową długość i szerokość geograficzną. W tym celu po prostu dodaj dwa ciągi do sygnatury funkcji — `latitude` i `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Utwórz nową klasę w projekcie o nazwie `ApiHelper.cs`. Zostanie ona użyta do połączenia z usługą Bing w celu sprawdzenia puntów przecięcia granic. Zaimplementuj funkcję `IsPointWithinBounds` w następujący sposób:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

>[AZURE.NOTE] Zastąp punkt końcowy interfejsu API adresem URL zapytania uzyskanym wcześniej z Centrum deweloperów Bing (to samo dotyczy klucza interfejsu API). 

Jeśli uzyskano wyniki zapytania, oznacza to, że określony punkt znajduje się w granicach wirtualnego ogrodzenia, dlatego zostanie zwrócona wartość `true`. Jeśli nie ma wyników, usługa Bing informuje nas, że punkt znajduje się poza obszarem wyszukiwania, dlatego zostanie zwrócona wartość `false`.

W pliku `NotificationsController.cs` utwórz operację sprawdzania przed instrukcją switch:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

W ten sposób powiadomienia będą wysyłane tylko wtedy, gdy punkt znajduje się w granicach.

##Testowanie powiadomień wypychanych w aplikacji platformy uniwersalnej systemu Windows

Po powrocie do aplikacji platformy uniwersalnej systemu Windows powinno być teraz możliwe przetestowanie powiadomień. W klasie `LocationHelper` utwórz nową funkcję o nazwie `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

>[AZURE.NOTE] Zastąp wartość zmiennej `POST_URL` lokalizacją wdrożonej aplikacji sieci Web utworzonej w poprzedniej sekcji. Teraz możesz uruchomić aplikację lokalnie, ale podczas wdrażania wersji publicznej należy skorzystać z hostingu zewnętrznego dostawcy.

Teraz zarejestrujmy aplikację platformy uniwersalnej systemu Windows na potrzeby powiadomień wypychanych. W programie Visual Studio kliknij pozycję **Projekt** > **Magazyn** > **Skojarz aplikację z magazynem**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Po zalogowaniu się do konta dewelopera wybierz istniejącą aplikację lub utwórz nową i skojarz z nią pakiet. 

Przejdź do Centrum deweloperów i otwórz utworzoną wcześniej aplikację. Kliknij pozycję **Usługi** > **Powiadomienia wypychane** > **Witryna usług Live**.

![](./media/notification-hubs-geofence/ms-live-services.png)

W witrynie zanotuj **Klucz tajny aplikacji** i **Identyfikator SID pakietu**. Obie wartości będą potrzebne w witrynie Azure Portal. Otwórz centrum powiadomień, kliknij pozycję **Ustawienia** > **Usługi powiadomień** > **Windows (WNS)** i wprowadź informacje w wymaganych polach.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Kliknij przycisk **Zapisz**.

Kliknij prawym przyciskiem myszy pozycję **Odwołania** w **Eksploratorze rozwiązań** i wybierz pozycje **Zarządzaj pakietami NuGet**. Musimy dodać odwołanie do **biblioteki zarządzanej usługi Microsoft Azure Service Bus**. Wyszukaj pakiet `WindowsAzure.Messaging.Managed` i dodaj go do projektu.

![](./media/notification-hubs-geofence/vs-nuget.png)

W celach testowych możemy ponownie utworzyć program obsługi zdarzeń `MainPage_Loaded` i dodać do niego następujący fragment kodu:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

Powyższy kod rejestruje aplikację w centrum powiadomień. Gotowe! 

W elemencie `LocationHelper` w programie obsługi `Geolocator_PositionChanged` możesz dodać fragment kodu, który będzie wymuszać umieszczanie lokalizacji w wirtualnym ogrodzeniu:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Ponieważ nie są używane rzeczywiste współrzędne (które mogą obecnie znajdować się poza granicami), lecz wstępnie zdefiniowane wartości testowe, po aktualizacji zostanie wyświetlone powiadomienie:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

##Co dalej?

Istnieje kilka kroków, których wykonanie może być konieczne, oprócz przedstawionych powyżej, aby upewnić się, że rozwiązanie jest gotowe do zastosowania w środowisku produkcyjnym.

Najpierw należy upewnić się, że wirtualne ogrodzenia są dynamiczne. To wymaga dodatkowej pracy z interfejsem API Bing w celu umożliwienia przekazywania nowych granic w ramach istniejącego źródła danych. Aby uzyskać więcej informacji na ten temat, zapoznaj się z [dokumentacją interfejsu API usług Bing Spatial Data Services](https://msdn.microsoft.com/library/ff701734.aspx).

Upewniając się, że powiadomienia są dostarczane do odpowiednich uczestników, warto adresować je przy użyciu [tagów](notification-hubs-tags-segment-push-message.md).

W powyższym rozwiązaniu opisano scenariusz, w którym może występować wiele różnych platform, dlatego nie ograniczono wirtualnego grodzenia do możliwości specyficznych dla określonego systemu. Jednak platforma uniwersalna systemu Windows oferuje wbudowane możliwości [wykrywania wirtualnych ogrodzeń](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).

Aby uzyskać więcej informacji o możliwościach usługi Notification Hubs, odwiedź [portal dokumentacji](https://azure.microsoft.com/documentation/services/notification-hubs/).



<!--HONumber=sep16_HO1-->


