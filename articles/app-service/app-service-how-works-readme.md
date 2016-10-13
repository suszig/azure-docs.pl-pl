<properties 
    pageTitle="Jak działa usługa Azure App Service" 
    description="Dowiedz się, jak działa usługa App Service" 
    keywords="app service, azure app service, scale, scalable, app service plan, app service cost"
    services="app-service" 
    documentationCenter="" 
    authors="yochay" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="02/10/2016" 
    ms.author="yochay"/>


# <a name="how-app-service-works"></a>Jak działa usługa App Service

Usługa Azure App Service to usługa w chmurze, która umożliwia rozwiązywanie praktycznych problemów dotyczących projektowania. Usługa App Service jest ukierunkowana na zapewnienie maksymalnej wydajności programowania i możliwości dostarczania aplikacji w skali chmury. Usługa App Service udostępnia również funkcje i platformy niezbędne do tworzenia aplikacji biznesowych dla przedsiębiorstw przy zachowaniu potrzebnej dla deweloperów obsługi najbardziej popularnych języków programowania (.NET, Java, PHP, Node.JS i Python).
Usługa App Service pozwala deweloperom korzystać z następujących możliwości:

* Tworzenie wysoce skalowalnych aplikacji sieci Web
* Szybkie tworzenie zapleczy aplikacji mobilnych za pomocą zestawu łatwych w użyciu funkcji mobilnych obsługujących zaplecza danych, uwierzytelnianie użytkowników i powiadomienia wypychane w usłudze Mobile Apps. 
* Implementowanie, wdrażanie i publikowanie interfejsów API za pomocą usługi API Apps.
* Integrowanie aplikacji biznesowych w ramach przepływów pracy i przekształcanie danych za pomocą usługi Logic Apps.

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)] 

Wszystkie typy aplikacji korzystają ze skalowalnej i elastycznej platformy aplikacji sieci Web, która zapewnia zoptymalizowane środowisko programowania obejmujące pełny cykl życia aplikacji — od ich projektowania po obsługę. Możliwości cyklu życia pozwalają na:

* Szybkie tworzenie aplikacji — zupełnie od początku albo przy użyciu pakietów OSS dostępnych w witrynie Azure Marketplace. 
* Ciągłe wdrażanie — automatyczne wdrażanie nowego kodu przy użyciu popularnych systemów kontroli źródła, takich jak TFS, GitHub czy BitBucket, oraz synchronizowanie zawartości z usług magazynów online, na przykład OneDrive lub DropBox.
* Testowanie w środowisku produkcyjnym — sprawne tworzenie środowisk przedprodukcyjnych i zarządzanie kierowanym do nich ruchem sieciowym. W razie potrzeby można debugować w chmurze i przywrócić wcześniejsze wdrożenie w przypadku znalezienia błędów.
* Uruchamianie zadań asynchronicznych i zadań wsadowych — można uruchamiać kod w tle lub aktywować go na podstawie zdarzeń (na przykład komunikatów kierowanych do kolejki usługi Azure Storage) i zaplanowanych godzin (za pomocą usługi CRON).
* Skalowanie aplikacji — dostępnych jest wiele opcji automatycznego skalowania usługi w poziomie i pionie na podstawie ruchu sieciowego i użycia zasobów. Można konfigurować prywatne środowiska dedykowane dla aplikacji.   
* Obsługa aplikacji — można korzystać z wielu funkcji debugowania i diagnostyki, aby zapobiegać problemom i efektywnie je rozwiązywać w czasie rzeczywistym (dzięki funkcjom takim jak automatyczne naprawianie czy debugowanie na żywo) lub po zakończeniu działania — analizując dzienniki i zrzuty pamięci.
 
Podsumowując, funkcje usługi App Service pozwalają deweloperom skoncentrować się na kodzie i szybko osiągnąć stabilny i wysoce skalowalny stan produkcji. Dzięki funkcjom interfejsu API oraz Aplikacji logiki deweloperzy mogą tworzyć praktyczne aplikacje dla przedsiębiorstw, które umożliwiają łączenie różnych rozwiązań biznesowych oraz integrowanie infrastruktury lokalnej z chmurą.  

[AZURE.INCLUDE [app-service-blueprint-how-app-service-works](../../includes/app-service-blueprint-how-app-service-works.md)]



<!--HONumber=Oct16_HO1-->


