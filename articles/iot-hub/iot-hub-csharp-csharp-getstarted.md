<properties
    pageTitle="Usługa Azure IoT Hub dla środowiska C# — wprowadzenie | Microsoft Azure"
    description="Samouczek z wprowadzeniem do usługi Azure IoT Hub dla środowiska C#. Użycie usługi Azure IoT Hub i środowiska C# z zestawami SDK IoT Microsoft Azure w celu wdrożenia rozwiązania Internetu rzeczy. "
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>


# Usługa Azure IoT Hub dla środowiska .NET — wprowadzenie

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Po ukończeniu tego samouczka będziesz mieć trzy aplikacje konsolowe środowiska Windows:

* **CreateDeviceIdentity** tworzy tożsamość urządzenia i skojarzony klucz zabezpieczeń w celu podłączenia urządzenia symulowanego.
* **ReadDeviceToCloudMessages** powoduje wyświetlenie telemetrii wysyłanej przez urządzenie symulowane.
* **SimulatedDevice** łączy się z centrum IoT przy użyciu utworzonej wcześniej tożsamości urządzenia i wysyła komunikat telemetrii co sekundę przy użyciu protokołu AMQPS.

> [AZURE.NOTE] Informacje dotyczące różnych zestawów SDK, których można użyć do utworzenia aplikacji — zarówno do uruchamiania na urządzeniach jak i w zapleczu rozwiązania —znajdują się w artykule [IoT Hub SDKs] (Zestawy SDK usługi IoT Hub) [lnk-hub-sdks].

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

+ Microsoft Visual Studio 2015.

+ Aktywne konto platformy Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł [Bezpłatna wersja próbna platformy Azure][lnk_free_trial].

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Centrum IoT zostało już utworzone i masz nazwę hosta oraz parametry połączenia potrzebne do ukończenia pozostałej części tego samouczka.

## Tworzenie tożsamości urządzenia

W tej sekcji opisano tworzenie aplikacji konsolowej środowiska Windows, która tworzy tożsamość urządzenia w rejestrze tożsamości w centrum IoT. Urządzenie nie może połączyć się z centrum IoT, jeśli nie ma wpisu w rejestrze tożsamości urządzeń. Więcej informacji znajduje się w sekcji „Device identity registry” (Rejestr tożsamości urządzenia) artykułu [IoT Hub Developer Guide] (Usługa IoT Hub — przewodnik dewelopera)[lnk-devguide-identity]. Po uruchomieniu ta aplikacja konsoli generuje unikatowy identyfikator urządzenia i klucz, których urządzenie może użyć do zidentyfikowania się podczas wysyłania komunikatów do chmury do usługi IoT Hub.

