---
title: "Omówienie podstaw usługi Azure Service Bus | Microsoft Docs"
description: "Wprowadzenie do używania usługi Service Bus w celu łączenia aplikacji Azure z innym oprogramowaniem."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 12654cdd-82ab-4b95-b56f-08a5a8bbc6f9
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/15/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: af8b10f0a460e695a39879718174e81f78934ef8
ms.contentlocale: pl-pl
ms.lasthandoff: 06/17/2017


---
<a id="azure-service-bus" class="xliff"></a>

# Azure Service Bus

Obojętnie, czy aplikacja lub usługa działa w chmurze, czy lokalnie, często wymaga interakcji z innymi aplikacjami lub usługami. Aby zapewnić szeroko użyteczny sposób obsługi tych interakcji, platforma Microsoft Azure oferuje usługę Service Bus. Ten artykuł przedstawia tę technologię, opisuje, czym ona jest, oraz dlaczego warto z niej korzystać.

<a id="service-bus-fundamentals" class="xliff"></a>

## Podstawy usługi Service Bus

Różne sytuacje wymagają różnych stylów komunikacji. Czasami najlepszym rozwiązaniem jest umożliwienie aplikacji wysyłania i odbierania komunikatów przy użyciu prostej kolejki. W innych sytuacjach zwykła kolejka nie wystarcza i lepszym rozwiązaniem jest kolejka z mechanizmem publikowania i subskrybowania. W niektórych przypadkach potrzebne jest jedynie połączenie między aplikacjami, a kolejki nie są wymagane. Usługa Service Bus oferuje wszystkie trzy opcje, umożliwiając aplikacjom interakcję na kilka różnych sposobów.

Service Bus to wielodostępna usługa w chmurze, co oznacza, że usługa jest współużytkowana przez wielu użytkowników. Każdy użytkownik, taki jak deweloper aplikacji, tworzy *przestrzeń nazw*, a następnie definiuje mechanizmy komunikacji potrzebne w ramach tej przestrzeni nazw. Rysunek 1 pokazuje jak wygląda ta architektura.

![][1]

**Rysunek 1. Service Bus udostępnia usługę wielodostępu na potrzeby łączenia aplikacji za pośrednictwem chmury.**

W przestrzeni nazw można używać jednego lub większej liczby wystąpień trzech różnych mechanizmów komunikacji, z których każdy łączy aplikacje w inny sposób. Dostępne są następujące mechanizmy:

* *Kolejki*, które umożliwiają komunikację jednokierunkową. Każda kolejka działa jako pośrednik (nazywany czasem *brokerem*), który przechowuje wysyłane wiadomości, dopóki nie zostaną odebrane. Każdy komunikat jest odbierany przez jednego adresata.
* *Tematy*, które zapewniają komunikację jednokierunkową przy użyciu *subskrypcji* (jeden temat może mieć wiele subskrypcji). Podobnie jak kolejka temat działa jako broker, ale każda subskrypcja może opcjonalnie korzystać z filtru w celu odbierania tylko komunikatów spełniających określone kryteria.
* *Przekaźniki*, które zapewniają komunikację dwukierunkową. W przeciwieństwie do kolejek i tematów przekaźnik nie przechowuje transmitowanych komunikatów (nie jest brokerem). Zamiast tego po prostu przekazuje je do aplikacji docelowej.

Podczas tworzenia kolejki, tematu lub przekaźnika tworzonemu elementowi nadawana jest nazwa. W połączeniu z przestrzenią nazw ta nazwa tworzy unikatowy identyfikator obiektu. Aplikacje mogą udostępnić tę nazwę usłudze Service Bus, a następnie używać kolejki, tematu lub przekaźnika do komunikowania się ze sobą. 

Aby użyć dowolnego z tych obiektów w scenariuszu dotyczącym przekaźnika, aplikacje systemu Windows mogą korzystać z technologii Windows Communication Foundation (WCF). Ta usługa jest określana jako [przekaźnik WCF](../service-bus-relay/relay-what-is-it.md). W przypadku kolejek i tematów aplikacje systemu Windows mogą używać interfejsów API obsługi komunikatów zdefiniowanych przez usługę Service Bus. Aby ułatwić korzystanie z tych obiektów aplikacjom innych niż aplikacje systemu Windows, firma Microsoft udostępnia zestawy SDK dla języka Java, Node.js i innych języków. Dostęp do kolejek i tematów można także uzyskać za pomocą [interfejsów API REST](/rest/api/servicebus/) i protokołu HTTP lub HTTPS. 

