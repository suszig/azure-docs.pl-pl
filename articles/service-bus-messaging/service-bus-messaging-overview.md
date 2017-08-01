---
title: "Omówienie obsługi komunikatów w usłudze Azure Service Bus | Microsoft Docs"
description: "Opis obsługi komunikatów w usłudze Service Bus oraz usługi Azure Relay"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 05/25/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: 3a4382979dd6e18c0e94b4a989bb8289882eeb89
ms.contentlocale: pl-pl
ms.lasthandoff: 05/26/2017


---
# <a name="service-bus-messaging-flexible-data-delivery-in-the-cloud"></a>Obsługa komunikatów w usłudze Service Bus: elastyczne dostarczanie danych w chmurze
Usługa Microsoft Azure Service Bus to niezawodna usługa dostarczania informacji. Celem tej usługi jest ułatwienie komunikacji. Gdy dwie lub więcej stron przystępuje do wymiany informacji, niezbędne jest rozwiązanie do komunikacji. Usługa Service Bus to mechanizm obsługiwany przez brokera lub mechanizm komunikacji innej firmy. Przypomina to usługi pocztowe w świecie rzeczywistym. Usługi pocztowe bardzo ułatwiają wysyłanie różnego rodzaju listów i przesyłek z różną gwarancją dostarczania w dowolnym miejscu na świecie.

Podobnie jak listy dostarczane za pomocą usług pocztowych, usługa Service Bus jest elastyczną usługą dostarczania informacji zarówno od nadawcy, jak i adresata. Usługa obsługi komunikatów gwarantuje dostarczenie informacji, nawet jeśli obie strony nigdy nie są w trybie online w tym samym czasie lub jeśli nie są dostępne dokładnie w tym samym czasie. W ten sposób obsługa komunikatów przypomina wysyłanie listu, podczas gdy komunikacja nieobsługiwana przez brokera jest podobna do wykonania rozmowy telefonicznej (lub do rozmów telefonicznych, jakie były kiedyś — przed połączeniami oczekującymi i identyfikatorem dzwoniącego, które bardziej przypominają komunikaty obsługiwane przez brokera).

Nadawca komunikatu może również wymagać różnych właściwości dostarczania, w tym transakcji, wykrywania duplikatów, wygaśnięcia działającego w tle lub tworzenia partii. Te wzorce również mają cechy wspólne z usługami pocztowymi: powtórne dostarczenie, wymagany podpis, zmiana adresu lub wycofanie.

Usługa Service Bus obsługuje dwa różne wzorce obsługi komunikatów: usługa *Azure Relay* i *obsługa komunikatów w usłudze Service Bus*.

## <a name="azure-relay"></a>Azure Relay
Składnik [WCF Relay](../service-bus-relay/relay-what-is-it.md) usługi Azure Relay jest scentralizowaną (ale wysoce zrównoważoną pod względem obciążenia) usługą, która obsługuje wiele różnych protokołów transportu i standardów usług internetowych. Obejmuje to usługi SOAP, WS-*, a nawet REST. [Usługa przekazywania](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) zapewnia różnorodność opcji łączności przekazywania i może pomóc podczas negocjowania bezpośrednich połączeń peer-to-peer, gdy jest to możliwe. Usługa Service Bus jest zoptymalizowana pod kątem programistów platformy .NET, którzy korzystają z usługi Windows Communication Foundation (WCF) ze względu na wydajność i użyteczność, oraz zapewnia pełny dostęp do jej usługi przekazywania danych za pomocą interfejsów SOAP i REST. Dzięki temu możliwa jest integracja dowolnego środowiska programowania SOAP lub REST z usługą Service Bus.

