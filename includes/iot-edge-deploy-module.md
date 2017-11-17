Jedną z kluczowych możliwości usługi Azure IoT krawędzi jest możliwość wdrażania modułów z urządzeniami IoT krawędzi z chmury. Moduł krawędzi IoT jest pakiet wykonywalnego zaimplementowane jako kontener. W tej sekcji możesz wdrożyć moduł, który generuje dane telemetryczne symulowane urządzenie. 

1. W portalu Azure przejdź do Centrum IoT.
1. Przejdź do **IoT krawędzi (wersja zapoznawcza)** i wybierz urządzenia IoT krawędzi.
1. Wybierz **ustawić modułów**.
1. Wybierz **Dodaj moduł krawędzi IoT**.
1. W **nazwa** wprowadź `tempSensor`. 
1. W **identyfikatora URI obrazu** wprowadź `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`. 
1. Pozostaw bez zmian innych ustawień, a następnie wybierz **zapisać**.

   ![Zapisz modułu krawędzi IoT po wprowadzeniu nazwy i identyfikatora URI obrazu](./media/iot-edge-deploy-module/name-image.png)

1. W **Dodaj moduły** krok, wybierz opcję **dalej**.
1. W **określić trasy** krok, wybierz opcję **dalej**.
1. W **szablon przeglądu** krok, wybierz opcję **przesyłania**.
1. Wróć do strony szczegółów urządzenia i wybierz **Odśwież**. Powinien zostać wyświetlony nowy moduł tempSensor systemem wzdłuż krawędzi IoT środowiska uruchomieniowego. 

   ![Widok tempSensor liście wdrożonej modułów][1]

<!-- Images -->
[1]: ../articles/iot-edge/media/tutorial-simulate-device-windows/view-module.png