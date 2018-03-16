---
title: "Znajdowanie trasy za pomocą usług Azure Location Based Services | Microsoft Docs"
description: "Ustalanie trasy do punktu orientacyjnego za pomocą usług Azure Location Based Services"
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
ms.openlocfilehash: b54d97afb4e762a7d252acf7cf53a5deac01d43f
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="route-to-a-point-of-interest-using-azure-location-based-services"></a>Ustalanie trasy do punktu orientacyjnego za pomocą usług Azure Location Based Services

W tym samouczku pokazano, jak używać konta usług Azure Location Based Services i zestawu Route Service SDK w celu znajdowania trasy do punktu orientacyjnego. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Pobieranie współrzędnych adresu
> * Wysyłanie do usługi Route Service zapytania dotyczącego wskazówek dojazdu do punktu orientacyjnego

## <a name="prerequisites"></a>Wymagania wstępne

Przed kontynuowaniem [utwórz konto usług Azure Location Based Services](./tutorial-search-location.md#createaccount) i [pobierz klucz subskrypcji dla swojego konta](./tutorial-search-location.md#getkey). Możesz też zapoznać się z informacjami o używaniu kontrolki mapy i interfejsów API usługi wyszukiwania podanymi w samouczku [Wyszukiwanie pobliskiego punktu orientacyjnego za pomocą usług Azure Location Based Services](./tutorial-search-location.md).


<a id="getcoordinates"></a>

## <a name="get-address-coordinates"></a>Pobieranie współrzędnych adresu

Poniższa procedura umożliwia utworzenie statycznej strony HTML z osadzonym interfejsem API kontrolki mapy usług Location Based Services. 

1. Na komputerze lokalnym utwórz nowy plik i nadaj mu nazwę **MapRoute.html**. 
2. Dodaj następujące składniki HTML do pliku:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Route</title>
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
    Zwróć uwagę, że w nagłówku HTML są osadzone lokalizacje zasobów dotyczące plików CSS i JavaScript dla biblioteki usług Azure Location Based Services. Zwróć też uwagę na segment *script* w treści pliku HTML, który będzie zawierał śródwierszowy kod JavaScript uzyskujący dostęp do interfejsów API usług Azure Location Based Services.

3. Dodaj następujący kod JavaScript do bloku *script* w pliku HTML. W skrypcie użyj klucza podstawowego z konta usług Location Based Services.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var LBSAccountKey = "<_your account key_>";
    var map = new atlas.Map("map", {
        "subscription-key": LBSAccountKey
    });
    ```
    Element **atlas.Map** zapewnia kontrolkę dla wizualnej interakcyjnej mapy internetowej i jest składnikiem interfejsu API kontrolki mapy platformy Azure.

4. Dodaj następujący kod JavaScript do bloku *script*. Spowoduje to dodanie warstwy *linestrings* do kontrolki mapy w celu wyświetlania trasy:

    ```JavaScript
    // Initialize the linestring layer for routes on the map
    var routeLinesLayerName = "routes";
    map.addLinestrings([], {
        name: routeLinesLayerName,
        color: "#2272B9",
        width: 5,
        cap: "round",
        join: "round",
        before: "labels"
    });
    ```

5. Dodaj następujący kod JavaScript, aby utworzyć punkt początkowy i punkt końcowy trasy:

    ```JavaScript
    // Create the GeoJSON objects which represent the start and end point of the route
    var startPoint = new atlas.data.Point([-122.130137, 47.644702]);
    var startPin = new atlas.data.Feature(startPoint, {
        title: "Microsoft",
        icon: "pin-round-blue"
    });

    var destinationPoint = new atlas.data.Point([-122.3352, 47.61397]);
    var destinationPin = new atlas.data.Feature(destinationPoint, {
        title: "Contoso Oil & Gas",
        icon: "pin-blue"
    });
    ```
    Ten kod tworzy dwa [obiekty GeoJSON](https://en.wikipedia.org/wiki/GeoJSON) reprezentujące punkt początkowy i punkt końcowy trasy. Punkt końcowy to kombinacja szerokości i długości geograficznej jednej ze *stacji benzynowych* wyszukanych w poprzednim samouczku [Wyszukiwanie pobliskiego punktu orientacyjnego za pomocą usług Azure Location Based Services](./tutorial-search-location.md).

6. Dodaj następujący kod JavaScript, aby dodać pinezki punktu początkowego i punktu końcowego do mapy:

    ```JavaScript
    // Fit the map window to the bounding box defined by the start and destination points
    var swLon = Math.min(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var swLat = Math.min(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    var neLon = Math.max(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var neLat = Math.max(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    map.setCameraBounds({
        bounds: [swLon, swLat, neLon, neLat],
        padding: 50
    });

    // Add pins to the map for the start and end point of the route
    map.addPins([startPin, destinationPin], {
        name: "route-pins",
        textFont: "SegoeUi-Regular",
        textOffset: [0, -20]
    });
    ``` 
    Wywołanie interfejsu API **map.setCameraBounds** dostosowuje okno mapy według współrzędnych punktu początkowego i punktu końcowego. Wywołanie interfejsu API **map.addPins** dodaje punkty do kontrolki mapy jako składniki wizualne.

7. Zapisz plik **MapRoute.html** na swoim komputerze. 

<a id="getroute"></a>

## <a name="query-route-service-for-directions-to-point-of-interest"></a>Wysyłanie do usługi Route Service zapytania dotyczącego wskazówek dojazdu do punktu orientacyjnego

W tej sekcji pokazano, jak używać interfejsu API usługi Route Service w usługach Azure Location Based Services do znajdowania trasy z danego punktu początkowego do punktu docelowego. Usługa Route Service udostępnia interfejsy API do planowania najszybszej, najkrótszej lub najbardziej ekologicznej trasy między dwiema lokalizacjami, uwzględniając ruch drogowy w czasie rzeczywistym. Umożliwia ona też użytkownikom planowanie tras w przyszłości, korzystając z obszernej historycznej bazy danych ruchu drogowego na platformie Azure i przewidując długość podróży trasami w dowolnym dniu i czasie. 

1. Otwórz plik **MapRoute.html** utworzony w poprzedniej sekcji i dodaj do bloku *script* następujący kod JavaScript, który przedstawia działanie usługi Route Service.

    ```JavaScript
    // Perform a request to the route service and draw the resulting route on the map
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(xhttp.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], { name: routeLinesLayerName });
        }
    };
    ```
    Ten fragment kodu tworzy element [XMLHttpRequest](https://xhr.spec.whatwg.org/) i dodaje procedurę obsługi zdarzeń, która analizuje przychodzącą odpowiedź. W przypadku pomyślnej odpowiedzi tworzy on tablicę współrzędnych dla liniowych segmentów pierwszej zwróconej trasy. Następnie dodaje ten zestaw współrzędnych dla tej trasy do warstwy *linestrings* mapy.

2. Dodaj następujący kod do bloku *script* w celu wysłania elementu XMLHttpRequest do usługi Route Service w usługach Azure Location Based Services:

    ```JavaScript
    var url = "https://atlas.microsoft.com/route/directions/json?";
    url += "&api-version=1.0";
    url += "&subscription-key=" + LBSAccountKey;
    url += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttp.open("GET", url, true);
    xhttp.send();
    ```
    Powyższe żądanie przedstawia wymagane parametry — klucz konta oraz współrzędne punktu początkowego i punktu końcowego w podanej kolejności. 

3. Zapisz lokalnie plik **MapRoute.html**, a następnie otwórz go w wybranej przeglądarce sieci Web i obejrzyj wynik. W przypadku pomyślnego połączenia z interfejsami API usług Location Based Services powinna pojawić się mapa podobna do poniższej. 

    ![Kontrolka mapy platformy Azure i usługa Route Service](./media/tutorial-route-location/lbs-map-route.png)


## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Pobieranie współrzędnych adresu
> * Wysyłanie do usługi Route Service zapytania dotyczącego wskazówek dojazdu do punktu orientacyjnego

Przejdź do samouczka [Znajdowanie tras dla różnych sposobów podróży za pomocą usług Azure Location Based Services](./tutorial-prioritized-routes.md), aby dowiedzieć się, jak używać usług Azure Location Based Services w celu określania priorytetów tras do punktów orientacyjnych na podstawie sposobu podróży. 
