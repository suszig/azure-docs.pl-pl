---
title: "Usługa Azure DocumentDB .NET zmienić zasoby & źródła procesora zestawu SDK | Dokumentacja firmy Microsoft"
description: "Dowiedz się wszystkiego o interfejsie API procesora źródła danych zmian i zestawu SDK, w tym daty wydania, daty wycofania i zmiany wprowadzone od każda wersja programu .NET zmiany źródła procesora zestawu SDK usługi DocumentDB."
services: cosmos-db
documentationcenter: .net
author: ealsur
manager: kirillg
editor: mimig1
ms.assetid: f2dd9438-8879-4f74-bb6c-e1efc2cd0157
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: maquaran
ms.openlocfilehash: 8c5a081105d37b8f1c78e90d9ccb8d945099b621
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="documentdb-net-change-feed-processor-sdk-download-and-release-notes"></a>Usługa DocumentDB .NET zmiany źródła strumieniowego procesora zestawu SDK: Pobierz i informacje o wersji
> [!div class="op_single_selector"]
> * [.NET](documentdb-sdk-dotnet.md)
> * [Źródła danych zmian .NET](documentdb-sdk-dotnet-changefeed.md)
> * [.NET Core](documentdb-sdk-dotnet-core.md)
> * [Node.js](documentdb-sdk-node.md)
> * [Java](documentdb-sdk-java.md)
> * [Python](documentdb-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/documentdb/)
> * [Dostawca zasobów REST](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

|   |   |
|---|---|
|**Pobierz zestaw SDK**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**Dokumentacja interfejsu API**|[Zmień dokumentacji interfejsu API biblioteki procesora źródła danych](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Wprowadzenie**|[Wprowadzenie do usługi DocumentDB zmiany źródła procesora zestawu .NET SDK](change-feed.md)|
|**Bieżąca platforma obsługiwane**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Program Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

## <a name="release-notes"></a>Informacje o wersji

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Dodaje obsługę platformy .NET Standard 2.0. Pakiet obsługuje teraz `netstandard2.0` i `net451` monikerów framework.
* Zgodny z [zestawu SDK .NET usługi DocumentDB](documentdb-sdk-dotnet.md) wersji 1.17.0 lub nowszym.
* Zgodny z [DocumentDB .NET Core SDK](documentdb-sdk-dotnet-core.md) wersji 1.5.1 lub nowszym.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Rozwiązuje problem z wyliczeniem oszacowania pracy pozostałej podczas zmiany źródła danych jest pusta lub oczekiwała żadne czynności.
* Zgodny z [zestawu SDK .NET usługi DocumentDB](documentdb-sdk-dotnet.md) wersji 1.13.2 lub nowszym.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Dodaje metody uzyskać szacunkową Praca pozostała na przetworzenie w źródła danych zmian.
* Zgodny z [zestawu SDK .NET usługi DocumentDB](documentdb-sdk-dotnet.md) wersji 1.13.2 lub nowszym.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK
* Zgodny z [zestawu SDK .NET usługi DocumentDB](documentdb-sdk-dotnet.md) wersji 1.14.1 i poniżej.

## <a name="release--retirement-dates"></a>Wersja & wycofania dat
Firma Microsoft udostępni powiadomienia co najmniej **12 miesięcy** klienta z wyprzedzeniem wycofanie SDK w celu złagodzenia przejścia do nowszej/nieobsługiwaną wersję.

Nowe funkcje i funkcjonalność i optymalizację, które są dodawane tylko do bieżącego zestawu SDK, w związku jest zalecane, zawsze Uaktualnij zestaw SDK najnowszą tak szybko jak to możliwe. 

Każde żądanie do rozwiązania Cosmos bazy danych przy użyciu wycofane zestawu SDK będą odrzucane przez usługę.

<br/>

| Wersja | Data wydania | Dacie wycofania |
| --- | --- | --- |
| [1.2.0](#1.2.0) |31 października 2017 r. |--- |
| [1.1.1](#1.1.1) |29 sierpnia 2017 r. |--- |
| [1.1.0](#1.1.0) |13 sierpnia 2017 r. |--- |
| [1.0.0](#1.0.0) |07 lipca 2017 r. |--- |


## <a name="faq"></a>Często zadawane pytania
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zobacz też
Aby dowiedzieć się więcej na temat rozwiązania Cosmos bazy danych, zobacz [bazy danych programu Microsoft Azure rozwiązania Cosmos](https://azure.microsoft.com/services/cosmos-db/) stronę usługi. 

