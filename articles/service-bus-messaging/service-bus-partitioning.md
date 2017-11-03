---
title: "Tworzenie tematów i kolejek usługi Azure Service Bus partycjonowanej | Dokumentacja firmy Microsoft"
description: "Opisuje sposób partycji tematów i kolejek usługi Service Bus przy użyciu wielu brokerzy wiadomości."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: a0c7d5a2-4876-42cb-8344-a1fc988746e7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm;hillaryc
ms.openlocfilehash: 5a4e69ea7e13cb017f8fb432c524c6a8ce9228a8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="partitioned-queues-and-topics"></a>Partycjonowane kolejki i tematy
Usługa Azure Service Bus jest stosowana w wielu brokerów komunikat do przetwarzania komunikatów i wiele magazynów obsługi komunikatów do przechowywania komunikatów. Konwencjonalne kolejka lub temat są obsługiwane przez brokera pojedynczej wiadomości i przechowywane w jeden Magazyn obsługi komunikatów. Usługa Service Bus *partycje* włączyć kolejek i tematów, lub *jednostki do obsługi komunikatów*, aby być dzielony na partycje w wielu brokerzy wiadomości i magazyny obsługi komunikatów. Oznacza to, że ogólną przepustowość partycjonowane jednostki nie jest już ograniczone przez wydajność brokera komunikatów pojedynczego lub magazynie obsługi komunikatów. Ponadto tymczasowego awaria magazynie obsługi komunikatów nie renderować partycjonowanej kolejka lub temat niedostępny. Partycjonowane kolejek i tematów może zawierać wszystkich zaawansowanych funkcji usługi Service Bus, takie jak obsługa transakcji i sesje.

Informacje o wewnętrzne usługi Service Bus, zobacz [Architektura usługi Service Bus] [ Service Bus architecture] artykułu.

Partycjonowanie jest włączone domyślnie podczas tworzenia jednostki dla wszystkich kolejek i tematów w wersjach Standard i Premium messaging. Można utworzyć Standard warstwy jednostki do obsługi komunikatów bez podziału na partycje, ale kolejek i tematów w przestrzeni nazw Premium zawsze są podzielone na partycje; Nie można wyłączyć tę opcję. 

Nie jest możliwe do zmiany opcji podziału na istniejącej kolejki lub tematu w warstwy standardowa lub Premium, można ustawić tylko opcji podczas tworzenia jednostki.

## <a name="how-it-works"></a>Jak to działa

Każdy partycjonowanej kolejka lub temat składa się z wielu fragmentów. Każdy fragment jest przechowywane w różnych magazynie obsługi komunikatów i obsługiwane przez brokera inny komunikat. Po wysłaniu wiadomości do kolejki podzielonym na partycje lub tematu usługi Service Bus przypisuje wiadomości na jeden z fragmentów. Zaznaczenie jest wykonywane losowo przez magistralę usług lub przy użyciu klucza partycji, który można określić nadawcy.

Gdy klient chce otrzymywać wiadomość z kolejki podzielonym na partycje lub subskrypcję tematu podzielonym na partycje, zapytania usługi Service Bus wszystkie fragmenty wiadomości, zwraca pierwszy komunikat o uzyskany z dowolnej magazyny obsługi komunikatów do odbiornika. Pamięci podręczne usługi Service Bus, innych komunikatów i zwraca je po odebraniu dodatkowe odbierania żądań. Odbieranie klienta nie został powiadomiony o partycjonowanie; zachowanie po stronie klienta partycjonowanej kolejka lub temat (na przykład Odczyt, ukończenia, odroczenie utraconych wiadomości, Odczyt z wyprzedzeniem) jest taka sama jak zachowanie regularne jednostki.

Nie ma żadnych dodatkowych kosztów przy wysyłaniu wiadomości lub odbierania wiadomości z kolejki podzielonym na partycje lub temat.

## <a name="enable-partitioning"></a>Włącz partycjonowania

Aby używać partycjonowanej kolejek i tematów z usługi Azure Service Bus, korzystanie z zestawu SDK platformy Azure w wersji 2,2 lub nowszej, lub określ `api-version=2013-10` w HTTP żądania.

### <a name="standard"></a>Standardowa

