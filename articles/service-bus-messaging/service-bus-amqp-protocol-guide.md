---
title: "Protokołu AMQP 1.0 w przewodniku protokołu usługi Azure Service Bus i usługi Event Hubs | Dokumentacja firmy Microsoft"
description: "Przewodnik dotyczący protokołu wyrażeń i opis protokołu AMQP 1.0 usługi Azure Service Bus i usługi Event Hubs"
services: service-bus-messaging,event-hubs
documentationcenter: .net
author: clemensv
manager: timlt
editor: 
ms.assetid: d2d3d540-8760-426a-ad10-d5128ce0ae24
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2017
ms.author: clemensv;hillaryc;sethm
ms.openlocfilehash: 4e1fa9db3b4801103069163c55a9b342a27d00ac
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# Protokołu AMQP 1.0 w przewodniku protokołu usługi Azure Service Bus i usługi Event Hubs

1.0 protokołu zaawansowane usługi kolejkowania komunikatów jest standardowe protokoły ramek i transfer asynchronicznie, bezpiecznego i niezawodnego przesyłania komunikatów między dwiema stronami. Jest podstawowy protokół Azure Service Bus Messaging i usługi Azure Event Hubs. Obie te usługi również obsługuje protokół HTTPS. Zastrzeżonym protokołem SBMP, który jest również obsługiwany jest obecnie wycofywane na rzecz protokołu AMQP.

Protokołu AMQP 1.0 jest wynikiem współpraca z branży szerokie zgromadzone dostawców oprogramowania pośredniczącego, takie jak Microsoft i Red Hat z wieloma użytkownikami oprogramowanie pośredniczące obsługi wiadomości, takie jak Morgan Chase JP reprezentujący branży usług finansowych. Forum normalizacji techniczne dla specyfikacji protokołu i rozszerzenie protokołu AMQP jest OASIS i został osiągnięty posiadanie zatwierdzania międzynarodowe standardem ISO/IEC 19494.

## Cele

W tym artykule krótko zawiera podsumowanie podstawowych pojęciach protokołu AMQP 1.0 wiadomości specyfikacji wraz z zestawem małych specyfikacji rozszerzenia projektu, które są aktualnie zatwierdzony w Komisji techniczne OASIS AMQP oraz wyjaśniono, jak usługi Azure Service Bus implementuje i tworzy z tymi specyfikacjami.

Celem jest dla Każdy deweloper przy użyciu dowolnego istniejącego stosu klienta protokołu AMQP 1.0 na dowolnej platformie mieć możliwość interakcji z usługi Azure Service Bus za pośrednictwem protokołu AMQP 1.0.

Typowe ogólnego przeznaczenia stosy protokołu AMQP 1.0, takie jak Apache protonów lub AMQP.NET Lite zaimplementować już wszystkie protokoły core protokołu AMQP 1.0. Tych podstawowych gestów czasami są ujęte w wyższego poziomu API; Apache protonów nawet oferuje dwa imperatywnych API Messenger i reaktywne API reaktora.

W poniższych kwestii przyjęto założenie, zarządzania połączenia protokołu AMQP, sesji, i łącza i obsługi transferów ramki i sterowanie przepływem są obsługiwane przez odpowiednich stosu (takich jak Apache protonów-C) i nie wymagają znacznie, jeśli istnieją szczególną uwagę deweloperów aplikacji. Przyjęto założenie, abstrakcyjnie istnienie kilka interfejsu API w nim elementów podstawowych takich jak możliwość nawiązania połączenia i utworzyć jakiegoś typu *nadawcy* i *odbiornika* obiekty abstrakcji, który następnie mają niektóre kształt `send()` i `receive()` operacje, odpowiednio.

Omawiając zaawansowanych możliwości usługi Azure Service Bus, takich jak przeglądanie wiadomości lub zarządzania sesjami, te funkcje zostały omówione w kategoriach protokołu AMQP, ale także jako warstwowych pseudo-implementacji u góry tego zakładanego abstrakcji interfejsu API.

## Co to jest AMQP?

Protokół AMQP jest protokołem ramek i transferu. Ramek oznacza zawiera struktury, dla strumieni danych binarnych, które wpływają w żadnym kierunku połączenia sieciowego. Struktura zapewnia nakreślenia różne bloków danych o nazwie *ramki*, aby być wymieniane między połączonych stron. Możliwości transferu upewnij się, ustanowić udostępnione opis o kiedy ramki są przekazywane, a podczas transferów uznaje się pełną zarówno komunikującymi się Stronami.

W przeciwieństwie do wersji wcześniej wygasłe projekt produkowane przez grupę roboczą protokołu AMQP, które są nadal używane przez kilka brokerzy wiadomości protokołu AMQP 1.0 ostateczne i standardowych grupy roboczej nie określają obecności brokera komunikatów lub dowolnej określonej topologii dla jednostek wewnątrz brokera komunikatów.

Protokół może służyć do symetrycznego komunikacji peer-to-peer, interakcji z brokerzy komunikatów, obsługujące kolejek i publikowania/subskrypcji jednostek, tak jak w przypadku usługi Azure Service Bus. Może również służyć do interakcji z infrastrukturą obsługi wiadomości gdzie wzorce interakcji różnią się od regularne kolejek, jak w przypadku usługi Azure Event Hubs. Centrum zdarzeń działa podobnie jak kolejka, gdy zdarzenia są wysyłane do niego, ale więcej działa jak Usługa magazynu serial podczas zdarzenia są odczytywane z nieco jest podobny stacji taśm. Klient odbiera przesunięcia do strumienia dostępnych danych i jest następnie udostępniany wszystkie zdarzenia z tym przesunięcie do najnowszej dostępnej.

