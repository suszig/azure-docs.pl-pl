---
title: "Wiele tras z usług na podstawie lokalizacji platformy Azure | Dokumentacja firmy Microsoft"
description: "Znajdź trasy dla różnych trybach podróży przy użyciu usługi na podstawie lokalizacji platformy Azure"
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: tutorial
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 7d8eb900bdc90a391d4121b7bfb863fc274fc564
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-location-based-services"></a>Znajdź trasy dla różnych trybach podróży przy użyciu usługi na podstawie lokalizacji platformy Azure

Ten samouczek przedstawia sposób użycia konta usługi na podstawie lokalizacji platformy Azure i zestawu SDK usług trasy, można znaleźć trasy do punktu odsetek priorytety przez tryb podróży. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Skonfiguruj zapytanie usługi trasy
> * Renderowanie trasy nadać priorytet w trybie podróży

## <a name="prerequisites"></a>Wymagania wstępne

Przed kontynuowaniem upewnij się, że [tworzenia konta usługi na podstawie lokalizacji Azure](./tutorial-search-location.md#createaccount), i [Pobierz klucz subskrypcji dla Twojego konta](./tutorial-search-location.md#getkey). Można również zauważyć, jak używać formantu mapy i interfejsy API wyszukiwania usługi zgodnie z opisem w samouczku [wyszukiwania w pobliżu z interesujących przy użyciu usługi na podstawie lokalizacji Azure](./tutorial-search-location.md), jak również informacje podstawowe sposoby użycia interfejsów API usługi trasy jako omówione w samouczku [trasy do punktu przydatne przy użyciu usługi na podstawie lokalizacji Azure](./tutorial-route-location.md).


<a id="queryroutes"></a>

## <a name="configure-your-route-service-query"></a>Skonfiguruj zapytanie usługi trasy

Poniższe kroki umożliwiają utworzenie statycznej strony HTML osadzonych z interfejsem API sterowania mapy lokalizacji na podstawie usług. 

1. Na komputerze lokalnym, Utwórz nowy plik i nadaj jej nazwę **MapTruckRoute.html**. 
2. Dodaj następujące składniki HTML do pliku:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Truck Route</title>
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #map {
                width: 100%;
                height: 100%;
            }
        </style>
    </head>
    
    <body>
        <div id="map"></div>
        <script>
            // Embed Map Control JavaScript code here
        </script>
    </body>

    </html>
    ```
    Należy pamiętać, że nagłówek HTML osadza lokalizacje zasobów pliki CSS i JavaScript dla biblioteki usług na podstawie lokalizacji platformy Azure. Spójrz również *skryptu* segmentu dodany do treści HTML, zawiera wbudowany kod JavaScript można uzyskać dostępu do interfejsu API Azure mapy formantu.
3. Dodaj następujący kod JavaScript do *skryptu* bloku pliku HTML. Zastąp symbol zastępczy *< klawisza insert >* z konta usługi na podstawie lokalizacji klucza podstawowego.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var subscriptionKey = "<insert-key>";
    var map = new atlas.Map("map", {
        "subscription-key": subscriptionKey
    });
    ```
    **Atlas. Mapa** zapewnia formantu mapy visual i interaktywne sieci web i wchodzi w skład interfejsu API Azure mapy formantu.

4. Dodaj następujący kod JavaScript do *skryptu* bloku, aby dodać wyświetlania przepływu ruchu do mapy:

    ```JavaScript
    // Add Traffic Flow to the Map
    map.setTraffic({
        flow: "relative"
    });
    ```
    Ten kod ustawia przepływ ruchu `relative`, czyli szybkości drogowej względem zwolnienia przepływu. Możesz również ustawić ją `absolute` szybkości podróży, lub `relative-delay` wyświetlające względną prędkość gdy różni się od zwolnienia przepływu. 

