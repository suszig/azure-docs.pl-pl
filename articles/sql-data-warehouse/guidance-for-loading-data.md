---
title: "Wskazówki dotyczące ładowania danych — Azure SQL Data Warehouse | Microsoft Docs"
description: "Zalecenia dotyczące ładowania danych oraz wyodrębniania, ładowania i transformacji (ELT) w usłudze Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 7b698cad-b152-4d33-97f5-5155dfa60f79
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 12/13/2017
ms.author: barbkess
ms.openlocfilehash: 8903be1361d1574a5d81b69223f608ccb7a698ea
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="guidance-for-loading-data-into-azure-sql-data-warehouse"></a>Wskazówki dotyczące ładowania danych do usługi Azure SQL Data Warehouse
Zalecenia dotyczące ładowania danych do usługi Azure SQL Data Warehouse i optymalizacji wydajności tego procesu. 

- Aby dowiedzieć się więcej na temat technologii PolyBase oraz projektowania procesu wyodrębniania, ładowania i transformacji (ELT, Extract, Load, and Transform), zobacz [Design ELT for SQL Data Warehouse (Projektowanie procesu ELT dla usługi SQL Data Warehouse)](design-elt-data-loading.md).
- Aby zapoznać się z samouczkiem dotyczącym ładowania, zobacz [Use PolyBase to load data from Azure blob storage to Azure SQL Data Warehouse (Ładowanie danych z usługi Azure Blob Storage do usługi Azure SQL Data Warehouse przy użyciu technologii PolyBase)](load-data-from-azure-blob-storage-using-polybase.md).


## <a name="extract-source-data"></a>Wyodrębnianie danych źródłowych

Podczas eksportowania danych z programu SQL Server lub usługi Azure SQL Data Warehouse do formatu pliku ORC, jeśli dane obejmują kolumny zawierające dużo tekstu, liczba kolumn może zostać ograniczona nawet do 50 z powodu błędów braku pamięci w środowisku Java. Aby obejść ten problem, można wyeksportować tylko podzestaw wszystkich kolumn.


## <a name="land-data-to-azure"></a>Ładowanie danych na platformie Azure
Technologia PolyBase nie umożliwia ładowania wierszy zawierających ponad milion bajtów danych. Dane umieszczane w plikach tekstowych w usłudze Azure Blob Storage lub Azure Data Lake Store muszą zawierać mniej niż milion bajtów danych w wierszu. Jest to niezależne od zdefiniowanego schematu tabeli.

Aby zminimalizować opóźnienie, warstwa magazynu i magazyn danych powinny znajdować się w jednej lokalizacji.

## <a name="data-preparation"></a>Przygotowywanie danych

Różne typy plików mają różną charakterystykę wydajności. Najszybsze ładowanie zapewnia użycie skompresowanych rozdzielanych plików tekstowych. Różnica w wydajności między kodowaniem UTF-8 a UTF-16 jest minimalna.

Duże pliki skompresowane można podzielić na mniejsze.

## <a name="create-designated-loading-users"></a>Tworzenie użytkowników ładujących
Aby uruchamiać zadania ładowania przy użyciu odpowiednich zasobów obliczeniowych, utwórz użytkowników ładujących na potrzeby uruchamiania ładowania. Przypisz każdego użytkownika ładującego do określonej klasy zasobów. Aby uruchomić ładowanie, zaloguj się jako jeden z użytkowników ładujących, a następnie uruchom ładowanie. Ładowanie zostanie uruchomione przy użyciu klasy zasobów przypisanej do użytkownika.  Jest to prostsza metoda niż zmienianie klasy zasobów użytkownika odpowiednio do bieżących potrzeb.

Ten kod umożliwia utworzenie użytkownika ładującego dla klasy zasobów staticrc20. Przyznaje użytkownikowi uprawnienia kontrolne w bazie danych, a następnie dodaje użytkownika jako członka roli bazy danych staticrc20. Aby uruchomić ładowanie z zasobami należącymi do klasy zasobów staticRC20, wystarczy zalogować się jako użytkownik LoaderRC20 i uruchomić ładowanie. 

    ```sql
    CREATE LOGIN LoaderRC20 WITH PASSWORD = 'a123STRONGpassword!';
    CREATE USER LoaderRC20 FOR LOGIN LoaderRC20;
    GRANT CONTROL ON DATABASE::[mySampleDataWarehouse] to LoaderRC20;
    EXEC sp_addrolemember 'staticrc20', 'LoaderRC20';
    ```

