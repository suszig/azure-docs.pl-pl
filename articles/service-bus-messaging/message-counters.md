---
title: "Liczba komunikatów usługi Azure Service Bus | Dokumentacja firmy Microsoft"
description: "Pobierz liczbę komunikatów usługi Azure Service Bus."
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
ms.date: 10/03/2017
ms.author: sethm
ms.openlocfilehash: f5d2aa551bbe77a66459907cf5cd1313bb907981
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="message-counters"></a>Liczniki komunikatów

Liczba komunikatów w subskrypcji i kolejek usługi za pomocą usługi Azure Resource Manager i usługi Service Bus można pobrać [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) interfejs API programu .NET Framework SDK.

Przy użyciu programu PowerShell można pobrać licznik w następujący sposób:

```powershell
(Get-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue).CountDetails
```

## <a name="message-count-details"></a>Szczegóły liczba wiadomości

Znajomość liczby aktywnych wiadomości jest przydatne do określenia, czy kolejka buduje zaległości, która wymaga więcej zasoby do przetworzenia niż aktualnie został wdrożony co. Poniższe szczegóły licznika są dostępne w [MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) klasy:

-   [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ActiveMessageCount): wiadomości w kolejce lub subskrypcji, które są aktywnie stanu i są gotowe do dostarczenia.
-   [DeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.deadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_DeadLetterMessageCount): wiadomości w kolejce wiadomości utraconych.
-   [ScheduledMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.scheduledmessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ScheduledMessageCount): wiadomości w stanie Zaplanowane.
-   [TransferDeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transferdeadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferDeadLetterMessageCount): wiadomości nie można przenieść do innej kolejki lub tematu i została przeniesiona do kolejki utraconych wiadomości transferu.
-   [TransferMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transfermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferMessageCount): wiadomości oczekujące na wysłanie przenieść do innej kolejki lub tematu.

Jeśli aplikacja chce zasoby oparte na długość kolejki są skalowane, go należy to zrobić z bardzo zmierzona tempie. Nabycie liczniki komunikatów jest kosztowna operacja wewnątrz brokera komunikatów, a jej wykonanie często bezpośrednio i negatywnie wpływa na wydajność jednostki.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o komunikatów usługi Service Bus, zobacz następujące tematy:

* [Podstawy usługi Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Kolejki, tematy i subskrypcje usługi Service Bus](service-bus-queues-topics-subscriptions.md)
* [Wprowadzenie do kolejek usługi Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak używać tematów i subskrypcji usługi Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)