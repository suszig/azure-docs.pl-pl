<properties
    pageTitle="Hybrydowa aplikacja lokalna/w chmurze (platforma .NET) | Microsoft Azure"
    description="Dowiedz się, jak utworzyć hybrydową aplikację lokalną/w chmurze platformy .NET przy użyciu przekaźnika usługi Azure Service Bus."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="05/23/2016"
    ms.author="sethm"/>

# Tworzenie hybrydowej aplikacji lokalnej/w chmurze platformy .NET przy użyciu przekaźnika usługi Azure Service Bus

## Wprowadzenie

Ten artykuł opisuje sposób tworzenia hybrydowej aplikacji w chmurze przy użyciu platformy Microsoft Azure i programu Visual Studio. W tym samouczku założono, że nie masz wcześniejszego doświadczenia w używaniu platformy Azure. W mniej niż 30 minut utworzysz aplikację korzystającą z wielu zasobów platformy Azure i działającą w chmurze.

Dowiesz się:

-   Jak utworzyć lub przystosować istniejącą usługę sieci Web do użytku przez rozwiązanie sieci Web.
-   Jak używać przekaźnika usługi Azure Service Bus do udostępniania danych między aplikacją platformy Azure a usługą sieci Web hostowaną w innym miejscu.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## Jak przekaźnik usługi Service Bus pomaga w tworzeniu rozwiązań hybrydowych

Rozwiązania biznesowe zwykle składają się z kombinacji niestandardowego kodu napisanego w celu spełnienia nowych i unikatowych wymagań biznesowych oraz istniejących funkcjonalności dostarczonych przez już stosowane rozwiązania i systemy.

Architekci rozwiązań zaczynają stosować usługi w chmurze w celu łatwiejszej obsługi wymagań skali i obniżenia kosztów operacyjnych. W ten sposób dowiadują się, że istniejące elementy zawartości usług, których chcieliby użyć jako bloków konstrukcyjnych dla swoich rozwiązań, znajdują się za firmową zaporą i są trudno dostępne dla rozwiązania w chmurze. Wiele wewnętrznych usług nie jest zbudowanych lub hostowanych w sposób umożliwiający ich łatwe uwidocznienie na krawędzi sieci firmowej.

Przekaźnik usługi Service Bus został zaprojektowany w celu bezpiecznego zapewniania dostępu do istniejących usług sieci Web Windows Communication Foundation (WCF) rozwiązaniom, które znajdują się poza firmą, bez konieczności wprowadzania istotnych zmian w infrastrukturze sieci firmowej. Takie usługi przekaźnika usługi Service Bus wciąż są hostowane wewnątrz istniejącego środowiska, ale delegują one nasłuchiwanie sesji i żądań przychodzących do usługi Service Bus hostowanej w chmurze. Usługa Service Bus chroni także te usługi przed nieautoryzowanym dostępem przy użyciu uwierzytelniania za pomocą [sygnatury dostępu współdzielonego](service-bus-sas-overview.md) (SAS, Shared Access Signature).

## Scenariusz rozwiązania

W tym samouczku utworzysz witrynę sieci Web ASP.NET, która umożliwi wyświetlanie listy produktów na stronie spisu produktów.

![][0]

W samouczku założono, że informacje o produktach znajdują się w istniejącym systemie lokalnym i używasz przekaźnika usługi Service Bus, aby uzyskać dostęp do tego systemu. Jest to symulowane przez usługę sieci Web, która działa w prostej aplikacji konsolowej i jest uzupełniana przez zestaw produktów w pamięci. Będziesz w stanie uruchomić tę aplikację konsolową na własnym komputerze i wdrożyć rolę sieci Web na platformie Azure. W ten sposób przekonasz się, że rola sieci Web działająca w centrum danych Azure rzeczywiście wywoła Twój komputer, mimo że prawie na pewno znajduje się on za przynajmniej jedną zaporą i warstwą translatora adresów sieciowych (NAT, network address translation).

