---
title: "Połączenie bramy do pakiet IoT Azure przy użyciu NUC Intel | Dokumentacja firmy Microsoft"
description: "Za pomocą zestawu Microsoft IoT komercyjnych bramy i monitorowania zdalnego wstępnie skonfigurowane rozwiązanie. Użyj brama brzegowa IoT Azure, aby włączyć urządzenia Sensor tag nawiązać połączenie zdalne rozwiązanie monitorowania, wysyłania danych telemetrycznych do chmury i odpowiadać metody wywoływane z poziomu pulpitu nawigacyjnego rozwiązania."
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
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: bda16be1094276fcecef1e708f9d7db307d94a89
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-telemetry-from-a-sensortag"></a>Połączenie bramy Azure IoT krawędzi zdalnego wstępnie skonfigurowane rozwiązanie monitorowania i wysłać dane telemetryczne z Sensor tag

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

Ten samouczek pokazuje, jak na potrzeby wysyłania danych temperatury i wilgotności z urządzeń Sensor tag do zdalnego wstępnie skonfigurowane rozwiązanie monitorowania Azure IoT krawędzi. Sensor tag nawiązanie połączenia bramy Intel NUC pomocą Bluetooth. W samouczku:

- Azure IoT krawędzi do wdrożenia bramy próbki.
- Pakiet IoT zdalne monitorowanie wstępnie skonfigurowane rozwiązanie jako zaplecza opartej na chmurze.

## <a name="overview"></a>Omówienie

W tym samouczku zostaną wykonane następujące kroki:

- Wdrożenie wystąpienia programu zdalnego wstępnie skonfigurowane rozwiązanie monitorowania do subskrypcji platformy Azure. Ten krok automatycznie wdraża i konfiguruje wiele usług platformy Azure.
- Skonfiguruj urządzenie bramy Intel NUC do komunikowania się z komputerem i zdalnego rozwiązanie monitorowania.
- Konfigurowanie bramy Intel NUC odbierać dane telemetryczne z urządzeń Sensor tag, a następnie wyślij je do zdalnego pulpitu nawigacyjnego monitorowania.

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[Texas Instruments Sensor tag cz][lnk-sensortag]. W tym samouczku pobiera dane telemetryczne przez sieć Bluetooth na urządzeniu Sensor tag.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> Zdalne rozwiązanie monitorowania udostępnia zestaw usług platformy Azure w ramach subskrypcji platformy Azure. Wdrożenie odzwierciedla architektury przedsiębiorstwa prawdziwe. Aby uniknąć niepotrzebnych wykorzystania platformy Azure, opłat, należy usunąć wystąpienia wstępnie skonfigurowanego rozwiązania na azureiotsuite.com po zakończeniu z nim. Jeśli potrzebujesz ponownie wstępnie skonfigurowanego rozwiązania, można go łatwo odtworzyć. Aby uzyskać więcej informacji na temat zmniejszenie zużycia podczas wykonywania zdalnego rozwiązanie monitorowania, zobacz [Konfigurowanie pakiet IoT Azure wstępnie rozwiązań dla celów demonstracyjnych][lnk-demo-config].

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

## <a name="configure-bluetooth-connectivity"></a>Konfiguracja połączenia Bluetooth

Konfigurować funkcję Bluetooth na NUC firmy Intel, aby włączyć w urządzeniu Sensor tag do łączenia i wysyłania danych telemetrycznych.

### <a name="find-the-mac-address-of-the-sensortag"></a>Znajdź adres MAC Sensor tag

1. W powłoce NUC firmy Intel uruchom następujące polecenie, aby odblokować usługa Bluetooth:

    ```bash
    sudo rfkill unblock bluetooth
    ```

