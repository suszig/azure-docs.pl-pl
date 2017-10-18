---
title: "Wprowadzenie do interfejsu API tabel usługi Azure Cosmos DB | Microsoft Docs"
description: "Dowiedz się, jak korzystać z usługi Azure Cosmos DB i popularnych interfejsów API MongoDB OSS w celu przechowywania ogromnych wolumenów danych typu klucz-wartość i wykonywania zapytań na tych danych przy zachowaniu małych opóźnień."
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
ms.openlocfilehash: c184b02f46ac2ad66c6ce607967a687c82d31477
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Wprowadzenie do usługi Azure Cosmos DB: Interfejs API tabel

Usługa [Azure Cosmos DB](introduction.md) zapewnia interfejs API tabel (w wersji zapoznawczej) dla aplikacji korzystających z usługi Azure Table Storage, które wymagają funkcji warstwy Premium, takich jak:

* [Gotowa do użytku dystrybucja globalna](distribute-data-globally.md).
* [Dedykowana przepływność](partition-data.md) na całym świecie.
* Opóźnienie rzędu kilku milisekund na poziomie 99. percentyla.
* Gwarantowana wysoka dostępność.
* [Automatyczne indeksowanie pomocnicze](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

Aplikacje te mogą być migrowane do usługi Azure Cosmos DB przy użyciu interfejsu API tabel bez zmian kodu i mogą korzystać z funkcji warstwy Premium. Interfejs API tabel jest dostępny dla platformy .NET i języka Python.

Zalecamy obejrzenie poniższego klipu wideo, w którym Aravind Ramachandran wyjaśnia, jak rozpocząć pracę z interfejsem API tabel na potrzeby korzystania z usługi Azure Cosmos DB:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Table-API-for-Azure-Cosmos-DB/player]
> 
> 

## <a name="premium-and-standard-table-apis"></a>Interfejsy API tabel, premium i standardowe
Jeśli aktualnie używasz usługi Table Storage, po przejściu na wersję zapoznawczą usługi Azure Cosmos DB z „tabelami premium” uzyskasz następujące korzyści:

| | Azure Table Storage | Usługa Azure Cosmos DB: magazyn tabel (wersja zapoznawcza) |
| --- | --- | --- |
| Opóźnienie | Niewielkie, ale brak górnych granic opóźnienia. | Milisekundowe opóźnienie odczytu i zapisu oraz odczyt o opóźnieniu <10 ms i zapis o opóźnieniu <15 ms w 99. percentylu, w dowolnej skali, w dowolnym miejscu na świecie. |
| Przepływność | Zmienny model przepływności. Tabele mają limit skalowalności 20 000 operacji/s. | Wysoka skalowalność dzięki [dedykowanej zarezerwowanej przepływności na tabelę](request-units.md), gwarantowanej umowami SLA. Konta nie mają górnego limitu przepływności i obsługują >10 milionów operacji/s na tabelę. |
| Dystrybucja globalna | Jeden region z jednym opcjonalnym dodatkowym regionem odczytu, co zapewnia wysoką dostępność. Nie można zainicjować trybu failover. | [Kompleksowa dystrybucja globalna](distribute-data-globally.md) do ponad 30 regionów. Obsługa [automatycznego i ręcznego trybu failover](regional-failover.md) w dowolnym momencie i każdym miejscu na świecie. |
| Indeksowanie | Tylko podstawowy indeks PartitionKey i RowKey. Brak dodatkowych indeksów. | Automatyczne i kompletne indeksowanie wszystkich właściwości, brak zarządzania indeksem. |
| Zapytanie | Wykonanie zapytania wykorzystuje indeks klucza podstawowego, a w przeciwnym przypadku skanuje. | Zapytania mogą korzystać z automatycznego indeksowania właściwości, co skraca czas odpowiedzi. Aparat bazy danych usługi Azure Cosmos DB obsługuje wartości zagregowane, rozwiązania geoprzestrzenne i sortowanie. |
| Spójność | Na poziomie „strong” w regionie podstawowym, na poziomie „eventual” w regionie pomocniczym. | [Pięć dobrze zdefiniowanych poziomów spójności](consistency-levels.md), równoważących dostępność, opóźnienia, przepływność i spójność w zależności od potrzeb aplikacji. |
| Cennik | Optymalizacja pod kątem magazynu. | Optymalizacja pod kątem przepływności. |
| Umowy SLA | Dostępność na poziomie 99,99%. | Dostępność na poziomie 99,99% w jednym regionie oraz możliwość dodania kolejnych regionów w celu podniesienia dostępności. [Wiodące w branży, kompleksowe umowy SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/) dotyczące ogólnej dostępności. |

## <a name="get-started"></a>Rozpoczęcie pracy

Utwórz konto usługi Azure Cosmos DB w witrynie [Azure Portal](https://portal.azure.com). Następnie zapoznaj się z naszym [przewodnikiem Szybki Start dotyczącym korzystania z interfejsu API tabel przy użyciu programu .NET](create-table-dotnet.md). 

## <a name="next-steps"></a>Następne kroki

Oto kilka wskazówek ułatwiających rozpoczęcie pracy:
* [Tworzenie aplikacji .NET za pomocą interfejsu API tabel](create-table-dotnet.md)
* [Opracowywanie zawartości przy użyciu interfejsu API tabel na platformie .NET](tutorial-develop-table-dotnet.md)
* [Wykonywanie zapytań dotyczących danych tabel przy użyciu interfejsu API tabel](tutorial-query-table.md)
* [Dowiedz się, jak skonfigurować dystrybucję globalną usługi Azure Cosmos DB przy użyciu interfejsu API tabel](tutorial-global-distribution-table.md)
* [Azure Cosmos DB — interfejs API tabel dla platformy .NET](table-sdk-dotnet.md)
* [Azure Cosmos DB — zestaw SDK tabel dla języka Python](table-sdk-python.md)

