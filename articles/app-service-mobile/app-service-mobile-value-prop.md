<properties
    pageTitle="Co to jest usługa Mobile Apps"
    description="Dowiedz się, jakie korzyści z usługi App Service można osiągać podczas pracy z aplikacjami mobilnymi w przedsiębiorstwie."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="05/03/2016"
    ms.author="krisagh"/>

# <a name="getting-started"> </a>Co to jest usługa Mobile Apps?

Usługa Azure App Service to w pełni zarządzana platforma jako usługa (PaaS) — oferta dla deweloperów z bogatym zestawem funkcji na potrzeby scenariuszy obejmujących sieć Web, pracę mobilną i dotyczących integracji. Usługa *Mobile Apps* w *usłudze Azure App Service* oferuje wysoce skalowalną, globalnie dostępną platformę tworzenia aplikacji mobilnych dla deweloperów w przedsiębiorstwach i integratorów systemów. Platforma ta oferuje bogaty zestaw funkcji dla deweloperów aplikacji mobilnych.

![Mobile Apps](./media/app-service-mobile-value-prop/overview.png)

##Dlaczego warto korzystać z usługi Mobile Apps?
Usługa *Mobile Apps* w *usłudze Azure App Service* oferuje wysoce skalowalną, globalnie dostępną platformę tworzenia aplikacji mobilnych dla deweloperów w przedsiębiorstwach i integratorów systemów. Platforma ta oferuje bogaty zestaw funkcji dla deweloperów aplikacji mobilnych. Dzięki usłudze Mobile Apps można:

- **Kompilować aplikacje natywne i międzyplatformowe** — bez względu na to, czy kompilujesz natywne aplikacje dla systemów iOS, Android i Windows, czy też międzyplatformowe aplikacje platform Xamarin lub Cordova (Phonegap), możesz korzystać z usługi App Service za pośrednictwem natywnych zestawów SDK.
- **Nawiązywać połączenia z systemami przedsiębiorstwa** — dzięki usłudze Mobile Apps można w ciągu kilku minut dodać opcję logowania firmowego oraz nawiązywać połączenia z zasobami przedsiębiorstwa lokalnie lub w chmurze.
- **Kompilować aplikacje gotowe do działania w trybie offline z opcją synchronizacji danych** — aby personel mobilny mógł pracować wydajnie, należy skompilować aplikacje działające w trybie offline, a następnie po nawiązaniu łączności za pomocą usługi Mobile Apps synchronizować w tle dane z dowolnych źródeł danych przedsiębiorstwa lub interfejsów API usługi SaaS.
- **Wypychać powiadomienia do milionów użytkowników w ciągu sekund** — można kontaktować się z klientami dzięki błyskawicznym powiadomieniom wypychanym z dowolnego urządzenia, spersonalizowanym zgodnie z ich potrzebami i wysyłanym w odpowiednim czasie.

## Funkcje Aplikacji mobilnej
Następujące funkcje są ważnymi elementami tworzenia aplikacji mobilnych z obsługą chmury:

- **Uwierzytelnianie i autoryzacja** — wybieraj z coraz dłuższej listy dostawców tożsamości, obejmującej usługę Azure Active Directory na potrzeby uwierzytelniania w przedsiębiorstwie, a także dostawców sieci społecznościowych, takich jak Facebook, Google, Twitter i konto Microsoft.  Usługa Azure Mobile Apps udostępnia usługę OAuth 2.0 dla każdego dostawcy.  Można również zintegrować zestaw SDK dla dostawcy tożsamości, aby korzystać z funkcji specyficznych dla tego dostawcy.

  Dowiedz się więcej na temat naszych [authentication features].

- **Dostęp do danych** — usługa Azure Mobile Apps oferuje współpracujące z urządzeniami przenośnymi źródło danych OData v3 połączone z usługą SQL Azure lub lokalnym programem SQL Server.  Ta usługa może opierać się na platformie Entity Framework, co pozwoli łatwo zintegrować ją z innymi dostawcami danych NoSQL i SQL, w tym [Azure Table Storage], MongoDB i [DocumentDB], a także dostawcami interfejsów API SaaS, takimi jak Office 365 i Salesforce.com.
- **Synchronizacja w trybie offline** — nasze zestawy SDK klientów ułatwiają kompilowanie niezawodnych i szybko reagujących aplikacji mobilnych korzystających z zestawu danych w trybie offline, które można automatycznie synchronizować z danymi zaplecza. Uwzględniają one również obsługę rozwiązywania konfliktów.

  Dowiedz się więcej na temat naszych [funkcje związane z danymi].

- **Powiadomienia wypychane** — nasze zestawy SDK klientów są bezproblemowo integrowane z możliwościami rejestracji w usłudze Azure Notification Hubs, co umożliwia wysyłanie powiadomień wypychanych do milionów użytkowników jednocześnie.

  Dowiedz się więcej na temat naszych [funkcje powiadomień wypychanych].

- **Zestawy SDK klientów** — oferujemy kompletny pakiet zestawów SDK klientów, które obsługują tworzenie aplikacji natywnych ([iOS], [Android] i [Windows]), międzyplatformowych ([Xamarin dla systemów iOS i Android], [Xamarin Forms]) i hybrydowych ([Apache Cordova]).  Każdy zestaw SDK klienta jest dostępny z licencją MIT i jest rozwiązaniem typu open source.

