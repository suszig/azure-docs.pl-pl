---
title: "Brama protokołu IoT Azure | Dokumentacja firmy Microsoft"
description: "Jak używać brama protokołu Azure IoT rozszerzenie Centrum IoT, możliwości i obsługa protokołu, aby umożliwić urządzeniom do nawiązania połączenia z koncentratorem przy użyciu protokołów nie natywnie obsługiwane przez Centrum IoT."
services: iot-hub
documentationcenter: 
author: kdotchkoff
manager: timlt
editor: 
ms.assetid: 555e59ae-3136-4533-8ba8-f3a3b6acf648
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: kdotchko
ms.openlocfilehash: b2ad2c6f5eeec2f803e2d2f5f98831b551efa8b1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# Obsługa protokołów dodatkowych Centrum IoT
Centrum IoT Azure natywnie obsługuje komunikację za pośrednictwem protokołów MQTT, AMQP i HTTP. W niektórych przypadkach urządzenia lub pola bramy nie można użyć jednej z tych standardowych protokołów i wymaga dostosowania protokołu. W takich przypadkach można użyć niestandardowych bramy. Niestandardowe bramy można włączyć protokołu dostosowanie punkty końcowe Centrum IoT mostkowanie ruchu do i z Centrum IoT. Można użyć [brama protokołu Azure IoT](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) jako brama niestandardowych umożliwiające dostosowanie protokołu IoT Hub.

## Brama protokołu IoT Azure
Brama protokołu Azure IoT to platforma dostosowania protokołu, który jest przeznaczony dla wysokiej skali, komunikację dwukierunkową urządzenie z Centrum IoT. Brama protokołu jest przekazujące składnik, który akceptuje połączenia urządzenia za pośrednictwem określonego protokołu. Tworzy ono ruch do Centrum IoT za pośrednictwem protokołu AMQP 1.0. Brama protokołu Azure IoT jest dostępna jako projekt oprogramowania typu open source do zapewnienia elastyczności dodanie obsługi różnych protokołów i protokołów.

Brama protokołu na platformie Azure w taki sposób, skalowalnej można wdrożyć za pomocą usługi Azure Service Fabric, roli proces roboczy usług Azure Cloud Services lub maszyn wirtualnych systemu Windows. Ponadto bramy protokołu można wdrożyć w środowiskach lokalnych, takich jak pole bram.

Brama protokołu Azure IoT zawiera karty protokołu MQTT, która umożliwia dostosowanie zachowanie protokołu MQTT, jeśli to konieczne. Ponieważ Centrum IoT udostępnia wbudowaną obsługę protokołu v3.1.1 MQTT, tylko warto za pomocą karty protokołu MQTT, jeśli protokół dostosowań lub określonych wymagań dotyczących dodatkowe funkcje są wymagane.

Karta MQTT przedstawiono również model programowania do tworzenia protokołu kart dla innych protokołów. Ponadto model programowania brama protokołu Azure IoT umożliwia podłączenie niestandardowych składników przetwarzania specjalnych, takich jak niestandardowe uwierzytelnianie, przekształcenia wiadomości, kompresji i dekompresji lub szyfrowania i odszyfrowywania ruchu między urządzenia i Centrum IoT.

Elastyczność brama protokołu i implementacji MQTT są zawarte w projekcie oprogramowania typu open source. Dzięki temu można dostosować wykonania zgodnie z potrzebami.

## Następne kroki
Aby dowiedzieć się więcej na temat brama protokołu Azure IoT i sposobu użycia, a następnie wdrożyć go jako część rozwiązania IoT, zobacz:

* [Repozytorium brama protokołu IoT Azure w serwisie GitHub](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Przewodnik dewelopera bramy protokołu Azure IoT](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

Aby dowiedzieć się więcej na temat planowania wdrożenia Centrum IoT, zobacz:

* [Porównaj z usługi Event Hubs][lnk-compare]
* [Skalowanie, wysokiej dostępności i odzyskiwania po awarii][lnk-scaling]
* [Przewodnik dewelopera Centrum IoT][lnk-devguide]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
