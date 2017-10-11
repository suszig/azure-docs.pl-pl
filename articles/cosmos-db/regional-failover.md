---
title: "Regionalnej pracy awaryjnej w usłudze Azure DB rozwiązania Cosmos | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o sposobie ręcznego i automatycznego działania trybu failover z bazy danych Azure rozwiązania Cosmos."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 446e2580-ff49-4485-8e53-ae34e08d997f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3d8ba08bc9f99cb77c9f03949fc5db299eb222c8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="automatic-regional-failover-for-business-continuity-in-azure-cosmos-db"></a>Automatyczne regionalnej pracy w trybie failover ciągłość prowadzenia działalności biznesowej w usłudze Azure DB rozwiązania Cosmos
Azure DB rozwiązania Cosmos upraszcza globalne rozkład danych oferując do pełnego zarządzania, [konta bazy danych w przypadku](distribute-data-globally.md) zawierających wyczyść wady i zalety między spójności, dostępność i wydajność, wszystkie z odpowiednimi gwarancje. Rozwiązania cosmos DB kont oferują wysoką dostępność, opóźnienia ms w pojedynczą cyfrą [dobrze zdefiniowane poziomy spójności](consistency-levels.md), przezroczysty regionalnej pracy awaryjnej z wielu interfejsów API oraz możliwość elastycznie skalować przepływność i Magazyn między globu. 

Rozwiązania cosmos bazy danych obsługuje zarówno jawnego i zasad zmiennych praca awaryjna które umożliwiają kontrolowanie zachowania systemu end-to-end w przypadku niepowodzenia. W tym artykule opisano:

* Jak działają ręcznej pracy awaryjnej w bazie danych rozwiązania Cosmos
* Jak pracy automatycznego przechodzenia w tryb failover do rozwiązania Cosmos bazy danych i co się dzieje, gdy dane Centrum umieszczane w dół?
* Jak można użyć ręcznej pracy awaryjnej w architekturach aplikacji?

Można również zapoznać się regionalnej pracy w trybie Failover w tym Azure piątek wideo Scott Hanselman i Ramana Karthik główny Menedżer zespołu inżynieryjnego.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

## <a id="ConfigureMultiRegionApplications"></a>Konfigurowanie aplikacji w przypadku
Zanim firma Microsoft Poznaj tryby pracy awaryjnej przyjrzymy się jak konfigurować aplikacji skorzystać w przypadku dostępności i zapewnienie odporności w wypadku regionalnej pracy w trybie Failover.

