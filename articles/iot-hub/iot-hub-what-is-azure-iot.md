---
title: "Rozwiązania platformy Azure dla Internetu rzeczy | Microsoft Docs"
description: "Przegląd architektury IoT na platformie Azure, w tym omówienie struktury przykładowego rozwiązania i relacji między nim a usługą Azure IoT Hub, zestawami SDK urządzeń i wstępnie skonfigurowanymi rozwiązaniami"
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
ms.date: 10/05/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 2dbf639abfa505eb329769bcc346efb5f1db443e


---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Następne kroki
Azure IoT Hub jest usługą platformy Azure, która umożliwia bezpieczną i niezawodną dwukierunkową komunikację między zapleczem aplikacji a milionami urządzeń. Umożliwia zapleczu aplikacji:

* Otrzymywanie danych telemetrycznych w odpowiedniej skali z urządzeń użytkownika.
* Przekazywanie danych z urządzeń użytkownika do strumieniowego procesora zdarzeń.
* Odbieranie przekazywanych plików z urządzeń.
* Wysyłanie poleceń chmura-urządzenie do określonych urządzeń.

Za pomocą usługi IoT Hub można implementować własne zaplecze rozwiązania. Ponadto usługa IoT Hub zawiera rejestr tożsamości urządzeń używany do aprowizacji urządzeń, ich poświadczeń zabezpieczeń oraz praw do łączenia się z centrum. Aby dowiedzieć się więcej na temat usługi IoT Hub, zobacz [Co to jest usługa IoT Hub?][lnk-iot-hub].

Aby dowiedzieć się, jak usługa Azure IoT Hub umożliwia oparte na standardach zarządzanie urządzeniami pozwalające na zdalne konfigurowanie i aktualizowanie urządzeń oraz zarządzanie nimi, zobacz [Omówienie zarządzania urządzeniami w usłudze Azure IoT Hub][lnk-device-management].

W celu wdrożenia aplikacji klienckich na wielu różnych platformach sprzętowych i w różnych systemach operacyjnych można użyć zestawów SDK urządzeń IoT. Zestawy SDK urządzeń IoT zawierają biblioteki, które ułatwiają wysyłanie danych telemetrycznych do usługi IoT Hub i odbieranie poleceń wysyłanych z chmury do urządzeń. Korzystając z zestawów SDK, można wybierać spośród wielu protokołów sieciowych służących do komunikowania się z usługą IoT Hub. Aby dowiedzieć się więcej, zobacz [i][lnk-device-sdks].

Aby rozpocząć pisanie kodu i uruchomić kilka przykładów, zobacz samouczek [Wprowadzenie do usługi IoT Hub][lnk-getstarted].

Warto również zainteresować się [Pakietem IoT Azure][lnk-iot-suite] , który stanowi kolekcję wstępnie skonfigurowanych rozwiązań. Pakiet IoT pozwala szybko rozpocząć pracę i skalować projekty IoT na potrzeby typowych scenariuszy, takich jak zdalne monitorowanie, zarządzanie zasobami i konserwacja predykcyjna.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md



<!--HONumber=Nov16_HO2-->