Poniżej przedstawiono zrzut ekranu strony startowej ukończonej aplikacji sieci Web.

![][1]

## Konfigurowanie środowiska deweloperskiego

Przed rozpoczęciem tworzenia aplikacji dla platformy Azure pobierz potrzebne narzędzia i skonfiguruj swoje środowisko deweloperskie.

1.  Zainstaluj zestaw Azure SDK dla platformy .NET, który znajduje się w sekcji [Pobierz narzędzia i zestaw SDK][].

2.  Kliknij link **Instalowanie zestawu SDK** dla używanej wersji programu Visual Studio. W krokach tego samouczka używany jest program Visual Studio 2015.

4.  Gdy zostanie wyświetlony monit o uruchomienie lub zapisanie instalatora, kliknij przycisk **Uruchom**.

5.  W **Instalatorze platformy sieci Web** kliknij przycisk **Zainstaluj** i kontynuuj instalację.

6.  Po zakończeniu instalacji będziesz mieć do dyspozycji wszystkie narzędzia niezbędne do tworzenia aplikacji. Zestaw SDK zawiera narzędzia, które pozwalają w łatwy sposób tworzyć aplikacje dla platformy Azure w programie Visual Studio. Jeśli nie masz zainstalowanego programu Visual Studio, zestaw SDK zainstaluje również bezpłatny program Visual Studio Express.

## Tworzenie przestrzeni nazw

Aby rozpocząć korzystanie z funkcji usługi Service Bus na platformie Azure, musisz najpierw utworzyć przestrzeń nazw usługi. Przestrzeń nazw zapewnia kontener określania zakresu na potrzeby adresowania zasobów usługi Service Bus w aplikacji.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## Tworzenie serwera lokalnego

Najpierw utworzysz lokalny (pozorny) system katalogu produktów. Będzie to dość proste. Możesz go traktować jako rzeczywisty lokalny system katalogu produktów z kompletną powierzchnią usług, którą próbujemy zintegrować.

Ten projekt jest aplikacją konsolową programu Visual Studio i używa [pakietu NuGet usługi Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) w celu uwzględnienia bibliotek i ustawień konfiguracji usługi Service Bus.

### Tworzenie projektu

1.  Korzystając z uprawnień administratora, uruchom program Microsoft Visual Studio. Aby uruchomić program Visual Studio z uprawnieniami administratora, kliknij prawym przyciskiem myszy ikonę programu **Visual Studio**, a następnie kliknij polecenie **Uruchom jako administrator**.

2.  W menu **Plik** programu Visual Studio kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**.

3.  W sekcji **Zainstalowane szablony** obszaru **Visual C#** kliknij pozycję **Aplikacja konsolowa**. W polu **Nazwa** wpisz nazwę **ProductsServer**:

    ![][11]

4.  Kliknij przycisk **OK**, aby utworzyć projekt **ProductsServer**.

7.  Jeśli masz już zainstalowany menedżer pakietów NuGet dla programu Visual Studio, przejdź do następnego kroku. W przeciwnym razie odwiedź stronę menedżera pakietów [NuGet][] i kliknij przycisk [Install NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) (Zainstaluj menedżer pakietów NuGet). Postępuj zgodnie z monitami, aby zainstalować menedżera pakietów NuGet, a następnie ponownie uruchom program Visual Studio.

7.  W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **ProductsServer**, a następnie kliknij polecenie **Zarządzaj pakietami NuGet**.

8.  Kliknij kartę **Przeglądanie**, a następnie wyszukaj ciąg `Microsoft Azure Service Bus`. Kliknij pozycję **Zainstaluj** i zaakceptuj warunki użytkowania.

    ![][13]

    Zwróć uwagę na to, że wymagane zestawy klientów są teraz przywoływane.

9.  Dodaj nową klasę dla kontraktu produktu. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **ProductsServer** i kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Klasa**.