Należy pamiętać, że chociaż usługa Service Bus sama działa w chmurze (to znaczy w centrach danych platformy Microsoft Azure), aplikacje z niej korzystające mogą być uruchamiane w dowolnym miejscu. Usługa Service Bus umożliwia na przykład łączenie aplikacji działających na platformie Azure lub aplikacji uruchamianych wewnątrz własnego centrum danych. Umożliwia także łączenie aplikacji działających na platformie Azure lub innej platformie chmurowej z aplikacjami lokalnymi lub z tabletami i telefonami. Istnieje nawet możliwość łącznia się urządzeń gospodarstwa domowego, czujników i innych urządzeń z centralną aplikacją lub między sobą. Usługa Service Bus jest mechanizmem komunikacji w chmurze, który jest dostępny niemal z dowolnego miejsca. Sposób korzystania z usługi Service Bus zależy od tego, co aplikacja ma robić.

<a id="queues" class="xliff"></a>

## Kolejki

Załóżmy, że użytkownik łączy dwie aplikacje przy użyciu kolejki usługi Service Bus. Na Rysunku 2 przedstawiono tę sytuację.

![][2]

**Rysunek 2. Kolejki usługi Service Bus zapewniają jednokierunkowe kolejkowanie asynchroniczne.**

Proces jest prosty: nadawca wysyła komunikat do kolejki usługi Service Bus, a odbiornik pobiera ten komunikat w późniejszym czasie. Kolejka może zawierać tylko jeden odbiornik, jak pokazano na rysunku 2. Możliwa jest również sytuacja, gdy wiele aplikacji odczytuje dane z tej samej kolejki. W tej ostatniej sytuacji każdy komunikat jest odczytywany tylko przez jeden odbiornik. W przypadku usługi multiemisji należy zamiast kolejki użyć tematu.

Każdy komunikat ma dwie części: zbiór właściwości, z których każda jest parą klucz/wartość, oraz ładunek komunikatu. Ładunkiem mogą być dane binarne, tekstowe, a nawet plik XML. Sposób ich używania zależy od tego, co aplikacja próbuje zrobić. Na przykład aplikacja wysyłająca komunikat o ostatniej sprzedaży może dołączać właściwości **Seller="Ava"** i **Amount=10000**. Treść komunikatu może zawierać zeskanowany obraz podpisanej umowy sprzedaży lub, jeśli nie istnieje, pozostać pusta.

Odbiornik może odczytywać komunikaty z kolejki usługi Service Bus na dwa różne sposoby. Pierwsza opcja o nazwie *[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode)* usuwa komunikat z kolejki i natychmiast go kasuje. Jest to proste, ale jeśli odbiornik ulegnie awarii przed zakończeniem przetwarzania komunikatu, komunikat zostanie utracony. Ponieważ został usunięty z kolejki, żaden inny odbiornik nie może już uzyskać do niego dostępu. 

Druga opcja, *[PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode)*, ma pomóc w rozwiązaniu tego problemu. Podobnie jak w przypadku opcji **ReceiveAndDelete**, odczyt za pomocą opcji **PeekLock** usuwa komunikat z kolejki. Nie powoduje jednak skasowania (faktycznego usunięcia) komunikatu. Zamiast tego blokuje komunikat, ukrywając go przed innymi odbiornikami, a następnie czeka na jedno z trzech zdarzeń:

* Jeśli odbiornik pomyślnie przetworzy komunikat, wywołuje metodę [Complete()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete), a kolejka usuwa komunikat. 
* Jeśli odbiornik zadecyduje, że nie może przetworzyć komunikatu, wywołuje metodę [Abandon()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon). Kolejka następnie usuwa blokadę z komunikatu i umożliwia dostęp do niego innym odbiornikom.
* Jeśli odbiornik nie wywoła żadnej z tych metod w skonfigurowanym czasie (domyślnie 60 sekund), kolejka zakłada, że wystąpił błąd odbiornika. W takim przypadku zachowuje się tak, jak gdyby odbiornik wywołał metodę **Abandon**, udostępniając komunikat innym odbiornikom.

Zauważ, że w tej sytuacji może się zdarzyć, że ten sam komunikat zostanie dostarczony dwukrotnie, być może do dwóch różnych odbiorników. Aplikacje korzystające z kolejek usługi Service Bus muszą być przygotowane na to zdarzenie. Aby ułatwić wykrywanie duplikatów, każdy komunikat ma unikatową wartość właściwości [MessageID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId), która domyślnie pozostaje taka sama niezależnie od tego, ile razy komunikat został odczytany z kolejki. 

