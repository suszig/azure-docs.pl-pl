<properties
    pageTitle="Jak używać kolejek usługi Service Bus (.NET) | Microsoft Azure"
    description="Dowiedz się, jak używać kolejek usługi Service Bus na platformie Azure. Przykłady kodu napisane w języku C# z użyciem interfejsu API programu .NET."
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
    ms.date="05/09/2016"
    ms.author="sethm"/>

# Jak używać kolejek usługi Service Bus

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

W tym artykule opisano sposób używania kolejek usługi Service Bus. Przykłady są napisane w języku C# i używają interfejsu API programu .NET. Scenariusze obejmują tworzenie kolejek oraz wysyłanie i odbieranie komunikatów. Aby uzyskać więcej informacji na temat kolejek, zobacz sekcję [Następne kroki](#next-steps).

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## Dodawanie pakietu NuGet usługi Service Bus

Dodanie [pakietu NuGet usługi Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus) jest najprostszym sposobem odwołania do interfejsu API usługi Service Bus i skonfigurowania aplikacji ze wszystkimi zależnościami usługi Service Bus. Aby zainstalować pakiet NuGet w aplikacji, wykonaj następujące kroki:

1.  W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy pozycję **Odwołania**, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**.
2.  Wyszukaj ciąg „Service Bus” i wybierz pozycję **Microsoft Azure Service Bus**. Kliknij przycisk **Zainstaluj**, aby ukończyć instalację, a następnie zamknij to okno dialogowe.

    ![][7]

Teraz można przystąpić do pisania kodu dla usługi Service Bus.

## Konfigurowanie parametrów połączenia usługi Service Bus

Usługa Service Bus używa parametrów połączenia do przechowywania punktów końcowych i poświadczeń. Parametry połączenia można umieścić w pliku konfiguracji zamiast kodować je na stałe:

- W przypadku korzystania z usług Azure Cloud Services zalecane jest przechowywanie parametrów połączenia przy użyciu systemu konfiguracji usługi platformy Azure (pliki csdef i cscfg).
- W przypadku korzystania z witryn sieci Web platformy Azure lub usługi Azure Virtual Machines zalecane jest przechowywanie parametrów połączenia przy użyciu systemu konfiguracji platformy .NET (na przykład plik Web.config).

W obu przypadkach można pobrać parametry połączenia za pomocą metody [CloudConfigurationManager.GetSetting][GetSetting], jak pokazano w dalszej części tego artykułu.

### Konfigurowanie parametrów połączenia

Mechanizm konfiguracji usługi umożliwia dynamiczną zmianę ustawień konfiguracji z poziomu [klasycznego portalu Azure][] bez ponownego wdrażania aplikacji. Na przykład dodaj etykietę `Setting` do pliku definicji usługi (csdef), jak pokazano w następnym przykładzie.

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
Następnie możesz określić wartości w pliku konfiguracji usługi (cscfg), jak pokazano w następnym przykładzie.

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

Użyj nazwy klucza i wartości klucza sygnatury dostępu współdzielonego (SAS) pobranych z klasycznego portalu Azure zgodnie z opisem w poprzedniej sekcji.

### Konfigurowanie parametrów połączenia w przypadku korzystania z witryn sieci Web lub usługi Azure Virtual Machines

W przypadku korzystania z witryn sieci Web lub usługi Virtual Machines zaleca się użycie systemu konfiguracji platformy .NET (na przykład **Web.config**). Do przechowywania parametrów połączenia służy element `<appSettings>`.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Użyj nazwy i wartości klucza sygnatury dostępu współdzielonego pobranych z klasycznego portalu Azure, zgodnie z opisem w poprzedniej sekcji.

## Tworzenie kolejki

Operacje zarządzania kolejkami usługi Service Bus można wykonywać przy użyciu klasy [NamespaceManager][]. Ta klasa dostarcza metody do tworzenia, wyliczania i usuwania kolejek.

Ten przykład tworzy obiekt [NamespaceManager][] przy użyciu klasy [CloudConfigurationManager][] platformy Azure za pomocą parametrów połączenia składających się z adresu podstawowego przestrzeni nazw usługi Service Bus oraz odpowiednich poświadczeń sygnatury dostępu współdzielonego z uprawnieniami do zarządzania nim. Te parametry połączenia mają postać pokazaną w poniższym przykładzie.

````
Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedSecretValue=yourKey
````

Skorzystaj z następującego przykładu, przyjmując ustawienia konfiguracji określone w poprzedniej sekcji.

```
// Create the queue if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.QueueExists("TestQueue"))
{
    namespaceManager.CreateQueue("TestQueue");
}
```

