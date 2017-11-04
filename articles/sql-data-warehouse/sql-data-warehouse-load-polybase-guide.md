---
title: "Przewodnik dla przy użyciu programu PolyBase w usłudze SQL Data Warehouse | Dokumentacja firmy Microsoft"
description: "Wskazówki i zalecenia dotyczące przy użyciu programu PolyBase w scenariuszach SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4757fce1-96b3-48ea-8a51-be1385705f9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 9/13/2017
ms.custom: loading
ms.author: cakarst;barbkess
ms.openlocfilehash: e8ae0eb96200c167a8758df4ce20b51452cc59a4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Przewodnik dla przy użyciu programu PolyBase w usłudze SQL Data Warehouse
Ten przewodnik zawiera informacje praktyczne dla przy użyciu programu PolyBase w usłudze SQL Data Warehouse.

Aby rozpocząć, zobacz [ładowanie danych przy użyciu programu PolyBase] [ Load data with PolyBase] samouczka.

## <a name="rotating-storage-keys"></a>Obracanie magazynu kluczy
Od czasu do czasu można zmienić klucza dostępu do usługi magazynu obiektów blob ze względów bezpieczeństwa.

Najbardziej elegancki sposób wykonania tego zadania jest procedura jest znana jako "obracanie kluczy". Można zauważyć ma dwa klucze magazynu dla konta magazynu obiektów blob. Jest to, aby można przejść

Obracanie klucze konta magazynu platformy Azure jest procesem prosty krok trzy

1. Utworzyć drugi poświadczeń o zakresie bazy danych na podstawie magazynu pomocniczego klucza dostępu
2. Utworzyć drugi zewnętrznego źródła danych poza tym nowe poświadczenie
3. Usunąć i utworzyć tabel zewnętrznych, wskazujący nowe źródło danych zewnętrznych

Po dokonaniu migracji że zewnętrzne tabel do nowego zewnętrznego źródła danych, a następnie można wykonać zadania czyszczenia:

1. Usuń pierwszy zewnętrznego źródła danych
2. Poświadczenia magazynu podstawowy klucz dostępu w oparciu o zakresie pierwszy Porzuć bazę danych
3. Zaloguj się do platformy Azure i ponownie wygenerować podstawowy klucz dostępu gotowy do następnego



## <a name="load-data-with-external-tables"></a>Ładowanie danych z tabel zewnętrznych
W tym przykładzie ładuje dane z magazynu obiektów blob platformy Azure do bazy danych magazynu danych SQL.

Zapisywanie danych bezpośrednio usuwa czas transferu danych zapytań. Przechowywanie danych z indeksem magazynu kolumn poprawia wydajność kwerend analizy zapytań przez maksymalnie 10 x.

W tym przykładzie używa instrukcji CREATE TABLE AS SELECT do wczytywania danych. Nowa tabela dziedziczy kolumny o nazwie w zapytaniu. Typy danych tych kolumn dziedziczy z definicji tabeli zewnętrznej.

CREATE TABLE AS SELECT jest wysokiej wydajności instrukcji języka Transact-SQL, który ładuje dane jednocześnie do wszystkich węzłów obliczeniowych SQL Data Warehouse.  Pierwotnie został opracowany dla aparatu masowego przetwarzania równoległego (MPP) w Analytics Platform System, a jest teraz w usłudze SQL Data Warehouse.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Zobacz [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)].

> [!NOTE]
> Obciążenia za pomocą tabeli zewnętrznej może zakończyć się niepowodzeniem z powodu błędu *"zapytanie zostało przerwane — Osiągnięto próg maksymalnej Odrzuć podczas odczytu z zewnętrznego źródła"*. Oznacza to, że dane zewnętrzne zawiera *zanieczyszczone* rekordów. Rekord danych jest uznawany za "zakłócone", jeśli dane rzeczywiste typy/liczba kolumn nie są zgodne z definicji kolumn tabeli zewnętrznej lub nie jest zgodny z określonego pliku zewnętrznego formatu danych. Aby rozwiązać ten problem, sprawdź poprawność tabeli zewnętrznej i definicje format pliku zewnętrznego i zgodne dane zewnętrzne z tych definicji. W przypadku, gdy podzbiór rekordów danych zewnętrznych zostały zmienione, można odrzucić tych rekordów dla zapytania przy użyciu opcji Odrzuć utworzyć DDL tabeli zewnętrznej.
> 
> 


