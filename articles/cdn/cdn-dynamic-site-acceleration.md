---
title: "Akceleracja dynamiczne witryny za pomocą usługi Azure CDN"
description: "Dynamiczne witryny przyspieszenie nowości"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: v-semcev
ms.openlocfilehash: be2719e0e02c8bc69800ef4a3e7da3c3164cb9dd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="dynamic-site-acceleration-via-azure-cdn"></a>Akceleracja dynamiczne witryny za pomocą usługi Azure CDN

Z rozłożenie mediów społecznościowych, handlu elektronicznego i web spersonalizowanych funkcji hyper szybko rośnie odsetek obsługiwanej użytkownikom końcowym zawartości jest generowany w czasie rzeczywistym. Użytkownicy oczekują aplikacje sieci web szybkie, niezawodne i spersonalizowane, niezależnie od przeglądarki, lokalizacji, urządzenia lub sieci. Jednak bardzo innowacje, wchodzące w tych środowisk, dlatego współpraca również powolna strona pliki do pobrania i zagrożenie jakości obsługi klienta. 

Możliwość sieci CDN w warstwie standardowa obejmuje możliwość przyspieszenia dostarczanie plików statycznych do pamięci podręcznej plików bliżej dla użytkowników końcowych. Jednak z dynamicznych aplikacji sieci web, buforowanie tej zawartości w lokalizacjach edge nie jest możliwe, ponieważ serwer generuje zawartość w odpowiedzi na zachowanie użytkownika. Przyspieszenia dostarczania zawartości takie jest bardziej złożone niż tradycyjne krawędzi buforowanie i wymaga precyzyjne Dostraja każdego elementu w ścieżce danych od chwili rozpoczęcia dostarczać rozwiązanie end-to-end. Z usługi Azure CDN dynamiczne lokacji Acceleration (DSA), jest widoczny zwiększenie wydajności stron sieci web z zawartością dynamiczną.

Azure CDN from Akamai i Verizon oferuje DSA optymalizacji za pośrednictwem **zoptymalizowane pod kątem** menu podczas tworzenia punktu końcowego.

## <a name="configuring-cdn-endpoint-to-accelerate-delivery-of-dynamic-files"></a>Konfigurowanie punktu końcowego CDN w celu przyspieszenia dostarczania plików dynamicznych

Można skonfigurować punktu końcowego CDN do optymalizacji dostarczania dynamiczne pliki za pomocą portalu Azure, wybierając **przyspieszenie dynamiczne witryny** opcję w obszarze **zoptymalizowane pod kątem** właściwości wyboru podczas tworzenia punktu końcowego. Aby programowo tak samo postąpić, można użyć naszych interfejsów API REST lub dowolny z zestawów SDK klienta. 

### <a name="probe-path"></a>Ścieżka sondy
Ścieżka sondy jest funkcja specyficzna dla dynamicznej przyspieszenie lokacji i prawidłowego jest wymagana do utworzenia. DSA używa małą *ścieżki sondowania* plik umieszczony na początek w celu zoptymalizowania routingu konfiguracje sieci CDN. Można pobierać i przekaż naszego przykładowego pliku do witryny lub użyć istniejącego zasobu DS, który jest około 10 KB dla ścieżki sondowania zamiast tego, jeśli istnieje zasób.

