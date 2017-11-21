---
title: "Moduł Azure IoT krawędzi C# | Dokumentacja firmy Microsoft"
description: "Utwórz moduł krawędzi IoT z kodem C# i wdróż je do urządzenia"
services: iot-edge
keywords: 
author: JimacoMS2
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: f93cfcdffd79b4cccdbd5f7c67ec42499bf7628c
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2017
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>Tworzenie i wdrażanie modułu krawędzi IoT C# w symulowane urządzenie — w wersji preview

Moduły krawędzi IoT umożliwia wdrażanie kodu, który implementuje logiki biznesowej bezpośrednio do urządzenia IoT krawędzi. W tym samouczku przedstawiono sposób tworzenia i wdrażania moduł krawędzi IoT filtrujące dane czujników na symulowane urządzenie brzegowe IoT utworzoną w programie IoT Edge Azure wdrożenia symulowanego urządzenia na [Windows] [ lnk-tutorial1-win]lub [Linux] [ lnk-tutorial1-lin] samouczki. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:    

> [!div class="checklist"]
> * Użyj programu Visual Studio Code, aby utworzyć moduł krawędzi IoT .NET core 2.0 w oparciu
> * Użyj programu Visual Studio Code i Docker, aby utworzyć obraz docker i opublikować ją w rejestrze 
> * Wdrażanie modułu do Twojego urządzenia IoT krawędzi
> * Widok wygenerowany danych


Moduł IoT krawędzi, który możesz utworzyć w tym samouczku filtruje temperatury dane generowane przez urządzenia i tylko wysyłanie wiadomości od początku po temperatura jest większa niż określona wartość progowa. 

## <a name="prerequisites"></a>Wymagania wstępne

