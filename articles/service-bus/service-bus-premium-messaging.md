<properties
    pageTitle="Omówienie warstw cenowych Premium i Standardowa komunikatów usługi Service Bus | Microsoft Azure"
    description="Komunikaty usługi Service Bus w warstwie Premium i Standardowa"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/16/2016"
    ms.author="darosa;sethm"/>

# Warstwy Premium i Standardowa komunikatów usługi Service Bus 

Komunikaty obsługiwane przez brokera usługi Service Bus, w tym jednostki komunikatów, takie jak kolejki i tematy, stanowią połączenie możliwości obsługi komunikatów dla przedsiębiorstw oraz zaawansowanej semantyki publikowania/subskrybowania w skali chmury. Komunikaty obsługiwane przez brokera usługi Service Bus pełnią rolę szkieletu komunikacyjnego dla wielu zaawansowanych rozwiązań w chmurze.

Warstwa *Premium* komunikatów usługi Service Bus stanowi odpowiedź na częste żądania klientów dotyczące skali, wydajności i dostępności dla aplikacji o krytycznym znaczeniu. Mimo że zestawy funkcji są niemal identyczne, te dwie warstwy komunikatów usługi Service Bus są przeznaczone do różnych zastosowań.

W poniższej tabeli wyróżniono pewne ogólne różnice.

| Premium                               | Standardowa                       |
|---------------------------------------|--------------------------------|
| Wysoka przepustowość                       | Zmienna przepustowość            |
| Przewidywalna wydajność               | Zmienne opóźnienie               |
| Przewidywalne ceny                   | Zmienne ceny i płatność zgodnie z rzeczywistym użyciem |
| Możliwość skalowania obciążenia | Nie dotyczy                            |
| Rozmiar komunikatu > 256 KB                  | Rozmiar komunikatu równy 256 KB          |

**Komunikaty usługi Azure Service Bus Premium** zapewniają izolację zasobów w warstwie procesora i pamięci, dlatego obciążenia poszczególnych klientów są od siebie odizolowane. Ten kontener zasobów jest nazywany *jednostką obsługi komunikatów*. Każda przestrzeń nazw w warstwie Premium ma przydzieloną co najmniej jedną jednostkę obsługi komunikatów. Możesz kupić 1, 2 lub 4 jednostki obsługi komunikatów dla każdej przestrzeni nazw usługi Service Bus w warstwie Premium. Pojedyncze obciążenie lub jednostka może obejmować wiele jednostek obsługi komunikatów, a ich liczbę można dowolnie zmieniać, chociaż opłaty są naliczane w cyklu 24-godzinnym lub codziennie. Pozwala to uzyskać przewidywalną i powtarzalną wydajność dla rozwiązania opartego na usłudze Service Bus.

Poprawa dotyczy nie tylko przewidywalności i dostępności, ale również szybkości działania. Komunikaty usługi Service Bus w warstwie Premium są oparte na aparacie magazynu wprowadzonym w usłudze [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Dzięki komunikatom w warstwie Premium maksymalna szybkość działania jest znacznie wyższa niż w przypadku warstwy Standardowa.

## Różnice techniczne dotyczące komunikatów w warstwie Premium

Poniżej przedstawiono niektóre różnice między komunikatami w warstwie Premium i Standardowa.

### Partycjonowane jednostki

Partycjonowane jednostki są obsługiwane przez komunikaty w warstwie Premium, ale nie działają tak samo, jak w przypadku komunikatów usługi Service Bus w warstwie Standardowa i Podstawowa. Komunikaty w warstwie Premium nie używają bazy danych SQL jako magazynu danych i nie występuje już potencjalne konkurowanie o zasoby typowe dla platformy współdzielonej. W związku z tym partycjonowanie nie jest konieczne. Ponadto liczba partycji została zmieniona z 16 partycji w przypadku komunikatów w warstwie Standardowa do dwóch partycji w warstwie Premium. Istnienie 2 partycji gwarantuje dostępność i jest to bardziej odpowiednia liczba dla środowiska uruchomieniowego w warstwie Premium. Aby uzyskać więcej informacji na temat partycjonowania, zobacz [Partitioned Messaging Entities](service-bus-partitioning.md) (Partycjonowane jednostki do obsługi komunikatów).

### Jednostki ekspresowe

Ze względu na działanie w całkowicie odizolowanym środowisku uruchomieniowym jednostki ekspresowe nie są już potrzebne w przypadku komunikatów w warstwie Premium. W związku z tym jednostki ekspresowe nie są obsługiwane w przestrzeniach nazw warstwy Premium. Aby uzyskać więcej informacji o funkcji jednostek ekspresowych, zobacz opis właściwości [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx).

## Następne kroki

Aby dowiedzieć się więcej na temat obsługi komunikatów usługi Service Bus, zobacz następujące tematy.

- [Introducing Azure Service Bus Premium messaging (Wprowadzenie do komunikatów usługi Service Bus w warstwie Premium) — wpis w blogu](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Introducing Azure Service Bus Premium messaging (Wprowadzenie do komunikatów usługi Service Bus w warstwie Premium) — Channel9](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Omówienie obsługi komunikatów w usłudze Service Bus](service-bus-messaging-overview.md)
- [Omówienie architektury usługi Azure Service Bus](service-bus-fundamentals-hybrid-solutions.md)
- [Jak używać kolejek usługi Service Bus](service-bus-dotnet-how-to-use-queues.md)



<!--HONumber=Jun16_HO2-->


