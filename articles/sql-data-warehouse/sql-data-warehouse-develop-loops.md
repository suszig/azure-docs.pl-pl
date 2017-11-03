---
title: "Wykorzystanie pętle T-SQL w usłudze Azure SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Porady dotyczące języka Transact-SQL pętli i zastąpienie kursory w usłudze Azure SQL Data Warehouse związane z opracowywaniem rozwiązań."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 40a872ff310f48bfd543ac184fe7301b85b50258
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="loops-in-sql-data-warehouse"></a>W przypadku usługi SQL Data Warehouse pętle
Magazyn danych SQL obsługuje [podczas][podczas] pętli wielokrotnie wykonywania bloków instrukcji. Operacja będzie kontynuowana dla tak długo, jak określone warunki są spełnione, lub do kodu w szczególności kończy się za pomocą pętli `BREAK` — słowo kluczowe. Pętle są szczególnie użyteczne w przypadku zastępowania kursory zdefiniowany w języku SQL. Na szczęście prawie wszystkie kursorów, które zostały napisane w języku SQL są szybkie przewijanie do przodu, odczytać tylko odmiany. W związku z tym [podczas] pętle są doskonałą alternatywę, jeśli okaże się, że konieczności zastąpić jeden.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Wykorzystanie pętle i zastępowanie kursory w usłudze SQL Data Warehouse
Jednak przed rozpoczęciem pracy w head należy poprosić samodzielnie następujące zapytania: "Można tego kursora można ponownie napisać z wykorzystaniem operacje na zestawie na podstawie?". W wielu przypadkach odpowiedzi będzie tak oraz jest często najlepszym rozwiązaniem. Na podstawie operację często wykonuje się znacznie szybciej niż przy użyciu metody iteracyjne, wiersz po wierszu.

Przewijanie kursory tylko do odczytu mogą łatwo zastępowane z konstrukcji pętli. Poniżej przedstawiono prosty przykład. Ten przykładowy kod aktualizuje statystyki dla każdej tabeli w bazie danych. Iterowanie po tabel w pętli możemy można wykonać każdego polecenia w sekwencji.

Najpierw utwórz tabeli tymczasowej zawierającą liczbę unikatowych wiersza używany do identyfikowania poszczególnych instrukcji:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Po drugie zainicjować zmienne wymaganych do wykonania pętli:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Teraz pętli instrukcje wykonywania jedną jednocześnie:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Na koniec porzucić tabeli tymczasowej utworzonego w pierwszym kroku

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad programistycznych, zobacz [omówienie tworzenia][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[podczas]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
