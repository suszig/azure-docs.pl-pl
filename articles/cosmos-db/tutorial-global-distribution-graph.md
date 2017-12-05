---
title: "Samouczek usługi Azure DB rozwiązania Cosmos dystrybucji globalne dla interfejsu API programu Graph | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować bazy danych Azure rozwiązania Cosmos dystrybucji globalnego przy użyciu interfejsu API programu Graph."
services: cosmos-db
keywords: globalne dystrybucji, wykres, gremlin
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: denlee
ms.custom: mvc
ms.openlocfilehash: eb55bdee60400b4b14f47a6a0b1d0682b267d26f
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-graph-api"></a>Konfigurowanie bazy danych Azure rozwiązania Cosmos dystrybucji globalnego przy użyciu interfejsu API programu Graph

W tym artykule zostanie przedstawiony sposób korzystać z portalu Azure do instalacji bazy danych Azure rozwiązania Cosmos globalne dystrybucji, a następnie nawiąż połączenie przy użyciu interfejsu API programu Graph (wersja zapoznawcza).

W tym artykule opisano następujące zadania: 

> [!div class="checklist"]
> * Skonfiguruj globalne dystrybucji przy użyciu portalu Azure
> * Skonfigurować globalne dystrybucji za pomocą [API Graph](graph-introduction.md) (wersja zapoznawcza)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-graph-api-using-the-net-sdk"></a>Łączenie z preferowanego regionu przy użyciu interfejsu API programu Graph przy użyciu zestawu .NET SDK

Interfejsu API programu Graph jest ujawniona jako biblioteki rozszerzenia u góry zestawu SDK usługi DocumentDB.

Aby korzystać z [globalne dystrybucji](distribute-data-globally.md), aplikacje klienckie można określić listy uporządkowanej preferencji regionów ma być używany do wykonywania operacji dokumentu. Można to zrobić przez ustawienie zasad połączenia. Na podstawie konfiguracji konta bazy danych Azure rozwiązania Cosmos, bieżącej dostępności regionalnych i na liście preferencji określone, optymalny punkt końcowy zostanie wybrany przez zestaw SDK, aby wykonać zapisu i operacji odczytu.

Ta lista preferencji został określony podczas inicjowania połączenia przy użyciu zestawów SDK. Zestawy SDK zaakceptować opcjonalny parametr "PreferredLocations" oznacza to uporządkowana lista regionów platformy Azure.

* **Zapisuje**: zestaw SDK będzie automatycznie wysyłać zapisuje wszystkie dane w bieżącej zapisu regionu.
* **Odczytuje**: wszystkie operacje odczytu, które zostaną wysłane do pierwszy dostępny region na liście PreferredLocations. Jeśli żądanie kończy się niepowodzeniem, klient się nie powieść w dół na liście, aby następny region i tak dalej. Zestawy SDK podejmie próbę odczytu z określonych w PreferredLocations regionów. Tak na przykład, jeśli konto bazy danych rozwiązania Cosmos jest dostępne w trzech regionów, ale klient tylko określa dwóch regionach i do zapisu dla PreferredLocations, następnie odczyty nie zostanie obsłużona poza region zapisu, nawet w przypadku pracy awaryjnej.

Aplikacja może sprawdź bieżący punkt końcowy zapisu i odczytu punktu końcowego wybrany przez zestaw SDK, sprawdzając dwie właściwości WriteEndpoint i ReadEndpoint dostępne w wersji zestawu SDK 1.8 i powyżej. Jeśli nie ustawiono właściwości PreferredLocations, zostanie obsłużona wszystkie żądania z bieżącego obszaru zapisu.

### <a name="using-the-sdk"></a>Przy użyciu zestawu SDK

Na przykład w przypadku zestawu .NET SDK `ConnectionPolicy` parametr `DocumentClient` Konstruktor ma właściwość o nazwie `PreferredLocations`. Tej właściwości można ustawić do listy nazwy regionów. Wyświetlanie nazwy dla [regiony platformy Azure] [ regions] może być określona jako część `PreferredLocations`.

> [!NOTE]
> Nie należy traktować jako długotrwałe stałe adresy URL punktów końcowych. Usługa może aktualizować je w dowolnym momencie. Zestaw SDK obsługuje automatycznie tej zmiany.
>
>

```cs
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect to Azure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
```

To wszystko, która kończy w tym samouczku. Znajdują się informacje dotyczące zarządzania spójności konta globalnie replikowanych odczytując [poziomów spójności w usłudze Azure DB rozwiązania Cosmos](consistency-levels.md). I uzyskać więcej informacji na temat sposobu globalnej replikacji bazy danych działa w usłudze Azure DB rozwiązania Cosmos, zobacz [dystrybucji danych globalnie z bazy danych Azure rozwiązania Cosmos](distribute-data-globally.md).

## <a name="next-steps"></a>Następne kroki

W tym samouczku wykonaniu następujących czynności:

> [!div class="checklist"]
> * Skonfiguruj globalne dystrybucji przy użyciu portalu Azure
> * Skonfiguruj globalne dystrybucji przy użyciu interfejsów API usługi DocumentDB

Możesz teraz przejść do następnym samouczku, aby dowiedzieć się, jak opracowywać lokalnie przy użyciu emulatora lokalnej bazy danych Azure rozwiązania Cosmos.

> [!div class="nextstepaction"]
> [Opracowywanie lokalnie w emulatorze](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