* Najpierw należy wdrożyć aplikację w wielu regionach
* Aby zapewnić dostęp małe opóźnienia z każdego regionu, wdrażania aplikacji, skonfigurować odpowiednie [listy regiony preferowanych](https://msdn.microsoft.com/library/microsoft.azure.documents.client.connectionpolicy.preferredlocations.aspx#P:Microsoft.Azure.Documents.Client.ConnectionPolicy.PreferredLocations) dla każdego regionu, za pomocą jednego z obsługiwanych zestawów SDK.

Poniższy fragment kodu przedstawia sposób inicjowania aplikacji w przypadku. Tutaj, konto bazy danych Azure rozwiązania Cosmos `contoso.documents.azure.com` jest skonfigurowany z dwóch regionach - zachodnie stany USA, Europa Północna, Europa a. 

* Aplikacja jest wdrażana w regionu zachodnie stany USA, (na przykład przy użyciu usługi aplikacji Azure) 
* Skonfigurowany z użyciem `West US` jako pierwszy preferowany region dla małych opóźnieniach odczytuje
* Skonfigurowany z użyciem `North Europe` jako drugi preferowany region (wysokiej dostępności podczas awarii regionalne)

W interfejsie API usługi DocumentDB ta konfiguracja wygląda następujący fragment kodu:

```cs
ConnectionPolicy usConnectionPolicy = new ConnectionPolicy 
{ 
    ConnectionMode = ConnectionMode.Direct,
    ConnectionProtocol = Protocol.Tcp
};

usConnectionPolicy.PreferredLocations.Add(LocationNames.WestUS);
usConnectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

DocumentClient usClient = new DocumentClient(
    new Uri("https://contosodb.documents.azure.com"),
    "memf7qfF89n6KL9vcb7rIQl6tfgZsRt5gY5dh3BIjesarJanYIcg2Edn9uPOUIVwgkAugOb2zUdCR2h0PTtMrA==",
    usConnectionPolicy);
```

Aplikacja jest wdrażana również w regionie Europa Północna, Europa w kolejności preferowanych regionów wycofane. Oznacza to region Europa Północna, Europa jest określony jako pierwszy dla odczytów małe opóźnienia. Następnie regionu zachodnie stany USA jest określana jako drugi preferowany region wysokiej dostępności podczas regionalnymi awariami.

Na poniższym diagramie architektury przedstawia wdrożenia aplikacji w przypadku których rozwiązania Cosmos bazę danych i aplikacji są skonfigurowane w czterech Azure regionów geograficznych.  

![Wdrożenie aplikacji rozproszonych globalnie z bazy danych Azure rozwiązania Cosmos](./media/regional-failover/app-deployment.png)

Teraz Przyjrzyjmy się jak usługa DB rozwiązania Cosmos obsługuje regionalnymi awariami za pomocą automatycznej pracy awaryjnej. 

## <a id="AutomaticFailovers"></a>Automatycznego przechodzenia w tryb failover
W rzadkich Azure regionalnej awarii lub awarii centrum danych rozwiązania Cosmos DB automatyczne wyzwolenie procesu przechodzenia w tryb failover wszystkich kont DB rozwiązania Cosmos obecności w regionie, których dotyczy. 

**Co się stanie, jeśli region odczytu ma awarii?**

Rozwiązania cosmos konta bazy danych za pomocą obszaru odczytu w jednym z odpowiednim regionów są odłączone od ich region zapisu i automatycznie oznaczone w trybie offline. Zestawy SDK DB rozwiązania Cosmos implementuje protokół regionalnych odnajdywania, który umożliwia im to automatycznie Wykryj, kiedy region jest dostępny i Przekierowanie do odczytu wywołań następny dostępny region na liście preferowanego regionu. Jeśli żaden regionów na liście preferowanych region nie jest dostępny, wywołania automatycznie wrócić do bieżącego obszaru zapisu. W kodzie aplikacji do obsługi regionalnej pracy w trybie Failover są konieczne żadne zmiany. W trakcie tego procesu całego gwarancje spójności nadal uznawane za DB rozwiązania Cosmos.

![Błędy region odczytu w usłudze Azure DB rozwiązania Cosmos](./media/regional-failover/read-region-failures.png)

Po awarii wznawia działanie dotyczy regionu, wszystkich odpowiednich kont DB rozwiązania Cosmos w regionie są automatycznie odzyskiwane przez usługę. Konta rozwiązania cosmos bazy danych, które ma odczytu regionu w regionie, których dotyczy, a następnie automatycznie synchronizować z bieżącego obszaru do zapisu i włączyć online. Zestawy SDK DB rozwiązania Cosmos odnajdywanie dostępności nowych regionu i ocenić, czy należy wybrać region jako bieżący obszar odczytu, na podstawie listy preferowanego regionu skonfigurowane przez aplikację. Kolejne odczyty są przekierowywane do regionu odzyskane bez konieczności wprowadzania jakichkolwiek zmian w kodzie aplikacji.

**Co się stanie, jeśli region zapisu ma awarii?**

Region wykorzystywanych w przypadku bieżącego obszaru zapisu dla danego konta rozwiązania Cosmos DB, następnie region zostaną automatycznie oznaczone w trybie offline. Następnie alternatywne region jest podnoszony, ponieważ region zapisu konto bazy danych rozwiązania Cosmos. Można w pełni kontrolować kolejność zaznaczania region dla kont rozwiązania Cosmos bazy danych za pośrednictwem portalu Azure lub [programowo](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_FailoverPriorityChange). 

![Priorytetów trybu failover dla bazy danych Azure rozwiązania Cosmos](./media/regional-failover/failover-priorities.png)

Podczas automatycznego przechodzenia w tryb failover DB rozwiązania Cosmos automatycznie wybierze kolejnego zapisu region dla danego konta rozwiązania Cosmos bazy danych na podstawie kolejności określony priorytet. 

![Zakończone niepowodzeniem regionu w usłudze Azure DB rozwiązania Cosmos](./media/regional-failover/write-region-failures.png)

Po awarii wznawia działanie dotyczy regionu, wszystkich odpowiednich kont DB rozwiązania Cosmos w regionie są automatycznie odzyskiwane przez usługę. 

* Rozwiązania cosmos DB kont z ich poprzednich region zapisu w regionie, których dotyczy, pozostanie w trybie offline dostępność odczytu nawet po odzyskaniu regionu. 
* Umożliwia wysyłanie zapytań tego obszaru można obliczyć wszelkie niezreplikowane zapisy podczas awarii, porównując go z danych dostępnych w bieżącym zapisu regionu. Oparte na potrzeby aplikacji, można wykonywać scalania i/lub konflikt rozpoznawania i zapisania ostatecznego zestawu zmian z powrotem do bieżącego obszaru zapisu. 
* Po ukończeniu scalania zmian, można przełączyć dotyczy region trybu online przez usunięcie i ponowne dodawanie regionu na koncie DB rozwiązania Cosmos. Po region jest dodany ponownie, można skonfigurować go jako regionu zapisu, wykonując ręcznego przełączania trybu failover za pośrednictwem portalu Azure lub [programowo](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate).

## <a id="ManualFailovers"></a>Ręczne praca awaryjna

Oprócz automatycznej pracy awaryjnej bieżącego obszaru zapisu danego konta rozwiązania Cosmos bazy danych może być zmieniony ręcznie dynamicznie do jednego z istniejących regionów odczytu. Ręczne przechodzenia w tryb failover może być inicjowane za pośrednictwem portalu Azure lub [programowo](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate). 

Zapewnienia ręcznej pracy awaryjnej **zero utraty danych** i **zero dostępności** utratą i bezpiecznie stanu zapisu transferu ze starego zapisu region na nową dla określonego konta DB rozwiązania Cosmos. Podobnie jak automatycznego przechodzenia w tryb failover SDK DB rozwiązania Cosmos automatycznie obsługuje zapisu region zmiany podczas ręcznej pracy awaryjnej i zapewnia, że wywołania są automatycznie przekierowywane do nowego regionu zapisu. W aplikacji do zarządzania trybu failover nie są konieczne nie zmiany kodu lub konfiguracji. 

![Ręcznej pracy awaryjnej w usłudze Azure DB rozwiązania Cosmos](./media/regional-failover/manual-failovers.png)

Typowe scenariusze, w którym ręcznego przełączania trybu failover może być przydatna, należą:

**Postępuj zgodnie z modelem zegara**: Jeśli aplikacje wzorce ruchu przewidywalną na podstawie czasu dnia, okresowo można zmienić stanu zapisu na najbardziej aktywne regionu geograficznego na podstawie czasu dnia.

**Aktualizacja usługi**: niektóre wdrożenia aplikacji rozproszonej globalny może wymagać ponownego routingu ruchu w innym regionie za pośrednictwem Menedżera ruchu podczas ich aktualizacji plan.. Takie wdrożenie aplikacji teraz umożliwia zachowanie stanu zapisu do regionu ręcznego przełączania trybu failover w przypadku, gdy ma być aktywne ruchu w oknie aktualizacji usługi.

**Ciągłość prowadzenia działalności biznesowej i odzyskiwania po awarii (BCDR) i rozwija wysokiej dostępności i odzyskiwania po awarii (HADR)**: większość aplikacji przedsiębiorstwa obejmują testy ciągłości biznesowej jako część procesu projektowania i wersji. BCDR i testowania HADR często jest to ważny krok w certyfikatach zgodności i gwarantujących dostępności usługi w przypadku regionalnej awarii. Można sprawdzić gotowość BCDR aplikacji korzystających z rozwiązania Cosmos bazy danych dla magazynu przez wyzwalania ręcznego przełączania trybu failover konta rozwiązania Cosmos bazy danych i/lub dodawanie i usuwanie region dynamicznie.

W tym artykule firma Microsoft przeglądowi sposób ręcznego i automatycznego pracy trybu failover do rozwiązania Cosmos bazy danych oraz sposobu konfigurowania kont DB rozwiązania Cosmos i aplikacje mają być dostępne globalnie. Przy użyciu funkcji obsługi replikacji globalne rozwiązania Cosmos DB, można zwiększyć czas oczekiwania na trasie i upewnij się, że są one wysokiej dostępności, nawet w przypadku niepowodzenia regionu. 

## <a id="NextSteps"></a>Następne kroki
* Dowiedz się więcej o sposobie obsługi DB rozwiązania Cosmos [dystrybucji globalne](distribute-data-globally.md)
* Dowiedz się więcej o [globalne spójności z bazy danych Azure rozwiązania Cosmos](consistency-levels.md)
* Tworzenie z wielu regionach, przy użyciu usługi Azure rozwiązania Cosmos DB [interfejsu API usługi DocumentDB](../cosmos-db/tutorial-global-distribution-documentdb.md)
* Dowiedz się, jak tworzyć [architektury w przypadku zapisywania](multi-region-writers.md) za pomocą usługi Azure DocumentDB