Istnieją przeciążenia metody [CreateQueue](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createqueue.aspx) umożliwiające dostrojenie właściwości kolejki (na przykład w celu ustawienia wartości czasu wygaśnięcia do zastosowania dla komunikatów wysyłanych do kolejki). Te ustawienia są stosowane przy użyciu klasy [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx). Poniższy przykład pokazuje, jak utworzyć kolejkę o nazwie `TestQueue` o maksymalnym rozmiarze 5 GB i domyślnym czasie wygaśnięcia komunikatu wynoszącym 1 minutę.

```
// Configure queue settings.
QueueDescription qd = new QueueDescription("TestQueue");
qd.MaxSizeInMegabytes = 5120;
qd.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new queue with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.QueueExists("TestQueue"))
{
    namespaceManager.CreateQueue(qd);
}
```

> [AZURE.NOTE] Można użyć metody [QueueExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.queueexists.aspx) na obiektach [NamespaceManager][], aby sprawdzić, czy kolejka o określonej nazwie już istnieje w przestrzeni nazw usługi.

## Wysyłanie komunikatów do kolejki

Aby wysłać komunikat do kolejki usługi Service Bus, aplikacja tworzy obiekt [QueueClient][] przy użyciu parametrów połączenia.

Poniższy kod przedstawia sposób tworzenia obiektu [QueueClient][] dla kolejki `TestQueue` właśnie utworzonej przy użyciu wywołania interfejsu API [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.createfromconnectionstring.aspx).

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

Client.Send(new BrokeredMessage());
```

Komunikaty wysyłane do i odbierane z kolejek usługi Service Bus są wystąpieniami klasy [BrokeredMessage][]. Obiekty [BrokeredMessage][] mają zestaw właściwości standardowych (takich jak [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) i [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), słownik, który służy do przechowywania określonych właściwości niestandardowych aplikacji, oraz treść dowolnych danych aplikacji. Aplikacja możne ustawić treść komunikatu przez przekazanie dowolnego obiektu podlegającego serializacji do konstruktora obiektu [BrokeredMessage][], a odpowiedni element **DataContractSerializer** jest następnie używany do serializacji obiektu. Alternatywnie można udostępnić obiekt **System.IO.Stream**.

W poniższym przykładzie pokazano sposób wysyłania pięciu testowych komunikatów do obiektu `TestQueue` [QueueClient][] uzyskanego w poprzednim przykładzie kodu.

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set some addtional custom app-specific properties.
  message.Properties["TestProperty"] = "TestValue";
  message.Properties["Message number"] = i;

  // Send message to the queue.
  Client.Send(message);
}
```

Kolejki usługi Service Bus obsługują maksymalny rozmiar komunikatu 256 KB w [warstwie Standardowa](service-bus-premium-messaging.md) i 1 MB w [warstwie Premium](service-bus-premium-messaging.md). Nagłówek, który zawiera standardowe i niestandardowe właściwości aplikacji, może mieć maksymalny rozmiar 64 KB. Nie ma żadnego limitu liczby komunikatów w kolejce, ale jest ograniczenie całkowitego rozmiaru komunikatów przechowywanych przez kolejkę. Ten rozmiar kolejki jest definiowany w czasie tworzenia, z górnym limitem 5 GB. Jeśli partycjonowanie jest włączone, górny limit jest większy. Aby uzyskać więcej informacji, zobacz [Partycjonowane jednostki do obsługi komunikatów](service-bus-partitioning.md).

## Jak odbierać komunikaty z kolejki

