<properties
    pageTitle="Usługa Azure IoT Hub dla środowiska Node.js — wprowadzenie | Microsoft Azure"
    description="Samouczek zawierający wprowadzenie do usługi Azure IoT Hub dla środowiska Node.js. Użycie usługi Azure IoT Hub i środowiska Node.js z zestawami SDK IoT Microsoft Azure w celu wdrożenia rozwiązania Internetu rzeczy. "
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="03/22/2016"
     ms.author="dobett"/>

# Usługa Azure IoT Hub dla środowiska Node.js — wprowadzenie

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

## Wprowadzenie

Azure IoT Hub to w pełni zarządzana usługa, która umożliwia bezpieczną i niezawodną komunikację dwukierunkową między milionami urządzeń Internetu rzeczy (IoT) i zapleczem rozwiązania. Jedno z największych wyzwań w projektach IoT polega na tym, jak w sposób niezawodny i bezpieczny połączyć urządzenia z zapleczem rozwiązania. Problem ten można rozwiązać, ponieważ usługa IoT Hub:

- Zapewnia niezawodne przesyłanie komunikatów z urządzeń do chmury i z chmury do urządzeń w hiperskali;
- Umożliwia bezpieczne komunikowanie się przy użyciu poświadczeń zabezpieczeń i kontroli dostępu dla poszczególnych urządzeń;
- Zawiera biblioteki urządzeń dla najbardziej popularnych języków i platform.

Ten samouczek przedstawia sposób wykonania następujących czynności:

- Użycie portalu Azure do utworzenia centrum IoT.
- Utworzenie tożsamości urządzenia w centrum IoT.
- Utworzenie symulowanego urządzenia, które wysyła telemetrię do zaplecza chmury.

Na końcu tego samouczka zostaną opisane trzy aplikacje konsoli środowiska Node.js:

* **CreateDeviceIdentity.js** tworzy tożsamość urządzenia i skojarzony klucz zabezpieczeń w celu podłączenia urządzenia symulowanego.
* **ReadDEviceToCloudMessages.js** powoduje wyświetlenie telemetrii wysyłanej przez urządzenie symulowane.
* **SimulatedDevice.js** łączy się z centrum IoT przy użyciu utworzonej wcześniej tożsamości urządzenia i co sekundę wysyła komunikat telemetryczny przy użyciu protokołu AMQPS.

> [AZURE.NOTE] Artykuł [IoT Hub SDKs] (Zestawy SDK usługi IoT Hub)[lnk-hub-sdks] zawiera informacje dotyczące różnych zestawów SDK, których można użyć do utworzenia aplikacji zarówno do uruchamiania na urządzaniach, jak i w zapleczu rozwiązania.

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

+ Środowisko Node.js w wersji 0.12.x lub nowszej. <br/> W artykule [Prepare your development environment] (Przygotowywanie środowiska projektowego)[lnk-dev-setup] opisano, jak zainstalować środowisko Node.js na potrzeby tego samouczka w systemie Windows lub Linux.

+ Aktywne konto platformy Azure. <br/>Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure][lnk_free_trial].

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Na końcu kliknij pozycję **Ustawienia** w bloku usługi IoT Hub, a następnie opcję **Obsługa wiadomości** w bloku **Ustawienia**. W bloku **Obsługa wiadomości** zanotuj **nazwę zgodną z centrum zdarzeń** oraz **punkt końcowy zgodny z centrum zdarzeń**. Wartości te będą potrzebne podczas tworzenia aplikacji **read-d2c-messages**.

![][6]

Utworzono centrum IoT i masz nazwę hosta centrum IoT, jego parametry połączenia, nazwę zgodną z centrum zdarzeń i punkt końcowy zgodny z centrum zdarzeń, potrzebne do wykonania pozostałej części tego samouczka.

## Tworzenie tożsamości urządzenia

W tej sekcji utworzysz aplikację konsoli środowiska Node.js, która tworzy nową tożsamość urządzenia w rejestrze tożsamości w centrum IoT. Urządzenie nie może połączyć się z centrum IoT, jeśli nie ma wpisu w rejestrze tożsamości urządzeń. Więcej informacji znajduje się w sekcji **Device Identity Registry** (Rejestr tożsamości urządzenia) w artykule [IoT Hub Developer Guide] (Usługa IoT Hub — przewodnik dewelopera)[lnk-devguide-identity]. Po uruchomieniu ta aplikacja konsoli generuje unikatowy identyfikator urządzenia i klucz, których urządzenie może użyć do zidentyfikowania się podczas wysyłania komunikatów do chmury do usługi IoT Hub.

