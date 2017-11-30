---
title: "Wdrażanie usługi Azure Stream Analytics Azure IoT krawędzi | Dokumentacja firmy Microsoft"
description: "Wdrażanie usługi Azure Stream Analytics jako moduł do urządzenia"
services: iot-edge
keywords: 
author: msebolt
manager: timlt
ms.author: v-masebo
ms.date: 11/28/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 5a143bbf7abb5304ac51782d517c02ec184a05a2
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Wdrażanie usługi Azure Stream Analytics jako moduł krawędzi IoT — w wersji preview

Urządzenia IoT może spowodować duże ilości danych. Czasami tych danych musi być analizowane albo przetworzone przed osiągnięciem chmury, aby zmniejszyć rozmiar dane przekazane lub wyeliminowanie obustronnych opóźnień możliwością szczegółowe informacje.

Krawędź IoT korzysta z wbudowanych modułów krawędzi IoT usługi Azure do szybkiego wdrożenia i [Azure Stream Analytics] [ azure-stream] (ASA) jest takich modułów. Można utworzyć zadania ASA z portalu, a następnie do Centrum IoT portalu, aby wdrożyć go jako moduł krawędzi IoT.  

Usługa Azure Stream Analytics zawiera Bogato strukturalnych składnię analizy danych, zarówno w chmurze, jak i na krawędzi IoT urządzeń. Aby uzyskać więcej informacji na temat ASA na krawędzi IoT, zobacz [dokumentacji ASA](../stream-analytics/stream-analytics-edge.md).

Ten samouczek przeprowadzi Cię przez tworzenie zadania usługi analiza strumienia Azure i jej wdrożenia na urządzenie brzegowe IoT, aby można było przetworzyć strumienia lokalnego telemetrii bezpośrednio na urządzeniu i generowania alertów do natychmiastowego działania dysku na urządzeniu.  Istnieją dwa moduły związane z tego samouczka. Czujnik temperatury symulowane modułu (tempSensor) generuje danych temperatury 20 do 120 stopni, zwiększana o 1 co 5 sekund. Moduł usługi Stream Analytics resetuje tempSensor gdy średnią 30 sekund osiągnie 70. W środowisku produkcyjnym ta funkcja może służyć do wyłącza maszynę lub podjęcia środków zapobiegawczych gdy temperatura osiągnie niebezpieczne poziomy. 

Omawiane kwestie:

> [!div class="checklist"]
> * Utwórz zadanie ASA do przetwarzania danych na krawędzi
> * Nowe zadanie ASA Uzyskuj dostęp do innych modułów krawędzi IoT
> * Wdrażanie do urządzenia IoT zadania ASA

## <a name="prerequisites"></a>Wymagania wstępne

* Centrum IoT 
* Urządzenia, który został utworzony i skonfigurowany w szybkiego startu lub wdrożyć Azure IoT krawędzi om symulowane urządzenie w [Windows] [ lnk-tutorial1-win] i [Linux] [ lnk-tutorial1-lin]. Należy znać klucza połączenia urządzenia i identyfikator urządzenia. 
* Docker uruchomione na urządzeniu IoT krawędzi
    * [Zainstaluj Docker w systemie Windows][lnk-docker-windows]
    * [Zainstaluj Docker w systemie Linux][lnk-docker-linux]
* Python 2.7.x na urządzeniu IoT krawędzi
    * [Instalowanie języka Python 2.7 w systemie Windows][lnk-python].
    * Większość dystrybucje systemu Linux, łącznie z Ubuntu, już Python 2.7 zainstalowane.  Użyj następującego polecenia, aby się upewnić, że zainstalowano narzędzia pip: `sudo apt-get install python-pip`.


## <a name="create-an-asa-job"></a>Utwórz zadanie ASA

W tej sekcji utworzysz zadanie usługi analiza strumienia Azure pobierają dane z Centrum IoT zapytania na danych telemetrycznych wysłanych z urządzenia i przekazać wyniki do kontenera (obiektu BLOB magazynu Azure). Aby uzyskać więcej informacji, zobacz **omówienie** sekcji [dokumentacji analityka strumienia][azure-stream]. 

