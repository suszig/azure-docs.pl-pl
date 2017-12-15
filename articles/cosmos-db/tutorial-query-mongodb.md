---
title: "Azure DB rozwiązania Cosmos: Jak wykonać zapytanie, za pomocą interfejsu API bazy danych MongoDB? | Microsoft Docs"
description: "Dowiedz się zbadać za pomocą interfejsu API bazy danych MongoDB dla bazy danych Azure rozwiązania Cosmos"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 1818476a95ddf373701ad93860b02ea4c2ad761d
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmos-db-how-to-query-with-api-for-mongodb"></a>Azure DB rozwiązania Cosmos: Jak zbadać za pomocą interfejsu API dla bazy danych MongoDB?

Azure DB rozwiązania Cosmos [interfejsu API dla bazy danych MongoDB](mongodb-introduction.md) obsługuje [zapytania powłoki MongoDB](https://docs.mongodb.com/manual/tutorial/query-documents/). 

W tym artykule opisano następujące zadania: 

> [!div class="checklist"]
> * Wykonywanie zapytania na danych z bazy danych MongoDB

## <a name="sample-document"></a>Przykładowy dokument

Zapytania w tym artykule, użyj następujących przykładowy dokument.

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
## <a id="examplequery1"></a>Przykładowe Zapytanie 1 

Podana dokument rodziny próbki powyżej, następujące zapytanie zwraca dokumenty Jeśli w polu identyfikatora odpowiada `WakefieldFamily`.

**Zapytanie**
    
    db.families.find({ id: “WakefieldFamily”})

**Wyniki**

    {
    "_id": "ObjectId(\"58f65e1198f3a12c7090e68c\")",
    "id": "WakefieldFamily",
    "parents": [
      {
        "familyName": "Wakefield",
        "givenName": "Robin"
      },
      {
        "familyName": "Miller",
        "givenName": "Ben"
      }
    ],
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ],
    "address": {
      "state": "NY",
      "county": "Manhattan",
      "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

## <a id="examplequery2"></a>Przykładowe zapytanie 2 

Dalej zapytanie zwraca wszystkie elementy podrzędne w rodzinie. 

**Zapytanie**
    
    db.familes.find( { id: “WakefieldFamily” }, { children: true } )

**Wyniki**

    {
    "_id": "ObjectId("58f65e1198f3a12c7090e68c")",
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ]
    }


## <a id="examplequery3"></a>Przykładowe zapytanie 3 

Dalej zapytanie zwraca wszystkie rodziny, które zostały zarejestrowane. 

**Zapytanie**
    
    db.families.find( { "isRegistered" : true })
**Wyniki** zostanie zwrócony żaden dokument. 

## <a id="examplequery4"></a>Przykładowe zapytanie 4

Dalej zapytanie zwraca wszystkie rodziny, które nie zostały zarejestrowane. 

**Zapytanie**
    
    db.families.find( { "isRegistered" : false })
**Wyniki**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery5"></a>Przykładowe zapytanie 5

Dalej zapytanie zwraca rodziny, które nie są zarejestrowane i stan jest NY. 

**Zapytanie**
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

**Wyniki**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}


## <a id="examplequery6"></a>Przykładowe zapytanie 6

Dalej zapytanie zwraca wszystkich rodzin, w których 8 klas podrzędnych.

**Zapytanie**
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

**Wyniki**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery7"></a>Przykładowe zapytanie 7

Dalej zapytanie zwraca wszystkich rodzin, których rozmiar tablicy elementów podrzędnych to 3.

**Zapytanie**
  
      db.Family.find( {children: { $size:3} } )

**Wyniki**

Nie będzie można zwrócić wyników, ponieważ nie ma więcej niż 2 elementów podrzędnych. Tylko wtedy, gdy parametr 2 to zapytanie powiodło się i zwróć pełnego dokumentu.

## <a name="next-steps"></a>Następne kroki

W tym samouczku wykonaniu następujących czynności:

> [!div class="checklist"]
> * Przedstawiono sposób zapytań przy użyciu bazy danych MongoDB 

Możesz teraz przejść do następnym samouczku informacje na temat dystrybucji danych globalnie.

> [!div class="nextstepaction"]
> [Globalny dystrybucji danych](tutorial-global-distribution-sql-api.md)

