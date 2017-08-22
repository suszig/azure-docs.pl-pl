---
title: "Wprowadzenie do usługi Azure Cosmos DB | Dokumentacja firmy Microsoft"
description: "Zapoznaj się z informacjami na temat usługi Azure Cosmos DB. Ta dostępna w skali światowej, wielomodelowa baza danych zapewnia małe opóźnienia, elastyczną skalowalność i wysoką dostępność."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: a855183f-34d4-49cc-9609-1478e465c3b7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/14/2017
ms.author: mimig
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: c9d04ae0bc11b99f893e5f003f136fbfe0dfccc9
ms.contentlocale: pl-pl
ms.lasthandoff: 08/11/2017

---

# <a name="welcome-to-azure-cosmos-db"></a>Azure Cosmos DB — Zapraszamy!

Azure Cosmos DB to dostępna w skali światowej, wielomodelowa baza danych firmy Microsoft. Usługa Azure Cosmos DB umożliwia elastyczne i niezależne skalowanie przepływności i przestrzeni dyskowej dla dowolnej liczby regionów geograficznych platformy Azure za pomocą jednego kliknięcia przycisku. W odróżnieniu od wszelkich innych usług baz danych, oferuje ona gwarancje dotyczące przepływności, opóźnienia, dostępności i spójności dzięki kompleksowym [umowom dotyczącym poziomu usług](https://aka.ms/acdbsla) (SLA, Service Level Agreement).

![Azure Cosmos DB to dostępna w skali światowej usługa bazy danych firmy Microsoft z elastycznym skalowaniem, gwarantowanymi małymi opóźnieniami, pięcioma modelami spójności oraz kompleksowymi, gwarantowanymi umowami SLA](./media/introduction/azure-cosmos-db.png)

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Rozwiązania, które korzystają z zalet usługi Azure Cosmos DB

Wszystkie [aplikacje internetowe, IoT i mobilne oraz gry](use-cases.md) wymagające obsługi ogromnych ilości operacji odczytu i zapisuje w skali [globalnej](distribute-data-globally.md) z krótkim czasem odpowiedzi dla różnych danych będą korzystać z zalet związanych z [gwarantowaną](https://azure.microsoft.com/support/legal/sla/cosmos-db/) dostępnością, wysoką przepływnością, małymi opóźnieniami i możliwościami dostosowania spójności, które są zapewniane przez usługę Azure Cosmos DB.

## <a name="key-capabilities"></a>Najważniejsze możliwości
Jako dostępna w skali światowej usługa bazy danych Azure Cosmos DB zapewnia następujące możliwości ułatwiające tworzenie skalowalnych i szybko reagujących aplikacji:

* **Gotowa do użytku dystrybucja globalna**
    * Możesz [dystrybuować dane](distribute-data-globally.md) w dowolnej liczbie [regionów platformy Azure](https://azure.microsoft.com/regions/) [jednym kliknięciem przycisku](tutorial-global-distribution-documentdb.md). Umożliwia to używanie Twoich danych tam, gdzie znajdują się użytkownicy, co zapewnia najniższe możliwe opóźnienie Twoim klientom. 
    * Za pomocą międzyregionalnych interfejsów API usługi Azure Cosmos DB aplikacja zawsze posiada informacje o tym, gdzie znajduje się najbliższy region, co pozwala wysyłać żądania do najbliższego centrum danych. Wszystko to jest możliwe bez potrzeby wprowadzania zmian konfiguracji — należy ustawić jedynie region zapisu i dowolną liczbę regionów odczytu, a pozostałe operacje będą obsługiwane za Ciebie.

* **Wiele modeli danych i popularnych interfejsów API na potrzeby uzyskiwania dostępu do danych i wykonywania względem nich zapytań**
    * Model danych oparty na technologii atom-rekord-sekwencja (ARS, atom-record-sequence), na podstawie którego zbudowana jest usługi Azure Cosmos DB, natywnie obsługuje wiele modeli danych, między innymi te oparte na dokumentach, grafach, parach klucz-wartość i kolumnach.
    * Interfejsy API dla następujących modeli danych są obsługiwane przez zestawy SDK dostępne w wielu językach:
        * [Interfejs API usługi DocumentDB](documentdb-introduction.md)
        * [Interfejs API usługi MongoDB](mongodb-introduction.md)
        * [Interfejs API tabel](table-introduction.md)
        * [Interfejs API programu Graph (Gremlin)](graph-introduction.md)
        * Dodatkowe modele danych będą dostępne wkrótce 

* **Elastycznie skalowana przepływność i przestrzeń dyskowa na żądanie, na całym świecie**
    * Łatwe skalowanie przepływności bazy danych z [sekundową](request-units.md) dokładnością i możliwość jej zmiany w dowolnym momencie. 
    * Skalowanie rozmiaru magazynu w sposób [przejrzysty i automatyczny](partition-data.md) w celu obsługi wymagań dotyczących rozmiaru teraz i w każdym innym momencie.

* **Tworzenie szybko reagujących aplikacji o kluczowym znaczeniu**
    * Usługa Azure Cosmos DB gwarantuje klientom całościowe małe opóźnienia na poziomie 99 percentyla. 
    * Dla typowego elementu o wielkości 1 KB usługa Cosmos DB gwarantuje całościowe opóźnienie operacji odczytu poniżej 10 ms oraz indeksowanego zapisu poniżej 15 ms na poziomie 99 percentyla w obrębie tego samego regionu świadczenia usługi Azure. Średnie opóźnienia są znacznie niższe (poniżej 5 ms).

* **Zapewnienie dostępności na poziomie „zawsze włączona”**
    * Dostępność na poziomie 99,99% w obrębie jednego regionu.
    * Możliwość wdrażania w dowolnej liczbie [regionów świadczenia usługi Azure](https://azure.microsoft.com/regions), aby uzyskać większą dostępność.
    * [Symulowanie awarii](regional-failover.md) co najmniej jednego regionu z gwarancją, że nie nastąpi utrata żadnych danych. 

* **Tworzenie aplikacji dystrybuowanych globalnie w odpowiedni sposób**
    * Pięć [modeli spójności](consistency-levels.md) zapewnia silną spójność, podobną do spójności SQL, oraz spójność ostateczną, podobną do spójności NoSQL (i wszystko pomiędzy nimi). 
  
* **Gwarancja zwrotu pieniędzy**
    * Szybkie dostarczanie danych lub zwrot pieniędzy. 
    * [Umowy dotyczące poziomu usług](https://aka.ms/acdbsla) obejmujące dostępność, opóźnienie, przepływność i spójność. 

* **Brak potrzeby zarządzania schematami i indeksami bazy danych**
    * Nie musisz już martwić się o synchronizowanie indeksów i schematu bazy danych ze schematem aplikacji. U nas nie ma schematów. 
    * Aparat bazy danych usługi Azure Cosmos DB jest w pełni niezależny od schematów — automatycznie indeksuje wszystkie dane, które pozyska, bez wymagania żadnych schematów ani indeksów, a także umożliwia wykonywanie błyskawicznych zapytań. 

* **Niski koszt posiadania**
    * Od pięciu do dziesięciu razy [bardziej ekonomicznie](https://aka.ms/cosmos-db-tco-paper) niż w przypadku rozwiązania niezarządzanego.
    * Trzy razy taniej niż w przypadku usługi DynamoDB.

## <a name="capability-comparison"></a>Porównanie możliwości

Usługa Azure Cosmos DB zapewnia najlepsze możliwości relacyjnych i nierelacyjnych baz danych.

| Możliwości | Relacyjne bazy danych   | Nierelacyjne bazy danych (NoSQL) |    Azure Cosmos DB |
| --- | --- | --- | --- |
| Dystrybucja globalna | Nie | Nie | Tak, gotowa do użycia dystrybucja w ponad 30 regionach z międzyregionalnymi interfejsami API|
| Skalowanie w poziomie | Nie | Tak | Tak, możliwe jest niezależne skalowanie magazynu i przepływności | 
| Gwarancje dot. opóźnienia | Nie | Tak | Tak, 99% odczytów trwa poniżej 10 ms, a 99% zapisów poniżej 15 ms | 
| Wysoka dostępność | Nie | Tak | Tak, usługa Cosmos DB jest zawsze włączona, umożliwia wymiany zgodne z modelem PACELC oraz udostępnia opcje automatycznego i ręcznego przechodzenia w tryb failover|
| Model danych + interfejs API | Relacyjny + SQL | Wiele modeli + OSS API | Wiele modeli + SQL + OSS API (więcej wkrótce) |
| Umowy SLA | Tak | Nie | Tak, kompleksowe umowy SLA dotyczące opóźnienia, przepływności, spójności i dostępności |


## <a name="next-steps"></a>Następne kroki
Rozpocznij pracę z usługą Azure Cosmos DB, korzystając z jednego z naszych przewodników:

* [Rozpoczynanie pracy z interfejsem API DocumentDB usługi Azure Cosmos DB](create-documentdb-dotnet.md)
* [Rozpoczynanie pracy z interfejsem API MongoDB usługi Azure Cosmos DB](create-mongodb-nodejs.md)
* [Rozpoczynanie pracy z interfejsem API Graph usługi Azure Cosmos DB](create-graph-dotnet.md)
* [Rozpoczynanie pracy z interfejsem API tabeli usługi Azure Cosmos DB](create-table-dotnet.md)

