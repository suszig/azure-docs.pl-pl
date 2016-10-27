<properties 
    pageTitle="Przewodnik programowania dotyczący usługi Azure Event Hubs | Microsoft Azure"
    description="Zawiera opis programowania w usłudze Azure Event Hubs przy użyciu zestawu Azure SDK dla platformy .NET."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="08/16/2016"
    ms.author="sethm" />


# <a name="event-hubs-programming-guide"></a>Przewodnik programowania w usłudze Event Hubs

W tym temacie opisano programowanie w usłudze Azure Event Hubs przy użyciu zestawu Azure SDK dla platformy .NET. Przyjęto założenie, że wstępnie znasz i rozumiesz usługę Event Hubs. Omówienie koncepcji usługi Event Hubs można znaleźć w temacie [Przegląd usługi Event Hubs](event-hubs-overview.md).

## <a name="event-publishers"></a>Wydawcy zdarzeń

Wysyłanie zdarzeń do centrum zdarzeń odbywa się za pomocą żądań POST protokołu HTTP albo za pomocą połączenia protokołu AMQP 1.0. Wybór, która metoda i kiedy ma być używana, zależy od konkretnego scenariusza. Połączenia protokołu AMQP 1.0 są mierzone jako połączenia obsługiwane przez brokera w Service Bus i są bardziej odpowiednie w scenariuszach z częstymi większymi ilościami wiadomości oraz wymaganiami dotyczącymi krótszych opóźnień, ponieważ zapewniają trwały kanał obsługi komunikatów.

Centra Event Hubs są tworzone i zarządzane przy użyciu klasy [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx). W przypadku używania zarządzanych interfejsów API platformy .NET głównymi konstrukcjami na potrzeby publikowania danych w usłudze Event Hubs są klasy [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) i [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx). Klasa [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) udostępnia kanał komunikacji protokołu AMQP, przez który zdarzenia są wysyłane do centrum zdarzeń. Klasa [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) reprezentuje zdarzenie i jest używana do publikowania komunikatów w centrum zdarzeń. Ta klasa zawiera treść, niektóre metadane i informacje nagłówka zdarzenia. Inne właściwości są dodawane do obiektu [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx), kiedy przechodzi on przez centrum zdarzeń.

## <a name="get-started"></a>Rozpoczęcie pracy

