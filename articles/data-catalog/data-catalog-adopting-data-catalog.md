<properties
    pageTitle="Wdrażanie usługi Azure Data Catalog | Microsoft Azure"
    description="W tym artykule przedstawiono podejście i proces dla organizacji rozważających wdrożenie usługi Azure Data Catalog, w tym definiowanie wizji, identyfikowanie najważniejszych przypadków użycia biznesowego i wybór projektu pilotażowego."
    services="data-catalog"
    documentationCenter=""
    authors="steelanddata"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="10/11/2016"
    ms.author="maroche"/>


# Wdrażanie usługi Azure Data Catalog
Ten artykuł ułatwia rozpoczęcie wdrażania usługi **Azure Data Catalog** w organizacji. Aby pomyślnie wdrożyć usługę **Azure Data Catalog**, należy skoncentrować się na trzech kluczowych elementach — zdefiniowaniu wizji, zidentyfikowaniu najważniejszych przypadków użycia biznesowego w ramach organizacji i wyborze projektu pilotażowego.

## Wprowadzenie do usługi Azure Data Catalog
Użytkownicy oczekują dziś zupełnie innych możliwości dostępu do specjalistycznych informacji o zasobach danych w środowisku pracy. Powszechne wykorzystywanie w miejscach pracy narzędzi mediów społecznościowych, takich jak Yammer, sprawia, że pracownicy oczekują możliwości szybkiego uzyskania pomocy i porad w szerokim zakresie tematów. Usługa **Azure Data Catalog** pomaga firmom i zespołom skonsolidować informacje o zasobach danych przedsiębiorstwa w centralnym repozytorium, w którym konsumenci danych mogą odnaleźć dostępne źródła danych i uzyskać wiedzę zamieszczaną przez ekspertów z danej dziedziny.

W tym artykule przedstawiono sposób rozpoczynania pracy z usługą **Azure Data Catalog**. W artykule opisano typowy plan wdrożenia usługi Azure Data Catalog dla fikcyjnej firmy o nazwie Adventure Works.

**Co to jest usługa Azure Data Catalog?**