1. Uruchom następujące polecenia, aby uruchomić usługę Bluetooth na NUC firmy Intel, a następnie wprowadź powłoki Bluetooth:

    ```bash
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Uruchom następujące polecenie do potęgi na kontrolerze Bluetooth:

    ```bash
    power on
    ```

    Jeśli kontroler jest włączona, zostanie wyświetlony komunikat z **Zmienianie zasilania w zakończyło się pomyślnie**.

1. Uruchom następujące polecenie, aby skanowanie w poszukiwaniu pobliskimi urządzeniami Bluetooth:

    ```bash
    scan on
    ```

1. Naciśnij przycisk zasilania na Sensor tag, aby był wykrywalny. Zielona LED miga.

1. Gdy pojawi się komunikat w powłoce, czy kontroler został odnaleziony Sensor tag, zanotuj adres MAC urządzenia. Adres MAC wygląda jak **A0:E6:F8:B5:F6:00**. Należy adres MAC w dalszej części tego samouczka, podczas konfigurowania bramy.

1. Uruchom następujące polecenie, aby wyłączyć skanowanie Bluetooth:

    ```bash
    scan off
    ```

1. Uruchom następujące polecenie, aby sprawdzić, czy można połączyć się z urządzeniem Sensor tag:

    ```bash
    connect <SensorTag MAC address>
    ```

    Jeśli łączysz się pomyślnie, powłoka zawiera komunikat **połączenie przebiegło pomyślnie** i wyświetla informacje o urządzeniu Sensor tag. Jeśli nie możesz połączyć wyboru Sensor tag jest nadal włączony.

1. Teraz możesz odłączyć Sensor tag i wyjdź z powłoki Bluetooth, uruchamiając następujące polecenia:

    ```bash
    disconnect
    exit
    ```

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-the-custom-iot-edge-module"></a>Utworzenie niestandardowego modułu krawędzi IoT

Teraz można tworzyć niestandardowe moduł IoT krawędzi, który umożliwia bramy do wysyłania komunikatów do zdalnego rozwiązanie monitorowania. Aby uzyskać więcej informacji dotyczących konfigurowania bramy i moduły krawędzi IoT, zobacz [pojęcia dotyczące usługi Azure IoT krawędzi][lnk-gateway-concepts].

Pobierz kod źródłowy niestandardowe moduły krawędzi IoT z serwisu GitHub, za pomocą następujących poleceń:

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

Tworzenie niestandardowego modułu krawędzi IoT przy użyciu następujących poleceń:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

Skrypt kompilacji umieszcza niestandardowego modułu krawędzi IoT libsensor2remotemonitoring.so w folderze kompilacji.

## <a name="configure-and-run-the-iot-edge-gateway"></a>Konfigurowanie i uruchamianie brama brzegowa IoT

Można teraz skonfigurować bramę brzegową IoT, aby wysłać dane telemetryczne z urządzenia Sensor tag do zdalnego pulpitu nawigacyjnego monitorowania. Aby uzyskać więcej informacji dotyczących konfigurowania bramy i moduły krawędzi IoT, zobacz [pojęcia dotyczące usługi Azure IoT krawędzi][lnk-gateway-concepts].

> [!TIP]
> W tym samouczku, Użyj standardowego `vi` Edytor tekstu na Intel NUC. Jeśli nie używasz `vi` przed, należy wykonać Samouczek wprowadzający, takich jak [Unix - vi samouczek edytora] [ lnk-vi-tutorial] zapoznać się z tego edytora. Alternatywnie możesz zainstalować użytkownikom większy komfort [nano](https://www.nano-editor.org/) edytora za pomocą polecenia `smart install nano -y`.

Otwórz przykładowy plik konfiguracji w **vi** edytora za pomocą następującego polecenia:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic/remote_monitoring.json
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
    "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Zastąp **macAddress** symbol zastępczy adres MAC z Sensor tag zanotowany wcześniej. Zastąp **deviceID** i **deviceKey** symbole zastępcze z identyfikatorów i klucze dla urządzeń wcześniej utworzony w zdalnym rozwiązanie monitorowania.

W konfiguracji modułu Sensor tag Znajdź następujące wiersze:

```json
"args": {
  "controller_index": 0,
  "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
  ...
}
```

Zastąp **urządzenia\_mac\_adres** symbol zastępczy adres MAC z Sensor tag zanotowany wcześniej.

Zapisz zmiany.

Teraz możesz uruchomić bramę przy użyciu następujących poleceń:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
/usr/share/azureiotgatewaysdk/samples/ble_gateway/ble_gateway remote_monitoring.json
```

Brama brzegowa IoT rozpoczyna się Intel NUC i wysyła dane telemetryczne z Sensor tag do zdalnego rozwiązanie monitorowania:

![Brama brzegowa IoT wysyła dane telemetryczne z Sensor tag][img-telemetry]

Naciśnij klawisz **Ctrl-C** aby zamknąć program w dowolnym momencie.

## <a name="view-the-telemetry"></a>Widok telemetrii

Brama jest teraz wysyłanie danych telemetrycznych z urządzeń Sensor tag do zdalnego rozwiązanie monitorowania. Można wyświetlić dane telemetryczne na pulpicie nawigacyjnym rozwiązania. Polecenia można również wysłać do urządzenia Sensor tag za pośrednictwem bramy z poziomu pulpitu nawigacyjnego rozwiązania.

- Przejdź do pulpitu nawigacyjnego rozwiązania.
- Wybierz urządzenie, skonfigurowane w bramy, która reprezentuje Sensor tag w **urządzenia do widoku** listy rozwijanej.
- Dane telemetryczne z urządzeń Sensor tag wyświetla na pulpicie nawigacyjnym.

![Wyświetl dane telemetryczne z urządzeń Sensor tag][img-telemetry-display]

> [!WARNING]
> Pozostawienie zdalnego monitorowania działającej na koncie Azure są rozliczane dla przy uruchomieniu. Aby uzyskać więcej informacji na temat zmniejszenie zużycia podczas wykonywania zdalnego rozwiązanie monitorowania, zobacz [Konfigurowanie pakiet IoT Azure wstępnie rozwiązań dla celów demonstracyjnych][lnk-demo-config]. Usuwanie wstępnie skonfigurowane rozwiązanie z konta platformy Azure po zakończeniu korzystania z niego.


## <a name="next-steps"></a>Następne kroki

Odwiedź stronę [Centrum deweloperów systemu Azure IoT](https://azure.microsoft.com/develop/iot/) więcej przykłady i dokumentacja Azure IoT.

[img-telemetry]: ./media/iot-suite-gateway-kit-get-started-sensortag/appoutput.png


[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-sensortag/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-sensortag]: http://www.ti.com/ww/en/wireless_connectivity/sensortag/
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started