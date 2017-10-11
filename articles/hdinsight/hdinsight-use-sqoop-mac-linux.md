---
title: "Apache Sqoop z platformą Hadoop — usługa Azure HDInsight | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak używać narzędzia Apache Sqoop do importowania i eksportowania między Hadoop w usłudze HDInsight i bazy danych SQL Azure."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
author: Blackmist
tags: azure-portal
keywords: hadoop sqoop, sqoop
ms.assetid: 303649a5-4be5-4933-bf1d-4b232083c354
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: larryfr
ms.openlocfilehash: 35dcbb91e6af1480685c9fd5b829c54277c1c605
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a>Apache Sqoop umożliwia importowanie i eksportowanie danych między Hadoop w usłudze HDInsight i bazy danych SQL

[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Dowiedz się, jak używać narzędzia Apache Sqoop importowania i eksportowania między klastrem usługi Hadoop w usłudze Azure HDInsight i bazy danych Azure SQL Database lub programu Microsoft SQL Server. Użyj dokumentów z krokami w tym `sqoop` polecenia bezpośrednio z headnode klastra usługi Hadoop. SSH umożliwia łączenie z węzłem głównym, a następnie uruchom polecenia w tym dokumencie.

> [!IMPORTANT]
> Kroki opisane w tym dokumencie pracować tylko z klastrami usługi HDInsight, które używają systemu Linux. Linux jest jedynym systemem operacyjnym używanym w połączeniu z usługą HDInsight w wersji 3.4 lub nowszą. Aby uzyskać więcej informacji, zobacz sekcję [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (Wycofanie usługi HDInsight w systemie Windows).

## <a name="install-freetds"></a>Zainstaluj protokół FreeTDS

1. Używanie protokołu SSH, aby połączyć się z klastrem usługi HDInsight. Na przykład następujące polecenie nawiązuje połączenie z podstawowym headnode klastra o nazwie `mycluster`:

    ```bash
    ssh CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Aby uzyskać więcej informacji, zobacz [Używanie protokołu SSH w usłudze HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Aby zainstalować protokół FreeTDS, użyj następującego polecenia:

    ```bash
    sudo apt --assume-yes install freetds-dev freetds-bin
    ```

    Protokół FreeTDS jest używany w kilku etapach nawiązać połączenia z bazą danych SQL.

## <a name="create-the-table-in-sql-database"></a>Tworzenie tabeli w bazie danych SQL

> [!IMPORTANT]
> Jeśli korzystasz z klastrem usługi HDInsight i tworzenie bazy danych SQL w [Tworzenie klastra i bazy danych SQL](hdinsight-use-sqoop.md), pomiń kroki opisane w tej sekcji. Bazy danych i tabeli zostały utworzone jako część kroki [Tworzenie klastra i bazy danych SQL](hdinsight-use-sqoop.md) dokumentu.

1. W sesji SSH wpisz następujące polecenie, aby połączyć się z serwerem bazy danych SQL.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Pojawi się dane wyjściowe podobne do następującego tekstu:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

2. W `1>` monit, wprowadź następujące zapytanie:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
    GO
    ```

    Gdy `GO` instrukcja została wprowadzona, poprzednie instrukcje są oceniane. Najpierw **mobiledata** tabela została utworzona, a następnie indeks klastrowany zostaną dodane do niego (wymagane przez usługę SQL Database).

    Użyj następującego zapytania, aby sprawdzić, czy tabela została utworzona:

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    Zostaną wyświetlone dane wyjściowe podobne do następującego tekstu:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

3. Wprowadź `exit` na `1>` prompt wyjść z narzędzia tsql.

## <a name="sqoop-export"></a>Sqoop eksportu

1. Połączenie SSH do klastra użyj następującego polecenia, aby sprawdzić, czy Sqoop widzą bazy danych SQL:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    Po wyświetleniu monitu wprowadź hasło dla nazwy logowania bazy danych SQL.

    To polecenie zwraca listę baz danych, w tym **sqooptest** bazy danych, który został utworzony wcześniej.

2. Aby wyeksportować dane z **hivesampletable** do **mobiledata** tabeli, należy użyć następującego polecenia:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1
    ```

    To polecenie powoduje, że Sqoop nawiązać **sqooptest** bazy danych. Sqoop następnie eksportuje dane z **wasb: / / / gałęzi/magazynu/hivesampletable** do **mobiledata** tabeli.

    > [!IMPORTANT]
    > Użyj `wasb:///` Jeśli konta usługi Azure Storage jest domyślny magazyn dla klastra. Użyj `adl:///` przypadku usługi Azure Data Lake Store.

3. Po zakończeniu wykonywania polecenia, użyj następującego polecenia do łączenia z bazą danych przy użyciu języka TSQL:

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P -p 1433 -D sqooptest
    ```

    Po nawiązaniu połączenia użyj następujące instrukcje, aby sprawdzić, czy dane został wyeksportowany do **mobiledata** tabeli:

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata
    GO
    ```

    Powinna zostać wyświetlona lista danych w tabeli. Typ `exit` aby wyjść z narzędzia tsql.

## <a name="sqoop-import"></a>Importuj Sqoop

1. Użyj następującego polecenia do importowania danych z **mobiledata** do tabeli w bazie danych SQL, **wasb: / / / samouczki/usesqoop/importeddata** katalogu w usłudze HDInsight:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    Pola są oddzielone znak tabulacji, a wiersze kończą się znakiem nowego wiersza.

2. Po zakończeniu importowania, użyj następującego polecenia do listy limit danych w nowym katalogu:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a>Za pomocą programu SQL Server

Umożliwia także Sqoop umożliwia importowanie i eksportowanie danych z programu SQL Server w centrum danych lub na maszynie wirtualnej hostowanej na platformie Azure. Różnice między przy użyciu bazy danych SQL i programu SQL Server są następujące:

* Zarówno HDInsight, jak i SQL Server musi być w tej samej sieci wirtualnej platformy Azure.

    Na przykład zobacz [HDInsight połączyć się z siecią lokalną](./connect-on-premises-network.md) dokumentu.

    Aby uzyskać więcej informacji o korzystaniu z sieci wirtualnej platformy Azure HDInsight, zobacz [rozszerzenie usługi HDInsight za pomocą usługi Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentu. Aby uzyskać więcej informacji w sieci wirtualnej Azure, zobacz [omówienie sieci wirtualnych](../virtual-network/virtual-networks-overview.md) dokumentu.

* Program SQL Server musi być skonfigurowane i umożliwiają uwierzytelnianie SQL. Aby uzyskać więcej informacji, zobacz [wybierz tryb uwierzytelniania](https://msdn.microsoft.com/ms144284.aspx) dokumentu.

* Może być konieczne skonfigurowanie serwera SQL na akceptowanie połączeń zdalnych. Aby uzyskać więcej informacji, zobacz [jak rozwiązywać problemy z nawiązywania połączenia z aparatem bazy danych programu SQL Server](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) dokumentu.

* Utwórz **sqooptest** bazy danych w programie SQL Server przy użyciu narzędzia, takie jak **programu SQL Server Management Studio** lub **tsql**. Kroki do używania interfejsu wiersza polecenia Azure działa tylko w bazie danych SQL Azure.

    Użyj następujących instrukcji języka Transact-SQL, aby utworzyć **mobiledata** tabeli:

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    ```

* Podczas nawiązywania połączenia z programem SQL Server z usługi HDInsight, należy użyć adresu IP programu SQL Server. Na przykład:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a>Ograniczenia

* Zbiorcze export - opartych na systemie Linux z usługi HDInsight, łącznik Sqoop, używany do eksportowania danych do programu Microsoft SQL Server lub bazy danych SQL Azure nie obsługuje obecnie zbiorcze operacje wstawiania.

* Przetwarzanie wsadowe — z opartą na systemie Linux usługą HDInsight przy użyciu `-batch` przełączyć podczas wykonywania operacji wstawienia, Sqoop sprawia, że wiele operacji wstawienia zamiast przetwarzanie wsadowe operacji insert.

## <a name="next-steps"></a>Następne kroki

Teraz ma przedstawiono sposób używania Sqoop. Aby dowiedzieć się więcej, zobacz:

* [Korzystanie z usługą HDInsight Oozie][hdinsight-use-oozie]: Użyj Sqoop działań w przepływie pracy Oozie.
* [Analizowanie danych opóźnienie transmitowane przy użyciu usługi HDInsight][hdinsight-analyze-flight-data]: Użyj gałąź rejestru, aby transmitowane analizować opóźnienie danych, a następnie użyj Sqoop eksportować dane do bazy danych Azure SQL.
* [Przekazywanie danych do usługi HDInsight][hdinsight-upload-data]: znajdowanie innych metod do przekazywania danych do magazynu obiektów Blob HDInsight/Azure.

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