10. W polu **Nazwa** wpisz nazwę **ProductsContract.cs**. Następnie kliknij pozycję **Dodaj**.

11. W pliku **ProductsContract.cs** zastąp definicję przestrzeni nazw następującym kodem, który definiuje kontrakt dla usługi.

    ```
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;
    
        // Define the data contract for the service
        [DataContract]
        // Declare the serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }
    
        // Define the service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();
    
        }
    
        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```

12. W pliku Program.cs zastąp definicję przestrzeni nazw następującym kodem, który dodaje usługę profilu i jej hosta.

    ```
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;
    
        // Implement the IProducts interface.
        class ProductsService : IProducts
        {
    
            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };
    
            // Display a message in the service console application
            // when the list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }
    
        }
    
        class Program
        {
            // Define the Main() function in the service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();
    
                Console.WriteLine("Press ENTER to close");
                Console.ReadLine();
    
                sh.Close();
            }
        }
    }
    ```

13. W Eksploratorze rozwiązań kliknij dwukrotnie plik **App.config**, aby otworzyć go w edytorze programu Visual Studio. W dolnej części elementu **&lt;system.ServiceModel&gt;** (ale nadal w ramach elementu &lt;system.ServiceModel&gt;) dodaj następujący kod XML. Koniecznie zastąp ciąg *yourServiceNamespace* nazwą Twojej przestrzeni nazw, a ciąg *yourKey* kluczem SAS, który został wcześniej pobrany z portalu:

    ```
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
14. W pliku App.config w elemencie **&lt;appSettings&gt;** zastąp wartość parametrów połączenia parametrami połączenia, które wcześniej zostały uzyskane z portalu. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

14. Naciśnij kombinację klawiszy **Ctrl+Shift+B** lub w menu **Kompilacja** kliknij pozycję **Kompiluj rozwiązanie**, aby skompilować aplikację i sprawdzić dokładność pracy wykonanej do tej pory.

## Tworzenie aplikacji ASP.NET

W tej sekcji utworzysz prostą aplikację ASP.NET, która będzie wyświetlać dane pobrane z usługi produktów.

### Tworzenie projektu

1.  Upewnij się, że program Visual Studio jest uruchomiony z uprawnieniami administratora.

2.  W menu **Plik** programu Visual Studio kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**.

3.  W sekcji **Zainstalowane szablony** w obszarze **Visual C#** kliknij pozycję **Aplikacja sieci Web ASP.NET**. Nazwij projekt **ProductsPortal**. Następnie kliknij przycisk **OK**.

    ![][15]

4.  Na liście **Wybierz szablon** kliknij pozycję **MVC**. 

6.  Zaznacz pole wyboru opcji **Hostuj w chmurze**.

    ![][16]

5. Kliknij przycisk **Zmień uwierzytelnianie**. W oknie dialogowym **Zmienianie uwierzytelniania** kliknij pozycję **Bez uwierzytelniania**, a następnie kliknij przycisk **OK**. W tym samouczku wdrożysz aplikację, która nie wymaga logowania użytkownika.

    ![][18]

6.  Upewnij się, że w sekcji **Microsoft Azure** okna dialogowego **Nowy projekt ASP.NET** zostało zaznaczone pole wyboru **Hostuj w chmurze**, a na liście rozwijanej została zaznaczona pozycja **App Service**.

    ![][19]

7. Kliknij przycisk **OK**. 

8. Teraz musisz skonfigurować zasoby platformy Azure dla nowej aplikacji sieci Web. Wykonaj wszystkie czynności opisane w sekcji [Konfigurowanie zasobów Azure dla nowej aplikacji sieci Web](../app-service-web/web-sites-dotnet-get-started.md#configure-azure-resources-for-a-new-web-app). Następnie wróć do tego samouczka i przejdź do następnego kroku.

5.  W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy pozycję **Modele** i kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Klasa**. W polu **Nazwa** wpisz nazwę **Product.cs**. Następnie kliknij pozycję **Dodaj**.

    ![][17]

### Modyfikowanie aplikacji sieci Web

1.  W pliku Product.cs w programie Visual Studio zastąp istniejącą definicję przestrzeni nazw następującym kodem.

    ```
    // Declare properties for the products inventory.
    namespace ProductsWeb.Models
    {
        public class Product
        {
            public string Id { get; set; }
            public string Name { get; set; }
            public string Quantity { get; set; }
        }
    }
    ```

2.  W Eksploratorze rozwiązań rozwiń folder **Kontrolery**, a następnie kliknij dwukrotnie plik **HomeController.cs**, aby otworzyć go w programie Visual Studio.

3. W pliku **HomeController.cs** zastąp istniejącą definicję przestrzeni nazw następującym kodem.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;
    
        public class HomeController : Controller
        {
            // Return a view of the products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```