Usługa przekaźnika obsługuje tradycyjne, jednokierunkowe komunikaty, komunikaty żądań/odpowiedzi oraz komunikaty równorzędne. Obsługuje ona również dystrybucję zdarzeń w zakresie Internetu w celu umożliwienia scenariuszy publikacji/subskrypcji i komunikacji poprzez gniazdo dwukierunkowe dla zwiększonej wydajności point-to-point. We wzorcu komunikatów obsługiwanych przez przekaźnik usługa lokalna łączy się z usługą przekazywania za pomocą portu wychodzącego i tworzy gniazdo dwukierunkowe dla komunikacji powiązanej z konkretnym adresem spotkania. Klient następnie może komunikować się z lokalną usługą poprzez wysyłanie komunikatów do usługi przekazywania kierującej komunikaty do adresu spotkania. Usługa przekazywania następnie „przekazuje” komunikaty do usługi lokalnej za pośrednictwem już istniejących gniazd dwukierunkowych. Klient nie potrzebuje bezpośredniego połączenia z usługą lokalną ani nie musi wiedzieć, gdzie usługa się znajduje. Usługa lokalna nie wymaga otwarcia w zaporze żadnych portów przychodzących.

Należy zainicjować połączenie między usługą lokalną i usługą przekaźnika przy użyciu zestawu powiązań „przekaźników” WCF. W tle powiązania przekaźników są mapowane na elementy powiązania transportu przeznaczone do tworzenia składników kanału WCF, które integrują się w chmurze z usługą Service Bus.

Usługa WCF Relay oferuje wiele korzyści, lecz wymaga, aby serwer i klient byli w trybie online w tym samym czasie w celu wysyłania i odbierania komunikatów. Nie jest to optymalne rozwiązanie dla komunikacji typu HTTP, w której żądania nie mogą mieć typowo długiego okresu życia, ani dla klientów łączących się tylko okazjonalnie, na przykład przeglądarek, aplikacji mobilnych itd. Komunikaty obsługiwane przez brokera obsługują komunikację odłączoną i mają wiele zalet. Klienci i serwery mogą łączyć się w zależności od potrzeb i wykonywać operacje w sposób asynchroniczny.

## <a name="brokered-messaging"></a>Komunikaty obsługiwane przez brokera
W przeciwieństwie do schematu przekaźnika komunikaty obsługiwane w usłudze Service Bus lub [komunikaty obsługiwane przez brokera](service-bus-queues-topics-subscriptions.md) mogą być traktowane jako asynchroniczne lub „czasowo odłączone”. Producenci (nadawcy) i konsumenci (odbiorcy) nie muszą być w trybie online w tym samym czasie. Infrastruktura obsługi komunikatów w niezawodny sposób przechowuje komunikaty w „brokerze” (np. w kolejce) do momentu, aż strona odbierająca jest gotowa do ich odebrania. Dzięki temu składniki aplikacji rozproszonej mogą być rozłączone zarówno dobrowolnie, na przykład w celu przeprowadzenia konserwacji, jak i z powodu awarii składników, bez wywierania wpływu na cały system. Ponadto aplikacja odbierająca może wymagać połączenia z Internetem tylko w pewnych porach dnia, jak w przypadku systemu zarządzania spisem, którego uruchomienie jest wymagane na koniec dnia roboczego.

Podstawowymi składnikami infrastruktury komunikatów obsługiwanych przez brokera usługi Service Bus są kolejki, tematy i subskrypcje.  Główną różnicą jest to, że tematy obsługują możliwości publikowania/subskrypcji, które mogą być użyte dla zaawansowanej, opartej na zawartości logiki routingu i dostarczania, w tym wysyłania do wielu adresatów. Te składniki umożliwiają nowe asynchroniczne scenariusze obsługi komunikatów, takie jak czasowe oddzielenie, publikowanie/subskrypcja i równoważenie obciążenia. Aby uzyskać więcej informacji na temat tych jednostek obsługi komunikatów, zobacz sekcję [Kolejki, tematy i subskrypcje usługi Magistrala usług](service-bus-queues-topics-subscriptions.md).

Podobnie jak w przypadku infrastruktury przekaźnika WCF Relay możliwości komunikatów obsługiwanych przez brokera są dostarczane dla programistów platform WCF oraz .NET, a także za pośrednictwem interfejsu REST.

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat obsługi komunikatów usługi Service Bus, zobacz następujące tematy.

* [Podstawy usługi Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Kolejki, tematy i subskrypcje usługi Service Bus](service-bus-queues-topics-subscriptions.md)
* [Jak używać kolejek usługi Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak używać tematów i subskrypcji usługi Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)


