---
title: "Znajdowanie wielu tras za pomocą usług Azure Location Based Services | Microsoft Docs"
description: "Znajdowanie tras dla różnych sposobów podróży za pomocą usług Azure Location Based Services"
services: location-based-services
keywords: 
author: kgremban
ms.author: kgremban
ms.date: 11/28/2017
ms.topic: tutorial
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 986e8667ca421baa78647e77e43fef92d81a7982
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-location-based-services"></a>Znajdowanie tras dla różnych sposobów podróży za pomocą usług Azure Location Based Services

W tym samouczku pokazano, jak używać konta usług Azure Location Based Services i zestawu Route Service SDK w celu znajdowania trasy do punktu orientacyjnego z uwzględnieniem priorytetów na podstawie sposobu podróży. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie zapytania do usługi Route Service
> * Renderowanie tras z uwzględnieniem priorytetów na podstawie sposobu podróży

## <a name="prerequisites"></a>Wymagania wstępne

Przed kontynuowaniem [utwórz konto usług Azure Location Based Services](./tutorial-search-location.md#createaccount) i [pobierz klucz ze swojego konta](./tutorial-search-location.md#getkey). Możesz też zapoznać się z informacjami o używaniu kontrolki mapy i interfejsów API usługi wyszukiwania podanymi w samouczku [Wyszukiwanie pobliskiego punktu orientacyjnego za pomocą usług Azure Location Based Services](./tutorial-search-location.md), a także poznać podstawy używania interfejsów API usługi Route Service przedstawione w samouczku [Ustalanie trasy do punktu orientacyjnego za pomocą usług Azure Location Based Services](./tutorial-route-location.md).


<a id="queryroutes"></a>

## <a name="configure-your-route-service-query"></a>Konfigurowanie zapytania do usługi Route Service

Poniższa procedura umożliwia utworzenie statycznej strony HTML z osadzonym interfejsem API kontrolki mapy usług Location Based Services. 

1. Na komputerze lokalnym utwórz nowy plik i nadaj mu nazwę **MapTruckRoute.html**. 
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
    Zwróć uwagę, że w nagłówku HTML są osadzone lokalizacje zasobów dotyczące plików CSS i JavaScript dla biblioteki usług Azure Location Based Services. Zwróć też uwagę na segment *script* dodany do treści pliku HTML, który zawiera śródwierszowy kod JavaScript uzyskujący dostęp do interfejsu API kontrolki mapy platformy Azure.
3. Dodaj następujący kod JavaScript do bloku *script* w pliku HTML. Zamień symbol zastępczy *<insert-key>* na klucz podstawowy konta usług Location Based Services.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var LBSAccountKey = "<_your account key_>";
    var map = new atlas.Map("map", {
        "subscription-key": LBSAccountKey
    });
    ```
    Element **atlas.Map** zapewnia kontrolkę dla wizualnej interakcyjnej mapy internetowej i jest składnikiem interfejsu API kontrolki mapy platformy Azure.

4. Dodaj następujący kod JavaScript do bloku *script*, aby dodać wyświetlanie przepływu ruchu do mapy:

    ```JavaScript
    // Add Traffic Flow to the Map
    map.setTraffic({
        flow: "relative"
    });
    ```
    Ten kod ustawia wartość przepływu ruchu na `relative`, czyli szybkość ruchu na drodze względem swobodnego przepływu ruchu. Możesz również ustawić ją na `absolute` (względna szybkość ruchu na drodze) lub `relative-delay` (umożliwia wyświetlanie względnej szybkości, gdy różni się ona od swobodnego przepływu ruchu). 

5. Dodaj następujący kod JavaScript, aby utworzyć pinezki dla punktu początkowego i punktu końcowego trasy:

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
    Ten kod tworzy dwa [obiekty GeoJSON](https://en.wikipedia.org/wiki/GeoJSON) reprezentujące punkt początkowy i punkt końcowy trasy. 

6. Dodaj następujący kod JavaScript, aby dodać warstwy *linestrings* do kontrolki mapy w celu wyświetlania tras na podstawie sposobu podróży, na przykład _car_ (samochód) i _truck_ (ciężarówka).

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

7. Dodaj następujący kod JavaScript, aby dodać punkt początkowy i punkt końcowy do mapy:

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
    Wywołanie interfejsu API **map.setCameraBounds** dostosowuje okno mapy według współrzędnych punktu początkowego i punktu końcowego. Wywołanie interfejsu API **map.addPins** dodaje punkty do kontrolki mapy jako składniki wizualne.

8. Zapisz plik **MapTruckRoute.html** na swoim komputerze. 

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Renderowanie tras z uwzględnieniem priorytetów na podstawie sposobu podróży

W tej sekcji pokazano, jak używać interfejsu API usługi Route Service w usługach Azure Location Based Services do znajdowania wielu tras z danego punktu początkowego do punktu docelowego na podstawie sposobu podróży. Usługa Route Service udostępnia interfejsy API do planowania najszybszej, najkrótszej lub najbardziej ekologicznej trasy między dwiema lokalizacjami, uwzględniając ruch drogowy w czasie rzeczywistym. Umożliwia ona też użytkownikom planowanie tras w przyszłości, korzystając z obszernej historycznej bazy danych ruchu drogowego na platformie Azure i przewidując długość podróży trasami w dowolnym dniu i czasie. 

1. Otwórz plik **MapTruckRoute.html** utworzony w poprzedniej sekcji i dodaj następujący kod JavaScript do bloku *script*, aby uzyskać trasę dla ciężarówki przy użyciu usługi Route Service.

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
    truckRouteUrl += "&subscription-key=" + LBSAccountKey;
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
    Ten fragment kodu tworzy element [XMLHttpRequest](https://xhr.spec.whatwg.org/) i dodaje procedurę obsługi zdarzeń, która analizuje przychodzącą odpowiedź. W przypadku pomyślnej odpowiedzi tworzy on tablicę współrzędnych dla zwróconej trasy i dodaje ją do warstwy `truckRouteLayerName` mapy. 
    
    Ten fragment kodu wysyła też zapytanie do usługi Route Service, aby uzyskać trasę dla określonego punktu początkowego i punktu końcowego dla klucza konta. Następujące parametry opcjonalne służą do wskazywania trasy dla dużej ciężarówki:
   - Parametr `travelMode=truck` określa tryb podróży jako *truck* (ciężarówka). Inne obsługiwane sposoby podróży to *taxi* (taksówka), *bus* (autobus), *van* (furgonetka), *motorcycle* (motocykl) i wartość domyślna *car* (samochód).
   - Parametry `vehicleWidth`, `vehicleHeight` i `vehicleLength` określają wymiary pojazdu w metrach i są uwzględniane tylko w przypadku sposobu podróży *truck*.
   - Parametr `vehicleLoadType` klasyfikuje ładunek jako niebezpieczny i objęty ograniczeniami na niektórych drogach. Obecnie jest on uwzględniany również tylko w przypadku sposobu podróży *truck*.

2. Dodaj następujący kod JavaScript, aby uzyskać trasę dla samochodu przy użyciu usługi Route Service:

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
    carRouteUrl += "&subscription-key=" + LBSAccountKey;
    carRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttpCar.open("GET", carRouteUrl, true);
    xhttpCar.send();
    ```
    Ten fragment kodu tworzy kolejny element [XMLHttpRequest](https://xhr.spec.whatwg.org/) i dodaje procedurę obsługi zdarzeń, która analizuje przychodzącą odpowiedź. W przypadku pomyślnej odpowiedzi tworzy on tablicę współrzędnych dla zwróconej trasy i dodaje ją do warstwy `carRouteLayerName` mapy. 
    
    Ten fragment kodu wysyła też zapytanie do usługi Route Service, aby uzyskać trasę dla określonego punktu początkowego i punktu końcowego dla klucza konta. Ponieważ nie są używane żadne inne parametry, zwracana jest trasa dla domyślnego sposobu podróży *car* (samochód). 

3. Zapisz lokalnie plik **MapTruckRoute.html**, a następnie otwórz go w wybranej przeglądarce sieci Web i obejrzyj wynik. W przypadku pomyślnego połączenia z interfejsami API usług Location Based Services powinna pojawić się mapa podobna do poniższej. 

    ![Określanie priorytetów tras za pomocą usługi Azure Route Service](./media/tutorial-prioritized-routes/lbs-prioritized-routes.png)

    Zwróć uwagę, że trasa ciężarówki ma kolor niebieski, a trasa samochodu — purpurowy.

## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie zapytania do usługi Route Service
> * Renderowanie tras z uwzględnieniem priorytetów na podstawie sposobu podróży

Przejdź do artykułów **Pojęcia** i **Instrukcje**, aby dokładnie poznać zestaw Azure Location Based Services SDK. 
