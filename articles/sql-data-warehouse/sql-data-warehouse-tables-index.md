---
title: "Indeksowanie tabel w usłudze SQL Data Warehouse | Microsoft Azure"
description: "Wprowadzenie do korzystania z tabeli indeksowanie w usłudze Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 03/15/2018
ms.author: barbkess
ms.openlocfilehash: f0e19be5ec7453a76ff4f22ead6d3885e50beca8
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="indexing-tables-in-sql-data-warehouse"></a>Indeksowanie tabel w usłudze SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Omówienie][Overview]
> * [Typy danych][Data Types]
> * [Dystrybuuj][Distribute]
> * [Indeks][Index]
> * [Partycji][Partition]
> * [Statystyki][Statistics]
> * [Tymczasowe][Temporary]
> 
> 

Magazyn danych SQL oferuje kilka opcji indeksowania tym [klastrowane indeksy magazynu kolumn][clustered columnstore indexes], [klastrowane indeksy i nieklastrowanych indeksów] [ clustered indexes and nonclustered indexes].  Ponadto zapewnia ona również nie opcji indeksu znanej także jako [sterty][heap].  W tym artykule omówiono zalety każdego typu indeksu, a także wskazówek dotyczących uzyskiwania większości wydajności poza z indeksów. Zobacz [Tworzenie tabeli składni] [ create table syntax] uzyskać więcej szczegółowych informacji na temat tworzenia tabeli w usłudze SQL Data Warehouse.

## <a name="clustered-columnstore-indexes"></a>Klastrowane indeksy magazynu kolumn
Domyślnie usługi SQL Data Warehouse tworzy klastrowany indeks magazynu kolumn, czy nie indeksu określono opcji dla tabeli. Tabel klastrowanego magazynu kolumn oferują zarówno najwyższy poziom kompresji danych, a także ogólną najlepszą wydajność zapytań.  Tabel klastrowanego magazynu kolumn będą zazwyczaj przewyższyć klastrowanego indeksu lub sterty tabel i są zazwyczaj najlepszym rozwiązaniem w przypadku dużych tabel.  Z tego względu klastrowanego magazynu kolumn jest najlepszym miejscem, aby rozpocząć, gdy wiadomo, jak indeksu tabeli.  

Aby utworzyć tabelę klastrowanego magazynu kolumn, po prostu określić KLASTROWANY indeks magazynu kolumn w klauzuli WITH lub pozostaw klauzuli WITH:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Istnieje kilka scenariuszy, w którym klastrowanego magazynu kolumn nie może być dobrym rozwiązaniem:

* Tabele magazynu kolumn nie obsługują varchar(max), nvarchar(max) i varbinary(max).  Zamiast tego należy rozważyć stosu lub indeksu klastrowanego.
* Tabele magazynu kolumn mogą być mniej wydajne przejściowej danych.  Należy wziąć pod uwagę sterty, a czasami nawet tymczasowych tabel.
* Mała tabele zawierające mniej niż 100 milionów wierszy.  Należy wziąć pod uwagę sterty tabel.

## <a name="heap-tables"></a>Tabele sterty
W przypadku tymczasowego kierowania danych do usługi SQL Data Warehouse może okazać się, że użycie tabeli stosu przyśpieszy cały proces.  Jest to spowodowane obciążeń do stosów są szybsze niż tabele indeksu, a w niektórych przypadkach można wykonać kolejne odczytu z pamięci podręcznej.  Jeśli dane są ładowane tylko pod kątem przygotowania do uruchomienia kolejnych przekształceń, załadowanie tabeli do stosu będzie znacznie szybsze niż załadowanie danych do tabeli klastrowanego magazynu kolumn. Ponadto podczas ładowania danych do [tabeli tymczasowej] [ Temporary] również załaduje znacznie szybciej niż podczas ładowania tabeli w magazynie trwałym.  

Dla tabel odnośników małych, wiersze mniej niż 100 milionów często sterty tabel sensu.  Rozpocznij klastra magazynu kolumn tabel do osiągnięcia optymalnej kompresji po więcej niż 100 milionów wierszy.

Aby utworzyć tabelę sterty, po prostu określić STERTY w klauzuli WITH:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Klastrowanych i nieklastrowanych indeksów
Indeksy klastrowane może przewyższyć tabel klastrowanego magazynu kolumn, gdy trzeba szybko pobrać pojedynczy wiersz.  Dla zapytania, gdy jeden lub kilka bardzo wyszukiwania wiersza jest wymagany do wydajność i szybkość skrajne należy wziąć pod uwagę indeksu klastra lub dodatkowej indeks nieklastrowany.  Wadą korzystania indeks klastrowany jest skorzystają tylko zapytania, które korzystać z wysokiej selektywnego filtru kolumny indeksu klastrowanego.  Do poprawy filtr na innych kolumn indeksu nieklastrowanego na indeks mogą być dodawane do innych kolumn.  Jednak każdy indeks, który zostanie dodany do tabeli doda zarówno miejsce i czas przetwarzania na obciążenia.

