---
title: "Wprowadzenie do usługi Azure Cosmos DB | Dokumentacja firmy Microsoft"
description: "Zapoznaj się z informacjami na temat usługi Azure Cosmos DB. Ta dostępna w skali światowej, wielomodelowa baza danych zapewnia małe opóźnienia, elastyczną skalowalność i wysoką dostępność."
services: cosmosdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: a855183f-34d4-49cc-9609-1478e465c3b7
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 1591a7935b10e22164d26a09c4e923101b517227
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017


---

# <a name="welcome-to-azure-cosmos-db"></a>Azure Cosmos DB — Zapraszamy!

Azure Cosmos DB to dostępna w skali światowej, wielomodelowa baza danych firmy Microsoft. Usługa Azure Cosmos DB umożliwia elastyczne i niezależne skalowanie przepływności i przestrzeni dyskowej dla dowolnej liczby regionów geograficznych platformy Azure za pomocą jednego kliknięcia przycisku. W odróżnieniu od wszelkich innych usług baz danych, oferuje ona gwarancje dotyczące przepływności, opóźnienia, dostępności i spójności dzięki kompleksowym [umowom dotyczącym poziomu usług](https://aka.ms/acdbsla) (SLA, Service Level Agreement).

![Azure Cosmos DB to dostępna w skali światowej usługa bazy danych firmy Microsoft z elastycznym skalowaniem, gwarantowanymi małymi opóźnieniami, pięcioma modelami spójności oraz kompleksowymi, gwarantowanymi umowami SLA](./media/introduction/azure-cosmos-db.png)

Usługa Azure Cosmos DB zawiera zoptymalizowany pod kątem zapisu, zarządzany przez zasoby i niezależny od schematów aparat bazy danych, który natywnie obsługuje wiele modeli danych: pary klucz-wartość, dokumenty, grafy i dane kolumnowe. Obsługuje również wiele interfejsów API na potrzeby uzyskiwania dostępu do danych, w tym [MongoDB](../documentdb/documentdb-protocol-mongodb.md), [DocumentDB SQL](../documentdb/documentdb-introduction.md), [Gremlin](graph-introduction.md) (wersja zapoznawcza) i [tabele platformy Azure](table-introduction.md) (wersja zapoznawcza), w sposób rozszerzalny. 

Usługę Azure Cosmos DB zapoczątkowano pod koniec 2010 r. w celu rozwiązania kluczowych problemów, które napotykają deweloperzy wielkoskalowych aplikacji wewnątrz firmy Microsoft. Jednak tworzenie dystrybuowanych globalnie aplikacji nie jest problemem, z którym boryka się tylko firma Microsoft, dlatego udostępniliśmy tę usługę na zewnątrz wszystkim deweloperom platformy Azure w formie usługi Azure DocumentDB. Usługa Azure Cosmos DB to kolejny duży krok w rozwoju usługi DocumentDB i teraz udostępniamy ją do użytku. W ramach tej wersji usługi Azure Cosmos DB klienci usługi DocumentDB (wraz ze swoimi danymi) stają się automatycznie klientami usługi Azure Cosmos DB. Przejście jest płynne i klienci mają teraz dostęp do większego zakresu nowych możliwości oferowanych przez usługę Azure Cosmos DB. 

## <a name="capability-comparison"></a>Porównanie możliwości

Usługa Azure Cosmos DB zapewnia najlepsze możliwości relacyjnych i nierelacyjnych baz danych.

| Możliwości | Relacyjne bazy danych    | Nierelacyjne bazy danych (NoSQL) |     Azure Cosmos DB |
| --- | --- | --- | --- |
| Dystrybucja globalna | x | x | ✓ Gotowa do użycia, ponad 30 regionów, multihosting |
| Skalowanie w poziomie | x | ✓ | ✓ Niezależne przestrzeni dyskowej i przepływności | 
| Gwarancje dot. opóźnienia | x | ✓ | ✓ < 10 ms dla operacji odczytu, < 15 ms dla operacji zapisu na poziomie 99 percentyla | 
| Wysoka dostępność | x | ✓ | ✓ Zawsze włączona, wymiany zgodne z modelem PACELC, automatyczny i ręczny tryb failover |
| Model danych + interfejs API | Relacyjny + SQL | Wiele modeli + OSS API | Wiele modeli + SQL + OSS API (więcej wkrótce) |
| Umowy SLA | ✓ | x | ✓ Kompleksowe umowy SLA dotyczące opóźnienia, przepływności, spójności i dostępności |

## <a name="key-capabilities"></a>Najważniejsze możliwości
Jako dostępna w skali światowej usługa bazy danych Azure Cosmos DB zapewnia następujące możliwości pomocne w tworzeniu skalowalnych, dystrybuowanych globalnie, szybko reagujących aplikacji:

* [**Gotowa do użytku dystrybucja globalna**](#global-distribution)
    * Twoja aplikacja jest udostępniana użytkownikom natychmiastowo i wszędzie. Teraz może to dotyczyć także Twoich danych.
    * Nie martw się o sprzęt, dodawanie węzłów, maszyny wirtualne lub rdzenie. Po prostu wskaż i kliknij, a Twoje dane będą dostępne. 

* [**Wiele modeli danych i popularnych interfejsów API na potrzeby uzyskiwania dostępu do danych i wykonywania na nich zapytań**](#data-models)
    * Obsługa wielu modeli danych, w tym par klucz-wartość, dokumentów, grafów i modeli kolumnowych.
    * Rozszerzalne interfejsy API dla technologii Node.js, Java, .NET, .NET Core, Python i MongoDB.
    * Rozwiązania Gremlin i SQL dla zapytań. 

* [**Elastycznie skalowana przepływność i przestrzeń dyskowa na żądanie, na całym świecie**](#horizontal-scale)
    * Łatwe skalowanie przepływności przy [sekundowych](../documentdb/documentdb-request-units.md) i [minutowych](https://aka.ms/acdbrupm) stopniach szczegółowości i możliwość wprowadzania zmian w dowolnym momencie. 
    * Skalowanie przestrzeni dyskowej w sposób [przejrzysty i automatyczny](partition-data.md) w celu spełnienia wymagań dotyczących rozmiaru teraz i w każdym innym momencie.

* [**Tworzenie szybko reagujących aplikacji o kluczowym znaczeniu**](#low-latency) 
    * Uzyskuj dostęp do swoich danych z opóźnieniem krótszym niż dziesięć milisekund (na poziomie 99 percentyla), w dowolnym miejscu na świecie. 

* [**Zapewnienie dostępności na poziomie „zawsze włączona”**](#high-availability)
    * Dostępność na poziomie 99,99% w obrębie jednego regionu.
    * Możliwość wdrażania w dowolnej liczbie [regionów świadczenia usługi Azure](https://azure.microsoft.com/regions), aby uzyskać większą dostępność.
    * [Symulowanie awarii](../documentdb/documentdb-regional-failovers.md) co najmniej jednego regionu z gwarancją, że nie nastąpi utrata żadnych danych. 

* [**Pisanie aplikacji dystrybuowanych globalnie w odpowiedni sposób**](#consistency)
    * [Pięć modeli spójności](../documentdb/documentdb-consistency-levels.md) oferuje silną spójność, podobną do spójności SQL, i spójność ostateczną, podobną do spójności NoSQL, oraz wszystko pomiędzy nimi. 
  
* [**Gwarancja zwrotu pieniędzy**](#sla) 
    * Szybkie dostarczanie danych lub zwrot pieniędzy. 
    * [Umowy dotyczące poziomu usług](https://aka.ms/acdbsla) obejmujące dostępność, opóźnienie, przepływność i spójność. 

* [**Brak schematu bazy danych/zarządzania indeksem**](#schema-free)
    * Nie musisz już martwić się o synchronizowanie indeksów i schematu bazy danych ze schematem aplikacji. U nas nie ma schematów. 

* [**Niski koszt posiadania**](#tco)
    * Od pięciu do dziesięciu razy [bardziej ekonomicznie](https://aka.ms/documentdb-tco-paper) niż w przypadku rozwiązania niezarządzanego.
    * Trzy razy taniej niż w przypadku usługi DynamoDB.

<a id="global-distribution"></a>

## <a name="global-distribution"></a>Dystrybucja globalna
Kontenery usługi Azure Cosmos DB są dystrybuowane w dwóch wymiarach: 

1. W obrębie danego regionu wszystkie zasoby są dzielone na partycje w poziomie przy użyciu partycji zasobów (dystrybucja lokalna). 
2. Każda partycja zasobów jest również replikowana w regionach geograficznych (dystrybucja globalna). 

![Diagram przedstawiający globalną dystrybucję usługi Azure Cosmos DB](./media/introduction/azure-cosmos-db-global-distribution.png) 

Gdy Twoja przestrzeń dyskowa i przepływność muszą być skalowane, usługa Cosmos DB w sposób niewidoczny wykonuje operacje zarządzania partycjami we wszystkich regionach. Niezależnie od skali, dystrybucji czy awarii, usługa Cosmos DB stale zapewnia pojedynczy obraz systemu globalnie dystrybuowanych zasobów. 

Globalna dystrybucja zasobów w usłudze Cosmos DB jest [gotowa do użycia](../documentdb/documentdb-distribute-data-globally.md). W każdym momencie za pomocą kilku kliknięć przycisków (lub programowo przy użyciu jednego wywołania interfejsu API) możesz skojarzyć dowolną liczbę regionów geograficznych ze swoim kontem bazy danych. 

Niezależnie od ilości danych lub liczby regionów, usługa Cosmos DB gwarantuje, że każdy nowo skojarzony region zacznie przetwarzać żądania klientów w czasie krótszym niż jedna godzina na poziomie 99 percentyla. Jest to wykonywane za pomocą rozmieszczania równoległego i kopiowania danych ze wszystkich źródłowych partycji zasobów do nowo skojarzonego regionu. Klienci mogą także usunąć istniejący region lub przenieść region, który był wcześniej skojarzony z ich kontem bazy danych, do trybu offline.

<a id="data-models"></a>
## <a name="multi-model-multi-api-support"></a>Obsługa wielu modeli i wielu interfejsów API
 Usługa Azure Cosmos DB natywnie obsługuje wiele modeli danych, w tym dokumenty, pary klucz-wartość, grafy i rodzinę kolumn. Podstawowy model zawartości aparatu bazy danych usługi Cosmos DB jest oparty na technologii atom-rekord-sekwencja (ARS, atom-record-sequence). Atomy składają się z małych zestawów typów podstawowych, takich jak ciąg, wartość logiczna i liczba. Rekordy to struktury złożone z tych typów. Sekwencje to tablice składające się z atomów, rekordów lub sekwencji. 

![Usługa Azure Cosmos DB obsługująca wiele modeli i wiele interfejsów API](./media/introduction/azure-cosmos-db-multimodel.png) 
 
 Aparat bazy danych może wydajnie tłumaczyć i rzutować różne modele danych na model danych oparty na technologii ARS. Podstawowy model danych usługi Cosmos DB jest natywnie dostępny z poziomu dynamicznie pisanych języków programowania i może zostać uwidoczniony w stanie takim, jaki jest, jako format JSON. 
 
 Usługa obsługuje również popularne interfejsy API bazy danych na potrzeby dostępu do danych i tworzenia zapytań. Aparat bazy danych usługi Cosmos DB obsługuje obecnie rozwiązania [DocumentDB SQL](../documentdb/documentdb-introduction.md), [MongoDB](../documentdb/documentdb-protocol-mongodb.md), [tabele platformy Azure](table-introduction.md) (wersja zapoznawcza) i [Gremlin](graph-introduction.md) (wersja zapoznawcza). Możesz nadal tworzyć aplikacje przy użyciu popularnych interfejsów API OSS i korzystać ze wszystkich zalet sprawdzonej w praktyce, w pełni zarządzanej i dostępnej w skali światowej usługi bazy danych. 

<a id="horizontal-scale"></a>
## <a name="horizontal-scaling-of-storage-and-throughput"></a>Skalowanie przestrzeni dyskowej i przepływności w poziomie
Wszystkie dane w obrębie kontenera usługi Cosmos DB (na przykład zbiór dokumentów, tabela lub graf) są dzielone na partycje w poziomie i w sposób niewidoczny zarządzane przez partycje zasobów. Partycja zasobu to spójny i wysoce dostępny kontener danych podzielonych na partycje za pomocą [klucza partycji określanego przez klienta](partition-data.md). Zapewnia ona pojedynczy obraz systemu dla zestawu zasobów, którymi zarządza, i jest podstawową jednostką skalowalności i dystrybucji. Usługa Cosmos DB jest zaprojektowana tak, aby umożliwić elastyczne skalowanie przepływności na podstawie wzorców ruchu aplikacji w różnych regionach geograficznych w celu obsługi zmieniających się obciążeń, różniących się w zależności od czasu i położenia geograficznego. Usługa zarządza partycjami w sposób niewidoczny, bez naruszania dostępności, spójności, opóźnienia ani przepływności kontenera usługi Cosmos DB.  
 
![Usługa Azure Cosmos DB jest skalowana w poziomie](./media/introduction/azure-cosmos-db-partitioning.png) 

Przepływność kontenera usługi Azure Cosmos DB można skalować elastycznie, programowo aprowizując przepływność przy użyciu [jednostek żądania na sekundę (RU/s)](../documentdb/documentdb-request-units.md). Wewnętrznie usługa niewidocznie zarządza partycjami zasobów w celu dostarczenia przepływności na dany kontener. Usługa Cosmos DB zapewnia dostępność przepływności do użycia we wszystkich regionach skojarzonych z danym kontenerem. Nowa przepływność zaczyna obowiązywać w ciągu pięciu sekund od zmiany wartości skonfigurowanej przepływności. 

Przepływność na kontenerze usługi Cosmos DB można aprowizować zarówno w sekundowych, jak i [minutowych (RU/m)](request-units-per-minute.md) stopniach szczegółowości. Przepływność aprowizowana w minutowym stopniu szczegółowości jest używana do zarządzania niespodziewanymi okresowymi wzrostami obciążenia, które pojawiają się w skali sekund. 

<a id="low-latency"></a>
## <a name="low-latency-guarantees-at-the-99th-percentile"></a>Gwarancja małych opóźnień na poziomie 99 percentyla
W ramach umowy SLA usługa Cosmos DB gwarantuje swoim klientom całościowe małe opóźnienia na poziomie 99 percentyla. Dla typowego elementu o wielkości 1 KB usługa Cosmos DB gwarantuje całościowe opóźnienie operacji odczytu poniżej 10 ms oraz indeksowanego zapisu poniżej 15 ms na poziomie 99 percentyla, w obrębie tego samego regionu świadczenia usługi Azure. Średnie opóźnienia są znacznie niższe (poniżej 5 ms).  Dzięki górnej granicy przetwarzania żądań dla każdej transakcji bazy danych usługa Cosmos DB umożliwia klientom wyraźne rozróżnienie między dużymi opóźnieniami transakcji a niedostępnością bazy danych.

<a id="high-availability"></a>
## <a name="transparent-multi-homing-and-9999-high-availability"></a>Przejrzysty multihosting i wysoka dostępność na poziomie 99,99%
Możesz dynamicznie przypisywać „priorytety” do regionów skojarzonych z Twoim kontem bazy danych usługi Azure Cosmos DB. Priorytety są używane do kierowania żądań do określonych regionów w przypadku regionalnych awarii. W razie mało prawdopodobnej regionalnej awarii usługa Cosmos DB automatycznie przechodzi w tryb failover w kolejności zależnej od priorytetów.

Aby sprawdzić kompleksową dostępność aplikacji, możesz [ręcznie wyzwolić tryb failover](../documentdb/documentdb-regional-failovers.md) (częstość jest ograniczona do dwóch operacji w ciągu godziny). Usługa Cosmos DB gwarantuje zerową utratę danych podczas ręcznego wywoływania regionalnego trybu failover. W przypadku wystąpienia regionalnej awarii usługa Cosmos DB gwarantuje górną granicę dla utraty danych podczas automatycznego trybu failover inicjowanego przez system. Po regionalnym przejściu w tryb failover nie musisz ponownie wdrażać swojej aplikacji, a umowy SLA dotyczące dostępności są utrzymywane przez usługę Azure Cosmos DB. 

W tym scenariuszu usługa Cosmos DB umożliwia interakcję z zasobami przy użyciu logicznych (niezależnych od regionów) albo fizycznych (określonych dla regionów) punktów końcowych. Pierwszy przypadek zapewnia, że w razie przejścia w tryb failover do aplikacji może w sposób niewidoczny zostać zastosowany multihosting. Drugi przypadek oferuje szczegółową kontrolę nad aplikację w celu przekierowania operacji odczytu i zapisu do określonych regionów. Usługa Cosmos DB gwarantuje umowę SLA dotyczącą dostępności na poziomie 99,99% dla każdego konta bazy danych. Gwarancje dostępności są niezależne od skali (aprowizowanej przepływności i przestrzeni dyskowej), liczby regionów i geograficznej odległości między regionami skojarzonymi z określoną bazą danych. 

<a id="consistency"></a>
## <a name="multiple-well-defined-consistency-models"></a>Wiele dobrze zdefiniowanych modeli spójności
Komercyjnie rozpowszechniane bazy danych można podzielić na dwie kategorie: bazy danych, które nie oferują żadnych dobrze zdefiniowanych, sprawdzalnych opcji spójności, i bazy danych, które oferują dwie skrajne opcje programowania (spójność silna lub ostateczna). Pierwsze zmuszają deweloperów aplikacji do zajmowania się drobnymi szczegółami protokołów replikacji i wymagają dokonywania trudnych wyborów między spójnością, dostępnością, opóźnieniem i przepływnością. Drugie kładą nacisk na konieczność wyboru między dwoma skrajnościami. Pomimo mnogości badań i ponad 50 propozycji modeli spójności społeczność związana z rozpowszechnianymi bazami danych nie była w stanie skomercjalizować poziomów spójności poza spójnością silną i ostateczną. 

Usługa Cosmos DB pozwala na wybór między [pięcioma dobrze zdefiniowanymi modelami spójności](../documentdb/documentdb-consistency-levels.md) obejmującymi całe spektrum spójności — silna, powiązana nieaktualność, [sesja](http://dl.acm.org/citation.cfm?id=383631), spójny prefiks i ostateczna. 

![Usługa Azure Cosmos DB oferuje wiele dobrze zdefiniowanych (swobodniejszych) modeli spójności do wyboru](media/introduction/azure-cosmos-db-consistency-levels.png)

W poniższej tabeli przedstawiono poszczególne gwarancje oferowane przez każdy poziom spójności.
 
**Poziomy spójności i gwarancje**

| Poziom spójności    | Gwarancje |
| --- | --- |
| Silna | Operacje atomowe |
| Powiązana nieaktualność    | Spójny prefiks. Odczyty opóźnione w stosunku do zapisów o k prefiksów lub interwał równy t |
| Sesja    | Spójny prefiks. Monotoniczne odczyty, monotoniczne zapisy, odczytywanie swoich zapisów, zapisy następują po odczytach |
| Spójny prefiks    | Zwracane aktualizacje to pewne prefiksy ze wszystkich aktualizacji, bez przerw |
| Ostateczna    | Odczyty poza kolejnością |

Na koncie usługi Cosmos DB można skonfigurować domyślny poziom spójności (i później przesłonić spójność dla określonego żądania odczytu). Wewnętrznie domyślny poziom spójności ma zastosowanie do danych w zestawach partycji, które mogą obejmować regiony. 


<a id="sla"></a>
## <a name="guaranteed-service-level-agreements"></a>Gwarantowane umowy dotyczące poziomu usług

Usługa Cosmos DB jest pierwszą zarządzaną usługą bazy danych, która oferuje [gwarancje umowy SLA](https://aka.ms/acdbsla) na poziomie 99,99% dla dostępności, przepływności, małego opóźnienia i spójności.
* Dostępność: umowa SLA dotycząca dostępności czasu pracy na poziomie 99,99% dla każdej operacji warstwy kontroli i płaszczyzny danych.
* Przepływność: 99,99% żądań ukończonych pomyślnie 
* Opóźnienie: 99,99% opóźnień krótszych niż 10 ms na poziomie 99 percentyla
* Spójność: 100% żądań odczytu spełnia gwarancję spójności dla żądanego poziomu spójności.


<a id="schema-free"></a>
## <a name="schema-free"></a>Brak schematów

Zarówno relacyjne bazy danych, jak i bazy danych NoSQL, wymagają zmierzenia się z zarządzaniem indeksami i schematami, obsługą wersji i migracją — wszystko to może być bardzo trudne w konfiguracji globalnej dystrybucji. Ale nie martw się — dzięki usłudze Cosmos DB ten problem nie istnieje! Korzystając z usługi Cosmos DB, nie musisz zarządzać schematami i indeksami, zajmować się obsługą wersji schematów ani martwić się o przerwy w działaniu aplikacji podczas migrowania schematów. Aparat bazy danych usługi Cosmos DB jest w pełni niezależny od schematów — automatycznie indeksuje wszystkie dane, które pozyska, bez wymagania żadnych schematów ani indeksów czy serwerów przetwarzających szybkie zapytania. 

<a id="tco"></a>
## <a name="low-cost-of-ownership"></a>Niski koszt posiadania

 Biorąc pod uwagę wszystkie aspekty całkowitych kosztów posiadania, zarządzane usługi w chmurze, takie jak Azure Cosmos DB, mogą być od pięciu do dziesięciu razy bardziej ekonomiczne niż konkurencyjne rozwiązania typu OSS działające lokalnie lub na maszynach wirtualnych. Ponadto usługa Azure Cosmos DB jest od dwóch do trzech razy tańsza niż usługa DynamoDB dla dużych obciążeń. Więcej informacji znajdziesz w [oficjalnym dokumencie na temat całkowitego kosztu posiadania](https://aka.ms/documentdb-tco-paper). 

## <a name="next-steps"></a>Następne kroki
Rozpocznij pracę z usługą Azure Cosmos DB, korzystając z jednego z naszych przewodników:

* [Rozpoczynanie pracy z interfejsem API DocumentDB usługi Azure Cosmos DB](create-documentdb-dotnet.md)
* [Rozpoczynanie pracy z interfejsem API MongoDB usługi Azure Cosmos DB](create-mongodb-nodejs.md)
* [Rozpoczynanie pracy z interfejsem API Graph usługi Azure Cosmos DB](create-graph-dotnet.md)
* [Rozpoczynanie pracy z interfejsem API tabeli usługi Azure Cosmos DB](create-table-dotnet.md)