**Azure Data Catalog** to w pełni zarządzana usługa na platformie Azure oraz wykaz informacji (metadanych) całego przedsiębiorstwa umożliwiający samodzielne odnajdywanie źródeł danych. Za pomocą usługi Data Catalog można rejestrować zasoby danych, odnajdywać je, dodawać do nich adnotacje i łączyć się z nimi. Usługa Data Catalog jest przeznaczona do zarządzania różnorodnymi zasobami informacji, ułatwiając ich znajdowanie, umożliwiając użytkownikom zrozumienie znalezionych zasobów danych i nawiązywanie połączenia z tymi zasobami, skracając czas potrzebny na analizę danych oraz zwiększając korzyści dla organizacji. Aby dowiedzieć się więcej, zobacz stronę usługi [Microsoft Azure Data Catalog](https://azure.microsoft.com/services/data-catalog/).

## Plan wdrożenia usługi Azure Data Catalog
Plan wdrożenia usługi **Azure Data Catalog** opisuje sposób, w jaki korzyści z używania usługi Data Catalog są komunikowane uczestnikom projektu i użytkownikom, jak również jakiego rodzaju szkolenia są zapewniane użytkownikom usługi. Jednym z kluczowych czynników dla powodzenia wdrożenia usługi Data Catalog jest to, jak skutecznie przedstawisz zalety i wartość usługi użytkownikom oraz uczestnikom projektu. Głównymi odbiorcami we wstępnym planie wdrożenia są użytkownicy usługi. Bez względu na to, jak wiele poparcia otrzymasz od uczestników projektu, jeśli użytkownicy lub klienci Twojej oferty usługi Data Catalog nie będą z niej korzystać, wdrożenie zakończy się niepowodzeniem. W związku z tym w niniejszym artykule przyjęto założenie, że uczestnicy projektu są w pełni zaangażowani, i skoncentrowano się na tworzeniu planu przyjęcia usługi Data Catalog przez użytkowników.
Skuteczny plan wdrożenia pomyślnie angażuje użytkowników i uczestników projektu w możliwości usługi Data Catalog i przekazuje im informacje oraz wskazówki dotyczące wykorzystania tych możliwości. Użytkownicy muszą zrozumieć wartość usługi Data Catalog oraz to, jak ułatwi im pracę i lepsze wykonywanie zadań. Gdy zobaczą, że usługa Data Catalog może pomóc im uzyskać lepsze wyniki w pracy z danymi, korzyści z wdrożenia tej usługi staną się oczywiste. Zmiana jest trudna, więc efektywny plan musi brać pod uwagę wyzwania z nią związane.

Plan wdrożenia pomaga przekazać użytkownikom informacje, które mają kluczowe znaczenie dla osiągnięcia przez nich sukcesu i założonych celów. Typowy plan wyjaśnia, jak usługa Data Catalog ułatwi życie użytkownikom, i zawiera następujące elementy:

-   **Wizja** — pomaga w zwięzłym omówieniu planu wdrożenia z użytkownikami i uczestnikami projektu. To Twoja krótka prezentacja pomysłu.
-   **Zespół pilotażowy i specjaliści** — uczenie się od zespołu pilotażowego i specjalistów ułatwia dostosowanie sposobu wprowadzenia zespołów i użytkowników do usługi Data Catalog. Specjaliści mogą szkolić użytkowników i współpracowników. Pozwala to również zidentyfikować czynniki blokujące i napędzające wdrożenie.
-   **Plan komunikacji i rozgłaszania** — ułatwia użytkownikom zrozumienie, w jaki sposób usługa Data Catalog może im pomóc, co sprzyja organicznemu przyjęciu usługi w zespołach, a ostatecznie w całej organizacji.
-   **Plan szkolenia** — kompleksowe szkolenie zwykle prowadzi do pomyślnego przyjęcia i pozytywnych wyników.

Poniżej przedstawiono kilka wskazówek dotyczących definiowania planu wdrożenia usługi **Azure Data Catalog**.

## Definiowanie wizji projektu Data Catalog
Pierwszym krokiem do zdefiniowania planu wdrożenia usługi **Azure Data Catalog** jest aspiracyjne opisanie tego, co próbujesz osiągnąć. Najlepiej, gdy wizja jest dość szeroka, ale wystarczająco zwięzła, aby zdefiniować konkretne krótko- i długoterminowe cele.

Oto kilka porad ułatwiających zdefiniowanie wizji:

-   **Zidentyfikuj najważniejszy motywator wdrożenia** — pomyśl o konkretnych potrzebach firmy związanych z zarządzaniem źródłami danych, które można zaspokoić dzięki usłudze Data Catalog. Pomaga to również określić najważniejsze korzyści i zalety usługi Data Catalog. Na przykład mogą istnieć wspólne źródła danych, które wszyscy nowi pracownicy muszą poznać i korzystać z nich, a obok nich ważne i złożone źródła danych, które dogłębnie rozumie tylko garstka kluczowych pracowników. Usługa **Azure Data Catalog** może sprawić, że te źródła danych będą łatwe do odnalezienia i zrozumienia, dzięki czemu dobrze znane słabe punkty można wyeliminować bezpośrednio i wcześnie w procesie wdrażania usługi.

-   **Przekaz powinien być rzeczowy i przejrzysty** — jasne zrozumienie wizji zapewnia wszystkim spójny obraz tego, jakie korzyści usługa Data Catalog oferuje organizacji i jak wizja wspiera cele organizacji.

-   **Zainspiruj ludzi, aby chcieli korzystać z usługi Data Catalog** — Twoja wizja i plan komunikacji powinny zachęcać pracowników do uznania usługi Data Catalog za usługę, która ułatwi im znajdowanie źródeł danych i łączenie się z nimi, co pozwoli im lepiej wykorzystywać możliwości danych.

-   **Określ konkretne cele i harmonogram** — dzięki temu plan wdrożenia będzie obejmował konkretne, osiągalne cele do zrealizowania. Harmonogram zapewnia skupienie się na realizacji planu i pozwala zdefiniować punkty kontrolne do oceny sukcesu.

Poniżej przedstawiono przykładową wizję dla planu wdrożenia usługi Data Catalog w fikcyjnej firmie o nazwie Adventure Works:

Usługa **Azure Data Catalog** umożliwia zespołowi działu finansowego firmy Adventure Works wydajną współpracę nad najważniejszymi źródłami danych. Dzięki temu każdy członek zespołu będzie mógł łatwo znaleźć potrzebne dane, korzystać z nich i dzielić się swoją wiedzą z całym zespołem.

Po zdefiniowaniu wizji należy zidentyfikować odpowiedni projekt pilotażowy dla usługi Data Catalog. Ogólnie rzecz biorąc, istnieje kilka scenariuszy dotyczących usługi Data Catalog, dlatego w następnej sekcji znajdują się wskazówki pomagające zidentyfikować odpowiednie przypadki użycia.

## Identyfikowanie biznesowych przypadków użycia usługi Data Catalog
Aby zidentyfikować przypadki użycia odpowiednie dla usługi Data Catalog, skontaktuj się z ekspertami z różnych jednostek biznesowych w celu zidentyfikowania odpowiednich przypadków użycia i problemów biznesowych do rozwiązania. Przejrzyj istniejące wyzwania napotykane przez pracowników dotyczące identyfikowania i zrozumienia zasobów danych. Na przykład czy zespoły dowiadują się o zasobach danych dopiero po zapytaniu kilku osób w organizacji, które dysponują odpowiednimi źródłami danych?

Najlepiej wybrać przypadki użycia reprezentujące „nisko wiszące owoce”, czyli te przypadki, które są ważne, a jednocześnie mają wysokie prawdopodobieństwo sukcesu, jeśli zostaną rozwiązane za pomocą usługi Data Catalog.

Poniżej przedstawiono kilka wskazówek dotyczących identyfikowania przypadków użycia:

-   **Zdefiniuj cele zespołu** — jak zespół osiąga swoje cele? Nie skupiaj się jeszcze na usłudze Data Catalog, ponieważ na tym etapie należy zachować obiektywność. Pamiętaj, że chodzi o wyniki biznesowe, nie technologię.

-   **Zdefiniuj problem biznesowy** — jakie problemy dotyczące znajdowania i poznawania zasobów danych napotyka zespół? Na przykład informacje o ważnych źródłach danych mogą znajdować się w skoroszytach programu Excel w folderze sieciowym, a zespół może poświęcać dużo czasu na lokalizowanie tych skoroszytów.

-   **Zrozum kulturę przyjmowania zmian przez zespół** — wiele problemów wdrożeniowych jest spowodowanych oporem przed zmianą, a nie z implementacją nowego narzędzia. To, jak zespół reaguje na zmianę, jest ważne podczas identyfikowania przypadków użycia, ponieważ istniejący proces może być stosowany, dlatego że „zawsze tak to robiliśmy” lub „jeśli coś nie jest popsute, to po co to naprawiać?”. Wdrażanie nowego narzędzia lub procesu jest zawsze najłatwiejsze, gdy osoby, których dotyczy zmiana, rozumieją korzyści z niej płynące i doceniają wagę problemów, które należy rozwiązać.

-   **Skup się na problemach związanych z zasobami danych** — omawiając problemy biznesowe, które napotyka zespół, musisz „przebić się przez gąszcz” i skoncentrować się na tym, co jest istotne dla bardziej efektywnego wykorzystania zasobów danych przedsiębiorstwa.

Oto niektóre przypadki użycia związane z usługą Data Catalog:

### Przykładowe przypadki użycia

-   **Rejestracja centralnych źródeł danych wysokiej wartości** — dział IT zarządza źródłami danych używanymi w organizacji. Dział IT może rozpocząć pracę z usługą Data Catalog poprzez zarejestrowanie wspólnych źródeł danych przedsiębiorstwa i dodanie do nich adnotacji.

-   **Rejestracja źródeł danych zespołów** — różne zespoły mają przydatne źródła danych biznesowych. Rozpocznij pracę z usługą **Azure Data Catalog** poprzez zidentyfikowanie i zarejestrowanie najważniejszych źródeł danych używanych przez wiele różnych zespołów i przechwycenie „plemiennej” wiedzy zespołu w adnotacjach usługi **Azure Data Catalog**.

-   **Samoobsługowa analiza biznesowa** — zespoły poświęcają mnóstwo czasu na łączenie danych z wielu źródeł. Zarejestruj źródła danych i dodaj do nich adnotacje w centralnej lokalizacji, aby wyeliminować proces ręcznego odnajdywania źródeł danych.

To tylko kilka przypadków zastosowań usługi **Azure Data Catalog**. Aby dowiedzieć się więcej o scenariuszach dla usługi Data Catalog, zobacz [Azure Data Catalog common scenarios](data-catalog-common-scenarios.md) (Typowe scenariusze dla usługi Azure Data Catalog).

Po zidentyfikowaniu niektórych przypadków użycia usługi Data Catalog powinny wyłonić się typowe scenariusze. W następnej sekcji omówiono sposób identyfikowania pierwszego projektu pilotażowego na podstawie przypadku użycia.

## Wybieranie projektu pilotażowego usługi Data Catalog
Kluczowe znaczenie dla sukcesu ma uproszczenie i rozpoczęcie od czegoś małego. Dobrze zdefiniowany projekt pilotażowy o ograniczonym zakresie ułatwia posuwanie projektu naprzód bez ryzyka ugrzęźnięcia z powodu projektu zbyt złożonego lub mającego zbyt wielu uczestników. Ważne jest jednak również, aby projekt pilotażowy obejmował różnych użytkowników, od wczesnych entuzjastów po sceptyków. Użytkownicy, którzy przyjmują rozwiązanie, pomagają dostosować przyszły plan komunikacji i rozgłaszania. Sceptycy pomagają zidentyfikować i rozwiązać blokujące problemy. W miarę jak sceptycy staną się mistrzami, możesz wykorzystać ich opinie, aby zidentyfikować czynniki sukcesu.

Plan pilotażowy powinien wprowadzać stopniowo cele biznesowe, które chcesz osiągnąć za pomocą usługi Data Catalog. Kiedy zdobędziesz doświadczenie w projekcie pilotażowym, możesz rozszerzyć bazę użytkowników. Wstępny zamknięty projekt pilotażowy jest dobry do ustalenia mierzalnego sukcesu, ale ostatecznym celem jest jego organiczny, naturalny rozwój i rozszerzenie. Dzięki organicznemu rozwojowi usługi Data Catalog użytkownicy mają kontrolę nad wykorzystaniem swoich danych i mogą wpływać na innych, zachęcając ich do przyjęcia wykazu i współtworzenia jego zawartości.

### Wybieranie właściwego zespołu docelowego
Podczas wybierania projektu pilotażowego wybierz zespół z najbardziej atrakcyjnymi scenariuszami, które rozwiązują istniejący problem biznesowy. Przykład: analityczka biznesowa tworzy raporty z bazy danych programu SQL Server. Problem polega na tym, że uzyskała informację o źródle danych dopiero po rozmowie z kilkoma współpracownikami. Ostatecznie, po tym jak zmarnowała czas na próby znalezienia źródeł danych do użycia, dowiedziała się o skoroszycie programu Excel zawierającym opis każdego źródła danych. Chociaż skoroszyt programu Excel odpowiednio opisuje tabele, których potrzebuje, szybko znalazłaby te źródła danych, gdyby zostały zarejestrowane i opatrzone adnotacjami w usłudze **Azure Data Catalog**.

### Identyfikowanie bohaterów danych
Pierwszy projekt pilotażowy powinien mieć kilka osób, które generują dane i wykorzystują dane, tak aby zespół miał zrównoważoną reprezentację.

**Producenci danych** to osoby posiadające wiedzę o źródłach danych. Na przykład Dominik w innym zespole pracował często z najważniejszymi źródłami danych firmy Adventure Works. Przed wdrożeniem usługi **Azure Data Catalog** Dominik utworzył skoroszyt programu Excel, aby zebrać informacje o źródłach danych firmy Adventure Works.

**Konsumenci danych** to osoby z doświadczeniem w wykorzystywaniu danych do rozwiązywania problemów biznesowych. Na przykład Irena jest analityczką biznesową i korzysta ze źródeł danych Adventure Works w programie SQL Server do analizy danych.

Jednym z problemów biznesowych, które rozwiązuje usługa **Azure Data Catalog**, jest połączenie **producentów danych** z **konsumentami danych**. Robi to, służąc jako centralne repozytorium informacji o źródłach danych przedsiębiorstwa. Przy użyciu usługi Data Catalog Dominik rejestruje źródła danych Adventure Works i programu SQL Server. Przy użyciu crowdsourcingu każdy użytkownik, który odnajdzie to źródło danych, może udostępnić swoje opinie na temat danych, a także korzystać z odnalezionych danych. Na przykład Irena odnajduje źródła danych przez wyszukiwanie w wykazie i udostępnia swoją specjalistyczną wiedzę o danych.  Teraz inni użytkownicy w organizacji korzystają z udostępnionej wiedzy po prostu przeszukując wykaz danych.

-   Aby dowiedzieć się więcej na temat rejestrowania źródeł danych, zobacz [Register data sources](data-catalog-get-started.md#exercise-2-registering-data-sources) (Rejestrowanie źródeł danych).

-   Aby dowiedzieć się więcej na temat odnajdywania źródeł danych, zobacz [Search data sources](data-catalog-get-started.md#exercise-3-discovering-registered-data-assets) (Wyszukiwanie źródeł danych).

### Rozpoczynanie od niewielkiego i skupionego projektu
W przypadku większości projektów pilotażowych w przedsiębiorstwie należy na początek umieścić w wykazie źródła danych wysokiej wartości, tak aby użytkownicy biznesowi mogli szybko docenić wartość usługi Data Catalog. Identyfikowanie wspólnych źródeł danych, które mogą być przydatne dla zespołu pilotażowego, najlepiej zacząć w dziale IT. Dla obsługiwanych źródeł danych, takich jak program SQL Server, zaleca się używanie narzędzia rejestracji źródła danych usługi **Azure Data Catalog**. Za pomocą narzędzia rejestracji źródła danych można zarejestrować różnorodne źródła danych, w tym bazy danych programu SQL Server, bazy danych Oracle i raporty usług SQL Server Reporting Services. Pełną listę bieżących źródeł danych można znaleźć w artykule [Azure Data Catalog supported data sources](data-catalog-dsr.md) (Obsługiwane źródła danych usługi Azure Data Catalog).

Po zidentyfikowaniu i zarejestrowaniu najważniejszych źródeł danych można również zaimportować opisy źródeł danych przechowywane w innych lokalizacjach. Interfejs API usługi Data Catalog umożliwia deweloperom załadowanie opisów i adnotacji z innej lokalizacji, takiej jak skoroszyt programu Excel utworzony i utrzymywany przez Dominika.

W następnej sekcji opisano przykładowy projekt z firmy Adventure Works.

### Przykładowy projekt
W tym przykładzie Irena, analityczka biznesowa, tworzy raporty dla swojego zespołu przy użyciu danych z bazy danych programu SQL Server. Problem polega na tym, że uzyskała informację o źródle danych dopiero po rozmowie z kilkoma współpracownikami. Szybko znalazłaby te źródła danych, gdyby zostały zarejestrowane i opatrzone adnotacjami w centralnej lokalizacji, takiej jak usługa **Azure Data Catalog**.

Aby zilustrować, jak łatwo Irena i jej zespół mogą znaleźć źródła danych wysokiej wartości, użyj narzędzia rejestracji źródła danych do wypełnienia wykazu informacjami (metadanymi) o źródłach danych. W ten sposób informacje o bazie danych są dostępne dla zespołu i przedsiębiorstwa, a nie tylko kilku osób. Gdy źródła danych są zarejestrowane w usłudze Data Catalog, Irena i jej zespół mogą łatwo z nich korzystać. Wynikiem jest bardziej kompleksowy i adekwatny wykaz danych dla jej zespołu i całego przedsiębiorstwa. W miarę jak coraz więcej zespołów zaczyna korzystać z usługi Data Catalog, źródła danych biznesowych stają się łatwiejsze do znalezienia i użycia, umożliwiając wykształcenie kultury skoncentrowanej na danych zapewniającej lepsze ich wykorzystywanie.

Aby dowiedzieć się więcej o narzędziu rejestracji źródła danych, zobacz [Rozpoczynanie pracy z usługą Azure Data Catalog](data-catalog-get-started.md).

W ramach projektu pilotażowego zespół Ireny korzysta również ze źródeł danych, które są opisane w skoroszycie programu Excel i obsługiwane przez Dominika oraz jego współpracowników. Ponieważ inne zespoły w przedsiębiorstwie również używają skoroszytów programu Excel do opisywania źródeł danych, dział IT decyduje się na utworzenie narzędzia do migracji skoroszytów programu Excel do usługi Data Catalog. Używając interfejsu API REST usługi Data Catalog do zaimportowania istniejących adnotacji, zespół projektu pilotażowego może mieć pełny wykaz danych składający się z metadanych wyodrębnionych ze źródeł danych przy użyciu narzędzia rejestracji źródła danych, uzupełniony o informacje opisane wcześniej przez producentów danych i konsumentów, bez konieczności ponownego ręcznego wprowadzania. Wraz z rozwojem wykazu danych przedsiębiorstwa organizacja może używać narzędzia rejestracji źródła danych dla wspólnych źródeł danych, a także interfejsu API usługi Data Catalog dla źródeł niestandardowych i nietypowych scenariuszy.

> [AZURE.NOTE] Napisaliśmy przykładowe narzędzie, które używa interfejsu API usługi **Azure Data Catalog** do migracji skoroszytu programu Excel do usługi Data Catalog. Aby dowiedzieć się więcej o interfejsie API usługi Data Catalog i przykładowym narzędziu, możesz [pobrać przykład kodu ad hoc skoroszytu](https://azure.microsoft.com/documentation/samples/data-catalog-dotnet-excel-register-data-assets/) i zapoznać się z dokumentacją [interfejsu API REST usługi Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267593.aspx).

Kiedy projekt pilotażowy jest gotowy, czas na wykonanie planu wdrożenia usługi Data Catalog.

### Realizacja
W tym momencie masz zidentyfikowane przypadki użycia dla usługi Data Catalog i zidentyfikowany pierwszy projekt. Ponadto zostały zarejestrowane najważniejsze źródła danych firmy Adventure Works i dodano informacje z istniejącego skoroszytu programu Excel za pomocą narzędzia utworzonego przez dział IT. Teraz nadszedł czas na współpracę z zespołem pilotażowym, aby rozpocząć proces wdrożenia usługi Data Catalog.

Oto kilka porad ułatwiających rozpoczęcie pracy:

-   **Wzbudź ekscytację** — użytkownicy biznesowi będą podekscytowani, jeśli będą uważali, że usługa **Azure Data Catalog** ułatwia im życie. Spróbuj poprowadzić rozmowę wokół rozwiązania i korzyści, jakie zapewnia, a nie technologii.

-   **Ułatw zmianę** — zacznij od czegoś małego i przedstaw plan użytkownikom biznesowym. Kluczowe znaczenie dla osiągnięcia sukcesu ma zaangażowanie użytkowników od samego początku, aby mieli wpływ na wynik i poczuli się w pewnym sensie właścicielami rozwiązania.

-   **Dbaj o entuzjastów** — entuzjaści jako pierwsi przyjmujący nowe rozwiązanie to użytkownicy, którzy pasjonują się tym, co robią, i z ochotą przekonują współpracowników do korzyści i zalet usługi **Azure Data Catalog**.

-   **Określ cel szkolenia** — użytkownicy biznesowi nie muszą wiedzieć wszystkiego o usłudze Data Catalog, więc ukierunkuj szkolenie na konkretne cele zespołu. Skup się na tym, co użytkownicy będą robić i jak mogą zmienić się niektóre z ich zadań, aby włączyć usługę **Azure Data Catalog** do ich codziennej pracy.

-   **Przyznawaj się do niepowodzeń** — jeśli projekt pilotażowy nie osiąga zakładanych wyników, ponownie oceń i zidentyfikuj obszary wymagające zmiany. Rozwiąż problemy w projekcie pilotażowym przed przejściem do większego zakresu.

Zanim zespół pilotażowy zacznie korzystać z usługi Data Catalog, zaplanuj spotkanie przed rozpoczęciem w celu omówienia oczekiwań dotyczących projektu pilotażowego i zapewnij wstępne szkolenie.

### Ustalanie oczekiwań
Ustalenie oczekiwań i celów ułatwia użytkownikom biznesowym skupienie się na osiągnięciu określonych wyników. Aby zachować zgodność projektu z harmonogramem, przydzielaj regularne (codziennie lub co tydzień, na podstawie zakresu i czasu trwania pilotażu) „zadania domowe”. Jedną z najbardziej wartościowych funkcji usługi Data Catalog jest crowdsourcing zasobów danych, umożliwiający użytkownikom biznesowym korzystanie z wiedzy i danych przedsiębiorstwa. Świetną pracą domową dla każdego członka zespołu pilotażowego jest zarejestrowanie lub opatrzenie adnotacją co najmniej jednego źródła danych, z którego korzystali. Zobacz [Register a data source](data-catalog-get-started.md#exercise-2-registering-data-sources) (Rejestrowanie źródła danych) i [Jak dodawać adnotacje do źródeł danych](data-catalog-get-started.md#exercise-4-annotating-registered-data-sources).

Spotykaj się z zespołem regularnie w celu przejrzenia niektórych z adnotacji. Dobre adnotacje dotyczące źródeł danych są bardzo ważne dla pomyślnego wdrożenia i przyjęcia usługi Data Catalog, ponieważ zapewniają wgląd w źródła istotnych danych w centralnej lokalizacji. Bez dobrych adnotacji wiedza na temat źródeł danych pozostaje rozproszona w całym przedsiębiorstwie. Zobacz [Jak dodawać adnotacje do źródeł danych](data-catalog-get-started.md#exercise-4-annotating-registered-data-sources).

Oczywiście ostatecznym testem projektu jest to, czy użytkownicy mogą odnaleźć i zrozumieć użycie źródeł danych, które są im potrzebne. Użytkownicy pilotażowi powinni regularnie testować wykaz w celu upewnienia się, że źródła danych, których używają w codziennej pracy, są odpowiednie. Gdy brak wymaganego źródła danych lub nie ma ono poprawnych adnotacji, powinno to być przypomnieniem o konieczności zarejestrowania dodatkowych źródeł danych lub udostępnienia dodatkowych adnotacji. Taka praktyka nie tylko zwiększa wartość pracy zespołu pilotażowego, ale sprzyja wytwarzaniu się odpowiednich nawyków, które przeniosą się do innych zespołów po ukończeniu projektu pilotażowego.

### Zapewnianie szkolenia
Szkolenie powinno wystarczyć użytkownikom do rozpoczęcia pracy i być dostosowane do określonych celów oraz doświadczenia członków zespołu pilotażowego. Aby rozpocząć szkolenie, możesz wykonać czynności opisane w artykule [Rozpoczynanie pracy z usługą Azure Data Catalog](data-catalog-get-started.md). Ponadto możesz pobrać [prezentację szkolenia dotyczącego projektu pilotażowego usługi Azure Data Catalog](https://github.com/Azure-Samples/data-catalog-dotnet-get-started/blob/master/Azure%20Data%20Catalog%20Training.pptx?raw=true). Ta prezentacja programu PowerPoint powinna ułatwić rozpoczęcie szkolenia wprowadzającego członków zespołu pilotażowego do usługi Data Catalog.

## Podsumowanie
Gdy zespół pilotażowy radzi sobie dość sprawnie i zostały osiągnięte początkowe cele, należy rozszerzyć wdrożenie usługi Data Catalog na więcej zespołów. Zastosuj i dostosuj procesy z projektu pilotażowego w celu rozwinięcia usługi Data Catalog w całej organizacji.

Wcześni użytkownicy rozwiązania, którzy uczestniczyli w pilotażu, mogą być bardzo pomocni w rozpowszechnianiu informacji o korzyściach z wdrożenia usługi Data Catalog. Mogą opowiedzieć innym zespołom, jak usługa Data Catalog pomogła ich zespołowi rozwiązać problemy biznesowe, łatwiej odnajdywać źródła danych i udostępniać informacji na temat źródeł danych, których używają. Na przykład entuzjaści z zespołu pilotażowego Adventure Works mogą pokazać innym, jak łatwo jest teraz znaleźć informacje o zasobach danych Adventure Works, które wcześniej były trudne do znalezienia i zrozumienia.

W tym artykule omówiono rozpoczynanie pracy z usługą **Azure Data Catalog** w organizacji. Mamy nadzieję, że udało Ci się uruchomić projekt pilotażowy usługi Data Catalog i rozwinąć wykaz danych w całej organizacji.

## Więcej informacji o usłudze Azure Data Catalog
-   [Strona produktu usługi Azure Data Catalog](https://azure.microsoft.com/services/data-catalog/)
-   [Dokumentacja dotycząca usługi Azure Data Catalog](https://azure.microsoft.com/documentation/services/data-catalog/)
-   [Azure Data Catalog common scenarios (Typowe scenariusze dla usługi Azure Data Catalog)](data-catalog-common-scenarios.md)
-   [Rejestrowanie źródeł danych (Rejestrowanie źródeł danych)](data-catalog-get-started.md#exercise-2-registering-data-sources)
-   [Search data sources (Wyszukiwanie źródeł danych)](data-catalog-get-started.md#exercise-3-discovering-registered-data-assets)
-   [Annotate data sources (Dodawanie adnotacji do źródeł danych)](data-catalog-get-started.md#exercise-4-annotating-registered-data-sources)
- [Crowdsourcing metadanych](data-catalog-get-started.md#exercise-5-crowdsourcing-metadata)



<!--HONumber=Oct16_HO3-->