Kolejki są przydatne w kilku sytuacjach. Umożliwiają one aplikacjom komunikowanie się nawet wtedy, gdy nie są uruchomione w tym samym czasie, co jest szczególnie przydatne w przypadku aplikacji wsadowych i aplikacji mobilnych. Kolejka z wieloma odbiornikami zapewnia również automatyczne równoważenie obciążenia, ponieważ wysłane wiadomości są rozkładane między te odbiorniki.

<a id="topics" class="xliff"></a>

## Tematy

Pomimo swojej przydatności, kolejki nie zawsze są odpowiednim rozwiązaniem. Czasami lepsze są tematy usługi Service Bus. Rysunek 3 ilustruje tę koncepcję.

![][3]

**Rysunek 3. Na podstawie filtru określonego przez subskrybującą aplikację może ona odbierać niektóre lub wszystkie komunikaty wysyłane do tematu usługi Service Bus.**

*Temat* jest pod wieloma względami podobny do kolejki. Nadawcy wysyłają komunikaty do tematu w taki sam sposób jak do kolejki, a komunikaty wyglądają tak samo jak w przypadku kolejek. Różnicą jest to, że tematy umożliwiają każdej aplikacji odbierającej tworzenie własnej *subskrypcji* przez zdefiniowanie *filtru*. Subskrybent będzie zatem widział tylko komunikaty zgodne z filtrem. Przykładowo, Rysunek 3 przedstawia nadawcę i temat z trzema subskrybentami, z których każdy ma własny filtr:

* Subskrybent 1 odbiera tylko komunikaty, które zawierają właściwość *Seller="Ava"*.
* Subskrybent 2 odbiera komunikaty, które zawierają właściwość *Seller="Ruby"* i/lub zawierają właściwość *Amount* o wartości większej niż 100 000. Być może Ruby jest kierownikiem ds. sprzedaży, więc chce widzieć własną sprzedaż i wszystkie duże transakcje sprzedaży niezależnie od tego, kto ich dokonuje.
* Subskrybent 3 ustawił filtr na wartość *True*, co oznacza, że odbiera wszystkie komunikaty. Ta aplikacja może być na przykład odpowiedzialna za utrzymanie dziennika inspekcji i w związku z tym musi widzieć wszystkie komunikaty.

Podobnie jak w przypadku kolejek, subskrybenci tematu mogą odczytywać komunikaty przy użyciu metody [ReceiveAndDelete lub PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode). W przeciwieństwie do kolejek pojedynczy komunikat wysłany do tematu może zostać odebrany w ramach wielu subskrypcji. Takie podejście, często nazywane *publikowaniem i subskrybowaniem* (lub *publikowaniem/subskrypcją*), jest przydatne, gdy wiele aplikacji jest zainteresowanych tymi samymi komunikatami. Dzięki zdefiniowaniu właściwego filtru każdy subskrybent może korzystać tylko z tej części strumienia komunikatów, którą chce wyświetlać.

<a id="relays" class="xliff"></a>

## Przekaźniki

Kolejki i tematy zapewniają jednokierunkową asynchroniczną komunikację za pośrednictwem brokera. Ruch przepływa tylko w jednym kierunku i nie ma bezpośredniego połączenia między nadawcami a odbiornikami. Ale co zrobić, jeśli nie potrzebujesz tego połączenia? Załóżmy, że aplikacje muszą zarówno wysyłać, jak i odbierać komunikaty, albo chcesz mieć bezpośrednie połączenie między nimi i nie potrzebujesz brokera do przechowywania komunikatów. Na potrzeby takich scenariuszy usługa Service Bus udostępnia *przekaźniki*, jak pokazano na rysunku 4.

![][4]

**Rysunek 4. Przekaźnik usługi Service Bus zapewnia synchroniczną, dwukierunkową komunikację między aplikacjami.**

Oczywistym pytaniem, jeśli chodzi o przekaźniki, jest: po co z nich korzystać? Nawet jeśli nie potrzebujesz kolejek, dlaczego warto udostępnić aplikacjom możliwość komunikacji za pomocą usługi w chmurze zamiast bezpośredniej interakcji? Odpowiedź brzmi: komunikacja bezpośrednia może być trudniejsza, niż się wydaje.

