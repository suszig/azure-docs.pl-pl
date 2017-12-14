---
title: "Rozpoczynanie pracy z usługą Azure IoT Hub (.NET) | Microsoft Docs"
description: "Dowiedz się, jak wysyłać komunikaty urządzenie-chmura do usługi Azure IoT Hub za pomocą zestawów SDK usługi IoT dla platformy .NET. Utwórz symulowane aplikacje urządzenia i usługi, aby zarejestrować urządzenie, wysyłać wiadomości i odczytywać wiadomości z usługi IoT Hub."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: f40604ff-8fd6-4969-9e99-8574fbcf036c
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ec9ff4a866ef8736ed260b4d17aa997433c1ef8d
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="connect-your-device-to-your-iot-hub-using-net"></a>Podłączanie urządzenia do usługi IoT Hub za pomocą środowiska .NET

[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na końcu tego samouczka będziesz mieć trzy aplikacje konsolowe .NET:

* **CreateDeviceIdentity** tworzy tożsamość urządzenia i skojarzony klucz zabezpieczeń w celu podłączenia aplikacji urządzenia.
* **ReadDeviceToCloudMessages** powoduje wyświetlenie telemetrii wysyłanej przez aplikację urządzenia.
* Projekt **SimulatedDevice** łączy się z usługą IoT Hub przy użyciu utworzonej wcześniej tożsamości urządzenia i wysyła komunikat telemetrii co sekundę przy użyciu protokołu MQTT.

Rozwiązanie Visual Studio z tymi trzema aplikacjami można pobrać lub sklonować z usługi Github.

```bash
git clone https://github.com/Azure-Samples/iot-hub-dotnet-simulated-device-client-app.git
```

> [!NOTE]
> Artykuł [Azure IoT SDKs][lnk-hub-sdks] (Zestawy SDK usługi Azure IoT) zawiera informacje dotyczące zestawów SDK usługi Azure IoT, przy użyciu których można tworzyć aplikacje zarówno do uruchamiania na urządzaniach, jak i w zapleczu rozwiązania.

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* Program Visual Studio 2015 lub Visual Studio 2017.
* Aktywne konto platformy Azure. (Jeśli go nie masz, możesz utworzyć [bezpłatne konto próbne][lnk-free-trial] w zaledwie kilka minut).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Centrum IoT zostało już utworzone i masz nazwę hosta oraz parametry połączenia usługi IoT Hub potrzebne do ukończenia pozostałej części tego samouczka.

<a id="DeviceIdentity_csharp"></a>
[!INCLUDE [iot-hub-get-started-create-device-identity-csharp](../../includes/iot-hub-get-started-create-device-identity-csharp.md)]

<a id="D2C_csharp"></a>
## <a name="receive-device-to-cloud-messages"></a>Odbieranie komunikatów z urządzenia do chmury
W tej sekcji opisano tworzenie aplikacji konsolowej .NET, która odczytuje komunikaty z urządzenia do chmury z usługi IoT Hub. Usługa IoT Hub udostępnia punkt końcowy zgodny z usługą [Azure Event Hubs][lnk-event-hubs-overview], aby umożliwić odczytywanie komunikatów z urządzenia do chmury. W celu uproszczenia informacji instrukcje w samouczku dotyczą tworzenia czytnika podstawowego, który nie jest odpowiedni do wdrożenia z użyciem dużej przepustowości. W samouczku [Process device-to-cloud messages][lnk-process-d2c-tutorial] (Przetwarzanie komunikatów przesyłanych z urządzeń do chmury) przedstawiono sposób przetwarzania komunikatów z urządzenia do chmury na dużą skalę. Więcej informacji na temat przetwarzania komunikatów z usługi Event Hubs znajduje się w samouczku [Rozpoczynanie pracy z usługą Event Hubs][lnk-eventhubs-tutorial]. (Ten samouczek dotyczy punktów końcowych usługi IoT Hub zgodnych z centrum zdarzeń).

> [!NOTE]
> Punkt końcowy zgodny z centrum zdarzeń przeznaczony do odczytywania komunikatów z urządzenia do chmury zawsze korzysta z protokołu AMQP.

1. W programie Visual Studio dodaj projekt Visual C# Windows Classic Desktop do bieżącego rozwiązania, używając szablonu projektu **Aplikacja konsoli (.NET Framework)**. Upewnij się, że program .NET Framework jest w wersji 4.5.1 lub nowszej. Nazwij projekt **ReadDeviceToCloudMessages**.

    ![Nowy projekt Visual C# Windows Classic Desktop][10a]

2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **ReadDeviceToCloudMessages**, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**.

3. W oknie **Menedżer pakietów NuGet** wyszukaj **WindowsAzure.ServiceBus**, wybierz opcję **Zainstaluj** i zaakceptuj warunki użytkowania. Ta procedura spowoduje pobranie, zainstalowanie i dodanie odniesienia do usługi [Azure Service Bus][lnk-servicebus-nuget] ze wszystkimi jej zależnościami. Ten pakiet umożliwia aplikacji połączenie się z punktem końcowym zgodnym z centrum zdarzeń w centrum IoT.

4. Dodaj następujące instrukcje `using` w górnej części pliku **Program.cs**:

    ```csharp
    using Microsoft.ServiceBus.Messaging;
    using System.Threading;
    ```

5. Dodaj następujące pola do klasy **Program**: Zastąp wartość symbolu zastępczego parametrami połączenia usługi IoT Hub dla centrum utworzonego w sekcji „Tworzenie centrum IoT”.

    ```csharp
    static string connectionString = "{iothub connection string}";
    static string iotHubD2cEndpoint = "messages/events";
    static EventHubClient eventHubClient;
    ```

6. Dodaj następującą metodę do klasy **Program**:

    ```csharp
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
    ```

    W metodzie tej używane jest wystąpienie **EventHubReceiver** do odbierania komunikatów ze wszystkich partycji odbioru z urządzenia do chmury centrum IoT. Zwróć uwagę na sposób przekazywania parametru `DateTime.Now` podczas tworzenia obiektu **EventHubReceiver** w taki sposób, aby odbierał komunikaty wysłane tylko po jego uruchomieniu. Ten filtr jest przydatny w środowisku testowym, gdyż umożliwia wyświetlenie bieżącego zestawu komunikatów. W środowisku produkcyjnym kod powinien sprawdzać, czy przetwarzane są wszystkie komunikaty. Więcej informacji znajduje się w samouczku [How to process IoT Hub device-to-cloud messages (Jak przetwarzać komunikaty z urządzenia do chmury w usłudze IoT Hub)][lnk-process-d2c-tutorial].

7. Na koniec dodaj następujące wiersze do metody **Główne**:

    ```csharp
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
    ```

## <a name="create-a-device-app"></a>Tworzenie aplikacji urządzenia

Ta sekcja zawiera instrukcje dotyczące tworzenia aplikacji konsolowej .NET, która symuluje urządzenie wysyłające komunikaty z urządzenia do chmury do usługi IoT Hub.

1. W programie Visual Studio dodaj projekt Visual C# Windows Classic Desktop do bieżącego rozwiązania, używając szablonu projektu **Aplikacja konsoli (.NET Framework)**. Upewnij się, że program .NET Framework jest w wersji 4.5.1 lub nowszej. Nazwij projekt **SimulatedDevice**.

    ![Nowy projekt Visual C# Windows Classic Desktop][10b]

2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **SimulatedDevice**, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**.

3. W oknie **Menedżera pakietów NuGet** wybierz opcję **Przeglądaj**, wyszukaj pozycję **Microsoft.Azure.Devices.Client**, wybierz opcję **Zainstaluj**, aby zainstalować pakiet **Microsoft.Azure.Devices.Client**, i zaakceptuj warunki użytkowania. Ta procedura spowoduje pobranie, zainstalowanie i dodanie odwołania do [pakietu NuGet zestawu SDK urządzenia w usłudze Azure IoT][lnk-device-nuget] oraz jego zależności.

4. Dodaj następującą instrukcję `using` w górnej części pliku **Program.cs**:

    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Newtonsoft.Json;
    ```

5. Dodaj następujące pola do klasy **Program**: Zastąp element `{iot hub hostname}` nazwą hosta centrum IoT pozyskaną w sekcji „Tworzenie centrum IoT”. Zastąp element `{device key}` kluczem urządzenia pozyskanym w sekcji „Tworzenie tożsamości urządzenia”.

    ```csharp
    static DeviceClient deviceClient;
    static string iotHubUri = "{iot hub hostname}";
    static string deviceKey = "{device key}";
    ```

6. Dodaj następującą metodę do klasy **Program**:

    ```csharp
    private static async void SendDeviceToCloudMessagesAsync()
    {
        double minTemperature = 20;
        double minHumidity = 60;
        int messageId = 1;
        Random rand = new Random();

        while (true)
        {
            double currentTemperature = minTemperature + rand.NextDouble() * 15;
            double currentHumidity = minHumidity + rand.NextDouble() * 20;

            var telemetryDataPoint = new
            {
                messageId = messageId++,
                deviceId = "myFirstDevice",
                temperature = currentTemperature,
                humidity = currentHumidity
            };
            var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
            var message = new Message(Encoding.ASCII.GetBytes(messageString));
            message.Properties.Add("temperatureAlert", (currentTemperature > 30) ? "true" : "false");

            await deviceClient.SendEventAsync(message);
            Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

            await Task.Delay(1000);
        }
    }
    ```

    Ta metoda wysyła nowy komunikat z urządzenia do chmury co sekundę. Komunikat zawiera obiekt serializacji JSON z identyfikatorem urządzenia i generowanymi losowo numerami do symulacji czujników temperatury i wilgotności.

7. Na koniec dodaj następujące wiersze do metody **Główne**:

    ```csharp
    Console.WriteLine("Simulated device\n");
    deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey ("myFirstDevice", deviceKey), TransportType.Mqtt);
    deviceClient.ProductInfo = "HappyPath_Simulated-CSharp";
    SendDeviceToCloudMessagesAsync();
    Console.ReadLine();
    ```

    Domyślnie metoda **Utwórz** w aplikacji .NET Framework tworzy wystąpienie **DeviceClient**, które używa protokołu AMQP do komunikowania się z usługą IoT Hub. Aby skorzystać z protokołu MQTT lub HTTPS, użyj zastępowania metody **Create** pozwalającego na określenie protokołu. Platforma uniwersalna systemu Windows oraz klienci PCL domyślnie używają protokołu HTTPS. Jeśli używasz protokołu HTTPS, dodaj również do projektu pakiet NuGet **Microsoft.AspNet.WebApi.Client**, aby dołączyć przestrzeń nazw **System.Net.Http.Formatting**.

Ten samouczek zawiera instrukcje tworzenia aplikacji urządzenia w usłudze IoT Hub. Możesz również użyć rozszerzenia programu Visual Studio [Connected Service for Azure IoT Hub][lnk-connected-service], aby dodać niezbędny kod do aplikacji urządzenia.

> [!NOTE]
> Dla uproszczenia ten samouczek nie zawiera opisu wdrożenia żadnych zasad ponawiania. W kodzie produkcyjnym należy wdrożyć zasady ponawiania (np. wycofywanie wykładnicze) zgodnie z sugestią w artykule z witryny MSDN [Transient Fault Handling][lnk-transient-faults] (Obsługa przejściowych błędów).

## <a name="run-the-apps"></a>Uruchamianie aplikacji

Teraz można przystąpić do uruchomienia aplikacji.

1. W programie Visual Studio w Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij przycisk **Ustaw projekty startowe**. Wybierz opcję **Wiele projektów startowych**, a następnie wybierz opcję **Uruchom** jako akcję dla projektów **ReadDeviceToCloudMessages** i **SimulatedDevice**.

    ![Właściwości projektu startowego][41]

2. Naciśnij klawisz **F5**, aby uruchomić obie aplikacje. Dane wyjściowe konsoli z aplikacji **SimulatedDevice** zawierają komunikaty, które aplikacja urządzenia wysyła do centrum IoT Hub. Dane wyjściowe konsoli z aplikacji **ReadDeviceToCloudMessages** zawierają komunikaty odbierane przez centrum IoT.

    ![Dane wyjściowe konsoli z aplikacji][42]

3. Na kafelku **Użycie** w [witrynie Azure Portal][lnk-portal] wyświetlana jest liczba komunikatów wysłanych do centrum IoT:

    ![Kafelek Użycie portalu Azure][43]

## <a name="next-steps"></a>Następne kroki

W tym samouczku opisano konfigurowanie centrum IoT Hub w witrynie Azure Portal, a następnie tworzenie tożsamości urządzenia w rejestrze tożsamości centrum IoT Hub. Tożsamość urządzenia została użyta, aby włączyć w aplikacji urządzenia funkcję wysyłania komunikatów z urządzenia do chmury do centrum IoT Hub. Utworzono również aplikację, która wyświetla komunikaty odbierane przez centrum IoT Hub.

Aby kontynuować wprowadzenie do usługi IoT Hub i zapoznać się z innymi scenariuszami IoT, zobacz:

* [Łączenie urządzenia][lnk-connect-device]
* [Wprowadzenie do zarządzania urządzeniami][lnk-device-management]
* [Getting started with IoT Edge][lnk-iot-edge] (Wprowadzenie do usługi IoT Edge)

Aby dowiedzieć się, jak rozszerzyć rozwiązanie IoT i przetwarzać komunikaty z urządzenia do chmury na dużą skalę, zobacz samouczek [Przetwarzanie komunikatów przesyłanych z urządzeń do chmury][lnk-process-d2c-tutorial].

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10a]: ./media/iot-hub-csharp-csharp-getstarted/create-receive-csharp1.png
[10b]: ./media/iot-hub-csharp-csharp-getstarted/create-device-csharp1.png


<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
