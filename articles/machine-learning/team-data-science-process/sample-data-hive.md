---
title: "Przykładowe dane w tabelach platformy Azure HDInsight Hive | Dokumentacja firmy Microsoft"
description: "Dół próbkowania dane w tabelach Hive w usłudze Azure HDInsight (Hadopop)"
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: f31e8d01-0fd4-4a10-b1a7-35de3c327521
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 357307a034b277e8c37e99bda1ed6a9a76e13f41
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="sample-data-in-azure-hdinsight-hive-tables"></a>Przykładowe dane w tabelach usługi Azure HDInsight Hive
W tym artykule firma Microsoft opisują sposób dół przykładowe dane przechowywane w tabelach platformy Azure HDInsight Hive za pomocą zapytań Hive. Firma Microsoft obejmuje trzy metody pobierania próbek popularly używany:

* Jednolite losowego pobierania próbek
* Losowe próbkowania według grup
* Stratyfikowana pobierania próbek

Następujące **menu** linki do tematów opisujących sposób przykładowe dane z różnych środowiskach magazynu.

[!INCLUDE [cap-sample-data-selector](../../../includes/cap-sample-data-selector.md)]

**Dlaczego przykładowe dane?**
Jeśli zestaw danych, które mają być analizowanie jest duży, zazwyczaj jest dobrym rozwiązaniem w dół przykładowych danych, aby zmniejszyć jego rozmiar mniejsze, ale reprezentatywny i łatwiejsze w zarządzaniu. To ułatwia zrozumienie danych, badanie i inżynieria funkcji. Swoją rolę w procesie nauki danych zespołu jest umożliwienie szybkiego prototypy funkcji przetwarzania danych i modeli uczenia maszynowego.

To zadanie próbkowania jest krokiem w [zespołu danych nauki procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="how-to-submit-hive-queries"></a>Temat dotyczący przesyłania zapytań programu Hive
W konsoli usługi Hadoop wiersza polecenia na węzła głównego klastra usługi Hadoop można przesłać zapytań programu hive. Aby to zrobić, zaloguj się do węzła głównego klastra usługi Hadoop, otwórz konsolę wiersza polecenia platformy Hadoop i wysyłanie zapytań programu Hive z tego miejsca. Aby uzyskać instrukcje dotyczące przesyłania zapytań programu Hive w konsoli usługi Hadoop wiersza polecenia, zobacz [sposobu przesyłania zapytań Hive](move-hive-tables.md#submit).

## <a name="uniform"></a>Jednolite losowego pobierania próbek
Jednolite próbkowania losowe oznacza, że każdego wiersza w zestawie danych ma taki sam sposób przez cały czas jest próbkowany. To może być zaimplementowany przez dodanie rand() dodatkowe pola zestawu danych w wewnętrznym zapytania "select" i w zapytaniu "Wybierz" zewnętrzne tego warunku na losowe pola.

Poniżej przedstawiono przykładowe zapytanie:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, …, fieldN
    from
        (
        select
            field1, field2, …, fieldN, rand() as samplekey
        from <hive table name>
        )a
    where samplekey<='${hiveconf:sampleRate}'

W tym miejscu `<sample rate, 0-1>` określa część rekordów, które użytkownicy mają do próbkowania.

## <a name="group"></a>Losowe próbkowania według grup
Podczas pobierania próbek danych podzielone na kategorie, warto uwzględnić lub wykluczyć wszystkie wystąpienia niektórych określonej wartości zmiennej podzielone na kategorie. Jest to, co oznacza "próbkowania przez grupę".
Na przykład jeśli zmienna kategorii "Stan", która zawiera wartości NY MA, urząd certyfikacji, NJ, PA, itp, mają rekordy z takim samym stanie zawsze być ze sobą, czy są próbkowane lub nie.

Poniżej przedstawiono przykładowe zapytanie tej próbki przez grupę:

    SET sampleRate=<sample rate, 0-1>;
    select
        b.field1, b.field2, …, b.catfield, …, b.fieldN
    from
        (
        select
            field1, field2, …, catfield, …, fieldN
        from <table name>
        )b
    join
        (
        select
            catfield
        from
            (
            select
                catfield, rand() as samplekey
            from <table name>
            group by catfield
            )a
        where samplekey<='${hiveconf:sampleRate}'
        )c
    on b.catfield=c.catfield

## <a name="stratified"></a>Stratyfikowana pobierania próbek
Losowe próbkowania jest uporządkować względem zmienną podzielone na kategorie gdy próbki otrzymane wartości czy podzielone na kategorie które znajdują się w tej samej stosunek jak populacji nadrzędnej, z którego uzyskano próbek. W tym samym przykładzie jako powyżej, załóżmy, że dane mają grupy przez Państwa, co oznacza NJ ma 100 uwagi, NY ma 60 uwag, a WA ma 300 uwag. Jeśli określisz częstotliwość próbkowania stratyfikowana jako 0,5, następnie otrzymaną próbkę ma około 50, 30 i 150 obserwacji NJ, NY i WA odpowiednio.

Poniżej przedstawiono przykładowe zapytanie:

    SET sampleRate=<sample rate, 0-1>;
    select
        field1, field2, field3, ..., fieldN, state
    from
        (
        select
            field1, field2, field3, ..., fieldN, state,
            count(*) over (partition by state) as state_cnt,
              rank() over (partition by state order by rand()) as state_rank
          from <table name>
        ) a
    where state_rank <= state_cnt*'${hiveconf:sampleRate}'


Aby uzyskać informacje na bardziej zaawansowane metody pobierania próbek, które są dostępne w gałęzi, zobacz [LanguageManual próbkowania](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Sampling).