5. Dodaj następujący kod JavaScript, aby utworzyć kody PIN dla punktu początkowego i końcowego trasy:

    ```JavaScript
    // Create the GeoJSON objects which represent the start and end point of the route
    var startPoint = new atlas.data.Point([-122.356099, 47.580045]);
    var startPin = new atlas.data.Feature(startPoint, {
        title: "Fabrikam, Inc.",
        icon: "pin-round-blue"
    });

    var destinationPoint = new atlas.data.Point([-122.130137, 47.644702]);
    var destinationPin = new atlas.data.Feature(destinationPoint, {
        title: "Microsoft",
        icon: "pin-blue"
    });
    ```
    Ten kod tworzy dwa [obiektów GeoJSON](https://en.wikipedia.org/wiki/GeoJSON) do reprezentowania punktu początkowego i końcowego trasy. 

6. Dodaj następujący kod JavaScript, aby dodać warstw *linestrings* do formantu mapy, aby wyświetlić tras na podstawie trybu transportu, na przykład _samochodu_ i _ciężarówka_.

    ```JavaScript
    // Place route layers on the map
    var carRouteLayerName = "car-route";
    map.addLinestrings([], {
        name: carRouteLayerName,
        color: "#B76DAB",
        width: 5,
        cap: "round",
        join: "round",
        before: "labels"
    });

    var truckRouteLayerName = "truck-route";
    map.addLinestrings([], {
        name: truckRouteLayerName,
        color: "#2272B9",
        width: 9,
        cap: "round",
        join: "round",
        before: carRouteLayerName
    });
    ```

7. Dodaj następujący kod JavaScript, aby dodać punkt początkowy i końcowy do mapy:

    ```JavaScript
    // Fit the map window to the bounding box defined by the start and destination points
    var swLon = Math.min(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var swLat = Math.min(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    var neLon = Math.max(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var neLat = Math.max(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    map.setCameraBounds({
        bounds: [swLon, swLat, neLon, neLat],
        padding: 100
    });

    // Add pins to the map for the start and end point of the route
    map.addPins([startPin, destinationPin], {
        name: "route-pins",
        textFont: "SegoeUi-Regular",
        textOffset: [0, -20]
    });
    ``` 
    Interfejs API **map.setCameraBounds** dostosowuje okna mapy zgodnie z współrzędne punktu początkowego i końcowego. Interfejs API **map.addPins** dodaje punkty do formantu mapy jako składniki wizualne.

8. Zapisz **MapTruckRoute.html** plików na tym komputerze. 

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Renderowanie trasy nadać priorytet w trybie podróży

W tej sekcji przedstawia sposób użycia interfejsu API usługi trasy Azure lokalizacji na podstawie usług można znaleźć wiele tras z danym start wskaż lokalizację docelową, na podstawie Twojej trybu transportu. Usługa trasa udostępnia interfejsy API do zaplanowania najszybciej, jak najkrótszym lub oznakowania trasy między dwiema lokalizacjami, biorąc pod uwagę warunki ruchu w czasie rzeczywistym. Umożliwia także użytkownikom planowanie trasy w przyszłości przy użyciu bazy danych szeroką gamę ruchu historycznych platformy Azure i prognozowania okresów trasy dla każdego dnia i godziny. 

1. Otwórz **MapTruckRoute.html** plik utworzony w poprzedniej sekcji i Dodaj następujący kod JavaScript, aby *skryptu* bloku, można uzyskać trasy ciężarówka, przy użyciu usługi trasy.

    ```JavaScript
    // Perform a request to the route service and draw the resulting truck route on the map
    var xhttpTruck = new XMLHttpRequest();
    xhttpTruck.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(this.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], {
                name: truckRouteLayerName
            });
        }
    };

    var truckRouteUrl = "https://atlas.microsoft.com/route/directions/json?";
    truckRouteUrl += "&api-version=1.0";
    truckRouteUrl += "&subscription-key=" + subscriptionKey;
    truckRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];
    truckRouteUrl += "&travelMode=truck";
    truckRouteUrl += "&vehicleWidth=2";
    truckRouteUrl += "&vehicleHeight=2";
    truckRouteUrl += "&vehicleLength=5";
    truckRouteUrl += "&vehicleLoadType=USHazmatClass2";

    xhttpTruck.open("GET", truckRouteUrl, true);
    xhttpTruck.send();
    ```
    Następujący fragment kodu tworzy [XMLHttpRequest](https://xhr.spec.whatwg.org/), i dodanie obsługi zdarzeń mógł przeanalizować odpowiedzi przychodzących. Dla pomyślnej odpowiedzi, jego tworzy tablicę współrzędne trasy zwrócił i dodaje go mapy `truckRouteLayerName` warstwy. 
    
    Następujący fragment kodu wysyła również zapytania do usługi trasy uzyskać trasy dla określonego początkowy i punkt końcowy dla Twojego konta subskrypcji klucza. Następujące parametry opcjonalne służą do wskazywania trasę dla duże ciężarówka:-parametr `travelMode=truck` Określa tryb podróży jako *ciężarówka*. Inne tryby podróży obsługiwane są *taksówki*, *magistrali*, *van*, *motocykla*, wartością domyślną *samochodu* . 
        Parametry `vehicleWidth`, `vehicleHeight`, i `vehicleLength` Określ wymiary mechanizm metry i uwzględniane są tylko jeśli jest tryb podróży *ciężarówka*. 
        - `vehicleLoadType` Klasyfikuje ładunku jako niebezpieczne i ograniczeniami na niektóre drogi. To jest również obecnie uważane za tylko w przypadku *ciężarówka* tryb. 

2. Dodaj następujący kod JavaScript można uzyskać trasy samochodu przy użyciu usługi trasy:

    ```JavaScript
    // Perform a request to the route service and draw the resulting car route on the map
    var xhttpCar = new XMLHttpRequest();
    xhttpCar.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(this.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], {
                name: carRouteLayerName
            });
        }
    };

    var carRouteUrl = "https://atlas.microsoft.com/route/directions/json?";
    carRouteUrl += "&api-version=1.0";
    carRouteUrl += "&subscription-key=" + subscriptionKey;
    carRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttpCar.open("GET", carRouteUrl, true);
    xhttpCar.send();
    ```
    Następujący fragment kodu tworzy innego [XMLHttpRequest](https://xhr.spec.whatwg.org/), i dodanie obsługi zdarzeń mógł przeanalizować odpowiedzi przychodzących. Dla pomyślnej odpowiedzi, jego tworzy tablicę współrzędne trasy zwrócił i dodaje go mapy `carRouteLayerName` warstwy. 
    
    Następujący fragment kodu wysyła również zapytania do usługi trasy uzyskać trasy dla określonego początkowy i punkt końcowy dla Twojego konta subskrypcji klucza. Ponieważ nie inne parametry są używane, trasa dla domyślnego trybu podróży *samochodu* jest zwracany. 

3. Zapisz **MapTruckRoute.html** plików lokalnie, a następnie otworzyć go w przeglądarce sieci web i sprawdź wynik. W przypadku pomyślnego połączenia z interfejsami API lokalizacji na podstawie usług powinna zostać wyświetlona mapy podobny do następującego. 

    ![Priorytetową tras za pomocą usługi Azure trasy](./media/tutorial-prioritized-routes/lbs-prioritized-routes.png)

    Należy pamiętać, że trasy ciężarówka w kolorze niebieskim, gdy trasa samochodu jest purpurowy.

## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Skonfiguruj zapytanie usługi trasy
> * Renderowanie trasy nadać priorytet w trybie podróży

Przejdź do **pojęcia** i **jak** artykuły, aby dowiedzieć się więcej lokalizacji na podstawie zestaw SDK usług Azure szczegółowo. 