Protokół AMQP 1.0 ma być rozszerzalny, umożliwiające dalsze specyfikacje w celu zwiększenia jego możliwości. Specyfikacje trzy rozszerzenia omówiony w niniejszym dokumencie ilustrację. Do komunikacji w ramach istniejącej infrastruktury protokołu HTTPS/Websocket, gdzie Konfigurowanie portów TCP protokołu AMQP natywnych może być trudne Specyfikacja powiązania definiuje sposób warstwy protokołu AMQP przez protokół WebSockets. Do interakcji z infrastrukturą obsługi wiadomości w czasie żądania/odpowiedzi do celów zarządzania lub w celu zapewnienia zaawansowanych funkcji, specyfikacji protokołu AMQP zarządzania definiuje wymagana interakcja podstawowe w nim elementów podstawowych. Integracji modelu autoryzacji federacyjnych Specyfikacja oświadczenia na podstawie zabezpieczeń protokołu AMQP definiuje sposób skojarzenia i odnawiania tokenów autoryzacji skojarzone z łącza.

## Podstawowe scenariusze protokołu AMQP

W tej sekcji opisano podstawowe sposoby użycia protokołu AMQP 1.0 z usługi Azure Service Bus, w tym tworzenie połączeń, sesje i linki i transferu wiadomości do i z jednostek usługi Service Bus, takich jak kolejki, tematy i subskrypcje.

Specyfikacja protokołu AMQP 1.0 jest najczęściej wiarygodne źródło, aby dowiedzieć się więcej na temat działania protokołu AMQP, ale specyfikację zostało zapisane dokładnie Przewodnik wdrożenia, a nie uczy protokołu. W tej sekcji koncentruje się na wprowadzenie tyle terminologii zgodnie z potrzebami dla opisujące, jak Usługa Service Bus używa protokołu AMQP 1.0. Aby uzyskać bardziej obszerne protokołu AMQP, a także szersze omówienie protokołu AMQP 1.0, możesz przejrzeć [tego kursu wideo][this video course].

### Połączenia i sesji

Protokół AMQP wymaga komunikacji programy *kontenery*; te zawierają *węzłów*, które są jednostkami komunikacji wewnątrz tych kontenerach. Kolejki mogą być takie węzła. Protokół AMQP umożliwia Multipleksowanie, dzięki pojedynczego połączenia mogą być używane dla wielu ścieżek komunikacji między węzłami. na przykład jednocześnie klienta aplikacji otrzymywać jednej kolejki i wysyłanie do innej kolejki przez to samo połączenie sieciowe.

![][1]

W związku z tym połączenie sieciowe jest zakotwiczony kontenera. Zainicjowaniu przez kontener w roli klienta wprowadzania wychodzące połączenie gniazda TCP do kontenera w roli odbiornik, która wykrywa i akceptuje połączenia TCP dla ruchu przychodzącego. Uzgadnianie połączenia obejmuje negocjowania wersja protokołu, zgłaszając lub negocjowania protokołu Transport Level Security (TLS/SSL) a uzgadnianie uwierzytelnianie/autoryzacja w zakresie połączenia, który jest oparty na SASL.

Usługa Azure Service Bus wymaga użycia protokołu TLS przez cały czas. Obsługuje połączenia za pośrednictwem portu TCP 5671, według którego połączenie TCP jest najpierw umieszczenia z protokołem TLS przed wprowadzeniem uzgadnianie protokołu AMQP i obsługuje także połączenia za pośrednictwem portu TCP 5672 zgodnie z którą serwer natychmiast oferuje obowiązkowe uaktualnienia połączenia protokołu TLS przy użyciu określonych AMQP modelu. Powiązanie protokołu AMQP Websocket tworzy tunel za pośrednictwem portu TCP 443, który następnie jest odpowiednikiem połączenia protokołu AMQP 5671.

Po skonfigurowaniu połączenia i TLS, usługi Service Bus oferuje dwie opcje mechanizmu SASL:

* ZWYKŁY SASL jest najczęściej używany do przekazywania poświadczeń użytkownika i hasło do serwera. Usługi Service Bus nie mają kont, ale nazwane [zasady zabezpieczenia dostępu do udostępnionych](service-bus-sas.md), która przyznaje prawa i są skojarzone z kluczem. Nazwa reguły jest używana jako nazwa użytkownika i klucza (jako tekst kodowany w formacie base64) jest używany jako hasło. Praw skojarzonych z wybranym reguły określają operacje dozwolone połączenia.
* SASL ANONIMOWEGO jest używana do pomijanie SASL autoryzacji, gdy klient chce używać modelu (CBS) oświadczenia na podstawie zabezpieczeń opisaną później. Po wybraniu tej opcji połączenie klienta można ustanowić anonimowo przez krótki czas, w którym klient może komunikować się tylko z punktem końcowym CBS i wykonaj uzgadnianie CBS.

Po nawiązaniu połączenia transportowego kontenery każdego zadeklarować maksymalny rozmiar ramki, które chcą obsługi, a po limit czasu bezczynności one będzie jednostronnie przerywaj nic się nie w połączeniu.

One zadeklarować, jak wiele równoczesnych kanały są obsługiwane. Kanał jest ścieżką jednokierunkowe, wychodzącego wirtualnego transferu na połączenie. Sesja trwa kanału z każdej z połączonych kontenerów do utworzenia ścieżki komunikację dwukierunkową.

Sesje oferują model sterowania przepływem na podstawie okna; Podczas tworzenia sesji, strony deklaruje liczbę ramek jest gotowa do akceptowania do jej okna odbierania. Jak ramki exchange strony, wypełnienie przesłane ramkami, że okna i transferów zatrzymany po zapełnieniu okna i dopóki okno pobiera resetowania lub przy użyciu rozszerzonych *przepływ performative* (*performative* jest określenie protokołu AMQP gestów poziomu protokołu wymieniane między dwiema stronami).

