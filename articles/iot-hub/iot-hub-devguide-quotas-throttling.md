---
title: "Zrozumienie przydziały Centrum IoT Azure i ograniczania przepustowości | Dokumentacja firmy Microsoft"
description: "Przewodnik dewelopera — opis przydziałów, które mają zastosowanie do Centrum IoT i oczekiwane zachowanie ograniczania przepustowości."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 425e1b08-8789-4377-85f7-c13131fae4ce
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: dobett
ms.openlocfilehash: 68a6e999ac0ffe97c08b6420dd6e71d7154b5de8
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Odwołanie - Centrum IoT przydziały i ograniczenia przepustowości

## <a name="quotas-and-throttling"></a>Limity przydziału i ograniczanie wydajności
Każdej subskrypcji platformy Azure może mieć maksymalnie 10 centra IoT i maksymalnie 1 wolnego koncentratora.

Każdy Centrum IoT jest udostępniane z określonej liczby jednostek w określonej jednostki SKU (Aby uzyskać więcej informacji, zobacz [cennik usługi Azure IoT Hub][lnk-pricing]). Jednostki SKU i liczby jednostek określić maksymalny limit przydziału codzienne wiadomości, które można wysyłać.

Jednostka SKU określa również sposób ograniczania przepustowości ograniczeń, które egzekwuje Centrum IoT na wszystkie operacje.

## <a name="operation-throttles"></a>Limity operacji
Limity operacji są ograniczenia szybkości, są stosowane w zakresach minuty, które mają na celu Unikaj nadużywania. Centrum IoT podejmuje próbę uniknięcia zwraca błędy, jeśli to możliwe, ale rozpoczyna zwracania wyjątków, jeśli przepustnicy naruszenia zbyt długo.

W poniższej tabeli przedstawiono limity wymuszone. Wartości odnoszą się do poszczególnych koncentratora.

| Ograniczenie | Bezpłatne i koncentratory S1 | Koncentratory s2 | Koncentratory S3 | 
| -------- | ------- | ------- | ------- |
| Operacje rejestru tożsamości (Tworzenie, pobieranie, listy, aktualizowanie i usuwanie) | 1.67/sec/Unit (jednostka/100/min) | 1.67/sec/Unit (jednostka/100/min) | 83.33/sec/Unit (jednostka-5000/min) |
| Połączenia urządzenia | Wyższy 100 na sekundę lub jednostki-12/s <br/> Na przykład dwie jednostki S1 są 2\*12 = 24/s, ale mają co najmniej 100 na sekundę przez jednostek. W przypadku dziewięć S1, masz 108 na sekundę (9\*12) między jednostek. | 120/sec/unit | 6000/sec/unit |
| Liczba elementów wysłanych z urządzenia do chmury | Wyższy 100 na sekundę lub jednostki-12/s <br/> Na przykład dwie jednostki S1 są 2\*12 = 24/s, ale mają co najmniej 100 na sekundę przez jednostek. W przypadku dziewięć S1, masz 108 na sekundę (9\*12) między jednostek. | 120/sec/unit | 6000/sec/unit |
| Liczba elementów wysłanych z chmury do urządzenia | 1.67/sec/Unit (jednostka/100/min) | 1.67/sec/Unit (jednostka/100/min) | 83.33/sec/Unit (jednostka-5000/min) |
| Liczba odebranych elementów wysłanych z chmury do urządzenia <br/> (tylko kiedy urządzenie korzysta z protokołu HTTPS)| 16.67/sec/Unit (jednostka-1000/min) | 16.67/sec/Unit (jednostka-1000/min) | 833.33/sec/Unit (jednostka-50000/min) |
| Przekazywanie pliku | 1.67 pliku przekazywania powiadomień na sekundę/jednostkę (jednostka/100/min) | 1.67 pliku przekazywania powiadomień na sekundę/jednostkę (jednostka/100/min) | 83.33 pliku przekazywania powiadomień/s/jednostek (jednostka-5000/min) |
| Metody bezpośrednie | 160KB/sec/unit<sup>1</sup> | 480KB/sec/unit<sup>1</sup> | 24MB/sec/unit<sup>1</sup> | 
| Liczba odczytów bliźniaczej reprezentacji urządzenia | 10 na sekundę | Wyższy 10 na sekundę lub jednostki-1/s | Jednostka-50/s |
| Liczba aktualizacji bliźniaczej reprezentacji urządzenia | 10 na sekundę | Wyższy 10 na sekundę lub jednostki-1/s | Jednostka-50/s |
| Operacje zadań <br/> (tworzenie, aktualizowanie, wyświetlanie, usuwanie) | 1.67/sec/Unit (jednostka/100/min) | 1.67/sec/Unit (jednostka/100/min) | 83.33/sec/Unit (jednostka-5000/min) |
| Przepływność operacji zadań poszczególnych urządzeń | 10 na sekundę | Wyższy 10 na sekundę lub jednostki-1/s | Jednostka-50/s |

