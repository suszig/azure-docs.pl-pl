<properties
    pageTitle="Jak używać przekaźnika usługi Service Bus z platformą .NET | Microsoft Azure"
    description="Dowiedz się, jak używać usługi Azure Service Bus w celu połączenia dwóch aplikacji udostępnianych w różnych lokalizacjach."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="05/06/2016"
    ms.author="sethm"/>


# Jak używać usługi Azure Service Bus

W tym artykule opisano sposób używania przekaźnika usługi Service Bus. Przykłady zostały napisane w języku C# i używają interfejsu API Windows Communication Foundation (WCF) z rozszerzeniami zawartymi w zestawie usługi Service Bus. Aby uzyskać więcej informacji na temat przekaźnika usługi Service Bus, zobacz temat [Omówienie komunikatów obsługiwanych przez przekaźnik usługi Service Bus](service-bus-relay-overview.md).

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## Czym jest przekaźnik usługi Service Bus?

[*Przekaźnik* usługi Service Bus](service-bus-relay-overview.md) umożliwia budowanie aplikacji hybrydowych, które działają w centrum danych platformy Azure oraz w lokalnym środowisku korporacyjnym. Przekaźnik usługi Service Bus ułatwia to, ponieważ umożliwia bezpieczne ujawnianie w chmurze publicznej usług Windows Communication Foundation (WCF) znajdujących się w korporacyjnym środowisku sieciowym bez konieczności otwierania połączenia przez zaporę i bez wprowadzania niepożądanych zmian do infrastruktury sieci korporacyjnej.

![Pojęcia dotyczące przekaźnika](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Przekaźnik usługi Service Bus umożliwia udostępnianie usług WCF w istniejącym środowisku korporacyjnym. Następnie nasłuchiwanie przychodzących sesji i żądań dotyczących tych usług WCF można delegować do usługi Service Bus działającej na platformie Azure. Dzięki temu można ujawniać te usługi w kodzie aplikacji działającej na platformie Azure, pracownikom mobilnym albo środowiskom partnerów w ekstranecie. Usługa Service Bus umożliwia bezpieczne i szczegółowe kontrolowanie osób uzyskujących dostęp do tych usług. Jest to nie tylko wydajny i bezpieczny sposób ujawniania funkcji oraz danych aplikacji z istniejących rozwiązań korporacyjnych, ale również metoda korzystania z nich w chmurze.

W tym artykule przedstawiono sposób korzystania z przekaźnika usługi Service Bus w celu utworzenia usługi sieci Web WCF ujawnianej za pomocą powiązania kanału TCP, która implementuje bezpieczną konwersację między dwiema stronami.

## Tworzenie przestrzeni nazw usługi

Aby rozpocząć korzystanie z przekaźnika usługi Service Bus na platformie Azure, najpierw należy utworzyć przestrzeń nazw. Przestrzeń nazw zapewnia kontener określania zakresu na potrzeby adresowania zasobów usługi Service Bus w aplikacji.

Aby utworzyć przestrzeń nazw usługi:

1.  Zaloguj się do [klasycznego portalu Azure][].

2.  W lewym okienku nawigacji portalu kliknij pozycję **Service Bus**.

3.  W dolnym okienku portalu kliknij pozycję **Utwórz**.

    ![](./media/service-bus-dotnet-how-to-use-relay/sb-queues-13.png)

4.  W oknie dialogowym **Dodawanie nowej przestrzeni nazw** wprowadź nazwę przestrzeni nazw.
    System od razu sprawdza, czy nazwa jest dostępna.

    ![](./media/service-bus-dotnet-how-to-use-relay/sb-queues-04.png)

5.  Po upewnieniu się, że nazwa przestrzeni nazw jest dostępna, wybierz kraj lub region, w którym przestrzeń nazw ma być hostowana (upewnij się, że używasz tego samego kraju/regionu, w którym są wdrażane zasoby obliczeniowe).

    > [AZURE.IMPORTANT] Wybierz *ten sam region*, którego zamierzasz użyć do wdrożenia aplikacji. Zapewni to najlepszą wydajność.

6.  W pozostałych polach okna dialogowego pozostaw wartości domyślne (**Obsługa komunikatów** i warstwa **Standardowa**), a następnie kliknij znacznik wyboru. W systemie zostanie utworzona i włączona przestrzeń nazw. Proces aprowizacji zasobów dla konta w systemie może potrwać kilka minut.

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-27.png)

    Utworzona przestrzeń nazw zostanie wyświetlona w portalu, a następnie po chwili aktywowana. Przed wykonaniem dalszych czynności zaczekaj, aż jej stan zmieni się na **Aktywna**.

