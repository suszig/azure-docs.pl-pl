---
title: "Jak używać usługi Azure lokalizacji na podstawie usług formantu mapy | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać biblioteki Javascript po stronie klienta formantu mapy usług na podstawie lokalizacji platformy Azure."
services: location-based-services
keywords: "Nie dodawaj lub Edytuj słowa kluczowe bez konsultacji z ekspert od optymalizacji pod kątem Wyszukiwarek."
author: philmea
ms.author: philmea
ms.date: 11/22/2017
ms.topic: article
ms.service: location-based-services
manager: timlt
ms.openlocfilehash: 06743640aae5e06d0160105458d9a3cfa35d5040
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="how-to-use-the-azure-location-based-services-map-control"></a>Jak używać usługi Azure lokalizacji na podstawie usług formantu mapy
Biblioteka języka Javascript po stronie klienta formantu mapy umożliwia renderowanie mapy i osadzone funkcje usług na podstawie lokalizacji platformy Azure w sieci web lub aplikacji mobilnej. 

## <a name="prerequisites"></a>Wymagania wstępne
Usługi na podstawie lokalizacji Azure konto i Subskrypcja klucz. Aby uzyskać informacje o tworzeniu konta i pobierania klucza subskrypcji, zobacz [jak zarządzać Twoje konto usługi na podstawie lokalizacji platformy Azure i klucze](how-to-manage-account-keys.md). 

## <a name="create-a-new-map-in-a-web-page-using-the-map-control-api"></a>Tworzenie nowej mapy na stronie sieci web przy użyciu interfejsu API sterowania mapy
Na stronie sieci web można osadzić mapy, przy użyciu biblioteki Javascript po stronie klienta formantu mapy.

1. Utwórz nowy plik i nadaj mu nazwę MapSearch.html.

2. Dodać usługi na podstawie lokalizacji Azure arkusza stylów i skrypt źródła odwołań do `<head>` elementu pliku:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
    ```
    
3. Aby renderować nowej mapy w przeglądarce, Dodaj **#map** odwołania w `<style>` elementu.

    ```html
    #map {
                width: 100%;
                height: 100%;
            }
    ``` 
    
4. Aby można było zainicjować formantu mapy, należy zdefiniować nową sekcję w treści html i utworzyć skrypt. Użyj własnego klucza subskrypcji z konta usługi na podstawie lokalizacji platformy Azure. 

    ```html
    <div id="map">
        <script>
            var subscriptionKey = "<_subscriptionKey_>";
            var map = new atlas.Map("map", {
                "subscription-key": subscriptionKey,
                center: [47.59093,-122.33263],
                zoom: 12
            });
        <script>
    <div>
    ```
    
5. Otwórz plik w przeglądarce sieci web i wyświetlić renderowanych mapy.