W warstwie standardowej obsługi wiadomości można utworzyć kolejki usługi Service Bus i tematów w rozmiarze 1, 2, 3, 4 lub 5 GB (wartość domyślna to 1 GB). Z partycjonowania włączone, usługi Service Bus tworzy 16 kopie (16 partycji) jednostki dla każdego Gigabajta określisz. Tak, można utworzyć kolejkę o rozmiarze 5 GB, z 16 partycji maksymalny rozmiar kolejki staje się (5 \* 16) = 80 GB. Maksymalny rozmiar kolejki podzielonym na partycje lub temat widzą analizując jego wpis [portalu Azure][Azure portal]w **omówienie** bloku dla tej jednostki.

### <a name="premium"></a>Premium

W przypadku przestrzeni nazw warstwy Premium można utworzyć kolejki usługi Service Bus i tematów w rozmiarze 1, 2, 3, 4, 5, 10, 20, 40 lub 80 GB (wartość domyślna to 1 GB). W partycji, domyślnie włączona, usługi Service Bus tworzy dwie partycje nieobsługujące jednostki. Maksymalny rozmiar kolejki podzielonym na partycje lub temat widzą analizując jego wpis [portalu Azure][Azure portal]w **omówienie** bloku dla tej jednostki.

Aby uzyskać więcej informacji na temat partycjonowania w warstwie Premium obsługi komunikatów, zobacz [usługi Service Bus w warstwie Premium i standardowa komunikatami w warstwie](service-bus-premium-messaging.md). 

### <a name="create-a-partitioned-entity"></a>Utwórz jednostkę podzielonym na partycje

Istnieje kilka sposobów tworzenia partycjonowanej kolejka lub temat. Podczas tworzenia kolejki lub tematu z aplikacji, można włączyć podział kolejka lub temat odpowiednio ustawiając [QueueDescription.EnablePartitioning] [ QueueDescription.EnablePartitioning] lub [ TopicDescription.EnablePartitioning] [ TopicDescription.EnablePartitioning] właściwości **true**. Te właściwości musi być ustawiony w tym czasie kolejki lub utworzeniu tematu. Jak wspomniano wcześniej, nie jest możliwe zmienianie te właściwości istniejącej kolejki lub temat. Na przykład:

```csharp
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

Alternatywnie można utworzyć kolejki podzielonym na partycje lub tematu w [portalu Azure] [ Azure portal] lub w programie Visual Studio. Podczas tworzenia kolejki lub tematu w portalu, **włączyć partycjonowania** opcję kolejka lub temat **Utwórz** bloku jest domyślnie zaznaczone. Tylko można wyłączyć tę opcję w jednostce warstwy standardowa; w warstwie Premium Partycjonowanie jest zawsze włączone. W programie Visual Studio, kliknij przycisk **włączyć partycjonowania** checkbox w **nową kolejkę** lub **nowy temat** okno dialogowe.

## <a name="use-of-partition-keys"></a>Użycie kluczy partycji
Gdy komunikat jest dodawanych do kolejki w podzielonym na partycje kolejka lub temat, usługi Service Bus sprawdza obecności klucza partycji. Jeśli zostanie znaleziony, wybiera fragmentu na podstawie tego klucza. Jeśli nie znajdzie klucza partycji, wybiera fragmentu na podstawie wewnętrznego algorytmu.

### <a name="using-a-partition-key"></a>Użycie klucza partycji
Niektóre scenariusze, takie jak sesji lub transakcji, wymagają wiadomości, które mają być przechowywane w określonych fragmentu. Te scenariusze wymagają użycia klucza partycji. Wszystkie komunikaty, które używają tego samego klucza partycji są przypisane do tego samego fragmentu. Jeśli fragment jest tymczasowo niedostępny, usługi Service Bus zwraca błąd.

W zależności od scenariusza inny komunikat właściwości są używane jako klucza partycji:

**Identyfikator sesji**: Jeśli wiadomość ma [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] ustaw właściwość, a następnie usługi Service Bus używa tej właściwości jako klucza partycji. Dzięki temu wszystkie komunikaty, które należą do tej samej sesji są obsługiwane przez tego samego brokera komunikatów. Dzięki temu usługa Service Bus zagwarantowanie komunikat porządkowanie oraz spójności stanów sesji.

**PartitionKey**: Jeśli wiadomość ma [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] właściwości, ale nie [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] ustaw właściwość, a następnie używa usługi Service Bus [PartitionKey] [ PartitionKey] właściwość jako klucza partycji. Jeśli wiadomość ma zarówno atrybut [SessionId] [ SessionId] i [PartitionKey] [ PartitionKey] zestaw właściwości obie właściwości muszą być takie same. Jeśli [PartitionKey] [ PartitionKey] właściwość jest ustawiona na wartość inną niż [SessionId] [ SessionId] właściwość, usługi Service Bus zwraca jest nieprawidłowa wyjątek operacji. [PartitionKey] [ PartitionKey] właściwości należy używać, gdy nadawca wysyła wiadomości transakcyjne pamiętać-session. Klucz partycji gwarantuje, że wszystkie komunikaty, które są wysyłane w ramach transakcji są obsługiwane przez tego samego brokera obsługi komunikatów.

**Identyfikator komunikatu**: Jeśli kolejka lub temat ma [QueueDescription.RequiresDuplicateDetection] [ QueueDescription.RequiresDuplicateDetection] ustawioną właściwość **true** i [ BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] lub [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] nie ustawiono właściwości, a następnie [ BrokeredMessage.MessageId] [ BrokeredMessage.MessageId] właściwość służy jako klucza partycji. (Należy pamiętać, że biblioteki Microsoft .NET i protokołu AMQP automatycznie przypisać identyfikator komunikatu, jeśli aplikacja wysyłająca nie). W takim przypadku wszystkie kopie tej samej wiadomości są obsługiwane przez tego samego brokera komunikatów. Dzięki temu usługa Service Bus do wykrywania i eliminowania duplikatów wiadomości. Jeśli [QueueDescription.RequiresDuplicateDetection] [ QueueDescription.RequiresDuplicateDetection] nie ustawiono właściwości **true**, magistrali usług nie należy wziąć pod uwagę [MessageId] [ MessageId] właściwość jako klucza partycji.

### <a name="not-using-a-partition-key"></a>Nie używa klucza partycji
W przypadku braku klucza partycji usługi Service Bus dystrybuuje wiadomości w okrężne do wszystkie fragmenty partycjonowanej kolejka lub temat. Wybrany fragment jest niedostępny, usługi Service Bus przypisuje komunikat różnych fragmentu. W ten sposób wysyłania powiedzie się niezależnie od tymczasowej niedostępności magazynie obsługi komunikatów. Jednak nie będzie osiągnąć gwarantuje kolejność, który zawiera klucz partycji.

Aby uzyskać bardziej szczegółowym omówieniem zależności między dostępności (nie klucza partycji) i spójności (przy użyciu klucza partycji), zobacz [w tym artykule](../event-hubs/event-hubs-availability-and-consistency.md). Te informacje dotyczą jednakowo partycjonowane jednostki magistrali usług i partycji usługi Event Hubs.

Aby zapewnić usługi Service Bus wystarczająco dużo czasu można umieścić w kolejce wiadomości do fragmentu innego [MessagingFactorySettings.OperationTimeout] [ MessagingFactorySettings.OperationTimeout] wartość określoną przez klienta, który wysyła wiadomości musi być większa od 15 sekund. Zalecane jest, aby ustawić [OperationTimeout] [ OperationTimeout] właściwości wartość domyślna wynosząca 60 sekund.

Należy pamiętać, że klucz partycji "PIN" komunikat do określonego fragmentu. Jeśli w magazynie obsługi komunikatów, który zawiera ten fragment jest niedostępny, usługi Service Bus zwraca błąd. W przypadku braku klucza partycji usługi Service Bus można wybrać różne fragmentu i powiedzie się. Dlatego zaleca się, że nie zostanie podany klucz partycji, chyba że jest to wymagane.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Tematy zaawansowane: transakcji za pomocą partycjonowane jednostki
Komunikaty, które są wysyłane jako część transakcji muszą określać klucz partycji. Może to być jeden z następujących właściwości: [BrokeredMessage.SessionId][BrokeredMessage.SessionId], [BrokeredMessage.PartitionKey][BrokeredMessage.PartitionKey], lub [ BrokeredMessage.MessageId][BrokeredMessage.MessageId]. Wszystkie komunikaty, które są wysyłane w ramach tej samej transakcji należy określić ten sam klucz partycji. Jeśli chcesz wysłać wiadomość bez klucza partycji w ramach transakcji, usługi Service Bus zwraca wyjątek Nieprawidłowa operacja. Próba wysyłania wielu wiadomości w tej samej transakcji o różnych kluczach partycji usługi Service Bus zwraca wyjątek Nieprawidłowa operacja. Na przykład:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Jeśli ustawiono wartość właściwości, które służą jako klucza partycji, usługi Service Bus PIN wiadomości do określonego fragmentu. Dzieje się tak, czy transakcja jest używany. Zaleca się, że nie określisz klucza partycji, jeśli nie jest konieczne.

## <a name="using-sessions-with-partitioned-entities"></a>Korzystanie z sesji z partycjonowane jednostki
Do wysyłania wiadomości transakcyjnych do tematu obsługujący sesji lub kolejki, aby wiadomość miała [BrokeredMessage.SessionId] [ BrokeredMessage.SessionId] zestawu właściwości. Jeśli [BrokeredMessage.PartitionKey] [ BrokeredMessage.PartitionKey] także określona właściwość, musi być taki sam jak [SessionId] [ SessionId] właściwości. Jeśli będą się różnić, usługi Service Bus zwraca wyjątek Nieprawidłowa operacja.

W odróżnieniu od regularne (niepartycjonowany) kolejki i tematy nie jest możliwe wysyłać wiele różnych sesji za pomocą pojedynczej transakcji. Jeśli podjęto, usługi Service Bus zwraca wyjątek Nieprawidłowa operacja. Na przykład:

```csharp
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Przekazywanie komunikatów automatyczne z partycjonowane jednostki
Usługa Service Bus obsługuje komunikat automatycznego przesyłania dalej z, aby lub między partycjonowane jednostki. Aby włączyć przekazywanie automatyczne wiadomości, ustaw [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] właściwości kolejki źródłowej lub subskrypcji. Jeśli komunikat określa klucz partycji ([SessionId][SessionId], [PartitionKey][PartitionKey], lub [MessageId] [ MessageId]), ten klucz partycji służy do jednostki docelowej.