1. W programie Visual Studio dodaj projekt Visual C# Windows Classic Desktop do bieżącego rozwiązania przy użyciu szablonu projektu **Aplikacja konsolowa**. Upewnij się, że program .NET Framework jest w wersji 4.5.1 lub nowszej. Nazwij projekt **CreateDeviceIdentity**.

    ![Nowy projekt Visual C# Windows Classic Desktop][10]

2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **CreateDeviceIdentity**, a następnie kliknij pozycję **Zarządzaj pakietami Nuget**.

3. W oknie **Menedżer pakietów Nuget** wybierz opcję **Przeglądaj**, wyszukaj pozycję **microsoft.azure.devices**, wybierz opcję **Zainstaluj**, aby zainstalować pakiet **Microsoft.Azure.Devices**, i zaakceptuj warunki użytkowania. Ta procedura spowoduje pobranie, zainstalowanie i dodanie odwołania do pakietu Nuget [zestawu SDK usługi Microsoft Azure IoT][lnk-nuget-service-sdk] oraz jego zależności.

    ![Okno Menedżer pakietów Nuget][11]

4. Dodaj następujące instrukcje `using` w górnej części pliku **Program.cs**:

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. Dodaj następujące pola do klasy **Program**: Zastąp wartość symbolu zastępczego parametrami połączenia dla centrum IoT utworzonego w poprzedniej sekcji.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Dodaj następującą metodę do klasy **Program**:

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    Ta metoda służy do tworzenia tożsamości urządzenia o identyfikatorze **myFirstDevice**. (Jeśli ten identyfikator urządzenia już istnieje w rejestrze, kod po prostu pobiera informacje o istniejącym urządzeniu). Aplikacja następnie wyświetla klucz podstawowy dla tej tożsamości. Tego klucza używa się w symulowanym urządzeniu, aby nawiązać połączenie z centrum IoT.

7. Na koniec dodaj następujące wiersze do metody **Główne**:

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Uruchom tę aplikację, a następnie zanotuj klucz urządzenia.

    ![Klucz urządzenia generowany przez aplikację][12]

> [AZURE.NOTE] Rejestr tożsamości usługi IoT Hub przechowuje tożsamości urządzenia tylko po to, aby umożliwić bezpieczny dostęp do centrum. Przechowuje identyfikatory urządzeń i klucze, które będą używane jako poświadczenia zabezpieczeń, oraz flagę włączone/wyłączone, która umożliwia wyłączenie dostępu do poszczególnych urządzeń. Jeśli aplikacja wymaga przechowywania innych metadanych dla określonego urządzenia, powinna korzystać z magazynu określonego dla aplikacji. Więcej informacji znajduje się w temacie [IoT Hub Developer Guide] (Usługa IoT Hub — przewodnik dewelopera)[lnk-devguide-identity].

## Odbieranie komunikatów z urządzenia do chmury

W tej sekcji opisano tworzenie aplikacji konsolowej środowiska Windows, która odczytuje komunikaty z urządzenia do chmury z usługi IoT Hub. Usługa IoT Hub udostępnia punkt końcowy zgodny z usługą [Azure Event Hubs][lnk-event-hubs-overview], aby umożliwić odczytywanie komunikatów z urządzenia do chmury. W celu uproszczenia informacji instrukcje w samouczku dotyczą tworzenia czytnika podstawowego, który nie jest odpowiedni do wdrożenia z użyciem dużej przepustowości. W samouczku [Process device-to-cloud messages] (Przetwarzanie komunikatów z urządzenia do chmury)[lnk-process-d2c-tutorial] przedstawiono, jak przetwarzać komunikaty z urządzenia do chmury na dużą skalę. Więcej informacji na temat przetwarzania komunikatów z usługi Event Hubs znajduje się w samouczku [Get Started with Event Hubs] (Usługa Event Hubs — wprowadzenie)[lnk-eventhubs-tutorial]. (Ten samouczek dotyczy punktów końcowych usługi IoT Hub zgodnych z usługą Event Hubs).

> [AZURE.NOTE] Punkt końcowy zgodny z usługą Event Hubs do odczytywania komunikatów z urządzenia do chmury zawsze korzysta z protokołu AMQPS.

1. W programie Visual Studio dodaj projekt Visual C# Windows Classic Desktop do bieżącego rozwiązania, używając szablonu projektu **Aplikacja konsolowa**. Upewnij się, że program .NET Framework jest w wersji 4.5.1 lub nowszej. Nazwij projekt **ReadDeviceToCloudMessages**.

    ![Nowy projekt Visual C# Windows Classic Desktop][10]

2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **ReadDeviceToCloudMessages**, a następnie kliknij pozycję **Zarządzaj pakietami Nuget**.

3. W oknie **Menedżer pakietów Nuget** wyszukaj pozycję **WindowsAzure.ServiceBus**, wybierz opcję **Zainstaluj** i zaakceptuj warunki użytkowania. Ta procedura spowoduje pobranie, zainstalowanie i dodanie odniesienia do usługi [Azure Service Bus][lnk-servicebus-nuget] ze wszystkimi jej zależnościami. Ten pakiet umożliwia połączenie z punktem końcowym zgodnym z usługą Event Hubs w centrum IoT.

4. Dodaj następujące instrukcje `using` w górnej części pliku **Program.cs**:

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. Dodaj następujące pola do klasy **Program**: Zastąp wartość symbolu zastępczego parametrami połączenia dla centrum IoT utworzonego w sekcji „Tworzenie centrum IoT”.

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. Dodaj następującą metodę do klasy **Program**:

        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;

                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }

    W metodzie tej używane jest wystąpienie **EventHubReceiver** do odbierania komunikatów ze wszystkich partycji odbioru z urządzenia do chmury centrum IoT. Zwróć uwagę na sposób przekazywania parametru `DateTime.Now` podczas tworzenia obiektu **EventHubReceiver** w taki sposób, aby odbierał komunikaty wysłane tylko po jego uruchomieniu. Ten filtr jest przydatny w środowisku testowym, gdyż umożliwia wyświetlenie bieżącego zestawu komunikatów. W środowisku produkcyjnym kod powinien sprawdzać, czy przetwarzane są wszystkie komunikaty. Więcej informacji znajduje się w samouczku [How to process IoT Hub device-to-cloud messages] (Jak przetwarzać komunikaty z urządzenia do chmury w usłudze IoT Hub)[lnk-process-d2c-tutorial].

7. Na koniec dodaj następujące wiersze do metody **Główne**:

        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        CancellationTokenSource cts = new CancellationTokenSource();

        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };

        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## Tworzenie aplikacji symulowanego urządzenia

Ta sekcja zawiera instrukcje dotyczące tworzenia aplikacji konsolowej środowiska Windows, która symuluje urządzenie wysyłające komunikaty z urządzenia do chmury do centrum IoT.

