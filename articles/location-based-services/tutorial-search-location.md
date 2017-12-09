---
title: "Usługi na podstawie wyszukiwania z lokalizacji platformy Azure | Dokumentacja firmy Microsoft"
description: "Wyszukiwanie w pobliżu z interesujących przy użyciu usługi na podstawie lokalizacji platformy Azure"
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
ms.openlocfilehash: 31b0df0442a46761cb19e390e723535ff5a81594
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="search-nearby-point-of-interest-using-azure-location-based-services"></a>Wyszukiwanie w pobliżu z interesujących przy użyciu usługi na podstawie lokalizacji platformy Azure

W tym samouczku przedstawiono sposób skonfigurowane konto z usług na podstawie lokalizacji platformy Azure, a następnie użyć podanego interfejsów API do wyszukania punktu zainteresowań. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie konta usługi z usługami na podstawie lokalizacji platformy Azure
> * Pobierz klucz subskrypcji dla Twojego konta
> * Utwórz nową stronę sieci web przy użyciu interfejsu API sterowania mapy
> * Użyj usługi wyszukiwania, aby znaleźć pobliskie z interesujących

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/).

# <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal
Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-location-based-services"></a>Tworzenie konta usługi z usługami na podstawie lokalizacji platformy Azure

Wykonaj następujące kroki, aby utworzyć nowe konto usługi na podstawie lokalizacji.

