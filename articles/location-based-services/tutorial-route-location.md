---
title: "Znajdź trasy z usług na podstawie lokalizacji platformy Azure | Dokumentacja firmy Microsoft"
description: "Trasy do punktu przydatne przy użyciu usługi na podstawie lokalizacji platformy Azure"
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
ms.openlocfilehash: 0f784e8ecd8fc94c12df1a819055718e06547b6b
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="route-to-a-point-of-interest-using-azure-location-based-services"></a>Trasy do punktu przydatne przy użyciu usługi na podstawie lokalizacji platformy Azure

Ten samouczek przedstawia sposób użycia konta usługi na podstawie lokalizacji platformy Azure i zestawu SDK usług trasy, można znaleźć trasy do punktu zainteresowań. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Pobierz adres współrzędnych
> * Zapytanie usługi trasy dla kierunków punktu zainteresowań

## <a name="prerequisites"></a>Wymagania wstępne

Przed kontynuowaniem upewnij się, że [tworzenia konta usługi na podstawie lokalizacji Azure](./tutorial-search-location.md#createaccount), i [Pobierz klucz subskrypcji dla Twojego konta](./tutorial-search-location.md#getkey). Można również zauważyć, jak używać formantu mapy i interfejsy API wyszukiwania usługi zgodnie z opisem w samouczku [wyszukiwania w pobliżu z interesujących przy użyciu usługi na podstawie lokalizacji Azure](./tutorial-search-location.md).


<a id="getcoordinates"></a>

## <a name="get-address-coordinates"></a>Pobierz adres współrzędnych

Poniższe kroki umożliwiają utworzenie statycznej strony HTML osadzonych z interfejsem API sterowania mapy lokalizacji na podstawie usług. 

1. Na komputerze lokalnym, Utwórz nowy plik i nadaj jej nazwę **MapRoute.html**. 
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
    Należy zwrócić uwagę, jak nagłówka HTML osadza lokalizacje zasobów pliki CSS i JavaScript dla biblioteki usług na podstawie lokalizacji platformy Azure. Spójrz również *skryptu* segmentu w treści pliku HTML, który będzie zawierać wbudowanego kodu JavaScript dostępu do interfejsów API Azure lokalizacji na podstawie usługi.

3. Dodaj następujący kod JavaScript do *skryptu* bloku pliku HTML. Zastąp symbol zastępczy *< klawisza insert >* z konta usługi na podstawie lokalizacji klucza podstawowego.

    ```HTML
            // Instantiate map to the div with id "map"
            var subscriptionKey = "<insert-key>";
            var map = new atlas.Map("map", {
                "subscription-key": subscriptionKey
            });

    ```
    **Atlas. Mapa** zapewnia formantu mapy visual i interaktywne sieci web i wchodzi w skład interfejsu API Azure mapy formantu.

4. Dodaj następujący kod JavaScript do *skryptu* bloku. Spowoduje to dodanie warstwę *linestrings* do formantu mapy, aby wyświetlić trasy:

    ```HTML
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

5. Dodaj następujący kod JavaScript, aby utworzyć punkt początkowy i końcowy dla trasy:

    ```HTML
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
    Ten kod tworzy dwa [obiektów GeoJSON](https://en.wikipedia.org/wiki/GeoJSON) do reprezentowania punktu początkowego i końcowego trasy. Punkt końcowy jest kombinacja szerokości geograficznej/długości dla jednego z *stacje benzyny* przeszukiwane w poprzednim samouczek [wyszukiwania w pobliżu z interesujących przy użyciu usługi na podstawie lokalizacji Azure](./tutorial-search-location.md).

6. Dodaj następujący kod JavaScript, aby dodać kody PIN dla punktu początkowego i końcowego do mapy:

    ```HTML
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
    Interfejs API **map.setCameraBounds** dostosowuje okna mapy zgodnie z współrzędne punktu początkowego i końcowego. Interfejs API **map.addPins** dodaje punkty do formantu mapy jako składniki wizualne.

7. Zapisz **MapRoute.html** plików na tym komputerze. 

<a id="getroute"></a>

## <a name="query-route-service-for-directions-to-point-of-interest"></a>Zapytanie usługi trasy dla kierunków punktu zainteresowań

W tej sekcji przedstawia sposób użycia interfejsu API usługi trasy Azure lokalizacji na podstawie usług można znaleźć trasy z punktu rozpoczęcia danego do miejsca docelowego. Usługa trasa udostępnia interfejsy API do zaplanowania najszybciej, jak najkrótszym lub oznakowania trasy między dwiema lokalizacjami, biorąc pod uwagę warunki ruchu w czasie rzeczywistym. Umożliwia także użytkownikom planowanie trasy w przyszłości przy użyciu bazy danych szeroką gamę ruchu historycznych platformy Azure i prognozowania okresów trasy dla każdego dnia i godziny. 

1. Otwórz **MapRoute.html** plik utworzony w poprzedniej sekcji i Dodaj następujący kod JavaScript, aby *skryptu* bloku, aby zilustrować usługi trasy.

    ```HTML
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
    Następujący fragment kodu tworzy [XMLHttpRequest](https://xhr.spec.whatwg.org/), i dodanie obsługi zdarzeń mógł przeanalizować odpowiedzi przychodzących. Dla pomyślnej odpowiedzi tworzy go tablicę współrzędne segmenty linii trasy pierwszy zwrócony. Następnie dodaje ten zestaw współrzędnych dla tej trasy do mapowania *linestrings* warstwy.

2. Dodaj następujący kod do *skryptu* bloku, aby wysłać XMLHttpRequest usługą trasy Azure lokalizacji na podstawie usług:

    ```HTML
            var url = "https://atlas.microsoft.com/route/directions/json?";
            url += "&api-version=1.0";
            url += "&subscription-key=" + subscriptionKey;
            url += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
                destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];
    
            xhttp.open("GET", url, true);
            xhttp.send();
    ```
    Żądanie powyżej przedstawiono wymaganych parametrów, które są Twoje konto subskrypcji klucza i współrzędne punktu początkowego i końcowego, w podanej kolejności. 

3. Zapisz **MapRoute.html** plików lokalnie, a następnie otworzyć go w przeglądarce sieci web i sprawdź wynik. W przypadku pomyślnego połączenia z interfejsami API lokalizacji na podstawie usług powinna zostać wyświetlona mapy podobny do następującego. 

    ![Formant mapy platformy Azure i usługi trasy](./media/tutorial-route-location/lbs-map-route.png)


## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Pobierz adres współrzędnych
> * Zapytanie usługi trasy dla kierunków punktu zainteresowań

Przejdź do samouczka [znaleźć trasy dla różnych trybach podróży przy użyciu usługi na podstawie lokalizacji Azure](./tutorial-prioritized-routes.md) Aby dowiedzieć się, jak używać usługi na podstawie lokalizacji Azure priorytety trasy do punktu zainteresowań, na podstawie trybu transportu. 
