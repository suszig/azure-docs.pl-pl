<properties 
    pageTitle="Samouczek dotyczący komunikatów obsługiwanych przez przekaźnik magistrali usług | Microsoft Azure"
    description="Utwórz aplikację kliencką i usługę magistrali usług przy użyciu komunikatów obsługiwanych przez przekaźnik magistrali usług."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="05/17/2016"
    ms.author="sethm" />

# Samouczek dotyczący komunikatów obsługiwanych przez przekaźnik magistrali usług

Ten samouczek przedstawia sposób tworzenia prostej aplikacji klienckiej i usługi magistrali usług przy użyciu funkcji „przekaźnika” magistrali usług. Aby skorzystać z odpowiedniego samouczka dotyczącego [komunikatów obsługiwanych przez brokera](service-bus-messaging-overview.md#Brokered-messaging) magistrali usług, zobacz [Komunikaty obsługiwane przez brokera w usłudze Magistrala usług dla platformy .NET — samouczek](service-bus-brokered-tutorial-dotnet.md).

Ten samouczek umożliwia poznanie kroków wymaganych do utworzenia aplikacji klienckiej i aplikacji usługi magistrali usług. Podobnie jak ich odpowiedniki dla platformy WCF, usługa jest strukturą ujawniającą jeden lub więcej punktów końcowych, z których każdy ujawnia co najmniej jedną operację usługi. Punkt końcowy usługi określa adres usługi, powiązanie zawierające informacje umożliwiające klientowi komunikowanie się z usługą i kontrakt definiujący funkcje zapewniane klientom przez usługę. Główna różnica między usługą platformy WCF a usługą magistrali usług jest ujawnianie punktu końcowego w chmurze, a nie lokalnie na komputerze użytkownika.

Po zapoznaniu się z sekwencją tematów w tym samouczku będziesz mieć uruchomioną usługę i klienta, który może wywoływać operacje usługi. W pierwszym temacie opisano sposób konfigurowania konta. Następne kroki opisują sposób definiowania usługi używającej kontraktu, wdrażania usługi i konfigurowania usługi w kodzie. Opisano również sposób hostowania i uruchamiania usługi. Tworzona jest samodzielnie hostowana usługa, a klient i usługa są uruchomione na tym samym komputerze. Usługę można skonfigurować przy użyciu kodu lub pliku konfiguracji.

Ostatnie trzy kroki opisują sposób tworzenia aplikacji klienckiej, konfigurowania aplikacji klienckiej oraz tworzenia i użycia klienta, który może uzyskać dostęp do funkcji hosta.

We wszystkich tematach w tej sekcji założono, że program Visual Studio jest używany jako środowisko projektowe.

## Tworzenie konta

Pierwszym krokiem jest utworzenie przestrzeni nazw magistrali usługi i uzyskanie klucza sygnatury dostępu współdzielonego. Przestrzeń nazw wyznacza granice każdej aplikacji uwidacznianej za pośrednictwem usługi Service Bus. Kombinacja przestrzeni nazw i klucza sygnatury dostępu współdzielonego zapewnia poświadczenia dla magistrali usług umożliwiające uwierzytelnienie dostępu do aplikacji.

1. Aby utworzyć przestrzeń nazw, odwiedź [klasycznego portalu Azure][]. Kliknij pozycję **Service Bus** po lewej stronie, a następnie kliknij pozycję **Utwórz**. Wpisz nazwę przestrzeni nazw, zaakceptuj wszystkie pozostałe wartości domyślne, a następnie kliknij znacznik wyboru OK.

    >[AZURE.NOTE] Nie trzeba używać tej samej przestrzeni nazw dla aplikacji klienckiej i aplikacji usługi.

    ![][4]

1. W oknie głównym portalu kliknij nazwę przestrzeni nazw utworzonej w poprzednim kroku.

2. Kliknij kartę **Konfiguruj**, aby wyświetlić domyślne zasady dostępu współdzielonego i klucze dla przestrzeni nazw.

    ![][1]

3. Zanotuj klucz podstawowy dla zasad **RootManageSharedAccessKey** lub skopiuj go do schowka. Ta wartość zostanie użyta w dalszej części tego samouczka.

## Definiowanie kontraktu usługi WCF do użycia z magistralą usług

Kontrakt usługi określa, jakie operacje (terminologia usługi sieci Web dla metod lub funkcji) obsługuje usługa. Kontrakty są tworzone przez definiowanie interfejsu C++, C# lub Visual Basic. Każda metoda w interfejsie odpowiada określonej operacji usługi. W odniesieniu do każdego interfejsu należy zastosować atrybut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx), a w odniesieniu do każdej operacji należy zastosować atrybut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx). Jeśli metoda w interfejsie z atrybutem [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) nie ma atrybutu [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), nie jest ujawniana. Kod dla tych zadań podano w przykładzie zamieszczonym na końcu procedury. Kontrakty i usługi bardziej szczegółowo omówiono w artykule [Projektowanie i implementowanie usług](https://msdn.microsoft.com/library/ms729746.aspx) w dokumentacji platformy WCF.

### Aby utworzyć kontrakt usługi Service Bus przy użyciu interfejsu

1. Otwórz program Visual Studio jako administrator, klikając prawym przyciskiem myszy ikonę programu w menu **Start**, a następnie wybierając polecenie **Uruchom jako administrator**.

2. Utwórz nowy projekt aplikacji konsoli. Kliknij menu **Plik** i wybierz pozycję **Nowy**, a następnie kliknij pozycję **Projekt**. W oknie dialogowym **Nowy projekt** kliknij pozycję **Visual C#** (jeśli pozycja **Visual C#** nie jest wyświetlana, sprawdź w obszarze **Inne języki**). Kliknij szablon **Aplikacja konsoli** i wpisz nazwę **EchoService**. Kliknij przycisk **OK**, aby utworzyć projekt.

    ![][2]

3. Zainstaluj pakiet NuGet magistrali usług. Ten pakiet automatycznie dodaje odwołania do bibliotek usługi Service Bus, jak również przestrzeń nazw **System.ServiceModel** usługi WCF. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) jest przestrzenią nazw umożliwiającą programowy dostęp do podstawowych funkcji platformy WCF. Usługa Service Bus używa wielu obiektów i atrybutów usługi WCF do definiowania kontraktów usług.

    W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij pozycję **Zarządzaj pakietami NuGet dla rozwiązania**. Kliknij kartę **Przeglądanie**, a następnie wyszukaj ciąg `Microsoft Azure Service Bus`. Upewnij się, że nazwa projektu jest zaznaczona w polu **Wersje**. Kliknij pozycję **Zainstaluj** i zaakceptuj warunki użytkowania.

    ![][3]

