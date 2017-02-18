> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)
> 
> 

W tym przewodniku po [przykładzie przekazywania danych do chmury przez symulowane urządzenia] przedstawiono, jak przy użyciu [zestawu SDK usługi Azure IoT Gateway][lnk-sdk] wysyłać dane telemetryczne przesyłane z urządzenia do chmury do usługi IoT Hub z symulowanych urządzeń.

Przewodnik składa się z następujących elementów:

1. **Architektura**: ważne informacje dotyczące architektury przykładu przekazywania danych do chmury przez symulowane urządzenia.
2. **Kompilowanie i uruchamianie**: czynności wymagane do skompilowania i uruchomienia przykładu.

## <a name="architecture"></a>Architektura
W przykładzie przekazywania danych do chmury przez symulowane urządzenia przedstawiono, jak przy użyciu zestawu SDK utworzyć bramę wysyłającą dane telemetryczne z symulowanych urządzeń do centrum IoT. Symulowane urządzenia nie mogą łączyć się bezpośrednio z usługą IoT Hub z następujących powodów:

* Urządzenia nie korzystają z protokołu komunikacji obsługiwanego przez usługę IoT Hub.
* Urządzenia nie są na tyle inteligentne, aby pamiętały przypisaną im przez usługę IoT Hub tożsamość.

Brama rozwiązuje te problemy z symulowanymi urządzeniami w następujące sposoby:

* Brama obsługuje protokół używany przez symulowane urządzenia, odbiera od urządzeń dane telemetryczne przesyłane z urządzenia do chmury i przekazuje te komunikaty do usługi IoT Hub przy użyciu protokołu obsługiwanego przez centrum IoT.
* Brama przechowuje tożsamości usługi IoT Hub w imieniu symulowanych urządzeń i działa jako serwer proxy, gdy symulowane urządzenia wysyłają komunikaty do usługi IoT Hub.

Na poniższym diagramie przedstawiono główne składniki przykładu wraz z modułami bramy:

![][1]

> [!NOTE]
> Moduły nie przekazują komunikatów bezpośrednio między sobą. Moduły publikują komunikaty w wewnętrznym brokerze, który dostarcza komunikaty do innych modułów przy użyciu mechanizmu subskrypcji, jak przedstawiono na poniższym diagramie. Aby uzyskać więcej informacji, zobacz [Wprowadzenie do zestawu SDK usługi IoT Gateway][lnk-gw-getstarted].
> 
> 

### <a name="protocol-ingestion-module"></a>Moduł pozyskiwania protokołu
Ten moduł jest punktem wyjścia do przekazywania danych z urządzeń (przez bramę) do chmury. W tym przykładzie moduł wykonuje&4; zadania:

1. Tworzy symulowane dane dotyczące temperatury. Należy pamiętać, że w przypadku używania rzeczywistych urządzeń, moduł odczytuje dane z tych urządzeń fizycznych.
2. Umieszcza symulowane dane dotyczące temperatury w zawartości komunikatu.
3. Dodaje właściwość ze sztucznym adresem MAC do komunikatu zawierającego symulowane dane dotyczące temperatury.
4. Udostępnia komunikat następnemu modułowi w łańcuchu.

> [!NOTE]
> Moduł o nazwie **Pozyskiwanie protokołu X** w powyższym diagramie w kodzie źródłowym nosi nazwę **Symulowane urządzenie**.
> 
> 

### <a name="mac-lt-gt-iot-hub-id-module"></a>Adres MAC &lt;-&gt; moduł identyfikatora usługi IoT Hub
Ten moduł skanuje w poszukiwaniu komunikatów z właściwością zawierającą dodany przez moduł pozyskiwania protokołu adres MAC aplikacji symulowanego urządzenia. Jeśli moduł znajdzie taką właściwość, dodaje do komunikatu kolejną właściwość z kluczem urządzenia usługi IoT Hub, a następnie udostępnia komunikat następnemu modułowi w łańcuchu. W ten sposób przykład kojarzy tożsamości urządzeń usługi IoT Hub z symulowanymi urządzeniami. Deweloper ręcznie konfiguruje mapowanie między adresami MAC a usługą IoT Hub w ramach konfigurowania modułu. 

> [!NOTE]
> W tym przykładzie adres MAC jest używany jako unikatowy identyfikator urządzenia i jest korelowany z tożsamością urządzenia usługi IoT Hub. Możesz jednak napisać swój własny moduł, który będzie używał innego unikatowego identyfikatora. Na przykład możesz mieć urządzenia z unikatowymi numerami seryjnymi lub dane telemetryczne z osadzoną w nich unikatową nazwą urządzenia, za pomocą której możesz określać tożsamość urządzenia usługi IoT Hub.
> 
> 

### <a name="iot-hub-communication-module"></a>Moduł komunikacyjny usługi IoT Hub
Ten moduł przyjmuje komunikaty z tożsamością urządzenia usługi IoT Hub przypisaną przez poprzedni moduł i przesyła zawartość komunikatu do usługi IoT Hub przy użyciu protokołu HTTP. Protokółu HTTP jest jednym z trzech protokołów obsługiwanych przez usługę IoT Hub.

Zamiast otwierać połączenie z usługą IoT Hub dla każdej aplikacji symulowanego urządzenia ten moduł otwiera jedno połączenie HTTP z bramy do centrum IoT i multipleksuje połączenia ze wszystkich symulowanych urządzeń przez to połączenie. Dzięki temu jedna brama może połączyć się ze znacznie większą liczbą urządzeń, symulowanych lub nie, niż by to było możliwe w przypadku otwierania unikatowego połączenia dla każdego urządzenia.

![][2]

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[przykładzie przekazywania danych do chmury przez symulowane urządzenia]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md

<!--HONumber=Feb17_HO1-->


