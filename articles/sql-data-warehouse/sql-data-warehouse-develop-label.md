---
title: "Korzystanie z etykiet do dokumentu zapytania w usłudze SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Porady dotyczące korzystania z etykiet do dokumentu zapytania w usłudze Azure SQL Data Warehouse związane z opracowywaniem rozwiązań."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 44988de8-04c1-4fed-92be-e1935661a4e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 9e75bbe528a427724a623305fbd45e2277e9d0af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Korzystanie z etykiet do dokumentu zapytania w usłudze SQL Data Warehouse
Magazyn danych SQL obsługuje pojęcie o nazwie etykiety zapytania. Przed przejściem do dowolnej głębokości teraz wyglądać na przykład jeden:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Ten ostatni wiersz znaczniki ciąg "Moje zapytania etykieta" w zapytaniu. Jest to szczególnie przydatne, ponieważ etykieta to zapytanie może przy użyciu widoków DMV. To zapewnia mechanizm do śledzenia zapytań problem, a także ułatwiają identyfikację postęp do uruchomienia ETL.

Dobrym konwencji nazewnictwa korzystnie tutaj. Na przykład coś, takich jak "projektu: procedura: instrukcja: komentarz" byłaby pomocna do jednoznacznej identyfikacji zapytania w między cały kod w kontroli źródła.

Aby przeprowadzić wyszukiwanie według etykiety można Użyj następującego zapytania, który używa dynamicznych widoków zarządzania:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Istotne jest zawijany nawiasy kwadratowe lub cudzysłowów wokół etykiety word podczas wykonywania zapytania. Etykieta jest słowem zastrzeżonym i będzie spowodowała błąd, jeśli nie został ograniczony.
> 
> 

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad programistycznych, zobacz [omówienie tworzenia][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
