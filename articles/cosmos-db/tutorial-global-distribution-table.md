---
title: "Samouczek usługi Azure globalne dystrybucji rozwiązania Cosmos bazy danych dla tabeli interfejsu API | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować bazy danych Azure rozwiązania Cosmos dystrybucji globalnego przy użyciu interfejsu API tabeli."
services: cosmos-db
keywords: globalne dystrybucji, tabeli
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
ms.date: 11/15/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: a1682ef88760da39d33fac2cc9f34e0e66ea19fb
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a>Konfigurowanie bazy danych Azure rozwiązania Cosmos dystrybucji globalnego przy użyciu interfejsu API tabeli

W tym artykule opisano następujące zadania: 

> [!div class="checklist"]
> * Skonfiguruj globalne dystrybucji przy użyciu portalu Azure
> * Skonfigurować globalne dystrybucji za pomocą [tabeli interfejsu API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Łączenie z preferowanego regionu przy użyciu interfejsu API tabeli

Aby korzystać z [globalne dystrybucji](distribute-data-globally.md), aplikacje klienckie można określić listy uporządkowanej preferencji regionów ma być używany do wykonywania operacji dokumentu. Można to zrobić przez ustawienie `TablePreferredLocations` wartości konfiguracji w pliku app.config dla zestawu SDK usługi Azure rozwiązania Cosmos DB tabeli interfejsu API. Rozwiązania Cosmos DB tabeli interfejsu API zestawu SDK usługi Azure będzie Wybierz najlepsze punktu końcowego do komunikowania się z na podstawie konfiguracji konta, bieżącej dostępności regionalnych i listy podany preferencji.

`TablePreferredLocations` Powinien zawierać rozdzielaną przecinkami listę preferowanych lokalizacji (podłączonej do wielu sieci) dla operacji odczytu. Każde wystąpienie klienta można określić podzbiór tych regionów, w preferowanej kolejności dla odczytów małe opóźnienia. Regionów muszą nosić przy użyciu ich [wyświetlane nazwy](https://msdn.microsoft.com/library/azure/gg441293.aspx), na przykład `West US`.

Wszystkie operacje odczytu są wysyłane do pierwszy dostępny region w `TablePreferredLocations` listy. Jeśli żądanie kończy się niepowodzeniem, klient się nie powieść w dół na liście, aby następny region i tak dalej.

Zestaw SDK podejmuje próbę odczytu z określonych w regionów `TablePreferredLocations`. Tak, na przykład, jeśli konto bazy danych jest dostępne w trzech regionów, ale klient określa tylko dwóch regionach i do zapisu dla `TablePreferredLocations`, a następnie odczyty nie zostanie obsłużona poza region zapisu, nawet w przypadku pracy awaryjnej.

Zestaw SDK będzie automatycznie wysyłać zapisuje wszystkie dane w bieżącej zapisu regionu.

Jeśli `TablePreferredLocations` właściwość nie jest ustawiona, zostanie obsłużona wszystkie żądania z bieżącego obszaru zapisu.

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

To wszystko, która kończy w tym samouczku. Znajdują się informacje dotyczące zarządzania spójności konta globalnie replikowanych odczytując [poziomów spójności w usłudze Azure DB rozwiązania Cosmos](consistency-levels.md). I uzyskać więcej informacji na temat sposobu globalnej replikacji bazy danych działa w usłudze Azure DB rozwiązania Cosmos, zobacz [dystrybucji danych globalnie z bazy danych Azure rozwiązania Cosmos](distribute-data-globally.md).

## <a name="next-steps"></a>Następne kroki

W tym samouczku wykonaniu następujących czynności:

> [!div class="checklist"]
> * Skonfiguruj globalne dystrybucji przy użyciu portalu Azure
> * Skonfiguruj globalne dystrybucji przy użyciu interfejsów API do tabeli bazy danych usługi Azure rozwiązania Cosmos

