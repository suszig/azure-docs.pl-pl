---
title: "Korzystanie z widoków T-SQL w usłudze Azure SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Porady dotyczące korzystania z widoków języka Transact-SQL w usłudze Azure SQL Data Warehouse związane z opracowywaniem rozwiązań."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: b5208f32-8f4a-4056-8788-2adbb253d9fd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: d2a03be810bd7f792876607ec735eb578b65a3b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="views-in-sql-data-warehouse"></a>Widoki w usłudze SQL Data Warehouse
Widoki są szczególnie przydatne w usłudze SQL Data Warehouse. Służy na kilka różnych sposobów poprawy jakości rozwiązania.  W tym artykule omówiono sposób wzbogacić rozwiązania z widoków, a także ograniczenia, które należy wziąć pod uwagę kilka przykładów.

> [!NOTE]
> Składnia `CREATE VIEW` nie została szczegółowo opisana w tym artykule. Zapoznaj się z [CREATE VIEW] [ CREATE VIEW] artykuł w witrynie MSDN, aby uzyskać informacje na ten.
> 
> 

## <a name="architectural-abstraction"></a>Abstrakcja architektury
Jest bardzo typowy wzorzec aplikacji do ponownego tworzenia tabel za pomocą tworzenia tabeli jako wybierz (CTAS) następuje zmiana nazwy wzorzec podczas ładowania danych obiektu.

W poniższym przykładzie dodaje Data nowych rekordów do wymiaru daty. Należy zauważyć, jak nowy tabela, DimDate_New, jest najpierw utworzyć i następnie zmieniona na zastępowanie wersji oryginalnej tabeli.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Jednak ta metoda może spowodować tabel znajdujących się i znika z punktu widzenia użytkownika, a także "Tabela nie istnieje" komunikaty o błędach. Widoków może służyć do zapewnienia użytkowników z warstwą prezentacji spójne, przy jednoczesnym obiektów zostały zmienione. Zapewniając użytkownikom dostęp do danych za pośrednictwem widoków, oznacza, że użytkownicy nie muszą mieć wgląd w tabeli. Zapewnia to spójny interfejs użytkownika podczas sprawdzeniu, czy projektanci magazyn danych można rozwijać modelu danych i zmaksymalizować wydajność przy użyciu CTAS podczas procesu ładowania danych.    

## <a name="performance-optimization"></a>Optymalizacja wydajności
Widoki można również używać do wymuszania zoptymalizowanych pod kątem wydajności sprzężenia między tabelami. Na przykład widoku można zastosować klucza dystrybucji nadmiarowe jako część łącząca kryteriów, aby zminimalizować przenoszenia danych.  Inną zaletą widoku można wymusić określonej kwerendy lub łącząca wskazówki. Korzystanie z widoków w ten sposób gwarantuje, że sprzężenia zawsze są realizowane w sposób optymalny uniknięcie do zapamiętania prawidłowa konstrukcja dla ich sprzężeń przez użytkowników.

## <a name="limitations"></a>Ograniczenia
Widoki w usłudze SQL Data Warehouse są tylko metadane.  W związku z tym nie dostępne są następujące opcje:

* Nie jest dostępna opcja powiązanie schematu
* Nie można aktualizować tabel podstawowych za pośrednictwem widoku
* Nie można tworzyć widoki w tabelach tymczasowych
* Nie jest obsługiwane dla ROZWIJANIA / wskazówki NOEXPAND
* Brak widoków indeksowanych w usłudze SQL Data Warehouse

## <a name="next-steps"></a>Następne kroki
Więcej porad dla deweloperów znajduje się w artykule [Omówienie programowania w usłudze SQL Data Warehouse][SQL Data Warehouse development overview].
Aby uzyskać `CREATE VIEW` składni, zapoznaj się [CREATE VIEW][CREATE VIEW].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