3.  W Eksploratorze rozwiązań rozwiń folder Views\Shared, a następnie kliknij dwukrotnie plik **_Layout.cshtml**, aby otworzyć go w edytorze programu Visual Studio.

5.  Zamień wszystkie wystąpienia ciągu **My ASP.NET Application** na **LITWARE's Products**.

6. Usuń linki **Home**, **About** oraz **Contact**. W poniższym przykładzie usuń wyróżniony kod.

    ![][41]

7.  W Eksploratorze rozwiązań rozwiń folder Views\Home, a następnie kliknij dwukrotnie plik **Index.cshtml**, aby otworzyć go w edytorze programu Visual Studio.
    Zastąp całą zawartość pliku następującym kodem.

    ```
    @model IEnumerable<ProductsWeb.Models.Product>
    
    @{
            ViewBag.Title = "Index";
    }
    
    <h2>Prod Inventory</h2>
    
    <table>
            <tr>
                <th>
                    @Html.DisplayNameFor(model => model.Name)
                </th>
                  <th></th>
                <th>
                    @Html.DisplayNameFor(model => model.Quantity)
                </th>
            </tr>
    
    @foreach (var item in Model) {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Name)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Quantity)
                </td>
            </tr>
    }
    
    </table>
    ```

9.  Aby sprawdzić dokładność pracy wykonanej do tej pory, naciśnij kombinację klawiszy **Ctrl+Shift+B** w celu skompilowania projektu.


### Lokalne uruchamianie aplikacji

Uruchom aplikację, aby sprawdzić, czy działa.

1.  Upewnij się, że projekt **ProductsPortal** jest aktywnym projektem. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu i wybierz polecenie **Ustaw jako projekt startowy**.
2.  W programie Visual Studio naciśnij klawisz F5.
3.  Aplikacja powinna uruchomić się w przeglądarce.

    ![][21]

## Składanie fragmentów

Następny krok polega na połączeniu lokalnego serwera produktów z aplikacją ASP.NET.

1.  Jeśli nie jest jeszcze otwarty, w programie Visual Studio ponownie otwórz projekt **ProductsPortal**, który został utworzony w sekcji „Tworzenie aplikacji ASP.NET”.

2.  Podobnie jak w sekcji „Tworzenie serwera lokalnego” dodaj pakiet NuGet do odwołań projektu. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **ProductsPortal**, a następnie kliknij polecenie **Zarządzaj pakietami NuGet**.

3.  Wyszukaj ciąg „Service Bus” i wybierz pozycję **Microsoft Azure Service Bus**. Następnie zakończ instalację i zamknij to okno dialogowe.

4.  W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **ProductsPortal**, kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Istniejący element**.

5.  Przejdź do pliku **ProductsContract.cs** z projektu konsolowego **ProductsServer**. Kliknij, aby zaznaczyć plik ProductsContract.cs. Kliknij strzałkę w dół obok pozycji **Dodaj**, a następnie kliknij polecenie **Dodaj jako link**.

    ![][24]

