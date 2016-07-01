<properties
    pageTitle="Co to jest usługa Azure Event Hubs? | Microsoft Azure"
    description="Omówienie i opis usługi Azure Event Hubs"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/12/2016"
    ms.author="sethm;nberdy"/>

# Co to jest usługa Azure Event Hubs?

Azure Event Hubs to wysoce skalowalna usługa transferu danych przychodzących, która może obsługiwać miliony zdarzeń na sekundę, dzięki czemu możliwe jest przetwarzanie i analizowanie olbrzymich ilości danych wytworzonych przez podłączone urządzenia i aplikacje. Usługa Event Hubs działa jako „drzwi wejściowe” dla potoku zdarzeń, a po pobraniu danych do Centrum zdarzeń można je przekształcać i przechowywać za pomocą dowolnego dostawcy analiz w czasie rzeczywistym lub za pomocą adapterów przetwarzania wsadowego i magazynowania. Usługa Event Hubs oddziela wytwarzanie strumienia zdarzeń od użycia tych zdarzeń, dzięki czemu odbiorcy zdarzeń mogą uzyskiwać dostęp do zdarzeń zgodnie z własnym harmonogramem.

## Możliwości usługi Event Hubs

Event Hubs to usługa służąca do przetwarzania zdarzeń, która dostarcza zdarzenia i dane telemetryczne do chmury w bardzo dużej skali, z małymi opóźnieniami i wysoką niezawodnością. Ta usługa jest szczególnie przydatna w przypadku:

- instrumentacji aplikacji
- przetwarzania czynności użytkownika lub przepływów pracy
- scenariuszy Internetu rzeczy (IoT)

Niektóre inne kluczowe możliwości usługi Event Hubs obejmują śledzenie zachowania w aplikacjach mobilnych, informacje o ruchu z farmy serwerów sieci Web, przechwytywanie zdarzeń w grach na konsole lub dane telemetryczne zbierane z maszyn przemysłowych bądź podłączonych pojazdów.

W przeciwieństwie do [kolejek i tematów usługi Service Bus](../service-bus/service-bus-messaging-overview.md), usługa Event Hubs koncentruje się na dostarczaniu obsługi strumienia komunikatów na dużą skalę. W związku z tym możliwości usługi Event Hubs różnią się od funkcji tematów usługi Service Bus na przykład tym, że są one silnie ukierunkowane pod kątem scenariuszy wysokiej wydajności i przetwarzania zdarzeń. W takiej sytuacji usługa Event Hubs nie implementuje niektórych możliwości obsługi komunikatów, które są dostępne w przypadku funkcji [tematy](../service-bus/service-bus-fundamentals-hybrid-solutions.md#topics). Jeśli takie możliwości są wymagane, tematy pozostają optymalnym wyborem.

## Następne kroki

Aby uzyskać szczegółowe informacje o usłudze Event Hubs, zobacz następujące tematy.

- [Przegląd usługi Event Hubs](event-hubs-overview.md)
- [Przewodnik programowania w usłudze Event Hubs](event-hubs-programming-guide.md)
- [Dostępność i pomoc techniczna usługi Event Hubs — często zadawane pytania](event-hubs-availability-and-support-faq.md)
- Rozpocznij pracę od [samouczka dotyczącego usługi Event Hubs][]
- Kompletna [przykładowa aplikacja korzystająca z usługi Event Hubs][]

[samouczka dotyczącego usługi Event Hubs]: event-hubs-csharp-ephcs-getstarted.md
[przykładowa aplikacja korzystająca z usługi Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097


<!--HONumber=Jun16_HO2-->


