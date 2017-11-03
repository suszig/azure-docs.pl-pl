---
title: "Procedury składowane w usłudze SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Wskazówki dotyczące implementowania procedur przechowywanych w usłudze Azure SQL Data Warehouse związane z opracowywaniem rozwiązań."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 9b238789-6efe-4820-bf77-5a5da2afa0e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: e42d80f0ca35f3fbb67389c66d072bc40d8a8d2c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="stored-procedures-in-sql-data-warehouse"></a>Procedury składowane w usłudze SQL Data Warehouse
Magazyn danych SQL obsługuje wiele funkcji języka Transact-SQL w programie SQL Server. Są ważniejsze skalowania w poziomie określonych funkcji, które należy wykorzystać w celu zmaksymalizowania wydajności rozwiązania.

Aby zachować skalowalność i wydajność usługi SQL Data Warehouse są jednak także niektóre funkcje i funkcje, które mają różnice funkcjonalne i inne osoby, które nie są obsługiwane.

W tym artykule opisano sposób wykonania procedur przechowywanych w usłudze SQL Data Warehouse.

## <a name="introducing-stored-procedures"></a>Wprowadzenie do procedury składowane
Procedury składowane są to dobry sposób na hermetyzując kodu SQL; zapisanie jej pobliżu danych w magazynie danych. Hermetyzując kod w jednostki zarządzane procedur składowanych pomóc deweloperom modularyzacji ich rozwiązań. ułatwianie większa ponownego wykorzystywania kodu. Każdej procedury składowanej również może przyjmować parametry, aby były bardziej elastyczne.

Magazyn danych SQL zawiera implementację uproszczony i zoptymalizowany procedury składowanej. Największych różnicę w porównaniu z programem SQL Server jest, że procedura składowana nie jest wstępnie skompilowanego kodu. W hurtowni danych możemy dotyczy ogólnie mniej czasu kompilacji. Jest większe znaczenie kodu procedury składowanej poprawnie są zoptymalizowane podczas działania względem dużych ilości danych. Celem jest do zapisywania godziny, minuty i sekundy nie milisekund. Dlatego jest bardziej użyteczne do procedury składowane można traktować jako kontenery logiki SQL.     

Gdy usługi SQL Data Warehouse wykonuje Twoja procedura składowana są przeanalizować instrukcji SQL, tłumaczenia i zoptymalizowany w czasie wykonywania. W trakcie tego procesu każda instrukcja jest konwertowany na zapytań rozproszonych. Kod SQL, który faktycznie jest wykonywane względem danych różni się w zapytaniu przesłane.

## <a name="nesting-stored-procedures"></a>Procedury przechowywane zagnieżdżenia
Jeśli procedury składowane wywołania innych procedur składowanych lub wykonać dynamicznej sql, a następnie wewnętrzny przechowywanych wywołanie procedury lub kod jest uznawany za zagnieżdżony.

Magazyn danych SQL obsługuje maksymalnie 8 poziomów zagnieżdżenia. Różni się nieco do programu SQL Server. Poziom zagnieździć w programie SQL Server to 32.

Wywołanie najwyższego poziomu procedury składowanej jest równa zagnieździć poziomu 1

```sql
EXEC prc_nesting
```
Jeśli procedura składowana powoduje EXEC innego wywołania następnie zwiększy to gniazdo poziom 2

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Jeśli drugiej procedury następnie wykonuje niektóre dynamiczne sql, a następnie to spowoduje zwiększenie poziomu zagnieżdżania do 3

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Uwaga SQL Data Warehouse nie obsługuje obecnie@NESTLEVEL. Należy śledzić poziom zagnieżdżania samodzielnie. Jest mało prawdopodobne, nastąpi trafienie poziomu limit zagnieżdżania 8, ale jeśli chcesz konieczne będzie ponownie działać kodu i "spłaszczanie" go tak, aby zmieścił się w ten limit.

## <a name="insertexecute"></a>INSERT... WYKONANIE
Magazyn danych SQL nie pozwala na korzystanie z zestawu wyników procedury składowanej w instrukcji INSERT. Istnieje jednak alternatywne podejście, których można użyć.

Znajduje się w następującym artykule w [tabel tymczasowych] na przykład, jak to zrobić.

## <a name="limitations"></a>Ograniczenia
Brak niektórych aspektów procedury przechowywanej Transact-SQL, które nie są zaimplementowane w usłudze SQL Data Warehouse.

Są to:

* tymczasowych procedur składowanych
* numerowane procedury składowane
* rozszerzone procedury składowane
* Procedur składowanych CLR
* Opcja szyfrowania
* opcji replikacji
* Parametry przechowywanymi w tabeli
* Parametry tylko do odczytu
* domyślne parametry
* Kontekst wykonywania
* Return — instrukcja

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad programistycznych, zobacz [omówienie tworzenia][development overview].

<!--Image references-->

<!--Article references-->
[tabel tymczasowych]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
