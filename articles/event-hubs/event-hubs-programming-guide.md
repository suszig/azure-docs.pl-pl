---
title: "Przewodnik programowania dotyczący usługi Azure Event Hubs | Microsoft Docs"
description: "Pisanie kodu dla usługi Azure Event Hubs przy użyciu zestawu .NET SDK usługi Azure."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 64cbfd3d-4a0e-4455-a90a-7f3d4f080323
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/17/2017
ms.author: sethm
ms.openlocfilehash: 405ec2b27b488b570c4a5c86e4950ff98233360e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="event-hubs-programming-guide"></a>Przewodnik programowania w usłudze Event Hubs

W tym artykule opisano kilka typowych scenariuszy w pisanie kodu przy użyciu usługi Azure Event Hubs i zestawu Azure .NET SDK. Przyjęto założenie, że wstępnie znasz i rozumiesz usługę Event Hubs. Omówienie koncepcji usługi Event Hubs można znaleźć w temacie [Przegląd usługi Event Hubs](event-hubs-what-is-event-hubs.md).

## <a name="event-publishers"></a>Wydawcy zdarzeń

Zdarzenia są wysyłane do Centrum zdarzeń za pomocą metody POST protokołu HTTP lub przy użyciu połączenia protokołu AMQP 1.0. Wybór, który ma zostać użyty, gdy zależy od konkretnego scenariusza. Połączenia protokołu AMQP 1.0 są mierzone jako połączenia obsługiwane przez brokera w Service Bus i są bardziej odpowiednie w scenariuszach z częstymi większymi ilościami wiadomości oraz wymaganiami dotyczącymi krótszych opóźnień, ponieważ zapewniają trwały kanał obsługi komunikatów.

Centra Event Hubs są tworzone i zarządzane przy użyciu klasy [NamespaceManager][]. W przypadku używania zarządzanych interfejsów API platformy .NET głównymi konstrukcjami na potrzeby publikowania danych w usłudze Event Hubs są klasy [EventHubClient](/dotnet/api/microsoft.servicebus.messaging.eventhubclient) i [EventData][]. [EventHubClient][] udostępnia kanał komunikacji protokołu AMQP, przez który zdarzenia są wysyłane do Centrum zdarzeń. [EventData][] klasy reprezentuje zdarzenie i jest używana do publikowania komunikatów do Centrum zdarzeń. Ta klasa zawiera treść, niektóre metadane i informacje nagłówka zdarzenia. Inne właściwości są dodawane do [EventData][] obiektów, kiedy przechodzi on przez Centrum zdarzeń.

## <a name="get-started"></a>Rozpoczynanie pracy