### <a name="create-a-storage-account"></a>Tworzenie konta magazynu

Konto usługi Azure Storage są wymagane do świadczenia punkt końcowy do użycia jako dane wyjściowe, w którym zadanie ASA. W poniższym przykładzie jest używany typ magazynu obiektów BLOB.  Aby uzyskać więcej informacji, zobacz **obiekty BLOB** sekcji [dokumentacji usługi Magazyn Azure][azure-storage].

1. W portalu Azure, przejdź do **Utwórz zasób** , a następnie wprowadź `Storage account` na pasku wyszukiwania. Wybierz **konta magazynu — obiekt blob, plików, tabeli, kolejki**.

2. Wprowadź nazwę konta magazynu, a następnie wybierz lokalizację, w którym znajduje się Centrum IoT. Kliknij przycisk **Utwórz**. Zapamiętaj nazwę później.

    ![Nowe konto magazynu][1]

3. Przejdź do konta magazynu, który został właśnie utworzony. Kliknij przycisk **Przeglądaj obiekty BLOB**. 
4. Utwórz nowy kontener dla modułu ASA do przechowywania danych. Ustaw poziom dostępu **kontenera**. Kliknij przycisk **OK**.

    ![Ustawienia magazynu][10]

### <a name="create-a-stream-analytics-job"></a>Tworzenie zadania usługi Stream Analytics

1. W portalu Azure, przejdź do **Utwórz zasób** > **Internetu rzeczy** i wybierz **zadania usługi analiza strumienia**.

2. Wprowadź nazwę, wybierz **krawędzi** jako środowisko macierzyste i użyj pozostałe wartości domyślne.  Kliknij przycisk **Utwórz**.

    >[!NOTE]
    >Obecnie ASA zadania na krawędzi IoT nie są obsługiwane w regionu zachodnie stany USA 2. 

3. Przejdź do utworzonego zadania. Wybierz **dane wejściowe** kliknięcie **Dodaj**.

4. Alias wejściowy wprowadź `temperature`, Ustaw typ źródła **strumienia danych**i użyj ustawień domyślnych dla innych parametrów. Kliknij przycisk **Utwórz**.

   ![Dane wejściowe ASA](./media/tutorial-deploy-stream-analytics/asa_input.png)

5. Wybierz **dane wyjściowe** kliknięcie **Dodaj**.

6. Alias wyjściowy wprowadź `alert`i użyj ustawień domyślnych dla innych parametrów. Kliknij przycisk **Utwórz**.

   ![Dane wyjściowe ASA](./media/tutorial-deploy-stream-analytics/asa_output.png)