6.  W edytorze programu Visual Studio otwórz plik **HomeController.cs** i zastąp definicję przestrzeni nazw następującym kodem. Koniecznie zastąp ciąg *yourServiceNamespace* nazwą Twojej przestrzeni nazw, a ciąg *yourKey* kluczem SAS. Dzięki temu klient będzie miał możliwość wywołania usługi lokalnej i zwrócenia wyniku wywołania.

    ```
    namespace ProductsWeb.Controllers
    {
        using System.Linq;
        using System.ServiceModel;
        using System.Web.Mvc;
        using Microsoft.ServiceBus;
        using Models;
        using ProductsServer;
    
        public class HomeController : Controller
        {
            // Declare the channel factory.
            static ChannelFactory<IProductsChannel> channelFactory;
    
            static HomeController()
            {
                // Create shared access signature token credentials for authentication.
                channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                    "sb://yourServiceNamespace.servicebus.windows.net/products");
                channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                    TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                        "RootManageSharedAccessKey", "yourKey") });
            }
    
            public ActionResult Index()
            {
                using (IProductsChannel channel = channelFactory.CreateChannel())
                {
                    // Return a view of the products inventory.
                    return this.View(from prod in channel.GetProducts()
                                     select
                                         new Product { Id = prod.Id, Name = prod.Name,
                                             Quantity = prod.Quantity });
                }
            }
        }
    }
    ```

7.  W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie **ProductsPortal**, kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Istniejący projekt**.

8.  Przejdź do projektu **ProductsServer**, a następnie kliknij dwukrotnie plik rozwiązania **ProductsServer.csproj**, aby go dodać.

9.  Serwer **ProductsServer** musi być uruchomiony, aby wyświetlić dane w aplikacji **ProductsPortal**. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie **ProductsPortal** i kliknij polecenie **Właściwości**. Wyświetli się okno dialogowe **Strony właściwości**.

10. Po lewej stronie kliknij pozycję **Projekt startowy**. Po prawej stronie kliknij pozycję **Wiele projektów startowych**. Upewnij się, że projekty **ProductsServer** i **ProductsPortal** są wymienione w tej kolejności i dla obu tych projektów ustawiono akcję **Uruchomienie**.

      ![][25]

11. W oknie dialogowym **Właściwości** kliknij pozycję **Zależności projektu**, która znajduje się po lewej stronie.

12. Na liście **Projekty** kliknij projekt **ProductsServer**. Upewnij się, że projekt **ProductsPortal** **nie** jest wybrany.

14. Na liście **Projekty** kliknij projekt **ProductsPortal**. Upewnij się, że projekt **ProductsServer** jest wybrany. 

    ![][26]

15. W oknie dialogowym **Strony właściwości** kliknij przycisk **OK**.

## Lokalne uruchamianie projektu

Aby przetestować aplikację lokalnie, w programie Visual Studio naciśnij klawisz **F5**. Serwer lokalny (**ProductsServer**) powinien uruchomić się jako pierwszy, a następnie aplikacja **ProductsPortal** powinna uruchomić się w oknie przeglądarki. Tym razem pojawi się spis produktów zawierający dane pobrane z lokalnego systemu usługi produktów.

![][10]

Naciśnij przycisk **Odśwież** na stronie **ProductsPortal**. Przy każdym odświeżeniu strony aplikacja serwera wyświetli komunikat podczas wywoływany metody `GetProducts()` z serwera **ProductsServer**.

## Wdrażanie projektu ProductsPortal w aplikacji sieci Web platformy Azure

