<properties
    pageTitle="Omówienie usługi Azure CDN"
    description="Dowiedz się, co to jest usługa Azure Content Delivery Network (CDN) i jak z niej korzystać w celu dostarczania zawartości wysokiej przepustowości przez buforowanie obiektów blob i zawartości statycznej."
    services="cdn"
    documentationCenter=".NET"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="06/06/2016"
    ms.author="casoper"/>

# Omówienie usługi Azure Content Delivery Network (CDN)

> [AZURE.NOTE] W tym dokumencie opisano, co to jest i jak działa usługa Azure Content Delivery Network (CDN) oraz funkcje każdego produktu Azure CDN.  Jeśli chcesz pominąć te informacje i przejść od razu do samouczka na temat tworzenia punktu końcowego usługi CDN, zobacz [Korzystanie z usługi CDN](cdn-create-new-endpoint.md).  Jeśli chcesz wyświetlić listę bieżących lokalizacji węzłów CDN, zobacz [Lokalizacje POP usługi Azure CDN](cdn-pop-locations.md).

Usługa Azure Content Delivery Network (CDN) buforuje zawartość statyczną sieci Web w strategicznie rozmieszczonych lokalizacjach w celu zapewnienia maksymalnej przepływności dostarczania zawartości dla użytkowników.  Usługa CDN oferuje deweloperom globalne rozwiązanie umożliwiające dostarczanie zawartości o wysokiej przepustowości przez zapisywanie zawartości w pamięci podręcznej w węzłach fizycznych na całym świecie. 

Zalety używania usługi CDN do buforowania zasobów witryny sieci Web obejmują:

- Lepszą wydajność i środowisko dla użytkowników końcowych, zwłaszcza w przypadku korzystania z aplikacji, w których wymagane jest wiele rund do załadowania zawartości.
- Duże skalowanie, aby lepiej obsługiwać natychmiastowe wysokie obciążenie, na przykład na początku zdarzenia uruchamiania produktu.
- Dystrybucję żądań użytkowników i obsługę zawartości z serwerów krawędzi, dzięki czemu mniej ruchu jest wysyłane do punktu początkowego.


## Jak to działa

![Omówienie usługi CDN](./media/cdn-overview/cdn-overview.png)

1. Użytkownik (Anna) żąda pliku (nazywanego również zasobem) przy użyciu adresu URL ze specjalną nazwą domeny, taką jak `<endpointname>.azureedge.net`.  System DNS kieruje żądanie do najlepiej działającej lokalizacji punktu obecności (POP, Point-of-Presence).  Zazwyczaj jest to punkt obecności znajdujący się geograficznie najbliżej użytkownika.

2. Jeśli serwery krawędzi w lokalizacji POP nie mają pliku w swojej pamięci podręcznej, serwer krawędzi żąda pliku z punktu początkowego.  Punktem początkowym może być aplikacja sieci Web platformy Azure, usługa Azure Cloud Service, konto usługi Azure Storage lub dowolny publicznie dostępny serwer sieci Web.

3. Punkt początkowy zwraca plik do serwera krawędzi, dołączając opcjonalne nagłówki HTTP opisujące czas wygaśnięcia (TTL, Time-to-Live) pliku.

4. Serwer krawędzi zapisuje plik w pamięci podręcznej i zwraca go do oryginalnego obiektu żądającego (Anna).  Plik pozostaje w pamięci podręcznej na serwerze krawędzi do momentu upłynięcia czasu wygaśnięcia (TTL).  Jeśli punkt początkowy nie określił wartości TTL, domyślny czas wygaśnięcia wynosi 7 dni.

5. Dodatkowi użytkownicy (na przykład Piotr) mogą następnie zażądać tego samego pliku przy użyciu tego samego adresu URL, a także mogą być kierowani do tego samego punktu obecności.

6. Jeśli czas wygaśnięcia pliku nie upłynął, serwer krawędzi zwraca plik z pamięci podręcznej.  Skutkuje to szybszymi czasami reakcji w środowisku użytkownika.


## Funkcje usługi Azure CDN

Istnieją trzy produkty Azure CDN: **Azure CDN Standard from Akamai**, **Azure CDN Standard from Verizon** i **Azure CDN Premium from Verizon**.  W tabeli poniżej wymieniono funkcje dostępne w poszczególnych produktach.

|       | Standard Akamai | Standard Verizon | Premium Verizon |
|-------|-----------------|------------------|-----------------|
| Łatwa integracja z usługami platformy Azure, takimi jak [Storage](cdn-create-a-storage-account-with-cdn.md), [Cloud Services](cdn-cloud-service-with-cdn.md), [Web Apps](../app-service-web/cdn-websites-with-cdn.md) i [Media Services](../media-services/media-services-manage-origins.md#enable_cdn) | **&#x2713;** | **&#x2713;** | **&#x2713;**|
| Obsługa protokołu HTTPS | **&#x2713;** | **&#x2713;** | **&#x2713;** |
| Równoważenie obciążenia | **&#x2713;** | **&#x2713;** | **&#x2713;** |
| Ochrona przed atakami DDoS | **&#x2713;** | **&#x2713;** | **&#x2713;** |
| Podwójny stos protokołów IPv4/IPv6 | **&#x2713;** | **&#x2713;** | **&#x2713;** |
| [Obsługa niestandardowych nazw domen](cdn-map-content-to-custom-domain.md) | **&#x2713;** | **&#x2713;** | **&#x2713;** |
| [Buforowanie ciągu zapytania](cdn-query-string.md) | **&#x2713;** | **&#x2713;** | **&#x2713;** |
| [Filtrowanie kraju](cdn-restrict-access-by-country.md) |  | **&#x2713;** | **&#x2713;** |
| [Szybkie przeczyszczanie](cdn-purge-endpoint.md) | **&#x2713;** | **&#x2713;** | **&#x2713;** |
| [Wstępne ładowanie zasobów](cdn-preload-endpoint.md) |  | **&#x2713;** | **&#x2713;** |
| [Podstawowa analiza](cdn-analyze-usage-patterns.md) |  | **&#x2713;** | **&#x2713;** |
| [Zarządzanie za pomocą interfejsu API REST](https://msdn.microsoft.com/library/mt634456.aspx) | **&#x2713;** | **&#x2713;** | **&#x2713;** |
| [Dostosowywalny, oparty na regułach aparat dostarczania zawartości](cdn-rules-engine.md) | | | **&#x2713;** |
| [Zaawansowane raporty HTTP](cdn-advanced-http-reports.md) | | | **&#x2713;** |
| [Statystyki w czasie rzeczywistym](cdn-real-time-stats.md) | | | **&#x2713;** |


## Następne kroki

Aby rozpocząć pracę z usługą CDN, zobacz [Korzystanie z usługi Azure CDN](./cdn-create-new-endpoint.md).

Jeśli jesteś istniejącym klientem usługi CDN, teraz możesz zarządzać punktami końcowymi usługi CDN za pomocą [portalu Microsoft Azure](https://portal.azure.com).

Aby zobaczyć usługę CDN w akcji, obejrzyj [wideo z sesji konferencji Build 2016](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Aby uzyskać informacje o cenach, zobacz [cennik usługi CDN](https://azure.microsoft.com/pricing/details/cdn/).

Czy jest jakaś funkcja, z której chcesz korzystać w usłudze Azure CDN?  [Prześlij nam swoją opinię](https://feedback.azure.com/forums/169397-cdn)! 



<!--HONumber=Jun16_HO2-->