## Funkcje usługi App Service
Poniższe funkcje platformy są zazwyczaj przydatne w przypadku witryn produkcyjnych aplikacji mobilnych.

- **Automatyczne skalowanie** — usługa App Service pozwala szybko skalować w górę lub w poziomie, a co za tym idzie — obsługiwać dowolne przychodzące obciążenia klientów. Ręcznie wybierz liczbę i rozmiar maszyn wirtualnych lub skonfiguruj automatyczne skalowanie, aby skalować zaplecze aplikacji mobilnej w oparciu o obciążenie lub harmonogram.

  Dowiedz się więcej na temat [automatyczne skalowanie].

- **Środowiska przejściowe** — usługa App Service może uruchamiać wiele wersji witryny, co umożliwia testowanie A/B, testowanie w środowisku produkcyjnym jako część większego planu metodyki DevOps oraz miejscowe wdrażanie przejściowe nowego zaplecza.

  Dowiedz się więcej na temat [środowiska przejściowe].

- **Ciągłe wdrażanie** — usługę App Service można zintegrować z typowymi systemami SCM, umożliwiając w ten sposób automatyczne wdrażanie nowej wersji zaplecza przez wypchnięcie do gałęzi systemu SCM.

  Dowiedz się więcej na temat [opcje wdrożenia].

- **Praca w sieci wirtualnej** — usługa App Service może nawiązywać połączenia z zasobami lokalnymi przy użyciu sieci wirtualnej, programu ExpressRoute lub połączeń hybrydowych.

  Dowiedz się więcej na temat [połączenia hybrydowe], [sieci wirtualnych] i [programu ExpressRoute].

- **Środowiska izolowane/specjalne** — usługę App Service można uruchamiać w specjalnym, w pełni izolowanym środowisku, aby bezpiecznie pracować z aplikacjami usługi App Service na dużą skalę.  Jest to idealne rozwiązanie w przypadku obciążeń aplikacji wymagających bardzo dużej skali, izolacji lub bezpiecznego dostępu do sieci.

  Dowiedz się więcej na temat [Środowiska usługi App Service].

## Wprowadzenie ##
Aby rozpocząć pracę z usługą Mobile Apps, wykonaj kroki samouczka [Wprowadzenie].  Kroki te obejmują podstawy tworzenia wybranego zaplecza aplikacji mobilnych i klienta, a następnie integrowanie uwierzytelniania, synchronizacji w trybie offline i powiadomień wypychanych.  Czynności opisane w samouczku [Wprowadzenie] można wykonać kilka razy — raz dla każdej aplikacji klienta.

Aby uzyskać więcej informacji o usłudze Azure Mobile Apps, zapoznaj się z naszą [mapą nauki].
Aby uzyskać więcej informacji o platformie usługi Azure App Service, zobacz artykuł [usłudze Azure App Service].

>[AZURE.NOTE] Jeśli chcesz zacząć korzystać z usługi Azure App Service przed utworzeniem konta platformy Azure, przejdź do artykułu [Try App Service](https://tryappservice.azure.com/?appServiceName=mobile) (Wypróbuj usługę App Service), w którym wyjaśniono, jak od razu utworzyć początkową aplikację sieci Web o krótkim okresie istnienia w usłudze App Service. Bez kart kredytowych i bez zobowiązań.

<!-- URLs. -->
[Migracja Usługi mobilnej do usługi App Service]: app-service-mobile-migrating-from-mobile-services.md
[usłudze Azure App Service]: ../app-service/app-service-value-prop-what-is.md
[Wprowadzenie]: app-service-mobile-ios-get-started.md
[Azure Table Storage]: ../storage/storage-getting-started-guide.md
[DocumentDB]: ../documentdb/documentdb-get-started.md
[authentication features]: ./app-service-mobile-auth.md
[funkcje związane z danymi]: ./app-service-mobile-offline-data-sync.md
[funkcje powiadomień wypychanych]: ../notification-hubs/notification-hubs-overview.md
[iOS]: ./app-service-mobile-ios-how-to-use-client-library.md
[Android]: ./app-service-mobile-android-how-to-use-client-library.md
[Windows]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin dla systemów iOS i Android]: ./app-service-mobile-dotnet-how-to-use-client-library.md
[Xamarin Forms]: ./app-service-mobile-xamarin-forms-get-started.md
[Apache Cordova]: ./app-service-mobile-cordova-how-to-use-client-library.md
[automatyczne skalowanie]: ../app-service-web/web-sites-scale.md
[środowiska przejściowe]: ../app-service-web/web-sites-staged-publishing.md
[opcje wdrożenia]: ../app-service-web/web-sites-deploy.md
[połączenia hybrydowe]: ../app-service-web/web-sites-hybrid-connection-get-started.md
[sieci wirtualne]: ../app-service-web/web-sites-integrate-with-vnet.md
[ExpressRoute]: ../app-service/app-service-app-service-environment-network-configuration-expressroute.md
[Środowiska usługi App Service]: ../app-service-web/app-service-app-service-environment-intro.md
[mapą nauki]: https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/



<!--HONumber=Jun16_HO2-->


