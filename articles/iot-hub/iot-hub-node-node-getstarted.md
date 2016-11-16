---
title: "Usługa Azure IoT Hub dla środowiska Node.js — wprowadzenie | Microsoft Docs"
description: "Samouczek zawierający wprowadzenie do usługi Azure IoT Hub dla środowiska Node.js. Użycie usługi Azure IoT Hub i środowiska Node.js z zestawami SDK IoT Microsoft Azure w celu wdrożenia rozwiązania Internetu rzeczy."
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 56618522-9a31-42c6-94bf-55e2233b39ac
ms.service: iot-hub
ms.devlang: javascript
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7ddd9c1ed88e30eaaa200dc6b83d34746da14aa8


---
# <a name="get-started-with-azure-iot-hub-for-nodejs"></a>Usługa Azure IoT Hub dla środowiska Node.js — wprowadzenie
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Po ukończeniu tego samouczka będziesz mieć trzy aplikacje konsolowe środowiska Node.js:

* **CreateDeviceIdentity.js** tworzy tożsamość urządzenia i skojarzony klucz zabezpieczeń w celu podłączenia urządzenia symulowanego.
* **ReadDeviceToCloudMessages.js** powoduje wyświetlenie telemetrii wysyłanej przez urządzenie symulowane.
* **SimulatedDevice.js** łączy się z centrum IoT przy użyciu utworzonej wcześniej tożsamości urządzenia i co sekundę wysyła komunikat telemetrii przy użyciu protokołu AMQP.

> [!NOTE]
> Artykuł [IoT Hub SDKs] (Zestawy SDK usługi IoT Hub)[lnk-hub-sdks] zawiera informacje dotyczące różnych zestawów SDK, których można użyć do utworzenia aplikacji zarówno do uruchamiania na urządzaniach, jak i w zapleczu rozwiązania.
> 
> 

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Środowisko Node.js w wersji 0.10.x lub nowszej.
* Aktywne konto platformy Azure. (Jeśli go nie masz, możesz utworzyć [bezpłatne konto][lnk-free-trial] w zaledwie kilka minut).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Utworzono centrum IoT. Istnieje nazwa hosta centrum IoT oraz parametry połączenia centrum IoT potrzebne do ukończenia pozostałej części tego samouczka.

## <a name="create-a-device-identity"></a>Tworzenie tożsamości urządzenia
W tej sekcji utworzysz aplikację konsolową środowiska Node.js, która tworzy tożsamość urządzenia w rejestrze tożsamości w centrum IoT. Urządzenie nie może połączyć się z centrum IoT, jeśli nie ma wpisu w rejestrze tożsamości urządzeń. Więcej informacji znajduje się w sekcji **Device Identity Registry** (Rejestr tożsamości urządzenia) w artykule [IoT Hub Developer Guide] (Usługa IoT Hub — przewodnik dewelopera)[lnk-devguide-identity]. Po uruchomieniu ta aplikacja konsolowa generuje unikatowy identyfikator i klucz urządzenia, których urządzenie może użyć do zidentyfikowania się podczas wysyłania komunikatów do chmury do usługi IoT Hub.

1. Utwórz nowy pusty folder o nazwie **createdeviceidentity**. W folderze **createdeviceidentity** utwórz plik package.json przy użyciu następującego polecenia z poziomu wiersza polecenia. Zaakceptuj wszystkie ustawienia domyślne:
   
    ```
    npm init
    ```
2. Z poziomu wiersza polecenia w folderze **createdeviceidentity** uruchom następujące polecenie, aby zainstalować pakiet zestawu SDK usługi **azure-iothub**:
   
    ```
    npm install azure-iothub --save
    ```
3. Za pomocą edytora tekstu utwórz plik **CreateDeviceIdentity.js** w folderze **createdeviceidentity**.
4. Dodaj następującą instrukcję `require` na początku pliku **CreateDeviceIdentity.js**:
   
    ```
    'use strict';
   
    var iothub = require('azure-iothub');
    ```
