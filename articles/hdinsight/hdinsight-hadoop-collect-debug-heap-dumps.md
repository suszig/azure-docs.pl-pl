<properties
    pageTitle="ヒープ ダンプによる Hadoop サービスのデバッグおよび分析 | Microsoft Azure"
    description="デバッグおよび分析に利用できるように、自動的に Hadoop サービスのヒープ ダンプを収集して Azure BLOB ストレージ アカウント内に配置します。"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/12/2015"
    ms.author="jgao"/>


# BLOB ストレージのヒープ ダンプを収集して Hadoop サービスをデバッグおよび分析する

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

ヒープ ダンプには、変数の値を含む、アプリケーションのメモリのスナップショットが含まれています。 
時に、ダンプが作成されました。 発生する問題を診断するのに便利ですが、 
実行時にします。 ヒープ ダンプを自動的に Hadoop サービスの収集し、内側に配置されます。 
HDInsightHeapDumps 下のユーザーの Azure Blob ストレージ アカウント/です。 

個別のサービスのさまざまなサービスのヒープ ダンプの収集を有効にする必要があります。 
クラスターです。 既定では、クラスターに対してこの機能はオフになっています。 これらのヒープ ダンプを指定できます。 
大規模なので、Blob ストレージ アカウントを監視することをお勧めは、保存場所 
コレクションが有効にします。

> [AZURE.NOTE] この記事の情報は、Windows ベースの HDInsight にのみ適用されます。 
Linux ベースの HDInsight については、次を参照してください [での Hadoop サービス用の有効化のヒープ ダンプ。
Linux ベースの HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)

## ヒープ ダンプの対象サービス

次のサービスのヒープ ダンプを有効にできます。

*  **hcatalog** -tempelton
*  **hive** -hiveserver2、metastore、derbyserver
*  **mapreduce** -jobhistoryserver
*  **yarn** -resourcemanager、nodemanager、timelineserver
*  **hdfs** -datanode、secondarynamenode、namenode

## ヒープ ダンプを有効にする構成要素

サービスのヒープ ダンプを有効にするには、適切な構成要素を設定する必要があります。 
そのサービスのセクションで指定されている **service_name**します。

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

値 **service_name** 上記のサービスのいずれかを指定できます。 
tempelton、hiveserver2、metastore、derbyserver、jobhistoryserver、resourcemanager、nodemanager、timelineserver、datanode、secondarynamenode、または namenode します。

## Azure PowerShell を使用して有効にする

たとえば、Azure PowerShell を使用して jobhistoryserver のヒープ ダンプを有効にするには、次の操作を実行します。

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## .NET SDK を使用して有効にする

たとえば、Azure HDInsight .NET SDK を使用して jobhistoryserver のヒープ ダンプを有効にするには、次の操作を実行します。

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));