Aby utworzyć tabelę indeks klastrowany, po prostu określić INDEKSU KLASTROWANEGO w klauzuli WITH:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Aby dodać nieklastrowanego indeksu dla tabeli, należy po prostu użyć następującej składni:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

## <a name="optimizing-clustered-columnstore-indexes"></a>Optymalizacja klastrowane indeksy magazynu kolumn
Tabel klastrowanego magazynu kolumn są zorganizowane w danych na segmenty.  Krytyczne znaczenie dla osiągnięcia optymalnego działania zapytań w tabeli magazynu kolumn jest posiadanie segmentu wysokiej jakości.  Jakość segmentu można zmierzyć przez liczbę wierszy w grupie skompresowany wiersza.  Jakość segmentu jest optymalny, gdy istnieją co najmniej 100 KB wiersze na wiersz skompresowany grupy i uzyskanie wydajności jako liczba wierszy przypadających na wiersz podejścia dotyczącego grupy 1 048 576 wierszy, czyli większości wiersze, które mogą zawierać grupę wierszy.

Poniżej widoku można tworzyć i używanych w systemie do obliczenia średniej wiersze na wiersz grupy i zidentyfikować indeksach magazynu kolumn nieoptymalnych klastra.  Ostatnia kolumna, w tym widoku wygeneruje jako instrukcji SQL, która może służyć do reorganizacji indeksów: sieci.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,    COUNT(DISTINCT rg.[partition_number])                    AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,    CEILING    ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Teraz, po utworzeniu widoku, uruchom to zapytanie do identyfikacji tabel z grupy wierszy jest mniejsza niż 100 KB wierszy.  Oczywiście można zwiększenie progu k 100, jeśli szukasz więcej jakości optymalne segmentu. 

