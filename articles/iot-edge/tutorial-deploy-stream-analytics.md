---
title: "Wdrażanie usługi Azure Stream Analytics Azure IoT krawędzi | Dokumentacja firmy Microsoft"
description: "Wdrażanie usługi Azure Stream Analytics jako moduł do urządzenia"
services: iot-edge
keywords: 
author: msebolt
manager: timlt
ms.author: v-masebo
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 88dc0860b4455d19b05b4f4f1766210e3f3c704e
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Wdrażanie usługi Azure Stream Analytics jako moduł krawędzi IoT — w wersji preview

Urządzenia IoT może spowodować duże ilości danych. Czasami tych danych musi być analizowane albo przetworzone przed osiągnięciem chmury, aby zmniejszyć rozmiar dane przekazane lub wyeliminowanie obustronnych opóźnień możliwością szczegółowe informacje.

[Usługa Azure Stream Analytics] [ azure-stream] (ASA) zawiera Bogato strukturalnych składnię do analizy danych, zarówno w chmurze, jak i na krawędzi IoT urządzenia. Aby uzyskać więcej informacji na temat ASA na krawędzi IoT, zobacz [dokumentacji ASA](../stream-analytics/stream-analytics-edge.md).

Ten samouczek przeprowadzi Cię przez tworzenie zadania usługi analiza strumienia Azure i jej wdrożenia na urządzenie brzegowe IoT, aby można było przetworzyć strumienia lokalnego telemetrii bezpośrednio na urządzeniu i generowania alertów do natychmiastowego działania dysku na urządzeniu.  Istnieją dwa moduły związane z tego samouczka. Moduł czujnik temperatury symulowane (tempSensor), który generuje dane temperatury 20 do 120 stopni zwiększana o 1 co 5 sekund i modułu ASA, który odfiltrowuje temperatura przekracza 100 stopni. Moduł ASA również resetuje tempSensor po 100 średnią 30 sekund.

Omawiane kwestie:

> [!div class="checklist"]
> * Utwórz zadanie ASA do przetwarzania danych na krawędzi
> * Nowe zadanie ASA Uzyskuj dostęp do innych modułów krawędzi IoT
> * Wdrażanie do urządzenia IoT zadania ASA

## <a name="prerequisites"></a>Wymagania wstępne

* Centrum IoT 
* Urządzenia, który został utworzony i skonfigurowany w szybkiego startu lub wdrożyć Azure IoT krawędzi om symulowane urządzenie w [Windows] [ lnk-tutorial1-win] i [Linux] [ lnk-tutorial1-lin].
* Docker na urządzeniu IoT krawędzi
    * [Zainstaluj Docker w systemie Windows] [ lnk-docker-windows] i upewnij się, że jest uruchomiona.
    * [Zainstaluj Docker w systemie Linux] [ lnk-docker-linux] i upewnij się, że jest uruchomiona.
* Python 2.7.x na urządzeniu IoT krawędzi
    * [Instalowanie języka Python 2.7 w systemie Windows][lnk-python].
    * Większość dystrybucje systemu Linux, łącznie z Ubuntu, już Python 2.7 zainstalowane.  Użyj następującego polecenia, aby się upewnić, że zainstalowano narzędzia pip: `sudo apt-get install python-pip`.

> [!NOTE]
> Należy pamiętać, ciąg połączenia urządzenia, a identyfikator urządzenia IoT krawędzi będzie niezbędne do celów tego samouczka.

Krawędź IoT korzysta z wbudowanych modułów krawędzi IoT usługi Azure do szybkiego wdrożenia i Azure Stream Analytics (ASA) jest takich modułów. Można utworzyć zadania ASA z portalu, a następnie do Centrum IoT portalu, aby wdrożyć go jako moduł krawędzi IoT.  

Aby uzyskać więcej informacji dotyczących usługi Azure Stream Analytics, zobacz **omówienie** sekcji [dokumentacji analityka strumienia][azure-stream].

## <a name="create-an-asa-job"></a>Utwórz zadanie ASA

W tej sekcji utworzysz zadanie usługi analiza strumienia Azure pobierają dane z Centrum IoT zapytania na danych telemetrycznych wysłanych z urządzenia i przekazać wyniki do kontenera (obiektu BLOB magazynu Azure). Aby uzyskać więcej informacji, zobacz **omówienie** sekcji [dokumentacji analityka strumienia][azure-stream]. 

> [!NOTE]
> Konto usługi Azure Storage są wymagane do świadczenia punkt końcowy do użycia jako dane wyjściowe, w którym zadanie ASA. W poniższym przykładzie jest używany typ magazynu obiektów BLOB.  Aby uzyskać więcej informacji, zobacz **obiekty BLOB** sekcji [dokumentacji usługi Magazyn Azure][azure-storage].

1. W portalu Azure, przejdź do **magazynu -> Utwórz zasób**, kliknij przycisk **zobaczyć wszystkie**i kliknij przycisk **konta magazynu — obiekt blob, plików, tabeli, kolejki**.

2. Wprowadź nazwę konta magazynu, a następnie wybierz lokalizację, w którym przechowywana jest Centrum IoT. Kliknij przycisk **Utwórz**. Zanotuj nazwę na później.

    ![Nowe konto magazynu][1]

3. W portalu Azure przejdź do konta magazynu, który został właśnie utworzony. Kliknij przycisk **Przeglądaj obiekty BLOB** w obszarze **usługa Blob**. 
4. Utwórz nowy kontener dla modułu ASA do przechowywania danych. Ustaw poziom dostępu _kontenera_. Kliknij przycisk **OK**.

    ![Ustawienia magazynu][10]