Tego okna na podstawie modelu jest z grubsza podobny do koncepcji TCP sterowaniu przepływem na podstawie okna, ale na poziomie sesji wewnątrz gniazda. Zezwala na wiele równoczesnych sesji protokołu koncepcji istnieje tak, aby ruch o wysokim priorytecie można presją poza ograniczeniem przepustowości normalnym ruchu, jak na autostrady lane express.

Obecnie usługa Azure Service Bus używa dokładnie jednej sesji dla każdego połączenia. Usługa Service Bus maksymalną — rozmiar ramki jest 262 144 bajty (256 KB) dla usługi magistrali Standard i usługi Event Hubs. Jest 1 048 576 (1 MB) dla usługi Service Bus w warstwie Premium. Usługa Service Bus nie nakłada oknami ograniczania przepustowości określonego poziomu sesji, ale resetuje okna regularnie w ramach kontroli przepływu na poziomie łącza (zobacz [następnej sekcji](#links)).

Połączenia, kanałów i sesje są tymczasowych. Jeśli połączenie podstawowe zwija połączeń, musi przywróciła tunelu TLS, kontekst autoryzacji SASL i sesji.

### Linki

Protokół AMQP przesyła komunikaty za pośrednictwem łącza. Łącze jest ścieżką komunikacji utworzonych w sesji, która umożliwia przesyłania komunikatów w jednym kierunku; Negocjowanie stanu transferu jest za pośrednictwem łącza i dwukierunkową między stronami połączonych.

![][2]

Łącza mogą być tworzone przy użyciu albo kontenera, w dowolnym momencie, a w istniejącej sesji, dzięki czemu AMQP różne od wielu innych protokołów, w tym HTTP i MQTT, gdzie inicjowania transferu i ścieżki transferu jest wyłączne uprawnień strony tworzenia połączenia gniazda.

Inicjowanie łącza kontenera zadaje przeciwną kontener, aby zaakceptować łącze i wybiera roli nadawcy i adresata. W związku z tym kontenera można zainicjować tworzenia jednokierunkowe lub komunikacja dwukierunkowa ścieżek, drugie modelowane jako pary łącza.

Łącza są o nazwie i skojarzone z węzłów. Zgodnie z początku, węzły są jednostkami komunikacji wewnątrz kontenera.

W usłudze Service Bus węzła odpowiada bezpośrednio kolejki, tematu, subskrypcji lub podkolejki subskrypcji lub kolejki utraconych wiadomości. Nazwa węzła używane w AMQP w związku z tym jest względna nazwa encji wewnątrz przestrzeni nazw usługi Service Bus. Jeśli nosi nazwę kolejki **Moja_kolejka**, jest to nazwa węzła jego protokołu AMQP. Subskrypcję tematu następuje Konwencji HTTP API przez są posortowane w kolekcji zasobów "subskrypcji" i w związku z tym subskrypcji **sub** lub temat **mytopic** ma nazwę węzła AMQP **mytopic/subskrypcji/sub**.

Klient nawiązujący połączenie jest również wymagane do używania nazwy węzła lokalnego do tworzenia łączy; Magistrala usług nie jest przetestowanego o te nazwy węzła i nie zinterpretowane. Stosy klienta protokołu AMQP 1.0 zazwyczaj korzystają ze schematu aby mieć pewność, które te nazwy węzłów tymczasowych są unikatowe w zakresie klienta.

### Transfery

Po ustanowieniu łącza można przesyłać komunikaty za pośrednictwem tego łącza. W protokołu AMQP, transfer jest wykonywane z gestów jawne protokołu ( *transferu* performative) który przenosi wiadomości od nadawcy do odbiorcy za pośrednictwem łącza. Przeniesienie została ukończona, gdy jej "rozliczenia", co oznacza, że obie strony zostało ustanowione udostępnionego zrozumienia wyniku tego przeniesienia.

![][3]

Ogólnie rzecz biorąc nadawca można wysyłać wiadomości "wstępnie rozliczane", co oznacza, że klient nie jest zainteresowana wyniku i odbiorca nie ma żadnych swoją opinię na temat wynik operacji. Tryb ten jest obsługiwany przez usługi Service Bus na poziomie protokołu AMQP, ale nie są widoczne w żadnym z interfejsów API klienta.

Regularne zdarza się, że komunikaty są wysyłane nierozliczonych i odbiornika następnie wskazuje zatwierdzenia lub odrzucenia przy użyciu *dyspozycji* performative. Odrzucenia występuje, gdy odbiorca nie może zaakceptować komunikat przyczyn odrzucenia komunikat zawiera informacje o przyczynie, który jest strukturą błąd wynika z protokołu AMQP. Jeśli wiadomości zostały odrzucone z powodu błędów wewnętrznych wewnątrz usługi Service Bus, usługa zwraca dodatkowe informacje w tej struktury, który może służyć do prezentowania wskazówek diagnostyki personelowi pomocy technicznej, jeśli są zgłoszenia żądania pomocy technicznej. Więcej informacji o błędach dowiesz się później.

Specjalna forma odrzucenia jest *wydane* stanie, co oznacza, że odbiorca ma zastrzeżeń techniczne do przeniesienia, ale również nie zainteresowania proces transferu. Tego przypadku istnieje, na przykład, gdy komunikat jest dostarczany do klienta usługi Service Bus, a klient wybiera "został porzucony" wiadomości, ponieważ nie można wykonać pracy spowodowanych przetworzeniu komunikatu; dostarczanie komunikatów, sama nie jest uszkodzone. Zmiana tego stanu jest *zmodyfikować* stanu, dzięki czemu zmiany do wiadomości, zgodnie z jego zwolnienia. Ten stan nie jest używany przez magistralę usług w chwili obecnej.

Specyfikacja protokołu AMQP 1.0 definiuje dodatkowe dyspozycji stanu wywołuje *Odebrano*, obsługującej pomaga do obsługi odzyskiwania łącza. Łącza odzyskiwania umożliwia przywracanie stanu łącza i wszelkie oczekujące wysyłki na nowe połączenie i sesji, gdy poprzednie połączenie i sesji zostały utracone.

Usługa Service Bus nie obsługuje odzyskiwania łącze; Jeśli klient utraci połączenie do usługi Service Bus z komunikatem nierozliczonych transfer oczekujące, transfer tej wiadomości zostaną utracone i klient musi ponownie połączyć, ponownie ustanowić połączenie i spróbuj ponownie transferu.

W efekcie usługi Service Bus i usługi Event Hubs obsługuje "co najmniej raz" transferów, których należy zapewnić wiadomości przechowywane i zaakceptowane nadawcy, ale nie obsługują "dokładnie jednokrotne" transferów na poziomie protokołu AMQP, gdy system będzie próbę odzyskania łącze i przejdź do negocjowania stan dostawy, aby uniknąć duplikowania transferu komunikatów.

Kompensacji możliwy duplikat wysyła, Usługa Service Bus obsługuje wykrywania duplikatów jako opcjonalna funkcja w kolejek i tematów. Wykrywanie duplikatów rejestruje identyfikatory komunikat komunikaty przychodzące w oknie czasu zdefiniowanych przez użytkownika, a następnie dyskretnie porzuca wszystkie wiadomości wysyłane z takich samych identyfikatorów wiadomości w tym samym oknie.

### Przepływ sterowania

Oprócz model kontroli przepływu na poziomie sesji wspomnianej wcześniej każde łącze ma własną model kontroli przepływu. Sterowanie przepływem poziomu sesji uniemożliwia konieczności obsługi zbyt wiele ramek na po sterowanie przepływem na poziomie łącza umieszcza aplikacji odpowiedzialnym za liczbę komunikatów, które chce obsługiwać z łącza, a gdy kontenera.

![][4]

Łącze, transferów tylko zdarzyć, gdy nadawca ma wystarczającą ilość *link środki*. Środki łącze jest zestaw przez odbiornik. przy użyciu liczników *przepływ* performative, której zakres to łącze. Nadawca przypisany środki łącza, próbuje Użyj tego środki przez dostarczanie wiadomości. Każdy komunikat dostarczania zmniejsza pozostałych łącze karty kredytowej przez 1. W przypadku środków łączy zatrzymać dostaw.

Kiedy roli odbiornik usługi Service Bus, usługa natychmiast udostępnia nadawcy środki wystarczającą łącze tak, aby natychmiast można wysyłać wiadomości. Jak używany środki łącze usługi Service Bus czasami wysyła *przepływ* performative nadawcy, aby zaktualizować salda środków łącza.

W roli nadawcy usługi Service Bus wysyła komunikaty do użycia w górę wszelkie środki oczekujących łącza.

Wywołanie "otrzymywać" na poziomie interfejsu API przekłada się na *przepływ* performative są wysyłane do usługi Service Bus przez klienta i usługi Service Bus zużywa tego środki przez pobranie pierwszej dostępna, odblokowany wiadomości z kolejki, blokowanie go i przenoszenia go. Jeśli nie ma jeszcze dostępna w celu dostarczania, wszelkie zaległe środki w dowolnym łączu nawiązać z danego podmiotu pozostaje zarejestrowane w kolejności odbioru, czy komunikaty są zablokowane i przekazywane staną się one dostępne, aby użyć wszelkie zaległe środki.

Blokady na komunikat o zwolnieniu podczas transferu jest rozliczany do jednego z terminala stanów *zaakceptowane*, *odrzucone*, lub *wydane*. Wiadomość zostanie usunięta z usługi Service Bus, gdy jest w stanie terminali *zaakceptowane*. Pozostaje w usłudze Service Bus i są dostarczane do następnego odbiornik, gdy transfer osiągnie jednym z innych stanów. Po osiągnięciu liczby dostarczania maksymalny dozwolony dla obiektu z powodu odrzucenia powtórzony lub wersje usługi Service Bus automatycznie przenosi wiadomości do kolejki utraconych wiadomości jednostki.

Mimo że interfejsów API usługi Service Bus nie ujawniaj bezpośrednio takiej opcji dzisiaj, klienta protokołu AMQP niższego poziomu można użyć modelu kredytów łącze aby włączyć interakcji "ściągania style" wydawania jedną jednostkę środków dla każdego żądania odbierania model "wypychania style" przez wystawienie dużej liczby środków łącza, a następnie odbierać wiadomości w miarę ich wprowadzania bez dalszej interakcji. Push jest obsługiwana przez [MessagingFactory.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PrefetchCount) lub [MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount) ustawienia właściwości. Gdy są one inną niż zero, klienta protokołu AMQP używa go jako środków łącza.

W tym kontekście jest wziąć pod uwagę, że zegara wygasania blokady dla komunikatu wewnątrz jednostki rozpoczyna się, gdy wiadomość jest pobierana z obiektu, nie kiedy wiadomość zostanie umieszczona w sieci. Zawsze, gdy klient wskazuje gotowości do odbierania wiadomości przez wystawienie środki łącza, dlatego należy się spodziewać aktywnie można ściąganie wiadomości w sieci i być gotowy do ich obsługi. W przeciwnym razie blokady komunikat mogło wygasnąć przed dostarczeniem nawet wiadomości. Użyj łącza środki kontroli przepływu bezpośrednio powinien odzwierciedlać natychmiastowego gotowości radzenia sobie z dostępnych wiadomości wysyłane do odbiornika.

Podsumowując w poniższych sekcjach schematyczny omówienie przepływu performative podczas interakcje interfejsu API. Każda sekcja zawiera opis różnych operacji logicznej. Niektóre z tych interakcji, może być "opóźnieniem," oznacza, że ich może być wykonana tylko na żądanie. Tworzenie nadawcy wiadomości nie może powodować interakcji sieci do czasu pierwszego komunikatu wysłanego lub wymagane.

Strzałki w poniższej tabeli oznaczają kierunek przepływu performative.

#### Tworzenie odbiornika wiadomości

| Klient | Service Bus |
| --- | --- |
| --> dołączyć ()<br/>Nazwa = {Nazwa łącza}<br/>Obsługa = {dojścia liczbowych},<br/>Rola =**odbiornika**,<br/>Źródło = {nazwa jednostki}<br/>docelowy = {identyfikator klienta link}<br/>) |Klient dołącza do jednostki jako odbiornik |
| Dołączanie jej końcu łącza odpowiedzi usługi Service Bus |<--dołączyć ()<br/>Nazwa = {Nazwa łącza}<br/>Obsługa = {dojścia liczbowych},<br/>Rola =**nadawcy**,<br/>Źródło = {nazwa jednostki}<br/>docelowy = {identyfikator klienta link}<br/>) |

