---
title: "Omówienie usług Azure Location Based Services | Microsoft Docs"
description: "Wprowadzenie do usług Azure Location Based Services (wersja zapoznawcza)"
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 02/05/2017
ms.topic: overview
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 9e6236f7d69556d7636962c98886d9f9508445ac
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="an-introduction-to-azure-location-based-services-preview"></a>Wprowadzenie do usług Azure Location Based Services (wersja zapoznawcza)
Usługi Azure Location Based Services to portfel usług geoprzestrzennych obejmujący interfejsy API usług dla map, wyszukiwania, wyznaczania tras, ruchu drogowego oraz stref czasowych. Portfel usług zgodnych z interfejsem Azure OneAPI umożliwia używanie znanych narzędzi deweloperskich do szybkiego tworzenia i skalowania rozwiązań, które będą integrować informacje o lokalizacji w rozwiązaniach platformy Azure. Usługi Azure Location Based Services udostępniają deweloperom ze wszystkich branż zaawansowane funkcje geoprzestrzenne wraz z aktualnymi danymi map niezbędnymi do zapewniania kontekstu geograficznego aplikacjom internetowym i mobilnym. Usługi Azure Location Based Services to zestaw interfejsów API REST zgodnych z interfejsem Azure One API wyposażony w internetową kontrolkę JavaScript. Dzięki niej programowanie jest bardzo łatwe i elastyczne, a rezultaty można przenosić na różne platformy. 

Usługi Azure Location Based Services składają się z pięciu głównych usług wspierających aplikacje platformy Azure wymagające kontekstu geograficznego. Każdą z tych usług omówiono poniżej.

**Render Service** — usługa Render Service została zaprojektowana dla deweloperów do tworzenia aplikacji internetowych i mobilnych związanych z mapowaniem. Usługa używa rastrowych obrazów graficznych o wysokiej jakości — dostępnych z 19 poziomami powiększenia — lub w pełni konfigurowalnych obrazów map w formacie wektorowym.

![Azure Location Based Services Map.png](media/about-location-based-services/Introduction_Map.png)

**Route Service** — usługa Route Service została opracowana z niezawodnymi obliczeniami geometrycznymi rzeczywistej infrastruktury oraz wieloma trybami wytycznych transportowych. Usługa umożliwia deweloperom obliczanie wytycznych w różnych trybach podróży, np. podróży samochodem, ciężarówką, rowerem lub pieszo; a także użycie wielu różnych danych wejściowych, np. warunków w ruchu drogowym, ograniczeń dotyczących masy pojazdu lub informacji dotyczących transportu materiałów niebezpiecznych.

![Azure Location Based Services Route.png](media/about-location-based-services/Introduction_Route.png)

