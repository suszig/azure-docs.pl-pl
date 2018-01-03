---
title: "Jak wykonać zapytanie dotyczące danych wykresu w usłudze Azure DB rozwiązania Cosmos? | Microsoft Docs"
description: "Dowiedz się, jak dane wykresu zapytania w usłudze Azure DB rozwiązania Cosmos"
services: cosmos-db
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: 
tags: 
ms.assetid: 8bde5c80-581c-4f70-acb4-9578873c92fa
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 01/02/2018
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: 5a635abfa9fa10cd8c8498e3c95a17af997cea3e
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="azure-cosmos-db-how-to-query-with-the-graph-api"></a>Azure DB rozwiązania Cosmos: Jak wykonać zapytanie z interfejsu API programu Graph?

Azure DB rozwiązania Cosmos [interfejsu API programu Graph](graph-introduction.md) obsługuje [Gremlin](https://github.com/tinkerpop/gremlin/wiki) zapytania. W tym artykule przedstawiono przykładowe dokumentach i zapytaniach ułatwiających rozpoczęcie pracy. A szczegółowe Gremlin odwołanie znajduje się w [Obsługa Gremlin](gremlin-support.md) artykułu.

W tym artykule opisano następujące zadania: 

> [!div class="checklist"]
> * Wykonywanie zapytania na danych z Gremlin

## <a name="prerequisites"></a>Wymagania wstępne

Dla tych zapytań do pracy musi mieć konto bazy danych Azure rozwiązania Cosmos i danych wykresu w kontenerze. Nie masz żadnego z tych? Zakończenie [szybkiego startu 5-minutowy](create-graph-dotnet.md) lub [samouczek developer](tutorial-query-graph.md) Tworzenie konta usługi i umieścić w bazie danych. Można uruchomić następujące kwerendy przy użyciu [Azure rozwiązania Cosmos DB .NET wykresu biblioteki](graph-sdk-dotnet.md), [konsoli Gremlin](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console), lub sterownika Gremlin ulubionych.

## <a name="count-vertices-in-the-graph"></a>Liczba wierzchołki na wykresie

Poniższy fragment kodu przedstawia sposób liczbę wierzchołki na wykresie:

```
g.V().count()
```

## <a name="filters"></a>Filtry

Można wykonywać przy użyciu jego Gremlin filtry `has` i `hasLabel` kroki i połączenie ich za pomocą `and`, `or`, i `not` do tworzenia bardziej złożonych filtrów. Azure DB rozwiązania Cosmos zapewnia niezależny od schematu indeksowania wszystkich właściwości w danej wierzchołki i stopni szybkiego zapytań:

```
g.V().hasLabel('person').has('age', gt(40))
```

## <a name="projection"></a>Projekcji

Niektóre właściwości wyników zapytania za pomocą można projektu `values` krok:

```
g.V().hasLabel('person').values('firstName')
```

## <a name="find-related-edges-and-vertices"></a>Znajdź krawędzi pokrewne i wierzchołków

Firma Microsoft do tej pory tylko przedstawiono operatorów zapytań, które działają w dowolnej bazy danych. Wykresy są szybkie i wydajne dla operacji przechodzenia, gdy musisz przejść do krawędzi pokrewne i wierzchołków. Spróbujmy wszystkich znajomych blogu Thomasa. Firma Microsoft to zrobić przy użyciu jego Gremlin `outE` krok, aby znaleźć wszystkie wyjściowego krawędzi blogu Thomasa, a następnie przechodzenie do wierzchołków w z tych krawędzi przy użyciu jego Gremlin `inV` krok:

```cs
g.V('thomas').outE('knows').inV().hasLabel('person')
```

Zapytanie dalej wykonuje dwie przeskoków w celu znalezienia wszystkich blogu Thomasa "znajomych przyjaciół", wywołując `outE` i `inV` dwa razy. 

```cs
g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')
```

Można tworzyć bardziej złożone kwerendy i wdrożyć logikę przechodzenie zaawansowanych wykresu przy użyciu Gremlin, tym wyrażeniach filtru mieszanie wykonywania pętli przy użyciu `loop` krok i wykonawcze przy użyciu warunkowego nawigacji `choose` kroku. Dowiedz się więcej o co można zrobić z [Obsługa Gremlin](gremlin-support.md)!

## <a name="next-steps"></a>Kolejne kroki

W tym samouczku wykonaniu następujących czynności:

> [!div class="checklist"]
> * Przedstawiono sposób zapytań przy użyciu wykresu 

Możesz teraz przejść do następnym samouczku informacje na temat dystrybucji danych globalnie.

> [!div class="nextstepaction"]
> [Globalny dystrybucji danych](tutorial-global-distribution-sql-api.md)
