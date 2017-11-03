## <a name="create-a-device-identity"></a>Tworzenie tożsamości urządzenia

W tej sekcji można użyć narzędzia Node.js o nazwie [explorer Centrum iothub] [ iot-hub-explorer] do tworzenia tożsamości urządzenia w ramach tego samouczka. W identyfikatorach urządzeń jest uwzględniana wielkość liter.

1. Uruchom następujące polecenie w wierszu polecenia środowiska:

    `npm install -g iothub-explorer@latest`

1. Następnie uruchom następujące polecenie, aby zalogować się do Centrum. SUBSTITUTE `{iot hub connection string}` wcześniej skopiowany ciągu połączenia Centrum IoT:

    `iothub-explorer login "{iot hub connection string}"`

1. Na koniec utworzy nową tożsamość urządzenia o nazwie `myDeviceId` przy użyciu polecenia:

    `iothub-explorer create myDeviceId --connection-string`

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Zanotuj parametry połączenia urządzenia z wyników. Ten ciąg połączenia urządzenia jest używany przez aplikację urządzenia nawiązać połączenia z Centrum IoT jako urządzenie.

![][img-identity]

Zapoznaj się [wprowadzenie do korzystania z Centrum IoT] [ lnk-getstarted] do programowego tworzenia tożsamości urządzenia.

<!-- images and links -->
[img-identity]: media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
