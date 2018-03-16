---
title: "Rozpoczynanie pracy z usługą Azure IoT Hub (Python) | Microsoft Docs"
description: "Dowiedz się, jak wysyłać komunikaty urządzenie-chmura do usługi Azure IoT Hub za pomocą zestawów SDK usługi IoT dla języka Python. Utwórz symulowane aplikacje urządzenia i usługi, aby zarejestrować urządzenie, wysyłać wiadomości i odczytywać wiadomości z usługi IoT Hub."
services: iot-hub
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: python
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/02/2018
ms.author: dkshir
ms.custom: na
ms.openlocfilehash: 2c14176a3d45b85ae94c27549e5d6448e240ed98
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="connect-your-simulated-device-to-your-iot-hub-using-python"></a>Podłączanie symulowanego urządzenia do usługi IoT Hub za pomocą języka Python
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na koniec tego samouczka będziesz mieć dwie aplikacje Python:

* **CreateDeviceIdentity.py**, która tworzy tożsamość urządzenia i skojarzony klucz zabezpieczeń w celu podłączenia symulowanej aplikacji urządzenia.
* **SimulatedDevice.py**, która łączy się z usługą IoT Hub przy użyciu utworzonej wcześniej tożsamości urządzenia i okresowo wysyła komunikat telemetrii przy użyciu protokołu MQTT.

> [!NOTE]
> Artykuł [Azure IoT SDKs][lnk-hub-sdks] (Zestawy SDK usług Azure IoT) zawiera informacje dotyczące zestawów SDK usług Azure IoT, przy użyciu których można tworzyć aplikacje zarówno do uruchamiania na urządzaniach, jak i w zapleczu rozwiązania.
> 
> 

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* [Środowisko Python 2.x lub 3.x][lnk-python-download]. Upewnij się, że używasz 32-bitowej lub 64-bitowej instalacji zgodnie z wymaganiami konfiguracji. Po wyświetleniu monitu podczas instalacji upewnij się, że język Python został dodany do zmiennej środowiskowej specyficznej dla platformy. Jeśli używasz środowiska Python 2.x, może być konieczne [zainstalowanie lub uaktualnienie systemu zarządzania pakietami języka Python — *pip*][lnk-install-pip].
* [Pakiet redystrybucyjny języka Visual C++][lnk-visual-c-redist] (jeśli używasz systemu operacyjnego Windows) umożliwiający korzystanie z natywnych bibliotek DLL języka Python.
* [Środowisko Node.js 4.0 lub nowsze][lnk-node-download]. Upewnij się, że używasz 32-bitowej lub 64-bitowej instalacji zgodnie z wymaganiami konfiguracji. Jest to niezbędne do zainstalowania [narzędzia IoT Hub Explorer][lnk-iot-hub-explorer].
* Aktywne konto platformy Azure. Jeśli go nie masz, możesz utworzyć [bezpłatne konto][lnk-free-trial] w zaledwie kilka minut.

> [!NOTE]
> Pakiety *pip* dla składników `azure-iothub-service-client` i `azure-iothub-device-client` są obecnie dostępne tylko w przypadku systemu operacyjnego Windows. W przypadku systemu operacyjnego Linux lub Mac OS zapoznaj się z sekcją poświęconą danemu systemowi w poście [Prepare your development environment for Python][lnk-python-devbox] (Przygotowanie środowiska projektowego dla języka Python).
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Utworzono centrum IoT. Użyj tej nazwy hosta usługi IoT Hub oraz jej parametrów połączenia w pozostałej części tego samouczka.

> [!NOTE]
> Centrum IoT można również łatwo utworzyć w wierszu polecenia, korzystając z interfejsu wiersza polecenia platformy Azure opartego na środowisku Python lub Node.js. Szybkie kroki wykonania tego zadania zawiera artykuł [Create an IoT hub using the Azure CLI 2.0][lnk-azure-cli-hub] (Tworzenie centrum IoT za pomocą interfejsu wiersza polecenia platformy Azure 2.0). 
> 

## <a name="create-a-device-identity"></a>Tworzenie tożsamości urządzenia
W tej sekcji znajdują się kroki umożliwiające utworzenie aplikacji konsoli języka Python, która tworzy tożsamość urządzenia w rejestrze tożsamości w centrum IoT. Urządzenie może nawiązać połączenie z usługą IoT Hub tylko wtedy, jeśli ma wpis w rejestrze tożsamości. Więcej informacji znajduje się w sekcji **Identity registry** (Rejestr tożsamości) artykułu [IoT Hub developer guide][lnk-devguide-identity] (Usługa IoT Hub — przewodnik dewelopera). Po uruchomieniu ta aplikacja konsoli generuje unikatowy identyfikator urządzenia i klucz, których urządzenie może użyć do zidentyfikowania się podczas wysyłania komunikatów do chmury do usługi IoT Hub.

