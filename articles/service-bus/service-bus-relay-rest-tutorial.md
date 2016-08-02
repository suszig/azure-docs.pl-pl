<properties
    pageTitle="Samouczek dotyczący komunikatów obsługiwanych przez przekaźnik usługi Service Bus przy użyciu interfejsu REST | Microsoft Azure"
    description="Utwórz prostą aplikację hosta przekaźnika usługi Service Bus, która uwidacznia interfejs REST."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="06/03/2016"
    ms.author="sethm" />

# Samouczek dotyczący interfejsu REST usługi Service Bus

W tym samouczku opisano sposób tworzenia prostej aplikacji hosta usługi Service Bus, która uwidacznia interfejs REST. Interfejs REST umożliwia klientowi sieci Web, takiemu jak przeglądarka sieci Web, uzyskiwanie dostępu do interfejsów API usługi Service Bus za pomocą żądań HTTP.

W tym samouczku użyto modelu programowania interfejsu REST usługi WCF (Windows Communication Foundation) w celu utworzenia usługi REST w usłudze Service Bus. Aby uzyskać więcej informacji, zobacz [Model programowania interfejsu REST usługi WCF](https://msdn.microsoft.com/library/bb412169.aspx) oraz [Projektowanie i implementowanie usług](https://msdn.microsoft.com/library/ms729746.aspx) w dokumentacji usługi WCF.

## Krok 1. Tworzenie przestrzeni nazw usługi

Pierwszym krokiem jest utworzenie przestrzeni nazw i uzyskanie klucza sygnatury dostępu współdzielonego (SAS, Shared Access Signature). Przestrzeń nazw wyznacza granice każdej aplikacji uwidacznianej za pośrednictwem usługi Service Bus. Klucz sygnatury dostępu współdzielonego jest automatycznie generowany przez system po utworzeniu przestrzeni nazw usługi. Kombinacja przestrzeni nazw i klucza sygnatury dostępu współdzielonego usługi dostarcza poświadczenia dla usługi Service Bus w celu uwierzytelnienia dostępu do aplikacji.

1. Aby utworzyć przestrzeń nazw usługi, przejdź do [klasycznego portalu Azure][]. Kliknij pozycję **Service Bus** po lewej stronie, a następnie kliknij pozycję **Utwórz**. Wpisz nazwę przestrzeni nazw, a następnie kliknij znacznik wyboru.

2. W oknie głównym [klasycznego portalu Azure][] kliknij nazwę przestrzeni nazw utworzonej w poprzednim kroku.

3. Kliknij kartę **Konfiguracja**.

4. W sekcji **generatora klucza dostępu współdzielonego** zanotuj **Klucz podstawowy** skojarzony z zasadami **RootManageSharedAccessKey** lub skopiuj go do schowka. Ta wartość zostanie użyta w dalszej części tego samouczka.

## Krok 2. Definiowanie kontraktu usługi WCF opartego na interfejsie REST do użycia z usługą Service Bus

Podobnie jak w przypadku innych usług korzystających z usługi Service Bus, podczas tworzenia usługi opartej na interfejsie REST należy zdefiniować kontrakt. Kontrakt określa operacje obsługiwane przez hosta. Operacja usługi może być rozumiana jako metoda usługi sieci Web. Kontrakty są tworzone przez definiowanie interfejsu C++, C# lub Visual Basic. Każda metoda w interfejsie odpowiada określonej operacji usługi. W odniesieniu do każdego interfejsu należy zastosować atrybut [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx), a w odniesieniu do każdej operacji należy zastosować atrybut [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx). Jeśli metoda w interfejsie z atrybutem [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) nie ma atrybutu [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), nie jest uwidaczniana. Kod używany do wykonywania tych zadań podano w przykładzie zamieszczonym na końcu procedury.

Główną różnicą między podstawowym kontraktem usługi Service Bus, a kontraktem opartym na interfejsie REST jest dodanie właściwości do atrybutu [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Ta właściwość umożliwia mapowanie metody w interfejsie na metodę po drugiej stronie interfejsu. W tym przypadku użyjemy atrybutu [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx), aby powiązać metodę z metodą GET protokołu HTTP. Dzięki temu usługa Service Bus może dokładnie pobierać i interpretować polecenia wysyłane do interfejsu.

### Aby utworzyć kontrakt usługi Service Bus przy użyciu interfejsu

1. Otwórz program Visual Studio jako administrator, klikając prawym przyciskiem myszy ikonę programu w menu **Start**, a następnie klikając polecenie **Uruchom jako administrator**.

2. Utwórz nowy projekt aplikacji konsoli. Kliknij menu **Plik** i wybierz pozycję **Nowy**, a następnie wybierz pozycję **Projekt**. W oknie dialogowym **Nowy projekt** kliknij pozycję **Visual C#**, wybierz szablon **Aplikacja konsolowa** i nadaj mu nazwę **ImageListener**. Użyj domyślnej **lokalizacji**. Kliknij przycisk **OK**, aby utworzyć projekt.

3. W przypadku projektu C# program Visual Studio utworzy plik `Program.cs`. Ta klasa zawiera pustą metodę `Main()` wymaganą do prawidłowej kompilacji projektu aplikacji konsolowej.

4. Dodaj odwołania do usługi Service Bus i pliku **System.ServiceModel.dll** w projekcie, instalując pakiet NuGet usługi Service Bus. Ten pakiet automatycznie dodaje odwołania do bibliotek usługi Service Bus, jak również przestrzeń nazw **System.ServiceModel** usługi WCF. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **ImageListener**, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**. Kliknij kartę **Przeglądanie**, a następnie wyszukaj ciąg `Microsoft Azure Service Bus`. Kliknij pozycję **Zainstaluj** i zaakceptuj warunki użytkowania.

5. Należy jawnie dodać odwołanie do pliku **System.ServiceModel.Web.dll** w projekcie:

    a. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder **References** w folderze projektu i kliknij pozycję **Dodaj odwołanie**.

    b. W oknie dialogowym **Dodawanie odwołania** kliknij kartę **Struktura** po lewej stronie i w polu **Wyszukaj** wpisz **System.ServiceModel.Web**. Zaznacz pole wyboru **System.ServiceModel.Web** i kliknij przycisk **OK**.

6. Dodaj następujące instrukcje `using` w górnej części pliku Program.cs.

    ```
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```

    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) jest przestrzenią nazw umożliwiającą programowy dostęp do podstawowych funkcji usługi WCF. Usługa Service Bus używa wielu obiektów i atrybutów usługi WCF do definiowania kontraktów usług. Ta przestrzeń nazw będzie występować w większości aplikacji przekaźnika usługi Service Bus. Podobnie przestrzeń nazw [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) pomaga zdefiniować kanał będący obiektem służącym do komunikacji z usługą Service Bus i przeglądarką sieci Web klienta. Wreszcie przestrzeń nazw [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) zawiera typy umożliwiające tworzenie aplikacji sieci Web.

7. Zmień nazwę przestrzeni nazw `ImageListener` na **Microsoft.ServiceBus.Samples**.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```

8. Bezpośrednio po otwierającym nawiasie klamrowym deklaracji przestrzeni nazw zdefiniuj nowy interfejs o nazwie **IImageContract** i zastosuj atrybut **ServiceContractAttribute** do interfejsu z wartością `http://samples.microsoft.com/ServiceModel/Relay/`. Wartość przestrzeni nazw różni się od przestrzeni nazw używanej w kodzie. Zamiast tego wartość przestrzeni nazw jest używana jako unikatowy identyfikator dla tego kontraktu i powinna zawierać informacje o wersji. Aby uzyskać więcej informacji, zobacz [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498) (Obsługa wersji usług). Jawne określenie przestrzeni nazw zapobiega dodawaniu domyślnej wartości przestrzeni nazw do nazwy kontraktu.

    ```
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```

9. W ramach interfejsu `IImageContract` zadeklaruj metodę dla pojedynczej operacji uwidacznianej przez kontrakt `IImageContract` w interfejsie i zastosuj atrybut `OperationContractAttribute` do metody, którą chcesz uwidocznić jako część publicznego kontraktu usługi Service Bus.

    ```
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```

10. W atrybucie **OperationContract** dodaj wartość **WebGet**.

    ```
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```

    Umożliwi to usłudze Service Bus kierowanie żądań metody GET protokołu HTTP do `GetImage` i translację wartości zwracanych metody `GetImage` na odpowiedzi metody GETRESPONSE protokołu HTTP. W dalszej części tego samouczka użyjesz przeglądarki sieci Web, aby uzyskać dostęp do tej metody i wyświetlić obraz w przeglądarce.

11. Bezpośrednio po definicji `IImageContract` zadeklaruj kanał dziedziczący z interfejsów `IImageContract` i `IClientChannel`.

    ```
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```

    Kanał jest obiektem usługi WCF, za pomocą którego usługa i klient przekazują do siebie informacje. W dalszej części utworzysz kanał w aplikacji hosta. Usługa Service Bus następnie użyje tego kanału do przekazywania żądań metody GET protokołu HTTP z przeglądarki do implementacji metody **GetImage**. Za pomocą kanału usługa Service Bus pobiera również wartość zwracaną metody **GetImage** i przekształca ją na wartość metody GETRESPONSE protokołu HTTP dla przeglądarki klienta.

12. W menu **Kompiluj** kliknij pozycję **Kompiluj rozwiązanie**, aby potwierdzić dokładność pracy wykonanej do tej pory.

### Przykład

Poniższy kod przedstawia podstawowy interfejs definiujący kontrakt usługi Service Bus.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## Krok 3. Implementowanie kontraktu usługi WCF opartego na interfejsie REST do użycia z usługą Service Bus

Aby utworzyć usługę korzystającą z usługi Service Bus i opartą na interfejsie REST, należy najpierw utworzyć kontrakt definiowany przy użyciu interfejsu. Następnym krokiem jest zaimplementowanie interfejsu. Jest to związane z utworzeniem klasy o nazwie **ImageService** implementującej interfejs **IImageContract** zdefiniowany przez użytkownika. Po zaimplementowaniu kontraktu należy skonfigurować interfejs przy użyciu pliku App.config. Plik konfiguracji zawiera niezbędne informacje dla aplikacji, takie jak nazwa usługi, nazwa kontraktu i typ protokołu używanego do komunikacji z usługą Service Bus. Kod używany do wykonywania tych zadań podano w przykładzie zamieszczonym po procedurze.

Tak jak w przypadku poprzednich kroków różnica między implementacją kontraktu opartego na interfejsie REST i podstawowego kontraktu usługi Service Bus jest niewielka.

### Aby zaimplementować kontrakt usługi Service Bus oparty na interfejsie REST

1. Utwórz nową klasę o nazwie **ImageService** bezpośrednio po definicji interfejsu **IImageContract**. Klasa **ImageService** implementuje interfejs **IImageContract**.

    ```
    class ImageService : IImageContract
    {
    }
    ```
    Podobnie jak w przypadku innych implementacji interfejsów, można zaimplementować definicję w innym pliku. Jednak w przypadku tego samouczka implementację umieszczono w tym samym pliku, w którym znajduje się definicja interfejsu i metoda `Main()`.

2. Zastosuj atrybut [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) do klasy **IImageService**, aby wskazać, że klasa jest implementacją kontraktu usługi WCF.

    ```
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```

    Jak wspomniano wcześniej, ta przestrzeń nazw nie jest tradycyjną przestrzenią nazw. Zamiast tego jest częścią architektury usługi WCF, która identyfikuje kontrakt. Aby uzyskać więcej informacji, zobacz [Nazwy kontraktów danych](https://msdn.microsoft.com/library/ms731045.aspx) w dokumentacji usługi WCF.

3. Dodaj obraz jpg do projektu.  

    Jest to obraz, który usługa wyświetla w przeglądarce będącej odbiorcą. Kliknij prawym przyciskiem myszy projekt i kliknij polecenie **Dodaj**. Następnie kliknij pozycję **Istniejący element**. Użyj okna dialogowego **Dodawanie istniejącego elementu**, aby przejść do odpowiedniego pliku jpg, a następnie kliknij przycisk **Dodaj**.

    Podczas dodawania pliku upewnij się, że pozycja **Wszystkie pliki** jest zaznaczona na liście rozwijanej obok pola **Nazwa pliku:**. W pozostałej części tego samouczka przyjęto założenie, że nazwa obrazu to „image.jpg”. Jeśli plik ma inną nazwę, należy zmienić nazwę pliku lub odpowiednio zmienić kod.

4. Aby upewnić się, że uruchomiona usługa może odnaleźć plik obrazu, w **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy plik obrazu, a następnie kliknij pozycję **Właściwości**. W okienku **Właściwości** ustaw opcję **Kopiuj do katalogu wyjściowego** na wartość **Kopiuj, jeśli nowszy**.

5. Dodaj do projektu odwołanie do zestawu **System.Drawing.dll** oraz następujące skojarzone instrukcje `using`.  

    ```
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```

6. W klasie **ImageService** dodaj następujący konstruktor, który ładuje mapę bitową i przygotowuje ją do wysłania do przeglądarki klienta.

    ```
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```

7. Bezpośrednio po poprzednim kodzie dodaj następującą metodę **GetImage** w klasie **ImageService** służącą do zwracania komunikatu protokołu HTTP zawierającego obraz.

    ```
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);

        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

        return stream;
    }
    ```

    W tej implementacji element **MemoryStream** służy do pobierania obrazu i przygotowywania go do przesyłania strumieniowego do przeglądarki. Rozpoczyna przekazywanie strumieniowe w pozycji zero, deklaruje zawartość strumienia jako obraz jpeg i przekazuje strumieniowo informacje.

8. W menu **Kompiluj** kliknij pozycję **Kompiluj rozwiązanie**.

### Aby zdefiniować konfigurację uruchamiania usługi sieci Web w usłudze Service Bus

1. W **Eksploratorze rozwiązań** kliknij dwukrotnie plik **App.config**, aby otworzyć go w edytorze programu Visual Studio.

    Plik **App.config** przypomina plik konfiguracji usługi WCF i zawiera on nazwę usługi, punkt końcowy (lokalizację uwidacznianą klientom i hostom przez usługę w celu wzajemnej komunikacji) i powiązanie (typ protokołu używanego do komunikacji). Główną różnicą jest odwoływanie się tego skonfigurowanego punktu końcowego usługi do powiązania [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx), które nie jest częścią programu .NET Framework.

2. Element XML `<system.serviceModel>` jest elementem usługi WCF definiującym co najmniej jedną usługę. Tutaj został użyty do zdefiniowania nazwy usługi i punktu końcowego. W dolnej części elementu `<system.serviceModel>` (ale nadal w elemencie `<system.serviceModel>`) dodaj element `<bindings>` mający następującą zawartość. Definiuje ona powiązania używane w aplikacji. Można zdefiniować wiele powiązań, ale w tym samouczku definiowane jest tylko jedno.

    ```
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```

    W tym kroku definiowane jest powiązanie [WebHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.webhttprelaybinding.aspx) usługi Service Bus z ustawieniem **relayClientAuthenticationType** o wartości **None**. To ustawienie wskazuje, że punkt końcowy korzystający z tego powiązania nie wymaga poświadczeń klienta.

3. Po elemencie `<bindings>` dodaj element `<services>`. Podobnie jak w przypadku powiązań, w pojedynczym pliku konfiguracji można zdefiniować wiele usług. Jednak w tym samouczku definiowana jest tylko jedna.

    ```
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```

    W tym kroku konfigurowana jest usługa korzystająca z wcześniej zdefiniowanego domyślnego powiązania **webHttpRelayBinding**. Używana jest również domyślna wartość **sbTokenProvider**, która jest definiowana w następnym kroku.

4. Po elemencie `<services>` utwórz element `<behaviors>` o następującej zawartości, zastępując element „SAS_KEY” *sygnaturą dostępu współdzielonego* uzyskaną w [klasycznego portalu Azure][] w kroku 1.

    ```
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```

5. W pliku App.config w elemencie `<appSettings>` zastąp całą wartość parametrów połączenia parametrami połączenia, które zostały wcześniej uzyskane z portalu. 

    ```
    <appSettings>
    <!-- Service Bus specific app settings for messaging connections -->
    <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```

6. W menu **Kompiluj** kliknij pozycję **Kompiluj rozwiązanie**, aby skompilować całe rozwiązanie.

### Przykład

Poniższy kod przedstawia implementację kontraktu i usługi opartej na interfejsie REST uruchomionej w usłudze Service Bus przy użyciu powiązania **WebHttpRelayBinding**.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Poniższy kod przedstawia plik App.config skojarzony z usługą.

```
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="[SAS_KEY]" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
</configuration>
```

## Krok 4. Hostowanie usługi WCF opartej na interfejsie REST, aby korzystała z usługi Service Bus

W tym kroku opisano sposób uruchamiania usługi sieci Web przy użyciu aplikacji konsolowej w usłudze Service Bus. Pełny kod napisany w tym kroku podano w przykładzie poniżej procedury.

### Aby utworzyć podstawowy adres usługi

1. W deklaracji funkcji `Main()` utwórz zmienną do przechowywania przestrzeni nazw projektu usługi Service Bus. Pamiętaj, aby zastąpić wartość `yourNamespace` nazwą przestrzeni nazw usługi, którą utworzono wcześniej.

    ```
    string serviceNamespace = "yourNamespace";
    ```
    Usługa Service Bus używa przestrzeni nazw do utworzenia unikatowego identyfikatora URI.

2. Utwórz wystąpienie identyfikatora `Uri` dla podstawowego adresu usługi, która jest oparta na tej przestrzeni nazw.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### Aby utworzyć i skonfigurować hosta usługi sieci Web

- Utwórz hosta usługi sieci Web przy użyciu adresu URI utworzonego wcześniej w tej sekcji.

    ```
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    Host usługi jest obiektem usługi WCF tworzącym wystąpienie aplikacji hosta. W tym przykładzie przekazywany jest do niego typ hosta, który ma zostać utworzony (klasa **ImageService**) oraz adres, pod którym ma zostać uwidoczniona aplikacja hosta.

### Aby uruchomić hosta usługi sieci Web

1. Otwórz usługę.

    ```
    host.Open();
    ```
    Usługa jest teraz uruchomiona.

2. Wyświetl komunikat z informacją, że usługa jest uruchomiona, oraz informacją o sposobie zatrzymania usługi.

    ```
    Console.WriteLine("Copy the following address into a browser to see the image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

3. Po zakończeniu zamknij hosta usługi.

    ```
    host.Close();
    ```

## Przykład

Poniższy przykład zawiera kontrakt usługi i implementację z poprzednich kroków samouczka i hostuje usługę w aplikacji konsolowej. Skompiluj następujący kod do pliku wykonywalnego o nazwie ImageListener.exe.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy the following address into a browser to see the image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### Kompilowanie kodu

Po utworzeniu rozwiązania należy wykonać następujące polecenie, aby uruchomić aplikację:

1. Naciśnij klawisz **F5** lub przejdź do lokalizacji pliku wykonywalnego (ImageListener\bin\Debug\ImageListener.exe), aby uruchomić usługę. Nie zatrzymuj aplikacji, ponieważ będzie ona wymagana do wykonania kolejnego kroku.

2. Skopiuj i wklej adres z wiersza polecenia do przeglądarki, aby wyświetlić obraz.

3. Po zakończeniu naciśnij klawisz **Enter** w oknie wiersza polecenia, aby zamknąć aplikację.

## Następne kroki

Teraz, po utworzeniu aplikacji korzystającej z usługi przekaźnika usługi Service Bus, zobacz następujące artykuły, aby dowiedzieć się więcej o komunikatach obsługiwanych przez przekaźnik:

- [Omówienie architektury usługi Azure Service Bus](service-bus-fundamentals-hybrid-solutions.md#relays)

- [Jak używać usługi Service Bus Relay](service-bus-dotnet-how-to-use-relay.md)

[klasycznego portalu Azure]: http://manage.windowsazure.com


<!--HONumber=Jun16_HO2-->


