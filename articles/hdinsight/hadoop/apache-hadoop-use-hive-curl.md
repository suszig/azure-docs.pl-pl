---
title: "Hadoop Hive za pomocą Curl w usłudze HDInsight - Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zdalnie przesyłania zadań Pig do usługi HDInsight przy użyciu programu Curl."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6ce18163-63b5-4df6-9bb6-8fcbd4db05d8
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/04/2017
ms.author: larryfr
ms.openlocfilehash: b05dbdcec3cfb5c78115061567bb6229623cd0ff
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="run-hive-queries-with-hadoop-in-hdinsight-using-rest"></a>Uruchamianie zapytań Hive z usługą Hadoop w usłudze HDInsight za pomocą usługi REST

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Dowiedz się, jak uruchamianie zapytań Hive z usługą Hadoop w usłudze Azure HDInsight klastra za pomocą interfejsu API REST usługi WebHCat.

[Curl](http://curl.haxx.se/) służy do pokazują, jak można interakcję z usługą HDInsight przy użyciu pierwotne żądania HTTP. [Jq](http://stedolan.github.io/jq/) narzędzie służy do przetwarzania danych JSON zwróconych z pozostałych żądań.

> [!NOTE]
> Jeśli znasz już przy użyciu serwerów opartych na systemie Linux platformą Hadoop, ale dopiero zaczynasz korzystać z usługi HDInsight, zobacz [co należy wiedzieć o Hadoop w HDInsight opartych na systemie Linux](../hdinsight-hadoop-linux-information.md) dokumentu.

## <a id="curl"></a>Uruchamianie zapytań Hive

> [!NOTE]
> Po użyciu cURL lub innego połączenia REST z usługą WebHCat, musi uwierzytelnić żądania, podając nazwę użytkownika i hasło administratora klastra usługi HDInsight.
>
> Dla poleceń w tej sekcji należy zastąpić **admin** z użytkownikiem w celu uwierzytelniania w klastrze. Zastąp ciąg **CLUSTERNAME** nazwą klastra. Po wyświetleniu monitu wprowadź hasło dla konta użytkownika.
>
> Interfejs API REST jest zabezpieczony za pomocą [uwierzytelniania podstawowego](http://en.wikipedia.org/wiki/Basic_access_authentication). Aby upewnić się, że poświadczenia są bezpiecznie wysyłane do serwera, należy zawsze tworzyć żądania przy użyciu protokołu HTTP Secure (HTTPS).

1. W wierszu polecenia wpisz następujące polecenie, aby sprawdzić możliwość nawiązania połączenia z klastrem usługi HDInsight:

    ```bash
    curl -u admin -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    Pojawi się odpowiedź podobna do następującego tekstu:

        {"status":"ok","version":"v1"}

    W tym poleceniu są używane następujące parametry:

    * **-u** — nazwa użytkownika i hasło używane do uwierzytelnienia żądania.
    * **-G** — wskazuje, że to żądanie jest operacji GET.

   Adres URL, na początek **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, jest taka sama dla wszystkich żądań. Ścieżka, **/status**, wskazuje, czy żądanie jest próbę zwrócenia stanu usługi WebHCat (znanej także jako Templeton) dla serwera. Możesz również poprosić o wersji programu Hive za pomocą następującego polecenia:

    ```bash
    curl -u admin -G https://CLUSTERNAME.azurehdinsight.net/templeton/v1/version/hive
    ```

    To żądanie zwraca odpowiedź podobny do następującego tekstu:

        {"module":"hive","version":"0.13.0.2.1.6.0-2103"}

2. Należy użyć następującego można utworzyć tabeli o nazwie **log4jLogs**:

    ```bash
    curl -u admin -d user.name=admin -d execute="set+hive.execution.engine=tez;DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    Używane z tym żądaniem następujące parametry:

   * **-d** — od `-G` nie jest używany domyślnie żądania metody POST. `-d`Określa dane, które są wysyłane z żądania.

     * **User.name** — użytkownik, który uruchamia polecenie.
     * **wykonanie** -instrukcje HiveQL do wykonania.
     * **statusdir** — stan tego zadania jest zapisywany w katalogu.

   Te instrukcje, wykonaj następujące czynności:
   
   * **DROP TABLE** — Jeśli tabela już istnieje, jest usuwany.
   * **Tworzenie tabeli zewnętrznej** — tworzy nową tabelę "zewnętrzne" w gałęzi. Tabele zewnętrzne przechowywać tylko definicji tabeli w gałęzi. Dane pozostaną w oryginalnej lokalizacji.

     > [!NOTE]
     > Jeśli oczekujesz zaktualizowania za pomocą zewnętrznego źródła danych, należy użyć tabel zewnętrznych. Na przykład procesu przekazywania danych lub inna operacja MapReduce.
     >
     > Usunięcie tabeli zewnętrznej jest **nie** Usuń dane, definicję tabeli.

   * **FORMAT wiersza** — jak dane są sformatowane. W każdym dzienniku pola są oddzielone spacją.
   * **PRZECHOWYWANE jako lokalizacji TEXTFILE** — przechowywania danych (katalog przykład/danych) i które są przechowywane jako tekst.
   * **Wybierz** -wybiera liczbę wszystkich wierszy gdzie kolumna **t4** zawiera wartość **[Błąd]**. Ta instrukcja zwraca wartość **3** są trzy wiersze, które zawierają tę wartość.

     > [!NOTE]
     > Należy zauważyć, że odstępy między instrukcje HiveQL są zastępowane przez `+` znak, gdy jest używany z Curl. Wartości w cudzysłowie, które zawierać spacji, takich jak ogranicznik, nie powinna zostać zastąpiona przez `+`.

   * **INPUT__FILE__NAME takich jak "% 25.log"** -niniejszych ogranicza wyszukiwanie do użycia tylko pliki kończy się rozszerzeniem. dziennika.

     > [!NOTE]
     > `%25` Jest formą zakodowane w adresie URL %, więc jest stan aktualny `like '%.log'`. % Musi być zakodowany, adres URL, ponieważ jest ona traktowana jako znak specjalny w adresach URL.

   To polecenie zwraca identyfikator zadania, który może służyć do sprawdzania stanu zadania.

       {"id":"job_1415651640909_0026"}

3. Aby sprawdzić stan zadania, użyj następującego polecenia:

    ```bash
    curl -G -u admin -d user.name=admin https://CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/JOBID | jq .status.state
    ```

    Zastąp **JOBID** o wartości zwracanej w poprzednim kroku. Na przykład, jeśli była zwracana wartość `{"id":"job_1415651640909_0026"}`, następnie **JOBID** będzie `job_1415651640909_0026`.

    Jeśli zadanie zostało zakończone, stan jest **zakończyło się pomyślnie**.

   > [!NOTE]
   > To żądanie Curl zwraca dokument JavaScript Object Notation (JSON), informacje o zadaniu. Jq służy do pobierania wartości stan.

4. Gdy stan zadania został zmieniony na **zakończyło się pomyślnie**, wyniki zadania można pobrać z magazynu obiektów Blob platformy Azure. `statusdir` Parametr przekazany z zapytaniem zawiera lokalizację pliku wyjściowego; w takim przypadku **/przykład/zwinięcie**. Ten adres przechowuje dane wyjściowe w **przykład/zwinięcie** katalogu w magazynie domyślne klastrów.

    Można wyświetlić listę i takie pliki należy pobierać za pomocą [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). Aby uzyskać więcej informacji na temat używania interfejsu wiersza polecenia Azure z usługą Azure Storage, zobacz [Użyj Azure CLI 2.0 z usługą Azure Storage](https://docs.microsoft.com/azure/storage/storage-azure-cli#create-and-manage-blobs) dokumentu.

5. Poniższe instrukcje umożliwiają utworzenie nowej tabeli "internal" o nazwie **errorLogs**:

    ```bash
    curl -u admin -d user.name=admin -d execute="set+hive.execution.engine=tez;CREATE+TABLE+IF+NOT+EXISTS+errorLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+STORED+AS+ORC;INSERT+OVERWRITE+TABLE+errorLogs+SELECT+t1,t2,t3,t4,t5,t6,t7+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log';SELECT+*+from+errorLogs;" -d statusdir="/example/curl" https://CLUSTERNAME.azurehdinsight.net/templeton/v1/hive
    ```

    Te instrukcje, wykonaj następujące czynności:

   * **Utwórz Tabela Jeśli nie ISTNIEJE** — tworzy tabelę, jeśli jeszcze nie istnieje. Ta instrukcja tworzy wewnętrznej tabeli, który jest przechowywany w magazynie danych programu Hive. Ta tabela jest zarządzana przez Hive.

     > [!NOTE]
     > W przeciwieństwie do tabel zewnętrznych porzucenie wewnętrznej tabeli powoduje usunięcie danych źródłowych.

   * **ORC AS PRZECHOWYWANE** -przechowuje dane w formacie zoptymalizowanych pod kątem wiersza kolumnowy (ORC). ORC jest wysoce zoptymalizowane i wydajne formatu do przechowywania danych gałęzi.
   * **ZASTĄP INSERT... Wybierz** -wybiera wiersze z **log4jLogs** tabeli, która zawiera **[Błąd]**, następnie wstawia dane do **errorLogs** tabeli.
   * **Wybierz** -wybiera wszystkie wiersze z nową **errorLogs** tabeli.

6. Za pomocą Identyfikatora zadanie zwracane w celu sprawdzenia stanu zadania. Po zakończyła się pomyślnie, użyj wiersza polecenia platformy Azure zgodnie z opisem wcześniej do pobrania i wyświetlenia wyników. Dane wyjściowe powinny zawierać trzy wiersze, które zawierają **[Błąd]**.

## <a id="nextsteps"></a>Następne kroki

Ogólne informacje na temat programu Hive z HDInsight:

* [Korzystanie z programu Hive z usługą Hadoop w usłudze HDInsight](hdinsight-use-hive.md)

Aby uzyskać informacje o innych metodach pracy z platformą Hadoop w usłudze HDInsight:

* [Korzystanie z języka Pig z usługą Hadoop w usłudze HDInsight](hdinsight-use-pig.md)
* [Używanie MapReduce z usługą Hadoop w usłudze HDInsight](hdinsight-use-mapreduce.md)

Jeśli używasz aplikacji Tez przy użyciu Hive, zobacz informacji o debugowaniu w następujących dokumentach:

* [Użyj widoku Ambari Tez w HDInsight opartych na systemie Linux](../hdinsight-debug-ambari-tez-view.md)

Aby uzyskać więcej informacji dotyczących interfejsu API REST używany w tym dokumencie, zobacz [odwołania WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) dokumentu.

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md




[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