1. Otwórz wiersz polecenia i zainstaluj **zestaw SDK usługi Azure IoT Hub dla języka Python**. Zamknij wiersz polecenia po zainstalowaniu zestawu SDK.

    ```
    pip install azure-iothub-service-client
    ```

2. Utwórz plik języka Python o nazwie **CreateDeviceIdentity.py**. Otwórz go w [edytorze języka Python lub w wybranym środowisku IDE][lnk-python-ide-list], na przykład w domyślnym środowisku [IDLE][lnk-idle].

3. Dodaj następujący kod umożliwiający zaimportowanie wymaganych modułów z zestawu SDK usługi:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceStatus, IoTHubError
    ```
2. Dodaj następujący kod, zastępując symbol zastępczy `[IoTHub Connection String]` parametrami połączenia centrum IoT utworzonego w poprzedniej sekcji. Jako identyfikatora `DEVICE_ID` można użyć dowolnej nazwy.
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "MyFirstPythonDevice"
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

3. Dodaj następującą funkcję umożliwiającą wydrukowanie niektórych informacji o urządzeniu.

    ```python
    def print_device_info(title, iothub_device):
        print ( title + ":" )
        print ( "iothubDevice.deviceId                    = {0}".format(iothub_device.deviceId) )
        print ( "iothubDevice.primaryKey                  = {0}".format(iothub_device.primaryKey) )
        print ( "iothubDevice.secondaryKey                = {0}".format(iothub_device.secondaryKey) )
        print ( "iothubDevice.connectionState             = {0}".format(iothub_device.connectionState) )
        print ( "iothubDevice.status                      = {0}".format(iothub_device.status) )
        print ( "iothubDevice.lastActivityTime            = {0}".format(iothub_device.lastActivityTime) )
        print ( "iothubDevice.cloudToDeviceMessageCount   = {0}".format(iothub_device.cloudToDeviceMessageCount) )
        print ( "iothubDevice.isManaged                   = {0}".format(iothub_device.isManaged) )
        print ( "iothubDevice.authMethod                  = {0}".format(iothub_device.authMethod) )
        print ( "" )
    ```
3. Dodaj następującą funkcję umożliwiającą utworzenie identyfikacji urządzenia przy użyciu Menedżera rejestru. 

    ```python
    def iothub_createdevice():
        try:
            iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
            auth_method = IoTHubRegistryManagerAuthMethod.SHARED_PRIVATE_KEY
            new_device = iothub_registry_manager.create_device(DEVICE_ID, "", "", auth_method)
            print_device_info("CreateDevice", new_device)

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "iothub_createdevice stopped" )
    ```
4. Na końcu dodaj funkcję main w przedstawiony sposób i zapisz plik.

    ```python
    if __name__ == '__main__':
        print ( "" )
        print ( "Python {0}".format(sys.version) )
        print ( "Creating device using the Azure IoT Hub Service SDK for Python" )
        print ( "" )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_createdevice()
    ```
5. W wierszu polecenia uruchom plik **CreateDeviceIdentity.py** w następujący sposób:

    ```python
    python CreateDeviceIdentity.py
    ```
6. Powinno zostać wyświetlone utworzone symulowane urządzenie. Zanotuj wartości parametrów **deviceId** i **primaryKey** tego urządzenia. Te wartości będą potrzebne później podczas tworzenia aplikacji, która łączy się z usługą IoT Hub jako urządzenie.

    ![Pomyślne utworzenie urządzenia][1]

> [!NOTE]
> Rejestr tożsamości usługi IoT Hub przechowuje tożsamości urządzenia tylko po to, aby umożliwić bezpieczny dostęp do centrum IoT. Przechowuje identyfikatory i klucze urządzeń, które będą używane jako poświadczenia zabezpieczeń, oraz flagę włączone/wyłączone, która umożliwia wyłączenie dostępu do poszczególnych urządzeń. Jeśli aplikacja wymaga przechowywania innych metadanych dla określonego urządzenia, powinna korzystać z magazynu określonego dla aplikacji. Więcej informacji znajduje się w temacie [IoT Hub developer guide][lnk-devguide-identity] (Usługa IoT Hub — przewodnik dewelopera).
> 
> 


## <a name="create-a-simulated-device-app"></a>Tworzenie aplikacji symulowanego urządzenia
Ta sekcja zawiera instrukcje dotyczące tworzenia aplikacji konsoli języka Python, która symuluje urządzenie i wysyła do centrum IoT komunikaty z urządzenia do chmury.

1. Otwórz nowy wiersz polecenia i zainstaluj zestaw SDK urządzenia usługi Azure IoT Hub dla języka Python w następujący sposób. Po ukończeniu instalacji zamknij wiersz polecenia.

    ```
    pip install azure-iothub-device-client
    ```
2. Utwórz plik o nazwie **SimulatedDevice.py**. Otwórz ten plik w edytorze języka Python lub w wybranym środowisku IDE (na przykład w środowisku IDLE).

3. Dodaj następujący kod umożliwiający zaimportowanie wymaganych modułów z zestawu SDK urządzenia.

    ```python
    import random
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError, DeviceMethodReturnValue
    ```
4. Dodaj następujący kod i zastąp symbol zastępczy `[IoTHub Device Connection String]` parametrami połączenia dla urządzenia. Parametry połączenia urządzenia mają zazwyczaj format `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`. Zastąp symbole zastępcze `<deviceId>` i `<primaryKey>` odpowiednio wartościami parametrów **deviceId** i **primaryKey** urządzenia utworzonego w poprzedniej sekcji. Zastąp symbol zastępczy `<hostName>` nazwą hosta centrum IoT. Zwykle jest to wartość `<IoT hub name>.azure-devices.net`.

    ```python
    # String containing Hostname, Device Id & Device Key in the format
    CONNECTION_STRING = "[IoTHub Device Connection String]"
    # choose HTTP, AMQP or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT
    MESSAGE_TIMEOUT = 10000
    AVG_WIND_SPEED = 10.0
    SEND_CALLBACKS = 0
    MSG_TXT = "{\"deviceId\": \"MyFirstPythonDevice\",\"windSpeed\": %.2f}"    
    ```
5. Dodaj następujący kod, aby zdefiniować wywołanie zwrotne potwierdzenia wysyłania. 

    ```python
    def send_confirmation_callback(message, result, user_context):
        global SEND_CALLBACKS
        print ( "Confirmation[%d] received for message with result = %s" % (user_context, result) )
        map_properties = message.properties()
        print ( "    message_id: %s" % message.message_id )
        print ( "    correlation_id: %s" % message.correlation_id )
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        SEND_CALLBACKS += 1
        print ( "    Total calls confirmed: %d" % SEND_CALLBACKS )
    ```
6. Dodaj następujący kod, aby zainicjować klienta urządzenia.

    ```python
    def iothub_client_init():
        # prepare iothub client
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
        # set the time until a message times out
        client.set_option("messageTimeout", MESSAGE_TIMEOUT)
        client.set_option("logtrace", 0)
        client.set_option("product_info", "HappyPath_Simulated-Python")
        return client
    ```
7. Dodaj następującą funkcję umożliwiającą sformatowanie i wysłanie komunikatu z symulowanego urządzenia do centrum IoT.

    ```python
    def iothub_client_telemetry_sample_run():

        try:
            client = iothub_client_init()
            print ( "IoT Hub device sending periodic messages, press Ctrl-C to exit" )
            message_counter = 0

            while True:
                msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))
                # messages can be encoded as string or bytearray
                if (message_counter & 1) == 1:
                    message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))
                else:
                    message = IoTHubMessage(msg_txt_formatted)
                # optional: assign ids
                message.message_id = "message_%d" % message_counter
                message.correlation_id = "correlation_%d" % message_counter
                # optional: assign properties
                prop_map = message.properties()
                prop_text = "PropMsg_%d" % message_counter
                prop_map.add("Property", prop_text)

                client.send_event_async(message, send_confirmation_callback, message_counter)
                print ( "IoTHubClient.send_event_async accepted message [%d] for transmission to IoT Hub." % message_counter )

                status = client.get_send_status()
                print ( "Send status: %s" % status )
                time.sleep(30)

                status = client.get_send_status()
                print ( "Send status: %s" % status )

                message_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
    ```
8. Na koniec dodaj funkcję main. 

    ```python
    if __name__ == '__main__':
        print ( "Simulating a device using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_telemetry_sample_run()
    ```
9. Zapisz i zamknij plik **SimulatedDevice.py**. Teraz można przystąpić do uruchomienia tej aplikacji.

> [!NOTE]
> Dla uproszczenia ten samouczek nie zawiera opisu wdrożenia żadnych zasad ponawiania. W kodzie produkcyjnym należy wdrożyć zasady ponawiania (np. wycofywanie wykładnicze) zgodnie z sugestią w artykule z witryny MSDN [Transient Fault Handling][lnk-transient-faults] (Obsługa przejściowych błędów).
> 
> 

## <a name="receive-messages-from-your-simulated-device"></a>Odbieranie komunikatów z symulowanego urządzenia
Do odbierania komunikatów telemetrycznych z urządzenia należy użyć [usługi Event Hubs][lnk-event-hubs-overview] — zgodnego punktu końcowego udostępnianego przez usługę IoT Hub, który odczytuje komunikaty wysyłane z urządzenia do chmury. Przeczytaj samouczek [Get Started with Event Hubs][lnk-eventhubs-tutorial] (Usługa Event Hubs — wprowadzenie), aby uzyskać informacje dotyczące sposobu przetwarzania komunikatów z usługi Event Hubs przeznaczonych dla zgodnego z centrum zdarzeń punktu końcowego centrum IoT. Usługa Event Hubs nie obsługuje jeszcze telemetrii w języku Python, można więc utworzyć aplikację konsoli środowiska [Node.js](iot-hub-node-node-getstarted.md#D2C_node) lub [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) opartą na usłudze Event Hubs, która umożliwi odczytywanie komunikatów wysyłanych z urządzenia do chmury pochodzących z usługi IoT Hub. W tym samouczku pokazano, jak skorzystać z [narzędzia IoT Hub Explorer][lnk-iot-hub-explorer] do odczytywania tych komunikatów urządzeń.

1. Otwórz wiersz polecenia i zainstaluj narzędzie IoT Hub Explorer. 

    ```
    npm install -g iothub-explorer
    ```

2. Uruchom następujące polecenie w wierszu polecenia, aby rozpocząć monitorowanie z urządzenia komunikatów wysyłanych z urządzenia do chmury. Użyj parametrów połączenia z Twojego centrum IoT w miejsce symbolu zastępczego po `--login`.

    ```
    iothub-explorer monitor-events MyFirstPythonDevice --login "[IoTHub connection string]"
    ```

3. Otwórz nowy wiersz polecenia i przejdź do katalogu zawierającego plik **SimulatedDevice.py**.

4. Uruchom plik **SimulatedDevice.py**, który okresowo wysyła dane telemetryczne do Twojego centrum IoT. 
   
    ```
    python SimulatedDevice.py
    ```
5. W wierszu polecenia z uruchomionym w poprzedniej sekcji narzędziem IoT Hub Explorer obserwuj komunikaty z urządzenia. 

    ![Komunikaty środowiska Python przesyłane z urządzenia do chmury][2]

## <a name="next-steps"></a>Następne kroki
W tym samouczku opisano konfigurowanie nowego centrum IoT Hub w witrynie Azure Portal, a następnie tworzenie tożsamości urządzenia w rejestrze tożsamości centrum. Tożsamość urządzenia została użyta, aby włączyć w aplikacji symulowanego urządzenia funkcję wysyłania komunikatów z urządzenia do chmury do centrum IoT Hub. Z pomocą narzędzia IoT Hub Explorer obserwowano komunikaty odbierane przez centrum IoT. 

Aby szczegółowo przeanalizować możliwości użycia zestawu SDK języka Python dla usługi Azure IoT Hub, odwiedź [to repozytorium GitHub][lnk-python-github]. Aby przejrzeć możliwości obsługi wiadomości zestawu SDK usługi Azure IoT Hub dla języka Python, możesz pobrać i uruchomić plik [iothub_messaging_sample.py][lnk-messaging-sample]. Aby zapoznać się z możliwościami symulacji ze strony urządzenia przy użyciu zestawu SDK urządzenia usługi Azure IoT Hub dla języka Python, możesz pobrać i uruchomić plik [iothub_client_sample.py][lnk-client-sample].

Aby kontynuować wprowadzenie do usługi IoT Hub i zapoznać się z innymi scenariuszami IoT, zobacz:

* [Łączenie urządzenia][lnk-connect-device]
* [Wprowadzenie do zarządzania urządzeniami][lnk-device-management]
* [Wdrażanie rozwiązań SI na urządzeniach brzegowych przy użyciu usługi Azure IoT Edge][lnk-iot-edge]

Aby dowiedzieć się, jak rozszerzyć rozwiązanie IoT i przetwarzać komunikaty z urządzenia do chmury na dużą skalę, zobacz samouczek [Przetwarzanie komunikatów przesyłanych z urządzeń do chmury][lnk-process-d2c-tutorial].
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[1]: ./media/iot-hub-python-getstarted/createdevice.png
[2]: ./media/iot-hub-python-getstarted/sendd2cmessage.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-azure-cli-hub]: https://docs.microsoft.com/azure/iot-hub/iot-hub-create-using-cli
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-idle]: https://docs.python.org/3/library/idle.html
[lnk-python-ide-list]: https://wiki.python.org/moin/IntegratedDevelopmentEnvironments
[lnk-iot-hub-explorer]: https://github.com/Azure/iothub-explorer
[lnk-python-github]: https://github.com/Azure/azure-iot-sdk-python
[lnk-messaging-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/service/samples/iothub_messaging_sample.py
[lnk-client-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample.py

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-python-devbox]: https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md

[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
