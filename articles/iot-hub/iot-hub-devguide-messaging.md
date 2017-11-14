---
title: Zrozumienie Centrum IoT Azure messaging | Dokumentacja firmy Microsoft
description: "Przewodnik dewelopera — urządzenia do chmury i wiadomości z Centrum IoT chmury do urządzenia. Zawiera informacje na temat formaty wiadomości i protokołów komunikacyjnych obsługiwanych."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2017
ms.author: dobett
ms.openlocfilehash: eebf1aea76c2e6c0a60d203189a6da16634bf016
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2017
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a>Urządzenia do chmury i wiadomości z Centrum IoT chmury do urządzenia

Użyj Centrum IoT wiadomości do komunikowania się z urządzeniami przez:

* Wysyłanie [urządzenia do chmury] [ lnk-d2c] zaplecza wiadomości z urządzeń do rozwiązania.
* Wysyłanie [chmury do urządzenia] [ lnk-c2d] wiadomości z rozwiązania zaplecza na urządzeniach.

Właściwości podstawowe Centrum IoT z obsługą wiadomości są niezawodność i trwałość wiadomości. Te właściwości Włącz odporności na niestabilne połączenie po stronie urządzenia i załadować wzrostów w przypadku przetwarzania po stronie chmury. Centrum IoT implementuje *co najmniej raz* gwarantuje dostarczania dla wiadomości zarówno urządzenia do chmury, jak i chmury do urządzenia.

Aby obejrzeć wprowadzenie do funkcji Centrum IoT, zobacz artykuły [Azure i Internet rzeczy] [ lnk-azure-iot] i [Omówienie usługi Azure IoT Hub][lnk-iot-hub-overview].

## <a name="when-to-use-iot-hub-messaging"></a>Kiedy należy używać Centrum IoT obsługi wiadomości

Jednokierunkowe powiadomień do aplikacji urządzenia, należy użyć urządzenia do chmury wiadomości do wysyłania telemetrii serie czasu i alertów z aplikacją urządzenia i wiadomości chmury do urządzenia.

* Zapoznaj się [wskazówki komunikację urządzenia do chmury] [ lnk-d2c-guidance] if wątpliwe między przy użyciu wiadomości urządzenia do chmury, zgłoszone właściwości lub przekazywania pliku.
* Zapoznaj się [wskazówki dotyczące komunikacji chmury do urządzenia] [ lnk-c2d-guidance] if wątpliwe między przy użyciu wiadomości chmury do urządzenia, odpowiednie właściwości lub metody bezpośredniego.

## <a name="next-steps"></a>Następne kroki

* Więcej informacji na temat Centrum IoT [urządzenia do chmury do obsługi komunikatów][lnk-d2c].
* Więcej informacji na temat Centrum IoT [wiadomości chmury do urządzenia][lnk-c2d].

[lnk-azure-iot]: iot-hub-what-is-azure-iot.md
[lnk-iot-hub-overview]: iot-hub-what-is-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md