* Urządzenie brzegowe IoT Azure utworzony w szybki start lub poprzednie samouczka.
* Parametry połączenia Centrum IoT Centrum IoT, która łączy się z urządzenia IoT krawędzi.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Rozszerzenie krawędzi IoT Azure dla programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). (Można zainstalować rozszerzenia z panelu rozszerzeń w Visual Studio Code.)
* [C# dla rozszerzenia Visual Studio Code (obsługiwane przez OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). (Można zainstalować rozszerzenia z panelu rozszerzeń w Visual Studio Code.)
* [Docker](https://docs.docker.com/engine/installation/). Community Edition (CE) dla danej platformy jest wystarczająca dla tego samouczka. Upewnij się, że można zainstalować na komputerze, który należy uruchomić na kodzie VS.
* [.NET core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="choose-or-sign-up-for-a-docker-registry"></a>Wybierz lub Załóż rejestru Docker
W tym samouczku umożliwia rozszerzenie Azure IoT krawędzi kodzie VS utworzenie modułu i Utwórz [obrazu Docker](https://docs.docker.com/glossary/?term=image). A następnie Wypchnij obraz Docker [repozytorium Docker](https://docs.docker.com/glossary/?term=repository) hostowanych na [Docker rejestru](https://docs.docker.com/glossary/?term=registry). Ponadto można wdrożyć obraz Docker dostarczana w [kontenera Docker](https://docs.docker.com/glossary/?term=container) z rejestru na urządzeniu IoT krawędzi.  

W tym samouczku, można użyć dowolnego rejestru zgodnego Docker. Są dwa popularne Docker rejestru usług dostępnych w chmurze **rejestru kontenera Azure** i **Centrum Docker**:

- [Azure rejestru kontenera](https://docs.microsoft.com/en-us/azure/container-registry/) jest dostępna w programie [płatnej subskrypcji](https://azure.microsoft.com/en-us/pricing/details/container-registry/). W tym samouczku **podstawowe** subskrypcji jest wystarczająca. 

- [Centrum docker](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) oferuje jedną wolnego prywatnym repozytorium, jeśli utworzysz konto dla identyfikatora (bezpłatnie) Docker 
    1. Aby utworzyć konto dla Identyfikatora Docker, postępuj zgodnie z instrukcjami w [zarejestrować dla Identyfikatora Docker](https://docs.docker.com/docker-id/#register-for-a-docker-id) w witrynie Docker. 

    2. Aby utworzyć prywatne repozytorium Docker, postępuj zgodnie z instrukcjami [utworzenie nowego repozytorium w Centrum Docker](https://docs.docker.com/docker-hub/repos/#creating-a-new-repository-on-docker-hub) w witrynie Docker.

W tym samouczku odpowiednio poleceń zapewnia się rejestru kontenera Azure i Centrum Docker.

## <a name="create-an-iot-edge-module-project"></a>Tworzenie projektu modułu krawędzi IoT
Pokaż następujące kroki należy jak utworzyć moduł krawędzi IoT na podstawie .NET core 2.0 przy użyciu programu Visual Studio Code i rozszerzenie Azure IoT krawędzi.
1. Otwórz VS kodu.
2. Użyj **widok | Zintegrowane Terminal** menu polecenie Otwórz terminal zintegrowane w kodzie VS.
3. W terminalu zintegrowanego, wprowadź następujące polecenie, aby zainstalować (lub zaktualizować) **AzureIoTEdgeModule** szablonu w dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

2. W terminalu zintegrowanego wprowadź następujące polecenie, aby utworzyć projekt dla nowego modułu:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule
    ```

    >[!NOTE]
    > To polecenie tworzy folder projektu **FilterModule**, w bieżącym folderze pracy. Jeśli chcesz utworzyć ją w innej lokalizacji, zmień katalogi przed uruchomieniem polecenia.
 
3. Użyj **pliku | Otwórz Folder** polecenia menu, przejdź do **FilterModule** folder, a następnie kliknij przycisk **wybierz Folder** otworzyć projektu w kodzie VS.
4. W kodzie VS explorer, kliknij przycisk **Program.cs** go otworzyć.
5. Dodaj następujące pole do **Program** klasy.

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

1. Dodaj następujące klasy do **Program** klasy. Te klasy definiują oczekiwano elementu schema dla treści wiadomości przychodzących.

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

1. W **Init** metoda, kod tworzy i konfiguruje **DeviceClient** obiektu. Ten obiekt umożliwia modułu do nawiązania połączenia lokalnego środowiska uruchomieniowego Azure IoT krawędzi do wysyłania i odbierania wiadomości. Parametr ciągu połączenia, które są używane w **Init** metody został dostarczony do modułu przez środowisko uruchomieniowe IoT krawędzi w zmiennej środowiskowej **EdgeHubConnectionString**. Po utworzeniu **DeviceClient**, kod rejestruje wywołanie zwrotne do odbierania wiadomości z Centrum IoT krawędzi za pośrednictwem **input1** punktu końcowego. Zastąp metodę użytą jako wywołania zwrotnego obsługi wiadomości na nową i Dołącz wywołań zwrotnych dla żądanej właściwości aktualizacje na dwie modułu. Aby to zrobić, Zamień ostatni wiersz **Init** metodę z następującym kodem:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

1. Dodaj następującą metodę do **Program** klasę, aby zaktualizować **temperatureThreshold** pole na podstawie żądanej właściwości wysłanych przez usługę zaplecza za pośrednictwem dwie modułu. Wszystkie moduły mają własne dwie modułu. Dwie moduł pozwala skonfigurować kod używany w module usługi zaplecza.

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

1. Zastąp **PipeMessage** metody przy użyciu następującej metody. Ta metoda jest wywoływana, gdy moduł wysyłany komunikat z Centrum krawędzi. Filtruje wiadomości na podstawie wartości temperatury w treści wiadomości i próg temperatury ustawione przez dwie modułu.

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

11. Skompiluj projekt. Użyj **widok | Eksplorator** polecenie, aby otworzyć Eksploratora kodzie VS. W Eksploratorze, kliknij prawym przyciskiem myszy **FilterModule.csproj** plik i kliknij przycisk **krawędzi IoT Tworzenie modułu** do kompilowania modułu i wyeksportuj plik binarny i jego zależności do utworzenia obrazu Docker folderu od w następnym kroku.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Tworzenie obrazu Docker i publikowanie go do rejestru

1. Utwórz obraz Docker.
    1. W kodzie VS explorer, kliknij przycisk **Docker** folderu, aby go otworzyć. Następnie wybierz folder dla danej platformy kontenera albo **linux x64** lub **windows nano**. 
    2. Kliknij prawym przyciskiem myszy **plik Dockerfile** plik i kliknij przycisk **krawędzi IoT Tworzenie modułu Docker obrazu**. 
    3. W **wybierz Folder** polu albo Przeglądaj lub wprowadź `./bin/Debug/netcoreapp2.0/publish`. Kliknij przycisk **wybierz Folder jako EXE_DIR**.
    4. W polu tekstowym wyskakujących w górnej części okna kodu programu VS wprowadź nazwę obrazu. Na przykład:`<docker registry address>/filtermodule:latest`, gdzie *adresu rejestru docker* jest Identyfikatorem Docker, jeśli używasz Centrum Docker lub podobne do `<your registry name>.azurecr.io`, jeśli używasz rejestru kontenera platformy Azure.
 
4. Zaloguj się do Docker. W terminalu zintegrowane wprowadź następujące polecenie: 

    - Centrum docker (Wprowadź swoje poświadczenia, po wyświetleniu monitu):
        
        ```csh/sh
        docker login
        ```

    - Dla kontenera platformy Azure rejestru:
        
        ```csh/sh
        docker login -u <username> -p <password> <Login server>
        ```
        
        Aby znaleźć nazwę użytkownika, hasło i zaloguj się za serwer do użycia w tym poleceniu, przejdź do [Azure portal] (https://portal.azure.com). Z **wszystkie zasoby**, kliknij Kafelek przypadku kontenera platformy Azure otworzyć jego właściwości, a następnie kliknij przycisk **klucze dostępu**. Skopiuj wartości w **Username**, **hasło**, i **logowania serwera** pola. Sould serwera logowania mieć postać: `<your registry name>.azurecr.io`.

3. Wypchnij obrazu do repozytorium Docker. Użyj **widok | Polecenie palety... | Krawędź: Wypychane IoT krawędzi modułu Docker obrazu** menu polecenie, a następnie wprowadź nazwę obrazu w polu tekstowym wyskakujących w górnej części okna kodu programu VS. Użyj takiej samej nazwy obraz używany w kroku 1.d.

## <a name="add-registry-credentials-to-edge-runtime-on-your-edge-device"></a>Dodawanie poświadczeń rejestru do środowiska wykonawczego Edge na urządzeniu krawędzi
Dodaj poświadczenia do rejestru do środowiska wykonawczego Edge na komputerze, na którym są uruchomione Twoje urządzenie brzegowe. Daje dostęp do środowiska uruchomieniowego do pobierania kontenera. 

- W systemie Windows uruchom następujące polecenie:
    
    ```cmd/sh
    iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

- Dla systemu Linux uruchom następujące polecenie:
    
    ```cmd/sh
    sudo iotedgectl login --address <docker-registry-address> --username <docker-username> --password <docker-password> 
    ```

## <a name="run-the-solution"></a>Uruchom rozwiązania

1. W [portalu Azure](https://portal.azure.com), przejdź do Centrum IoT.
2. Przejdź do **IoT krawędzi (wersja zapoznawcza)** i wybierz urządzenia IoT krawędzi.
3. Wybierz **ustawić modułów**. 
2. Dodaj **tempSensor** modułu. Ten krok jest tylko wymagane, jeśli nie zostały wcześniej wdrożone **tempSensor** modułu do Twojego urządzenia IoT krawędzi.
    1. Wybierz **Dodaj moduł krawędzi IoT**.
    2. W **nazwa** wprowadź `tempSensor`.
    3. W **identyfikatora URI obrazu** wprowadź `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Pozostaw innych ustawień bez zmian i kliknij przycisk **zapisać**.
9. Dodaj **filtermodule**
    1. Wybierz **Dodawanie modułu krawędzi IoT** ponownie.
    2. W **nazwa** wprowadź `filtermodule`.
    3. W **obrazu** wprowadź swój adres obrazu, na przykład `<docker registry address>/filtermodule:latest`.
    4. Sprawdź **edycji modułu dwie** pole.
    5. Zastąp dane JSON w polu tekstowym dla modułu dwie następujące JSON: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. Kliknij pozycję **Zapisz**.
12. Kliknij przycisk **Dalej**.
13. W **Określ tras** kroku, skopiuj kod JSON poniżej w polu tekstowym. Moduły opublikować wszystkie komunikaty do środowiska wykonawczego krawędzi. Deklaracyjne reguły w środowisku uruchomieniowym definiują, do których przepływu wiadomości. W tym samouczku należy na dwa sposoby. Pierwszy trasy transportu wiadomości z czujnika temperatury w module filtru za pośrednictwem punktu końcowego "input1", który jest punkt końcowy, który został skonfigurowany z **FilterMessages** programu obsługi. Drugi trasy transportu wiadomości z modułem filtru do Centrum IoT. W tej trasy `upstream` jest specjalne lokalizacji docelowej, która informuje krawędzi koncentratora do wysyłania komunikatów do Centrum IoT. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. Kliknij przycisk **Dalej**.
5. W **szablon przeglądu** kroku, kliknij przycisk **przesyłania**. 
6. Wróć do strony szczegółów urządzenia IoT krawędzi, a następnie kliknij przycisk **Odśwież**. Powinien zostać wyświetlony nowy **filtermodule** uruchomiona wraz z **tempSensor** modułu i **środowiska uruchomieniowego krawędzi IoT**. 

## <a name="view-generated-data"></a>Widok wygenerowany danych

Aby monitorować urządzenia do chmury wiadomości wysyłane z urządzenia IoT krawędzi do Centrum IoT:
1. Rozszerzenia Azure IoT Toolkit należy skonfigurować parametry połączenia Centrum IoT: 
    1. Użyj **widok | Eksplorator** polecenie, aby otworzyć Eksploratora kodzie VS. 
    3. W Eksploratorze kliknij **urządzenia IOT HUB** , a następnie kliknij przycisk **...** . Kliknij przycisk **ustawić parametry połączenia Centrum IoT** , a następnie wprowadź parametry połączenia dla Centrum IoT, które urządzenia IoT krawędzi łączy w wyskakującym oknie. 

        Aby znaleźć ciąg połączenia, kliknij Kafelek Centrum IoT w portalu Azure, a następnie kliknij przycisk **zasady dostępu współużytkowanego**. W **zasady dostępu współużytkowanego**, kliknij przycisk **iothubowner** zasad i skopiuj połączenia Centrum IoT ciąg w **iothubowner** okna.   

1. Do monitorowania danych otrzymywanych przez Centrum IoT należy używać **widok | Polecenie palety... | IoT: Uruchom komunikat D2C monitorowania** polecenia menu. 
2. Aby zatrzymać monitorowanie danych, należy użyć **widok | Polecenie palety... | IoT: Zatrzymaj komunikat D2C monitorowania** polecenia menu. 

## <a name="next-steps"></a>Następne kroki

W tym samouczku utworzony moduł IoT krawędzi, który zawiera kod, aby filtrować nieprzetworzone dane generowane przez urządzenia IoT krawędzi. Można kontynuować do jednej z następujących samouczków, aby uzyskać informacje o innych metod, które ułatwiają krawędź IoT Azure możesz przekształcić w danych biznesowych na krawędzi.

> [!div class="nextstepaction"]
> [Wdrażanie funkcji platformy Azure jako moduł](tutorial-deploy-function.md)
> [wdrożenia usługi Azure Stream Analytics jako moduł](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
