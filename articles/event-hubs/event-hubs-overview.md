<properties 
    pageTitle="Omówienie usługi Azure Event Hubs | Microsoft Azure"
    description="Wprowadzenie do usługi Azure Event Hubs i jej omówienie."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm" />


# <a name="azure-event-hubs-overview"></a>Omówienie usługi Azure Event Hubs

Wiele nowoczesnych rozwiązań zostało utworzonych w celu zapewnienia adaptacyjnego środowiska użytkownika lub ulepszenia produktów dzięki ciągłym informacjom zwrotnym i zautomatyzowanej telemetrii. W przypadku takich rozwiązań wyzwaniem jest znalezienie bezpiecznego i niezawodnego sposobu przetwarzania bardzo dużych ilości informacji pochodzących od wielu jednoczesnych wydawców. Usługa Microsoft Azure Event Hubs to usługa platformy zarządzanej, która stanowi podstawę do pobierania danych na dużą skalę w ramach szerokiej gamy scenariuszy. Przykłady takich scenariuszy to śledzenie zachowania w aplikacji mobilnych, zbieranie informacji o ruchu z farmy serwerów sieci Web, przechwytywanie zdarzeń w grach na konsole lub zbieranie danych telemetrycznych z maszyn przemysłowych lub połączonych pojazdów. Typową rolą usługi Event Hubs w architekturze rozwiązań jest to, że działa ona jako „drzwi wejściowe” dla potoku zdarzeń i często nazywana jest *systemem zbierania zdarzeń*. System zbierania zdarzeń to składnik lub usługa, która znajduje się między wydawcami zdarzeń, a ich odbiorcami. Jego rolą jest oddzielenie procesu tworzenia strumienia zdarzeń od użycia tych zdarzeń.

![Usługa Event Hubs](./media/event-hubs-overview/IC759856.png)

Usługa Azure Event Hubs to usługa służąca do przetwarzania zdarzeń, która dostarcza zdarzenia i dane telemetryczne do chmury w bardzo dużej skali, z małymi opóźnieniami i wysoką niezawodnością. Ta usługa, używana wraz z innymi usługami podrzędnymi, jest szczególnie przydatna podczas instrumentacji aplikacji, przetwarzaniu środowiska użytkownika lub przepływu pracy oraz scenariuszach Internetu rzeczy (IoT). Usługa Event Hubs zapewnia funkcję obsługi strumienia komunikatów i chociaż centrum zdarzeń to jednostka podobna do kolejek i tematów, to ma właściwości, które bardzo różnią się od tradycyjnych metod przesyłania komunikatów w firmie. Scenariusze dotyczące przesyłania komunikatów w firmie zwykle wymagają zaawansowanych funkcji, takich jak sekwencjonowanie, obsługa utraconych komunikatów, obsługa transakcji i silne gwarancje dostarczenia, podczas gdy dominującą kwestią w przypadku pobierania zdarzeń jest duża przepływność i elastyczność przetwarzania dla strumieni zdarzeń. W związku z tym możliwości usługi Event Hubs różnią się od tematów usługi Service Bus tym, że są one silnie ukierunkowane pod kątem scenariuszy wysokiej przepływności i przetwarzania zdarzeń. W takiej sytuacji usługa Event Hubs nie implementuje niektórych możliwości obsługi komunikatów, które są dostępne w przypadku tematów. Jeśli takie możliwości są wymagane, tematy pozostają optymalnym wyborem.

Centrum zdarzeń jest tworzone na poziomie przestrzeni nazw usługi Event Hubs, podobnie jak w przypadku kolejek i tematów usługi Service Bus. Usługa Event Hubs używa protokołów AMQP i HTTP jako podstawowych interfejsów API. Na poniższym diagramie przedstawiono relację między usługami Event Hubs i Service Bus.

![Usługa Event Hubs](./media/event-hubs-overview/IC741188.png)

## <a name="conceptual-overview"></a>Omówienie pojęć

