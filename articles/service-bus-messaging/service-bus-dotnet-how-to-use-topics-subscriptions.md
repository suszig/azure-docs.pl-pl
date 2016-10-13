<properties
    pageTitle="Używanie tematów usługi Service Bus z platformą .NET | Microsoft Azure"
    description="Dowiedz się, jak używać tematów i subskrypcji usługi Service Bus z platformą .NET na platformie Azure. Przykłady kodu są napisane dla aplikacji .NET."
    services="service-bus-messaging"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus-messaging"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>


# Jak używać tematów i subskrypcji usługi Service Bus

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

W tym artykule opisano sposób używania tematów i subskrypcji usługi Service Bus. Przykłady są napisane w języku C# i używają interfejsów API platformy .NET. Omówione scenariusze obejmują tworzenie tematów i subskrypcji, tworzenie filtrów subskrypcji, wysyłanie komunikatów do tematów, otrzymywanie komunikatów z subskrypcji, a także usuwanie tematów i subskrypcji. Aby uzyskać więcej informacji o tematach i subskrypcjach, zobacz sekcję [Następne kroki](#next-steps).

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## Konfigurowanie aplikacji w taki sposób, aby używała usługi Service Bus

Gdy tworzysz aplikację, która używa usługi Service Bus, musisz dodać odwołanie do zestawu usługi Service Bus i dołączyć odpowiednie przestrzenie nazw. Najłatwiejszym sposobem na to jest pobranie odpowiedniego pakietu [NuGet](https://www.nuget.org).

## Uzyskiwanie pakietu NuGet usługi Service Bus

Dodanie [pakietu NuGet usługi Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus) jest najprostszym sposobem uzyskania interfejsu API usługi Service Bus i skonfigurowania aplikacji ze wszystkimi zależnościami usługi Service Bus. Aby zainstalować pakiet NuGet usługi Service Bus w projekcie, wykonaj następujące kroki:

1.  W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy pozycję **Odwołania**, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**.
2.  Wyszukaj ciąg „Service Bus” i wybierz pozycję **Microsoft Azure Service Bus**. Kliknij przycisk **Zainstaluj**, aby ukończyć instalację, a następnie zamknij poniższe okno dialogowe:

    ![][7]

Teraz można przystąpić do pisania kodu dla usługi Service Bus.

## Tworzenie parametrów połączenia usługi Service Bus

Usługa Service Bus używa parametrów połączenia do przechowywania punktów końcowych i poświadczeń. Parametry połączenia można umieścić w pliku konfiguracji zamiast kodować je na stałe:

- W przypadku korzystania z usług platformy Azure zalecane jest przechowywanie parametrów połączenia przy użyciu systemu konfiguracji usługi platformy Azure (pliki csdef i cscfg).
- W przypadku korzystania z usług Azure Websites lub Azure Virtual Machines zalecane jest przechowywanie parametrów połączenia przy użyciu systemu konfiguracji platformy .NET (na przykład plik Web.config).

W obu przypadkach można pobrać parametry połączenia za pomocą metody `CloudConfigurationManager.GetSetting`, jak pokazano w dalszej części tego artykułu.

### Konfigurowanie parametrów połączenia

Mechanizm konfiguracji usługi umożliwia dynamiczną zmianę ustawień konfiguracji z poziomu witryny [Azure Portal][] bez ponownego wdrażania aplikacji. Na przykład dodaj etykietę `Setting` do pliku definicji usługi (**csdef**), jak pokazano w następnym przykładzie.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

Następnie możesz określić wartości w pliku konfiguracji usługi (.cscfg).

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Użyj nazwy klucza i wartości klucza sygnatury dostępu współdzielonego pobranych z portalu zgodnie z opisem w poprzedniej sekcji.

### Konfigurowanie parametrów połączenia w przypadku korzystania z usługi Azure Websites lub Azure Virtual Machines

W przypadku korzystania z witryn sieci Web lub usługi Virtual Machines zaleca się użycie systemu konfiguracji platformy .NET (na przykład Web.config). Do przechowywania parametrów połączenia służy element `<appSettings>`.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Użyj nazwy i wartości klucza sygnatury dostępu współdzielonego pobranych z witryny [Azure Portal][] zgodnie z opisem w poprzedniej sekcji.

## Tworzenie tematu

Operacje zarządzania tematami i subskrypcjami usługi Service Bus można wykonywać przy użyciu klasy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx). Ta klasa dostarcza metody do tworzenia, wyliczania i usuwania tematów.