1. Utwórz nowy pusty folder o nazwie **createdeviceidentity**. W folderze **createdeviceidentity** utwórz nowy plik package.json przy użyciu następującego polecenia z poziomu wiersza polecenia. Zaakceptuj wszystkie ustawienia domyślne:

    ```
    npm init
    ```

2. Z poziomu wiersza polecenia w folderze **createdeviceidentity** uruchom następujące polecenie w celu zainstalowania pakietu **azure iothub**:

    ```
    npm install azure-iothub --save
    ```

3. Za pomocą edytora tekstu utwórz nowy plik **CreateDeviceIdentity.js** w folderze **createdeviceidentity**.

4. Dodaj następującą instrukcję `require` na początku pliku **CreateDeviceIdentity.js**:

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Dodaj następujący kod do pliku **CreateDeviceIdentity.js**, zastępując wartość symbolu zastępczego parametrami połączenia centrum IoT utworzonego w poprzedniej sekcji: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Dodaj następujący kod, aby utworzyć nową definicję urządzenia w rejestrze tożsamości urządzeń w centrum IoT. Ten kod tworzy nowe urządzenie, jeśli identyfikator urządzenia nie istnieje w rejestrze. W przeciwnym razie zwraca klucz istniejącego urządzenia:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstDevice';
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

9. Zanotuj **identyfikator urządzenia** i **klucz urządzenia**. Będą potrzebne później podczas tworzenia aplikacji, która łączy się z usługą IoT Hub jako urządzenie.

> [AZURE.NOTE] Rejestr tożsamości usługi IoT Hub przechowuje tożsamości urządzenia tylko po to, aby umożliwić bezpieczny dostęp do centrum. Przechowuje identyfikatory urządzeń i klucze, które będą używane jako poświadczenia zabezpieczeń, oraz flagę włączone/wyłączone, która umożliwia wyłączenie dostępu do poszczególnych urządzeń. Jeśli aplikacja wymaga przechowywania innych metadanych dla określonego urządzenia, powinna korzystać z magazynu określonego dla aplikacji. Więcej informacji znajduje się w artykule [IoT Hub Developer Guide] (Usługa IoT Hub — przewodnik dewelopera)[lnk-devguide-identity].

## Odbieranie komunikatów z urządzenia do chmury

W tej sekcji opisano tworzenie aplikacji konsoli środowiska Node.js, która odczytuje komunikaty z urządzenia do chmury z usługi IoT Hub. Centrum IoT udostępnia punkt końcowy zgodny z usługą [Event Hubs][lnk-event-hubs-overview], aby umożliwić odczytywanie komunikatów z urządzenia do chmury. W celu uproszczenia informacji instrukcje w samouczku dotyczą tworzenia czytnika podstawowego, który nie jest odpowiedni do wdrożenia z użyciem dużej przepustowości. W samouczku [Process device-to-cloud messages] (Przetwarzanie komunikatów z urządzenia do chmury)[lnk-process-d2c-tutorial] przedstawiono sposób przetwarzania komunikatów z urządzenia do chmury na dużą skalę. Samouczek [Get Started with Event Hubs] (Usługa Event Hubs — wprowadzenie)[lnk-eventhubs-tutorial] zawiera dalsze informacje dotyczące sposobu przetwarzania komunikatów z usługi Event Hubs i dotyczy punktów końcowych usługi IoT Hub zgodnych z centrami zdarzeń.

> [AZURE.NOTE] Punkt końcowy zgodny z usługą Event Hubs do odczytywania komunikatów z urządzenia do chmury zawsze korzysta z protokołu AMQPS.

1. Utwórz nowy pusty folder o nazwie **readdevicetocloudmessages**. W folderze **readdevicetocloudmessages** utwórz nowy plik package.json przy użyciu następującego polecenia z poziomu wiersza polecenia. Zaakceptuj wszystkie ustawienia domyślne:

    ```
    npm init
    ```

2. Z poziomu wiersza polecenia w folderze **readdevicetocloudmessages** uruchom następujące polecenie, aby zainstalować pakiety **amqp10** i **bluebird**:

    ```
    npm install amqp10 bluebird --save
    ```

