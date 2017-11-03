---
title: "Zrozumienie niestandardowe punkty końcowe Centrum IoT Azure | Dokumentacja firmy Microsoft"
description: "Przewodnik dewelopera — korzystanie z reguł routingu do kierowania wiadomości urządzenia do chmury do niestandardowe punkty końcowe."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: dobett
ms.openlocfilehash: a499783fc02e1371562edd41b827758e19fbd823
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Urządzenia do chmury wiadomości wysyłanych tras wiadomości i niestandardowe punkty końcowe

Centrum IoT umożliwia trasy [wiadomości urządzenia do chmury] [ lnk-device-to-cloud] do punktów końcowych usługi połączonej Centrum IoT na podstawie właściwości komunikatu. Reguły routingu zapewniają elastyczność do wysyłania wiadomości, których potrzebują przejścia bez konieczności stosowania dodatkowych usług do przetwarzania komunikatów lub napisać dodatkowy kod. Reguł routingu, które można skonfigurować ma następujące właściwości:

| Właściwość      | Opis |
| ------------- | ----------- |
| **Nazwa**      | Unikatowa nazwa identyfikuje reguły. |
| **Źródło**    | Pochodzenie strumień danych, które będą używane. Na przykład telemetrii urządzenia. |
| **Warunek** | Wyrażenia zapytania dla reguły routingu, która jest uruchamiana nagłówki i treści wiadomości i używany w celu określenia, czy jest ono dopasowania dla punktu końcowego. Aby uzyskać więcej informacji na temat tworzenia warunku trasy, zobacz [odwołanie - zapytania języka twins urządzenia i zadania][lnk-devguide-query-language]. |
| **Punkt końcowy**  | Nazwa punktu końcowego, w którym Centrum IoT wysyła komunikaty, które są zgodne z warunkiem. Punkty końcowe musi należeć do tego samego regionu Centrum IoT, w przeciwnym razie może być wymagana dla regionu między zapisów. |

Pojedynczy komunikat może zgodne z warunkiem na wielu reguł routingu, w których przypadku Centrum IoT dostarcza wiadomość do punktu końcowego skojarzone z każdą regułę dopasowany. Centrum IoT automatycznie deduplicates dostarczanie komunikatów, jeśli komunikat odpowiada wielu reguł mające tego samego miejsca docelowego, jest ona tylko zapisywana do tego miejsca docelowego raz.

Centrum IoT ma wartość domyślną [wbudowanym punktem końcowym][lnk-built-in]. Można tworzyć niestandardowe punkty końcowe do wyznaczania tras wiadomościom przez połączenie innych usług w Twojej subskrypcji do koncentratora. Centrum IoT obecnie obsługuje kontenery magazynu Azure, usługa Event Hubs, kolejek usługi Service Bus i tematów usługi Service Bus jako niestandardowe punkty końcowe.

> [!NOTE]
> Centrum IoT obsługuje tylko zapisywania danych do usługi Azure Storage kontenery jako obiekty BLOB.

> [!WARNING]
> Usługa Service Bus kolejek i tematów z **sesji** lub **wykrywania duplikatów** włączone nie są obsługiwane jako niestandardowe punkty końcowe.

Aby uzyskać więcej informacji na temat tworzenia niestandardowych punktów końcowych w Centrum IoT, zobacz [punkty końcowe Centrum IoT][lnk-devguide-endpoints].

Aby uzyskać więcej informacji na temat odczytu z niestandardowe punkty końcowe zobacz:

* Odczytywanie z [kontenery magazynu Azure][lnk-getstarted-storage].
* Odczytywanie z [usługi Event Hubs][lnk-getstarted-eh].
* Odczytywanie z [kolejek usługi Service Bus][lnk-getstarted-queue].
* Odczytywanie z [tematów usługi Service Bus][lnk-getstarted-topic].

### <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat punkty końcowe Centrum IoT, zobacz [punkty końcowe Centrum IoT][lnk-devguide-endpoints].

Aby uzyskać więcej informacji na temat języka zapytań, służy do definiowania reguł routingu, zobacz [Centrum IoT zapytania języka twins urządzenia, zadania i rozsyłania wiadomości][lnk-devguide-query-language].

[Centrum IoT procesu wiadomości urządzenia do chmury, za pomocą trasy] [ lnk-d2c-tutorial] samouczek pokazuje, jak używać reguł routingu oraz niestandardowe punkty końcowe.

[lnk-built-in]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-to-cloud]: iot-hub-devguide-messages-d2c.md
[lnk-devguide-query-language]: iot-hub-devguide-query-language.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-getstarted-eh]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-getstarted-queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[lnk-getstarted-topic]: ../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md
[lnk-getstarted-storage]: ../storage/blobs/storage-blobs-introduction.md
