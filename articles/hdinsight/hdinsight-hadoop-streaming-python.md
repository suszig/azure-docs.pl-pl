<properties
   pageTitle="HDInsight での Python MapReduce ジョブの開発 | Microsoft Azure"
   description="Linux ベースの HDInsight クラスターで Python MapReduce ジョブを作成、実行する方法を説明します。"
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

#HDInsight 用 Python ストリーミング プログラムの開発

Hadoop には MapReduce に対するストリーミング API が用意されていて、Java 以外の言語の map 関数と reduce 関数を記述することができます。 この記事では、Python を使用して、MapReduce 操作を実行する方法を説明します。

> [AZURE.NOTE] このドキュメントでの Python コードは、Windows ベースの HDInsight クラスターで使用できますが、このドキュメントの手順では Linux ベースのクラスターに固有です。

この記事が情報と例に、Michael Noll が公開に基づいて [Python での Hadoop MapReduce プログラムの作成](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/)します。

##前提条件

この記事の手順を完了するには、次のものが必要です。

* HDInsight クラスターでの Linux ベースの Hadoop

* テキスト エディター

* Windows クライアントとして PuTTY および PSCP これらのユーティリティは「<a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">PuTTY のダウンロード ページ</a>」から入手できます。

##文字数

この例では、マッパーとレジューサを使用して、基本的な文字カウントを実装します。 マッパーは文をいくつかの単語に分割し、レジューサは単語数や回数を合計して出力を生成します。

次のフローチャートは、map および reduce のフェーズで実際に何が実行されるかを示しています。

![MapReduce の図](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

##Python について

Python は、多くの他の言語より少ないコード行で概念を表現できる高度な汎用プログラム言語です。 迅速なアプリケーションに最適な構造、動的型付けのほか、その洗練された構文といった特徴のため、データ サイエンティストの間で、近年、プロトタイプ言語として一般的になりつつあります。

Python はすべての HDInsight クラスターにインストールされています。

##MapReduce ストリーミング

Hadoop では、ジョブで使用される map および reduce のロジックを含むファイルを指定できます。 map および reduce ロジックに固有の要件は次のとおりです。

* **入力**: マップと削減のコンポーネントは STDIN から入力データを読み取る必要があります。

* **出力**: マップと削減コンポーネントは STDOUT に出力データを書き込む必要があります。

* **データ形式**: 使用および生成さデータは、タブ文字で区切られたキーと値のペアである必要があります。

Python ではこれらの要件を使用して簡単に処理できる、 **sys** モジュールを使用して、STDIN からの読み取りを **印刷** STDOUT に出力を印刷します。 その他、キーと値の間にタブ (`\t`) 文字を使用してデータを簡単に書式設定できます。

##マッパーとリデューサーの作成

マッパーとレジューサ テキスト ファイルをここでは **mapper.py** と **reducer.py** (これは、何が作成する) ためです。 これらは、好みのエディターを使用して作成できます。

###Mapper.py

という名前の新しいファイルを作成する **mapper.py** し、内容として次のコードを使用します。

    #!/usr/bin/env python

    # Use the sys module
    import sys

    # 'file' in this case is STDIN
    def read_input(file):
        # Split each line into words
        for line in file:
            yield line.split()

    def main(separator='\t'):
        # Read the data using read_input
        data = read_input(sys.stdin)
        # Process each words returned from read_input
        for words in data:
            # Process each word
            for word in words:
                # Write to STDOUT
                print '%s%s%d' % (word, separator, 1)

    if __name__ == "__main__":
        main()

コードが読み取られ、解読されるまでしばらく待ちます。

###Reducer.py

という名前の新しいファイルを作成する **reducer.py** し、内容として次のコードを使用します。

    #!/usr/bin/env python

    # import modules
    from itertools import groupby
    from operator import itemgetter
    import sys

    # 'file' in this case is STDIN
    def read_mapper_output(file, separator='\t'):
        # Go through each line
        for line in file:
            # Strip out the separator character
            yield line.rstrip().split(separator, 1)

    def main(separator='\t'):
        # Read the data using read_mapper_output
        data = read_mapper_output(sys.stdin, separator=separator)
        # Group words and counts into 'group'
        #   Since MapReduce is a distributed process, each word
        #   may have multiple counts. 'group' will have all counts
        #   which can be retrieved using the word as the key.
        for current_word, group in groupby(data, itemgetter(0)):
            try:
                # For each word, pull the count(s) for the word
                #   from 'group' and create a total count
                total_count = sum(int(count) for current_word, count in group)
                # Write to stdout
                print "%s%s%d" % (current_word, separator, total_count)
            except ValueError:
                # Count was not a number, so do nothing
                pass

    if __name__ == "__main__":
        main()

##ファイルのアップロード

両方とも **mapper.py** と **reducer.py** はその実行前に、クラスターのヘッド ノードである必要があります。 これらをアップロードする最も簡単な方法を使用する **scp** (**pscp** Windows クライアントを使用している場合)。

同じディレクトリに、クライアントから **mapper.py** と **reducer.py**, 、次のコマンドを使用します。 置換 **ユーザー名** を SSH ユーザーと **clustername** 、クラスターの名前に置き換えます。

    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:

これにより、ファイルがローカル システムからヘッド ノードにコピーされます。

> [AZURE.NOTE] SSH アカウントをセキュリティで保護するパスワードを使用した場合は、パスワードを入力するように求められます。 SSH キーを使用している場合は、`-i` パラメーターと、秘密キーのパスを使用する必要があることがあります。例`scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`:

##MapReduce の実行

1. SSH を使用したクラスターへの接続:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] SSH アカウントをセキュリティで保護するパスワードを使用した場合は、パスワードを入力するように求められます。 SSH キーを使用している場合は、`-i` パラメーターと、秘密キーのパスを使用する必要があることがあります。例`ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`:

