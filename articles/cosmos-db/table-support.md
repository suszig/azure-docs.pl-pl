---
title: "Pomocy technicznej platformy Azure Table Storage w usłudze Azure DB rozwiązania Cosmos | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak interfejsu API Azure rozwiązania Cosmos DB tabeli i tabele magazynu Azure współdziałać ze sobą."
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: 
ms.assetid: 378f00f2-cfd9-4f6b-a9b1-d1e4c70799fd
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: c0c8f1aee75c4ee5cc35758b71ef573637fd3edd
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="developing-with-azure-cosmos-db-table-api-and-azure-table-storage"></a>Tworzenie z interfejsu API Azure rozwiązania Cosmos DB tabeli i tabel Azure Table storage

Interfejsu API Azure rozwiązania Cosmos DB tabeli magazynu tabel Azure współużytkować ten sam model danych tabeli i udostępnianie tego samego tworzenia, usuwania, aktualizowania lub operacje zapytań za pośrednictwem ich zestawów SDK. 

## <a name="developing-with-the-azure-cosmos-db-table-api"></a>Programowania z użyciem Azure rozwiązania Cosmos API tabeli bazy danych

W tej chwili [interfejsu API Azure rozwiązania Cosmos DB tabeli](table-introduction.md) ma cztery zestawy SDK, które są dostępne dla rozwoju: 
- [Microsoft.Azure.CosmosDB.Table](https://aka.ms/tableapinuget) zestawu .NET SDK. Ta biblioteka zawiera tej samej klasy i metody podpisy jako publicznego [zestawu SDK usługi Magazyn systemu Windows Azure](https://www.nuget.org/packages/WindowsAzure.Storage), ale ma także możliwość łączenia z bazy danych Azure rozwiązania Cosmos kont przy użyciu interfejsu API tabeli. 
- [Zestaw SDK Python](table-sdk-python.md). Nowe rozwiązania Cosmos DB Python zestaw SDK usługi Azure jest tylko zestaw SDK obsługującą magazyn tabel Azure w języku Python. Zestaw SDK połączy się z magazynu tabel Azure i interfejsu API Azure rozwiązania Cosmos bazy danych tabeli.
- [Java SDK](table-sdk-java.md). Tego zestawu SDK usługi Magazyn Azure ma możliwość łączenia z bazy danych Azure rozwiązania Cosmos kont przy użyciu interfejsu API tabeli.
- [Zestaw node.js SDK](table-sdk-nodejs.md). Tego zestawu SDK usługi Magazyn Azure ma możliwość łączenia z bazy danych Azure rozwiązania Cosmos kont przy użyciu interfejsu API tabeli.

Dodatkowe informacje dotyczące pracy z interfejsem API tabeli jest dostępna w [— często zadawane pytania: tworzenie przy użyciu interfejsu API tabeli](faq.md#develop-with-the-table-api) artykułu.

## <a name="developing-with-the-azure-table-storage"></a>Tworzenie z magazynem tabel Azure

[Magazyn tabel Azure](table-storage-overview.md) ma wiele zestawów SDK jest dostępny i samouczki dostępne, które są teraz dostępne w [magazynu tabel Azure](table-storage-overview.md) sekcji. Artykuły te są aktualizowane jako współdziałanie między magazynem tabel Azure SDK i interfejsów API usługi Azure rozwiązania Cosmos DB tabeli staje się dostępny.  