## Uzyskiwanie domyślnych poświadczeń zarządzania dla przestrzeni nazw

Aby wykonywać operacje zarządzania, takie jak tworzenie połączenia przekazywania, w nowej przestrzeni nazw, należy skonfigurować dla przestrzeni nazw regułę autoryzacji sygnatury dostępu współdzielonego. Aby uzyskać więcej informacji na temat sygnatury dostępu współdzielonego, zobacz temat [Uwierzytelnianie za pomocą sygnatury dostępu współdzielonego przy użyciu usługi Service Bus][].

1.  W lewym okienku nawigacji kliknij węzeł **Service Bus**, aby wyświetlić listę dostępnych przestrzeni nazw.
    ![](./media/service-bus-dotnet-how-to-use-relay/sb-queues-13.png)

2.  Na wyświetlonej liście kliknij dwukrotnie nazwę nowo utworzonej przestrzeni nazw.
    ![](./media/service-bus-dotnet-how-to-use-relay/sb-queues-09.png)

3.  U góry strony kliknij kartę **Konfiguracja**.

4.  Po udostępnieniu przestrzeni nazw usługi Service Bus domyślnie zostanie utworzona reguła **SharedAccessAuthorizationRule**, w której parametr **KeyName** jest ustawiony na wartość **RootManageSharedAccessKey**. Na tej stronie wyświetlony zostanie klucz, a także klucze podstawowe i pomocnicze dla reguły domyślnej.

## Uzyskiwanie pakietu NuGet usługi Service Bus

Dodanie [pakietu NuGet usługi Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus) jest najprostszym sposobem odwołania do interfejsu API usługi Service Bus i skonfigurowania aplikacji ze wszystkimi zależnościami usługi Service Bus. Aby zainstalować pakiet NuGet w aplikacji, wykonaj następujące kroki:

1.  W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy pozycję **Odwołania**, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**.
2.  Wyszukaj ciąg „Service Bus” i wybierz pozycję **Microsoft Azure Service Bus**. Kliknij przycisk **Zainstaluj**, aby ukończyć instalację, a następnie zamknij poniższe okno dialogowe.

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)


## Używanie usługi Service Bus, aby ujawnić usługę sieci Web SOAP i korzystać z niej z protokołem TCP

W celu ujawnienia istniejącej usługi sieci Web SOAP WCF na potrzeby zewnętrznego korzystania z tej usługi należy wprowadzić zmiany do jej powiązań i adresów. W tym celu konieczne może być wprowadzenie zmian do pliku konfiguracyjnego albo do kodu — w zależności od sposobu konfiguracji usług WCF. Program WCF umożliwia posiadanie wielu punktów końcowych sieci w tej samej usłudze, dzięki czemu można zachować istniejące wewnętrzne punkty końcowe, jednocześnie dodając punkty zewnętrzne usługi Service Bus na potrzeby dostępu zewnętrznego.

W ramach tego zadania utworzysz prostą usługę WCF i dodasz do niej odbiornik usługi Service Bus. W tym ćwiczeniu przyjęto, że masz pewną znajomość programu Visual Studio i w związku z tym nie omówiono wszystkich szczegółów tworzenia projektu. Zamiast tego ćwiczenie koncentruje się na kodzie.