```sql
SELECT    *
FROM    [dbo].[vColumnstoreDensity]
WHERE    COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Po uruchomieniu zapytania, które można rozpocząć przyjrzeć się dane i analiza wyników. W następującej tabeli opisano, co ma zostać wyszukane w analizy grupy wierszy.

| Kolumna | Jak używać tych danych |
| --- | --- |
| [table_partition_count] |Jeśli tabela jest podzielona na partycje, może spodziewać zobaczyć liczby wyższej grupy Otwórz wiersz. Każda partycja w dystrybucji może teoretycznie istnieje grupa Otwórz wiersz skojarzony z nim. Uwzględnić to analizy. Mała tabelę, która ma zostać podzielona na partycje można zoptymalizowana przez usunięcie całkowicie partycjonowania ponieważ może to poprawić kompresji. |
| [row_count_total] |Całkowita liczba wierszy tabeli. Na przykład tej wartości można użyć do obliczenia odsetek wierszy w stanie skompresowane. |
| [row_count_per_distribution_MAX] |Jeśli wszystkie wiersze są równomiernie ta wartość będzie docelowy liczba wierszy przypadających na dystrybucji. Porównuje tę wartość z compressed_rowgroup_count. |
| [COMPRESSED_rowgroup_rows] |Całkowita liczba wierszy w formacie magazynu kolumn dla tabeli. |
| [COMPRESSED_rowgroup_rows_AVG] |Jeżeli to średnia liczba wierszy jest znacznie mniejsza niż maksymalna liczba wierszy dla grupy wierszy, należy rozważyć użycie CTAS lub ALTER INDEX REBUILD do ponownej danych |
| [COMPRESSED_rowgroup_count] |Liczba grup wierszy w formacie magazynu kolumn. Jeśli ta liczba jest bardzo wysoka w odniesieniu do tabeli jest wskaźnik brakuje gęstość magazynu kolumn. |
| [COMPRESSED_rowgroup_rows_DELETED] |Wiersze logicznie są usuwane z formatu magazynu kolumn. Jeśli liczba jest wysoka względem rozmiar tabeli, należy rozważyć ponowne tworzenie partycji lub odbudowanie indeksu, ponieważ spowoduje to usunięcie ich fizycznie. |
| [COMPRESSED_rowgroup_rows_MIN] |Umożliwia to w połączeniu z kolumnami średnia i maksymalna zrozumieć zakres wartości dla grupy wierszy w Twojego magazynu kolumn. Niski numer powyżej wartości progowej obciążenia (102400 na partycji wyrównane dystrybucji) sugeruje, że optymalizacji są dostępne podczas ładowania danych |
| [COMPRESSED_rowgroup_rows_MAX] |Jak powyżej |
| [OPEN_rowgroup_count] |Otwórz wiersz grupy to normalne zachowanie. Czy jedną spodziewać jednej grupy Otwórz wiersz na tabeli dystrybucji (60). Nadmiernej liczby sugeruje ładowania w partycji danych. Sprawdź strategii partycjonowania, aby zapewnić, że jest dźwięku |
| [OPEN_rowgroup_rows] |Każda grupa wiersza może mieć 1 048 576 wierszy w nim co najwyżej. Użyj tej wartości, aby zobaczyć, jak Pełna grupy Otwórz wierszy są obecnie |
| [OPEN_rowgroup_rows_MIN] |Otwieranie obszaru roboczego grupy oznaczać, że dane jest strumieniem ładowany do tabeli albo czy poprzedniej obciążenia za pośrednictwem rozrzucone pozostałych wierszy do tej grupy wierszy. Użyj MIN, MAX, AVG kolumn, aby zobaczyć, jak dużo danych jest znajdowało się w Otwórz wiersz grup. W przypadku małych tabel może to być 100% wszystkich danych! W takim przypadku ALTER INDEX REBUILD wymusić danych do magazynu kolumn. |
| [OPEN_rowgroup_rows_MAX] |Jak powyżej |
| [OPEN_rowgroup_rows_AVG] |Jak powyżej |
| [CLOSED_rowgroup_rows] |Spójrz na wiersz zamkniętego wiersze grupy w celu sprawdzenia związane z poprawnością. |
| [CLOSED_rowgroup_count] |Liczba grupy zamkniętego wierszy powinna być niska, jeśli dowolne są widoczne w ogóle. Grupy wierszy skompresowane za pomocą ALTER INDEX można przekonwertować grupy wierszy zamkniętego... REORGANIZACJA polecenia. Jednak nie jest to zwykle wymagane. Zamknięte grupy są automatycznie konwertowane na grupy wierszy magazynu kolumn przez proces w tle "przenoszenia krotki". |
| [CLOSED_rowgroup_rows_MIN] |Grupy wierszy zamkniętego powinny mieć współczynnik wypełnienia bardzo duże. Jeśli brakuje współczynnika wypełnienia grupy wierszy zamkniętego dalszej analizy magazynu kolumn jest wymagana. |
| [CLOSED_rowgroup_rows_MAX] |Jak powyżej |
| [CLOSED_rowgroup_rows_AVG] |Jak powyżej |
| [Rebuild_Index_SQL] |SQL do odbudowywania indeksu magazynu kolumn dla tabeli |

## <a name="causes-of-poor-columnstore-index-quality"></a>Przyczyny jakości indeksu magazynu kolumn niska
Po zidentyfikowaniu tabel z jakością niską segmentu, należy zidentyfikować przyczynę.  Poniżej przedstawiono niektóre typowe przyczyny segmentu słabą jakość:

1. Wykorzystania pamięci, gdy indeks został skompilowany.
2. Duża liczba operacji DML
3. Mała lub ścieknie operacji obciążenia
4. Zbyt wiele partycji

Te czynniki mogą powodować indeksu magazynu kolumn, aby znacznie mniejsza niż optymalne 1 miliona wierszy dla każdej grupy wierszy.  Może również spowodować wiersze przejść do grupy wierszy delta zamiast grupy skompresowany wiersza. 

### <a name="memory-pressure-when-index-was-built"></a>Wykorzystania pamięci, gdy indeks został skompilowany.
Liczba wierszy w grupę skompresowany wierszy są bezpośrednio związane z szerokość wiersza i ilość dostępnej pamięci, aby przetworzyć grupę wierszy.  Jeśli wiersze są zapisywane w tabelach magazynu kolumn przy dużym wykorzystaniu pamięci, może to spowodować obniżenie jakości segmentów w magazynie kolumn.  W związku z tym najlepszym rozwiązaniem jest zapewnienie sesji, który zapisuje dostęp tabel indeksu magazynu kolumn do ilości pamięci, jak to możliwe.  Ponieważ istnieje zależność między pamięcią i współbieżności, wskazówki dotyczące alokacji pamięci prawo zależy od danych w każdym wierszu tabeli, jednostki magazynu danych, które są przydzielone do systemu i liczbę gniazd współbieżności można nadawać sesji którego zapisuje dane do tabeli.  Najlepszym rozwiązaniem zaleca się uruchomienie z xlargerc, jeśli używasz DW300 lub mniej largerc, jeśli używasz DW400 DW600 i mediumrc, jeśli używasz DW1000 lub nowszym.

### <a name="high-volume-of-dml-operations"></a>Duża liczba operacji DML
Duża liczba operacji DML, które aktualizować i usuwać wiersze można wprowadzać nieefektywne podejście do magazynu kolumn. Dotyczy to zwłaszcza po zmodyfikowaniu większość wiersze, grupy wierszy.

* Usuwanie wiersza z grupy wierszy skompresowany tylko logicznie oznacza wiersza jako usunięte. Wiersz pozostaje w grupie wiersza skompresowany, dopóki odbudowaniu partycji lub tabeli.
* Wstawienie wiersza dodaje do wiersza do tabeli wewnętrznej magazynu wierszy o nazwie grupy wierszy delta. Wstawionego wiersza nie jest konwertowany na magazynu kolumn, aż grupy wierszy delta jest zapełniony i jest oznaczone jako zamknięte. Grupy wierszy zostaną zamknięte po upływie maksymalną pojemność 1 048 576 wierszy. 
* Aktualizacja wiersza w formacie magazynu kolumn jest przetwarzany jako delete logicznej, a następnie insert. Wstawionego wiersza mogą być przechowywane w magazynie delta.

Umieścić w zadaniu wsadowym aktualizacji i Wstaw operacje przekraczające próg zbiorczego 102 400 wierszy przypadających na partycję wyrównany dystrybucji będą zapisywane bezpośrednio do formatu magazynu kolumn. Jednak przy założeniu Rozdziel, konieczne będzie można modyfikować ponad milion 6.144 wierszy w ramach jednej operacji w tym celu włączone. Jeśli liczba wierszy w danej partycji wyrównane dystrybucji znajduje się mniej niż 102400 wiersze nastąpi przejście do sklepu różnicowych i pozostaną dostępne do momentu wystarczające wiersze zostały wstawione lub zmodyfikowane zamknąć grupy wierszy lub indeks został ponownie skompilowany.

### <a name="small-or-trickle-load-operations"></a>Mała lub ścieknie operacji obciążenia
Mała liczba godzin ładuje, że przepływ do usługi SQL Data Warehouse czasami nazywa się ścieknie obciążeń. Zazwyczaj reprezentują near stały strumień danych jest pozyskanych przez system. Jednak ponieważ ten strumień jest bliski ciągłego wolumin wierszy nie jest szczególnie duże. Najczęściej danych znacznie jest poniżej wartości progowej, wymaganych do bezpośredniego obciążenia do formatu magazynu kolumn.

W takich sytuacjach warto często najpierw trafić dane w magazynie obiektów blob platformy Azure i pozwól mu gromadzone przed załadowaniem. Ta metoda jest często nazywany *przetwarzanie wsadowe micro*.

### <a name="too-many-partitions"></a>Zbyt wiele partycji
Innym czynnikiem, który należy wziąć pod uwagę jest wpływ partycjonowania na tabel klastrowanego magazynu kolumn.  Przed partycje, SQL Data Warehouse już dzieli dane 60 baz danych.  Dodatkowo partycjonowania dzieli dane.  Jeśli partycji danych, a następnie należy wziąć pod uwagę, że **każdego** partycji musi mieć co najmniej 1 milion wierszy do korzystania z klastrowanego indeksu magazynu kolumn.  Jeśli partycji tabeli na partycje 100, a następnie tabela musi mieć co najmniej 6 miliardy wierszy do korzystania z klastrowanego indeksu magazynu kolumn (60 dystrybucje * partycje 100 * 1 milion wierszy). Jeśli tabela 100 partycji nie ma 6 miliardy wierszy, Zmniejsz liczbę partycji lub tabeli sterty zamiast tego Rozważ użycie.

Po tabele zostały załadowane z niektórych danych, wykonaj następujące czynności, aby zidentyfikować i Przebuduj tabele z indeksami magazynu kolumn nieoptymalnych klastra.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Ponowne tworzenie indeksów do poprawy jakości segmentu
### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>Krok 1: Określenie lub Utwórz użytkownika, który używa klasy zasobu prawo
Szybki sposób natychmiast poprawić jakość segmentu jest odbudowanie indeksu.  SQL zwrócone przez widok powyżej zwróci błąd instrukcji ALTER INDEX REBUILD, która może służyć do reorganizacji indeksów: sieci.  Ponowne tworzenie indeksów sieci, należy pamiętać, że można przydzielić wystarczającej ilości pamięci do sesji, który będzie odbudowanie indeksu.  Aby to zrobić, należy zwiększyć klasa zasobów użytkownika, który ma uprawnienia do odbudowywania indeksu w tej tabeli do minimum, zalecane.  Nie można zmienić klasy zasobów użytkownika właściciela bazy danych, więc jeśli nie utworzono użytkownika w systemie, należy w tym celu najpierw.  Zalecanym przez nas minimum jest xlargerc Jeśli używasz DW300 lub mniej, largerc Jeśli używasz DW400 DW600 i mediumrc, jeśli używasz DW1000 lub nowszym.

Poniżej przedstawiono przykładowy sposób przydzielania większej ilości pamięci do użytkownika przez odpowiednie zwiększenie ich klasy zasobów.  Aby uzyskać więcej informacji o zasobie klasy oraz sposobu tworzenia nowego użytkownika można znaleźć w [zarządzania współbieżności i obciążenia] [ Concurrency] artykułu.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Krok 2: Ponownie klastrowane indeksy magazynu kolumn za pomocą nowszej użytkownika klasy zasobu
Logowania jako użytkownik z kroku 1 (np. LoadUser), który jest teraz za pomocą nowszej klasy zasobów i wykonywania instrukcji ALTER INDEX.  Pamiętaj, że ten użytkownik ma uprawnienia ALTER tabel, w którym jest odbudować indeksu.  Poniższe przykłady prezentują sposób odbudowywania indeksu magazynu kolumn całą lub sposobie odbudowania jednej partycji. W dużych tabel jest więcej praktyczne odbudowy indeksów jednej partycji naraz.

Alternatywnie w zamiast odbudowanie indeksu, nowa za pomocą tabeli można skopiować tabeli [CTAS][CTAS].  Jaki sposób jest najlepsza? Dla dużych ilości danych [CTAS] [ CTAS] jest zwykle szybsze niż [ALTER INDEX][ALTER INDEX]. W przypadku mniejszych woluminów danych [ALTER INDEX] [ ALTER INDEX] jest łatwiejsza w użyciu i nie będzie trzeba wymienić tabeli.  Zobacz **ponowne tworzenie indeksów CTAS i przełączanie partycji** poniżej więcej szczegółów na temat sposobu Odbuduj indeksy z CTAS.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Ponowne tworzenie indeksu w usłudze SQL Data Warehouse jest operacją w trybie offline.  Aby uzyskać więcej informacji na temat ponowne tworzenie indeksów w sekcji ALTER INDEX REBUILD w [defragmentacji indeksy magazynu kolumn][Columnstore Indexes Defragmentation], i [ALTER INDEX] [ ALTER INDEX].

### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Krok 3: Sprawdź, czy uległa poprawie jakości segmentu klastrowanego magazynu kolumn
Ponownie uruchom zapytanie, które zidentyfikowanych tabeli z słaby segment jakości i sprawdź jakości segmentu została ulepszona.  Jeżeli nie poprawy jakości segmentu, może to być, czy wiersze w tabeli są bardzo szeroki.  Należy rozważyć użycie nowszej klasy zasobu lub DWU podczas odbudowywania z indeksów.

## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Ponowne tworzenie indeksów CTAS i przełączanie partycji
W tym przykładzie użyto [CTAS] [ CTAS] i przełączania w celu odbudowania partycji tabeli partycji. 

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Aby uzyskać więcej informacji o ponowne tworzenie partycji przy użyciu `CTAS`, zobacz [partycji] [ Partition] artykułu.

## <a name="next-steps"></a>Kolejne kroki
Aby dowiedzieć się więcej, zobacz artykuły w [omówienie tabeli][Overview], [typy danych tabeli][Data Types], [Dystrybucja tabeli] [ Distribute], [Partycjonowania tabeli][Partition], [utrzymania statystyk tabeli] [ Statistics] i [Tabel tymczasowych][Temporary].  Aby dowiedzieć się więcej o najlepszych rozwiązaniach, zobacz [najlepsze rozwiązania magazynu danych SQL][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./resource-classes-for-workload-management.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[ALTER INDEX]: https://msdn.microsoft.com/library/ms188388.aspx
[heap]: https://msdn.microsoft.com/library/hh213609.aspx
[clustered indexes and nonclustered indexes]: https://msdn.microsoft.com/library/ms190457.aspx
[create table syntax]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore Indexes Defragmentation]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[clustered columnstore indexes]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