#### Tworzenie nadawcy wiadomości

| Klient | Service Bus |
| --- | --- |
| --> dołączyć ()<br/>Nazwa = {Nazwa łącza}<br/>Obsługa = {dojścia liczbowych},<br/>Rola =**nadawcy**,<br/>Źródło = {klienta łącza id}<br/>docelowy = {nazwa jednostki}<br/>) |Żadna akcja ze strony |
| Żadna akcja ze strony |<--dołączyć ()<br/>Nazwa = {Nazwa łącza}<br/>Obsługa = {dojścia liczbowych},<br/>Rola =**odbiornika**,<br/>Źródło = {klienta łącza id}<br/>docelowy = {nazwa jednostki}<br/>) |

#### Tworzenie nadawcy wiadomości (błąd)

| Klient | Service Bus |
| --- | --- |
| --> dołączyć ()<br/>Nazwa = {Nazwa łącza}<br/>Obsługa = {dojścia liczbowych},<br/>Rola =**nadawcy**,<br/>Źródło = {klienta łącza id}<br/>docelowy = {nazwa jednostki}<br/>) |Żadna akcja ze strony |
| Żadna akcja ze strony |<--dołączyć ()<br/>Nazwa = {Nazwa łącza}<br/>Obsługa = {dojścia liczbowych},<br/>Rola =**odbiornika**,<br/>Źródło o wartości null,<br/>docelowy = null<br/>)<br/><br/><--odłączyć ()<br/>Obsługa = {dojścia liczbowych},<br/>zamknięte =**true**,<br/>błąd = {informacje o błędzie}<br/>) |