1. W programie Visual Studio dodaj projekt Visual C# Windows Classic Desktop do bieżącego rozwiązania, używając szablonu projektu **Aplikacja konsolowa**. Upewnij się, że program .NET Framework jest w wersji 4.5.1 lub nowszej. Nazwij projekt **SimulatedDevice**.

    ![Nowy projekt Visual C# Windows Classic Desktop][10]

2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **SimulatedDevice**, a następnie kliknij pozycję **Zarządzaj pakietami Nuget**.

3. W oknie **Menedżer pakietów Nuget** wybierz pozycję **Przeglądaj**, wyszukaj pozycję **Microsoft.Azure.Devices.Client**, wybierz pozycję **Zainstaluj**, aby zainstalować pakiet **Microsoft.Azure.Devices.Client**, i zaakceptuj warunki użytkowania. Ta procedura spowoduje pobranie, zainstalowanie i dodanie odwołania do [pakietu Nuget zestawu SDK urządzenia w usłudze Azure IoT Hub][lnk-device-nuget] oraz jego zależności.

4. Dodaj następującą instrukcję `using` w górnej części pliku **Program.cs**:

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. Dodaj następujące pola do klasy **Program**: Zastąp wartości symboli zastępczych nazwą hosta centrum IoT pobraną w sekcji „Tworzenie centrum IoT” oraz kluczem urządzenia pobranym w sekcji „Tworzenie tożsamości urządzenia”.

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. Dodaj następującą metodę do klasy **Program**:

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    Ta metoda wysyła nowy komunikat z urządzenia do chmury co sekundę. Komunikat zawiera obiekt serializacji JSON z identyfikatorem urządzenia i generowanym losowo numerem do symulacji czujnika prędkości wiatru.

7. Na koniec dodaj następujące wiersze do metody **Główne**:

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  Domyślnie metoda **Utwórz** tworzy wystąpienie **DeviceClient**, które używa protokołu AMQP do komunikowania się z usługą IoT Hub. Aby użyć protokołu HTTPS, użyj zastępowania metody **Utwórz**, które pozwala na określenie protokołu. Jeśli używasz protokołu HTTPS, dodaj również do projektu pakiet Nuget **Microsoft.AspNet.WebApi.Client**, aby dołączyć przestrzeń nazw **System.Net.Http.Formatting**.

Ten samouczek zawiera instrukcje tworzenia klienta urządzenia usługi IoT Hub. Możesz również użyć rozszerzenia programu Visual Studio [Connected Service for Azure IoT Hub][lnk-connected-service], aby dodać niezbędny kod do aplikacji klienta urządzenia.


> [AZURE.NOTE] Dla uproszczenia ten samouczek nie zawiera opisu wdrożenia żadnych zasad ponawiania. W kodzie produkcyjnym należy wdrożyć zasady ponawiania (np. wycofywanie wykładnicze) zgodnie z sugestią w artykule z witryny MSDN [Transient Fault Handling] (Obsługa przejściowego błędu)[lnk-transient-faults].

## Uruchamianie aplikacji

Teraz można uruchomić aplikacje.

1.  W programie Visual Studio w Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij przycisk **Ustaw projekty startowe**. Wybierz opcję **Wiele projektów startowych**, a następnie wybierz opcję **Uruchom** jako akcję dla projektów **ReadDeviceToCloudMessages** i **SimulatedDevice**.

    ![Właściwości projektu startowego][41]

2.  Naciśnij klawisz **F5**, aby uruchomić obie aplikacje. Dane wyjściowe konsoli z aplikacji **SimulatedDevice** zawierają komunikaty, które symulowane urządzenie wysyła do centrum IoT. Dane wyjściowe konsoli z aplikacji **ReadDeviceToCloudMessages** zawierają komunikaty odbierane przez centrum IoT.

    ![Dane wyjściowe konsoli z aplikacji][42]

3. Na kafelku **Użycie** w [portalu Azure][lnk-portal] wyświetlana jest liczba komunikatów wysłanych do centrum:

    ![Kafelek Użycie portalu Azure][43]


## Następne kroki

W tym samouczku opisano konfigurowanie centrum IoT w portalu, a następnie tworzenie tożsamości urządzenia w rejestrze tożsamości centrum. Tożsamość urządzenia została użyta, aby włączyć w aplikacji symulowanego urządzenia funkcję wysyłania komunikatów z urządzenia do chmury do centrum. Utworzono również aplikację, która wyświetla komunikaty odbierane przez centrum. 

Aby kontynuować wprowadzenie do usługi IoT Hub i zapoznać się z innymi scenariuszami IoT, zobacz:

- [Łączenie urządzenia][lnk-connect-device]
- [Wprowadzenie do zarządzania urządzeniami][lnk-device-management]
- [Getting started with the Gateway SDK][lnk-gateway-SDK] (Wprowadzenie do korzystania z zestawu SDK bramy)

W samouczku [Process device-to-cloud messages][lnk-process-d2c-tutorial] (Przetwarzanie komunikatów z urządzenia do chmury) przedstawiono, jak rozszerzyć rozwiązanie IoT i przetwarzać komunikaty z urządzenia do chmury na dużą skalę.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-sdks-summary.md
[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide.md#identityregistry
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/



<!--HONumber=Sep16_HO3-->