Lepszym rozwiązaniem jest uruchamianie ładowania przy użyciu statycznych, a nie dynamicznych klas zasobów. Użycie statycznych klas zasobów gwarantuje takie same zasoby niezależnie od [poziomu usług](performance-tiers.md#service-levels). W przypadku użycia dynamicznej klasy zasobów zasoby różnią się w zależności od poziomu usług. W przypadku dynamicznych klas zasobów niższy poziom usług oznacza, że prawdopodobnie konieczne będzie użycie większej klasy zasobów na potrzeby użytkownika ładującego.

### <a name="example-for-isolating-loading-users"></a>Przykład izolacji użytkowników ładujących

Często wymagane jest umożliwienie ładowania danych do usługi SQL DW wielu użytkownikom. Ponieważ instrukcja [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] wymaga uprawnień kontrolnych (CONTROL) w bazie danych, prowadzi to do umożliwienia wielu użytkownikom dostępu do wszystkich schematów z uprawnieniami do kontroli. Aby to ograniczyć, można użyć instrukcji DENY CONTROL.

Rozważmy na przykład dwa schematy bazy danych, schema_A dla działu A i schema_B dla działu B. Załóżmy, że użytkownicy bazy danych o nazwie user_A i user_B są użytkownikami ładującymi technologii PolyBase w działach A i B. Obaj użytkownicy otrzymali uprawnienia CONTROL do bazy danych. Osoby, które utworzyły schematy A i B, teraz blokują swoje schematy przy użyciu instrukcji DENY:

```sql
   DENY CONTROL ON SCHEMA :: schema_A TO user_B;
   DENY CONTROL ON SCHEMA :: schema_B TO user_A;
```   

Użytkownicy user_A i user_B nie mają teraz dostępu do schematu drugiego działu.


## <a name="load-to-a-staging-table"></a>Ładowanie do tabeli przejściowej

Aby przyspieszyć ładowanie, załaduj dane do tabeli przejściowej sterty przy użyciu działania okrężnego. Jest to najbardziej wydajny sposób na przeniesienie danych z warstwy usługi Azure Storage do usługi SQL Data Warehouse.

Planując duże zadanie ładowania, przeskaluj magazyn danych w górę.

Aby zapewnić optymalną wydajność ładowania, uruchamiaj tylko jedno zadanie ładowania jednocześnie

### <a name="optimize-columnstore-index-loads"></a>Optymalizowanie ładowania do indeksu magazynu kolumn

Indeksy magazynu kolumn wymagają dużej ilości pamięci w celu skompresowania danych w grupy wierszy wysokiej jakości. Najlepsza wydajność kompresji i indeksu występuje wówczas, gdy indeks magazynu kolumn kompresuje 1 048 576 wierszy w każdej grupie wierszy. Jest to maksymalna liczba wierszy na grupę wierszy. Jeśli pamięci jest za mało, indeks magazynu kolumn może nie osiągać maksymalnej szybkości kompresji. To z kolei wpływa na wydajność zapytań. Aby zapoznać się ze szczegółowymi informacjami, zobacz [Columnstore memory optimizations (Optymalizowanie pamięci na potrzeby magazynu kolumn)](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

- Aby mieć pewność, że użytkownik ładujący ma wystarczająco dużo pamięci do uzyskania maksymalnej szybkości kompresji, korzystaj z użytkowników ładujących będących członkami średnich lub dużych klas zasobów. 
- Załaduj wystarczającą liczbę wierszy, aby całkowicie wypełnić nowe grupy wierszy. Podczas ładowania zbiorczego poszczególne grupy po 1 048 576 wierszy są przekazywane bezpośrednio do magazynu kolumn. Jeśli zadanie ładowania obejmuje mniej niż 102 400 wierszy, wiersze są wysyłane do magazynu delta i przechowywane w indeksie klastrowanym do momentu zebrania wystarczającej liczby wierszy do skompresowania. W przypadku załadowania zbyt małej liczby wierszy wszystkie wiersze mogą zostać przekazane do magazynu delta, a nie natychmiast skompresowane do formatu magazynu kolumn.


### <a name="handling-loading-failures"></a>Postępowanie w przypadku błędów ładowania

Ładowanie z użyciem tabeli zewnętrznej może zakończyć się wystąpieniem błędu *Zapytanie zostało przerwane — osiągnięto maksymalny próg odrzuceń podczas odczytu ze źródła zewnętrznego*. To wskazuje, że dane zewnętrzne zawierają *zanieczyszczone* rekordy. Rekord danych jest uznawany za „zanieczyszczony”, jeśli rzeczywiste typy danych i liczba kolumn nie odpowiadają definicjom kolumn w tabeli zewnętrznej lub jeśli dane nie są zgodne z określonym zewnętrznym formatem pliku. 

Aby rozwiązać ten problem, upewnij się, że definicje tabeli zewnętrznej i zewnętrznego formatu pliku są prawidłowe, a dane zewnętrzne są zgodne z tymi definicjami. Jeśli podzestaw rekordów danych zewnętrznych jest zanieczyszczony, możesz odrzucić te rekordy w zapytaniach, używając opcji odrzucania w kodzie DDL CREATE EXTERNAL TABLE.



## <a name="insert-data-into-production-table"></a>Wstawianie danych do tabeli produkcyjnej
Poniżej przedstawiono zalecenia dotyczące wstawiania wierszy do tabel produkcyjnych.


### <a name="batch-insert-statements"></a>Zbiorcze instrukcje INSERT
Jednorazowe ładowanie do małej tabeli za pomocą [instrukcji INSERT](/sql/t-sql/statements/insert-transact-sql.md) lub nawet okresowe ponowne ładowanie wyszukiwania to strategie, które mogą sprawdzić się w przypadku instrukcji takich jak `INSERT INTO MyLookup VALUES (1, 'Type 1')`.  Pojedyncze wstawienia są mniej wydajne niż ładowanie zbiorcze. 

Jeśli w ciągu dnia wykonujesz kilka tysięcy lub więcej wstawień, utwórz partię wstawień i załaduj je zbiorczo.  Opracuj proces dołączania pojedynczych wstawień do pliku, a następnie kolejny proces okresowo ładujący ten plik.

### <a name="create-statistics-after-the-load"></a>Tworzenie statystyk po załadowaniu

Aby poprawić wydajność zapytań, należy utworzyć statystyki dla wszystkich kolumn wszystkich tabel po pierwszym załadowaniu danych, a następnie po każdej istotnej zmianie danych.  Aby zapoznać się ze szczegółowym opisem statystyk, zobacz temat [Statystyki][Statystyki]. Poniższy przykład umożliwia utworzenie statystyk dotyczących pięciu kolumn tabeli Customer_Speed.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="rotate-storage-keys"></a>Rotacja kluczy magazynu
Zalecanym rozwiązaniem w zakresie zabezpieczeń jest regularne zmienianie klucza dostępu do usługi Blob Storage. Z kontem usługi Blob Storage powiązane są dwa klucze magazynu. To umożliwia przenoszenie kluczy.

Aby przeprowadzić rotację kluczy konta usługi Azure Storage:

1. Utwórz drugie poświadczenie o zakresie bazy danych na podstawie pomocniczego klucza dostępu do magazynu.
2. Utwórz drugie zewnętrzne źródło danych na podstawie tego nowego poświadczenia.
3. Usuń tabele zewnętrzne i utwórz nowe, wskazujące nowe zewnętrzne źródła danych. 

Po przeprowadzeniu migracji tabel zewnętrznych do nowego źródła danych wykonaj następujące zadania oczyszczania:

1. Usuń pierwsze zewnętrzne źródło danych.
2. Usuń pierwsze poświadczenie o zakresie bazy danych utworzone na podstawie podstawowego klucza dostępu do magazynu.
3. Zaloguj się do platformy Azure i ponownie wygeneruj podstawowy klucz dostępu, aby był gotowy do kolejnej rotacji.


## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się, jak monitorować proces ładowania, zobacz [Monitor your workload using DMVs (Monitorowanie obciążenia przy użyciu widoków DMV)](sql-data-warehouse-manage-monitor.md).



