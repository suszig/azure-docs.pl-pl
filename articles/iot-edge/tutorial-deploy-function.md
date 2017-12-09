---
title: "Wdrażanie funkcji Azure Azure IoT krawędzi | Dokumentacja firmy Microsoft"
description: "Wdrażanie funkcji platformy Azure jako moduł do urządzenia"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1dfe46d307a076ae02362c4bba292602001ed915
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2017
---
# <a name="deploy-azure-function-as-an-iot-edge-module---preview"></a>Wdrażanie funkcji platformy Azure jako moduł krawędzi IoT — w wersji preview
Aby wdrożyć kod, który implementuje logiki biznesowej bezpośrednio do urządzenia IoT krawędzi, możesz użyć funkcji Azure. W tym samouczku przedstawiono sposób tworzenia i wdrażania funkcji Azure filtrujące dane czujników na symulowane urządzenie brzegowe IoT utworzoną w programie IoT Edge Azure wdrożenia symulowanego urządzenia na [Windows] [ lnk-tutorial1-win]lub [Linux] [ lnk-tutorial1-lin] samouczki. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:     

> [!div class="checklist"]
> * Visual Studio Code umożliwia tworzenie funkcji platformy Azure
> * Użyj kodzie VS i Docker, aby utworzyć obraz Docker i opublikować ją w rejestrze 
> * Wdrażanie modułu do Twojego urządzenia IoT krawędzi
> * Widok wygenerowany danych


Funkcji platformy Azure, który utworzono w tym samouczku filtruje temperatury dane generowane przez urządzenia i tylko wysyła komunikaty od początku do Centrum IoT Azure, gdy temperatura jest większa niż określona wartość progowa. 

## <a name="prerequisites"></a>Wymagania wstępne

