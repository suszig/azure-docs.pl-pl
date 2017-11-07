---
title: "Połączenia z urządzeniem przy użyciu środowiska Node.js | Dokumentacja firmy Microsoft"
description: "Opisuje sposób podłącz urządzenie do wstępnie pakiet IoT Azure zdalnego monitorowania rozwiązania przy użyciu aplikacji napisanych w Node.js."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 87a2e97638508eef1d90a219cfb38d1fcac81d55
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Podłącz urządzenie do zdalnego wstępnie skonfigurowane rozwiązanie monitorowania (Node.js)
[!INCLUDE [iot-suite-v1-selector-connecting](../../includes/iot-suite-v1-selector-connecting.md)]

## <a name="create-a-nodejs-sample-solution"></a>Tworzenie rozwiązania próbki node.js

Upewnij się, tej wersji środowiska Node.js 0.11.5 lub nowszy jest zainstalowany na komputerze deweloperskim. Można uruchomić `node --version` w wierszu polecenia, aby sprawdzić wersję.

1. Utwórz folder o nazwie **RemoteMonitoring** na komputerze deweloperskim. Przejdź do tego folderu w środowisku wiersza polecenia.

1. Uruchom następujące polecenia, aby pobrać i zainstalować te pakiety, potrzebne do ukończenia przykładową aplikację:

    ```
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. W **RemoteMonitoring** folderu, Utwórz plik o nazwie **remote_monitoring.js**. Otwórz ten plik w edytorze tekstu.

1. W **remote_monitoring.js** plików, należy dodać następujące `require` instrukcji:

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. Dodaj następujące deklaracje zmiennych po instrukcji `require`. Zastąp wartości zastępcze [Identyfikator urządzenia] i [Klucz urządzenia] wartościami wymienionymi dla urządzenia na zdalnym pulpicie nawigacyjnym rozwiązania. Użyj nazwy hosta usługi IoT Hub z pulpitu nawigacyjnego rozwiązania w celu zastąpienia wartości zastępczej [Nazwa usługi IoTHub]. Jeśli na przykład host usługi IoT Hub ma nazwę **contoso.azure-devices.net**, zastąp wartość zastępczą [Nazwa usługi IoTHub] ciągiem **contoso**:

    ```nodejs
    var connectionString = 'HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Dodaj następujące zmienne do definiowania niektóre dane telemetryczne podstawowej:

    ```nodejs
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    ```

1. Dodaj następującą funkcję pomocnika drukowanie wyników operacji:

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Dodaj następującą funkcję pomocnika służące do wartości danych telemetrycznych losowe:

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. Dodaj następującą definicję dla **DeviceInfo** obiekt urządzenie wysyła podczas uruchamiania:

    ```nodejs
    var deviceMetaData = {
        'ObjectType': 'DeviceInfo',
        'IsSimulatedDevice': 0,
        'Version': '1.0',
        'DeviceProperties': {
            'DeviceID': deviceId,
            'HubEnabledState': 1
        }
    };
    ```

1. Dodaj następującą definicję dwie urządzenia zgłoszone wartości. Ta definicja zawiera opisy bezpośredniego metody, które obsługuje urządzenia:

    ```nodejs
    var reportedProperties = {
        "Device": {
            "DeviceState": "normal",
            "Location": {
                "Latitude": 47.642877,
                "Longitude": -122.125497
            }
        },
        "Config": {
            "TemperatureMeanValue": 56.7,
            "TelemetryInterval": 45
        },
        "System": {
            "Manufacturer": "Contoso Inc.",
            "FirmwareVersion": "2.22",
            "InstalledRAM": "8 MB",
            "ModelNumber": "DB-14",
            "Platform": "Plat 9.75",
            "Processor": "i3-9",
            "SerialNumber": "SER99"
        },
        "Location": {
            "Latitude": 47.642877,
            "Longitude": -122.125497
        },
        "SupportedMethods": {
            "Reboot": "Reboot the device",
            "InitiateFirmwareUpdate--FwPackageURI-string": "Updates device Firmware. Use parameter FwPackageURI to specifiy the URI of the firmware file"
        },
    }
    ```

1. Dodaj następującą funkcję obsługi **ponowny rozruch** bezpośrednie wywołanie metody:

    ```nodejs
    function onReboot(request, response) {
        // Implement actual logic here.
        console.log('Simulated reboot...');

        // Complete the response
        response.send(200, "Rebooting device", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

1. Dodaj następującą funkcję obsługi **InitiateFirmwareUpdate** bezpośrednie wywołania metody. Ta metoda bezpośrednia używa parametru do określenia lokalizacji pobierania obrazu oprogramowania układowego i inicjuje oprogramowanie układowe aktualizacji na urządzeniu asynchronicznie:

    ```nodejs
    function onInitiateFirmwareUpdate(request, response) {
        console.log('Simulated firmware update initiated, using: ' + request.payload.FwPackageURI);

        // Complete the response
        response.send(200, "Firmware update initiated", function(err) {
            if(!!err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });

        // Add logic here to perform the firmware update asynchronously
    }
    ```

1. Dodaj następujący kod, aby utworzyć wystąpienie klienta:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Dodaj następujący kod, aby:

    * Otwórz połączenie.
    * Wyślij **DeviceInfo** obiektu.
    * Konfigurowanie obsługi dla żądanej właściwości.
    * Wysłać zgłoszonego właściwości.
    * Rejestrowanie procedur obsługi dla metod bezpośredniego.
    * Rozpocznij wysyłanie danych telemetrycznych.

    ```nodejs
    client.open(function (err) {
        if (err) {
            printErrorFor('open')(err);
        } else {
            console.log('Sending device metadata:\n' + JSON.stringify(deviceMetaData));
            client.sendEvent(new Message(JSON.stringify(deviceMetaData)), printErrorFor('send metadata'));

            // Create device twin
            client.getTwin(function(err, twin) {
                if (err) {
                    console.error('Could not get device twin');
                } else {
                    console.log('Device twin created');

                    twin.on('properties.desired', function(delta) {
                        console.log('Received new desired properties:');
                        console.log(JSON.stringify(delta));
                    });

                    // Send reported properties
                    twin.properties.reported.update(reportedProperties, function(err) {
                        if (err) throw err;
                        console.log('twin state reported');
                    });

                    // Register handlers for direct methods
                    client.onDeviceMethod('Reboot', onReboot);
                    client.onDeviceMethod('InitiateFirmwareUpdate', onInitiateFirmwareUpdate);
                }
            });

            // Start sending telemetry
            var sendInterval = setInterval(function () {
                temperature += generateRandomIncrement();
                externalTemperature += generateRandomIncrement();
                humidity += generateRandomIncrement();

                var data = JSON.stringify({
                    'DeviceID': deviceId,
                    'Temperature': temperature,
                    'Humidity': humidity,
                    'ExternalTemperature': externalTemperature
                });

                console.log('Sending device event data:\n' + data);
                client.sendEvent(new Message(data), printErrorFor('send event'));
            }, 5000);

            client.on('error', function (err) {
                printErrorFor('client')(err);
                if (sendInterval) clearInterval(sendInterval);
                client.close(printErrorFor('client.close'));
            });
        }
    });
    ```

1. Zapisać zmiany w **remote_monitoring.js** pliku.

1. Uruchom następujące polecenie w wierszu polecenia, aby uruchomić przykładową aplikację:
   
    ```
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-v1-visualize-connecting](../../includes/iot-suite-v1-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdk-node
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
