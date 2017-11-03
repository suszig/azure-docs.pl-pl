---
title: "Wprowadzenie do platformy Azure rozwiązania Cosmos DB: interfejs API dla bazy danych MongoDB | Dokumentacja firmy Microsoft"
description: "Poznaj wykorzystania bazy danych rozwiązania Cosmos Azure do przechowywania i bardzo dużych woluminów dokumentów JSON zapytania z niskim opóźnieniem, korzystając z popularnych interfejsów API bazy danych MongoDB OSS."
keywords: Co to jest MongoDB
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 4afaf40d-c560-42e0-83b4-a64d94671f0a
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: anhoh
ms.openlocfilehash: 4dbf91a3c1d6a287d7337647f9e059566c7ddbe5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-cosmos-db-api-for-mongodb"></a>Wprowadzenie do platformy Azure rozwiązania Cosmos DB: interfejs API dla bazy danych MongoDB

[Azure Cosmos DB](../cosmos-db/introduction.md) to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft do aplikacji o krytycznym znaczeniu. Usługa Azure Cosmos DB zapewnia [kompleksową globalną dystrybucję](distribute-data-globally.md), [elastyczne skalowanie przepływności i pamięci](partition-data.md) w skali globalnej, milisekundowe opóźnienia w 99. percentylu, [pięć odpowiednio zdefiniowanych poziomów spójności](consistency-levels.md) oraz gwarantowaną wysoką dostępność, wspierane przez [wiodące w branży umowy SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Usługa Azure Cosmos DB [automatycznie indeksuje dane](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) bez konieczności zarządzania schematami i indeksami. To usługa wielomodelowa, obsługująca modele dokumentowe, klucz-wartość, wykresy i kolumny. 

![Azure rozwiązania Cosmos bazy danych: Baza danych MongoDB interfejsu API](./media/mongodb-introduction/cosmosdb-mongodb.png) 

Rozwiązania cosmos DB bazy danych może służyć do przechowywania danych dla aplikacji napisanych dla [MongoDB](https://docs.mongodb.com/manual/introduction/). Oznacza to, że przy użyciu istniejących [sterowniki](https://docs.mongodb.org/ecosystem/drivers/), aplikacji tworzonych na potrzeby bazy danych MongoDB mogą teraz komunikować się z rozwiązania Cosmos bazy danych i użyć bazy danych DB rozwiązania Cosmos zamiast bazy danych MongoDB. W wielu przypadkach można przełączać z przy użyciu bazy danych MongoDB do rozwiązania Cosmos bazy danych, zmieniając po prostu ciąg połączenia. Z tej funkcji, umożliwia łatwe tworzenie i uruchamianie aplikacji bazy danych MongoDB na platformie Azure w chmurze z dystrybucji globalne DB rozwiązania Cosmos Azure i [kompleksowe branży SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db), kontynuując używanie znanych umiejętności Narzędzia i bazy danych mongodb.


## <a name="what-is-the-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>Jakie są zalety korzystania z bazy danych rozwiązania Cosmos Azure dla aplikacji bazy danych MongoDB?

**Elastycznie skalowalne przepływność i Magazyn:** łatwe skalowanie bazy danych MongoDB do potrzeb aplikacji. Dane są przechowywane na dyskach półprzewodnikowych (SSD, solid-state drive) dla zapewnienia przewidywalnych, niskich opóźnień. Rozwiązania cosmos bazy danych obsługuje bazy danych MongoDB kolekcje, które można skalować do niemal nieograniczonego rozmiaru magazynu i udostępnionej przepływności. Można też elastycznie skalować rozwiązania Cosmos DB z przewidywalną wydajnością bezproblemowo wraz z rozwojem aplikacji. 

**W przypadku replikacji:** DB rozwiązania Cosmos niewidocznie są replikowane do wszystkich regionach jest skojarzony z Twoim kontem bazy danych MongoDB, umożliwiając tworzenie aplikacji, które wymagają globalny dostęp do danych, zapewniając wady i zalety między spójności, dostępność i wydajność, wszystkie odpowiednie gwarancje. Rozwiązania cosmos DB zapewnia przezroczysty tryb failover regionalnych, z wielu interfejsów API oraz możliwość elastycznie skalować przepływność i magazyn na całym świecie. Dowiedz się więcej w [globalnie dystrybucji danych](distribute-data-globally.md).

**Zgodność z bazy danych MongoDB**: korzystania z istniejącej bazy danych MongoDB wiedzy, kod aplikacji i narzędzi. Można tworzyć aplikacje przy użyciu bazy danych MongoDB i wdrożyć je w środowisku produkcyjnym za pomocą w pełni zarządzana usługa bazy danych rozwiązania Cosmos globalnie rozproszone.

**Nie zarządzania serwerem**: nie masz, zarządzanie i skalowania bazy danych MongoDB. Rozwiązania cosmos bazy danych jest w pełni zarządzana usługa, co oznacza, że nie trzeba zarządzać infrastruktury ani maszyn wirtualnych. Rozwiązania cosmos bazy danych jest dostępna w 30 + [regiony platformy Azure](https://azure.microsoft.com/regions/services/).

**Możliwość dostosowania poziomu spójności**: możliwość wyboru spośród pięciu dobrze zdefiniowanych poziomów spójności w celu osiągnięcia optymalnego kompromisu między wydajnością a spójnością. Dla zapytań i operacji odczytu bazy danych rozwiązania Cosmos oferuje pięć różne poziomy spójności: silne, nieaktualność, sesji, prefiks spójne i "ostateczna". Te szczegółowe, dokładnie zdefiniowane poziomy spójności umożliwiają ustalanie optymalnych kompromisów między spójnością, dostępnością i opóźnieniem. Aby dowiedzieć się więcej, zobacz [Maksymalizowanie dostępności i wydajności za pomocą poziomów spójności](consistency-levels.md).

**Automatyczne indeksowanie**: domyślnie DB rozwiązania Cosmos automatycznie indeksuje wszystkie właściwości w dokumentach w Twojej bazy danych MongoDB bazy danych i nie oczekuje ani nie wymaga żadnego schematu lub tworzenia indeksów pomocniczych.

**Klasy Enterprise** -DB rozwiązania Cosmos Azure obsługuje wiele replik lokalnych do dostarczania 99,99% dostępności i ochrony danych w wypadku niepowodzenia lokalne i regionalne. Azure DB rozwiązania Cosmos ma klasy enterprise [certyfikaty zgodności](https://www.microsoft.com/trustcenter) i funkcje zabezpieczeń. 

Dowiedz się więcej, w tym Azure piątek wideo z Scott Hanselman i Azure rozwiązania Cosmos DB główny Engineering Menedżer Kirill Gavrylyuk.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Introducing-Azure-Cosmos-DB/player]
> 

## <a name="how-to-get-started"></a>Jak zacząć

Wykonaj poradniki Szybki Start bazy danych MongoDB, Utwórz konto DB rozwiązania Cosmos i migracji istniejącej aplikacji Mongo DB do użycia rozwiązania Cosmos DB, lub utworzyć nowy:

* [Migrowanie istniejących aplikacji sieci web Node.js bazy danych MongoDB](create-mongodb-nodejs.md).
* [Tworzenie aplikacji sieci web interfejsu API bazy danych MongoDB z usług .NET i portalu Azure](create-mongodb-dotnet.md)
* [Tworzenie aplikacji konsoli bazy danych MongoDB interfejsu API języka Java i portalu Azure](create-mongodb-java.md)

## <a name="next-steps"></a>Następne kroki

Informacji na temat interfejsu API bazy danych MongoDB DB rozwiązania Cosmos Azure została zintegrowana z ogólną dokumentację bazy danych rozwiązania Cosmos platformy Azure, ale poniżej przedstawiono kilka wskazówek, które ułatwią rozpoczęcie pracy:

* Postępuj zgodnie z [Połącz z kontem bazy danych MongoDB](connect-mongodb-account.md) samouczkiem, aby dowiedzieć się, jak uzyskać informacje o parametrach połączenia konta.
* Postępuj zgodnie z [MongoChef użycia z bazy danych Azure rozwiązania Cosmos](mongodb-mongochef.md) samouczkiem, aby dowiedzieć się, jak utworzyć połączenie między usługą Azure DB rozwiązania Cosmos bazy danych i aplikacji bazy danych MongoDB w MongoChef.
* Postępuj zgodnie z [migrowanie danych do bazy danych rozwiązania Cosmos Azure z protokołem obsługuje bazy danych mongodb](mongodb-migrate.md) samouczka w celu zaimportowania danych do interfejsu API dla bazy danych MongoDB.
* Połączyć z interfejsem API dotyczące korzystania z bazy danych MongoDB konta [Robomongo](mongodb-robomongo.md).
* Dowiedz się, ile RUs operacji za pomocą [GetLastRequestStatistics polecenia i portalu Azure metryki](request-units.md#GetLastRequestStatistics).
* Dowiedz się, jak [Konfigurowanie preferencji odczytu dla aplikacji rozproszonych globalnie](../cosmos-db/tutorial-global-distribution-mongodb.md).
