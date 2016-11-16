---
title: "Co to jest usługa Azure Event Hubs? | Microsoft Docs"
description: "Omówienie i opis usługi Azure Event Hubs"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 4391d750-5bbe-456d-9091-b416127bc754
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fcc3e2cb76a06607d0bfbbad810968b70d67e4f4


---
# <a name="what-is-azure-event-hubs"></a>Co to jest usługa Azure Event Hubs?
Azure Event Hubs to wysoce skalowalna usługa transferu danych przychodzących, która może obsługiwać miliony zdarzeń na sekundę, dzięki czemu możliwe jest przetwarzanie i analizowanie olbrzymich ilości danych wytworzonych przez podłączone urządzenia i aplikacje. Usługa Event Hubs działa jako „drzwi wejściowe” dla potoku zdarzeń, a po pobraniu danych do Centrum zdarzeń można je przekształcać i przechowywać za pomocą dowolnego dostawcy analiz w czasie rzeczywistym lub za pomocą adapterów przetwarzania wsadowego i magazynowania. Usługa Event Hubs oddziela wytwarzanie strumienia zdarzeń od użycia tych zdarzeń, dzięki czemu odbiorcy zdarzeń mogą uzyskiwać dostęp do zdarzeń zgodnie z własnym harmonogramem. Aby uzyskać więcej informacji i danych technicznych, zobacz [Przegląd usługi Event Hubs](event-hubs-overview.md).

## <a name="event-hubs-capabilities"></a>Możliwości usługi Event Hubs
Event Hubs to usługa służąca do przetwarzania zdarzeń, która zapewnia funkcje przetwarzania zdarzeń i danych telemetrycznych w bardzo dużej skali, z małymi opóźnieniami i wysoką niezawodnością. Ta usługa jest szczególnie przydatna w przypadku:

* instrumentacji aplikacji
* przetwarzania czynności użytkownika lub przepływów pracy
* scenariuszy Internetu rzeczy (IoT)

Niektóre inne kluczowe możliwości usługi Event Hubs obejmują śledzenie zachowania w aplikacjach mobilnych, informacje o ruchu z farmy serwerów sieci Web, przechwytywanie zdarzeń w grach na konsole lub dane telemetryczne zbierane z maszyn przemysłowych bądź podłączonych pojazdów.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać szczegółowe informacje o usłudze Event Hubs, zobacz następujące tematy.

* [Przegląd usługi Event Hubs](event-hubs-overview.md)
* [Przewodnik programowania w usłudze Event Hubs](event-hubs-programming-guide.md)
* [Dostępność i pomoc techniczna usługi Event Hubs — często zadawane pytania](event-hubs-availability-and-support-faq.md)
* Wprowadzenie do [usługi Event Hubs — samouczek][usługi Event Hubs — samouczek]
* Kompletna [przykładowa aplikacja korzystająca z usługi Event Hubs][przykładowa aplikacja korzystająca z usługi Event Hubs]

[usługi Event Hubs — samouczek]: event-hubs-csharp-ephcs-getstarted.md
[przykładowa aplikacja korzystająca z usługi Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097



<!--HONumber=Nov16_HO2-->


