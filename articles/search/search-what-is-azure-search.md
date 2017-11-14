---
title: "Co to jest usługa Azure Search | Dokumentacja firmy Microsoft"
description: "Wyszukiwanie Azure jest usługą wyszukiwania pełni zarządzane chmury hostowanej. Dowiedz się więcej informacji, zobacz Omówienie tej funkcji."
services: search
manager: jhubbard
author: HeidiSteen
documentationcenter: 
ms.assetid: 50bed849-b716-4cc9-bbbc-b5b34e2c6153
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 11/10/2017
ms.author: heidist
ms.openlocfilehash: 63c7bcc1bf4e650f913d31e5687c31257a85bfee
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2017
---
# <a name="what-is-azure-search"></a>Co to jest usługa Azure Search?
Usługa Azure Search jest rozwiązaniem chmury wyszukiwanie jako usługa, która zapewnia deweloperom interfejsów API i narzędzia do dodawania obsługi wyszukiwania sformatowanego za pośrednictwem zawartości w aplikacjach sieci web, mobilnych i enterprise.

Funkcje są dostępne za pośrednictwem prostą [interfejsu API REST](/rest/api/searchservice/) lub [zestawu .NET SDK](search-howto-dotnet-sdk.md) który maskuje złożoność związanego z używaniem pobierania informacji. Oprócz interfejsów API portalu Azure umożliwia obsługę administracji i zarządzania zawartością, narzędzia prototypowania i badania z indeksów. Ponieważ usługa jest uruchamiana w chmurze, infrastruktury i dostępności są zarządzane przez firmę Microsoft.

<a name="feature-drilldown"></a>

## <a name="feature-summary"></a>Podsumowanie funkcji