5. Dodaj następujący kod do pliku **CreateDeviceIdentity.js** i zastęp wartość symbolu zastępczego parametrami połączenia centrum IoT utworzonego w poprzedniej sekcji: 
   
    ```
    var connectionString = '{iothub connection string}';
   
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```
6. Dodaj następujący kod, aby utworzyć definicję urządzenia w rejestrze tożsamości urządzeń w centrum IoT. Ten kod tworzy urządzenie, jeśli identyfikator urządzenia nie istnieje w rejestrze. W przeciwnym razie zwraca klucz istniejącego urządzenia:
   
    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });
   
    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```
7. Zapisz i zamknij plik **CreateDeviceIdentity.js**.
8. Aby uruchomić aplikację **createdeviceidentity**, wykonaj następujące polecenie w wierszu polecenia w folderze createdeviceidentity:
   
    ```
    node CreateDeviceIdentity.js 
    ```
9. Zanotuj **identyfikator urządzenia** i **klucz urządzenia**. Te wartości będą potrzebne później podczas tworzenia aplikacji, która łączy się z usługą IoT Hub jako urządzenie.

> [!NOTE]
> Rejestr tożsamości usługi IoT Hub przechowuje tożsamości urządzenia tylko po to, aby umożliwić bezpieczny dostęp do centrum. Przechowuje identyfikatory i klucze urządzeń, które będą używane jako poświadczenia zabezpieczeń, oraz flagę włączone/wyłączone, która umożliwia wyłączenie dostępu do poszczególnych urządzeń. Jeśli aplikacja wymaga przechowywania innych metadanych dla określonego urządzenia, powinna korzystać z magazynu określonego dla aplikacji. Więcej informacji znajduje się w artykule [IoT Hub Developer Guide] (Usługa IoT Hub — przewodnik dewelopera)[lnk-devguide-identity].
> 
> 

## <a name="receive-devicetocloud-messages"></a>Odbieranie komunikatów z urządzenia do chmury
W tej sekcji opisano tworzenie aplikacji konsolowej środowiska Node.js, która odczytuje komunikaty z urządzenia do chmury z usługi IoT Hub. Centrum IoT udostępnia punkt końcowy zgodny z usługą [Event Hubs][lnk-event-hubs-overview], aby umożliwić odczytywanie komunikatów z urządzenia do chmury. W celu uproszczenia informacji instrukcje w samouczku dotyczą tworzenia czytnika podstawowego, który nie jest odpowiedni do wdrożenia z użyciem dużej przepustowości. W samouczku [Process device-to-cloud messages] (Przetwarzanie komunikatów z urządzenia do chmury)[lnk-process-d2c-tutorial] przedstawiono sposób przetwarzania komunikatów z urządzenia do chmury na dużą skalę. Samouczek [Get Started with Event Hubs] (Usługa Event Hubs — wprowadzenie)[lnk-eventhubs-tutorial] zawiera dalsze informacje dotyczące sposobu przetwarzania komunikatów z usługi Event Hubs i dotyczy punktów końcowych usługi IoT Hub zgodnych z centrami zdarzeń.

> [!NOTE]
> Punkt końcowy zgodny z centrum zdarzeń przeznaczony do odczytywania komunikatów z urządzenia do chmury zawsze korzysta z protokołu AMQP.
> 
> 

1. Utwórz nowy pusty folder o nazwie **readdevicetocloudmessages**. W folderze **readdevicetocloudmessages** utwórz plik package.json przy użyciu następującego polecenia z poziomu wiersza polecenia. Zaakceptuj wszystkie ustawienia domyślne:
   
    ```
    npm init
    ```
2. Z poziomu wiersza polecenia w folderze **readdevicetocloudmessages** uruchom następujące polecenie, aby zainstalować pakiet **azure-event-hubs**:
   
    ```
    npm install azure-event-hubs --save
    ```
3. Za pomocą edytora tekstu utwórz plik **ReadDeviceToCloudMessages.js** w folderze **readdevicetocloudmessages**.
4. Dodaj następujące instrukcję `require` na początku pliku **ReadDeviceToCloudMessages.js**:
   
    ```
    'use strict';
   
    var EventHubClient = require('azure-event-hubs').Client;
    ```
5. Dodaj następującą deklarację zmiennej i zastąp wartość symbolu zastępczego parametrami połączenia dla centrum IoT:
   
    ```
    var connectionString = '{iothub connection string}';
    ```
6. Dodaj następujące dwie funkcje, które drukują dane wyjściowe do konsoli:
   
    ```
    var printError = function (err) {
      console.log(err.message);
    };
   
    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```
7. Dodaj następujący kod, aby utworzyć element **EventHubClient**, otwórz połączenie z centrum IoT, a następnie utwórz odbiornik dla każdej partycji. Ta aplikacja używa filtru podczas tworzenia odbiornika, więc odbiornik odczytuje tylko komunikaty wysyłane do usługi Centrum IoT po uruchomieniu odbiornika. Ten filtr jest przydatny w środowisku testowym, gdyż dzięki niemu jest wyświetlany tylko bieżący zestaw komunikatów. W środowisku produkcyjnym kod powinien sprawdzać, czy są przetwarzane wszystkie komunikaty — więcej informacji znajduje się w samouczku [How to process IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial] (Jak przetwarzać komunikaty z urządzenia do chmury w usłudze IoT Hub):
   
    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```
8. Zapisz i zamknij plik **ReadDeviceToCloudMessages.js**.