#### Komunikat o zamykaniu odbiornik/nadawcy

| Klient | Service Bus |
| --- | --- |
| --> odłączyć ()<br/>Obsługa = {dojścia liczbowych},<br/>zamknięte =**true**<br/>) |Żadna akcja ze strony |
| Żadna akcja ze strony |<--odłączyć ()<br/>Obsługa = {dojścia liczbowych},<br/>zamknięte =**true**<br/>) |

#### Wyślij (Powodzenie)

| Klient | Service Bus |
| --- | --- |
| --> transfer (<br/>Identyfikator dostawy = {dojścia liczbowych},<br/>tag dostarczania = {binary dojścia}<br/>rozliczenia =**false**,, więcej =**false**,<br/>Stan =**null**,<br/>Wznów =**false**<br/>) |Żadna akcja ze strony |
| Żadna akcja ze strony |<--(dyspozycji<br/>Rola odbiornik,<br/>najpierw = {identyfikator dostawy}<br/>ostatnio = {identyfikator dostawy}<br/>rozliczenia =**true**,<br/>Stan =**zaakceptowane**<br/>) |

#### Wyślij (błąd)

| Klient | Service Bus |
| --- | --- |
| --> transfer (<br/>Identyfikator dostawy = {dojścia liczbowych},<br/>tag dostarczania = {binary dojścia}<br/>rozliczenia =**false**,, więcej =**false**,<br/>Stan =**null**,<br/>Wznów =**false**<br/>) |Żadna akcja ze strony |
| Żadna akcja ze strony |<--(dyspozycji<br/>Rola odbiornik,<br/>najpierw = {identyfikator dostawy}<br/>ostatnio = {identyfikator dostawy}<br/>rozliczenia =**true**,<br/>Stan =**odrzucone**()<br/>błąd = {informacje o błędzie}<br/>)<br/>) |

#### Receive

| Klient | Service Bus |
| --- | --- |
| --> (przepływu<br/>łącze kredytów = 1<br/>) |Żadna akcja ze strony |
| Żadna akcja ze strony |< transfer ()<br/>Identyfikator dostawy = {dojścia liczbowych},<br/>tag dostarczania = {binary dojścia}<br/>rozliczenia =**false**,<br/>więcej =**false**,<br/>Stan =**null**,<br/>Wznów =**false**<br/>) |
| --> dyspozycji)<br/>Rola =**odbiornika**,<br/>najpierw = {identyfikator dostawy}<br/>ostatnio = {identyfikator dostawy}<br/>rozliczenia =**true**,<br/>Stan =**zaakceptowane**<br/>) |Żadna akcja ze strony |

#### Odbieranie komunikatu wielu

| Klient | Service Bus |
| --- | --- |
| --> (przepływu<br/>łącze kredytów = 3<br/>) |Żadna akcja ze strony |
| Żadna akcja ze strony |< transfer ()<br/>Identyfikator dostawy = {dojścia liczbowych},<br/>tag dostarczania = {binary dojścia}<br/>rozliczenia =**false**,<br/>więcej =**false**,<br/>Stan =**null**,<br/>Wznów =**false**<br/>) |
| Żadna akcja ze strony |< transfer ()<br/>Identyfikator dostawy = {dojścia liczbowych + 1},<br/>tag dostarczania = {binary dojścia}<br/>rozliczenia =**false**,<br/>więcej =**false**,<br/>Stan =**null**,<br/>Wznów =**false**<br/>) |
| Żadna akcja ze strony |< transfer ()<br/>Identyfikator dostawy = {dojścia liczbowych + 2},<br/>tag dostarczania = {binary dojścia}<br/>rozliczenia =**false**,<br/>więcej =**false**,<br/>Stan =**null**,<br/>Wznów =**false**<br/>) |
| --> dyspozycji)<br/>Rola odbiornik,<br/>najpierw = {identyfikator dostawy}<br/>ostatnio = {identyfikator dostawy + 2},<br/>rozliczenia =**true**,<br/>Stan =**zaakceptowane**<br/>) |Żadna akcja ze strony |

