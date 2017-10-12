---
title: "Rozwiązania platformy Azure dla Internetu rzeczy (Pakiet IoT) | Microsoft Docs"
description: "Przegląd architektury IoT na platformie Azure, w tym omówienie struktury przykładowego rozwiązania i relacji między nim a Pakietem IoT Azure i wstępnie skonfigurowanymi rozwiązaniami."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 437d2655-896f-4a9e-a4a8-b864790d3ef8
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 320190488bb4c7b8192421f9dd50a5264f558584
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="azure-iot-suite"></a>Pakiet IoT Azure
Pakiet IoT Azure firmy Microsoft jest rozwiązaniem klasy korporacyjnej, które umożliwia szybkie rozpoczęcie pracy dzięki zestawowi rozszerzalnych, wstępnie skonfigurowanych rozwiązań. Rozwiązania te umożliwiają obsługę typowych scenariuszy IoT, takich jak [zdalne monitorowanie][lnk-preconfigured-solutions], [konserwacja zapobiegawcza][lnk-predictive-maintenance] i [połączona fabryka][lnk-connected-factory]. Rozwiązania te stanowią implementacje opisanej w tym artykule architektury IoT.

Te wstępnie skonfigurowane rozwiązania są kompletne, niezawodne i uniwersalne. Obejmują:

- Symulowane urządzenia, które ułatwiają rozpoczęcie pracy.
- Wstępnie skonfigurowane usługi platformy Azure, takie jak [Azure IoT Hub][Azure IoT Hub], [Azure Event Hubs][Azure Event Hubs], [Azure Stream Analytics][Azure Stream Analytics], [Azure Machine Learning][Azure Machine Learning] i [Azure Storage][Azure storage].
- Konsole zarządzania przeznaczone dla określonych rozwiązań.

Wstępnie skonfigurowane rozwiązania obejmują sprawdzony kod, gotowy do wdrożenia produkcyjnego, który można dostosować i rozszerzyć pod kątem implementacji własnych scenariuszy IoT.

Warto również zainteresować się usługą [Azure IoT Hub][Azure IoT Hub], używaną w wielu wstępnie skonfigurowanych rozwiązaniach. Usługa [Azure IoT Hub][Azure IoT Hub] zapewnia bezpieczną i niezawodną komunikację dwukierunkową między urządzeniami a chmurą używaną w ramach architektury wstępnie skonfigurowanego rozwiązania.

## <a name="next-steps"></a>Następne kroki
Przejrzyj te zasoby, aby kontynuować poznawanie Pakietu IoT i wstępnie skonfigurowanych rozwiązań:

* [Co to jest Pakiet IoT Azure?][lnk-whatissuite]
* [Co to są wstępnie skonfigurowane rozwiązania Pakietu IoT Azure?][lnk-whatarepreconfigured]

[lnk-whatissuite]: iot-suite-overview.md
[lnk-whatarepreconfigured]: iot-suite-what-are-preconfigured-solutions.md

[lnk-preconfigured-solutions]: iot-suite-getstarted-preconfigured-solutions.md
[Azure IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[Azure Event Hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[Azure Machine Learning]: https://azure.microsoft.com/documentation/services/machine-learning/
[Azure storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-connected-factory]: iot-suite-connected-factory-overview.md