Następny krok polega na przekonwertowaniu frontonu projektu **ProductsPortal** w aplikację sieci Web platformy Azure. Najpierw przeprowadź wdrożenie projektu **ProductsPortal**, postępując zgodnie z instrukcjami w sekcji [Wdrażanie projektu sieci Web w aplikacji sieci Web platformy Azure](../app-service-web/web-sites-dotnet-get-started.md#deploy-the-web-project-to-the-azure-web-app). Po zakończeniu wdrażania wróć do tego samouczka i przejdź do następnego kroku. 

Skopiuj adres URL wdrożonej aplikacji sieci Web, ponieważ będzie potrzebny w kolejnym kroku. Ten adres URL możesz również uzyskać w oknie Działanie usługi Azure App Service w programie Visual Studio:

![][9] 
   

> [AZURE.NOTE] Gdy projekt sieci Web **ProductsPortal** zostanie automatycznie uruchomiony po wdrożeniu, w oknie przeglądarki może zostać wyświetlony komunikat o błędzie. Jest to oczekiwane. Błąd występuje, ponieważ aplikacja **ProductsServer** nie jest jeszcze uruchomiona.

### Ustawianie projektu ProductsPortal jako aplikacji sieci Web

Przed uruchomieniem aplikacji w chmurze musisz się upewnić, że aplikacja **ProductsPortal** jest uruchamiana z poziomu programu Visual Studio jako aplikacja sieci Web.

1. W programie Visual Studio kliknij prawym przyciskiem myszy projekt **ProjectsPortal**, a następnie kliknij polecenie **Właściwości**.

3. W lewej kolumnie kliknij pozycję **Sieć Web**.

5. W sekcji **Akcja uruchamiania** kliknij przycisk **Początkowy adres URL** i w polu tekstowym wprowadź adres URL wcześniej wdrożonej aplikacji sieci Web, na przykład `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

6. W menu **Plik** programu Visual Studio kliknij polecenie **Zapisz wszystko**.

7. W menu Kompilacja programu Visual Studio kliknij polecenie **Kompiluj ponownie rozwiązanie**.

## Uruchamianie aplikacji

2.  Naciśnij klawisz F5, aby skompilować i uruchomić aplikację. Serwer lokalny (aplikacja konsolowa **ProductsServer**) powinien uruchomić się jako pierwszy, a następnie aplikacja **ProductsPortal** powinna uruchomić się w oknie przeglądarki, jak pokazano na poniższym zrzucie ekranu. Ponownie pojawi się spis produktów zawierający dane pobrane z lokalnego systemu usługi produktów, a dane zostaną wyświetlone w aplikacji sieci Web. Sprawdź adres URL, aby upewnić się, że aplikacja **ProductsPortal** działa w chmurze jako aplikacja sieci Web platformy Azure. 

    ![][1]

    > [AZURE.IMPORTANT] Aplikacja konsolowa **ProductsServer** musi działać i być w stanie udostępniać dane aplikacji **ProductsPortal**. Jeśli przeglądarka wyświetla komunikat o błędzie, zaczekaj kilka sekund, aż serwer **ProductsServer** zostanie załadowany i wyświetli następujący komunikat. Następnie naciśnij przycisk **Odśwież** w przeglądarce.

    ![][37]

3. W przeglądarce naciśnij przycisk **Odśwież** na stronie **ProductsPortal**. Przy każdym odświeżeniu strony aplikacja serwera wyświetli komunikat podczas wywoływany metody `GetProducts()` z serwera **ProductsServer**.

    ![][38]

## Następne kroki  

Aby dowiedzieć się więcej na temat usługi Service Bus, zobacz następujące zasoby:  

* [Azure Service Bus][sbwacom]  
* [Jak używać kolejek usługi Service Bus][sbwacomqhowto]  


  [0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
  [1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [Pobierz narzędzia i zestaw SDK]: http://go.microsoft.com/fwlink/?LinkId=271920
  [NuGet]: http://nuget.org
  
  [11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
  [13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
  [15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
  [16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
  [17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
  [18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
  [19]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-6.png
  [9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
  [10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

  [21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
  [24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
  [25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
  [26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
  [27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png
  
  [36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
  [37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
  [38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
  [41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
  [43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png


  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md




<!--HONumber=sep16_HO1-->


