# <a name="azure-service-bus"></a>Azure Service Bus
Obojętnie, czy aplikacja lub usługa działa w chmurze, czy lokalnie, często wymaga interakcji z innymi aplikacjami lub usługami. Aby zapewnić szeroko użyteczny sposób, aby to zrobić, system Azure oferuje usługi Service Bus. Ten artykuł przedstawia tę technologię, opisuje, czym ona jest, oraz dlaczego warto z niej korzystać.

## <a name="service-bus-fundamentals"></a>Podstawy usługi Service Bus
Różne sytuacje wymagają różnych stylów komunikacji. Czasami najlepszym rozwiązaniem jest umożliwienie aplikacji wysyłania i odbierania komunikatów przy użyciu prostej kolejki. W innych sytuacjach zwykła kolejka nie wystarcza i lepszym rozwiązaniem jest kolejka z mechanizmem publikowania i subskrybowania. I w niektórych przypadkach naprawdę potrzebne jest połączenie między aplikacjami &#151; kolejki nie są wymagane. Usługa Service Bus udostępnia wszystkie trzy opcje, dzięki czemu aplikacje interakcję na kilka różnych sposobów.

Service Bus to wielodostępna usługa w chmurze, co oznacza, że usługa jest współużytkowana przez wielu użytkowników. Każdy użytkownik, taki jak twórca aplikacji, tworzy *przestrzeń nazw*, a następnie definiuje mechanizmy komunikacji, których potrzebuje w ramach tego obszaru nazw. [Rysunek 1](#Fig1) pokazuje, jak wygląda.

<a name="Fig1"></a>![Diagram usługi Azure Service Bus][svc-bus]

**Rysunek 1. Service Bus udostępnia usługę wielodostępu na potrzeby łączenia aplikacji za pośrednictwem chmury.**

W przestrzeni nazw można używać jednego lub większej liczby wystąpień czterech różnych mechanizmów komunikacji, z których każdy łączy aplikacje w inny sposób. Dostępne są następujące mechanizmy:

* *Kolejki*, które umożliwiają komunikację jednokierunkową. Każda kolejka działa jako pośrednik (nazywany czasem *brokerem*), który przechowuje wysyłane wiadomości, dopóki nie zostaną odebrane. Każdy komunikat jest odbierany przez jednego adresata.
* *Tematy*, które zapewniają komunikację jednokierunkową przy użyciu *subskrypcji* (jeden temat może mieć wiele subskrypcji). Podobnie jak kolejka temat działa jako broker, ale każda subskrypcja może opcjonalnie korzystać z filtru w celu odbierania tylko komunikatów spełniających określone kryteria.
* *Przekaźniki*, które zapewniają komunikację dwukierunkową. W przeciwieństwie do kolejek i tematów przekaźnik nie jest brokerem i nie przechowuje transmitowanych komunikatów. Zamiast tego po prostu przekazuje je do aplikacji docelowej.
* *Usługa Event Hubs*, która zapewniają dostarczanie zdarzeń i danych telemetrycznych do chmury w bardzo dużej skali, z małymi opóźnieniami i wysoką niezawodnością.

Podczas tworzenia kolejki, tematu, przekaźnika lub centrum zdarzeń nadajesz nazwę tworzonemu elementowi. W połączeniu z przestrzenią nazw ta nazwa tworzy unikatowy identyfikator obiektu. Aplikacje mogą udostępnić tę nazwę usłudze Service Bus, a następnie użyć tej kolejki, tematu, przekaźnika lub centrum zdarzeń do komunikowania się ze sobą. 

Aby użyć dowolnego z tych obiektów, aplikacje systemu Windows mogą korzystać z programu Windows Communication Foundation (WCF). W przypadku kolejek, tematów i usługi Event Hubs aplikacje systemu Windows mogą również używać interfejsów API obsługi komunikatów zdefiniowanych przez usługę Service Bus. Aby ułatwić korzystanie z tych obiektów aplikacjom innych niż aplikacje systemu Windows, firma Microsoft udostępnia zestawy SDK dla języka Java, Node.js i innych języków. Dostęp do kolejek, tematów i usługi Event Hubs można także uzyskać za pomocą interfejsów API REST za pośrednictwem protokołu HTTP. 

Należy pamiętać, że chociaż usługa Service Bus sama działa w chmurze (to znaczy w centrach danych platformy Microsoft Azure), aplikacje z niej korzystające mogą być uruchamiane w dowolnym miejscu. Usługa Service Bus umożliwia na przykład łączenie aplikacji działających na platformie Azure lub aplikacji uruchamianych wewnątrz własnego centrum danych. Umożliwia także łączenie aplikacji działających na platformie Azure lub innej platformie chmurowej z aplikacjami lokalnymi lub z tabletami i telefonami. Istnieje nawet możliwość łącznia się urządzeń gospodarstwa domowego, czujników i innych urządzeń z centralną aplikacją lub między sobą. Usługa Service Bus jest ogólnym mechanizmem komunikacji w chmurze, który jest dostępny niemal z dowolnego miejsca. Sposób korzystania z usługi Service Bus zależy od tego, co aplikacja ma robić.

## <a name="queues"></a>Kolejki
Załóżmy, że użytkownik łączy dwie aplikacje przy użyciu kolejki usługi Service Bus. [Rysunek 2](#Fig2) przedstawiono tę sytuację.

<a name="Fig2"></a>![Diagram kolejek usługi Service Bus][queues]

**Rysunek 2. Kolejki usługi Service Bus zapewniają jednokierunkowe kolejkowanie asynchroniczne.**

Proces jest prosty: nadawca wysyła komunikat do kolejki usługi Service Bus, a odbiornik pobiera ten komunikat w późniejszym czasie. Kolejki mogą mieć tylko jeden odbiornik, jako [na rysunku 2](#Fig2) programy lub wiele aplikacji może odczytywać z tej samej kolejki. W tej ostatniej sytuacji każdy komunikat jest odczytywany przez tylko jeden odbiornik. W przypadku usługi multiemisji należy zamiast kolejki użyć tematu.

Każdy komunikat ma dwie części: zbiór właściwości, z których każda jest parą klucz/wartość, oraz binarną treść komunikatu. Sposób ich używania zależy od tego, co aplikacja próbuje zrobić. Na przykład aplikacja wysyłająca komunikat o ostatniej sprzedaży może dołączać właściwości *Seller="Ava"* i *Amount=10000*. Treść komunikatu może zawierać zeskanowany obraz podpisanej umowy sprzedaży lub, jeśli nie istnieje, po prostu pozostać pusta.

Odbiornik może odczytywać komunikaty z kolejki usługi Service Bus na dwa różne sposoby. Pierwsza opcja o nazwie *ReceiveAndDelete* usuwa komunikat z kolejki i natychmiast go kasuje. Jest to proste, ale jeśli odbiornik ulegnie awarii przed zakończeniem przetwarzania komunikatu, komunikat zostanie utracony. Ponieważ został usunięty z kolejki, żaden inny odbiornik nie może już uzyskać do niego dostępu. 

Druga opcja, *PeekLock*, ma pomóc w rozwiązaniu tego problemu. Podobnie jak ReceiveAndDelete odczytu PeekLock usuwa komunikat z kolejki. Nie powoduje jednak skasowania (faktycznego usunięcia) komunikatu. Zamiast tego blokuje komunikat, ukrywając go przed innymi odbiornikami, a następnie czeka na jedno z trzech zdarzeń:

* Jeśli odbiornik pomyślnie przetworzy komunikat, wywołuje metodę *Complete*, a kolejka usuwa komunikat. 
* Jeśli odbiornik zadecyduje, że nie może przetworzyć komunikatu, wywołuje metodę *Abandon*. Kolejka następnie usuwa blokadę z komunikatu i umożliwia dostęp do niego innym odbiornikom.
* Jeśli odbiornik nie wywoła żadnej z tych metod w skonfigurowanym czasie (domyślnie 60 sekund), kolejka zakłada, że wystąpił błąd odbiornika. W takim przypadku zachowuje, jak gdyby odbiornik wywołał porzucenia, udostępniając komunikat innym odbiornikom.

Należy zauważyć, że w tej sytuacji może się zdarzyć, że ten sam komunikat zostanie dostarczony dwukrotnie, być może nawet do dwóch różnych odbiorników. Aplikacje korzystające z usługi Service Bus muszą być na to przygotowane. Aby ułatwić wykrywanie duplikatów, każdy komunikat ma unikatową właściwości MessageID, która domyślnie pozostaje taka sama niezależnie od tego, jak wiele razy komunikat został odczytany z kolejki. 

Kolejki są przydatne w kilku sytuacjach. Umożliwiają one aplikacjom komunikowanie się nawet wtedy, gdy nie są uruchomione w tym samym czasie, co jest szczególnie przydatne w przypadku aplikacji wsadowych i aplikacji dla urządzeń przenośnych. Kolejka z wieloma odbiornikami zapewnia również automatyczne równoważenie obciążenia, ponieważ wysłane wiadomości są rozkładane między te odbiorniki.

## <a name="topics"></a>Tematy
Pomimo swojej przydatności, kolejki nie zawsze są odpowiednim rozwiązaniem. Czasami lepsze są tematy usługi Service Bus. [Rysunek 3](#Fig3) ilustruje tę koncepcję.

<a name="Fig3"></a>![Diagram tematów usługi Service Bus i subskrypcji][topics-subs]

**Rysunek 3. Na podstawie filtru określonego przez subskrybującą aplikację może ona odbierać niektóre lub wszystkie komunikaty wysyłane do tematu usługi Service Bus.**

Temat jest pod wieloma względami podobny do kolejki. Nadawcy wysyłają komunikaty do tematu w taki sam sposób jak do kolejki, a komunikaty wyglądają tak samo jak w przypadku kolejek. Istotną różnicą jest to, że tematy umożliwić każdej aplikacji odbierającej utworzenie własnej subskrypcji przez zdefiniowanie *filtru*. Subskrybent będzie wtedy widział tylko komunikaty zgodne z filtrem. Na przykład [rysunek 3](#Fig3) przedstawia nadawcę i temat z trzema subskrybentami, każde z nich ma własny filtr:

* Subskrybent 1 odbiera tylko komunikaty, które zawierają właściwość *Seller="Ava"*.
* Subskrybent 2 odbiera komunikaty, które zawierają właściwość *Seller="Ruby"* i/lub zawierają właściwość *Amount* o wartości większej niż 100 000. Być może Ruby jest kierownikiem ds. sprzedaży, a więc chce widzieć zarówno własną sprzedaż i wszystkie duże transakcje sprzedaży niezależnie od tego, kto ich dokonuje.
* Subskrybent 3 ustawił filtr na wartość *True*, co oznacza, że odbiera wszystkie komunikaty. Ta aplikacja może być na przykład odpowiedzialna za utrzymanie dziennika inspekcji i w związku z tym musi widzieć wszystkie komunikaty.

Podobnie jak w przypadku kolejek subskrybenci tematu mogą odczytywać komunikaty przy użyciu ReceiveAndDelete lub PeekLock. W przeciwieństwie do kolejek pojedynczy komunikat wysłany do tematu może zostać odebrany przez wielu subskrybentów. Takie podejście, często nazywane *Publikuj i Subskrybuj*, jest przydatne, gdy wiele aplikacji mogą być zainteresowani tej wiadomości. Dzięki zdefiniowaniu właściwego filtru każdy subskrybent może korzystać tylko z tej części strumienia komunikatów, którą chce wyświetlać.

## <a name="relays"></a>Przekaźniki
Kolejki i tematy zapewniają jednokierunkową asynchroniczną komunikację za pośrednictwem brokera. Ruch przepływa tylko w jednym kierunku i nie ma bezpośredniego połączenia między nadawcami a odbiornikami. Ale co zrobić, jeśli tego nie chcesz? Załóżmy, że aplikacje muszą zarówno wysyłać, jak i odbierać komunikaty, albo chcesz mieć bezpośrednie połączenie między nimi i nie potrzebujesz brokera do przechowywania komunikatów. Do takich scenariuszy adresu usługi Service Bus udostępnia przekaźniki, jak [4 rysunek](#Fig4) pokazuje.

<a name="Fig4"></a>![Diagram przekaźnik magistrali usług][relay]

**Rysunek 4. Przekaźnik usługi Service Bus zapewnia synchroniczną, dwukierunkową komunikację między aplikacjami.**

Oczywistym pytaniem, jeśli chodzi o przekaźniki, jest „po co z nich korzystać?” Nawet jeśli nie potrzebujesz kolejek, dlaczego warto udostępnić aplikacjom możliwość komunikacji za pomocą usługi w chmurze zamiast bezpośredniej interakcji? Odpowiedź brzmi: komunikacja bezpośrednia może być trudniejsza, niż się wydaje.

Załóżmy, że chcesz nawiązać połączenie między dwiema lokalnymi aplikacjami, z których obie działają wewnątrz firmowych centrów danych. Każda z tych aplikacji znajduje się za zaporą, a każde centrum danych prawdopodobnie używa translatora adresów sieciowych (NAT). Zapora blokuje przychodzące dane na wszystkich, z wyjątkiem kilku, portach, a korzystanie z NAT oznacza, że maszyna, na której uruchomiona jest każda aplikacja, nie ma stałego adresu IP osiągalnego bezpośrednio spoza centrum danych. Bez dodatkowej pomocy połączenie tych aplikacji za pośrednictwem publicznego Internetu jest problematyczne.

Przekaźnik usługi Service Bus zapewnia tę pomoc. Aby komunikować się dwukierunkowo przez przekaźnik, każda aplikacja ustanawia wychodzące połączenie TCP za pomocą usługi Service Bus, a następnie utrzymuje je otwarte. Cała komunikacja między dwiema aplikacjami odbywa się przez te połączenia. Ponieważ każde połączenie zostało nawiązane z wewnątrz centrum danych, zapora będzie zezwalała na ruch przychodzący do każdej aplikacji bez konieczności otwierania nowych portów. To podejście zapewnia również obejście problemu translatora adresów sieciowych, ponieważ każda aplikacja ma spójny punkt końcowy w chmurze podczas całej komunikacji. Dzięki wymianie danych za pośrednictwem przekaźnika aplikacje mogą uniknąć problemów, które w przeciwnym razie spowodowałyby trudności w komunikacji. 

Aby korzystać z przekaźników usługi Service Bus, aplikacje polegają na Windows Communication Foundation (WCF). Usługa Service Bus udostępnia wiązania WCF, dzięki którym interakcje za pomocą przekaźników stają się proste dla aplikacji systemu Windows. Aplikacje, które już używają WCF, mogą zazwyczaj określić tylko jedno z tych powiązań, a następnie komunikować się ze sobą za pośrednictwem przekaźnika. Jednak inaczej niż w przypadku kolejek i tematów korzystanie z przekaźników z aplikacji innych niż aplikacje systemu Windows, o ile jest to możliwe, wymaga nieco pracy programistycznej, ponieważ nie są udostępniane żadne biblioteki standardowe.

W przeciwieństwie do kolejek i tematów aplikacje nie tworzą jawnie przekaźników. Zamiast tego aplikacja, która chce odbierać komunikaty, ustanawia połączenie TCP z usługą Service Bus, a przekaźnik jest tworzony automatycznie. Jeśli połączenie zostanie porzucone, przekaźnik jest usuwany. Aby umożliwić aplikacji znalezienie przekaźnika utworzonego przez określony odbiornik, Usługa Service Bus udostępnia rejestr, który umożliwia aplikacjom zlokalizowanie konkretnego przekaźnika według nazwy.

Przekaźniki są odpowiednim rozwiązaniem, gdy potrzebna jest bezpośrednia komunikacja między aplikacjami. Rozważmy na przykład system rezerwacji linii lotniczych uruchomiony w lokalnym centrum danych, który musi być dostępny z kiosków odprawy, urządzeń przenośnych i innych komputerów. Aplikacje działające we wszystkich tych systemach mogą polegać na przekaźnikach usługi Service Bus w chmurze na potrzeby komunikacji wszędzie tam, gdzie mogą być uruchamiane.

## <a name="event-hubs"></a>Event Hubs
Usługa Event Hubs to wysoce skalowalny system przyjmowania danych, który może przetwarzać miliony zdarzeń na sekundę, umożliwiając aplikacji przetwarzanie i analizowanie olbrzymich ilości danych generowanych przez połączone urządzenia i aplikacje. Za pomocą takiego centrum zdarzeń można na przykład zbierać na żywo dane wydajności silników z floty samochodów. Po zebraniu danych do usługi Event Hubs można przekształcać dane za pomocą dowolnego dostawcy analiz w czasie rzeczywistym lub przechowywać je przy użyciu klastra magazynu. Aby uzyskać więcej informacji na temat usługi Event Hubs, zobacz [Przegląd usługi Event Hubs][Event Hubs overview].

## <a name="summary"></a>Podsumowanie
Łączenie aplikacji zawsze było częścią tworzenia kompletnych rozwiązań, a zakres scenariuszy, które wymagają komunikowania się ze sobą aplikacji i usług, ciągle się zwiększa, ponieważ coraz więcej aplikacji i urządzeń ma połączenie z Internetem. Zapewniając technologie chmurowe służące osiągnięciu tego celu za pośrednictwem kolejek, tematów, przekaźników i usługi Event Hubs, usługa Service Bus ułatwia wdrożenie tej istotnej funkcji i sprawia, że jest ona szerzej dostępna.

[svc-bus]: ./media/hybrid-solutions/SvcBus_01_architecture.png
[queues]: ./media/hybrid-solutions/SvcBus_02_queues.png
[topics-subs]: ./media/hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[relay]: ./media/hybrid-solutions/SvcBus_04_relay.png
[Event Hubs overview]: https://msdn.microsoft.com/library/azure/dn836025.aspx
