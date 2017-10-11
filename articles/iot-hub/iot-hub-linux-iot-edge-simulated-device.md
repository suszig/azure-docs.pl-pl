---
title: "Symulacji urządzenia Azure IoT krawędzi (Linux) | Dokumentacja firmy Microsoft"
description: "Jak używać krawędzi IoT Azure w systemie Linux, aby utworzyć symulowane urządzenie, który wysyła dane telemetryczne za pośrednictwem bramy krawędzi IoT z Centrum IoT."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 11e7bf28-ee3d-48d6-a386-eb506c7a31cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.openlocfilehash: 5349960373ae6815862c5f79a69dd6d5d9d624ab
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-linux"></a>Wysyłać urządzenia do chmury z symulowane urządzenie (Linux) za pomocą krawędzi IoT Azure

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>Jak uruchomić przykład

Skrypt **build.sh** generuje swoje dane wyjściowe w folderze **build** w lokalnej kopii repozytorium **iot-edge**. Te dane wyjściowe obejmuje cztery moduły krawędzi IoT używane w tym przykładzie.

Miejsca skryptu kompilacji:

* **liblogger.so** w **kompilacji/moduły/rejestratora** folderu.
* **libiothub.so** w **kompilacji/moduły/Centrum iothub** folderu.
* **lib\_tożsamości\_map.so** w **kompilacji/moduły/identitymap** folderu.
* **libsimulated\_device.so** w **kompilacji/moduły/symulowane\_urządzenia** folderu.

Użyj tych ścieżek dla **ścieżka modułu** wartości, jak pokazano w następującym pliku ustawień JSON:

Symulowane\_urządzenia\_chmury\_przekazać\_przykładowy proces przyjmuje ścieżkę do pliku konfiguracji JSON jako argument wiersza polecenia. Następujący przykładowy plik JSON znajduje się w repozytorium SDK pod adresem **przykłady\\symulowane\_urządzenia\_chmury\_przekazać\_próbki\\src\\ Symulowane\_urządzenia\_chmury\_przekazać\_próbki\_lin.json**. Ten plik konfiguracyjny działa jest chyba że zmodyfikujesz skrypt kompilacji do umieszczenia krawędzi IoT modułów lub przykładowych plików wykonywalnych w innych niż domyślne lokalizacje.

> [!NOTE]
> Ścieżki modułu są względem katalogu, z którym jest uruchomiony symulowane\_urządzenia\_chmury\_przekazać\_przykładowy plik wykonywalny, nie katalog, w którym znajduje się plik wykonywalny. Przykładowy plik konfiguracji JSON domyślnie zapisywania "deviceCloudUploadGatewaylog.log" w bieżącym katalogu roboczym.

W edytorze tekstów otwórz plik **przykłady/symulowane\_urządzenia\_chmury\_przekazać\_próbki/src/symulowane\_urządzenia\_chmury\_przekazać\_lin.json** w lokalnej kopii **krawędzi iot** repozytorium. Ten plik konfiguruje modułów IoT krawędzi w bramie próbki:

* **Centrum IoTHub** łączy się z Centrum IoT z modułu. Można skonfigurować do wysyłania danych do Centrum IoT. W szczególności ustawić **IoTHubName** wartość nazwy Centrum IoT i ustaw **IoTHubSuffix** do wartości **azure devices.net**. Ustaw **transportu** wartość do jednego z: **HTTP**, **AMQP**, lub **MQTT**. Obecnie tylko **HTTP** udostępnia jedno połączenie TCP dla wszystkich wiadomości urządzenia. Jeśli wartość jest ustawiona **AMQP**, lub **MQTT**, bramy zachowuje oddzielne połączenie TCP z Centrum IoT dla każdego urządzenia.
* **Mapowania** modułu mapuje adresy MAC symulowane urządzeń identyfikatorom urządzenia IoT Hub. Upewnij się, że **deviceId** wartości zgodnie z identyfikatorami urządzeń dwóch dodane do Centrum IoT oraz że **deviceKey** wartości zawierają klucze dwa urządzeń.
* **BLE1** i **BLE2** moduły są symulowanego urządzenia. Należy zwrócić uwagę, jak ich adresów MAC pasują do adresów w **mapowania** modułu.
* **Rejestratora** modułu rejestruje działanie bramy w pliku.
* **Ścieżka modułu** wartości pokazano w przykładzie założono, że uruchamiasz próbki z **kompilacji** folderu w lokalnej kopii **krawędzi iot** repozytorium.
* **Łącza** łączy tablicy w dolnej części pliku JSON **BLE1** i **BLE2** modułów **mapowania** modułu i **mapowania** moduł **Centrum IoTHub** modułu. Gwarantuje również, że wszystkie komunikaty są rejestrowane przez **rejestratora** modułu.

```json
{
    "modules": [
        {
            "name": "IotHub",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/iothub/libiothub.so"
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
              "module.path": "./modules/identitymap/libidentity_map.so"
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
              "module.path": "./modules/simulated_device/libsimulated_device.so"
            }
            },
            "args": {
              "macAddress": "01:01:01:01:01:01"
            }
          },
        {
            "name": "BLE2",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/simulated_device/libsimulated_device.so"
            }
            },
            "args": {
              "macAddress": "02:02:02:02:02:02"
            }
          },
        {
            "name": "Logger",
          "loader": {
            "name": "native",
            "entrypoint": {
              "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args": {
              "filename": "deviceCloudUploadGatewaylog.log"
            }
          }
    ],
    "links": [
        {
            "source": "*",
            "sink": "Logger"
        },
        {
            "source": "BLE1",
            "sink": "mapping"
        },
        {
            "source": "BLE2",
            "sink": "mapping"
        },
        {
            "source": "mapping",
            "sink": "IotHub"
        }
    ]
}
```

Zapisz zmiany wprowadzone w pliku konfiguracji.

Aby uruchomić przykład:

1. W powłoki, przejdź do **iot krawędzi/kompilacji** folderu.
2. Uruchom następujące polecenie:
   
    ```sh
    ./samples/simulated_device_cloud_upload/simulated_device_cloud_upload_sample ../samples/simulated_device_cloud_upload/src/simulated_device_cloud_upload_lin.json
    ```
3. Można użyć [explorer urządzenia] [ lnk-device-explorer] lub [explorer Centrum iothub] [ lnk-iothub-explorer] narzędzia do monitorowania wiadomości, które otrzymuje Centrum IoT z bramy. Na przykład za pomocą Eksploratora Centrum iothub można monitorować wiadomości urządzenia do chmury przy użyciu następującego polecenia:

    ```sh
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać większą wiedzę Azure IoT krawędzi i wypróbować niektóre przykłady kodu, odwiedź następujące samouczki deweloperów i zasoby:

* [Wysyłać urządzenia do chmury z fizycznego urządzenia z krawędzią IoT Azure][lnk-physical-device]
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
