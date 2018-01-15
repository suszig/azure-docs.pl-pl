---
title: "Wdrażanie usługi Azure Machine Learning Azure IoT krawędzi | Dokumentacja firmy Microsoft"
description: "Wdrażanie usługi Azure Machine Learning jako moduł do urządzenia"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 12/13/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: a0131fdbbf926d59eae06089cde109649a1433b8
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/12/2018
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Wdrażanie usługi Azure Machine Learning jako moduł krawędzi IoT — w wersji preview

Moduły krawędzi IoT umożliwia wdrażanie kodu, który implementuje logiki biznesowej bezpośrednio do urządzenia IoT krawędzi. Ten samouczek przedstawia wdrażanie usługi Azure Machine Learning moduł, który prognozuje, jeśli urządzenia nie powiodło się na podstawie danych czujnika na symulowane urządzenie brzegowe IoT utworzony w [krawędzi IoT Azure wdrożenia symulowanego urządzenia w systemie Windows] [ lnk-tutorial1-win] lub [Linux] [ lnk-tutorial1-lin] samouczki. 

Ten samouczek zawiera informacje na temat wykonywania następujących czynności: 

> [!div class="checklist"]
> * Utwórz moduł usługi Azure Machine Learning
> * Wypychanie kontener modułu do rejestru kontenera platformy Azure
> * Wdrażanie usługi Azure Machine Learning module do Twojego urządzenia IoT krawędzi
> * Widok wygenerowany danych

Moduł usługi Azure Machine Learning, który można utworzyć w tym samouczku odczytuje dane temperatury generowany przez urządzenie i tylko wysyła komunikaty od początku do Centrum IoT Azure, gdy jej prognozuje awarii (nazywane anomalii). 


## <a name="prerequisites"></a>Wymagania wstępne