3. W Eksploratorze rozwiązań kliknij dwukrotnie plik Program.cs, aby otworzyć go w edytorze, jeśli nie został jeszcze otwarty.

4. Dodaj następujące instrukcje using na początku pliku:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Zmień nazwę przestrzeni nazw z domyślnej nazwy **EchoService** na **Microsoft.ServiceBus.Samples**.

    >[AZURE.IMPORTANT] W samouczku używana jest przestrzeń nazw C# **Microsoft.ServiceBus.Samples**, która jest przestrzenią nazw zarządzanego typu kontraktu używanego w pliku konfiguracji w kroku [Konfigurowanie klienta platformy WCF](#configure-the-wcf-client). Możesz określić dowolną przestrzeń nazw podczas kompilowania tego przykładu, jednak samouczek nie będzie działać, jeśli nie zmodyfikujesz następnie odpowiednio przestrzeni nazw kontraktu i usługi w pliku konfiguracji aplikacji. Przestrzeń nazw określona w pliku App.config musi być taka sama jak przestrzeń nazw określona w plikach C#.

1. Bezpośrednio po deklaracji przestrzeni nazw `Microsoft.ServiceBus.Samples`, ale w przestrzeni nazw, zdefiniuj nowy interfejs o nazwie `IEchoContract` i zastosuj atrybut `ServiceContractAttribute` do interfejsu z wartością przestrzeni nazw **http://samples.microsoft.com/ServiceModel/Relay/**. Wartość przestrzeni nazw różni się od przestrzeni nazw używanej w kodzie. Zamiast tego wartość przestrzeni nazw jest używana jako unikatowy identyfikator dla tego kontraktu. Jawne określenie przestrzeni nazw zapobiega dodawaniu domyślnej wartości przestrzeni nazw do nazwy kontraktu.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] Zazwyczaj przestrzeń nazw kontraktu usługi zawiera schemat nazewnictwa uwzględniający informacje o wersji. Uwzględnienie informacji o wersji w przestrzeni nazw kontraktu usługi umożliwia usługom izolowanie istotnych zmian przez zdefiniowanie nowego kontraktu usługi z nową przestrzenią nazw i ujawnienie go w nowym punkcie końcowym. W ten sposób klienci mogą nadal używać starego kontraktu usługi bez konieczności aktualizacji. Informacje o wersji mogą zawierać datę lub numer kompilacji. Aby uzyskać więcej informacji, zobacz [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498) (Obsługa wersji usług). Schemat nazewnictwa przestrzeni nazw kontraktu usługi, używany w tym przykładzie, nie zawiera informacji o wersji.