Przed przejściem do kroków poniżej wykonaj następującą procedurę, aby skonfigurować swoje środowisko:

1.  W programie Visual Studio utwórz aplikację konsolową, która zawiera dwa projekty — „Client” i „Service” — w ramach rozwiązania.
2.  Do obu projektów dodaj pakiet NuGet usługi Microsoft Azure Service Bus. To spowoduje dodanie do Twoich projektów wszystkich niezbędnych odwołań do zestawu.

### Jak utworzyć usługę

Najpierw należy utworzyć samą usługę. Każda usługa WCF składa się z co najmniej trzech oddzielnych części:

-   Definicja kontraktu, który opisuje, jakie komunikaty są wymieniane i jakie operacje będą wywoływane.
-   Implementacja wspomnianego kontraktu.
-   Host, który hostuje usługę WCF i ujawnia szereg punktów końcowych.

Przykłady kodu w tej sekcji dotyczą każdego z tych składników.

Kontrakt definiuje jedną operację `AddNumbers`, która dodaje dwie liczby i zwraca wynik. Interfejs `IProblemSolverChannel` ułatwia klientowi zarządzanie czasem życia serwera proxy. Utworzenie takiego interfejsu jest traktowane jako najlepsze rozwiązanie. Dobrze jest umieścić tę definicję kontraktu w osobnym pliku, do którego można się odwoływać zarówno z projektu „Client”, jak i z projektu „Service”, ale kod można również skopiować do obu tych projektów.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Gdy kontrakt jest na miejscu, wdrożenie jest proste.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### Programowe konfigurowanie hosta usługi

Gdy kontrakt i implementacja są na miejscu, można hostować usługę. Hosting następuje wewnątrz obiektu [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx), który zajmuje się zarządzaniem wystąpieniami usługi i hostuje punkty końcowe nasłuchujące komunikatów. Poniższy kod służy do konfigurowania usługi ze standardowym lokalnym punktem końcowym i punktem końcowym usługi Service Bus, dzięki czemu umożliwia porównanie wewnętrznych i zewnętrznych punktów końcowych. Zastąp ciąg *namespace* nazwą odpowiedniej przestrzeni nazw, a ciąg *yourKey* kluczem sygnatury dostępu współdzielonego uzyskanym w poprzednim kroku konfiguracji.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "yourKey")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

W tym przykładzie utworzysz dwa punkty końcowe należące do tej samej implementacji kontraktu. Jeden jest lokalny, a drugi jest rzutowany za pośrednictwem usługi Service Bus. Podstawową różnicę między nimi stanowią powiązania — [NetTcpBinding](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) dla lokalnego punktu końcowego, a [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) dla punktu końcowego Service Bus i adresów. Lokalny punkt końcowy ma adres sieci lokalnej z unikatowym portem. Punkt końcowy usługi Service Bus ma adres złożony z ciągu `sb`, nazwy przestrzeni nazw i ścieżki „solver”. Skutkuje to identyfikatorem URI `sb://[serviceNamespace].servicebus.windows.net/solver`, który identyfikuje punkt końcowy usługi jako punkt końcowy protokołu TCP usługi Service Bus z w pełni kwalifikowaną nazwą DNS. Jeśli umieścisz kod, zastępując symbole zastępcze zgodnie z powyższymi wskazówkami, w funkcji `Main` aplikacji **Service**, wówczas uzyskasz usługę funkcjonalną. Jeśli chcesz, aby usługa nasłuchiwała wyłącznie usługi Service Bus, usuń deklarację punktu końcowego.

### Konfigurowanie hosta usługi w pliku App.config