| Kategoria | Funkcje |
|----------|----------|
|Wyszukiwanie pełnotekstowe i analiza tekstu | [Wyszukiwanie pełnotekstowe](search-lucene-query-architecture.md) jest podstawowym przypadek użycia dla większości aplikacji na podstawie wyszukiwania. Zapytania można sformułować za pomocą obsługiwanych składni. <br/><br/>[**Prosta składnia zapytań** ](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) zawiera operatorów logicznych, frazy operatorami wyszukiwania, Operatorzy sufiks, pierwszeństwo operatorów.<br/><br/>[**Składnia zapytań Lucene** ](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) obejmuje wszystkie operacje w prosty składni z rozszerzeniami dla Wyszukiwanie rozmyte, wyszukiwanie w sąsiedztwie termin zwiększania i wyrażenia regularne.| 
| Integracja danych | Azure indeksy wyszukiwania zaakceptować danych z dowolnego źródła, pod warunkiem jest przesyłany w strukturze danych JSON. <br/><br/> Opcjonalnie, dla obsługiwanych źródeł danych na platformie Azure, możesz użyć [ **indeksatory** ](search-indexer-overview.md) automatycznie przeszukiwać [bazy danych SQL Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md), [bazy danych Azure rozwiązania Cosmos](search-howto-index-documentdb.md), lub [magazynu obiektów Blob Azure](search-howto-indexing-azure-blob-storage.md) Synchronizacja indeksu wyszukiwania elementu zawartości o magazynie danych podstawowych. Indeksatory obiektów Blob platformy Azure można wykonać *łamania dokumentu* dla [indeksowania głównych formatach](search-howto-indexing-azure-blob-storage.md), łącznie z dokumentów Microsoft Office, plików PDF i HTML. |
| Językową analiza | Analizatory są składnikami używanej w tekście przetwarzania podczas operacji indeksowania i wyszukiwania. Istnieją dwa typy. <br/><br/>[**Niestandardowe analizatorów leksykalne** ](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) są używane dla zapytania wyszukiwania złożonych przez dopasowanie fonetycznych i wyrażenia regularne. <br/><br/>[**Analizatorów języka** ](https://docs.microsoft.com/rest/api/searchservice/language-support) Lucene lub Microsoft są używane do obsługi inteligentnie czasowników, płci, nieregularne rzeczowniki mnogiej (na przykład "myszy" a "myszy"), programu word do łączenia, w tym linguistics specyficzne dla języka Dzielenie wyrazów (dla języków, bez spacji) i inne. |
| Wyszukiwanie Geo | Usługa Azure Search przetwarza filtrów i wyświetla lokalizacje geograficzne. Umożliwia użytkownikom eksplorować dane oparte na bliskości wynik wyszukiwania lokalizacji fizycznej. [Obejrzyj ten film](https://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) lub [Przejrzyj ten przykład](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) Aby dowiedzieć się więcej. |
| Funkcje środowiska użytkownika | [**Sugestie dotyczące wyszukiwania** ](https://docs.microsoft.com/rest/api/searchservice/suggesters) można włączyć Autouzupełnianie zapytań w pasku wyszukiwania. Rzeczywiste dokumenty w indeksie sugerowane użytkowników wprowadzania danych wejściowych z częściowa wyszukiwania. <br/><br/>[**Nawigacji aspektowej** ](https://docs.microsoft.com/azure/search/search-faceted-navigation) jest włączana za pomocą parametru pojedynczego zapytania. Usługa Azure Search zwraca strukturze nawigacji aspektowej służący jako kodzie listę kategorii w celu samodzielnego filtrowania (na przykład do elementów katalogu filtru zakresu cen lub marki). <br/><br/> [**Filtry** ](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) można zastosować nawigacji aspektowej w interfejsie użytkownika aplikacji, zwiększyć formułowanie zapytania i na podstawie określonych przez użytkownika lub dewelopera kryteria filtrowania. Tworzenie filtrów przy użyciu składni OData.<br/><br/> [**Wyróżnianie trafień** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) stosuje formatowanie do słowo kluczowe w wynikach wyszukiwania. Możesz wybrać pola, które zwraca zaznaczony fragmentów.<br/><br/>[**Sortowanie** ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) jest dostępna w przypadku wielu pól za pomocą schematu indeksu i następnie przełączane na etapie zapytania z pojedynczy parametr.<br/><br/> [**Stronicowanie** ](search-pagination-page-layout.md) i ograniczania wyników wyszukiwania jest bezpośrednia z precyzyjne Zaczekaj formant, który udostępnia usługi Azure Search w wynikach wyszukiwania.  
| Trafność | [**Proste oceniania** ](/rest/api/searchservice/add-scoring-profiles-to-a-search-index) jest ważnym elementem usługi Azure Search. Profile oceniania służą do modelu znaczenie w zależności od wartości w same dokumenty. Na przykład może być nowsza produktów lub obniżonej cenie produktów są wyświetlane w wynikach wyszukiwania. Można również tworzyć profile oceniania przy użyciu tagów spersonalizowanych wyników na podstawie preferencji wyszukiwania klienta już śledzone i przechowywane w innej lokalizacji. |
| Monitorowanie i raportowanie | [**Wyszukaj analizy ruchu** ](search-traffic-analytics.md) są gromadzone i analizować je w celu odblokowania wgląd w dane dotyczące co użytkownicy są wpisane w polu wyszukiwania. <br/><br/>Metryki dotyczące zapytań na sekundę, opóźnienia i ograniczania przechwycenie i zgłaszane na stronach portalu nie konieczności dodatkowej konfiguracji. Można też łatwo indeks monitor i dokument zlicza tak, aby w razie potrzeby można dostosować pojemności. Aby uzyskać więcej informacji, zobacz [obsługi administracyjnej](search-manage.md) |
| Narzędzia prototypowania i inspekcji | W portalu, można użyć [ **Kreatora importu danych** ](search-import-data-portal.md) indeksatorów, Projektant indeksu pozostawić indeksu, konfigurowania i [ **Eksplorator wyszukiwania** ](search-explorer.md)do testowania zapytania i Dostosuj oceniania profilów. Można również otworzyć indeksu do wyświetlenia jego schematu. |
| Infrastruktura | **Wysokiej dostępności platformy** zapewnia bardzo niezawodnej usługi wyszukiwanie. Podczas skalowania poprawnie, [usługi Azure Search udostępnia SLA 99,9%](https://azure.microsoft.com/support/legal/sla/search/v1_0/).<br/><br/> **W pełni zarządzany i skalowalne** jako rozwiązaniem na trasie usługi Azure Search wymaga absolutnie nie zarządzanie infrastrukturą. Usługi można dostosowane do potrzeb użytkownika przez skalowanie w dwóch wymiarów dokumentu miejsca do magazynowania, wyższy ładowania zapytań, lub obie.

## <a name="how-to-use-azure-search"></a>Jak używać usługi Azure Search
### <a name="step-1-provision-service"></a>Krok 1: Świadczenia usługi
Można pokrętła usługi Azure Search w [portalu Azure](https://portal.azure.com/) lub za pomocą [interfejsu API usługi Azure Resource Management](/rest/api/searchmanagement/). Można wybrać bezpłatnej usługi udostępniane innym subskrybentów lub [płatnej warstwy](https://azure.microsoft.com/pricing/details/search/) który rezerwuje zasoby używane tylko przez usługę. Warstw płatnych w dwóch wymiarów można skalować usługi: 

- Dodaj replik zwiększa wydajność do obsługi obciążeń duże zapytania.   
- Dodawanie partycji można powiększać magazyn dla kolejnych dokumentów. 

Dzięki obsłudze przepływności magazynu i zapytania dokumentu oddzielnie, można kalibrować pozyskiwania na podstawie wymagań produkcji.

### <a name="step-2-create-index"></a>Krok 2: Tworzenie indeksu
Zanim można przekazać zawartości wyszukiwanie, należy najpierw zdefiniować indeksu usługi Azure Search. Indeks przypomina tabeli bazy danych, która przechowuje dane i może zaakceptować zapytania wyszukiwania. Definiowania schematu indeksu, aby mapować do uwzględnienia struktury dokumentów, który chcesz przeszukać, podobne do pól w bazie danych.

Schemat może być utworzone w portalu Azure lub programistycznie przy użyciu [zestawu .NET SDK](search-howto-dotnet-sdk.md) lub [interfejsu API REST](/rest/api/searchservice/).

### <a name="step-3-index-data"></a>Krok 3: Dane indeksu
Po zdefiniowaniu indeksu, możesz przekazać zawartość. Można użyć modelu wypychania i ściągania.

Model polegający na ściąganiu pobiera dane z zewnętrznych źródeł danych. Jest on obsługiwany przez *indeksatory* usprawnić i zautomatyzować aspektów wprowadzanie danych, takich jak połączenie, odczytywanie i serializacja danych. [Indeksatory](/rest/api/searchservice/Indexer-operations) są dostępne dla bazy danych Azure rozwiązania Cosmos, baza danych SQL Azure, magazyn obiektów Blob Azure i SQL Server w maszynie Wirtualnej platformy Azure. Możesz skonfigurować indeksator dla na żądanie lub zaplanowane odświeżanie danych.

Model wypychania jest zapewniana za pomocą zestawu SDK lub interfejsów API REST, używany do wysyłania zaktualizowane dokumenty do indeksu. Można wypychać dane z niemal dowolnego zestawu danych w formacie JSON. Zobacz [Add, update lub delete dokumentów](/rest/api/searchservice/addupdate-or-delete-documents) lub [sposób użycia zestawu .NET SDK)](search-howto-dotnet-sdk.md) wskazówki dotyczące ładowania danych.

### <a name="step-4-search"></a>Krok 4: wyszukiwania
Po wypełnieniu indeksu, możesz [wysyłania zapytań wyszukiwania](/rest/api/searchservice/Search-Documents) do punktu końcowego usługi za pomocą prostego żądania HTTP z interfejsu API REST lub zestawu .NET SDK.

## <a name="how-azure-search-compares"></a>Jak porównanie usługi Azure Search

Klienci często dotyczy, jak usługa Azure Search porównuje z innych rozwiązań dotyczących wyszukiwania. Poniższa tabela zawiera podsumowanie podstawowych różnic.

| W porównaniu do | Podstawowe różnice |
|--|--|
|Bing | [Interfejsu API Bing sieci Web wyszukiwania](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/) wyszukuje indeksów Bing.com pasujących terminów przesyłania. Indeksy są tworzone z kodu HTML, XML i innej zawartości sieci web w publicznych witrynach. [Wyszukiwania usługi Bing niestandardowe](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/) oferuje typów zawartości, ograniczone do indywidualnych witryn sieci web z tej samej technologii przez przeszukiwarkę sieci Web.<br/><br/>Usługa Azure Search wyszukuje indeks zdefiniowane przez użytkownika, wypełniane przy użyciu danych i dokumentów, którego jesteś właścicielem, często z różnych źródeł. Wyszukiwanie Azure ma funkcje przeszukiwarki dla niektórych źródeł danych za pośrednictwem [indeksatory](search-indexer-overview.md), ale możesz wypchnąć dowolnych dokumentów JSON, zgodny schemat indeksu do jednego, skonsolidowanego zasobów można wyszukiwać. |
|Wyszukiwanie w bazie danych | [Wyszukiwanie pełnotekstowe programu SQL Server](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) jest wewnętrzny dla systemu DBMS w tabelach SQL zawartości. <br/><br/>Wyszukiwanie Azure przechowuje zawartość ze źródeł heterogenicznych i oferuje tekst specjalnych funkcji przetwarzania, takich jak analiza językowe i niestandardowych. [Aparat wyszukiwania pełnotekstowego](search-lucene-query-architecture.md) w usłudze Azure Search jest zbudowany na Apache Lucene standardem w pobierania informacji. <br/><br/>Użycie zasobów jest inny punkt modulacją. Wyszukiwanie w języku naturalnym jest często znacznym w praktyce. Odciążanie wyszukiwania dedykowane rozwiązania zachowuje zasoby do przetworzenia transakcji. Przez jego oddzielenie wyszukiwania można łatwo dostosować skalowania, aby dopasować woluminu zapytania.|
|Wyszukiwanie dedykowane rozwiązania | W lokalnej lub w chmurze rozwiązań usług są wyszukiwania dedykowane rozwiązania z pełnego zakresu funkcjonalności. Technologie wyszukiwania zwykle oferować kontrolę nad potoków indeksowanie i kwerendy, dostęp do bardziej zaawansowane funkcje zapytań i filtrowanie składni, kontrolę nad rangę oraz przydatności i funkcji samodzielnego i inteligentnych wyszukiwania. <br/><br/>Można znaleźć rozwiązania dedykowane wyszukiwania oferowane w chmurze usługi lub jako autonomiczny serwer hostowana na lokalnym lub maszynie wirtualnej. Usługi w chmurze jest właściwie, jeśli chcesz [gotowe rozwiązanie przy minimalnym nakładzie kosztów i konserwacji i skali regulowany](#cloud-service-advantage). <br/><br/>W ramach modelu chmury wielu dostawców oferują funkcje można porównywać pod względem linii bazowej, wyszukiwanie pełnotekstowe, geograficznie wyszukiwania i zdolności do obsługi określonego poziomu niejednoznaczności w danych wejściowych wyszukiwania. Zwykle ma [specjalnych funkcji](#feature-drilldown), lub ułatwienia i uproszczenia ogólnego interfejsów API, narzędzi i zarządzania, który określa najlepszego dopasowania. |

Wśród dostawców w chmurze usługi Azure Search jest najwyższego poziomu dla obciążeń wyszukiwania pełnotekstowego w zawartości magazynów i baz danych na platformie Azure, w przypadku aplikacji, które opierają się głównie na wyszukiwania dla pobierania informacji i nawigację po zawartości. Sile klucza obejmują:

+ Integracja danych Azure (przeszukiwarek) w warstwie indeksowania
+ Portal Azure centralnego zarządzania
+ Azure skalowania, niezawodności i dostępności światowej klasy
+ Niestandardowe i językowej analizy ze analizatorów wyszukiwania pełnotekstowego stałe w 56 językach
+ [Podstawowe funkcje wspólne dla skoncentrowane na wyszukiwanie aplikacji](#feature-drilldown): oceniania, tworzenie aspektów, sugestii, synonimy, geograficznie wyszukiwania i inne.

> [!Note]
> Źródła danych inne niż Azure są w pełni obsługiwane, ale zależą od metodologii wypychania bardziej intensywnie dla kodu, a nie indeksatorów. Za pomocą interfejsów API, można przekazać żadnych kolekcji dokumentów JSON do indeksu usługi Azure Search.

Między klientami tych, które mogą skorzystać z szeroką gamą funkcji w usłudze Azure Search zawiera katalogi online, programy — biznesowych i odnajdywania dokumenty i aplikacje.

## <a name="rest-api--net-sdk"></a>Interfejs API REST | .net SDK

Natomiast wielu zadań, które może zostać przeprowadzone w portalu, usługi Azure Search jest przeznaczony dla deweloperów, którzy zamierzają integrować funkcji wyszukiwania do istniejących aplikacji. Dostępne są następujące interfejsy programowania.

|Platforma |Opis |
|-----|------------|
|[REST](/rest/api/searchservice/) | Polecenia HTTP obsługiwane przez wszystkie platformy programowania i język, w tym Xamarin, Java i JavaScript|
|[Zestaw SDK platformy .NET](search-howto-dotnet-sdk.md) | .NET otoki dla interfejsu API REST oferuje efektywne kodowanie w C# i innych języków kodu zarządzanego docelowy program .NET Framework |

## <a name="free-trial"></a>Bezpłatna wersja próbna
Subskrybenci platformy Azure można [alokowanie usługi w warstwie bezpłatna](search-create-service-portal.md).

Jeśli nie masz subskrybenta, możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Otrzymasz kredyt do wypróbowania płatnych usług Azure. Po wyczerpaniu, można zachować konto i korzystać z [bezpłatnych usług platformy Azure](https://azure.microsoft.com/free/). Karta kredytowa nie jest obciążona, chyba że jawnie zmienić ustawienia i poprosisz o jej obciążenie.

Można też [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): subskrypcji Your MSDN otrzymasz kredyt, co miesiąc, używanego programu płatnych usług Azure. 

## <a name="how-to-get-started"></a>Jak zacząć

1. Tworzenie usługi w [warstwę bezpłatna](search-create-service-portal.md).

2. Kroków opisanych w co najmniej jeden z następujących samouczków. 

  + [Jak używać zestawu .NET SDK](search-howto-dotnet-sdk.md) przedstawiono podstawowe kroki w kodzie zarządzanym.  
  + [Wprowadzenie do interfejsu API REST](https://github.com/Azure-Samples/search-rest-api-getting-started) zawiera te same czynności, za pomocą interfejsu API REST.  
  + [Utworzyć pierwszy indeks w portalu](search-get-started-portal.md) przy użyciu wbudowanych funkcji indeksowanie i prototypu.   

Aparaty wyszukiwania są typowe sterowniki pobierania informacji w aplikacjach mobilnych, w sieci web i w magazynach danych firmowych. Usługa Azure Search udostępnia narzędzia do tworzenia środowiska wyszukiwania podobne do tych na dużych witryn sieci web.

Dowiedz się, w tym 9-minutowy film wideo z Menedżera programów Liam Cavanagh, integrowanie z wyszukiwarki korzyści aplikacji. Krótkie pokazy opisano najważniejsze funkcje usługi Azure Search i jak wygląda Typowy przepływ pracy. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0 – 3 min omawia kluczowe funkcje i przypadki użycia.
+ 3 4 minut obejmuje Inicjowanie obsługi usługi. 
+ 4 – 6 min obejmuje Kreatora importu danych używany do tworzenia indeksu za pomocą wbudowanych nieruchomości zestawu danych.
+ 6 9 minut obejmuje Eksplorator wyszukiwania i różne zapytania.


