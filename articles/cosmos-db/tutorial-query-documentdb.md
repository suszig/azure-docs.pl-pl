---
title: "Jak zapytania SQL w usłudze Azure DB rozwiązania Cosmos? | Microsoft Docs"
description: "Dowiedz się zbadać za pomocą programu SQL w usłudze Azure DB rozwiązania Cosmos"
services: cosmos-db
documentationcenter: 
author: rafats
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: tutorial-develop, mvc
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: rafats
ms.openlocfilehash: ae2d7c4ecdeeb88714cdaa0558950ea52fab99d3
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
---
# <a name="azure-cosmos-db-how-to-query-using-sql"></a>Azure DB rozwiązania Cosmos: Jak wykonać zapytanie, przy użyciu programu SQL?

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Azure DB rozwiązania Cosmos [interfejsu API SQL](documentdb-introduction.md) obsługiwanych badania dokumentów za pomocą programu SQL. Ten artykuł zawiera przykładowy dokument i dwa przykładowe zapytania SQL oraz wyniki.

W tym artykule opisano następujące zadania: 

> [!div class="checklist"]
> * Wykonywanie zapytania na danych z programu SQL

## <a name="sample-document"></a>Przykładowy dokument

Zapytania SQL, w tym artykule Użyj następujących przykładowy dokument.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
## <a name="where-can-i-run-sql-queries"></a>Gdzie można uruchomić zapytania SQL

Zapytań za pomocą Eksploratora danych w portalu Azure za pośrednictwem [interfejsu API REST i zestawy SDK](documentdb-sdk-dotnet.md), a nawet [Plac zabaw dla zapytań](https://www.documentdb.com/sql/demo), która działa na podstawie istniejącego zestawu danych przykładowych zapytań.

Aby uzyskać więcej informacji na temat kwerend SQL zobacz:
* [Zapytanie SQL i składni SQL](documentdb-sql-query.md)

## <a name="prerequisites"></a>Wymagania wstępne

W tym samouczku założono, że masz konto bazy danych Azure rozwiązania Cosmos i kolekcji. Nie masz żadnego z tych? Zakończenie [szybkiego startu 5-minutowy](create-mongodb-nodejs.md) lub [samouczek developer](tutorial-develop-mongodb.md) Aby utworzyć konto i kolekcji.

## <a name="example-query-1"></a>Przykładowe Zapytanie 1

Podana dokument rodziny próbki powyżej, następujące zapytanie SQL zwraca dokumenty Jeśli w polu identyfikatora odpowiada `WakefieldFamily`. Ponieważ chodzi o `SELECT *` instrukcji, wyniki kwerendy jest cały dokument JSON:

**Zapytanie**

    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"

**Wyniki**

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

## <a name="example-query-2"></a>Przykładowe zapytanie 2

Dalej zapytania zwraca wszystkie nazwy podanej dzieci w rodzinie odpowiada o identyfikatorze `WakefieldFamily` uporządkowanych według ich klasy.

**Zapytanie**

    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.children.grade ASC

**Wyniki**

    [
      { "givenName": "Jesse" }, 
      { "givenName": "Lisa"}
    ]


## <a name="next-steps"></a>Następne kroki

W tym samouczku wykonaniu następujących czynności:

> [!div class="checklist"]
> * Przedstawiono sposób zapytań przy użyciu programu SQL  

Możesz teraz przejść do następnym samouczku informacje na temat dystrybucji danych globalnie.

> [!div class="nextstepaction"]
> [Globalny dystrybucji danych](tutorial-global-distribution-documentdb.md)

