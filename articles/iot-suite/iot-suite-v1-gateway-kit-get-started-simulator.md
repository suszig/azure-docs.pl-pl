---
title: "Połączenie bramy do pakiet IoT Azure przy użyciu NUC Intel | Dokumentacja firmy Microsoft"
description: "Za pomocą zestawu Microsoft IoT komercyjnych bramy i monitorowania zdalnego wstępnie skonfigurowane rozwiązanie. Brama brzegowa IoT Azure umożliwia nawiązywanie połączenia zdalnego rozwiązanie monitorowania, wysłać symulowanych dane telemetryczne do chmury i odpowiadać na metody wywoływane z poziomu pulpitu nawigacyjnego rozwiązania."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: f3b4a80b37b0b869847c90834731d4c23883a961
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a>Połączenie bramy Azure IoT krawędzi zdalnego wstępnie skonfigurowane rozwiązanie monitorowania i wysłać symulowanych telemetrii

[!INCLUDE [iot-suite-v1-gateway-kit-selector](../../includes/iot-suite-v1-gateway-kit-selector.md)]

Ten samouczek pokazuje, jak używać krawędzi IoT Azure do symulowania temperatury i wilgotności do wysłania dane do zdalnego wstępnie skonfigurowane rozwiązanie monitorowania. W samouczku:

- Azure IoT krawędzi do wdrożenia bramy próbki.
- Pakiet IoT zdalne monitorowanie wstępnie skonfigurowane rozwiązanie jako zaplecza opartej na chmurze.

## <a name="overview"></a>Omówienie

W tym samouczku zostaną wykonane następujące kroki:

- Wdrożenie wystąpienia programu zdalnego wstępnie skonfigurowane rozwiązanie monitorowania do subskrypcji platformy Azure. Ten krok automatycznie wdraża i konfiguruje wiele usług platformy Azure.
- Skonfiguruj urządzenie bramy Intel NUC do komunikowania się z komputerem i zdalnego rozwiązanie monitorowania.
- Skonfiguruj bramę brzegową IoT, aby wysłać dane telemetryczne symulowane, które można wyświetlić na pulpicie nawigacyjnym rozwiązania.