Klasy platformy .NET, które obsługują usługę Event Hubs, są udostępniane w zestawie Microsoft.ServiceBus.dll. Najprostszym sposobem odwołania do interfejsu API usługi Service Bus i skonfigurowania aplikacji ze wszystkimi zależnościami usługi Service Bus jest pobranie [pakietu NuGet usługi Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Alternatywnie można użyć [konsoli menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) w programie Visual Studio. Aby to zrobić, należy wydać następujące polecenie w oknie [konsoli menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

```
Install-Package WindowsAzure.ServiceBus
```

## <a name="create-an-event-hub"></a>Tworzenie centrum zdarzeń

Do tworzenia usług Event Hubs służy klasa [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx). Na przykład:

```
var manager = new Microsoft.ServiceBus.NamespaceManager("mynamespace.servicebus.windows.net");
var description = manager.CreateEventHub("MyEventHub");
```

W większości przypadków zaleca się użycie metody [CreateEventHubIfNotExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createeventhubifnotexists.aspx), aby uniknąć generowania wyjątków, jeśli usługa jest uruchamiana ponownie. Na przykład:

```
var description = manager.CreateEventHubIfNotExists("MyEventHub");
```

Wszystkie operacje tworzenia usług Event Hubs, w tym metoda [CreateEventHubIfNotExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createeventhubifnotexists.aspx), wymagają uprawnień **Zarządzania** do danej przestrzeni nazw. Jeśli chcesz ograniczyć uprawnienia aplikacji wydawcy lub odbiorcy, możesz uniknąć wywołań tych operacji Create w kodzie produkcyjnym, korzystając z poświadczeń z ograniczonymi uprawnieniami.

Klasa [EventHubDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubdescription.aspx) zawiera szczegółowe informacje o centrum zdarzeń, w tym reguły autoryzacji, interwał przechowywania komunikatów, identyfikatory partycji, stan i ścieżkę. Ta klasa służy do aktualizacji metadanych w centrum zdarzeń.

## <a name="create-an-event-hubs-client"></a>Tworzenie klienta usługi Event Hubs

Podstawową klasą służącą do interakcji z usługą Event Hubs jest klasa [Microsoft.ServiceBus.Messaging.EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx). Ta klasa udostępnia zarówno funkcje nadawcy, jak i odbiornika. Można utworzyć wystąpienie tej klasy przy użyciu metody [Create](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx), jak pokazano w poniższym przykładzie.

```
var client = EventHubClient.Create(description.Path);
```

Metoda ta wykorzystuje informacje o połączeniu usługi Service Bus w pliku App.config, w sekcji `appSettings`. Przykładowy kod XML sekcji `appSettings` używany do przechowywania informacji o połączeniu usługi Service Bus można znaleźć w  dokumentacji metody [Microsoft.ServiceBus.Messaging.EventHubClient.Create(System.String)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx).

Innym rozwiązaniem jest utworzenie klienta na podstawie parametrów połączenia. Ta opcja dobrze działa w przypadku używania ról procesów roboczych platformy Azure, ponieważ ciąg parametrów można przechowywać we właściwości konfiguracji dla procesu roboczego. Na przykład:

```
EventHubClient.CreateFromConnectionString("your_connection_string");
```

Parametry połączenia będą w tym samym formacie, w jakim są wyświetlane w pliku App.config dla wcześniejszych metod:

```
Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=Manage;SharedAccessKey=[key]
```

Na koniec istnieje również możliwość utworzenia obiektu [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) z wystąpienia klasy [MessagingFactory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx), jak pokazano w poniższym przykładzie.

```
var factory = MessagingFactory.CreateFromConnectionString("your_connection_string");
var client = factory.CreateEventHubClient("MyEventHub");
```

Należy pamiętać, że dodatkowe obiekty [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) utworzone za pomocą wystąpienia fabryki obsługi komunikatów będą ponownie używały tego samego podstawowego połączenia TCP. Z tego względu te obiekty mają limit przepływności po stronie klienta. Metoda [Create](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.create.aspx) ponownie używa pojedynczej fabryki obsługi wiadomości. Jeśli potrzebujesz bardzo wysokiej przepływności od jednego nadawcy, możesz utworzyć wiele fabryk komunikatów i jeden obiekt [EventHubClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx) z każdej fabryki obsługi komunikatów.

## <a name="send-events-to-an-event-hub"></a>Wysyłanie zdarzeń do centrum zdarzeń

Zdarzenia wysyłane są do centrum zdarzeń przez utworzenie wystąpienia klasy [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) i wysłanie go za pomocą metody [Send](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.send.aspx). Ta metoda przyjmuje jeden parametr wystąpienia klasy [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) i synchronicznie wysyła go do centrum zdarzeń.

## <a name="event-serialization"></a>Serializacja zdarzeń

Klasa [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) ma [cztery przeciążone konstruktory](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.eventdata.aspx), które wymagają różnych parametrów, takich jak obiekt i serializator, tablica bajtów lub strumień. Istnieje również możliwość utworzenia wystąpienia klasy [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx), a następnie ustawienia strumienia treści. W przypadku używania formatu JSON z klasą [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) można użyć funkcji **Encoding.UTF8.GetBytes()** do pobrania tablicy bajtowej dla ciągu zakodowanego w formacie JSON.

## <a name="partition-key"></a>Klucz partycji

Klasa [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) ma właściwość [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx) umożliwiającą nadawcy określenie wartości, dla której obliczana jest wartość skrótu w celu utworzenia przypisania partycji. Użycie klucza partycji gwarantuje, że wszystkie zdarzenia z tym samym kluczem są wysyłane do tej samej partycji w centrum zdarzeń. Wspólne klucze partycji zawierają identyfikatory sesji użytkownika i unikatowe identyfikatory nadawcy. Właściwość [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx) jest opcjonalna i może zostać podana w przypadku użycia metody [Microsoft.ServiceBus.Messaging.EventHubClient.Send(Microsoft.ServiceBus.Messaging.EventData)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) lub [Microsoft.ServiceBus.Messaging.EventHubClient.SendAsync(Microsoft.ServiceBus.Messaging.EventData)](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx). Jeśli nie podano wartości dla właściwości [PartitionKey](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.partitionkey.aspx), wysyłane zdarzenia są dystrybuowane na partycje przy użyciu modelu działania okrężnego.

## <a name="batch-event-send-operations"></a>Operacje wysyłania partii zdarzeń

Wysyłanie zdarzeń w partiach może znacznie zwiększyć przepływność. Metoda [SendBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.sendbatch.aspx) przyjmuje parametr **IEnumerable** typu [EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.aspx) i wysyła całą partię jako operacją niepodzielną do centrum zdarzeń.

```
public void SendBatch(IEnumerable<EventData> eventDataList);
```

Należy pamiętać, że pojedyncza partia nie może przekraczać limitu 256 KB dla zdarzenia. Ponadto każdy komunikat w partii używa tej samej tożsamości wydawcy. Nadawca jest odpowiedzialny za upewnienie się, że partia nie przekracza maksymalnego rozmiaru zdarzenia. Jeśli go przekroczy, zostanie wygenerowany błąd metody **Send** klienta.

## <a name="send-asynchronously-and-send-at-scale"></a>Wysyłanie asynchroniczne i wysyłanie na dużą skalę

Zdarzenia można również wysyłać do centrum zdarzeń asynchronicznie. Wysyłanie asynchroniczne może zwiększyć szybkość, z jaką klient jest w stanie wysyłać zdarzenia. Metody [Send](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.send.aspx) i [SendBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.sendbatch.aspx) są dostępne w wersjach asynchronicznych, które zwracają obiekt [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Ta technika może zwiększyć przepływność, jednak może również spowodować, że klient będzie kontynuować wysyłanie zdarzeń nawet wtedy, gdy jest ograniczany przez usługę Event Hubs, co może skutkować błędami klienta lub utratą komunikatów, jeśli metoda nie została poprawnie zaimplementowana. Dodatkowo można użyć właściwości [RetryPolicy](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.cliententity.retrypolicy.aspx) na kliencie do sterowania opcjami ponawiania prób klienta.

## <a name="create-a-partition-sender"></a>Tworzenie nadawcy dla partycji

Chociaż przeważnie zdarzenia są wysyłane do centrum zdarzeń z użyciem klucza partycji, w niektórych przypadkach można wysyłać zdarzenia bezpośrednio do danej partycji. Na przykład:

```
var partitionedSender = client.CreatePartitionedSender(description.PartitionIds[0]);
```

Metoda [CreatePartitionedSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.createpartitionedsender.aspx) zwraca obiekt [EventHubSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubsender.aspx) umożliwiający publikowanie zdarzeń na określonej partycji centrum zdarzeń.

## <a name="event-consumers"></a>Odbiorcy zdarzeń

Usługa Event Hubs ma dwa podstawowe modele użycia zdarzeń: odbiorniki bezpośrednie i abstrakcje wyższego poziomu, takie jak klasa [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx). Odbiorniki bezpośrednie są odpowiedzialne za własną koordynację dostępu do partycji w obrębie grupy odbiorców.

### <a name="direct-consumer"></a>Bezpośredni odbiorca

Najbardziej bezpośrednim sposobem odczytu z partycji w obrębie grupy odbiorców jest użycie klasy [EventHubReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubreceiver.aspx). Aby utworzyć wystąpienie tej klasy, należy użyć wystąpienia klasy [EventHubConsumerGroup](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubconsumergroup.aspx). W poniższym przykładzie należy określić identyfikator partycji podczas tworzenia odbiornika dla grupy odbiorców.

```
EventHubConsumerGroup group = client.GetDefaultConsumerGroup();
var receiver = group.CreateReceiver(client.GetRuntimeInformation().PartitionIds[0]);
```

Metoda [CreateReceiver](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubconsumergroup.createreceiver.aspx) ma kilka przeciążeń, które ułatwiają kontrolę nad tworzonym czytnikiem. Te metody obejmują określanie przesunięcia jako ciągu lub sygnatury czasowej oraz możliwość określenia, czy uwzględniać to określone przesunięcie w zwróconym strumieniu, czy rozpocząć po nim. Po utworzeniu odbiornika można uruchomić odbieranie zdarzeń na zwracanym obiekcie. Metoda [Receive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubreceiver.receive.aspx) ma cztery przeciążenia sterujące parametrami operacji odbierania, takimi jak rozmiar partii i czas oczekiwania. Używając asynchronicznych wersji tych metod, można zwiększyć przepływność odbiorcy. Na przykład:

```
bool receive = true;
string myOffset;
while(receive)
{
    var message = receiver.Receive();
    myOffset = message.Offset;
    string body = Encoding.UTF8.GetString(message.GetBytes());
    Console.WriteLine(String.Format("Received message offset: {0} \nbody: {1}", myOffset, body));
}
```

W odniesieniu do określonej partycji komunikaty są odbierane w kolejności, w jakiej zostały wysłane do centrum zdarzeń. Przesunięcie jest tokenem ciągu służącym do identyfikowania komunikatów w partycji.

Należy zauważyć, że pojedyncza partycja w obrębie grupy odbiorców w żadnym momencie nie może mieć połączonych więcej niż 5 współbieżnych czytników. Gdy czytniki nawiązują połączenie lub rozłączają się, ich sesje mogą pozostać aktywne przez kilka minut, zanim usługa rozpozna, że zostały odłączone. W tym czasie ponowne połączenie się z partycją może zakończyć się niepowodzeniem. Aby uzyskać kompletny przykład pisania aplikacji bezpośredniego odbiornika dla usługi Event Hubs, zobacz przykład [Odbiorniki bezpośrednie w usłudze Service Bus Event Hubs](https://code.msdn.microsoft.com/Event-Hub-Direct-Receivers-13fa95c6).

### <a name="event-processor-host"></a>Host procesora zdarzeń

Klasa [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) przetwarza dane z usługi Event Hubs. Podczas tworzenia czytników zdarzeń na platformie .NET należy używać tej implementacji. Klasa [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) udostępnia bezpieczne wątkowo, wieloprocesowe, bezpieczne środowisko uruchomieniowe dla implementacji procesora zdarzeń, które umożliwia także tworzenie punktów kontrolnych i zarządzanie dzierżawą partycji.

Aby używać klasy [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx), można zaimplementować interfejs [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx). Ten interfejs zawiera trzy metody:

- [OpenAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.openasync.aspx)

- [CloseAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.closeasync.aspx)

- [ProcessEventsAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.processeventsasync.aspx)

Aby rozpocząć przetwarzanie zdarzeń, utwórz wystąpienie klasy [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx), podając odpowiednie parametry centrum zdarzeń. Następnie wywołaj metodę [RegisterEventProcessorAsync](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.registereventprocessorasync.aspx), aby zarejestrować swoją implementację interfejsu [IEventProcessor](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ieventprocessor.aspx) w środowisku uruchomieniowym. W tym momencie host będzie podejmować próby uzyskania dzierżawy na każdej partycji w centrum zdarzeń za pomocą „zachłannego” algorytmu. Te dzierżawy będą trwać przez dany przedział czasu, a następnie muszą być odnowione. W miarę jak nowe węzły, w tym przypadku wystąpienia procesów roboczych, przechodzą w tryb online, umieszczają rezerwacje dzierżawy i z czasem obciążenie przesuwa się między węzłami, ponieważ każdy próbuje uzyskać więcej dzierżaw.

![Host procesora zdarzeń](./media/event-hubs-programming-guide/IC759863.png)

W miarę upływu czasu zostaje ustalona równowaga. Ta dynamiczna funkcja umożliwia zastosowanie skalowania automatycznego na podstawie procesora CPU do odbiorców, zarówno w celu skalowania w górę, jak i w dół. Ponieważ usługa Event Hubs nie obsługuje bezpośredniej koncepcji liczby komunikatów, średnie wykorzystanie procesora CPU jest często najlepszym mechanizmem mierzenia skali zaplecza lub odbiorcy. Jeśli wydawcy zaczną publikować więcej zdarzeń, niż odbiorcy mogą przetworzyć, zwiększenie użycia procesora CPU przez odbiorców może służyć do powodowania automatycznego skalowania liczby wystąpień procesu roboczego.

Klasa [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx) implementuje również mechanizm tworzenia punktów kontrolnych oparty na usłudze Azure Storage. Ten mechanizm przechowuje przesunięcie na podstawie partycji, dzięki czemu każdy odbiorca może określić, jaki był ostatni punkt kontrolny od poprzedniego odbiorcy. Ponieważ partycje przechodzą między węzłami za pośrednictwem dzierżaw, jest to mechanizm synchronizacji, który ułatwia przesunięcie obciążenia.

## <a name="publisher-revocation"></a>Odwołanie wydawcy

Oprócz zaawansowanych funkcji środowiska uruchomieniowego klasy [EventProcessorHost](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx), usługa Event Hubs umożliwia odwołanie wydawcy w celu zablokowania określonym wydawcom możliwości wysyłania zdarzeń do centrum zdarzeń. Te funkcje są szczególnie przydatne, jeśli zostały naruszone zabezpieczenia tokenu wydawcy lub aktualizacja oprogramowania powoduje nieuprawnione zachowanie wydawcy. W takich sytuacjach dla tożsamości wydawcy, która jest częścią jego tokenu sygnatury dostępu współdzielonego, można zablokować dostęp do publikowania zdarzeń.

Aby uzyskać więcej informacji o odwołaniu wydawcy i sposobie wysyłania zdarzeń do usługi Event Hubs jako wydawca, zobacz przykład [Event Hubs Large Scale Secure Publishing](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab) (Bezpieczne publikowanie na dużą skalę w usłudze Event Hubs).

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o scenariuszach usługi Event Hubs, skorzystaj z następujących linków:

- [Przegląd interfejsu API usługi Event Hubs](event-hubs-api-overview.md)
- [Przegląd usługi Event Hubs](event-hubs-overview.md)
- [Przykłady kodu dla usługi Event Hubs](http://code.msdn.microsoft.com/site/search?query=event hub&f[0].Value=event hub&f[0].Type=SearchText&ac=5)
- [Dokumentacja interfejsu API hosta procesora zdarzeń](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost.aspx)



<!--HONumber=Oct16_HO3-->