> [!Note]
> DSA wiąże się z dodatkowych opłat. Aby uzyskać więcej informacji, zobacz [cennikiem](https://azure.microsoft.com/pricing/details/cdn/) Aby uzyskać więcej informacji.

Poniższe zrzuty ekranu przedstawiają procesu za pomocą portalu Azure.
 
![Dodawanie nowego punktu końcowego CDN](./media/cdn-dynamic-site-acceleration/01_Endpoint_Profile.png) 

*Rysunek 1: Dodawanie nowy punkt końcowy CDN z profilu CDN*
 
![Tworzenie nowego punktu końcowego CDN za pomocą algorytmu DSA](./media/cdn-dynamic-site-acceleration/02_Optimized_DSA.png)  

*Rysunek 2: Tworzenie punktu końcowego usługi CDN za pomocą przyspieszenie lokacji dynamicznej optymalizacji wybrane*

Po utworzeniu punktu końcowego CDN ma to zastosowanie optymalizacje DSA dla wszystkich plików spełniających określone kryteria. W poniższej sekcji opisano optymalizacji DSA szczegółowo.

## <a name="dsa-optimization-using-azure-cdn"></a>Optymalizacja DSA za pomocą usługi Azure CDN

Dynamiczne przyspieszenie witryny na platformie Azure CDN przyspiesza dostarczania dynamicznych zasobów przy użyciu następujących metod:

-   Optymalizacja trasy
-   Optymalizacje TCP
-   Pobieranie z wyprzedzeniem obiektów (tylko Akamai)
-   Kompresja obrazu przenośnych (tylko Akamai)

### <a name="route-optimization"></a>Optymalizacja trasy

Optymalizacja trasy jest ważne, ponieważ Internet jest dynamiczne miejsce, w którym ruch i tymczasowo awarie są ciągle zmieniana topologii sieci. Protokół BGP (Border Gateway) jest protokołem routingu z Internetu, ale może to być szybsze tras za pomocą pośredniczącej serwerów punktu obecności (PoP). 

Optymalizacja trasy wybiera optymalny ścieżka do źródła, aby witryna jest stale dostępny i dynamicznej zawartości jest dostarczany do użytkowników końcowych za pomocą trasy najszybsze i najbardziej niezawodnym możliwe. 

Sieć Akamai używa techniki do zbierania danych w czasie rzeczywistym i porównanie różnych ścieżek za pomocą różnych węzłach w serwerze Akamai, a także trasy protokołu BGP domyślnej różnych Otwórz Internet w celu określenia najszybszy trasy od źródła i krawędzi sieci CDN. Te techniki uniknąć internetowych punktów przeciążenia i długi trasy. 

Podobnie sieci Verizon używa kombinacji emisji DNS, dużej pojemności obsługuje POP i kontroli kondycji ustalenie najlepszych bramy do najważniejsze dane trasy z klienta do źródła.
 
W związku z tym pełni dynamicznych i transakcyjnego dostarczania zawartości szybciej i bardziej niezawodnie dla użytkowników końcowych, nawet wtedy, gdy jest uncacheable. 

### <a name="tcp-optimizations"></a>Optymalizacje TCP

Transmission Control Protocol (TCP) jest standardem zestawu protokołów internetowych, używanych do ich dostarczania informacji między aplikacjami sieci IP.  Domyślnie jest kilka i z powrotem żądań wymagane do konfigurowania połączeń TCP, jak również limity w celu uniknięcia congestions sieci, które powodują wydajność na dużą skalę. Ten problem dotyczy usługi Azure CDN from Akamai optymalizując w trzech obszarach: 

 - Wyeliminowanie start powolne
 - Korzystanie z usług połączeń trwałych
 - dostrajanie parametrów pakietów TCP (tylko Akamai)

#### <a name="eliminating-slow-start"></a>Wyeliminowanie start powolne

*Powolna start* jest częścią protokołu TCP, który uniemożliwia przeciążenie sieci, ograniczając ilość danych przesyłanych przez sieć. Rozpoczyna się od przeciążenia mały rozmiar okna między nadawcą i odbiorcą aż do osiągnięcia maksymalnej lub wykrycia utraty pakietów.

Azure CDN from Akamai i Verizon eliminuje powolne rozpocznie się za trzy kroki:

1.  Zarówno Akamai i Verizon przez sieć umożliwia przepustowości monitorowania kondycji i pomiar przepustowości połączeń między serwerami PoP krawędzi.
2. Metryki są współużytkowane serwery krawędzi PoP, aby każdy serwer zdaje się warunków sieciowych i kondycji serwera punkty obecności wokół nich.  
3. Serwery krawędzi CDN są teraz możliwość założenia dotyczące niektórych parametrów transmisji, na przykład co optymalnego rozmiaru okna powinna być podczas komunikowania się z innymi serwerami krawędzi sieci CDN w jego pobliżu. Ten krok oznacza, że przeciążenia początkowy rozmiar okna można zwiększyć, jeśli kondycję połączenia między serwerami krawędzi CDN jest w stanie wyższej transferów danych pakietów.  

#### <a name="leveraging-persistent-connections"></a>Korzystanie z usług połączeń trwałych

Za pomocą CDN, mniejszej liczby unikatowych maszyn podłączyć się do serwera pochodzenia bezpośrednio w porównaniu z użytkowników łączących się bezpośrednio do źródła. Azure CDN from Akamai i Verizon puli również żądań użytkowników ze sobą w celu nawiązywania połączeń mniej z punktu początkowego.

Jak wspomniano wcześniej, połączeń TCP zająć wiele żądań i z powrotem w uzgadniania nawiązać nowe połączenie. Połączenia trwałe, znanej także jako "HTTP Keep-Alive," ponowne użycie istniejących połączeń TCP dla wielu żądań HTTP zapisać czasami opóźnienia i przyspieszyć proces dostarczania. 

Sieci Verizon wysyła również okresowe pakiety utrzymywania aktywności za pośrednictwem połączenia TCP, aby zapobiec Otwieranie połączenia z zamknięte.

#### <a name="tuning-tcp-packet-parameters"></a>Dostrajanie parametrów pakietów TCP

Usługi Azure CDN from Akamai również osadzonych utworów parametry połączenia do serwera zarządzania i zmniejsza ilość długi zasięg wymaganą liczbę przekazywanych do pobierania zawartości w lokacji przy użyciu następujących metod:

1.  Zwiększanie początkowej przeciążenia okna tak, aby więcej pakietów mogą być wysyłane bez oczekiwania na potwierdzenie.
2.  Zmniejszenie limitu czasu początkowej retransmisji wykrycia straty, a retransmisji występuje szybciej.
3.  Zmniejszenie limitu czasu retransmisji minimalną i maksymalną, aby skrócić czas oczekiwania przed przy założeniu, że pakiety zostały utracone podczas transmisji.

### <a name="object-prefetch-akamai-only"></a>Pobieranie z wyprzedzeniem obiektów (tylko Akamai)

Większość witryn sieci Web składają się z strony HTML, który odwołuje się do różnych zasobów takich jak obrazy czy skrypty. Zazwyczaj, gdy klient zażąda strony sieci Web, przeglądarki najpierw pobiera i analizuje obiektu HTML, a następnie tworzy dodatkowych żądań do połączonych zasobów, które są wymagane do pełni załadowania strony. 

*Pobieranie z wyprzedzeniem* jest technika, którą można pobrać obrazów i skrypty osadzone w strony HTML podczas HTML jest obsługiwane w przeglądarce, ale przed przeglądarki nawet zgłasza żądania dotyczące tych obiektów. 

Z **pobrana z wyprzedzeniem** włączona opcja w czasie, gdy służy sieci CDN w warstwie podstawowa HTML strony przeglądarki klienta, sieć CDN analizuje pliku w formacie HTML i dodatkowych żądań Sprawdź dla każdego połączonych zasobów i zapisz go w pamięci podręcznej. Gdy klient wysyła żądania dla połączonych zasobów, serwer graniczny CDN już ma żądane obiekty i może obsługiwać je bezpośrednio bez podróż do źródła. Optymalizacja przynosi korzyści zarówno buforowalnej, jak i buforowalnej zawartości.

### <a name="adaptive-image-compression-akamai-only"></a>Kompresja adaptacyjną obrazu (tylko Akamai)

Niektóre urządzenia, szczególnie przenośnych te wystąpić niższych szybkościach sieci od czasu do czasu. W tych scenariuszach jest bardziej korzystne dla użytkownika szybciej otrzymać mniejszych obrazów na ich strony sieci Web nie czekać zbyt długo dla obrazów pełnej rozdzielczości.

Funkcja ta automatycznie monitoruje jakości sieci i wykorzystuje standardowe metody kompresji JPEG, gdy szybkości sieci są wolniejszej zwiększające czas dostarczenia.

Kompresja adaptacyjną obrazu | Rozszerzenia plików  
--- | ---  
Kompresja JPEG | jpg, JPEG, jpe, .jig, .jgig, .jgi

## <a name="caching"></a>Buforowanie

Z DSA buforowanie jest domyślnie wyłączona w sieci CDN, nawet wtedy, gdy punkt początkowy zawiera nagłówki pamięci podręcznej formant/wygasa w odpowiedzi. To ustawienie domyślne jest wyłączona, ponieważ DSA jest zazwyczaj używana w przypadku dynamicznego zasoby, które nie mają być buforowane, ponieważ są one unikatowe dla każdego klienta, a następnie włączenie buforowania domyślnie mogą być dzielone to zachowanie.

Jeśli masz witrynę z różnymi statycznych i dynamicznych zasobów, najlepiej zająć rozwiązanie hybrydowe, aby uzyskać najlepszą wydajność. 

Jeśli używasz sieci ADN z Verizon Premium, możesz włączyć buforowanie ponownie dla określonych przypadków przy użyciu aparatu reguł.  

Alternatywą jest użyć dwóch punktów końcowych usługi CDN. Jeden z DSA dostarczać zasoby dynamiczne i inny punkt końcowy typu statycznego optymalizacji, takie jak dostarczanie ogólne sieci web, do dostarczania buforowalnej zasobów. Aby osiągnąć to alternatywne, należy zmodyfikować adresami URL strony sieci Web bezpośrednio do elementu zawartości w punkcie końcowym CDN, który ma być używany. 

Na przykład: `mydynamic.azureedge.net/index.html` stronę dynamicznego i jest załadowany z punktu końcowego DSA.  Strony html odwołuje się do wielu zasobów statycznych, takich jak biblioteki języka JavaScript lub obrazów, które są ładowane z statycznego punktu końcowego usługi CDN, takich jak `mystatic.azureedge.net/banner.jpg` i `mystatic.azureedge.net/scripts.js`. 

Przykład można znaleźć [tutaj](https://docs.microsoft.com/azure/cdn/cdn-cloud-service-with-cdn#controller) dotyczące sposobu używania kontrolerów w aplikacji sieci web ASP.NET do obsługi zawartości przy użyciu określonego adresu URL usługi CDN.




