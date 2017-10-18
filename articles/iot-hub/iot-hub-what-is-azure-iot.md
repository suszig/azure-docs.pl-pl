---
title: "Rozwiązania platformy Azure dla Internetu rzeczy (Pakiet IoT) | Microsoft Docs"
description: "Przegląd przykładowej architektury rozwiązania IoT i jej relacji z urządzeniami, usługą Azure IoT Hub, zestawami SDK urządzeń Azure IoT, zestawami SDK usługi Azure IoT i innymi usługami Azure."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a859e379-dca7-42fa-bdf6-1125c86ad140
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: dobett
ms.openlocfilehash: 417ca4b6ecc39cbdafd8e12b5360b370d0ce79fa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Następne kroki

Azure IoT Hub jest usługą platformy Azure, która umożliwia bezpieczną i niezawodną dwukierunkową komunikację między zapleczem rozwiązania a milionami urządzeń. Umożliwia zapleczu rozwiązania:

* Otrzymywanie danych telemetrycznych w odpowiedniej skali z urządzeń użytkownika.
* Przekazywanie danych z urządzeń użytkownika do strumieniowego procesora zdarzeń.
* Odbieranie przekazywanych plików z urządzeń.
* Wysyłanie komunikatów chmura-urządzenie do określonych urządzeń.

Za pomocą usługi IoT Hub można implementować własne zaplecze rozwiązania. Ponadto usługa IoT Hub zawiera rejestr tożsamości używany do aprowizacji urządzeń, ich poświadczeń zabezpieczeń oraz praw do łączenia się z centrum IoT. Aby dowiedzieć się więcej na temat usługi IoT Hub, zobacz [Co to jest usługa IoT Hub][lnk-iot-hub].

Aby dowiedzieć się, jak usługa Azure IoT Hub umożliwia oparte na standardach zarządzanie urządzeniami pozwalające na zdalne zarządzanie urządzeniami, zobacz [Omówienie zarządzania urządzeniami za pomocą usługi IoT Hub][lnk-device-management].

W celu wdrożenia aplikacji klienckich na wielu różnych platformach sprzętowych i w różnych systemach operacyjnych można użyć zestawów SDK urządzeń Azure IoT. Te zestawy SDK urządzeń zawierają biblioteki, które ułatwiają wysyłanie danych telemetrycznych do centrum IoT Hub i odbieranie komunikatów wysyłanych z chmury do urządzeń. Korzystając z zestawów SDK urządzeń, można wybierać spośród wielu protokołów sieciowych służących do komunikowania się z usługą IoT Hub. Aby dowiedzieć się więcej, zobacz [informacje dotyczące zestawów SDK urządzeń][lnk-device-sdks].

Aby rozpocząć pisanie kodu i uruchomić kilka przykładów, zobacz samouczek [Wprowadzenie do usługi IoT Hub][lnk-getstarted].

Warto również zainteresować się [Pakietem IoT Azure][lnk-iot-suite], który stanowi kolekcję wstępnie skonfigurowanych rozwiązań. Pakiet IoT pozwala szybko rozpocząć pracę i skalować projekty IoT na potrzeby typowych scenariuszy, takich jak zdalne monitorowanie, zarządzanie zasobami i konserwacja predykcyjna.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md