### Komunikaty

W poniższych sekcjach opisano właściwości z sekcji standardowy komunikat protokołu AMQP, które są używane przez usługi Service Bus oraz sposobu mapowania ich w zestawie interfejsu API usługi Service Bus.

#### nagłówek

| Nazwa pola | Sposób użycia | Nazwa interfejsu API |
| --- | --- | --- |
| trwałe |- |- |
| Priorytet |- |- |
| czas wygaśnięcia |Czas wygaśnięcia dla tego komunikatu |[Wartość TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive) |
| pierwszy przejmującą |- |- |
| Liczba dostarczania |- |[DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) |

#### properties

| Nazwa pola | Sposób użycia | Nazwa interfejsu API |
| --- | --- | --- |
| Identyfikator komunikatu |Zdefiniowane przez aplikację, dowolnych identyfikator dla tego komunikatu. Używane do wykrywania duplikatów. |[Identyfikator komunikatu](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) |
| Identyfikator użytkownika |Identyfikator aplikacji zdefiniowane przez użytkownika nie jest interpretowany przez magistralę usług. |Nie jest dostępny za pośrednictwem interfejsu API usługi Service Bus. |
| na |Identyfikator docelowego zdefiniowanym przez aplikację nie interpretowany przez magistralę usług. |[Do](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_To) |
| Temat |Identyfikator celu zdefiniowanym przez aplikację wiadomości, nie jest interpretowany przez magistralę usług. |[Etykiety](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) |
| Odpowiedz do |Wskaźnik ścieżki odpowiedzi zdefiniowanym przez aplikację nie interpretowany przez magistralę usług. |[ReplyTo](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ReplyTo) |
| Identyfikator korelacji |Identyfikator korelacji zdefiniowanym przez aplikację nie interpretowany przez magistralę usług. |[CorrelationId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_CorrelationId) |
| Typ zawartości |Zdefiniowane przez aplikację wskaźnika typu zawartości dla treści nie interpretowany przez magistralę usług. |[Typ zawartości](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ContentType) |
| kodowanie zawartości |Zdefiniowane przez aplikację kodowania zawartości wskaźnik dla treści nie interpretowany przez magistralę usług. |Nie jest dostępny za pośrednictwem interfejsu API usługi Service Bus. |
| czas w przypadku wygaśnięcia bezwzględne |Deklaruje, w których bezwzględną błyskawiczne komunikat wygasa. Ignorowane w danych wejściowych (nagłówek zaobserwowano TTL) autorytatywne w danych wyjściowych. |[ExpiresAtUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ExpiresAtUtc) |
| Godzina utworzenia |Deklaruje po tym czasie wiadomość została utworzona. Nie są używane przez usługi Service Bus |Nie jest dostępny za pośrednictwem interfejsu API usługi Service Bus. |
| Identyfikator grupy |Zdefiniowane przez aplikację identyfikator powiązany zestaw komunikatów. Używane dla sesji usługi Service Bus. |[Identyfikator sesji](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) |
| grupy sekwencji |Licznik identyfikowanie Sekwencja względna liczba wiadomości w sesji. Ignorowane przez magistralę usług. |Nie jest dostępny za pośrednictwem interfejsu API usługi Service Bus. |
| Odpowiedz do grupy identyfikator |- |[ReplyToSessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ReplyToSessionId) |

## Zaawansowane możliwości usługi Service Bus

W tej sekcji omówiono zaawansowanych możliwości usługi Azure Service Bus, które są oparte na wersji roboczej rozszerzeń protokołu AMQP, obecnie opracowywane w Komisji techniczne OASIS dla protokołu AMQP. Usługa Service Bus implementuje najnowsze wersje tych projektów i przyjmuje zmiany wprowadzone w jak te wersje robocze osiągnąć standardowych.

> [!NOTE]
> Zaawansowane operacje komunikatów magistrali usług są obsługiwane za pomocą wzorca żądanie/odpowiedź. Szczegółowe informacje o tych operacji są opisane w artykule [protokołu AMQP 1.0 w usłudze Service Bus: opartych na protokole żądanie odpowiedź operacji](service-bus-amqp-request-response.md).
> 
> 

### Protokół AMQP zarządzania

Specyfikacja zarządzania AMQP jest pierwszy rozszerzenia projektu omówionych w tym artykule. Określenie tej wartości definiuje zestaw protokołów w warstwie ponad protokołu AMQP, które umożliwiają zarządzanie interakcji z infrastrukturą obsługi wiadomości za pośrednictwem protokołu AMQP. Specyfikacja definiuje ogólne operacje takie jak *utworzyć*, *odczytu*, *aktualizacji*, i *usunąć* zarządzania jednostek w infrastrukturze obsługi wiadomości i zestaw operacji zapytania.

Wszystkie te gesty wymagają interakcji żądanie/odpowiedź, między klientem a infrastruktury obsługi wiadomości i w związku z tym specyfikację definiuje sposób modelu tego wzorca interakcji u góry AMQP: klient łączy się infrastruktury obsługi wiadomości, inicjuje sesję, a następnie tworzy parę łącza. Na jedno łącze klient działa jako nadawcę i innych działa jako odbiornik, co powoduje utworzenie pary linki, które mogą działać jako kanału dwukierunkowego.

