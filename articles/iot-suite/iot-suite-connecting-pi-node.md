---
title: "Zapewnij malinowe Pi do monitorowania zdalnego w środowisku Node.js - Azure | Dokumentacja firmy Microsoft"
description: "Opisano sposób podłączania urządzeń Pi malina do wstępnie pakiet IoT Azure zdalnego monitorowania rozwiązania przy użyciu aplikacji napisanych w Node.js."
services: iot-suite
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
ms.date: 10/18/2017
ms.author: dobett
ms.openlocfilehash: d401dde25bf4ab430ac045fb6cfd90050a7ec2e7
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/31/2017
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Podłącz urządzenie Pi malina do zdalnego wstępnie skonfigurowane rozwiązanie monitorowania (Node.js)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Ten samouczek pokazuje, jak nawiązać zdalnego wstępnie skonfigurowane rozwiązanie monitorowania urządzenia fizycznego. W tym samouczku używamy Node.js, czyli dobra opcja w przypadku środowiska z ograniczeniami minimalnego zasobów.

### <a name="required-hardware"></a>Wymagany sprzęt

Komputer stacjonarny, aby włączyć zdalne łączenie się z wiersza polecenia na malina Pi.

[Microsoft IoT Starter Kit malina Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) lub równoważne składników. W tym samouczku wykorzystuje następujące składniki z zestawu:

- Pi malinowe 3
- Karta MicroSD (z NOOBS)
- Kabla USB Mini
- Kabla Ethernet

### <a name="required-desktop-software"></a>Wymagane oprogramowanie komputerowe

Należy klient SSH na komputerze pulpitu umożliwia zdalny dostęp do wiersza polecenia na malina Pi.

