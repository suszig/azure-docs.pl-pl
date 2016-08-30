<properties
 pageTitle="Rozwiązania platformy Azure dla Internetu rzeczy | Microsoft Azure"
 description="Przegląd architektury IoT na platformie Azure, w tym omówienie struktury przykładowego rozwiązania i relacji między nim a Pakietem IoT Azure i wstępnie skonfigurowanymi rozwiązaniami"
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="05/25/2016"
 ms.author="dobett"/>

[AZURE.INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## Pakiet IoT Azure

Pakiet IoT Azure firmy Microsoft jest rozwiązaniem klasy korporacyjnej, które umożliwia szybkie rozpoczęcie pracy dzięki zestawowi rozszerzalnych, wstępnie skonfigurowanych rozwiązań pozwalających obsługiwać typowe scenariusze IoT, takie jak [zdalne monitorowanie][lnk-preconfigured-solutions] i [konserwacja predykcyjna][lnk-predictive-maintenance]. Rozwiązania te stanowią implementacje opisanej wcześniej architektury IoT.

Te niezawodne rozwiązania są kompletne i uniwersalne. Obejmują symulowane urządzenia, które ułatwiają rozpoczęcie pracy, oraz wstępnie skonfigurowane usługi platformy Azure, takie jak [Azure IoT Hub][], [Azure Event Hubs][], [Azure Stream Analytics][], [Azure Machine Learning][] i [Azure Storage][], a także konsole zarządzania przeznaczone dla określonych rozwiązań. Wstępnie skonfigurowane rozwiązania obejmują sprawdzony kod, gotowy do wdrożenia produkcyjnego, który można dostosować i rozszerzyć pod kątem implementacji własnych scenariuszy IoT.

Warto również zainteresować się usługą [Azure IoT Hub][], używaną w wielu wstępnie skonfigurowanych rozwiązaniach. Usługa [Azure IoT Hub][] zapewnia bezpieczną i niezawodną komunikację dwukierunkową między chmurą a urządzeniami używanymi w ramach rozwiązania.

## Następne kroki

Poniższe zasoby zawierają więcej informacji na temat architektury IoT na platformie Azure:

- [Omówienie Pakietu IoT Azure][lnk-suite-overview].
- [Wprowadzenie do wstępnie skonfigurowanych rozwiązań IoT][lnk-preconfigured-solutions].
- [Connecting your device to the IoT Suite remote monitoring solution][lnk-connecting] (Łączenie urządzenia z rozwiązaniem Pakietu IoT do monitorowania zdalnego).
- [Azure IoT Hub]

[lnk-suite-overview]: iot-suite-overview.md
[lnk-connecting]: iot-suite-connecting-devices.md
[lnk-preconfigured-solutions]: iot-suite-getstarted-preconfigured-solutions.md
[Azure IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[Azure Event Hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[Azure Machine Learning]: https://azure.microsoft.com/documentation/services/machine-learning/
[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md


<!--HONumber=jun16_HO2-->


