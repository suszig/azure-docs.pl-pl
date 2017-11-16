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
ms.date: 11/15/2017
ms.author: anhoh
ms.openlocfilehash: eca720f365a00070afd2a657829f5b108ab91fb9
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="introduction-to-azure-cosmos-db-api-for-mongodb"></a>Wprowadzenie do platformy Azure rozwiązania Cosmos DB: interfejs API dla bazy danych MongoDB

[Azure Cosmos DB](../cosmos-db/introduction.md) to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft do aplikacji o krytycznym znaczeniu. Usługa Azure Cosmos DB zapewnia [kompleksową globalną dystrybucję](distribute-data-globally.md), [elastyczne skalowanie przepływności i pamięci](partition-data.md) w skali globalnej, milisekundowe opóźnienia w 99. percentylu, [pięć odpowiednio zdefiniowanych poziomów spójności](consistency-levels.md) oraz gwarantowaną wysoką dostępność, wspierane przez [wiodące w branży umowy SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Usługa Azure Cosmos DB [automatycznie indeksuje dane](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) bez konieczności zarządzania schematami i indeksami. To usługa wielomodelowa, obsługująca modele dokumentowe, klucz-wartość, wykresy i kolumny. 

![Azure rozwiązania Cosmos bazy danych: Baza danych MongoDB interfejsu API](./media/mongodb-introduction/cosmosdb-mongodb.png) 

Azure DB rozwiązania Cosmos baz danych może służyć do przechowywania danych dla aplikacji napisanych dla [MongoDB](https://docs.mongodb.com/manual/introduction/). Ta funkcja oznacza, że przy użyciu istniejących [sterowniki](https://docs.mongodb.org/ecosystem/drivers/), aplikacji tworzonych na potrzeby bazy danych MongoDB mogą teraz komunikować się z bazy danych Azure rozwiązania Cosmos i użyć bazy danych DB rozwiązania Cosmos Azure zamiast bazy danych MongoDB. W wielu przypadkach można przełączać z przy użyciu bazy danych MongoDB do bazy danych Azure rozwiązania Cosmos, zmieniając po prostu ciąg połączenia. Z tej funkcji, umożliwia łatwe tworzenie i uruchamianie aplikacji bazy danych MongoDB na platformie Azure w chmurze z dystrybucji globalne DB rozwiązania Cosmos Azure i [kompleksowe SLA branży](https://azure.microsoft.com/support/legal/sla/cosmos-db), kontynuując używanie znanych umiejętności Narzędzia i bazy danych mongodb.

**Zgodność z bazy danych MongoDB**: korzystania z istniejącej bazy danych MongoDB wiedzy, kod aplikacji i narzędziami bazy danych Azure rozwiązania Cosmos implementuje ten protokół przewodowy 3.4 bazy danych MongoDB (wersja 5) i obsługuje [potoku agregacji bazy danych MongoDB](mongodb-feature-support.md#aggregation-pipeline). Można tworzyć aplikacje przy użyciu bazy danych MongoDB i wdrożyć je do produkcji, używając w pełni zarządzana i globalnie rozproszone usługi bazy danych Azure rozwiązania Cosmos.

## <a name="what-is-the-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>Jakie są zalety korzystania z bazy danych rozwiązania Cosmos Azure dla aplikacji bazy danych MongoDB?

**Elastycznie skalowalne przepływność i Magazyn:** spełniają wymagania aplikacji wymaga przez łatwe skalowanie w górę lub w dół bazy danych MongoDB. Dane są przechowywane na dyskach półprzewodnikowych (SSD) dla małych opóźnień przewidywalne. Azure DB rozwiązania Cosmos obsługuje bazy danych MongoDB kolekcje, które można skalować do niemal nieograniczonego rozmiaru magazynu i udostępnionej przepływności. W miarę wzrostu aplikacji usługę Azure Cosmos DB można bezproblemowo elastycznie skalować z przewidywalną wydajnością. 

**W przypadku replikacji:** bazy danych Azure rozwiązania Cosmos niewidocznie są replikowane do wszystkich regionach jest skojarzony z Twoim kontem bazy danych MongoDB, umożliwiając tworzenie aplikacji, które wymagają globalny dostęp do danych, zapewniając wady i zalety między spójności, dostępność i wydajność, wszystkie odpowiednie gwarancje. Usługa Azure Cosmos DB zawiera przezroczyste rozwiązania regionalnej pracy awaryjnej z międzyregionalnymi interfejsami API oraz możliwość elastycznego skalowania przepływności i magazynów na całym świecie. Dowiedz się więcej w [globalnie dystrybucji danych](distribute-data-globally.md).

**Nie zarządzania serwerem**: nie masz, zarządzanie i skalowania bazy danych MongoDB. Azure DB rozwiązania Cosmos jest w pełni zarządzana usługa, co oznacza, że nie trzeba zarządzać infrastruktury ani maszyn wirtualnych. Azure DB rozwiązania Cosmos jest dostępna w 30 + [regiony platformy Azure](https://azure.microsoft.com/regions/services/).

**Dostosowywalne poziomy spójności:** wybierz z pięciu dobrze zdefiniowane poziomy spójności w celu osiągnięcia optymalnego kompromisu między wydajnością a spójnością. Dla zapytań i operacji odczytu usługa Azure Cosmos DB oferuje pięć różnych poziomów spójności: „silna”, „powiązana nieaktualność”, „sesja”, „spójny prefiks” i „ostateczna”. Te szczegółowe, dokładnie zdefiniowane poziomy spójności umożliwiają ustalanie optymalnych kompromisów między spójnością, dostępnością i opóźnieniem. Aby dowiedzieć się więcej, zobacz [Maksymalizowanie dostępności i wydajności za pomocą poziomów spójności](consistency-levels.md).

**Automatyczne indeksowanie**: Domyślnie bazy danych rozwiązania Cosmos Azure automatycznie indeksuje wszystkie właściwości w dokumentach w Twojej bazy danych MongoDB bazy danych i nie oczekuje ani nie wymaga żadnego schematu lub tworzenia indeksów pomocniczych. Ponadto funkcja unikatowy indeks umożliwia ograniczenie unikatowości dla wszystkich pól dokumentu, które już są automatycznie indeksowane w usłudze Azure DB rozwiązania Cosmos.

**Klasy Enterprise**: bazy danych rozwiązania Cosmos Azure obsługuje wiele replik lokalnych do dostarczania 99,99% dostępności i ochrony danych w wypadku niepowodzenia lokalne i regionalne. Azure DB rozwiązania Cosmos ma klasy enterprise [certyfikaty zgodności](https://www.microsoft.com/trustcenter) i funkcje zabezpieczeń. 

Dowiedz się więcej, w tym Azure piątek wideo z Scott Hanselman i Azure rozwiązania Cosmos DB główny Engineering Menedżer Kirill Gavrylyuk.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Introducing-Azure-Cosmos-DB/player]
> 

## <a name="how-to-get-started"></a>Jak zacząć

Wykonaj poradniki Szybki Start bazy danych MongoDB, aby utworzyć konto bazy danych Azure rozwiązania Cosmos i migracji istniejącej aplikacji bazy danych MongoDB do używania bazy danych Azure rozwiązania Cosmos, lub utworzyć nowy:

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
