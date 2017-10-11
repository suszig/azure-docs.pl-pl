---
title: "Porównanie Centrum IoT Azure do usługi Azure Event Hubs | Dokumentacja firmy Microsoft"
description: "Porównanie usług Centrum IoT i Azure centra zdarzeń wyróżnianie różnice funkcjonalne i przypadki użycia. Porównanie zawiera obsługiwanych protokołów, zarządzanie urządzeniami, monitorowania i przekazywania plików."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: aeddea62-8302-48e2-9aad-c5a0e5f5abe9
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: a37df79a38a35b61cca72918d4d893a4bfc83b7e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="comparison-of-azure-iot-hub-and-azure-event-hubs"></a>Porównanie Centrum IoT Azure i usługi Azure Event Hubs
Jedną z głównych zastosowania Centrum IoT jest zbieranie danych telemetrycznych z urządzeń. Z tego powodu Centrum IoT często jest porównywany z [Azure Event Hubs][Azure Event Hubs]. Podobnie jak Centrum IoT Event Hubs to usługa, która umożliwia transfer danych przychodzących zdarzenia i dane telemetryczne do chmury w bardzo dużej skali, z małymi opóźnieniami i wysoką niezawodnością przetwarzania zdarzeń.

Jednak usługi mają wiele różnice, które są szczegółowo opisane w poniższej tabeli:

| Obszar | Usługa IoT Hub | Usługa Event Hubs |
| --- | --- | --- |
| Wzorce komunikacji | Umożliwia [komunikację urządzenia do chmury] [ lnk-d2c-guidance] (wiadomości, plik przekazywania oraz właściwości zgłoszone) i [komunikacji chmury do urządzenia] [ lnk-c2d-guidance] (bezpośrednie metod, odpowiednie właściwości wiadomości). |Tylko umożliwia transfer danych przychodzących zdarzeń (zazwyczaj wzięta pod uwagę scenariusze urządzenia do chmury). |
| Informacje o stanie urządzenia | [Urządzenie twins] [ lnk-twins] można przechowywać i zapytania o informacje o stanie urządzeń. | Mogą być przechowywane żadne informacje o stanie urządzeń. |
| Obsługa protokołu urządzeń |Obsługuje MQTT, MQTT przez protokół WebSockets, AMQP, AMQP za pośrednictwem protokołu WebSockets i HTTP. Ponadto Centrum IoT współpracuje z [brama protokołu Azure IoT][lnk-azure-protocol-gateway], bramy można dostosowywać protokołu wykonania obsługuje protokoły niestandardowe. |Obsługa protokołu AMQP, AMQP za pośrednictwem protokołu WebSockets i HTTP. |
| Bezpieczeństwo |Miejsce na urządzenie tożsamości i kontroli dostępu odwoływalny. Zobacz [zabezpieczeń części przewodnika deweloperów Centrum IoT]. |Udostępnia centra zdarzeń na poziomie [udostępnionych zasady dostępu][Event Hubs - security], z ograniczoną odwołania obsługuje za pośrednictwem [zasad wydawcy][Event Hubs publisher policies]. Rozwiązania IoT często są wymagane do zaimplementowania niestandardowego rozwiązania do obsługi poświadczenia na urządzeniu i środki ochrony przed fałszowaniem. |
| Monitorowanie operacji |Umożliwia rozwiązania IoT subskrybować bogaty zestaw urządzenia tożsamości zarządzania i łączność zdarzeń, takich jak błędy uwierzytelniania poszczególnych urządzeń, ograniczania i wyjątków zły format. Zdarzenia te umożliwiają szybkie identyfikowanie problemów z łącznością na poziomie poszczególnych urządzeń. |Przedstawia tylko agregacji metryki. |
| Skalowanie |Jest zoptymalizowana pod kątem obsługi miliony równocześnie połączonych urządzeń. |Gazomierzy połączeń zgodnie [przydziały Azure Event Hubs][Azure Event Hubs quotas]. Z drugiej strony Usługa Event Hubs umożliwia określenie partycji dla każdej wiadomości wysyłane. |
| Zestawy SDK urządzenia |Udostępnia [urządzenia zestawów SDK] [ Azure IoT SDKs] dla wielu różnych platformach i językach, oprócz bezpośrednich MQTT AMQP i HTTP interfejsów API. |Jest obsługiwane na .NET, Java i C, oprócz interfejsów wysyłania protokołu AMQP i HTTP. |
| Przekazywanie pliku |Umożliwia rozwiązania IoT przekazywania plików z urządzenia do chmury. Zawiera punkt końcowy powiadomienia pliku integracji przepływu pracy i operacji monitorowania kategorii w celu obsługi debugowania. | Nie jest obsługiwane. |
| Rozsyłanie wiadomości do wielu punktów końcowych | Maksymalnie 10 niestandardowe punkty końcowe są obsługiwane. Reguły określają, jak komunikaty są kierowane do niestandardowe punkty końcowe. Aby uzyskać więcej informacji, zobacz [wysyłania i odbierania wiadomości z Centrum IoT][lnk-devguide-messaging]. | Wymaga dodatkowego kodu napisać i hostowanych na potrzeby rozsyłania komunikatów. |

Podsumowując nawet jeżeli tylko przypadek użycia jest przychodzący telemetrii urządzenia do chmury Centrum IoT zapewnia usługi, której celem jest łączność urządzenia IoT. Nadal rozwiń atrakcyjne dotyczących następujących scenariuszy z funkcji specyficznych dla IoT. Centra zdarzeń jest przeznaczona dla zdarzeń wejściowych w bardzo dużej skali, zarówno w kontekście między centrum danych i centrum danych wewnątrz scenariuszy.

Nie jest rzadko używać Centrum IoT i centra zdarzeń w tym samym rozwiązaniu. Centrum IoT obsługuje komunikację urządzenia do chmury, a usługa Event Hubs obsługuje zdarzenia późniejszym etapie ruch przychodzący do aparatami przetwarzania w czasie rzeczywistym.

### <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat planowania wdrożenia Centrum IoT, zobacz [skalowanie, wysokiej dostępności i odzyskiwania po awarii][lnk-scaling].

Aby dokładniej analizować możliwości Centrum IoT, zobacz:

* [Przewodnik dewelopera Centrum IoT][lnk-devguide]
* [Symuluje urządzenia IoT krawędzi][lnk-iotedge]

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[Azure Event Hubs]: ../event-hubs/event-hubs-what-is-event-hubs.md
[zabezpieczeń części przewodnika deweloperów Centrum IoT]: iot-hub-devguide-security.md
[Event Hubs - security]: ../event-hubs/event-hubs-authentication-and-security-model-overview.md
[Event Hubs publisher policies]: ../event-hubs/event-hubs-features.md#event-publishers
[Azure Event Hubs quotas]: ../event-hubs/event-hubs-quotas.md
[Azure IoT SDKs]: https://github.com/Azure/azure-iot-sdks
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
