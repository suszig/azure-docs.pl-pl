---
title: "Grupuj według opcje w usłudze SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Wskazówki dotyczące implementowania grupy Opcje w usłudze Azure SQL Data Warehouse związane z opracowywaniem rozwiązań."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f95a1e43-768f-4b7b-8a10-8a0509d0c871
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: da71cb834c13da5d0f5690f471efc6c696163f30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="group-by-options-in-sql-data-warehouse"></a>Grupuj według opcje w usłudze SQL Data Warehouse
[GROUP BY] [ GROUP BY] jest używana do agregowania danych podsumowania zestawu wierszy. Ma również kilka opcji, które rozszerzają jego funkcjonalność konieczne działał wokół, ponieważ nie są bezpośrednio obsługiwane przez Magazyn danych SQL Azure.

Te opcje są

* GROUP BY z pakietem ZBIORCZYM
* GROUPING SETS
* Klauzula GROUP BY modułu

## <a name="rollup-and-grouping-sets-options"></a>Ustawia opcje ROLLUP i grouping
Jest to najprostsza opcja używania `UNION ALL` zamiast tego przeprowadzić pakiet zbiorczy zamiast polegania na jawnej składni. Wynik jest dokładnie taka sama

Poniżej znajduje się przykład grupy za pomocą instrukcji `ROLLUP` opcji:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Za pomocą pakietu ZBIORCZEGO możemy żądanych agregacji następujące:

* Kraj lub Region
* Kraj
* Całkowita suma

Aby zamienić to będą musieli używać `UNION ALL`; Określanie agregacji jawnie muszą zwracać takie same wyniki:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

GROUPING SETS wszystkie konieczne jest przyjąć tej samej nazwy głównej, ale tylko utworzyć UNION ALL sekcjach poziomów agregacji, które chcesz zobaczyć

## <a name="cube-options"></a>Opcje modułu
Istnieje możliwość utworzenia grupy przez z modułu przy użyciu podejścia UNION ALL. Problem polega na tym, że kod może szybko stać się skomplikowane i niewygodna. Aby temu zaradzić możesz użyć tego bardziej zaawansowane metody.

Użyjmy w powyższym przykładzie.

Pierwszym krokiem jest określenie "module" definiujący wszystkie poziomy agregacji, jeśli chcesz utworzyć. Należy pamiętać o CROSS JOIN dwóch tabel pochodnych. Spowoduje to wygenerowanie wszystkie poziomy firmie Microsoft. Pozostała część kodu jest naprawdę formatowania.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

Wyniki CTAS są widoczne poniżej:

![][1]

Drugim krokiem jest określenie tabeli docelowej do przechowywania wyników pośrednich:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Trzeci krok ma pętlę naszych modułu wykonywania agregacji kolumn. Zapytanie zostanie uruchomione jeden raz dla każdego wiersza w tabeli tymczasowej #Cube i zapisać wyniki w tabeli tymczasowej #Results

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Na koniec zostanie zwrócona po prostu przeczytaj z tabeli tymczasowej #Results wyników

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Przez podzielenie kod na sekcje i generowanie konstrukcji pętli kodu staje się bardziej łatwe w zarządzaniu i łatwy w obsłudze.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad programistycznych, zobacz [omówienie tworzenia][development overview].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GROUP BY]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
