---
title: "Użyj Visual Studio Code do opracowywania C# modułu Azure IoT krawędzi | Dokumentacja firmy Microsoft"
description: "Opracowywania i wdrażania modułu C# Azure IoT krawędzi w kodzie VS bez przełączania kontekstu"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 269f77e5015175e45e0078926ef06699811889a4
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="use-visual-studio-code-to-develop-c-module-with-azure-iot-edge"></a>Użyj programu Visual Studio Code do opracowywania C# modułu krawędzi IoT Azure
Ten artykuł zawiera szczegółowe instrukcje dotyczące używania [Visual Studio Code](https://code.visualstudio.com/) jako narzędzie programowanie głównego do opracowywania i wdrażania własnych modułów IoT krawędzi. 

## <a name="prerequisites"></a>Wymagania wstępne
Ten samouczek zakłada, że używasz komputera lub maszyny wirtualnej z systemem Windows lub Linux jako komputerze deweloperskim. Urządzenia IoT krawędzi może być inne fizyczne urządzenie lub urządzenia IoT krawędzi można symulować na komputerze deweloperskim.

Upewnij się, że masz następujące samouczki ukończone przed rozpoczęciem tej wskazówki.
- Wdrożenie usługi Azure IoT krawędzi na symulowane urządzenie w [Windows](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-windows) lub [systemu Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)
- [Opracowywania i wdrażania modułu krawędzi IoT C# do symulowane urządzenie](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module)

Poniżej przedstawiono listę kontrolną, która zawiera elementy powinna mieć po zakończeniu poprzedniego samouczki.

- [Visual Studio Code](https://code.visualstudio.com/). 
- [Rozszerzenie krawędzi IoT Azure dla programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
- [C# dla rozszerzenia Visual Studio Code (obsługiwane przez OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 
- [Python 2.7](https://www.python.org/downloads/)
- [IoT krawędzią formantu skryptu](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
- Szablon AzureIoTEdgeModule (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Aktywnym Centrum IoT z co najmniej IoT urządzenia.

Proponowana jest również zainstalować [Docker obsługę kodzie VS](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) aby lepiej zarządzać modułu obrazów i kontenerów.

## <a name="deploy-an-azure-iot-edge-module-in-vs-code"></a>Wdrażanie modułu Azure IoT krawędzi w kodzie VS

### <a name="list-your-iot-hub-devices"></a>Lista urządzeń Centrum IoT
Istnieją dwa sposoby, aby wyświetlić listę urządzeń Centrum IoT w kodzie VS. Można wybrać w obu przypadkach, aby kontynuować.

#### <a name="sign-in-your-azure-account-in-vscode-and-choose-your-iot-hub"></a>Zaloguj się na koncie Azure w VSCode, a następnie wybierz pozycję Centrum IoT
1. Polecenie palety (F1 lub Ctrl + Shift + P), wpisz i wybierz **Azure: Zaloguj**. Następnie kliknij przycisk  **kopiowania* & Otwórz** w oknie podręcznym. Wklej (Ctrl + V) kod w przeglądarce, a następnie kliknij przycisk Kontynuuj. Następnie zaloguj się za pomocą konta platformy Azure. Informacje o swoim koncie w kodzie VS pasek stanu jest widoczny.
2. Polecenie palety (F1 lub Ctrl + Shift + P), wpisz i wybierz **IoT: Wybierz Centrum IoT**. Najpierw wybierz subskrypcję, której utworzono Centrum IoT w poprzednich instrukcji. Następnie wybierz pozycję Centrum IoT, które zawiera urządzenia IoT krawędzi.


#### <a name="set-iot-hub-connection-string"></a>Ustawianie parametrów połączenia Centrum IoT
1. Polecenie palety (F1 lub Ctrl + Shift + P), wpisz i wybierz **IoT: Ustaw parametry połączenia Centrum IoT**. Upewnij się, że możesz wkleić łączącego ciągu w obszarze zasady **iothubowner** (znajduje się on w zasady dostępu współużytkowanego Centrum IoT w portalu Azure).
 

Lista urządzeń w Eksploratorze urządzeń Centrum IoT w lewym pasku bocznym.

### <a name="start-your-iot-edge-runtime-and-deploy-a-module"></a>Rozpoczynanie pracy programu runtime krawędzi IoT i wdrażanie modułu
Zainstaluj i uruchom środowiska uruchomieniowego Azure IoT Edge na urządzeniu. I wdrożenie symulowane czujnik moduł, który będzie wysyłać dane telemetryczne z Centrum IoT.
1. Polecenie palety, wybierz **krawędzi: Instalator krawędzi** i wybierz z krawędzi IoT identyfikator urządzenia. Lub kliknij prawym przyciskiem myszy identyfikator krawędzi urządzenia na liście urządzeń i wybierz polecenie **krawędzi Instalatora**.
2. Polecenie palety, wybierz **krawędzi: Start krawędzi** można uruchomić programu runtime krawędzi. Widać odpowiednie dane wyjściowe w terminalu zintegrowanego.
3. Sprawdź stan środowiska uruchomieniowego krawędzi w Eksploratorze rozwiązania Docker. Zielony oznacza, że jest uruchomiona. Twoje środowisko uruchomieniowe krawędzi IoT została uruchomiona pomyślnie.
4. Teraz programu runtime krawędzi jest uruchomiona, co oznacza, że komputer teraz symuluje urządzenia. Następnym krokiem jest symulować sensorthing, który wysyła wiadomości do Twojego urządzenia krawędzi. W palecie polecenia, wpisz i wybierz **krawędzi: plik konfiguracji Generowanie krawędzi**. I wybierz folder do utworzenia tego pliku. W pliku wygenerowanego deployment.json, Zastąp linię "<registry>/<image>:<tag>" z `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
5. Wybierz **krawędzi: tworzenie wdrożenia dla urządzenie brzegowe** i wybierz identyfikator urządzenie brzegowe można utworzyć nowego wdrożenia. Lub kliknij prawym przyciskiem myszy identyfikator krawędzi urządzenia na liście urządzeń i wybierz **tworzenie wdrożenia dla urządzenie brzegowe**. 
6. Powinna zostać wyświetlona z krawędzi IoT uruchomione w Eksploratorze rozwiązania Docker z czujnika symulowane. Kliknij prawym przyciskiem myszy kontener, w Eksploratorze rozwiązania Docker. Możesz obserwować docker dzienniki dla każdego modułu.
7. Kliknij prawym przyciskiem myszy identyfikator urządzenia brzegowe i monitorować D2C wiadomości w kodzie VS.
8. Aby zatrzymać programu runtime krawędzi IoT i moduł czujnika, wpisz i wybierz **krawędzi: Zatrzymaj krawędzi** w palecie polecenia.

## <a name="develop-and-deploy-a-c-module-in-vs-code"></a>Tworzenie i wdrażanie modułu C# w kodzie VS
W samouczku [opracowanie modułu C#](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module), zaktualizować, kompilacji i opublikuj w kodzie VS obrazu modułu, a następnie odwiedź portalu Azure, aby wdrożyć modułu C#. W tej sekcji przedstawiono sposób użycia kodzie VS do wdrażania i monitorowania modułu C#.

### <a name="start-a-local-docker-registry"></a>Uruchom rejestru lokalnych docker
W tym samouczku, można użyć dowolnego rejestru zgodnego Docker. Są dwa popularne Docker rejestru usług dostępnych w chmurze [rejestru kontenera Azure](https://docs.microsoft.com/azure/container-registry/) i [Centrum Docker](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Ta sekcja używa [lokalnego rejestru Docker](https://docs.docker.com/registry/deploying/), który jest łatwiejsze do celów testowania podczas wcześniejszego programowania.
W kodzie VS **zintegrowane terminal**(Ctrl + "), uruchom następujące polecenia, aby uruchomić w rejestrze lokalnym.  

```cmd/sh
docker run -d -p 5000:5000 --name registry registry:2 
```

> [!NOTE]
> Powyżej przykładzie pokazano konfiguracji rejestru, które są tylko odpowiednie do testowania. Rejestru gotowe do produkcji musi być chroniony przez protokół TLS i najlepiej należy używać mechanizmu kontroli dostępu. Zalecane jest użycie [rejestru kontenera Azure](https://docs.microsoft.com/azure/container-registry/) lub [Centrum Docker](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) wdrażania modułów krawędzi IoT gotowe do produkcji.

### <a name="create-an-iot-edge-module-project"></a>Tworzenie projektu modułu krawędzi IoT
Pokaż następujące kroki należy jak utworzyć moduł krawędzi IoT na podstawie .NET core 2.0 przy użyciu programu Visual Studio Code i rozszerzenie Azure IoT krawędzi. Jeśli po wykonaniu tej sekcji samouczka poprzedniej, można bezpiecznie pominąć tę sekcję.
1. W programie Visual Studio Code, wybierz **widoku** > **zintegrowane terminali** otworzyć terminal zintegrowane kodzie VS.
3. W terminalu zintegrowanego, wprowadź następujące polecenie, aby zainstalować (lub zaktualizować) **AzureIoTEdgeModule** szablonu w dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

2. Tworzenie projektu dla nowego modułu. Poniższe polecenie tworzy folder projektu **FilterModule**, w bieżącym folderze roboczych:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule
    ```
 
3. Wybierz **pliku** > **Otwórz Folder**.
4. Przejdź do **FilterModule** folder i kliknij przycisk **wybierz Folder** otworzyć projektu w kodzie VS.
5. W kodzie VS explorer, kliknij przycisk **Program.cs** go otworzyć.
6. Dodaj `temperatureThreshold` zmienną **Program** klasy. Ta zmienna Określa wartość, której może przekraczać zmierzone temperatury dane do wysłania do Centrum IoT. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

7. Dodaj `MessageBody`, `Machine`, i `Ambient` klasy do **Program** klasy. Te klasy definiują oczekiwano elementu schema dla treści wiadomości przychodzących.

    ```csharp
    class MessageBody
    {
        public Machine machine {get;set;}
        public Ambient ambient {get; set;}
        public string timeCreated {get; set;}
    }
    class Machine
    {
       public double temperature {get; set;}
       public double pressure {get; set;}         
    }
    class Ambient
    {
       public double temperature {get; set;}
       public int humidity {get; set;}         
    }
    ```

8. W **Init** metoda, kod tworzy i konfiguruje **DeviceClient** obiektu. Ten obiekt umożliwia modułu do nawiązania połączenia lokalnego środowiska uruchomieniowego Azure IoT krawędzi do wysyłania i odbierania wiadomości. Parametry połączenia używane w **Init** metody został dostarczony do modułu przez środowisko uruchomieniowe IoT krawędzi. Po utworzeniu **DeviceClient**, kod rejestruje wywołanie zwrotne do odbierania wiadomości z Centrum IoT krawędzi za pośrednictwem **input1** punktu końcowego. Zastąp `SetInputMessageHandlerAsync` metody z nową i Dodaj `SetDesiredPropertyUpdateCallbackAsync` metodę dla żądanej właściwości aktualizacji. Aby to zrobić, Zamień ostatni wiersz **Init** metodę z następującym kodem:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

9. Dodaj `onDesiredPropertiesUpdate` metodę **Program** klasy. Ta metoda odbiera aktualizacje na odpowiednie właściwości z dwie modułu i aktualizuje **temperatureThreshold** ze zmienną. Wszystkie moduły mają własne dwie moduł, który umożliwia skonfigurowanie z kodem uruchomionym wewnątrz modułu bezpośrednio z chmury.

    ```csharp
    static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"]!=null)
                temperatureThreshold = desiredProperties["TemperatureThreshold"];

        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error when receiving desired property: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error when receiving desired property: {0}", ex.Message);
        }
        return Task.CompletedTask;
    }
    ```

10. Zastąp `PipeMessage` metody z `FilterMessages` metody. Ta metoda jest wywoływana, gdy moduł odbiera komunikat z Centrum IoT krawędzi. Odfiltrowuje wiadomości, które zgłosiły temperatury poniżej progu temperatura określonego przez dwie modułu. Dodano również **MessageType** właściwości komunikat z wartością ustawioną na **alertu**. 

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        int counterValue = Interlocked.Increment(ref counter);

        try {
            DeviceClient deviceClient = (DeviceClient)userContext;

            byte[] messageBytes = message.GetBytes();
            string messageString = Encoding.UTF8.GetString(messageBytes);
            Console.WriteLine($"Received message {counterValue}: [{messageString}]");

            // Get message body
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
                    $"exceeds threshold {temperatureThreshold}");
                var filteredMessage = new Message(messageBytes);
                foreach (KeyValuePair<string, string> prop in message.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }

                filteredMessage.Properties.Add("MessageType", "Alert");
                await deviceClient.SendEventAsync("output1", filteredMessage);
            }

            // Indicate that the message treatment is completed
            return MessageResponse.Completed;
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
            // Indicate that the message treatment is not completed
            DeviceClient deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed
            DeviceClient deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

11. Aby skompilować projekt, kliknij prawym przyciskiem myszy **FilterModule.csproj** plików w Eksploratorze i kliknij przycisk **krawędzi IoT Tworzenie modułu**. Ten proces kompiluje modułu i eksportuje dane binarne i jego zależności w folderze, który jest używany do tworzenia obrazu Docker.


### <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Tworzenie obrazu Docker i publikowanie go do rejestru

1. W Eksploratorze kodzie VS rozwiń **Docker** folderu. Następnie rozwiń folder dla danej platformy kontenera albo **linux x64** lub **windows nano**.
2. Kliknij prawym przyciskiem myszy **plik Dockerfile** plik i kliknij przycisk **krawędzi IoT Tworzenie modułu Docker obrazu**. 
3. W **wybierz Folder** okna, przejdź do albo wprowadź `./bin/Debug/netcoreapp2.0/publish`. Kliknij przycisk **wybierz Folder jako EXE_DIR**.
4. W polu tekstowym wyskakujących w górnej części okna kodu programu VS wprowadź nazwę obrazu. Na przykład: `<your container registry address>/filtermodule:latest`. Jeśli są wdrażane w lokalnym rejestrze, powinien być `localhost:5000/filtermodule:latest`.
5. Wypchnij obrazu do repozytorium Docker. Użyj **krawędzi: Push krawędzi IoT modułu Docker obrazu** polecenia i wprowadź adres URL obrazu, w polu tekstowym wyskakujących w górnej części okna kodu programu VS. Użyj używany w ponad krok ten sam adres URL obrazu.

### <a name="deploy-your-iot-edge-modules"></a>Wdrażanie własnych modułów krawędzi IoT

1. Otwórz `deployment.json` pliku, Zastąp **modułów** sekcji z poniżej zawartości:
    ```json
    "tempSensor": {
        "version": "1.0",
        "type": "docker",
        "status": "running",
        "restartPolicy": "always",
        "settings": {
            "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
            "createOptions": ""
        }
    },
    "filtermodule": {
        "version": "1.0",
        "type": "docker",
        "status": "running",
        "restartPolicy": "always",
        "settings": {
            "image": "localhost:5000/filtermodule:latest",
            "createOptions": ""
        }
    }
    ```

2. Zastąp **tras** sekcji z poniżej zawartości:
    ```json
    {
        "routes": {
            "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
            "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
        }
    }
    ```
   > [!NOTE]
   > Deklaracyjne reguły w środowisku uruchomieniowym definiują, do których przepływu wiadomości. W tym samouczku należy na dwa sposoby. Pierwszy trasy transportu wiadomości z czujnika temperatury w module filtru za pośrednictwem punktu końcowego "input1", który jest punkt końcowy, który został skonfigurowany z obsługą FilterMessages. Drugi trasy transportu wiadomości z modułem filtru do Centrum IoT. W tej trasy nadrzędnego jest specjalne lokalizacji docelowej, która informuje krawędzi koncentratora do wysyłania komunikatów do Centrum IoT.

3. Zapisz ten plik.
4. Polecenie palety, wybierz **krawędzi: tworzenie wdrożenia dla urządzenie brzegowe**. Następnie wybierz identyfikator urządzenia IoT krawędzi Aby utworzyć wdrożenie. Lub kliknij prawym przyciskiem myszy identyfikator urządzenia na liście urządzeń i wybierz polecenie **tworzenie wdrożenia dla urządzenie brzegowe**.
5. Wybierz `deployment.json` aktualne. W oknie danych wyjściowych widać odpowiednie dane wyjściowe dla danego wdrożenia.
6. Uruchomienie programu runtime krawędzi w palety polecenia. **Krawędź: Krawędzi początkowej**
7. Twoje krawędzi IoT środowiska uruchomieniowego uruchomione w Eksploratorze rozwiązania Docker symulowane czujnik i moduł filtru jest widoczny.
8. Kliknij prawym przyciskiem myszy identyfikator urządzenia brzegowe i monitorować D2C wiadomości w kodzie VS.


## <a name="next-steps"></a>Kolejne kroki

W tym samouczku utworzony moduł krawędzi IoT i wdrożyć go na urządzeniu IoT krawędzi w kodzie VS. Można kontynuować do jednej z następujących samouczków, aby dowiedzieć się więcej o innych scenariuszy podczas opracowywania Azure IoT krawędź w kodzie VS.

> [!div class="nextstepaction"]
> [Debugowanie modułu C# w kodzie VS](how-to-vscode-debug-csharp-module.md)
