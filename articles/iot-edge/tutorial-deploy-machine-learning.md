---
title: "Wdrażanie usługi Azure Machine Learning Azure IoT krawędzi | Dokumentacja firmy Microsoft"
description: "Wdrażanie usługi Azure Machine Learning jako moduł do urządzenia"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: e061e599f365bf3d343cb59b8dc6a61e06627517
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Wdrażanie usługi Azure Machine Learning jako moduł krawędzi IoT — w wersji preview

Moduły krawędzi IoT umożliwia wdrażanie kodu, który implementuje logiki biznesowej bezpośrednio do urządzenia IoT krawędzi. Ten samouczek przeprowadzi Cię przez moduł usługi Azure Machine Learning, który prognozuje, jeśli urządzenia nie powiodło się na podstawie danych czujnika na symulowane urządzenie brzegowe IoT utworzoną w programie IoT Edge Azure wdrożenia symulowanego urządzenia na wdrażanie [Windows] [ lnk-tutorial1-win] lub [Linux] [ lnk-tutorial1-lin] samouczki. Omawiane kwestie: 

> [!div class="checklist"]
> * Wdrażanie usługi Azure Machine Learning module do Twojego urządzenia IoT krawędzi
> * Widok wygenerowany danych

Jeśli chcesz użyć własnych [usługi Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/) modelu w rozwiązaniu będzie [wdrożenia modelu](https://aka.ms/aml-iot-edge-doc) krawędzi IoT i hosta w rejestrze kontenera, takich jak [rejestru kontenera Azure](../container-registry/index.yml) lub Docker.

## <a name="prerequisites"></a>Wymagania wstępne

* Urządzenie brzegowe IoT Azure utworzoną w pierwszym samouczku lub Szybki Start.
* Parametry połączenia Centrum IoT Centrum IoT, która łączy się z urządzenia IoT krawędzi.
* Kontener uczenie Maszynowe Azure

## <a name="create-the-azure-ml-container"></a>Tworzenie kontenera uczenie Maszynowe Azure
Aby utworzyć kontener z usługi Azure ML, postępuj zgodnie z instrukcjami [toolkit AI Azure IoT Edge](https://aka.ms/aml-iot-edge-anomaly-detection).

## <a name="run-the-solution"></a>Uruchom rozwiązania

1. Na [portalu Azure](https://portal.azure.com), przejdź do Centrum IoT.
1. Przejdź do **IoT krawędzi (wersja zapoznawcza)** i wybierz urządzenia IoT krawędzi.
1. Wybierz **ustawić modułów**.
1. Wybierz **modułu dodawanie krawędzi IoT**.
1. W **nazwa** wprowadź `tempSensor`.
1. W **identyfikatora URI obrazu** wprowadź `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
1. Pozostaw innych ustawień bez zmian, a następnie wybierz **zapisać**.
1. Na **Dodaj moduły** krok, wybierz opcję **modułu dodawanie krawędzi IoT** ponownie.
1. W **nazwa** wprowadź nazwę kontenera w poprzedniej sekcji. Zapoznaj się [toolkit AI Azure IoT Edge](https://aka.ms/aml-iot-edge-anomaly-detection) dla pomocy w znajdowaniu nazwę.
1. W **obrazu** wprowadź obrazu URI kontenera, w którym wprowadzono w poprzedniej sekcji. Zapoznaj się [toolkit AI Azure IoT Edge](https://aka.ms/aml-iot-edge-anomaly-detection) dla pomocy w znajdowaniu obrazu.
1. Kliknij pozycję **Zapisz**.
1. W **Dodaj moduły** kroku, kliknij przycisk **dalej**.
1. Aktualizowanie tras dla modułu:
1. W **Określ tras** kroku, skopiuj kod JSON poniżej w polu tekstowym. Moduły opublikować wszystkie komunikaty do środowiska wykonawczego krawędzi. Deklaracyjne reguły w środowisku uruchomieniowym definiują, do których przepływu wiadomości. W tym samouczku należy na dwa sposoby. Pierwszy trasy transportu wiadomości z czujnika temperatury modułu nauczania komputera za pośrednictwem punktu końcowego "amlInput", która jest punkt końcowy, który Użyj wszystkich modułów usługi Azure Machine Learning. Drugi trasy transportu wiadomości z modułu nauczania maszyny do Centrum IoT. W tej trasy "amlOutput" jest punktu końcowego, który umożliwia dane wyjściowe wszystkich modułów uczenia maszynowego Azure i '' powyżej$ "to specjalne lokalizacji docelowej, która informuje krawędzi koncentratora do wysyłania komunikatów do Centrum IoT. 

    ```json
    {
        "routes": {
            "sensorToMachineLearning":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/machinelearningmodule/inputs/amlInput\")",
            "machineLearningToIoTHub": "FROM /messages/modules/machinelearningmodule/outputs/amlOutput INTO $upstream"
        }
    }
    ``` 

1. Kliknij przycisk **Dalej**. 
1. W kroku "Szablon przeglądu" kliknij przycisk "Zatwierdź". 
1. Wróć do strony szczegółów urządzenia, a następnie kliknij przycisk "Odśwież".  Powinny pojawić się nowe '' machinelearningmodule'' uruchomiona wraz z modułu"tempSensor" i "runtime krawędzi IoT".

## <a name="view-generated-data"></a>Widok wygenerowany danych

 W kodzie VS **widok | Polecenie palety... | IoT: Monitorowanie komunikatów D2C uruchomienia** polecenia menu do monitorowania danych otrzymywanych w Centrum IoT. 

## <a name="next-steps"></a>Następne kroki

W tym samouczku wdrożono modułu krawędzi IoT obsługiwane przez usługę uczenie maszynowe Azure. Można kontynuować do wszystkich innych samouczków, aby uzyskać informacje o innych metod, które ułatwiają krawędź IoT Azure możesz przekształcić w danych biznesowych na krawędzi.

> [!div class="nextstepaction"]
> [Wdrażanie funkcji platformy Azure jako moduł](tutorial-deploy-function.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
