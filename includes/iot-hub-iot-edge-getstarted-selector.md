> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-get-started.md)
> 
> 

Ten artykuł zawiera szczegółowy przewodnik dotyczący [przykładowego kodu Hello world][lnk-helloworld-sample] w celu zilustrowania podstawowych składników architektury usługi [Azure IoT Edge][lnk-iot-edge]. W przykładzie użyto usługi Azure IoT Edge do utworzenia prostej bramy, która co pięć sekund rejestruje w pliku komunikat „hello world”.

Przewodnik składa się z następujących elementów:

* **Hello World przykładowa architektura**: w tym artykule opisano sposób [pojęcia architektury Azure IoT krawędzi] [ lnk-edge-concepts] dotyczą przykładowe Hello World i sposób składniki dopasowania.
* **Jak utworzyć przykład**: czynności wymagane do utworzenia przykładu.
* **Jak uruchomić przykład**: czynności wymagane do uruchomienia przykładu. 
* **Typowe dane wyjściowe**: przykładowe dane wyjściowe, których należy spodziewać się po uruchomieniu przykładu.
* **Wstawki kodu**: kolekcja fragmentów kodu, aby pokazać, jak próbki Hello World implementuje najważniejsze składniki bramy IoT krawędzi.


## <a name="hello-world-sample-architecture"></a>Przykładowa architektura Witaj, świecie
Przykład Witaj, świecie ilustruje pojęcia opisane w poprzedniej sekcji. Przykład Witaj świecie implementuje bramy IoT krawędzi, która ma potoku składają się z dwóch modułów krawędzi IoT:

* Moduł *witaj, świecie* co pięć sekund tworzy komunikat i przekazuje go do modułu rejestratora.
* Moduł *rejestratora* zapisuje odebrane komunikaty w pliku.

![Architektura przykładowej aplikacji Hello world utworzonej przy użyciu usługi Azure IoT Edge][4]

Zgodnie z opisem w poprzedniej sekcji, moduł Witaj, świecie nie przekazuje komunikatów co pięć sekund bezpośrednio do modułu rejestratora. Zamiast tego publikuje komunikaty do brokera co pięć sekund.

Moduł rejestratora odbiera komunikaty od broker i podejmuje działania dotyczące tych komunikatów, zapisując treść komunikatu do pliku.

Moduł rejestratora używa jedynie komunikatów z brokera, natomiast nigdy nie publikuje do brokera nowych komunikatów.

![Kierowanie komunikatów między modułami przez brokera w usłudze Azure IoT Edge][5]

Powyższej ilustracji przedstawiono architekturę próbki Hello World i względnych ścieżek do plików źródłowych, które implementują różnych części próbki w [repozytorium][lnk-iot-edge]. Eksploruj kod na własną, lub użyj wstawki kodu w tym artykule jako przewodnika.

<!-- Images -->
[4]: media/iot-hub-iot-edge-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-iot-edge-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/iot-edge/tree/master/samples/hello_world
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-edge-concepts]: ../articles/iot-hub/iot-hub-iot-edge-overview.md