3. Za pomocą edytora tekstu utwórz nowy plik **ReadDeviceToCloudMessages.js** w folderze **readdevicetocloudmessages**.

4. Dodaj następujące instrukcję `require` na początku pliku **ReadDeviceToCloudMessages.js**:

    ```
    'use strict';

    var AMQPClient = require('amqp10').Client;
    var Policy = require('amqp10').Policy;
    var translator = require('amqp10').translator;
    var Promise = require('bluebird');
    ```

5. Dodaj następujące deklaracje zmiennych, zastępując symbole zastępcze zapisanymi wcześniej wartościami. Wartość symbolu zastępczego **{your event hub-compatible namespace}** pochodzi z pola **Event Hub-compatible endpoint** w portalu — ma formę **namespace.servicebus.windows.net** (bez prefiksu *sb://*).

    ```
    var protocol = 'amqps';
    var eventHubHost = '{your event hub-compatible namespace}';
    var sasName = 'iothubowner';
    var sasKey = '{your iot hub key}';
    var eventHubName = '{your event hub-compatible name}';
    var numPartitions = 2;
    ```

    > [AZURE.NOTE] W przypadku tego kodu przyjmuje się założenie, że usługa IoT Hub została utworzona w (bezpłatnej) warstwie F1. Bezpłatne centrum IoT ma dwie partycje o nazwie „0” i „1”. Jeśli centrum IoT zostało utworzone przy innej warstwy cenowej, dostosuj kod tworzenia elementu **MessageReceiver** dla każdej partycji.

6. Dodaj następującą definicję filtru. Ta aplikacja używa filtru podczas tworzenia odbiornika, więc odbiornik odczytuje tylko komunikaty wysyłane do usługi Centrum IoT po uruchomieniu odbiornika. Jest to przydatne w środowisku testowym, gdyż dzięki temu jest wyświetlany bieżący zestaw komunikatów, ale w środowisku produkcyjnym kod powinien koniecznie przetworzyć wszystkie komunikaty — więcej informacji znajduje się w samouczku [How to process IoT Hub device-to-cloud messages] (Jak przetwarzać komunikaty z urządzenia do chmury usługi IoT Hub)[lnk-process-d2c-tutorial].

    ```
    var filterOffset = new Date().getTime();
    var filterOption;
    if (filterOffset) {
      filterOption = {
      attach: { source: { filter: {
      'apache.org:selector-filter:string': translator(
        ['described', ['symbol', 'apache.org:selector-filter:string'], ['string', "amqp.annotation.x-opt-enqueuedtimeutc > " + filterOffset + ""]])
        } } }
      };
    }
    ```

7. Dodaj następujący kod, aby utworzyć adres odbioru i klienta AMQP:

    ```
    var uri = protocol + '://' + encodeURIComponent(sasName) + ':' + encodeURIComponent(sasKey) + '@' + eventHubHost;
    var recvAddr = eventHubName + '/ConsumerGroups/$default/Partitions/';
    
    var client = new AMQPClient(Policy.EventHub);
    ```

8. Dodaj następujące dwie funkcje, które drukują dane wyjściowe do konsoli:

    ```
    var messageHandler = function (partitionId, message) {
      console.log('Received(' + partitionId + '): ', message.body);
    };
    
    var errorHandler = function(partitionId, err) {
      console.warn('** Receive error: ', err);
    };
    ```

9. Dodaj następującą funkcję, która działa jako odbiornik dla danej partycji, przy użyciu filtru:

    ```
    var createPartitionReceiver = function(partitionId, receiveAddress, filterOption) {
      return client.createReceiver(receiveAddress, filterOption)
        .then(function (receiver) {
          console.log('Listening on partition: ' + partitionId);
          receiver.on('message', messageHandler.bind(null, partitionId));
          receiver.on('errorReceived', errorHandler.bind(null, partitionId));
        });
    };
    ```

10. Dodaj następujący kod, aby połączyć się z punktem końcowym zgodnym z centrum zdarzeń i uruchomić odbiorniki:

    ```
    client.connect(uri)
      .then(function () {
        var partitions = [];
        for (var i = 0; i < numPartitions; ++i) {
          partitions.push(createPartitionReceiver(i, recvAddr + i, filterOption));
        }
        return Promise.all(partitions);
    })
    .error(function (e) {
        console.warn('Connection error: ', e);
    });
    ```

11. Zapisz i zamknij plik **ReadDeviceToCloudMessages.js**.

## Tworzenie aplikacji symulowanego urządzenia

Ta sekcja zawiera instrukcje dotyczące tworzenia aplikacji konsoli środowiska Node.js, która symuluje urządzenie wysyłające komunikaty z urządzenia do chmury do centrum IoT.

1. Utwórz nowy pusty folder o nazwie **simulateddevice**. W folderze **simulateddevice** utwórz nowy plik package.json przy użyciu następującego polecenia z poziomu wiersza polecenia. Zaakceptuj wszystkie ustawienia domyślne:

    ```
    npm init
    ```

2. Z poziomu wiersza polecenia w folderze **simulateddevice** uruchom następujące polecenie, aby zainstalować pakiet **azure-iot-device-amqp**:

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

5. Dodaj zmienną **connectionString** i użyj jej do utworzenia klienta urządzenia. Zastąp **{youriothubname}** nazwą centrum IoT, a **{yourdeviceid}** i **{yourdevicekey}** wartościami urządzenia wygenerowanymi w sekcji *Tworzenie tożsamości urządzenia*:

    ```
    var connectionString = 'HostName={youriothubname}.azure-devices.net;DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    
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
            var data = JSON.stringify({ deviceId: 'mydevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 2000);
      }
    };
    ```

8. Otwórz połączenie z centrum IoT i rozpocznij wysyłanie komunikatów:

    ```
    client.open(connectCallback);
    ```

9. Zapisz i zamknij plik **SimulatedDevice.js**.

> [AZURE.NOTE] Dla uproszczenia ten samouczek nie zawiera opisu wdrożenia żadnych zasad ponawiania. W kodzie produkcyjnym należy wdrożyć zasady ponawiania (np. wycofywanie wykładnicze) zgodnie z sugestią w artykule z witryny MSDN [Transient Fault Handling] (Obsługa przejściowego błędu)[lnk-transient-faults].


## Uruchamianie aplikacji

Teraz można uruchomić aplikacje.

1. Z poziomu wiersza polecenia w folderze **readdevicetocloudmessages** uruchom następujące polecenie, aby rozpocząć monitorowanie centrum IoT:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![][7]

2. Z poziomu wiersza polecenia w folderze **simulateddevice** uruchom następujące polecenie, aby rozpocząć wysyłanie danych telemetrycznych do centrum IoT:

    ```
    node SimulatedDevice.js
    ```

    ![][8]

3. Na kafelku **Użycie** w [portalu Azure][lnk-portal] wyświetlana jest liczba komunikatów wysłanych do centrum:

    ![][43]

## Następne kroki

W tym samouczku opisano konfigurowanie nowego centrum IoT w portalu, a następnie tworzenie tożsamości urządzenia w rejestrze tożsamości centrum. Tożsamość urządzenia została użyta, aby włączyć w symulowanej aplikacji urządzenia funkcję wysyłania komunikatów z urządzenia do chmury do centrum. Została również utworzona aplikacja, która wyświetla komunikaty odebrane przez centrum. Dalsze informacje na temat funkcji centrum IoT oraz innych scenariuszy IoT znajdują się w następujących samouczkach:

- W artykule [Send Cloud-to-Device messages with IoT Hub] (Wysyłanie komunikatów z chmury do urządzenia przy użyciu usługi IoT Hub)[lnk-c2d-tutorial] przedstawiono, jak wysyłać komunikaty do urządzeń i przetwarzać odebrane informacje zwrotne wygenerowane w usłudze IoT Hub.
- Artykuł [Process Device-to-Cloud messages] (Przetwarzanie komunikatów z urządzenia do chmury)[lnk-process-d2c-tutorial] przedstawia, jak w sposób niezawodny przetwarzać telemetrię i komunikaty interaktywne pochodzące z urządzeń.
- W artykule [Uploading files from devices] (Przekazywanie plików z urządzeń)[lnk-upload-tutorial] opisano wzorzec, według którego komunikaty z chmury do urządzenia są używane do usprawnienia przekazywania plików z urządzeń.

<!-- Images. -->
[6]: ./media/iot-hub-node-node-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide.md#identityregistry
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/doc/devbox_setup.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-upload-tutorial]: iot-hub-csharp-csharp-file-upload.md

[lnk-hub-sdks]: iot-hub-sdks-summary.md
[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/



<!--HONumber=jun16_HO2-->


