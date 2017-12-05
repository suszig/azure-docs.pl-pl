---
title: "Samouczek usługi Azure globalne dystrybucji rozwiązania Cosmos DB dla bazy danych MongoDB interfejsu API | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować bazy danych Azure rozwiązania Cosmos dystrybucji globalnego przy użyciu interfejsu API bazy danych MongoDB."
services: cosmos-db
keywords: globalne dystrybucji, bazy danych MongoDB
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: a934643be02abeadecf9e8384e29bd3e0e1a36f5
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-mongodb-api"></a>Konfigurowanie bazy danych Azure rozwiązania Cosmos dystrybucji globalnego przy użyciu interfejsu API bazy danych MongoDB

W tym artykule zostanie przedstawiony sposób użycia portalu Azure do instalacji bazy danych Azure rozwiązania Cosmos globalne dystrybucji, a następnie nawiąż połączenie przy użyciu interfejsu API bazy danych MongoDB.

W tym artykule opisano następujące zadania: 

> [!div class="checklist"]
> * Skonfiguruj globalne dystrybucji przy użyciu portalu Azure
> * Skonfigurować globalne dystrybucji za pomocą [API bazy danych MongoDB](mongodb-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-the-mongodb-api"></a>Weryfikowanie ustawień regionalnych przy użyciu interfejsu API bazy danych MongoDB
Najprostszym sposobem sprawdzanie globalnej konfiguracji w obrębie interfejsu API dla bazy danych MongoDB ma być uruchamiany o podwójnej precyzji *isMaster()* polecenie z poziomu powłoki Mongo.

Z powłoki Mongo:

   ```
      db.isMaster()
   ```
   
Przykładowe wyniki:

   ```JSON
      {
         "_t": "IsMasterResponse",
         "ok": 1,
         "ismaster": true,
         "maxMessageSizeBytes": 4194304,
         "maxWriteBatchSize": 1000,
         "minWireVersion": 0,
         "maxWireVersion": 2,
         "tags": {
            "region": "South India"
         },
         "hosts": [
            "vishi-api-for-mongodb-southcentralus.documents.azure.com:10255",
            "vishi-api-for-mongodb-westeurope.documents.azure.com:10255",
            "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
         ],
         "setName": "globaldb",
         "setVersion": 1,
         "primary": "vishi-api-for-mongodb-southindia.documents.azure.com:10255",
         "me": "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
      }
   ```

## <a name="connecting-to-a-preferred-region-using-the-mongodb-api"></a>Łączenie z preferowanego regionu przy użyciu interfejsu API bazy danych MongoDB

Interfejs API bazy danych MongoDB umożliwia określenie preferencji odczytu kolekcji globalnie rozproszone bazy danych. Dla obu małe opóźnienia odczytów, jak i globalnego wysokiej dostępności, firma Microsoft zaleca ustawienie preferencji odczytu kolekcji *najbliższej*. Przeczytaj preferencję *najbliższej* jest skonfigurowany do odczytu z najbliżej regionu.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

Dla aplikacji za pomocą podstawowego odczytu/zapisu regionu i regionu pomocniczego do odzyskiwania awaryjnego (DR) scenariuszy, firma Microsoft zaleca ustawienie preferencji odczytu kolekcji *pomocniczy preferowane*. Odczytu preferencję *pomocniczy preferowane* jest skonfigurowany do odczytu z regionu pomocniczego, gdy region podstawowy jest niedostępny.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

Ponadto jeśli chcesz ręcznie określić Twojej odczytu regionów. Można ustawić regionu tagu w swoich preferencji odczytu.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
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