1. W ramach interfejsu `IEchoContract` zadeklaruj metodę dla pojedynczej operacji uwidacznianej przez kontrakt `IEchoContract` w interfejsie i zastosuj atrybut `OperationContractAttribute` do metody, którą chcesz uwidocznić jako część publicznego kontraktu usługi Service Bus.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. Bezpośrednio po definicji interfejsu `IEchoContract` zadeklaruj kanał dziedziczący zarówno po `IEchoContract`, jak i interfejsie `IClientChannel`, w sposób pokazany poniżej:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Kanał jest obiektem platformy WCF, za pomocą którego host i klient przekazują do siebie informacje. Później napiszesz kod dla kanału obsługujący echo informacji przesyłanych między dwiema aplikacjami.

1. W menu **Kompilacja** kliknij pozycję **Kompiluj rozwiązanie** lub naciśnij klawisze **Ctrl+Shift+B**, aby potwierdzić dokładność pracy wykonanej do tej pory.

### Przykład

Poniższy kod przedstawia podstawowy interfejs definiujący kontrakt usługi Service Bus.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Teraz, gdy interfejs został utworzony, możesz go zaimplementować.

## Implementowanie kontraktu platformy WCF do użycia z magistralą usług

Aby utworzyć przekaźnik magistrali usług, należy najpierw utworzyć kontrakt definiowany przy użyciu interfejsu. Więcej informacji na temat tworzenia interfejsu podano w opisie poprzedniego kroku. Następnym krokiem jest zaimplementowanie interfejsu. Jest to związane z utworzeniem klasy o nazwie `EchoService` implementującej interfejs `IEchoContract` zdefiniowany przez użytkownika. Po zaimplementowaniu interfejsu należy go skonfigurować przy użyciu pliku konfiguracji App.config. Plik konfiguracji zawiera niezbędne informacje dla aplikacji, takie jak nazwa usługi, nazwa kontraktu i typ protokołu używanego do komunikacji z usługą Service Bus. Kod używany do wykonywania tych zadań podano w przykładzie zamieszczonym po procedurze. Bardziej ogólne omówienie sposobu implementowania kontraktu usługi można znaleźć w temacie [Implementowanie kontraktów usług](https://msdn.microsoft.com/library/ms733764.aspx) w dokumentacji platformy WCF.

1. Utwórz nową klasę o nazwie `EchoService` bezpośrednio po definicji interfejsu `IEchoContract`. Klasa `EchoService` implementuje interfejs `IEchoContract`. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Podobnie jak w przypadku innych implementacji interfejsów, można zaimplementować definicję w innym pliku. Jednak w przypadku tego samouczka implementację umieszczono w tym samym pliku, w którym znajduje się definicja interfejsu i metoda `Main`.

1. Zastosuj atrybut [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) do interfejsu `IEchoContract`. Ten atrybut określa nazwę usługi i przestrzeń nazw. Po wykonaniu tych czynności klasa `EchoService` wygląda następująco:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Zaimplementuj metodę `Echo` zdefiniowaną w interfejsie `IEchoContract` w klasie `EchoService`. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. Kliknij pozycję **Kompilacja**, a następnie kliknij pozycję **Kompiluj rozwiązanie**, aby potwierdzić dokładność wykonanej pracy.

### Aby zdefiniować konfigurację hosta usługi

1. Plik konfiguracji jest bardzo podobny do pliku konfiguracji platformy WCF. Zawiera on nazwę usługi, punkt końcowy (lokalizację ujawnianą klientom i hostom przez magistralę usług w celu wzajemnej komunikacji) i powiązanie (typ protokołu używanego do komunikacji). Główną różnicą jest odwoływanie się tego skonfigurowanego punktu końcowego usługi do powiązania [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx), które nie jest częścią programu .NET Framework. Powiązanie [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) jest jednym z powiązań zdefiniowanych przez magistralę usług.

1. W **Eksploratorze rozwiązań** kliknij dwukrotnie plik App.config, aby otworzyć go w edytorze programu Visual Studio.

2. W elemencie `<appSettings>` zastąp symbole zastępcze nazwą przestrzeni nazw usługi i kluczem sygnatury dostępu współdzielonego skopiowanym we wcześniejszym kroku. 

1. W tagach `<system.serviceModel>` dodaj element `<services>`. W pojedynczym pliku konfiguracji można skonfigurować wiele aplikacji magistrali usług. W tym samouczku zdefiniowano jednak tylko jedną.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. W elemencie `<services>` dodaj element `<service>`, aby zdefiniować nazwę usługi.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. W elemencie `<service>` zdefiniuj lokalizację kontraktu punktu końcowego i wpisz powiązanie dla punktu końcowego.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Punkt końcowy określa, gdzie klient będzie szukać aplikacji hosta. Później w samouczku ten krok zostanie użyty do utworzenia identyfikatora URI, który w pełni ujawnia hosta za pośrednictwem magistrali usług. Powiązanie deklaruje, że protokół TCP jest używany do komunikowania się z magistralą usług.

1. W menu **Kompilacja** kliknij pozycję **Kompiluj rozwiązanie**, aby potwierdzić dokładność wykonanej pracy.

### Przykład

Poniższy kod przedstawia implementację kontraktu usługi.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Poniższy kod przedstawia podstawowy format pliku App.config skojarzonego z hostem usługi.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## Hostowanie i uruchamianie podstawowej usługi sieci Web w celu zarejestrowania w magistrali usług

W tym kroku opisano sposób uruchamiania podstawowej usługi magistrali usług.

### Aby utworzyć poświadczenia magistrali usług

1. W procedurze `Main()` utwórz dwie zmienne do przechowywania przestrzeni nazw i klucza sygnatury dostępu współdzielonego odczytanego z okna konsoli.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    Klucz sygnatury dostępu współdzielonego zostanie później użyty w celu uzyskania dostępu do projektu magistrali usług. Przestrzeń nazw jest przekazywana jako parametr do procedury `CreateServiceUri` w celu utworzenia identyfikatora URI.

4. Korzystając z obiektu [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx), zadeklaruj zamiar użycia klucza sygnatury dostępu współdzielonego jako typu poświadczeń. Dodaj poniższy kod bezpośrednio po kodzie dodanym w ostatnim kroku.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### Aby utworzyć podstawowy adres usługi

1. Po kodzie dodanym w ostatnim kroku utwórz wystąpienie identyfikatora `Uri` dla podstawowego adresu usługi. Ten identyfikator URI określa schemat magistrali usług, przestrzeń nazw i ścieżkę interfejsu usługi.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    Skrót „sb” (schemat magistrali usług) oznacza, że używany jest protokół TCP. Tę deklarację umieszczono również uprzednio w pliku konfiguracji, gdy określono powiązanie [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx).
    
    W tym samouczku użyto identyfikatora URI `sb://putServiceNamespaceHere.windows.net/EchoService`.

### Aby utworzyć i skonfigurować hosta usługi

1. Ustaw tryb łączności `AutoDetect`.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Tryb łączności opisuje protokół (HTTP lub TCP) używany przez usługę do komunikowania się z magistralą usług. Korzystając z ustawienia domyślnego `AutoDetect`, usługa próbuje połączyć się z magistralą usług przy użyciu protokołu TCP (jeśli jest dostępny) lub HTTP (jeśli protokół TCP jest niedostępny). To ustawienie nie dotyczy protokołu określonego przez usługę dla komunikacji klienckiej. Ten protokół jest ustalany na podstawie używanego powiązania. Na przykład usługa może używać powiązania [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) określającego, że jej punkt końcowy (ujawniony na magistrali usług) komunikuje się z klientami za pośrednictwem protokołu HTTP. Ta sama usługa może określać powiązanie **ConnectivityMode.AutoDetect** określające, że usługa komunikuje się z magistralą usług za pośrednictwem protokołu TCP.

1. Utwórz hosta usługi przy użyciu identyfikatora URI utworzonego wcześniej w tej sekcji.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Host usługi jest obiektem platformy WCF tworzącym wystąpienie usługi. W tym miejscu możesz przekazać typ usługi, który chcesz utworzyć (typ `EchoService`), a także adres, pod którym usługa powinna być ujawniona.

1. Na początku pliku Program.cs dodaj odwołania do opisów [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) i [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx).

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. Ponownie w procedurze `Main()` skonfiguruj punkt końcowy do obsługi dostępu publicznego.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Ten krok umożliwia poinformowanie magistrali usług, że Twoja aplikacja może być powszechnie znajdowana przez zbadanie źródła danych ATOM magistrali usług dla projektu. Jeśli ustawisz opcję **DiscoveryType** z ustawieniem **private**, klient nadal mógłby uzyskać dostęp do usługi. Usługa nie byłaby jednak wyświetlana podczas wyszukiwania w przestrzeni nazw magistrali usług. Zamiast tego klient musiałby wcześniej znać ścieżkę punktu końcowego.

1. Zapisz poświadczenia usługi w punktach końcowych zdefiniowanych w pliku App.config:

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Jak zauważono w poprzednim kroku, w pliku konfiguracji możesz zadeklarować kilka usług i punktów końcowych. W takim przypadku ten kod pomijałby plik konfiguracji i wyszukiwałby każdy punkt końcowy, do którego powinien zastosować Twoje poświadczenia. W tym samouczku plik konfiguracyjny zawiera jednak tylko jeden punkt końcowy.

### Aby otworzyć hosta usługi

1. Otwórz usługę.

    ```
    host.Open();
    ```

1. Poinformuj użytkownika, że usługa jest uruchomiona, i wyjaśnij, jak zamknąć usługę.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Po zakończeniu zamknij hosta usługi.

    ```
    host.Close();
    ```

1. Naciśnij klawisze **Ctrl+Shift+B**, aby skompilować projekt.

### Przykład

Poniższy przykład zawiera kontrakt usługi i implementację z poprzednich kroków samouczka i hostuje usługę w aplikacji konsoli. Skompiluj poniższy kod w celu utworzenia pliku wykonywalnego o nazwie EchoService.exe.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## Tworzenie klienta platformy WCF dla kontraktu usługi

Następnym krokiem jest utworzenie podstawowej aplikacji klienckiej magistrali usług i zdefiniowanie kontraktu usługi, który będzie implementowany w następnych krokach. Wiele z tych kroków przypomina kroki związane z tworzeniem usługi: definiowanie kontraktu, edytowanie pliku App.config, nawiązywanie połączenia z magistralą usług przy użyciu poświadczeń i tak dalej. Kod używany do wykonywania tych zadań podano w przykładzie zamieszczonym po procedurze.

1. Utwórz nowy projekt w bieżącym rozwiązaniu programu Visual Studio dla klienta, wykonując następujące czynności:
    1. W Eksploratorze rozwiązań w tym samym rozwiązaniu, które zawiera tę usługę, kliknij prawym przyciskiem myszy bieżące rozwiązanie (nie projekt) i kliknij pozycję **Dodaj**. Następnie kliknij pozycję **Nowy projekt**.
    2. W oknie dialogowym **Dodawanie nowego projektu** kliknij pozycję **Visual C#** (jeśli pozycja **Visual C#** nie jest wyświetlana, sprawdź w obszarze **Inne języki**), wybierz szablon **Aplikacja konsoli** i wpisz nazwę **EchoClient**.
    3. Kliknij przycisk **OK**.
<br />

1. W Eksploratorze rozwiązań kliknij dwukrotnie plik Program.cs w projekcie **EchoClient**, aby otworzyć go w edytorze, jeśli nie został jeszcze otwarty.

1. Zmień nazwę przestrzeni nazw z domyślnej nazwy `EchoClient` na `Microsoft.ServiceBus.Samples`.

1. Zainstaluj [pakiet NuGet magistrali usług](https://www.nuget.org/packages/WindowsAzure.ServiceBus). W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **EchoClient**, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**. Kliknij kartę **Przeglądanie**, a następnie wyszukaj ciąg `Microsoft Azure Service Bus`. Kliknij pozycję **Zainstaluj** i zaakceptuj warunki użytkowania.

    ![][3]

1. Dodaj instrukcję `using` dla przestrzeni nazw [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) w pliku Program.cs. 

    ```
    using System.ServiceModel;
    ```

1. Dodaj definicję kontraktu usługi do przestrzeni nazw jak pokazano w poniższym przykładzie. Ta definicja jest identyczna z definicją używaną w projekcie **Usługa**. Ten kod należy dodać na początku przestrzeni nazw `Microsoft.ServiceBus.Samples`.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Naciśnij klawisze **Ctrl+Shift+B**, aby skompilować klienta.

### Przykład

Poniższy kod wyświetla bieżący stan pliku Program.cs w projekcie EchoClient.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## Konfigurowanie klienta platformy WCF

W tym kroku zostanie utworzony plik App.config dla podstawowej aplikacji klienckiej uzyskującej dostęp do usługi utworzonej wcześniej w tym samouczku. Ten plik App.config definiuje kontrakt, powiązanie i nazwę punktu końcowego. Kod używany do wykonywania tych zadań podano w przykładzie zamieszczonym po procedurze.

1. W Eksploratorze rozwiązań kliknij projekt **EchoClient** i kliknij dwukrotnie plik **App.config**, aby otworzyć go w edytorze programu Visual Studio.

2. W elemencie `<appSettings>` zastąp symbole zastępcze nazwą przestrzeni nazw usługi i kluczem sygnatury dostępu współdzielonego skopiowanym we wcześniejszym kroku.

1. W elemencie system.serviceModel dodaj element `<client>`.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Ten krok umożliwia zadeklarowanie, że definiowana jest aplikacja kliencka w stylu platformy WCF.

1. W elemencie `client` zdefiniuj nazwę, kontrakt i typ powiązania punktu końcowego.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    W tym kroku definiowana jest nazwa punktu końcowego i kontrakt zdefiniowany w usłudze oraz deklarowane jest użycie przez aplikację kliencką protokołu TCP do komunikowania się z magistralą usług. Nazwa punktu końcowego jest używana w następnym kroku do powiązania tej konfiguracji pliku końcowego z identyfikatorem URI usługi.

1. Kliknij menu **Plik**, a następnie pozycję **Zapisz wszystko**.

## Przykład

Poniższy kod przedstawia plik App.config dla klienta Echo.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## Implementowanie klienta platformy WCF do wywoływania magistrali usług

W tym kroku zostanie zaimplementowana podstawowa aplikacja kliencka uzyskująca dostęp do usługi utworzonej wcześniej w tym samouczku. Podobnie jak usługa, klient wykonuje wiele takich samych operacji w celu uzyskania dostępu do magistrali usług:

1. Ustawienie trybu łączności.
1. Utworzenie identyfikatora URI, który lokalizuje usługę hosta.
1. Uzyskanie poświadczeń zabezpieczeń.
1. Zastosowanie poświadczeń do połączenia.
1. Otwarcie połączenia.
1. Wykonanie zadań specyficznych dla aplikacji.
1. Zamknięcie połączenia.

Jedną z głównych różnic jest jednak użycie przez aplikację kliencką kanału do ustanowienia połączenia z magistralą usług, podczas gdy usługa korzysta z elementu **ServiceHost**. Kod używany do wykonywania tych zadań podano w przykładzie zamieszczonym po procedurze.

### Aby zaimplementować aplikację kliencką

1. Ustaw tryb łączności **AutoDetect**. Dodaj następujący kod w metodzie `Main()` aplikacji **EchoClient**.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Zdefiniuj zmienne do przechowywania wartości przestrzeni nazw i klucza sygnatury dostępu współdzielonego odczytanego z okna konsoli.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Utwórz identyfikator URI, który określa lokalizację hosta w projekcie magistrali usług.

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Utwórz obiekt poświadczeń dla punktu końcowego przestrzeni nazw Twojej usługi.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. Utwórz fabrykę kanałów ładującą konfigurację opisaną w pliku App.config.

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Fabryka kanałów jest obiektem platformy WCF tworzącym kanał komunikacyjny dla usługi i aplikacji klienckich.

1. Zastosuj poświadczenia magistrali usług.

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Utwórz i otwórz kanał dla usługi.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Zapisz podstawowy interfejs użytkownika i funkcje dla echa.

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    W kodzie użyto wystąpienia obiektu kanału jako serwera proxy dla usługi.

1. Zamknij kanał i fabrykę.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## Aby uruchomić aplikacje

1. Naciśnij klawisze **Ctrl+Shift+B** w celu skompilowania rozwiązania. Powoduje to skompilowanie zarówno projektu klienta, jak i projektu usługi utworzonych w poprzednich krokach.

2. Przed uruchomieniem aplikacji klienckiej musisz upewnić się, że aplikacja usługi jest uruchomiona. W Eksploratorze rozwiązań w programie Visual Studio kliknij prawym przyciskiem myszy rozwiązanie **EchoService**, a następnie kliknij pozycję **Właściwości**.

3. W oknie dialogowym właściwości rozwiązania kliknij pozycję **Projekt startowy**, a następnie kliknij przycisk **Wiele projektów startowych**. Upewnij się, że pozycja **EchoService** jest wyświetlana na początku listy. 

4. Ustaw w polu **Akcja** zarówno dla projektu **EchoService**, jak i projektu **EchoClient** ustawienie **Uruchom**.

    ![][5]

5. Kliknij pozycję **Zależności projektu**. W polu **Projekty** wybierz pozycję **EchoClient**. Upewnij się, że w polu **Zależny od** jest zaznaczona opcja **EchoService**.

    ![][6]

6. Kliknij przycisk **OK**, aby zamknąć okno dialogowe **Właściwości**.

7. Naciśnij klawisz **F5**, aby uruchomić oba projekty.

8. Oba okna konsoli zostaną otwarte z monitami o podanie nazwy przestrzeni nazw. Najpierw należy uruchomić usługę, dlatego w oknie konsoli **EchoService** wprowadź przestrzeń nazw, a następnie naciśnij klawisz **Enter**.

9. Następnie zostanie wyświetlony monit o podanie klucza sygnatury dostępu współdzielonego. Wprowadź klucz sygnatury dostępu współdzielonego i naciśnij klawisz ENTER.

    Oto przykładowe dane wyjściowe z okna konsoli. Poniższe wartości podano tutaj wyłącznie jako przykład.

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    Aplikacja usługi wyświetla w oknie konsoli adres, na którym nasłuchuje, jak w poniższym przykładzie.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. W oknie konsoli **EchoClient** wprowadź te same informacje, które zostały wprowadzone uprzednio dla aplikacji usługi. Wykonaj poprzednie kroki, aby wprowadzić tę samą przestrzeń nazw i klucz sygnatury dostępu współdzielonego dla aplikacji klienckiej.

11. Po wprowadzeniu tych wartości klient otworzy kanał do usługi i będzie monitować o wprowadzenie tekstu, jak w poniższym przykładzie danych wyjściowych konsoli.

    `Enter text to echo (or [Enter] to exit):` 

    Wprowadź tekst, który zostanie wysłany do aplikacji usługi, i naciśnij klawisz Enter. Ten tekst jest wysyłany do usługi za pośrednictwem operacji usługi Echo i pojawia się w oknie konsoli usługi, jak w poniższym przykładzie danych wyjściowych.

    `Echoing: My sample text`

    Aplikacja kliencka odbiera wartość zwracaną przez operację `Echo`, która jest oryginalnym tekstem, i wyświetla go w oknie swojej konsoli. Poniżej przestawiono przykład danych wyjściowych z okna konsoli klienta.

    `Server echoed: My sample text`

12. Możesz kontynuować wysyłanie wiadomości tekstowych z klienta do usługi w ten sposób. Po zakończeniu naciśnij klawisz Enter w oknach konsoli klienta i usługi, aby zamknąć obie aplikacje.

## Przykład

W poniższym przykładzie przedstawiono sposób tworzenia aplikacji klienckiej, wywoływania operacji usług i zamykania klienta po zakończeniu wywołania operacji.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## Następne kroki

W tym samouczku przedstawiono sposób kompilowania aplikacji klienckiej i usługi magistrali usług przy użyciu funkcji „przekaźnika” magistrali usług. Aby skorzystać z podobnego samouczka dotyczącego [komunikatów obsługiwanych przez brokera](service-bus-messaging-overview.md#Brokered-messaging) magistrali usług, zobacz [Komunikaty obsługiwane przez brokera w usłudze Magistrala usług dla platformy .NET — samouczek](service-bus-brokered-tutorial-dotnet.md).

Aby dowiedzieć się więcej na temat magistrali usług, zobacz następujące tematy:

- [Omówienie obsługi komunikatów w usłudze Service Bus](service-bus-messaging-overview.md)
- [Podstawy usługi Service Bus](service-bus-fundamentals-hybrid-solutions.md)
- [Architektura usługi Service Bus](service-bus-architecture.md)

[klasycznego portalu Azure]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png



<!--HONumber=Jun16_HO2-->


