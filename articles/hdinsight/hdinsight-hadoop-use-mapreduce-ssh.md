<properties
   pageTitle="HDInsight での MapReduce と Hadoop の SSH 接続 の使用 | Microsoft Azure"
   description="SSH を使用して HDInsight で Hadoop を使用して MapReduce ジョブを実行する方法を説明します。"
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
   ms.date="11/06/2015"
   ms.author="larryfr"/>

# SSH による HDInsight での MapReduce と Hadoop の使用

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

この記事では、Secure Shell (SSH) を使用して HDInsight クラスターで Hadoop に接続し、Hadoop コマンドを使用して MapReduce ジョブを送信する方法を説明します。

> [AZURE.NOTE] Linux ベースの Hadoop サーバーの使用に慣れて既に HDInsight に慣れていない場合、 [Linux ベースの HDInsight の使用方法](hdinsight-hadoop-linux-information.md)します。

##<a id="prereq"></a>前提条件

この記事の手順を完了するには、次のものが必要です。

* Linux ベースの HDInsight (HDInsight で Hadoop を使用) クラスター

* SSH クライアント SSH クライアントを備えた Linux、Unix、Mac オペレーティング システム Windows ユーザーは [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) などのクライアントをダウンロードする必要があります。

##<a id="ssh"></a>SSH を使用した接続

SSH コマンドを使用して、HDInsight クラスターの完全修飾ドメイン名 (FQDN) に接続します。 FQDN が指定した後に、クラスター名になります **. >.azurehdinsight.net**します。 次の例、という名前のクラスターに接続 **myhdinsight**:

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**SSH 認証に証明書キーを指定した場合** HDInsight クラスターを作成したときに、たとえば、クライアント システムの秘密キーの場所を指定する必要があります。

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**SSH 認証のパスワードを指定した場合** HDInsight クラスターを作成したときに、入力を求められたらパスワードを指定する必要があります。

HDInsight で SSH の使用に関する詳細については、次を参照してください。 [SSH Linux、Unix、OS X から HDInsight 上の Linux ベースの Hadoop を使用する](hdinsight-hadoop-linux-use-ssh-unix.md)です。

###PuTTY (Windows クライアント)

Windows ではビルトイン SSH クライアントは提供されません。 使用することをお勧め **PuTTY**, からダウンロードできる [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)します。

PuTTY の使用の詳細については、次を参照してください。 [SSH Windows から Linux ベースの Hadoop を使用する ](hdinsight-hadoop-linux-use-ssh-windows.md)です。

##<a id="hadoop"></a>Hadoop コマンドの使用

1. HDInsight クラスターに接続した後は、次を使用して **Hadoop** MapReduce ジョブを開始するコマンド。

        hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    起動、 **wordcount** に含まれているクラス、 **hadoop-examples.jar mapreduce** ファイルです。 使用して、入力として、 **wasb://example/data/gutenberg/davinci.txt** にドキュメントと出力が格納されている **wasb:////例/データ/確認**します。

    > [AZURE.NOTE] この MapReduce ジョブとサンプル データの詳細については、次を参照してください。 [HDInsight で Hadoop MapReduce の使用](hdinsight-use-mapreduce.md)します。

2. 処理中に詳細が生成され、ジョブが完了すると、次のような情報が返されます。

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. ジョブの完了時に、次のコマンドを使用して、格納されている出力ファイルを一覧表示 **wasb://example/data/WordCountOutput**:

        hdfs dfs -ls wasb:///example/data/WordCountOutput

    2 つのファイルに表示されす **_SUCCESS** と **- part-r-00000**します。  **- Part-r-00000** ファイルには、このジョブの出力が含まれています。

    > [AZURE.NOTE] 一部の MapReduce ジョブは複数の結果を分割することがあります **一部 r-###** ファイルです。 このとき、ファイルの順番を特定するには ##### サフィックスを使用します。

4. 出力を表示するには、次のコマンドを使用します。

        hdfs dfs -cat wasb:///example/data/WordCountOutput/part-r-00000

    含まれる単語の一覧が表示されます、 **wasb://example/data/gutenberg/davinci.txt** ファイルと各単語が発生した回数。 ファイルに含まれるデータの例を次に示します。

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>概要

このように、Hadoop コマンドを使用すると、HDInsight クラスターで簡単に MapReduce ジョブを実行し、ジョブ出力を表示できます。

##<a id="nextsteps"></a>次のステップ

HDInsight での MapReduce ジョブに関する全般的な情報:

* [Use MapReduce on HDInsight Hadoop (HDInsight Hadoop での MapReduce の使用)](hdinsight-use-mapreduce.md)

HDInsight での Hadoop のその他の使用方法に関する情報

* [HDInsight での Hive と Hadoop の使用](hdinsight-use-hive.md)

* [HDInsight での Pig と Hadoop の使用](hdinsight-use-pig.md)