Klasy platformy .NET, które obsługują usługę Event Hubs, są udostępniane w zestawie Microsoft.ServiceBus.dll. Najprostszym sposobem odwołania do interfejsu API usługi Service Bus i skonfigurowania aplikacji ze wszystkimi zależnościami usługi Service Bus jest pobranie [pakietu NuGet usługi Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Alternatywnie można użyć [konsoli menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) w programie Visual Studio. Aby to zrobić, należy wydać następujące polecenie w oknie [konsoli menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

```
Install-Package WindowsAzure.ServiceBus
```

## <a name="create-an-event-hub"></a>Tworzenie centrum zdarzeń
Do tworzenia usług Event Hubs służy klasa [NamespaceManager][]. Na przykład:

```csharp
var manager = new Microsoft.ServiceBus.NamespaceManager("mynamespace.servicebus.windows.net");
var description = manager.CreateEventHub("MyEventHub");
```

W większości przypadków zaleca się użycie metody [CreateEventHubIfNotExists][], aby uniknąć generowania wyjątków, jeśli usługa jest uruchamiana ponownie. Na przykład:

```csharp
var description = manager.CreateEventHubIfNotExists("MyEventHub");
```

Wszystkie operacje tworzenia usług Event Hubs, w tym metoda [CreateEventHubIfNotExists][], wymagają uprawnień **Zarządzania** do danej przestrzeni nazw. Jeśli chcesz ograniczyć uprawnienia aplikacji wydawcy lub odbiorcy, możesz uniknąć wywołań tych operacji Create w kodzie produkcyjnym, korzystając z poświadczeń z ograniczonymi uprawnieniami.

[EventHubDescription](/dotnet/api/microsoft.servicebus.messaging.eventhubdescription) klasa zawiera szczegółowe informacje o Centrum zdarzeń, w tym reguły autoryzacji, interwał przechowywania komunikatów, identyfikatory partycji, stan i ścieżkę. Ta klasa służy do aktualizacji metadanych w Centrum zdarzeń.

## <a name="create-an-event-hubs-client"></a>Tworzenie klienta usługi Event Hubs
Podstawowe klasy do interakcji z usługą Event Hubs jest [Microsoft.ServiceBus.Messaging.EventHubClient][EventHubClient]. Ta klasa udostępnia zarówno funkcje nadawcy, jak i odbiornika. Można utworzyć wystąpienie tej klasy przy użyciu metody [Create](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.create), jak pokazano w poniższym przykładzie.

```csharp
var client = EventHubClient.Create(description.Path);
```

Metoda ta wykorzystuje informacje o połączeniu usługi Service Bus w pliku App.config, w sekcji `appSettings`. Przykładowy kod XML sekcji `appSettings` używany do przechowywania informacji o połączeniu usługi Service Bus można znaleźć w  dokumentacji metody [Microsoft.ServiceBus.Messaging.EventHubClient.Create(System.String)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Create_System_String_).

Innym rozwiązaniem jest utworzenie klienta na podstawie parametrów połączenia. Ta opcja dobrze działa w przypadku używania ról procesów roboczych platformy Azure, ponieważ ciąg parametrów można przechowywać we właściwości konfiguracji dla procesu roboczego. Na przykład:

```csharp
EventHubClient.CreateFromConnectionString("your_connection_string");
```

Parametry połączenia będą w tym samym formacie, w jakim są wyświetlane w pliku App.config dla wcześniejszych metod:

```
Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[key]
```

Na koniec istnieje również możliwość utworzenia obiektu [EventHubClient][] z wystąpienia klasy [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory), jak pokazano w poniższym przykładzie.

```csharp
var factory = MessagingFactory.CreateFromConnectionString("your_connection_string");
var client = factory.CreateEventHubClient("MyEventHub");
```

Należy pamiętać, że dodatkowe obiekty [EventHubClient][] utworzone za pomocą wystąpienia fabryki obsługi komunikatów będą ponownie używały tego samego podstawowego połączenia TCP. Z tego względu te obiekty mają limit przepływności po stronie klienta. Metoda [Create](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Create_System_String_) ponownie używa pojedynczej fabryki obsługi wiadomości. Jeśli potrzebujesz bardzo wysokiej przepływności od jednego nadawcy, możesz utworzyć wiele fabryk komunikatów i jeden obiekt [EventHubClient][] z każdej fabryki obsługi komunikatów.

## <a name="send-events-to-an-event-hub"></a>Wysyłanie zdarzeń do Centrum zdarzeń
Wysyłanie zdarzeń do Centrum zdarzeń przez utworzenie [EventData][] wystąpienia i wysłanie go za pomocą [wysyłania](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) metody. Ta metoda przyjmuje jeden [EventData][] parametru wystąpienia i synchronicznie wysyła go do Centrum zdarzeń.

## <a name="event-serialization"></a>Serializacja zdarzeń
Klasa [EventData][] ma [cztery przeciążone konstruktory](/dotnet/api/microsoft.servicebus.messaging.eventdata#constructors_), które wymagają różnych parametrów, takich jak obiekt i serializator, tablica bajtów lub strumień. Istnieje również możliwość utworzenia wystąpienia klasy [EventData][], a następnie ustawienia strumienia treści. W przypadku używania formatu JSON z klasą [EventData][] można użyć funkcji **Encoding.UTF8.GetBytes()** do pobrania tablicy bajtowej dla ciągu zakodowanego w formacie JSON.

## <a name="partition-key"></a>Klucz partycji
Klasa [EventData][] ma właściwość [PartitionKey][] umożliwiającą nadawcy określenie wartości, dla której obliczana jest wartość skrótu w celu utworzenia przypisania partycji. Użycie klucza partycji gwarantuje, że wszystkie zdarzenia z tym samym kluczem są wysyłane do tej samej partycji w zdarzeniu koncentratora. Wspólne klucze partycji zawierają identyfikatory sesji użytkownika i unikatowe identyfikatory nadawcy. Właściwość [PartitionKey][] jest opcjonalna i może zostać podana w przypadku użycia metody [Microsoft.ServiceBus.Messaging.EventHubClient.Send(Microsoft.ServiceBus.Messaging.EventData)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) lub [Microsoft.ServiceBus.Messaging.EventHubClient.SendAsync(Microsoft.ServiceBus.Messaging.EventData)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendAsync_Microsoft_ServiceBus_Messaging_EventData_). Jeśli nie podano wartości dla właściwości [PartitionKey][], wysyłane zdarzenia są dystrybuowane na partycje przy użyciu modelu działania okrężnego.

### <a name="availability-considerations"></a>Zagadnienia dotyczące dostępności

Użycie klucza partycji jest opcjonalne, a użytkownik powinien zastanów się, czy należy użyć jednej. W wielu przypadkach przy użyciu klucza partycji jest dobrym rozwiązaniem, jeśli zdarzenie kolejność jest ważna. Użycie klucza partycji, partycje wymagają dostępności w jednym węźle, a awarii wraz z upływem czasu; na przykład podczas obliczania ponownego uruchomienia węzłów i poprawki. W efekcie ustawiony identyfikator partycji i tej partycji przestanie być dostępny dla jakiegoś powodu, próba dostępu do danych w tej partycji zakończy się niepowodzeniem. Jeśli najważniejszych jest wysoka dostępność, nie należy określać klucz partycji. w takim przypadku zdarzenia zostaną wysłane na partycje przy użyciu modelu okrężnego opisanych powyżej. W tym scenariuszu utworzysz jawną wyboru między dostępności (Brak Identyfikatora partycji) i spójności (przypinanie zdarzenia do Identyfikatora partycji).

Kolejnym zagadnieniem jest obsługa opóźnienia w przetwarzaniu zdarzeń. W niektórych przypadkach może być lepszym rozwiązaniem danych i ponów próbę, niż można spróbować i nadąża z przetwarzaniem, który może powodować opóźnienia dalszego przetwarzania podrzędnego. Na przykład z giełdowych lepiej jest oczekiwania pełną aktualne dane, ale w czatu na żywo lub scenariusza VOIP raczej trzeba danych szybko, nawet jeśli nie jest ukończone.

Podany w tych scenariuszach te zagadnienia dotyczące dostępności, możesz wybrać jedną z powodu następującego błędu strategii obsługi:

- Stop (Zatrzymaj odczytywania z usługi Event Hubs, dopóki elementy zostały usunięte)
- Upuść (komunikaty nie są ważne, upuść je)
- Spróbuj ponownie (ponownych prób, które komunikaty, jak widać umieścić)
- [Utraconych](../service-bus-messaging/service-bus-dead-letter-queues.md) (Użyj kolejki lub innego Centrum zdarzeń, aby wiadomości utraconych tylko komunikatów nie może przetworzyć)

Aby uzyskać więcej informacji i omówienie kompromis między dostępności i spójności zobacz [dostępności i spójności w usłudze Event Hubs](event-hubs-availability-and-consistency.md). 

## <a name="batch-event-send-operations"></a>Operacje wysyłania partii zdarzeń
Wysyłanie zdarzeń w partiach może znacznie zwiększyć przepływność. [SendBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__) ma metodę **IEnumerable** parametr typu [EventData][] i wysyła całą partię jako operacją niepodzielną do Centrum zdarzeń.

```csharp
public void SendBatch(IEnumerable<EventData> eventDataList);
```

Należy pamiętać, że pojedyncza partia nie może przekraczać limitu 256 KB dla zdarzenia. Ponadto każdy komunikat w partii używa tej samej tożsamości wydawcy. Nadawca jest odpowiedzialny za upewnienie się, że partia nie przekracza maksymalnego rozmiaru zdarzenia. Jeśli go przekroczy, zostanie wygenerowany błąd metody **Send** klienta.

## <a name="send-asynchronously-and-send-at-scale"></a>Wysyłanie asynchroniczne i wysyłanie na dużą skalę
Możesz również wysłać zdarzenia do Centrum zdarzeń asynchronicznie. Wysyłanie asynchroniczne może zwiększyć szybkość, z jaką klient jest w stanie wysyłać zdarzenia. Metody [Send](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) i [SendBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__) są dostępne w wersjach asynchronicznych, które zwracają obiekt [Task](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Ta technika może zwiększyć przepływność, jednak może również spowodować, że klient będzie kontynuować wysyłanie zdarzeń nawet wtedy, gdy jest ograniczany przez usługę Event Hubs, co może skutkować błędami klienta lub utratą komunikatów, jeśli metoda nie została poprawnie zaimplementowana. Dodatkowo można użyć właściwości [RetryPolicy](/dotnet/api/microsoft.servicebus.messaging.cliententity#Microsoft_ServiceBus_Messaging_ClientEntity_RetryPolicy) na kliencie do sterowania opcjami ponawiania prób klienta.

## <a name="create-a-partition-sender"></a>Tworzenie nadawcy dla partycji
Najbardziej typowe do wysyłania zdarzeń do Centrum zdarzeń bez klucza partycji, ale w niektórych przypadkach można wysyłać zdarzenia bezpośrednio do danej partycji. Na przykład:

```csharp
var partitionedSender = client.CreatePartitionedSender(description.PartitionIds[0]);
```

[CreatePartitionedSender](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_CreatePartitionedSender_System_String_) zwraca [EventHubSender](/dotnet/api/microsoft.servicebus.messaging.eventhubsender) obiekt, czy można użyć do publikowania zdarzeń partycji Centrum konkretnego zdarzenia.

## <a name="event-consumers"></a>Odbiorcy zdarzeń
Usługa Event Hubs ma dwa podstawowe modele użycia zdarzeń: odbiorniki bezpośrednie i abstrakcje wyższego poziomu, takie jak klasa [EventProcessorHost][]. Odbiorniki bezpośrednie są odpowiedzialne za własną koordynację dostępu do partycji w obrębie grupy odbiorców.

### <a name="direct-consumer"></a>Bezpośredni odbiorca
Najbardziej bezpośrednim sposobem odczytu z partycji w obrębie grupy odbiorców jest użycie klasy [EventHubReceiver](/dotnet/apie/microsoft.servicebus.messaging.eventhubreceiver). Aby utworzyć wystąpienie tej klasy, należy użyć wystąpienia klasy [EventHubConsumerGroup](/dotnet/api/microsoft.servicebus.messaging.eventhubconsumergroup). W poniższym przykładzie należy określić identyfikator partycji podczas tworzenia odbiornika dla grupy odbiorców.

```csharp
EventHubConsumerGroup group = client.GetDefaultConsumerGroup();
var receiver = group.CreateReceiver(client.GetRuntimeInformation().PartitionIds[0]);
```

Metoda [CreateReceiver](/dotnet/api/microsoft.servicebus.messaging.eventhubconsumergroup#methods_summary) ma kilka przeciążeń, które ułatwiają kontrolę nad tworzonym czytnikiem. Te metody obejmują określanie przesunięcia jako ciągu lub sygnatury czasowej oraz możliwość określenia, czy uwzględniać to określone przesunięcie w zwróconym strumieniu, czy rozpocząć po nim. Po utworzeniu odbiornika można uruchomić odbieranie zdarzeń na zwracanym obiekcie. Metoda [Receive](/dotnet/api/microsoft.servicebus.messaging.eventhubreceiver#methods_summary) ma cztery przeciążenia sterujące parametrami operacji odbierania, takimi jak rozmiar partii i czas oczekiwania. Używając asynchronicznych wersji tych metod, można zwiększyć przepływność odbiorcy. Na przykład:

```csharp
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

W odniesieniu do określonej partycji komunikaty są odbierane w kolejności, w jakiej zostały wysłane do Centrum zdarzeń. Przesunięcie jest tokenem ciągu służącym do identyfikowania komunikatów w partycji.

Należy zauważyć, że pojedyncza partycja w obrębie grupy odbiorców w żadnym momencie nie może mieć połączonych więcej niż 5 współbieżnych czytników. Gdy czytniki nawiązują połączenie lub rozłączają się, ich sesje mogą pozostać aktywne przez kilka minut, zanim usługa rozpozna, że zostały odłączone. W tym czasie ponowne połączenie się z partycją może zakończyć się niepowodzeniem. Aby uzyskać kompletny przykład pisania aplikacji bezpośredniego odbiornika usługi Event hubs, zobacz [odbiorniki bezpośrednie centra zdarzeń](https://code.msdn.microsoft.com/Event-Hub-Direct-Receivers-13fa95c6) próbki.

### <a name="event-processor-host"></a>Host procesora zdarzeń
Klasa [EventProcessorHost][] przetwarza dane z usługi Event Hubs. Podczas tworzenia czytników zdarzeń na platformie .NET należy używać tej implementacji. Klasa [EventProcessorHost][] udostępnia bezpieczne wątkowo, wieloprocesowe, bezpieczne środowisko uruchomieniowe dla implementacji procesora zdarzeń, które umożliwia także tworzenie punktów kontrolnych i zarządzanie dzierżawą partycji.

Aby używać klasy [EventProcessorHost][], można zaimplementować interfejs [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor). Ten interfejs zawiera trzy metody:

* [OpenAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_OpenAsync_Microsoft_ServiceBus_Messaging_PartitionContext_)
* [CloseAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_CloseAsync_Microsoft_ServiceBus_Messaging_PartitionContext_Microsoft_ServiceBus_Messaging_CloseReason_)
* [ProcessEventsAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_ProcessEventsAsync_Microsoft_ServiceBus_Messaging_PartitionContext_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__)

Aby rozpocząć przetwarzanie zdarzeń, Utwórz wystąpienie [EventProcessorHost][], podając odpowiednie parametry Centrum zdarzeń. Następnie wywołaj metodę [RegisterEventProcessorAsync](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost#Microsoft_ServiceBus_Messaging_EventProcessorHost_RegisterEventProcessorAsync__1), aby zarejestrować swoją implementację interfejsu [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor) w środowisku uruchomieniowym. W tym momencie host będzie podejmować próby uzyskania dzierżawy na każdej partycji w Centrum zdarzeń za pomocą "zachłannego" algorytmu. Te dzierżawy będą trwać przez dany przedział czasu, a następnie muszą być odnowione. W miarę jak nowe węzły, w tym przypadku wystąpienia procesów roboczych, przechodzą w tryb online, umieszczają rezerwacje dzierżawy i z czasem obciążenie przesuwa się między węzłami, ponieważ każdy próbuje uzyskać więcej dzierżaw.

![Host procesora zdarzeń](./media/event-hubs-programming-guide/IC759863.png)

W miarę upływu czasu zostaje ustalona równowaga. Ta dynamiczna funkcja umożliwia zastosowanie skalowania automatycznego na podstawie procesora CPU do odbiorców, zarówno w celu skalowania w górę, jak i w dół. Ponieważ usługa Event Hubs nie obsługuje bezpośredniej koncepcji liczby komunikatów, średnie wykorzystanie procesora CPU jest często najlepszym mechanizmem mierzenia skali zaplecza lub odbiorcy. Jeśli wydawcy zaczną publikować więcej zdarzeń, niż odbiorcy mogą przetworzyć, zwiększenie użycia procesora CPU przez odbiorców może służyć do powodowania automatycznego skalowania liczby wystąpień procesu roboczego.

Klasa [EventProcessorHost][] implementuje również mechanizm tworzenia punktów kontrolnych oparty na usłudze Azure Storage. Ten mechanizm przechowuje przesunięcie na podstawie partycji, dzięki czemu każdy odbiorca może określić, jaki był ostatni punkt kontrolny od poprzedniego odbiorcy. Ponieważ partycje przechodzą między węzłami za pośrednictwem dzierżaw, jest to mechanizm synchronizacji, który ułatwia przesunięcie obciążenia.

## <a name="publisher-revocation"></a>Odwołanie wydawcy
Oprócz zaawansowanych funkcji środowiska wykonawczego [EventProcessorHost][], usługa Event Hubs umożliwia odwołanie wydawcy w celu zablokowania dostępu określonych wydawców do wysyłania zdarzeń do Centrum zdarzeń. Te funkcje są szczególnie przydatne, jeśli zostały naruszone zabezpieczenia tokenu wydawcy lub aktualizacja oprogramowania powoduje nieuprawnione zachowanie wydawcy. W takich sytuacjach dla tożsamości wydawcy, która jest częścią jego tokenu sygnatury dostępu współdzielonego, można zablokować dostęp do publikowania zdarzeń.

Aby uzyskać więcej informacji o odwołaniu wydawcy i sposobie wysyłania zdarzeń do usługi Event Hubs jako wydawca, zobacz przykład [Event Hubs Large Scale Secure Publishing](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab) (Bezpieczne publikowanie na dużą skalę w usłudze Event Hubs).

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej o scenariuszach usługi Event Hubs, skorzystaj z następujących linków:

* [Omówienie interfejsu API usługi Event Hubs](event-hubs-api-overview.md)
* [Co to jest usługa Event Hubs](event-hubs-what-is-event-hubs.md)
* [Availability and consistency in Event Hubs](event-hubs-availability-and-consistency.md) (Dostępność i spójność w usłudze Event Hubs)
* [Dokumentacja interfejsu API hosta procesora zdarzeń](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)

[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[EventHubClient]: /dotnet/api/microsoft.servicebus.messaging.eventhubclient
[EventData]: /dotnet/api/microsoft.servicebus.messaging.eventdata
[CreateEventHubIfNotExists]: /dotnet/api/microsoft.servicebus.namespacemanager.createeventhubifnotexists
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.eventdata#Microsoft_ServiceBus_Messaging_EventData_PartitionKey
[EventProcessorHost]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