- System Windows nie zawiera klienta SSH. Firma Microsoft zaleca używanie [PuTTY](http://www.putty.org/).
- Większość dystrybucje systemu Linux i Mac OS obejmują narzędzia wiersza polecenia SSH. Aby uzyskać więcej informacji, zobacz [SSH za pomocą systemu Linux lub Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Wymagane oprogramowanie Pi malina

Jeśli jeszcze tego nie zrobiono tego wcześniej, zainstaluj środowiska Node.js w wersji 4.0.0 lub nowszego z malina Pi. Poniższe kroki pokazują, jak zainstalować Node.js v6.11.4 na Twoje Pi malina:

1. Nawiązać połączenie przy użyciu Pi malina `ssh`. Aby uzyskać więcej informacji, zobacz [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) na [witryny sieci Web Pi malina](https://www.raspberrypi.org/).

1. Można zaktualizować Twojego Pi malina, użyj następującego polecenia:

    ```sh
    sudo apt-get update
    ```

1. Podczas pobierania plików binarnych Node.js z Pi malina, użyj następującego polecenia:

    ```sh
    wget https://nodejs.org/dist/v6.11.4/node-v6.11.4-linux-armv7l.tar.gz
    ```

1. Aby zainstalować pliki binarne, użyj następującego polecenia:

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.11.4-linux-armv7l.tar.gz
    ```

1. Użyj następującego polecenia, aby sprawdzić, czy pomyślnie zainstalowano Node.js v6.11.4:

    ```sh
    node --version
    ```

## <a name="create-a-nodejs-solution"></a>Tworzenie rozwiązania Node.js

Wykonaj następujące czynności, za pomocą `ssh` połączenie z Pi malina:

1. Utwórz folder o nazwie `RemoteMonitoring` w folderze macierzysty na malina Pi. Przejdź do tego folderu, w wierszu polecenia:

    ```sh
    cd ~
    mkdir RemoteMonitoring
    cd RemoteMonitoring
    ```

1. Aby pobrać i zainstalować te pakiety, które należy wykonać przykładową aplikację, uruchom następujące polecenia:

    ```sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. W `RemoteMonitoring` folderu, Utwórz plik o nazwie **remote_monitoring.js**. Otwórz ten plik w edytorze tekstu. Można użyć na Pi malina `nano` lub `vi` edytory tekstów.

1. W **remote_monitoring.js** plików, należy dodać następujące `require` instrukcji:

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. Dodaj następujące deklaracje zmiennych po instrukcji `require`. Zastąp symbole zastępcze `{Device Id}` i `{Device Key}` wartościami zauważyć urządzenia udostępnione w zdalnym rozwiązanie monitorowania. Użyj nazwy hosta Centrum IoT z rozwiązania, aby zamienić `{IoTHub Name}`. Na przykład, jeśli jest Twojej nazwy hosta Centrum IoT `contoso.azure-devices.net`, Zastąp `{IoTHub Name}` z `contoso`:

    ```nodejs
    var connectionString = 'HostName={IoTHub Name}.azure-devices.net;DeviceId={Device Id};SharedAccessKey={Device Key}';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Aby zdefiniować niektóre dane telemetryczne podstawowej, Dodaj następujące zmienne:

    ```nodejs
    var temperature = 50;
    var temperatureUnit = 'F';
    var humidity = 50;
    var humidityUnit = '%';
    var pressure = 55;
    var pressureUnit = 'psig';
    ```

1. Aby zdefiniować niektórych wartości właściwości, należy dodać następujące zmienne:

    ```nodejs
    var temperatureSchema = 'chiller-temperature;v1';
    var humiditySchema = 'chiller-humidity;v1';
    var pressureSchema = 'chiller-pressure;v1';
    var interval = "00:00:05";
    var deviceType = "Chiller";
    var deviceFirmware = "1.0.0";
    var deviceFirmwareUpdateStatus = "";
    var deviceLocation = "Building 44";
    var deviceLatitude = 47.638928;
    var deviceLongitude = -122.13476;
    ```

1. Dodaj następującą zmienną do definiowania właściwości zgłoszony do wysłania do rozwiązania. Te właściwości obejmują metadanych opisujących metod i używa telemetrii urządzenia:

    ```nodejs
    var reportedProperties = {
      "Protocol": "MQTT",
      "SupportedMethods": "Reboot,FirmwareUpdate,EmergencyValveRelease,IncreasePressure",
      "Telemetry": {
        "TemperatureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\"}",
          "MessageSchema": {
            "Name": temperatureSchema,
            "Format": "JSON",
            "Fields": {
              "temperature": "Double",
              "temperature_unit": "Text"
            }
          }
        },
        "HumiditySchema": {
          "Interval": interval,
          "MessageTemplate": "{\"humidity\":${humidity},\"humidity_unit\":\"${humidity_unit}\"}",
          "MessageSchema": {
            "Name": humiditySchema,
            "Format": "JSON",
            "Fields": {
              "humidity": "Double",
              "humidity_unit": "Text"
            }
          }
        },
        "PressureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"pressure\":${pressure},\"pressure_unit\":\"${pressure_unit}\"}",
          "MessageSchema": {
            "Name": pressureSchema,
            "Format": "JSON",
            "Fields": {
              "pressure": "Double",
              "pressure_unit": "Text"
            }
          }
        }
      },
      "Type": deviceType,
      "Firmware": deviceFirmware,
      "FirmwareUpdateStatus": deviceFirmwareUpdateStatus,
      "Location": deviceLocation,
      "Latitude": deviceLatitude,
      "Longitude": deviceLongitude
    }
    ```

1. Aby wydrukować wyniki operacji, należy dodać funkcję pomocnika następujące:

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

1. Dodanie następujących funkcji do wywołania metody bezpośrednio z rozwiązania do obsługi. W tym rozwiązaniu zastosowano bezpośredniego metod do działania na urządzeniach:

    ```nodejs
    function onDirectMethod(request, response) {
      // Implement logic asynchronously here.
      console.log('Simulated ' + request.methodName);

      // Complete the response
      response.send(200, request.methodName + ' was called on the device', function (err) {
        if (!!err) {
          console.error('An error ocurred when sending a method response:\n' +
            err.toString());
        } else {
          console.log('Response to method \'' + request.methodName +
            '\' sent successfully.');
        }
      });
    }
    ```

1. Dodaj następujący kod do wysyłania danych telemetrycznych do rozwiązania. Aplikacja kliencka dodanie właściwości do komunikat, aby zidentyfikować schematu komunikat:

    ```node.js
    function sendTelemetry(data, schema) {
      var d = new Date();
      var payload = JSON.stringify(data);
      var message = new Message(payload);
      message.properties.add('$$CreationTimeUtc', d.toISOString());
      message.properties.add('$$MessageSchema', schema);
      message.properties.add('$$ContentType', 'JSON');

      console.log('Sending device message data:\n' + payload);
      client.sendEvent(message, printErrorFor('send event'));
    }
    ```

1. Dodaj następujący kod, aby utworzyć wystąpienie klienta:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Dodaj następujący kod, aby:

    - Otwórz połączenie.
    - Konfigurowanie obsługi dla żądanej właściwości.
    - Wysłać zgłoszonego właściwości.
    - Rejestrowanie procedur obsługi dla metod bezpośredniego.
    - Rozpocznij wysyłanie danych telemetrycznych.

    ```nodejs
    client.open(function (err) {
      if (err) {
        printErrorFor('open')(err);
      } else {
        // Create device Twin
        client.getTwin(function (err, twin) {
          if (err) {
            console.error('Could not get device twin');
          } else {
            console.log('Device twin created');

            twin.on('properties.desired', function (delta) {
              // Handle desired properties set by solution
              console.log('Received new desired properties:');
              console.log(JSON.stringify(delta));
            });

            // Send reported properties
            twin.properties.reported.update(reportedProperties, function (err) {
              if (err) throw err;
              console.log('twin state reported');
            });

            // Register handlers for all the method names we are interested in.
            // Consider separate handlers for each method.
            client.onDeviceMethod('Reboot', onDirectMethod);
            client.onDeviceMethod('FirmwareUpdate', onDirectMethod);
            client.onDeviceMethod('EmergencyValveRelease', onDirectMethod);
            client.onDeviceMethod('IncreasePressure', onDirectMethod);
          }
        });

        // Start sending telemetry
        var sendTemperatureInterval = setInterval(function () {
          temperature += generateRandomIncrement();
          var data = {
            'temperature': temperature,
            'temperature_unit': temperatureUnit
          };
          sendTelemetry(data, temperatureSchema)
        }, 5000);

        var sendHumidityInterval = setInterval(function () {
          humidity += generateRandomIncrement();
          var data = {
            'humidity': humidity,
            'humidity_unit': humidityUnit
          };
          sendTelemetry(data, humiditySchema)
        }, 5000);

        var sendPressureInterval = setInterval(function () {
          pressure += generateRandomIncrement();
          var data = {
            'pressure': pressure,
            'pressure_unit': pressureUnit
          };
          sendTelemetry(data, pressureSchema)
        }, 5000);

        client.on('error', function (err) {
          printErrorFor('client')(err);
          if (sendTemperatureInterval) clearInterval(sendTemperatureInterval);
          if (sendHumidityInterval) clearInterval(sendHumidityInterval);
          if (sendPressureInterval) clearInterval(sendPressureInterval);
          client.close(printErrorFor('client.close'));
        });
      }
    });
    ```

1. Zapisać zmiany w **remote_monitoring.js** pliku.

1. Aby uruchomić przykładową aplikację, uruchom następujące polecenie z wiersza polecenia na Pi malina:

    ```sh
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
