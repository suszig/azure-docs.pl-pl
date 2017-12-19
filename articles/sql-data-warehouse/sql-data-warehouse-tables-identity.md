---
title: "Tworzenie kluczy dwuskładnikowego przy użyciu tożsamości | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie korzystania z tożsamości do tworzenia kluczy Surogat na tabelach."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: e10b58743fad5f7c2c4f00b51f06d4ec9bcb6768
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="create-surrogate-keys-by-using-identity"></a>Tworzenie kluczy dwuskładnikowego przy użyciu tożsamości
> [!div class="op_single_selector"]
> * [Omówienie][Overview]
> * [Typy danych][Data Types]
> * [Dystrybuuj][Distribute]
> * [Indeks][Index]
> * [Partycji][Partition]
> * [Statystyki][Statistics]
> * [Tymczasowe][Temporary]
> * [Tożsamości][Identity]
> 
> 

Do tworzenia kluczy Surogat na ich tabel, podczas projektowania modeli magazynu danych, takich jak wiele Modelarze danych. Właściwość IDENTITY można używać na osiągnięcie tego celu po prostu i efektywnie bez wpływu na wydajność obciążenia. 

## <a name="get-started-with-identity"></a>Rozpoczynanie pracy z tożsamości
Można zdefiniować tabelę jako mający właściwość IDENTITY podczas tworzenia tabeli za pomocą składni, która jest podobna do następującej instrukcji:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1) NOT NULL
,   C2 INT NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;
```

Następnie można użyć `INSERT..SELECT` do wypełnienia tabeli.

## <a name="behavior"></a>Zachowanie
Właściwość IDENTITY jest przeznaczona do skalują poza wszystkie dystrybucje w magazynie danych bez wpływu na wydajność obciążenia. W związku z tym implementacja tożsamości jest zorientowany osiągnięcie tych celów. W tej sekcji przedstawiono wszystkie szczegóły implementacji, aby ułatwić zrozumienie ich dokładniejszego.  

### <a name="allocation-of-values"></a>Alokacja wartości
Właściwość IDENTITY nie gwarantuje kolejność, w którym są przydzielane wartości Surogat, odzwierciedla zachowanie programu SQL Server i bazy danych SQL Azure. Jednak w usłudze Azure SQL Data Warehouse, braku gwarancji jest większa. 

Poniższy przykład jest ilustracji:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)    NOT NULL
,   C2 VARCHAR(30)              NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

INSERT INTO dbo.T1
VALUES (NULL);

INSERT INTO dbo.T1
VALUES (NULL);

SELECT *
FROM dbo.T1;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

Dwa wiersze w powyższym przykładzie jest dystrybucji 1. Pierwszy wiersz zawiera wartość dwuskładnikowego 1 w kolumnie `C1`, a drugi wiersz zawiera wartość zastępcza 61. Obie te wartości zostały wygenerowane przez właściwość tożsamości. Jednak alokacji wartości nie jest ciągły. To zachowanie jest celowe.

### <a name="skewed-data"></a>Spowodowałoby zafałszowanie danych 
Zakres wartości dla typu danych są rozmieszczone równomiernie w obrębie dystrybucje. Jeśli tabela rozproszona odczuwa spowodowałoby zafałszowanie danych, dostępna na typ danych wartości z zakresu można wyczerpane przedwcześnie. Na przykład jeśli wszystkie dane kończy się w jednym dystrybucji, następnie efektywnie tabeli ma dostęp do tylko jednej szóstej wartości typu danych. Z tego powodu właściwość IDENTITY jest ograniczona do `INT` i `BIGINT` tylko typy danych.

### <a name="selectinto"></a>WYBIERZ... DO
Zaznaczenie istniejącej kolumny tożsamości do nowej tabeli nowej kolumny, która dziedziczy właściwości tożsamości, chyba że jest spełniony jeden z następujących warunków:
- Instrukcja SELECT zawiera sprzężenia.
- Wiele instrukcji "SELECT" są sprzęgane przy użyciu UNION.
- Kolumna tożsamości jest wymieniona więcej niż jeden raz na liście wyboru.
- Kolumna tożsamości jest częścią wyrażenia typu.
    
Jeśli jeden z tych warunków jest prawdziwy, kolumna jest tworzony NOT NULL zamiast dziedziczenia właściwości tożsamości.

### <a name="create-table-as-select"></a>UTWÓRZ TABLE AS SELECT
Tworzenie tabeli jako wybierz (CTAS) następuje tego samego zachowania programu SQL Server jest udokumentowany wybierz pozycję... DO. Jednak nie można określić właściwości tożsamości w definicji kolumny `CREATE TABLE` element instrukcji. Ponadto nie można używać funkcji IDENTITY w `SELECT` częścią CTAS. Aby wypełnić tabeli, należy użyć `CREATE TABLE` do definiowania tabeli, a następnie `INSERT..SELECT` aby wypełnić go.

## <a name="explicitly-insert-values-into-an-identity-column"></a>Jawnie wstawić wartości w kolumnie tożsamości 
Magazyn danych SQL obsługuje `SET IDENTITY_INSERT <your table> ON|OFF` składni. Ta składnia umożliwia jawnie wstawić wartości w kolumnie tożsamości.

Aby użyć wstępnie zdefiniowanych wartości ujemnych dla niektórych wierszy wymiarami, takich jak wiele Modelarze danych. Przykładem jest wiersza "Nieznany element członkowski" lub wartość -1. 

Skrypt dalej pokazano, jak jawnie dodać ten wiersz za pomocą USTAWIĆ atrybut IDENTITY_INSERT:

```sql
SET IDENTITY_INSERT dbo.T1 ON;