## <a name="create-statistics-on-newly-loaded-data"></a>Tworzenie statystyk na nowo załadowanych danych
Usługa Azure SQL Data Warehouse nie obsługuje jeszcze automatycznego tworzenia ani aktualizowania statystyk.  W celu uzyskania najlepszej wydajności zapytań należy utworzyć statystyki dla wszystkich kolumn wszystkich tabel po pierwszym załadowaniu danych, a następnie po każdej istotnej zmianie.  Szczegółowy opis statystyk znajduje się w temacie [Statystyki][Statistics] w grupie artykułów dla deweloperów.  Poniżej przedstawiono prosty przykład tworzenia statystyk dotyczących tabeli załadowanej w tym przykładzie.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-with-external-tables"></a>Eksportuj dane z tabel zewnętrznych
W tej sekcji przedstawiono sposób eksportowania danych z magazynu danych SQL do magazynu obiektów blob platformy Azure przy użyciu tabel zewnętrznych. W tym przykładzie użyto, tworzenie zewnętrznych TABLE AS SELECT czyli wysokiej wydajności instrukcję Transact-SQL, aby wyeksportować dane równolegle z węzłów obliczeniowych.

Poniższy przykład tworzy tabelę zewnętrzną Weblogs2014 przy użyciu definicje kolumn i danych z dbo. Tabela dzienników sieci Web. W definicji tabeli zewnętrznej jest przechowywany w usłudze SQL Data Warehouse i wyników w instrukcji SELECT są eksportowane do katalogu "/ archiwum/log2014 /" w kontenerze obiektów blob, określony przez źródło danych. Dane są eksportowane w formacie pliku określony tekst.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```
## <a name="isolate-loading-users"></a>Izolowanie podczas ładowania użytkowników
Często jest potrzeba wielu użytkowników, które można załadować danych do magazynu danych SQL. Ponieważ [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] wymaga uprawnienia kontroli bazy danych, spowoduje utworzenie z wieloma użytkownikami z kontroli dostępu za pośrednictwem wszystkich schematów. To ograniczenie, można użyć instrukcji sterowania ODMÓW.

Przykład: Należy wziąć pod uwagę schema_A schematów bazy danych dla działu A i schema_B dla działu B Let bazy danych użytkowników user_A i user_B można użytkowników dla ładowanie w Dział A i B, odpowiednio PolyBase. Oba przyznano uprawnienia kontroli bazy danych.
Twórcy blokady teraz A i B schematu w dół ich schematów przy użyciu odmowy:

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```   
 O tym user_A i user_B powinna teraz zostać zablokowane ze schematu innego działu.
 
## <a name="polybase-performance-optimizations"></a>Program PolyBase optymalizacji wydajności
Aby osiągnąć optymalną wydajność przy użyciu programu PolyBase ładowania Sugerujemy następujące wartości:
- Podziel duże pliki skompresowane na mniejsze pliki skompresowane. Typy kompresji obsługiwane obecnie nie są podzielne. W związku z tym wydajności będzie mieć wpływ ładowania jednego dużego pliku.
- Dla największą szybkość ładowania Załaduj do round_robin, tabela przemieszczania stosu. Są to najbardziej wydajny sposób przenoszenia danych z warstwy magazynu do magazynu danych.
- Wszystkie typy plików mają różne charakterystyki. Najszybszym obciążenia Użyj skompresowane rozdzielane pliki tekstowe. Różnica między UTF-8 i UTF-16 wydajności jest minimalny.
- Kolokuj warstwa magazynu i magazynu danych, aby zminimalizować opóźnienie
- Jeśli zadanie ładowania dużych skalowanie w górę magazynu danych.

## <a name="polybase-limitations"></a>Ograniczenia programu PolyBase
Aparat PolyBase w usłudze SQL magazynu danych ma następujące ograniczenia wymagające brane pod uwagę podczas projektowania zadanie ładowania:
- Pojedynczy wiersz nie może być większa niż 1 000 000 bajtów. Dotyczy to niezależnie od definicja schematu tabeli.
- Gdy eksportowanie danych do formatu pliku ORC z kolumn duże tekstu SQL Server lub magazyn danych SQL Azure można ograniczyć do kupić 50 kolumn z powodu java błędów pamięci. Aby obejść ten problem, należy wyeksportować tylko podzestaw kolumn.





## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej na temat przenoszenia danych do usługi SQL Data Warehouse, zobacz [Omówienie migracji danych][data migration overview].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[data migration overview]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
