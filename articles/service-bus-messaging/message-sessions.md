---
title: "Azure sesje komunikatów usługi Service Bus | Dokumentacja firmy Microsoft"
description: "Obsługa sekwencji komunikatów usługi Azure Service Bus z sesji."
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2017
ms.author: sethm
ms.openlocfilehash: c6441d2119518e89a869ee52e5f0b80450ae2bbe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="message-sessions--first-in-first-out-fifo"></a>Sesje komunikatów / najpierw, — pierwszy na wyjściu (FIFO) 

Sesje usługi Service Bus Włącz obsługę wspólnego i uporządkowanych niepowiązany sekwencji powiązane wiadomości. Do osiągnięcia gwarancji FIFO w usłudze Service Bus, musisz użyć sesji. Magistrala usług nie jest przetestowanego o rodzaju relacji między wiadomości i również nie definiuje określonego modelu określania, gdy rozpoczyna się lub kończy w sekwencji komunikatów.

Wszystkich nadawców można utworzyć sesji podczas przesyłania komunikatów do tematu lub kolejki przez ustawienie [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId) broker właściwości niektórych zdefiniowanym przez aplikację Unikatowy identyfikator sesji. Na poziomie protokołu AMQP 1.0, ta wartość jest mapowany na *identyfikator grupy* właściwości.

Na kolejek obsługujących sesji lub subskrypcji, sesje wejścia istnienia po co najmniej jeden komunikat z sesją [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId). Gdy sesja istnieje, nie jest dostępna ustalonym ani interfejsu API dla czas wygaśnięcia sesji lub zniknie. Teoretycznie obecnie sesji do następnej wiadomości w czasie roku, może zostać odebrany komunikat i jeśli **SessionId** zgodny, sesja jest taki sam, z punktu widzenia usługi Service Bus.

Zwykle jednak aplikacja ma wyczyść pojęcie Jeżeli zestaw komunikatów powiązanych zaczyna się i kończy się; Jednak usługi Service Bus nie określa żadnych szczególnych zasad.

Przykład sposobu odróżniać sekwencji transferu plików jest ustalenie **etykiety** właściwości do pierwszej wiadomości **start**, pośrednie wiadomości do **zawartości**, i na ostatnich wiadomości **zakończenia**. Względne położenie zawartości wiadomości może być obliczona jako bieżący komunikat *SequenceNumber* delta **start** komunikatu *SequenceNumber*.

Funkcja sesji w usługi Service Bus umożliwia określonej operacji, odbierania w formie [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) w języku C# i interfejsów API języka Java. Włącz tę funkcję, ustawiając [element requiresSession](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) właściwości kolejki lub subskrypcji, za pomocą usługi Azure Resource Manager lub przez ustawienie flagi w portalu. Jest to wymagane, przed podjęciem próby użycia powiązanych operacji interfejsu API.

W portalu flaga jest ustawiona z poniższe pole wyboru:

![][2]

Interfejsy API dla sesji istnieje na klientach kolejki i subskrypcji. Jest konieczne modelu, gdzie możesz kontrolować podczas odbierania sesji i komunikatów i obsługi na podstawie modelu, podobnie jak *OnMessage*, że ukrywa złożoność zarządzania odbieranie w pętli.

## <a name="session-features"></a>Funkcje sesji

Umożliwia równoczesnych demultipleksowanie strumieni komunikatów przeplotem gwarantując gwarantujących uporządkowanego dostarczenia.

![][1]

