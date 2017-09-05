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
ms.date: 08/29/2017
ms.author: arramac
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 3ccc3b176df2f0a5d864554a74508292d272bd5a
ms.contentlocale: pl-pl
ms.lasthandoff: 08/30/2017

---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Wprowadzenie do usługi Azure Cosmos DB: Interfejs API tabel

Usługa [Azure Cosmos DB](introduction.md) udostępnia interfejs API tabel (wersja zapoznawcza) na potrzeby aplikacji napisanych dla usługi Azure Table Storage, które wymagają funkcji warstwy Premium, takich jak [kompleksowa dystrybucja globalna](distribute-data-globally.md), [dedykowana przepływność](partition-data.md) na całym świecie, milisekundowe opóźnienie w 99. percentylu, gwarantowana wysoka dostępność i [automatyczne indeksowanie pomocnicze](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). Te aplikacje mogą być migrowane do usługi Azure Cosmos DB przy użyciu interfejsu API tabel bez zmian kodu i mogą korzystać z funkcji warstwy Premium.

Zalecamy rozpoczęcie od obejrzenia poniższego klipu wideo, w którym Aravind Ramachandran wyjaśnia, jak rozpocząć pracę z interfejsem API tabel na potrzeby usługi Azure Cosmos DB.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Table-API-for-Azure-Cosmos-DB/player]
> 
> 

## <a name="premium-and-standard-table-apis"></a>Interfejsy API tabel, premium i standardowe
Jeśli aktualnie używasz magazynu tabel Azure, możesz uzyskać następujące korzyści, jeśli zaczniesz używać wersji zapoznawczej „tabel premium” usługi Azure Cosmos DB:

|  | Azure Table Storage | Usługa Azure Cosmos DB: magazyn tabel (wersja zapoznawcza) |
| --- | --- | --- |
| Opóźnienie | Niewielkie, ale brak górnych granic opóźnienia | Milisekundowe opóźnienie odczytu i zapisu oraz odczyt o opóźnieniu <10 ms i zapis o opóźnieniu <15 ms w 99. percentylu, w dowolnej skali, w dowolnym miejscu na świecie |
| Przepływność | Zmienny model przepływności. Tabele mają limit skalowalności 20 000 operacji/s | Wysoka skalowalność dzięki [dedykowanej zarezerwowanej przepływności na tabelę](request-units.md), wspieranej przez umowy SLA. Konta nie mają górnego limitu przepływności i obsługują >10 milionów operacji/s na tabelę |
| Dystrybucja globalna | Jeden region z jednym opcjonalnym dodatkowym regionem odczytu, co zapewnia wysoką dostępność. Nie można zainicjować trybu failover | [Kompleksowa dystrybucja globalna](distribute-data-globally.md) do ponad 30 regionów, obsługa [automatycznego i ręcznego trybu failover](regional-failover.md) w dowolnym momencie i dowolnym miejscu na świecie |
| Indeksowanie | Tylko podstawowy indeks PartitionKey i RowKey. Brak dodatkowych indeksów | Automatyczne i kompletne indeksowanie wszystkich właściwości, brak zarządzania indeksem |
| Zapytanie | Wykonanie zapytania wykorzystuje indeks klucza podstawowego, a w przeciwnym przypadku skanuje. | Zapytania mogą korzystać z automatycznego indeksowania właściwości, co skraca czas odpowiedzi. Mechanizm bazy danych usługi Azure Cosmos DB obsługuje agregację, rozwiązania geoprzestrzenne i sortowanie. |
| Spójność | Na poziomie „strong” w regionie podstawowym, na poziomie „eventual” w regionie pomocniczym | [Pięć dobrze zdefiniowanych poziomów spójności](consistency-levels.md), równoważących dostępność, opóźnienia, przepływność i spójność w zależności od potrzeb aplikacji |
| Cennik | Optymalizacja pod kątem pamięci  | Optymalizacja pod kątem przepływności |
| Umowy SLA | Dostępność 99,9% | Dostępność 99,99% w jednym regionie oraz możliwość dodania kolejnych regionów w celu podniesienia dostępności. [Wiodące w branży, kompleksowe umowy SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/) dotyczące ogólnej dostępności |

## <a name="how-to-get-started"></a>Jak zacząć

Utwórz konto usługi Azure Cosmos DB w witrynie [Azure Portal](https://portal.azure.com) i rozpocznij od dokumentu [Szybki start dla interfejsu API tabel przy użyciu platformy .NET](create-table-dotnet.md). 

## <a name="next-steps"></a>Następne kroki

Oto kilka wskazówek ułatwiających rozpoczęcie pracy:
* [Tworzenie aplikacji .NET za pomocą interfejsu API tabel](create-table-dotnet.md)
* [Opracowywanie zawartości przy użyciu interfejsu API tabel na platformie .NET](tutorial-develop-table-dotnet.md)
* [Wykonywanie zapytań dotyczących danych tabel przy użyciu interfejsu API tabel](tutorial-query-table.md)
* [Jak skonfigurować dystrybucję globalną usługi Azure Cosmos DB przy użyciu interfejsu API tabel](tutorial-global-distribution-table.md)
* [Zestaw SDK interfejsu API tabel usługi Azure Cosmos DB dla platformy .NET](table-sdk-dotnet.md)

