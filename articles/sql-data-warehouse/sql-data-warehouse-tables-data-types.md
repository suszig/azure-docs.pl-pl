---
title: "Typy danych wskazówki - Azure SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Zalecenia dotyczące Definiowanie typów danych, które są zgodne z usługą Magazyn danych SQL."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: d4a1f0a3-ba9f-44b9-95f6-16a4f30746d6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 06/02/2017
ms.author: shigu;barbkess
ms.openlocfilehash: 5c24c71af16bd9851d9caf15fecfa4bb76f5f77e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="guidance-for-defining-data-types-for-tables-in-sql-data-warehouse"></a>Wskazówki dotyczące definiowania typów danych w przypadku tabel w usłudze SQL Data Warehouse
Zalecenia te umożliwiają definiowanie typów danych tabeli, które są zgodne z usługą Magazyn danych SQL. Oprócz zgodności minimalizując rozmiar typów danych o poprawia wydajność kwerend.

Magazyn danych SQL obsługuje najczęściej używane typy danych. Aby uzyskać listę obsługiwanych typów danych, zobacz [typy danych](/sql/docs/t-sql/statements/create-table-azure-sql-data-warehouse.md#datatypes) w instrukcji CREATE TABLE. 


## <a name="minimize-row-length"></a>Minimalizowanie długość wiersza
Minimalizowanie rozmiar typów danych o skraca długość wiersza, co prowadzi do poprawy wydajności zapytania. Użyj najmniejszą typu danych, który działa danych. 

- Unikaj definiowania kolumn znakowych o długości dużych domyślne. Na przykład jeśli wartość najdłuższym 25 znaków, następnie zdefiniuj kolumny jako VARCHAR(25). 
- Unikaj używania [NVARCHAR] [ NVARCHAR] potrzebne tylko VARCHAR.
- Jeśli to możliwe, zamiast VARCHAR(MAX) lub NVARCHAR(MAX), albo użyć NVARCHAR(4000) lub VARCHAR(8000).

Jeśli używasz programu Polybase można załadować tabel, określona długość wiersza tabeli nie może przekraczać 1 MB. Jeśli wiersz o zmiennej długości danych przekracza 1 MB, można załadować wiersza za pomocą narzędzia BCP, ale nie przy użyciu programu PolyBase.

## <a name="identify-unsupported-data-types"></a>Zidentyfikuj nieobsługiwane typy danych
W przypadku migracji z innej bazy danych SQL bazy danych, może wystąpić typy danych, które nie są obsługiwane w usłudze SQL Data Warehouse. Skorzystaj z tej kwerendy, aby odnaleźć nieobsługiwane typy danych w istniejącej schematu SQL.

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```


## <a name="unsupported-data-types"></a>Użyj obejścia nieobsługiwane typy danych

Na poniższej liście przedstawiono typy danych nie obsługuje usługi SQL Data Warehouse, a udostępnia opis rozwiązań alternatywnych, których można użyć zamiast nieobsługiwane typy danych.

| Nieobsługiwany typ danych | Obejście problemu |
| --- | --- |
| [Geometria][geometry] |[varbinary][varbinary] |
| [Lokalizacja geograficzna][geography] |[varbinary][varbinary] |
| [Identyfikator hierarchii][hierarchyid] |[nvarchar][nvarchar](4000) |
| [Obraz][ntext,text,image] |[varbinary][varbinary] |
| [tekst][ntext,text,image] |[varchar][varchar] |
| [ntext][ntext,text,image] |[nvarchar][nvarchar] |
| [sql_variant][sql_variant] |Podziel kolumnę na kilka jednoznacznie kolumn. |
| [Tabela][table] |Konwertuj do tabel tymczasowych. |
| [Znacznik czasu][timestamp] |Zmian kodu w celu użycia [datetime2] [ datetime2] i `CURRENT_TIMESTAMP` funkcji.  Obsługiwane są tylko stałe jako domyślne, w związku z tym current_timestamp nie może być zdefiniowana jako ograniczenie domyślne. Jeśli trzeba migrować wartości wersji wierszy z typu kolumny znaczników czasu, użyj [BINARNE][BINARY](8) lub [VARBINARY][BINARY](8) dla nie wartość NULL lub Wiersz wersji wartości NULL. |
| [XML][xml] |[varchar][varchar] |
| [Typ zdefiniowany przez użytkownika][user defined types] |Przekonwertować typu danych natywnych, gdy jest to możliwe. |
| wartości domyślne | Wartości domyślne obsługuje literały i tylko stałe.  Inne niż deterministyczne wyrażenia lub funkcje, takie jak `GETDATE()` lub `CURRENT_TIMESTAMP`, nie są obsługiwane. |


## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej, zobacz:

- [Najlepsze rozwiązania magazynu danych SQL][SQL Data Warehouse Best Practices]
- [Przegląd tabeli][Overview]
- [Dystrybucja tabeli][Distribute]
- [Indeksowanie tabeli][Index]
- [Podział na partycje tabeli][Partition]
- [Obsługa statystyk tabeli][Statistics]
- [Tabele tymczasowe][Temporary]

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[create table]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[binary]: https://msdn.microsoft.com/library/ms188362.aspx
[bit]: https://msdn.microsoft.com/library/ms177603.aspx
[char]: https://msdn.microsoft.com/library/ms176089.aspx
[date]: https://msdn.microsoft.com/library/bb630352.aspx
[datetime]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[datetimeoffset]: https://msdn.microsoft.com/library/bb630289.aspx
[decimal]: https://msdn.microsoft.com/library/ms187746.aspx
[float]: https://msdn.microsoft.com/library/ms173773.aspx
[geometry]: https://msdn.microsoft.com/library/cc280487.aspx
[geography]: https://msdn.microsoft.com/library/cc280766.aspx
[hierarchyid]: https://msdn.microsoft.com/library/bb677290.aspx
[int]: https://msdn.microsoft.com/library/ms187745.aspx
[money]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[real]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[table]: https://msdn.microsoft.com/library/ms175010.aspx
[time]: https://msdn.microsoft.com/library/bb677243.aspx
[timestamp]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[uniqueidentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[xml]: https://msdn.microsoft.com/library/ms187339.aspx
[user defined types]: https://msdn.microsoft.com/library/ms131694.aspx