Załóżmy, że chcesz nawiązać połączenie między dwiema lokalnymi aplikacjami, z których obie działają wewnątrz firmowych centrów danych. Każda z tych aplikacji znajduje się za zaporą, a każde centrum danych prawdopodobnie używa translatora adresów sieciowych (NAT). Zapora blokuje przychodzące dane na wszystkich, z wyjątkiem kilku, portach, a korzystanie z NAT oznacza, że maszyna, na której uruchomiona jest każda aplikacja, nie ma stałego adresu IP osiągalnego bezpośrednio spoza centrum danych. Bez dodatkowej pomocy połączenie tych aplikacji za pośrednictwem publicznego Internetu jest problematyczne.

Przekaźnik usługi Service Bus może w tym pomóc. Aby komunikować się dwukierunkowo przez przekaźnik, każda aplikacja ustanawia wychodzące połączenie TCP za pomocą usługi Service Bus, a następnie utrzymuje je otwarte. Cała komunikacja między dwiema aplikacjami odbywa się przez te połączenia. Ponieważ każde połączenie zostało nawiązane z wewnątrz centrum danych, zapora zezwala na ruch przychodzący do każdej aplikacji bez konieczności otwierania nowych portów. To podejście zapewnia również obejście problemu translatora adresów sieciowych, ponieważ każda aplikacja ma spójny punkt końcowy w chmurze podczas całej komunikacji. Dzięki wymianie danych za pośrednictwem przekaźnika aplikacje mogą uniknąć problemów, które w przeciwnym razie spowodowałyby trudności w komunikacji. 

Aby korzystać z przekaźników usługi Service Bus, aplikacje polegają na programie Windows Communication Foundation (WCF). Usługa Service Bus udostępnia wiązania WCF, dzięki którym interakcje za pomocą przekaźników stają się proste dla aplikacji systemu Windows. Aplikacje, które już używają technologii WCF, mogą zazwyczaj określić jedno z tych powiązań, a następnie komunikować się ze sobą za pośrednictwem przekaźnika. Jednak inaczej niż w przypadku kolejek i tematów korzystanie z przekaźników z aplikacji innych niż aplikacje systemu Windows, o ile jest to możliwe, wymaga nieco pracy programistycznej, ponieważ nie są udostępniane żadne biblioteki standardowe.

W przeciwieństwie do kolejek i tematów aplikacje nie tworzą jawnie przekaźników. Zamiast tego aplikacja, która chce odbierać komunikaty, ustanawia połączenie TCP z usługą Service Bus, a przekaźnik jest tworzony automatycznie. Jeśli połączenie zostanie porzucone, przekaźnik jest usuwany. Aby umożliwić aplikacji znalezienie przekaźnika utworzonego przez określony odbiornik, usługa Service Bus udostępnia rejestr, który umożliwia aplikacjom zlokalizowanie konkretnego przekaźnika według nazwy.

Przekaźniki są odpowiednim rozwiązaniem, gdy potrzebna jest bezpośrednia komunikacja między aplikacjami. Rozważmy na przykład system rezerwacji linii lotniczych uruchomiony w lokalnym centrum danych, który musi być dostępny z kiosków odprawy, urządzeń przenośnych i innych komputerów. Aplikacje działające w tych wszystkich systemach mogą polegać na przekaźnikach usługi Service Bus w chmurze na potrzeby komunikacji wszędzie tam, gdzie mogą być uruchamiane.

<a id="summary" class="xliff"></a>

## Podsumowanie

Łączenie aplikacji zawsze było częścią tworzenia kompletnych rozwiązań, a zakres scenariuszy, które wymagają komunikowania się ze sobą aplikacji i usług, ciągle się zwiększa, ponieważ coraz więcej aplikacji i urządzeń ma połączenie z Internetem. Zapewniając technologie chmurowe służące osiągnięciu tego celu za pośrednictwem kolejek, tematów i przekaźników, usługa Service Bus ułatwia zaimplementowanie tej istotnej funkcji i sprawia, że jest ona szerzej dostępna.

<a id="next-steps" class="xliff"></a>

## Następne kroki

Teraz, kiedy znasz już podstawy usługi Azure Service Bus, skorzystaj z poniższych linków, aby dowiedzieć się więcej.

* Jak używać [kolejek usługi Service Bus](service-bus-dotnet-get-started-with-queues.md)
* Jak używać [tematów usługi Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* Jak używać [przekaźnika usługi Service Bus](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md)
* [Przykłady usługi Service Bus](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png