2. MapReduce ジョブを開始するには次のコマンドを使用します。

        hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input wasb:///example/data/gutenberg/davinci.txt -output wasb:///example/wordcountout

    このコマンドには次のようなものがあります。

    * **hadoop-streaming.jar**: MapReduce 操作のストリーミングを実行時に使用します。 Hadoop と指定した外部 MapReduce コードとの橋渡しを務めます。

    * **-ファイル**: 指定したファイルがこの MapReduce ジョブに必要なすべてのワーカー ノードに、コピーする必要があります Hadoop に伝えます。

    * **-マッパー**: マッパーとして使用するファイルを Hadoop に伝えます。

    * **-レジューサ**: レジューサとして使用するファイルを Hadoop に伝えます。

    * **-入力**: 文字数をカウントする入力ファイルです。

    * **-出力**: に、出力が書き込まれるディレクトリです。

        > [AZURE.NOTE] このディレクトリは、ジョブによって作成されます。

一連を確認する必要があります **情報** ステートメント ジョブの開始時にし、最後に、 **マップ** と **削減** 操作がパーセンテージで表示します。

    15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%
    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%
    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%

完了すると、ジョブに関するステータス情報が返されます。

##出力を表示する

ジョブが完了したら、次のコマンドを使用して出力を表示します。

    hadoop fs -text /example/wordcountout/part-00000

これにより、文字の一覧と、その出現回数が表示されます。 出力データは次のようになります。

    wrenching       1
    wretched        6
    wriggling       1
    wrinkled,       1
    wrinkles        2
    wrinkling       2

##次のステップ

これで、HDInsight でストリーミング MapRedcue ジョブを使用する方法に関する説明は終わりです。次のリンクを使用して、Azure HDInsight を操作するその他の方法について調べることもできます。

* [HDInsight での Hive の使用](hdinsight-use-hive.md)
* [HDInsight での Pig の使用](hdinsight-use-pig.md)
* [HDInsight での MapReduce ジョブの使用](hdinsight-use-mapreduce.md)