Hosta można również skonfigurować przy użyciu pliku App.config. Kod hostowania usługi pojawia się w następnym przykładzie.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Definicje punktu końcowego są w tym przypadku przeniesione do pliku App.config. Należy zauważyć, że pakiet NuGet już dodał szereg definicji do pliku App.config i są to wymagane rozszerzenia konfiguracji dla usługi Service Bus. Poniższy przykład, który jest dokładnym odpowiednikiem poprzedniego kodu, powinien pojawić się bezpośrednio pod elementem **system.serviceModel**. Ten przykład kodu zakłada, że przestrzeń nazw języka C# w Twoim projekcie ma nazwę **Service**.
Zastąp symbole zastępcze nazwą obszaru nazw usługi Service Bus i kluczem sygnatury dostępu współdzielonego.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Po wprowadzeniu tych zmian usługa będzie uruchamiana tak jak poprzednio, ale z dwoma aktywnymi punktami końcowymi: jednym lokalnym i jednym nasłuchującym w chmurze.

### Tworzenie klienta

#### Programowe konfigurowanie klienta

W celu korzystania z usługi możesz utworzyć klienta WCF, używając obiektu [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx). Usługa Service Bus używa opartego na tokenie modelu zabezpieczeń, który jest implementowany z użyciem sygnatury dostępu współdzielonego. Klasa [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) reprezentuje dostawcę tokenu zabezpieczającego z wbudowanymi metodami Factory, które zwracają dobrze znanych dostawców tokenu. W poniższym przykładzie użyto metody [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) w celu obsłużenia uzyskiwania odpowiedniego tokenu sygnatury dostępu współdzielonego. Nazwa i klucz są uzyskiwane z portalu zgodnie z opisem w poprzedniej sekcji.

Najpierw należy utworzyć odwołanie do kodu kontraktu `IProblemSolver` albo skopiować ten kod z usługi do projektu Client.

Następnie należy zastąpić kod w metodzie `Main` klienta, ponownie zastępując tekst zastępczy nazwą przestrzeni nazw usługi Service Bus i kluczem sygnatury dostępu współdzielonego.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","yourKey") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Po wykonaniu tych czynności można skompilować klienta i usługę, uruchomić je (najpierw usługę), a następnie klient wywoła usługę i wyświetli **9**. Klienta i serwer można uruchamiać na różnych maszynach, nawet w różnych sieciach, a komunikacja między nimi będzie nadal działać.  Kod klienta można również uruchomić w chmurze lub lokalnie.

#### Konfigurowanie klienta w pliku App.config

Poniższy kod przedstawia sposób konfigurowania klienta przy użyciu pliku App.config.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Definicje punktu końcowego są w tym przypadku przeniesione do pliku App.config. Poniższy przykład, który jest taki sam, jak kod przedstawiony poprzednio, powinien pojawić się bezpośrednio pod elementem **system.serviceModel**. W tym przypadku, tak jak poprzednio, należy zastąpić symbole zastępcze nazwą przestrzeni nazw usługi Service Bus i kluczem sygnatury dostępu współdzielonego.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## Następne kroki

Teraz, kiedy znasz już podstawy usługi przekaźnika usługi Service Bus, skorzystaj z poniższych linków, aby dowiedzieć się więcej.

- [Omówienie komunikatów obsługiwanych przez przekaźnik usługi Service Bus](service-bus-relay-overview.md)
- [Omówienie architektury usługi Azure Service Bus](service-bus-fundamentals-hybrid-solutions.md)
- Pobierz przykłady usługi Service Bus ze strony [przykładów dla platformy Azure][] lub zobacz [przegląd przykładów usługi Service Bus][].

  [klasycznego portalu Azure]: http://manage.windowsazure.com
  [Uwierzytelnianie za pomocą sygnatury dostępu współdzielonego przy użyciu usługi Service Bus]: service-bus-shared-access-signature-authentication.md
  [przykładów dla platformy Azure]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [przegląd przykładów usługi Service Bus]: service-bus-samples.md


<!--HONumber=Jun16_HO2-->