Zalecanym sposobem odbierania komunikatów z kolejki jest użycie obiektu [QueueClient][]. Obiekty [QueueClient][] mogą pracować w dwóch różnych trybach: [ReceiveAndDelete i PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

W przypadku używania trybu **ReceiveAndDelete** odbieranie jest operacją pojedynczego zrzutu. Oznacza to, że kiedy usługa Service Bus odbiera żądanie odczytu komunikatu w kolejce, oznacza komunikat jako wykorzystany i zwraca go do aplikacji. Tryb **ReceiveAndDelete** jest najprostszym modelem i działa najlepiej w scenariuszach, w których aplikacja może tolerować nieprzetworzenie komunikatu w razie awarii. Aby to zrozumieć, rozważmy scenariusz, w którym konsument wystawia żądanie odbioru, a następnie ulega awarii przed jego przetworzeniem. Usługa Service Bus oznacza komunikat jako wykorzystany, dlatego gdy aplikacja zostanie ponownie uruchomiona i ponownie rozpocznie korzystanie z komunikatów, pominie utracony komunikat, który został wykorzystany przed awarią.

W trybie **PeekLock** (tryb domyślny) odbieranie staje się operacją dwuetapową, co umożliwia obsługę aplikacji, które nie tolerują brakujących komunikatów. Gdy usługa Service Bus odbiera żądanie, znajduje następny komunikat do wykorzystania, blokuje go w celu uniemożliwienia innym klientom odebrania go i zwraca go do aplikacji. Kiedy aplikacja zakończy przetwarzanie komunikatu (lub niezawodnie zapisze go w celu przyszłego przetwarzania), wykonuje drugi etap procesu odbierania przez wywołanie metody [Complete][] dla odebranego komunikatu. Gdy usługa Service Bus widzi wywołanie metody [Complete][], oznacza komunikat jako wykorzystany i usuwa go z kolejki.

W poniższym przykładzie pokazano, jak komunikaty mogą być odbierane i przetwarzane przy użyciu domyślnego trybu **PeekLock**. Aby określić inną wartość właściwości [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx), można użyć innego przeciążenia metody [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.createfromconnectionstring.aspx). W tym przykładzie użyto wywołania zwrotnego [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.onmessage.aspx) do przetwarzania komunikatów przychodzących do kolejki `TestQueue`.

```
string connectionString =
  CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
QueueClient Client =
  QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

// Callback to handle received messages.
Client.OnMessage((message) =>
{
    try
    {
        // Process message from queue.
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Test Property: " +
        message.Properties["TestProperty"]);

        // Remove message from queue.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in queue.
        message.Abandon();
    }
}, options);
```

Ten przykład konfiguruje wywołanie zwrotne metody [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.onmessage.aspx) przy użyciu obiektu [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx). Właściwość [AutoComplete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) jest ustawiona na wartość **false** w celu umożliwienia ręcznej kontroli nad tym, kiedy wywołać metodę [Complete][] na odebranym komunikacie. Właściwość [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) jest ustawiona na 1 minutę, co powoduje, że klient oczekuje przez jedną minutę na komunikat, zanim upłynie limit czasu wywołania i klient wyśle nowe wywołanie w celu sprawdzenia komunikatów. Ta wartość właściwości zmniejsza liczbę przypadków, gdy klient wysyła wywołania, które mogą być obciążane i nie pobierają komunikatów.

## Sposób obsługi awarii aplikacji i komunikatów niemożliwych do odczytania

Usługa Service Bus zapewnia funkcję ułatwiające bezpieczne odzyskiwanie w razie błędów w aplikacji lub trudności z przetwarzaniem komunikatu. Jeśli aplikacja odbiorcy z jakiegoś powodu nie może przetworzyć komunikatu, wówczas może wywołać metodę [Abandon](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) dla odebranego komunikatu (zamiast metody [Complete][]). Powoduje to odblokowanie komunikatu w kolejce przez usługę Service Bus i ponowne udostępnienie go do odebrania przez tę samą lub inną odbierającą aplikację.

Istnieje również limit czasu skojarzony z komunikatem zablokowanym w kolejce i jeśli aplikacja nie może przetworzyć komunikatu przed upływem limitu czasu blokady (na przykład jeśli wystąpiła awaria aplikacji), usługa Service Bus automatycznie odblokowuje komunikaty i udostępnia je do ponownego odbioru.

W przypadku gdy aplikacja przestaje działać po przetworzeniu komunikatu, ale przed wysłaniem żądania [Complete][], komunikat zostanie dostarczony do aplikacji po jej ponownym uruchomieniu. Jest to często nazywane **przetwarzaniem co najmniej raz**, co oznacza, że każdy komunikat jest przetwarzany co najmniej raz, ale w pewnych sytuacjach ten sam komunikat może być dostarczony ponownie. Jeśli scenariusz nie toleruje dwukrotnego przetwarzania, deweloperzy aplikacji powinni dodać dodatkową logikę do swojej aplikacji w celu obsługi dwukrotnego dostarczania komunikatów. Jest to często osiągane przy użyciu właściwości [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) komunikatu, która pozostaje stała między kolejnymi próbami dostarczenia.

## Następne kroki

Teraz, kiedy znasz już podstawy kolejek usługi Service Bus, skorzystaj z poniższych linków, aby dowiedzieć się więcej.

-   Przeczytaj o jednostkach obsługi komunikatów usługi Service Bus w artykule [Kolejki, tematy i subskrypcje][].
-   Utwórz działającą aplikację, która wysyła i odbiera komunikaty z kolejki usługi Service Bus, korzystając z [samouczka Komunikaty obsługiwane przez brokera w usłudze Service Bus dla platformy .NET][].
-   Pobierz przykłady usługi Service Bus ze strony [przykładów dla platformy Azure][] lub zobacz [przegląd przykładów usługi Service Bus][].

  [klasycznego portalu Azure]: http://manage.windowsazure.com
  [7]: ./media/service-bus-dotnet-how-to-use-queues/getting-started-multi-tier-13.png
  [Kolejki, tematy i subskrypcje]: service-bus-queues-topics-subscriptions.md
  [samouczka Komunikaty obsługiwane przez brokera w usłudze Service Bus dla platformy .NET]: service-bus-brokered-tutorial-dotnet.md
  [przykładów dla platformy Azure]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [przegląd przykładów usługi Service Bus]: service-bus-samples.md
  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [Complete]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx


<!--HONumber=Jun16_HO2-->