1. W lewym górnym rogu witryny [Azure Portal](https://portal.azure.com) kliknij przycisk **Utwórz zasób**.
2. W polu *Wyszukaj w portalu Marketplace* wpisz **location based services**.
3. Na liście *Wyniki* kliknij pozycję **Usługi oparte na lokalizacji (preview)**. Kliknij przycisk **Utwórz** znajdujący się poniżej mapy. 
4. Na **tworzenie lokalizacji na podstawie usługi konta** strony, wprowadź następujące wartości:
    - *Nazwa* z nowego konta. 
    - *Subskrypcji* , który ma być używany dla tego konta.
    - *Grupy zasobów* nazwę dla tego konta. Można wybrać opcję *Utwórz nowy* lub *Użyj istniejącego* grupy zasobów.
    - Wybierz *lokalizacja grupy zasobów*.
    - Odczyt *warunki dotyczące* i zaznacz pole wyboru, aby zaakceptować warunki. 
    - Na koniec kliknij **Utwórz** przycisku.
   
    ![Tworzenie konta usług Location Based Services w portalu](./media/tutorial-search-location/create-lbs-account.png)


<a id="getkey"></a>

## <a name="get-the-subscription-key-for-your-account"></a>Pobierz klucz subskrypcji dla Twojego konta

Po pomyślnym utworzeniu konta usługi na podstawie lokalizacji, wykonaj kroki, aby połączyć wyszukiwanie w mapie interfejsów API:

1. Otwórz konto usługi na podstawie lokalizacji w portalu.
2. Przejdź do swojego konta **ustawienia**, a następnie wybierz **klucze**.
3. Kopiuj **klucz podstawowy** do Schowka. Zapisz lokalnie, do użycia w krokach postępowania. 

    ![Pobierz klucz podstawowy w portalu](./media/tutorial-search-location/lbs-get-key.png)


<a id="createmap"></a>

## <a name="create-new-web-page-using-azure-map-control-api"></a>Utwórz nową stronę sieci web przy użyciu interfejsu API Azure mapy formantu
Azure interfejs API sterowania mapy jest biblioteki wygodny klienta, która pozwala łatwo zintegrować usługi na podstawie lokalizacji Azure aplikacji sieci web. Ukrywa złożoność systemu od zera wywołań usługi REST, a zwiększa wydajność pracy ze składnikami styleable i dostosowania. Poniższe kroki pokazują, jak utworzyć statyczne strony HTML osadzonych z interfejsem API sterowania mapy lokalizacji na podstawie usług. 

1. Na komputerze lokalnym, Utwórz nowy plik i nadaj jej nazwę **MapSearch.html**. 
2. Dodaj następujące składniki HTML do pliku:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Search</title>

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
    Zwróć uwagę, że nagłówek HTML zawiera pliki zasobów CSS i JavaScript, obsługiwanych przez biblioteki Azure formantu mapy. Uwaga *skryptu* dodane do segmentu *treści* pliku HTML. Ten segment będzie zawierać wbudowanego kodu JavaScript dostępu do interfejsów API Azure lokalizacji na podstawie tej usługi.
 
3.  Dodaj następujący kod JavaScript do *skryptu* bloku pliku HTML. Zastąp symbol zastępczy *< klawisza insert >* z konta usługi na podstawie lokalizacji klucza podstawowego. 

    ```HTML/JavaScript
    // Instantiate map to the div with id "map"
    var subscriptionKey = "<insert-key>";
    var map = new atlas.Map("map", {
        "subscription-key": subscriptionKey
    });
    ```
    Ten segment inicjuje interfejs API sterowania mapy dla klucza subskrypcji. **Atlas** jest przestrzeni nazw, która zawiera interfejs API sterowania mapy Azure i powiązane składniki visual. **Atlas. Mapa** zapewnia formantu mapy visual i interaktywne sieci web. Można zauważyć, jak mapy wygląda przez otwarcie strony HTML w przeglądarce. 

4. Dodaj następujący kod JavaScript do *skryptu* bloku, aby dodać warstwę numerów PIN wyszukiwania do formantu mapy:

    ```HTML/JavaScript
    // Initialize the pin layer for search results to the map
    var searchLayerName = "search-results";
    map.addPins([], {
        name: searchLayerName,
        cluster: false,
        icon: "pin-round-darkblue"
    });
    ```

5. Zapisz plik na komputerze. 


<a id="usesearch"></a>

## <a name="use-search-service-to-find-nearby-point-of-interest"></a>Użyj usługi wyszukiwania, aby znaleźć pobliskie z interesujących

W tej sekcji przedstawia sposób użycia interfejsu API usługi wyszukiwanie Azure lokalizacji na podstawie usług do znajdowania punktu zainteresowania na mapie. Jest przeznaczone dla deweloperów wyszukać adresy, interesujących akcji i inne informacje geograficzne interfejsu API RESTful. Usługa wyszukiwania przypisuje informacji współrzędne geograficzne do określonego adresu. 

1. Otwórz **MapSearch.html** plik utworzony w poprzedniej sekcji i Dodaj następujący kod JavaScript, aby *skryptu* bloku, aby zilustrować usługi wyszukiwania. 
    ```HTML/JavaScript
    // Perform a request to the search service and create a pin on the map for each result
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function () {
        var searchPins = [];

        if (this.readyState === 4 && this.status === 200) {
            var response = JSON.parse(this.responseText);

            var poiResults = response.results.filter((result) => { return result.type === "POI" }) || [];

            searchPins = poiResults.map((poiResult) => {
                var poiPosition = [poiResult.position.lon, poiResult.position.lat];
                return new atlas.data.Feature(new atlas.data.Point(poiPosition), {
                    name: poiResult.poi.name,
                    address: poiResult.address.freeformAddress,
                    position: poiResult.position.lat + ", " + poiResult.position.lon
                });
            });

            map.addPins(searchPins, {
                name: searchLayerName
            });

            var lons = searchPins.map((pin) => { return pin.geometry.coordinates[0] });
            var lats = searchPins.map((pin) => { return pin.geometry.coordinates[1] });

            var swLon = Math.min.apply(null, lons);
            var swLat = Math.min.apply(null, lats);
            var neLon = Math.max.apply(null, lons);
            var neLat = Math.max.apply(null, lats);

            map.setCameraBounds({
                bounds: [swLon, swLat, neLon, neLat],
                padding: 50
            });
        }
    };
    ```
    Następujący fragment kodu tworzy [XMLHttpRequest](https://xhr.spec.whatwg.org/), i dodanie obsługi zdarzeń mógł przeanalizować odpowiedzi przychodzących. Dla pomyślnej odpowiedzi, zbiera adresy, nazwy, szerokości i logitude informacji dla każdej lokalizacji zwracane w `searchPins` zmiennej. Na koniec dodaje ten zbiór lokalizację `map` formantu numerów PIN. 

2. Dodaj następujący kod do *skryptu* bloku, aby wysłać XMLHttpRequest usłudze Wyszukiwanie Azure lokalizacji na podstawie usług:

    ```HTML/JavaScript
    var url = "https://atlas.microsoft.com/search/fuzzy/json?";
    url += "&api-version=1.0";
    url += "&query=gasoline%20station";
    url += "&subscription-key=" + subscriptionKey;
    url += "&lat=47.6292";
    url += "&lon=-122.2337";
    url += "&radius=100000";

    xhttp.open("GET", url, true);
    xhttp.send();
    ``` 
    Ta Wstawka kodu używa wyszukiwania podstawowego interfejsu API usługi wyszukiwania o nazwie **Wyszukiwanie rozmyte**. Obsługuje on najbardziej rozmytego dowolną kombinację adresu obsługi danych wejściowych lub *POI* tokenów. Wyszukuje pobliskich **stacji benzyny**, dla danego adresu w współrzędne geograficzne i w obrębie określonej usługi radius. Twoje konto subskrypcji klucza podane wcześniej w przykładowym pliku używa do wywoływania usług na podstawie lokalizacji. Zwraca wyniki w postaci szerokości geograficznej/geograficzne pary dla lokalizacji można odnaleźć. Numery PIN wyszukiwania można zaobserwować, otwierając stronę HTML w przeglądarce. 

3. Dodaj następujące wiersze do *skryptu* bloku, aby utworzyć wyskakujące okienka dla punktów odsetek zwrócony przez usługę wyszukiwania:

    ```HTML/JavaScript
    // Add a popup to the map which will display some basic information about a search result on hover over a pin
    var popup = new atlas.Popup();
    map.addEventListener("mouseover", searchLayerName, (e) => {
        var popupContentElement = document.createElement("div");
        popupContentElement.style.padding = "5px";

        var popupNameElement = document.createElement("div");
        popupNameElement.innerText = e.features[0].properties.name;
        popupContentElement.appendChild(popupNameElement);

        var popupAddressElement = document.createElement("div");
        popupAddressElement.innerText = e.features[0].properties.address;
        popupContentElement.appendChild(popupAddressElement);

        var popupPositionElement = document.createElement("div");
        popupPositionElement.innerText = e.features[0].properties.position;
        popupContentElement.appendChild(popupPositionElement);

        popup.setPopupOptions({
            position: e.features[0].geometry.coordinates,
            content: popupContentElement
        });

        popup.open(map);
    });
    ```
    Interfejs API **atlas. Menu podręczne** informuje okno zakotwiczonych w pozycji wymagane na mapie. Następujący fragment kodu ustawia zawartości i pozycji dla elementu popup, a także dodaje odbiornik zdarzeń do `map` kontroli oczekiwanie na _myszy_ na menu podręcznego. 

4. Zapisz plik, a następnie otwórz **MapSearch.html** pliku w przeglądarce sieci web i sprawdź wynik. W tym momencie mapy w przeglądarce pokazuje informacje wyskakujące okienka po umieszczeniu na jeden z kodów PIN wyszukiwania pokazano, podobny do następującego. 

    ![Formant mapy platformy Azure i usługi wyszukiwania](./media/tutorial-search-location/lbs-map-search.png)


## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie konta usługi z usługami na podstawie lokalizacji platformy Azure
> * Pobierz klucz subskrypcji dla Twojego konta
> * Utwórz nową stronę sieci web przy użyciu interfejsu API sterowania mapy
> * Użyj usługi wyszukiwania, aby znaleźć pobliskie z interesujących

Przejdź do samouczka [trasy do punktu przydatne przy użyciu usługi na podstawie lokalizacji Azure](./tutorial-route-location.md) Aby dowiedzieć się, jak używać usługi na podstawie lokalizacji platformy Azure do trasy do punktu zainteresowań. 
