---
title: "Użyj MapReduce i Curl z platformą Hadoop w usłudze HDInsight - Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zdalne uruchamianie zadań MapReduce z Hadoop w usłudze HDInsight przy użyciu programu Curl."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: bc6daf37-fcdc-467a-a8a8-6fb2f0f773d1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: larryfr
ms.openlocfilehash: dd3e5904ee21ee74da5adaa65abd7865a82c8b36
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a>Uruchamianie zadań MapReduce z Hadoop w usłudze HDInsight za pomocą usługi REST

Dowiedz się, jak za pomocą interfejsu API REST usługi WebHCat do uruchomienia zadań MapReduce na platformie Hadoop w klastrze usługi HDInsight. Zwinięcie służy do pokazują, jak można interakcję z usługą HDInsight przy użyciu pierwotne żądania HTTP do uruchomienia zadań MapReduce.

> [!NOTE]
> Jeśli znasz już przy użyciu serwerów opartych na systemie Linux platformą Hadoop, ale jesteś nowym użytkownikiem usługi HDInsight, zobacz [co należy wiedzieć o opartych na systemie Linux platformą Hadoop w HDInsight](../hdinsight-hadoop-linux-information.md) dokumentu.


## <a id="prereq"></a>Wymagania wstępne

* Usługi w klastrze usługi HDInsight Hadoop
* [Narzędzie curl](http://curl.haxx.se/)
* [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Uruchamianie zadań MapReduce przy użyciu programu Curl

> [!NOTE]
> Gdy używasz Curl lub innego połączenia REST z usługą WebHCat, wymagane jest uwierzytelnienie żądania przez podanie nazwy użytkownika administratora klastra usługi HDInsight i hasła. Należy użyć nazwy klastra jako część identyfikatora URI, który służy do wysyłania żądań do serwera.
>
> Dla poleceń w tej sekcji należy zastąpić **admin** z użytkownikiem w celu uwierzytelniania w klastrze. Zastąp ciąg **CLUSTERNAME** nazwą klastra. Po wyświetleniu monitu podaj hasło dla konta użytkownika.
>
> Interfejs API REST jest zabezpieczony za pomocą [uwierzytelniania podstawowego dostępu](http://en.wikipedia.org/wiki/Basic_access_authentication). Należy zawsze tworzyć żądania przy użyciu protokołu HTTPS, aby upewnić się, że poświadczenia są bezpiecznie wysyłane do serwera.


1. W wierszu polecenia wpisz następujące polecenie, aby sprawdzić możliwość nawiązania połączenia z klastrem usługi HDInsight:

    ```bash
    curl -u admin -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Pojawi się odpowiedź podobną do poniższej JSON:

        {"status":"ok","version":"v1"}

    W tym poleceniu są używane następujące parametry:

   * **-u**: Określa nazwę użytkownika i hasło używane do uwierzytelniania żądania
   * **-G**: oznacza, że ta operacja jest żądanie pobrania

   Początek identyfikatora URI **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, jest taka sama dla wszystkich żądań.

2. Aby przesłać zadania MapReduce, użyj następującego polecenia:

    ```bash
    curl -u admin -d user.name=admin -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/CurlOut https://CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar
    ```

    Koniec identyfikatora URI (/ mapreduce/jar) określa, że usługi WebHCat to żądanie rozpoczęcia zadania MapReduce z klasy w pliku jar. W tym poleceniu są używane następujące parametry:

   * **-d**: `-G` nie jest używana, więc domyślnie żądania metody POST. `-d`Określa dane, które są wysyłane z żądania.
    * **User.name**: użytkownik, który uruchamia polecenie
    * **JAR**: uruchomiono lokalizacji pliku jar, który zawiera klasę jako
    * **Klasa**: klasa, która zawiera logikę MapReduce
    * **ARG**: argumenty do przekazania do zadania MapReduce. W tym przypadku, pliku wejściowego tekstu i katalog, w którym są używane dla danych wyjściowych

   To polecenie powinny zostać zwrócone identyfikator zadania, który może służyć do sprawdzania stanu zadania:

       {"id":"job_1415651640909_0026"}

3. Aby sprawdzić stan zadania, użyj następującego polecenia:

    ```bash
    curl -G -u admin -d user.name=admin https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Zastąp **JOBID** o wartości zwracanej w poprzednim kroku. Na przykład, jeśli była zwracana wartość `{"id":"job_1415651640909_0026"}`, następnie będzie identyfikator JOBID `job_1415651640909_0026`.

    Jeśli zadanie zostało zakończone, zwracany jest stan `SUCCEEDED`.

   > [!NOTE]
   > To żądanie Curl zwraca informacje o zadaniu dokumentu JSON. Jq służy do pobierania wartości stan.

4. Gdy stan zadania został zmieniony na `SUCCEEDED`, wyniki zadania można pobrać z magazynu obiektów Blob platformy Azure. `statusdir` Parametr przekazywany z zapytaniem zawiera lokalizację pliku wyjściowego. W tym przykładzie jest lokalizacja `/example/curl`. Ten adres przechowuje dane wyjściowe zadania w magazynie domyślne klastrów w `/example/curl`.

Można wyświetlić listę i takie pliki należy pobierać za pomocą [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Aby uzyskać więcej informacji na temat pracy z obiektami blob z wiersza polecenia platformy Azure, zobacz [przy użyciu 2.0 interfejsu wiersza polecenia platformy Azure z usługą Azure Storage](../../storage/common/storage-azure-cli.md#create-and-manage-blobs) dokumentu.

## <a id="nextsteps"></a>Następne kroki

Aby uzyskać ogólne informacje na temat zadań MapReduce w usłudze HDInsight:

* [Używanie MapReduce z usługą Hadoop w usłudze HDInsight](hdinsight-use-mapreduce.md)

Aby uzyskać informacje o innych metodach pracy z platformą Hadoop w usłudze HDInsight:

* [Korzystanie z programu Hive z usługą Hadoop w usłudze HDInsight](hdinsight-use-hive.md)
* [Korzystanie z języka Pig z usługą Hadoop w usłudze HDInsight](hdinsight-use-pig.md)

Aby uzyskać więcej informacji na temat interfejsu REST, który jest używany w tym artykule, zobacz [odwołania WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).