* Urządzenie brzegowe IoT Azure utworzoną w pierwszym samouczku lub Szybki Start.
* Parametry połączenia Centrum IoT Centrum IoT, która łączy się z urządzenia IoT krawędzi.
* Konto usługi Azure Machine Learning. Aby utworzyć konto, postępuj zgodnie z instrukcjami [Tworzenie usługi Azure Machine Learning kont i zainstaluj usługi Azure Machine Learning Workbench](../machine-learning/preview/quickstart-installation.md#create-azure-machine-learning-accounts). Nie trzeba zainstalować aplikację workbench w tym samouczku. 
* Moduł zarządzania dla usługi Azure ML na tym komputerze. Aby skonfigurować środowisko i utworzyć konto, postępuj zgodnie z instrukcjami [konfiguracji zarządzania modelu](https://docs.microsoft.com/azure/machine-learning/preview/deployment-setup-configuration).

## <a name="create-the-azure-ml-container"></a>Tworzenie kontenera uczenie Maszynowe Azure
W tej sekcji pobierania plików uczonego modelu i przekonwertować je na kontenera usługi uczenie Maszynowe Azure.  

Na komputerze z programem modułu zarządzania dla usługi Azure ML, Pobierz i Zapisz [iot_score.py](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/iot_score.py) i [model.pkl](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/model.pkl) z zestawu narzędzi IoT uczenie Maszynowe Azure w serwisie GitHub. Te pliki zdefiniuj przeszkolone maszyny, uczenie modelu, które zostaną wdrożone do urządzenia Iot krawędzi. 

Umożliwia utworzenie kontenera, w którym można wdrożyć do urządzenia IoT brzegowe trenowanego modelu.

```cmd
az ml service create realtime --model-file model.pkl -f iot_score.py -n machinelearningmodule -r python
```
Nazwa usługi *machinelearningmodule* w tym przykładzie staje się nazwa obrazu kontenera docker.

### <a name="view-the-container-repository"></a>Widok repozytorium kontenera

Sprawdź, czy obraz kontenera został pomyślnie utworzony i przechowywane w repozytorium kontenera platformy Azure, który jest skojarzony ze środowiskiem machine learning.

1. Na [portalu Azure](https://portal.azure.com), przejdź do **wszystkie usługi** i wybierz **rejestrów kontenera**.
2. Wybierz rejestru. Nazwa powinna zaczynać się znakiem **mlcr** należącej do grupy zasobów, lokalizacji i subskrypcji, który został użyty do konfiguracji modułu zarządzania.
3. Wybierz **klawisze dostępu**
4. Kopiuj **logowania serwera**, **Username**, i **hasło**.  Należy je do dostępu do rejestru z urządzeń krawędzi.
5. Wybierz **repozytoria**
6. Wybierz **machinelearningmodule**
7. Obecnie masz pełny obraz ścieżka kontenera. Zanotuj tę ścieżkę obrazu w następnej sekcji. Powinien wyglądać następująco: **.azureacr.io/machinelearningmodule:1 < registry_name >**

## <a name="add-registry-credentials-to-your-edge-device"></a>Dodaj rejestru poświadczenia na urządzeniu krawędzi

Dodaj poświadczenia do rejestru do środowiska wykonawczego Edge na komputerze, na którym są uruchomione Twoje urządzenie brzegowe. To polecenie umożliwia dostęp środowiska uruchomieniowego do ściągnięcia kontenera.

Linux:
   ```cmd
   sudo iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password> 
   ```

W systemie Windows:
   ```cmd
   iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password> 
   ```

## <a name="run-the-solution"></a>Uruchom rozwiązania

1. Na [portalu Azure](https://portal.azure.com), przejdź do Centrum IoT.
1. Przejdź do **IoT krawędzi (wersja zapoznawcza)** i wybierz urządzenia IoT krawędzi.
1. Wybierz **ustawić modułów**.
1. Jeśli moduł tempSensor już wcześniej wdrożony na urządzeniu IoT krawędzi, może automatycznego wypełniania. Jeśli nie jest ona już na liście modułów, należy go dodać.
    1. Wybierz **Dodaj moduł krawędzi IoT**.
    2. W **nazwa** wprowadź `tempSensor`.
    3. W **identyfikatora URI obrazu** wprowadź `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Wybierz pozycję **Zapisz**.
1. Dodaj moduł, który został utworzony uczenia maszynowego.
    1. Wybierz **Dodaj moduł krawędzi IoT**.
    1. W **nazwa** wprowadź`machinelearningmodule`
    1. W **obrazu** wprowadź swój adres obrazu, na przykład `<registry_name>.azurecr.io/machinelearningmodule:1`.
    1. Wybierz pozycję **Zapisz**.
1. W **Dodaj moduły** krok, wybierz opcję **dalej**.
1. W **Określ tras** kroku, skopiuj kod JSON poniżej w polu tekstowym. Pierwszy trasy transportu wiadomości z czujnika temperatury modułu nauczania komputera za pośrednictwem punktu końcowego "amlInput", która jest punkt końcowy, który Użyj wszystkich modułów usługi Azure Machine Learning. Drugi trasy transportu wiadomości z modułu nauczania maszyny do Centrum IoT. W tej trasy punktu końcowego, który umożliwia dane wyjściowe wszystkich modułów usługi Azure Machine Learning jest "amlOutput" i "powyżej$" oznacza Centrum IoT. 

    ```json
    {
        "routes": {
            "sensorToMachineLearning":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/machinelearningmodule/inputs/amlInput\")",
            "machineLearningToIoTHub": "FROM /messages/modules/machinelearningmodule/outputs/amlOutput INTO $upstream"
        }
    }
    ``` 

1. Wybierz **dalej**. 
1. W **szablon przeglądu** krok, wybierz opcję **przesyłania**. 
1. Wróć do strony szczegółów urządzenia i wybierz **Odśwież**.  Powinien zostać wyświetlony nowy **machinelearningmodule** uruchomiona wraz z **tempSensor** modułu i modułów środowiska uruchomieniowego IoT krawędzi.

## <a name="view-generated-data"></a>Widok wygenerowany danych

Można wyświetlić komunikaty urządzenia do chmury, które urządzenia IoT krawędzi wysyła przy użyciu rozszerzenia Azure IoT Toolkit dla programu Visual Studio Code. 

1. W programie Visual Studio Code, wybierz **urządzenia IoT Hub**. 
2. Wybierz **...**  następnie wybierz **ustawić parametry połączenia Centrum IoT** z menu. 

   ![Urządzenia IoT Hub więcej menu](./media/tutorial-deploy-machine-learning/set-connection.png)

3. W polu tekstowym, który zostanie otwarty w górnej części strony wprowadź ciąg połączenia iothubowner Centrum IoT. Urządzenie brzegowe IoT powinien znajdują się na liście urządzeń Centrum IoT.
4. Wybierz **...**  ponownie wybierz **rozpocząć monitorowanie komunikat D2C**.
5. Sprawdź komunikaty pochodzące z tempSensor co pięć sekund, które machinelearningmodule dołącza jego ocenę kondycji urządzenia. 

   ![Azure ML odpowiedzi w treści wiadomości](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku wdrożono modułu krawędzi IoT obsługiwane przez usługę uczenie maszynowe Azure. Można kontynuować do wszystkich innych samouczków, aby uzyskać informacje o innych metod, które ułatwiają krawędź IoT Azure możesz przekształcić w danych biznesowych na krawędzi.

> [!div class="nextstepaction"]
> [Wdrażanie funkcji platformy Azure jako moduł](tutorial-deploy-function.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
