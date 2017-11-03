## <a name="create-a-device-identity"></a>Tworzenie tożsamości urządzenia

W tej sekcji użyjesz [portalu Azure] [ lnk-azure-portal] do tworzenia tożsamości urządzenia w rejestrze tożsamości w Centrum IoT. Urządzenie nie może połączyć się z centrum IoT, jeśli nie ma wpisu w rejestrze tożsamości. Więcej informacji znajduje się w sekcji „Identity registry” (Rejestr tożsamości) artykułu [IoT Hub Developer Guide][lnk-devguide-identity] (Usługa IoT Hub — przewodnik dewelopera). **Explorer urządzenia** w portalu pomaga Wygeneruj Unikatowy identyfikator urządzenia i klucz urządzenia służy do identyfikacji podczas łączenia się z Centrum IoT. W identyfikatorach urządzeń jest uwzględniana wielkość liter.

1. Upewnij się, że użytkownik jest zalogowany do [portalu Azure][lnk-azure-portal].

1. Na pasku przechodzenia kliknij **wszystkie zasoby** i Znajdź zasobu Centrum IoT.

    ![Przejdź do Centrum Iot][img-find-iothub]

1. Po otwarciu zasobu Centrum IoT kliknij **Explorer urządzenia** narzędzia, a następnie kliknij przycisk **Dodaj** u góry. Podaj nazwę dla nowego urządzenia, na przykład **myDeviceId**i kliknij przycisk **zapisać**.

    ![Tworzenie tożsamości urządzenia w portalu][img-create-device]

   Spowoduje to utworzenie nowej tożsamości urządzenia Centrum IoT.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. W **Explorer urządzenia**na listę urządzeń, kliknij urządzenie nowo utworzona i zanotuj **ciąg połączenia---klucz podstawowy**. 

    ![Ciąg połączenia urządzenia][img-connection-string]

> [!NOTE]
> Rejestr tożsamości usługi IoT Hub przechowuje tożsamości urządzenia tylko po to, aby umożliwić bezpieczny dostęp do centrum IoT. Przechowuje identyfikatory urządzeń i klucze, które będą używane jako poświadczenia zabezpieczeń, oraz flagę włączone/wyłączone, która umożliwia wyłączenie dostępu do poszczególnych urządzeń. Jeśli aplikacja wymaga przechowywania innych metadanych dla określonego urządzenia, powinna korzystać z magazynu określonego dla aplikacji. Więcej informacji znajduje się w temacie [IoT Hub Developer Guide][lnk-devguide-identity] (Usługa IoT Hub — przewodnik dewelopera).

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