Poniższy przykład tworzy obiekt `NamespaceManager` przy użyciu klasy `CloudConfigurationManager` platformy Azure z użyciem parametrów połączenia składających się z adresu podstawowego przestrzeni nazw usługi Service Bus oraz odpowiednich poświadczeń sygnatury dostępu współdzielonego z uprawnieniami do zarządzania nim. Te parametry połączenia mają następującą postać:

```
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Skorzystaj z następującego przykładu, przyjmując ustawienia konfiguracji określone w poprzedniej sekcji.

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Istnieją przeciążenia metody [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) umożliwiające ustawienie właściwości tematu, na przykład w celu ustawienia wartości czasu wygaśnięcia do zastosowania dla komunikatów wysyłanych do tematu. Te ustawienia są stosowane przy użyciu klasy [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx). Poniższy przykład pokazuje, jak utworzyć temat o nazwie **TestTopic** o maksymalnym rozmiarze 5 GB i domyślnym czasie wygaśnięcia komunikatu wynoszącym 1 minutę.

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [AZURE.NOTE] Można użyć metody [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) na obiektach [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx), aby sprawdzić, czy temat o określonej nazwie już istnieje w przestrzeni nazw.

## Tworzenie subskrypcji

Można również tworzyć subskrypcje tematu przy użyciu klasy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx). Subskrypcje są nazywane i mogą zawierać opcjonalny filtr, który ogranicza zestaw komunikatów przesyłany do wirtualnej kolejki subskrypcji.

> [AZURE.IMPORTANT] Aby móc odbierać komunikaty w ramach subskrypcji, musisz utworzyć subskrypcję przed wysłaniem komunikatów do tematu. Jeśli z tematem nie są powiązane żadne subskrypcje, temat odrzuci te komunikaty.

### Tworzenie subskrypcji z filtrem domyślnym (MatchAll)

Filtr **MatchAll** jest filtrem domyślnym, który jest używany, gdy podczas tworzenia nowej subskrypcji nie został określony żaden filtr. Kiedy używasz filtru **MatchAll**, wszystkie komunikaty opublikowane do tematu są umieszczane w wirtualnej kolejce subskrypcji. Poniższy przykład tworzy subskrypcję o nazwie „AllMessages” i używa domyślnego filtru **MatchAll**.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### Tworzenie subskrypcji z filtrami

Można również ustawiać filtry pozwalające określić, które komunikaty wysyłane do tematu powinny znajdować się w subskrypcji określonego tematu.

Najbardziej elastycznym typem filtru obsługiwanym przez subskrypcje jest klasa [SqlFilter][], która implementuje podzbiór standardu SQL92. Filtry SQL działają na właściwościach komunikatów, które są publikowane do tematu. Aby uzyskać więcej informacji na temat wyrażeń, które mogą być używane z filtrem SQL, zobacz składnię [SqlFilter.SqlExpression][].

Poniższy przykład tworzy subskrypcję o nazwie **HighMessages** z obiektem [SqlFilter][], który wybiera tylko komunikaty o właściwości niestandardowej **MessageNumber** wyższej niż 3.

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Podobnie poniższy przykład tworzy subskrypcję o nazwie **LowMessages** z obiektem [SqlFilter][], który wybiera tylko komunikaty o właściwości **MessageNumber** równej 3 lub mniejszej.

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

W takiej sytuacji komunikat wysłany do tematu `TestTopic` zawsze jest dostarczany do odbiorców mających subskrypcję **AllMessages** i selektywnie dostarczany do odbiorców mających subskrypcje **HighMessages** i **LowMessages** (w zależności od zawartości komunikatu).

## Wysyłanie komunikatów do tematu

Aby wysłać komunikat do tematu usługi Service Bus, aplikacja tworzy obiekt [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) przy użyciu parametrów połączenia.

Poniższy kod przedstawia sposób tworzenia obiektu [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) dla tematu **TestTopic** utworzonego wcześniej przy użyciu wywołania interfejsu API [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx).

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Komunikaty wysyłane do tematów usługi Service Bus są wystąpieniami klasy [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx). Obiekty [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) mają zestaw właściwości standardowych (takich jak [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) i [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), słownik, który służy do przechowywania określonych właściwości niestandardowych aplikacji, oraz treść dowolnych danych aplikacji. Aplikacja może ustawić treść komunikatu przez przekazanie dowolnego obiektu podlegającego serializacji do konstruktora obiektu [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx), a odpowiedni element **DataContractSerializer** jest następnie używany do serializacji obiektu. Alternatywnie można udostępnić strumień **System.IO.Stream**.

W poniższym przykładzie pokazano sposób wysyłania pięciu testowych komunikatów do obiektu **TestTopic** [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) uzyskanego w poprzednim przykładzie kodu. Należy pamiętać, że wartość właściwości [MessageNumber](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) każdego komunikatu różni się w zależności od iteracji pętli (określa subskrypcje, które go otrzymają).

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Tematy usługi Service Bus obsługują maksymalny rozmiar komunikatu 256 KB w [warstwie Standardowa](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera standardowe i niestandardowe właściwości aplikacji, może mieć maksymalny rozmiar 64 KB. Nie ma żadnego limitu liczby komunikatów w temacie, ale jest ograniczenie całkowitego rozmiaru komunikatów przechowywanych przez temat. Rozmiar tematu jest definiowany w czasie tworzenia, z górnym limitem 5 GB. Jeśli partycjonowanie jest włączone, górny limit jest większy. Aby uzyskać więcej informacji, zobacz [Partycjonowane jednostki do obsługi komunikatów](service-bus-partitioning.md).

## Jak odbierać komunikaty z subskrypcji

Zalecanym sposobem odbierania komunikatów z subskrypcji jest użycie obiektu [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx). Obiekty [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) mogą pracować w dwóch różnych trybach: [*ReceiveAndDelete* i *PeekLock*](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

W przypadku używania trybu **ReceiveAndDelete** odbieranie jest operacją pojedynczego zrzutu. Oznacza to, że kiedy usługa Service Bus odbiera żądanie odczytu komunikatu w subskrypcji, oznacza komunikat jako wykorzystywany i zwraca go do aplikacji. Tryb **ReceiveAndDelete** jest najprostszym modelem i działa najlepiej w scenariuszach, w których aplikacja może tolerować nieprzetworzenie komunikatu w razie awarii. Aby to zrozumieć, rozważmy scenariusz, w którym konsument wystawia żądanie odbioru, a następnie ulega awarii przed jego przetworzeniem. Usługa Service Bus oznaczyła komunikat jako wykorzystany, dlatego gdy aplikacja zostanie ponownie uruchomiona i ponownie rozpocznie korzystanie z komunikatów, pominie utracony komunikat, który został wykorzystany przed awarią.

W trybie **PeekLock** (tryb domyślny) odbieranie staje się operacją dwuetapową, co umożliwia obsługę aplikacji, które nie tolerują brakujących komunikatów. Gdy usługa Service Bus odbiera żądanie, znajduje następny komunikat do wykorzystania, blokuje go w celu uniemożliwienia innym klientom odebrania go i zwraca go do aplikacji. Kiedy aplikacja zakończy przetwarzanie komunikatu (lub niezawodnie zapisze go w celu przyszłego przetwarzania), wykonuje drugi etap procesu odbierania przez wywołanie metody [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) dla odebranego komunikatu. Gdy wywołanie metody **Complete** stanie się widoczne dla usługi Service Bus, usługa oznaczy komunikat jako wykorzystany i usunie go z subskrypcji.

W poniższym przykładzie pokazano, jak komunikaty mogą być odbierane i przetwarzane przy użyciu domyślnego trybu **PeekLock**. Aby określić inną wartość właściwości [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx), można użyć innego przeciążenia metody [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx). W tym przykładzie użyto wywołania zwrotnego [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) do przetwarzania komunikatów przychodzących do subskrypcji **HighMessages**.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

Ten przykład konfiguruje wywołanie zwrotne metody [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) przy użyciu obiektu [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx). Właściwość [AutoComplete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) jest ustawiona na **false** w celu umożliwienia ręcznej kontroli nad tym, kiedy wywołać metodę [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) na odebranym komunikacie. Właściwość [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) jest ustawiona na 1 minutę, co powoduje, że klient oczekuje przez jedną minutę na komunikat, zanim upłynie limit czasu funkcji automatycznego odnawiania i klient wyśle nowe wywołanie w celu sprawdzenia komunikatów. Ta wartość właściwości zmniejsza liczbę przypadków, gdy klient wysyła wywołania, które mogą być obciążane i nie pobierają komunikatów.

## Sposób obsługi awarii aplikacji i komunikatów niemożliwych do odczytania

Usługa Service Bus zapewnia funkcję ułatwiającą bezpieczne odzyskiwanie w razie błędów w aplikacji lub trudności z przetwarzaniem komunikatu. Jeśli aplikacja odbierająca z jakiegoś powodu nie można przetworzyć komunikatu, wówczas może wywołać metodę [Abandon](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) dla odebranego komunikatu (zamiast metody [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). Powoduje to odblokowanie komunikatu w subskrypcji przez usługę Service Bus i ponowne udostępnienie go do odebrania przez tę samą lub inną odbierającą aplikację.

Istnieje również limit czasu skojarzony z komunikatem zablokowanym w subskrypcji i jeśli aplikacja nie może przetworzyć komunikatu przed upływem limitu czasu blokady (na przykład jeśli wystąpiła awaria aplikacji), usługa Service Bus automatycznie odblokowuje komunikat i udostępnia go do ponownego odbioru.

W przypadku gdy aplikacja przestaje działać po przetworzeniu komunikatu, ale przed wysłaniem żądania [Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx), komunikat zostanie dostarczony do aplikacji po jej ponownym uruchomieniu. Jest to często określane jako *Przetwarzanie co najmniej raz*, co oznacza, że każdy komunikat jest przetwarzany co najmniej raz, ale w pewnych sytuacjach ten sam komunikat może być dostarczony ponownie. Jeśli scenariusz nie toleruje dwukrotnego przetwarzania, deweloperzy aplikacji powinni dodać dodatkową logikę do swojej aplikacji w celu obsługi dwukrotnego dostarczania komunikatów. Jest to często osiągane przy użyciu właściwości [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) komunikatu, która pozostaje stała między kolejnymi próbami dostarczenia.

## Usuwanie tematów i subskrypcji

W poniższym przykładzie pokazano sposób usuwania tematu **TestTopic** z przestrzeni nazw usługi **HowToSample**.

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Usunięcie tematu powoduje również usunięcie subskrypcji, które są zarejestrowane z tematem. Subskrypcje mogą być również usuwane niezależnie. Poniższy kod ilustruje sposób usuwania subskrypcji o nazwie **HighMessages** z tematu **TestTopic**.

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## Następne kroki

Teraz, kiedy znasz już podstawy tematów i subskrypcji usługi Service Bus, skorzystaj z poniższych linków, aby dowiedzieć się więcej.

-   [Kolejki, tematy i subskrypcje][].
-   [Przykładowe filtry tematu][]
-   Dokumentacja interfejsów API dla elementu [SqlFilter][].
-   Utwórz działającą aplikację, która wysyła i odbiera komunikaty z kolejki usługi Service Bus: [samouczka Komunikaty obsługiwane przez brokera w usłudze Service Bus dla platformy .NET][].
-   Przykłady usługi Service Bus: pobierz ze strony [przykładów dla platformy Azure][] lub zobacz [przegląd przykładów](../service-bus/service-bus-samples.md).

  [Azure Portal]: https://portal.azure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [Kolejki, tematy i subskrypcje]: service-bus-queues-topics-subscriptions.md
  [Przykładowe filtry tematu]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [samouczka Komunikaty obsługiwane przez brokera w usłudze Service Bus dla platformy .NET]: service-bus-brokered-tutorial-dotnet.md
  [przykładów dla platformy Azure]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2



<!--HONumber=Sep16_HO5-->