[!INCLUDE [iot-suite-v1-gateway-kit-prerequisites](../../includes/iot-suite-v1-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> Zdalne rozwiązanie monitorowania udostępnia zestaw usług platformy Azure w ramach subskrypcji platformy Azure. Wdrożenie odzwierciedla architektury przedsiębiorstwa prawdziwe. Aby uniknąć niepotrzebnych wykorzystania platformy Azure, opłat, należy usunąć wystąpienia wstępnie skonfigurowanego rozwiązania na azureiotsuite.com po zakończeniu z nim. Jeśli potrzebujesz ponownie wstępnie skonfigurowanego rozwiązania, można go łatwo odtworzyć. Aby uzyskać więcej informacji na temat zmniejszenie zużycia podczas wykonywania zdalnego rozwiązanie monitorowania, zobacz [Konfigurowanie pakiet IoT Azure wstępnie rozwiązań dla celów demonstracyjnych][lnk-demo-config].

[!INCLUDE [iot-suite-v1-gateway-kit-view-solution](../../includes/iot-suite-v1-gateway-kit-view-solution.md)]

Powtórz poprzednie kroki, aby dodać drugiego urządzenia przy użyciu Identyfikatora urządzenia, takich jak **device02**. Próbki wysyła dane z dwóch symulowane urządzeń w bramie do zdalnego rozwiązanie monitorowania.

[!INCLUDE [iot-suite-v1-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-v1-gateway-kit-prepare-nuc-connectivity.md)]

[!INCLUDE [iot-suite-v1-gateway-kit-prepare-nuc-software](../../includes/iot-suite-v1-gateway-kit-prepare-nuc-software.md)]

## <a name="build-the-custom-iot-edge-module"></a>Utworzenie niestandardowego modułu krawędzi IoT

Teraz można tworzyć niestandardowe moduł IoT krawędzi, który umożliwia bramy do wysyłania komunikatów do zdalnego rozwiązanie monitorowania. Aby uzyskać więcej informacji dotyczących konfigurowania bramy i moduły krawędzi IoT, zobacz [pojęcia dotyczące usługi Azure IoT krawędzi][lnk-gateway-concepts].

Pobierz kod źródłowy niestandardowe moduły krawędzi IoT z serwisu GitHub, za pomocą następujących poleceń:

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

Tworzenie niestandardowego modułu krawędzi IoT przy użyciu następujących poleceń:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

Skrypt kompilacji umieszcza niestandardowego modułu krawędzi IoT libsimulator.so w folderze kompilacji.

## <a name="configure-and-run-the-iot-edge-gateway"></a>Konfigurowanie i uruchamianie brama brzegowa IoT

Można teraz skonfigurować bramę brzegową IoT, aby wysłać symulowanych dane telemetryczne do zdalnego pulpitu nawigacyjnego monitorowania. Aby uzyskać więcej informacji dotyczących konfigurowania bramy i moduły krawędzi IoT, zobacz [pojęcia dotyczące usługi Azure IoT krawędzi][lnk-gateway-concepts].

> [!TIP]
> W tym samouczku, Użyj standardowego `vi` Edytor tekstu na Intel NUC. Jeśli nie używasz `vi` przed, należy wykonać Samouczek wprowadzający, takich jak [Unix - vi samouczek edytora] [ lnk-vi-tutorial] zapoznać się z tego edytora. Alternatywnie możesz zainstalować użytkownikom większy komfort [nano](https://www.nano-editor.org/) edytora za pomocą polecenia `smart install nano -y`.

Otwórz przykładowy plik konfiguracji w **vi** edytora za pomocą następującego polecenia:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
```

W konfiguracji modułu z Centrum IoTHub Znajdź następujące wiersze:

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

Zastąp symbole zastępcze informacji Centrum IoT utworzony i zapisany na początku tego samouczka. Wartość IoTHubName wygląda jak **yourrmsolution37e08**, i wartość IoTSuffix jest zwykle **azure devices.net**.

W konfiguracji modułu mapowania Znajdź następujące wiersze:

```json
args": [
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  },
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Zastąp **deviceID** i **deviceKey** symbole zastępcze z identyfikatorów i klucze dla urządzeń wcześniej utworzony w zdalnym rozwiązanie monitorowania.

Zapisz zmiany.

Teraz możesz uruchomić brama brzegowa IoT, używając następujących poleceń:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

Brama rozpoczyna się Intel NUC i wysyła dane telemetryczne symulowane do zdalnego rozwiązanie monitorowania:

![Brama brzegowa IoT generuje symulowane telemetrii][img-simulated telemetry]

Naciśnij klawisz **Ctrl-C** aby zamknąć program w dowolnym momencie.

## <a name="view-the-telemetry"></a>Widok telemetrii

Brama brzegowa IoT teraz wysyła dane telemetryczne symulowane do zdalnego rozwiązanie monitorowania. Można wyświetlić dane telemetryczne na pulpicie nawigacyjnym rozwiązania.

- Przejdź do pulpitu nawigacyjnego rozwiązania.
- Wybierz jedną z dwóch urządzeń skonfigurowanych w bramie w **urządzenia do widoku** listy rozwijanej.
- Wyświetla dane telemetryczne z urządzenia bramy na pulpicie nawigacyjnym.

![Wyświetl dane telemetryczne z urządzeń symulowane bramy][img-telemetry-display]

> [!WARNING]
> Pozostawienie zdalnego monitorowania działającej na koncie Azure są rozliczane dla przy uruchomieniu. Aby uzyskać więcej informacji na temat zmniejszenie zużycia podczas wykonywania zdalnego rozwiązanie monitorowania, zobacz [Konfigurowanie pakiet IoT Azure wstępnie rozwiązań dla celów demonstracyjnych][lnk-demo-config]. Usuwanie wstępnie skonfigurowane rozwiązanie z konta platformy Azure po zakończeniu korzystania z niego.

## <a name="next-steps"></a>Następne kroki

Odwiedź stronę [Centrum deweloperów systemu Azure IoT](https://azure.microsoft.com/develop/iot/) więcej przykłady i dokumentacja Azure IoT.

[img-simulated telemetry]: ./media/iot-suite-v1-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-v1-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started