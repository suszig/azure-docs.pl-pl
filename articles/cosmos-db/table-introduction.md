---
title: "Wprowadzenie do interfejsu API tabel usługi Azure Cosmos DB | Microsoft Docs"
description: "Dowiedz się, jak korzystać z usługi Azure Cosmos DB do przechowywania i wysyłania zapytań do ogromnych wolumenów danych typu klucz-wartość z niskim opóźnieniem przy użyciu popularnych interfejsów API MongoDB OSS."
services: cosmosdb
author: bhanupr
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/10/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 8226ecfb5fa8c70b162ae776679302db6cfdcdbc
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017


---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Wprowadzenie do usługi Azure Cosmos DB: Interfejs API tabel

[Azure Cosmos DB](introduction.md) to rozproszona globalnie wielomodelowa usługa bazy danych firmy Microsoft do aplikacji o krytycznym znaczeniu. Usługa Azure Cosmos DB zapewnia [kompleksową globalną dystrybucję](../documentdb/documentdb-distribute-data-globally.md), [elastyczne skalowanie przepływności i pamięci](partition-data.md) w skali globalnej, milisekundowe opóźnienia w 99. percentylu, [pięć odpowiednio zdefiniowanych poziomów spójności](../documentdb/documentdb-consistency-levels.md) oraz gwarantowaną wysoką dostępność, wspierane przez [wiodące w branży umowy SLA](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/). Usługa Azure Cosmos DB [automatycznie indeksuje dane](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) bez konieczności zarządzania schematami i indeksami. To usługa wielomodelowa, obsługująca modele dokumentowe, klucz-wartość, wykresy i kolumny. 

![Interfejs API magazynu tabel Azure (Azure Table Storage) i usługa Azure Cosmos DB](./media/table-introduction/premium-tables.png) 

Usługa Azure Cosmos DB zapewnia interfejs API tabel dla aplikacji wymagających magazynu klucz-wartość o elastycznym schemacie. Zestawy SDK [magazynu tabel Azure](../storage/storage-introduction.md) i interfejsy API REST można wykorzystywać do pracy z usługą Azure Cosmos DB. Usługa Azure Cosmos DB umożliwia tworzenie tabel o wysokich wymaganiach dotyczących przepływności. Usługa Azure Cosmos DB obsługuje tabele o zoptymalizowanej przepływności (nazywane nieformalnie „tabelami premium”), dostępne obecnie w publicznej wersji zapoznawczej. 

Z magazynu tabel Azure można także korzystać w przypadku tabel o wysokich wymaganiach dotyczących pamięci i niższych wymaganiach w kwestii przepływności. Przyszła aktualizacja usługi Azure Cosmos DB wprowadzi obsługę tabel zoptymalizowanych pod kątem pamięci, a istniejące i nowe konta magazynów tabel Azure zostaną uaktualnione do usługi Azure Cosmos DB.

## <a name="premium-and-standard-table-apis"></a>Interfejsy API tabel, premium i standardowe
Jeśli aktualnie używasz magazynu tabel Azure, możesz uzyskać następujące korzyści, jeśli zaczniesz używać wersji zapoznawczej „tabel premium” usługi Azure Cosmos DB:

|  | Azure Table Storage | Usługa Azure Cosmos DB: magazyn tabel (wersja zapoznawcza) |
| --- | --- | --- |
| Opóźnienie | Niewielkie, ale brak górnych granic opóźnienia | Milisekundowe opóźnienie odczytu i zapisu, wspierane przez odczyty o opóźnieniu <10 ms i zapisy o opóźnieniu <15 ms w 99. percentylu, w dowolnej skali, w dowolnym miejscu na świecie |
| Przepływność | Wysoka skalowalność, brak dedykowanego modelu przepływności. Tabele mają limit skalowalności 20 000 operacji/s | Wysoka skalowalność dzięki [dedykowanej zarezerwowanej przepływności na tabelę](../documentdb/documentdb-request-units.md), wspieranej przez umowy SLA. Konta nie mają górnego limitu przepływności i obsługują >10 milionów operacji/s na tabelę |
| Dystrybucja globalna | Jeden region z jednym opcjonalnym dodatkowym regionem odczytu, co zapewnia wysoką dostępność. Nie można zainicjować trybu failover | [Kompleksowa dystrybucja globalna](../documentdb/documentdb-distribute-data-globally.md) do ponad 30 regionów, obsługa [automatycznego i ręcznego trybu failover](../documentdb/documentdb-regional-failovers.md) w dowolnym momencie i dowolnym miejscu na świecie |
| Indeksowanie | Tylko podstawowy indeks PartitionKey i RowKey. Brak dodatkowych indeksów | Automatyczne i kompletne indeksowanie wszystkich właściwości, brak zarządzania indeksem |
| Zapytanie | Wykonanie zapytanie wykorzystuje indeks klucza podstawowego, a w przeciwnym wypadku skanuje. | Zapytania mogą korzystać z automatycznego indeksowania właściwości, co skraca czas odpowiedzi. Mechanizm bazy danych usługi Azure Cosmos DB obsługuje agregację, rozwiązania geoprzestrzenne i sortowanie. |
| Spójność | Na poziomie „strong” w regionie podstawowym, na poziomie „eventual” w regionie pomocniczym | [pięć dobrze zdefiniowanych poziomów spójności](../documentdb/documentdb-consistency-levels.md), równoważących dostępność, opóźnienia, przepływność i spójność w zależności od potrzeb aplikacji |
| Cennik | Optymalizacja pod kątem pamięci  | Optymalizacja pod kątem przepływności |
| Umowy SLA | Dostępność 99,9% | Dostępność 99,99% w jednym regionie oraz możliwość dodania kolejnych regionów w celu podniesienia dostępności. [Wiodące w branży, kompleksowe umowy SLA](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) dotyczące ogólnej dostępności |

## <a name="how-to-get-started"></a>Jak zacząć

Utwórz konto usługi Azure Cosmos DB w witrynie [Azure Portal](https://portal.azure.com) i rozpocznij od dokumentu [Szybki start dla interfejsu API tabel przy użyciu platformy .NET](create-table-dotnet.md). 

## <a name="next-steps"></a>Następne kroki

Oto kilka wskazówek ułatwiających rozpoczęcie pracy:
* Rozpocznij pracę z [interfejsem API tabel usługi Azure Cosmos DB](create-table-dotnet.md) przy użyciu istniejącego zestawu SDK tabel platformy NET.
* Dowiedz się więcej o [globalnej dystrybucji przy użyciu usługi Azure Cosmos DB](../documentdb/documentdb-distribute-data-globally.md).
* Dowiedz się więcej o [aprowizowanej przepływności w usłudze Azure Cosmos DB](../documentdb/documentdb-request-units.md).
