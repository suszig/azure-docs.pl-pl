> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)

Ten przewodnik [symulowane urządzenie chmury Przekaż przykładowe] przedstawiono sposób użycia [Azure IoT krawędzi] [ lnk-sdk] do wysyłania danych telemetrycznych urządzenia do chmury do Centrum IoT z symulowanego urządzenia .

Przewodnik składa się z następujących elementów:

* **Architektura**: architektury informacji na temat [symulowane urządzenie chmury Przekaż przykładowe].
* **Kompilowanie i uruchamianie**: czynności wymagane do skompilowania i uruchomienia przykładu.

## <a name="architecture"></a>Architektura

[symulowane urządzenie chmury Przekaż przykładowe] przedstawiono sposób tworzenia bramy, która wysyła dane telemetryczne z symulowanego urządzenia do Centrum IoT. Urządzenie może nie móc połączyć się bezpośrednio z Centrum IoT, ponieważ urządzenie:

* Nie używa protokołu komunikacji rozpoznawany przez Centrum IoT.
* Nie jest inteligentne do zapamiętania tożsamość przypisana przez Centrum IoT.

Brama brzegowa IoT umożliwia rozwiązanie tych problemów w następujący sposób:

* Brama rozumie protokół używany przez urządzenie odbiera telemetrię urządzenia do chmury z urządzenia i przesyła dalej wiadomości do Centrum IoT przy użyciu protokołu rozpoznawany przez Centrum IoT.

* Brama mapuje tożsamości Centrum IoT na urządzeniach i działa jako serwer proxy, gdy urządzenie wysyła komunikaty do Centrum IoT.

Na poniższym diagramie przedstawiono główne składniki próbki, łącznie z modułów krawędzi IoT:

![Diagram — komunikat symulowane urządzenie przechodzi przez bramę do Centrum IoT][1]

Ten przykład zawiera trzy moduły, wchodzące w skład bramy:
1. Moduł pozyskiwania protokołu
1. Adres MAC &lt;-&gt; moduł identyfikatora usługi IoT Hub
1. Moduł komunikacyjny usługi IoT Hub

Moduły nie przekazują komunikatów bezpośrednio między sobą. Moduły publikowania komunikatów wewnętrzny brokera, która dostarcza wiadomości do innych modułów przy użyciu mechanizmu subskrypcji. Aby uzyskać więcej informacji, zobacz [wprowadzenie do usługi Azure IoT krawędzi][lnk-gw-getstarted].

![Diagram — moduły bramy komunikować się z brokera][2]

### <a name="protocol-ingestion-module"></a>Moduł pozyskiwania protokołu

Moduł wprowadzanie protokołu jest punkt początkowy dla procesu pobieranie danych z urządzeń, za pośrednictwem bramy i w chmurze. 

W przykładzie ten moduł:

1. Tworzy symulowane temperatury danych. Jeśli używasz urządzenia fizyczne moduł odczytuje dane z tych urządzeń fizycznych.
1. Tworzy komunikat.
1. Umieszczenie danych symulowane temperatury w treści wiadomości.
1. Dodaje właściwość fałszywy adres MAC do wiadomości.
1. Udostępnia komunikat do następnego modułu w łańcuchu.

Moduł wprowadzanie protokołu **simulated_device.c** w kodzie źródłowym.

### <a name="mac-lt-gt-iot-hub-id-module"></a>Adres MAC &lt;-&gt; moduł identyfikatora usługi IoT Hub

MAC &lt; - &gt; modułu identyfikator Centrum IoT działa jako translator. W tym przykładzie adres MAC jest używany jako unikatowy identyfikator urządzenia i jest korelowany z tożsamością urządzenia usługi IoT Hub. Możesz jednak napisać swój własny moduł, który będzie używał innego unikatowego identyfikatora. Na przykład urządzenia może mieć unikatowe numery seryjne lub dane telemetryczne mogą obejmować nazwę unikatową urządzenia osadzonego.

W przykładzie ten moduł:

1. Skanowanie w poszukiwaniu komunikatów, których właściwość adres MAC.
1. W przypadku adresu MAC, dodaje innej właściwości przy użyciu klucza urządzenia IoT Hub do wiadomości. 
1. Udostępnia komunikat do następnego modułu w łańcuchu.

Deweloper konfiguruje mapowanie między tożsamości Centrum IoT i adresów MAC do skojarzenia symulowanego urządzenia z Centrum IoT tożsamości urządzenia. Deweloper ręcznie dodaje mapowania w ramach konfiguracji modułu.

MAC &lt; - &gt; moduł identyfikator Centrum IoT to **identitymap.c** w kodzie źródłowym. 

### <a name="iot-hub-communication-module"></a>Moduł komunikacyjny usługi IoT Hub

Moduł komunikacji Centrum IoT otwiera pojedynczego połączenia HTTPS z bramy z Centrum IoT. HTTPS jest jednym z trzech protokołów rozpoznawany przez Centrum IoT. Ten moduł uniemożliwia konieczności otwierania połączenia dla każdego urządzenia przez Multipleksowanie połączenia ze wszystkich urządzeń za pośrednictwem jednego połączenia. Takie podejście umożliwia pojedyncza brama do łączenia wielu urządzeń. 

W przykładzie ten moduł:

1. Pobiera komunikaty z Centrum IoT urządzenia właściwości klucza, która została przypisana w poprzednim module. 
1. Wysyła zawartość komunikatu do Centrum IoT przy użyciu protokołu HTTPS. 

Moduł komunikacji Centrum IoT to **iothub.c** w kodzie źródłowym.

## <a name="before-you-get-started"></a>Przed rozpoczęciem

Przed rozpoczęciem pracy należy:

* [Tworzenie Centrum IoT] [ lnk-create-hub] w Twojej subskrypcji platformy Azure. Nazwa Centrum jest konieczne w ramach tego przewodnika próbki. Jeśli go nie masz, możesz utworzyć [bezpłatne konto][lnk-free-trial] w zaledwie kilka minut.
* Dodaj dwa urządzenia do Centrum IoT i zanotuj ich identyfikatorów i klucze urządzenia. Można użyć [explorer urządzenia] [ lnk-device-explorer] lub [explorer Centrum iothub] [ lnk-iothub-explorer] narzędzi, aby dodać urządzenia do Centrum IoT i pobieranie ich kluczy.


<!-- Images -->
[1]: media/iot-hub-iot-edge-simulated-selector/image1.png
[2]: media/iot-hub-iot-edge-simulated-selector/image2.png

<!-- Links -->
[symulowane urządzenie chmury Przekaż przykładowe]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/iot-edge
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
[lnk-create-hub]: ../articles/iot-hub/iot-hub-create-through-portal.md