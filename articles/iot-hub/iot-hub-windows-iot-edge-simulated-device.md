---
title: "Symulacji urządzenia Azure IoT krawędzi (system Windows) | Dokumentacja firmy Microsoft"
description: "Jak używać krawędzi IoT Azure w systemie Windows, aby utworzyć symulowane urządzenie, który wysyła dane telemetryczne przez bramę Azure IoT krawędzi do Centrum IoT."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 6a2aeda0-696a-4732-90e1-595d2e2fadc6
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2017
ms.author: andbuc
ms.openlocfilehash: 0aa1836ee1445894022b95fefc2338ef53698240
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-windows"></a>Użyj Azure IoT krawędzi wysyłać urządzenia do chmury z symulowane urządzenie (system Windows)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="run-the-sample"></a>Uruchom próbki

**Build.cmd** skrypt generuje dane wyjściowe w **kompilacji** folderu w lokalnej kopii **krawędzi iot** repozytorium. Te dane wyjściowe obejmuje cztery moduły krawędzi IoT używane w tym przykładzie.

Skrypt kompilacji tworzy następujące pliki:

* **Logger.dll** w **kompilacji\\modułów\\rejestratora\\debugowania** folderu.
* **iothub.dll** w **kompilacji\\modułów\\Centrum iothub\\debugowania** folderu.
* **tożsamość\_map.dll** w **kompilacji\\modułów\\identitymap\\debugowania** folderu.
* **Symulowane\_device.dll** w **kompilacji\\modułów\\symulowane\_urządzenia\\debugowania** folderu.

Użycie tych ścieżek dla **ścieżka modułu** wartości, jak pokazano w symulowanych\_urządzenia\_chmury\_przekazać\_win JSON pliku ustawień.

Symulowane\_urządzenia\_chmury\_procesu przekazywania przykładowych przyjmuje ścieżkę do pliku konfiguracji JSON jako argument wiersza polecenia. Następujący przykładowy plik JSON znajduje się w repozytorium SDK pod adresem **przykłady\\symulowane\_urządzenia\_chmury\_przekazać\_próbki\\src\\ Symulowane\_urządzenia\_chmury\_przekazać\_win.json**. Ten plik konfiguracyjny działa jest chyba że zmodyfikujesz skrypt kompilacji do umieszczenia krawędzi IoT modułów lub przykładowych plików wykonywalnych w innych niż domyślne lokalizacje.

> [!NOTE]
> Ścieżki modułu są względem katalogu gdzie symulowane\_urządzenia\_chmury\_przekazać\_znajduje się sample.exe. Przykładowy plik konfiguracji JSON domyślnie zapisywania "deviceCloudUploadGatewaylog.log" w bieżącym katalogu roboczym.

W edytorze tekstów otwórz plik **przykłady\\symulowane\_urządzenia\_chmury\_przekazać\\src\\symulowane\_urządzenia\_chmury\_przekazać\_win.json** w lokalnej kopii **krawędzi iot** repozytorium. Ten plik konfiguruje modułów IoT krawędzi w bramie próbki:

* **Centrum IoTHub** łączy się z Centrum IoT z modułu. Można skonfigurować do wysyłania danych do Centrum IoT. W szczególności ustawić **IoTHubName** wartość nazwy Centrum IoT i ustaw **IoTHubSuffix** do wartości **azure devices.net**. Ustaw **transportu** wartość do jednego z: **HTTP**, **AMQP**, lub **MQTT**. Obecnie tylko **HTTP** udostępnia jedno połączenie TCP dla wszystkich wiadomości urządzenia. Jeśli wartość jest ustawiona **AMQP**, lub **MQTT**, bramy zachowuje oddzielne połączenie TCP z Centrum IoT dla każdego urządzenia.
* **Mapowania** modułu mapuje adresy MAC symulowane urządzeń do Twojego urządzenia IoT Hub identyfikatorów. Ustaw **deviceId** wartości identyfikatorów urządzeń dodane do Centrum IoT. Ustaw **deviceKey** wartości do dwóch urządzeń kluczy.
* **BLE1** i **BLE2** moduły są symulowanego urządzenia. Należy zwrócić uwagę, jak adresy MAC modułu pasują do adresów w **mapowania** modułu.
* **Rejestratora** modułu rejestruje działanie bramy w pliku.
* **Ścieżka modułu** wartości podane w poniższym przykładzie są względem katalogu gdzie symulowane\_urządzenia\_chmury\_przekazać\_znajduje się sample.exe.
* **Łącza** łączy tablicy w dolnej części pliku JSON **BLE1** i **BLE2** modułów **mapowania** modułu i **mapowania** moduł **Centrum IoTHub** modułu. Gwarantuje również, że wszystkie komunikaty są rejestrowane przez **rejestratora** modułu.

```json
{
    "modules" :
    [
      {
        "name": "IotHub",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\iothub\\Debug\\iothub.dll"
          }
          },
          "args": {
            "IoTHubName": "<<insert here IoTHubName>>",
            "IoTHubSuffix": "<<insert here IoTHubSuffix>>",
            "Transport": "HTTP"
          }
        },
      {
        "name": "mapping",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\identitymap\\Debug\\identity_map.dll"
          }
          },
          "args": [
            {
              "macAddress": "01:01:01:01:01:01",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            },
            {
              "macAddress": "02:02:02:02:02:02",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            }
          ]
        },
      {
        "name": "BLE1",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "01:01:01:01:01:01",
            "messagePeriod" : 2000
          }
        },
      {
        "name": "BLE2",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "02:02:02:02:02:02",
            "messagePeriod" : 2000
          }
        },
      {
        "name": "Logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
          }
        },
        "args": {
          "filename": "deviceCloudUploadGatewaylog.log"
        }
      }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IotHub" }
    ]
}
```

Zapisz zmiany wprowadzone w pliku konfiguracji.

Aby uruchomić przykład:

1. W wierszu polecenia przejdź do **kompilacji** folderu w lokalnej kopii **krawędzi iot** repozytorium.
2. Uruchom następujące polecenie:
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. Można użyć [explorer urządzenia] [ lnk-device-explorer] lub [explorer Centrum iothub] [ lnk-iothub-explorer] narzędzia do monitorowania wiadomości, które otrzymuje Centrum IoT z bramy. Na przykład za pomocą Eksploratora Centrum iothub można monitorować wiadomości urządzenia do chmury przy użyciu następującego polecenia:

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać większą wiedzę krawędzi IoT i wypróbować niektóre przykłady kodu, odwiedź następujące samouczki deweloperów i zasoby:

* [Wysyłanie wiadomości urządzenia do chmury z fizycznego urządzenia IoT krawędzi][lnk-physical-device]
* [Krawędź IoT Azure][lnk-iot-edge]

Aby dokładniej analizować możliwości Centrum IoT, zobacz:

* [Przewodnik dewelopera Centrum IoT][lnk-devguide]
* [Zabezpieczanie rozwiązania IoT od podstaw w górę][lnk-securing]

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
