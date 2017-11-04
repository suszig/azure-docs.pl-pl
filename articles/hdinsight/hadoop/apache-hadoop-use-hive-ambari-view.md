---
title: "Używanie widoków Ambari do pracy z Hive w usłudze Azure HDInsight (Hadoop) - | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak Hive View z przeglądarki sieci web umożliwia wysyłanie zapytań programu Hive. Widok Hive jest częścią interfejsie użytkownika sieci Web Ambari, które zostały dostarczone z klastrem usługi HDInsight opartej na systemie Linux."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1abe9104-f4b2-41b9-9161-abbc43de8294
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/23/2017
ms.author: larryfr
ms.openlocfilehash: 8293da8c77725d051f295826d9a78bf81055dcb3
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="use-ambari-hive-view-with-hadoop-in-hdinsight"></a>Użyj widoku Hive narzędzia Ambari z platformą Hadoop w usłudze HDInsight

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Dowiedz się, jak uruchamianie zapytań Hive przy użyciu widoku Hive narzędzia Ambari. Ambari to zarządzania i monitorowania narzędzia dostarczanego z klastrami HDInsight opartych na systemie Linux. Jednym z funkcje oferowane przez Ambari jest interfejsem użytkownika sieci Web, który może służyć do uruchamiania zapytań Hive.

> [!NOTE]
> Ambari ma wiele funkcji, które nie zostały omówione w tym dokumencie. Aby uzyskać więcej informacji, zobacz [Zarządzanie klastrami usługi HDInsight przy użyciu interfejsu użytkownika sieci Web Ambari](../hdinsight-hadoop-manage-ambari.md).

## <a name="prerequisites"></a>Wymagania wstępne

* Klaster usługi HDInsight opartej na systemie Linux. Aby uzyskać informacje dotyczące tworzenia klastrów, zobacz [rozpocząć korzystanie z platformy Hadoop w usłudze HDInsight](apache-hadoop-linux-tutorial-get-started.md).

> [!IMPORTANT]
> Kroki opisane w tym dokumencie wymagają klastra usługi Azure HDInsight, która używa systemu Linux. Linux jest tylko system operacyjny używany w usłudze HDInsight w wersji 3.4 lub nowszej. Aby uzyskać więcej informacji, zobacz sekcję [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Wycofanie usługi HDInsight w systemie Windows).

## <a name="open-the-hive-view"></a>Otwórz widok gałęzi

Widoki Ambari można otworzyć z portalu Azure. Wybierz z klastrem usługi HDInsight, a następnie wybierz **widoków Ambari** z **szybkie linki** sekcji.

![Szybkie linki części portalu](./media/apache-hadoop-use-hive-ambari-view/quicklinks.png)

Wybierz z listy widoków, __Hive View__.

![Wybrany widok gałęzi](./media/apache-hadoop-use-hive-ambari-view/select-hive-view.png)

> [!NOTE]
> Gdy uzyskujesz dostęp do narzędzia Ambari, zostanie wyświetlony monit o uwierzytelniania do lokacji. Wprowadź administratora (domyślna `admin`) konta i hasło, które zostały użyte podczas tworzenia klastra.

Należy wyświetlić stronę podobną do poniższej ilustracji:

![Obraz arkusza kwerend widoku Hive](./media/apache-hadoop-use-hive-ambari-view/ambari-hive-view.png)

## <a name="run-a-query"></a>Uruchamianie zapytania

Aby uruchomić zapytanie Hive, wykonaj następujące kroki w widoku Hive.