7. Wybierz **zapytania**.
8. Zastąp tekst domyślny następujące zapytanie:

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 70
    ```
9. Kliknij pozycję **Zapisz**.

## <a name="deploy-the-job"></a>Zadanie wdrażania

Teraz można przystąpić do wdrażania zadania ASA na urządzeniu IoT krawędzi.

1. W portalu Azure, w Centrum IoT, przejdź do **IoT krawędzi (wersja zapoznawcza)** i otwórz jej stronę szczegółów urządzenia IoT krawędzi.
1. Wybierz **ustawić modułów**.
1. Jeśli wdrożono modułu tempSensor na tym urządzeniu może automatycznego wypełniania. Jeśli nie, aby dodać ten moduł, wykonaj następujące kroki:
   1. Kliknij przycisk **Dodaj moduł krawędzi IoT**
   1. Wprowadź `tempSensor` z nazwą i `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` dla identyfikatora URI obrazu. 
   1. Pozostaw bez zmian innych ustawień, a następnie kliknij przycisk **zapisać**.
1. Aby dodać zadanie krawędzi ASA, zaznacz **Import Azure Stream Analytics IoT krawędzi Module**.
1. Wybierz subskrypcję i zadania krawędzi ASA, utworzony. 
1. Wybierz subskrypcję i utworzone konto magazynu. Kliknij pozycję **Zapisz**.

    ![Moduł zestawu][6]

1. Skopiuj nazwę, która została wygenerowana automatycznie dla modułu ASA. 

    ![Moduł temperatury][11]

1. Kliknij przycisk **dalej** można skonfigurować tras.
1. Skopiuj poniższe polecenie, aby **tras**.  Zastąp _{moduleName}_ ze skopiowanego Nazwa modułu:

    ```json
    {
        "routes": {                                                               
          "telemetryToCloud": "FROM /messages/modules/tempSensor/* INTO $upstream", 
          "alertsToCloud": "FROM /messages/modules/{moduleName}/* INTO $upstream", 
          "alertsToReset": "FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")", 
          "telemetryToAsa": "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/{moduleName}/inputs/temperature\")" 
        }
    }
    ```

1. Kliknij przycisk **Dalej**.

1. W **szablon przeglądu** kroku, kliknij przycisk **przesyłania**.

1. Wróć do strony szczegółów urządzenia, a następnie kliknij przycisk **Odśwież**.  Powinien zostać wyświetlony nowy moduł Stream Analytics uruchomiona wraz z **IoT krawędź agent** modułu i **Centrum IoT krawędzi**.

    ![dane wyjściowe z modułu][7]

## <a name="view-data"></a>Wyświetlanie danych

Teraz można przejść do Twojego urządzenia IoT krawędzi wyewidencjonować interakcji między modułem ASA i modułu tempSensor.

Sprawdź, czy w rozwiązaniu Docker są uruchomione wszystkie moduły:

   ```cmd/sh
   docker ps  
   ```

   ![dane wyjściowe docker][8]

Zobacz wszystkie dane dzienników i metryki systemu. Użyj nazwy modułu usługi Stream Analytics:

   ```cmd/sh
   docker logs -f {moduleName}  
   ```

Należy się patrzeć na komputerze temperatury stopniowo rosną, dopóki nie osiągnie 70 stopni 30 sekund. Następnie moduł Stream Analytics wyzwala Resetowanie i temperatury maszyny spadnie wstecz do 21. 

   ![Dziennik docker][9]


## <a name="next-steps"></a>Następne kroki

W tym samouczku należy skonfigurować kontenera magazynu Azure i zadanie analizy przesyłania strumieniowego do analizowania danych z urządzenia IoT krawędzi.  Następnie ładowany niestandardowego modułu ASA do przeniesienia danych z urządzenia, za pomocą strumienia do obiektu BLOB do pobrania.  Można kontynuować do innych samouczków do dalszego Zobacz tworzenia rozwiązania dla biznesu Azure IoT krawędzi.

> [!div class="nextstepaction"] 
> [Wdróż model uczenia maszynowego Azure jako moduł][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[4]: ./media/tutorial-deploy-stream-analytics/add_device.png
[5]: ./media/tutorial-deploy-stream-analytics/asa_job.png
[6]: ./media/tutorial-deploy-stream-analytics/set_module.png
[7]: ./media/tutorial-deploy-stream-analytics/module_output.png
[8]: ./media/tutorial-deploy-stream-analytics/docker_output.png
[9]: ./media/tutorial-deploy-stream-analytics/docker_log.png
[10]: ./media/tutorial-deploy-stream-analytics/storage_settings.png
[11]: ./media/tutorial-deploy-stream-analytics/temp_module.png


<!-- Links -->
[lnk-what-is-iot-edge]: what-is-iot-edge.md
[lnk-module-dev]: module-development.md
[iot-hub-get-started-create-hub]: ../../includes/iot-hub-get-started-create-hub.md
[azure-iot]: https://docs.microsoft.com/en-us/azure/iot-hub/
[azure-storage]: https://docs.microsoft.com/en-us/azure/storage/
[azure-stream]: https://docs.microsoft.com/en-us/azure/stream-analytics/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

[lnk-docker-windows]: https://docs.docker.com/docker-for-windows/install/ 
[lnk-docker-linux]: https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/
[lnk-python]: https://www.python.org/downloads/