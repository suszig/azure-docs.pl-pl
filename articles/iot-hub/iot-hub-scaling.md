---
title: Centrum IoT Azure skalowanie | Dokumentacja firmy Microsoft
description: "Jak skalować Centrum IoT do obsługi sieci przepływności przewidywanego wiadomości. Zawiera podsumowanie obsługiwana przepływność dla każdej warstwy i opcje dotyczące dzielenia na fragmenty."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: e7bd4968-db46-46cf-865d-9c944f683832
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2cb263103da05b10c24aab71d81c43eb25987565
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="scale-your-iot-hub-solution"></a>Skalować rozwiązania IoT hub
Centrum IoT Azure może obsługiwać do milionów równocześnie połączonych urządzeń. Aby uzyskać więcej informacji, zobacz [Centrum IoT cennik][lnk-pricing]. Każda jednostka Centrum IoT umożliwia pewną liczbę wiadomości codzienne.

Aby prawidłowo skalować rozwiązania, należy wziąć pod uwagę określonego korzystanie z Centrum IoT. W szczególności należy wziąć pod uwagę przepustowość wymagana szczytu w ramach następujących kategorii działań:

* Komunikaty z urządzenia do chmury
* Komunikaty chmury do urządzenia
* Operacje rejestru tożsamości

Oprócz tych informacji przepływności, zobacz [Centrum IoT przydziały i limity] [ IoT Hub quotas and throttles] i odpowiednio zaprojektować rozwiązanie.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Przepustowość urządzenia do chmury i chmury do urządzenia wiadomości
Najlepszym sposobem rozmiaru rozwiązania IoT Hub jest do oceny ruchu na podstawie jednostkę.

Urządzenia do chmury wiadomości zgodna z tymi wytycznymi długoterminowa przepustowość.

| Warstwa | Długoterminowa przepustowość | Wskaźnik wysyłania stałej |
| --- | --- | --- |
| S1 |Maksymalnie 1111 KB na minutę na jednostkę<br/>(1,5 GB/dzień/jednostka) |Średnia 278 komunikatów na minutę na jednostkę<br/>(400 000 komunikatów/dzień na jednostkę) |
| S2 |Maksymalnie 16 MB na minutę na jednostkę<br/>(22.8 GB/dzień/jednostka) |Średnia 4,167 komunikatów na minutę na jednostkę<br/>(6 mln wiadomości/dzień na jednostkę) |
| S3 |Maksymalnie 814 MB na minutę na jednostkę<br/>(1144.4 GB/dzień/jednostka) |Średnia 208,333 komunikatów na minutę na jednostkę<br/>(300 milionów wiadomości/dzień na jednostkę) |

## <a name="identity-registry-operation-throughput"></a>Przepływność operacji rejestru tożsamości
Operacje rejestru tożsamości Centrum IoT nie powinna być czasu wykonywania operacji, ponieważ przede wszystkim dotyczące inicjowania obsługi urządzeń.

Dla określonego serii numerów wydajności, zobacz [Centrum IoT przydziały i limity][IoT Hub quotas and throttles].

## <a name="sharding"></a>Dzielenia na fragmenty
Podczas jednego centrum IoT można skalować do milionów urządzeń, czasami rozwiązanie wymaga charakterystyk wydajności właściwe, które nie może zagwarantować pojedynczego Centrum IoT. W takim przypadku zaleca się partycji urządzenia do wielu centra IoT. Centra IoT wielu smooth seria ruchu i uzyskiwanie wymagana przepustowość lub szybkości działania, które są wymagane.

## <a name="next-steps"></a>Następne kroki
Aby dokładniej analizować możliwości Centrum IoT, zobacz:

* [Przewodnik dewelopera Centrum IoT][lnk-devguide]
* [Symuluje urządzenia Azure IoT krawędzi][lnk-iotedge]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
