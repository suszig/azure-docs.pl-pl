---
title: "Wdrażanie funkcji Azure Azure IoT krawędzi | Dokumentacja firmy Microsoft"
description: "Wdrażanie funkcji platformy Azure jako moduł do urządzenia"
services: iot-edge
keywords: 
author: JimacoMS2
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 82019392edc99f0adccb26b483e77b7c576822fb
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
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
* [C# dla rozszerzenia Visual Studio Code (obsługiwane przez OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). (Można zainstalować rozszerzenia z panelu rozszerzeń w Visual Studio Code.)
* [Rozszerzenie krawędzi IoT Azure dla programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). (Można zainstalować rozszerzenia z panelu rozszerzeń w Visual Studio Code.)
* [Docker](https://docs.docker.com/engine/installation/). Community Edition (CE) dla danej platformy jest wystarczająca dla tego samouczka. 
* [.NET core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="set-up-a-docker-registry"></a>Ustawienie rejestru Docker
W tym samouczku umożliwia rozszerzenie Azure IoT krawędzi kodzie VS Tworzenie funkcji platformy Azure i kompilacji [obrazu Docker](https://docs.docker.com/glossary/?term=image) z nim. A następnie Wypchnij obraz Docker [repozytorium Docker](https://docs.docker.com/glossary/?term=repository) obsługiwanych przez [Docker rejestru](https://docs.docker.com/glossary/?term=registry). Ponadto można wdrożyć obraz Docker dostarczana w [kontenera Docker](https://docs.docker.com/glossary/?term=container) z rejestru na urządzeniu IoT krawędzi.  

W tym samouczku, można użyć dowolnego rejestru zgodnego Docker. Są dwa popularne Docker rejestru usług dostępnych w chmurze **rejestru kontenera Azure** i **Centrum Docker**:

- [Azure rejestru kontenera](https://docs.microsoft.com/en-us/azure/container-registry/) jest dostępna w programie [płatnej subskrypcji](https://azure.microsoft.com/en-us/pricing/details/container-registry/). W tym samouczku **podstawowe** subskrypcji jest wystarczająca. 

- [Centrum docker](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) oferuje jedną wolnego prywatnym repozytorium, jeśli utworzysz konto dla identyfikatora (bezpłatnie) Docker 
    1. Aby utworzyć konto dla Identyfikatora Docker, postępuj zgodnie z instrukcjami w [zarejestrować dla Identyfikatora Docker](https://docs.docker.com/docker-id/#register-for-a-docker-id) w witrynie Docker. 

    2. Aby utworzyć prywatne repozytorium Docker, postępuj zgodnie z instrukcjami [utworzenie nowego repozytorium w Centrum Docker](https://docs.docker.com/docker-hub/repos/#creating-a-new-repository-on-docker-hub) w witrynie Docker.

W tym samouczku odpowiednio polecenia podano rejestru kontenera Azure i Centrum Docker.

## <a name="create-a-function-project"></a>Tworzenie projektu — funkcja
Poniższe kroki pokazują, jak utworzyć funkcję krawędzi IoT przy użyciu programu Visual Studio Code i rozszerzenie Azure IoT krawędzi.
1. Otwórz VS kodu.
2. Użyj **widok | Zintegrowane Terminal** menu polecenie Otwórz terminal zintegrowane w kodzie VS.
3. W terminalu zintegrowanego, wprowadź następujące polecenie, aby zainstalować (lub zaktualizować) **AzureIoTEdgeFunction** szablonu w dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. W terminalu zintegrowanego wprowadź następujące polecenie, aby utworzyć projekt dla nowego modułu:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction
    ```

    >[!NOTE]
    > To polecenie tworzy folder projektu **funkcji FilterFunction**, w bieżącym folderze pracy. Jeśli chcesz utworzyć ją w innej lokalizacji, zmień katalogi przed uruchomieniem polecenia.

3. Użyj **pliku | Otwórz Folder** polecenia menu, przejdź do **funkcji FilterFunction** folder, a następnie kliknij przycisk **wybierz Folder** otworzyć projektu w kodzie VS.
4. W kodzie VS explorer, kliknij przycisk **EdgeHubTrigger Csharp** folderu, następnie kliknij przycisk **run.csx** plik, aby go otworzyć.
5. Dodaj następującą instrukcję po `#r "Microsoft.Azure.Devices.Client"` instrukcji:

    ```csharp
    #r "Newtonsoft.Json"
    ```

5. Dodaj następujące instrukcje using po istniejącej `using` instrukcji:

    ```csharp
    using Newtonsoft.Json;
    ```

1. Dodaj następujące klasy. Te klasy definiują oczekiwano elementu schema dla treści wiadomości przychodzących.

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

1. Zastąp treści **Uruchom** metodę z następującym kodem. Filtruje wiadomości na podstawie wartości temperatury w treści wiadomości i temperatury wartość progową.

    ```csharp
    const int temperatureThreshold = 25;

    byte[] messageBytes = messageReceived.GetBytes();
    var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

    if (!string.IsNullOrEmpty(messageString))
    {
        // Get the body of the message and deserialize it
        var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);
        
        if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
        {
            // We will send the message to the output as the temperature value is greater than the threashold
            var filteredMessage = new Message(messageBytes);
            // We need to copy the properties of the original message into the new Message object
            foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
            {
                filteredMessage.Properties.Add(prop.Key, prop.Value);
            }
            // We are adding a new property to the message to indicate it is an alert
            filteredMessage.Properties.Add("MessageType", "Alert");
            // Send the message        
            await output.AddAsync(filteredMessage);
            log.Info("Received and transfered a message with temperature above the threshold");
        }
    }
    ```

11. Zapisz plik.

## <a name="publish-a-docker-image"></a>Publikowanie obrazu Docker

1. Utwórz obraz Docker.
    1. W kodzie VS explorer, kliknij przycisk **Docker** folderu, aby go otworzyć. Następnie wybierz folder dla danej platformy kontenera albo **linux x64** lub **windows nano**. 
    2. Kliknij prawym przyciskiem myszy **plik Dockerfile** plik i kliknij przycisk **krawędzi IoT Tworzenie modułu Docker obrazu**. 
    3. W **wybierz Folder** polu, przejdź do folderu projektu **funkcji FilterFunction**i kliknij przycisk **wybierz Folder jako EXE_DIR**. 
    4. W polu tekstowym wyskakujących w górnej części okna kodu programu VS wprowadź nazwę obrazu. Na przykład `<docker registry address>/filterfunction:latest`, gdzie *adresu rejestru docker* jest Identyfikatorem Docker, jeśli używasz Centrum Docker lub jest podobny do `<your registry name>.azurecr.io`, jeśli używasz rejestru kontenera platformy Azure.
 
4. Zaloguj się do Docker. W terminalu zintegrowane wprowadź następujące polecenie: 

    - Centrum docker (Wprowadź swoje poświadczenia, po wyświetleniu monitu):
        
        ```csh/sh
        docker login
        ```

    - Rejestr kontenera platformy Azure:
        
        ```csh/sh
        docker login -u <username> -p <password> <Login server>
        ```
        
        Aby znaleźć nazwę użytkownika, hasło i zaloguj się za serwer do użycia w tym poleceniu, przejdź do [Azure portal] (https://portal.azure.com). Z **wszystkie zasoby**, kliknij Kafelek przypadku kontenera platformy Azure otworzyć jego właściwości, a następnie kliknij przycisk **klucze dostępu**. Skopiuj wartości w **Username**, **hasło**, i **logowania serwera** pola. Sould serwera logowania mieć postać: `<your registry name>.azurecr.io`.

3. Wypchnij obrazu do repozytorium Docker. Użyj **widok | Polecenie palety... | Krawędź: Wypychane IoT krawędzi modułu Docker obrazu** menu polecenie, a następnie wprowadź nazwę obrazu w polu tekstowym wyskakujących w górnej części okna kodu programu VS. Użyj takiej samej nazwy obraz używany w kroku 1.c.

## <a name="add-registry-credentials-to-your-edge-device"></a>Dodaj rejestru poświadczenia na urządzeniu krawędzi
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

1. W **portalu Azure**, przejdź do Centrum IoT.
2. Przejdź do **IoT krawędzi (wersja zapoznawcza)** i wybierz urządzenia IoT krawędzi.
1. Wybierz **ustawić modułów**. 
2. Dodaj **tempSensor** modułu. Ten krok jest tylko wymagane, jeśli nie zostały wcześniej wdrożone **tempSensor** modułu do Twojego urządzenia IoT krawędzi.
    1. Wybierz **Dodaj moduł krawędzi IoT**.
    2. W **nazwa** wprowadź `tempSensor`.
    3. W **identyfikatora URI obrazu** wprowadź `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Pozostaw innych ustawień bez zmian i kliknij przycisk **zapisać**.
1. Dodaj **funkcji filterfunction** modułu.
    1. Wybierz **Dodawanie modułu krawędzi IoT** ponownie.
    2. W **nazwa** wprowadź `filterfunction`.
    3. W **obrazu** wprowadź swój adres obrazu, na przykład `<docker registry address>/filterfunction:latest`.
    74. Kliknij pozycję **Zapisz**.
2. Kliknij przycisk **Dalej**.
3. W **Określ tras** kroku, skopiuj kod JSON poniżej w polu tekstowym. Moduły opublikować wszystkie komunikaty do środowiska wykonawczego krawędzi. Deklaracyjne reguły w środowisku uruchomieniowym definiują, do których przepływu wiadomości. W tym samouczku należy na dwa sposoby. Pierwszy trasy transportu wiadomości z czujnika temperatury w module filtru za pośrednictwem punktu końcowego "input1", który jest punkt końcowy, który został skonfigurowany z **FilterMessages** programu obsługi. Drugi trasy transportu wiadomości z modułem filtru do Centrum IoT. W tej trasy `upstream` jest specjalne lokalizacji docelowej, która informuje krawędzi koncentratora do wysyłania komunikatów do Centrum IoT. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterfunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterfunction/outputs/* INTO $upstream"
       }
    }
    ```

4. Kliknij przycisk **Dalej**.
5. W **szablon przeglądu** kroku, kliknij przycisk **przesyłania**. 
6. Wróć do strony szczegółów urządzenia IoT krawędzi, a następnie kliknij przycisk **Odśwież**. Powinien zostać wyświetlony nowy **funkcji filterfunction** uruchomionego wraz z modułu **tempSensor** modułu i **środowiska uruchomieniowego krawędzi IoT**. 

## <a name="view-generated-data"></a>Widok wygenerowany danych

Aby monitorować urządzenia do chmury wiadomości wysyłane z urządzenia IoT krawędzi do Centrum IoT:
1. Rozszerzenia Azure IoT Toolkit należy skonfigurować parametry połączenia Centrum IoT: 
    1. Użyj **widok | Eksplorator** polecenie, aby otworzyć Eksploratora kodzie VS. 
    3. W Eksploratorze kliknij **urządzenia IOT HUB** , a następnie kliknij przycisk **...** . Kliknij przycisk **ustawić parametry połączenia Centrum IoT** , a następnie wprowadź parametry połączenia dla Centrum IoT, które urządzenia IoT krawędzi łączy w wyskakującym oknie. 

        Aby znaleźć ciąg połączenia, kliknij Kafelek Centrum IoT w portalu Azure, a następnie kliknij przycisk **zasady dostępu współużytkowanego**. W **zasady dostępu współużytkowanego**, kliknij przycisk **iothubowner** zasad i skopiuj połączenia Centrum IoT ciąg w **iothubowner** okna.   

1. Do monitorowania danych otrzymywanych przez Centrum IoT należy używać **widok | Polecenie palety... | IoT: Uruchom komunikat D2C monitorowania** polecenia menu. 
2. Aby zatrzymać monitorowanie danych, należy użyć **widok | Polecenie palety... | IoT: Zatrzymaj komunikat D2C monitorowania** polecenia menu. 

## <a name="next-steps"></a>Następne kroki

W tym samouczku został utworzony funkcji platformy Azure, który zawiera kod, aby filtrować nieprzetworzone dane generowane przez urządzenia IoT krawędzi. Aby zachować Eksplorowanie usługi Azure IoT krawędzi, Dowiedz się, jak używać urządzenia IoT jako brama. 

> [!div class="nextstepaction"]
> [Tworzenie urządzenia bramy IoT](how-to-create-gateway-device.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
