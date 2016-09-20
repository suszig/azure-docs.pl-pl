<properties
 pageTitle="Rozwiązania platformy Azure dla Internetu rzeczy | Microsoft Azure"
 description="Przegląd architektury IoT na platformie Azure, w tym omówienie struktury przykładowego rozwiązania i relacji między nim a usługą Azure IoT Hub, zestawami SDK urządzeń i wstępnie skonfigurowanymi rozwiązaniami"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="07/19/2016"
 ms.author="dobett"/>

[AZURE.INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## Następne kroki

Azure IoT Hub jest usługą platformy Azure, która umożliwia bezpieczną i niezawodną dwukierunkową komunikację między zapleczem aplikacji a milionami urządzeń. Usługa ta pozwala zapleczu aplikacji odbierać dane telemetryczne z urządzeń na dużą skalę oraz kierować te dane do procesora zdarzeń strumienia, odbierać pliki przekazywane z urządzeń, a także wysyłać polecenia z chmury do określonych urządzeń. Za pomocą usługi IoT Hub można implementować własne zaplecze rozwiązania. Ponadto usługa IoT Hub zawiera rejestr tożsamości urządzeń używany do aprowizacji urządzeń, ich poświadczeń zabezpieczeń oraz praw do łączenia się z centrum. Aby dowiedzieć się więcej na temat usługi IoT Hub, zobacz [Co to jest usługa IoT Hub?][lnk-iot-hub].

Aby dowiedzieć się, jak usługa Azure IoT Hub umożliwia oparte na standardach zarządzanie urządzeniami IoT pozwalające na zdalne konfigurowanie i aktualizowanie urządzeń oraz zarządzanie nimi, zobacz [Omówienie zarządzania urządzeniami w usłudze Azure IoT Hub][lnk-device-management].

W celu wdrożenia aplikacji klienckich na wielu różnych platformach sprzętowych i w różnych systemach operacyjnych można użyć zestawów SDK urządzeń IoT. Zestawy SDK urządzeń IoT zawierają biblioteki, które ułatwiają wysyłanie danych telemetrycznych do usługi IoT Hub i odbieranie poleceń wysyłanych z chmury do urządzeń. Zestawy SDK udostępniają możliwość wyboru wielu protokołów sieciowych służących do komunikowania się z usługą IoT Hub. Aby dowiedzieć się więcej, zobacz [i][lnk-device-sdks].

Aby rozpocząć pisanie kodu i uruchomić kilka przykładów, zobacz samouczek [Wprowadzenie do usługi IoT Hub][lnk-getstarted].

Warto również zainteresować się [Pakietem IoT Azure][lnk-iot-suite] , który stanowi kolekcję wstępnie skonfigurowanych rozwiązań. Pakiet IoT pozwala szybko rozpocząć pracę i skalować projekty IoT na potrzeby typowych scenariuszy, takich jak zdalne monitorowanie, zarządzanie zasobami i konserwacja predykcyjna.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md


<!--HONumber=sep16_HO1-->


