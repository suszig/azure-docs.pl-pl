---
title: "Przewodnik po rozwiązaniu połączonej fabryki pakietu IoT Azure | Microsoft Docs"
description: "Opis połączonej fabryki wstępnie skonfigurowanego rozwiązania Azure IoT oraz jej architektury."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: 517e908a744734139ed0aeee314a4f3b9eda86cc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a>Przewodnik po wstępnie skonfigurowanym rozwiązaniu połączonej fabryki

[Wstępnie skonfigurowane rozwiązanie][lnk-preconfigured-solutions] połączonej fabryki pakietu IoT to implementacja kompleksowego rozwiązania przemysłowego, które:

* Łączy się zarówno z symulowanymi urządzeniami przemysłowymi, na których działają serwery OPC UA na liniach produkcyjnych symulowanej fabryki, jak i z rzeczywistymi urządzeniami serwerów OPC UA. Aby uzyskać więcej informacji na temat OPC UA, zobacz [Connected factory FAQ (Połączona fabryka — często zadawane pytania)](iot-suite-faq-cf.md).
* Pokazuje operacyjne kluczowe wskaźniki wydajności oraz ogólną wydajność sprzętu dla tych urządzeń i linii produkcyjnych.
* Demonstruje, jak aplikacja w chmurze może zostać użyta na potrzeby interakcji z systemami serwerów OPC UA.
* Umożliwia połączenie własnych urządzeń serwerów OPC UA.
* Umożliwia przeglądanie i modyfikowanie danych serwera OPC UA.
* Integruje się z usługą Azure Time Series Insights (TSI) w celu udostępnienia niestandardowych widoków danych z serwerów OPC UA.

Możesz go użyć jako punktu wyjścia dla Twojej własnej implementacji i je [dostosować][lnk-customize], aby spełniało Twoje własne określone wymagania biznesowe.

Ten artykuł przeprowadzi Cię przez niektóre kluczowe elementy rozwiązania połączonej fabryki, aby umożliwić Ci zrozumienie jego sposobu działania. Ta wiedza ułatwi Ci:

* Rozwiązywanie problemów w rozwiązaniu.
* Planowanie sposobu dostosowywania rozwiązania, aby spełniało Twoje wymagania.
* Projektowanie własnego rozwiązania IoT korzystającego z usług Azure.

Aby uzyskać więcej informacji, zobacz [Connected factory FAQ (Połączona fabryka — często zadawane pytania)](iot-suite-faq-cf.md).

## <a name="logical-architecture"></a>Architektura logiczna

Poniższy diagram przedstawia składniki logiczne wstępnie skonfigurowanego rozwiązania:

![Architektura logiczna połączonej fabryki][connected-factory-logical]

## <a name="communication-patterns"></a>Wzorce komunikacji

Rozwiązanie używa [specyfikacji OPC UA Pub/Sub](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) do wysyłania danych telemetrycznych OPC UA do centrum IoT Hub w formacie JSON. W tym celu rozwiązanie używa modułu [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) IoT Edge.