<sup>1</sup>ograniczenie rozmiaru miernika jest 8 KB

Ważne jest, aby wyjaśnić, że *połączenia urządzenia* ograniczania kontroluje częstotliwość, w którym można nawiązać nowe połączenia urządzenia z Centrum IoT. *Połączenia urządzenia* przepustnicy nie kontroluje maksymalna liczba równocześnie połączonych urządzeń. Przepustnica zależy od liczby jednostek, które są udostępniane dla Centrum IoT.

Na przykład jeśli kupisz pojedynczą jednostkę S1 get przepustnicy połączenia o szybkości 100 na sekundę. W związku z tym nawiązać 100 000 urządzeń ma co najmniej 1 000 sekund (około 16 minut). Jednak może mieć dowolną liczbę równocześnie połączonych urządzeń zgodnie z urządzeń zarejestrowanych w rejestrze tożsamości.

Szczegółowe omówienie Centrum IoT ograniczania zachowania, zobacz wpis w blogu [Centrum IoT ograniczania przepustowości i][lnk-throttle-blog].

> [!NOTE]
> W dowolnym momencie prawdopodobnie zwiększyć przydziały i limity przepustowości przez odpowiednie zwiększenie liczby elastycznie jednostki w Centrum IoT.
> 
> [!IMPORTANT]
> Operacje rejestru tożsamości są przeznaczone dla środowiska wykonawczego używanych do zarządzania urządzeniami i inicjowania obsługi scenariuszy. Odczytu lub aktualizacji dużej liczby urządzeń tożsamości jest obsługiwany za pośrednictwem [importowanie i eksportowanie zadań][lnk-importexport].
> 
> 

## <a name="other-limits"></a>Inne ograniczenia.

Centrum IoT wymusza inne ograniczenia operacyjne:

| Operacja | Limit |
| --------- | ----- |
| Przekazywanie pliku identyfikatorów URI | 10000 URI SAS może być limit dla konta magazynu w tym samym czasie. <br/> Jednocześnie może istnieć 10 identyfikatorów URI sygnatury dostępu współdzielonego. |
| Zadania | Historia zadań są przechowywane w górę do 30 dni <br/> Maksymalna współbieżnych zadań to 1 (bezpłatnie) i S1, 5 (w przypadku S2), 10 (na potrzeby S3). |
| Dodatkowe punkty końcowe | Płatnej SKU koncentratory może być 10 dodatkowe punkty końcowe. Bezpłatne koncentratory SKU może mieć jeden dodatkowy punkt końcowy. |
| Reguły routingu wiadomości | Płatnej SKU koncentratory mogą mieć 100 reguły routingu. Bezpłatne koncentratory SKU może mieć pięć reguły routingu. |
| Urządzenia do chmury do obsługi komunikatów | Rozmiar maksymalny komunikatu 256 KB |
| Obsługa wiadomości chmury do urządzenia | Komunikat maksymalny rozmiar 64 KB |
| Obsługa wiadomości chmury do urządzenia | Maksymalna liczba oczekujących komunikatów w celu dostarczania to 50 |
| Metoda bezpośrednia | Metoda bezpośrednia maksymalny rozmiar ładunku jest 128KB |

> [!NOTE]
> Maksymalna liczba urządzeń, którymi możesz połączyć się z jednego centrum IoT jest obecnie, 500 000. Jeśli chcesz podwyższyć ten limit, skontaktuj się z [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="latency"></a>Opóźnienie
Centrum IoT dokłada starań zapewnić małe opóźnienia dla wszystkich operacji. Jednak ze względu na warunki sieciowe i inne czynniki nieprzewidywalne go nie może zagwarantować maksymalnego czasu oczekiwania. Podczas projektowania rozwiązania, wykonaj następujące czynności:

* Należy unikać wprowadzania żadnych założeń dotyczących maksymalnego czasu oczekiwania żadnej operacji centrum IoT.
* Udostępnić Centrum IoT w regionie Azure najbardziej zbliżony do urządzenia.
* Należy rozważyć użycie krawędzi IoT Azure do wykonywania operacji wrażliwy na opóźnienia, na urządzeniu lub w pobliżu urządzenia bramy.

Wiele jednostek Centrum IoT wpłynąć na ograniczenia przepustowości, jak opisano wcześniej, ale nie udostępniają wszystkie korzyści dodatkowe opóźnienia lub gwarancji.
Jeśli widzisz zwiększa nieoczekiwane opóźnienia operacji, skontaktuj się z [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Kolejne kroki
Inne tematy dokumentacji, w tym przewodniku deweloperów Centrum IoT obejmują:

* [Punkty końcowe Centrum IoT][lnk-devguide-endpoints]
* [Język zapytań Centrum IoT urządzenia twins, zadań i rozsyłania wiadomości][lnk-devguide-query]
* [Obsługa MQTT Centrum IoT][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