**Search Service** — usługa Search Service została opracowana dla deweloperów do wyszukiwania adresów, miejsc, list biznesowych według nazwy lub kategorii oraz innych informacji geograficznych. Usługa Search Service może też [odwracać adresy geokodowania](https://en.wikipedia.org/wiki/Reverse_geocoding) i przechodzić przez ulice w oparciu o długość/szerokość geograficzną. 

![Azure Location Based Services Search.png](media/about-location-based-services/Introduction_Search.png)

**Time Zone Service** — usługa Time Zone Service umożliwia wysyłanie zapytań o bieżące, historyczne i przyszłe informacje o strefie czasowej przy użyciu par szerokość-długość geograficzna lub identyfikatora [IANA ID](http://www.iana.org/). Usługa Time Zone ponadto umożliwia konwertowanie identyfikatorów strefy czasowej systemu Microsoft Windows na strefy czasowe IANA, pobieranie przesunięcia strefy czasowej względem czasu UTC oraz pobieranie bieżącego czasu w odpowiedniej strefie czasowej. Typowa odpowiedź JSON na zapytanie wysyłane do usługi Time Zone Service wygląda następująco:

```JSON
{
    "Version": "2017c",
    "ReferenceUtcTimestamp": "2017-11-20T23:09:48.686173Z",
    "TimeZones": [{
        "Id": "America/Los_Angeles",
        "ReferenceTime": {
            "Tag": "PST",
            "StandardOffset": "-08:00:00",
            "DaylightSavings": "00:00:00",
            "WallTime": "2017-11-20T15:09:48.686173-08:00",
            "PosixTzValidYear": 2017,
            "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
        }
    }]
}
```

**Traffic Service** — usługa Traffic Service to zbiór usług internetowych zaprojektowanych dla deweloperów do tworzenia aplikacji internetowych i mobilnych wymagających informacji o ruchu. Oferta jest podzielona następująco:
1. Traffic Flow — zapewnia prędkości i czasy podróży w czasie rzeczywistym dla wszystkich kluczowych dróg w sieci; oraz 
2. Traffic Incidents — zapewnia dokładny wgląd w korki na drogach i zdarzenia w sieci dróg.

![Azure Location Based Services Traffic](media/about-location-based-services/Introduction_Traffic.png)

Usługi Azure Location Based Services zostały opracowane z myślą o mobilności i mogą wspierać aplikacje międzyplatformowe, ponieważ model programowania jest niezależny i obsługuje dane wyjściowe JSON za pośrednictwem interfejsów API REST. Ponadto usługi Azure LBS oferują wygodną kontrolkę mapy JavaScript z prostym modelem programowania, która umożliwia szybkie opracowywanie aplikacji internetowych i mobilnych. 

Usługi Azure Location Based Services używają schematu uwierzytelniania opartego na kluczach, więc uzyskanie dostępu do usług wymaga jedynie przejścia do witryny [Azure Portal](http://portal.azure.com) i utworzenia konta usług Azure Location Based Services. Konto będzie zawierać dwa wstępnie wygenerowane klucze do użycia. Rozpocznij integrowanie tych możliwości opartych na lokalizacji bezpośrednio w aplikacjach przy użyciu dowolnego z kluczy w żądaniach wysyłanych do usług Azure Location Based Services.

## <a name="relationship-with-bing-maps"></a>Relacja z usługą Mapy Bing
Usługi Azure Location Based Services opisane w tym dokumencie różnią się od usług udostępnianych przez usługę Mapy Bing.  Mimo że korzystają z wielu takich samych funkcji, te usługi są różne i nie są ze sobą związane.  Dostępność nowej usługi w obrębie platformy Azure, która będzie zarządzana oddzielnie, nie ma żadnego wpływu na produkt lub harmonogram działania usługi Mapy Bing.

Celem firmy Microsoft jest zapewnienie wyboru dla społeczności deweloperów pod względem ofert usług lokalizacji.  Poniżej przedstawiono pewne proste wskazówki dla deweloperów dotyczące wyboru odpowiedniej usługi w przypadku różnych przypadków użycia i sytuacji klienta.  Należy pamiętać, że te wskazówki dotyczą aktualnie usług Azure LBS na etapie publicznej wersji zapoznawczej i zostaną zaktualizowane po ogłoszeniu ogólnej dostępności tych usług w dalszej części 2018 roku.

| Kryteria klienta | Użyj usług Azure Location Based Services, gdy… | Użyj usługi Mapy Bing, gdy… |
| ------------- | ------------- | ------------- |
| Środowisko deweloperskie | Tworzenie w ramach innych usług platformy Azure lub korzystanie z nich | Używanie chmury innej firmy lub innego środowiska deweloperskiego |
| Etap opracowywania  | Ponieważ usługi Azure LBS znajdują się obecnie w publicznej wersji zapoznawczej, są one optymalizowane pod kątem testowania na wczesnym etapie i opracowywania weryfikacji koncepcji | Na potrzeby środowiska produkcyjnego wymagana jest umowa SLA klasy korporacyjnej |
| Opcje cennika | Wystarczające są wstępne opcje cenowe na potrzeby dewelopera | Wymagany jest dostosowany cennik klasy korporacyjnej |
| Środowisko przypadków użycia | Wymagane jest użycie w pojeździe | Użycie w pojeździe nie jest wymagane |
| Pokrycie geograficznie | Indie, Chiny, Japonia i Korea Południowa — niewymagane | Indie, Chiny, Japonia i Korea Południowa — wymagane |
| Zawartość związana z mapami | Standardowe mapy powierzchni są wystarczające | Wymagane są obrazy satelitarne, z powietrza i ulic |
| Bazowe źródło map | Preferowane jest użycie danych mapowych programu TomTom | Preferowane jest użycie danych mapowych programu HERE |

Utwórz [konto usług Azure Location Based Services już dzisiaj](http://aka.ms/azurelbsportal).

## <a name="next-steps"></a>Następne kroki

Omówiliśmy pokrótce usługi Azure Location Based Services (wersja zapoznawcza). Następnym krokiem będzie wypróbowanie prostej aplikacji z przykładem użycia usług Location Based Services oraz utworzenie kompleksowego scenariusza w Twojej aplikacji internetowej.

> [!div class="nextstepaction"]
> [Uruchamianie demonstracyjnego wyszukiwania na mapie za pomocą usług Azure Location Based Services (wersja zapoznawcza)](quick-demo-map-app.md)
> [Wyszukiwanie pobliskiego punktu orientacyjnego przy użyciu usług Azure Location Based Services](tutorial-search-location.md)