## <a name="create-a-simulated-device-app"></a>Tworzenie aplikacji symulowanego urządzenia
Ta sekcja zawiera instrukcje dotyczące tworzenia aplikacji konsolowej środowiska Node.js, która symuluje urządzenie wysyłające komunikaty z urządzenia do chmury do centrum IoT.

1. Utwórz nowy pusty folder o nazwie **simulateddevice**. W folderze **simulateddevice** utwórz plik package.json przy użyciu następującego polecenia z poziomu wiersza polecenia. Zaakceptuj wszystkie ustawienia domyślne:
   
    ```
    npm init
    ```
2. Z poziomu wiersza polecenia w folderze **simulateddevice** uruchom następujące polecenie, aby zainstalować pakiet zestawu SDK urządzenia **azure-iot-device** i pakiet **azure-iot-device-amqp**:
   
    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```
3. Za pomocą edytora tekstu utwórz nowy plik **SimulatedDevice.js** w folderze **simulateddevice**.
4. Dodaj następujące instrukcje `require` na początku pliku **SimulatedDevice.js**:
   
    ```
    'use strict';
   
    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```
5. Dodaj zmienną **connectionString** i użyj jej do utworzenia klienta urządzenia. Zastąp **{youriothostname}** nazwą centrum IoT utworzonego w sekcji *Tworzenie centrum IoT*. Zastąp **{yourdevicekey}** wartością klucza urządzenia wygenerowanego w sekcji *Tworzenie tożsamości urządzenia*:
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
   
    var client = clientFromConnectionString(connectionString);
    ```
6. Dodaj następującą funkcję, aby wyświetlić dane wyjściowe z aplikacji:
   
    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Utwórz wywołanie zwrotne i użyj funkcji **setInterval**, aby co sekundę wysyłać nowy komunikat do centrum IoT:
   
    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
   
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
8. Otwórz połączenie z centrum IoT i rozpocznij wysyłanie komunikatów:
   
    ```
    client.open(connectCallback);
    ```
9. Zapisz i zamknij plik **SimulatedDevice.js**.

> [!NOTE]
> Dla uproszczenia ten samouczek nie zawiera opisu wdrożenia żadnych zasad ponawiania. W kodzie produkcyjnym należy wdrożyć zasady ponawiania (np. wycofywanie wykładnicze) zgodnie z sugestią w artykule z witryny MSDN [Transient Fault Handling] (Obsługa przejściowego błędu)[lnk-transient-faults].
> 
> 

## <a name="run-the-applications"></a>Uruchamianie aplikacji
Teraz można uruchomić aplikacje.

1. Z poziomu wiersza polecenia w folderze **readdevicetocloudmessages** uruchom następujące polecenie, aby rozpocząć monitorowanie centrum IoT:
   
    ```
    node ReadDeviceToCloudMessages.js 
    ```
   
    ![Aplikacja kliencka usługi IoT Hub dla środowiska Node.js do monitorowania komunikatów między urządzeniem i chmurą][7]
2. Z poziomu wiersza polecenia w folderze **simulateddevice** uruchom następujące polecenie, aby rozpocząć wysyłanie danych telemetrycznych do centrum IoT:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![Aplikacja kliencka urządzenia usługi IoT Hub dla środowiska Node.js do wysyłania komunikatów między urządzeniem i chmurą][8]
3. Na kafelku **Użycie** w [portalu Azure][lnk-portal] wyświetlana jest liczba komunikatów wysłanych do centrum:
   
    ![Kafelek Użycie witryny Azure Portal przedstawiający liczbę komunikatów wysłanych do usługi IoT Hub][43]

## <a name="next-steps"></a>Następne kroki
W tym samouczku opisano konfigurowanie nowego centrum IoT w witrynie Azure Portal, a następnie tworzenie tożsamości urządzenia w rejestrze tożsamości centrum. Tożsamość urządzenia została użyta, aby włączyć w aplikacji symulowanego urządzenia funkcję wysyłania komunikatów z urządzenia do chmury do centrum. Utworzono również aplikację, która wyświetla komunikaty odbierane przez centrum. 

Aby kontynuować wprowadzenie do usługi IoT Hub i zapoznać się z innymi scenariuszami IoT, zobacz:

* [Łączenie urządzenia][lnk-connect-device]
* [Wprowadzenie do zarządzania urządzeniami][lnk-device-management]
* [Wprowadzenie do zestawu SDK bramy usługi IoT][lnk-gateway-SDK]

W samouczku [Process device-to-cloud messages][lnk-process-d2c-tutorial] (Przetwarzanie komunikatów z urządzenia do chmury) przedstawiono, jak rozszerzyć rozwiązanie IoT i przetwarzać komunikaty z urządzenia do chmury na dużą skalę.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/



<!--HONumber=Nov16_HO2-->


