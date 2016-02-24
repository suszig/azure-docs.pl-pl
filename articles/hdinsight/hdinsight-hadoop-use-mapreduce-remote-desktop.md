<properties
   pageTitle="HDInsight での MapReduce と Hadoop の Remote Desktop の使用 | Microsoft Azure"
   description="リモート デスクトップを使用して、HDInsight で Hadoop に接続し、MapReduce ジョブを実行する方法を説明します。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="12/04/2015"
   ms.author="larryfr"/>

# リモート デスクトップによる HDInsight での MapReduce と Hadoop の使用

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

この記事では、リモート デスクトップを使用して HDInsight クラスター上の Hadoop に接続し、Hive コマンドを使用して MapReduce ジョブを実行する方法について説明します。

##<a id="prereq"></a>前提条件

この記事の手順を完了するには、次のものが必要です。

* Windows ベースの HDInsight (HDInsight で Hadoop を使用) クラスター

* Windows 10、Windows 8、Windows 7 を実行するクライアント コンピューター

##<a id="connect"></a>リモート デスクトップへの接続します。

「[RDP を使用した HDInsight クラスターへの接続](hdinsight-administer-use-management-portal.md#rdp)」の手順に従って、HDInsight クラスターのリモート デスクトップを有効にしてからデスクトップに接続します。

##<a id="hadoop"></a>Hadoop コマンドの使用

HDInsight クラスターのデスクトップに接続したら、次の手順に従って Hadoop コマンドを使用して MapReduce ジョブを実行します。

1. HDInsight デスクトップから開始、 **Hadoop コマンド ライン**します。 新しいコマンド プロンプトが開き、 **c:\apps\dist\hadoop-& lt; のバージョン番号 >** ディレクトリ。

    > [AZURE.NOTE] Hadoop が更新されると、バージョン番号が変更されます。  **%Hadoop_home%** パスを確認する環境変数を使用することができます。 たとえば、`cd %HADOOP_HOME%` では、バージョン番号がわからなくても、ディレクトリは Hadoop ディレクトリに変更されます。

2. 使用する、 **Hadoop** サンプルの MapReduce ジョブを実行するには、次のコマンドを使用してコマンド。

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    起動、 **wordcount** に含まれているクラス、 **hadoop-examples.jar mapreduce** 、現在のディレクトリ内のファイルです。 使用して、入力として、 **wasb://example/data/gutenberg/davinci.txt** にドキュメントと出力が格納されている: **wasb:////例/データ/確認**します。

    > [AZURE.NOTE] この MapReduce ジョブとサンプル データの詳細については、次を参照してください。 <a href="hdinsight-use-mapreduce.md">HDInsight の Hadoop MapReduce の使用</a>します。

2. 処理中に詳細が生成され、ジョブが完了すると、次のような情報が返されます。

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. ジョブが完了したときは、次のコマンドを使用して、出力ファイルの保存場所を一覧表示する **wasb://example/data/WordCountOutput**:

        hadoop fs -ls wasb:///example/data/WordCountOutput

    2 つのファイルに表示されす **_SUCCESS** と **- part-r-00000**します。  **- Part-r-00000** ファイルには、このジョブの出力が含まれています。

    > [AZURE.NOTE] 一部の MapReduce ジョブは複数の結果を分割することがあります **一部 r-###** ファイルです。 このとき、ファイルの順番を特定するには ##### サフィックスを使用します。

4. 出力を表示するには、次のコマンドを使用します。

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    含まれる単語の一覧が表示されます、 **wasb://example/data/gutenberg/davinci.txt** と共に各単語が発生した回数の合計のファイルです。 ファイルに含まれるデータの例を次に示します。

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>概要

このように、Hadoop を使用すると、HDInsight クラスターで簡単に MapReduce ジョブを実行し、ジョブ出力を表示できます。

##<a id="nextsteps"></a>次のステップ

HDInsight での MapReduce ジョブに関する全般的な情報:

* [Use MapReduce on HDInsight Hadoop (HDInsight Hadoop での MapReduce の使用)](hdinsight-use-mapreduce.md)

HDInsight での Hadoop のその他の使用方法に関する情報

* [HDInsight での Hive と Hadoop の使用](hdinsight-use-hive.md)

* [HDInsight での Pig と Hadoop の使用](hdinsight-use-pig.md)

