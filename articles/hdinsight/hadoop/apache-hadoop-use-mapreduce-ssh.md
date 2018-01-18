---
title: "MapReduce i SSH połączenia z platformą Hadoop w usłudze HDInsight - Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać SSH do uruchomienia zadań MapReduce przy użyciu platformy Hadoop w usłudze HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 844678ba-1e1f-4fda-b9ef-34df4035d547
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2018
ms.author: larryfr
ms.openlocfilehash: 08c67168cde60c1b7551806da8542c98dcb82f55
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>Używanie MapReduce z usługą Hadoop w usłudze HDInsight przy użyciu protokołu SSH

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Dowiedz się, jak można przesłać zadania MapReduce z połączenia protokołu Secure Shell (SSH) do usługi HDInsight.

> [!NOTE]
> Jeśli znasz już przy użyciu serwerów opartych na systemie Linux platformą Hadoop, ale jesteś nowym użytkownikiem usługi HDInsight, zobacz [porady HDInsight opartych na systemie Linux](../hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Wymagania wstępne

* Klaster opartych na systemie Linux usługą HDInsight (Hadoop w usłudze HDInsight)

  > [!IMPORTANT]
  > Linux jest jedynym systemem operacyjnym używanym w połączeniu z usługą HDInsight w wersji 3.4 lub nowszą. Aby uzyskać więcej informacji, zobacz sekcję [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Wycofanie usługi HDInsight w systemie Windows).

* Klient SSH. Aby uzyskać więcej informacji, zobacz [używanie SSH z usługą HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md)

## <a id="ssh"></a>Połącz przy użyciu protokołu SSH

Połącz z klastrem przy użyciu protokołu SSH. Na przykład następujące polecenie nawiązuje połączenie z klastra o nazwie **myhdinsight**:

```bash
ssh admin@myhdinsight-ssh.azurehdinsight.net
```

**Jeśli używasz klucza certyfikatu dla uwierzytelniania SSH**, może być konieczne na przykład określ lokalizację klucza prywatnego w systemie klienta:

```bash
ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net
```

**Jeśli używasz hasła dla uwierzytelniania SSH**, musisz podać hasło po wyświetleniu monitu.

Aby uzyskać więcej informacji o korzystaniu z protokołu SSH z usługą HDInsight, zobacz [używanie SSH z usługą HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="hadoop"></a>Użyj poleceń usługi Hadoop

1. Po połączeniu się z klastrem usługi HDInsight, użyj następującego polecenia, można uruchomić zadania MapReduce:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    To polecenie uruchamia `wordcount` klasy, która jest zawarta w `hadoop-mapreduce-examples.jar` pliku. Używa `/example/data/gutenberg/davinci.txt` dokumentu jako dane wejściowe i wyjściowe są przechowywane w `/example/data/WordCountOutput`.

    > [!NOTE]
    > Aby uzyskać więcej informacji dotyczących tego zadania MapReduce i przykładowe dane, zobacz [MapReduce używany w Hadoop w usłudze HDInsight](hdinsight-use-mapreduce.md).

2. Zadanie emituje szczegóły przetwarzania i zwraca informacje podobne do następującego po zakończeniu zadania:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Po zakończeniu zadania, użyj następującego polecenia, aby wyświetlić listę plików wyjściowych:

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    To polecenie wyświetla dwa pliki `_SUCCESS` i `part-r-00000`. `part-r-00000` Plik zawiera dane wyjściowe dla tego zadania.

    > [!NOTE]
    > Niektóre zadania MapReduce mogą podzielone wyniki w wielu **części r-###** plików. Jeśli tak, użyj ### przyrostka, aby wskazać kolejność plików.

4. Aby wyświetlić dane wyjściowe, użyj następującego polecenia:

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    To polecenie wyświetla listę słów, które są zawarte w **wasb://example/data/gutenberg/davinci.txt** plików i liczba wystąpił każdego wyrazu. Przykładem danych, który jest zawarty w pliku jest następujący tekst:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Podsumowanie

Jak widać, polecenia Hadoop umożliwiają łatwe do uruchomienia zadań MapReduce w klastrze usługi HDInsight, a następnie Wyświetl dane wyjściowe zadania.

## <a id="nextsteps"></a>Następne kroki

Aby uzyskać ogólne informacje na temat zadań MapReduce w usłudze HDInsight:

* [Korzystać z usługi MapReduce na HDInsight Hadoop](hdinsight-use-mapreduce.md)

Aby uzyskać informacje o innych metodach pracy z platformą Hadoop w usłudze HDInsight:

* [Korzystanie z programu Hive z usługą Hadoop w usłudze HDInsight](hdinsight-use-hive.md)
* [Korzystanie z języka Pig z usługą Hadoop w usłudze HDInsight](hdinsight-use-pig.md)