5. W portalu Azure, przejdź do **Utwórz zasób** > **Internetu rzeczy** i wybierz **zadania usługi analiza strumienia**.

6. Wprowadź nazwę, wybierz **krawędzi** jako środowisko macierzyste i użyj pozostałe wartości domyślne.  Kliknij przycisk **Utwórz**.

    >[!NOTE]
    >Obecnie są ograniczonej liczby regionach, które obsługują ASA zadania na krawędzi IoT. Wybierz jedną z następujących jako lokalizację: środkowe stany USA, zachodnie centralnej nam, północno-środkowe stany, wschodnie stany USA, Kanada Wschodnia, Japonia Wschodnia, Wschodnia Azja, Europa Północna, Wielka Brytania Południowa, Australia Wschodnia, Brazylia Południowa. 

    ![Utwórz ASA][5]

2. Przejdź do utworzonego zadania w obszarze **topologii zadania**, wybierz pozycję **dane wejściowe**, kliknij przycisk **Dodaj**.

3. Wprowadź nazwę `temperature`, wybierz **strumienia danych** jako typu źródłowego i Użyj domyślnych ustawień dla innych parametrów. Kliknij przycisk **Utwórz**.

    ![Dane wejściowe ASA][2]

    > [!NOTE]
    > Dodatkowe dane wejściowe mogą obejmować krawędzi IoT określonych punktów końcowych.

4. W obszarze **topologii zadania**, wybierz pozycję **dane wyjściowe**, kliknij przycisk **Dodaj**.

5. Wprowadź nazwę `alert` i użyj ustawień domyślnych. Kliknij przycisk **Utwórz**.

    ![Dane wyjściowe ASA][3]

6. W obszarze **topologii zadania**, wybierz pozycję **zapytania**i wprowadź następujące:

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 100
    ```

## <a name="deploy-the-job"></a>Zadanie wdrażania

Teraz można przystąpić do wdrażania zadania ASA na urządzeniu IoT krawędzi.

1. W portalu Azure, w Centrum IoT, przejdź do **IoT krawędzi (wersja zapoznawcza)** , a następnie otwórz Twojej *{deviceId}*w bloku.

1. Wybierz **ustawić modułów**, a następnie wybierz pozycję **Import Azure usługi IoT krawędzi Module**.

1. Wybierz subskrypcję i zadania krawędzi ASA, utworzony. Następnie wybierz konta magazynu. Kliknij pozycję **Zapisz**.

    ![Moduł zestawu][6]

1. Kliknij przycisk **Dodawanie modułu krawędzi IoT** dodać moduł czujnik temperatury. Wprowadź _tempSensor_ dla nazwy, `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` dla adresu URL obrazu. Pozostaw bez zmian innych ustawień, a następnie kliknij przycisk **zapisać**.

    ![Moduł temperatury][11]

1. Skopiuj nazwę modułu ASA. Kliknij przycisk **dalej** można skonfigurować tras.

1. Skopiuj poniższe polecenie, aby **tras**.  Zastąp _{moduleName}_ z nazwą modułu skopiowany:

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

1. Wróć do strony szczegółów urządzenia, a następnie kliknij przycisk **Odśwież**.  Powinien zostać wyświetlony nowy _{moduleName}_ uruchomionego wraz z modułu **IoT krawędź agent** modułu i **Centrum IoT krawędzi**.

    ![dane wyjściowe z modułu][7]

## <a name="view-data"></a>Wyświetlanie danych

Teraz można przejść do Twojego urządzenia IoT krawędzi wyewidencjonować interakcji między modułem ASA i modułu tempSensor.

1. W wierszu polecenia należy skonfigurować parametry połączenia urządzenia IoT krawędzi środowiska uruchomieniowego:

    ```cmd/sh
    iotedgectl setup --connection-string "{device connection string}" --auto-cert-gen-force-no-passwords  
    ```

1. Uruchom polecenie, aby uruchomić środowisko uruchomieniowe:

    ```cmd/sh
    iotedgectl start  
    ```

1. Uruchom polecenie, aby wyświetlić modułów uruchomionych:

    ```cmd/sh
    docker ps  
    ```

    ![dane wyjściowe docker][8]

1. Uruchom polecenie, aby zobaczyć wszystkie dzienniki systemu i danych metryki. Użyj nazwy modułu z powyżej:

    ```cmd/sh
    docker logs -f {moduleName}  
    ```

    ![Dziennik docker][9]

1. W portalu Azure na koncie magazynu w obszarze **usługa Blob**, kliknij przycisk **Przeglądaj obiekty BLOB**, wybierz z kontenera i wybierz nowo utworzony plik JSON.

1. Kliknij przycisk **Pobierz** i wyświetlić wyniki.

## <a name="next-steps"></a>Następne kroki

W tym samouczku należy skonfigurować kontenera magazynu Azure i zadanie analizy przesyłania strumieniowego do analizowania danych z urządzenia IoT krawędzi.  Następnie ładowany niestandardowego modułu ASA do przeniesienia danych z urządzenia, za pomocą strumienia do obiektu BLOB do pobrania.  Można kontynuować do innych samouczków do dalszego Zobacz tworzenia rozwiązania dla biznesu Azure IoT krawędzi.

> [!div class="nextstepaction"] 
> [Wdróż model uczenia maszynowego Azure jako moduł][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[2]: ./media/tutorial-deploy-stream-analytics/asa_input.png
[3]: ./media/tutorial-deploy-stream-analytics/asa_output.png
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