Usługa Event Hubs zapewnia strumieniowe przesyłanie komunikatów za pomocą partycjonowanego wzorca odbiorcy. Kolejki i zasoby używają modelu [odbiorców konkurencyjnych](https://msdn.microsoft.com/library/dn568101.aspx), w ramach którego każdy odbiorca podejmuje próbę odczytu z tej samej kolejki lub zasobu. To konkurowanie o zasoby powoduje w rezultacie złożoność i konieczność użycia limitów skalowania dla aplikacji przetwarzających strumień. Usługa Event Hubs używa partycjonowanego wzorca odbiorców, w ramach którego każdy odbiorca odczytuje tylko konkretny podzbiór, lub partycję, strumienia komunikatów. Ten wzorzec umożliwia skalowanie w poziomie przetwarzania zdarzeń oraz udostępnia inne funkcje dotyczące strumienia, które są niedostępne w przypadku kolejek i tematów.

### <a name="partitions"></a>Partycje

Partycja to uporządkowana sekwencja zdarzeń przechowywana w centrum zdarzeń. Po nadejściu nowszych zdarzeń są one dodawane na końcu sekwencji. Partycję można traktować jako „dziennik zatwierdzania”.

![Usługa Event Hubs](./media/event-hubs-overview/IC759857.png)

Partycje zachowują dane przez skonfigurowany czas przechowywania, który jest ustawiany na poziomie centrum zdarzeń. To ustawienie dotyczy wszystkich partycji w centrum zdarzeń. Zdarzenia wygasają czasowo — nie można ich jawnie usunąć. Centrum zdarzeń zawiera wiele partycji. Każda partycja jest niezależna i zawiera własną sekwencję danych. W związku z tym partycje często rosną w różnym tempie.

![Usługa Event Hubs](./media/event-hubs-overview/IC759858.png)

Liczba partycji jest określana w czasie tworzenia centrum zdarzeń i musi zawierać się w przedziale od 2 do 32 (wartość domyślna to 4). Partycje stanowią mechanizm organizacji danych i są bardziej powiązane ze stopniem równoległości podrzędnej wymaganym podczas używania aplikacji niż z przepływnością usługi Event Hubs. Sprawia to, że wybór liczby partycji w centrum zdarzeń jest bezpośrednio związany z oczekiwaną liczbą jednoczesnych czytników. Po utworzeniu centrum zdarzeń liczby partycji nie można zmienić. Z tego powodu tę liczbę należy rozważyć w kontekście długoterminowo oczekiwanej skali. Limit wynoszący 32 partycje można zmienić, kontaktując się z zespołem usługi Service Bus.

Choć partycje można identyfikować i wysyłać do nich bezpośrednio, najlepiej jest unikać wysyłania danych do konkretnych partycji. Zamiast tego można użyć konstrukcji wyższego poziomu wprowadzonych w sekcjach [Wydawca zdarzeń](#event-publisher) i [Zasady wydawcy](#capacity-and-security).

W kontekście usługi Event Hubs komunikaty są określane jako *dane zdarzenia*. Dane zdarzenia zawierają treść zdarzenia, zdefiniowany przez użytkownika zbiór właściwości oraz różne metadane dotyczące zdarzenia, takie jak jego przesunięcie w partycji i numer w sekwencji strumienia. Partycje są wypełnione sekwencją danych zdarzenia.

## <a name="event-publisher"></a>Wydawca zdarzeń

Każda jednostka, która wysyła zdarzenia lub dane do centrum zdarzeń jest *wydawcą zdarzeń*. Wydawcy zdarzeń mogą publikować zdarzenia przy użyciu protokołu HTTPS lub AMQP 1.0. Wydawcy zdarzeń używają tokenu sygnatury dostępu współdzielonego w celu identyfikowania siebie w centrum zdarzeń i mogą mieć unikatową tożsamość lub używają typowego tokenu sygnatury dostępu współdzielonego, w zależności od wymagań scenariusza.

Aby uzyskać więcej informacji na temat pracy z sygnaturą dostępu współdzielonego, zobacz [Shared Access Signature Authentication with Service Bus](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) (Uwierzytelnianie za pomocą sygnatury dostępu współdzielonego przy użyciu usługi Service Bus).

### <a name="common-publisher-tasks"></a>Typowe zadania wydawcy

W tej sekcji opisano typowe zadania dla wydawców zdarzeń.

#### <a name="acquire-a-sas-token"></a>Uzyskanie tokenu sygnatury dostępu współdzielonego

Sygnatura dostępu współdzielonego to mechanizm uwierzytelniania usługi Event Hubs. Usługa Service Bus udostępnia zasady sygnatury dostępu współdzielonego na poziomie przestrzeni nazw i centrum zdarzeń. Token sygnatury dostępu współdzielonego jest generowany na podstawie klucza sygnatury dostępu współdzielonego i jest skrótem SHA adresu URL zakodowanym w określonym formacie. Przy użyciu nazwy klucza (zasady) i tokenu usługa Service Bus może ponownie wygenerować skrót i w ten sposób uwierzytelnić nadawcę. Zwykle tokeny sygnatury dostępu współdzielonego dla wydawców zdarzeń są tworzone jedynie z uprawnieniami do **wysyłania** w określonym centrum zdarzeń. Ten mechanizm adresu URL tokenu sygnatury dostępu współdzielonego stanowi podstawę do identyfikacji wydawcy wprowadzoną w ramach zasad wydawcy. Aby uzyskać więcej informacji na temat pracy z sygnaturą dostępu współdzielonego, zobacz [Shared Access Signature Authentication with Service Bus](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) (Uwierzytelnianie za pomocą sygnatury dostępu współdzielonego przy użyciu usługi Service Bus).

#### <a name="publishing-an-event"></a>Publikowanie zdarzenia

Zdarzenie można opublikować za pośrednictwem protokołu AMQP 1.0 lub HTTPS. Usługa Service Bus udostępnia klasę [EventHubClient](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.eventhubclient.aspx) służącą do publikowania zdarzeń w centrum zdarzeń od klientów platformy .NET. W przypadku innych środowisk uruchomieniowych i platform można używać dowolnego klienta protokołu AMQP 1.0, na przykład [Apache Qpid](http://qpid.apache.org/). Zdarzenia można publikować indywidualnie lub w partiach. Jedna publikacja (wystąpienie danych zdarzeń) ma limit wynoszący 256 KB, niezależnie od tego, czy jest to pojedyncze zdarzenie, czy partia. Publikowanie większych zdarzeń spowoduje wystąpienie błędu. Najlepszym rozwiązaniem dla wydawców jest niebranie pod uwagę partycji w ramach centrum zdarzeń i określenie jedynie *klucza partycji* (zostanie wprowadzony w następnej sekcji) lub tożsamości za pomocą ich tokenu sygnatury dostępu współdzielonego.

Decyzja o korzystaniu z protokołu AMQP lub HTTPS jest specyficzna dla scenariusza użycia. Protokół AMQP wymaga ustanowienia trwałego gniazda dwukierunkowego oprócz protokołu TLS lub SSL/ TLS. Może to być kosztowna operacja pod względem ruchu w sieci, ale odbywa się tylko na początku sesji protokołu AMQP. Protokół HTTPS ma mniejszy narzut początkowy, ale wymaga dodatkowego narzutu związanego z protokołem SSL w przypadku każdego żądania. Dla wydawców często publikujących zdarzenia protokół AMQP oferuje znaczne oszczędności wydajności i przepływności oraz zmniejszenie opóźnienia.

### <a name="partition-key"></a>Klucz partycji

Klucz partycji to wartość, która służy do mapowania danych zdarzeń przychodzących na określone partycje na potrzeby organizowania danych. Klucz partycji to wartość podawana przez nadawcę przekazywana do centrum zdarzeń. Jest on przetwarzany przez statyczną funkcję tworzenia skrótu, za pomocą której tworzone jest przypisanie partycji. Jeśli nie określisz klucza partycji podczas publikowania zdarzenia, używane jest przypisanie działania okrężnego. Podczas korzystania z kluczy partycji wydawca zdarzeń ma informacje tylko o kluczu partycji, a nie partycji, na której publikowane są zdarzenia. To oddzielenie klucza od partycji powoduje, że nadawca nie musi wiedzieć zbyt dużo o przetwarzaniu podrzędnym i przechowywaniu zdarzeń. Klucze partycji są ważne w przypadku organizowania danych w celu przetwarzania podrzędnego, ale zasadniczo nie są związane z samymi partycjami. Unikatowa tożsamość urządzenia lub użytkownika stanowi dobry klucz partycji, ale inne atrybuty, takie jak lokalizacja geograficzna, mogą również zostać użyte do grupowania powiązanych zdarzeń w jedną partycję. Na poniższej ilustracji przedstawiono nadawców zdarzeń używających kluczy partycji do przypinania do partycji.

![Usługa Event Hubs](./media/event-hubs-overview/IC759859.png)

Usługa Event Hubs zapewnia, że wszelkie zdarzenia współużytkujące taką samą wartość klucza partycji są poprawnie dostarczane na tę samą partycję. Istotne jest, że jeśli klucze partycji są używane wraz z zasadami wydawcy opisanymi w następnej sekcji, to tożsamość wydawcy i wartość klucza partycji muszą być zgodne. W przeciwnym razie wystąpi błąd.

### <a name="event-consumer"></a>Odbiorcy zdarzeń

Każda jednostka, która odczytuje dane zdarzenia z centrum zdarzeń, jest odbiorcą zdarzenia. Wszyscy odbiorcy zdarzeń odczytują strumień zdarzeń za pomocą partycji w grupie odbiorców. Każda partycja powinna mieć tylko jeden aktywny czytnik. Wszyscy odbiorcy usługi Event Hubs nawiązują połączenie za pomocą sesji protokołu AMQP 1.0, w ramach której zdarzenia są dostarczane, gdy tylko staną się dostępne. Klient nie musi sondować dostępności danych.

#### <a name="consumer-groups"></a>Grupy odbiorców

Mechanizm publikowania/subskrypcji usługi Event Hubs jest włączany za pomocą grup odbiorców. Grupa odbiorców stanowi widok (stan, pozycja lub przesunięcie) całego centrum zdarzeń. Dzięki grupom odbiorców wiele aplikacji odbiorczych może mieć osobny widok strumienia zdarzeń i niezależnie odczytywać strumień we własnym tempie i przy użyciu własnego przesunięcia. W architekturze przetwarzania strumieni każda aplikacja podrzędna odpowiada grupie odbiorców. Jeśli chcesz zapisać dane zdarzenia do magazynu długoterminowego, to ta aplikacja edytora magazynu odpowiada grupie odbiorców. Przetwarzanie złożonych zdarzeń jest wykonywane przez inną, oddzielną grupę odbiorców. Dostęp do partycji można uzyskać tylko za pośrednictwem grupy odbiorców. W centrum zdarzeń zawsze istnieje domyślna grupa odbiorców, a w przypadku centrum zdarzeń warstwy standardowej można utworzyć maksymalnie 20 grup odbiorców.

Oto przykłady konwencji identyfikatora URI grupy odbiorców:

    //<my namespace>.servicebus.windows.net/<event hub name>/<Consumer Group #1>
    //<my namespace>.servicebus.windows.net/<event hub name>/<Consumer Group #2>

Na poniższej ilustracji przedstawiono odbiorców zdarzeń w ramach grupy odbiorców.

![Usługa Event Hubs](./media/event-hubs-overview/IC759860.png)

#### <a name="stream-offsets"></a>Przesunięcia strumienia

Przesunięcie to pozycja zdarzenia w partycji. Przesunięcie można traktować jako kursor po stronie klienta. Przesunięcie to numer bajtu zdarzenia. Umożliwia to odbiorcy zdarzeń (czytnikowi) określenie punktu w strumieniu zdarzeń, od którego ma zostać rozpoczęte odczytywanie zdarzeń. Przesunięcie można określić jako sygnaturę czasową lub wartość przesunięcia. Odbiorcy są zobowiązani do przechowywania własnych wartości przesunięcia poza usługą Event Hubs.

![Usługa Event Hubs](./media/event-hubs-overview/IC759861.png)

W ramach partycji każde zdarzenie zawiera przesunięcie. To przesunięcie jest używane przez odbiorców do określenia lokalizacji w sekwencji zdarzeń dla danej partycji. Przesunięcia mogą być przekazywane do centrum zdarzeń jako wartość liczbowa lub wartość znacznika czasu, gdy czytnik nawiązuje połączenie.

#### <a name="checkpointing"></a>Tworzenie punktów kontrolnych

*Tworzenie punktów kontrolnych* jest procesem, za pomocą którego czytniki oznaczają lub zatwierdzają swoją pozycję w sekwencji zdarzeń partycji. Odpowiedzialność za tworzenie punktów kontrolnych spoczywa na odbiorcy i odbywa się dla każdej partycji w ramach grupy odbiorców. Oznacza to, że dla każdej grupy odbiorców każdy czytnik partycji musi śledzić swoją bieżącą pozycję w strumieniu zdarzeń i może poinformować usługi, gdy uzna, że strumień danych jest pełny. Jeśli czytnik rozłączy się od partycji, po swoim ponownym połączeniu rozpoczyna odczyt punktu kontrolnego, który został wcześniej przesłany przez ostatni czytnik tej partycji w danej grupie odbiorców. Gdy czytnik nawiązuje połączenie, przekazuje on to przesunięcie do centrum zdarzeń, aby określić lokalizację, w której należy rozpocząć odczyt. W ten sposób można użyć procesu tworzenia punktów kontrolnych zarówno do oznaczenia zdarzeń jako „ukończone” przez aplikacje podrzędne, jak i zapewnienia odporności zdarzenia na pracę awaryjną między czytnikami działającymi na różnych komputerach. Ponieważ dane zdarzenia są zachowywane przez okres przechowywania określony w momencie tworzenia centrum zdarzeń, możliwe jest zwrócenie starszych danych, określając niższe przesunięcie z tego procesu tworzenia punktów kontrolnych. Dzięki temu mechanizmowi tworzenie punktów kontrolnych zapewnia zarówno odporność na pracę w trybie failover, jak i kontrolowane powtarzanie strumienia zdarzeń.

#### <a name="common-consumer-tasks"></a>Typowe zadania odbiorców

W tej sekcji opisano typowe zadania odbiorców zdarzeń lub czytników usługi Event Hubs. Wszyscy odbiorcy usługi Event Hubs nawiązują połączenie za pomocą protokołu AMQP 1.0. Protokół AMQP 1.0 stanowi sesję i dwukierunkowy kanał komunikacyjny zależny od stanu. Każda partycja zawiera sesję linku protokołu AMQP 1.0, która ułatwia transport zdarzeń rozdzielonych według partycji.

##### <a name="connect-to-a-partition"></a>Nawiązywanie połączenia z partycją

Aby korzystać ze zdarzeń z centrum zdarzeń, odbiorca musi nawiązać połączenie z partycją. Jak już wspomniano, dostęp do partycji zawsze odbywa się za pośrednictwem grupy odbiorców. W ramach partycjonowanego modelu odbiorców tylko jeden czytnik powinien być aktywny w partycji w danym momencie w obrębie grupy odbiorców. Popularną praktyką jest, aby podczas nawiązywania bezpośredniego połączenia z partycjami używany był mechanizm dzierżawienia w celu skoordynowania połączeń czytników z określonymi partycjami. Dzięki temu możliwe jest, aby dla każdej partycji w grupie odbiorców istniał tylko jeden aktywny czytnik. Zarządzanie pozycją w sekwencji dla czytnika jest ważnym zadaniem, które odbywa się za pośrednictwem procesu tworzenia punktów kontrolnych. Ta funkcja jest upraszczana przez użycie klasy [EventProcessorHost](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.eventprocessorhost.aspx) dla klientów programu .NET. [EventProcessorHost](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.eventprocessorhost.aspx) to inteligentny agent odbiorcy, który jest opisany w następnej sekcji.

##### <a name="read-events"></a>Zdarzenia odczytywania

Po otwarciu sesji i linku protokołu AMQP 1.0 dla określonej partycji zdarzenia są dostarczane do klienta protokołu AMQP 1.0 przez usługę Event Hubs. Ten mechanizm dostarczania zapewnia wyższą przepływność i mniejsze opóźnienia niż mechanizmy oparte na ściąganiu, na przykład HTTP GET. Podczas wysyłania zdarzeń do klienta każde wystąpienie danych zdarzenia zawiera ważne metadane, takie jak przesunięcie i numer sekwencji, które są używane do ułatwienia tworzenia punktów kontrolnych sekwencji zdarzeń.

![Usługa Event Hubs](./media/event-hubs-overview/IC759862.png)

Użytkownik odpowiada za zarządzanie tym przesunięciem w sposób, który zapewnia największy postęp zarządzania dotyczący przetwarzania strumienia.

## <a name="capacity-and-security"></a>Pojemność i zabezpieczenia

Usługa Event Hubs to wysoce skalowalna architektura równoległa służąca do transferu danych przychodzących strumienia. W związku z tym istnieje kilka kluczowych aspektów, które należy uwzględnić podczas doboru wielkości i skalowania rozwiązania opartego na usłudze Event Hubs. Pierwszymi z elementów sterujących pojemnością są *jednostki przepływności*, które zostały opisane w poniższej sekcji.

### <a name="throughput-units"></a>Jednostki przepływności

Pojemność przepływności usługi Event Hubs jest kontrolowana przez jednostki przepływności. Jednostki przepływności to zakupione wcześniej jednostki pojemności. Pojedyncza jednostka przepływności ma następujące parametry:

- Transfer danych przychodzących: maksymalnie 1 MB na sekundę lub 1000 zdarzeń na sekundę.

- Transfer danych wychodzących: maksymalnie 2 MB na sekundę.

Transfer danych przychodzących jest ograniczany do pojemności zapewnianej przez liczbę zakupionych jednostek przepływności. Wysyłanie danych przekraczających te wartości spowoduje wystąpienie wyjątku „przekroczono limit przydziału”. Ta wartość to 1 MB na sekundę lub 1000 zdarzeń na sekundę, w zależności od tego, co będzie miało miejsce wcześniej. Transfer danych wychodzących nie powoduje generowania wyjątków ograniczania przepływności, ale jest ograniczony do wielkości transferu danych zgodnie z zakupionymi jednostkami przepływności: 2 MB na sekundę na jednostkę przepływności. Jeśli wystąpią wyjątki szybkości publikowania lub oczekiwany będzie większy transfer danych wychodzących, należy sprawdzić liczbę jednostek przepływności zakupionych dla przestrzeni nazw, w której utworzono centrum zdarzeń. Aby uzyskać więcej jednostek przepływności, możesz dostosować ustawienie na stronie **Przestrzenie nazw** na stronie na karcie **Skala** w [klasycznej witrynie Azure Portal][]. Możesz również zmienić to ustawienie za pomocą interfejsów API platformy Azure.

Podczas gdy partycje to pojęcie związane z organizowaniem danych, to jednostki przepływności dotyczą wyłącznie pojemności. Jednostki przepływności są rozliczane co godzinę i są kupowane wcześniej. Po zakupieniu jednostki przepływności są rozliczane za co najmniej jedną godzinę. Dla przestrzeni nazw usługi Event Hubs można kupić maksymalnie 20 jednostek przepływności, a dla konta platformy Azure istnieje limit 20 jednostek przepływności. Te jednostki przepływności są współużytkowane przez wszystkie usługi Event Hubs w danej przestrzeni nazw.

Jednostki przepływności są udostępniane na podstawie bieżącej sytuacji i ich natychmiastowy zakup może być niemożliwy. Jeśli potrzebujesz określonej pojemności, zaleca się wcześniejsze zakupienie tych jednostek przepływności. Jeśli potrzebujesz więcej niż 20 jednostek przepływności, możesz nawiązać kontakt z pomocą techniczną platformy Azure, aby kupić więcej jednostek przepływności na podstawie zobowiązania w blokach po 20 jednostek przepływności, aż do 100 pierwszych jednostek przepływności. Ponadto możesz kupować bloki po 100 jednostek przepływności.

Zalecane jest staranne równoważenie jednostek przepływności i partycji w celu osiągnięcia optymalnej skali w ramach usługi Event Hubs. Jedna partycja ma maksymalną skalę wynoszącą jedną jednostkę przepływności. Liczba jednostek przepływności powinna być mniejsza lub równa liczbie partycji w ramach centrum zdarzeń.

Aby uzyskać szczegółowe informacje o cenach, zobacz [Usługa Event Hubs — cennik](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="publisher-policy"></a>Zasady wydawcy

Usługa Event Hubs umożliwia szczegółową kontrolę nad wydawcami zdarzeń za pomocą *zasad wydawcy*. Zasady wydawcy to zestaw funkcji środowiska uruchomieniowego zaprojektowany w celu ułatwienia działania dużej liczby niezależnych wydawców zdarzeń. Dzięki zasadom wydawcy każdy wydawca używa swojego unikatowego identyfikatora podczas publikowania zdarzeń w centrum zdarzeń przy użyciu następującego mechanizmu:

    //<my namespace>.servicebus.windows.net/<event hub name>/publishers/<my publisher name>

Nie jest konieczne wcześniejsze tworzenie nazw wydawców, ale muszą one być zgodne z tokenem sygnatury dostępu współdzielonego użytym podczas publikowania zdarzenia w celu zapewnienia niezależnych tożsamości wydawcy. Aby uzyskać więcej informacji na temat sygnatury dostępu współdzielonego, zobacz [Shared Access Signature Authentication with Service Bus](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) (Uwierzytelnianie za pomocą sygnatury dostępu współdzielonego przy użyciu usługi Service Bus). Podczas używania zasad wydawcy wartość **PartitionKey** jest ustawiana na nazwę wydawcy. Aby zapewnić prawidłowe działanie, te wartości muszą być zgodne.

## <a name="summary"></a>Podsumowanie

Usługa Azure Event Hubs udostępnia usługę przetwarzania zdarzeń i telemetrii w hiperskali, której można użyć do wspólnego monitorowania przepływu aplikacji i użytkownika na dowolnym poziomie. Dzięki udostępnieniu możliwości publikowania/subskrypcji z niskim opóźnieniem i na bardzo dużą skalę usługa Event Hubs służy jako „wjazd” dla danych big data. Za pomocą tożsamości opartej na wydawcy i list odwołania te funkcje zostały rozszerzone do postaci typowych scenariuszy Internetu rzeczy (IoT). Aby uzyskać więcej informacji na temat tworzenia aplikacji usługi Event Hubs, zobacz [Event Hubs programming guide](event-hubs-programming-guide.md) (Przewodnik dotyczący programowania w usłudze Event Hubs).

## <a name="next-steps"></a>Następne kroki

Teraz, kiedy znasz już pojęcia związane z usługą Event Hubs, możesz przejść do następujących scenariuszy:

- Wprowadzenie do [usługi Event Hubs — samouczek].
- Kompletna [przykładowa aplikacja korzystająca z usługi Event Hubs].

[Klasyczna witryna Azure Portal]: http://manage.windowsazure.com
[Samouczek dotyczący usługi Event Hubs]: event-hubs-csharp-ephcs-getstarted.md
[przykładowa aplikacja korzystająca z usługi Event Hubs]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Event-Hub-286fd097



<!--HONumber=Oct16_HO3-->


