---
title: "Wyszukiwanie za pomocą usług Azure Location Based Services | Microsoft Docs"
description: "Wyszukiwanie punktu orientacyjnego w pobliżu za pomocą usług Azure Location Based Services"
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
ms.openlocfilehash: 8da7d9112c9527945ab4b524625603faa84cf00d
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="search-nearby-point-of-interest-using-azure-location-based-services"></a>Wyszukiwanie punktu orientacyjnego w pobliżu za pomocą usług Azure Location Based Services

W tym samouczku przedstawiono sposób konfigurowania konta przy użyciu usług Azure Location Based Services, a następnie używania udostępnionych interfejsów API do wyszukiwania punktu orientacyjnego. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie konta przy użyciu usług Azure Location Based Services
> * Identyfikowanie klucza podstawowego dla konta usług Azure Location Based Services
> * Tworzenie nowej strony internetowej przy użyciu interfejsu API kontrolki mapy
> * Wyszukiwanie punktu orientacyjnego w pobliżu przy użyciu usługi Search Service

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/).

# <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal
Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-location-based-services"></a>Tworzenie konta przy użyciu usług Azure Location Based Services

Wykonaj następujące kroki, aby utworzyć nowe konto usług Location Based Services.

1. W lewym górnym rogu witryny [Azure Portal](https://portal.azure.com) kliknij przycisk **Utwórz zasób**.
2. W polu *Wyszukaj w portalu Marketplace* wpisz **location based services**.
3. Na liście *Wyniki* kliknij pozycję **Usługi oparte na lokalizacji (preview)**. Kliknij przycisk **Utwórz** znajdujący się poniżej mapy. 
4. Na stronie **Tworzenie konta usług Location Based Services** wprowadź następujące wartości:
    - *Nazwa* nowego konta. 
    - *Subskrypcja*, która ma być używana dla tego konta.
    - Nazwa *grupy zasobów* dla tego konta. Można wybrać pozycję *Utwórz nowe* lub *Użyj istniejącego* dla grupy zasobów.
    - Wybierz *lokalizację grupy zasobów*.
    - Zapoznaj się z *Warunkami dotyczącymi wersji zapoznawczej* i zaznacz pole wyboru, aby zaakceptować warunki. 
    - Na koniec kliknij przycisk **Utwórz**.
   
    ![Tworzenie konta usług Location Based Services w portalu](./media/tutorial-search-location/create-lbs-account.png)


<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>Pobieranie klucza podstawowego konta

Po pomyślnym utworzeniu konta usług Location Based Services wykonaj kroki, aby połączyć je z interfejsami API wyszukiwania na mapach:

1. Otwórz konto usług Location Based Services w portalu.
2. Przejdź do obszaru **USTAWIENIA** konta, a następnie wybierz pozycję **Klucze**.
3. Skopiuj **klucz podstawowy** do schowka. Zapisz go lokalnie do użycia w kolejnych krokach. 

    ![Pobieranie klucza podstawowego w portalu](./media/tutorial-search-location/lbs-get-key.png)


<a id="createmap"></a>

## <a name="create-new-web-page-using-azure-map-control-api"></a>Tworzenie nowej strony internetowej przy użyciu interfejsu API kontrolki mapy platformy Azure
Interfejs API kontrolek mapy platformy Azure to wygodna biblioteka klienta, która ułatwia integrowanie usług Azure Location Based Services z aplikacją internetową. Zmniejsza ona złożoność podstawowych wywołań usługi REST i zwiększa produktywność dzięki składnikom z możliwością definiowania stylu i dostosowywania. Poniższe kroki pokazują, jak utworzyć statyczną stronę HTML osadzoną przy użyciu interfejsu API kontrolki mapy w usługach Location Based Services. 

1. Na maszynie lokalnej utwórz nowy plik i nadaj mu nazwę **MapSearch.html**. 
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
    Zwróć uwagę, że nagłówek HTML zawiera pliki zasobów CSS i JavaScript obsługiwane przez bibliotekę kontrolek mapy platformy Azure. Zanotuj segment *script* dodany do *treści* pliku HTML. Ten segment będzie zawierać wbudowany kod JavaScript umożliwiający dostęp do interfejsów API usług Azure Location Based Services.
 
3.  Dodaj następujący kod JavaScript do bloku *script* w pliku HTML. W skrypcie użyj klucza podstawowego z konta usług Location Based Services. 

    ```JavaScript
    // Instantiate map to the div with id "map"
    var LBSAccountKey = "<_your account key_>";
    var map = new atlas.Map("map", {
        "subscription-key": LBSAccountKey
    });
    ```
    Ten segment inicjuje interfejs API kontrolki mapy dla klucza konta usług Azure Location Based Services. **Atlas** to przestrzeń nazw, która zawiera interfejs API kontrolki mapy platformy Azure i powiązane składniki wizualne. **atlas.Map** udostępnia kontrolkę wizualnej i interaktywnej mapy internetowej. Aby sprawdzić, jak wygląda mapa, można otworzyć stronę HTML w przeglądarce. 

4. Dodaj następujący kod JavaScript do bloku *script*, aby dodać warstwę pinezek wyszukiwania do kontrolki mapy:

    ```JavaScript
    // Initialize the pin layer for search results to the map
    var searchLayerName = "search-results";
    map.addPins([], {
        name: searchLayerName,
        cluster: false,
        icon: "pin-round-darkblue"
    });
    ```

5. Zapisz plik na maszynie. 


<a id="usesearch"></a>

## <a name="use-search-service-to-find-nearby-point-of-interest"></a>Wyszukiwanie punktu orientacyjnego w pobliżu przy użyciu usługi Search Service

W tej sekcji przedstawiono sposób wyszukiwania punktu orientacyjnego na mapie przy użyciu interfejsu API usługi Search Service w usługach Azure Location Based Services. Jest to interfejs API RESTful umożliwiający deweloperom wyszukiwanie adresów, punktów orientacyjnych i innych informacji geograficznych. Usługa Search Service przypisuje informacje o długości i szerokości geograficznej do określonego adresu. 

1. Otwórz plik **MapSearch.html** utworzony w poprzedniej sekcji i dodaj następujący kod JavaScript do bloku *script*, aby zilustrować usługę Search Service. 
    ```JavaScript
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
    Ten fragment kodu tworzy element [XMLHttpRequest](https://xhr.spec.whatwg.org/) i dodaje procedurę obsługi zdarzeń, która analizuje przychodzącą odpowiedź. W celu uzyskania pomyślnej odpowiedzi zbiera on informacje o adresach, nazwach oraz długości i szerokości geograficznej dla każdej zwróconej lokalizacji w zmiennej `searchPins`. Na koniec ta kolekcja punktów lokalizacji jest dodawana do kontrolki `map` jako pinezki. 

2. Dodaj następujący kod do bloku *script*, aby wysłać element XMLHttpRequest do usługi Search Service w usługach Azure Location Based Services:

    ```JavaScript
    var url = "https://atlas.microsoft.com/search/fuzzy/json?";
    url += "&api-version=1.0";
    url += "&query=gasoline%20station";
    url += "&subscription-key=" + LBSAccountKey;
    url += "&lat=47.6292";
    url += "&lon=-122.2337";
    url += "&radius=100000";

    xhttp.open("GET", url, true);
    xhttp.send();
    ``` 
    Ten fragment kodu używa podstawowego interfejsu API usługi Search Service o nazwie **Wyszukiwanie rozmyte**. Zarządza on najbardziej rozmytymi danymi wejściowymi, które obsługują dowolną kombinację tokenów adresu lub *punktu orientacyjnego*. Umożliwia wyszukiwanie pobliskiej **stacji benzynowej** dla danego adresu z określoną długością i szerokością geograficzną i w obrębie wybranego promienia. Klucz podstawowy konta podany wcześniej w przykładowym pliku jest używany do wywoływania usługi Location Based Services. Wyniki są zwracane w postaci par długość/szerokość geograficzna dla znalezionych lokalizacji. Aby obejrzeć pinezki wyszukiwania, można otworzyć stronę HTML w przeglądarce. 

3. Dodaj następujące wiersze do bloku *script*, aby utworzyć wyskakujące okienka dla punktów orientacyjnych zwróconych przez usługę Search Service:

    ```JavaScript
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
    Element **atlas.Popup** interfejsu API udostępnia okno informacyjne zakotwiczone w żądanym położeniu na mapie. Ten fragment kodu ustawia zawartość i położenie wyskakującego okienka, a także dodaje do kontrolki `map` odbiornik zdarzeń oczekujący na przesunięcie _myszy_ przez wyskakujące okienko. 

4. Zapisz plik, a następnie otwórz plik **MapSearch.html** pliku w przeglądarce internetowej i sprawdź wynik. W tym momencie mapa w przeglądarce pokazuje wyskakujące okienka informacyjne (podobne do poniższego) po najechaniu kursorem na dowolną wyświetloną pinezkę wyszukiwania. 

    ![Kontrolka mapy platformy Azure i usługa Search Service](./media/tutorial-search-location/lbs-map-search.png)


## <a name="next-steps"></a>Następne kroki
W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie konta przy użyciu usług Azure Location Based Services
> * Pobieranie klucza podstawowego konta
> * Tworzenie nowej strony internetowej przy użyciu interfejsu API kontrolki mapy
> * Wyszukiwanie punktu orientacyjnego w pobliżu przy użyciu usługi Search Service

Przejdź do samouczka [Ustalanie trasy do punktu orientacyjnego za pomocą usług Azure Location Based Services](./tutorial-route-location.md), aby dowiedzieć się, jak używać usług Azure Location Based Services w celu określenia trasy do punktu orientacyjnego. 