Rozwiązanie zawiera również klienta OPC UA zintegrowanego w aplikacji internetowej, który może nawiązywać połączenia z lokalnymi serwerami OPC UA. Klient używa [zwrotnego serwera proxy](https://wikipedia.org/wiki/Reverse_proxy) i otrzymuje pomoc z centrum IoT Hub, aby nawiązywać połączenie bez potrzeby otwierania portów w lokalnej zaporze. Ten wzorzec komunikacji jest zwany [komunikacją wspieraną przez usługę](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/). W tym celu rozwiązanie używa modułu [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT Edge.


## <a name="simulation"></a>Symulacja

Symulowane stacje i symulowane systemy zarządzania produkcją (MES) składają się na linię produkcyjną fabryki. Symulowane urządzenia i moduł wydawcy OPC są oparte na [standardzie OPC UA .NET][lnk-OPC-UA-NET-Standard] opublikowanym przez organizację OPC Foundation.

Składniki serwer proxy OPC i wydawca OPC są implementowane jako moduły oparte na usłudze [Azure IoT Edge][lnk-Azure-IoT-Gateway]. Każda symulowana linia produkcyjna ma dołączoną wyznaczoną bramę.

Wszystkie składniki symulacji działają w kontenerach platformy Docker hostowanych na maszynie wirtualnej platformy Azure z systemem Linux. Symulacja jest domyślnie skonfigurowana do uruchamiania ośmiu symulowanych linii produkcyjnych.

## <a name="simulated-production-line"></a>Symulowana linia produkcyjna

Na linii produkcyjnej są produkowane części. Składa się ona z różnych stacji: stacji montażowej, stacji testowania i stacji pakowania.

Symulacja przetwarza i aktualizuje dane, które są udostępniane za pośrednictwem węzłów OPC UA. Wszystkie stacje symulowanej linii produkcyjnej są zarządzane przez system MES za pośrednictwem serwera OPC UA.

## <a name="simulated-manufacturing-execution-system"></a>Symulowany system zarządzania produkcją

System MES monitoruje poszczególne stacje na linii produkcyjnej za pośrednictwem serwera OPC UA w celu wykrycia zmian stanu stacji. System MES wywołuje metody OPC UA sterujące stacjami i przekazuje produkt z jednej stację do następnej do czasu ukończenia jego produkcji.

## <a name="gateway-opc-publisher-module"></a>Moduł wydawcy OPC bramy

Moduł wydawcy OPC łączy się z serwerami OPC UA stacji i subskrybuje węzły OPC do opublikowania. Moduł ten konwertuje dane węzła do formatu JSON, szyfruje je i wysyła do usługi IoT Hub w postaci komunikatów publikowania/subskrybowania serwera OPC UA.

Moduł wydawcy OPC wymaga tylko wychodzącego portu https (443) i może współdziałać z istniejącą infrastrukturą przedsiębiorstwa.

## <a name="gateway-opc-proxy-module"></a>Moduł serwera proxy OPC bramy

Moduł serwera proxy OPC UA bramy tworzy tunel do przesyłania poleceń binarnych oraz komunikatów sterujących serwera OPC UA i wymaga tylko wychodzącego portu https (443). Może on współdziałać z istniejącą infrastrukturą przedsiębiorstwa, w tym z internetowymi serwerami proxy.

Moduł proxy używa metod urządzeń usługi IoT Hub do transferowania pakietowych danych TCP/IP w warstwie aplikacji, dzięki czemu gwarantuje zaufanie punktu końcowego oraz zapewnia szyfrowanie danych i integralność przy użyciu protokołu SSL/TLS.

Protokół przekazywania danych binarnych serwera OPC UA za pośrednictwem serwera proxy korzysta z uwierzytelniania i szyfrowania UA.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

Moduł wydawcy OPC bramy subskrybuje węzły serwera OPC UA w celu wykrycia zmian w wartościach danych. Jeśli w jednym z węzłów zostanie wykryta zmiana danych, ten moduł wysyła komunikaty do usługi Azure IoT Hub.

Usługa IoT Hub stanowi źródło zdarzeń dla usługi Azure TSI. Dane są przechowywane w usłudze TSI przez 30 dni na podstawie sygnatur czasowych dołączonych do komunikatów. Te dane obejmują:

* Identyfikator ApplicationUri serwera OPC UA
* Identyfikator NodeId serwera OPC UA
* Wartość węzła
* Sygnaturę czasową źródła
* Nazwę DisplayName serwera OPC UA

Obecnie w usłudze TSI klienci nie mogą dostosować czasu przechowywania danych.

Usługa TSI przesyła zapytania o dane węzła przy użyciu funkcji SearchSpan (Time.From, Time.To) i agreguje je według identyfikatora ApplicationUri, NodeId lub nazwy DisplayName serwera OPC UA.

Aby móc pobrać dane z mierników ogólnej wydajności sprzętu i wskaźników KPI oraz wykresów serii czasu, dane są agregowane według liczby zdarzeń oraz funkcji Sum, Avg, Min i Max.

Serie czasu są tworzone w inny sposób. Ogólna wydajność sprzętu i kluczowe wskaźniki wydajności są obliczane na podstawie danych podstawowych stacji i przetwarzane na potrzeby topologii (linii produkcyjnych, fabryk, przedsiębiorstw) w aplikacji.

Ponadto serie czasu dla topologii ogólnej wydajności sprzętu i kluczowych wskaźników wydajności są obliczane w aplikacji za każdym razem, kiedy wyświetlany przedział czasu jest gotowy. Na przykład widok dnia jest aktualizowany zawsze o pełnej godzinie.

Widok serii czasu danych węzła pochodzi bezpośrednio z usługi TSI i jest tworzony za pomocą agregacji dla przedziału czasu.

## <a name="iot-hub"></a>Usługa IoT Hub
Usługa [IoT Hub][lnk-IoT Hub] odbiera dane wysyłane z modułu wydawcy OPC do chmury i udostępnia je usłudze Azure TSI. 

Usługa IoT Hub w rozwiązaniu wykonuje ponadto następujące czynności:
- Obsługuje rejestr tożsamości, w którym są przechowywane identyfikatory dla wszystkich modułów wydawcy OPC i wszystkich modułów serwera proxy OPC.
- Służy jako kanał transportowy komunikacji dwukierunkowej modułu serwera proxy OPC.

## <a name="azure-storage"></a>Azure Storage
To rozwiązanie używa usługi Azure Blob Storage jako magazynu dysków dla maszyny wirtualnej i do przechowywania danych wdrożenia.

## <a name="web-app"></a>Aplikacja internetowa
Aplikacja internetowa wdrożona w ramach wstępnie skonfigurowanego rozwiązania obejmuje zintegrowanego klienta OPC UA, przetwarzanie alertów i wizualizację danych telemetrycznych.

## <a name="next-steps"></a>Następne kroki

Możesz kontynuować poznawanie Pakietu IoT, czytając następujące artykuły:

* [Uprawnienia w witrynie azureiotsuite.com][lnk-permissions]
* [Deploy a gateway on Windows or Linux for the connected factory preconfigured solution (Wdrażanie bramy w systemie Windows lub Linux na potrzeby wstępnie skonfigurowanego rozwiązania połączonej fabryki)](iot-suite-connected-factory-gateway-deployment.md)

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-permissions.md