A [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) odbiornika jest tworzony przez klienta, akceptując sesji. Imperatively, wywołań klienta [QueueClient.AcceptMessageSession](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesession#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSession) lub [QueueClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesessionasync#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSessionAsync) w języku C#. W modelu reaktywne wywołania zwrotnego rejestruje obsługi sesji, zgodnie z opisem później.

Gdy [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) jest akceptowany i gdy jest on przechowywany przez klienta, którego klient utrzymuje wyłącznej blokady wszystkie komunikaty z tej sesji [SessionId](/en-us/dotnet/api/microsoft.servicebus.messaging.messagesession.sessionid?view=azureservicebus-4.1.1#Microsoft_ServiceBus_Messaging_MessageSession_SessionId) istniejącej kolejki lub subskrypcji, a także na wszystkie komunikaty z tym **SessionId** który nadal odbierane podczas sesji jest przechowywany.

Zwolnienia blokady podczas **Zamknij** lub **CloseAsync** są nazywane lub wygaśnięcia blokady w przypadkach, w których aplikacja jest to możliwe. Blokady sesji powinien być traktowany jak blokady na wyłączność pliku, co oznacza, że aplikacji należy zamknąć sesji natychmiast nie należy go lub nie oczekuje żadnych dalszych komunikatów.

Gdy wiele równoczesnych odbiorników ściąganie danych z kolejki, komunikaty należących do określonej sesji są wysyłane do określonego odbiornika, który aktualnie posiada blokady dla tej sesji. Z tej operacji strumienia komunikatów przeplotem znajdującej się w jednym kolejki lub subskrypcji cofnąć jest prawidłowo multipleksowany do różnych odbiorników i odbiorcy tych można również na żywo na komputerach z innego klienta, ponieważ Zarządzanie blokadą się stanie po stronie serwera, wewnątrz Usługa Service Bus.

Kolejka jest jednak nadal kolejki: brak jest losowe dostępu. Wiele równoczesnych odbiorników poczekaj, aby zaakceptować określonej sesji lub zaczekaj na wiadomości z określonej sesji, jeśli ma wiadomości w górnej części kolejki należących do sesji, która odbiornik nie zostało jeszcze zażądane, wysyłki przytrzymaj, dopóki nie odbiornika sesji twierdzi, że sesji.

Na poprzedniej ilustracji przedstawiono trzy odbiornikami równoczesnych sesji, które aktywnie należy wykonać wiadomości z kolejki dla każdego odbiornika postęp. Poprzedniej sesji z *SessionId*= 4 nie ma żadnych aktywnych, będący właścicielem klienta, co oznacza, że żadne komunikaty są dostarczane innym osobom do chwili podjęcia tej wiadomości przez nowo utworzony, będącej właścicielem, jeśli odbiornik sesji.

Podczas wyświetlanych może być ograniczający, proces jednego odbiorcę może obsłużyć wiele równoczesnych sesji, szczególnie w przypadku, gdy są one zapisywane z kodem ściśle asynchroniczne; juggling kilka dozen równoczesnych sesji jest skutecznie automatycznie z modelem wywołania zwrotnego.

Strategii obsługi wielu równoczesnych sesji, zgodnie z którymi każdej sesji tylko sporadycznie odbiera komunikaty, obsługi usunąć za jakiś czas bezczynności sesji i wznowić przetwarzania podczas sesji jest traktowany jako dociera do następnej sesji.

Blokadzie sesją odbiornika sesji jest parasola blokad komunikatu używany przez *blokady peek* tryb rozliczeń. Odbiornik nie może jednocześnie mieć dwa komunikaty "w locie", ale komunikaty muszą być przetwarzane w kolejności. Nowy komunikat można uzyskać tylko w przypadku, gdy poprzednie wiadomości została wykonana lub lettered wiadomości. Porzucanie przyczyny wiadomości, takie same wiadomości ma być obsługiwana ponownie przy następnej operacji odbioru.

## <a name="message-session-state"></a>Komunikat stanu sesji

Jeśli przepływy pracy są przetwarzane w dużej skali, systemy chmur wysokiej dostępności, program obsługi przepływu pracy skojarzonego z określoną sesją musi być w stanie odzyskać z nieoczekiwanych awarii i istnieć możliwość wznawiania pracy częściowo ukończone na innych proces lub komputera, z którym rozpoczęcia pracy.

Funkcji stanu sesji umożliwia adnotacji zdefiniowanym przez aplikację wiadomości sesji wewnątrz brokera, tak, aby stan przetwarzania zarejestrowane względem tej sesji staje się dostępne natychmiast po sesji są uzyskiwane przez nowe procesora.

Z perspektywy usługi Service Bus komunikat stanu sesji jest przezroczystości obiektu binarnego, które można przechowywać danych o rozmiarze jeden komunikat, który jest 256 KB dla standardowych magistrali usługi i 1 MB dla usługi Service Bus w warstwie Premium. Stan przetwarzania względem sesji może być przechowywany wewnątrz stanu sesji lub stan sesji może wskazywać niektórych lokalizacji magazynu lub rekordu bazy danych, który zawiera takie informacje.

Interfejsy API do zarządzania stanu sesji [metoda SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) i [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState), można znaleźć w [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) obiektu w języku C# i interfejsów API języka Java. Zwraca wartość sesji, która była wcześniej bez stanu sesji **null** odwołania dla **GetState**. Wyczyszczenie stanu sesji wcześniej ustawiony wykonuje się za pomocą [SetState(null)](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_).

Wszystkie istniejące sesje w kolejce lub subskrypcji mogą być wyliczane przy użyciu **SessionBrowser** metody interfejsu API języka Java i przy [GetMessageSessions](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessions) na [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) i [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) w kliencie programu .NET.

Stan sesji przechowywane w kolejce lub w się, że subskrypcja, liczy się przydział magazynowania tej jednostki. Po zakończeniu działania aplikacji przy użyciu sesji w związku z tym zaleca dla aplikacji, aby wyczyścić stanu zachowanych Aby uniknąć kosztów zarządzania zewnętrznego.

## <a name="next-steps"></a>Następne kroki

- [Pełny przykład](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.Azure.ServiceBus/BasicSessionSendReceiveUsingQueueClient) wysyłać i odbierać komunikaty oparte na sesji z usługi Service Bus kolejki przy użyciu biblioteki .NET Standard.
- [Przykładowe](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/Sessions) używający klienta .NET Framework do obsługi komunikatów obsługujący sesji. 

Aby dowiedzieć się więcej o komunikatów usługi Service Bus, zobacz następujące tematy:

* [Podstawy usługi Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Kolejki, tematy i subskrypcje usługi Service Bus](service-bus-queues-topics-subscriptions.md)
* [Wprowadzenie do kolejek usługi Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak używać tematów i subskrypcji usługi Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/message-sessions/sessions.png
[2]: ./media/message-sessions/queue-sessions.png