INSERT INTO dbo.T1
(   C1
,   C2
)
VALUES (-1,'UNKNOWN')
;

SET IDENTITY_INSERT dbo.T1 OFF;

SELECT  *
FROM    dbo.T1
;
```    

## <a name="load-data-into-a-table-with-identity"></a>Ładowanie danych do tabeli o tożsamości

Obecność właściwości tożsamości implikacje niektórych kodu ładowania danych. W tej sekcji opisano niektóre z wzorców podstawowych ładowania danych do tabel za pomocą tożsamości. 

### <a name="load-data-with-polybase"></a>Ładowanie danych za pomocą usługi PolyBase
Aby załadować dane do tabeli i generowanie klucza dwuskładnikowego przy użyciu tożsamości, należy utworzyć tabeli, a następnie użyć INSERT... Wybierz lub Wstaw... WARTOŚCI do wykonania obciążenia.

Poniższy przykład prezentuje podstawowy wzorzec:
 
```sql
--CREATE TABLE with IDENTITY
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)
,   C2 VARCHAR(30)
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

--Use INSERT..SELECT to populate the table from an external table
INSERT INTO dbo.T1
(C2)
SELECT  C2
FROM    ext.T1
;

SELECT  *
FROM    dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE] 
> Nie jest możliwe użycie `CREATE TABLE AS SELECT` obecnie, podczas ładowania danych do tabeli z kolumną tożsamości.
> 

Aby uzyskać więcej informacji na ładowanie danych przy użyciu narzędzia (BCP) programu kopiowania zbiorczego zobacz następujące artykuły:

- [Obciążenia przy użyciu programu PolyBase][]
- [Najlepsze rozwiązania w zakresie programu PolyBase][]

### <a name="load-data-with-bcp"></a>Ładowanie danych za pomocą narzędzia BCP
BCP jest narzędziem wiersza polecenia, które umożliwia ładowanie danych do usługi SQL Data Warehouse. Jeden z jego parametrów (-E) steruje zachowaniem BCP podczas ładowania danych do tabeli z kolumną tożsamości. 

W przypadku -E wartości przechowywane w pliku wejściowym dla kolumny o tożsamości są zachowywane. Jeśli jest -E *nie* określone wartości w tej kolumnie są ignorowane. Jeśli w kolumnie tożsamości nie jest dołączony, dane są ładowane normalnego. Wartości są generowane zgodnie z zasadami inkrementacji i inicjatora właściwości.

Aby uzyskać więcej informacji na ładowanie danych przy użyciu narzędzia BCP zobacz następujące artykuły:

- [Obciążenia za pomocą narzędzia BCP][]
- [Narzędzie BCP w witrynie MSDN][]

## <a name="catalog-views"></a>Widoków wykazu
Magazyn danych SQL obsługuje `sys.identity_columns` widoku katalogu. W tym widoku można zidentyfikować kolumny, która ma właściwość IDENTITY.

Aby lepiej zrozumieć schemat bazy danych, w tym przykładzie pokazano, jak zintegrować `sys.identity_columns` z innymi widokami katalog systemu:

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       CASE WHEN ic.column_id IS NOT NULL
             THEN 1
        ELSE 0
        END AS is_identity 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
LEFT JOIN   sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="limitations"></a>Ograniczenia
Właściwość IDENTITY nie można użyć w następujących scenariuszach:
- Gdy typ danych kolumny nie jest INT lub BIGINT
- Gdzie kolumna jest również dystrybucji kluczy
- Gdy tabela jest tabeli zewnętrznej 

Następujące funkcje pokrewne nie są obsługiwane w usłudze SQL Data Warehouse:

- [IDENTITY()][]
- [@@IDENTITY][]
- [SCOPE_IDENTITY][]
- [ATRYBUTU IDENT_CURRENT][]
- [IDENT_INCR][]
- [IDENT_SEED][]
- [POLECENIE DBCC CHECK_IDENT()][]

## <a name="tasks"></a>Zadania

Ta sekcja zawiera niektóre przykładowy kod służący do wykonywania typowych zadań, podczas pracy z kolumn tożsamości.

> [!NOTE] 
> Kolumna C1 jest tożsamość w następujące zadania.
> 
 
### <a name="find-the-highest-allocated-value-for-a-table"></a>Znajdź największa wartość przydzielonego dla tabeli
Użyj `MAX()` funkcji, aby ustalić najwyższą wartość przydzielone dla tabeli rozproszonych:

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-the-seed-and-increment-for-the-identity-property"></a>Znajdź początkowej i wartości przyrostu dla właściwości tożsamości
Widokach katalogów służy do odnajdywania inkrementacji i inicjatora konfiguracji wartości tożsamości dla tabeli za pomocą następującej kwerendy: 

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       ic.seed_value
,       ic.increment_value 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
JOIN        sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="next-steps"></a>Następne kroki

* Aby dowiedzieć się więcej na temat tworzenia tabel, zobacz [omówienie tabeli][Overview], [typów danych tabeli][Data Types], [dystrybucji tabeli] [ Distribute], [Indeksu tabeli][Index], [partycji tabeli][Partition], i [ Tabele tymczasowe][Temporary]. 
* Aby uzyskać więcej informacji na temat najlepszych rozwiązań, zobacz [najlepsze rozwiązania w zakresie usługi SQL Data Warehouse][SQL Data Warehouse Best Practices].  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Identity]: ./sql-data-warehouse-tables-identity.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

[Obciążenia za pomocą narzędzia bcp]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp/
[Obciążenia przy użyciu programu PolyBase]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/
[Najlepsze rozwiązania w zakresie programu PolyBase]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide/


<!--MSDN references-->
[Identity property]: https://msdn.microsoft.com/library/ms186775.aspx
[sys.identity_columns]: https://msdn.microsoft.com/library/ms187334.aspx
[IDENTITY()]: https://msdn.microsoft.com/library/ms189838.aspx
[@@IDENTITY]: https://msdn.microsoft.com/library/ms187342.aspx
[SCOPE_IDENTITY]: https://msdn.microsoft.com/library/ms190315.aspx
[ATRYBUTU IDENT_CURRENT]: https://msdn.microsoft.com/library/ms175098.aspx
[IDENT_INCR]: https://msdn.microsoft.com/library/ms189795.aspx
[IDENT_SEED]: https://msdn.microsoft.com/library/ms189834.aspx
[POLECENIE DBCC CHECK_IDENT()]: https://msdn.microsoft.com/library/ms176057.aspx

[Narzędzie bcp w witrynie MSDN]: https://msdn.microsoft.com/library/ms162802.aspx
  

<!--Other Web references-->  