| Operacja logiczna | Klient | Service Bus |
| --- | --- | --- |
| Utworzenie żądania odpowiedzi ścieżki |--> dołączyć ()<br/>Nazwa = {*nazwę łącza*},<br/>Obsługa = {*liczbowych dojście*},<br/>Rola =**nadawcy**,<br/>źródło =**null**,<br/>docelowy = "Zarządzanie myentity / $"<br/>) |Żadna akcja ze strony |
| Utworzenie żądania odpowiedzi ścieżki |Żadna akcja ze strony |\<--dołączyć ()<br/>Nazwa = {*nazwę łącza*},<br/>Obsługa = {*liczbowych dojście*},<br/>Rola =**odbiornika**,<br/>Źródło o wartości null,<br/>docelowy = "myentity"<br/>) |
| Utworzenie żądania odpowiedzi ścieżki |--> dołączyć ()<br/>Nazwa = {*nazwę łącza*},<br/>Obsługa = {*liczbowych dojście*},<br/>Rola =**odbiornika**,<br/>Źródło = "Zarządzanie myentity / $",<br/>docelowy = "identyfikator myclient$"<br/>) | |
| Utworzenie żądania odpowiedzi ścieżki |Żadna akcja ze strony |\<--dołączyć ()<br/>Nazwa = {*nazwę łącza*},<br/>Obsługa = {*liczbowych dojście*},<br/>Rola =**nadawcy**,<br/>Źródło = "myentity"<br/>docelowy = "identyfikator myclient$"<br/>) |

Posiadanie tej pary łącza, wykonania żądania i odpowiedzi jest prosta: żądanie jest komunikat wysłany do jednostki wewnątrz infrastruktury obsługi wiadomości, która obsługuje usługę tego wzorca. W tym komunikacie żądania *odpowiedzi* w *właściwości* ustawiono sekcji *docelowego* identyfikator dla łącza, na którym dostarczyć odpowiedź. Jednostki obsługi przetwarza żądania, a następnie dostarcza odpowiedzi przez łącze których *docelowej* odpowiada wskazany identyfikator *odpowiedzi* identyfikator.

Oczywiście wzorzec wymaga, aby kontenera klienta oraz identyfikatora docelowego odpowiedzi generowane przez klientów były unikatowe przez wszystkich klientów i, ze względów bezpieczeństwa również trudna do przewidzenia.

Wymiany komunikatów protokołu zarządzania i do wszystkich protokołów, które używają tego samego wzorca odbywa się na poziomie aplikacji; nie definiują nowe gesty poziomu protokołu AMQP. Jest zamierzone, dzięki czemu aplikacje mogą korzystać natychmiastowego tych rozszerzeń z zgodne stosy protokołu AMQP 1.0.

Usługi Service Bus nie zawiera obecnie implementacji funkcji podstawowych specyfikacji zarządzania, ale wzorzec żądań i odpowiedzi w specyfikacji zarządzania jest podstawowym dla funkcji oświadczenia na podstawie zabezpieczeń i prawie wszystkie zaawansowane funkcje omówione w poniższych sekcjach.

### Autoryzacji opartej na oświadczeniach

Projekt specyfikacji protokołu AMQP oświadczeń — na podstawie-autoryzacji (CBS) oparty na wzorcu żądanie/odpowiedź specyfikacji zarządzania i opisano model uogólniony korzystania z protokołu AMQP tokenów zabezpieczeń.

Domyślny model zabezpieczeń protokołu AMQP opisano we wprowadzeniu opiera się na SASL i integruje się z uzgadniania połączenia protokołu AMQP. Przy użyciu SASL ma tę zaletę, który zapewnia rozszerzonego modelu, dla którego zdefiniowano zestaw mechanizmów z mogą korzystać każdego protokołu, który formalnie leans na SASL. Wśród tych mechanizmów są "Zwykły" przesyłania nazwy użytkownika i hasła, aby powiązać zabezpieczeń na poziomie protokołu TLS, "ANONYMOUS" Express braku jawne uwierzytelniania/autoryzacji i szerokiej gamy dodatkowych mechanizmów zezwalające na przekazywanie uwierzytelniania i/lub poświadczenia autoryzacji lub tokeny "EXTERNAL".

Integracja SASL protokołu AMQP firmy ma dwie wady:

* Wszystkie poświadczenia i tokeny należą do zakresu połączenia. Warto zapewnienia kontroli dostępu zróżnicowanych na podstawie na jednostkę; infrastruktury obsługi wiadomości na przykład dzięki czemu elementu nośnego tokenu wysłać do kolejki A, ale nie do kolejki B. W kontekście autoryzacji zakotwiczony połączenie nie jest możliwe używać pojedynczego połączenia oraz jeszcze tokenów dostępu różnych dla A kolejek i B.
* Tokeny dostępu są zwykle prawidłowe tylko przez ograniczony czas. Ważność ta wymaga od użytkownika okresowo ponownie pozyskać tokenów i zapewnia możliwość wystawcy tokenów odmowy wystawiania świeże token, jeśli zostały zmienione uprawnienia dostępu. Połączenia protokołu AMQP może trwać przez dłuższy czas. Model warstwy SASL tylko zapewnia możliwość ustawienie tokenu w czasie połączenia, co oznacza, że infrastruktury obsługi wiadomości, albo ma Rozłącz klienta po wygaśnięciu tokenu lub musi zaakceptować ryzyko związane z zezwoleniem kontynuację komunikacji z klientem kto ma uprawnienia zostały odwołane w tymczasowych.

Specyfikację protokołu AMQP CBS implementowanych przez usługi Service Bus umożliwia obejście elegancki dla obu tych problemów: umożliwia klienta, aby skojarzyć tokenów dostępu z każdego węzła i zaktualizować te tokeny, zanim wygasną, bez zakłócania pracy przepływ komunikatów.

CBS definiuje węzłem zarządzanie wirtualnym o nazwie *$cbs*, aby zapewniać infrastruktury obsługi wiadomości. Do węzła zarządzania akceptuje tokeny w imieniu innych węzłów w infrastrukturze obsługi wiadomości.