## <a name="considerations-and-guidelines"></a>Zagadnienia i wskazówki
* **Funkcje wysokiej spójności**: Jeśli jednostki korzysta z funkcji, takich jak sesje, wykrywania duplikatów lub jawne kontrolę nad kluczem partycjonowania, a następnie operacje obsługi wiadomości zawsze są kierowane do określonego fragmenty. Jeśli występują dowolne z fragmentów dużego natężenia ruchu sieciowego lub odpowiedni magazyn jest zła, te operacje kończą się niepowodzeniem, i zmniejsza dostępności. Generalnie spójności jest nadal znacznie wyższa niż niepartycjonowany jednostki; tylko podzestaw ruchu wystąpiły problemy, a nie cały ruch. Aby uzyskać więcej informacji, zobacz [Omówienie dostępności i spójności](../event-hubs/event-hubs-availability-and-consistency.md).
* **Zarządzanie**: należy wykonać operacji, takich jak tworzenie, Update i Delete na wszystkie fragmenty jednostki. Jeśli wszystkie fragment jest zła może spowodować błędy do tych operacji. Dla operacji Get informacje takie jak liczba wiadomości musi być agregowana z wszystkie fragmenty. Jeśli wszystkie fragment jest zła, stan dostępności jednostki został zgłoszony jako ograniczone.
* **Niska scenariuszy komunikat**: dla takich scenariuszy, szczególnie w przypadku, gdy przy użyciu protokołu HTTP, może zajść potrzeba wykonania wielu operacji pobrania w celu uzyskania wszystkie komunikaty. Dla żądania odbierania fronton wykonuje receive na wszystkie fragmenty i przechowuje wszystkie odpowiedzi. Żądania odbierania kolejnych w ramach tego samego połączenia będzie korzystać z tej pamięci podręcznej i odbierania opóźnienia będzie niższa. Jednak jeśli masz wiele połączeń lub za pomocą protokołu HTTP, który ustanawia nowego połączenia dla każdego żądania. W efekcie nie ma żadnej gwarancji, że będzie on trafić w tym samym węźle. Jeśli wszystkie istniejące wiadomości są zablokowane i są przechowywane w innej frontonu, operacja receive zwraca **null**. Po pewnym czasie wygaśnięcia wiadomości i może odbierać je ponownie. Utrzymanie aktywności HTTP jest zalecane.
* **Przeglądaj/Peek wiadomości**: [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) nie zawsze zwraca liczbę wiadomości określona w [MessageCount](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_MessageCount) właściwości. Istnieją dwie typowe powody to. Jedną z przyczyn jest to, że zagregowane rozmiar kolekcji komunikatów przekracza maksymalny rozmiar 256 KB. Inną przyczyną jest to, że jeśli kolejka lub temat ma [właściwości parametr EnablePartitioning](/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnablePartitioning) ustawioną **true**, partycji może nie mieć wystarczającej ilości komunikaty, aby ukończyć żądanej liczby komunikatów. Ogólnie rzecz biorąc, jeśli aplikacja chce otrzymywać określoną liczbę wiadomości, powinna wywołać [PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) aż pobiera tej liczby wiadomości, lub nie ma żadnych więcej komunikatów do wglądu. Aby uzyskać więcej informacji, przykłady kodu, w tym temacie [QueueClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) lub [SubscriptionClient.PeekBatch](/dotnet/api/microsoft.servicebus.messaging.subscriptionclient#Microsoft_ServiceBus_Messaging_SubscriptionClient_PeekBatch_System_Int32_).

## <a name="latest-added-features"></a>Najnowsze funkcje dodane
* Dodawanie lub usuwanie reguł jest teraz obsługiwana przez partycjonowane jednostki. Inne niż niepartycjonowany jednostki, te operacje nie są obsługiwane w transakcji. 
* Protokół AMQP jest teraz obsługiwana do wysyłania i odbierania wiadomości do i z partycjonowane jednostki.
* Protokół AMQP jest teraz obsługiwana przez następujące operacje: [wysyłania partii](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_BrokeredMessage__), [odbierania partii](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_ReceiveBatch_System_Int32_), [Receive numer porządkowy](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_Receive_System_Int64_), [wgląd](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_Peek), [ Odnów blokady](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_RenewMessageLock_System_Guid_), [zaplanować komunikat](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_ScheduleMessageAsync_Microsoft_ServiceBus_Messaging_BrokeredMessage_System_DateTimeOffset_), [Anuluj zaplanowane wiadomość](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_CancelScheduledMessageAsync_System_Int64_), [Dodaj regułę](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [Usuń regułę](/dotnet/api/microsoft.servicebus.messaging.ruledescription), [Sesji odnawiania blokady](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_RenewLock), [stanu sesji zestaw](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_), [stanu sesji Get](/dotnet/api/microsoft.servicebus.messaging.messagesession#Microsoft_ServiceBus_Messaging_MessageSession_GetState), i [wyliczyć sesji](/dotnet/api/microsoft.servicebus.messaging.queueclient#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessionsAsync).

## <a name="partitioned-entities-limitations"></a>Ograniczenia partycjonowane jednostki
Obecnie Usługa Service Bus nakłada następujące ograniczenia partycjonowanej kolejek i tematów:

* Partycjonowane kolejek i tematów nie obsługują wysyłanie wiadomości, które należą do różnych sesji w ramach jednej transakcji.
* Usługa Service Bus umożliwia obecnie maksymalnie 100 partycjonowanej kolejki i tematy na przestrzeń nazw. Każdy partycjonowanej kolejka lub temat, liczy się przydziału 10 000 jednostek na przestrzeń nazw (nie ma zastosowania do warstwy Premium).

## <a name="next-steps"></a>Następne kroki
Zawiera omówienie [obsługi protokołu AMQP 1.0 dla usługi Service Bus podzielona na partycje, kolejek i tematów] [ AMQP 1.0 support for Service Bus partitioned queues and topics] Aby dowiedzieć się więcej na temat partycjonowania jednostek obsługi komunikatów. 

[Service Bus architecture]: service-bus-architecture.md
[Azure portal]: https://portal.azure.com
[QueueDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnablePartitioning
[TopicDescription.EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.topicdescription#Microsoft_ServiceBus_Messaging_TopicDescription_EnablePartitioning
[BrokeredMessage.SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId
[BrokeredMessage.PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_PartitionKey
[SessionId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_PartitionKey
[QueueDescription.RequiresDuplicateDetection]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_RequiresDuplicateDetection
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[MessagingFactorySettings.OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[OperationTimeout]: /dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout
[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[AMQP 1.0 support for Service Bus partitioned queues and topics]: service-bus-partitioned-queues-and-topics-amqp-overview.md