1. Z __zapytania__ karcie, wklej poniższe instrukcje HiveQL do arkusza:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;
    ```

    Te instrukcje, wykonaj następujące czynności:

   * `DROP TABLE`: Usuwa tabeli i plik danych w przypadku, gdy tabela już istnieje.

   * `CREATE EXTERNAL TABLE`: Tworzy nową tabelę "external" w gałęzi.
   Tabele zewnętrzne przechowywać tylko definicji tabeli w gałęzi. Dane pozostaną w oryginalnej lokalizacji.

   * `ROW FORMAT`: Pokazuje, jak dane są sformatowane. W takim przypadku pól w każdym dzienniku są oddzielone spacją.

   * `STORED AS TEXTFILE LOCATION`: Zawiera przechowywania danych i że jest przechowywane jako tekst.

   * `SELECT`: Wybiera liczbę wszystkich wierszy, gdzie t4 kolumna zawiera wartość [Błąd].

     > [!NOTE]
     > Użyj tabel zewnętrznych, jeśli potrzebujesz danych do zaktualizowania przez źródło zewnętrzne, takie jak dane automatyczne przekazywanie procesu lub inna operacja MapReduce. Usunięcie tabeli zewnętrznej jest *nie* Usuń dane, definicję tabeli.

    > [!IMPORTANT]
    > Pozostaw __bazy danych__ zaznaczenie na __domyślne__. Przykłady w tym dokumencie Użyj domyślnej bazy danych uwzględnionych w usłudze HDInsight.

2. Aby uruchomić zapytanie, należy użyć **Execute** znajdujący się poniżej arkusza. Przycisk zmieni kolor pomarańczowy, a tekst zostanie zmieniony na **zatrzymać**.

3. Po zakończeniu zapytanie **wyniki** karcie są wyświetlane wyniki operacji. Następujący tekst jest wynik kwerendy:

        sev       cnt
        [ERROR]   3

    Można użyć **dzienniki** kartę, aby wyświetlić informacje o rejestrowaniu utworzenia zadania.

   > [!TIP]
   > Pobierz lub zapisać wyniki z **Zapisz wyniki** okno dialogowe listy rozwijanej w prawym górnym rogu **wyniki przetwarzania zapytania** sekcji.

4. Wybierz pierwsze cztery wiersze tego zapytania, a następnie wybierz **Execute**. Zwróć uwagę, że nie ma żadnych wyników po zakończeniu zadania. Przy użyciu **Execute** przycisku po wybraniu części zapytania działa tylko wybrane instrukcje. W takim przypadku zaznaczenie nie włączono końcowej instrukcji, która pobiera wiersze z tabeli. Jeśli wybierzesz tylko ten wiersz i użyj **Execute**, powinny pojawić się oczekiwanych rezultatów.

5. Aby dodać arkusz, należy użyć **nowy arkusz** przycisk w dolnej części **edytora zapytań**. W nowym arkuszu wprowadź poniższe instrukcje HiveQL:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';
    ```

  Te instrukcje, wykonaj następujące czynności:

   * **Utwórz Tabela Jeśli nie ISTNIEJE**: tworzy tabelę, jeśli już nie istnieje. Ponieważ **zewnętrznych** — słowo kluczowe nie jest używana, jest tworzony wewnętrznej tabeli. Wewnętrznej tabeli są przechowywane w magazynie danych programu Hive i zarządza całkowicie Hive. W przeciwieństwie do tabel zewnętrznych, upuszczając wewnętrznej tabeli usuwa danych źródłowych.

   * **ORC AS PRZECHOWYWANE**: przechowuje dane w formacie zoptymalizowanych pod kątem wiersza kolumnowy (ORC). ORC jest wysoce zoptymalizowane i wydajne formatu do przechowywania danych gałęzi.

   * **ZASTĄP INSERT... Wybierz**: wybiera wiersze z **log4jLogs** tabeli, która zawiera `[ERROR]`, a następnie wstawia dane do **errorLogs** tabeli.

Użyj **Execute** przycisk, aby uruchomić to zapytanie. **Wyniki** karta nie zawiera żadnych informacji, gdy kwerenda zwraca zero wierszy. Stan powinny być widoczne jako **zakończyło się pomyślnie** po zakończeniu działania zapytania.

### <a name="visual-explain"></a>Wyjaśnić Visual

Aby wyświetlić wizualizacji planu zapytania, zaznacz **wyjaśnić Visual** kartę poniżej arkusza.

**Wyjaśnić Visual** widoku zapytania mogą być pomocne w opis przepływu złożonych zapytań. Odpowiednika tekstową tego widoku można wyświetlić za pomocą **Wyjaśnij** przycisk w edytorze zapytań.

### <a name="tez-ui"></a>Interfejs użytkownika aplikacji tez

Aby wyświetlić interfejs użytkownika aplikacji Tez dla zapytania, zaznacz **Tez** kartę poniżej arkusza.

> [!IMPORTANT]
> Tez nie jest używany do rozpoznawania wszystkich zapytań. Wiele zapytań można rozwiązać, bez korzystania z aplikacji Tez. 

