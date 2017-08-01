---
title: "Omówienie architektury przetwarzania komunikatów usługi Azure Service Bus | Microsoft Docs"
description: "Opis architektury przetwarzania komunikatów i przekaźników usługi Azure Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: baf94c2d-0e58-4d5d-a588-767f996ccf7f
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 8f987d079b8658d591994ce678f4a09239270181
ms.openlocfilehash: ced46c64c1c105aa987759e05ab3680bc399f9a0
ms.contentlocale: pl-pl
ms.lasthandoff: 05/18/2017


---
# <a name="service-bus-architecture"></a>Architektura usługi Service Bus
W tym artykule opisano architekturę przetwarzania komunikatów usługi Azure Service Bus.

## <a name="service-bus-scale-units"></a>Jednostki skalowania usługi Service Bus
Usługa Service Bus jest zorganizowana według *jednostek skalowania*. Jednostka skalowania jest jednostką wdrożenia i zawiera wszystkie składniki wymagane do uruchomienia usługi. Każdy region wdraża co najmniej jedną jednostkę skalowania usługi Service Bus.

Przestrzeń nazw usługi Service Bus jest mapowana na jednostkę skalowania. Jednostka skalowania obsługuje wszystkie typy jednostek usługi Service Bus: jednostki przekaźników i komunikatów obsługiwanych przez brokera (kolejki, tematy, subskrypcje). Jednostka skalowania usługi Service Bus składa się z następujących składników:

* **Zestaw węzłów bramy.** Węzły bramy uwierzytelniają żądania przychodzące i obsługują żądania przekaźnika. Każdy węzeł bramy ma publiczny adres IP.
* **Zestaw węzłów brokera komunikatów.** Węzły brokera komunikatów przetwarzają żądania dotyczące jednostek obsługi komunikatów.
* **Jeden magazyn bramy.** Magazyn bramy przechowuje dane dla każdej jednostki zdefiniowanej w tej jednostce skalowania. Magazyn bramy jest wdrażany w oparciu o bazę danych SQL Azure.
* **Wiele magazynów obsługi komunikatów.** Usługa obsługi komunikatów przechowuje komunikaty wszystkich kolejek, tematów i subskrypcji, które są zdefiniowane w tej jednostce skalowania. Zawiera również wszystkie dane subskrypcji. O ile nie włączono opcji [partycjonowanie jednostki do obsługi komunikatów](service-bus-partitioning.md), kolejka lub temat są mapowane na jeden magazyn obsługi komunikatów. Subskrypcje są przechowywane w tym samym magazynie obsługi komunikatów jako ich temat nadrzędny. Z wyjątkiem [obsługi komunikatów w wersji Premium](service-bus-premium-messaging.md) usługi Service Bus, magazyny obsługi komunikatów są wdrażane na podstawie baz danych SQL Azure.

## <a name="containers"></a>Kontenery
Każda jednostka obsługi komunikatów ma przypisany określony kontener. Kontener jest konstrukcją logiczną, która używa dokładnie jednego magazynu obsługi komunikatów do przechowywania wszystkich odpowiednich danych dla tego kontenera. Każdy kontener jest przypisany do węzła brokera obsługi komunikatów. Zazwyczaj kontenerów jest więcej niż węzłów brokera obsługi komunikatów. W związku z tym każdy węzeł brokera obsługi komunikatów ładuje wiele kontenerów. Dystrybucja kontenerów do węzła brokera obsługi komunikatów jest zorganizowana w taki sposób, że wszystkie węzły brokera obsługi komunikatów są równo obciążone. Jeśli wzorzec obciążenia zmienia się (na przykład jeden z kontenerów staje się bardzo zajęty) lub jeśli węzeł brokera obsługi komunikatów staje się tymczasowo niedostępny, kontenery są redystrybuowane między węzły brokera obsługi komunikatów.

## <a name="processing-of-incoming-messaging-requests"></a>Przetwarzanie przychodzących żądań obsługi komunikatów
Gdy klient wysyła żądanie do usługi Service Bus, usługa Azure Load Balancer kieruje je do dowolnego z węzłów bramy. Węzeł bramy autoryzuje żądanie. Jeśli żądanie dotyczy jednostki obsługi komunikatów (kolejki, tematu, subskrypcji), węzeł bramy wyszukuje jednostkę w magazynie bramy i określa, w którym magazynie obsługi komunikatów znajduje się ta jednostka. Następnie wyszukuje węzeł brokera obsługi komunikatów aktualnie obsługujący ten kontener i wysyła żądanie do tego węzła brokera obsługi komunikatów. Węzeł brokera obsługi komunikatów przetwarza żądanie i aktualizuje stan jednostki w magazynie kontenera. Węzeł brokera obsługi komunikatów wysyła następnie odpowiedź do węzła bramy wysyłającego z kolei odpowiednią odpowiedź do klienta, który wysłał początkowe, oryginalne żądanie.

![Przetwarzanie przychodzących żądań obsługi komunikatów](./media/service-bus-architecture/ic690644.png)

## <a name="processing-of-incoming-relay-requests"></a>Przetwarzanie przychodzących żądań przekaźnika
Gdy klient wysyła żądanie do usługi [Azure Relay](/azure/service-bus-relay/), usługa Azure Load Balancer kieruje je do dowolnego z węzłów bramy. Jeśli żądanie jest żądaniem nasłuchiwania, węzeł bramy tworzy nowy przekaźnik. Jeśli żądanie jest żądaniem połączenia z określonym przekaźnikiem, węzeł bramy przesyła żądanie połączenia do węzła bramy, który jest właścicielem przekaźnika. Węzeł bramy będący właścicielem przekaźnika wysyła żądanie spotkania do nasłuchującego klienta, prosząc odbiornik o utworzenie tymczasowego kanału do węzła bramy, który odebrał żądanie połączenia.

Po nawiązaniu połączenia przekaźnika klienci mogą wymieniać komunikaty za pośrednictwem węzła bramy, który jest używany na potrzeby spotkania.

![Przetwarzanie przychodzących żądań przekaźnika WCF](./media/service-bus-architecture/ic690645.png)

## <a name="next-steps"></a>Następne kroki
Teraz, po zapoznaniu się z architekturą usługi Service Bus, skorzystaj z następujących linków, aby uzyskać więcej informacji:

* [Omówienie obsługi komunikatów w usłudze Service Bus](service-bus-messaging-overview.md)
* [Omówienie usługi Azure Relay](../service-bus-relay/relay-what-is-it.md)
* [Podstawy usługi Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Rozwiązanie do obsługi komunikatów kolejek korzystające z funkcji kolejek usługi Service Bus](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)