* Urządzenie brzegowe IoT Azure utworzony w szybki start lub poprzednie samouczka.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [C# dla rozszerzenia Visual Studio Code (obsługiwane przez OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Rozszerzenie krawędzi IoT Azure dla programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Docker](https://docs.docker.com/engine/installation/). Community Edition (CE) dla danej platformy jest wystarczająca dla tego samouczka. 
* [.NET core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Tworzenie rejestru kontenerów
W tym samouczku umożliwia rozszerzenie Azure IoT krawędzi kodzie VS utworzenie modułu i Utwórz **obrazu kontenera** z plików. A następnie Wypchnij tego obrazu, aby **rejestru** który przechowywanych i zarządzanych obrazów. Ponadto można wdrożyć obraz z rejestru do uruchomienia na urządzeniu IoT krawędzi.  

W tym samouczku, można użyć dowolnego rejestru zgodnego Docker. Są dwa popularne Docker rejestru usług dostępnych w chmurze [rejestru kontenera Azure](https://docs.microsoft.com/azure/container-registry/) i [Centrum Docker](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). W tym samouczku korzysta z rejestru kontenera platformy Azure. 

1. W [portalu Azure](https://portal.azure.com), wybierz pozycję **Utwórz zasób** > **kontenery** > **rejestru kontenera Azure** .
2. Nadaj nazwę rejestru, wybierz subskrypcję, wybierz grupę zasobów i ustawić jednostki SKU **podstawowe**. 
3. Wybierz pozycję **Utwórz**.
4. Po utworzeniu kontenera rejestru przejdź do niego i wybierz **klucze dostępu**. 
5. Przełącz **administrator** do **włączyć**.
6. Skopiuj wartości **logowania serwera**, **Username**, i **hasło**. Te wartości będą używane w dalszej części tego samouczka. 

## <a name="create-a-function-project"></a>Tworzenie projektu — funkcja
Poniższe kroki pokazują, jak utworzyć funkcję krawędzi IoT przy użyciu programu Visual Studio Code i rozszerzenie Azure IoT krawędzi.
1. Otwórz kod programu Visual Studio.
2. Aby otworzyć terminal zintegrowane kodzie VS, wybierz **widoku** > **zintegrowane terminali**.
3. Aby zainstalować (lub zaktualizować) **AzureIoTEdgeFunction** szablonu w dotnet, uruchom następujące polecenie w zintegrowanym terminala:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. Tworzenie projektu dla nowego modułu. Poniższe polecenie tworzy folder projektu **funkcji FilterFunction**, w bieżącym folderze roboczych:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction
    ```

3. Wybierz **pliku** > **Otwórz Folder**, a następnie wyszukaj **funkcji FilterFunction** folderu i Otwórz projekt w kodzie VS.
4. W Eksploratorze kodzie VS rozwiń **EdgeHubTrigger Csharp** folderu, otwórz **run.csx** pliku.
5. Zastąp zawartość pliku następującym kodem:

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
        const int temperatureThreshold = 25;
        byte[] messageBytes = messageReceived.GetBytes();
        var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

        if (!string.IsNullOrEmpty(messageString))
        {
            // Get the body of the message and deserialize it
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                // Send the message to the output as the temperature value is greater than the threashold
                var filteredMessage = new Message(messageBytes);
                // Copy the properties of the original message into the new Message object
                foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }
                // Add a new property to the message to indicate it is an alert
                filteredMessage.Properties.Add("MessageType", "Alert");
                // Send the message        
                await output.AddAsync(filteredMessage);
                log.Info("Received and transferred a message with temperature above the threshold");
            }
        }
    }

    //Define the expected schema for the body of incoming messages
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

11. Zapisz plik.

## <a name="publish-a-docker-image"></a>Publikowanie obrazu Docker

1. Utwórz obraz Docker.
    1. W Eksploratorze kodzie VS rozwiń **Docker** folderu. Następnie rozwiń folder dla danej platformy kontenera albo **linux x64** lub **windows nano**. 
    2. Kliknij prawym przyciskiem myszy **plik Dockerfile** plik i kliknij przycisk **krawędzi IoT Tworzenie modułu Docker obrazu**. 
    3. Przejdź do **funkcji FilterFunction** folderu projektu i kliknij przycisk **wybierz Folder jako EXE_DIR**. 
    4. W polu tekstowym wyskakujących w górnej części okna kodu programu VS wprowadź nazwę obrazu. Na przykład: `<your container registry address>/filterfunction:latest`. Adres rejestru kontenera jest taki sam jak serwer logowania, które zostały skopiowane z rejestru. Powinna być w formie `<your container registry name>.azurecr.io`.
 
4. Zaloguj się do Docker. W terminalu zintegrowanego wprowadź następujące polecenie: 

   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Aby znaleźć nazwę użytkownika, hasło i zaloguj się za serwer do użycia w tym poleceniu, przejdź do [Azure portal] (https://portal.azure.com). Z **wszystkie zasoby**, kliknij Kafelek przypadku kontenera platformy Azure otworzyć jego właściwości, a następnie kliknij przycisk **klucze dostępu**. Skopiuj wartości w **Username**, **hasło**, i **logowania serwera** pola. 

3. Wypchnij obrazu do repozytorium Docker. Wybierz **widoku** > **polecenia palety...**  następnie wyszukaj **krawędzi: Push krawędzi IoT modułu Docker obrazu**.
4. W polu tekstowym wyskakujących wprowadzić taką samą nazwę obrazu, który został użyty w kroku 1.d.

## <a name="add-registry-credentials-to-your-edge-device"></a>Dodaj rejestru poświadczenia na urządzeniu krawędzi
Dodaj poświadczenia do rejestru do środowiska wykonawczego Edge na komputerze, na którym są uruchomione Twoje urządzenie brzegowe. Daje dostęp do środowiska uruchomieniowego do pobierania kontenera. 

- W systemie Windows uruchom następujące polecenie:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Dla systemu Linux uruchom następujące polecenie:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Uruchom rozwiązania

1. W **portalu Azure**, przejdź do Centrum IoT.
2. Przejdź do **IoT krawędzi (wersja zapoznawcza)** i wybierz urządzenia IoT krawędzi.
1. Wybierz **ustawić modułów**. 
2. Jeśli została już wdrożona **tempSensor** modułu do tego urządzenia, jego może być wypełnione automatycznie. Jeśli nie, wykonaj następujące kroki, aby dodać go:
    1. Wybierz **Dodaj moduł krawędzi IoT**.
    2. W **nazwa** wprowadź `tempSensor`.
    3. W **identyfikatora URI obrazu** wprowadź `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Pozostaw innych ustawień bez zmian i kliknij przycisk **zapisać**.
1. Dodaj **funkcji filterFunction** modułu.
    1. Wybierz **Dodawanie modułu krawędzi IoT** ponownie.
    2. W **nazwa** wprowadź `filterFunction`.
    3. W **obrazu** wprowadź swój adres obrazu, na przykład `<docker registry address>/filterfunction:latest`.
    74. Kliknij pozycję **Zapisz**.
2. Kliknij przycisk **Dalej**.
3. W **Określ tras** kroku, skopiuj kod JSON poniżej w polu tekstowym. Pierwszy trasy transportu wiadomości z czujnika temperatury w module filtru za pośrednictwem punktu końcowego "input1". Drugi trasy transportu wiadomości z modułem filtru do Centrum IoT. W tej trasy `$upstream` jest specjalne lokalizacji docelowej, która informuje krawędzi koncentratora do wysyłania komunikatów do Centrum IoT. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterFunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterFunction/outputs/* INTO $upstream"
       }
    }
    ```

4. Kliknij przycisk **Dalej**.
5. W **szablon przeglądu** kroku, kliknij przycisk **przesyłania**. 
6. Wróć do strony szczegółów urządzenia IoT krawędzi, a następnie kliknij przycisk **Odśwież**. Powinien zostać wyświetlony nowy **funkcji filterfunction** uruchomionego wraz z modułu **tempSensor** modułu i **środowiska uruchomieniowego krawędzi IoT**. 

## <a name="view-generated-data"></a>Widok wygenerowany danych

Aby monitorować urządzenia do chmury wiadomości wysyłane z urządzenia IoT krawędzi do Centrum IoT:
1. Rozszerzenia Azure IoT Toolkit należy skonfigurować parametry połączenia Centrum IoT: 
    1. W portalu Azure, przejdź do Centrum IoT i wybierz **zasady dostępu współużytkowanego**. 
    2. Wybierz **iothubowner** następnie skopiować wartość **klucz podstawowy ciągu połączenia**.
    1. W Eksploratorze kodzie VS kliknij **urządzenia IOT HUB** , a następnie kliknij przycisk **...** . 
    1. Wybierz **ustawić parametry połączenia Centrum IoT** i wprowadź parametry połączenia Centrum Iot w oknie podręcznym. 

1. Aby monitorować danych otrzymywanych przez Centrum IoT, wybierz **widoku** > **palety polecenia...**  i wyszukaj **IoT: rozpocząć monitorowanie komunikat D2C**. 
2. Aby zatrzymać monitorowanie danych, należy użyć **IoT: zatrzymać monitorowanie komunikat D2C** w palecie polecenia. 

## <a name="next-steps"></a>Następne kroki

W tym samouczku został utworzony funkcji platformy Azure, który zawiera kod, aby filtrować nieprzetworzone dane generowane przez urządzenia IoT krawędzi. Aby zachować Eksplorowanie usługi Azure IoT krawędzi, Dowiedz się, jak używać urządzenia IoT jako brama. 

> [!div class="nextstepaction"]
> [Tworzenie urządzenia bramy IoT](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
