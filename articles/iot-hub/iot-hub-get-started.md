---
title: "Centrum IoT Azure — wprowadzenie połączenie urządzenia IoT z chmurą | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak i Podłącz urządzenia IoT i starter Kit do Centrum IoT Azure. Urządzenia można wysyłać dane telemetryczne Centrum IoT i Centrum IoT można monitorować i zarządzania urządzeniami."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: Samouczek Centrum Azure iot
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 34742208e9189eb31310b58770ee4a22e33f56d5
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="azure-iot-hub-get-started-tutorials"></a>Usługa Azure IoT Hub wprowadzenie samouczki

Azure IoT Hub i zestawy SDK urządzenia Azure IoT służy do tworzenia rozwiązań Internetu rzeczy (IoT):

* Centrum IoT Azure jest w pełni zarządzana usługa w chmurze, która bezpieczny sposób łączy, monitoruje i zarządza urządzeniami IoT. Zestawy SDK urządzenia IoT Azure umożliwia wdrożenie urządzenia IoT.
* Użyj bramy IoT w bardziej złożonych scenariuszach IoT. Na przykład, gdzie należy wziąć pod uwagę czynników, takich jak starszych urządzeń, kosztów przepustowości, zasady zabezpieczeń i prywatności lub przetwarzania danych krawędzi. W tych scenariuszach użyj [Azure IoT krawędzi](https://docs.microsoft.com/azure/iot-edge/) do zaimplementowania bramy, która łączy się z Centrum IoT z urządzeń.

## <a name="what-the-tutorials-cover"></a>Obejmuje samouczków

Te samouczki przedstawiono Azure IoT Hub i zestawy SDK urządzenia. Samouczków opisano typowe scenariusze IoT, aby zademonstrować możliwości Centrum IoT. Samouczków przedstawiono również sposób łączenia Centrum IoT z innymi usługami platformy Azure i narzędzia umożliwiające tworzenie bardziej zaawansowanych rozwiązania IoT. W samouczkach można używać urządzeń IoT symulowane lub rzeczywistej. Ponadto można poznać sposoby użyć bramy, aby umożliwić urządzeniom na łączenie z Centrum IoT.

## <a name="set-up-your-device"></a>Konfigurowanie urządzenia

Podłącz urządzenia IoT lub bramy do Centrum IoT Azure. Można wybrać urządzenie fizyczne lub symulowane rozpoczęcie pracy:

| Urządzenia IoT                       | Język programowania |
|----------------------------------|----------------------|
| Raspberry Pi                     | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| Zestaw deweloperski IoT                       | [Arduino w VSCode][DevKit]     |
| Intel Edison                     | [Node.js][Ed_Nd], [C][Ed_C]    |
| Adafruit Feather HUZZAH ESP8266  | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 operacją deweloperów       | [Arduino][Th_Ard]              |
| M0 Adafruit piór              | [Arduino][M0_Ard]              |
| Symulowane urządzenie na komputerze           | [.NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth] |
| Symulator urządzeń online         | [Pi malinowe (Node.js)][Ol_Sim] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[Sim_NET]: iot-hub-csharp-csharp-getstarted.md
[Sim_Jav]: iot-hub-java-java-getstarted.md
[Sim_Nd]: iot-hub-node-node-getstarted.md
[Sim_Pyth]: iot-hub-python-getstarted.md
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
[Sim_Lnx]: iot-hub-linux-iot-edge-get-started.md
[Sim_Win]: iot-hub-windows-iot-edge-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md
