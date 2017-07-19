---
title: "Wprowadzenie do interfejsu API tabel usługi Azure Cosmos DB | Microsoft Docs"
description: "Dowiedz się, jak korzystać z usługi Azure Cosmos DB do przechowywania i wysyłania zapytań do ogromnych wolumenów danych typu klucz-wartość z niskim opóźnieniem przy użyciu popularnych interfejsów API MongoDB OSS."
services: cosmos-db
author: bhanupr
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/09/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 245ce9261332a3d36a36968f7c9dbc4611a019b2
ms.openlocfilehash: 07dc5e1520cf60e19a1961ad85d2aea0cc7bd15f
ms.contentlocale: pl-pl
ms.lasthandoff: 06/09/2017


---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Wprowadzenie do usługi Azure Cosmos DB: Interfejs API tabel

[Azure Cosmos DB](introduction.md) to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft do aplikacji o krytycznym znaczeniu. Usługa Azure Cosmos DB zapewnia [kompleksową globalną dystrybucję](distribute-data-globally.md), [elastyczne skalowanie przepływności i pamięci](partition-data.md) w skali globalnej, milisekundowe opóźnienia w 99. percentylu, [pięć odpowiednio zdefiniowanych poziomów spójności](consistency-levels.md) oraz gwarantowaną wysoką dostępność, wspierane przez [wiodące w branży umowy SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Usługa Azure Cosmos DB [automatycznie indeksuje dane](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) bez konieczności zarządzania schematami i indeksami. To usługa wielomodelowa, obsługująca modele oparte na dokumentach, parach klucz-wartość, grafach i kolumnach. 

![Interfejs API magazynu tabel Azure (Azure Table Storage) i usługa Azure Cosmos DB](./media/table-introduction/premium-tables.png) 

Usługa Azure Cosmos DB obejmuje interfejs API tabel (wersja zapoznawcza) dla aplikacji, które wymagają magazynu kluczy i wartości z elastycznym schematem, przewidywalnej wydajności, globalnej dystrybucji i wysokiej przepływności. Interfejs API tabel obejmuje tę samą funkcjonalność co magazyn tabel Azure, ale korzysta z zalet aparatu usługi Azure Cosmos DB. 

Z magazynu tabel Azure można także korzystać w przypadku tabel o wysokich wymaganiach dotyczących pamięci i niższych wymaganiach w kwestii przepływności. Przyszła aktualizacja usługi Azure Cosmos DB wprowadzi obsługę tabel zoptymalizowanych pod kątem pamięci, a istniejące i nowe konta magazynów tabel Azure zostaną uaktualnione do usługi Azure Cosmos DB.

## <a name="premium-and-standard-table-apis"></a>Interfejsy API tabel, premium i standardowe
Jeśli aktualnie używasz magazynu tabel Azure, możesz uzyskać następujące korzyści, jeśli zaczniesz używać wersji zapoznawczej „tabel premium” usługi Azure Cosmos DB:

|  | Azure Table Storage | Usługa Azure Cosmos DB: magazyn tabel (wersja zapoznawcza) |
| --- | --- | --- |
| Opóźnienie | Niewielkie, ale brak górnych granic opóźnienia | Milisekundowe opóźnienie odczytu i zapisu, wspierane przez odczyty o opóźnieniu <10 ms i zapisy o opóźnieniu <15 ms w 99. percentylu, w dowolnej skali, w dowolnym miejscu na świecie |
| Przepływność | Wysoka skalowalność, brak dedykowanego modelu przepływności. Tabele mają limit skalowalności 20 000 operacji/s | Wysoka skalowalność dzięki [dedykowanej zarezerwowanej przepływności na tabelę](request-units.md), wspieranej przez umowy SLA. Konta nie mają górnego limitu przepływności i obsługują >10 milionów operacji/s na tabelę |
| Dystrybucja globalna | Jeden region z jednym opcjonalnym dodatkowym regionem odczytu, co zapewnia wysoką dostępność. Nie można zainicjować trybu failover | [Kompleksowa dystrybucja globalna](distribute-data-globally.md) do ponad 30 regionów, obsługa [automatycznego i ręcznego trybu failover](regional-failover.md) w dowolnym momencie i dowolnym miejscu na świecie |
| Indeksowanie | Tylko podstawowy indeks PartitionKey i RowKey. Brak dodatkowych indeksów | Automatyczne i kompletne indeksowanie wszystkich właściwości, brak zarządzania indeksem |
| Zapytanie | Wykonanie zapytania wykorzystuje indeks klucza podstawowego, a w przeciwnym przypadku skanuje. | Zapytania mogą korzystać z automatycznego indeksowania właściwości, co skraca czas odpowiedzi. Mechanizm bazy danych usługi Azure Cosmos DB obsługuje agregację, rozwiązania geoprzestrzenne i sortowanie. |
| Spójność | Na poziomie „strong” w regionie podstawowym, na poziomie „eventual” w regionie pomocniczym | [pięć dobrze zdefiniowanych poziomów spójności](consistency-levels.md), równoważących dostępność, opóźnienia, przepływność i spójność w zależności od potrzeb aplikacji |
| Cennik | Optymalizacja pod kątem pamięci  | Optymalizacja pod kątem przepływności |
| Umowy SLA | Dostępność 99,9% | Dostępność 99,99% w jednym regionie oraz możliwość dodania kolejnych regionów w celu podniesienia dostępności. [Wiodące w branży, kompleksowe umowy SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/) dotyczące ogólnej dostępności |

## <a name="how-to-get-started"></a>Jak zacząć

Utwórz konto usługi Azure Cosmos DB w witrynie [Azure Portal](https://portal.azure.com) i rozpocznij od dokumentu [Szybki start dla interfejsu API tabel przy użyciu platformy .NET](create-table-dotnet.md). 

## <a name="next-steps"></a>Następne kroki

Oto kilka wskazówek ułatwiających rozpoczęcie pracy:
* Rozpocznij pracę z [interfejsem API tabel usługi Azure Cosmos DB](create-table-dotnet.md) przy użyciu istniejącego zestawu SDK tabel platformy NET.
* Dowiedz się więcej o [globalnej dystrybucji przy użyciu usługi Azure Cosmos DB](distribute-data-globally.md).
* Dowiedz się więcej o [aprowizowanej przepływności w usłudze Azure Cosmos DB](request-units.md).