Jeśli Tez użyte do rozpoznania zapytania, zostanie wyświetlony skierowany acykliczne Graph (DAG). Jeśli chcesz wyświetlić grupy DAG dla zapytań, które było wykonywane w ciągu ostatnich lub jeśli chcesz debugować proces aplikacji Tez, użyj [widoku Tez](../hdinsight-debug-ambari-tez-view.md) zamiast tego.

## <a name="view-job-history"></a>Wyświetlanie historii zadań

__Zadania__ kartę Wyświetla historię zapytań programu Hive.

![Obraz w historii zadań](./media/apache-hadoop-use-hive-ambari-view/job-history.png)

## <a name="database-tables"></a>Tabele bazy danych

Można użyć __tabel__ kartę, aby pracować z tabelami w bazie danych Hive.

![Obraz na karcie tabel](./media/apache-hadoop-use-hive-ambari-view/tables.png)

## <a name="saved-queries"></a>Zapisane kwerendy

Z **zapytania** kartę, można opcjonalnie zapisać zapytania. Po zapisaniu kwerendy, można użyć ponownie z __zapisane kwerendy__ kartę.

![Obraz karty zapisane kwerendy](./media/apache-hadoop-use-hive-ambari-view/saved-queries.png)

## <a name="user-defined-functions"></a>Funkcje zdefiniowane przez użytkownika

Można również rozszerzyć zasięg Hive za pomocą funkcji zdefiniowanej przez użytkownika (UDF). Użyj funkcji zdefiniowanej przez użytkownika do implementacji funkcji lub logikę, która nie jest łatwo zgodnie z modelem w HiveQL.

Deklarowanie i Zapisywanie zestawu funkcji UDF przy użyciu **UDF** u góry widoku Hive. Te funkcje UDF mogą być używane z **edytora zapytań**.

![Obraz karty funkcji zdefiniowanej przez użytkownika](./media/apache-hadoop-use-hive-ambari-view/user-defined-functions.png)

Po dodaniu funkcji zdefiniowanej przez użytkownika do widoku Hive **Wstaw funkcje UDF** pojawi się przycisk w dolnej części **edytora zapytań**. Wybranie tego wpisu powoduje wyświetlenie listy rozwijanej funkcji UDF, zdefiniowane w widoku Hive. Wybieranie funkcji zdefiniowanej przez użytkownika dodaje instrukcje HiveQL do zapytania, aby włączyć funkcję zdefiniowaną przez użytkownika.

Na przykład, jeśli zdefiniowano UDF z następującymi właściwościami:

* Nazwa zasobu: myudfs

* Ścieżka zasobu: /myudfs.jar

* Nazwa funkcji zdefiniowanej przez użytkownika: myawesomeudf

* Nazwa klasy funkcji zdefiniowanej przez użytkownika: com.myudfs.Awesome

Przy użyciu **Wstaw funkcje UDF** przycisk zawiera wpis o nazwie **myudfs**, z innej listy rozwijanej dla każdej funkcji zdefiniowanej przez użytkownika zdefiniowane dla tego zasobu. W takim przypadku jest **myawesomeudf**. Wybranie tego wpisu dodaje następujące na początku zapytania:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

Następnie można UDF w zapytaniu. Na przykład `SELECT myawesomeudf(name) FROM people;`.

Aby uzyskać więcej informacji o korzystaniu z funkcji UDF z Hive w usłudze HDInsight zobacz następujące artykuły:

* [Przy użyciu języka Python z Hive i Pig w usłudze HDInsight](python-udf-hdinsight.md)
* [Jak dodać niestandardowe UDF gałęzi do usługi HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a>Gałąź, ustawienia

Można zmienić różne ustawienia Hive, takie jak zmiany aparat wykonywania programu Hive w aplikacji Tez (ustawienie domyślne) do MapReduce.

## <a id="nextsteps"></a>Następne kroki

Ogólne informacje na temat Hive w usłudze HDInsight:

* [Korzystanie z programu Hive z usługą Hadoop w usłudze HDInsight](hdinsight-use-hive.md)

Aby uzyskać informacje o innych metodach pracy z platformą Hadoop w usłudze HDInsight:

* [Korzystanie z języka Pig z usługą Hadoop w usłudze HDInsight](hdinsight-use-pig.md)
* [Używanie MapReduce z usługą Hadoop w usłudze HDInsight](hdinsight-use-mapreduce.md)
