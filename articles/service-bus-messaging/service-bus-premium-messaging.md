---
title: "Omówienie warstw cenowych Premium i Standardowa komunikatów usługi Service Bus | Microsoft Docs"
description: "Komunikaty usługi Service Bus w warstwie Premium i Standardowa"
services: service-bus-messaging
documentationcenter: .net
author: djrosanova
manager: timlt
editor: 
ms.assetid: e211774d-821c-4d79-8563-57472d746c58
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/18/2017
ms.author: darosa;sethm;jotaub
translationtype: Human Translation
ms.sourcegitcommit: f223ea7ac168d3c800b6ba664b31ab66b382d6cc
ms.openlocfilehash: 2657965ff3ee028263f9ef0c48024fe1839eee6e


---
# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Warstwy Premium i Standardowa komunikatów usługi Service Bus
Komunikaty usługi Service Bus, w tym jednostki, takie jak kolejki i tematy, stanowią połączenie możliwości obsługi komunikatów dla przedsiębiorstw oraz zaawansowanej semantyki publikowania/subskrybowania w skali chmury. Obsługa komunikatów usługi Service Bus pełni rolę szkieletu komunikacyjnego dla wielu zaawansowanych rozwiązań w chmurze.

Warstwa *Premium* komunikatów usługi Service Bus stanowi odpowiedź na częste żądania klientów dotyczące skali, wydajności i dostępności dla aplikacji o krytycznym znaczeniu. Mimo że zestawy funkcji są niemal identyczne, te dwie warstwy komunikatów usługi Service Bus są przeznaczone do różnych zastosowań.

W poniższej tabeli wyróżniono pewne ogólne różnice.

| Premium | Standardowa (Standard) |
| --- | --- |
| Wysoka przepływność |Zmienna przepływność |
| Przewidywalna wydajność |Zmienne opóźnienie |
| Stałe ceny |Zmienne ceny i płatność zgodnie z rzeczywistym użyciem |
| Możliwość skalowania obciążenia |Nie dotyczy |
| Rozmiar komunikatu do 1 MB |Rozmiar komunikatu do 256 KB |

**Obsługa komunikatów usługi Service Bus Premium** zapewnia izolację zasobów w warstwie procesora i pamięci, dlatego obciążenia poszczególnych klientów są od siebie odizolowane. Ten kontener zasobów jest nazywany *jednostką obsługi komunikatów*. Każda przestrzeń nazw w warstwie Premium ma przydzieloną co najmniej jedną jednostkę obsługi komunikatów. Możesz kupić 1, 2 lub 4 jednostki obsługi komunikatów dla każdej przestrzeni nazw usługi Service Bus w warstwie Premium. Pojedyncze obciążenie lub jednostka może obejmować wiele jednostek obsługi komunikatów, a ich liczbę można dowolnie zmieniać, chociaż opłaty są naliczane w cyklu 24-godzinnym lub codziennie. Pozwala to uzyskać przewidywalną i powtarzalną wydajność dla rozwiązania opartego na usłudze Service Bus.

Poprawa dotyczy nie tylko przewidywalności i dostępności, ale również szybkości działania. Komunikaty usługi Service Bus w warstwie Premium są oparte na aparacie magazynu wprowadzonym w usłudze [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Dzięki komunikatom w warstwie Premium maksymalna szybkość działania jest znacznie wyższa niż w przypadku warstwy Standardowa.

## <a name="premium-messaging-technical-differences"></a>Różnice techniczne dotyczące komunikatów w warstwie Premium
Poniżej przedstawiono niektóre różnice między komunikatami w warstwie Premium i Standardowa.

### <a name="partitioned-queues-and-topics"></a>Partycjonowane kolejki i tematy
Partycjonowane kolejki i tematy są obsługiwane przez komunikaty w warstwie Premium, ale nie działają tak samo jak w przypadku komunikatów usługi Service Bus w warstwie Standardowa i Podstawowa. Komunikaty w warstwie Premium nie używają bazy danych SQL jako magazynu danych i nie występuje już potencjalne konkurowanie o zasoby typowe dla platformy współdzielonej. W związku z tym partycjonowanie nie jest konieczne, aby zapewnić wydajność. Ponadto liczba partycji została zmieniona z szesnastu partycji w przypadku komunikatów w warstwie Standardowa do dwóch partycji w warstwie Premium. Istnienie dwóch partycji gwarantuje dostępność i jest to bardziej odpowiednia liczba dla środowiska uruchomieniowego w warstwie Premium. Aby uzyskać więcej informacji na temat partycjonowania, zobacz [Partitioned queues and topics](service-bus-partitioning.md) (Partycjonowane kolejki i tematy).

### <a name="express-entities"></a>Jednostki ekspresowe
Ze względu na fakt, że komunikaty warstwy Premium działają w całkowicie odizolowanym środowisku uruchomieniowym, jednostki ekspresowe nie są obsługiwane obszarach nazw w warstwie Premium. Aby uzyskać więcej informacji o funkcji jednostek ekspresowych, zobacz opis właściwości [QueueDescription.EnableExpress](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress).

## <a name="get-started-with-premium-messaging"></a>Wprowadzenie do obsługi komunikatów Premium

Rozpoczęcie pracy z obsługą komunikatów Premium jest proste, a proces jest podobny, jak w przypadku standardowej obsługi komunikatów. Rozpocznij od [utworzenia przestrzeni nazw](service-bus-create-namespace-portal.md). W obszarze **Warstwa cenowa** wybierz pozycję **Premium**.

![create-premium-namespace][create-premium-namespace]

Możesz również tworzyć [przestrzenie nazw Premium za pomocą szablonów usługi Azure Resource Manager](https://azure.microsoft.com/en-us/resources/templates/101-servicebus-pn-ar/).


## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat obsługi komunikatów usługi Service Bus, zobacz następujące tematy.

* [Introducing Azure Service Bus Premium messaging](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/) (Wprowadzenie do komunikatów usługi Service Bus w warstwie Premium) — wpis w blogu
* [Introducing Azure Service Bus Premium messaging](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging) (Wprowadzenie do komunikatów usługi Service Bus w warstwie Premium) — Channel9
* [Omówienie obsługi komunikatów w usłudze Service Bus](service-bus-messaging-overview.md)
* [Jak używać kolejek usługi Service Bus](service-bus-dotnet-get-started-with-queues.md)

<!--Image references-->

[create-premium-namespace]: ./media/service-bus-premium-messaging/select-premium-tier.png



<!--HONumber=Jan17_HO3-->