Gest protokół jest żądanie/odpowiedź programu exchange zgodnie z definicją w specyfikacji zarządzania. Czy oznacza, że klient nawiąże pary powiązania z *$cbs* węzła i następnie przekazuje żądania wychodzącego łącze, a następnie czeka na odpowiedź na łączy przychodzących.

Komunikat żądania ma następujące właściwości aplikacji:

| Klucz | Optional (Opcjonalność) | Typ wartości | Wartość zawartości |
| --- | --- | --- | --- |
| Operacja |Nie |Ciąg |**Put token** |
| type |Nie |Ciąg |Typ tokenu dotyczy żądanie put. |
| name |Nie |Ciąg |Dotyczy tokenu "audience". |
| wygaśnięcia |Tak |sygnatura czasowa |Czas wygaśnięcia tokenu. |

*Nazwa* właściwość identyfikuje jednostki, z którym skojarzony jest token. W usłudze Service Bus jest ścieżka do kolejki i tematu/subskrypcji. *Typu* właściwość identyfikuje typ tokenu:

| Typ tokenu | Opis elementu tokenu | Typ treści | Uwagi |
| --- | --- | --- | --- |
| amqp:jwt |Tokenu Web JSON (JWT) |Wartość AMQP (ciąg) |Jeszcze niedostępne. |
| amqp:SWT |Simple Web Token (SWT) |Wartość AMQP (ciąg) |Obsługiwane tylko w przypadku SWT tokenów wystawionych przez AAD/ACS |
| servicebus.Windows.NET:sastoken |Token SAS magistrali usług |Wartość AMQP (ciąg) |- |

Tokeny przyznaje prawa. Usługa Service Bus zna trzech podstawowych praw: "Wyślij" umożliwia wysyłanie "Nasłuchiwania" umożliwia odbieranie i "Manage" umożliwia manipulowanie jednostek. SWT tokeny wystawione przez usługi AAD/ACS jawnie zawierają te uprawnienia jako oświadczenia. Tokeny sygnatury dostępu Współdzielonego usługi magistrali dotyczą zasady skonfigurowane w przestrzeni nazw lub jednostki, a te zasady są skonfigurowane z uprawnień. Podpisywania tokenu z klucz skojarzony z tej reguły w związku z tym sprawia, że token express odpowiednich praw. Token skojarzony z jednostki przy użyciu *put token* pozwala na połączony klient wchodzić w interakcje z jednostką na token praw. Łącze, w którym klient ma na *nadawcy* rola wymaga "Send" prawo; biorąc *odbiornika* rola wymaga "Nasłuchiwania" po prawej.

Komunikat odpowiedzi zawiera następujące *właściwości aplikacji* wartości

| Klucz | Optional (Opcjonalność) | Typ wartości | Wartość zawartości |
| --- | --- | --- | --- |
| Kod stanu |Nie |int |Kod odpowiedzi HTTP **[specyfikacją RFC2616]**. |
| Opis stanu |Tak |Ciąg |Opis stanu. |

Klient może wywołać *put token* wielokrotnie i dla dowolnej jednostki w infrastrukturze obsługi wiadomości. Tokeny są ograniczone do bieżącego klienta i zakotwiczonych w bieżącym połączeniu, co oznacza, że serwer porzuca wszystkie tokeny zachowanych po spadku połączenia.

Bieżąca implementacja usługi Service Bus zezwala tylko CBS w połączeniu z metodą SASL "ANONYMOUS". Połączenie SSL/TLS musi zawsze istnieć przed uzgadnianie SASL.

Mechanizm ANONIMOWY w związku z tym musi być obsługiwana przez wybrany klienta protokołu AMQP 1.0. Dostęp anonimowy oznacza, że uzgadnianie początkowego połączenia z tym tworzenie początkowej sesji się w stanie bez usługi Service Bus wiedząc, który tworzy połączenie.

Po nawiązaniu połączenia i sesji podłączania łącza do *$cbs* węzła i wysyłanie *put token* żądania są tylko dozwolone operacji. Nieprawidłowy token musi być ustawiona pomyślnie za pomocą *put token* żądania dla jednego z węzłów jednostki w ciągu 20 sekund po nawiązaniu połączenia, w przeciwnym razie połączenie jest jednostronnie przerwane przez magistralę usług.

Klient jest następnie odpowiedzialny za rejestrowanie informacji o wygaśnięciu tokenu. Po wygaśnięciu tokenu usługi Service Bus niezwłocznie porzuca wszystkie linki połączenia na odpowiednie jednostki. Aby tego uniknąć, klienta można zastąpić token dla węzła nową w dowolnym momencie za pośrednictwem wirtualnej *$cbs* węzła zarządzania o tej samej *put token* gestu i bez pobierania kategoriach ruch ładunku w różnych łączy.

## Następne kroki

Aby dowiedzieć się więcej na temat protokołu AMQP, skorzystaj z następujących łączy:

* [Omówienie protokołu AMQP magistrali usług]
* [Obsługa protokołu AMQP 1.0 tematów i kolejek usługi Service Bus na partycje]
* [Protokół AMQP w usłudze Service Bus dla systemu Windows Server]

[this video course]: https://www.youtube.com/playlist?list=PLmE4bZU0qx-wAP02i0I7PJWvDWoCytEjD
[1]: ./media/service-bus-amqp-protocol-guide/amqp1.png
[2]: ./media/service-bus-amqp-protocol-guide/amqp2.png
[3]: ./media/service-bus-amqp-protocol-guide/amqp3.png
[4]: ./media/service-bus-amqp-protocol-guide/amqp4.png

[Omówienie protokołu AMQP magistrali usług]: service-bus-amqp-overview.md
[Obsługa protokołu AMQP 1.0 tematów i kolejek usługi Service Bus na partycje]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[Protokół AMQP w usłudze Service Bus dla systemu Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
